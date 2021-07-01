+++
title = "Attending an AWS hosted event"
weight = 40
pre = "<b>- </b>"
+++

If you are attending an AWS hosted event, follow the below checks before starting your workshop to ensure your workshop environment is deployed successfully.

{{% notice warning %}}
Please disconnect from VPN before starting to workshop as Some VPNs may block the access to public URLs.
{{% /notice %}}  

1. Login to your team Dashboard via using Hash URL shared with you before the event.
    - You should see the below screen as your Team Dashboard.

    ![team](/intro/team-dashboard.png)

2. Login to your AWS Console from your Team Dashboard.

3. Update your Team Name by clicking "Set Team Name" button in your Team Dashboard.

4. In AWS hosted events, Workshop environment is already deployed for you as Cloud Formation Stack.
    - Go to CloudFormation from services and check a Cloudformation stack is deployed (CREATE_COMPLETE) as shown below.

    ![team](/intro/check-stack.png)

    - Click the created stack and Select  **Outputs** tab. You will see information like on the screenshot below.

    ![Source Environment Information in AWS Console](/intro/self-service-env-awsconsole-info.en.png)

    - Copy - paste this information to your local notepad or keep this page open in a separate browser tab, as you will use them during the workshop.

5. Navigate to EC2 service, check that 5 instances are deployed and Running as shown below.

![team](/intro/check-instances.png)

{{% notice note %}}
If you see any issues at one of these steps, reach out to your AWS Event Operator for resolution.
{{% /notice %}}  

### Great! You are all set to go

Now, choose your application (Java or .NET) and start modernizing
