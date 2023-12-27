---
layout: post
title: "🧱 Programando con Objetos: Excepciones y Returns"
tags: [Paradigmas, POO, OOP, Orientación a Objetos, Object Oriented]
---

Cuando llamamos a un método, normalmente se ejecutara sentencia a sentencia desde el principio hasta encontrar una sentencia con un `return`, o al encontrar el final del método. Si en algún punto quisieras prevenir que se siga ejecutando el método, podemos insertar un `return`, asegurandonos que todo el resto del método no se ejecutará.

- 📋 **Una sentencia return nos previene de que el método se siga ejecutando bajo ciertas circunstancias**

```typescript
class PaymentProcessor
{
  public prepare(): void
  {
    if (someConditionToStopExecution) {
      return; // A method with a void return type returns nothing.
    }
    
    //...
    
    this.gateway.execute(...);
  }
  
  public process(): boolean
  {
    if (someOtherConditionToStopExecution) {
      return false; // A method with a specific return type can return a specific value.
    }
    
    // ...
    return true;
  }
}
```

```typescript
  if (someConditionToStopExecution) {
    return; // A method with a void return type returns nothing.
  }
```

Este tipo de condiciones suelen llamarse _guard clauses_ y utilizan el _early return_ para salir del método lo antes posible en caso de que algo no sea valido o esperado, suelen ayudar a dejar más limpio el código haciendo que ciertas condiciones de validaciones se deben cumplir como pre-condiciones para poder ir al flujo principal del método asegurando todo lo necesario para realizar el comportamiento esperado.

Otro camino para parar la ejecución de un método es lanzar una excepción. Una excepción es un tipo especial de objeto que, cuando es instanciado, recoge información sobre donde el objeto (excepción) fue instanciado y que paso antes de que ocurra (también llamado _stack trace_).

Normalmente una excepción indica algún tipo de fallo, como podrían ser los siguientes puntos:

- Argumentos de entrada a un método que no son válidos.
- Un estructura _Map_ que no tiene un valor determinado para una clave dada o que no existe la clave de esa lista de elementos.
- Un servicio externo que no puede ejecutarse o que al llamarlo provoca un error.

Dejamos un ejemplo para ver como lanzamos una excepción.

- 📋 **Una excepción también nos previene de que el método se siga ejecutando bajo ciertas circunstancias**

```typescript
class PaymentProcessor
{
  public prepare(arguments): void
  {
    const validator = this.validatorService.validate(arguments);
    
    if (validator.hasInvalidArguments()) {
      throw new RuntimeException('Something is wrong'); // Podemos proporcionar un mensaje determinado para cada tipo de excepción. (Nos da un poco de contexto del problema que hay en el programa).
    }
    
    // ...
  }
}
```

Tan pronto como sea claro que el método no esta en condiciones de realizar su trabajo correctamente, debemos lanzar una excepción. La diferencia con un simple _return statement_ es que el método no devuelve nada cuando lanza una excepción. De hecho, la ejecución del programa se detiene y solo puede ser recogido (el objeto o excepción) por un cliente que ha envuelto la llamada al método dentro de un bloque _try/catch_ como veremos a continuación.

- 📋 **Un cliente se puede recuperar de una excepción si utiliza el bloque _try/catch_**

```typescript
const paymentProcessor = new PaymentProcessor();

try {
  paymentProcessor.prepare(arguments); // Este lanza la excepción
} catch (error) {
  if (error instanceof Exception) {
    console.log('An error was occurred: ', error);
  }
  // ...
}
```

Los lenguajes de programación vienen con sus propios conjuntos de excepciones predefinidos. Ellos forman algún tipo de jerarquía normalmente, como pueden ser `RuntimeException` la cuál suele extender de un tipo `Exception` genérico, ó `InvalidArgumentException` que puede extender de un tipo como `LogicException` que a su vez extiende de la general `Exception`.

Además nosotros también podemos definir nuestras propias clases de excepciones. Estás siempre deben extender de una que ya venga predefinida del lenguaje, como mostraremos a continuación.

- 📋 **Definiendo excepciones personalizadas**

```typescript
class CanNotFindPayment extends RuntimeException
{
  constructor(id: PaymentId) {
    super(`Payment with id: ${id} was not found`);
  }
}
```

```typescript
class PaymentProcessor
{
  public async pay(id: PaymentId): Promise<void>
  {
    //...
    
    const payment = await this.repository.search(id);
    
    if (!payment) {
      throw new CanNotFindPayment(id);
    }
    
    // ...
  }
}
```

De la misma manera podemos atraparla en una clase cliente más arriba mediante el bloque _try/catch_ de la siguiente manera:

```typescript
const payment = new PaymentProcessor();

try {
  await payment.pay(1); // Este lanza la excepción
} catch (error) {
  if (error instanceof CanNotFindPayment) {
    console.log('An error was found: ', error.message);
  }
  
  await this.alertManager.send(someAlertMessageOfUnexpectedError);
}
```

Las excepciones son un aspecto importante del diseño de objetos. Son parte del conjunto completo de comportamientos que un cliente puede esperar de un objeto.

## Conclusión
Las sentencias de retorno y el manejo de excepciones son herramientas poderosas en TypeScript y en cualquier lenguaje que las utilice. Utilizadas correctamente, mejoran la claridad, la robustez, la sostenibilidad y el evitar repetir código, lo cuál no es un detalle menor.
Es importante recordar que la excepción es para situaciones excepcionales y no debe usarse para controlar el flujo normal del programa. Con estas prácticas, puedes escribir código más limpio, eficiente y fácil de mantener.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/211164605-ed461c29-b3c2-4eef-acf3-ad8cd9bdbbdc.png"/></p>
