---
layout: post
title: Configurar Xdebug en VScode con ddev en Ubuntu
tags:
  - github
  - drupal
  - ddev
  - xdebug
categories:
  - drupal
  - config
comments: true
visible: 1
---
![storage](/images/Xdebug_Logo.svg.png)

Buenas a tod@s l@s que puedan estar leyendo este cachito de Internet.

Como primer post siempre se espera que sea una presentación del escritor hacia el lector, pero bueno siempre hay tiempo de ir conociéndonos poco a poco con cada post nuevo que pueda ir introduciendo por aquí. En principio la mayoría de posts que vaya introduciendo por aquí irá dirigido a mi yo del futuro para que se acuerde de lo que realizó y cómo o porqué lo realizo. Si esto puede servir a alguien más bienvenido sea. Así que....

### ¡Al turrón!

En este post intentaré describir las configuraciones necesarias para poder debuguear nuestro Drupal en Vscode, utilizando como stack DDEV y como SO linux, más concretamente [KDE Neon](https://neon.kde.org/), basada en ubuntu.

### ¿Que es DDEV?

Pero un momento espera D que..? DDEV es una herramienta de desarrollo de código abierto, construida sobre Docker. Con ella se pueden crear fácilmente entornos de alojamiento local, y sus configuraciones de servidor pueden ser controladas por versión. Originalmente pensado para el desarrollo de Drupal, ddev puede fácilmente alojar sitios de Drupal, Wordpress y GravCMS. Como está basado en Docker, ddev es compatible con Windows, Mac y Linux.

¿Tienes ganas de saber más? ¿Cual es su arquitectura? ¿Cómo se instala? Para ello te invito a que leas este [magnifico post](https://davidjguru.github.io/blog/creating-development-environments-for-drupal-with-ddev) de [@davidjguru](https://twitter.com/davidjguru) en el cual da respuesta a todas las preguntas anteriores.

### Configuración DDEV

El contenedor web levantado por DDEV por defecto viene con Xdebug desactivado, por lo que nuestro primer paso será activarlo. Para ello podemos hacerlo a través de dos vías, cambiando la variable `xdebug_enabled` del archivo *config.yaml* del path .ddev de tu proyecto a true, o ejecutando los comandos `ddev exec enable_xdebug` y `ddev exec disable_xdebug`  para activarlo y desactivarlo a petición. De la primera forma siempre tendremos activo xdebug cada vez que iniciemos DDEV con `ddev start`. De la segunda manera cada vez que queramos depurar nuestro código tendremos que ejecutar el comando para activar xdebug.

### Configuración VScode

Para configurar VScode necesitamos realizar los siguientes pasos :

1. Instalar la extensión [php-debug](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug)

2. Añadir al archivo de configuración 'launch.json' el siguiente fragmento de configuración:

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Listen for XDebug",
      "type": "php",
      "request": "launch",
      "port": 9000,
      "pathMappings": {
        "/var/www/html": "/home/user/projects/project_name"
      },
      "stopOnEntry": false,
      "log": false,
      "xdebugSettings": {
        "max_children": 100,
        "max_depth": 5
      }
    },
  ]
}
```

Donde:
- **pathMappings**: Aquí es donde se realiza el mapeo en el cual se indica que el path *`/var/www/html`* de DDEV, 'apunta' hacia la carpeta donde tenemos nuestro proyecto *`/home/user/projects/project_name`*
- **stopOnEntry**: Si ponemos a *true* esta variable, al iniciar la depuración parará siempre en el primer punto de inicio de nuestro proyecto.

Una vez configurado nuestro IDE, tocaría por último configurar nuestro navegador, el cual tendremos que instalar una extensión para habilitar/deshabilitar la depuración y el rastreo fácilmente. Yo suelo usar la extensión [Xdebug helper](https://addons.mozilla.org/es/firefox/addon/xdebug-helper-for-firefox/?src=search) la cual puedo activar y desactivar fácilmente con un click.



Bueno pues una vez configurado todo lo único que nos queda es probarlo, nos dirigimos a VScode y pulsamos F5:

![](/images/vscode_debug_active.png)

Ya tenemos a la escucha nuestro IDE para depurar. Nos dirigimos a nuestra portal en nuestro navegador y nada más comenzar, cómo tenemos activado la variable **stopOnEntry** a true nos saltará automáticamente vscode:

![](/images/vscode_debug.png)



Listo!! Ya podemos depurar con nuestro IDE, poner puntos de ruptura y trabajar sobre ellos.

En el próximo post hablaré sobre la configuración de VScode y Xdebug con contendores Docker.

Hasta la próxima!!!

<script id="dsq-count-scr" src="//saganakat-github-io.disqus.com/count.js" async></script>