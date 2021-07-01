+++
title = "Modernize your .NET App"
weight = 110
pre = "<b>B. </b>"
+++

In this section of the workshop, you will modernize your .NET Application.

You .NET application is running on IIS (Internet Information Services) Web Server on Windows 20212 R2 Server. Application communicates with SQL server which is running on separate server.

- You don't have too much information about your source .NET application and at the end of this module you will deploy it into below architecture shown below.
![source-target](/modernization/net-source-target.png)

In this workshop, you will use AWS migration and Modernization tools as well as native Database tools to help your modernization journey.

1. Native Backup and Restore migration approach to migrate your MSSQL Database to AWS RDS (Relational Database Service).
2. App2Container to
    - Analyze;
    - Containerize;
    - Create Application's deployment artifacts;
    - Deploy your Application to Amazon ECS (Elastic Container Service);
    - Create CI/CD pipeline (Optional).

{{% notice note %}}
In this workshop you will use a worker machine for all App2Container operations which will communicate with the Source Server via <a href="https://docs.microsoft.com/en-us/windows/win32/winrm/portal" target="_blank">WinRM</a> (Windows Remote Management).
{{% /notice %}}  

For Windows machines login details, please check  [Connecting your Windows Instances]({{< ref "/connect-windows.it.md" >}}) through out the .NET section.

NOTE: Database Migration is optional in this module.

1. Please continue with the next section [Migrate Your Database]({{< ref "/net-migrate-your-db/_index.it.md" >}}) to migrate your Database using Native Backup and Restore migration approach.
2. Alternatively,  continue with [Containerize your .NET App]({{< ref "/net-containerize-your-app/_index.it.md" >}})  section to start directly from App2Container processes.

Good Luck!
