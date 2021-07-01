+++
title = "Restaurar BD en RDS"
weight = 30
pre = "<b>1.3 </b>"
+++



1. Navegue hasta el Servicio RDS y seleccione la Base de Datos que ha creado anteriormente.
2. En la pestaña Conectividad y seguridad, copie y anote la URL de su base de datos.
   ![6_db](/db-mig/net-create-db-5.png)
3. En su servidor de base de datos de origen, haga clic en Connect en la esquina superior izquierda de SQL Studio.
    ![6_db](/db-mig/net-restore-1.png)
4. En la ventana de conexión, introduzca los siguientes parámetros:

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Server name                | RDS DB Endpoint URL    |
    | Authentication         | SQL Server Authentication |
    | Login      | unicorn (Si estableció un usuario diferente al crear su base de datos RDS, introdúzcalo aquí en su lugar.)            |
    | Password      | awsrocks2021  (Si ha establecido una contraseña diferente al crear su base de datos RDS, introdúzcala aquí.)          |

    ![6_db](/db-mig/net-restore-2.png)

5. Una vez conectado, verá que su base de datos RDS se hace visible en el panel izquierdo como se indica a continuación.
    ![6_db](/db-mig/net-restore-3.png)
6. Haga clic en Databases en el panel izquierdo y haga clic en "New Query" como se muestra a continuación.
    ![6_db](/db-mig/net-restore-8.png)
7. En la pantalla de consulta, pegue el siguiente comando y haga clic en el botón "!Execute".
    - Sustituye el nombre del bucket S3 por el nombre del bucket que has creado en la sección anterior.

{{% notice note %}}
Si ha definido un nombre de base de datos RDS diferente durante la creación anterior. Introdúzcalo en el siguiente comando. (por ejemplo, "unicornsql")
{{% /notice %}}  


```powershell
exec msdb.dbo.rds_restore_database
    @restore_db_name='unicorn',
    @s3_arn_to_restore_from='arn:aws:s3:::<<insert name of your s3 bucket>>/MySiteDB.bak';
```

Cuando ejecute la consulta, debería ver que se ha creado la tarea RESTORE_DB.
    ![6_db](/db-mig/net-restore-4.png)

8. Abra otra pantalla de consulta y ejecute el siguiente comando para comprobar el estado de la tarea. Debería ver que la tarea RESTORE_DB ha sido exitosa como se muestra a continuación.

- Si ve el estado como "in progress" ejecute la consulta de nuevo en unos minutos.

  ```powershell
  exec msdb.dbo.rds_task_status @db_name='unicorn';
  ```

![6_db](/db-mig/net-restore-5.png)
9. En el panel izquierdo, debería ver que la base de datos de unicorn se ha restaurado a partir del archivo de copia de seguridad que ha creado.
![6_db](/db-mig/net-restore-6.png)

### Felicidades

Ha restaurado con éxito su base de datos de origen en su base de datos RDS de destino. Ahora vaya a la siguiente sección para actualizar las cadenas de conexión de la aplicación para dirigir su tráfico a la base de datos RDS.
