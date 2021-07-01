+++
title = "CI/CD 파이프라인 생성"
weight = 90
pre = "<b>2.7 </b>"
+++

이전 단계에서는 애플리케이션을 분석하고 App2Container로 컨테이너화 한 다음 Amazon Elastic Container Service (ECS) 및 Amazon Elastic Container Registry (ECR)에 배포했습니다.

App2Container는 아래 스크린 샷과 같이 컨테이너화 된 애플리케이션을위한 CI / CD 파이프 라인을 생성하는 기능도 제공합니다.

![app2container-discover](/create-pipeline/create-pipeline-overview.png)

이전 단계에서 "app2container generate app-deployment"명령을 실행하면 [배포 아티팩트 생성]({{< ref "java-containerize-your-app/java-create-deployment-artefacts.ko.md" >}}) 섹션에서 App2Container는 애플리케이션 폴더 아래에 pipeline.json 파일을 생성합니다.

#### 1. pipeline.json 준비

1. 소스 웹 서버에서 애플리케이션 폴더로 이동하여 pipeline.json 파일을 오픈 합니다.
    - 아래의 명령어를 수행 합니다. \<java-app-id\>는 환경에 맞게 아래 경로를 업데이트하십시오.

    ```bash
    nano /root/app2container/<java-app-id>/pipeline.json
    ```

    - app2container가 일부 속성 (예 : repositoryName, 이미지)을 채웠음을 알 수 있습니다.
    - eleaseInfo 섹션에서 컨테이너를 배포 할 클러스터 또는 서비스를 정의 할 수 있습니다. 또한 "Beta"또는 "Prod"릴리스로 정의 할 수 있습니다.

    ![app2container-discover](/create-pipeline/pipeline-json-1.png)

    이 시나리오에서는 이전 단계에서 만든 기존 클러스터 및 서비스에 컨테이너를 배포합니다. 따라서, 실습자들은 아래의 내용을 입력 해야 합니다.

    - ClusterName
    - ServiceName
    - beta 버전 활성화 ( 속성값을 true로 설정 합니다.)

2. AWS 콘솔로 이동하여 환경에서 ClusterName 및 ServiceName을 수집합니다.

    - ECS 서비스로 이동하여 아래와 같이 clusterName을 메모장에 복사합니다.
    ![ecs-cluster-1](/deploy-to-aws/ecs-cluster-1.png)
    - 이제 서비스를 클릭하고 화면 상단에서 서비스 이름을 복사하십시오.
    ![app2container-discover](/create-pipeline/ecs-service-name.png)

3. 수집된 클러스터명과 서비스명을 기준으로 pipeline.json 파일로 이동하여 업데이트합니다.

    - 아래에 표시된대로 ClusterName, ServiceName을 붙여넣고 beta 버전 활성화 ( "true"로 설정)합니다.
    ![app2container-discover](/create-pipeline/pipeline-json-2.png)

4. "ctrl+x"로 파일을 종료하고 "y"를 입력하고 Enter 키를 눌러 동일한 이름으로 파일을 저장합니다.

#### 2. Pipeline 생성 ( Generate )

이제 파이프 라인 설정이 정의되었으며 CI/CD 파이프 라인을 생성 할 준비가 되었습니다.

1. 소스 웹 서비스에서 다음 명령을 조정하여 CI/CD 파이프 라인 CloudFormation 템플릿을 생성합니다.

```bash
app2container generate pipeline --application-id <java-app-id>
```

이 명령으로 app2container는 다음을 생성 합니다.

- CodeCommit 리포지토리
- 모든 buildspec 파일
- CloudFormation 템플릿 생성
- CodeCommit 리포지토리에 파일 커밋

그리고 AWS 환경에서 전체 CI/CD 파이프 라인을 생성 하기 위한 CloudFormation 템플릿을 생성합니다.

```bash
root@ip-10-0-0-202:~/app2container/java-tomcat-21fb1eb0# app2container generate pipeline --application-id java-tomcat-21fb1eb0
✔ Created CodeCommit repository
✔ Generated buildspec file(s)
✔ Generated CloudFormation templates
✔ Committed files to CodeCommit repository
👍 Pipeline resource template generation successful for application java-tomcat-21fb1eb0

💡 You're all set to use AWS CloudFormation to manage your pipeline stack.

Next Steps:
1. Edit the CloudFormation template as necessary.
2. Create a pipeline stack using the AWS CLI or the AWS Console. AWS CLI command:  

 aws cloudformation deploy --template-file /root/app2container/java-tomcat-21fb1eb0/Artifacts/Pipeline/CodePipeline/ecs-pipeline-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-java-tomcat-21fb1eb0-ecs-pipeline-stack
```

