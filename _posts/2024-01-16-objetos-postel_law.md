---
layout: post
title: "🧱 Programando con Objetos: Postel's Law"
tags: [Paradigmas, POO, OOP, Orientación a Objetos, Object Oriented]
---

En nuestro post anterior, exploramos en profundidad el principio de Separación de Comandos y Consultas (CQS), una piedra angular para construir software de calidad. Como breve conclusión de ese post,
sabemos que para aplicar CQS es necesario que sea una convención del equipo, donde decidimos si vamos a implementar o no nuestro principio CQS y en qué lugar.
No podemos ir a medias tintas, ya que si hiciéramos eso, no sabríamos qué esperar de nuestras interfaces, básicamente porque terminaríamos consultando si realmente aplica o no el principio, lo cuál es algo que queremos evitar.

Para poder lograr esto de una manera más sencilla, podríamos decidir aplicarlo en ciertos límites de nuestro sistema, por ejemplo: ciertos módulos,
servicios en particular a los cuales sabemos que aplicarán CQS, mientras que los demás artefactos "x" e "y" podemos decir que no necesariamente aplican este principio, por lo tanto,
no esperaremos nada de ellos a nivel de diseño. Aún aplicando este principio, podrías preguntarte: ¿Hay algo que puedas hacer a nivel de código para aumentar la confiabilidad del sistema?
Y ahí es donde ingresa el tema de este nuevo post, donde profundizaremos en otro concepto igualmente crucial: la **Ley de Postel**, también conocida como el **Principio de Robustez**.

## 🌉 La Ley de Postel: Fundamentos y Significado

La Ley de Postel, originaria del diseño de protocolos de red, nos dice: "Sé conservador en lo que envías, pero liberal en lo que aceptas".
Este principio es vital en entornos donde la incertidumbre y las imperfecciones son comunes, como en las redes, donde los paquetes pueden perderse o llegar fuera de orden.

## 📡 Aplicando la Ley en el Desarrollo de Software

Más allá de las redes, este principio tiene aplicaciones poderosas en el desarrollo de software. Al ser conservadores en lo que respondemos,
nos adherimos estrictamente a las especificaciones y contratos declarados en nuestro sistema, proporcionando garantías fuertes a nuestros clientes sobre nuestras respuestas.
Y todos sabemos que esto es muy importante, ya que en nuestra vida cotidiana por ejemplo, si compramos cualquier producto, generalmente los más caros, estaríamos mucho más tranquilos sabiendo que
vamos a tener una garantía sobre nuestro producto, donde en caso de que ocurran ciertos sucesos o condiciones, esta garantía nos asegurará que todo estará bien, que podría ser reemplazado, arreglar, y no tendremos mayores problemas sobre imprevistos.

Claramente, esto en el software también se traduce en confianza con los sistemas con los que nos conectamos, ya que esto se traduce en un software más predecible y confiable.
Después de todo, uno no espera en un entorno productivo que si el contrato de una API o cualquier sistema externo nos especifica que si le mandamos dos números al endpoint como resultado obtendremos su suma de ambos,
pero bajo ciertas condiciones de los parámetros, por ejemplo, si le mandamos dos strings en lugar de dos números, es decir, `{ a: "2", b: "2" }`, nos termine devolviendo algo como "22", ya que por ejemplo en algún lenguaje se puede interpretar que la suma de dos strings es su concatenación.
Sería algo poco esperable realmente, si bien nos aceptó los argumentos de entrada como nos postula la ley, no es muy conservador en respetar conceptualmente la firma que tenía el contrato de devolver la suma de ambos argumentos, por lo tanto debería cambiar la forma de procesar los datos para adherirse a nuestro "Principio de Robustez".

Por otro lado, ser liberales en lo que aceptamos significa manejar las entradas de forma más flexible. Si podemos entender la intención detrás de una entrada imperfecta, deberíamos
procesarla adecuadamente. Esto no solo mejora la robustez del sistema, sino también la experiencia del usuario, como podría ser el caso de aceptar dos strings como números en el ejemplo anterior pero manejandolos de manera adecuada
como mostramos a continuación.

Caso con strings:
 - Entrada: `{ a: "2", b: "2" }`
 - Respuesta: `4`

Caso con números:
 - Entrada: `{ a: 2, b: 2 }`
 - Respuesta: `4`

## ⚖️ Equilibrio entre Flexibilidad y Rigor

Sin embargo, existe un equilibrio delicado. Si una entrada es incomprensible o errónea, debemos adherirnos al principio de "fallar rápido" (fail fast), informando al usuario de manera clara y útil para corregir el problema. Este enfoque nos ayuda a evitar errores y mejora la comunicación con los usuarios.

## 🚀 Conclusión: Construyendo Software Confiable y Amigable

Al aplicar la Ley de Postel, no solo construimos sistemas más robustos, sino también más amigables y confiables. Esta ley nos guía a ser meticulosos en nuestras salidas y comprensivos en nuestras entradas, una filosofía esencial para cualquier desarrollador que busque la excelencia.

Quedará ver, para los próximos posts, ejemplos sobre diferentes casos de inputs y outputs del sistema, así como también el falla rápido que mencionabamos anteriormente.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/211164605-ed461c29-b3c2-4eef-acf3-ad8cd9bdbbdc.png"/></p>
