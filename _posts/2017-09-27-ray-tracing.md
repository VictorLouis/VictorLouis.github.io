---
title: Rasterización con BSP en mi primer computador
teaser: La influencia de mi primer PC en mi carrera.
category: Computación Visual
tags: [BSP, algorithms, visual]
---

Binary Space Partitioning
--------------------------------------

Un dia en 1996 instalaron el primer computador de mesa al cual tuve acceso vino con Windows 95, como era pequeño estaba lleno de curiosidad por ver que tenía, una vez lo encendí y lo use pero no había mucho que hacer aparte de usar Office, MS Paint o un explorador viejo llamado Netscape esa vez no capto mucho mi interés pero un dia una de las compañeras de trabajo de mi mama visitó la casa con el hijo adolescente coincidencialmente este traía un CD-ROM que fue una influencia en mi interés por los computadores y las ciencias detrás de estos, era un CD con un juego llamado DOOM, el adolescente me pregunto si queria que lo instalara obviamente yo respondí que sí, él instalo el juego y procedió a enseñarme  los comandos de MS-DOS necesarios para ejecutarlo una vez terminada la instalación lo probe y la experiencia fue bastante sorprendente. En el transcurso del curso me enteré que el motor gráfico de este juego se hizo con un método visto en clase y ahora por curiosidad me encuentro explicando dicho tema.

BSP en ciencias de la computación es un método para subdividir recursivamente un espacio en conjuntos convexos por hiperplanos. Esta subdivisión da lugar a una representación de objetos dentro del espacio por medio de una estructura de datos de árbol conocida como árbol BSP.

BSP se desarrolló en el contexto de los gráficos de computador 3D, donde la estructura de un árbol de BSP permite la información espacial sobre los objetos en una escena que es útil en la representación, tal como su ordenar a partir del frente-a-atras con respecto a un espectador en un lugar dado, para ser accedido rápidamente.

El motor Doom hace uso de un sistema conocido como partición de espacio binario (BSP). Se debe usar una herramienta, conocida como un constructor de nodos, para generar los datos BSP para un nivel antes de que pueda ser reproducido. Dependiendo del tamaño y la complejidad del nivel, este proceso puede tomar bastante tiempo.
BSP divide el nivel en un árbol binario: cada ubicación del árbol es un nodo que representa un área particular del nivel (con el nodo raíz que representa todo el nivel). En cada rama del árbol hay una línea divisoria que divide el área del nodo en dos subnodos. Al mismo tiempo, la línea divisoria divide los bordados en segmentos de línea llamados *segmentos*.

En las hojas del árbol son polígonos convexos, donde no es útil dividir el nivel más arriba. Estos polígonos convexos se denominan subsectores (o SSECTORS), y están unidos a un sector particular. Cada subsector tiene una lista de segs asociados con ella.
El sistema BSP es realmente una manera muy inteligente de clasificar los subsectores en el orden correcto para la representación. El algoritmo es bastante simple:

> 1. Comienza en el nodo raíz.
>
> 2. Dibuja recursivamente los nodos secundarios de este nodo. Se dibuja primero el nodo secundario más cercano a la cámara. (Esto se puede encontrar mirando a qué lado de la línea divisoria
>  de un nodo dado está encendida la cámara).
>   
> 3. Cuando se alcanza un subsector, dibujalo.

```c++

void traverse_tree(bsp_tree* tree, point eye) {
    if (tree->empty())
        return;

    int location = tree->find_location(eye);
    if (location > 0) { // caso de que el ojo este antes de la ubicacion
        traverse_tree(tree->back, eye);
        display(tree->polygon_list);
        traverse_tree(tree->front, eye);
    } else if (location < 0) { // caso el ojo esta detras de la ubicacion
        traverse_tree(tree->front, eye);
        display(tree->polygon_list);
        traverse_tree(tree->back, eye);
    } else { // eye が分割超平面上にある場合
        traverse_tree(tree->front, eye);
        traverse_tree(tree->back, eye);
    }
}

```

Para una columna dada de píxeles, el proceso finaliza cuando toda la columna está llena (es decir, no quedan más huecos). Este orden asegura que durante el juego real, no se pierda el tiempo dibujando objetos que no son visibles; como resultado, los mapas pueden llegar a ser muy grandes sin ninguna pena de velocidad.

Para concluir esta experiencia fue nostálgica y didáctica antes no tenía mucho conocimiento de la computación visual aunque siempre me ha interesado el procesamiento de imágenes espero encontrar más herramientas de este tipo en el transcurso del curso con las que pueda interactuar y obtener ideas.


