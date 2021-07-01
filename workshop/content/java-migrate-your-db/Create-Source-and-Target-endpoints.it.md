+++
title = "Create Source and Target Endpoints"
weight = 30
pre = "<b>1.4 </b>"
+++




Go back to AWS Console, **AWS Database Migration Service** screen, click on **Endpoints** and  **Create endpoint** button.

### 1. Create the Source Endpoint

- Use the following parameters to configure the endpoint:

    | Parameter           | Value                                          |
    | ------------------- | ---------------------------------------------- |
    | Endpoint type       | Source endpoint                                |
    | Endpoint identifier | source-PostgreSQL-endpoint                                |
    | Source engine       | PostgreSQL                                          |
    | Server name         | 10.0.0.140                                            |
    | Port                | 5432                                           |
    | SSL mode            | none                                           |
    | User name           | unicorn                                 |
    | Password            | awsrocks2021                                   |
    | Database Name            | unicorn                                   |

    ![source-endpoint](/db-mig/source-endpoint-1.png)

    ![source-endpoint](/db-mig/source-endpoint-2-privateip.png)

    Open **Test endpoint connection (optional)** section, then in the **VPC** drop-down select **TargetVPC** and click the **Run test** button to verify that your endpoint configuration is valid.

    ![test-source-endpoint](/db-mig/source-endpoint-3.png)

    Test will run for a minute and you should see **successful** message in the **Status** column. Click on the **Create endpoint** button to create the endpoint.

    In case of any errors - make sure you've configured the endpoint parameters correctly and that the Replication Instance was created with **Publicly Accessible** parameter checked.

### 2. Create the Target Endpoint

- Repeat all steps to create the target endpoint with the following parameter values:

    | Parameter           | Value                                                 |
    | ------------------- | ----------------------------------------------------- |
    | Endpoint type       | Target endpoint                                       |
    | Select RDS DB instance | checked                                            |
    | RDS Instance        | Select your RDS instance from the drop-down (if it's not visible enter values manually)          |
    | Endpoint Identifier | unicorn                                       |
    | Target Engine       | PostgreSQL (will be pre-populated)                                                |
    | Server Name         | DNS name of your RDS database (leave the the pre-populated value)                             |
    | Port                | 5432     (will be pre-populated)                                             |
    | SSL mode            | none                                                  |
    | User name           | (leave the pre-populated value)                                                 |
    | Password            | Enter password you used when you creating the RDS database| (awsrocks2021)

![source-endpoint](/db-mig/target-endpoint-1.png)

- Select "Provide access information manually" box.
![source-endpoint](/db-mig/target-endpoint-2.png)

- Under **Test endpoint connection (optional)** select **TargetVPC** in the **VPC** drop down and click the **Run test** button to verify that your endpoint is valid.

![source-endpoint](/db-mig/target-endpoint-3.png)
    Test will run for a minute and in the end you should see **successful** message in the **Status** column. Click on the **Create endpoint** button to create the endpoint.

{{% notice note %}}
Additional endpoint connection tests can be performed from **Endpoints** list by clicking the **Actions** button and then **Test connection**.
{{% /notice %}}
