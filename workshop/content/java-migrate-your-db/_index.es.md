+++
title = "Migrar su Base de Datos"
weight = 30
pre = "<b>- 1. </b>"
+++


En esta sección, realizará una migración de base de datos utilizando la base de datos PostgreSQL de origen y como destino una base de datos Amazon RDS para PostgreSQL con **AWS Database Migration Service**. Dado que se trata de una migración de base de datos homogénea (los motores de la base de datos de origen y de destino son los mismos), la estructura del esquema, los tipos de datos y el código de la base de datos son compatibles entre las bases de datos de origen y de destino, lo que significa que este tipo de migración no requiere ninguna conversión del esquema.

Puede revisar <a href="https://docs.aws.amazon.com/es_es/dms/latest/userguide/CHAP_Source.PostgreSQL.html#CHAP_Source.PostgreSQL.Homogeneous" target="_blank">Migración de PostgreSQL a PostgreSQL con AWS DMS</a> para conocer los detalles y las limitaciones.

![1](/db-mig/DMS-overview.png)

Puede conocer más detalles sobre este servicio viendo el siguiente vídeo.

<center><iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/zb4GcjEdl8U" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

En este laboratorio, usted realizará los siguientes pasos:

1. [Crear DB de destino]({{< ref "/Create-target-DB.es.md" >}})

2. [Crear Instancia de Replicación]({{< ref "Create-Replication-instance.es.md" >}})

3. [Crear endpoints de origen y destino]({{< ref "Create-Source-and-Target-endpoints.es.md" >}})

4. [Configurar la base de datos de origen]({{< ref "configure_source_database.es.md" >}})

5. [Crear y ejecutar la tarea de replicación]({{< ref "Create-and-run-Replication-task.es.md" >}})

6. [Resumen]({{< ref "Summary.es.md" >}})