#### 3. Pipeline 생성 ( Create )

1. 웹 서버에서 "app2container generate pipeline" 명령의 결과 화면에서 권고하는 "aws cloudformation deploy"명령을 실행합니다.
    - 이 명령을 사용하면 환경에 필요한 모든 AWS 리소스를 생성하고 구성 할 CloudFormation 스택이 배포됩니다.
    - 명령은 다음과 같습니다.

    ```bash
    aws cloudformation deploy --template-file /root/app2container/java-tomcat-21fb1eb0/Artifacts/Pipeline/CodePipeline/ecs-pipeline-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-java-tomcat-21fb1eb0-ecs-pipeline-stack
    ```

    - 이 명령을 실행하면 "pipeline-stack"으로 끝나는 Cloudformation 스택이 생성됩니다.

    {{% notice note %}}
이 프로세스는 몇 분 정도 소요됩니다. 배포 중 백그라운드 작업을 검토하려면 AWS 콘솔로 돌아갑니다.
    {{% /notice %}}  

    ```bash
    root@ip-10-0-0-202:~/app2container/java-tomcat-21fb1eb0# aws cloudformation deploy --template-file /root/app2container/<java-app-id>/Artifacts/Pipeline/CodePipeline/ecs-pipeline-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<java-app-id>-ecs-pipeline-stack

    Waiting for changeset to be created..
    Waiting for stack create/update to complete
    ...
    ```

2. AWS Console에서 <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank"> Cloudformation Stack</a> 서비르소 이동하여 "Stacks"를 클릭 합니다.
    - 스택이“a2c-java-tomcat-21fb1eb0-ecs-pipeline-stack”이라는 이름으로 생성 된 것을 볼 수 있습니다.
    - Nested 스택은 각 배포의 기본 스택에 의해 수행 됩니다.
    - 각 스택의 "이벤트" 탭에서 트리거된 이벤트를 볼 수 있습니다.
    ![deploy-cloudformation-1](/create-pipeline/pipeline-cf.png)

3. eveloper Tools에서 CodePipeline 서비스로 이동합니다.
    - 파이프 라인이 생성되는 것을 볼 수 있습니다.
    - 파이프 라인을 클릭 합니다.
    ![deploy-cloudformation-1](/create-pipeline/codepipeline-1.png)
4. 파이프 라인을 클릭하면 수명주기와 상태를 검토하게됩니다.
    - 베타 릴리스가 파이프 라인에 있습니다.
    ![deploy-cloudformation-1](/create-pipeline/pipeline-2.png)

5. ECS 서비스로 이동하여 클러스터를 선택합니다.
    - 클러스터의 변경 사항을 검토하십시오.
    - 새 작업이 CI/CD 파이프 라인의 일부로 활성화되는 것을 볼 수 있습니다.
{{% notice note %}}
클러스터에 Desired 컨테이너가 1 개로 설정되어 있으므로 컨테이너를 시작하면 이전 배포가 제거됩니다.
{{% /notice %}}
    ![deploy-cloudformation-1](/create-pipeline/pipeline-ecs-new-service.png)

6. ECS 서비스가 성공적으로 시작되면 Source WebServer 콘솔로 이동합니다. 아래 출력에서 ​​애플리케이션 스택이 성공적으로 생성되었음을 알리는 출력을 볼 수 있습니다!

```bash
root@ip-10-0-0-202:~/app2container/java-tomcat-21fb1eb0# aws cloudformation deploy --template-file /root/app2container/<java-app-id>/Artifacts/Pipeline/CodePipeline/ecs-pipeline-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<java-app-id>-ecs-pipeline-stack

Waiting for changeset to be created..
Waiting for stack create/update to complete
Successfully created/updated stack - a2c-<java-app-id>-ecs-pipeline-stack
```

#### 축하합니다.

CI/CD 파이프 라인을 생성하고 애플리케이션 컨테이너의 베타 버전을 ECS 클러스터로 시작했습니다.

----
추가적으로, 정의 된 S3 버킷에서 아티팩트를 검토 할 수 있습니다.

1. S3 서비스로 이동하고 정의 된 S3 버킷으로 이동합니다.

   - App2Container는 S3 버킷의 각 애플리케이션에 대한 버킷을 생성합니다.
    ![deploy-cloudformation-1](/create-pipeline/s3-1.png)

   - ECS 및 파이프 라인에 대한 별도의 하위 폴더를 볼 수 있습니다. 예를 들어 EKS를 타겟 배포로 결정하면 이 위치에 EKS 용 별도의 버킷이 있습니다.
    ![deploy-cloudformation-1](/create-pipeline/s3-2.png)
