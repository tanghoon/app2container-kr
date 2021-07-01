+++
title = "Create Target DB"
weight = 15
pre = "<b>1.1 </b>"
+++

### Database Migration

Database migrations can be performed in a number of ways, and for the purpose of this workshop we will perform a **continuous data replication** migration using <a href="https://aws.amazon.com/dms/" target="_blank">AWS Database Migrations Service (DMS)</a>.

Before you configure **AWS DMS**, you will need to create your target database in the AWS account provided. Use **AWS Relation Database Service (RDS)** to perform this activity making it easy to set up, operate, and scale a relational database in the cloud.

As the prerequisites, you require a network connectivity between your Source and Target environments.
In order to replicate the real life scenarios, in this workshop, we built VPC peering for you between Source and Target environments.

{{% notice note %}}
Required Security Groups for your environment are created for you in this workshop.
{{% /notice %}}
![Replication Instance Architecture](/db-mig/vpc-peering.png)

### Create the subnet group for target database

1. Go to the **AWS Console**, from **Services** choose **RDS**, select **Subnet groups** from the menu on the left and click **Create DB Subnet Group**

2. On the **Create DB subnet group** enter the following information

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Name                | database-subnet-group     |
    | Description         | Subnets where RDS will be deployed |
    | VPC      | TargetVPC            |

    In the **Add subnets** panel add one subnet from each Availability Zone (us-west-2a and us-west-2b) with CIDRs 10.1.101.0/24 and 10.1.201.0/24, then press **Create** button.

    ![RDS Subnet group creation](/db-mig/net-db-subnet-group.png)

### Create Parameter Group for target Database

In PostgreSQL, foreign keys (referential integrity constraints) are implemented using triggers. During the full load phase, AWS DMS loads each table one at a time. Recommendations are that you disable foreign key constraints during a full load, using one of the following methods:

- Temporarily disable all triggers from the instance, and finish the full load
- Use the **session_replication_role** parameter in PostgreSQL.

In this workshop, we will update  **session_replication_role** parameter to "replica". For this, we will create a new parameter Group for the target DB and apply those parameters while creating the target RDS DB.

You can check <a href="https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Target.PostgreSQL.html" target="_blank">Using a PostgreSQL database as a target</a> for details.

1. Select **Parameter groups** from the menu on the left and click **Create Parameter Group**

2. From the **Parameter group family**, select postgres10
3. Give a name  & description (e.g. unicorn-db-parameter-group) and click created.

    ![1](/db-mig/db-parameter-group.png)

4. From the list click "unicorn-db-parameter-group" which you've just created
5. In search box, search for "session_replication_role" and select the parameter and edit its value to "replica"
6. Click Save changes.

    ![1](/db-mig/db-parameters-2.png)

### Create the target database

1. Now select **Databases** from the menu on the left and click **Create database**

2. From the **Engine options**, select PostgreSQL and Version PostgreSQL 10.15-R1

    ![1](/db-mig/db-create-1.png)

{{% notice note %}}
You can confirm the source PostgreSQL version from the source database by typing: **psql -V**
{{% /notice %}}

In the **Template** section select "Free Tier".

![Free tier template selection](/db-mig/db-template.png)

{{% notice note %}}
Choosing "Free Tier" ensures the template limits your options in the next steps of the wizard, so that you stay within the limits of AWS Free Tier.
{{% /notice %}}

In the **Settings** section, configure the DB instance identifier (unicorn), Master username (unicorn) and Master password (awsrocks2021) for your new database instance.

![3_db](/db-mig/db-create-2.png)

{{% notice note %}}
Make sure to write down **Master username** and **Master password**, as you will use it later.
{{% /notice %}}

Select **db.t2.micro** from the Burstable DB instance class,  **General Purpose (SSD)** for Storage Type and uncheck "Enable storage autoscaling" (we dont need more than 20 GB of storage for this database).
![4_db](/db-mig/4_db.png)

3. For the **Availability & durability**, keep the **Do not create a standby instance** option selected.

    ![5_db](/db-mig/5_db.png)

{{% notice note %}}
For production workloads, we recommend enabling the standby instance to enable <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html" target="_blank">Multi-AZ Deployment</a> for higher availability.
{{% /notice %}}  

4. In the **Connectivity** section:

    - In **Virtual Private Cloud (VPC)**, select **TargetVPC** (this is the <a href="https://aws.amazon.com/vpc/" target="_blank">Amazon Virtual Private Cloud</a> that was automatically created for this lab)
    ![6_db](/db-mig/db-create-3.png)
    - In **Additional connectivity configuration -> VPC Security Group**, select **Choose existing** VPC security group and choose "PGRDSSG" security group which was created for you.
    Ensure Database port is set to 5432.
    - Note that the DB Subnet group you have created earlier will be automatically selected

    ![6_db](/db-mig/java-db-create-4.png)

5. For the **Database authentication**, choose **Password authentication**.

    ![6_2_db](/db-mig/6_2_db.png)

6. From Additional configuration section, Enter initial database name ('unicorn') and select the DB parameter group ('unicorn-db-parameter-group') which you've created earlier. Uncheck Backup, Performance Insights, Monitoring boxes.
    ![6_db](/db-mig/db-create-5.png)

7. Finally, review the **Estimated monthly costs** and click the **Create database** button.

   ![8_2_db](/db-mig/8_2_db.png)
