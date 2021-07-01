+++
title = "Requisitos previos de App2Container"
weight = 30
pre = "<b>2.1 </b>"
+++

AWS App2Container requiere que se completen los siguientes requisitos previos antes de utilizarlo por primera vez.
En este laboratorio, hemos preinstalado todas las herramientas de prerrequisitos y hemos creado el entorno de origen compatible con estos prerrequisitos.

Puede consultar la lista de requisitos previos en [Requisitos previos de App2Container](https://docs.aws.amazon.com/es_es/app2container/latest/UserGuide/start-containerize-dotnet-app.html)

Como siguiente paso, siga las siguientes tareas para configurar su servicio App2Container.

#### 1. Conceder permisos a los usuarios de IAM

{{% notice note %}}
Si ha realizado el módulo anterior (Modernize your Java App) y ha creado un usuario IAM para app2container, puede saltarse este paso.
{{% /notice %}}
App2Container necesita acceso a los servicios de AWS para ejecutar la mayoría de sus comandos. Hay dos conjuntos muy diferentes de permisos necesarios para ejecutar los comandos de *app2container*.

- El usuario o grupo de propósito general puede ejecutar todos los comandos excepto los que se ejecutan con la opción --deploy.
- Para el despliegue, App2Container debe ser capaz de crear o actualizar objetos de AWS para los servicios de gestión de contenedores (Amazon ECR con Amazon ECS o Amazon EKS), y crear canalizaciones CI/CD con AWS CodePipeline. Esto requiere permisos elevados que solo deberían utilizarse para el despliegue.

Le recomendamos que cree recursos IAM de propósito general, y si planea usar App2Container para desplegar sus contenedores o crear pipelines, que cree recursos IAM separados para el despliegue que tenga derechos elevados.
{{% notice note %}}
Para este taller, crearemos un usuario con los derechos de administrador para que pueda desplegar una aplicación en contenedor utilizando los servicios de AWS para el despliegue que son soportados por App2Container.
{{% /notice %}}

1. **Acceda a la consola de IAM haciendo clic en "Services" > "IAM" (Identity Access Management)**.

    a. En el panel izquierdo, haga clic en users > add user

    b. Marque "programmatic access".

    c. haga clic en "Next: Permissions"

    ![create-iam-user](/setting-up-app2container/create-iam-user.png)

2. Establezca los permisos para su usuario de App2Container.

    a. Seleccione "Attach existing policies directly"

    b. Marque "AdministratorAccess".

    c. Haga clic en "Next: Tags"

    ![set-permissions](/setting-up-app2container/set-permissions.png)

3. Revisar y crear un usuario

    a. Seleccione "Next: Review" y luego haga clic en Create User

    b. En la pantalla siguiente, busque el ID de la clave de acceso y la clave de acceso secreta y cópielos (los utilizará en los siguientes pasos).

    ![create-user](/setting-up-app2container/create-user.png)

    Utilizarás las credenciales de usuario de app2container que has creado en los pasos anteriores.

#### 2. Habilitar WinRM en el servidor web de origen

Para conectarse a su servidor de origen de Windows desde la máquina de trabajado de Windows Server 2019, deberá habilitar el protocolo WinRM en su servidor web de origen.
{{% notice note %}}
Su servidor de aplicaciones debe cumplir los requisitos que se enumeran para Windows en la [lista de aplicaciones soportadas](https://docs.aws.amazon.com/app2container/latest/UserGuide/supported-applications.html).
{{% /notice %}}

1. Conéctese a su servidor Source-NET-Web utilizando el protocolo RDP como se explica en [Conectando sus Instancias Windows]({{< ref "/connect-windows.es.md" >}}).
    - Puede encontrar el servidor web de origen navegando al servicio EC2 como se muestra a continuación. El nombre del servidor de la base de datos de origen es "Source-NET-WebServer"
   ![6_db](/db-mig/net-web-ec2.png)
{{% notice note %}}
Asegúrese de obtener la contraseña del servidor web como se describe en la sección [Conectando sus Instancias Windows]({{< ref "/connect-windows.es.md" >}}).
{{% /notice %}}
{{% notice note %}}
Copie la contraseña del servidor web en su bloc de notas, la utilizará en los siguientes pasos.
{{% /notice %}}
2. Vaya a la carpeta de descargas. Verá que se han descargado 2 archivos para este laboratorio.
    - WinRMSetup
    - New-SelfSignedCertificateEx
![create-user](/setting-up-app2container/net-winrm-setup-1.png)

3. Abra la consola de PowerShell
4. Vaya a la carpeta "Downloads" y ejecute el comando "ls" para asegurarse de que los archivos WinRMSetup.ps1 y New-SelfSignedCertificateEx.ps1 están en la misma ubicación.

    ```powershell
    cd Downloads
    ls
    ```

5. Ejecute el comando "WinRMSetup.ps1" e introduzca "R" cuando se lo pida la salida del comando.

    ```powershell
    .\WinRMSetup.ps1
    ```

    - Obtendrá el siguiente resultado.
        ![create-user](/setting-up-app2container/net-winrm-setup-2.png)

Ahora, usted ha configurado su Servidor Web de Origen para ser accedido a través del protocolo winRM en el puerto 5986. Y la máquina de trabajo remoto puede ahora acceder a él.

#### 3. Almacena tus secretos en AWS Secrets Manager

App2Container utiliza AWS Secrets Manager para administrar las credenciales para conectar su máquina de trabajo a los servidores de aplicaciones con el fin de ejecutar comandos remotos. Secrets Manager cifra sus secretos para almacenarlos y proporciona un nombre de recurso de Amazon (ARN) para que pueda acceder al secreto.

Cuando se ejecuta el comando de configuración remota, se proporciona el ARN del secreto para que App2Container utilice para conectarse a su servidor de destino cuando se ejecuta el comando remoto.

1. Navega hasta AWS Secrets Manager y haz clic en "Store a new secret"
![create-user](/setting-up-app2container/net-store-secret.png)
2. Seleccione "Other type of secrets", añada los siguientes parámetros y haga clic en "Next".

| Key              | Value             |
| ---------------------- | ---------------- |
| username                | Administrator           |
| password                | Copie la contraseña del servidor web que obtuvo en los pasos anteriores al conectarse al servidor web.           |

![create-user](/setting-up-app2container/net-store-secret-2-update.png)
3. Dé un nombre al secreto (por ejemplo, "unicornshopwebcredentials") y una descripción. Haga clic en "Next".
![create-user](/setting-up-app2container/net-store-secret-3.png)
4. Deja todo por defecto. Y haga clic en "Next".
![create-user](/setting-up-app2container/net-store-secret-4.png)
5. Una vez que almacene su secreto, haga clic en el nombre de la pantalla como se muestra a continuación.
![create-user](/setting-up-app2container/net-store-secret-5.png)
6. Copie "Secret ARN" en su bloc de notas como se muestra a continuación. Ya que lo utilizará en los pasos posteriores.
![create-user](/setting-up-app2container/net-store-secret-6.png)

#### 4. Configurar el perfil de AWS en la máquina de trabajo

Como vas a utilizar la maquina de trabajo para todo tu proceso de App2Container, tendrás que configurar tu maquina de trabajo con las configuraciones de tu perfil de AWS.

1. Conéctese a su máquina de trabajo como se explica en la sección [Conectando sus Instancias Windows]({{< ref "/connect-windows.es.md" >}}).

En la máquina de trabajo, debes crear un perfil para la CLI de AWS que App2Container pueda utilizar.  En este laboratorio configuramos el perfil por defecto abriendo powershell y ejecutando 'aws configure'.

{{% notice note %}}
Utilizarás las credenciales de usuario de app2container que has creado en los pasos anteriores.
{{% /notice %}}

```powershell
aws configure

  AWS Access Key ID [None]: <<add AWS access key from previous steps>>
  AWS Secret Access Key [None]: <<add AWS secret access key from previous steps>>
  Default region name [None]: us-west-2
  Default output format [None]: [blank]
```

¡Felicidades! Con este paso, ha configurado los prerrequisitos de su entorno y está listo para la instalación de App2Container en su entorno. En la siguiente sección, usted instalará App2Container en su máquina de trabajo y lo configurará para iniciar su proceso de contenerización.
