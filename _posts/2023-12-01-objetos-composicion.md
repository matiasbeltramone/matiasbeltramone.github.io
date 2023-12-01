---
layout: post
title: "🧱 Programando con Objetos: Composición"
tags: [Paradigmas, POO, OOP, Orientación a Objetos, Object Oriented]
---

En nuestras lecciones anteriores, hemos explorado varios conceptos fundamentales de la programación orientada a objetos. Hoy, profundizaremos en el concepto de composición, un pilar esencial en el diseño de software robusto y flexible. La composición no solo es crucial para entender cómo los objetos pueden trabajar juntos de manera efectiva, sino que también es una piedra angular para lograr un código más limpio, sostenible y escalable. A través de ejemplos prácticos, veremos cómo la **composición** puede transformar nuestra manera de construir y estructurar nuestras aplicaciones.

## Composición

Además de ser un ejemplo de _polimorfismo_, el ejemplo de la lección anterior también muestra cómo un objeto `Gateway` puede ser utilizado por otro objeto
de tipo `ElectronicPayment` para realizar parte de su trabajo. Si `Gateway` es un servicio, también se puede proporcionar a `ElectronicPayment` como un argumento del constructor. `ElectronicPayment` podría luego asignar el objeto `Gateway` a una de sus propiedades de instancia.

- 📋 **La instancia de Gateway es asignada a una propiedad de ElectronicPayment**

```typescript
class ElectronicPayment
{
  private readonly gateway: Gateway;
  
  public construct(gateway: Gateway)
  {
    this.gateway = gateway;
  }
}
```

La asignación de un objeto a la propiedad de otro objeto se denomina composición de objetos (_object composition_). Estás
construyendo un objeto más complejo a partir de objetos más simples. La composición de objetos
se puede combinar con **polimorfismo** para componer tú objeto a partir de otros objetos,
cuyo tipo (de interfaz) conocemos, pero no la clase real o la implementación de ese momento.

La composición se puede usar con un objeto del tipo servicio _(más adelante indagaremos más en este tema)_, haciendo que parte de su comportamiento sea configurable.
También se puede usar con otros tipos de objetos, como entidades (a veces conocidas
como "modelos" aunque en lo que a mi y a muchos en la industria respecta son conceptos diferentes), donde la composición se utiliza para elementos secundarios relacionados. Por ejemplo, un objeto de pedido (_Order_) que contiene objetos de línea (_OrderLine_) podría usar la composición para establecer la relación entre un pedido y sus líneas de compra, siendo una línea de compra por ejemplo "Coca-Cola 2L x2unidades", es decir, que line contiene una descripción y una cantidad del producto. En ese caso, un cliente podría no proporcionar un solo
Objeto _Line_, pero si una colección de objetos _Line_ ya que representaría a varios productos con sus cantidades respectivamente.

- 📋 **Un objeto Order asigna multiples objetos Line a su propiedad _lines_**

```typescript
class Order
{
  private lines: Array<Line>;
  
  public construct(lines: Array<Line>)
  {
    this.lines = lines; // Each element in lines is a Line object.
  }
}
```

Depende el lenguaje utilizado podremos tener colecciones, listas, arrays, más tipados o menos tipados, pero se entiende el concepto buscado, donde básicamente esperamos recibir un conjunto de datos del mismo tipo, en este caso _Line_ para asignarlo como una lista de compra de un pedido u _Order_.

En resumen, la composición nos ofrece una herramienta poderosa para construir estructuras de software más flexibles y sostenibles en el tiempo. A diferencia de la herencia, que a menudo introduce rigidez y dependencias complicadas, la composición nos permite ensamblar objetos de manera más natural y modular. A medida que avanzamos hacia temas más avanzados como el encapsulamiento en nuestra próxima lección, veremos cómo estos conceptos se entrelazan para formar los principios básicos de un diseño de software efectivo y eficiente. Espero que estos ejemplos hayan ilustrado claramente la importancia de la composición y cómo puede ser aplicada para mejorar la calidad y la flexibilidad de nuestros códigos.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/211164605-ed461c29-b3c2-4eef-acf3-ad8cd9bdbbdc.png"/></p>
