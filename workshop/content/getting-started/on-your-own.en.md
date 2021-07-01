+++
title = "Running the workshop on your own"
weight = 20
pre = "<b>- </b>"
+++

{{% notice warning %}}
Complete this section ONLY if you are running the workshop on your own. If you are at an AWS hosted event (such as re:Invent, GameDay, Workshop, or any other event hosted by an AWS employee), go to [Attending an AWS hosted event]({{< ref "/hosted.md" >}}).
{{% /notice %}}

### Self-paced learning environment

This lab assumes you have access to an **AWS Account** with <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html" target="_blank">Administrator privileges</a>. To create a new AWS Accounts please follow <a href="https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/" target="_blank">How do I create and activate a new Amazon Web Services account?</a> article.

Remember to [cleanup]({{< ref "/cleanup/_index.en.md" >}}) your AWS account after running the lab, to avoid unnecessary charges!

   {{% notice note %}}
This workshop designed to work on us-west-2 region. Please ensure you selected this region.
  {{% /notice %}}  

#### Deployment

1. Click on the button below <a href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=App2ContainerWorkshop&templateURL=https://app2container-workshop-artifacts.s3-us-west-2.amazonaws.com/app2container_workshop_template.yml" target="_blank"><img src="https://application-migration-with-aws-workshop.s3-us-west-2.amazonaws.com/static/cloudformation-launch-stack.png"></a>

2. On the **Step 1 - Specify template** confirm that the URL <https://app2container-workshop-artifacts.s3-us-west-2.amazonaws.com/app2container_workshop_template.yml> is entered in the **Amazon S3 URL** field and press **Next**
  ![CloudFormation Step 1](/intro/cloudformation-step1.en.png)

3. On the **Step 2 - Specify stack details** screen make sure App2ContainerWorkshopp is entered in the **Stack name** field and press **Next**
  {{% notice note %}}
  You can add your Public IP address in "YourIPAddress" section to limit the access only your IP address. (e.g. x.x.x.x/32). The default value (0.0.0.0/0) will allow the traffic for all IPs.
  {{% /notice %}}  
  ![CloudFormation Step 2](/intro/cloudformation-step2.en.png)

4. On the **Step 3 - Configure stack options** screen don't make any changes, just press **Next**  

5. On the **Step 4 - Review** screen, scroll to the bottom of the page and check all checkboxes, as on the screenshot below, then press **Next** for the template to be deployed.  
  ![CloudFormation Step 4](/intro/cloudformation-step4.en.png)

{{% notice note %}}
If the deployment fails, go to deployment stack **Events** tab and confirm the root cause. A common root cause is **ecsExecutionRole** or **ecsAutoscaleRole** IAM roles already exist in the AWS account. Delete them and re-run deployment of the source environment CloudFormation template.
{{% /notice %}}

When the template is in the **CREATE_COMPLETE** status, you can find information about created source environment by going to **AWS Console -> CloudFormation**, selecting  **App2ContainerWorkshop** stack and selecting to the **Outputs** tab. You will see information like on the screenshot below.

![Source Environment Information in AWS Console](/intro/self-service-env-awsconsole-info.en.png)

- Copy - paste this information to your local notepad, as you will use them during the workshop.

### Great! You are all set to go

Now, choose your application (Java or .NET) and start modernizing