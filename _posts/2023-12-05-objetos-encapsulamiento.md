---
layout: post
title: "🧱 Programando con Objetos: Encapsulación"
tags: [Paradigmas, POO, OOP, Orientación a Objetos, Object Oriented]
image: "/images/mgb-test.png"
---

Existe un principio fundamental pero a menudo malinterpretado: la encapsulación. Este concepto, más que un simple mecanismo técnico, es una filosofía de diseño que impregna cada línea de código que escribimos y cada objeto que creamos.

Imaginemos un mundo donde los detalles complejos y las operaciones internas de un sistema están elegantemente escondidos detrás de una cortina de simplicidad. Eso es lo que la encapsulación nos permite hacer.
Nos brinda la libertad de interactuar con objetos complejos de una manera sencilla y segura, sin necesidad de comprender o interactuar con sus complejidades accidentales.

## 🛡️ Encapsulación

### 🚗 Introducción

Para arrancar un motor de auto, solamente necesitamos presionar un botón o girar una llave, dependiendo del tipo de auto. No necesitamos conectar cables debajo del capó, girar el cigüeñal, los cilindros e iniciar el ciclo de potencia del motor, o al menos no directamente bajo nuestro conocimiento; de lo contrario, necesitaríamos ser demasiado expertos en cada uno de estos dominios. Todos estos detalles están ocultos dentro del capó del auto.

**Solamente tenemos una simple interfaz**: un botón de encendido, el volante del auto y algunos pedales (claramente hay más cosas...). Esto básicamente nos ilustra cómo un objeto tiene una interfaz, es decir, una parte visible a la que pueden acceder quienes la utilizan, es decir, los clientes del objeto, para lograr interactuar con el mismo.

### 💡 El Concepto de Encapsulación

> Es la habilidad de los objetos para esconder partes, ya sea de sus estados o comportamientos, de otros objetos, exponiendo solamente una _interfaz_ limitada al resto del programa, con los funcionamientos que deseamos exponer al cliente de nuestra clase, es decir, las cosas que queremos que los demás sepan y puedan usar de ese objeto.

Encapsular algo significa ocultar ciertos detalles de implementación, así como ciertos datos del objeto y exponer solo el comportamiento que deseamos que sea utilizable al cliente mediante métodos públicos.

### 🛫 Ejemplo Práctico

Imaginemos que tenemos una interfaz _FlyingTransport_ con un método `fly(origin, destination, passengers)`. Cuando diseñamos un simulador de transporte aéreo, podemos restringir que la clase _Airport_ trabaje solo con objetos que implementan la interfaz _FlyingTransport_. Esto te asegura que cada objeto pasado al objeto _Airport_, sin importar si es un _Airplane_, un _Helicopter_ o un _DomesticatedGryphon_ (sí, leíste bien, un hipogrifo como esos que aparecen en Harry Potter), o en caso de que no hayas visto la película, es un animal que puede volar en definitiva, cualquiera de estos estará habilitado para aterrizar o despegar de este Aeropuerto.

<p align="center"><img width="50%" src="../images/encapsulacion-1.png"/></p>
<p align="center">Gracias DALL-E</p>

### 🌍 Encapsulación en Nuestro Entorno

Ahora que nos adentramos un poco con ejemplos algo más tirando a analogías con nuestro mundo (bueno quizás el hipogrifo no tanto...😅), podemos acercarnos todavía más a nuestro mundo de programación con otros ejemplos que es probable que un programador se cruce en su día a día.

Quizás todavía nos cuesta entender a qué llamamos `encapsulación`, pero cuando miramos un poco las cosas que utilizamos, nos damos cuenta de que está en todas partes y no lo hemos notado, como la mayoría de las cosas.

### 📚 Librerías y Frameworks

