+++
title = "App2Container 설치 및 초기화"
weight = 30
pre = "<b>2.2 </b>"
+++

#### 1. Webserver 접속


1. Java 애플리케이션이 실행되는 **Source-Java-Webserver**서버에 연결합니다.

   웹 서버를 연결하는 옵션은 여러 가지가 있습니다. 리눅스 인스턴스에 연결하는 방법에 대한 자세한 내용은 이 <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html" target="_blank">문서</a>를 참조하십시오.

    - AWS Systems Manager의 <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html" target="_blank">Session Manager</a>를 사용하여 웹 서버에 연결할 수 있습니다. Session Manager는 인바운드 포트를 열거나, 기본 호스트를 유지 보수하거나, SSH 키를 관리할 필요 없이 안전하고 감사 가능한 인스턴스 관리 기능을 제공합니다.
       
        - Amazon EC2 인스턴스로 이동하여 Java 웹 서버를 선택하고 "Connect"을 클릭합니다.
        - “Session Manager”를 선택하고 “Connect”를 클릭 합니다.
       
          ![create-s3-bucket](/setting-up-app2container/session-manager.png)

    **혹은**

    - 웹 서버에 SSH로 접속하기 위해서는 다음 세부 정보가 필요합니다.

        a. 웹서버의 Public DNS 명<br>
        b. 웹서버의 Username<br>
        c. SSHKey <br>

        이 정보는 CloudFormation 스택의 Output 섹션에서 찾을 수 있습니다. 스택은 <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank">CloudFormation</a> 콘솔로 이동하여 액세스할 수 있습니다.

        | Parameter              | Value                                               |
        | ---------------------- | --------------------------------------------------- |
        | Username   | ubuntu                                |
        | SSHKey  | SSHKeyURL      (이 키로 .pem 파일을 생성해야 합니다.)                         |
        | Public DNS Name        | JavaWebServerDNSName                                    |

  {{% notice note %}}
  SSH 키의 경우 URL에서 텍스트 편집기로 키를 복사하여 .pem 파일을 생성하고 .pem 파일로 저장해야 합니다.
  {{% /notice %}}  

#### 2. AWS 접근 환경 구성

웹 서버에서 App2Container가 사용할 수 있는 AWS CLI에 대한 프로파일을 생성해야 합니다. 이 실습에서는 'aws configure'를 실행하여 기본 프로파일을 설정합니다.

{{% notice note %}}
App2Container가 필수 구성 요소에서 생성한 App2Container 사용자 정보에 대한 자격 증명을 사용합니다.
{{% /notice %}}

```bash
  sudo su   
```

```bash
  aws configure
```

```bash
  AWS Access Key ID [None]: <<이전 단계에서 생성된 AWS access key 입력>>
  AWS Secret Access Key [None]: <<이전 단계에서 생성된  AWS secret access key 입력>>
  Default region name [None]: us-west-2
  Default output format [None]: [blank]
```

#### 3. App2Container 설치

1. Linux 서버에서 App2Container 명령을 실행하려면 높은 사용 권한이 필요합니다. 루트로 실행하거나 서버에 로그인할 때 "sudo su"를 사용하거나 각 명령에 접두사 "sudo"를 사용 하는지 확인하십시오.

    ```bash
    sudo su
    ```

2. Linux용 App2Container를 다운로드 하고 설치 합니다.

    - 소스 Linux 서버에 연결되면 curl 명령을 사용하여 Amazon S3에서 App2Container 설치 패키지를 다운로드합니다.

    ```bash
    curl -o AWSApp2Container-installer-linux.tar.gz https://app2container-release-us-east-1.s3.us-east-1.amazonaws.com/latest/linux/AWSApp2Container-installer-linux.tar.gz
    ```

3. 압축을 풀고 해당 폴더로 이동하십시오.

    ```bash
    tar xvf AWSApp2Container-installer-linux.tar.gz
    ```

4. 압축을 푼 폴더에서 설치 스크립트를 실행하고 프롬프트를 따릅니다.

      ```bash
       ./install.sh
      ```

