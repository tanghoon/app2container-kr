+++
title = "Descubre y analiza"
weight = 50
pre = "<b>2.3 </b>"
+++

Su proceso de modernización comienza con el descubrimiento de las aplicaciones Java que se ejecutan en su sistema, la creación de un inventario y el posterior análisis del mismo.

![app2container-discover](/discover-and-analyze/discover-overview-diagram.png)

#### 1. Crear un inventario de aplicaciones

1. Ejecute el siguiente comando para listar las aplicaciones que se están ejecutando en su servidor

    ```bash
    app2container inventory
    ```

    - La salida es una colección de objetos JSON con un nodo para cada aplicación. Cada objeto incluirá pares clave/valor y comenzará con el Java-app-id como se muestra a continuación.

    ```JSON
    "java-app-id": {
        "processId": pid,
        "cmdline": "/user/bin/java ...",
        "applicationType": "java-apptype"
    }
    ```

2. Su salida será como la que se muestra a continuación. Observe que "java-tomcat-21fb1eb0" es un identificador de aplicación Java, copie su identificador de aplicación en un bloc de notas ya que lo utilizará en los próximos pasos.

    ```JSON
    {
      "java-tomcat-21fb1eb0": {
        "processId": 1033,
        "cmdline": "/usr/bin/java ... -Dcatalina.home=/usr/local/tomcat8 -Djava.io.tmpdir=/usr/local/tomcat8/temp org.apache.catalina.startup.Bootstrap start ",
        "applicationType": "java-tomcat",
        "webApp": "ROOT"
      }
    }
    ```

#### 2. Analice su aplicación

1. Utilice el ID de la aplicación de la salida JSON del comando de inventario y luego ejecute el comando "app2container analyze --application-id \<java-app-id\>" después de reemplazar \<java-app-id\> con el ID de la aplicación que ha anotado en el paso anterior.

    ```bash
    app2container analyze --application-id <java-app-id>
    ```

2. La salida del comando "analyze" proporciona información sobre las tareas completadas por app2container y recomendaciones para los próximos pasos.

    ```bash
    root@ip-10-0-0-129:/home/ubuntu# app2container analyze --application-id java-tomcat-21fb1eb0
    ✔ Created artifacts folder /root/app2container/java-tomcat-21fb1eb0
    ✔ Generated analysis data in /root/app2container/java-tomcat-21fb1eb0/analysis.json
    👍 Analysis successful for application java-tomcat-21fb1eb0

    💡 Next Steps:
    1. View the application analysis file at /root/app2container/java-tomcat-21fb1eb0/analysis.json.
    2. Edit the application analysis file as needed.
    3. Start the containerization process using this command: app2container containerize --application-id java-tomcat-21fb1eb0
    ```

3. Localice el archivo analysis.json de la salida del comando y abra el archivo para revisarlo.

    - (NOTA: Utilice la ubicación de su archivo analysis.json provisto en la salida)

      ```bash
      cat /root/app2container/<java-app-id>/analysis.json
      ```

    - Una vez que abra el archivo, verá que el archivo analysis.json tiene 2 secciones: EDITABLE y NO EDITABLE.

      - La sección EDITABLE incluye la sección containerParameters donde puede especificar sus ajustes preferidos como ContainerBaseImage o Imagetag. Estos parámetros se utilizarán durante el proceso de contenerización más adelante. En este ejercicio se utiliza Ubuntu:18.04 para containerBaseImage.
        ![java-analysis](/discover-and-analyze/java-analysis-1.png)

      - La sección NO EDITABLE incluye información de análisis a nivel de aplicación que app2container utilizará durante la contenerización, como datos del sistema operativo, puertos en uso, dependencias, bibliotecas de software, etc.

{{% notice note %}}
Este análisis es especialmente importante cuando se trabaja con aplicaciones heredadas o cuando no hay suficiente documentación sobre la aplicación.
{{% /notice %}}

  ![java-analysis](/discover-and-analyze/java-analysis-2.png)

En este taller, usted no cambiará ningún parámetro EDITABLE, pero en escenarios de la vida real puede actualizar esta sección en base a sus necesidades, por ejemplo, cambiando el containerBaseImage, o habilitando DynamicLogging.

Con este paso, usted completa el análisis y puede continuar con la contenerización de su aplicación en la siguiente sección.
