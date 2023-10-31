---
title: "El Juego de la Vida de Conway: Desarrollando un Autómata Celular"
categories: [Simulación, Java]
tags: [Juego de la vida de Conway, Autómata Celular, Java, Desarrollo de escritorio]
date: 2021-05-16 15:14:00 -0500
math: false
mermaid: false
pin: false
img_path: /assets/img/posts/juego-vida-conway-desarrollando-automata-celular/
image: game-of-life-post.webp
---
El Juego de la Vida de Conway, diseñado por John Horton Conway en 1970, es un cautivador autómata celular. Lo que lo hace verdaderamente intrigante es que es un juego de cero jugadores, lo que significa que su evolución está determinada únicamente por el estado inicial, sin necesidad de ninguna entrada una vez que comienza el juego.

## Las Reglas

El tablero de juego es una cuadrícula plana que se enrolla como un donut. Está compuesto por cuadrados que representan células. Cada célula tiene ocho vecinos, incluyendo los de direcciones diagonales. Las células pueden estar en uno de dos estados: "vivas" o "muertas" (encendidas o apagadas). El estado de todas las células evoluciona en unidades de tiempo discretas o turnos. Todas las células se actualizan simultáneamente en cada turno, siguiendo estas reglas simples pero fascinantes:

1. **Una célula muerta con exactamente tres vecinos vivos cobrará vida.**
2. **Una célula viva con dos o tres vecinos vivos continuará prosperando; de lo contrario, perecerá.**

>Estas reglas rigen todo el juego y conducen a patrones complejos y fascinantes. Para obtener más información, puedes visitar la [página de Wikipedia](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).
{: .prompt-info }

## Interfaz de Usuario Amigable

La ventana de control proporciona una interfaz intuitiva para ejecutar el juego. También muestra estadísticas de población para cada estado y generación. Puedes iniciar, pausar y reiniciar el juego en cualquier momento. Además, puedes personalizar los colores, como se muestra a continuación.

![Ventana de control](https://github.com/crixodia/java-game-of-life/raw/master/images/contro-gui.png)
_Ventana de control_

### Guardar y Cargar Patrones

Una característica destacada de esta implementación es la capacidad de guardar y cargar patrones. Para guardar un patrón, simplemente dibújalo en la cuadrícula y haz clic en el botón de guardar. También puedes especificar un nombre para el patrón.

![Diálogo para guardar patrones](https://github.com/crixodia/java-game-of-life/raw/master/images/save-dialog.png)
_Diálogo para guardar patrones_

Una vez que los patrones están guardados, puedes cargarlos fácilmente. Esta operación limpia la cuadrícula y carga el patrón seleccionado.

![Diálogo para cargar patrones](https://github.com/crixodia/java-game-of-life/raw/master/images/open-dialog.png)
_Diálogo para cargar patrones_

### Visualización de la Cuadrícula

La cuadrícula proporciona una representación visual de los cambios de estado. Dado que el tablero se enrolla como un donut, hay bordes limitados. Esta implementación utiliza una cuadrícula toroidal, que es una cuadrícula bidimensional con condiciones de límites periódicos. Esto significa que los bordes superior e inferior están conectados, al igual que los bordes izquierdo y derecho.

![Visualización de la cuadricula](https://github.com/crixodia/java-game-of-life/raw/master/images/grid-gui.png)
_Visualización de la cuadricula_

### Animaciones y Archivos GIF

Para aquellos que aprecian la visualización de patrones a lo largo del tiempo, esta implementación ofrece la capacidad de generar animaciones. Puedes especificar una ruta de carpeta para guardar imágenes generadas, personalizar colores e incluso utilizar un archivo para generar patrones aleatorios basados en su contenido.

Cuando generas una animación, obtendrás un archivo GIF, similar al que se muestra a continuación. Incluso puedes crear bucles infinitos para una fascinación continua.

![Bucle infinito](https://github.com/crixodia/java-game-of-life/raw/master/examples/GIFgen/Profile_Life_NFT/animation.gif)
_Bucle infinito_

## Explora Ejemplos

¿Quieres sumergirte de inmediato? Puedes descargar varios patrones y ejemplos desde [aquí](https://github.com/crixodia/java-game-of-life/blob/master/examples/). No dudes en descargar y explorar este [proyecto](https://github.com/crixodia/java-game-of-life). ¡Tus comentarios y opiniones son muy apreciados!
