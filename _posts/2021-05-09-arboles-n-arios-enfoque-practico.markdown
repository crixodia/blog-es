---
title: "Árboles N-arios: Un Enfoque Práctico"
categories: [Estructura de Datos, Java]
tags: [árboles n-arios, java, desarrollo de escritorio, parsing]
date: 2021-05-09 15:14:00 -0500
math: false
mermaid: true
pin: false
img_path: /assets/img/posts/arboles-n-arios-enfoque-practico/
image: n-ary-trees-post.webp
---
A diferencia de los árboles binarios, que solo tienen dos nodos hijos, los árboles n-arios pueden tener cualquier cantidad de nodos hijos. En esta entrada, nos sumergiremos en el mundo de los árboles n-arios y exploraremos su aplicación en la gestión de árboles genealógicos.

## Comprendiendo los Árboles N-arios

En esencia, un árbol n-ario es una estructura de datos abstracta no lineal definida de forma recursiva con un conjunto de nodos. Estos nodos mantienen una lista que apunta a otros nodos.

```mermaid
---
title: "Árbol N-ario"
---
graph TD
    A[Raíz] --> B(Hijo 1)
    A --> C(Hijo 2)
    A --> D(Hijo 3)
    A --> E(...)
    A --> F(Hijo N)
    B --> G(Hijo 1)
    B --> H(Hijo 2)
    B --> I(Hijo 3)
    B --> J(...)
    B --> K(Hijo N)
```

## Árboles Genealógicos y Árboles N-arios

Una aplicación convincente de los árboles n-arios es la gestión de árboles genealógicos. Los árboles n-arios se ajustan perfectamente a los requisitos de los árboles genealógicos, ofreciendo una estructura jerárquica con relaciones padre-hijo. A continuación, puedes ver un ejemplo de un árbol genealógico inspirado en la mitología griega.

![Muestra de árbol genealógico](greek-family-tree-light.webp){: .light }
![Muestra de árbol genealógico](greek-family-tree-dark.webp){: .dark }
_Árbol Genealógico Griego_

## Definición de los Objetos

Para crear un árbol genealógico, implementaremos un objeto genérico. Además, definiremos la estructura del árbol como se describe a continuación. Es importante tener en cuenta que los métodos implementados en la clase del árbol pueden variar según tus necesidades específicas y el tipo de objetos utilizados como valores en los nodos.

### Implementación del Nodo

La implementación de nuestro nodo del árbol es sencilla. Consiste en un objeto padre, un objeto raíz y una lista de objetos hijos. Para agregar nodos, puedes usar el siguiente método en Java:

```java
public Node add(Object o){
    Node newNode = new Node(o);
    this.children.add(newNode);
    return newNode;
}
```

>Puedes revisar la implementación completa de la [clase Node aquí.](https://github.com/crixodia/nary-family-tree/blob/master/ArbolGen/src/CapaNegocio/Node.java)
{: .prompt-info }

### Implementación del Árbol

El principal desafío en este proyecto es implementar la estructura del árbol. Dado que los árboles pueden almacenarse en archivos, deberás diseñar un analizador para traducir el árbol en texto y viceversa. Además, para mostrar el árbol en una interfaz gráfica de usuario (GUI), necesitarás un analizador que pueda convertirlo en un objeto de tipo `DefaultMutableTreeNode`.

Para este proyecto, me basé en el siguiente seudocódigo para comprender el proceso (suponiendo un conocimiento básico de pilas):

```text
define stack
stack.push(first line)
while objects exist
    S1 = stack.peek()
    S2 = read object
    if depth(S1) < depth(S2)
        S1.addChild(S2)
        stack.push(S2)
    else
        while depth(S1) >= depth(S2) and stack.size() >= 2
            stack.pop()
            S1 = stack.peek()
    S1.addChild(S2)
    stack.push(S2)
return stack.get(0)
```

### Manejo de Archivos: Guardar y Abrir Árboles

La lectura y el guardado de árboles en archivos pueden simplificarse al incluir el padre de cada elemento. Cada nodo se representa en una línea separada en el siguiente formato: `padre:valor`. Los atributos adicionales del objeto se especifican con dos puntos.

Por ejemplo:

```text
Aang:Katara:0:0
Aang:Katara,Tenzin:Pema:0:0
Tenzin:Pema,Meelo::0:0
Tenzin:Pema,Jinora::0:0
Tenzin:Pema,Ikki::0:0
Aang:Katara,Bumi::0:0
Aang:Katara,Kya::0:0
```

Para leer estas líneas, puedes extraer el padre y los valores del nodo y luego utilizar el método `addNewNode(padre, valor)`, como `addNewNode(Aang, Tenzin)`.

Para guardar árboles, recorre todo el árbol de manera preordenada y concatena el padre y los valores del nodo en el formato especificado.

```mermaid
---
title: Recorrido en Preorden
---
graph TD
    subgraph Tree
        A((A)) --> B((B))
        A --> C((C))
        B --> D((D))
        B --> E((E))
        C --> F((F))
        C --> G((G))
        D --> H((H))
        D --> I((I))
        E --> J((J))
        E --> K((K))
        F --> L((L))
        F --> M((M))
        G --> N((N))
        G --> O((O))
    end
    Tree --> P
    subgraph P[Recorrido]
        x([ A - B - D - H - I - E - J - K - C - F - L - M - G - N - O ])
    end
```

>Puedes encontrar métodos para buscar, eliminar, calcular la profundidad y modificar nodos en la [clase Tree](https://github.com/crixodia/nary-family-tree/blob/master/ArbolGen/src/CapaNegocio/Tree.java).
{: .prompt-info }

### Garantizar la Unicidad de Objetos

Para manejar casos en los que un hijo tiene el mismo ID que su padre, garantizamos la unicidad a través del cónyuge del padre. Al crear un objeto e insertarlo en el árbol, verificamos que aún no exista basándonos en el ID de su cónyuge. Esto se logra mediante la anulación del método `equals(o)`:

```java
@Override
public boolean equals(Object obj) {
    GenObj aux = (GenObj) obj;
    return nombre.equals(aux.nombre) && conyuge.equals(aux.conyuge);
}
```

## Interfaz Gráfica de Usuario (GUI)

La interfaz gráfica de usuario te permite crear, modificar, insertar, eliminar, guardar y abrir archivos. También proporciona visualización de atributos para cada nodo.

>Puedes descargar ejemplos de árboles desde la [carpeta de ejemplos](https://github.com/crixodia/nary-family-tree/tree/master/examples) y abrirlos para probar la funcionalidad.
{: .prompt-tip }

![Interfaz Gráfica de Usuario](https://github.com/crixodia/nary-family-tree/raw/master/assets/gui.jpg)
_Interfaz Gráfica de Usuario_

## Descarga el Proyecto Completo

Si estás interesado en explorar el proyecto completo, puedes descargarlo desde [GitHub](https://github.com/crixodia/nary-family-tree). ¡No dudes en dejar tus comentarios y opiniones!
