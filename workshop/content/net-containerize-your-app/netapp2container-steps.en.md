+++
title = "Install & Initialize App2Container"
weight = 40
pre = "<b>2.2 </b>"
+++

#### 1. Install app2container on the Worker Machine

1. Connect to your Worker Machine as explained in [Connecting your Windows Instances]({{< ref "/connect-windows.en.md" >}}) section.
    - You can find the Worker Machine by navigating to EC2 service as shown below.  name: "Worker Machine"
   ![6_db](/db-mig/net-worker-machine.png)

2. Open a web browser and Download the AWSApp2Container Windows installer to your server from the following location : <https://app2container-release-us-east-1.s3.us-east-1.amazonaws.com/latest/windows/AWSApp2Container-installer-windows.zip>

3. The Installer file will be downloaded into "Downloads" folder. Navigate to "Downloads folder" and extract "AWSApp2Container-installer-windows.zip" file.
4. Run "install.ps1" powershell script and enter "R" when prompted by the command output. Then press "y" to accept terms and conditions

    ```powershell
    ./install.ps1
    ```

    ![create-user](/setting-up-app2container/net-install-a2c.png)

5. (optional) Familiarize yourself with the available command for App2Container

    - Run the following command to List the available commands for App2Container

    ```powershell
    app2container help
    ```

#### 2. Initialize App2Container

The init command performs one-time initialization tasks for App2Container. This interactive command prompts for the information required to set up the local App2Container environment.

1. First, You will need to create an Amazon S3 bucket where App2Container can store artifacts during the containerization process.

{{% notice note %}}
If you've already created an S3 bucket in "Modernize your Java App" section, you can reuse the same bucket and you can skip this step.
{{% /notice %}}

- Navigate to the S3 console and enter a unique name for your bucket (app2container-\<Your-Initials\>-\<Date\>) and click create and store the name of the bucket in a notepad (this will be required in subsequent steps)

    ![create-s3-bucket](/setting-up-app2container/s3-create-bucket.png)

2. On the worker Machine initialize app2container with  "app2container init" command.

    ```powershell
    app2container init
    ```

  **You are prompted to provide the following information:**

| Parameter              | Value             |
| ---------------------- | ---------------- |
| Workspace directory path                | Leave Default. (A local directory where App2Container can store artifacts during the containerization process. The default is C:\Users\Administrator\AppData\Local\app2container.)|
| AWS profile               | Leave Default. (Contains information needed to run App2Container, such as your AWS access keys.)          |
| Amazon S3 bucket                | Enter the Amazon S3 bucket name (app2container-Your-Initials-Date) you've created in previous step. (e.g. sd-jan-29)          |
| Permission to collect usage metrics              | Leave Default. (this option is to allow App2Container to collect information about the host operating system, application type, and the app2container commands that you run.)          |
| Whether to enforce signed images               | Leave Default. (You can optionally require that images are signed using Docket Content Trust (DCT))          |

{{% notice note %}}
Ensure to enter the Amazon S3 bucket name (app2container-Your-Initials-Date) you've created in previous step once prompted. See example output below.
{{% /notice %}}

![create-user](/setting-up-app2container/net-a2c-init.png)

#### 3. Configure Remote Connection

You need to run "remote configure" command from a worker machine to configure the connections needed to run remote workflows on application servers. This interactive command prompts for the required information for each application server that you enter.

| Server IP Address              | Value             |
| ---------------------- | ---------------- |
| Server IP Address                | Source Server (Source-NET-Webserver) Private IP Address. (You can find from EC2>Source-NET-Webserver> Details> Private IPv4  addresses)           |
| Server FQDN                | Leave blank          |
| Secret ARN                | ARN for Secret you've created on AWS Secrets Manager earlier          |
| Continue to configure another server ?                | n          |

{{% notice note %}}
Copy the Private IP address of the Web Server to your notepad, you will use it in the following sections.
{{% /notice %}}

```powershell
app2container remote configure
```

  ![create-user](/setting-up-app2container/net-remote-configure.png)

{{% notice note %}}
You can configure multiple servers at this step for large scale projects.
{{% /notice %}}

Congrats! You have now installed and initialize the App2container on your Worker Machine and ready to start containerizing your .NET application via remotely running the app2container process without directly touching your source application server.
