+++
title = "Create Target DB"
weight = 15
pre = "<b>1.1 </b>"
+++


In this scenario, you will use "Native Backup Restore" migration approach for your source SQL server.

As the prerequisites, you require a network connectivity between your Source and Target environments.
In order to replicate the real life scenarios, in this workshop, we built VPC peering for you between Source and Target environments.

{{% notice note %}}
Required Security Groups for your environment are created for you in this workshop.
{{% /notice %}}
![Replication Instance Architecture](/db-mig/vpc-peering.png)

To set up for native backup and restore, you need three components:

1. An Amazon S3 bucket to store your backup files.
2. An AWS Identity and Access Management (IAM) role to access the bucket.
3. The SQLSERVER_BACKUP_RESTORE option added to an option group on your DB instance.

### 1. Create S3 Bucket to Store your backup files

- Navigate to the S3 console and enter a unique name for your bucket (rds-backup-your initials-date) and click create and store the name of the bucket in a notepad (this will be required in subsequent steps)

 ![1](/db-mig/net-create-s3-rds.png)

### 2. Create Option Group for target Database

To enable native backup and restore on your DB instance, you add the SQLSERVER_BACKUP_RESTORE option to an option group on your DB instance. For more information and instructions, see <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.SQLServer.Options.BackupRestore.html" target="_blank">Support for native backup and restore in SQL Server</a>.

1. Navigate to the RDS console
2. Select **Option groups** from the menu on the left and click **Create Group**
3. Give a name (e.g. unicorndb-sql-option) and give the description as the same.
4. From the **Engine**, select **sqlserver-ex**
5. From "Major Engine Version", select "13.00"
6. Click Create.
    ![1](/db-mig/net-option-1.png)
7. From the Option Groups, select your created Option Group and click "Add option" as shown below.
    ![1](/db-mig/net-option-2.png)
8. In the Add option screen, from drop down menu, select "SQLSERVER_BACKUP_RESTORE" option.
9. In the IAM role section, select "Create a new role" and give a name for the role (e.g. SqlBackupRestoreIamRole)
10. In the S3 Destination section,  select your S3 bucket (rds-backup--) which you've created in previous steps from the menu as shown below.
11. Click "Add Option".
    ![1](/db-mig/net-option-3.png)

### Create the subnet group for target database

{{% notice note %}}
If you created subnet group already in Java section, you can use the same Subnet Group.
{{% /notice %}}

1. Go to the **AWS Console**, from **Services** choose **RDS**, select **Subnet groups** from the menu on the left and click **Create DB Subnet Group**

2. On the **Create DB subnet group** enter the following information

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Name                | database-subnet-group     |
    | Description         | Subnets where RDS will be deployed |
    | VPC      | TargetVPC            |

    In the **Add subnets** panel add one subnet from each Availability Zone (us-west-2a and us-west-2b) with CIDRs 10.1.101.0/24 and 10.1.201.0/24, then press **Create** button.

    ![RDS Subnet group creation](/db-mig/net-db-subnet-group.png)

### Create the target database

1. Now select **Databases** from the menu on the left and click **Create database**

2. From the **Engine options**, select Microsoft SQL Server and select the version details as below:

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Edition                | SQL Server Express Edition     |
    | Version         | SQL Server 2016 13.00.5820.21.v1 |

    ![1](/db-mig/net-create-db-1.png)

3. In the **Template** section select "Free Tier".

    ![Free tier template selection](/db-mig/db-template.png)

    {{% notice note %}}
Choosing of "Free Tier" template limits your options in the next steps of the wizard, so that you stay within the limits of AWS Free Tier.
{{% /notice %}}

4. In the **Settings** section, configure the DB instance identifier (unicorn), Master username (unicorn) and Master password (awsrocks2021) for your new database instance.

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | DB Instance Identifier                | unicorn (enter "unicornsql" if you created unicorn DB in Java section already)     |
    | Master Username         | unicorn |
    | Master password         | awsrocks2021 |

    ![3_db](/db-mig/db-create-2.png)

{{% notice note %}}
Make sure to write down **Master username** and **Master password**, as you will use it later.
{{% /notice %}}

5. Select **db.t2.micro** from the Burstable DB instance class,  **General Purpose (SSD)** for Storage Type and uncheck "Enable storage autoscaling" (we don't need more than 20 GB of storage for this database).

    ![4_db](/db-mig/4_db.png)

{{% notice note %}}
For production workloads, we recommend enabling the standby instance to enable <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html" target="_blank">Multi-AZ Deployment</a> for higher availability.
{{% /notice %}}

6. In the **Connectivity** section:
    - In **Virtual Private Cloud (VPC)**, select **TargetVPC** (this is the <a href="https://aws.amazon.com/vpc/" target="_blank">Amazon Virtual Private Cloud</a> that was automatically created for this lab)
        ![6_db](/db-mig/db-create-3.png)
    - In **Additional connectivity configuration -> VPC Security Group**, select **Choose existing** VPC security group and choose "SQLRDSSG" security group.
        Ensure Database port is set to 1433.
    - Note that the DB Subnet group you have created earlier will be automatically selected
        ![6_db](/db-mig/net-create-db-2.png)
{{% notice note %}}
Note: SQLRDSSG Security Group is already created for you and allows traffic from your Source Database and Source Web Server.
{{% /notice %}}
7. From Additional configuration section,  select the Option group ('unicorndb-sql-option-group') which you've created earlier.
8. Uncheck Backup, Performance Insights and Monitoring boxes as shown below.
   ![6_db](/db-mig/net-create-db-3.png)

9. Finally, review the **Estimated monthly costs** and click the **Create database** button.

   ![8_2_db](/db-mig/8_2_db.png)

10. Creating the Database will take few mins. You can check it is status from "Databases" section. Once it is ready, you should see it's status as "Available" as shown below.
    - Click DB name as shown below.

    ![6_db](/db-mig/net-create-db-4.png)

11. In the Connectivity & Security tab, copy and note the Endpoint URL for your DB. You will use this endpoint URL to connect your database in the following sections.
   ![6_db](/db-mig/net-create-db-5.png)
