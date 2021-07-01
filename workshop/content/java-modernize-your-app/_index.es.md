+++
title = "Modernice su aplicación Java"
weight = 20
pre = "<b>A. </b>"
+++

En esta sección del taller, modernizará su aplicación Java.

Su aplicación Java se esta ejecutando sobre Tomcat, y esta se conecta a un servidor separado de base de datos PostgreSQL.

- No tiene demasiada información sobre su aplicación Java y al final de este módulo la desplegará en la arquitectura que se muestra a continuación.
![source-target](/modernization/java-source-target.png)

En este taller, utilizará las herramientas de migración y modernización de AWS que lo ayudará en su viaje de modernización:

1. Database Migration Service (DMS) para migrar su base de datos PostgreSQL a AWS RDS (Relational Database Service).
2. App2Container para
    - Analizar,
    - Containerizar,
    - Crear artefactos de despliegue de la aplicación,
    - Desplegar su aplicación en Amazon ECS (Elastic Container Service)
    - Crear un pipeline de CI/CD (Opcional)

NOTA: La migración de la base de datos es opcional en este módulo;

1. Continúe con la siguiente sección [Migrar su Base de Datos]({{< ref "/java-migrate-your-db/_index.es.md" >}}) para migrar su base de datos utilizando el servicio Database Migration Service (DMS)
2. Como alternativa, continúe con [Containerizar su App Java]({{< ref "/java-containerize-your-app/_index.es.md" >}}) para iniciar directamente los procesos de App2Container.

¡Buena Suerte!
