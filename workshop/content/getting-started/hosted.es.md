+++
title = "Asistir a un evento organizado por AWS"
weight = 40
pre = "<b>- </b>"
+++

Si asiste a un evento organizado por AWS, siga las siguientes comprobaciones antes de comenzar el taller para asegurarse de que el entorno del taller se despliega correctamente.

{{% notice warning %}}
Por favor, desconéctese de la VPN antes de comenzar el taller, ya que algunas VPNs pueden bloquear el acceso a URLs públicas.
{{% /notice %}}  

1. Inicie sesión en el panel de control de su equipo utilizando la URL de Hash que se le comunicó antes del evento.
    - Deberá ver la siguiente pantalla como su Tablero de Equipo.

    ![team](/intro/team-dashboard.png)

2. Inicie sesión en su consola de AWS desde su tablero de equipo.

3. Actualice el nombre de su equipo haciendo clic en el botón "Set Team Name" en el panel de control del equipo.

4. En los eventos alojados en AWS, el entorno del taller ya está desplegado para usted como stack de Cloudformation.
    - Vaya a CloudFormation desde los servicios y compruebe que se ha desplegado un stack de Cloudformation (CREATE_COMPLETE) como se muestra a continuación.

    ![team](/intro/check-stack.png)

    - Haga click en el stack creado y seleccione la pestaña **Outputs**. Verá información como en la captura de pantalla de abajo.

    ![Source Environment Information in AWS Console](/intro/self-service-env-awsconsole-info.en.png)

    - Copie - pegue esta información en su bloc de notas local o mantenga esta página abierta en otra pestaña del navegador, ya que la utilizará durante el taller.

5. Navegue al servicio EC2, compruebe que hay 5 instancias desplegadas y en funcionamiento como se muestra a continuación.

![team](/intro/check-instances.png)

{{% notice note %}}
Si observa algún problema en uno de estos pasos, póngase en contacto con su operador de eventos de AWS para resolverlo.
{{% /notice %}}  

### Muy bien. Ya está todo listo para empezar.

Ahora, elige tu aplicación (Java o .NET) y empieza a modernizar
