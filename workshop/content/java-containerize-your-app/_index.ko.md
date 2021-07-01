+++
title = "Java 앱 컨테이너화"
weight = 40
pre = "<b>- 2. </b>"
+++

#### AWS App2Container 개요

<a href="https://aws.amazon.com/app2container" target="_blank">App2Container (A2C)</a> 는 사내 데이터 센터 또는 가상 시스템에서 실행되는 애플리케이션을 리프트&쉬프트 방식으로 Amazon ECS 또는 Amazon EKS에서 관리되는 컨테이너에서 실행할 수 있도록 지원하는 커맨드라인 도구입니다.

기존 애플리케이션을 컨테이너로 이동하는 것이 애플리케이션 현대화의 출발점이 되는 경우가 많습니다. 컨테이너화에는 다음과 같은 많은 이점이 있습니다.

- 운영 오버헤드 및 인프라 비용 절감
- 개발 및 구축 민첩성을 높입니다.
- 조직 전반의 구축 및 구축 프로세스 표준화 합니다.

#### AWS App2Container 작동 방식

App2Container를 사용하여 윈도우즈 또는 Linux에서 실행 중인 하나 이상의 애플리케이션에 대한 컨테이너 이미지를 생성할 수 있습니다. 여기에는 상용 기성 애플리케이션(COTS)이 포함됩니다. App2Container는 애플리케이션이 컨테이너를 컨테이너에 포함하기 위해 소스 코드가 필요하지 않습니다.

애플리케이션을 실행하는 애플리케이션 서버에서 App2Container를 직접 사용할 수 있습니다. 서버가 애플리케이션을 컨테이너화하여 AWS에 배포하기 위한 요구 사항을 충족하지 않거나 애플리케이션 서버에 Docker 엔진을 설치하지 않으려는 경우 작업자 시스템을 설정하여 사용할 수 있습니다.

![app2container-intro](/app2container/app2container-diagram.png)

App2Container는 4가지 주요 카테고리에서 다음 작업을 수행합니다.

1. [검색 및 분석]({{< ref "/java-containerize-your-app/java-discover-and-analyze.ko.md" >}})

    - 컨테이너화할 후보인 실행 중인 모든 ASP.NET(Windows) 및 Java 응용 프로그램(Linux)을 식별하는 응용 프로그램 서버에 대한 인벤토리 목록을 만듭니다.
    - 협력 프로세스 및 네트워크 포트 종속성을 포함하여 실행 중인 애플리케이션의 런타임 종속성을 분석합니다.
2. [추출 및 컨테이너화]({{< ref "/java-containerize-your-app/java-extract-and-containerize.ko.md" >}})
    - 컨테이너화를 위한 애플리케이션 아티팩트를 추출하고 Docker 파일을 생성합니다.
    - 애플리케이션 컨테이너에 대한 빌드를 시작합니다.
3. [배포 아티팩트 생성]({{< ref "/java-containerize-your-app/java-create-deployment-artefacts.ko.md" >}})
    - 애플리케이션 컨테이너를 AWS에 배포하는 데 필요한 아티팩트를 생성합니다.
    - AWS CloudFormation 템플릿을 빌드하여 Amazon ECS 또는 Amazon EKS를 사용하여 컨테이너를 배포하는 데 필요한 컴퓨팅, 네트워크 및 보안 인프라를 구성합니다.
    - 프로필, 애플리케이션 분석 및 모범 사례에 따라 아티팩트의 주요 값을 미리 채웁니다.
4. [AWS 클라우드 배포]({{< ref "/java-containerize-your-app/java-deploy-to-aws.ko.md" >}})
    - Amazon ECS 또는 Amazon EKS에 컨테이너를 배포합니다.
    - 필요한 경우 AWS CodePipeline 및 관련 서비스를 사용하여 CI/CD 파이프라인을 생성하여 애플리케이션 컨테이너 구축 및 구축을 자동화합니다.

{{% notice note %}}
**AWS App2Container**는 추가 비용 없이 제공됩니다. 다른 AWS 서비스를 사용하여 Amazon ECR, Amazon ECS 및 Amazon EKS와 같은 컨테이너형 애플리케이션을 실행하는 경우에만 요금이 청구됩니다.
{{% /notice %}}  

이 실습에서는 App2Container를 사용하여 Linux Ubuntu에서 레거시 Java 애플리케이션을 포함하고 Amazon ECS(Elastic Container Service)에 배포합니다.

1. [App2Container 사전 요건]({{< ref "/prereq.ko.md" >}})  
2. [App2Container 설치 및 초기화]({{< ref "/app2container-steps.ko.md" >}})  
