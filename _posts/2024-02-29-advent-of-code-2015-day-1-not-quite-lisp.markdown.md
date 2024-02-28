---
title: "Resolviendo Advent of Code 2015, Día 1: Not Quite Lisp"
categories: [Advent of Code, Python]
tags: [resolución de problemas, python, algoritmos, estructura de datos]
date: 2024-02-29 15:00:00 -0500
math: false
mermaid: false
pin: false
img_path: /assets/img/posts/advent-of-code/2015/
image: day-1-not-quite-lisp.webp
---

Desde que descubrí [Advent of Code](https://adventofcode.com/2023/events), me he propuesto resolver todos sus acertijos para cada año. En esta serie de blogs, no solo proporcionaré las soluciones, sino también breves explicaciones con su código. Comenzando con el evento Advent of Code 2015, empecemos con el [Day 1: Not Quite Lisp](https://adventofcode.com/2015/day/1).

## Manejando la entrada

Para simplificar la implementación de ambas partes, crearemos una función llamada `read_input` que lea el archivo de entrada y lo convierta en una estructura de datos conveniente. Dado que la solución de entrada para este día solo consta de una única línea de caracteres, la función `read_input` se verá así:

```python
def read_input(file):
    s = []
    with open(file) as f:
        for c in f.read():
            s.append(c)
    return s
```

## Parte 1

Nuestro input del acertijo consistirá en una cadena de paréntesis. Un paréntesis de apertura, `(`, significa que Santa debe subir un piso, mientras que un paréntesis de cierre, `)`, indica que debe bajar un piso. Para determinar el piso al que las instrucciones llevan a Santa, sigue estos sencillos pasos: itera sobre la cadena de paréntesis y suma `1` a una variable si el carácter actual es `(`. Y resta `1` al encontrar un `)`. . He creado un diccionario que mapea `(`, `)` a `1` y `-1` respectivamente. Por lo tanto, la solución de la primera parte es:

```python
def solve(input):
    instruction = {'(': 1, ')': -1}
    sum = 0
    pos = 0
    for i, c in enumerate(input):
        sum += instruction[c]
    return sum
```

## Parte 2

Dadas las mismas instrucciones, ahora tenemos la tarea de encontrar la posición del carácter que hace que Santa entre al sótano por primera vez. Para lograr esto, crearemos una nueva variable llamada `pos` (para posición). Ten en cuenta que cuando la posición del piso se convierte en `-1`, debemos sumar el índice `i` a `pos`. Al hacerlo, es sencillo combinar la solución para ambas partes. La solución de la segunda parte es:

```python
def solve(input):
    instruction = {'(': 1, ')': -1}
    sum = 0
    pos = 0
    for i, c in enumerate(input):
        sum += instruction[c]
        if sum == -1 and pos == 0:
            pos = i + 1
    return sum, pos
```

Finalmente, puedes encontrar el código fuente completo [aquí](https://github.com/crixodia/aoc/blob/main/2015/01_not_quite_lisp/main.py). Espero que te sea de utilidad y no dudes en comentar tus sugerencias.
