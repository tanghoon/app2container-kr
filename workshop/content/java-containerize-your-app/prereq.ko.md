+++
title = "App2Container 사전 요건"
weight = 20
pre = "<b>2.1 </b>"
+++

AWS App2Container를 처음 사용하려면 다음 필수 구성 요소를 완료해야 합니다. 이 실습에서는 모든 필수 구성 요소 도구를 사전 설치하고 이러한 필수 구성 요소와 호환되는 소스 환경을 만들었습니다.

[App2Container 사전요구사항](https://docs.aws.amazon.com/app2container/latest/UserGuide/start-containerize-java-app.html) 에서 확인할 수 있습니다.

App2Container 서비스를 설정하려면 다음 작업을 수행하십시오.


#### 1. IAM 유저에 사용 권한 부여

App2Container는 대부분의 명령을 실행하려면 AWS 서비스에 액세스해야 합니다. *app2container* 명령을 실행하려면 두 가지 매우 다른 사용 권한 세트가 필요합니다.

- 범용 사용자 또는 그룹은 --deploy 옵션과 함께 실행되는 명령을 제외하고 모든 명령을 실행할 수 있습니다.
- 배포를 위해 App2Container는 컨테이너 관리 서비스에 대한 AWS 개체(Amazon ECR(Amazon ECS 또는 Amazon EKS 포함)를 생성하거나 업데이트할 수 있어야 하며 AWS CodePipeline을 사용하여 CI/CD 파이프라인을 생성할 수 있어야 합니다. 이렇게 하려면 배포에만 사용해야 하는 높은 사용 권한이 필요합니다.

IAM Resource 구성의 권장 사항은 일반적인 용도로 생성하는 것을 추천합니다. App2Container를 사용하여 컨테이너를 배포하거나 파이프라인을 생성할 계획인 경우 권한을 높인 배포를 위해 별도의 IAM 리소스를 생성하는 것이 좋습니다.
{{% notice note %}}
이 워크샵에서는 App2 Container에서 지원하는 AWS 서비스를 사용하여 컨테이너형 애플리케이션을 배포할 수 있도록 관리자 권한을 가진 사용자를 생성하겠습니다.
{{% /notice %}}

1. "Services">"IAM"(ID 액세스 관리)을 클릭하여 **IAM 콘솔**로 이동하십시오.

    a. 왼쪽 창에서 Users > Add user를 누르십시오.

    b. 사용자 이름(예: app2container) 입력

    c. 'programmatic access'을 체크 합니다.

    d. next: permissions(다음: 권한)을 클릭합니다.

    ![create-iam-user](/setting-up-app2container/create-iam-user.png)

2. app2container 사용자에 대한 사용 권한을 설정합니다.

    a. "Attach existing policies directly" 를 선택합니다.
    b. "AdministratorAccess" 를 확인합니다. 
    c. "Next: Tag"를 클릭합니다.

    ![set-permissions](/setting-up-app2container/set-permissions.png)

3. 사용자 생성 및 확인

    a. “Next: Review”를 클릭 합니다. 사용자 생성 결과를 확인 합니다.

    b. 화면상에 표시된 Access key ID와 Secret Access Key 값을 카피하여 로컬 메모장에 저장 합니다.(이 값들은 다음 단계에서 사용 됩니다. )

    ![create-user](/setting-up-app2container/create-user.png)

이 단계에서는 사전 요구 사항에 따른 기본 설정을 수행 하였습니다. 다음 실습으로 이동할 준비가 되었습니다.
다음 실습에서는 App2Container를 설치하고 컨테이너화 프로세스를 시작하도록 설정합니다.
