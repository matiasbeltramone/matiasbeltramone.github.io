---
layout: post
title: "🧱 Programando con Objetos: Modelado"
tags: [Paradigmas, POO, OOP, Orientación a Objetos, Object Oriented]
---

Bienvenidos a otra lección en nuestra serie sobre Programación Orientada a Objetos. Hemos estado hablando de **abstracciones**, de simplificar la realidad y de representarla con **objetos y clases** según nuestro "modelo de dominio". Pero, **¿qué significa exactamente modelar en este contexto?** En esta lección, vamos a profundizar en cómo el modelado se relaciona con la abstracción y exploraremos cómo aplicar estos conceptos para simplificar la complejidad de la realidad.

## 🌍 La Ciencia de los Patrones y Modelos

Vivimos en un mundo en constante evolución, complejo, caótico y lleno de ruido. Sin embargo, la inteligencia humana consigue dar sentido a todo este caos en una búsqueda de la **elegancia** que se esconde detrás de la simetría a través de los **patrones** que identificamos en nuestra realidad.

<p align="center"><img width="50%" src="https://github.com/matiasbeltramone/object-oriented-programming/assets/22304957/177c3eea-11a3-4274-a1da-4e49bb6a6a56"/></p>
<p align="center"><img width="50%" src="https://github.com/matiasbeltramone/object-oriented-programming/assets/22304957/177c3eea-11a3-4274-a1da-4e49bb6a6a56?raw=true"/></p>
<p align="center"><img width="50%" src="https://github-production-user-asset-6210df.s3.amazonaws.com/22304957/285682574-dc2a6d92-fc6a-4466-a001-6578a3d0ffa1.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20231220%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20231220T003815Z&X-Amz-Expires=300&X-Amz-Signature=041f08de139a1308b2a977fdb780bf7d9d5584033ae0ba826898b233d73aa802&X-Amz-SignedHeaders=host&actor_id=22304957&key_id=0&repo_id=584591436"/></p>

### 🗺️ Modelos en Nuestra Vida Diaria

Un modelo es una **construcción conceptual simplificada** de una realidad más compleja. A través de esta reconstrucción somos capaces de entender mejor dicha realidad para lograr utilizarla a nuestro favor. Ejemplos cotidianos incluyen mapas, ecuaciones físicas y diagramas, como olvidar que nuestro querido Albert Einstein logro simplificar semejante teoría y conceptos muy complejos en la fórmula que podemos dilucidar debajo en la imagen (E=mc^2), claramente esto demuestra la elegancia y simplificación de una realidad mucho más compleja.

<p align="center"><img width="50%" src="https://github.com/matiasbeltramone/object-oriented-programming/assets/22304957/dc2a6d92-fc6a-4466-a001-6578a3d0ffa1"/></p>

### 📐 Aplicación de Modelos en POO

En la Programación Orientada a Objetos, el modelado implica seleccionar y representar solo aquellos aspectos de un objeto real que son pertinentes para nuestro objetivo. Es aquí donde la **abstracción** juega un papel crucial. Abstraer significa centrarse en las características esenciales de un objeto desde una perspectiva específica, ignorando los detalles irrelevantes para el contexto en el que estamos trabajando.

#### Ejemplo: Modelando un `Auto`

- **En un juego de carreras**: Un `Auto` se modelaría con atributos como `velocidad`, `aceleración`, y métodos como `acelerar()` `frenar()` o `girar()`, bueno hasta podría ser `usarNitro()` 😇.

<p align="center"><img width="50%" src="https://github.com/matiasbeltramone/object-oriented-programming/assets/22304957/12132e09-cba1-44bb-8d1b-26e8aa45a757"/></p>

- **En un sistema de gestión de concesionario de coches**: Aquí, el mismo `Auto` podría tener atributos como `precio`, `modelo`, `color`, y métodos como `vender()` o `presentarInformación()`.

### 🎶 Partituras y Diagramas: Modelos en la Práctica

Desde partituras musicales hasta diagramas complejos, estos son ejemplos de cómo representamos y simplificamos la información para hacerla manejable y útil.

<p align="center"><img width="50%" src="https://github.com/matiasbeltramone/object-oriented-programming/assets/22304957/703644bf-e230-4b8c-a0c8-cfc8795a83b0"/></p>

### 🐦 Modelando la Naturaleza: Un Ejercicio Práctico

Imaginemos que queremos modelar **"El comportamiento natural de las aves"**. Inicialmente, podríamos enunciar un modelo simple: "Las aves pueden volar". Pero a medida que iteramos y ajustamos nuestro modelo, nos enfrentamos a excepciones como los pingüinos. Este ejercicio nos muestra cómo encontrar el equilibrio entre la simplicidad y la necesidad de capturar la esencia de la realidad.

### 👨‍💻 Más a nivel de código: El Desafío de Modelar con `Date()`

En programación, a menudo nos encontramos con desafíos de modelado, donde conceptos aparentemente simples en la realidad se vuelven complejos en código. Un claro ejemplo es el uso del objeto `Date()` en JavaScript, especialmente al representar fechas y meses.

Tomemos el caso de representar "Enero". A primera vista, parece sencillo, pero en JavaScript, al crear una fecha para el 15 de enero de 2023 con `const fechaEnEnero = new Date(2023, 0, 15);`, nos enfrentamos a varias complejidades:

- **Indexación de Meses:** En JavaScript, los meses se indexan comenzando desde 0, no desde 1. Por lo tanto, enero se representa con 0, algo que puede resultar contra intuitivo y propenso a errores.

- **Creación Completa de Fecha:** Para especificar un mes, debemos definir también el año, el día, y por defecto se asignan la hora y el huso horario. Este nivel de detalle, aunque necesario para algunas aplicaciones, puede resultar excesivo cuando solo nos interesa el mes.

- **Interfaz de la API:** La API de `Date` en JavaScript puede ser desafiante para ciertas operaciones, como ajustes de zona horaria o cálculos de fechas, lo que puede complicar aún más su uso.

Este ejemplo destaca cómo algo tan simple como "representar un mes" puede convertirse en un ejercicio de modelado complejo en programación. Lo que en la vida real se concibe de manera directa, en el código requiere de una abstracción cuidadosa y consideración de varios factores adicionales.

#### Reflexión: Abstracción vs. Complejidad

Este caso ilustra el delicado equilibrio entre representar fielmente la realidad y mantener la simplicidad en nuestros modelos de programación. A menudo, nuestro desafío es encontrar la abstracción adecuada que simplifique sin perder la esencia del concepto que queremos modelar. ¿Cómo abordarías tú este equilibrio en tus propios proyectos de programación?

Sobre este tema en particular de modelado de fechas podemos hablar mucho más largo y tendido y ver su implementación en diferentes lenguajes de programación en otra ocasión, pero no vamos a encontrar muchas excepciones a lo que estamos hablando, solo que algunos son más sencillo de hacer operaciones que otros.

### 💡 Conclusión y Reflexión

Los modelos buscan simplificar nuestra compleja realidad. En la programación, esto se traduce en hacer abstracciones y modelados para simplificar nuestras vidas diariamente. **¿Cómo encontrarías tú el equilibrio entre representar la realidad y mantener la simplicidad en tus modelos?** Comparte tus experiencias y reflexiones sobre cómo has aplicado la abstracción y el modelado en tus proyectos de programación.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/211164605-ed461c29-b3c2-4eef-acf3-ad8cd9bdbbdc.png"/></p>
