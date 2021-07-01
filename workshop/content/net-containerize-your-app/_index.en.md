+++
title = "Containerize your .NET App"
weight = 130
pre = "<b>- 2. </b>"
+++

#### AWS App2Container Overview

<a href="https://aws.amazon.com/app2container" target="_blank">App2Container (A2C)</a> is a command line tool to help you lift and shift applications that run in your on-premises data centers or on virtual machines, so that they run in containers that are managed by Amazon ECS or Amazon EKS.

Moving legacy applications to containers is often the starting point toward application modernization. There are many benefits to containerization:

- Reduces operational overhead and infrastructure costs.
- Increases development and deployment agility.
- Standardizes build and deployment processes across an organization.

![app2container-intro](/app2container/app2container-diagram.png)

#### How AWS App2Container works (Worker Machine)

You can use App2Container to generate container images for one or more applications running on Windows or Linux servers that are compatible with the Open Containers Initiative (OCI). This includes commercial off-the-shelf applications (COTs). App2Container does not need source code for the application to containerize it.

You can use App2Container directly on the application servers that are running your applications, or perform the containerization and deployment steps on a worker machine.

In this module you will perform all containerization and deployment steps from a worker machine.

![app2container-intro](/app2container/app2container-remote-diagram.png)
When using App2Container on worker machine, it performs following tasks in 4 main categories:

1. [Remote Discover And Analyze]({{< ref "/net-containerize-your-app/net-discover-and-analyze.en.md" >}})
    - Builds a secure communication to Source Server from Worker machine via WinRM.
    - Analyzes the Source Server remotely and collects analysis files inside the worker machine.
    - Creates a folder inside the worker machine for each remote Source Server.
    - Creates an inventory list for the application server that identifies all running ASP.NET (Windows)  that are candidates to containerize.
    - Analyzes the run-time dependencies of supported applications that are running, including cooperating processes and network port dependencies.
2. [Remote Extract and Containerize]({{< ref "/net-containerize-your-app/net-extract-and-containerize.en.md" >}})
    - Extracts application artifacts for containerization from source server to Worker machine.
    - In Worker machine, generates a Dockerfile.
    - In Worker machine, Initiates builds for the application container.
3. [Create Deployment Artefacts]({{< ref "/net-containerize-your-app/net-create-deployment-artefacts.en.md" >}})
    - In Worker Machine, generates the artifacts needed to deploy your application container in AWS.
    - Builds an AWS CloudFormation template to configure required compute, network, and security infrastructure to deploy containers using Amazon ECS or Amazon EKS.
    - Pre-fills key values in the artifacts based on your profile, the application analysis, and best practices.
4. [Deploy to AWS Cloud]({{< ref "/net-containerize-your-app/net-deploy-to-aws.en.md" >}})
    - Deploys the containers on Amazon ECS or Amazon EKS.
    - Optionally creates a CI/CD pipeline with AWS CodePipeline and associated services, to automate building and deploying your application containers.

{{% notice note %}}
**AWS App2Container** is offered at no additional charge. You are charged only when you use other AWS services to run your containerized application, such as Amazon ECR, Amazon ECS, and Amazon EKS.
{{% /notice %}}  

In this lab you will containerize your legacy .NET application running on Windows 2012 within Windows 2019 container using App2Container.

With the updated Operating System, you will deploy it on Amazon ECS (Elastic Container Service).

Follow the below steps to start setting up your App2Container environment.

1. [App2Container Prerequisites]({{< ref "/netprereq.en.md" >}})  
2. [Install & Initialize App2Container]({{< ref "/netapp2container-steps.en.md" >}})  
