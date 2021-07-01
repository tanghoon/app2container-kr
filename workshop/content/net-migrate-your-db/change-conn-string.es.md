+++
title = "Cambiar datos de conexión en la aplicación"
weight = 40
pre = "<b>1.4 </b>"
+++


En esta etapa, ha creado con éxito una nueva base de datos Microsoft SQL en Amazon RDS y ha migrado su base de datos existente.

Ahora, cambiará los ajustes de configuración de la base de datos en su servidor de aplicaciones .NET de origen para que su aplicación comience a utilizar la nueva base de datos migrada.

1. Conéctese a su servidor web .NET de origen como se explica en la sección [Conectando sus Instancias Windows]({{< ref "/connect-windows.es.md" >}}).
    - Puede encontrar el Servidor Web Source .NET navegando al servicio EC2 como se muestra a continuación. El nombre del servidor web de Source NET es "Source-NET-WebServer".
   ![6_db](/db-mig/net-web-ec2.png)

2. Su aplicación .NET obtiene las configuraciones de conexión a la base de datos desde el archivo "web.config" ubicado dentro de la carpeta de la aplicación.

    - Navegue a la carpeta de la aplicación en "c:\Unicornshop" y abra el archivo Web.config como se muestra a continuación.

   ![6_db](/db-mig/net-web-config.png)

3. Una vez abierto el archivo, navegue hasta la sección "\<connectionStrings\>".
     - verá que los parámetros de la BD ya están configurados con los parámetros de la BD de origen.
   ![6_db](/db-mig/net-web-config-2.png)

4. Actualice los parámetros con los valores de la base de datos de destino de RDS.

   | Parameter           | Value                    |
   | ------------------- | ------------------------ |
   | server                | Su cadena de conexión de RDS    |
   | database         | unicorn |
   | user id      | unicorn (Si estableció un usuario diferente al crear su base de datos RDS, introdúzcalo aquí en su lugar.)             |
   | password      | awsrocks2021  (Si ha establecido una contraseña diferente al crear su base de datos RDS, introdúzcala aquí.)          |

   - Debería tener una configuración similar a la que se muestra a continuación:

      ![6_db](/db-mig/net-web-config-3.png)

5. Ahora, navegue a Cloudformation y copie NETWebAppURL (como se muestra a continuación) y péguelo en su navegador. Debería ver su aplicación web funcionando con su nueva base de datos migrada.

   ![6_db](/db-mig/net-web-url.png)

#### Felicidades

   ![6_db](/db-mig/net-web-last.png)

Ha migrado con éxito su base de datos y su aplicación web está ahora utilizando la base de datos RDS migrada.
