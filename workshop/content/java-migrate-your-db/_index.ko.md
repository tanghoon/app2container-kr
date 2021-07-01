+++
title = "데이터베이스 마이그레이션"
weight = 30
pre = "<b>- 1. </b>"
+++

이 섹션에서는 **AWS Database Migration Service** 를 사용하여 PostgreSQL DB의 소스 환경에서 Amazon RDS PostgreSQL 데이터베이스로 데이터베이스 마이그레이션을 수행 합니다.

해당 작업은 동종의 데이터베이스 마이그레이션이기 때문에 (소스 및 타겟 데이터베이스 엔진이 동일함) - 스키마 구조, 데이터 유형 및 데이터베이스 코드가 소스 및 타겟 데이터베이스간에 호환되므로 마이그레이션 작업 수행시에는 스키마 변환이  필요하지 않습니다.

여러분은 <a href="https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.PostgreSQL.html#CHAP_Source.PostgreSQL.Homogeneous" target="_blank">AWS MDS를 사용하여 PostgreSQL 에서 PortgreSQL로 마이그레이션 수행</a>에서 보다 자세한 사항과 제약점을 확인 하실수 있습니다.

![1](/db-mig/DMS-overview.png)

아래 비디오를 통해 이 서비스에 대한 자세한 내용을 확인할 수 있습니다.

<center><iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/zb4GcjEdl8U" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

이 실습에서는 다음 단계를 수행합니다.

1. [타겟 DB 생성]({{< ref "/Create-target-DB.ko.md" >}})

2. [복제 인스턴스 생성]({{< ref "Create-Replication-instance.ko.md" >}})

3. [소스 / 타겟 엔드포인트 생성]({{< ref "Create-Source-and-Target-endpoints.ko.md" >}})

4. [소스 데이터베이스 구성 검토]({{< ref "configure_source_database.ko.md" >}})

5. [복제 타스크 생성 및 실행]({{< ref "Create-and-run-Replication-task.ko.md" >}})

6. [요약]({{< ref "Summary.ko.md" >}})