¿Usaste alguna vez librerías o frameworks? Generalmente la respuesta es sí, y en caso negativo, seguramente cualquier curso o implementación que quieras realizar hará uso de algo de ello en muy poco tiempo, es código escrito por otras personas ya probado y validado por una empresa o comunidad, la cual hace alguna actividad en específico, en teoría de la mejor forma posible, para que no tengamos que nosotros escribir el código para esa acción, por ejemplo: enviar un e-mail a una persona, ya que es algo que todo sistema utiliza y sería ya aburrido reinventar la rueda todo el tiempo para la misma acción una y otra vez (aunque cuando uno es junior nos causa mucha emoción tratar de implementar nuestras propias cosas 😎).

La mayoría de estas librerías o frameworks utilizados suelen ser open source, es decir, que podemos inspeccionar y revisar su implementación o código fuente para ver cómo está implementada por dentro esa interfaz que nos exponen para utilizar. Aunque tenemos esta posibilidad, si uno le pregunta a la mayoría de los desarrolladores, es probable que muchos de ellos nunca hayan realizado esta acción (a menos que sean un tanto curiosos y no les guste tanto la magia como me suele pasar a mí). Muchas veces supongo que la mayoría no lo hace porque por un lado no imaginamos que se puede hacer, no encontramos por qué es necesario hacerlo, creemos que es difícil y no vamos a entenderlo, o el motivo particular que tenga cada desarrollador. Pero la realidad es que, al estar hechos para ser reutilizables, suelen estar muy bien realizados, con interfaces bien definidas y documentadas, y simplemente viéndolo de afuera podemos hacer uso de ellas sin conocer los detalles de implementación, y esto hace que veamos claramente lo bien implementado que está, ya que confiamos en la interfaz y su documentación para hacer uso de ella sin importarnos qué hace por detrás para que funcione.

### 😎 Encapsulación en la Práctica

Pero en definitiva, de eso trata la **encapsulación** de alguna manera: no necesitamos saber cómo está hecho porque entendemos que, mandando `x(a, b)` e `y(c, d)` como mensaje a un objeto, realiza tal cosa, como bien indica su nombre y sus parámetros respectivos.

La mayoría del código que tocamos o trabajamos apesta de alguna manera, y la verdad es que no hay que sentirse avergonzado de eso, pero el encapsulamiento es una de las técnicas que podemos utilizar para que nuestro código apeste un poco menos.

Debemos escribir código como si fuera para programadores estúpidos, o quizás no es que sean estúpidos, pero al menos sí ignorantes, ya que es muy probable que no tengan el mismo conocimiento que tú tienes en ese momento, ni tampoco la información que posees en esa situación. Y para esto viene bien una frase conocida: "Ama como si no hubiera un mañana, baila como si nadie te viera, canta como si nadie te escuchara y escribe código de una manera que imagines que el próximo que lo vea sea un asesino serial que conoce tu dirección y puede ir a buscarte en caso de no entender el código" 😅.

### 🚀 Mejorando Nuestro Código

Necesitamos saber de qué forma nuestro código apesta, está sucio o es problemático de alguna manera y por qué deberíamos preocuparnos; de lo contrario, nuestro _manager_ o líder jamás nos dejará hacer _refactoring_ para hacer limpieza, ya que dirá que no hay tiempo, necesito que solamente produzcas nuevas funcionalidades (a que sí te pasó o pasa seguido...).

El código que no esté a la altura de las circunstancias o no tenga ciertas buenas prácticas de nuestra industria afecta a la productividad a mediano y largo plazo, y esto de largo plazo no es que hablemos exclusivamente de años para notarlo, quizás sean meses o semanas. Sabemos que un par de programadores pueden escribir mucho **código espagueti** en un par de semanas. Y nosotros queremos evitar justamente eso, el código deteriorado, ya que, si no tenemos cuidado, puede volverse muy deteriorado en muy poco tiempo, y de esto nos damos cuenta porque cada vez es más difícil agregar funcionalidades nuevas y nuestro equipo se vuelve muy lento implementando cosas.

## 🌿 Sostenibilidad en Programación

### 🤔 La Sensación del Código Imperfecto

