---
title: "Álgebra Lineal Hecha Eficiente: Multiplicación de Matrices Strassen"
categories: [Algoritmos, Python]
tags: [Ciencias de la Computación, Python, Complejidad Temporal, Strassen]
date: 2021-10-13 11:55:00 -0500
math: true
mermaid: false
pin: false
img_path: /assets/img/posts/algebra-lineal-eficiente-multiplicacion-matrices-strassen/
image: strassen-post.webp
---
Durante tu tiempo en la universidad, es probable que hayas estudiado álgebra lineal y aprendido acerca de la multiplicación de matrices. Es posible que incluso hayas implementado un algoritmo de multiplicación de matrices utilizando bucles anidados, lo que conlleva una complejidad temporal de $$O(n^3)$$. En esta entrada de blog, exploraremos el algoritmo de Strassen, un método eficiente para multiplicar matrices de gran tamaño.

## Multiplicación Tradicional de Matrices

La multiplicación de matrices implica bucles anidados que iteran a través de las dimensiones de dos matrices. Esto resulta en una complejidad temporal de $$O(n^3)$$, lo que puede ser computacionalmente costoso para matrices grandes. Aquí tienes una implementación en Python de la multiplicación de matrices tradicional:

```python
def mul_matrix(X, Y):
  Z = [[0 for i in range(len(X))] for j in range(len(Y))]
  for i in range(len(X)):
    for j in range(len(Y[0])):
      for k in range(len(X[0])):
        Z[i][j] += X[i][k] * Y[k][j]
  return Z
```

## El Algoritmo de Strassen

Volker Strassen ideó un algoritmo más eficiente para multiplicar matrices grandes, siempre y cuando sean matrices cuadradas y tengan dimensiones que sean potencias de dos. El algoritmo de Strassen descompone la multiplicación de matrices en subproblemas más pequeños, reduciendo así la complejidad general.

