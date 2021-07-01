+++
title = "Change DB Endpoint at Application"
weight = 60
pre = "<b>1.6 </b>"
+++

At this stage, you've successfully created a new PostGreSQL Database on Amazon RDS and migrated your existing Database and set your ongoing replication settings.

Now, you will change the Database endpoint configuration settings on your Source Java Application server so that your application starts using the new migrated database.

1. Go to RDS Service, and click "Databases" from the right pane.
    - Select your RDS database "unicorn" which you've created earlier.
    - From the "Connectivity & Security" tab, copy Endpoint URL to your notepad. You will use this endpoint in the next steps.

    ![source-endpoint](/db-mig/java-rds-endpoint.png)

2. Connect to the **Source-Java-Webserver** where your Java application runs.

   You have multiple options to connect your Web Server. Check <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html" target="_blank">this article</a> for more detail on how to connect to your Linux instance.

    - You can connect to your Web server via using AWS Systems Manager - <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html" target="_blank">Session Manager</a>. Session Manager provides secure and auditable instance management without the need to open inbound ports, maintain bastion hosts, or manage SSH keys.
       
        - Go to your Amazon EC2 instances, select your Java Web Server and Click "Connect".
        - Choose "Session Manager" and click "Connect"
       
          ![create-s3-bucket](/setting-up-app2container/session-manager.png)

    **OR**

    - If you prefer to SSH into the webserver, you would need the following details.

        a. Public DNS Name of the webserver <br>
        b. Username configured in the webserver <br>
        c. SSHKey <br>

        You can find this information in the output section of the cloudformation stack. The stack can be accessed by navigating to the <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank">CloudFormation</a> console.

        | Parameter              | Value                                               |
        | ---------------------- | --------------------------------------------------- |
        | Username   | ubuntu                                |
        | SSHKey  | SSHKeyURL      (You need to create .pem file from this key)                          |
        | Public DNS Name        | JavaWebServerDNSName                                    |

  {{% notice note %}}
  For SSH key, you need to create a .pem file via copying the key from the URL to your text editor and save it as .pem file.
  {{% /notice %}}  

3. Your application gets the Database connection configurations from your server environment variables. Open the environment file under /etc folder and update the "RDS_HOSTNAME" with the RDS endpoint you've copied earlier.

    ```bash
    sudo su
    nano /etc/environment
    ```

    - Once you open the file, you will see that RDS_HOSTNAME is already set to "10.0.0.140" which is source DB private IP address. Change the parameter with RDS endpoint and save and exit from the file.
    ![source-endpoint](/db-mig/java-env-var.png)

    - After the changes, exit from the file with "ctrl+x" and type "y" and hit enter to save the file with the same name.

4. In order to make environment variable changes active, run the below command.

    ```bash
    source /etc/environment
    ```

5. Now, restart your Tomcat Webserver to trigger the changes on your web application with below commands.

    ```bash
    cd /usr/local/tomcat8/bin/
    ./shutdown.sh
    ./startup.sh
    ```

    - you should see the below output on your terminal.

    ![source-endpoint](/db-mig/java-start-tomcat.png)

6. Go to <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank">App2Container Cloudformation Stack</a>, and copy "JavaWebApplicationURL" parameter from the **Output** section of the **App2ContainerWorkshop**

7. Open the browser and paste your "JavaWebApplicationURL".

    - You should see your Web application running successfully as below.

        ![source-endpoint](/db-mig/java-app-url.png)

    - Navigate to "BROWSE MOVIES" tab, you should see the list of unicorn movies.

        ![source-endpoint](/db-mig/java-url-movies.png)

#### Congratulations

You have successfully migrated your Database and your Web Application is now using the migrated RDS Database.
