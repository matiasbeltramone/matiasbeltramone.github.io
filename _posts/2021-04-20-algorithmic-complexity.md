---
layout: post
title: "Algorithmic Complexity With Big-O, Big-θ, Big-Ω"
tags: [Algorithmic, Complexity, Big-O]
---

Cada algoritmo o conjunto de instrucciones requiere un tiempo y espacio para ejecutarse, por lo que cuando creamos algoritmos para resolver nuestros problemas técnicos debemos examinar la *complejidad temporal y espacial* de la ejecución.

Por lo general, solo escuchamos hablar de la notación **Big-O** cuando hablamos de la complejidad y el rendimiento algorítmicos, ¡y hay una razón para esto!
Pero también es importante comprender **Big-θ** (Big-Theta) y **Big-Ω** (Big-Omega).

La notación **Big-O** describe el **peor rendimiento o complejidad de un algoritmo**. En otras palabras, **Big-O** nos dice la cantidad máxima
de tiempo o la cantidad máxima de almacenamiento que este algoritmo necesitará ejecutar.

Podemos graficar el tiempo de ejecución del peor caso de un algoritmo, o límite superior (upper bound), en un eje bidimensional.

El `eje x` representa el tamaño de datos de entrada y el `eje y` representa la cantidad de tiempo o espacio que requiere este algoritmo.

En otras palabras: a medida que aumenta el tamaño de nuestros datos, ¿cómo afecta al rendimiento del algoritmo o a los requisitos de espacio?
Ilustremos esto con un ejemplo.

```
function find(array, value) {
  for (let i = 0; i < array.length; i++) {
    if (array[i] === value) {
      return true
    }
  }
  
  return false
}
```

En esta función **buscamos un valor** específico. El primer argumento es una matriz de valores para buscar y el segundo argumento es el valor que estamos buscando.

Estamos usando un ciclo `for` para iterar a través de cada valor en la matriz y verificar si es igual al valor objetivo.
Si el valor está en la matriz, devuelve `true`; de lo contrario, devuelve `false`.

¿Cuánto tiempo tardará en ejecutarse este algoritmo? Bueno debemos pensar, ¿cuál es nuestro peor escenario? En el peor de los casos, nuestra matriz será extremadamente grande y el valor que estamos buscando no estará allí, lo que significa que tendremos que verificar cada elemento de la matriz y eventualmente devolver `false`.
A medida que aumenta la longitud de la matriz, el tiempo necesario para ejecutar este algoritmo aumenta de manera proporcional.

Si tenemos una matriz con cuatro elementos, la cantidad máxima de veces que iteraremos a través de nuestra matriz buscando este elemento es cuatro.
Del mismo modo, si tenemos una matriz con 512 elementos, la cantidad máxima de veces que iteraremos a través de nuestra matriz en busca de este elemento es 512.

Este tipo de algoritmo se conoce como **algoritmo de tiempo lineal** y se representa como **O(n)** donde el rendimiento del algoritmo crece proporcionalmente
al tamaño de entrada. Mapear (`array.map()`), filtrar (`array.filter()`) ó clonar (`array.clone()`) una matriz y ejecutar una tarea para cada elemento implica en todos estos casos que tienen un tiempo de ejecución **O(n)** (asumiendo que no contienen otro ciclo). A medida que aumenta el número de datos en la matriz, también lo hace el tiempo de ejecución.

**Big-θ** y **Big-Ω** son muy similares a **Big-O** ya que describen límites en un algoritmo. Mientras que **Big-O** mide el **peor de los casos**, la notación **Big-Ω** (Big-Omega) se utiliza para describir la **complejidad espacial y temporal** del **mejor caso** de un algoritmo. Llamamos a este escenario del mejor de los casos un límite inferior asintótico (asymptotic lower-bound). Volviendo al ejemplo de `find(array, value)`, el mejor escenario sería si el valor que estamos buscando se encuentra en el primer índice de nuestra matriz.

**Big-θ** (Big-Theta) describe el **espacio y la complejidad temporal promedio** de un algoritmo. En nuestro ejemplo de `find(array, value)` el **caso promedio** es que buscaríamos la mitad de la matriz antes de encontrar nuestro valor de búsqueda.

No debería necesitar conocer **Big-θ** y **Big-Ω** para una entrevista técnica, pero es bueno estar familiarizado con ellos.
