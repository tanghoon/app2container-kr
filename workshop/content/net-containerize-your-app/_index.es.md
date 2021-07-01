+++
title = "Containerice su aplicación .NET"
weight = 130
pre = "<b>- 2. </b>"
+++

#### Descripción general de AWS App2Container

<a href="https://aws.amazon.com/app2container" target="_blank">App2Container (A2C)</a> es una herramienta de línea de comandos que le ayuda a migrar las aplicaciones que se ejecutan en sus centros de datos locales o en máquinas virtuales, para que se ejecuten en contenedores administrados por Amazon ECS o Amazon EKS.

Migrar aplicaciones heredadas a contenedores suele ser el punto de partida hacia la modernización de las aplicaciones. La utilización de contenedores tiene muchas ventajas:

- Reduce los gastos operativos y de infraestructura.
- Aumenta la agilidad de desarrollo y despliegue.
- Estandariza los procesos de creación y despliegue en toda la organización.

![app2container-intro](/app2container/app2container-diagram.png)

#### Cómo funciona AWS App2Container (Worker Machine)

Puede utilizar App2Container para generar imágenes de contenedores para una o más aplicaciones que se ejecuten en servidores Windows o Linux que sean compatibles con Open Containers Initiative (OCI). Esto incluye las aplicaciones comerciales disponibles (COT). App2Container no necesita el código fuente de la aplicación para contenerla.

Puede utilizar App2Container directamente en los servidores de aplicaciones que están ejecutando sus aplicaciones, o realizar los pasos de contenerización y despliegue en una máquina de trabajo.

En este módulo, usted realizará todos los pasos de contenedorización y despliegue desde una máquina de trabajo.

![app2container-intro](/app2container/app2container-remote-diagram.png)
Cuando se utiliza App2Container en una máquina de trabajado, realiza las siguientes tareas en 4 categorías principales:

1. [Descubre y analiza]({{< ref "/net-containerize-your-app/net-discover-and-analyze.es.md" >}})
    - Construye una comunicación segura con el Servidor de Origen desde la máquina del trabajador a través de WinRM.
    - Analiza el Servidor de Origen remotamente y recoge los archivos de análisis dentro de la máquina de trabajo.
    - Crea una carpeta dentro de la máquina de trabajo para cada Servidor de Origen remoto.
    - Crea una lista de inventario para el servidor de aplicaciones que identifica todos los ASP.NET (Windows) en ejecución que son candidatos a contenerizarse.
    - Analiza las dependencias en tiempo de ejecución de las aplicaciones compatibles que se están ejecutando, incluidos los procesos que cooperan y las dependencias de los puertos de red.
2. [Extraer y contenerizar]({{< ref "/net-containerize-your-app/net-extract-and-containerize.es.md" >}})
    - Extrae los artefactos de la aplicación para su contenerización desde el servidor de origen a la máquina Worker.
    - En la máquina Worker, genera un Dockerfile.
    - En la máquina Worker, inicia la construcción del contenedor de la aplicación.
3. [Crear artefactos de despliegue]({{< ref "/net-containerize-your-app/net-create-deployment-artefacts.es.md" >}})
    - En la maquina worker, genera los artefactos necesarios para desplegar su contenedor de aplicaciones en AWS.
    - Crea una plantilla de AWS CloudFormation para configurar la infraestructura de computación, red y seguridad necesaria para implementar contenedores mediante Amazon ECS o Amazon EKS.
    - Rellena previamente los valores clave en los artefactos basándose en su perfil, el análisis de la aplicación y las prácticas recomendadas.
4. [Implementación en AWS]({{< ref "/net-containerize-your-app/net-deploy-to-aws.es.md" >}})
    - Implementa los contenedores en Amazon ECS o Amazon EKS.
    - Opcionalmente, crea una canalización CI/CD con AWS CodePipeline y los servicios asociados, para automatizar la creación e implementación de los contenedores de la aplicación.

{{% notice note %}}
**AWS App2Container** se ofrece sin cargo adicional. Solo se le cobra cuando utiliza otros servicios de AWS para ejecutar su aplicación en contenedores, como Amazon ECR, Amazon ECS y Amazon EKS.
{{% /notice %}}  

En este laboratorio vas a contenerizar tu aplicación .NET heredada que se ejecuta en Windows 2012 dentro de un contenedor de Windows 2019 utilizando App2Container.

Con el sistema operativo actualizado, lo desplegarás en Amazon ECS (Elastic Container Service).

Siga los siguientes pasos para comenzar a configurar su entorno App2Container.

1. [Requisitos previos de App2Container]({{< ref "/netprereq.es.md" >}})  
2. [Instalar e inicializar App2Container]({{< ref "/netapp2container-steps.es.md" >}})  
