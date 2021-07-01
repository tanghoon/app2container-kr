+++
title = "Crear DB de destino"
weight = 15
pre = "<b>1.1 </b>"
+++


En este caso, utilizará el enfoque de migración "Native Backup Restore" para su servidor SQL de origen.

Como requisito previo, se requiere una conectividad de red entre los entornos de origen y destino.
Con el fin de replicar los escenarios de la vida real, en este taller, construimos VPC peering para usted entre los entornos de origen y destino.

{{% notice note %}}
En este taller se crean los grupos de seguridad necesarios para su entorno.
{{% /notice %}}
![Replication Instance Architecture](/db-mig/vpc-peering.png)

Para configurar una copia de seguridad y restauración nativa, necesitas tres componentes:

1. Un bucket de Amazon S3 para almacenar los archivos de copia de seguridad.
2. Un rol de AWS Identity and Access Management (IAM) para acceder al bucket.
3. La opción SQLSERVER_BACKUP_RESTORE añadida a un grupo de opciones en su instancia de base de datos.

### 1. Crear un bucket de S3 para almacenar sus archivos de copia de seguridad

- Navegue a la consola de S3 e introduzca un nombre único para su bucket (rds-backup-sus iniciales-fecha) y haga clic en crear y guarde el nombre del bucket en un bloc de notas (esto será necesario en los pasos posteriores)

 ![1](/db-mig/net-create-s3-rds.png)

### 2. Crear grupo de opciones para la base de datos de destino

Para habilitar la copia de seguridad y la restauración nativas en su instancia de base de datos, debe añadir la opción SQLSERVER_BACKUP_RESTORE a un grupo de opciones de su instancia de base de datos. Para obtener más información e instrucciones, consulte <a href="https://docs.aws.amazon.com/es_es/AmazonRDS/latest/UserGuide/Appendix.SQLServer.Options.BackupRestore.html" target="_blank">Compatibilidad con copia de seguridad y restauración nativas en SQL Server</a>.

1. Navegue hasta la consola RDS
2. Seleccione **Option groups** en el menú de la izquierda y haga clic en **Create Group**
3. Dele un nombre (por ejemplo, unicorndb-sql-option) e ingrese lo mismo en la descripción.
4. En la opción **Engine**, selecciona **sqlserver-ex**.
5. En "Major Engine Version", seleccione "13.00"
6. Haga clic en Crear.
    ![1](/db-mig/net-option-1.png)
7. En Option Groups, seleccione el grupo de opciones creado y haga clic en "Add option", como se muestra a continuación.
    ![1](/db-mig/net-option-2.png)
8. En la pantalla de Add option, en el menú desplegable, seleccione la opción "SQLSERVER_BACKUP_RESTORE".
9. En la sección de IAM roles, seleccione "Create a new role" e ingrese un nombre al rol (por ejemplo, SqlBackupRestoreIamRole)
10. En la sección S3 Destination, selecciona tu bucket S3 (rds-backup--) que has creado en los pasos anteriores en el menú como se muestra a continuación.
11. Haga clic en "Add Option".
    ![1](/db-mig/net-option-3.png)

### Crear el grupo de subredes para la base de datos de destino

{{% notice note %}}
Si ya ha creado un grupo de subred en la sección Java, puede utilizar el mismo grupo de subred.
{{% /notice %}}

1. Vaya a la **AWS Console**, desde **Services** elija **RDS**, seleccione **Subnet groups** en el menú de la izquierda y haga clic en **Create DB Subnet Group**.

2. En el apartado **Create DB subnet group** introduzca la siguiente información

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Name                | database-subnet-group     |
    | Description         | Subnets where RDS will be deployed |
    | VPC      | TargetVPC            |

    En el panel **Add subnets** añada una subred de cada Zona de Disponibilidad (us-west-2a y us-west-2b) con los CIDRs 10.1.101.0/24 y 10.1.201.0/24, y pulse el botón **Create**.

    ![RDS Subnet group creation](/db-mig/net-db-subnet-group.png)

