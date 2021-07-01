+++
title = "데이터베이스 마이그레이션"
weight = 120
pre = "<b>- 1. </b>"
+++


이 섹션에서는 **기본 백업 및 복원** 방법을 사용하여 마이그레이션을 수행합니다. 소스 SQL 데이터베이스를 타겟 Amazon RDS for MSSQL 데이터베이스로 데이터베이스 마이그레이션을 수행합니다.

해당 작업은 동종의 데이터베이스 마이그레이션이기 때문에 (소스 및 대상 데이터베이스 엔진이 동일함) - 스키마 구조, 데이터 유형 및 데이터베이스 코드가 소스 및 대상 데이터베이스간에 호환되므로 마이그레이션 작업 수행시에는 스키마 변환이 필요하지 않습니다.

{{% notice note %}}
이 시나리오에서는 소스 데이터베이스의 실시간 복제가 필요하지 않으므로 DMS (데이터베이스 마이그레이션 서비스)가 사용되지 않습니다.
{{% /notice %}}

Below Diagram shows the high level steps when using Native Backup and Restore migrations.

<a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Procedural.Importing.html" target="_blank">기본 백업 및 복원</a>의 세부 정보 및 제약 사항을 확인 할 수 있습니다.

![1](/db-mig/net-db-migration-overview.png)

이 실습에서는 다음 단계를 수행합니다.

1. [타겟 DB 생성]({{< ref "/netCreate-target-DB.ko.md" >}})

2. [소스 DB 백업]({{< ref "backup-source-db.ko.md" >}})

3. [RDS에서 DB 복원]({{< ref "restore-db-on-rds.ko.md" >}})

4. [응용 프로그램에서 연결 문자열 (Connection Sting) 변경]({{< ref "change-conn-string.ko.md" >}})

5. [요약]({{< ref "Summary.ko.md" >}})
