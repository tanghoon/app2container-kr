+++
title = "Backup Source DB"
weight = 30
pre = "<b>1.2 </b>"
+++

Using native .bak files to back up and restore databases is usually the fastest way to back up and restore databases.

In this section, you will backup your source Database and upload it into S3 bucket which you've created earlier.

1. Connect to your Source-NET-DB Server as explained in [Connecting your Windows Instances]({{< ref "/connect-windows.it.md" >}}) section.
    - You can find the Source DB Server by navigating to EC2 service as shown below. Source DB server name is "Source-NET-DBServer"
   ![6_db](/db-mig/net-db-ec2.png)
{{% notice note %}}
Ensure to get the DB server password as described in [Connecting your Windows Instances]({{< ref "/connect-windows.it.md" >}}) section.
{{% /notice %}}
2. Once you log in, From Start menu, click "Microsoft SQL Server Management Studio"

   ![6_db](/db-mig/net-sql-1.png)
3. In the connection window, Enter the below parameters:

| Parameter           | Value                    |
| ------------------- | ------------------------ |
| Server name                | .    |
| Authentication         | SQL Server Authentication |
| Login      | sa            |
| Password      | password-1            |

   ![6_db](/db-mig/net-sql-2.png)
4. Once you are connected to the Database engine, Right Click to "MySiteDB" under Databases and select "Backup" as shown below.
   ![6_db](/db-mig/net-sql-3.png)
5. On the Backup settings screen, ensure that you've selected Backup type "Full" and take note the Backup file location. You will use this location in the next step.
   ![6_db](/db-mig/net-sql-4.png)
6. Once backup completed, you will get the notification as below:
   ![6_db](/db-mig/net-sql-5.png)
7. Now, you have the backup file, but in order to upload the backup file to S3 bucket you will need to configure your windows server AWS credentials.

8. Run a Windows Powershell terminal from start menu and run the "aws s3 cp" command to copy the backup file to S3 bucket. Replace your S3 bucket name in the below command.

   {{% notice note %}}
   awscli has already been installed in this machine and you will use the ec2 instance role to access to S3 bucket.You can check <a href="https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html" target="_blank">AWS CLI details</a> for real life integrations.
   {{% /notice %}}

   ```powershell
   aws s3 cp “C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\MySiteDB.bak” s3://<<insert name of your s3 bucket>>
   ```

9. Once you run the command, navigate to your S3 bucket, you should see the MySiteDB.bak file uploaded into this bucket.
   ![6_db](/db-mig/net-sql-6.png)
