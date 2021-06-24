+++
title = "A.Java 모더나이제이션(현대화)"
weight = 4
+++

워크샵의 이 섹션에서는 Java 애플리케이션을 현대화합니다.

사용 중인 애플리케이션은 Tomcat 웹 서버의 Java 애플리케이션과 Postgre에 의해 구성됩니다. DB 서버의 경우 별도로 구성되어 실행되는 데이터베이스 입니다.

* 여러분은 현재 소스 Java Tomcat 애플리케이션에 대한 정보가 거의 없으며, 전체 실습이 종료 되면 아래 표시된 아키텍처의 형태로 배포합니다.

![ReferenceImage](/images/java-source-target.png)

이 워크샵에서는 AWS 마이그레이션 및 Modernization 툴을 사용하여 현대화 여정을 지원합니다.

1. 데이터베이스 이관 : Postgre 마이그레이션을 위해서 DMS(데이터베이스 마이그레이션 서비스)를 이용하여 AWS RDS(관계형 데이터베이스 서비스)로 이동합니다.
2. app2container를 이용한 어플리케이션 이관
   * 분석
   * 컨테이너화
   * 애플리케이션 배포 아티팩트 생성
   * 애플리케이션을 Amazon ECS(Elastic Container Servier)에 배포
   * CI/CD 파이프라인 생성 (선택 사항)
  
참고: 데이터베이스 마이그레이션 실습은 선택 사항입니다.

1. DMS(데이터베이스 마이그레이션 서비스)를 사용하여 데이터베이스를 마이그레이션하려면 다음 섹션으로 계속 진행 하십시오.
   
2. 또는, Java Containerize your Java App 실습을 진행하여 App2Container 프로세스를 실습 합니다.