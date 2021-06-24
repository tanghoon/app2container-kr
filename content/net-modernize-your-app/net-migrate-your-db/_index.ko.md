+++
title = "1. 데이터베이스 마이그레이션"
weight = 60

+++
이 섹션에서는 기본 백업 및 복원 마이그레이션을 사용하여 원본 SQL 데이터베이스를 대상 Amazon RDS for MSSQL 데이터베이스로 데이터베이스 마이그레이션을 수행합니다.

이것은 동종 데이터베이스 마이그레이션이기 때문에 (소스 및 대상 데이터베이스 엔진이 동일함) - 스키마 구조, 데이터 유형 및 데이터베이스 코드가 소스 및 대상 데이터베이스간에 호환되므로 이러한 종류의 마이그레이션에서는 스키마 변환이  필요하지 않습니다.

{{% notice note%}}
이 시나리오에서는 원본 데이터베이스의 실시간 복제가 필요하지 않으므로 DMS (데이터베이스 마이그레이션 서비스)가 사용되지 않습니다.
{{% /notice %}}

아래 다이어그램은 기본 백업 및 복원 마이그레이션을 사용할 때 높은 수준의 단계를 보여줍니다.

[기본 백업 및 복원](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Procedural.Importing.html)의 세부 정보 및 제약 사항을 확인할 수 있습니다.

![db-migration](/images/net-db-migration-overview.png)

이 실습에서는 다음 단계를 수행합니다.

1. 타겟 DB 생성

2. 소스 DB 백업

3. RDS에서 DB 복원

4. 응용 프로그램에서 연결 문자열 (Connection Sting) 변경

5. 요약

