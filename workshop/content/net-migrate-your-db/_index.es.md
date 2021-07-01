+++
title = "Migra su Base de Datos"
weight = 120
pre = "<b>- 1. </b>"
+++


En esta sección, realizará una migración de base de datos desde la base de datos SQL de origen a una base de datos de Amazon RDS con MSSQL de destino con el método de la migración de **Copia de seguridad y restauración nativa**.

Dado que se trata de una migración de base de datos homogénea (los motores de la base de datos de origen y de destino son los mismos), la estructura del esquema, los tipos de datos y el código de la base de datos son compatibles entre las bases de datos de origen y de destino, lo que significa que este tipo de migración no requiere ninguna conversión del esquema.

{{% notice note %}}
En este escenario, la base de datos de origen no requiere ninguna replicación en tiempo real, por lo que el Servicio de Migración de Bases de Datos (DMS) no se utiliza para la replicación.
{{% /notice %}}

El siguiente diagrama muestra los pasos de alto nivel cuando se utilizan migraciones de copia de seguridad y restauración nativas.

Puede comprobar <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Procedural.Importing.html" target="_blank">Importación y exportación de bases de datos de SQL Server</a> para detalles y limitaciones.

![1](/db-mig/net-db-migration-overview.png)

En este laboratorio, realizarás los siguientes pasos:

1. [Crear una DB de destino]({{< ref "/netCreate-target-DB.es.md" >}})

2. [Realizar una copia de la DB de origen]({{< ref "backup-source-db.es.md" >}})

3. [Restaurar la DB en RDS]({{< ref "restore-db-on-rds.es.md" >}})

4. [Cambiar los datos de conexión de la Aplicación]({{< ref "change-conn-string.es.md" >}})

5. [Resumen]({{< ref "Summary.es.md" >}})
