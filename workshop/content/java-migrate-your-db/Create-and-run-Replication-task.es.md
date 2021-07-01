+++
title = "Crear y ejecutar la tarea de replicación"
weight = 40
pre = "<b>1.5 </b>"
+++

### Configure y ejecute una tarea de replicación

En la consola de **AWS DMS**, vaya a **Database migration tasks** y haga click en el boton **Create Task**.

1. Introduzca los siguientes valores de parámetros en la pantalla **Create database migration task**:

    | Parameter              | Value                                               |
    | ---------------------- | --------------------------------------------------- |
    | Task identifier        | replication-cdc                                     |
    | Replication instance   | replication-instance                                |
    | Source endpoint        | source-postgre-endpoint                                     |
    | Target endpoint        | unicorn                                     |
    | Migration type         | Migrate existing data and replicate ongoing changes |

    ![Create-task-1](/db-mig/Create-task-1.png)

2. En **Task settings** deje los valores que figuran abajo:

    ![create-task-2](/db-mig/java-create-task-2.png)

3. En **Table mappings** seleccione la opción **Wizard**, pulse el boton **Add new selection rule** y seleccione "Enter a schema".
    - Deje los valores por defecto.

    ![Create-task-3](/db-mig/Create-task-3.png)

4. Desplácese hasta la parte inferior de la pantalla y haga clic en el botón **Create task** para crear la tarea e iniciar la replicación de datos.

Mientras se ejecuta la tarea, puede revisar sus detalles para ver información sobre las tablas y el número de filas que se replican.

![create-task-4](/db-mig/Create-task-5.png)
