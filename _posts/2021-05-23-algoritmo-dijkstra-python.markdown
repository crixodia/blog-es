---
title: "El Algoritmo de Dijkstra en Python"
categories: [Algoritmos, Python]
tags: [python, teoría de grafos, algoritmo de dijkstra]
date: 2021-05-23 11:55:00 -0500
math: true
mermaid: true
pin: false
media_subpath: /assets/img/posts/algoritmo-dijkstra-python/
image: python-dijkstra-post.webp
---
Si estás estudiando ingeniería o ciencias de la computación, es probable que hayas encontrado la teoría de grafos, un campo fascinante con numerosas aplicaciones en estas disciplinas.

El algoritmo de Dijkstra es un concepto fundamental en la teoría de grafos, diseñado para encontrar el camino más corto entre nodos en un grafo. Este grafo puede representar cualquier cosa, desde una infraestructura de red hasta el costo de viajar entre ciudades utilizando rutas específicas. Crucialmente, este algoritmo se basa en etiquetas con números positivos, que representan el costo asociado con atravesar una arista.

La esencia del algoritmo de Dijkstra radica en explorar sistemáticamente todos los caminos más cortos desde un nodo de origen a todos los demás nodos. El proceso se detiene cuando identifica el camino más corto a todos los nodos o alcanza un objetivo específico. Sin embargo, es importante tener en cuenta que este algoritmo no es adecuado para grafos con costos de arista negativos. En tales casos, querrías implementar el algoritmo de Bellman-Ford.

## El Algoritmo

Desglosemos los pasos del algoritmo de Dijkstra para encontrar el camino más corto:

1. Inicializa un vector $$V$$ de tamaño $$N$$ (número de nodos) para almacenar las distancias desde el nodo inicial $$X$$ a todos los demás nodos. Inicializa las distancias en $$V$$ con el valor máximo posible, excepto para el nodo de inicio $$X$$.
2. Establece el nodo actual en $$A \leftarrow X$$.
3. Itera sobre todos los nodos adyacentes a $$A$$, excluyendo aquellos que ya han sido visitados. En otras palabras, considera nodos no visitados $$\left \{ P_{1}, P_{2}, P_{3}, ..., P_{N} \right \}$$.
4. Calcula la distancia potencial desde $$A$$ hasta sus vecinos utilizando la fórmula: $$\mathrm{dp(} P_{i} \mathrm{)} = V_{A} + \mathrm{d(}A,V_{i}\mathrm{)}$$, donde $$\mathrm{d(}A,V_{i}\mathrm{)}$$ representa la distancia desde $$A$$ hasta $$P_{i}$$. Si esta distancia calculada es más corta que la distancia actualmente almacenada en $$V$$, actualiza $$V$$ con esta distancia más corta.
5. Marca el nodo $$A$$ como visitado.
6. Establece el siguiente nodo actual $$A$$ como aquel con la distancia más pequeña en $$V$$ y repite el proceso desde el paso 3 hasta que no queden nodos no visitados.

## Implementación en Python

En Python, puedes implementar el algoritmo de Dijkstra con una matriz de adyacencia que representa el grafo ponderado, el nodo inicial y opcionalmente el nodo objetivo. A continuación, se muestra un fragmento de código en Python que sigue los pasos del algoritmo:

```python
def find_all(wmat, start, end=-1):
    n = len(wmat)

    dist = [inf]*n
    dist[start] = wmat[start][start]  # 0

    spVertex = [False]*n
    parent = [-1]*n

    path = [{}]*n

    for count in range(n-1):
        minix = inf
        u = 0

        for v in range(len(spVertex)):
            if spVertex[v] == False and dist[v] <= minix:
                minix = dist[v]
                u = v

        spVertex[u] = True
        for v in range(n):
            if not(spVertex[v]) and wmat[u][v] != 0 and dist[u] + wmat[u][v] < dist[v]:
                parent[v] = u
                dist[v] = dist[u] + wmat[u][v]

    for i in range(n):
        j = i
        s = []
        while parent[j] != -1:
            s.append(j)
            j = parent[j]
        s.append(start)
        path[i] = s[::-1]

    return (dist[end], path[end]) if end >= 0 else (dist, path)
```