>Para obtener más información sobre el algoritmo de Strassen, puedes consultar [este artículo de Wikipedia](https://en.wikipedia.org/wiki/Strassen_algorithm).
{: .prompt-info }

### El Proceso

Supongamos que tenemos dos matrices cuadradas, $$X$$ e $$Y$$, y que deseamos obtener su producto, $$Z$$. Si $$X$$ e $$Y$$ no tienen la forma $$2^n \times 2^n$$ (es decir, su forma no es una potencia de dos), debemos rellenar los espacios vacíos con ceros.

Comencemos con fragmentos del mismo tamaño.

$$
X=\begin{pmatrix}
A & B\\
C & D
\end{pmatrix},
Y=\begin{pmatrix}
E & F\\
G & H
\end{pmatrix},
Z=\begin{pmatrix}
Z_{11} & Z_{12}\\
Z_{21} & Z_{22}
\end{pmatrix}
$$

Vale la pena mencionar que $$A$$, $$B$$, $$C$$, $$D$$, $$E$$, $$F$$, $$G$$, $$H$$ y $$Z_{ij}$$ siguen los requisitos del algoritmo de Strassen. Luego, al multiplicar $$X$$ e $$Y$$, obtenemos:

$$
Z_{11} = AE+BG
$$

$$
Z_{12} = AF+BH
$$

$$
Z_{21} = CE+DG
$$

$$
Z_{22} = CF+DH
$$

Con esto, la cantidad de multiplicaciones no se reduce. Pero si definimos las siguientes matrices:

$$
P_{1} = A(F-H)
$$

$$
P_{2} = (A+B)H
$$

$$
P_{3} = (C+D)E
$$

$$
P_{4} = D(G-E)
$$

$$
P_{5} = (A+D)(E+H)
$$

$$
P_{6} = (B-D)(G+H)
$$

$$
P_{7} = (A-C)(E+F)
$$

Podemos reescribir $$Z$$ de la siguiente manera:

$$
Z=\begin{pmatrix}
P_{5}+P_{4}-P_{2}+P_{6} & P_{1}+P_{2}\\
P_{3} + P_{4} & P_{5}+P_{1}-P_{3}-P_{7}
\end{pmatrix}
$$

Este proceso debe repetirse de manera recursiva hasta obtener una matriz $$2 \times 2$$.

### Implementación en Python

Para comprender mejor este proceso, implementemos este algoritmo en Python. Necesitaremos funciones auxiliares que nos permitan dividir las matrices y fusionarlas.

#### Dividir Matrices

```python
def divide_matrix(A):
  mid = len(A)//2
  m_11 = [M[:mid] for M in A[:mid]]
  m_12 = [M[mid:] for M in A[:mid]]
  m_21 = [M[:mid] for M in A[mid:]]
  m_22 = [M[mid:] for M in A[mid:]]

  return (m_11, m_12, m_21, m_22)
```

#### Fusionar Matrices

```python
def merge_matrix(matrix_11, matrix_12, matrix_21, matrix_22):
  matrix_total = []
  rows1 = len(matrix_11)
  rows2 = len(matrix_21)
  for i in range(rows1):
    matrix_total.append(matrix_11[i] + matrix_12[i])
  for j in range(rows2):
    matrix_total.append(matrix_21[j] + matrix_22[j])
  return matrix_total
```

#### Sumar Matrices

```python
def add_matrix(X, Y):
  n = len(X)
  if n == 1:
    return [[X[0][0] + Y[0][0]]]
  S = []
  for i in range(n):
    S.append([])
    for j in range(n):
      S[i].append(X[i][j] + Y[i][j])
  return S
```

#### Restar Matrices

```python
def sub_matrix(X, Y):
  n = len(X)
  if n == 1:
    return [[X[0][0] - Y[0][0]]]
  S = []
  for i in range(n):
    S.append([])
    for j in range(n):
      S[i].append(X[i][j] - Y[i][j])
  return S
```

#### Implementación del Algoritmo

Finalmente, podemos implementar el algoritmo de Strassen como se explicó anteriormente. Ten en cuenta que siempre utiliza el algoritmo de Strassen para multiplicar las submatrices, por lo que es recursivo.

```python
def strassen(X, Y):
  if len(X) == 1:
    return [[X[0][0] * Y[0][0]]]
  else:
    A, B, C, D = divide_matrix(X)
    E, F, G, H = divide_matrix(Y)
    
    P1 = strassen(A, sub_matrix(F,H))
    P2 = strassen(add_matrix(A, B), H)
    P3 = strassen(add_matrix(C, D), E)
    P4 = strassen(D, sub_matrix(G, E))
    P5 = strassen(add_matrix(A, D), add_matrix(E, H))
    P6 = strassen(sub_matrix(B, D), add_matrix(G, H))
    P7 = strassen(sub_matrix(A, C), add_matrix(E, F))
    
    Z11 = add_matrix(sub_matrix(add_matrix(P5, P4), P2), P6)
    Z12 = add_matrix(P1, P2)
    Z21 = add_matrix(P3, P4)
    Z22 = sub_matrix(sub_matrix(add_matrix(P5, P1), P3), P7)

    return merge_matrix(Z11, Z12, Z21, Z22)
```

Puedes probarlo de la siguiente manera:

```python
A = [[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]]
B = [[1,1,1,1],[1,1,1,1],[1,1,1,1],[1,1,1,0]]

print("Strassen")
print(*strassen(A,B), sep='\n')
print("\nClásico")
print(*mul_matrix(A,B), sep='\n')
```

**Salida:**

$$
\begin{pmatrix}
1 & 2 & 3 & 4\\
5 & 6 & 7 & 8\\
9 & 10 & 11 & 12\\
13 & 14 & 15 & 16
\end{pmatrix}
\begin{pmatrix}
1 & 1 & 1 & 1\\
1 & 1 & 1 & 1\\
1 & 1 & 1 & 1\\
1 & 1 & 1 & 0
\end{pmatrix}
=\begin{pmatrix}
10& 10& 10& 6\\
26& 26& 26& 18\\
42& 42& 42& 30\\
58& 58& 58& 42
\end{pmatrix}
$$

## Conclusión

El límite superior del método clásico es $$O(n^3)$$. Sin embargo, para el algoritmo de Strassen, es $$O(n^{\log_{2}7})$$ o aproximadamente $$O(n^{2.807})$$. Puede que pienses que la diferencia es solo de algunos decimales, pero con el siguiente gráfico te darás cuenta de que la diferencia es significativa. La complejidad de la versión clásica crece más rápido a medida que $$n$$ aumenta.

![Strassen Vs. Clásico](strassen-vs-classic.webp)
_Comparación entre el método clásico y Strassen_

Puedes encontrar el código completo en Python del algoritmo de Strassen [aquí](https://gist.github.com/crixodia/4e87ce94ce8c12e2006f63c1e534c2a0). No dudes en dejar comentarios, preguntas o seguirme en las redes sociales para obtener más contenido como este.
