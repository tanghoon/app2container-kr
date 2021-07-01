+++
title = "Extraer y contenerizar"
weight = 60
pre = "<b>2.4 </b>"
+++

En este módulo, transformará su aplicación con app2container. Las actividades de la fase de Extracción y Contenerización dependen de si ejecuta todos los pasos en el servidor de aplicaciones, o si utiliza el servidor de aplicaciones para el análisis y una máquina de trabajo para la contenerización y el despliegue.
![extract-and-containerize-intro](/extract-and-containerize/extract-containerize-overview.png)
En este escenario ([Containerice su aplicación java]({{< ref "/java-containerize-your-app/_index.es.md" >}})), ejecutarás todos los pasos siguientes en el servidor de aplicaciones.

{{% notice note %}}
Separar el entorno de la contenerización utilizando la máquina de trabajo es una buena práctica y le proporciona beneficios de seguridad y funcionales. La opción de la máquina de trabajo se utiliza en el siguiente escenario [Containerice su aplicación .NET]({{< ref "/net-containerize-your-app/_index.es.md" >}}).
{{% /notice %}}  

1. Ejecute los siguientes comandos para iniciar el proceso de contenerización, sustituya \<java-app-id\> por el ID de su aplicación Java.

    ```bash
    app2container containerize --application-id <java-app-id>
    ```

    - El proceso de contenerización tardará unos minutos y App2Container creará el archivo docker y el archivo deployment.json que incluye toda la configuración relacionada con el despliegue de AWS.
    - Una vez que el proceso de contenerización haya finalizado, debería ver la siguiente salida.

    ```bash
    root@ip-10-0-0-129:/home/ubuntu# app2container containerize --application-id java-tomcat-21fb1eb0
    ✔ AWS prerequisite check succeeded
    ✔ Docker prerequisite check succeeded
    ✔ Extracted container artifacts for application
    ✔ Entry file generated
    ✔ Dockerfile generated under /root/app2container/java-tomcat-21fb1eb0/Artifacts
    ✔ Generated dockerfile.update under /root/app2container/java-tomcat-21fb1eb0/Artifacts
    ✔ Generated deployment file at /root/app2container/java-tomcat-21fb1eb0/deployment.json
    👍 Containerization successful. Generated docker image java-tomcat-21fb1eb0

    💡 You're all set to test and deploy your container image.

    Next Steps:
    1. View the container image with "docker images" and test the application.
    2. When you're ready to deploy to AWS, please edit the deployment file as needed at /root/app2container/java-tomcat-21fb1eb0/deployment.json.
    3. Generate deployment artifacts using "app2container generate app-deployment --application-id java-tomcat-21fb1eb0"
    ```

2. Ahora, tienes tu imagen docker lista. Puedes comprobarlo ejecutando el siguiente comando.

    ```bash
    docker images
    ```

    - Como puede ver, la imagen del contenedor fue creada para su aplicación Java con la imagen base del contenedor elegida (ubuntu 18.4) y la etiqueta (última).

    ```bash
    root@ip-10-0-0-129:/home/ubuntu# docker images
    REPOSITORY           TAG     IMAGE ID      CREATED        SIZE
    java-tomcat-21fb1eb0 latest  5e382c3f2e6a  3 minutes ago  715MB
    ubuntu               18.04   2c047404e52d  6 weeks ago    63.3MB
    ```

    Con este paso, has logrado contenerizar tu aplicación. Ahora está lista para ser desplegada en AWS.

3. Al ejecutar el comando "app2container containerize", se crea automáticamente un archivo deployment.json. Este archivo incluye la configuración del despliegue de AWS. Puede actualizar este archivo para personalizar el despliegue en su entorno de AWS. Por ejemplo, puede seleccionar una VPC específica para su entorno de destino o definir la configuración de asignación de cpu/memoria para su contenedor.


- Abra el archivo y revise la configuración. NOTA: Actualice la ubicación de su archivo en el siguiente comando desde la salida del comando.

```bash
nano /root/app2container/<java-app-id>/deployment.json
```

- Una vez que abra el archivo, observará que hay varios ajustes de configuración para cada servicio de AWS. App2Container pre-configura estos ajustes para usted, pero puede actualizarlos para adoptar su entorno.

En este taller, tendrá que comprobar 2 valores.

1. Asegúrese de que "createEcsArtifacts" esté configurado como "true". [x]
    - La sección ecsParameters es para definir ECS (Elastic Container Service) como el entorno de destino. Debes establecer "createEcsArtifacts" en "false" si no quieres desplegar en ECS.
{{% notice note %}}
Observe que "deployTarget" se define como "FARGATE" a continuación. Puedes definirlo como "EC2" si quieres tener el de las instancias ec2 que alojan tus contenedores.
{{% /notice %}}  
    ![java-deployment-1](/extract-and-containerize/java-deployment-1.png)

2. Introduzca su vpc-id de destino para desplegar su aplicación en la red de destino.

    - Vaya a la consola de AWS y navegue hasta <a href="https://us-west-2.console.aws.amazon.com/vpc/home?region=us-west-2#vpcs" target="_blank"> VPC console </a>
    - En sus VPCs, busque "TargetVPC" y copie el vpc-id como se muestra a continuación.
    ![java-deployment-1](/extract-and-containerize/java-deployment-vpc.png)
    - En el archivo deployment.json, busque la parte "reuseResources" y pegue su valor copiado para "vpcId".
    ![java-deployment-1](/extract-and-containerize/java-deployment-2.png)

3. Salga del archivo con "ctrl+x" y escriba "y" y pulse enter para guardar el archivo con el mismo nombre.

{{% notice note %}}
Si no actualiza su VPC-ID en el archivo deployment.json, app2container desplegará su aplicación en la VPC predeterminada.
{{% /notice %}}  

¡Felicitaciones! Ha definido todos los ajustes de configuración de AWS de destino y está listo para desplegar su aplicación.

En la siguiente sección crearás artefactos de despliegue para tu aplicación.