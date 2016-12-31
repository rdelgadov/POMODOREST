---
layout: post
title:  "Post Final"
date:   31-12-2016 12:00:00
author: pomodorest
categories: progress
---

#Tele_dir

### Introducción 

Ya terminado el curso CC5407 del departamento de Ciencias de la Computación de la Facultad de Ingeniería de la Universidad de Chile, se presentará el proyeccto tele_dir finalizado.

### Problema

Recordando un poco, el problema seleccionado fué la gran cantidad de software de teleoperación disponible para distintos robots ( turtlebot, drone, Pr2, etc ), lo que se consideró como un problema debido a la poca extensibilidad que presentan estos softwares.
Ademas que estos software implementan la publicación continua, por lo que el robot queda "tomado" y no puede ser operado por otra herramienta de teleoperación si existe una corriendo.
También su rigidez en cuanto a configuraciones hacen que los distintos tele_op sean similares en funcionalidad pero diferentes paquetes en ROS.

### Solución

Como respuesta a estas problematicas, se creó tele_dir, un software capaz de operar a diferentes tipos de robots mientras estos sean operados con mensajes de ROS y no con ActionLib. El software se encuentra disponible en ROS para la versión índigo, se puede añadir con el  comando 
  sudo apt-get install ros-indigo-tele_dir

Luego basta ejecutar el comando rosrun con el paquete tele_dir para ejecutar el programa.

Tele_dir se puede separar en dos herramientas:

* XMLHandler : XMLHandler es la clase que se preocupa de manejar todos los archivos de configuración desde su creación, validación y edición. Siempre que se edita o crea un archivo, este mantiene la consistencia necesaria.

* TeleDir :  TeleDir es el programa en sí. Crea los keybinding señalados en el archivo de configuración, consta de una tecla, un mensaje y un tópico. Luego de esto, comienza a escuchar el input del teclado y a publicar el mensaje correspondiente en el tópico asignado, esto permite que se publique un mensaje por cada evento de tecla presionado, por lo que no bloquea el control del robot en caso de estar activo pero sin uso.


