+++
title = "- 2. .NET 앱 컨테어너화"
chapter = true
weight = 70

+++
#### AWS App2Container Overview

[App2Container (A2C)](https://aws.amazon.com/app2container)는 Amazon ECS 또는 Amazon EKS에서 관리하는 컨테이너에서 실행되도록 온 프레미스 데이터 센터 또는 가상 머신에서 실행되는 애플리케이션을 리프트 및 쉬프트 하는 데 도움이되는 명령 줄 도구입니다.

레거시 애플리케이션을 컨테이너로 이동하는 것이 애플리케이션 모더나이제이션의 시작점 인 경우가 많습니다. 컨테이너화에는 많은 이점이 있습니다.

- 운영 오버 헤드 및 인프라 비용을 줄입니다.
- 개발 및 배포 민첩성을 향상시킵니다.
- 조직 전체의 빌드 및 배포 프로세스를 표준화합니다.

<img src="/images/d/app2container-diagram.png" alt="app2container-diagram.png" style="zoom:100%;" />

####  AWS App2Container 어떻게 작동할까요 ? (Worker Machine)

App2Container를 사용하여 OCI (Open Containers Initiative) 와 호환되는 Windows 또는 Linux 서버에서 실행되는 하나 이상의 애플리케이션에 대한 컨테이너 이미지를 생성 할 수 있습니다. 여기에는 상용 애플리케이션 (commercial off-the-shelf applications - COT)이 포함됩니다. App2Container는 애플리케이션이 컨테이너화하는 데 소스 코드가 필요하지 않습니다.

애플리케이션을 실행하는 애플리케이션 서버에서 App2Container를 직접 사용하거나 작업자 머신(Worker Machine)에서 컨테이너화 및 배포 단계를 수행 할 수 있습니다.

이 모듈에서는 작업자 머신에서 모든 컨테이너화 및 배포 단계를 수행합니다.

<img src="/images/d/app2container-remote-diagram.png" alt="app2container-remote-diagram.png" style="zoom:100%;" />

작업자 컴퓨터에서 App2Container를 사용하는 경우 4 가지 주요 범주에서 다음 작업을 수행합니다.

1. 원격 검색 및 분석
   - WinRM을 통해 작업자 컴퓨터에서 소스 서버로 보안 통신을 구축합니다.
   - Source Server를 원격으로 분석하고 작업자 머신 내부에서 분석 파일을 수집합니다.
   - 각 원격 소스 서버에 대해 작업자 컴퓨터 내에 폴더를 만듭니다.
   - 컨테이너화 후보 인 실행중인 모든 ASP.NET (Windows)을 식별하는 응용 프로그램 서버의 인벤토리 목록을 만듭니다.
   - 협력 프로세스 및 네트워크 포트 종속성을 포함하여 실행중인 지원되는 애플리케이션의 런타임 종속성을 분석합니다.
2. 원격 추출 및 컨테이너화
   - 컨테이너화를 위해 소스 서버에서 작업자 머신으로 애플리케이션 아티팩트를 추출합니다.
   - 작업자 컴퓨터에서 Dockerfile을 생성합니다.
   - 작업자 컴퓨터에서 애플리케이션 컨테이너에 대한 빌드를 시작합니다.
3. 배포 아티팩트 생성
   - Worker Machine에서 AWS에 애플리케이션 컨테이너를 배포하는 데 필요한 아티팩트를 생성합니다.
   - Amazon ECS 또는 Amazon EKS를 사용하여 컨테이너를 배포하는 데 필요한 컴퓨팅, 네트워크 및 보안 인프라를 구성하는 AWS CloudFormation 템플릿을 빌드합니다.
   - 프로필, 애플리케이션 분석 및 모범 사례를 기반으로 아티팩트의 키 값을 미리 채 웁니다.
4. AWS 클라우드에 배포
   - Amazon ECS 또는 Amazon EKS에 컨테이너를 배포합니다.
   - 선택적으로 AWS CodePipeline 및 관련 서비스를 사용하여 CI / CD 파이프 라인을 생성하여 애플리케이션 컨테이너 구축 및 배포를 자동화합니다.

{{% notice note%}}
AWS App2Container는 추가 비용없이 제공됩니다. Amazon ECR, Amazon ECS, Amazon EKS와 같은 컨테이너화 된 애플리케이션을 실행하기 위해 다른 AWS 서비스를 사용하는 경우에만 요금이 부과됩니다.
{{% /notice %}}

이 실습에서는 App2Container를 사용하여 Windows 2019 컨테이너 내에서 Windows 2012에서 실행되는 레거시 .NET 애플리케이션을 컨테이너화합니다.

업데이트 된 운영 체제를 사용하여 Amazon ECS (Elastic Container Service)에 배포합니다.

다음 단계에 따라 App2Container 환경 설정을 시작하십시오.

1. App2Container Prerequisites
2. Install & Initialize App2Container
