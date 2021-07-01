+++
title = "Containerize your Java App"
weight = 40
pre = "<b>- 2. </b>"
+++

#### AWS App2Container Overview

<a href="https://aws.amazon.com/app2container" target="_blank">App2Container (A2C)</a> is a command line tool to help you lift and shift applications that run in your on-premises data centers or on virtual machines, so that they run in containers that are managed by Amazon ECS or Amazon EKS.

Moving legacy applications to containers is often the starting point toward application modernization. There are many benefits to containerization:

- Reduces operational overhead and infrastructure costs.
- Increases development and deployment agility.
- Standardizes build and deployment processes across an organization.

#### How AWS App2Container works

You can use App2Container to generate container images for one or more applications running on Windows or Linux. This includes commercial off-the-shelf applications (COTS). App2Container does not need source code for the application to containerize it.

You can use App2Container directly on the application servers that are running your applications. If your server does not meet the requirements to containerize your application and deploy it to AWS, or if you do not want to install the Docker engine on the application server, you can set up and use a worker machine.

![app2container-intro](/app2container/app2container-diagram.png)

App2Container performs following tasks in 4 main categories:

1. [Discover And Analyze]({{< ref "/java-containerize-your-app/java-discover-and-analyze.en.md" >}})

    - Creates an inventory list for the application server that identifies all running ASP.NET (Windows) and Java applications (Linux) that are candidates to containerize.
    - Analyzes the run-time dependencies of supported applications that are running, including cooperating processes and network port dependencies.
2. [Extract and Containerize]({{< ref "/java-containerize-your-app/java-extract-and-containerize.en.md" >}})
    - Extracts application artifacts for containerization and generates a Dockerfile.
    - Initiates builds for the application container.
3. [Create Deployment Artifacts]({{< ref "/java-containerize-your-app/java-create-deployment-artefacts.en.md" >}})
    - Generates the artifacts needed to deploy your application container in AWS.
    - Builds an AWS CloudFormation template to configure required compute, network, and security infrastructure to deploy containers using Amazon ECS or Amazon EKS.
    - Pre-fills key values in the artifacts based on your profile, the application analysis, and best practices.
4. [Deploy to AWS Cloud]({{< ref "/java-containerize-your-app/java-deploy-to-aws.en.md" >}})
    - Deploys the containers on Amazon ECS or Amazon EKS.
    - Optionally creates a CI/CD pipeline with AWS CodePipeline and associated services, to automate building and deploying your application containers.

{{% notice note %}}
**AWS App2Container** is offered at no additional charge. You are charged only when you use other AWS services to run your containerized application, such as Amazon ECR, Amazon ECS, and Amazon EKS.
{{% /notice %}}  

In this lab you will containerize a legacy Java application on Linux Ubuntu using App2Container, and deploy it on Amazon ECS (Elastic Container Service).

1. [App2Container Prerequisites]({{< ref "/prereq.en.md" >}})  
2. [Install & Initialize App2Container]({{< ref "/app2container-steps.en.md" >}})  
