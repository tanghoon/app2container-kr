+++
title = "AWS 클라우드 배포"
weight = 170
pre = "<b>2.6 </b>"
+++

App2containe 프로세스 완료를 위한 마지막 단계만이 남았습니다.

![app2container-intro](/deploy-to-aws/Deploy-to-AWS-overview.png)

지금까지 다음을 수행 하였습니다.

- 애플리케이션 컨테이너 이미지는 이미 ECR (Elastic Container Registry)로 이동되었으며 ECS 작업 정의가 생성 및 등록되었습니다.
- App2container는 Cloud에 필요한 모든 리소스를 자동으로 배포하고 구성할 수 있도록 Master Cloudformation 템플릿을 만들었습니다.

이 섹션에서는 Cloudformation 템플릿을 시작하고 AWS 콘솔에서 배포 프로세스를 검토합니다.

1. Worker Machine에서 "app2container generate app-deployment" 출력에서 제안된 명령을 실행합니다.

    ```powershell
    aws cloudformation deploy --template-file C:\Users\Administrator\AppData\Local\app2container\<net-app-id>\EcsDeployment\ecs-master.yml --capabilities CAPABILITY_NAMED_IAM --stack-name a2c-<net-app-id>-ECS
    ```

    - 이 명령을 실행하면 "a2c-\<net-app-id\>-ECS"라는 Cloudformation 스택이 생성됩니다.

    {{% notice note %}}
이 과정은 몇 분 정도 걸립니다. AWS 콘솔로 돌아가 배포 중의 백그라운드 활동을 검토합니다.
    {{% /notice %}}  

2. AWS 콘솔에서 서비스의 "CloudFormation"으로 이동하고 "Stack"을 클릭합니다.
    - 스택이 "a2c-\<net-app-id\>-ECS"라는 이름으로 생성된 것을 볼 수 있습니다.
    - Nexted 스택은 각 배포의 기본 스택에 의해 수행되는 것을 알 수 있습니다. 예 : ALB (Application LoadBalancer) 및 ECS 클러스터를 위한 별도의 스택.
    - 각 스택의 "Event"탭에서 트리거 된 이벤트를 볼 수 있습니다.
    ![deploy-cloudformation-1](/deploy-to-aws/net-deploy-1.png)

3. ALB 스택이 완료되고 ClusterStack이 CloudFormation에서 트리거되면 ECS 서비스로 이동합니다.
    - 왼쪽 창에서 "Cluster"를 클릭합니다.
    - Y새 ECS 클러스터가 생성되는 것을 볼 수 있습니다.
    - 아래 화면에서 강조 표시된대로 “Cluster”이름을 클릭합니다.
    {{% notice note %}}
컨테이너가 EC2 인스턴스에 배포될 것입니다. 이는 추출 및 컨테이너화 단계의 deployment.json 파일에서 선택되었습니다. 기본 인프라를 제어 및 관리를 원하지 않고 컨테이너화 된 워크로드에 집중하려면 "FARGATE"로 변경할 수 있습니다.
    {{% /notice %}}
        ![deploy-cloudformation-1](/deploy-to-aws/net-deploy-2.png)

    {{% notice note %}}
<a href="https://aws.amazon.com/fargate" target="_blank">AWS FARGATE</a>는 컨테이너를위한 서버리스 컴퓨팅 엔진입니다. FARGATE를 사용하면 서버를 프로비저닝하고 관리할 필요가 없으며 컨테이너를 실행하는 데 필요한 리소스에 대해서만 비용을 지불하면됩니다.
    {{% /notice %}}

4. EC2 서비스로 이동하여 자동으로 시작되는 새 인스턴스를 찾습니다. 이 인스턴스는 컨테이너를 호스팅하고 Elastic Container Service (ECS)에서 관리합니다.
    ![deploy-cloudformation-1](/deploy-to-aws/net-deploy-3.png)
5. 클러스터가 생성된 후 서비스 탭에서 애플리케이션이 배포되고 상태가 "ACTIVE"임을 확인할 수 있습니다.
    - “Desired Tasks” 과 “Running Tasks”을 볼 수 있으며 실행 유형은 App2container에 의해 정의됩니다.
    - 이 배포의 Launch Type 은 EC2입니다.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-4.png)
6.  **Task** 탭을 클릭합니다. 여기에서 Task가 생성되어 실행중인 것을 볼 수 있습니다.
    - 세부 사항을 보려면 Task 이름을 클릭하십시오.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-5.png)
7. Task 화면에서 태스크 구성을 검토하십시오.
    - Load Balancing 섹션에서 컨테이너에 대해 Target Group 이 자동으로 생성되었음을 확인할 수 있습니다.
    - Target Group 을 클릭하십시오.
    {{% notice note %}}
VPC, 서브넷 및 보안 그룹도 정의되어 있습니다. 환경 요구 사항에 따라 업데이트 할 수 있습니다.
    {{% /notice %}}
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-7.png)
8. Target Group 내에서 Target Group 구성을 검토 합니다.
    - <a href="https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html" target="_blank">Target Groups</a>은 하나 이상의 등록된 대상으로 요청을 라우팅하는 데 사용됩니다. App2container는 Target Group을 생성하고 이를 컨테이너 및 로드밸런서에 등록 합니다.
    - Load Balancer를 클릭합니다.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-8.png)
9. Load Balancer 화면에서 애플리케이션에 대한 로드밸런서 구성을 검토하십시오.
    - Public DNS 이름을 복사하여 브라우저에 붙여 넣습니다.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-9.png)

#### 축하합니다.

10. 애플리케이션을 컨테이너화하고 Amazon ECS에 배포했습니다.
    ![deploy-cloudformation-2](/deploy-to-aws/net-deploy-11.png)
11. CloudFormation Service로 이동하여 App2container에서 여러 스택을 생성하고 컨테이너화 된 애플리케이션에 필요한 모든 리소스가 자동으로 생성되었는지 확인합니다.
    ![deploy-cloudformation-2](/deploy-to-aws/cloudformation-ecs-net-final.png)
12. Worker machine 콘솔로 이동하면 애플리케이션 스택이 성공적으로 생성되었음을 나타내는 아래와 같은 출력이 표시됩니다!
![deploy-cloudformation-2](/deploy-to-aws/net-deploy-10.png)
