+++
title = "자신의 AWS 계정에서 워크샵 실행"
weight = 20
pre = "<b>- </b>"
+++

{{% notice warning %}}
본 실습은 개인 계정을 이용하는 경우에만 수행합니다. AWS 호스팅 이벤트(예: re:Invent, GameDay, Workshop 또는 AWS 직원이 주최하는 다른 이벤트)를 사용하는 경우에는 다음의 [AWS 호스팅 이벤트에 참석]({{< ref "/hosted.md" >}})으로 이동 합니다.
{{% /notice %}}

### 개인 계정으로 실습 환경 구성 하기

이 실습에서는 <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html" target="_blank">관리자 권한</a>이 있는 **AWS 계정** 에 액세스할 수 있다고 가정합니다. 새 AWS 계정을 생성하려면 새로운 AWS 계정을 생성하고 <a href="https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/" target="_blank">활성화하는 방법</a>을 참조 하여 계정 준비 작업을 수행 합니다.


불필요한 요금을 받지 않도록 실습이 완료된 이후에 AWS 계정의 자원을 [삭제]({{< ref "/cleanup/_index.md" >}}) 하는 것을 잊지 마십시오!

   {{% notice note %}}
이 워크샵은 us-west-2 지역에서 작동하도록 설계되었습니다. 이 지역을 선택했는지 확인하십시오.
  {{% /notice %}}  

#### 실습 환경 배포

1. 아래의 Launch Stack 버튼을 클릭 하십시오. <a href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=App2ContainerWorkshop&templateURL=https://app2container-workshop-artifacts.s3-us-west-2.amazonaws.com/app2container_workshop_template.yml" target="_blank"><img src="https://application-migration-with-aws-workshop.s3-us-west-2.amazonaws.com/static/cloudformation-launch-stack.png"></a>

2. **Step 1 - Specify template** 영역에서 <https://app2container-workshop-artifacts.s3-us-west-2.amazonaws.com/app2container_workshop_template.yml> URL이 **Amazon S3 URL** 필드에 입력되었는지 확인하고 **다음**을 누르십시오.
  ![CloudFormation Step 1](/intro/cloudformation-step1.en.png)

3. **Step 2 - Specify stack details** 화면에서 App2ContainerWorkshop 으로 **Stack Name** 필드에 입력되었는지 확인하고 다음을 누릅니다.
  {{% notice note %}}
 Public IP 주소를 "IP 주소" 입력란에 추가하여 IP 주소 액세스만 제한할 수 있습니다. (예: x.x.x.x/32). 기본값(0.0.0.0/0)은 모든 IP에 대해 트래픽을 허용합니다.
  {{% /notice %}}  
  ![CloudFormation Step 2](/intro/cloudformation-step2.en.png)

4. **Step 3 - Configure stack options** 화면에서 변경하지 말고 **Next**를 누르십시오. 

5. **Step 4 - Review** 화면에서 아래 화면과 같이 페이지 하단으로 스크롤하여 모든 확인란을 선택한 후 **Next**를 눌러 템플릿을 배포합니다.  
  ![CloudFormation Step 4](/intro/cloudformation-step4.en.png)

{{% notice note %}}
배포가 실패하면 **Events** 탭으로 이동하여 근본 원인을 확인합니다. 일반적인 원인은 **ecsExecutionRole** 또는 **ecsAutoscaleRoleIAM** 역할이 AWS 계정에 이미 있기 때문입니다. 이러한 구성 요소를 삭제하고 소스 환경 CloudFormation 템플릿 배포를 다시 실행합니다.
{{% /notice %}}

템플릿이 **CREATE_COMPLETE** 상태이면 **AWS Console -> CloudFormation** 으로 이동하고 **App2ContainerWorkshop** 스택을 선택한 다음 **Outputs** 탭을 선택하여 생성된 소스 환경에 대한 정보를 찾을 수 있습니다. 아래와 같은 정보가 스크린샷에 표시됩니다.


![Source Environment Information in AWS Console](/intro/self-service-env-awsconsole-info.en.png)

- 이 정보를 복사 하여 여러분의 컴퓨터 메모장에 붙여 넣습니다.

### 실습을 수행 하기 위한 준비가 완료 되었습니다.

모더나이제이션 실습을 위해서 애플리케이션(Java 또는 .NET) 을 선택 합니다.