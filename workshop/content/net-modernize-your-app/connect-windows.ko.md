+++
title = "Windows 인스턴스에 연결"
weight = 20
pre = "<b>- </b>"
+++

#### 1. RDP를 통해 Windows 서버에 연결

Windows 인스턴스에 연결하려면 초기 관리자 암호를 가져온 후 원격 데스크톱을 사용하여 인스턴스에 연결할 때, 이 암호를 입력해야합니다. 자세한 내용은 [Windows 인스턴스에 연결](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html#connect-rdp)을 참조하십시오.

- CloudFormation 서비스로 이동하여 Workshop 스택을 클릭합니다.
- 출력 탭에서 아래와 같이 SSHKeyURL 링크를 클릭합니다. 관리자 액세스를 검색하는 데 사용할 SSH 키를 가져옵니다.
- 다음 단계에서 사용할 키를 메모장에 복사합니다.
![create-iam-user](/setting-up-app2container/ssh-key-url.png)

- EC2 서비스로 이동하여 **Source-NET-Webserver** 를 클릭하고 콘솔 오른쪽 상단에있는 **Connect**을 클릭합니다.
- "인스턴스에 연결"화면에서 "RDP 클라이언트"를 클릭하고 아래와 같이 "암호 가져 오기"를 클릭합니다.
![create-iam-user](/setting-up-app2container/net-get-password.png)
- 복사한 SSHKey를 붙여 넣기를 수행하고 "Decrypt Password"를 클릭 합니다.
![create-iam-user](/setting-up-app2container/net-decrypt-password.png)
- 표시된 암호를 메모장에 복사합니다.
![create-iam-user](/setting-up-app2container/net-copy-password.png)

#### 2. Session Manager로 Web Server 접근

Session Manager를 통해 서버 powershell에 연결할 수도 있습니다. "Session Manager"에 대한 자세한 내용은 [Session-Manager-Getting-Started](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started.html)를 참조하세요.

- EC2 서비스로 이동하여 **Source-NET-Webserver** 를 클릭하고 콘솔 오른쪽 상단에있는 "Connect"를 클릭합니다.
- "인스턴스에 연결"화면에서 **Session Manager**를 클릭하고 아래와 같이 "Connect"을 클릭합니다.
    ![create-iam-user](/setting-up-app2container/net-connect-session-manager.png)
- 웹 서버에서 Powershell 콘솔에 직접 연결됩니다.
