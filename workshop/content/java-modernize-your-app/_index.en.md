+++
title = "Modernize your Java App"
weight = 20
pre = "<b>A. </b>"
+++

In this section of the workshop, you will modernize your Java Application.

You application is composed by a Java Application on Tomcat Web Server and a PostgreSQL Database running on separate DB Server.

- You do not have too much information about your source Java Tomcat application and at the end of this module you will deploy it into architecture shown below.
![source-target](/modernization/java-source-target.png)

in this workshop, you will use AWS migration and Modernization tools to help your modernization journey:

1. Database Migration Service (DMS) to migrate your PostgreSQL Database to AWS RDS (Relational Database Service).
2. App2Container to
    - Analyze;
    - Containerize;
    - Create Application's deployment artifacts;
    - Deploy your Application to Amazon ECS (Elastic Container Service);
    - Create CI/CD pipeline (Optional).

NOTE: Database Migration is optional in this module.

1. Please continue with the next section [Migrate Your Database]({{< ref "/java-migrate-your-db/_index.en.md" >}}) to migrate your Database using Database Migration Service (DMS)
2. Alternatively,  continue with [Containerize your Java App]({{< ref "/java-containerize-your-app/_index.en.md" >}})  section to start directly from App2Container processes.

Good Luck!
