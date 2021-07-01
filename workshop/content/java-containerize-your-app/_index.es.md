+++
title = "Containerice su aplicación Java"
weight = 40
pre = "<b>- 2. </b>"
+++

#### Descripción general de AWS App2Container

<a href="https://aws.amazon.com/app2container" target="_blank">App2Container (A2C)</a> es una herramienta de línea de comandos que le ayuda a migrar las aplicaciones que se ejecutan en sus centros de datos locales o en máquinas virtuales, para que se ejecuten en contenedores administrados por Amazon ECS o Amazon EKS.

Migrar aplicaciones heredadas a contenedores suele ser el punto de partida hacia la modernización de las aplicaciones. La utilización de contenedores tiene muchas ventajas:

- Reduce los gastos operativos y de infraestructura.
- Aumenta la agilidad de desarrollo y despliegue.
- Estandariza los procesos de creación y despliegue en toda la organización.

#### Cómo funciona AWS App2Container

Puede utilizar App2Container para generar imágenes de contenedores para una o más aplicaciones que se ejecuten en Windows o Linux. Esto incluye aplicaciones comerciales (COTS). App2Container no necesita el código fuente de la aplicación para generar la imagen.

Puede utilizar App2Container directamente en los servidores de aplicaciones que ejecutan sus aplicaciones. Si su servidor no cumple los requisitos para contenerizar su aplicación e implementarla en AWS, o si no desea instalar el motor Docker en el servidor de aplicaciones, puede configurar y utilizar una máquina de trabajo.

![app2container-intro](/app2container/app2container-diagram.png)

App2Container realiza las siguientes tareas en 4 categorías principales:

1. [Descubre y analiza]({{< ref "/java-containerize-your-app/java-discover-and-analyze.es.md" >}})

    - Crea una lista de inventario para el servidor de aplicaciones que identifica todas las aplicaciones ASP.NET (Windows) y Java (Linux) que se están ejecutando y que son candidatas a ser contenedorizadas.
    - Analiza las dependencias en tiempo de ejecución de las aplicaciones compatibles que se están ejecutando, incluidos los procesos que cooperan y las dependencias de los puertos de red.
2. [Extraer y contenerizar]({{< ref "/java-containerize-your-app/java-extract-and-containerize.es.md" >}})
    - Extrae los artefactos de la aplicación para la contenerización y genera un Dockerfile.
    - Inicia la construcción del contenedor de la aplicación.
3. [Crear artefactos de despliegue]({{< ref "/java-containerize-your-app/java-create-deployment-artefacts.es.md" >}})
    - Genera los artefactos necesarios para desplegar su contenedor de aplicaciones en AWS.
    - Crea una plantilla de AWS CloudFormation para configurar la infraestructura informática, de red y de seguridad necesaria para implementar contenedores mediante Amazon ECS o Amazon EKS.
    - Rellena previamente los valores clave de los artefactos basándose en su perfil, el análisis de la aplicación y las mejores prácticas.
4. [Implementación en la nube de AWS]({{< ref "/java-containerize-your-app/java-deploy-to-aws.es.md" >}})
    - Despliega los contenedores en Amazon ECS o Amazon EKS.
    - Opcionalmente crea una canalización CI/CD con AWS CodePipeline y servicios asociados, para automatizar la creación e implementación de sus contenedores de aplicaciones.

{{% notice note %}}
**AWS App2Container** se ofrece sin cargo adicional. Solo se le cobra cuando utiliza otros servicios de AWS para ejecutar su aplicación en contenedores, como Amazon ECR, Amazon ECS y Amazon EKS.
{{% /notice %}}  

En este laboratorio, usted pondrá en contenedores una aplicación Java heredada en Linux Ubuntu utilizando App2Container, y la desplegará en Amazon ECS (Elastic Container Service).

1. [Requisitos previos de App2Container]({{< ref "/prereq.es.md" >}})  
2. [Instalar e inicializar App2Container]({{< ref "/app2container-steps.es.md" >}})  
