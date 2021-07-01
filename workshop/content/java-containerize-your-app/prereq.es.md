+++
title = "Requisitos previos de App2Container"
weight = 20
pre = "<b>2.1 </b>"
+++

AWS App2Container requiere que se completen los siguientes requisitos previos antes de utilizarlo por primera vez.
En este laboratorio, hemos preinstalado todas las herramientas de prerrequisitos y hemos creado el entorno de origen compatible con estos prerrequisitos.

Puede consultar la lista de requisitos previos en [Requisitos previos de App2Container](https://docs.aws.amazon.com/app2container/latest/UserGuide/start-containerize-java-app.html)

Como siguiente paso, siga las siguientes tareas para configurar su servicio App2Container.

#### 1. Conceder permisos a los usuarios de IAM

App2Container necesita acceso a los servicios de AWS para ejecutar la mayoría de sus comandos. Hay dos conjuntos muy diferentes de permisos necesarios para ejecutar los comandos de *app2container*.

- El usuario o grupo de propósito general puede ejecutar todos los comandos excepto los que se ejecutan con la opción --deploy.
- Para el despliegue, App2Container debe ser capaz de crear o actualizar objetos de AWS para los servicios de gestión de contenedores (Amazon ECR con Amazon ECS o Amazon EKS), y de crear canalizaciones CI/CD con AWS CodePipeline. Esto requiere permisos elevados que solo deberían utilizarse para el despliegue.

Le recomendamos que cree recursos IAM de propósito general, y si planea usar App2Container para desplegar sus contenedores o crear pipelines, que cree recursos IAM separados para el despliegue que tenga derechos elevados.
{{% notice note %}}
Para este taller, crearemos un usuario con los derechos de administrador para que pueda desplegar una aplicación en contenedor utilizando los servicios de AWS para el despliegue que son soportados por App2Container.
{{% /notice %}}

1. **Acceda a la consola de IAM haciendo clic en "Services" > "IAM" (Identity Access Management)**.

    a. En el panel izquierdo, haga clic en Users > Add user

    b. Proporcione un nombre de usuario, por ejemplo, app2container

    c. Marque la opción 'programmatic access'

    d. haga click en Next: Permissions

    ![create-iam-user](/setting-up-app2container/create-iam-user.png)

2. Establezca los permisos para su usuario de app2Container.

    a. Seleccione "Attach existing policies directly"

    b. Marque "AdministratorAccess"

    c. Haga clic en "Next: Tags"

    ![set-permissions](/setting-up-app2container/set-permissions.png)

3. Revisar y crear un usuario

    a. Seleccione “Next: Review” y haga clic en "Create user"

    b. En la pantalla siguiente, busque el ID de la clave de acceso y la clave de acceso secreta y cópialos (los usarás en los siguientes pasos).

    ![create-user](/setting-up-app2container/create-user.png)

¡Felicitaciones! Con este paso, has configurado los prerrequisitos de tu entorno y estás listo para pasar a la siguiente sección. En la siguiente sección, instalará App2Container y lo configurará para iniciar su proceso de contenerización.
