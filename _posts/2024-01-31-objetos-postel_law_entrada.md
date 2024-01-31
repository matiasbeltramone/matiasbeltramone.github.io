---
layout: post
title: "🧱 Programando con Objetos: La Ley de Postel"
tags: [Paradigmas, POO, OOP, Orientación a Objetos, Programación Orientada a Objetos]
---

En esta oportunidad, vamos a hablar sobre un tema relacionado de alguna manera con nuestra "Ley de Robustez" que vimos en el post anterior, particularmente de las "entradas" o "inputs" de nuestro sistema, ya sea en nuestros métodos, constructores, etc. La idea es que podamos observar una clase que vimos anteriormente en nuestro post sobre CQS, que si no lo viste todavía te invito a que te des una vuelta para poder complementar bien esta nueva lección. La idea es revisar nuestra clase FileStore y poder prestar atención a cómo está diseñada, para que no tengas que buscarla te la facilito en las próximas líneas. La idea es que, luego del snippet de código, puedas responder a la pregunta realizada debajo.

### Análisis de la Clase FileStore 🔍

Dada la implementación que tenemos en el snippet a continuación, viendo la entrada y la salida de datos, en este caso enfocándonos directamente sobre las diferentes entradas de la clase, ¿hay algo que creas que puede salir mal, dadas diferentes tipos de entradas? Te invito a que, antes de seguir leyendo la respuesta que se encuentra debajo, trates de pensar qué casos pueden darnos algún tipo de problema.
```ts
import * as fs from 'fs';
import * as path from 'path';

export class FileStore {
  workingDirectory: string;

  save(id: number, message: string): void {
    const filePath = this.getFileName(id);
    fs.writeFileSync(filePath, message);
  }

  read(id: number): string {
    const filePath = this.getFileName(id);
    return fs.readFileSync(filePath, 'utf8');
  }

  private getFileName(id: number): string {
    return path.join(this.workingDirectory, `${id}.txt`);
  }
}
```

### Posibles Complicaciones 🚨

Resulta que algunas personas piensan que el ID en general puede ser problemático porque, por ejemplo, podría ser 0 o negativo, pero lo único que realmente hacemos con el ID aquí es concatenarle el string `.txt`, y eso siempre va a ser posible de realizar. Así que incluso los números negativos simplemente se convertirán en un string con un signo menos delante, por lo que eso no va a ser realmente un problema.

Además, viendo nuestra otra entrada posible en el método save(), por ejemplo, si proporcionas un mensaje, cualquier tipo de mensaje simplemente se escribirá en el archivo, e incluso si proporcionas un mensaje nulo, el archivo que se escribirá simplemente estará claramente vacío. Así que eso tampoco resulta ser un problema en sí.

Por otro lado, el directorio de trabajo sí que puede ser problemático de varias maneras. Lo primero en lo que quiero enfocarme es que el directorio de trabajo puede ser nulo (lo cual en un ratito lo paso a explicar...). Si el directorio de trabajo es nulo, entonces  `path.join()` en la última línea de código, allí en el método `getFileName()`, lanzará una excepción de tipos si el directorio de trabajo es nulo. Y nota que este método `getFileName()` que contiene la llamada a  `path.join()` y no solo eso sino que se utiliza también en los métodos `read()` y `save()` ya que son parte de su implementación, así que los tres métodos fallarán si el directorio de trabajo es nulo. Sin embargo, el valor predeterminado para el directorio de trabajo es nulo como podemos observar en el código (seguramente alguno me diga que en JavaScript en realidad es `undefined` pero para el caso educativo es lo mismo ya que sigue representando la ausencia del valor), por lo que es fácil escribir código así como el siguiente snippet, es decir, código inválido.

```ts
const fileStore = new FileStore();
fileStore.save(42, "Hi Guys"); // This throws an exception
```

Puedes crear un nuevo almacenamiento de archivos y empezar a llamar a sus métodos sin ningún problema, y fallará. Esto siempre fallará. Si bien esto compila o transpila como gustes llamarle, es decir, pasa claramente la parte estática de chequeos, pero falla en tiempo de ejecución, y esto es a lo que me refiero cuando digo que es muy, muy fácil escribir accidentalmente código que no protege suficientemente sus variantes. Es muy fácil escribir código que puede estar en un estado inválido, y de hecho, gran parte de las librerías de JavaScript, TypeScript, clases bases de .NET, Java suelen ser así. Y de hecho, muchas librerías orientadas a objetos están diseñadas de esa manera, es decir, teniendo problemas de objetos incompletos.

