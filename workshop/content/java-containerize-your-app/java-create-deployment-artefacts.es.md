+++
title = "Crear artefactos de despliegue"
weight = 70
pre = "<b>2.5 </b>"
+++

En este paso, App2Container genera los artefactos necesarios para implementar su contenedor de aplicaciones en AWS, basándose en el análisis de la aplicación y en las prácticas recomendadas.

Tiene dos opciones para el despliegue en su entorno de administración de contenedores de destino (Amazon ECR con Amazon ECS o Amazon EKS).

{{% notice note %}}
En este escenario, desplegará su contenedor de aplicaciones en Amazon Elastic Container Register (Amazon ECR) con Amazon ECS.
{{% /notice %}}  

![app2container-intro](/create-deployment-artifacts/create-deployment-overview.png)

1. Ejecute el siguiente comando para crear la plantilla de AWS CloudFormation

    ```bash
    app2container generate app-deployment --application-id <java-app-id>
    ```

2. El proceso tardará unos minutos. Debería ver la siguiente salida.

    ```bash
    root@ip-10-0-0-129:/home/ubuntu# app2container generate app-deployment --application-id java-tomcat-21fb1eb0
    ✔ AWS prerequisite check succeeded
    ✔ Docker prerequisite check succeeded
    ✔ Created ECR Repository
    ✔ Registered ECS Task Definition with ECS
    ✔ Uploaded CloudFormation resources to S3 Bucket: app2container-bucket-0121
    ✔ Generated CloudFormation Master template at: /root/app2container/java-tomcat-6c144e52/EcsDeployment/ecs-master.yml
    👍 CloudFormation templates and additional deployment artifacts generated successfully for application java-tomcat-21fb1eb0

    💡 You're all set to use AWS CloudFormation to manage your application stack.
    ```

Con este comando, App2container realiza los siguientes pasos:

1. Crea un repositorio de Amazon ECR y copia allí la imagen del contenedor de aplicaciones que has creado.
   - Compruébelo iniciando sesión en la consola de AWS y navegando hasta el servicio Elastic Container Service (ECS).
     - Haga clic en "Amazon ECR" en el panel izquierdo.
     - Verá que la imagen de su contenedor se registra automáticamente en este repositorio ECR.
     ![ecr-1](/create-deployment-artifacts/deployment-ecr.png)
     - Una vez que haga clic en su repositorio, verá que su imagen de contenedor con la etiqueta "latest" está desplegada.
     ![ecr-2](/create-deployment-artifacts/deployment-ecr-2.png)
{{% notice note %}}
Puede actualizar esta etiqueta y otros parámetros del contenedor en el archivo "analysis.json".
{{% /notice %}}  
2. Crea una definición de tarea ECS y la registra en ECS.
    - NOTA: Las definiciones de tareas de ECS son necesarias para ejecutar imágenes Docker en ECS. Para más información sobre las definiciones de tareas, puede consultar <a href="https://docs.aws.amazon.com/es_es/AmazonECS/latest/developerguide/task_definitions.html" target="_blank">Amazon ECS Task Definitions</a>.
    ![ecs-task-definiton](/create-deployment-artifacts/deployment-task-definition.png)
3. Carga todos los recursos de CloudFormation en el bucket S3 seleccionado.
    - NOTA: Ha definido este bucket anteriormente en la sección [Install & Initialize App2container]({{< ref "/app2container-steps.es.md" >}}).
    - Navega a la consola de AWS S3 y haz clic en el bucket creado por usted.
    - Verás que todos los artefactos de despliegue se cargan en este bucket.
        ![ecs-task-definiton](/create-deployment-artifacts/deployment-s3-bucket.png)
4. Crea una plantilla de CloudFormation.
    - App2Container revisa su entorno y actualiza la plantilla de CloudFormation con la configuración definida en los pasos anteriores, los resultados del análisis de la aplicación.
    - Esta plantilla de CloudFormation describe todos los recursos necesarios y sus dependencias para que puedas lanzarlos y configurarlos juntos como un stack.
    - En su servidor web, localice la ubicación del archivo ecs-master.yml desde la salida del comando "app2container generate app-deployment" y abra el archivo.

{{% notice note %}}
No es necesario que hagas ningún cambio en este archivo ahora, pero ten en cuenta que puedes personalizarlo en función de tus necesidades
{{% /notice %}}  

```bash
cat /root/app2container/<java-app-id>/EcsDeployment/ecs-master.yml
```

¡Felicitaciones!, ya está todo listo para desplegar su aplicación Java en AWS ECS. Continúe con la siguiente sección para activar el despliegue.
