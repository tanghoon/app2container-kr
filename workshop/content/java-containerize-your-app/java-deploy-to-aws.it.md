+++
title = "Deploy to AWS Cloud"
weight = 80
pre = "<b>2.6 </b>"
+++

![app2container-intro](/deploy-to-aws/Deploy-to-AWS-overview.png)

At this stage:

- Your application container image is already present in ECR (Elastic Container Registry) and ECS Task definition is created and registered.
- App2container created the Master Cloudformation template for you to automatically deploy and configure all required resources in AWS.

In this section, you will launch your Cloudformation template and review the deployment process in AWS console.

1. In your web server, run suggested command from "app2container generate app-deployment" output. Update the \<java-app-id\> with your ID.

    ```bash
    aws cloudformation deploy --template-file /root/app2container/<java-app-id>/EcsDeployment/ecs-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<java-app-id>-ECS
    ```

    - When you run this command, it will create a Cloudformation stack named as "a2c-<java-app-id>-ECS".

    {{% notice note %}}
    The process will take few mins. Go back to AWS Console to review the background activities during the deployment.
    {{% /notice %}}

    ```bash
    root@ip-10-0-0-202:/home/ubuntu# aws cloudformation deploy --template-file /root/app2container/<java-app-id>/EcsDeployment/ecs-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<java-app-id>-ECS

    Waiting for changeset to be created..
    Waiting for stack create/update to complete
    ...
    ```

2. Navigate to the <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks" target="_blank"> Cloudformation console</a>
    - You will see that a stack has been created with name "a2c-java-tomcat-21fb1eb0-ECS".
    - Notice Nested stacks are triggered by main stack for each deployment. E.g. A separate stacks for ALB (Application LoadBalancer) and ECS Clusters.
    - You can see the triggered events under "events" tab for each stack.
    ![deploy-cloudformation-1](/deploy-to-aws/deploy-cloudformation-1.png)

3. After ALB stack and Cluster stack have been completed, Navigate to ECS Service.
    - Click to "Clusters" on the left pane.
    - You will see new ECS Cluster is being created.
    - Click to "Cluster" Name as highlighted in below screen.
    {{% notice note %}}
    Notice that your container is deployed into FARGATE. This has been selected in deployment.json file in extract and containerize steps. You can change it to "EC2", if you prefer to have the control on the host servers and manage them.
    {{% /notice %}}
        ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-1.png)

    {{% notice note %}}
    <a href="https://aws.amazon.com/fargate" target="_blank">AWS FARGATE</a> is a serverless compute engine for containers. With FARGATE, you dont need to provision and manage servers and you only pay for the resources required to run your containers.
    {{% /notice %}}

4. After the cluster is created, under Services tab, you will see your application deployed and service status is “ACTIVE"
    - You can see the values of "Desired Tasks" and "Running Tasks" and Launch type are defined by App2container
    ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-2.png)
5. Click Tasks tab. Here you can see that Task has been created and is running.
    - Click on the Task to see its details.
    ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-3.png)
6. Click Clusters and when you are inside the clusters, Click Services tab and go into Service.
    - While you are inside the Service.
    - Under Load Balancing section, you will see that Target Group has automatically been created for your container.
    - Click Target Group.
{{% notice note %}}
Notice that , VPC, subnet and Security Groups are also defined. You can update those based on your environment requirements.
{{% /notice %}}
    ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-4.png)
7. Inside the Target Groups, Review the Target Group configuration
    - <a href="https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html" target="_blank">Target Groups</a> are used to route requests to one or more registered targets. App2container creates Target Groups and registers it with your containers and Loadbalancer.
    - Click LoadBalancer
    ![deploy-cloudformation-2](/deploy-to-aws/loadbalancer-1.png)
8. In the Loadbalancers section, Review the Loadbalancer configurations for your application.
    - Copy the Public DNS Name and paste it in your browser.
    ![deploy-cloudformation-2](/deploy-to-aws/loadbalancer-2.png)

#### Congratulations

9. Congratulations!! You have containerized your application and deployed it into Amazon ECS.
    ![deploy-cloudformation-2](/deploy-to-aws/java-last.png)
10. Go to CloudFormation Service and notice the multiple stacks are created by App2container and all the required resources for your containerized application are automatically created for you.
    ![deploy-cloudformation-2](/deploy-to-aws/cloudformation-ecs-final.png)
11. Go to your Source WebServer Console, You should see the below output stating that your application stack is created successfully!
![deploy-cloudformation-2](/deploy-to-aws/stack-final.png)