Para ver si el algoritmo funciona correctamente, creemos un ejemplo de una matriz de adyacencia para un grafo ponderado:

```mermaid
---
title: Grafo ejemplo
---
graph LR
    A((a))
    A -- 2 --- B((b))
    A -- 1 --- F((f))
    B -- 2 --- C((c))
    B -- 2 --- D((d))
    B -- 4 --- E((e))
    C -- 3 --- E((e))
    C -- 1 --- Z((z))
    D -- 4 --- E((e))
    D -- 3 --- F((f))
    E -- 7 --- G((g))
    F -- 5 --- G((g))
    G -- 6 --- Z((z))
```

```python
wmat = [[0, 2, 0, 0, 0, 1, 0, 0],
        [2, 0, 2, 2, 4, 0, 0, 0],
        [0, 2, 0, 0, 3, 0, 0, 1],
        [0, 2, 0, 0, 4, 3, 0, 0],
        [0, 4, 3, 4, 0, 0, 7, 0],
        [1, 0, 0, 3, 0, 0, 5, 0],
        [0, 0, 0, 0, 7, 5, 0, 6],
        [0, 0, 1, 0, 0, 0, 6, 0]]
```

## Módulo de Python

Puedes descargar e importar el módulo de Python para el algoritmo de Dijkstra [aquí](https://github.com/crixodia/python-dijkstra) e importarlo.

```python
import dijkstra
```

## Encontrar Distancias y Caminos

### Encontrar Todas las Distancias y Caminos

Para encontrar todas las distancias y caminos desde el nodo inicial a todos los demás nodos, utiliza la siguiente función:

```python
dijkstra.find_all(wmat, start, end=-1):
```

Devuelve una tupla que contiene listas de distancias y caminos. Por ejemplo:

```python
print(dijkstra.find_all(wmat, 0))
```

**Salida:**

```bash
(
    [0, 2, 4, 4, 6, 1, 6, 5],
    [
        [0],
        [0, 1],
        [0, 1, 2],
        [0, 5, 3],
        [0, 1, 4],
        [0, 5],
        [0, 5, 6],
        [0, 1, 2, 7]
    ]
)
```

### Encontrar el Camino Más Corto

Para encontrar el camino más corto desde el nodo inicial al nodo objetivo, utiliza esta función:

```python
dijkstra.find_shortest_path(wmat, start, end=-1)
```

Por ejemplo, para encontrar el camino más corto desde el nodo 0 al nodo 7:

```python
print(dijkstra.find_shortest_path(wmat, 0, 7))
```

**Salida:**

```bash
[0, 1, 2, 7]
```

Si deseas el camino sin especificar el nodo objetivo:

```python
print(dijkstra.find_shortest_path(wmat, 0))
```

**Salida:**

```bash
[[0], [0, 1], [0, 1, 2], [0, 5, 3], [0, 1, 4], [0, 5], [0, 5, 6], [0, 1, 2, 7]]
```

### Encontrar la Distancia Más Corta

Para encontrar la distancia más corta desde el nodo inicial al nodo objetivo, utiliza esta función:

```python
dijkstra.find_shortest_distance(wmat, start, end=-1)
```

Por ejemplo, para encontrar la distancia más corta desde el nodo 0 al nodo 7:

```python
print(dijkstra.find_shortest_distance(wmat, 0, 7))
```

**Salida:**

```bash
5
```

Si deseas la distancia sin especificar el nodo objetivo:

```python
print(dijkstra.find_shortest_distance(wmat, 0))
```

**Salida:**

```bash
[0, 2, 4, 4, 6, 1, 6, 5]
```

Puedes descargar el módulo de Python [aquí](https://github.com/crixodia/python-dijkstra). ¡No dudes en dejar tus comentarios y sugerencias!
