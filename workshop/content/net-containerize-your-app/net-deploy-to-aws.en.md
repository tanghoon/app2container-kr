+++
title = "Deploy to AWS Cloud"
weight = 170
pre = "<b>2.6 </b>"
+++

Congratulations! You are now in the last step of App2container process.

![app2container-intro](/deploy-to-aws/Deploy-to-AWS-overview.png)

At this stage:

- Your application container image is already moved into ECR (Elastic Container Registry) and ECS Task definition is created and registered.
- App2container created the Master Cloudformation template for you to automatically deploy and configure all required resources in Cloud.

In this section, you will launch your Cloudformation template and review the deployment process in AWS console.

1. In your Worker Machine, run suggested command from "app2container generate app-deployment" output.

    ```powershell
    aws cloudformation deploy --template-file C:\Users\Administrator\AppData\Local\app2container\<net-app-id>\EcsDeployment\ecs-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<net-app-id>-ECS
    ```

    - When you run this command, it will create a Cloudformation stack named as "a2c-\<net-app-id\>-ECS".

    {{% notice note %}}
    The process will take few mins. Go back to AWS Console to review the background activities during the deployment.
    {{% /notice %}}  

2. In AWS Console, Go to "CloudFormation" from services and Click "Stacks".
    - You will see that a stack has been created with name "a2c-\<net-app-id\>-ECS".
    - Notice Nested stacks are triggered by main stack for each deployment. E.g. A separate stacks for ALB (Application LoadBalancer) and ECS Clusters.
    - You can see the triggered events under "events" tab for each stack.
    ![deploy-cloudformation-1](/deploy-to-aws/net-deploy-1.png)

3. After ALB stack completed and ClusterStack triggered in CloudFormation, Navigate to ECS Service.
    - Click to "Clusters" on the left pane.
    - You will see new ECS Cluster is being created.
    - Click to "Cluster" Name as highlighted in below screen.
{{% notice note %}}
Note that your container is going to be deployed into EC2 instance. This has been selected in deployment.json file in extract and containerize steps. You can change it to "FARGATE", if you dont want to control and manage underlying infrastructure and focus on your containerized workloads.
{{% /notice %}}
        ![deploy-cloudformation-1](/deploy-to-aws/net-deploy-2.png)

{{% notice note %}}
<a href="https://aws.amazon.com/fargate" target="_blank">AWS FARGATE</a> is a serverless compute engine for containers. With FARGATE, you don't need to provision and manage servers and you only pay for the resources required to run your containers.
{{% /notice %}}

4. Navigate to EC2 Service and locate the new Instance is being launched automatically. This instance is going to host your container and will be managed by Elastic Container Service (ECS).
    ![deploy-cloudformation-1](/deploy-to-aws/net-deploy-3.png)
5. After the Cluster is created. Under Services tab, you should see your application is deployed and status is "ACTIVE"
    - You can see the "Desired Tasks" and "Running Tasks" and Launch type are defined by App2container.
    - Notice that Launch type is EC2 for this deployment.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-4.png)
6. Click Tasks tab. Here you can see that Task has been created and running.
    - Click the Task Name to see its details.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-5.png)
7. When you are inside the Task, review the Task configurations.
    - Under Load Balancing section, you will see that Target Group has automatically been created for your container.
    - Click Target Group.
{{% notice note %}}
Notice that , VPC, subnet and Security Groups are also defined. You can update those based on your environment requirements.
{{% /notice %}}
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-7.png)
8. Inside the Target Groups, Review the Target Group configuration
    - <a href="https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html" target="_blank">Target Groups</a> are used to route requests to one or more registered targets. App2container creates Target Groups and registers it with your containers and Loadbalancer.
    - Click LoadBalancer
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-8.png)
9. When you are in Loadbalancers, Review the Loadbalancer configurations for your application.
    - Copy the Public DNS Name and paste it in your browser.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-9.png)

#### Congragulations

10. Congragulations!! You have containerized your application and deployed it into Amazon ECS.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-11.png)
11. Go to CloudFormation Service and notice the multiple stacks are created by App2container and all the required resources for your containerized application are automatically created for you.
    ![deploy-cloudformation-2](/deploy-to-aws/cloudformation-ecs-net-final.png)
12. Go to your Worker Machine  Console, You should see the below output stating that your application stack is created successfully!
![deploy-cloudformation-2](/deploy-to-aws/net-deploy-10.png)
