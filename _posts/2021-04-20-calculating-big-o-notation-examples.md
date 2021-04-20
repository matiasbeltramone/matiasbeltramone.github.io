---
layout: post
title: "Calculating Complex Big-O Notation"
tags: [Algorithmic, Complexity, Big-O]
---

Ahora que tenemos una comprensión básica de la notación `Big-O` ([Algorithmic Complexity](https://github.com/matiasbeltramone/object-oriented-design/blob/master/algorithmic-complexity.md)), podemos comenzar a buscar cálculos más costosos.
Supongamos que se le pide que ordene una matriz de números enteros de menor a mayor valor. El método de **fuerza bruta** sería comparar cada elemento
de la matriz con cualquier otro elemento. Este algoritmo se conoce como `Bubble Sort` y es conocido por su **espantoso rendimiento**.

```
1. function bubbleSort(arr) {
2.  for (var i = 0; i < arr.length; i++) {
3.    // Notice that j < (length - i)
4.    for (var j = 0; j < arr.length - i - 1; j++) {
5.      // Compare the adjacent positions
6.      if (arr[j] > arr[j + 1]) {
7.        // Swap the numbers
8.        var tmp = arr[j];
9.        arr[j] = arr[j + 1];
10.       arr[j + 1] = tmp;
11.     }
12.    }
13.  }
14.  return arr;
15. }
```

En el ejemplo de código anterior (`find(array, value)`), teníamos un bucle `for`, por lo que nuestra notación `Big-O` era bastante sencilla, pero en este ejemplo tenemos `dos bucles for` y están `anidados` uno dentro del otro. *¿Qué significa esto para nuestro desempeño?*

Si tiene bucles anidados, debe multiplicar sus complejidades para encontrar el límite superior asintótico. Analicemos esto un poco:

- Línea 2: Tenemos que revisar cada elemento de la matriz, cuyo número total llamaremos **n**. Entonces esto sería **O(n)**

- Línea 4: Tenemos que recorrer cada elemento de la matriz, menos uno (ya que el bucle interno solo se ejecuta en el penúltimo elemento). Este sería **O(n-1)**

- Líneas 6, 8-10, 14: podría pensar que esto se ejecuta para cada elemento de la matriz (que en el peor de los casos es así),
pero evaluamos el tiempo de ejecución de forma aislada. De forma aislada, esta sentencia `if` se ejecuta una vez. Esto se llama `tiempo constante` y lo escribimos como **O(1)**. A medida que aumenta el tamaño de los datos **(n)**, no cambia la cantidad de veces que se ejecuta este fragmento de código.

Para obtener nuestro valor general de **Big-O**, ahora tenemos que evaluar estas líneas de código en contexto.

La mayor advertencia para comprender la notación **Big-O** es que descartamos las constantes y los términos menos significativos.
**Big-O** describe la tasa de crecimiento a largo plazo de funciones en oposición a sus magnitudes absolutas.

Aquí hay algunos ejemplos de simplificación de **Big-O**:

1. **O(3)** sigue siendo un **tiempo constante** y se escribirá como **O(1)**.

2. **O(2n)** sigue siendo un **tiempo lineal** y se escribirá como **O(n)**. 

3. **O(n + 3)** => O(n)

4. **O(n^3 + n^2)** => O(n^3)

5. **O(524x + 240y)** => O(x + y) "x" e "y" son valores diferentes, por lo que no puedo tirarlos, ni combinarlos.

Dados estos parámetros, el tiempo de ejecución de la línea 4 de **O(n-1)** cambiará a **O(n)** a medida que descartemos el término menos significativo.
Las líneas 6, 8-10 y 14 son términos de **tiempo constante** menos significativos, por lo que también podemos descartarlos. Nos quedamos con la **O(n)**
del bucle exterior y la **O(n)** del bucle interior. La pregunta ahora es, ¿qué hacemos con estos n valores? ¿Los multiplicamos o sumamos?

Si los **bucles están anidados**, los valores de `Big-O` se **multiplicarán**. Si los **bucles son adyacentes**, se **sumaran** los valores de `Big-O`.

Dadas las reglas anteriores, el tiempo de ejecución final de `Big-O` de nuestro algoritmo de `bubble sort` es **O(n*n)** u **O(n^2)**.

# Tipos de Big-O

Acontinuación veremos los diferentes tipos de tiempos existentes con sus respectivas gráficas.

## Constant Time

**O(1)**, también conocido como **tiempo constante**, describe un algoritmo que se ejecutará en la misma cantidad de tiempo independientemente
de la cantidad de datos. Aquí definimos una función que no toma argumentos y devuelve las palabras "Hola mundo".
No depende de ninguna entrada y, por lo tanto, es un algoritmo de tiempo constante con **O(1)**.

```
function sayHello() {
  return 'Hello world';
}
```

![image](https://user-images.githubusercontent.com/22304957/112723859-5e93bb00-8eef-11eb-9af7-f80c280b25c9.png)

## Logarithmic Time

**O(log n)** describe un algoritmo que es de **naturaleza logarítmica** (el tamaño del problema disminuye a la mitad cada vez que se ejecuta).
La **búsqueda binaria**, es de naturaleza logarítmica: con cada paso a través del algoritmo, la cantidad de datos se reduce a la mitad.

```
function binarySearch(list, item) {
  // if list is sorted in ascending order
  let low = 0
  let high = list.length
  
  while (low <= high) {
    let mid = Math.floor((low + high) / 2)
    let guess = list[mid]

    if (guess === item) {
      return true
    }

    if (guess < item) {
      low = mid + 1
    } else {
      high = mid - 1
    }
  }
  
  return false
}
```

![image](https://user-images.githubusercontent.com/22304957/112723980-ea0d4c00-8eef-11eb-875b-804a77c24200.png)

## Linear Time

**O(n)** describe un algoritmo cuyo rendimiento crece de **forma lineal y proporcional** al tamaño de la entrada.
Un bucle **for** tiene tiempo de ejecución **O(n)**.

El ciclo for a continuación se ejecuta una vez para cada elefante de la matriz. Si n es la longitud de elephants,
entonces esta función tiene **O(n)**, o rendimiento de **tiempo lineal**.

El mapeo de una matriz, el filtrado de una matriz, la clonación de una matriz y la ejecución de una tarea para cada
elemento de una matriz tienen un tiempo de ejecución **O(n)**, asumiendo que no contienen otro ciclo.
A medida que aumenta el número de datos en la matriz, también lo hace el tiempo de ejecución.

```
function printElephants(elephants) {
  for (let i = 0; i < elephants.length; i++) {
    console.log(elephants[i])
  }
}
```

![image](https://user-images.githubusercontent.com/22304957/112724076-4a03f280-8ef0-11eb-8684-24cd3e6df4fc.png)

## Super-Linear Time

Un algoritmo de tiempo superlineal, que se ejecuta en **O(n log n)**, es **menos eficaz** que un algoritmo de **tiempo lineal**, pero más eficaz que un algoritmo exponencial.

Como resultado, los algoritmos como `merge sort` y `heap sort` son una mejor opción para ordenar datos que los algoritmos como `bubble sort`.

```
function mergeSort(arr) {
  if (arr.length < 2) {
    return arr;
  }
  
  const middle = Math.floor(arr.length / 2);
  const left = arr.slice(0, middle);
  const right = arr.slice(middle);
  
  return merge(mergeSort(left), mergeSort( right));
}

function merge(left, right) {
  const sorted = [];
  
  while (left.length && right.length) {
    if (left[0] <= right[0]) {
      sorted.push(left.shift());
    } else {
      sorted.push(right.shift());
    }
  }
  
  let results = [...sorted, ...left, ...right];
  
  return results;
```

![image](https://user-images.githubusercontent.com/22304957/112724188-dc0bfb00-8ef0-11eb-9ebf-9629789bcd47.png)

## Polynomial Time

El tiempo polinomial u O(n^2) describe un algoritmo en el que el rendimiento es **directamente proporcional al cuadrado del tamaño de los datos**.
Esto se encuentra comúnmente en algoritmos que utilizan **dos bucles for anidados**, como `bubble sort`.

La premisa de `bubble sort` es comparar todos los elementos de una matriz con todos los demás elementos e intercambiarlos si el elemento de la
izquierda es mayor que el elemento de la derecha. Esto provoca comparaciones **O(n^2)** porque tenemos que **comparar n elementos con n otros elementos**.

```
1. function bubbleSort(arr) {
2.  for (var i = 0; i < arr.length; i++) {
3.    // Notice that j < (length - i)
4.    for (var j = 0; j < arr.length - i - 1; j++) {
5.      // Compare the adjacent positions
6.      if (arr[j] > arr[j + 1]) {
7.        // Swap the numbers
8.        var tmp = arr[j];
9.        arr[j] = arrr[j + 1];
10.       arr[j + 1] = tmp;
11.     }
12.    }
13.  }
14.  return arr;
}
```

![image](https://user-images.githubusercontent.com/22304957/112724340-98fe5780-8ef1-11eb-8cb7-d3a0bbb00817.png)

## Exponential Time

Los algoritmos de **tiempo exponencial** se duplican con cada nuevo punto de datos. El algoritmo de **Fibonacci** recursivo
(una función recursiva es una función que se llama a sí misma), que encuentra el siguiente número en una secuencia sumando los dos valores
anteriores, es un **algoritmo exponencial**.

```
function fibonacci(num) {
  if (num <= 1) return num
  
  return fibonacci(num - 1) + fibonacci(num - 2)
}
```

![image](https://user-images.githubusercontent.com/22304957/112724388-e37fd400-8ef1-11eb-872e-43dea1e1df9f.png)

## Factorial Time

El **tiempo factorial** u **O(n!)** Es el algoritmo de **peor rendimiento** porque crece rápidamente a medida que aumenta el tamaño de n.

El **problema del viajante** es un problema famoso en informática en el que la solución de fuerza bruta es **O(n!)**.

El viajante de ventas hace la siguiente pregunta: "Dada una lista de ciudades y las distancias entre cada par de ciudades,
¿cuál es la ruta más corta posible que visita cada ciudad y regresa a la ciudad de origen?"

No escribiré código para el problema de `Traveling Salesman` aca, pero puedes leer más en [Wikipedia!](https://en.wikipedia.org/wiki/Travelling_salesman_problem)

![image](https://user-images.githubusercontent.com/22304957/112724468-51c49680-8ef2-11eb-996f-a7d80a2d4795.png)

## Graphing Big-O Notation

Aquí hay un gráfico que muestra los tiempos de ejecución de `Big-O` mencionados anteriormente y su rendimiento a medida que aumenta el tamaño de los datos
de entrada.

El **tiempo constante** es el tiempo de ejecución con **mayor rendimiento** y el **tiempo factorial** es el de **peor rendimiento**.

![image](https://user-images.githubusercontent.com/22304957/112724488-6b65de00-8ef2-11eb-81c4-eb958e482883.png)

Adaptación al español. Todos los derechos reservados para 'Decoding the Process Interview'.