Los programadores a menudo sienten que el código no es como nos gustaría que fuese, y aunque quizás no sepamos por qué, ese sentimiento es real. Tenemos esa sensación simplemente porque nos damos cuenta de que no podemos ser tan productivos como podríamos serlo en condiciones mejores. Esto surge también porque nos pasamos más tiempo leyendo código que escribiéndolo. Fácilmente, gastamos 10 veces más tiempo leyéndolo que escribiéndolo.

### 📝 Escribir para Programadores Ignorantes

Cuando hablamos de escribir código para programadores ignorantes, lo decimos porque, al escribir una funcionalidad, sabemos mucho sobre ella o sobre lo que se supone que debe hacer, y es difícil imaginarse **no saber lo que sabemos en ese momento** sobre ese problema. Porque, en definitiva, ya lo sabemos y no podemos quitarnos esa información, haciendo como si no lo supiéramos (o al menos es una actividad que no es para cualquiera...).

Por lo tanto, necesitamos hacer el código legible y entendible, para que tanto mis colegas como mi yo del futuro, que ya no tenga la misma cantidad de información que tenía al momento de haber escrito esa funcionalidad, le sea más sencillo de entender al momento de tener que volver a leer esa funcionalidad y no pensar que es chino básico.

### 🚀 Hacer APIs Entendibles

Tengo que hacer que mis _APIs_ sean entendibles, comprensibles y reducir la cantidad de lectura necesaria sobre ellas. Por eso decimos que debemos tratar de imaginar lo que sería leer ese problema si no tuviéramos la información que tenemos al momento de crear esa funcionalidad. Pero para lograr lo anterior, necesitamos algo más procesable, algo más concreto que nos permita llevar ese concepto a cabo y de esto es lo que va la `encapsulación`.

## 📚 Definiciones de Encapsulación

### 🤖 **Information Hiding**

**Information hiding** es una de las partes más inentendidas de la encapsulación o uno de los aspectos más incomprendidos de **Object Oriented Design**, y parte de esto es por el nombre que posee. **Information hiding** no es particularmente útil como nombre, porque muchos lo interpretan como sinónimo de que no puedes exponer los campos de la clase como campos públicos, o puede ser en Java, por ejemplo, mediante métodos `getSomeProperty()` y `setSomeProperty(...)`. Hay un malentendimiento sobre que hacer esto de meter propiedades privadas y agregar estos métodos get y set es de lo que trata _information hiding_. En la mayoría de las universidades, terciarios y cursos relacionados a orientación de objetos, se lo trata de esta manera.

Por lo tanto, quizás sería mejor tener el nombre de **Implementation Hiding**, ya que de esto es de lo que trata el principio en sí. No es tanto que no se pueda exponer ningún tipo de dato o propiedades en sí. La idea es que la forma en que almacenamos la información internamente en la clase es un detalle de implementación que no siempre tiene que ser expuesto a nuestros clientes o consumidores.

### 🌐 Ejemplos de Encapsulación

#### **Opción 1:**

Un ejemplo podría ser tener la clase _User_ con un campo _username_ y _password_ como campos públicos pudiendo acceder facilmente a esos campos:

```typescript
class User {
  public username: string;
  public password: string;
}
```

#### Opción 2:

A la hora de hacer el modelado, también podemos hacer que para obtener estos datos necesitemos un método getter para obtener la _password_ y un método setter para asignarle una al usuario como podemos observar en el ejemplo:

```typescript
class User {
  private username: string;
  private password: string;
  
  getUsername(): string {
    return this.username;
  }
  
  setUsername(username: string): void {
    this.username = username;
  }
  
  getPassword(): string {
    return this.password;
  }
  
  setPassword(password: string): void {
    this.password = password;
  }
}
```

#### Opción 3:

