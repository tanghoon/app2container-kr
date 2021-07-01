+++
title = "Extract and Containerize"
weight = 60
pre = "<b>2.4 </b>"
+++

In this module, you will transform your application with app2container. The Extract and Containerize phase activities depends on whether you are running all steps on the application server, or are using the application server for the analysis and a worker machine for containerization and deployment.
![extract-and-containerize-intro](/extract-and-containerize/extract-containerize-overview.png)
In this scenario ( [Containerize your java app]({{< ref "/java-containerize-your-app/_index.en.md" >}})), you will be running all below steps on the application server.

{{% notice note %}}
Separating containerization environment using worker machine is a best practice and provides you security and functional benefits. Worker machine option is used In the next scenario [Containerize your .NET app]({{< ref "/net-containerize-your-app/_index.en.md" >}}).
{{% /notice %}}  

1. Run the following commands to start the containerization  process, replace \<java-app-id\> with your Java Application ID.

    ```bash
    app2container containerize --application-id <java-app-id>
    ```

    - Containerization process will take few minutes and App2Container will create the docker file and deployment.json file which includes all AWS deployment related configuration.
    - Once containerization process is finished you should see output as below.

    ```bash
    root@ip-10-0-0-129:/home/ubuntu# app2container containerize --application-id java-tomcat-21fb1eb0
    ✔ AWS prerequisite check succeeded
    ✔ Docker prerequisite check succeeded
    ✔ Extracted container artifacts for application
    ✔ Entry file generated
    ✔ Dockerfile generated under /root/app2container/java-tomcat-21fb1eb0/Artifacts
    ✔ Generated dockerfile.update under /root/app2container/java-tomcat-21fb1eb0/Artifacts
    ✔ Generated deployment file at /root/app2container/java-tomcat-21fb1eb0/deployment.json
    👍 Containerization successful. Generated docker image java-tomcat-21fb1eb0

    💡 You're all set to test and deploy your container image.

    Next Steps:
    1. View the container image with "docker images" and test the application.
    2. When you're ready to deploy to AWS, please edit the deployment file as needed at /root/app2container/java-tomcat-21fb1eb0/deployment.json.
    3. Generate deployment artifacts using "app2container generate app-deployment --application-id java-tomcat-21fb1eb0"
    ```

2. Now, you have your docker image ready. You can verify this by running the command below.

    ```bash
    docker images
    ```

    - As you can see, the container image was created for your Java Application with chosen container base image (ubuntu 18.4) and tag (latest).

    ```bash
    root@ip-10-0-0-129:/home/ubuntu# docker images
    REPOSITORY           TAG     IMAGE ID      CREATED        SIZE
    java-tomcat-21fb1eb0 latest  5e382c3f2e6a  3 minutes ago  715MB
    ubuntu               18.04   2c047404e52d  6 weeks ago    63.3MB
    ```

    With this step, you have successfully containerized your application. It is now ready to be deployed on AWS.

3. When you run "App2container containerize" command, it automatically creates a deployment.json file. This file includes AWS deployment configuration. You can update this file to customize the deployment in your AWS environment. E.g. You can select specific VPC for your target environment or define cpu/memory allocation setting for your container..

- Open the file and review the configuration. NOTE: Update your file location in the below command from the command output.

```bash
nano /root/app2container/<java-app-id>/deployment.json
```

- Once you open the file, you will notice that there are various configuration settings for each AWS service. App2Container pre-builds those settings for you, but you can update these to adopt your environment.

In this workshop, you will need to check 2 places.

1. Ensure "createEcsArtifacts" is set to "true". [x]
    - ecsParameters section is for the settings when ECS (Elastic Container Service) is decided as the target environment. You need to set "createEcsArtifacts" to "false" if you dont want to deploy into ECS.
{{% notice note %}}
Notice that "deployTarget" is set to "FARGATE" below. You can set this to "EC2" if you want the control to manage the ec2 instances that host your containers.
{{% /notice %}}  
    ![java-deployment-1](/extract-and-containerize/java-deployment-1.png)

2. Enter your Target vpc-id to deploy your application into your target network.

    - Go to AWS console and Navigate to the <a href="https://us-west-2.console.aws.amazon.com/vpc/home?region=us-west-2#vpcs" target="_blank"> VPC console </a>
    - In your VPCs, find "TargetVPC" and copy the vpc-id as shown below.
    ![java-deployment-1](/extract-and-containerize/java-deployment-vpc.png)
    - In deployment.json file, Find "reuseResources" part and paste your copied for "vpcId" value.
    ![java-deployment-1](/extract-and-containerize/java-deployment-2.png)

3. Exit from the file with "ctrl+x" and type "y" and hit enter to save the file with the same name.

{{% notice note %}}
If you dont update your VPC-ID in deployment.json file, app2container will deploy your application into the Default VPC.
{{% /notice %}}  

Congratulations! You have defined all your target AWS configuration settings and ready to deploy your application.

In the next section you will create deployment artifacts for your deployment.
