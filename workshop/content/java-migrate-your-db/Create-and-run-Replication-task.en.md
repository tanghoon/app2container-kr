+++
title = "Create and Run a Replication Task"
weight = 40
pre = "<b>1.5 </b>"
+++

### Configure and Run a Replication Task

Still in the **AWS DMS** console, go to **Database migration tasks** and click the **Create Task** button.

1. Enter the following parameter values in the **Create database migration task** screen:

    | Parameter              | Value                                               |
    | ---------------------- | --------------------------------------------------- |
    | Task identifier        | replication-cdc                                     |
    | Replication instance   | replication-instance                                |
    | Source endpoint        | source-postgre-endpoint                                     |
    | Target endpoint        | unicorn                                     |
    | Migration type         | Migrate existing data and replicate ongoing changes |

    ![Create-task-1](/db-mig/Create-task-1.png)

2. In the **Task settings** panel leave everything else as below:

    ![create-task-2](/db-mig/java-create-task-2.png)

3. In the **Table mappings** panel select **Wizard** mode, press the **Add new selection rule** button and Selet "Enter a schema".
    - Leave the default values.

    ![Create-task-3](/db-mig/Create-task-3.png)

4. Scroll to the bottom of the screen and click the **Create task** button to create the task and start the data replication.

While the task is running, you can review it's details to see information about tables and number of rows that are replicated.

![create-task-4](/db-mig/Create-task-5.png)
