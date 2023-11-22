---
layout: post
title: "🧱 Programando con Objetos: Herencia"
tags: [Paradigmas, POO, OOP, Orientación a Objetos, Object Oriented]
---

Es posible definir solo una parte de una clase y dejar a otros expandirla. Por ejemplo, podemos tener una clase sin propiedades y métodos, pero si lo que llamamos firmas de métodos o que algunos llaman `contratos`. Ese tipo de clase normalmente es llamado `interface`, y muchos lenguajes orientados a objetos permiten definir ese tipo de clase. Una clase luego puede implementar la interfaz y proveer la implementación concreta de los métodos que fueron declarados o definidos en la interfaz.

### Interfaces

- 📋 **CashPayment y ElectronicPayment implementando la interfaz PaymentProcessor**

```typescript
interface PaymentProcessor
{
  public process(): Promise<void>;
}
```

La interfaz `PaymentProcessor` declara al método `process()` como contrato a cumplir, pero no ofrece una implementación concreta.

```typescript
class CashPayment implements PaymentProcessor {}
```

`CashPayment` es una implementación incorrecta de `PaymentProcessor`, por qué no tiene una implementación concreta para `process()` por lo tanto esta clase no funcionará.

```typescript
class ElectronicPayment implements PaymentProcessor {
  public async process(): Promise<void>
  {
    // ...
  }
}
```

`ElectronicPayment` es una correcta implementación del `PaymentProcessor` ya que provee una implementación concreta del método `process()`.

### Clases Abstractas

Una interfaz no define ninguna implementación, pero una `abstract class` puede hacerlo si es que lo desea. Esta nos permite proveer la implementación de algunos métodos y la firma de algunos otros, dependiendo de la necesidad en cuestión. Una clase abstracta **no puede** ser instanciada, primero tiene que ser extendida por una clase que provea de las implementaciones para los métodos `abstractos`, en caso de existir.

- 📋 **ElectronicPayment extiende de la clase abstracta PaymentProcessor**

```typescript
abstract class PaymentProcessor
{
  abstract public prepare(): Promise<void>; // Debe ser definido por las subclases que extienden de PaymentProcessor
  
  public async process(): Promise<void> // Provee una implementación por defecto que heredará la subclase, pudiendo esta utilizarla o sobreescribirla.
  {
    // ...
  }
}
```

```typescript
class ElectronicPayment extends PaymentProcessor
{
  public async prepare(): Promise<void>
  {
    //...
  }
}
```

`ElectronicPayment` es una correcta implementación de `PaymentProcessor` ya que provee de una implementación concreta para el método abstracto `prepare()` que fue declarado en la clase padre.

### Clases

Finalmente, una clase puede proveernos de implementaciones para todos sus métodos pero permitir a otras clases extenderla, abriendo la posibilidad de que puedan `sobreescribir` la implementación por defecto de nuestra clase.

- 📋 **ElectronicPayment extiende de PaymentProcessor y le cambia parte de su comportamiento**

```typescript
class PaymentProcessor // PaymentProcessor es una clase normal sin métodos abstractos
{
  public async prepare(): Promise<void>
  {
    // Hacer algo como comportamiento por defecto.
  }
}

class ElectronicPayment extends PaymentProcessor
{
  public async prepare(): Promise<void>
  {
    // Hacer algo diferente a la clase padre.
  }
}
```

`ElectronicPayment` hereda de `PaymentProcessor`, que ahora es una clase padre. Además se puede sobreescribir el comportamiento del método prepare() en la subclase.

Las clases que extienden de otra clase pueden tener acceso a los métodos que tienen el _scope_ público y protegidos dentro de la clase padre.

- 📋 **Acceder a métodos públicos y protegidos**

```typescript
class Payment
{
  public async prepare(): Promise<void>
  {
  }
  
  protected async process(): Promise<void>
  {
  }
  
  private async cancel(): Promise<void>
  {
  }
}

class CashPayment extends Payment
{
  public async someMethod(): Promise<void>
  {
    await this.prepare(); // prepare() esta disponible porque esta declarado como método público.
    await this.process(); // process() esta disponible porque esta declarado como método protegido.
    await this.cancel(); // cancel() no esta disponible ya que es un método privado de clase.
  }
}
```

Las clases hijas solamente puede sobreeescribir los métodos que usen el atributo `público` y `protegido` dentro de la clase padre.

- 📋 **Sobreescribiendo métodos públicos y protegidos**

```typescript
class Payment
{
  public async prepare(): Promise<void>
  {
  }
  
  protected async process(): Promise<void>
  {
  }
  
  private async cancel(): Promise<void>
  {
  }
}

class CashPayment extends Payment
{
  public async prepare(): Promise<void> // prepare() puede ser sobreescrito porque es un método público.
  {
    // ...
  }
  
  protected async process(): Promise<void> // process() puede ser sobreescrito porque es un método protegido.
  {
    // ...
  }
  
  private async cancel(): Promise<void> // cancel() no puede ser sobreescrito porque es un método privado.
  {
    // No funciona
  }
}
```

