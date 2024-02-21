---
layout: post
title: "🧱 Programando con Objetos: Fail Fast"
tags: [Paradigmas, POO, OOP, Orientación a Objetos, Programación Orientada a Objetos]
---

Hasta ahora con lo que aprendimos, en el diseño orientado a objetos, nos dimos cuenta de que hay casos en los que no podemos utilizar el sistema de tipos estáticos para establecer ciertas precondiciones.
En su lugar, tendremos que depender de clausulas de guarda en tiempo de ejecución, como este chequeo que vimos en lecciones anteriores sobre el argumento nulo, por ejemplo.

## 🛡️ Verificación en Tiempo de Ejecución
**Guardianes del Código: Las Clausulas de Guarda**

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

Ahora, si observas la clase `FileStore`, ¿hay algo más que podría andar mal? Pensemos unos segundos...

**¿Que pasaría si la cadena que enviamos para representar el directorio de trabajo es una ruta inválida?**

Esto también es algo difícil de verificar al menos en momento del diseño, pero si podemos comprobarlo en tiempo de ejecución.

Una opción es verificar si el directorio de trabajo existe. Y si no existe, entonces lanzar una excepción
tan pronto como descubras que este es el caso.

```ts
import * as fs from 'fs';

class FileStore {
    constructor(workingDirectory: string) {
        if (!this.workingDirectory) {
            throw new ArgumentNullException("Working directory cannot be null or undefined.");
        }

        if (!fs.existsSync(this.workingDirectory)) {
            throw new ArgumentException(`Boo: The working directory '${this.workingDirectory}' does not exist.`);
        }
    }

    // Resto de la clase...
}
```

Claramente es a efectos educativos no discutiremos sobre si esta bueno o no utilizar existsSync en el caso del entorno de `node.js` en un caso de producción 
ya que sería bloqueante la operación, aún asi su versión async sería complicada de aplicar ya que en el constructor dificulta esta tarea por lo tanto lo dejaremos de esta manera.

## 💡 Mensajes de Excepción como Documentación
**Hablando Claro: Los Mensajes de Excepción como Guías**

Así que este es otro ejemplo de fallar rápido, y puedes usar esa excepción para proporcionar un mensaje de excepción amable
y educado al usuario de la clase, es decir, para otros compañeros desarrolladores.
En este caso, simplemente escribímos `"Boo: The working directory '${this.workingDirectory}' does not exist."`, pero puedes
imaginar que esto podría ser algo mucho más detallado, como:

>"Intentaste proporcionar un directorio de trabajo que no representa un directorio de trabajo válido o existente.
>No es tu culpa, porque no fue posible diseñar la clase FileStore de tal manera que esto sea una precondición tipificada estáticamente.
>Pero por favor, proporciona una ruta válida a un directorio existente."

```ts
import * as fs from 'fs';

class FileStore {
    constructor(workingDirectory: string) {
        if (!this.workingDirectory) {
            throw new ArgumentNullException("Working directory cannot be null or undefined.");
        }

        if (!fs.existsSync(this.workingDirectory)) {
            throw new ArgumentException(`You attempted to provide a working directory string that does not represent an actual
              working directory. It's not your fault, as it wasn't possible to design the FileStore class in such a way
              that this could be a statically typed precondition. But please, provide a valid path to an existing directory.
            `);
        }
    }

    // Resto de la clase...
}
```

La verdad es que puedes ser tan detallado como quieras. Lo importante aquí es que si quieres que el usuario,
en este caso, estamos hablando de otros programadores, y quieres o deseas que disfruten o se deleiten con tu código
o con la clase FileStore en nuestro caso en particular, tienes que tratarlos de tal manera que no se sientan estúpidos.

A nadie le gusta sentirse estúpido. Así que si escribes mensajes de excepción que les hablen de manera educada y
de alguna manera les digan que no había forma de que pudieran haber sabido esto de antemano,
pero aún así se tropezaron con este problema en particular, entonces aquí te dejo la forma de cómo puedes lidiar con eso.

## 📚 Menos Documentación, Más Claridad
**Escribe Menos, Expresa Más: El Poder de los Mensajes Detallados**

Esto además casi funciona como documentación.
Si de algo no tenemos que tener dudas es que mientras más detallados son los mensajes de excepción, menos documentación
hay que escribir. Casi podrías decir que la excepción en sí misma es parte de la documentación.

Pero la ventaja aquí es que el mensaje de la excepción vive mucho más cerca del código fuente que cualquier sistema
de documentación existente, como el tan conocido Swagger o quizás hasta algo más rudimentario y manual.

Mientras más explícito y útil sea ese mensaje de excepción, ya que recordemos de nuevo a quien va dirigido esto,
concluimos que los mensajes de excepción están dirigidos a otros programadores. No están dirigidos a los usuarios.

**Los usuarios siempre deberían ver algo no técnico.**

Así que si volvemos a la Ley de Postel, deberíamos tratar de ser lo más tolerantes posible con la entrada que permitimos que nos llegue.
Si podemos entender la entrada, la aceptaremos. Pero si no podemos entender la entrada que nos llegó en absoluto,
deberíamos ser tan explícitos sobre los problemas que tenemos con esa entrada y también explícitos sobre cómo el cliente de nuestras clases
puede resolver esos problemas para utilizarlas correctamente.

## 🎯 Conclusiones
**Clausulas de Guarda y Excepciones: Pilares de una POO Robusta**

Espero que te ayude a darte cuenta que tan importante se vuelven estas clausulas de guarda a nivel de tiempo de ejecución,
cuan importante se vuelven para que nuestros objetos esten completos y no se queden tan facilmente en un estado inconsistente,
y que además se lleven un TIP adicional sobre lo muy importante que se vuelven los mensajes de las excepciones
que colocamos, para que otros desarrolladores puedan entender que esta pasando y les sea más fácil
poder encontrar el problema, y fácilmente llegar a la solución adecuada entendiendo que error sucedió en nuestro sistema.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/211164605-ed461c29-b3c2-4eef-acf3-ad8cd9bdbbdc.png"/></p>
