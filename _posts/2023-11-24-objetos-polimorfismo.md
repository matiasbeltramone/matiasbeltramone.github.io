---
layout: post
title: "🧱 Programando con Objetos: Polimorfismo"
tags: [Paradigmas, POO, OOP, Orientación a Objetos, Object Oriented]
---

El polimorfismo, uno de los conceptos centrales de la programación orientada a objetos, se refiere a la capacidad de que diferentes objetos sean tratados como instancias de la misma clase.
Esencialmente, el polimorfismo permite que distintos objetos respondan a la misma interfaz o método de maneras distintas, facilitando la flexibilidad y extensibilidad en el diseño de software. En la práctica
significa que si un parametro tiene un cierto tipo de clase, cualquier objeto que es una instancia de esa clase puede ser un argumento válido.
Por ejemplo, cualquier instancia de `Gateway` puede ser pasado como un argumento del método `prepare()` como vemos en el siguiente ejemplo:

- 📋 **Cualquier instancia del tipo Gateway será aceptado en prepare()**

```typescript
class Gateway
{
    constructor(gatewayType: GatewayType) { ... }
    
    public pay(payment: Payment) {
      //...
    }
}

class ElectronicPayment
{
  public async prepare(gateway: Gateway): Promise<void>
  {
    const payment = new Payment(...);
    gateway.pay(payment);
  }
}

const gateway = new Gateway(GatewayType.PAYPAL);
const electronicPayment = new ElectronicPayment();

await electronicPayment.prepare(gateway);
```

En este ejemplo, ElectronicPayment puede aceptar cualquier subtipo de Gateway, como PaypalGateway o StripeGateway. Cada uno de estos tipos de gateway puede tener
su propia implementación del método pay, demostrando polimorfismo en acción.

Dado que una instancia de `Gateway` podría haberse configurado de una manera diferente según el caso, o dicho de otra manera
tienen un estado interno diferente al de otra instancia de `Gateway` (ejemplo, que una sea Paypal, Stripe o Skrill...), cada instancia de `Gateway`
podría en teoría comportarse de manera diferente al menos a nivel interno ya que a nivel de firma debería respetar lo que dice que hace, en nuestro caso seguramente varia que tipo de gateway estamos utilizando, es decir, podría ser **Paypal, Stripe, etc**

Las subclases pueden introducir aún más variación en el comportamiento. Ya hemos
analizado la herencia y cómo podemos usarla para cambiar el comportamiento de una clase principal anulando (parte de) su comportamiento en una subclase.
Cualquier objeto que sea una instancia de una subclase de `Gateway` también cuenta como una instancia de `Gateway` en sí.
Esto hace que cualquier instancia de esa subclase de `Gateway` también es un argumento válido para los parámetros de tipo `Gateway`.

- 📋 **Cualquier subclase del tipo Gateway será aceptado en prepare()**

```typescript
class Gateway
{    
    public pay(payment: Payment) {
      //...
    }
}

class PaypalGateway extends Gateway {
    public pay(payment: Payment) { // Sobreescribimos la implementación por defecto de la clase Gateway con las particularidades de Paypal.
      const prepareInformation = await this.paypal.process(payment);
      //...
    }
}

class StripeGateway extends Gateway {
    public pay(payment: Payment) { // Sobreescribimos la implementación por defecto de la clase Gateway con las particularidades de Stripe.
      const prepareInformation = await this.stripe.prepare(payment); // Vemos que Stripe primero necesita de un preprocesamiento para realizar el pago y luego lo procesa.
      const processed = await this.stripe.process(prepareInformation);
      //...
    }
}

class ElectronicPayment
{
  public async prepare(gateway: Gateway): Promise<void>
  {
    const payment = new Payment(...);
    gateway.pay(payment);
  }
}

const paypalGateway = new PaypalGateway();
const electronicPayment = new ElectronicPayment();

await electronicPayment.prepare(gateway);
```

Se puede usar tanto con `Paypal` como vemos en el ejemplo anterior o con `Stripe` como vemos a continuación:

```typescript
const paypalGateway = new PaypalGateway();
const electronicPayment = new ElectronicPayment();

await electronicPayment.prepare(gateway);
```

