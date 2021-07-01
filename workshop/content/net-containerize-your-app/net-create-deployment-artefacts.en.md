+++
title = "Create Deployment Artifacts"
weight = 160
pre = "<b>2.5 </b>"
+++

In this step, App2Container generates the artifacts needed to deploy your application container in AWS. App2Container pre-fills key values in the artifacts based on your profile, the application analysis, and best practices.

You have two options for deployment to your target container management environment (Amazon ECR with Amazon ECS or Amazon EKS).

{{% notice note %}}
In this scenario, you will deploy your application container into Amazon ECR with Amazon ECS.
{{% /notice %}}  

![app2container-intro](/create-deployment-artifacts/create-deployment-overview.png)

![app2container-intro](/create-deployment-artifacts/net-generate-6.png)

1. Run "app2container generate app-deployment" command to create the CloudFormation template for your deployment.

    ```powershell
    app2container generate app-deployment --application-id <net-app-id>
    ```

2. The process will take few mins. You should see the below output.

![app2container-intro](/create-deployment-artifacts/net-generate-1.png)

 With this command, App2container does the following activities:

1. Creates an ECR (Elastic Container Registry) Repository moves your application container image to the registry.
   - Go AWS Console and navigate to ECS Service.
     - Click "Amazon ECR" on the left pane.
     - You will see that your container is automatically registered with this ECR repository.
     ![app2container-intro](/create-deployment-artifacts/net-generate-2.png)
     - Once you click into your Repository, you will see that your container image with the "latest" tag is deployed.
     ![app2container-intro](/create-deployment-artifacts/net-generate-3.png)
{{% notice note %}}
You can update this tag and other container parameters in "analysis.json" file.
{{% /notice %}}  
2. Creates an ECS Task Definition and registers it with ECS.
    - NOTE: ECS Task definitions required to run docker images in ECS. For further information about Task Definitions, you can check <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html" target="_blank">Amazon ECS Task Definitions</a>.
    ![app2container-intro](/create-deployment-artifacts/net-generate-4.png)
3. Uploads all CloudFormation resources to your selected S3 bucket.
    - NOTE: You have defined this bucket earlier in  [Install & Initialize App2container]({{< ref "/app2container-steps.en.md" >}}) section.
    - Go to S3 service. And navigate to your defined bucket initially.
    - You will see that app2container created a separate bucket for all your applications and put all the deployment artifacts are uploaded in those bucket.
    ![app2container-intro](/create-deployment-artifacts/net-generate-7.png)
4. Creates a CloudFormation Master template.
    - App2Container reviews your environment and prefills the Cloudformation template based on your desired configurations you've defined in previous steps, application analysis and best practices.
    - This CloudFormation template describes all required resources and their dependencies so you can launch and configure them together as a stack.
    - On the Worker Machine, locate the EcsDeployment folder location in your application folder as shown below and open the file.
    ![app2container-intro](/create-deployment-artifacts/net-generate-5.png)
    - Locate the ecs-master.yml file location in EcsDeployment folder as shown below and open the file.
![app2container-intro](/create-deployment-artifacts/net-generate-6.png)
   {{% notice note %}}
You do not need to make any changes in this file in this workshop. But you can also update this file based on your needs.
  {{% /notice %}}  

Congrats!, you are now all set and ready to deploy your .NET Application to AWS ECS! Continue with the next section to trigger the deployment.
