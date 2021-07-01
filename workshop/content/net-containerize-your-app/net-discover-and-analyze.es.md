+++
title = "Descubre y analiza"
weight = 140
pre = "<b>2.3 </b>"
+++

El proceso de App2container comienza con el descubrimiento de las aplicaciones .NET que se están ejecutando en su servidor mediante la creación de un inventario de aplicaciones y su posterior análisis.
![app2container-discover](/discover-and-analyze/discover-overview-diagram.png)

#### 1. Crear un inventario de aplicaciones

Como usted está usando una máquina de trabajo para todos sus procesos de App2container, usted usará un proceso "remote". Una vez que ejecute los comandos remotos desde su máquina de trabajo, se conectará con su servidor de origen y ejecutará los comandos en él.

{{% notice note %}}
El uso de una maquina de trabajo con la conectividad remota le permite estandarizar sus actividades de escalado.
{{% /notice %}}

1. Ejecute el comando "app2container remote inventory" de la siguiente manera para listar las aplicaciones .NET que se están ejecutando en su servidor.

   - utilice la dirección IP privada de su "Source Server" con el parámetro "target" que aparece a continuación.

      ```powershell
      app2container remote inventory --target <private-IP of your source server>
      ```

   - Una vez que ejecute el comando, verá la salida "Remote inventory retrieved successfully" como se indica a continuación.

      ![net-analysis](/discover-and-analyze/net-inventory-1.png)

2. App2container crea una carpeta separada para cada servidor "Target" de origen en la máquina de trabajado.

   - Navegue a la carpeta "C:\Users\Administrator\AppData\Local\app2container\remote\" como se indica a continuación.
   - - Debería ver una carpeta con la dirección IP de su "Source Server".

      ![net-analysis](/discover-and-analyze/net-inventory-2.png)

{{% notice note %}}
La carpeta Appdata es una carpeta oculta por defecto. Si no puede ver esta carpeta, seleccione "View" > y marque la casilla "Hidden items". O bien, navegue a la carpeta "C:\Users\Administrator\AppData\" directamente.
{{% /notice %}}

3. Dentro de la carpeta, debería ver el archivo "inventory.json" con el que app2container recuperó el inventario de aplicaciones .NET de su servidor de origen de forma remota.
   ![net-analysis](/discover-and-analyze/net-inventory-3.png)

4. Abra el archivo "inventory.json" con el bloc de notas. El archivo incluye una colección de objetos JSON con una entrada para cada aplicación.

   - Cada objeto de aplicación incluirá pares clave/valor y comenzará con el .NET-app-id como se muestra a continuación (iis-default-web-site-f5342b48512c).
   - Copie el ID de la aplicación en su bloc de notas, ya que lo utilizará en los próximos pasos.
      ![net-analysis](/discover-and-analyze/net-inventory-4.png)

#### 2. Analice su aplicación

1. Localice el ID de la aplicación en la salida JSON del comando de inventario y ejecute el comando "app2container remote analyze --application-id \<net-app-id\> --target \<source-server-private-IP\>" como se indica a continuación, sustituyendo net-app-id por el ID de la aplicación que ha copiado en el paso anterior.

   ```powershell
   app2container remote analyze --application-id <net-app-id> --target <source-server-private-IP>
   ```

2. La salida del comando "Analyze" proporciona las tareas que ha completado app2container y las recomendaciones para los próximos pasos.

   ![net-analysis](/discover-and-analyze/net-analyze-1.png)

3. Con el comando Analyze, App2container crea una carpeta para cada aplicación. Localícela como se indica a continuación e ingrese en la carpeta.

   ![net-analysis](/discover-and-analyze/net-analyze-2.png)

4. Localice el archivo analysis.json dentro de la carpeta y abra el archivo con el bloc de notas para revisarlo.

Una vez que abra el archivo, verá que el archivo analysis.json tiene 2 secciones: EDITABLE y NON-EDITABLE.

   - La sección EDITABLE incluye los Parámetros del contenedor donde puede especificar su configuración preferida como ContainerBaseImage o etiqueta de imagen. Estos parámetros se utilizarán durante el proceso de contenerización más adelante. Tenga en cuenta que, en este ejercicio, la imagen base del contenedor se actualizará de Windows 2012 a Windows 2019.

{{% notice note %}}
La capacidad de actualizar el sistema operativo es uno de los mejores beneficios del proceso de contenerización. Con App2container, este proceso se puede gestionar a escala.
{{% /notice %}}

   ![net-analysis](/discover-and-analyze/net-analyze-3.png)

   - La sección NO EDITABLE incluye información de análisis a nivel de aplicación que app2container utilizará durante la contenerización, como datos del sistema operativo, puertos en uso, dependencias, bibliotecas de software, etc.

{{% notice note %}}
Este análisis es especialmente importante cuando se trabaja con aplicaciones heredadas o cuando no hay suficiente documentación sobre la aplicación.
{{% /notice %}}

Con este paso, has completado el análisis y puedes continuar con la contenerización de tu aplicación en la siguiente sección.
