+++
title = "Change Connection String at Application"
weight = 40
pre = "<b>1.4 </b>"
+++


At this stage, you've successfully created a new Microsoft SQL Database on Amazon RDS and migrated your existing Database.

Now, you will change the Database endpoint configuration settings on your Source .NET Application server so that your application starts using the new migrated database.

1. Connect to your Source .NET Web Server as explained in [Connecting your Windows Instances]({{< ref "/connect-windows.it.md" >}}) section.
    - You can find the Source .NET Web Server by navigating to EC2 service as shown below. Source NET Web server name is "Source-NET-WebServer"
   ![6_db](/db-mig/net-web-ec2.png)

2. Your .NET application gets the Database connection configurations from "web.config" file located inside the application folder.

    - Navigate to the application folder at "c:\Unicornshop" and open the Web.config file as shown below.

   ![6_db](/db-mig/net-web-config.png)

3. Once you open the file, navigate to "\<connectionStrings\>" section.
     - you will see that DB parameters are already set with Source DB paramenters.
   ![6_db](/db-mig/net-web-config-2.png)

4. Update the parameters with the RDS Target DB values.

   | Parameter           | Value                    |
   | ------------------- | ------------------------ |
   | server                | Your RDS endpoint URL    |
   | database         | unicorn |
   | user id      | unicorn (If you set a different user while creating your RDS DB, enter it here instead.)             |
   | password      | awsrocks2021  (If you set a different password while creating your RDS DB, enter it here instead.)          |

   - You should have the similar settings as shown below:

      ![6_db](/db-mig/net-web-config-3.png)

5. Now, Navigate to Cloudformation and copy NETWebAppURL (as shown below) and paste it into your browser. You should see your Web Application working with your new migrated database.

   ![6_db](/db-mig/net-web-url.png)

#### Congratulations

   ![6_db](/db-mig/net-web-last.png)

You have successfully migrated your Database and your Web Application is now using the migrated RDS Database.