Es indistinto ya que en definitiva son objetos polimorficos que responden ambos al método `pay()` siendo del tipo `Gateway` el cuál es el necesario para el funcionamiento del método `prepare()` dentro de `ElectronicPayment`.

Usar subclases para cambiar el comportamiento de los objetos **a menudo no se recomienda**. En la mayoría de las situaciones, es mejor usar **polimorfismo** con un
tipo de parámetro en el método que sea una interfaz. Esto se ve igual en el código, pero ahora `Gateway` es una interfaz.

- 📋 **Cualquier instancia de la interfaz Gateway es válida en el método prepare()**

```typescript
interface Gateway
{
  public pay(payment: Payment);
}

class ElectronicPayment
{
  public async prepare(gateway: Gateway): Promise<void>
  {
    const payment = new Payment(...);
    gateway.pay(payment);
  }
}
```

En base a esta intefaz declarada veamos un ejemplo para ver como dos objetos son polimórficos gracias a ello:

```typescript
interface Gateway
{
  public pay(payment: Payment);
}

class PaypalGateway implements Gateway {
  public async pay(payment: Payment): Promise<void> {
     const prepareInformation = await this.paypal.prepare(payment);
     //...
  }
}

class StripeGateway implements Gateway {
  public async pay(payment: Payment): Promise<void> {
     const prepareInformation = await this.stripe.prepare(payment);
     const processed = await this.stripe.process(prepareInformation);
     //...
  }
}

class ElectronicPayment
{
  public prepare(gateway: Gateway): void
  {
    const payment = new Payment(...);
    gateway.pay(payment);
  }
}
```

Al utilizar la interfaz `Gateway` como contrato ahora tenemos la implementación de dos objetos polimórficos entre sí gracias a ella, `PaypalGateway` y `StripeGateway` los cuales en la forma de pago electrónica pueden ser utilizados indistintamente como argumentos de entrada del método `prepare()`, más allá de que cada uno de los gateways de pago realicen internamente diferentes pasos para poder realizar el pago con que sepan responder al mensaje polimórfico `pay()` es suficiente para que se puedan intercambiar y utilizar sin mayores problemas.

```typescript
const payment = new Payment(...);
const gateway = new PaypalGateway(paypalConfiguration);

await this.electronicPayment.prepare(gateway);
```

```typescript
const payment = new Payment(...);
const gateway = new StripeGateway(apiKey);

await this.electronicPayment.prepare(gateway);
```

Vemos como podemos utilizar ambas clases que seguramente tienen diferentes formas de configurarse, uno quizas necesita todo un objeto de configuración y el otro simplemente una key para poder utilizar el servicio, además como podemos observar uno necesita una llamada a la API por ejemplo un método `prepare()` y el otro gateway necesita de dos llamadas para realizar un pago `prepare() y process()` pero más allá de eso ambos realizan la misma función, es decir, poder realizar un pago mediante el método `pay()` el cuál es requerido por la interfaz `Gateway`.

### Ejemplo en Otro Contexto

Imaginemos un sistema de notificaciones donde diferentes servicios (EmailNotifier, SMSNotifier, PushNotifier) implementan una interfaz común Notifier. Cada servicio tiene su propia manera de enviar notificaciones, pero desde la perspectiva del sistema, todos son tratados uniformemente como Notifier.

```typescript
interface Notifier {
  notify(message: string): void;
}
```

### ⚖️ Ventajas y Precauciones
- Ventajas: El polimorfismo aumenta la reutilización del código y la flexibilidad del diseño. Permite escribir código más genérico y extensible, facilitando el mantenimiento y la escalabilidad.
- Precauciones: Sin embargo, un uso excesivo puede llevar a un diseño complicado, especialmente si la jerarquía de herencia se vuelve muy profunda o confusa.

### 🖋️ Conclusión
El polimorfismo es una herramienta poderosa en el arsenal de la POO, facilitando un diseño de software flexible y mantenible. Nos permite pensar en términos de interfaces y comportamientos más que en tipos concretos, lo que abre puertas a un diseño de software más abstracto y robusto.

### 💬 Tu Turno
¿Has utilizado el polimorfismo en tus proyectos? ¿Qué desafíos o ventajas has encontrado? ¿Lo utilizaste para reemplazar IFs por polimorfismo?

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/211164605-ed461c29-b3c2-4eef-acf3-ad8cd9bdbbdc.png"/></p>
