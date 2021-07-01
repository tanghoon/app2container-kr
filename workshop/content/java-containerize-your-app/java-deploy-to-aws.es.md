+++
title = "Implementación en AWS"
weight = 80
pre = "<b>2.6 </b>"
+++

![app2container-intro](/deploy-to-aws/Deploy-to-AWS-overview.png)

En esta etapa:

- La imagen de su contenedor de aplicación ya está presente en ECR (Elastic Container Registry) y la definición de ECS Task está creada y registrada.
- App2container ha creado la plantilla Master Cloudformation para que usted pueda desplegar y configurar automáticamente todos los recursos necesarios en AWS.

En esta sección, lanzará su plantilla Cloudformation y revisará el proceso de despliegue en la consola de AWS.

1. En su servidor web, ejecute el comando sugerido en la salida de "app2container generate app-deployment". Actualice el \<java-app-id> con su ID.

    ```bash
    aws cloudformation deploy --template-file /root/app2container/<java-app-id>/EcsDeployment/ecs-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<java-app-id>-ECS
    ```

    - Al ejecutar este comando, se creará un stack de Cloudformation denominada "a2c-<java-app-id>-ECS".

    {{% notice note %}}
    El proceso durará unos minutos. Vuelva a la consola de AWS para revisar las actividades en segundo plano durante el despliegue.
    {{% /notice %}}

    ```bash
    root@ip-10-0-0-202:/home/ubuntu# aws cloudformation deploy --template-file /root/app2container/<java-app-id>/EcsDeployment/ecs-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<java-app-id>-ECS

    Waiting for changeset to be created..
    Waiting for stack create/update to complete
    ...
    ```

2. Navegue hacia la <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks" target="_blank">consola de Cloudformation</a>
    - Verá que se ha creado un stack con el nombre "a2c-java-tomcat-21fb1eb0-ECS".
    - Observe que los stacks anidados son iniciados por el stack principal para cada despliegue. Por ejemplo, un stack separado para el ALB (Application LoadBalancer) y los clusters ECS.
    - Puede ver los eventos desencadenados en la pestaña "Events" para cada stack.
    ![deploy-cloudformation-1](/deploy-to-aws/deploy-cloudformation-1.png)

3. Una vez completado el stack de ALB y el stack del Cluster, navegue hasta el Servicio ECS.
    - Haga clic en "Clusters" en el panel izquierdo.
    - Verá que se está creando un nuevo clúster ECS.
    - Haga clic en el nombre del "Cluster" como se destaca en la pantalla de abajo.
    {{% notice note %}}
    Observe que su contenedor está desplegado en FARGATE. Esto ha sido seleccionado en el archivo deployment.json en los pasos de extracción y containerización. Puedes cambiarlo a "EC2", si prefieres tener el control sobre los servidores que ejecutan los contenedores.
    {{% /notice %}}
        ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-1.png)


    <a href="https://aws.amazon.com/fargate" target="_blank">AWS FARGATE</a> es un motor de computación sin servidor para contenedores. Con FARGATE, no necesita aprovisionar y gestionar servidores y solo paga por los recursos necesarios para ejecutar sus contenedores.


4. Una vez creado el clúster, en la pestaña de Servicios, verá su aplicación desplegada y el estado del servicio es "ACTIVE"
    - Puede ver los valores de "Desired Tasks" y "Running Tasks" y el tipo de lanzamiento es definido por App2container
    ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-2.png)
5. Haga clic en la pestaña Tasks. Aquí puede ver que la Tarea ha sido creada y se está ejecutando.
    - Haga clic en la tarea para ver sus detalles.
    ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-3.png)
6. Haz clic en Clusters y cuando estés dentro de clusters, haz clic en la pestaña Servicios e ingrese en Servicio.
    - Mientras está dentro del Servicio.
    - En la sección de Load Balancing, verá que se ha creado automáticamente un Target Group para su contenedor.
    - Haga clic en el Target Group.
{{% notice note %}}
Observe que también se han definido la VPC, la subred y los grupos de seguridad. Puede actualizarlos en función de los requisitos de su entorno.
{{% /notice %}}
    ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-4.png)
7. Dentro del Target Group, revise la configuración del Target Group
    - <a href="https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html" target="_blank">Target Groups</a> se utilizan para dirigir las solicitudes a uno o más objetivos registrados. App2container crea Targets groups y los registra con sus contenedores y su Loadbalancer.
    - Haga clic en LoadBalancer
    ![deploy-cloudformation-2](/deploy-to-aws/loadbalancer-1.png)
8. En la sección de Loadbalancers, revise las configuraciones del Loadbalancer para su aplicación.
    - Copie el nombre del DNS público y péguelo en su navegador.
    ![deploy-cloudformation-2](/deploy-to-aws/loadbalancer-2.png)

#### Felicitaciones

9. ¡Felicitaciones! Ha puesto su aplicación en contenedores y la ha desplegado en Amazon ECS.
    ![deploy-cloudformation-2](/deploy-to-aws/java-last.png)
10. Vaya a CloudFormation Service y observe que App2container crea varios stacks y que todos los recursos necesarios para su aplicación en contenedor se crean automáticamente para usted.
    ![deploy-cloudformation-2](/deploy-to-aws/cloudformation-ecs-final.png)
11. Vaya a la consola de su servidor web de origen, debería ver la siguiente salida indicando que su stack de aplicaciones se ha creado con éxito.
![deploy-cloudformation-2](/deploy-to-aws/stack-final.png)
