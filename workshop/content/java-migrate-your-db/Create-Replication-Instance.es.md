+++
title = "Crear Instancia de replicación"
weight = 20
pre = "<b>1.2 </b>"
+++

### Crear grupo de subred de replicación

Uno de los prerrequisitos para utilizar el **AWS DMS** es haber configurado un **subnet group**, que es un conjunto de subredes que serán utilizadas por la **DMS Replication Instance**.

1. Vaya a **Consola de AWS > Services > Database Migration Service > Subnet groups** y haga click en el botón **Create subnet group**.
2. En la sección **Create replication subnet group** introduzca los siguientes valores de parámetros:

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Name                | dms-subnet-group     |
    | Description         | Target VPC Subnet Group for DMS |
    | VPC                 | TargetVPC   |
    | Add subnets         | select **TargetVPC-private-a-db**, **TargetVPC-private-b-db** |

    ![Replication-instance-networ](/db-mig/java-subnet-group.png)

3. Haga click en el botón **Create subnet group**

### Crear una instancia de replicación de AWS DMS

En este paso creará una instancia de replicación del servicio <a href="https://aws.amazon.com/dms/" target="_blank">AWS Database Migration Service</a> que inicia la conexión entre las bases de datos de origen y de destino, transfiere los datos y almacena en caché cualquier cambio que se produzca en la base de datos de origen durante la carga inicial de datos.

1. En **AWS Console**, vaya a **Services** y **Database Migration Service**.

2. Haga click en **Replication instances** y luego en el botón **Create replication instance**.

    ![Replication-instance-create](/db-mig/Replication-instance-create.png)

3. En la pantalla **Create replication instance** configure una nueva instancia de replicación con los siguientes valores de parámetros:

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Name                | replication-instance     |
    | Description         | DMS replication instance |
    | Instance Class      | dms.t3.medium            |
    | Engine version      | 3.4.3                    |
    |Allocated storage (GB)| 50                      |
    | VPC                 | TargetVPC            |
    | Multi-AZ            | Unchecked                |
    | Publicly accessible | Checked                  |

    Like on the screenshot below.

    ![replication-instance-conf](/db-mig/replication-instance-conf.png)

    En **Advanced security and network configuration**, asegúrese de seleccionar el grupo de subred de replicación, la zona de disponibilidad (us-west-2a) y el grupo de seguridad de la instancia de replicación que creó anteriormente.

    ![Replication-instance-conf](/db-mig/advanced-security.png)

4. Haga click en el botón **Create**.

{{% notice note %}}
Si obtiene un error que dice "SYSTEM ERROR MESSAGE: Cannot create a dms.t2.medium replication instance", cree de nuevo la instancia de replicación DMS pero seleccione **us-west-2b** **Availability Zone**, y si esto no ayuda - seleccione una **instancia mas grande**.
{{% /notice %}}  
