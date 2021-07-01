+++
title = "Instalar e inicializar App2Container"
weight = 30
pre = "<b>2.2 </b>"
+++

#### 1. Conéctese al servidor web


1. Conéctese al **Source-Java-Webserver** donde se ejecuta su aplicación Java.

   Tiene múltiples opciones para conectar su Servidor Web. Verifique <a href="https://docs.aws.amazon.com/es_es/AWSEC2/latest/UserGuide/AccessingInstances.html" target="_blank">este artículo</a> para más detalles sobre cómo conectarse a su instancia Linux.

   - Puede conectarse a su servidor web mediante el uso de AWS Systems Manager - <a href="https://docs.aws.amazon.com/es_es/systems-manager/latest/userguide/session-manager.html" target="_blank">Session Manager</a>. Session Manager proporciona una gestión de instancias segura y auditable sin necesidad de abrir puertos de entrada, mantener hosts de bastión o gestionar claves SSH.
       
        - Vaya a sus instancias de Amazon EC2, seleccione su servidor web Java y haga clic en "Connect".
        - Seleccione "Session Manager" y haga clic en "Connect"
       
          ![create-s3-bucket](/setting-up-app2container/session-manager.png)

    **O**

    - Si prefieres conectarse por SSH al servidor web, necesitarás los siguientes datos.

        a. Public DNS Name of the webserver <br>
        b. Username configured in the webserver <br>
        c. SSHKey <br>

        Puede encontrar esta información en la sección de salida del stack de cloudformation. Se puede acceder al stack navegando a la sección <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank">CloudFormation</a> de la consola.

        | Parameter              | Value                                               |
        | ---------------------- | --------------------------------------------------- |
        | Username   | ubuntu                                |
        | SSHKey  | SSHKeyURL      (You need to create .pem file from this key)                          |
        | Public DNS Name        | JavaWebServerDNSName                                    |

  {{% notice note %}}
  Para la clave SSH, necesita crear un archivo .pem copiando la clave desde la URL a su editor de texto y guardarlo como archivo .pem.
  {{% /notice %}}  



#### 2. Configure su perfil de AWS

En el servidor web, necesitamos crear un perfil para la CLI de AWS que App2Container pueda utilizar.  En este laboratorio configuramos el perfil por defecto ejecutando 'aws configure'.

{{% notice note %}}
Utilizará las credenciales generada para el usuario de app2container que ha creado en el paso de prerrequisitos.
{{% /notice %}}

```bash
  sudo su   
```

```bash
  aws configure
```

```bash
  AWS Access Key ID [None]: <<add AWS access key from previous steps>>
  AWS Secret Access Key [None]: <<add AWS secret access key from previous steps>>
  Default region name [None]: us-west-2
  Default output format [None]: [blank]
```

#### 3. Instalar App2Container

1. La ejecución del comando App2Container en un servidor Linux requiere permisos elevados. Compruebe si está ejecutando como root o utilice "sudo su" cuando inicie la sesión en el servidor, o ponga el prefijo "sudo" para cada comando.

    ```bash
    sudo su
    ```

2. Descargue e instale App2Container para Linux

    - Una vez conectado al servidor linux de origen, utilice el comando *curl* para descargar el paquete de instalación de App2Container desde Amazon S3.

    ```bash
    curl -o AWSApp2Container-installer-linux.tar.gz https://app2container-release-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/AWSApp2Container-installer-linux.tar.gz
    ```

3. Extraiga el paquete a una carpeta local en el servidor y navegue a esa carpeta.

    ```bash
    tar xvf AWSApp2Container-installer-linux.tar.gz
    ```

4. Ejecute el script de instalación desde la carpeta donde lo extrajo y siga las indicaciones

      ```bash
       ./install.sh
      ```

5. (opcional) Familiarícese con los comandos disponibles para App2Container

    ```bash
    app2container help
    ```

#### 4. Inicializar App2Container

El comando init realiza tareas de inicialización únicas para App2Container. Este comando interactivo solicita la información necesaria para configurar el entorno local de App2Container.

1. En primer lugar, tendrá que crear un bucket de Amazon S3 donde App2Container pueda almacenar los artefactos durante el proceso de contenerización.

    - Navegue a la consola de S3 e introduzca un nombre único para su bucket (app2container-Sus-iniciales-fecha) y haga clic en crear y guarde el nombre del bucket en un bloc de notas (esto será necesario en los pasos posteriores)
    ![create-s3-bucket](/setting-up-app2container/s3-create-bucket.png)

2. Vuelva a entrar en el servidor web de origen e inicie app2container con el comando "app2container init".

    ```bash
    app2container init
    ```

  **Se le pide que proporcione la siguiente información:**

| Parameter              | Value             |
| ---------------------- | ---------------- |
| Workspace directory path                | Dejar por defecto. (Un directorio local donde App2Container puede almacenar artefactos durante el proceso de contenerización. El valor por defecto es /root/app2container.)|
| Use AWS EC2 Instance profile               | Dejar por defecto. (Puede elegir utilizar el perfil de instancia EC2 para ejecutar las operaciones de App2Container.)          |
| AWS profile               | Dejar por defecto. (Contiene la información necesaria para ejecutar App2Container, como sus claves de acceso a AWS.)          |
| Amazon S3 bucket                | Introduce el nombre del bucket de Amazon S3 (app2container-Sus-iniciales-fecha) que has creado en el paso anterior. (por ejemplo, app2container-sd-jan-29)          |
| Permission to collect usage metrics              | Dejar por defecto. (esta opción es para permitir que App2Container recopile información sobre el sistema operativo del host, el tipo de aplicación y los comandos de app2container que se ejecutan.)          |
| Permission to Automatically upload logs and App2Container generated artifacts on crashes and internal errors | Dejar por defecto. (esta opción es para permitir que App2Container cargue los registros y los artefactos en caso de errores internos)          |
| Whether to enforce signed images               | Dejar por defecto. (Puede requerir opcionalmente que las imágenes se firmen utilizando Docket Content Trust (DCT))          |

{{% notice note %}}
Asegúrese de introducir el nombre correcto del bucket de Amazon S3 (app2container-Sus-iniciales-fecha) que ha creado en el paso anterior una vez que se le solicite. Vea el ejemplo de salida a continuación.
{{% /notice %}}

```bash
Workspace directory path for artifacts[default: /root/app2container]:
Use AWS EC2 Instance profile 'arn:aws:iam::-:instance-profile/App2ContainerWorkshop-EC2InstanceProfile-' configured with this instance? (Y/N)[default: n]
AWS Profile (configured using 'aws configure --profile')[default: default]:
Optional S3 bucket for application artifacts: app2container-sd-jan-29
Report usage metrics to AWS? (Y/N)[default: y]:
Automatically upload logs and App2Container generated artifacts on crashes and internal errors? (Y/N)[default: y]:
Require images to be signed using Docker Content Trust (DCT)? (Y/N)[default: n]:
```

¡Felicitaciones! Usted ha instalado e inicializado App2Container y ahora está listo para comenzar a contenerizar su aplicación Java.

En los próximos módulos, usted va a contenerizar su aplicación web y desplegarla en AWS ECS con App2Container.
