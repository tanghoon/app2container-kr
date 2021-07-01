+++
title = "Java 앱 모더나이제이션"
weight = 20
pre = "<b>A. </b>"
+++

워크샵의 이 섹션에서는 Java 애플리케이션을 모더나이즈 (현대화) 합니다.

운영 중인 애플리케이션은 Tomcat 서버의 Java 애플리케이션과 PostgreSQL에 의해 구성 되었습니다. PostgreSQL DB 서버의 경우 별도로 구성되어 실행 되고 있습니다.

- 여러분은 현재 소스 Java Tomcat 애플리케이션에 대한 정보가 거의 없으며, 실습이 종료 되면 아래 표시된 아키텍처의 형태로 배포를 하게 합니다.
![source-target](/modernization/java-source-target.png)

이 워크샵에서는 다음의 AWS 마이그레이션 및 모더나이제이션 툴을 사용하여 현대화 여정을 지원합니다.

1. Postgre 마이그레이션을 위해서 Database Migration Service (DMS)를 이용하여 AWS RDS(관계형 데이터베이스 서비스)로 이동합니다. 
2. App2Container를 이용한 어플리케이션 이관
    - 분석;
    - 컨테이너화;
    - 애플리케이션 배포 아티팩트 생성;
    - 애플리케이션을 Amazon ECS(Elastic Container Servier)에 배포;
    - CI/CD 파이프라인 생성 (선택 사항).

NOTE: 데이터베이스 마이그레이션 실습은 선택 사항입니다.

1. 데이터베이스를 마이그레이션 하기 위해서는 [데이터베이스 이관]({{< ref "/java-migrate-your-db/_index.ko.md" >}}) 으로 이동하여 Database Migration Service (DMS)를 실습 합니다.
2. 또한,  [Java 앱 컨테이너화]({{< ref "/java-containerize-your-app/_index.ko.md" >}}) 로 이동하여 App2Container 프로세스를 실습 합니다.

감사합니다.
