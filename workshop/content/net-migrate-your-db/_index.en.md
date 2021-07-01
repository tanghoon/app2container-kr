+++
title = "Migrate Your Database"
weight = 120
pre = "<b>- 1. </b>"
+++


In this section, you will perform a database migration from the source SQL database to a target Amazon RDS for MSSQL database with using **Native Backup and Restore** migration.

Since this is a homogeneous database migration (the source and target database engines are the same) - the schema structure, data types, and database code are compatible between the source and target databases, which means that this kind of migration doesn't require any schema conversion.

{{% notice note %}}
In this scenario, The source Database doesn't require any real time replication, thus, Database Migration Service (DMS) is not used for replication.
{{% /notice %}}

Below Diagram shows the high level steps when using Native Backup and Restore migrations.

You can check <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Procedural.Importing.html" target="_blank">using native backup and restore</a> for details and limitations.

![1](/db-mig/net-db-migration-overview.png)

In this lab, you will perform the following steps:

1. [Create Target DB]({{< ref "/netCreate-target-DB.en.md" >}})

2. [Backup Source DB]({{< ref "backup-source-db.en.md" >}})

3. [Restore DB on RDS]({{< ref "restore-db-on-rds.en.md" >}})

4. [Change Connection String at Application]({{< ref "change-conn-string.en.md" >}})

5. [Summary]({{< ref "Summary.en.md" >}})
