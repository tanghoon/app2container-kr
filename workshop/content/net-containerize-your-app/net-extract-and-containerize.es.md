+++
title = "Extraer y contenerizar"
weight = 150
pre = "<b>2.4 </b>"
+++

En este módulo, transformarás tu aplicación con app2container. Las actividades de la fase de transformación dependen de si está ejecutando todos los pasos en el servidor de aplicaciones, o está utilizando el servidor de aplicaciones para el análisis y una máquina de trabajo para la contenerización y el despliegue.
![extract-and-containerize-intro](/extract-and-containerize/extract-containerize-overview.png)
En este escenario ([Containerice su aplicación .NET]({{< ref "/net-containerize-your-app/_index.es.md" >}})), ejecutara todos los siguientes pasos en su maquina de trabajo.

{{% notice note %}}
Separar el entorno de la contenedorización utilizando una máquina de trabajo es una buena práctica y le proporciona beneficios funcionales y de seguridad y le permite estandarizar su proceso de contenerización para ayudar a sus esfuerzos de contenerización a mayor escala.
{{% /notice %}}  

#### 1. Extraiga sus artefactos del servidor de origen a la máquina de trabajo

1. Como siguiente paso sugerido desde la salida del comando "app2container analyze", ejecute el comando "app2container remote extract" para recuperar todos los artefactos de la aplicación en la máquina de trabajado. Actualice el ID de la aplicación y la dirección IP de destino en función de su entorno.

```powershell
app2container remote extract --target <private-IP of your source server> --application-id <net-app-id>
```

- El proceso de extracción tomará unos minutos y App2Container creará un archivo zip que incluye todos los artefactos de la aplicación. Después de este punto, no necesita comunicarse con el servidor de aplicaciones de origen ya que app2container tiene ahora todos los artefactos relacionados con la aplicación en la máquina de trabajo.
{{% notice note %}}
En este punto, también puede actualizar los artefactos dentro del archivo zip y continuar la contenerización con el archivo zip actualizado.
{{% /notice %}}  

![extract-and-containerize-intro](/extract-and-containerize/net-extract-1.png)

- Una vez que el proceso de contenerización haya terminado, debería ver la salida que se muestra a continuación.
![extract-and-containerize-intro](/extract-and-containerize/net-extract-2.png)

#### 2. Containerice su aplicación en la máquina de trabajo

1. Sustituya **PrivateIPAddress** por la dirección IP de su servidor web y ejecute "app2container containerize --input-archive" en su máquina Worker.
    - Proporcione la ubicación del archivo zip para su parámetro input-archive. Este comando le permite contenerizar basándose en los archivos extraídos en la máquina de trabajado en lugar de hacer la contenerización en la máquina de origen.

{{% notice note %}}
Puede copiar el comando sugerido desde la salida de la CLI.
{{% /notice %}}

```powershell
app2container containerize --input-archive C:\Users\Administrator\AppData\Local\app2container\remote\<PrivateIPAddress>\<net-app-id>\<net-app-id>.zip
```

El proceso tardará unos minutos, ya que la máquina Worker necesitará descargar la imagen base de Windows 2019 para su contenedor. Una vez finalizado el proceso, debería ver el siguiente resultado.
    ![extract-and-containerize-intro](/extract-and-containerize/net-containerize-1.png)

2. En esta etapa, deberías tener tus imágenes de contenedor listas. Ejecute "docker images" en la máquina de trabajo para comprobar que las imágenes están construidas.

```powershell
docker images
```

- Puede ver que la imagen del contenedor se crea para su aplicación .NET con el containerBaseImage elegido (Windowsservercore-ltsc-2019) y imageTag (latest).
![extract-and-containerize-intro](/extract-and-containerize/net-containerize-2.png)

{{% notice note %}}
Observe que, en este paso, ha actualizado su sistema operativo. Esto puede ser especialmente útil cuando sus aplicaciones se ejecutan en versiones de SO EOL (End Of Life).
{{% /notice %}}  
3. Al ejecutar el comando "App2container containerize", se crea automáticamente un archivo deployment.json dentro de la carpeta de la aplicación (como se muestra a continuación)

- Este archivo incluye las configuraciones de despliegue de AWS.
- Puede actualizar este archivo para adaptar el despliegue a su entorno de AWS.
- Por ejemplo, puede seleccionar una VPC específica para su entorno de destino o definir la configuración de asignación de cpu/memoria para su contenedor.

![extract-and-containerize-intro](/extract-and-containerize/net-containerize-3.png)
4. Abra el archivo y revise las configuraciones.

- Una vez que abra el archivo, notará que hay varios ajustes de configuración para cada servicio de AWS. App2Container ha pre-configurado estos ajustes para usted, pero puede actualizarlos para adoptar su entorno.
  - En este taller, tendrá que comprobar 2 lugares.
![extract-and-containerize-intro](/extract-and-containerize/net-containerize-4.png)

1. Asegúrese de que "createEcsArtifacts" esté establecido en "true". [x]
    - La sección ecsParameters es para la configuración cuando se decide que ECS (Elastic Container Service) es el entorno de destino. Debes establecer "createEcsArtifacts" en "false" si no quieres desplegar en ECS.

2. Introduzca su vpc-id de destino para desplegar su aplicación en la red de destino.

    - Vaya a la consola de AWS y navegue hasta el servicio VPC.
    - En sus VPCs, busque "TargetVPC" y copie el vpc-id como se muestra a continuación.
    ![java-deployment-1](/extract-and-containerize/java-deployment-vpc.png)

    - En el archivo deployment.json, busque la parte "reuseResources" y pegue el valor copiado de "vpcId". Como se muestra en el ejemplo anterior.

{{% notice note %}}
Si no actualiza su VPC-ID en el archivo deployment.json, app2container desplegará su aplicación en la VPC predeterminada.
{{% /notice %}}  

¡Felicidades! Ha definido todos los ajustes de configuración de AWS de destino y está listo para desplegar su aplicación.

En la siguiente sección crearás artefactos de despliegue para tu despliegue.