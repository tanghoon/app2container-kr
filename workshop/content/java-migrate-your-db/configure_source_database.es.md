+++
title = "Revisar la configuración de la base de datos de origen"
weight = 25
pre = "<b>1.3 </b>"
+++

{{% notice warning %}}
Esta sección es opcional y de carácter informativo. Todas las configuraciones ya están actualizadas para este taller. Sin embargo, puede seguir accediendo al servidor de bases de datos y revisar las configuraciones que se indican a continuación.
{{% /notice %}}  

### Ejecutar la tarea de DMS con replicación continua (Change Data Capture - CDC)

Para garantizar un tiempo de inactividad mínimo para la migración de la base de datos, va a utilizar la replicación continua de los cambios (también conocida como **Captura de datos de cambios (CDC)**) desde la base de datos de origen a la base de datos de destino. Para más información sobre CDC y el soporte nativo de CDC de **AWS DMS** vea <a href="https://aws.amazon.com/blogs/database/aws-dms-now-supports-native-cdc-support/" target="_blank">este artículo</a>.

#### Actualizando las configuraciones del servidor de origen PostgreSQL

Para la replicación continua con **AWS DMS** desde la base de datos PostgreSQL, es necesario actualizar los siguientes parámetros en el archivo de configuración postgresql.conf.

- Defina wal_level = logical

- Defina max_replication_slots a un valor superior a 1
  - El valor de max_replication_slots debe establecerse en función del número de tareas que desee ejecutar. Por ejemplo, para ejecutar cinco tareas es necesario establecer un mínimo de cinco slots. Los slots se abren automáticamente en cuanto se inicia una tarea y permanecen abiertas incluso cuando la tarea ya no se está ejecutando. Es necesario eliminar manualmente esos slots.

- Defina max_wal_senders a un valr superior a 1
  - El parametro max_wal_senders establece el número de tareas concurrentes que pueden ejecutarse.

- Defina wal_sender_timeout = 0
  - El parámetro wal_sender_timeout termina las conexiones de replicación que están inactivas por más tiempo que el número de milisegundos especificado. Aunque el valor predeterminado es de 60 segundos, se recomienda establecer este parámetro en cero, lo que desactiva el mecanismo de tiempo de espera.

--------------

Puede revisar los ajustes de configuración de PostgreSQL con el siguiente comando después de haber iniciado sesión en el Servidor DB.

1. Obtenga la información del servidor de base de datos de la aplicación Java. Puede obtener esta información en <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank">CloudFormation</a>, en la sección de salida. Para su entorno de **auto-aprendizaje** utilice los parámetros de la sección **Output** del stack **App2ContainerWorkshop**

    | Parameter              | Value                                               |
    | ---------------------- | --------------------------------------------------- |
    | Public DNS Name        | JavaDBServerDNSName                                    |
    | Username   | ubuntu                                |
    | SSHKey  | SSHKeyURL      (You need to create .pem file from this key)                          |

{{% notice note %}}
Para la clave SSH, necesita crear un archivo .pem copiando la clave desde la URL a su editor de texto y guardarlo como archivo .pem.
{{% /notice %}}  

2. Conéctese al servidor **Source Java DB Server** donde se ejecuta su base de datos de origen.

    Verifique <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html" target="_blank">este artículo</a> para obtener mas detalles de como conectarse a una instancia Linux.

3. Ejecute el siguiente comando para revisar el archivo de configuración de postgresql.

    ```bash
    cat /etc/postgresql/10/main/postgresql.conf
    ```
