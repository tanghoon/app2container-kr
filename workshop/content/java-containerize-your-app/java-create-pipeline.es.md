+++
title = "Cree su Pipeline de CI/CD"
weight = 90
pre = "<b>2.7 </b>"
+++

En los pasos anteriores, ha analizado su aplicación, la ha puesto en contenedores con App2Container y la ha desplegado en Amazon Elastic Container Service (ECS) y Amazon Elastic Container Registry (ECR).

App2Container también proporciona la capacidad de crear una linea de producción CI/CD para su aplicación en contenedor, como en la captura de pantalla de abajo.

![app2container-discover](/create-pipeline/create-pipeline-overview.png)

En los pasos anteriores, al ejecutar el comando "app2container generate app-deployment" en la sección [Crear artefactos de despliegue]({{< ref "java-containerize-your-app/java-create-deployment-artefacts.es.md" >}}), App2Container crea un archivo pipeline.json en la carpeta de su aplicación.

#### 1. Preparar pipeline.json

1. En su servidor web de origen, navegue hasta la carpeta de su aplicación y abra el archivo pipeline.json.
    - Actualice la siguiente ruta con su \<java-app-id\>.

    ```bash
    nano /root/app2container/<java-app-id>/pipeline.json
    ```

    - Puede ver que app2container ha completado algunos atributos (por ejemplo, repositoryName, image).
    - En la sección releaseInfo, puedes definir en qué clúster o servicio deseas desplegar tu contenedor. Además, puedes definirlo como versión "Beta" o "Prod".

    ![app2container-discover](/create-pipeline/pipeline-json-1.png)

    En este escenario, desplegarás su contenedor en el Cluster y Servicio existente que has creado en los pasos anteriores. Por lo tanto, necesitarás insertar tu

    - ClusterName
    - ServiceName
    - enable the beta version. (establecer el valor del atributo en true)

2. Ahora, ve a la consola de AWS para obtener el ClusterName y el ServiceName de tu entorno.

    - Navegue hasta el servicio ECS y copie el clusterName como se muestra a continuación en su bloc de notas.
    ![ecs-cluster-1](/deploy-to-aws/ecs-cluster-1.png)
    - Ahora, haz clic en tu Servicio y desde la parte superior de la pantalla, copia el nombre de tu servicio.
    ![app2container-discover](/create-pipeline/ecs-service-name.png)

3. Ahora tiene su nombre de clúster y nombre de servicio. Ahora ve al archivo pipeline.json y actualiza el archivo con estos datos.

    - Pegue el nombre de su clúster, el nombre del servicio y habilite la versión beta (ponga "true") como se ve a continuación.
    ![app2container-discover](/create-pipeline/pipeline-json-2.png)

4. Salga del archivo con "ctrl+x" y escriba "y" y pulse enter para guardar el archivo con el mismo nombre.

#### 2. Generar Pipeline

Ahora sus configuraciones de canalización están definidas y está listo para generar una canalización CI/CD.

1. En el Servidor Web de Origen, ejecute los siguientes comandos para generar la Plantilla de CloudFormation del Pipeline CI/CD

```bash
app2container generate pipeline --application-id <java-app-id>
```

Con este comando, app2container crea

- Repositorio de CodeCommit
- Genera todos los archivos buildspec
- Generar plantillas de CloudFormation
- Sube los archivos en el repositorio CodeCommit

Y crea una plantilla de CloudFormation para crear una canalización completa de CI/CD en su entorno de AWS.

```bash
root@ip-10-0-0-202:~/app2container/java-tomcat-21fb1eb0# app2container generate pipeline --application-id java-tomcat-21fb1eb0
✔ Created CodeCommit repository
✔ Generated buildspec file(s)
✔ Generated CloudFormation templates
✔ Committed files to CodeCommit repository
👍 Pipeline resource template generation successful for application java-tomcat-21fb1eb0

💡 You're all set to use AWS CloudFormation to manage your pipeline stack.

Next Steps:
1. Edit the CloudFormation template as necessary.
2. Create a pipeline stack using the AWS CLI or the AWS Console. AWS CLI command:  

 aws cloudformation deploy --template-file /root/app2container/java-tomcat-21fb1eb0/Artifacts/Pipeline/CodePipeline/ecs-pipeline-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-java-tomcat-21fb1eb0-ecs-pipeline-stack
```

