+++
title = "Resumen de la modernización"
weight = 4
+++

No hay un único camino que nuestros clientes tomen cuando se modernizan en AWS, pero una vez que comienzas a modernizar tus aplicaciones obtienes muchos beneficios importantes como los siguientes:

1. **Agilidad**: Desarrollar y desplegar más rápido para lograr los objetivos de negocio.
2. **Desarrollo empresarial**: Construya y opere utilizando un ecosistema probado de herramientas nativas de la nube.
3. **Portable y aislado**: Permitir el despliegue de aplicaciones portátil, escalable y aislado.
4. **Eficiencia operativa**: Reduzca la carga operativa de TI y consiga una infraestructura informática optimizada.

Para este taller, usted ha identificado 2 aplicaciones en su empresa. Y ha pensado que serían buenas candidatas para la modernización.

- A. Aplicación Java Tomcat
- B. Aplicación .NET

-------------
Pero tiene algunos retos:

1. Las aplicaciones son antiguas y el dueño de la aplicación ha dejado la empresa hace años sin dejar ninguna documentación.
2. Usted o su equipo no son expertos en el proceso de contenerización, por lo que no se sienten cómodos tocando estas aplicaciones.
3. Usted o su equipo ha comenzado recientemente su viaje a la nube con AWS y quiere estar seguro de que está siguiendo las mejores prácticas de la nube mientras despliega las aplicaciones en la nube.
4. Usted o su equipo están muy ocupados y no pueden dedicar demasiado tiempo a contenerizar estas aplicaciones.

-------------
Teniendo en cuenta estos retos:

- En este taller, utilizará las herramientas de migración y modernización de AWS para ayudarle en su viaje de modernización.
- Para cada aplicación, utilizará diferentes herramientas de migración y modernización en función de su caso de uso y sus requisitos:
    1. <a href="https://aws.amazon.com/dms/" target="_blank">Database Migration Service (DMS)</a> para migrar su base de datos a AWS RDS (Relational Database Service).
    2. <a href="https://aws.amazon.com/app2container" target="_blank">AWS App2Container (A2C)</a> para analizar, contenerizar e implementar su aplicación en Amazon ECS (Elastic Container Service).

#### Ahora, navega a [Cómo empezar]({{< ref "getting-started/_index.es.md" >}}) y elige tu aplicación (Java o .NET) para empezar a modernizarla.

{{% notice note %}}
Cada aplicación sigue procesos similares, por lo que puede seguir cada una de ellas de forma independiente a la otra.
{{% /notice %}}
