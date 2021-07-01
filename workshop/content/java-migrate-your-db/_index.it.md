+++
title = "Migrate Your Database"
weight = 30
pre = "<b>- 1. </b>"
+++


In this section, you will perform a database migration using the source PostgreSQL database to a target Amazon RDS for PostgreSQL database with **AWS Database Migration Service**. Since this is a homogeneous database migrations (the source and target database engines are the same) - the schema structure, data types, and database code are compatible between the source and target databases, which means that this kind of migration doesn't require any schema conversion.

You can check <a href="https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.PostgreSQL.html#CHAP_Source.PostgreSQL.Homogeneous" target="_blank">Migrating from PostgreSQL to PostgreSQL using AWS DMS</a> for details and limitations.

![1](/db-mig/DMS-overview.png)

You can learn more details about this service by watching the video below.

<center><iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/zb4GcjEdl8U" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

In this lab, you will perform the following steps:

1. [Create Target DB]({{< ref "/Create-target-DB.it.md" >}})

2. [Create Replication Instance]({{< ref "Create-Replication-instance.it.md" >}})

3. [Create Source and Target Endpoints]({{< ref "Create-Source-and-Target-endpoints.it.md" >}})

4. [Configure Source Database]({{< ref "configure_source_database.it.md" >}})

5. [Create and Run Replication Task]({{< ref "Create-and-run-Replication-task.it.md" >}})

6. [Summary]({{< ref "Summary.it.md" >}})
