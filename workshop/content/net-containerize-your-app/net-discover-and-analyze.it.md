+++
title = "Discover And Analyze"
weight = 140
pre = "<b>2.3 </b>"
+++

Your App2container process starts with discovering the running .NET applications in your server by creating application inventory and then analyzing them.
![app2container-discover](/discover-and-analyze/discover-overview-diagram.png)

#### 1. Create Application Inventory

As you are using Worker Machine for all your App2container process, you will use "remote" process. Once you run the remote commands from your worker machine, it will connect with your source server and run the commands on it.

{{% notice note %}}
Using Worker Machine with the remote connectivity enables you to standardize your scaling activities.
{{% /notice %}}

1. Run the "app2container remote inventory" command as follows to list the .NET applications that are running on your server.

   - use your "Source Server" private IP address with the "target" parameter below.

      ```powershell
      app2container remote inventory --target <private-IP of your source server>
      ```

   - Once you run the command, you will see "Remote inventory retrieved successfully" output as below.

      ![net-analysis](/discover-and-analyze/net-inventory-1.png)

2. App2container creates a separate folder for each "Target" source server in worker machine.

   - Navigate to "C:\Users\Administrator\AppData\Local\app2container\remote\" folder as below.
   - You should see a folder named with your "source-server" IP address.

      ![net-analysis](/discover-and-analyze/net-inventory-2.png)

{{% notice note %}}
Appdata folder is a hidden folder by default. If you can't see this folder, Select "View" > and Check "Hidden items" box. Or Navigate to "C:\Users\Administrator\AppData\" folder directly. 
{{% /notice %}}

3. Inside the folder, you should see "inventory.json" file which app2container retrieved the .NET application inventory from your source server remotely.
   ![net-analysis](/discover-and-analyze/net-inventory-3.png)

4. Open the "inventory.json" file with notepad. The file includes a JSON object collection with one entry for each application.

   - Each application object will include key/value pairs and will start with the .NET-app-id as shown below (iis-default-web-site-f5342b48512c).
   - Copy the application-id to your notepad as you will use it in next steps.
      ![net-analysis](/discover-and-analyze/net-inventory-4.png)

#### 2. Analyze your Application

1. Locate the application ID for the application in the JSON output of the inventory command and then run the "app2container remote analyze --application-id \<net-app-id\> --target \<source-server-private-IP\>" command as follows, replacing net-app-id with the application ID that you copied in the previous step.

   ```powershell
   app2container remote analyze --application-id <net-app-id> --target <source-server-private-IP>
   ```

2. Output of the "Analyze" command provides what tasks has been completed by app2container and recommendations for the next steps.

   ![net-analysis](/discover-and-analyze/net-analyze-1.png)

3. With Analyze command, App2container creates a folder for each application. Locate it as below and go inside the folder.

   ![net-analysis](/discover-and-analyze/net-analyze-2.png)

4. Locate the analysis.json file inside the folder and  open the file with notepad for review.

Once you open the file, you will see that analysis.json file has 2 sections: EDITABLE and NON-EDITABLE.

   - EDITABLE section includes container Parameters where you can specify your preferred settings like ContainerBaseImage or image tag. These parameters will be used during containerization process later. Notice that, In this exercise, Container base image will be upgraded from Windows 2012 to Windows 2019.

{{% notice note %}}
Capability to upgrade the Operating System is one of the best benefits of containerization process. With App2container, this process could be handled at scale.
{{% /notice %}}

   ![net-analysis](/discover-and-analyze/net-analyze-3.png)

   - NON-EDITABLE section includes application level analysis information which app2container will use during containerization like OS data, ports in use, dependencies, software libraries etc..

{{% notice note %}}
This analysis is especially important when working with legacy applications or where there is not enough application documentation.
{{% /notice %}}

With this step, you completed the analysis and can continue with containerization of your application in the next section.
