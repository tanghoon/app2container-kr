+++
title = "Ejecutar en su propia cuenta de AWS"
weight = 20
pre = "<b>- </b>"
+++

{{% notice warning %}}
Complete esta seccion SOLO si esta ejecutando este taller por su cuenta. Si usted esta en un evento de AWS (como re:Invent, GameDay, Workshop, o cualquier otro evento provisto por empleados de AWS), seleccione la opcion [Atender un evento AWS]({{< ref "/hosted.md" >}}).
{{% /notice %}}

### Entorno de autoaprendizaje

Este laboratorio asume que usted tiene acceso a una **cuenta AWS** con <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html" target="_blank">privilegios de Administrador</a>. Para crear una nueva cuenta de AWS por favor siga el articulo <a href="https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/" target="_blank">¿Como crear y activar una cuenta nueva en Amazon Web Services?</a>.

!Recuerde [limpiar]({{< ref "/cleanup/_index.es.md" >}}) su cuenta de AWS luego de ejecutar el laboratorio, para evitar cualquier consumo innecesario!

   {{% notice note %}}
Este taller fue diseñado para ejecutarse en la region us-west-2. Por favor asegurese de seleccionar esa region.
  {{% /notice %}}  

#### Deployment

1. Haga clic en el botón de abajo <a href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=App2ContainerWorkshop&templateURL=https://app2container-workshop-artifacts.s3-us-west-2.amazonaws.com/app2container_workshop_template.yml" target="_blank"><img src="https://application-migration-with-aws-workshop.s3-us-west-2.amazonaws.com/static/cloudformation-launch-stack.png"></a>

2. En **Step 1 - Specify template** confirme que la URL <https://app2container-workshop-artifacts.s3-us-west-2.amazonaws.com/app2container_workshop_template.yml> es introducida en el campo **Amazon S3 URL** y pulse **Next**
  ![CloudFormation Step 1](/intro/cloudformation-step1.en.png)

3. En **Step 2 - Specify stack details** asegurese de que el nombre App2ContainerWorkshop este ingresado en el campo **Stack name** y pulse **Next**
  {{% notice note %}}
  Puedes añadir tu dirección IP pública en la sección "YourIPAddress" para limitar el acceso sólo a tu dirección IP. (por ejemplo, x.x.x.x/32). El valor por defecto (0.0.0.0/0) permitirá el tráfico de todas las IPs.
  {{% /notice %}}  
  ![CloudFormation Step 2](/intro/cloudformation-step2.en.png)

4. En **Step 3 - Configure stack options** no haga ningun cambio, sólo pulse **Next**

5. En **Step 4 - Review** desplácese hasta la parte inferior de la página y marque todas las casillas de verificación, como en la captura de pantalla siguiente, y luego pulse **Create task** para que se despliegue la plantilla.
  ![CloudFormation Step 4](/intro/cloudformation-step4.en.png)

{{% notice note %}}
Si el despliegue falla, vaya a la pestaña **Events** de la pila de despliegue y confirme la causa raíz. Una causa común es que los roles IAM **ecsExecutionRole** o **ecsAutoscaleRole** ya existen en la cuenta de AWS. Elimínelos y vuelva a ejecutar el despliegue de la plantilla CloudFormation del entorno de origen.
{{% /notice %}}

Cuando la plantilla está en el estado **CREATE_COMPLETE** puedes encontrar información sobre el entorno de origen creado yendo a **AWS Console -> CloudFormation**, seleccionando el stack **App2ContainerWorkshop** y yendo a la pestaña **Outputs**. Verá información como en la captura de pantalla de abajo.

![Source Environment Information in AWS Console](/intro/self-service-env-awsconsole-info.en.png)

- Copie - pegue esta información en su bloc de notas local, ya que la utilizará durante el taller.

### Muy bien. Ya está todo listo para empezar.

Ahora, elige tu aplicación (Java o .NET) y empieza a modernizar