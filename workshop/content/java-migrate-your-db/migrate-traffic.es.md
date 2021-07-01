+++
title = "Cambiar la conexión en la App"
weight = 60
pre = "<b>1.6 </b>"
+++

En esta etapa, ha creado con éxito una nueva base de datos PostGreSQL en Amazon RDS y ha migrado su base de datos existente y establecido su configuración de replicación en curso.

Ahora, cambiará los ajustes de configuración del endpoint de la base de datos en su servidor de aplicaciones Java de origen para que su aplicación comience a utilizar la nueva base de datos migrada.

1. Vaya al Servicio RDS y haga clic en "Databases" en el panel.
    - Seleccione su base de datos RDS "unicornio" que ha creado anteriormente.
    - En la pestaña "Conectivity & security", copie la URL del endpoint en su bloc de notas. Utilizará este endpoint en los próximos pasos.

    ![source-endpoint](/db-mig/java-rds-endpoint.png)

2. Conéctese al **Servidor-Java-Fuente** donde se ejecuta su aplicación Java.

   Tiene múltiples opciones para conectar su Servidor Web. Verifique <a href="https://docs.aws.amazon.com/es_es/AWSEC2/latest/UserGuide/AccessingInstances.html" target="_blank">este artículo</a> para más detalles sobre cómo conectarse a su instancia Linux.

    - Puede conectarse a su servidor web mediante el uso de AWS Systems Manager - <a href="https://docs.aws.amazon.com/es_es/systems-manager/latest/userguide/session-manager.html" target="_blank">Session Manager</a>. Session Manager proporciona una gestión de instancias segura y auditable sin necesidad de abrir puertos de entrada, mantener hosts de bastión o gestionar claves SSH.
       
        - Vaya a sus instancias de Amazon EC2, seleccione su servidor web Java y haga clic en "Connect".
        - Seleccione "Session Manager" y haga clic en "Connect"
       
          ![create-s3-bucket](/setting-up-app2container/session-manager.png)

    **O**

    - Si prefieres conectarse por SSH al servidor web, necesitarás los siguientes datos.

        a. Public DNS Name of the webserver <br>
        b. Username configured in the webserver <br>
        c. SSHKey <br>

        Puede encontrar esta información en la sección de salida del stack de cloudformation. Se puede acceder al stack navegando a la sección <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank">CloudFormation</a> de la consola.

        | Parameter              | Value                                               |
        | ---------------------- | --------------------------------------------------- |
        | Username   | ubuntu                                |
        | SSHKey  | SSHKeyURL      (You need to create .pem file from this key)                          |
        | Public DNS Name        | JavaWebServerDNSName                                    |

  {{% notice note %}}
  Para la clave SSH, necesita crear un archivo .pem copiando la clave desde la URL a su editor de texto y guardarlo como archivo .pem.
  {{% /notice %}}  

3. Su aplicación obtiene las configuraciones de conexión a la base de datos de las variables de entorno de su servidor. Abra el archivo de entorno en la carpeta /etc y actualice el "RDS_HOSTNAME" con la URL del endpoint RDS que ha copiado anteriormente.

    ```bash
    sudo su
    nano /etc/environment
    ```

    - Una vez que abra el archivo, verá que RDS_HOSTNAME ya tiene definido el valor en "10.0.0.140" que es la dirección IP privada de la BD de origen. Cambie el parámetro con RDS endpoint, guarde y salga del archivo.
    ![source-endpoint](/db-mig/java-env-var.png)

    - Después de los cambios, salga del archivo con "ctrl+x" y escriba "y" y pulse enter para guardar el archivo con el mismo nombre.

4. Para activar los cambios en las variables de entorno, ejecute el siguiente comando.

    ```bash
    source /etc/environment
    ```

5. Ahora, reinicie su servidor Tomcat para activar los cambios en su aplicación web con los siguientes comandos.

    ```bash
    cd /usr/local/tomcat8/bin/
    ./shutdown.sh
    ./startup.sh
    ```

    - debería ver la siguiente salida en su terminal.

    ![source-endpoint](/db-mig/java-start-tomcat.png)

6. Vaya a <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank">App2Container Cloudformation Stack</a>, y copie el valor del parametro "JavaWebApplicationURL" que lo encuentra en la sección **Output** del stack **App2ContainerWorkshop**

7. Abra su navegador y pegue "JavaWebApplicationURL".

    - Debería ver que su aplicación web se ejecuta con éxito como se indica a continuación.

        ![source-endpoint](/db-mig/java-app-url.png)

    - Navega a la pestaña "BROWSE MOVIES", deberías ver la lista de películas de unicornios.

        ![source-endpoint](/db-mig/java-url-movies.png)

#### ¡ Felicidades !

Ha migrado con éxito su base de datos y su aplicación web está ahora utilizando la base de datos RDS migrada.