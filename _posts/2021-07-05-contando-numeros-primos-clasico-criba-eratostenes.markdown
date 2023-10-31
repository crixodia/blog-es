---
title: "Contando Números Primos: Clásico vs. Criba de Eratóstenes"
categories: [Algoritmos, Python]
tags: [Python, Complejidad Temporal, Criba de Eratosthenes, Números Primos]
date: 2021-07-05 00:00:00 -0500
math: true
mermaid: false
pin: false
img_path: /assets/img/posts/contando-numeros-primos-clasico-criba-eratostenes/
image: erathostenes-post.webp
---
Contar números primos es un problema fundamental en programación. Si bien para muchos la elección clásica es la primera opción, la eficiencia del algoritmo importa. En esta publicación de blog, exploraremos dos enfoques para resolver este problema y compararemos su rendimiento.

## Comprendiendo los Números Primos

Los números primos son enteros divisibles solo por 1 y por sí mismos. Contaremos estos números únicos dentro de un rango dado.

### Enfoque Clásico

El enfoque clásico implica verificar cada número dentro del rango para determinar si es primo. Usamos una función `isPrime` simple con este propósito. Aquí tienes el código:

```python
def isPrime(n:int) -> bool:
  if n <= 1:
      return False
  for i in range(2,int(sqrt(n))+1):
    if n%i == 0:
      return False
  return True

def countPrimesClassic(n:int)->int:
  c = 0
  for i in range(2,n):
    if isPrime(i):
      c+=1
  return c
```

Este enfoque clásico funciona, pero puede ser lento para rangos grandes.

### Criba de Eratóstenes

La Criba de Eratóstenes es un método más eficiente para contar números primos. Funciona eliminando sistemáticamente números no primos. El algoritmo es el siguiente:

1. Crea una lista de enteros consecutivos desde $$2$$ hasta $$n$$.
2. Comienza con el número primo más pequeño, $$p = 2$$.
3. Elimina múltiplos de $$p$$ marcándolos en la lista.
4. Encuentra el número no marcado más pequeño mayor que $$p$$ y establece $$p$$ en este número. Repite desde el paso 3.
5. Cuando el algoritmo termina, los números no marcados son primos menores que $$n$$.

Aquí tienes el código de la Criba de Eratóstenes:

```python
from math import sqrt
def countPrimesSieve(n: int) -> int:
    if n <= 2: return 0
    np, ans = [False]*n, 1
    for i in range(3, int(sqrt(n))+1, 2):
        if np[i]: continue
        for j in range(i*i, n, 2*i): np[j] = True
    for i in range(3, n, 2):
        if not np[i]: ans += 1
    return ans
```

## Comparación de Rendimiento

Para comparar el rendimiento de estos algoritmos, realizamos pruebas desde 1 hasta 5000 con 5 muestras en cada iteración. Los resultados son claros: la Criba de Eratóstenes es más rápida.

![Tiempo del algoritmo clásico](prime-classic.webp)
_Tiempo del algoritmo clásico_

![Tiempo de la Criba de Eratóstenes](prime-sieve.webp)
_Tiempo de la Criba de Eratóstenes_

La Criba de Eratóstenes toma, en promedio, 1 segundo menos que el enfoque clásico y hasta 9 segundos menos en el peor caso.

## Conclusión

Contar números primos es un ejercicio valioso en el diseño de algoritmos. Si bien este problema puede no ser un requisito profesional, ayuda a reforzar conceptos matemáticos y fomenta el diseño eficiente de algoritmos. Al elegir el enfoque correcto, puedes optimizar tu código y ahorrar tiempo valioso.

Si encontraste útil esta publicación, no olvides dejar tus comentarios y preguntas. Sígueme en las redes sociales para obtener más contenido como este.
