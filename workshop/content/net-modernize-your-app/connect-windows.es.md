+++
title = "Conectando sus Instancias Windows"
weight = 20
pre = "<b>- </b>"
+++

#### 1. Conectese a su servidor Windows a través de RDP

Para conectarse a una instancia de Windows, debe recuperar la contraseña de administrador inicial y luego especificar esta contraseña cuando se conecte a su instancia utilizando Remote Desktop.
Para más detalles, consulte [Conectarse a una instancia de Windows mediante RDP](https://docs.aws.amazon.com/es_es/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html#connect-rdp).

- Vaya al servicio CloudFormation y haga clic en el stack del taller.
- En la pestaña de Outputs, haga clic en el enlace SSHKeyURL como se muestra a continuación. Se mostrará la clave SSH que se utilizará para recuperar el acceso de administrador.
- Copie la clave en su bloc de notas, ya que la utilizará en el siguiente paso.
![create-iam-user](/setting-up-app2container/ssh-key-url.png)

- Vaya a EC2 Service y haga clic en su Source-NET-Web Server y haga clic en "Connect" en la parte superior derecha de la consola.
- En la pantalla "Connect to instance", haga clic en "RDP Client" y haga clic en "Get password" como se muestra a continuación:
![create-iam-user](/setting-up-app2container/net-get-password.png)
- En la siguiente pantalla, pegue su clave SSH que ha copiado en el paso anterior y haga clic en "Decrypt Password".
![create-iam-user](/setting-up-app2container/net-decrypt-password.png)
- Recibirás tu contraseña, cópiala en tu bloc de notas ya que la utilizarás durante el taller.
![create-iam-user](/setting-up-app2container/net-copy-password.png)

#### 2. Conecte su servidor web a través de Session Manager

También puede conectarse a su servidor powershell a través de Session Manager. Para más información sobre "Session Manager", siga [Session Manager-Getting-Started](https://docs.aws.amazon.com/es_es/systems-manager/latest/userguide/session-manager-getting-started.html)

- Vaya a EC2 Service y haga clic en Source-NET-Web Server y haga clic en "Connect" en la parte superior derecha de la consola.
- En la pantalla "Connect to instance", haga clic en "Session Manager" y haga clic en "Connect" como se muestra a continuación:
    ![create-iam-user](/setting-up-app2container/net-connect-session-manager.png)
- Se conectará directamente a la consola de Powershell en el servidor web.
