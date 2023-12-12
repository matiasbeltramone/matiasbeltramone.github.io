---
layout: post
title: "🧱 Programando con Objetos: Abstracción"
tags: [Paradigmas, POO, OOP, Orientación a Objetos, Object Oriented]
image: https://github.com/matiasbeltramone/matiasbeltramone.github.io/assets/22304957/4d7caf66-7b33-4652-b4de-eb2490ee99f5
---

Bienvenidos a una nueva lección de nuestra serie sobre Programación Orientada a Objetos. En nuestra lección anterior, exploramos el concepto de encapsulamiento y cómo nos ayuda a ocultar la complejidad innecesaria,
como los detalles de implementación y la protección de invariantes en nuestro sistema. Hoy, avanzaremos un paso más y nos sumergiremos en el mundo de la **Abstracción**. Este principio es fundamental en POO,
ya que nos permite centrarnos en lo que es relevante para un contexto específico, omitiendo los detalles que no son necesarios. Veremos cómo la abstracción nos ayuda a construir modelos más limpios y sostenibles de la realidad,
facilitando el manejo de sistemas complejos.

## 🎨 Abstracción

La mayoría del tiempo, cuando creamos programas con **POO**, creamos objetos basados en objetos de la vida real. Sin embargo, los objetos del programa no representan a los reales con un 100% de precisión (y es raro que esto suceda). En cambio, tus objetos solo modelan atributos y comportamientos del objeto real en un contexto específico que necesitamos, ignorando el resto.

Por ejemplo, en una clase `Airplane`, podríamos considerar su existencia tanto en un simulador de vuelos como en una aplicación de reservas de avión. Pero, en el contexto de la aplicación de reservas, deberías preocuparte solo sobre mostrar el mapa de asientos y si están disponibles o no, para que un cliente pueda realizar la reserva.

<p align="center"><img src="https://user-images.githubusercontent.com/22304957/71416078-b7fea180-263d-11ea-8940-51e7b67deade.png"/></p>
<p align="center">Fuente: © Dive Into DESIGN PATTERNS - 2014-Actualidad Refactoring.Guru. All rights reserved. Illustrations by Dmitry Zhart</p>

Lo mismo nos puede pasar, por ejemplo, con un Billete como el Dólar Estadounidense (USD).

<p align="center"><img src="https://user-images.githubusercontent.com/22304957/218905678-19da63c2-80c5-4157-a3fa-c840c642efe0.png"/></p>

Para las personas en el día a día, solo representa un papel que tiene un valor, ejemplo: 1 USD. Por lo tanto, seguramente estaría representado de la siguiente manera:

```typescript
class Money {
  constructor(amount: Amount, currency: Currency) {
    this.amount = amount;
    this.currency = currency;
  }
}
```

Como vemos, para las personas que lo utilizan cada día importan dos cosas:

- Monto (Amount): 1
- Tipo de Moneda (Currency): USD

Por otro lado, si el sistema a modelar fuera dentro de la Reserva Federal de Estados Unidos (FED), seguramente el modelado del objeto `Money` tendría más características, como por ejemplo:

```typescript
class Money {
  constructor(id: Identification, amount: Amount, currency: Currency) {
    this.id = id;
    this.amount = amount;
    this.currency = currency;
  }
}
```

Siendo el ID algo muy importante para la entidad del banco central, ya que permite verificar la identidad del objeto creado y realizar un seguimiento de su ciclo de vida en la sociedad.

### 🌐 Otros Ejemplos 

- **Sistema de Gestión de Biblioteca**: Imagina una clase `Libro` en una aplicación de biblioteca. En un contexto simple, podrías modelar atributos como título, autor y ISBN. Pero, en un contexto más complejo, como un sistema de gestión de biblioteca, podrías necesitar detalles adicionales como ubicación física, historial de préstamos y estado de conservación.

- **Aplicación de Gestión de Empleados**: En una aplicación para la gestión de empleados, una clase `Empleado` puede ser muy diferente según el departamento. Por ejemplo, para el departamento de RRHH, los detalles relevantes podrían incluir historial de empleo y evaluaciones de desempeño, mientras que para el departamento de contabilidad, serían más importantes los detalles relacionados con la nómina y los beneficios.

- **Sistema de Monitoreo Ambiental**: En un sistema de monitoreo ambiental, una clase `Sensor` podría abstraerse de diferentes maneras. Para la visualización de datos, podría ser suficiente modelar la ubicación y el tipo de sensor. Pero para la calibración y el mantenimiento técnico, necesitarías detalles más profundos como especificaciones técnicas y registros de mantenimiento.

### 🎓 Conclusión
La `Abstracción` es un modelo de un fenómeno u objeto del mundo real, limitado a un contexto específico, que representa todos los detalles relacionados a ese contexto o situación con una alta precisión y omitiendo todo el resto que no es necesario para el problema de negocio planteado.

Al igual que el encapsulamiento, la abstracción nos ayuda a gestionar la complejidad y a construir sistemas más claros y sostenibles. A medida que continuamos explorando los principios de POO, recuerda que la abstracción no se trata solo de omitir detalles, sino de capturar la esencia de lo que estamos modelando. En nuestra próxima lección, profundizaremos un poco más sobre el modelado en sí, ampliando aún más nuestro entendimiento de la programación orientada a objetos.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/211164605-ed461c29-b3c2-4eef-acf3-ad8cd9bdbbdc.png"/></p>
