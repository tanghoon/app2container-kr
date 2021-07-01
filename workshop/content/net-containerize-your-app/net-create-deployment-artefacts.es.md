+++
title = "Crear artefactos de despliegue"
weight = 160
pre = "<b>2.5 </b>"
+++

En este paso, App2Container genera los artefactos necesarios para implementar su contenedor de aplicaciones en AWS. App2Container rellena previamente los valores clave en los artefactos basándose en su perfil, el análisis de la aplicación y las mejores prácticas.

Tiene dos opciones para el despliegue en su entorno de gestión de contenedores de destino (Amazon ECR con Amazon ECS o Amazon EKS).

{{% notice note %}}
En este escenario, desplegará su contenedor de aplicaciones en Amazon ECR con Amazon ECS.
{{% /notice %}}  

![app2container-intro](/create-deployment-artifacts/create-deployment-overview.png)

![app2container-intro](/create-deployment-artifacts/net-generate-6.png)

1. Ejecute el comando "app2container generate app-deployment" para crear la plantilla de CloudFormation para su despliegue.

    ```powershell
    app2container generate app-deployment --application-id <net-app-id>
    ```

2. El proceso tardará unos minutos. Debería ver la siguiente salida.

![app2container-intro](/create-deployment-artifacts/net-generate-1.png)

 Con este comando, App2container realiza las siguientes actividades:

1. Crea un Repositorio ECR (Elastic Container Registry) y mueve la imagen del contenedor de la aplicacion al registro.
   - Ve a la consola de AWS y navega hasta el servicio ECS.
     - Haz clic en "Amazon ECR" en el panel izquierdo.
     - Verás que tu contenedor se registra automáticamente en este repositorio ECR.
     ![app2container-intro](/create-deployment-artifacts/net-generate-2.png)
     - Una vez que haga clic en Repository, verá que su imagen de contenedor con la etiqueta "latest" está desplegada.
     ![app2container-intro](/create-deployment-artifacts/net-generate-3.png)
{{% notice note %}}
Puede actualizar esta etiqueta y otros parámetros del contenedor en el archivo "analysis.json".
{{% /notice %}}  
2. Crea una definición de tarea ECS y la registra en ECS.
    - NOTA: Las definiciones de tareas de ECS son necesarias para ejecutar imágenes Docker en ECS. Para más información sobre las definiciones de tareas, puede consultar <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html" target="_blank">Amazon ECS Task Definitions</a>.
    ![app2container-intro](/create-deployment-artifacts/net-generate-4.png)
3. Carga todos los recursos de CloudFormation en el bucket S3 seleccionado.
    - NOTA: Ha definido este bucket anteriormente en la sección [Instalar e inicializar App2container]({{< ref "/app2container-steps.es.md" >}}).
    - Vaya al servicio S3. Y navega a tu bucket definido inicialmente.
    - Verás que app2container ha creado un directorio separado para todas tus aplicaciones y ha puesto todos los artefactos de despliegue en esos directorios.
    ![app2container-intro](/create-deployment-artifacts/net-generate-7.png)
4. Crea una plantilla maestra de CloudFormation.
    - App2Container revisa su entorno y rellena previamente la plantilla de Cloudformation basándose en las configuraciones deseadas que ha definido en los pasos anteriores, el análisis de la aplicación y las mejores prácticas.
    - Esta plantilla de CloudFormation describe todos los recursos necesarios y sus dependencias para que pueda lanzarlos y configurarlos juntos como un stack.
    - En la máquina de trabajo, localice la ubicación de la carpeta EcsDeployment en su carpeta de aplicaciones como se muestra a continuación y abra el archivo.
    ![app2container-intro](/create-deployment-artifacts/net-generate-5.png)
    - Localice la ubicación del archivo ecs-master.yml en la carpeta EcsDeployment como se muestra a continuación y abra el archivo.
![app2container-intro](/create-deployment-artifacts/net-generate-6.png)
   {{% notice note %}}
En este taller no es necesario hacer ningún cambio en este archivo. Pero también puede actualizar este archivo en función de sus necesidades.
  {{% /notice %}}  

Felicidades, ya está todo listo para desplegar su aplicación .NET en AWS ECS. Continúe con la siguiente sección para activar el despliegue.
