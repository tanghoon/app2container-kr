+++
title = "AWS 호스팅 이벤트에 참석"
weight = 40
pre = "<b>- </b>"
+++

AWS 호스팅 이벤트에 참석하는 경우 워크샵을 시작하기 전에 아래 확인 사항을 따라 워크샵 환경이 성공적으로 구축되었는지 확인하십시오.

{{% notice warning %}}
일부 VPN 환경은 Public URL에 대한 액세스를 차단할 수 있으므로 워크샵을 시작하기 전에 VPN과의 연결을 끊으십시오.
{{% /notice %}}  

1. 이벤트 전에 공유한 해시 URL을 사용하여 팀 대시보드에 로그인합니다.
    - 아래 화면이 팀 대시보드로 표시됩니다.

    ![team](/intro/team-dashboard.png)

2. 팀 대시보드에서 AWS 콘솔에 로그인합니다.

3. 팀 대시보드에서 "팀 이름 설정" 단추를 클릭하여 팀 이름을 업데이트합니다.

4. AWS 호스팅 이벤트에서 Workshop 환경은 이미 Cloud Formation Stack으로 구현되어 있습니다.
    - 서비스에서 클라우드 포메이션으로 이동하여 아래와 같이 클라우드 포메이션 스택이 구축되었는지(CREATE_COMPLETE) 확인하십시오.

    ![team](/intro/check-stack.png)

    - 생성된 스택을 클릭하고 **Outputs** 탭을 클릭합니다. 아래와 같은 정보가 스크린샷에 표시됩니다.

    ![Source Environment Information in AWS Console](/intro/self-service-env-awsconsole-info.en.png)

    - 이 정보를  로컬 메모장에 복사-붙여넣기를 수행하거나, 워크샵 중에 사용할 별도의 브라우저 탭에서 볼 수 있도록 이 페이지를 열어 둡니다.

5. EC2 서비스로 이동하여 아래 나온 것처럼 5개의 인스턴스가 배포되어 있고 실행 중인지 확인합니다.

![team](/intro/check-instances.png)

{{% notice note %}}
실습을 수행하는 중에 문제가 발견되면 AWS 이벤트 운영자에게 문의하여 해결하십시오.
{{% /notice %}}  

### 실습을 수행 하기 위한 준비가 완료 되었습니다.

모더나이제이션 실습을 위해서 애플리케이션(Java 또는 .NET) 을 선택 합니다.
