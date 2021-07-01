+++
title = "AWS 클라우드 배포"
weight = 80
pre = "<b>2.6 </b>"
+++

![app2container-intro](/deploy-to-aws/Deploy-to-AWS-overview.png)

이 단계에서

- 애플리케이션 컨테이너 이미지가 이미 ECR(Elastic Container Registry)에 있고 ECS 작업 정의가 생성 및 등록 되었습니다.
- App2container는 AWS에서 필요한 모든 리소스를 자동으로 배포하고 구성할 수 있도록 Master Cloudformation 템플릿을 생성했습니다.

이 섹션에서는 Cloudformation 템플릿을 시작하고 AWS 콘솔에서 배포 프로세스를 검토합니다.

1. 웹 서버에서 "app2container generate app-deployment"의 output에서 ​제시된 명령을 실행합니다. 사전에 복사된 애플리케이션 ID로 <java-app-id>를 업데이트하십시오.

    ```bash
    aws cloudformation deploy --template-file /root/app2container/<java-app-id>/EcsDeployment/ecs-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<java-app-id>-ECS
    ```

    - 명령을 수행하게 되면, Cloudformation 스택은 "a2c-<java-app-id>-ECS" 이름으로 생성 됩니다.
    {{% notice note %}}
이 프로세스는 몇 분 정도 소요됩니다. 배포 중 백그라운드 작업을 검토하려면 AWS 콘솔로 돌아갑니다.
    {{% /notice %}}

    ```bash
    root@ip-10-0-0-202:/home/ubuntu# aws cloudformation deploy --template-file /root/app2container/<java-app-id>/EcsDeployment/ecs-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<java-app-id>-ECS

    Waiting for changeset to be created..
    Waiting for stack create/update to complete
    ...
    ```

2. <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks" target="_blank"> Cloudformation 콘솔</a>로 이동 합니다.
    - 스택이 "a2c-java-tomcat-21fb1eb0-ECS"라는 이름으로 생성 되었음을 알 수 있습니다.
    - Nested 스택은 각 배포의 기본 스택에 의해 트리거됩니다. 예. ALB(Application LoadBalancer) 및 ECS 클러스터는 별도의 스택으로 분리 되어 있습니다.
    - 각 스택의 "events" 탭에서 실행된 이벤트를 확인 할 수 있습니다
    ![deploy-cloudformation-1](/deploy-to-aws/deploy-cloudformation-1.png)

3. ALB 스택 및 클러스터 스택이 완료된 후 ECS 서비스로 이동합니다.
    - 왼쪽 창에서 "클러스터"를 클릭합니다.
    - 새로운 ECS 클러스터가 생성되는 것을 볼 수 있습니다.
    - 화면 아래에서 강조 표시된 "클러스터" 이름을 클릭합니다
    {{% notice note %}}
컨테이너가 FARGATE에 배치되어 있습니다. 이는 deployment.json 파일의 추출 및 컨테이너화 단계에서 선택되었습니다. 호스트 서버에 대한 제어 권한을 갖고 이를 관리하려는 경우 "EC2"로 변경할 수 있습니다.
    {{% /notice %}}
        ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-1.png)

    {{% notice note %}}
<a href="https://aws.amazon.com/fargate" target="_blank">AWS FARGATE</a>는 컨테이너용 서버리스 컴퓨팅 엔진입니다. FARGATE를 사용하면 서버를 프로비저닝 및 관리할 필요가 없으며 컨테이너 실행에 필요한 리소스 비용만 지불하면 됩니다.
    {{% /notice %}}

4. 클러스터가 생성된 후 서비스 탭에서 애플리케이션이 배포되고 서비스 상태가 "Active"인 것을 볼 수 있습니다.
    - App2container에서 정의한 "Desired Tasks"와 "Running Tasks"의 값과 Launch 유형을 확인할 수 있습니다.
    ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-2.png)
5. Tasks 탭을 클릭합니다. 여기에서 Task가 생성되어 실행되고 있는 것을 볼 수 있습니다.
    - 상세 내용을 확인 하기 위하여 Task를 클릭 합니다.
    ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-3.png)
6. 클러스터를 클릭하고 서비스 탭을 클릭하여 서비스로 이동합니다.
    - 서비스 화면 내에서
    - 로드 밸런싱 섹션에서 컨테이너에 대해 Target Group이 자동으로 생성된 것을 볼 수 있습니다.
    - Target Group을 클릭합니다.
{{% notice note %}}
VPC, 서브넷 및 Secrity Group도 정의되어 있습니다. 환경 요구 사항에 따라 업데이트할 수 있습니다.
{{% /notice %}}
    ![deploy-cloudformation-2](/deploy-to-aws/ecs-cluster-4.png)
7. Target Group 내에서 구성 검토
    - <a href="https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html" target="_blank">Target Groups</a>은 하나 이상의 등록된 Target 으로 요청을 라우팅 하는 데 사용됩니다. App2container는 Target Group을 생성하고 이를 컨테이너 및 Loadbalancer에 등록합니다
    - 로드발란서를 클릭 합니다.
    ![deploy-cloudformation-2](/deploy-to-aws/loadbalancer-1.png)
8. 로드 밸런서 섹션에서 애플리케이션에 대한 로드 밸런서 구성을 검토하십시오.
    - Public DNS 이름을 복사하여 브라우저에 붙여넣습니다.
    ![deploy-cloudformation-2](/deploy-to-aws/loadbalancer-2.png)

#### 축하합니다.

9. 축하합니다!! 애플리케이션을 컨테이너화하고 Amazon ECS에 배포했습니다.
    ![deploy-cloudformation-2](/deploy-to-aws/java-last.png)
10. CloudFormation Service로 이동하여 여러 스택이 App2container에 의해 생성되고 컨테이너화된 애플리케이션에 필요한 모든 리소스가 자동으로 생성되는지 확인합니다.
    ![deploy-cloudformation-2](/deploy-to-aws/cloudformation-ecs-final.png)
11. 소스 웹 서버 콘솔로 이동하십시오. 애플리케이션 스택이 성공적으로 생성되었음을 나타내는 아래 출력이 표시되어야 합니다!
![deploy-cloudformation-2](/deploy-to-aws/stack-final.png)
