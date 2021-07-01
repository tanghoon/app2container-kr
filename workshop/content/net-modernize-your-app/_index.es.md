+++
title = "Modernice su aplicación .NET"
weight = 110
pre = "<b>B. </b>"
+++

En esta sección del taller, usted modernizará su aplicación .NET.

Su aplicación .NET se ejecuta en el servidor web IIS (Internet Information Services) en un servidor Windows 20212 R2. La aplicación se comunica con el servidor SQL que se ejecuta en un servidor separado.

- Usted no tiene demasiada información sobre su aplicación .NET ni del codigo fuente y al final de este módulo la desplegará en la arquitectura que se muestra a continuación.
![source-target](/modernization/net-source-target.png)

En este taller, utilizará las herramientas de migración y modernización de AWS, así como las herramientas de bases de datos nativas para ayudarle en su viaje de modernización.

1. Enfoque nativo de migración de copia de seguridad y restauración para migrar su base de datos MSSQL a AWS RDS (Relational Database Service).
2. App2Container para
    - Analizar,
    - Containerizar,
    - Crear artefactos de despliegue de la aplicación,
    - Desplegar su aplicación en Amazon ECS (Elastic Container Service)
    - Crear un pipeline de CI/CD (Opcional)


{{% notice note %}}
En este taller se utilizará una máquina de trabajo para todas las operaciones de App2Container que se comunicará con el servidor de origen a través de <a href="https://docs.microsoft.com/en-us/windows/win32/winrm/portal" target="_blank">WinRM</a> (Windows Remote Management).
{{% /notice %}}  

Para los detalles de conexión de las máquinas Windows, por favor revise [Conectando sus Instancias Windows]({{< ref "/connect-windows.es.md" >}}) a través de la sección .NET.

NOTA: La migración de la base de datos es opcional en este módulo.

1. Por favor, continúe con la siguiente sección [Migrar su Base de datos]({{< ref "/net-migrate-your-db/_index.es.md" >}}) para migrar su Base de Datos utilizando el enfoque de migración Nativa de Copia de Seguridad y Restauración.
2. Alternativamente, continúe con la sección [Containerice su aplicación .NET]({{< ref "/net-containerize-your-app/_index.es.md" >}}) para iniciar directamente los procesos de App2Container.

¡Buena suerte!
