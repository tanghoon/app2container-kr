+++
title = "App2Container Prerequisites"
weight = 20
pre = "<b>2.1 </b>"
+++

AWS App2Container requires below prerequisites completed before you use it for the first time.
In this lab, we have pre-installed all the prerequisites tools and created the source environment compatible with these prerequisites.

You can check the prerequisites check-list from [App2Container Prerequisites](https://docs.aws.amazon.com/app2container/latest/UserGuide/start-containerize-java-app.html)

As the next step, please follow the below tasks to set-up your App2Container service.

#### 1. Grant permissions to IAM Users

App2Container needs access to AWS services in order to run most of its commands. There are two very different sets of permissions needed to run *app2container* commands.

- The general purpose user or group can run all of the commands except commands that are run with the --deploy option.
- For deployment, App2Container must be able to create or update AWS objects for container management services (Amazon ECR with Amazon ECS or Amazon EKS), and to create CI/CD pipelines with AWS CodePipeline. This requires elevated permissions that should only be used for deployment.

We recommend that you create general purpose IAM resources, and if you plan to use App2Container to deploy your containers or create pipelines, that you create separate IAM resources for deployment which has elevated rights.
{{% notice note %}}
For this workshop, we will create a user with the administrator rights so it can deploy a containerized application using the AWS services for deployment that are supported by App2Container.
{{% /notice %}}

1. **Navigate to the IAM Console by clicking “Services” > “IAM” (Identity Access Management)**

    a. From the left pane, click Users > Add user

    b.  Provide a User name, e.g. app2container

    c.  Check the 'programmatic access'

    d. click Next: Permissions

    ![create-iam-user](/setting-up-app2container/create-iam-user.png)

2. Set permissions for your app2Container user.

    a.  Select "Attach existing policies directly"
    b.  Check "AdministratorAccess"
    c. Click "Next: Tags"

    ![set-permissions](/setting-up-app2container/set-permissions.png)

3. Review and Create user

    a. Select “Next: Review” and then click Create user

    b. On the Success screen, find the Access key ID and Secret Access Key and copy them down (you’ll use them in the following steps).

    ![create-user](/setting-up-app2container/create-user.png)

Congratulations! With this step, you set up your environment prerequisites and you are ready for to go to the next section. In the next section, you will install App2Container and set it up to start your containerization process.