5. (선택 사항) App2Container에 사용할 수 있는 명령어를 확입합니다.

    ```bash
    app2container help
    ```

#### 4. App2Container 초기화

init 명령은 App2 Container에 대해 일회성 초기화 작업을 수행합니다. 이 대화형 명령은 로컬 App2 Container 환경을 설정하는 데 필요한 정보를 묻는 메시지를 표시합니다.

1. App2Container가 컨테이너화 프로세스 중에 아티팩트를 저장할 수 있는 Amazon S3 버킷을 생성해야 합니다.

    - S3 콘솔로 이동하여 버킷의 고유 이름(App2Container-영문이름-날짜)을 입력하고 create를 클릭하여 버킷 이름을 메모장에 저장합니다. (이 작업은 이후 단계에서 필요 합니다)
    ![create-s3-bucket](/setting-up-app2container/s3-create-bucket.png)

2. 소스 웹 서버에 다시 로그인하고 "app2container init" 명령을 사용하여 App2Container를 초기화합니다.

    ```bash
    app2container init
    ```

  **다음 정보를 입력하라는 메시지가 표시됩니다.**

| Parameter              | Value             |
| ---------------------- | ---------------- |
| Workspace directory path                | 기본값을 유지 합니다. (App2Container가 컨테이너화 프로세스 중에 아티팩트를 저장할 수있는 로컬 디렉토리. 기본값은 /root/app2container 입니다.)|
| Use AWS EC2 Instance profile               | 기본값을 유지 합니다. (EC2 인스턴스 프로파일을 사용하여 App2Container 작업을 실행하도록 선택할 수 있습니다.) |
| AWS profile               | 기본값을 유지 합니다. (AWS 액세스 키와 같이 App2Container를 실행하는 데 필요한 정보가 들어 있습니다.)  |
| Amazon S3 bucket                | 전 단계에서 생성한 Amazon S3 버킷 이름(app2container-Your-Initials-Date)을 입력합니다. (예: app2container-sd-jan-29) |
| Permission to collect usage metrics              | 기본값을 유지 합니다. 이 옵션은 app2container가 호스트 운영 체제, 애플리케이션 유형 및 사용자가 실행하는 app2container 명령에 대한 정보를 수집하도록 허용하는 것입니다. |
| Permission to Automatically upload logs and App2Container generated artifacts on crashes and internal errors | 기본값을 유지 합니다. (이 옵션은 app2container에서 충돌 및 내부 오류 시 로그 및 아티팩트를 업로드할 수 있도록 허용하는 것입니다.) |
| Whether to enforce signed images               | 기본값을 유지 합니다. (선택적으로 DCT(Docket Content Trust)를 사용하여 이미지를 서명하도록 요구할 수 있습니다.) |

{{% notice note %}}
메시지가 표시되면 이전 단계에서 생성한 올바른 Amazon S3 버킷 이름(app2container-Your-Initials-Date)을 입력해야 합니다. 아래 예제 출력을 참조하십시오.
{{% /notice %}}

```bash
Workspace directory path for artifacts[default: /root/app2container]:
Use AWS EC2 Instance profile 'arn:aws:iam::-:instance-profile/App2ContainerWorkshop-EC2InstanceProfile-' configured with this instance? (Y/N)[default: n]
AWS Profile (configured using 'aws configure --profile')[default: default]:
Optional S3 bucket for application artifacts: app2container-sd-jan-29
Report usage metrics to AWS? (Y/N)[default: y]:
Automatically upload logs and App2Container generated artifacts on crashes and internal errors? (Y/N)[default: y]:
Require images to be signed using Docker Content Trust (DCT)? (Y/N)[default: n]:
```

App2Container를 설치하고 초기화했으며, 이제 Java 애플리케이션 컨테이너를 시작할 준비가 되었습니다.

다음 실습에서는 웹 애플리케이션을 포함한 App2Container를 사용하여 AWS ECS에 배포합니다.
