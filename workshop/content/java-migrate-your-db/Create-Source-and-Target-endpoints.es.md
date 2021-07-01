+++
title = "Crear endpoints de origen y destino"
weight = 30
pre = "<b>1.4 </b>"
+++




Vuelva en la consola de AWS, a la pantalla del **AWS Database Migration Service**, haz clic en el botón **Endpoints** y **Create endpoint**.

### 1. Creación del enpoint de origen

- Utilice los siguientes parámetros para configurar el endpoint:

    | Parameter           | Value                                          |
    | ------------------- | ---------------------------------------------- |
    | Endpoint type       | Source endpoint                                |
    | Endpoint identifier | source-PostgreSQL-endpoint                                |
    | Source engine       | PostgreSQL                                          |
    | Server name         | 10.0.0.140                                            |
    | Port                | 5432                                           |
    | SSL mode            | none                                           |
    | User name           | unicorn                                 |
    | Password            | awsrocks2021                                   |
    | Database Name            | unicorn                                   |

    ![source-endpoint](/db-mig/source-endpoint-1.png)

    ![source-endpoint](/db-mig/source-endpoint-2-privateip.png)

    Abra la sección **Test endpoint connection (optional)**, luego en la opción **VPC** seleccione **Target VPC** y haga clic en el botón **Run test** para verificar que la configuración de su endpoint es válida.

    ![test-source-endpoint](/db-mig/source-endpoint-3.png)

    La prueba se ejecutará durante un minuto y debería ver un mensaje de **successful** en la columna **Status**. Haga clic en el botón **Create endopint** para crear el mismo.

    En caso de que se produzcan errores, asegúrese de que ha configurado correctamente los parámetros del endpoint y de que la Instancia de Replicación se ha creado con el parámetro **Publicly Accessible** marcado.

### 2. Creación del endpoint de destino

- Repita todos los pasos para crear el punto final de destino con los siguientes valores de parámetros:

    | Parameter           | Value                                                 |
    | ------------------- | ----------------------------------------------------- |
    | Endpoint type       | Target endpoint                                       |
    | Select RDS DB instance | checked                                            |
    | RDS Instance        | Select your RDS instance from the drop-down (if it's not visible enter values manually)          |
    | Endpoint Identifier | unicorn                                       |
    | Target Engine       | PostgreSQL (will be pre-populated)                                                |
    | Server Name         | DNS name of your RDS database (leave the the pre-populated value)                             |
    | Port                | 5432     (will be pre-populated)                                             |
    | SSL mode            | none                                                  |
    | User name           | (leave the pre-populated value)                                                 |
    | Password            | Enter password you used when you creating the RDS database| (awsrocks2021)

![source-endpoint](/db-mig/target-endpoint-1.png)

- Seleccione la casilla "Provide access information manually".
![source-endpoint](/db-mig/target-endpoint-2.png)

- En **Test endpoint connection (optional)** seleccione **TargetVPC** en la opción **VPC** y haga click en el boton **Run test** para verificar el que el endpoint es correcto.

![source-endpoint](/db-mig/target-endpoint-3.png)
    La prueba se ejecutará durante un minuto y debería ver un mensaje de **successful** en la columna **Status**. Haga clic en el botón **Create endopint** para crear el mismo.

{{% notice note %}}
Se pueden realizar pruebas adicionales de conexión de puntos finales desde la lista de **Endpoints** haciendo clic en el botón **Actions** y luego en **Test connection**.
{{% /notice %}}