#### 3. Crear Pipeline

1. En su servidor web, ejecute el comando "aws cloudformation deploy" sugerido desde la salida del comando "app2container generate pipeline".
    - Con este comando se desplegará el stack de CloudFormation que va a crear y configurar todos los recursos de AWS necesarios en su entorno.
    - Su comando sugerido debe ser como el siguiente:

    ```bash
    aws cloudformation deploy --template-file /root/app2container/java-tomcat-21fb1eb0/Artifacts/Pipeline/CodePipeline/ecs-pipeline-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-java-tomcat-21fb1eb0-ecs-pipeline-stack
    ```

    - Cuando se ejecuta este comando, se crea un stack de Cloudformation que termina con "pipeline-stack".

    {{% notice note %}}
    El proceso durará unos minutos. Vuelva a la consola de AWS para revisar las actividades en segundo plano durante el despliegue.
    {{% /notice %}}  

    ```bash
    root@ip-10-0-0-202:~/app2container/java-tomcat-21fb1eb0# aws cloudformation deploy --template-file /root/app2container/<java-app-id>/Artifacts/Pipeline/CodePipeline/ecs-pipeline-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<java-app-id>-ecs-pipeline-stack

    Waiting for changeset to be created..
    Waiting for stack create/update to complete
    ...
    ```

2. En la Consola de AWS, vaya a <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank"> Cloudformation Stack</a> de servicios y haga clic en "Stacks".
    - Verá que se ha creado un stack con el nombre "a2c-java-tomcat-21fb1eb0-ecs-pipeline-stack".
    - Observe que los stacks anidados son iniciados por el stack principal para cada despliegue.
    - Puede ver los eventos desencadenados en la pestaña "Events" de cada stack.
    ![deploy-cloudformation-1](/create-pipeline/pipeline-cf.png)

3. Navegue hasta el servicio CodePipeline en Herramientas para desarrolladores
    - Verá que se está creando su canalización.
    - Haga clic en su canalización
    ![deploy-cloudformation-1](/create-pipeline/codepipeline-1.png)
4. Una vez que haga clic en su canalización, revisará su ciclo de vida y su estado.
    - Observe que su versión beta está en progreso.
    ![deploy-cloudformation-1](/create-pipeline/pipeline-2.png)

5. Navegue hasta ECS Service y seleccione su Cluster.
    - Revise los cambios en su Cluster.
    - Verá que la nueva tarea se está activando como parte de su canalización CI/CD.
{{% notice note %}}
Como su clúster está configurado para tener 1 contenedor deseado, una vez que lance su contenedor, el despliegue anterior será eliminado.
{{% /notice %}}
    ![deploy-cloudformation-1](/create-pipeline/pipeline-ecs-new-service.png)

6. Una vez que el servicio ECS se haya lanzado con éxito, vaya a la consola de su servidor web de origen, debería ver la siguiente salida indicando que su stack de aplicaciones se ha creado con éxito.

```bash
root@ip-10-0-0-202:~/app2container/java-tomcat-21fb1eb0# aws cloudformation deploy --template-file /root/app2container/<java-app-id>/Artifacts/Pipeline/CodePipeline/ecs-pipeline-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<java-app-id>-ecs-pipeline-stack

Waiting for changeset to be created..
Waiting for stack create/update to complete
Successfully created/updated stack - a2c-<java-app-id>-ecs-pipeline-stack
```

#### Felicitaciones

Ha creado una canalización CI/CD y ha lanzado una versión BETA de su contenedor de aplicaciones en su clúster ECS.

----

Además, puede revisar los artefactos de su bucket S3 definido.

1. Vaya a los servicios S3 y navegue hasta su bucket S3 definido

- App2Container Crea un bucket para cada una de sus aplicaciones en su bucket S3
![deploy-cloudformation-1](/create-pipeline/s3-1.png)

- Y puedes ver las subcarpetas separadas para tu ECS y pipeline. Por ejemplo, si decide que EKS sea su objetivo de despliegue, habrá un bucket separado para EKS en esta ubicación.
![deploy-cloudformation-1](/create-pipeline/s3-2.png)