Es muy fácil crear una instancia de un objeto sin que esa instancia del objeto esté completamente inicializada de manera correcta, y luego puedes empezar a llamar a métodos en ese objeto, y aún así compila, pero fallará en tiempo de ejecución porque falta inicialización con estados válidos.

Así que necesitamos cambiar el diseño de la clase `file store` para evitar que estos tipos de accidentes ocurran. Necesitamos hacerlo más difícil para evitar cometer errores simples de uso como el que estamos viendo en el ejemplo anterior. Debería ser una invariante del sistema `file store`. De alguna manera debería ser una precondición de la clase `file store` que el directorio de trabajo nunca pueda ser nulo porque si el directorio de trabajo es nulo, nada va a funcionar.

Vamos a ocultar los códigos de implementación para hacerlo un poco más fácil de seguir lo que está sucediendo aquí.

```ts
export class FileStore {
  workingDirectory: string;

  save(id: number, message: string): void { ... }

  read(id: number): string { ... }

  private getFileName(id: number): string { ... }
}
```

### Mejorando el Diseño para Prevenir Errores 🛠️

Pero lo primero que puedes hacer es hacer que el directorio de trabajo sea un requisito. Y la forma en que puedes hacer esto con un diseño orientado a objetos es agregar un constructor que tome un directorio de trabajo como entrada.

```ts
export class FileStore {
  workingDirectory: string;

  constructor(workingDirectory: string) {
    this.workingDirectory = workingDirectory;
  }

  save(id: number, message: string): void { ... }

  read(id: number): string { ... }

  private getFileName(id: number): string { ... }
}
```

Ahora, antes de que se agregara este constructor, el "compilador" de TypeScript por defecto agregaba un constructor predeterminado. Este es un constructor sin parámetros. Pero ahora que hay un constructor explícito que toma un argumento, ese constructor predeterminado que el compilador de TypeScript agregaba automáticamente vacío antes ya no está ahí para utilizarlo. Así que el único constructor que la clase `FileStore` tiene ahora es este constructor que toma un directorio de trabajo como entrada.

Así que ahora no podemos crear una nueva instancia de la clase FileStore a menos que suministremos un valor para nuestro directorio de trabajo. Así que eso nos lleva a parte del camino para asegurar la invariante que vimos anteriormente de la clase `FileStore`. Pero si miramos el código ahora, ¿puede el directorio de trabajo ser alguna vez nulo?

### Complicaciones con nulos 🎢

Hay al menos dos formas en las que el directorio de trabajo todavía puede ser nulo. Te invito a que, antes de seguir leyendo, puedas reflexionar sobre estas dos maneras de que pueda ser null el valor de la clase.

```ts
export class FileStore {
  constructor(workingDirectory: string) {
    this.workingDirectory = workingDirectory;
  }

  workingDirectory: string;

  save(id: number, message: string): void { ... }

  read(id: number): string { ... }

  private getFileName(id: number): string { ... }
}
```

La primera de ellas es la propiedad del directorio de trabajo que tiene un setter público o bueno en nuestro caso directamente la propiedad `workingDirectory` es pública por lo que facilmente podes realizar algo como lo siguiente.

```ts
const fileStore = new FileStore('test_directory');
fileStore.workingDirectory = null;
fileStore.save(42, "Hi Guys"); // This throws an exception again
```

Por lo tanto, es posible asignar nulo a esa propiedad fácilmente aunque lo inicialicemos de buena manera. Así que cambiémoslo a un setter privado o en nuestro caso la propiedad privada, lo que significa que solo la clase misma puede asignar un valor a la propiedad.

```ts
export class FileStore {
  constructor(workingDirectory: string) {
    this.workingDirectory = workingDirectory;
  }

  private workingDirectory: string;

  save(id: number, message: string): void { ... }

  read(id: number): string { ... }

  private getFileName(id: number): string { ... }
}
```

