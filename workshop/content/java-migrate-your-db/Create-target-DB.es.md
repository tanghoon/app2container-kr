+++
title = "Crear DB de destino"
weight = 15
pre = "<b>1.1 </b>"
+++

### Migración de la base de datos

Las migraciones de bases de datos pueden realizarse de varias maneras, y para el propósito de este taller realizaremos una migración de **replicación continua de datos** utilizando <a href="https://aws.amazon.com/dms/" target="_blank">AWS Database Migrations Service (DMS)</a>.

Antes de configurar **AWS DMS**, deberá crear su base de datos de destino en la cuenta de AWS proporcionada. Utilice **AWS Relation Database Service (RDS)** para realizar esta actividad facilitando la configuración, el funcionamiento y el escalado de una base de datos relacional en la nube.

Como requisito previo, se requiere una conectividad de red entre los entornos de origen y destino.
Con el fin de replicar los escenarios de la vida real, en este taller, construimos un peering de VPC para usted entre los entornos de origen y destino.

{{% notice note %}}
Los Grupos de Seguridad requeridos para este entorno ya fueron creados para usted.
{{% /notice %}}
![Replication Instance Architecture](/db-mig/vpc-peering.png)

### Create the subnet group for target database

1. Ingrese a la **consola de AWS**, desde **Services** elija **RDS**, seleccione **Subnet groups** en el menú de la izquierda y haga clic en **Create DB Subnet Group**.

2. En **Create DB subnet group** ingrese la siguiente información

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Name                | database-subnet-group     |
    | Description         | Subnets where RDS will be deployed |
    | VPC      | TargetVPC            |

    En la sección **Add subnets** añada una subred de cada Zona de Disponibilidad (us-west-2a y us-west-2b) con los CIDRs 10.1.101.0/24 y 10.1.201.0/24, y pulse el botón **Create**.

    ![RDS Subnet group creation](/db-mig/net-db-subnet-group.png)

### Crear grupo de parámetros para la base de datos de destino

En PostgreSQL, foreign keys (restricciones de integridad referencial) se implementan mediante triggers. Durante la fase de carga completa, AWS DMS carga cada tabla de una en una. Se recomienda que desactive las restricciones de clave externa durante una carga completa, utilizando uno de los siguientes métodos:

- Desactivar temporalmente todos los triggers de la instancia, y terminar la carga completa.
- Utilice el parámetro **session_replication_role** en PostgreSQL.

En este taller, actualizaremos el parámetro **session_replication_role** a "replica". Para ello, crearemos un nuevo grupo de parámetros para la base de datos de destino y aplicaremos esos parámetros al crear la base de datos RDS de destino.

Puede verificar <a href="https://docs.aws.amazon.com/es_es/dms/latest/userguide/CHAP_Target.PostgreSQL.html" target="_blank">Uso de una base de datos de PostgreSQL como destino para AWS Database Migration Service</a> para mas detalles.

1. Seleccione **Parameter groups** en el menú de la izquierda y haga click en **Create Parameter Group**

2. De **Parameter group family**, seleccione postgres10
3. Ingrese un nombre y una descripción (e.g. unicorn-db-parameter-group) y haga click en **Create**.

    ![1](/db-mig/db-parameter-group.png)

4. De la lista haga click en "unicorn-db-parameter-group" que acaba de crear
5. En el cuadro de búsqueda, busque "session_replication_role" y seleccione el parámetro y edite su valor a "replica"
5. Haga click en **Save changes**.

    ![1](/db-mig/db-parameters-2.png)

### Crear la base de datos de destino

1. Ahora seleccione **Databases** en el menú de la izquierda y haga clic en **Create database**

2. En **Engine options**, seleccione PostgreSQL y la versión PostgreSQL 10.15-R1

    ![1](/db-mig/db-create-1.png)

{{% notice note %}}
Puede confirmar la versión de PostgreSQL de la base de datos de origen escribiendo ; **psql -V**
{{% /notice %}}

En la sección **Template** seleccione "Free Tier".

![Free tier template selection](/db-mig/db-template.png)

{{% notice note %}}
La elección de "Free tier" garantiza que la plantilla limite sus opciones en los siguientes pasos del asistente, para que se mantenga dentro de los límites de la capa gratuita de AWS.
{{% /notice %}}

En la sección **Settings**, configure el identificador de la instancia de Base de datos (unicorn), Master username (unicorn) y Master password (awsrocks2021) para su nueva instancia de base de datos.

![3_db](/db-mig/db-create-2.png)

{{% notice note %}}
Asegúrese de anotar el **Master username** y **Master password**, ya que los utilizará más adelante.
{{% /notice %}}

Seleccione **db.t2.micro** de la clase de instancia de Burstable DB, **General Purpose (SSD)** para el tipo de almacenamiento y desmarque "Enable storage autoscaling" (no necesitamos más de 20 GB de almacenamiento para esta base de datos).
![4_db](/db-mig/4_db.png)

3. Para la opción **Availability & durability**, mantenga seleccionada la opción **Do not create a standby instance**.

    ![5_db](/db-mig/5_db.png)

{{% notice note %}}
Para las cargas de trabajo de producción, recomendamos habilitar las instancias standby <a href="https://docs.aws.amazon.com/es_es/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html" target="_blank">Alta disponibilidad (Multi-AZ) para Amazon RDS</a> para alta disponibilidad.
{{% /notice %}}  

4. En la sección de **Connectivity**:

    - En **Virtual Private Cloud (VPC)**, seleccione **TargetVPC** (es la <a href="https://aws.amazon.com/vpc/" target="_blank">Amazon Virtual Private Cloud</a> que se creó automáticamente para este laboratorio)
    ![6_db](/db-mig/db-create-3.png)
    - En **Additional connectivity configuration -> VPC Security Group**, seleccione **Choose existing** -> **Existing VPC security groups** y seleccione "PGRDSSG" el cual fue creado para usted. Asegúrese de que el puerto de la base de datos está configurado en 5432.
    - Tenga en cuenta que el grupo de subred de la base de datos que ha creado anteriormente se seleccionará automáticamente.

    ![6_db](/db-mig/java-db-create-4.png)

5. Para **Database authentication**, seleccione **Password authentication**.

    ![6_2_db](/db-mig/6_2_db.png)

6. En la sección de configuración adicional, introduzca el nombre de la base de datos inicial ('unicorn') y seleccione el grupo de parámetros de la base de datos ('unicorn-db-parameter-group') que ha creado anteriormente. Desmarque las casillas de **Backup**, **Performance Insights**, **Monitoring**.
    ![6_db](/db-mig/db-create-5.png)

7. Por ultimo, revise **Estimated monthly costs** y seleccione el boton **Create database**.

   ![8_2_db](/db-mig/8_2_db.png)
