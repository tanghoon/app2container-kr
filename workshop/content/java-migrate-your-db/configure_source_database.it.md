+++
title = "Review Source Database Configuration"
weight = 25
pre = "<b>1.3 </b>"
+++

{{% notice warning %}}
This section is optional and for informational only. All settings are already updated for this workshop for you. But you can sill login to DB server and review below listed the configurations.
{{% /notice %}}  

### Run DMS Replication Task with Change Data Capture (CDC)

To ensure minimal downtime for the database migration, you're going to use continuous replication of changes (also known as **Change Data Capture (CDC)**) from the source database to the target database. For more information about CDC and native CDC support of **AWS DMS** see <a href="https://aws.amazon.com/blogs/database/aws-dms-now-supports-native-cdc-support/" target="_blank">this article</a>.

#### Updating Source PostgreSQL's configurations

For **AWS DMS** continuous replication from PostgreSQL database, you need to update the following parameters in the postgresql.conf configuration file.

- Set wal_level = logical

- Set max_replication_slots to a value greater than 1.
  - The max_replication_slots value should be set according to the number of tasks that you want to run. For example, to run five tasks you need to set a minimum of five slots. Slots open automatically as soon as a task starts and remain open even when the task is no longer running. You need to manually delete open slots.

- Set max_wal_senders to a value greater than 1.
  - The max_wal_senders parameter sets the number of concurrent tasks that can run.

- Set wal_sender_timeout =0
  - The wal_sender_timeout parameter terminates replication connections that are inactive longer than the specified number of milliseconds. Although the default is 60 seconds, we recommend that you set this parameter to zero, which disables the timeout mechanism.

--------------

You can review the PostgreSQL configuration settings with below command after you've logged into DB Server.

1. Get the source Java Source Database Server information. You can find this information from <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank">CloudFormation</a>, <br> output section.
    for **self-paced lab** use below parameters from the **Output** section of the **App2ContainerWorkshop**

    | Parameter              | Value                                               |
    | ---------------------- | --------------------------------------------------- |
    | Public DNS Name        | JavaDBServerDNSName                                    |
    | Username   | ubuntu                                |
    | SSHKey  | SSHKeyURL      (You need to create .pem file from this key)                          |

{{% notice note %}}
For SSH key, you need to create a .pem file via copying the key from the URL to your text editor and save it as .pem file.
{{% /notice %}}  

2. Connect to the **Source Java DB Server** where your Source Database runs.

    check <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html" target="_blank">this article</a> for more detail on how to connect to your Linux instance.

3. Run the below command to go through the postgresql configuration file.

    ```bash
    cat /etc/postgresql/10/main/postgresql.conf
    ```
