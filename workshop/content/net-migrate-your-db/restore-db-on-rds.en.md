+++
title = "Restore DB on RDS"
weight = 30
pre = "<b>1.3 </b>"
+++



1. Navigate to RDS Service and select your Database you've created earlier.
2. In the Connectivity & Security tab, copy and note the Endpoint URL for your DB.
   ![6_db](/db-mig/net-create-db-5.png)
3. In your Source DB Server, Click Connect on the Up Left corner of the SQL Studio.
    ![6_db](/db-mig/net-restore-1.png)
4. In the connection window, Enter the below parameters:

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Server name                | RDS DB Endpoint URL    |
    | Authentication         | SQL Server Authentication |
    | Login      | unicorn (If you set a different user while creating your RDS DB, enter it here instead.)            |
    | Password      | awsrocks2021  (If you set a different password while creating your RDS DB, enter it here instead.)          |

    ![6_db](/db-mig/net-restore-2.png)

5. Once connected, you will see your RDS DB become visible on the left pane as below.
    ![6_db](/db-mig/net-restore-3.png)
6. Click Databases on the left pane and Click "New Query" as shown below.
    ![6_db](/db-mig/net-restore-8.png)
7. In the Query screen, paste the below command and Click "!Execute" button.
    - Replace the S3 bucket name below with your bucket name you've created in previous section.

{{% notice note %}}
If you defined different RDS DB name while creating earlier. Enter it in below command instead. (e.g. “unicornsql” )
{{% /notice %}}  


```powershell
exec msdb.dbo.rds_restore_database
    @restore_db_name='unicorn',
    @s3_arn_to_restore_from='arn:aws:s3:::<<insert name of your s3 bucket>>/MySiteDB.bak';
```

When you run the query, you should see the RESTORE_DB task has been created.
    ![6_db](/db-mig/net-restore-4.png)

8. Open another query screen and run the below command to check the status of the task. You should see the RESTORE_DB task has been success as shown below.

- If you see status as "in progress" execute the query again in few minutes.

  ```powershell
  exec msdb.dbo.rds_task_status @db_name='unicorn';
  ```

![6_db](/db-mig/net-restore-5.png)
9. On the left pane, you should see now unicorn DB has been restored from the backup file you've created.
![6_db](/db-mig/net-restore-6.png)

### Congratulations

You've successfully restored your source DB in your Target RDS Database. Now go to next section to update the Application connection strings to direct its traffic to the RDS Database.
