+++
title = "Install & Initialize App2Container"
weight = 30
pre = "<b>2.2 </b>"
+++

#### 1. Connect to the Webserver


1. Connect to the **Source-Java-Webserver** where your Java application runs.

   You have multiple options to connect your Web Server. Check <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html" target="_blank">this article</a> for more detail on how to connect to your Linux instance.

    - You can connect to your Web server via using AWS Systems Manager - <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html" target="_blank">Session Manager</a>. Session Manager provides secure and auditable instance management without the need to open inbound ports, maintain bastion hosts, or manage SSH keys.
       
        - Go to your Amazon EC2 instances, select your Java Web Server and Click "Connect".
        - Choose "Session Manager" and click "Connect"
       
          ![create-s3-bucket](/setting-up-app2container/session-manager.png)

    **OR**

    - If you prefer to SSH into the webserver, you would need the following details.

        a. Public DNS Name of the webserver <br>
        b. Username configured in the webserver <br>
        c. SSHKey <br>

        You can find this information in the output section of the cloudformation stack. The stack can be accessed by navigating to the <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank">CloudFormation</a> console.

        | Parameter              | Value                                               |
        | ---------------------- | --------------------------------------------------- |
        | Username   | ubuntu                                |
        | SSHKey  | SSHKeyURL      (You need to create .pem file from this key)                          |
        | Public DNS Name        | JavaWebServerDNSName                                    |

  {{% notice note %}}
  For SSH key, you need to create a .pem file via copying the key from the URL to your text editor and save it as .pem file.
  {{% /notice %}}  



#### 2. Configure your AWS profile

In the webserver, we need to create a profile for the AWS CLI which App2Container can use.  In this lab we setup the default profile by running ‘aws configure’.

{{% notice note %}}
You will use the credentials for app2container user details you’ve created in the prerequisite step.
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

#### 3. Install App2Container

1. Running App2Container commands on a Linux server requires elevated permissions. Check if you running as root or use  "sudo su" when you log-in to server, or prefix "sudo" for each command.

    ```bash
    sudo su
    ```

2. Download and install App2Container for Linux

    - Once connected to the source linux server, Use the *curl* command to download the App2Container installation package from Amazon S3.

    ```bash
    curl -o AWSApp2Container-installer-linux.tar.gz https://app2container-release-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/AWSApp2Container-installer-linux.tar.gz
    ```

3. Extract the package to a local folder on the server and navigate to that folder.

    ```bash
    tar xvf AWSApp2Container-installer-linux.tar.gz
    ```

4. Run the install script from the folder where you extracted it and follow the prompts

      ```bash
       ./install.sh
      ```

5. (optional) Familiarize yourself with the available command for App2Container

    ```bash
    app2container help
    ```

#### 4. Initialize App2Container

The init command performs one-time initialization tasks for App2Container. This interactive command prompts for the information required to set up the local App2Container environment.

1. First, You will need to create an Amazon S3 bucket where App2Container can store artifacts during the containerization process.

    - Navigate to the S3 console and enter a unique name for your bucket (app2container-Your Initials-Date) and click create and store the name of the bucket in a notepad (this will be required in subsequent steps)
    ![create-s3-bucket](/setting-up-app2container/s3-create-bucket.png)

2. Login back to Source Web server and initiliaze app2container with  "app2container init" command.

    ```bash
    app2container init
    ```

  **You are prompted to provide the following information:**

| Parameter              | Value             |
| ---------------------- | ---------------- |
| Workspace directory path                | Leave Default. (A local directory where App2Container can store artifacts during the containerization process. The default is /root/app2container.)|
| Use AWS EC2 Instance profile               | Leave Default. (You can choose to use EC2 instance profile to run App2Container operations.)          |
| AWS profile               | Leave Default. (Contains information needed to run App2Container, such as your AWS access keys.)          |
| Amazon S3 bucket                | Enter the Amazon S3 bucket name (app2container-Your-Initials-Date) you have created in previous step. (e.g. app2container-sd-jan-29)          |
| Permission to collect usage metrics              | Leave Default. (this option is to allow App2Container to collect information about the host operating system, application type, and the app2container commands that you run.)          |
| Permission to Automatically upload logs and App2Container generated artifacts on crashes and internal errors | Leave Default. (this option is to allow App2Container to upload logs and artifacts on crashes and internal errors)          |
| Whether to enforce signed images               | Leave Default. (You can optionally require that images are signed using Docket Content Trust (DCT))          |

{{% notice note %}}
Make sure to enter the correct Amazon S3 bucket name (app2container-Your-Initials-Date) you've created in previous step once prompted. See example output below.
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

Congratulations! You have installed and initialized App2Container and you are now ready to start containerizing your Java Application.

In the next modules, you will containerize your Web Application and deploy it to AWS ECS with App2Container.
