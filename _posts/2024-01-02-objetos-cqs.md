---
layout: post
title: "🧱 Programando con Objetos: CQS"
tags: [Paradigmas, POO, OOP, Orientación a Objetos, Object Oriented]
---

- CQS nos dice que nuestras operaciones o deben ser comandos o consultas, pero no ambas.

> 💡 IMPORTANTE CQS no es CQRS, esto es algo diferente ya que es un patron de diseño de arquitectura que describe una forma de escribir software distribuido.

`Bertrand Meyer` lo introdujo para objetos, pero podría ser aplicado a programacion funcional, estructurada o procedural. Es sobre `operaciones` y no lo llama métodos (objetos) o funciones (funcional) justamente porque cree que aplica para diferentes paradigmas, de lo contrario es como que se aplica a uno solo por la terminología utilizada.

**Commands:** Produce efectos secundarios en el sistema, este efecto puede ser tan pequeño como cambiar un bit en la memoria, pero también podría ser algo que tenga más efectos permanentes, como eliminar una fila en la base de datos, o escribir una archivo en el disco, enviar un email o algo del estilo. Es algo que tiene un efecto secundario observable en nuestro sistema.

**Queries:** Retornan datos, es una `OPERACIÓN` que devuelve datos, y hay que entender que estas terminologías fueron acuñadas o tomadas antes de que las bases de datos relacionales fueran distribuidas o extendido en la comunidad, por lo tanto, no estamos hablando de `query` como algo que se envía a una base de datos, que luego se traduce en una consulta SQL y esto regresa datos de esa base de datos. **PODRÍA** ser una operación que lo haga, pero también podria ser una operación que simplemente devuelva algunos datos que ya están en memoria simplemente.

Es un principio porque en realidad no es que no se pueda realizar en nuestro lenguaje esto de no hacer efectos secundarios y a la vez no devolver datos. Pero es un prinicipio porque nos dice que **NO DEBERÍAMOS** realizarlo como buena práctica de programación.


**Commands: _Mutate State_. Cambian el estado observable de la aplicación.**

`save(order: Order): void;`

- ¿Si nos preguntamos que es lo que realiza ese método anterior que diríamos?

No sabemos que tiene por detrás como implementación real ese método pero por lo que leemos y esa era la intención del ejercicio, es que interpretamos que eso
guarda un pedido de alguna manera. No sabemos como lo guarda, pero si sabemos que es lo que hace o al menos eso suponemos, es decir, guardar en definitiva, luego puede ser que se guarde en memoria, en un base de datos, en un archivo del disco, etc. Pero esperamos q el pedido pasado como argumento del método se guarde.

`send(message: T): void;`

- ¿Si nos preguntamos que es lo que realiza ese método anterior que diríamos?

Es fácil de suponer que envía un mensaje. Quizás no sabemos que tipo de mensaje es, podría ser un email, una notificacion _push_
o un mensaje que se envia a una _queue_ en otra maquina.

`associate(foo: IFoo, bar: Bar): void;`

- ¿Si nos preguntamos que es lo que realiza ese método anterior que diríamos?

Si intentamos descifrar solo viendolo que hace supongo que podemos concluir que asocia `Foo` a `Bar`, lo suponemos por la firma y los argumentos de entrada, no sabemos como sucede, pero sabemos que se asocian entre ellos. No sabemos si foo se referencia desde bar, o viceversa o si hay referencias entre ambas, pero si sabemos que algo se modifica en el sistema para asociarlos de alguna manera.

- ¿Qué es común entre todos estos métodos vistos anteriormente?

El tipo de retorno que es `void` y eso es lo que hace que reconozcas un comando porque no tiene sentido que invoquemos a un metodo que devuelve `void` si no esperamos que haga algún efecto secundario en el sistema. Ya que si no hace nada como efecto secundario como implementación real y no devuelve nada. ¿Para que querría invocarlo entonces?
Entonces a menos que sea un `metodo degenerado` y no haga nada, sabemos que es un _command_ con un _side effect_ al tener `void` en la firma de los mismos.

Los comandos además pueden invocar `queries`, es posible que desde un comando invoquemos `queries`, pero no que desde las `queries` invoquemos un comando.


**Queries: Do not mutate state. No cambian el estado observable de la aplicación.**

```typescript
getOrders(userId: number): Order[];
```

- ¿Si nos preguntamos que es lo que realiza ese método anterior que diríamos?

Obtener los pedidos de un usuario con identificador "xxxx-xxx-xxxx-xxx", es decir, devuelve una lista de pedidos de cierto usuario o al menos es una buena conjetura de lo que leemos como firma de método.

```typescript
map(paypalPayment: PaypalPayment): Payment;
```

- ¿Si nos preguntamos que es lo que realiza ese método anterior que diríamos?

Imagino que mapea o transforma el argumento `PaypalPayment` en un tipo `Payment`, es decir, toma la información de `PaypalPayment` que es específica de esa plataforma y lo traduce en una implementacion de `Payment` que es algo propio de nuestro dominio seguramente.

