+++
title = "- Windows 인스턴스 연결"
chapter = true
weight = 51

+++
1. #### RDP를 통해 Windows 서버에 연결

   Windows 인스턴스에 연결하려면 초기 관리자 암호를 가져온 후 원격 데스크톱을 사용하여 인스턴스에 연결할 때, 이 암호를 입력해야합니다. 자세한 내용은 Windows 인스턴스에 연결을 참조하십시오.

   - CloudFormation 서비스로 이동하여 Workshop 스택을 클릭합니다.

   - 출력 탭에서 아래와 같이 SSHKeyURL 링크를 클릭합니다. 관리자 액세스를 검색하는 데 사용할 SSH 키를 가져옵니다.

   - 다음 단계에서 사용할 키를 메모장에 복사합니다.

     <img src="/images/B/1/1.output.png" alt="1.output.png" style="zoom:100%;" />

   - EC2 서비스로 이동하여 **Source-NET-Webserver** 를 클릭하고 콘솔 오른쪽 상단에있는 **Connect**을 클릭합니다.

   - "인스턴스에 연결"화면에서 "RDP 클라이언트"를 클릭하고 아래와 같이 "암호 가져 오기"를 클릭합니다.

     <img src="/images/B/1/2.net-get-password.png" alt="2.net-get-password" style="zoom:100%;" />

   - 표시된 암호를 메모장에 복사합니다.

     <img src="/images/B/1/3.net-copy-password.png" alt="3.net-copy-password" style="zoom:100%;" />

2. #### Connect your Web Server via Session Manager

   세션 관리자를 통해 서버 powershell에 연결할 수도 있습니다. "Session Manager"에 대한 자세한 내용은 [Session-Manager-Getting-Started](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started.html)를 참조하세요.

   - EC2 서비스로 이동하여 **Source-NET-Webserver** 를 클릭하고 콘솔 오른쪽 상단에있는 "Connect"를 클릭합니다.

   - "인스턴스에 연결"화면에서 **Session Manager**를 클릭하고 아래와 같이 "Connect"을 클릭합니다.

     <img src="/images/B/1/4.net-connect-session-manager.png" alt="4.net-connect-session-manager" style="zoom:100%;" />

   - 웹 서버에서 Powershell 콘솔에 직접 연결됩니다.

   