**Por ejemplo**: Decidimos poder conocer el historial de contraseñas del usuario y en lugar de guardar solo un _string_, podríamos guardar una lista o _array_ de elementos string como son las passwords. Entonces, cuando quieren guardar una nueva contraseña, directamente hacemos un _append_ o agregamos a esta lista de elementos la nueva contraseña. Entonces, cuando el usuario quiere obtener la contraseña, simplemente devolvemos la última contraseña de la lista pero como ahora tienes las contraseñas viejas también, puedes hacer ciertas validaciones extras, como decir que si una contraseña nueva es igual a una anterior o difiere en un % muy bajo con respecto a las anteriores, como por ejemplo incrementar un número en la misma, podemos rechazar este pedido de nueva contraseña ya que es insegura para el usuario.

Ese tipo de validación es un detalle de implementación, esto es algo que depende de las reglas comerciales del proyecto en sí. Por lo tanto, al final uno no está ocultando la información mediante el `setPassword()`, sino que estamos ocultando la implementación de ese método según las reglas de negocio que son cambiantes, pero son bajo la misma firma de método.

Además, te puede pasar que vos, al tener esta lista de contraseñas viejas, si bien podrías, no quieras exponer a los clientes las contraseñas anteriores por más que estén guardadas y solamente quieras exponer la última contraseña mediante algún método `getPassword()`, pero además te permite realizar todo este tipo de lógica de validación. Entonces aquí no ocultas TODA la información de la contraseña (como nos puede dar a entender _information hiding_), en realidad ocultas solo una parte; en realidad, lo que estás ocultando es todo lo que hay de detalle de implementación atrás de ese método y expones, por ejemplo, un `getPassword()` que en realidad lo que no sabemos es que de alguna manera estamos exponiendo solo la última contraseña, ya que es lo que le interesa al cliente saber para poder hacer un `log-in`, por ejemplo, pero también existe otra información interna que no exponemos como son las contraseñas anteriores que tuvo el usuario en otros momentos.

```typescript
class User {
  private username: string;
  private passwords: Array<string>;
  
  getUsername(): string {
    return this.username;
  }
  
  setUsername(username: string): void {
    this.username = username;
  }
  
  getPassword(): string {
    return this.passwords.pop();
  }
  
  setPassword(password: string): void {
    //... Some validations
    
    if (!isValidPassword) {
      return;
    }
    
    this.password.append(password);
  }
}
```

### 🛡️ Protección de Invariantes

- _Protection of Invariants_: El lenguaje funciona en contra de nosotros porque la mayoría no entendemos qué es una invariante. Muchas veces en los cursos marcan esto de verificar pre-condiciones y pos-condiciones. Estas condiciones son reglas sobre lo que es un estado válido e inválido, donde tratamos mediante estas reglas que los estados inválidos sean imposibles de llegar, o al menos hacerlos tan difíciles de alcanzar como sea posible, para lograr que el objeto quede en un estado válido siempre que sea posible.

Lamentablemente es muy fácil en el paradigma crear clases que sean fáciles de poner en un estado inválido, pero si tenemos más cuidado, es también posible escribir clases de modo que sea difícil de poner estados inválidos en las mismas. Las pre y pos condiciones es algo que _Bertrand Meyer_ introdujo como concepto mediante **aserciones**.

## 🎯 Conclusiones

Pensar el encapsulamiento mediante estas dos reglas:

- _Information Hiding_
- _Protection of Invariants_

Es posible que no sea lo suficientemente concreto para entenderlo del todo, por lo tanto, podemos ir un paso más allá mediante _Object Oriented Design_ y principios de diseño que son más concretos, más accionables y permiten mapear de mejor manera estos dos principios. Lo haremos mediante las siguientes reglas de diseño:

- Command Query Separation
- Postel's Law.

Los veremos luego en otras lecciones con sus definiciones y ejemplos respectivos, de todas maneras, espero que todo lo escrito sirva para dar una mejor aproximación de un recurso tan importante en objetos y en el diseño de los mismos como es la encapsulación, ya que de ello luego, en definitiva, da pie a todos los patrones y principios en general del paradigma.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/211164605-ed461c29-b3c2-4eef-acf3-ad8cd9bdbbdc.png"/></p>