```typescript
create<T>(): T;
```

 - ¿Si nos preguntamos que es lo que realiza ese método anterior que diríamos?

Crea algo de tipo genérico "T" que es lo que devuelve claramente según la firma, es decir, de alguna manera recuperamos una instancia de un tipo T sea lo que sea ese tipo en cuestión.

- ¿Qué es común entre todos estos métodos vistos anteriormente?

La caracteristica en común que tienen todos los métodos anteriores es que todas las firmas devuelven algo.

Podriamos argumentar que las queries son `IDEMPOTENTES`.

Si no estamos familizariamos con IDEMPOTENCIA, nos dice que si invocas una operación "una" vez o "n" veces, con los mismos parámetros de entrada no debe variar su resultado o cambiar el estado del sistema en comparación con la primera vez que se invoco la primer operación. Si cierta variable del sistema "x" estaba en cierto estado, después de invocar "n" cantidad de veces una operación de _query_ no debe variar el valor de nuestra variable "x" ni realizar ningún otro cambio de estado en nuestro sistema.

Por ejemplo: una operacion de eliminación que no es una consulta, por cierto, es un comando, pero un _delete_ es idempotente en el sentido de que si eliminamos un recurso y luego intentamos eliminarlo de nuevo el elemento sigue eliminado, es una operación idempotente natural al llamarla dos veces no puede producir más cambios de estados en nuestro sistema. Entonces una _query_ es una operacion idempotente en el sentido de que llamar "n" veces a una consulta no debería cambiar la respuesta devuelta en ninguno de los casos o variar según la llamada, siempre que obviamente consultemos con los mismos parametros de entrada. Eso signfica que las _queries_ son seguras para llamarlas.

Si pensamos en nuestro sistema como _basic correctness_ es cuando hablamos de requisitos funcionales, por ejemplo: el sistema hace lo que se supone que debe hacer, es seguro invocar una _query_ y realmente no importa si invoca la consulta "1" o "n" veces, es decir, puede tener impacto en la _performance_, pero esto es otra cosa, ya no cambia la corrección básica del sistema.

Si invocamos una _query_ con ciertos parámetros una vez y no cambia nada hasta que vuelva a invocar a esa _query_, debemos obtener la misma respuesta. Entonces no tenemos realmente que preocuparnos por que sucede si invoco de nuevo esa consulta. Ya que sabemos que no cambiará nada en el sistema.

Puede haber otros impactos como performance, pero no del lado de _basic correctness_. Podemos suponer con seguridad que realmente no importa si invocamos "1" o "n" veces a las consultas, eso hace más fácil de razonar al codigo porque eso podemos suponer que con seguridad cuando miramos las firmas de métodos como las que estan en el ejemplo de _queries_ no tienen efectos secundarios, sabemos que todas las _queries_ lo cumplen y son seguras de invocar solo con mirar su firma.

### Casos Prácticos (...y algo confusos): Queries

Nos encontramos con este método:

```typescript
class FileStore {
  public async read(id: number): Promise<void> { 
   const path = pathManager.combine(this.workingDirectory, id + ".txt");
   const text = fileManager.readAllText(path);

   await this.emitEvent(new MessageWasRead({ message: text }));
  }
}
```

- ¿Qué podemos decir del método anterior?

1. Método que por leer el nombre con sus argumentos parece directamente una _query_.
2. Pero al ver la firma vemos que devuelve `void` ¡Oh sorpresa!, osea se parece más a un comando.

Este es el clásico código mal diseñado que de alguna manera apesta.

Podemos mejorar esto inmediatamente devolviendo el valor correspondiente para que se vuelva a parecer a una _query_ como debería ser la intención del método:

```typescript
class FileStore {
  public async read(id: number): Promise<string> { 
   const path = pathManager.combine(this.workingDirectory, id + ".txt");
   const text = fileManager.readAllText(path);

   await this.emitEvent(new MessageWasRead({ message: text }));

   return text;
  }
}
```

Las siguientes preguntas surgen sobre este código anterior:

1. ¿Es una _query_? ó
2. ¿Es un comando porque realiza efectos secundarios en nuestro sistema?

- Normalmente la mayoría no está tan seguro de la respuesta.

Pero en realidad no es tan dificil de ver que es un comando por ahora ya que en definitiva emite un evento en nuestro caso llamado `MessageWasRead`.

Entonces: ¿Es un evento un efecto secundario?

- Si y lo podemos ver pensando en el `EventDispatcher` que publica en algún sistema estos eventos ya que su firma debería tener un `void` en su método `dispatch` ya que emite un evento pero no necesita devolver nada realmente solo es un aviso al sistema de que alguna modificación generalmente de estado ha ocurrido en nuestro sistema.
- Por lo tanto tenemos que hacer algunas modificaciones para no generar este evento y cumplir con CQS como veníamos hablando anteriormente.

Primeramente podemos quitar a ese emitir eventos tranquilamente del método y de la clase.

```typescript
class FileStore {
  public async read(id: number): Promise<string> { 
   const path = pathManager.combine(this.workingDirectory, id + ".txt");
   const text = fileManager.readAllText(path);

   return text;
  }
}
```

