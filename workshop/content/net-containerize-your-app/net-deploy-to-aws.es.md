+++
title = "Implementación en AWS"
weight = 170
pre = "<b>2.6 </b>"
+++

¡Enhorabuena! Ahora está en el último paso del proceso de App2container.

![app2container-intro](/deploy-to-aws/Deploy-to-AWS-overview.png)

En esta etapa:

- La imagen de su contenedor de aplicación ya se ha trasladado al ECR (Elastic Container Registry) y se ha creado y registrado la definición de ECS Task.
- App2container ha creado la plantilla Master Cloudformation para que usted pueda desplegar y configurar automáticamente todos los recursos necesarios en la nube.

En esta sección, lanzará su plantilla Cloudformation y revisará el proceso de despliegue en la consola de AWS.

1. En su máquina de trabajo, ejecute el comando sugerido en la salida de "app2container generate app-deployment".

    ```powershell
    aws cloudformation deploy --template-file C:\Users\Administrator\AppData\Local\app2container\<net-app-id>\EcsDeployment\ecs-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<net-app-id>-ECS
    ```

    - Cuando ejecute este comando, se creará un stack de Cloudformation denominada "a2c-\<net-app-id>-ECS".

    {{% notice note %}}
    El proceso durará unos minutos. Vuelva a la consola de AWS para revisar las actividades en segundo plano durante el despliegue.
    {{% /notice %}}  

2. En la consola de AWS, vaya a "CloudFormation" desde los servicios y haz clic en "Stacks".
    - Verás que se ha creado una pila con el nombre "a2c-\<net-app-id>-ECS".
    - Observe que los stacks anidados son iniciados por el stack principal para cada despliegue. Por ejemplo, un stack separado para el ALB (Application LoadBalancer) y los clusters ECS.
    - Puede ver los eventos desencadenados en la pestaña "Events" para cada stack.
    ![deploy-cloudformation-1](/deploy-to-aws/net-deploy-1.png)

3. Después de que el stack de ALB se haya completado y ClusterStack se haya activado en CloudFormation, navegue hasta el servicio ECS.
    - Haga clic en "Clusters" en el panel izquierdo.
    - Verá que se está creando un nuevo clúster ECS.
    - Haga clic en el nombre del "Cluster" como se destaca en la pantalla de abajo.
{{% notice note %}}
Tenga en cuenta que su contenedor se va a desplegar en una instancia EC2. Esto ha sido seleccionado en el archivo deployment.json en los pasos de extracción y containerización. Puedes cambiarlo a "FARGATE", si no quieres controlar y gestionar la infraestructura subyacente y centrarte en tus cargas de trabajo en contenedores.
{{% /notice %}}
        ![deploy-cloudformation-1](/deploy-to-aws/net-deploy-2.png)

{{% notice note %}}
<a href="https://aws.amazon.com/fargate" target="_blank">AWS FARGATE</a> es un motor de computación sin servidor para contenedores. Con FARGATE, no necesita aprovisionar y gestionar servidores y solo paga por los recursos necesarios para ejecutar sus contenedores.
{{% /notice %}}

4. Navega hasta el Servicio EC2 y localiza la nueva Instancia que se lanzará automáticamente. Esta instancia va a alojar su contenedor y será gestionada por Elastic Container Service (ECS).
    ![deploy-cloudformation-1](/deploy-to-aws/net-deploy-3.png)
5. Una vez creado el Cluster. En la pestaña de Services, debería ver que su aplicación está desplegada y el estado es "ACTIVE".
    - Usted puede ver las "Desired Tasks" y "Running Tasks" y el tipo de lanzamiento son definidos por App2container.
    - Observe que el tipo de lanzamiento es EC2 para este despliegue.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-4.png)
6. Haga clic en la pestaña Tareas. Aquí puede ver que la tarea se ha creado y está en marcha.
    - Haga clic en el nombre de la tarea para ver sus detalles.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-5.png)
7. Cuando esté dentro de la tarea, revise las configuraciones de la misma.
    - En la sección de Load Balancing, verá que se ha creado automáticamente un Target Group para su contenedor.
    - Haga clic en Target Group.
{{% notice note %}}
Observe que también se han definido la VPC, la subred y los grupos de seguridad. Puede actualizarlos en función de los requisitos de su entorno.
{{% /notice %}}
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-7.png)
8. Dentro de Target Groups, revise la configuración del Target Group
    - <a href="https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html" target="_blank">Target Groups</a> se utilizan para dirigir las solicitudes a uno o más objetivos registrados. App2container crea Target Groups y los registra con sus contenedores y su Loadbalancer.
    - Haga clic en LoadBalancer
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-8.png)
9. Cuando esté en Loadbalancers, revise las configuraciones del Loadbalancer para su aplicación.
    - Copie el nombre del DNS público y péguelo en su navegador.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-9.png)

#### Felicitaciones

10. ¡¡¡Felicitaciones!!! Has puesto tu aplicación en contenedores y la has desplegado en Amazon ECS.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-11.png)
11. Vaya a CloudFormation Service y observe que App2container crea varios stacks y que todos los recursos necesarios para su aplicación en contenedor se crean automáticamente para usted.
    ![deploy-cloudformation-2](/deploy-to-aws/cloudformation-ecs-net-final.png)
12. Vaya a la consola de su máquina de trabajo, debería ver la siguiente salida indicando que su pila de aplicaciones se ha creado con éxito.
![deploy-cloudformation-2](/deploy-to-aws/net-deploy-10.png)
