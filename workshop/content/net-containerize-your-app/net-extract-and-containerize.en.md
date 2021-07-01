+++
title = "Extract and Containerize"
weight = 150
pre = "<b>2.4 </b>"
+++

In this module, you will be transforming your application with app2container. The transform phase activities depends on whether you are running all steps on the application server, or are using the application server for the analysis and a worker machine for containerization and deployment.
![extract-and-containerize-intro](/extract-and-containerize/extract-containerize-overview.png)
In this scenario ( [Containerize your .NET app]({{< ref "/net-containerize-your-app/_index.en.md" >}})), you will be running all below steps on the Worker Machine.

{{% notice note %}}
Separating the containerization environment using a worker machine is a best practice and provides you security and functional benefits and allows you to standardize your containerization process to help your larger scale containerization efforts.
{{% /notice %}}  

#### 1. Extract your artifacts from Source Server to Worker Machine

1. As suggested next step from the "app2container analyze" command output, run "app2container remote extract" command to retrieve all Application artifacts to the worker machine. Update the application-id and target IP address based on your environment.

```powershell
app2container remote extract --target <private-IP of your source server> --application-id <net-app-id>
```

- Extraction process will take few minutes and App2Container will create the zip file an which includes all application artifacts. After this point, you don't need to communicate with Source application server as app2container now has all the application related artifacts in the worker machine.
{{% notice note %}}
At this point, you can also update the artifacts inside the zip file and continue the containerization with the updated zip file.
{{% /notice %}}  

![extract-and-containerize-intro](/extract-and-containerize/net-extract-1.png)

- Once containerization process finished you should see the output as below.
![extract-and-containerize-intro](/extract-and-containerize/net-extract-2.png)

#### 2. Containerize your application on Worker Machine

1. Replace **PrivateIPAddress** with the IP address of your web server and Run "app2container containerize --input-archive" on your Worker machine.
    - Provide the zip file location for your input-archive parameter. This command allows you to containerize based on the extracted files on the Worker machine rather than doing the containerization on source machine.

{{% notice note %}}
You can copy the suggested command from the CLI output.
{{% /notice %}}

```powershell
app2container containerize --input-archive C:\Users\Administrator\AppData\Local\app2container\remote\<PrivateIPAddress>\<net-app-id>\<net-app-id>.zip
```

The process will take few mins, as Worker machine will need download the Windows 2019 base image for your container. Once process finished, you should see the below output.
    ![extract-and-containerize-intro](/extract-and-containerize/net-containerize-1.png)

2. At this stage, you should have your container images ready. Run "docker images" on Worker machine to check the images are built.

```powershell
docker images
```

- You can see the container image is created for your .NET application with the chosen containerBaseImage (Windowsservercore-ltsc-2019) and imageTag (latest).
![extract-and-containerize-intro](/extract-and-containerize/net-containerize-2.png)

{{% notice note %}}
Notice that, you've updated your operating system at this step. This could be especially useful where your applications are running on EOL(End Of Life) OS versions.
{{% /notice %}}  
3. When you run "App2container containerize" command, it automatically creates a deployment.json file inside the application folder (as shown below)

- This file includes AWS deployment configurations.
- You can update this file to adapt the deployment with your AWS environment.
- E.g. You can select specific VPC for your target environment or define cpu/memory allocation setting for your container.

![extract-and-containerize-intro](/extract-and-containerize/net-containerize-3.png)
4. Open the file and review the configurations.

- Once you open the file, you will notice that there are various configuration settings for each AWS service. App2Container pre-built those settings for you, but you can update these to adopt your environment.
  - In this workshop, you will need to check 2 places.
![extract-and-containerize-intro](/extract-and-containerize/net-containerize-4.png)

1. Ensure "createEcsArtifacts" is set to "true". [x]
    - ecsParameters section is for the settings when ECS (Elastic Container Service) is decided as the target environment. You need to set "createEcsArtifacts" to "false" if you don't want to deploy into ECS.

2. Enter your Target vpc-id to deploy your application into your target network.

    - Go to AWS console and Navigate to VPC Service.
    - In your VPCs, find "TargetVPC" and copy the vpc-id as shown below.
    ![java-deployment-1](/extract-and-containerize/java-deployment-vpc.png)

    - In deployment.json file, Find "reuseResources" part and paste your copied for "vpcId" value. As show on the above example.

{{% notice note %}}
If you don't update your VPC-ID in deployment.json file, app2container will deploy your application into the Default VPC.
{{% /notice %}}  

Congratulations! You have defined all your target AWS configuration settings and ready to deploy your application.

In the next section you will create deployment artifacts for your deployment.
