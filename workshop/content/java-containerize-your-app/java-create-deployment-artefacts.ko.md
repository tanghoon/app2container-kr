+++
title = "배포 아티팩트 생성"
weight = 70
pre = "<b>2.5 </b>"
+++

이 단계에서 App2Container는 애플리케이션 분석 및 모범 사례를 기반으로 AWS에 애플리케이션 컨테이너를 배포하는 데 필요한 아티팩트를 생성합니다.

타겟 컨테이너 관리 환경(Amazon ECR 기반의 Amazon ECS 또는 Amazon EKS)에 배포하기 위한 두 가지 옵션이 있습니다.

{{% notice note %}}
이 시나리오에서는 애플리케이션 컨테이너를 Amazon ECS를 사용하여 Amazon Elastic Container Register(Amazon ECR)에 배포합니다.
{{% /notice %}}  

![app2container-intro](/create-deployment-artifacts/create-deployment-overview.png)

1. 다음 명령을 실행하여 AWS CloudFormation 템플릿을 생성합니다.

    ```bash
    app2container generate app-deployment --application-id <java-app-id>
    ```

2. 이 과정은 몇 분 정도 걸립니다. 아래 출력이 표시되어야 합니다.

    ```bash
    root@ip-10-0-0-129:/home/ubuntu# app2container generate app-deployment --application-id java-tomcat-21fb1eb0
    ✔ AWS prerequisite check succeeded
    ✔ Docker prerequisite check succeeded
    ✔ Created ECR Repository
    ✔ Registered ECS Task Definition with ECS
    ✔ Uploaded CloudFormation resources to S3 Bucket: app2container-bucket-0121
    ✔ Generated CloudFormation Master template at: /root/app2container/java-tomcat-6c144e52/EcsDeployment/ecs-master.yml
    👍 CloudFormation templates and additional deployment artifacts generated successfully for application java-tomcat-21fb1eb0

    💡 You're all set to use AWS CloudFormation to manage your application stack.
    ```

generate app-deployment 명령으로 App2container는 다음 단계를 수행합니다.

1. Amazon ECR 리포지토리를 생성하고 생성한 애플리케이션 컨테이너 이미지를 해당 리포지토리에 복사합니다.
   - AWS 콘솔에 로그인하고 Elastic Container Service(ECS) 서비스로 이동하여 이를 확인합니다.
     - 왼쪽 창에서 "Amazon ECR"을 클릭합니다.
     - 컨테이너가 이 ECR 리포지토리에 자동으로 등록된 것을 볼 수 있습니다.
     ![ecr-1](/create-deployment-artifacts/deployment-ecr.png)
     - 리포지토리를 클릭하면 "latest" 태그가 있는 컨테이너 이미지가 배포된 것을 볼 수 있습니다
     ![ecr-2](/create-deployment-artifacts/deployment-ecr-2.png)
{{% notice note %}}
"analysis.json" 파일에서 이 태그 및 기타 컨테이너 파라미터 변수를 업데이트할 수 있습니다.
{{% /notice %}}  
2. ECS 작업 정의를 생성하고 ECS에 등록합니다.
    - NOTE: ECS에서 도커 이미지를 실행하려면 ECS 작업 정의가 필요합니다. 작업 정의에 대한 자세한 내용은 <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html" target="_blank">Amazon ECS Task Definitions</a>에서 확인할 수 있습니다.
    ![ecs-task-definiton](/create-deployment-artifacts/deployment-task-definition.png)
3. 선택한 S3 버킷에 모든 CloudFormation 리소스를 업로드합니다
    - NOTE: 사전에 [app2container 설치 및 초기화]({{< ref "/app2container-steps.ko.md" >}})에서 정의 하였습니다.
    - NAWS S3 콘솔로 이동하여 생성한 버킷을 클릭합니다.
    - Y모든 배포 아티팩트가 이 버킷에 업로드된 것을 확인 할 수 있습니다.
        ![ecs-task-definiton](/create-deployment-artifacts/deployment-s3-bucket.png)
4. CloudFormation 템플릿을 생성 합니다.
    - App2Container는 환경을 검토하고 이전 단계에서 정의한 구성, 애플리케이션 분석 결과로 CloudFormation 템플릿을 업데이트합니다.
    - CloudFormation 템플릿은 필요한 모든 리소스와 해당 종속성을 설명하므로 스택으로 함께 시작하고 구성할 수 있습니다.
    - 웹 서버의 "app2container generate app-deployment" 명령 출력에서 ​​ecs-master.yml 파일 위치를 찾아 파일을 오픈 합니다.

{{% notice note %}}
지금 이 파일을 변경할 필요는 없지만 요구 사항에 따라 사용자 지정할 수 있습니다.
{{% /notice %}}  

```bash
cat /root/app2container/<java-app-id>/EcsDeployment/ecs-master.yml
```

축하합니다! 이제 Java 애플리케이션을 AWS ECS에 배포할 준비가 모두 완료되었습니다! 다음 섹션을 계속 진행하여 배포를 수행 합니다.