### Ejemplo Avanzado: Herencia en Frameworks MVC
Para aquellos con conocimientos más avanzados, especialmente aquellos familiarizados con frameworks MVC como Laravel, Nest.js, o Ruby on Rails, la herencia se manifiesta de manera significativa en los modelos del dominio. Aquí se emplea un patrón comúnmente conocido como Active Record. En este contexto, la mayoría de los modelos en nuestro sistema heredan de una clase base, que puede llamarse "Model", "Entity" o "Database", según la convención del framework específico.

Este enfoque de herencia nos proporciona acceso a funcionalidades relacionadas con la manipulación de datos, tales como consultas a la base de datos, manejo de relaciones entre entidades, validaciones, entre otros. Por ejemplo, en un framework como Laravel, al extender la clase "Model", un modelo de usuario automáticamente hereda métodos para realizar operaciones de base de datos como guardar, actualizar, buscar o eliminar registros.

Es importante señalar que este ejemplo no pretende debatir la eficacia o las implicaciones del uso del patrón Active Record, sino que sirve como una ilustración práctica del poder y la versatilidad de la herencia en la programación orientada a objetos, especialmente en entornos de desarrollo de aplicaciones web modernas.

### Reflexión

Aunque la herencia es una característica fundamental de la programación orientada a objetos, su uso debe ser medido y considerado cuidadosamente.
En la práctica, un uso excesivo o inadecuado de la herencia puede conducir a un diseño de software confuso y difícil de mantener.
Sin embargo, hay situaciones donde la herencia es particularmente útil y eficaz:

1. **Definir Interfaces para Dependencias**: Especialmente útil cuando la implementación implica interactuar con sistemas de terceros.
2. **Crear Jerarquías de Objetos**: Como en el caso de las excepciones personalizadas que extienden de clases de excepción predefinidas.

Sin embargo, es importante mantener un equilibrio entre la herencia y la composición. La composición, al ofrecer una alternativa más flexible, puede ser preferible en muchos casos donde
la herencia podría parecer la opción obvia.

### Cautelas en el Uso de la Herencia
- **Evitar el Uso Excesivo**: La herencia no siempre es la solución óptima. Es crucial evaluar si realmente aporta claridad y simplificación al diseño del código.
- **Uso de final o equivalentes**: En algunos lenguajes, la palabra clave final (o su equivalente) puede utilizarse para prevenir que otras clases extiendan de la nuestra, asegurando un mayor control sobre la arquitectura del software.
- **Alternativas a la Herencia**: Patrones de diseño como Estrategia o Decorador pueden ofrecer alternativas viables a la herencia, evitando algunos de sus inconvenientes comunes.

#### Ejemplo uso de final keyword

- 📋 **Payment no puede ser extendida**

```typescript
final class Payment
{
  // ...
}

class ElectronicPayment extends Payment // Payment fue marcada como final, entonces ElectronicPayment no puede extenderla.
{
  // ... No funciona
}
```

IMPORTANTE: En el caso particular de TypeScript, lenguaje en el que estamos dando los ejemplos, esto no es posible de realizar, el ejemplo anterior mostrado es simbólico.

### Casos de Uso Apropiados
A pesar de sus limitaciones, la herencia es extremadamente útil en ciertos contextos. Por ejemplo, en la creación de frameworks o bibliotecas donde se espera que los usuarios extiendan
y personalicen funcionalidades específicas. En estos casos, la herencia puede proporcionar una base sólida y flexible para la extensión de funcionalidades.

### Conclusión
En resumen, la herencia en la programación orientada a objetos es una poderosa herramienta que permite a los desarrolladores crear estructuras de código eficientes y reutilizables.
A través de interfaces, clases abstractas y herencia de clases, podemos definir comportamientos comunes y estructuras en una clase base, permitiendo que las clases derivadas los adopten
o modifiquen según sea necesario.

- **Interfaces**: Proporcionan un contrato que otras clases pueden implementar, asegurando la consistencia en la forma en que diferentes clases son utilizadas.
- **Clases Abstractas**: Ofrecen una mezcla entre implementaciones concretas y métodos abstractos, sirviendo como un esqueleto para clases más específicas.
- **Herencia de Clases**: Permite reutilizar y extender el comportamiento de las clases existentes, lo cual es fundamental para evitar la duplicación de código y facilitar el mantenimiento.

Sin embargo, es crucial utilizar la herencia con precaución y entender cuándo es apropiado su uso. En muchos casos, la composición puede ser una alternativa más flexible a la herencia.
Además, es importante recordar que no todas las lenguajes de programación manejan la herencia de la misma manera, por lo que las prácticas pueden variar dependiendo del lenguaje en uso.

En conclusión, la herencia, cuando se utiliza correctamente, puede ser un elemento esencial para un diseño de software robusto y modular.
Invito a los desarrolladores a explorar estos conceptos y aplicarlos de manera que enriquezcan sus habilidades de programación y la calidad de sus proyectos.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/211164605-ed461c29-b3c2-4eef-acf3-ad8cd9bdbbdc.png"/></p>
