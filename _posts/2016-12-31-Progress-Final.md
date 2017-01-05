---
layout: post
title:  "Post Final"
date:   31-12-2016 12:00:00
author: pomodorest
categories: progress
---

# Tele_dir

### Introducción 

Ya terminado el curso CC5407 del departamento de Ciencias de la Computación de la Facultad de Ingeniería de la Universidad de Chile, se presentará el proyecto tele_dir finalizado, su estado con respecto a la integración y las conclusiones del cierre del curso.

### Problema

Recordando un poco, el problema seleccionado fué la gran cantidad de software de teleoperación disponible para distintos robots ( turtlebot, drone, Pr2, etc ), lo que se consideró como un error debido a la poca extensibilidad que presentan estos softwares.
Otro punto a cuestionar de estos software es que implementan la publicación continua, lo que quiere decir que se envía un continuo de mensajes a un tópico específico con un mensaje por defalut, por lo que el robot queda "tomado" y no puede ser operado por otra herramienta de teleoperación si existe alguna corriendo.
También su rigidez en cuanto a configuraciones hacen que los distintos tele_op sean similares en funcionalidad pero diferentes paquetes en ROS, por lo que hay que instalar uno diferente por robot diferente con el que quiera trabajar

### Solución

Como respuesta a estas problematicas, se creó tele_dir, un software capaz de operar a diferentes tipos de robots mientras estos sean operados con mensajes de ROS y no con ActionLib.

Tele_dir se puede separar en dos herramientas:

* XMLHandler : XMLHandler es la clase que se preocupa de manejar todos los archivos de configuración desde su creación, validación y edición. Siempre que se edita o crea un archivo, este mantiene la consistencia necesaria.

* TeleDir :  TeleDir es el programa en sí. Pregunta las opciones que se quieren ejecutar y llama a XMLHandler si lo necesita. La opción de controlar al robot crea los keybinding señalados en el archivo de configuración, constan de una tecla, un mensaje y un tópico. Luego de esto, comienza a escuchar el input del teclado y a publicar el mensaje correspondiente en el tópico asignado, esto permite que se publique un mensaje por cada evento de tecla presionado, por lo que no bloquea el control del robot en caso de estar activo pero sin uso.

El software tambien depende de otro paquete de ROS para operar ya que escapaba de los alcances del proyecto desarrollar esa parte que fue muy importante en el proyecto, este es el [link](https://github.com/baalexander/rospy_message_converter) al proyecto llamado rospy-message-convertor.

### Diseño de la solución

El diseño fue pensado para ser extendible y amigable con el usuario, por lo que se trato de dejar la mayor parte al usuario sobre la operación, es decir, el usuario crea sus propias configuraciones. Sobre la extensibilidad se puede acotar que el programa cuenta con un menú de opciones donde el usuario escoge que módulo correr, por lo que para añadir nuevas funcionalidades basta con crear una nueva opción con su funcionalidad.

Se trabajó bajo los principios de extensibilidad y modularización en su mayor parte. Entre los modulos se implementaron funciones auxiliares para tomar una entrada valida de nuestro programa (ej: Pedir  "Y" o "N" se utiliza en reiteradas ocaciones y solo difiere el mensaje con el que va acompañada la opción), funciones para rellenar un mensaje valido de ROS y para rellenar cada parametro de un diccionario. Se intento testear la gran parte de las funciones, pero como muchas de ellas trabajaban con entradas del usuario (Raw_input o input), se hizo casi imposible testearlas todas, pero, gracias a este problema se identificaron partes de código que podian ser modularizadas en fracciones mas pequeñas que permitian el testeo.

Se Crearon dos Clases distintas para manejar el programa dado que una parte se preocupa netamente de la edición, creación y validación de los archivos de configuración, por lo que se opto por esta forma en caso de tener que cambiar el formato de los archivos de configuración en un futuro o el sistema de validación.

### Integración

Al ser tele_dir un paquete nuevo en ROS la integración fue mas sencilla que un cambio al core o a herramientas ya existentes, los pasos a seguir estan detallados en este [link](http://wiki.ros.org/bloom/Tutorials/FirstTimeRelease). Se utilizo la herramienta bloom para la integración, la cual consiste en editar un archivo yml en un repositorio en github que contiene la lista de paquetes con sus descripciones y links correspondientes. Si bien se hizo la integración con el archivo yml, tele_dir fue lanzado hace unos dias por la actualización de ROS pero se detectaron errores que no permiten ejecutarlo, aun así se puede descargar el código de este [link](https://github.com/rdelgadov/tele_dir) y ejecutar el archivo teleDir.py para manipular los robots.

Como la integración fué hace unos dias aun no esta disponible la wiki del proyecto en ROS y queda como tarea pendiente.

### Conclusiones

Es importante considerar la integración desde un principio en el desarrollo ya que es un paso importante para contribuir con ROS y que demanda mucho mas tiempo del esperado. 

Existen muchos paquetes en ROS, lo que genera que hayan muchas herramientas que permitan mejorar alguna ya existente, asi como también existen muchas que son similares en funcionalidad pero que difieren en el uso que se les da.

El hecho de ser un S.O que acepta varios lenguajes (C++ y python en particular) genera que muchas ventajas de python no puedan ser usadas por tener tipos estáticos heredados de C++.

La mayor parte del core de ROS posee funciones muy interesantes para el desarrollo de software pero son funciones privadas por lo que la unica forma de accederlas es buscar como funciona cada herramineta de ROS y luego mirar el código fuente para copiar las funciones.










