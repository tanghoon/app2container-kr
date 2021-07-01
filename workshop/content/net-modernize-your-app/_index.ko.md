+++
title = ".NET 앱 모더나이제이션"
weight = 110
pre = "<b>B. </b>"
+++

이 섹션에서는 .NET 애플리케이션을 모더나이즈(현대화) 합니다.

.NET 응용 프로그램이 Windows 20212 R2 Server의 IIS (인터넷 정보 서비스) 웹 서버에서 실행되고 있습니다. 애플리케이션은 별도의 서버에서 실행되는 SQL 서버와 통신합니다.

- 소스 .NET 애플리케이션에 대한 충분한 정보는 제공되지 않습니다. 실습이 완료되면 아래에 표시된 아키텍처 상에 애플리케이션을  배포합니다.
![source-target](/modernization/net-source-target.png)

이 워크숍에서는 AWS 마이그레이션 및 모더나이제이션 도구와 기본 데이터베이스 도구를 사용하여 모더나이제이션 과정을 지원합니다.

1. 기본 백업 및 복원 마이그레이션 접근 방식을 통한 MSSQL 데이터베이스를 AWS RDS (관계형 데이터베이스 서비스)로 마이그레이션을 수행합니다.
2. App2Container 를 통해 다음을 수행 합니다.
    - 분석
    - 컨테이너화
    - 애플리케이션 배포 아티팩트 생성
    - Amazon ECS (Elastic Container Service)에 애플리케이션 배포
    - CI / CD 파이프 라인을 생성 (선택 사항)

{{% notice note %}}
이 워크숍에서는 <a href="https://docs.microsoft.com/en-us/windows/win32/winrm/portal" target="_blank">WinRM</a> (Windows 원격 관리)을 통해 소스 서버와 통신하는 모든 App2Container 작업을  Worker Machine에서 수행합니다.
{{% /notice %}}  

Windows 머신 로그인 상세 정보는 .NET 섹션의  [Windows 인스턴스에 연결]({{< ref "/connect-windows.ko.md" >}}) 문서를 참고하세요.

NOTE: 이터베이스 마이그레이션은 실습 과정중에서 선택 사항입니다.

1. 기본 백업 및 복원 마이그레이션 접근 방식을 사용하여 [데이터베이스 마이그레이션]({{< ref "/net-migrate-your-db/_index.ko.md" >}})을 수행 하려면 다음 섹션인 데이터베이스 마이그레이션을 계속하십시오.
2. 또는 .[NET 앱 컨테이너화 섹션]({{< ref "/net-containerize-your-app/_index.ko.md" >}})을 진행하여 App2Container 프로세스에서 직접 시작하십시오.
