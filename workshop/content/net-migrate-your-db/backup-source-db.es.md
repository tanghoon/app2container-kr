+++
title = "Copia DB de origen"
weight = 30
pre = "<b>1.2 </b>"
+++

Vamos a utilizar archivos .bak nativos para hacer copias de seguridad y restaurar bases de datos suele ser la forma más rápida de hacerlo.

En esta sección, usted hará una copia de seguridad de su base de datos de origen y la subirá al bucket S3 que ha creado anteriormente.

1. Conéctese a su servidor Source-NET-DBServer como se explica en la sección [Conectando sus Instancias Windows]({{< ref "/connect-windows.es.md" >}}).
    - Puede encontrar el servidor de BD de origen navegando al servicio EC2 como se muestra a continuación. El nombre del servidor de BD de origen es "Source-NET-DBServer".
   ![6_db](/db-mig/net-db-ec2.png)
{{% notice note %}}
Asegúrese de obtener la contraseña del servidor de bases de datos como se describe en la sección [Conectando sus Instancias Windows]({{< ref "/connect-windows.es.md" >}}).
{{% /notice %}}
2. Una vez que inicie la sesión, en el menú Inicio, haga clic en "Microsoft SQL Server Management Studio"

   ![6_db](/db-mig/net-sql-1.png)
3. En la ventana de conexión, introduzca los siguientes parámetros:

| Parameter           | Value                    |
| ------------------- | ------------------------ |
| Server name                | .    |
| Authentication         | SQL Server Authentication |
| Login      | sa            |
| Password      | password-1            |

   ![6_db](/db-mig/net-sql-2.png)
4. Una vez que esté conectado al motor de la base de datos, haga clic con el botón derecho del ratón en "MySiteDB" bajo Bases de datos y seleccione "Back Up" como se muestra a continuación.
   ![6_db](/db-mig/net-sql-3.png)
5. En la pantalla de configuración de Back Up, asegúrese de que ha seleccionado el tipo de copia de seguridad "Full" y tome nota de la ubicación del archivo de copia de seguridad. Utilizará esta ubicación en el siguiente paso.
   ![6_db](/db-mig/net-sql-4.png)
6. Una vez completada la copia de seguridad, obtendrá la siguiente notificación:
   ![6_db](/db-mig/net-sql-5.png)
7. Ahora, usted tiene el archivo de copia de seguridad, pero para poder subir el archivo de copia de seguridad al bucket S3 tendrá que configurar su servidor de Windows credenciales de AWS.

8. Ejecute un terminal de Windows Powershell desde el menú de inicio y ejecute el comando "aws s3 cp" para copiar el archivo de copia de seguridad en el bucket de S3. Reemplace el nombre de su cubo de S3 en el siguiente comando.

   {{% notice note %}}
   awscli ya se ha instalado en esta máquina y se utilizará el rol de instancia ec2 para acceder al bucket S3. Puedes consultar <a href="https://docs.aws.amazon.com/es_es/cli/latest/userguide/cli-chap-welcome.html" target="_blank">detalles de la CLI de AWS</a> para ver las integraciones de la vida real.
   {{%/notice %}}

   ```powershell
   aws s3 cp “C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\MySiteDB.bak” s3://<<insert name of your s3 bucket>>
   ```

9. Una vez que ejecute el comando, navegue a su bucket S3, debería ver el archivo MySiteDB.bak cargado en el.
   ![6_db](/db-mig/net-sql-6.png)
