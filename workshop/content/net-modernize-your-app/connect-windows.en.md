+++
title = "Connecting your Windows Instances"
weight = 20
pre = "<b>- </b>"
+++

#### 1. Connect your Windows Server via RDP

To connect to a Windows instance, you must retrieve the initial administrator password and then specify this password when you connect to your instance using Remote Desktop.
For further details, please refer to [Connecting to Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html#connect-rdp).

- Go to CloudFormation service and Click to your Workshop stack.
- From output tab, click SSHKeyURL link as shown below. It will bring the SSH key which will be used to retrieve Administrator access.
- Copy the key to your notepad as you will use it in the next step.
![create-iam-user](/setting-up-app2container/ssh-key-url.png)

- Go to EC2 Service and click your Source-NET-Web Server and click "Connect" on the upper right side of the console.
- On the "Connect to instance" screen, Click "RDP client" and Click "Get password" as shown below:
![create-iam-user](/setting-up-app2container/net-get-password.png)
- On the coming screen, paste your SSHKey which you've copied in the previous step and click "Decrypt Password"
![create-iam-user](/setting-up-app2container/net-decrypt-password.png)
- You will get your password, copy it to your notepad as you will use it during the workshop.
![create-iam-user](/setting-up-app2container/net-copy-password.png)

#### 2. Connect your Web Server via Session Manager

You can also connect to your server powershell via Session Manager. For further information about "Session Manager", follow [Session Manager-Getting-Started](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started.html )

- Go to EC2 Service and click your Source-NET-Web Server and click "Connect" on the upper right side of the console.
- On the "Connect to instance" screen, Click "Session Manager" and Click "Connect" as shown below:
    ![create-iam-user](/setting-up-app2container/net-connect-session-manager.png)
- You will directly connect to Powershell console on the webserver.