Eso protege a los objetos de tener el directorio de trabajo explícitamente configurado como nulo a través del setter de la propiedad. Pero aún así, también es posible invocar el constructor de `FileStore`  con una cadena nula. Debido a que el problema es que  `string` es un tipo de referencia, y un tipo de referencia puede ser nulo en la mayoría de los lenguajes orientados a objetos. Por lo tanto, para protegernos de eso, desafortunadamente no podemos protegernos de eso de manera estática (o sí depende del lenguaje y ciertos checks), lo haremos en tiempo de ejecución mediante otro mecanismo.

### Detalles de lenguajes 🔎

Me voy a detener un momento por aquí ya que estoy dando los ejemplos en TypeScript, algunos se preguntarán si al estar tipado como `string` en realidad sería suficiente y no se podría pasar `null` y la respuesta en realidad es "depende", si buscamos algo de información nos daremos cuenta de lo siguiente:

> En C#, y en muchos otros lenguajes de programación orientados a objetos, los tipos de referencia, como las cadenas (string), pueden ser null. Esto significa que es posible pasar un valor null a un constructor o método que espera un tipo de referencia. Por ejemplo, si tienes un constructor en C# que espera una cadena (string), es técnicamente posible pasarle null, a menos que el lenguaje o el entorno de programación ofrezca mecanismos para prevenirlo.

> En TypeScript, que es un superset de JavaScript, ocurre algo similar. Los tipos como string pueden ser null o undefined a menos que el programador tome medidas para evitarlo. TypeScript ofrece algunas características que ayudan a manejar estas situaciones, como la verificación de nulidad estricta (strictNullChecks en la configuración de TypeScript). Con strictNullChecks activado, TypeScript no permitirá asignar null o undefined a un tipo string a menos que explícitamente se declare que puede ser null o undefined, estamos hablando al menos de los checks estáticos claro esta, ya que el transpilado en JavaScript sobre esto no hace ningún tipo de validación ya que no es tipado en sí.

```ts
class FileStore {
    constructor(private workingDirectory: string) {
        this.workingDirectory = workingDirectory;
    }

    // Resto de la clase...
}

// Con strictNullChecks activado, la siguiente línea causará un error de compilación en TypeScript
const store = new FileStore(null);
// Pero en caso de no estar activado tranquilamente se podría pasar sin ningún problema haciendo que falle en Runtime o tiempo de ejecución.
```

Sin embargo, por ejemplo C# 8.0 introdujo las "Referencias nulas no permitidas" (Non-Nullable Reference Types), que cambian este comportamiento. Cuando esta característica está habilitada, C# trata todos los tipos de referencia como no nulos por defecto, y necesitas especificar explícitamente si un tipo de referencia puede ser null algo así como TypeScript con los strictNullChecks activados. Pero bueno fuera de estas aclaraciones y casos particulares si no estuvieran esas cosas activadas debemos hacer comprobaciones de programación defensiva que es común que lo hagamos para validar siempre cosas de nuestras clases.

### Hacks 🕵️‍♂️
Pero si de alguna manera te queda dudas de que teniendo el strictNullChecks hace que esto que veremos como medida de seguridad no sea necesario, antes de continuar desde ya te digo que haciendo hacks como este que sigue podría hacer que vuelva a fallar en Runtime pero no en tiempo estático.

```ts
const store = new FileStore(null as unknown as string);
```

Así que en su lugar, puedes agregar una cláusula de guarda la cual tenemos 100% de seguridad. Una cláusula de guarda es algo que solo evalúa si el directorio de trabajo es nulo o no en tiempo de ejecución, y luego falla rápidamente si resulta que el directorio de trabajo es efectivamente nulo.

```ts
class FileStore {
    constructor(private workingDirectory: string) {
      if (!this.workingDirectory) {
        throw new ArgumentNullException("Working directory cannot be null or undefined.");
      }

      this.workingDirectory = workingDirectory;
    }

    // Resto de la clase...
}
```

Eso es todo y espero que hayas podido llevarte algún consejo nuevo en esta lección. Seguramente seguiremos con alguna mini lección sobre conceptos de null, para más luego seguir como veníamos diciendo en "Ley de Robustez" mediante fallos tempranos o early returns e indagar un poco también sobre salidas de los programas.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/211164605-ed461c29-b3c2-4eef-acf3-ad8cd9bdbbdc.png"/></p>