Si ahora colapsamos el método para solo observar su firma nos queda `public async read(id: number): Promise<string> { ... }`. Donde ahora podemos intuir de una mejor manera que este método simplemente lee en base a un identificador `id` y nos devuelve un texto asociado a ese `id` seguramente algún mensaje o texto que tenía ese identificador.

Por lo tanto no nos hace falta entender demasiado los detalles de implementación del método. Es suficiente saber que este método lee un mensaje asociado a un `id`. No es importante saber como funciona desde una perspectiva general para tener noción de lo que realiza nuestra función y esa es la idea.


### Casos Prácticos (...y algo confusos): Commands

```typescript
class FileStore {
  public async save(id: number, message: string): Promise<string> { 
   const path = pathManager.combine(this.workingDirectory, id + ".txt");
   await fileManager.writeAllText(path, message);

   return path;
  }
  
  public workingDirectory(): string { ... }
  public async read(id: number): Promise<string> { ... }
}
```

- ¿Dentro de nuestra clase donde esta el comando?

La mayoría responde en el método `save`, pero si miramos la firma de una mejor manera vemos que devuelve una cadena de texto, y acabamos de aprender que el comando debe devolver `void`, es decir, nada. Entonces aca vemos que es dos cosas, una _query_ y un _command_.

Debemos transformarlo un poco para poder aplicar al principio CQS.

```typescript
class FileStore {
  public async save(id: number, message: string): Promise<void> { 
   const path = pathManager.combine(this.workingDirectory, id + ".txt");
   await fileManager.writeAllText(path, message);
  }
  
  public workingDirectory(): string { ... }
  public async read(id: number): Promise<string> { ... }
}
```

El tema es que el `path` ahora que lo quitamos puede ser importante para quien lo llame al metodo `save` actualmente, entonces no podemos hacer estos cambios
sin pensar en las consecuencias de las clases o clientes que utilizan nuestra clase y para que necesitan ese `path` que ya no voy a devolver al menos en el método `save`.

Pero lo que si puedo hacer es agregar un nuevo método para obtenerlo directamente:

```typescript
class FileStore {
  public async save(id: number, message: string): Promise<void> { 
   const path = pathManager.combine(this.workingDirectory, id + ".txt");
   await fileManager.writeAllText(path, message);
  }
  
  public getFileName(id: number): string {
   return pathManager.combine(this.workingDirectory, id + ".txt");
  }
  
  public workingDirectory(): string { ... }
  public async read(id: number): Promise<string> { ... }
}
```

Como ahora el método `getFileName` es una _query_ que es seguro de llamar la cantidad de veces necesarias porque no produce ningún tipo de _side effects_, podemos llamarlo miles de veces sin que afecte al sistema que estamos programando.

Entonces ahora quien lo necesite puede llamarlo directamente para obtener el `path` en base a un `id`, pero quien no lo necesite, simplemente no hará nada.

Aún con el código mejorado vemos una oportunidad de mejora ya que vemos codigo repetido 3 veces

```typescript
class FileStore {
  public workingDirectory(): string { ... }

  public async save(id: number, message: string): Promise<void> { 
    const path = this.getFileName(id);
    await fileManager.writeAllText(path, message);
  }
  
  public async read(id: number): Promise<string> {
    const path = this.getFileName(id);
    const text = await fileManager.readAllText(path);
    
    return text;
  }
  
  public getFileName(id: number): string {
    return pathManager.combine(this.workingDirectory, id + ".txt");
  }
}
```

Esta bien llamar a una _query_ desde un _command_ porque esta _query_ no tiene un efecto secundario sobre nuestro sistema.

Para resolver nuestro problema inicial que no era ni una _query_ ni un _command_, tuvimos que descomponer el problema en dos `operaciones` distintas, una que produce un _side effect_ ya que devuelve `void` y otra que es una consulta ya que devuelve un `string` y es segura de invocar las veces que sea necesario sin producir _side effects_.

La idea de haber realizado todo esto apoyandonos sobre CQS es que tan solo mirando las firmas de los métodos podamos detectar si es una _query_ o un _command_.
Si respetamos esto en todo nuestro código vamos a poder confiar en esa base de codigo de manera sencilla en cuanto a si un método solo consulta informacion o produce algún efecto secundario
o altera algo en nuestro sistema. Además que no necesitamos leer todo el detalle de implementación para darnos cuenta de que hace en general.
Hace que sea más fácil razonar el código a nivel general, sin mirar la implementacion en detalle.

> 💡 `CQS Makes it easier to reason about code.`

Comenzando a leer los nombres de métodos y sus firmas aplicando este patron podremos razonar facilmente sobre lo q esta haciendo sin necesitar el ir a detalles de implementación.

En resumen, el principio CQS nos ayuda a crear un código más claro y sostenible al separar claramente las funciones en comandos y consultas.
Esta práctica no solo mejora la legibilidad sino que también facilita la depuración y el testeo del software.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/211164605-ed461c29-b3c2-4eef-acf3-ad8cd9bdbbdc.png"/></p>