### Crear la base de datos de destino

1. Ahora seleccione **Databases** en el menú de la izquierda y haga clic en **Create database**

2. Desde las **Engine options**, seleccione Microsoft SQL Server y seleccione los detalles de la versión como se indica a continuación:

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Edition                | SQL Server Express Edition     |
    | Version         | SQL Server 2016 13.00.5820.21.v1 |

    ![1](/db-mig/net-create-db-1.png)

3. En la sección **Template** seleccione "Free Tier".

    ![Free tier template selection](/db-mig/db-template.png)

{{% notice note %}}
La elección de la plantilla "Free Tier" limita sus opciones en los siguientes pasos del asistente, para que se mantenga dentro de los límites de AWS Free Tier.
{{% /notice %}}

4. En la sección **Settings**, configure el identificador de la instancia de base de datos (unicorn), el nombre de usuario maestro (unicorn) y la contraseña maestra (awsrocks2021) para su nueva instancia de base de datos.

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | DB Instance Identifier                | unicorn (enter "unicornsql" if you created unicorn DB in Java section already)     |
    | Master Username         | unicorn |
    | Master password         | awsrocks2021 |

    ![3_db](/db-mig/db-create-2.png)

{{% notice note %}}
Asegúrese de anotar el **Master username** y **Master password**, ya que los utilizará más adelante.
{{% /notice %}}

5. Seleccione **db.t2.micro** de la clase de instancia Burstable DB, **General Purpose (SSD)** para el tipo de almacenamiento y desmarque "Enable storage autoscaling" (no necesitamos más de 20 GB de almacenamiento para esta base de datos).

    ![4_db](/db-mig/4_db.png)

{{% notice note %}}
Para las cargas de trabajo de producción, se recomienda habilitar la configuración <a href="https://docs.aws.amazon.com/es_es/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html" target="_blank">Despliegue Multi-AZ</a> para alta disponibilidad
{{% /notice %}}

6. En la sección **Connectivity**:
    - En **Virtual Private Cloud (VPC)**, seleccione **TargetVPC** (se trata de la <a href="https://aws.amazon.com/vpc/" target="_blank">Amazon Virtual Private Cloud</a> que se creó automáticamente para este laboratorio)
        ![6_db](/db-mig/db-create-3.png)
    - En **Additional connectivity configuration -> VPC Security Group**, seleccione **Choose existing** y elija el grupo de seguridad "SQLRDSSG".
        Asegúrese de que el puerto de la base de datos esté configurado en 1433.
    - Tenga en cuenta que el grupo de subred de la base de datos que ha creado anteriormente se seleccionará automáticamente
        ![6_db](/db-mig/net-create-db-2.png)
{{% notice note %}}
Nota: El grupo de seguridad SQLRDSSG ya está creado para usted y permite el tráfico desde su base de datos de origen y su servidor web de origen.
{{% /notice %}}
7. En la sección de configuración adicional, seleccione el grupo de opciones ('unicorndb-sql-option-group') que ha creado anteriormente.
8. Desmarque las casillas de copia de seguridad, Performance Insights y Monitoring como se muestra a continuación.
   ![6_db](/db-mig/net-create-db-3.png)

9. Por último, revise los **Estimated monthly costs** y pulse el botón **Create database**.

   ![8_2_db](/db-mig/8_2_db.png)

10. La creación de la base de datos tardará unos minutos. Puede comprobar su estado en la sección "Databases". Una vez que esté lista, debería ver su estado como "Available" como se muestra a continuación.
    - Haga clic en el nombre de la base de datos como se muestra a continuación.

    ![6_db](/db-mig/net-create-db-4.png)

11. En la pestaña Connectivity & Security, copie y anote la URL del punto final de su base de datos. Utilizará esta URL de punto final para conectar su base de datos en las siguientes secciones.
   ![6_db](/db-mig/net-create-db-5.png)
