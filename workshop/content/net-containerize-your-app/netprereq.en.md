+++
title = "App2Container Prerequisites"
weight = 30
pre = "<b>2.1 </b>"
+++

AWS App2Container requires below prerequisites completed before you use it for the first time.
In this lab, we have pre-installed all the prerequisites tools and created the source environment compatible with these prerequisites.

You can check the Prerequisites check-list from [App2Container Prerequisites](https://docs.aws.amazon.com/app2container/latest/UserGuide/start-containerize-dotnet-app.html)

As the next step, please follow the below tasks to set-up your App2Container service.

#### 1. Grant permissions to IAM Users

{{% notice note %}}
If you have done previous module (Modernize your Java App) and created IAM user for app2container, you can skip this step.
{{% /notice %}}
App2Container needs access to AWS services in order to run most of its commands. There are two very different sets of permissions needed to run *app2container* commands.

- The general purpose user or group can run all of the commands except commands that are run with the --deploy option.
- For deployment, App2Container must be able to create or update AWS objects for container management services (Amazon ECR with Amazon ECS or Amazon EKS), and to create CI/CD pipelines with AWS CodePipeline. This requires elevated permissions that should only be used for deployment.

We recommend that you create general purpose IAM resources, and if you plan to use App2Container to deploy your containers or create pipelines, that you create separate IAM resources for deployment which has elevated rights.
{{% notice note %}}
For this workshop, we will create a user with the administrator rights so it can deploy a containerized application using the AWS services for deployment that are supported by App2Container.
{{% /notice %}}

1. **Navigate to the IAM Console by clicking “Services” > “IAM” (Identity Access Management)**

    a. From the left pane, click users > add user
    b.  Check the 'programmatic access'
    c. click Next: Permissions

    ![create-iam-user](/setting-up-app2container/create-iam-user.png)

2. Set permissions for your App2Container user.

    a.  Select "Attach existing policies directly"
    b.  Check "AdministratorAccess"
    c. Click "Next: Tags"

    ![set-permissions](/setting-up-app2container/set-permissions.png)

3. Review and Create User

    a. Select “Next: Review” and then click Create User
    b. On the Success screen, find the Access key ID and Secret Access Key and copy them down (you’ll use them in the following steps).

    ![create-user](/setting-up-app2container/create-user.png)

    You will use the credentials for app2container user details you’ve created in previous steps.

#### 2. Enable WinRM on Source WebServer

In order to connect to your Windows Source Server from  Windows Server 2019 worker machine, you will need to enable WinRM protocol on your Source Web Server.
{{% notice note %}}
Your application server must meet the requirements that are listed for Windows in the [Supported Applications](https://docs.aws.amazon.com/app2container/latest/UserGuide/supported-applications.html) list
{{% /notice %}}

1. Connect to your Source-NET-Web Server using RDP protocol as explained in [Connecting your Windows Instances]({{< ref "/connect-windows.en.md" >}}) section.
    - You can find the Source Web Server by navigating to EC2 service as shown below. Source DB server name is "Source-NET-Web-Server"
   ![6_db](/db-mig/net-web-ec2.png)
{{% notice note %}}
Ensure to get the Web server password as described in [Connecting your Windows Instances]({{< ref "/connect-windows.en.md" >}}) section.
{{% /notice %}}
{{% notice note %}}
Copy Web Server password into your notepad, you will use it in the following steps.
{{% /notice %}}
2. Navigate to Downloads folder. You will see 2 files downloaded for you for this workhsop.
    - WinRMSetup
    - New-SelfSignedCertificateEx
![create-user](/setting-up-app2container/net-winrm-setup-1.png)

3. Open the PowerShell console
4. Navigate to "Downloads" folder and run "ls" command to ensure WinRMSetup.ps1 and New-SelfSignedCertificateEx.ps1 files are at the same location.

    ```powershell
    cd Downloads
    ls
    ```

5. Run the "WinRMSetup.ps1" command and enter "R" when prompted by the command output.

    ```powershell
    .\WinRMSetup.ps1
    ```

    - You will get the below output.
        ![create-user](/setting-up-app2container/net-winrm-setup-2.png)

Now, you've configured your Source Web Server to be accessed via winRM protocol on port 5986. And Remote Worker machine can now access to it.

#### 3. Store your Secrets on AWS Secrets Manager

App2Container uses AWS Secrets Manager to manage the credentials for connecting your worker machine to application servers in order to run remote commands. Secrets Manager encrypts your secrets for storage, and provides an Amazon Resource Name (ARN) for you to access the secret.

When you run the remote configure command, you provide the secret ARN for App2Container to use to connect to your target server when running the remote command.

1. Navigate to AWS Secrets Manager and Click "Store a new secret"
![create-user](/setting-up-app2container/net-store-secret.png)
2. Select "Other type of secrets" and add below parameters and click "next"

| Key              | Value             |
| ---------------------- | ---------------- |
| username                | Administrator           |
| password                | Copy your Web Server password you get in previous steps while connecting to Web Server.           |

![create-user](/setting-up-app2container/net-store-secret-2-update.png)
3. Give a secret name (e.g "unicornshopwebcredentials") and Description. Click "Next"
![create-user](/setting-up-app2container/net-store-secret-3.png)
4. Leave everything as default. And click "Next"
![create-user](/setting-up-app2container/net-store-secret-4.png)
5. Once you store your secret, click the screen name as shown below.
![create-user](/setting-up-app2container/net-store-secret-5.png)
6. Copy "Secret ARN" to your notepad as shown below. As you will use it in later steps.
![create-user](/setting-up-app2container/net-store-secret-6.png)

#### 4. Configure your AWS profile on Worker Machine

As you will use Worker Machine for all your App2Container process, you will need to configure your Worker Machine with your AWS profile configurations.

1. Connect to your Worker Machine as explained in [Connecting your Windows Instances]({{< ref "/connect-windows.en.md" >}}) section.

In the Worker Machine, you must create a profile for the AWS CLI which App2Container can use.  In this lab we setup the default profile by opening powershell and running ‘aws configure’.

{{% notice note %}}
You will use the credentials for app2container user details you’ve created in previous steps.
{{% /notice %}}

```powershell
aws configure

  AWS Access Key ID [None]: <<add AWS access key from previous steps>>
  AWS Secret Access Key [None]: <<add AWS secret access key from previous steps>>
  Default region name [None]: us-west-2
  Default output format [None]: [blank]
```

Congrats! With this step, you set up your environment prerequisites and ready for App2Container installation on your environment. In the next section, you will install App2Container on your Worker Machine and set it up to start your containerization process.
