+++
title = "Cleanup"
weight = 300
pre = "<b></b>"
+++

For the self-paced lab, at the end make sure to delete _all_ resources that you've created, including:

1. In <a href="https://us-west-2.console.aws.amazon.com/rds/home?region=us-west-2#databases:" target="_blank">AWS RDS Console</a>
   - Delete created **databases** (without final snapshot and without retaining automated backups)
    ![RDS Confirm Deletion](/cleanup/db-delete-confirm.en.png)

2. In <a href="https://us-west-2.console.aws.amazon.com/dms/v2/home?region=us-west-2#replicationInstances" target="_blank">AWS DMS Console</a>
   - Stop and (when it's in Stopped state) - delete created **tasks**. *Wait for the task to be deleted*.
   - Delete created **endpoints**. *Wait for endpoints to be deleted*.
   - Delete created **replication instances**. *Wait for the replication instance to be deleted*.
   - Delete created **subnet group**

     Go to <a href="https://us-west-2.console.aws.amazon.com/dms/v2/home?region=us-west-2#dashboard" target="_blank">AWS DMS Dashboard</a> to confirm that everything was deleted (assuming you didn't use DMS before, you should see 0's everywhere, like on the screenshot below).
     ![DMS Dashboard confirmation](/cleanup/dms-dashboard-final.en.png)

3. In <a href="https://us-west-2.console.aws.amazon.com/efs/home?region=us-west-2" target="_blank">Amazon EFS Console</a>
   - Delete created **Elastic File Systems**

4. In <a href="https://us-west-2.console.aws.amazon.com/ecs/home?region=us-west-2#/getStarted" target="_blank">AWS ECS</a>
   - Delete created **services**
   - Delete created **task definitions**
   - Delete created **clusters**
   - Delete created **ECR registries**

5. In <a href="https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#Home:" target="_blank">AWS EC2 Console</a>
   - Terminate created **EC2 machines**
   - Delete created **Load balancers**

6. In <a href="https://s3.console.aws.amazon.com/s3/" target="_blank">AWS S3</a>
   - Delete created **S3 buckets**

7. In <a href="https://console.aws.amazon.com/iam/" target="_blank">AWS IAM console</a>
   - Delete created **IAM users/roles**

8. In <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks" target="_blank">AWS CloudFormation</a>
   - Delete the **app2container workshop** stack

When the **App2ContainerWorkshop** stack is deleted, review your AWS Account and **delete all remaining resources that were created during this lab**.

Additionally we would appreciate if you would provide <a href="https://eventbox.dev/survey/QRHSVTI">anonymous feedback for this workshop</a>.
