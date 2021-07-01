+++
title = "Instalar e inicializar App2Container"
weight = 40
pre = "<b>2.2 </b>"
+++

#### 1. Instalar app2container en la máquina de trabajo

1. Conéctese a su máquina de trabajo como se explica en la sección [Conectando sus Instancias Windows]({{< ref "/connect-windows.es.md" >}}).
    - Puede encontrar la máquina de trabajo navegando al servicio EC2 como se muestra a continuación. nombre: "Worker-Machine"
   ![6_db](/db-mig/net-worker-machine.png)

2. Abra un navegador web y descargue el instalador de AWSApp2Container Windows en su servidor desde la siguiente ubicación : <https://app2container-release-us-east-1.s3.us-east-1.amazonaws.com/latest/windows/AWSApp2Container-installer-windows.zip>

3. El archivo de instalación se descargará en la carpeta "Downloads". Vaya a la carpeta "Downloads" y extraiga el archivo "AWSApp2Container-installer-windows.zip".
4. Ejecute el script powershell "install.ps1" e introduzca "R" cuando se lo pida la salida del comando. A continuación, pulse "y" para aceptar los términos y condiciones

    ```powershell
    ./install.ps1
    ```

    ![create-user](/setting-up-app2container/net-install-a2c.png)

5. (opcional) Familiarícese con los comandos disponibles para App2Container

    - Ejecute el siguiente comando para listar los comandos disponibles para App2Container

    ```powershell
    app2container help
    ```

#### 2. Initializar App2Container

El comando init realiza tareas de inicialización para App2Container. Este comando interactivo solicita la información necesaria para configurar el entorno local de App2Container.

1. En primer lugar, tendrá que crear un bucket de Amazon S3 donde App2Container pueda almacenar los artefactos durante el proceso de contenerización.

{{% notice note %}}
Si ya has creado un bucket de S3 en la sección "Modernice su aplicación Java", puedes reutilizar el mismo bucket y puedes saltarte este paso.
{{% /notice %}}

- Navegue a la consola de S3 e introduzca un nombre único para su bucket (app2container-\<Sus-Iniciales\>-\<fecha\>) y haga clic en Create y guarde el nombre del bucket en un bloc de notas (esto será necesario en los pasos posteriores)

    ![create-s3-bucket](/setting-up-app2container/s3-create-bucket.png)

2. En la máquina de trabajado inicialice app2container con el comando "app2container init".

    ```powershell
    app2container init
    ```

  **Se le pide que proporcione la siguiente información:**

| Parameter              | Value             |
| ---------------------- | ---------------- |
| Workspace directory path                | Dejar por defecto. (Un directorio local donde App2Container puede almacenar artefactos durante el proceso de contenerización. El valor predeterminado es C:\Users\Administrator\AppData\Local\app2container.)|
| AWS profile               | Dejar por defecto. (Contiene la información necesaria para ejecutar App2Container, como sus claves de acceso a AWS).          |
| Amazon S3 bucket                | Introduce el nombre del bucket de Amazon S3 (app2container-Sus-Iniciales-Fecha) que has creado en el paso anterior. (por ejemplo, sd-jan-29)          |
| Permission to collect usage metrics              | Dejar por defecto. (esta opción es para permitir que App2Container recopile información sobre el sistema operativo del host, el tipo de aplicación y los comandos de app2container que se ejecutan).          |
| Whether to enforce signed images               | Dejar por defecto. (Puede requerir opcionalmente que las imágenes se firmen utilizando Docket Content Trust (DCT))          |

{{% notice note %}}
Asegúrese de introducir el nombre del cubo de Amazon S3 (app2container-Sus-Iniciales-Fecha) que ha creado en el paso anterior una vez que se le solicite. Vea el ejemplo de salida a continuación.
{{% /notice %}}

![create-user](/setting-up-app2container/net-a2c-init.png)

#### 3. Configurar la conexión remota

Es necesario ejecutar el comando "remote configure" desde la máquina de trabajo para configurar las conexiones necesarias para ejecutar los flujos de trabajo remotos en los servidores de aplicaciones. Este comando interactivo solicita la información necesaria para cada servidor de aplicaciones que se introduzca.

| Server IP Address              | Value             |
| ---------------------- | ---------------- |
| Server IP Address                | Dirección IP privada del servidor de origen (Source-NET-Webserver). (Puede encontrar desde EC2>Source-NET-Webserver> Details> Private IPv4  addresses)           |
| Server FQDN                | Dejar en blanco          |
| Secret ARN                | ARN para el secreto que has creado en AWS Secrets Manager anteriormente          |
| Continue to configure another server ?                | n          |

{{% notice note %}}
Copie la dirección IP privada del servidor web en su bloc de notas, la utilizará en las siguientes secciones.
{{% /notice %}}

```powershell
app2container remote configure
```

  ![create-user](/setting-up-app2container/net-remote-configure.png)

{{% notice note %}}
Puede configurar varios servidores en este paso para proyectos a gran escala.
{{% /notice %}}

¡Felicidades! Ahora ha instalado e inicializado el App2container en su máquina de trabajo y está listo para comenzar a contenerizar su aplicación .NET mediante la ejecución remota del proceso app2container sin tocar directamente su servidor de aplicaciones de origen.
