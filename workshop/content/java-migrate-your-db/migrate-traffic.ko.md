+++
title = "애플리케이션 DB접속 정보 변경"
weight = 60
pre = "<b>1.6 </b>"
+++

여러분은 Amazon RDS에 새 PostgreSQL 데이터베이스를 성공적으로 생성하고 기존 데이터베이스를 마이그레이션하고 지속적인 복제 설정을 지정 하였습니다.

이제 애플리케이션이 새로 마이그레이션된 데이터베이스를 사용하기 위해서 소스 Java 애플리케이션 서버에서 데이터베이스 Endpoint 구성 설정을 변경 하도록 하겠습니다.

1. RDS 서비스로 이동하여 오른쪽 창에서 "Databases"를 클릭합니다.
    - 이전에 생성한 RDS 데이터베이스 "unicorn"을 선택합니다.
    - Connectivity & Security 탭에서 Endpoint URL을 메모장에 복사합니다. 다음 단계에서 이 Endpoint을 사용합니다.

    ![source-endpoint](/db-mig/java-rds-endpoint.png)

2. Java 애플리케이션이 실행되는 **Source-Java-Webserver**에 연결합니다.

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

3. 응용 프로그램은 서버의 환경 변수에서 데이터베이스 연결 구성을 가져옵니다. /etc 폴더에서 environment 파일을 열고 이전에 복사한 RDS Endpoint로 "RDS_HOSTNAME"을 업데이트합니다. 

    ```bash
    sudo su
    nano /etc/environment
    ```

    - 파일을 열면 RDS_HOSTNAME이 이미 소스 DB Private IP 주소인 "10.0.0.140"으로 설정되어 있는 것을 볼 수 있습니다. RDS Endpoint를 사용하여 파라미터 변수를 변경하고 파일을 저장하고 종료합니다.
    ![source-endpoint](/db-mig/java-env-var.png)

    - 변경 후 "ctrl+x"로 파일을 종료하고 "y"를 입력한 후 Enter 키를 눌러 동일한 이름으로 파일을 저장합니다.

4. 환경 변수 변경을 활성화하려면 아래 명령을 실행하십시오.

    ```bash
    source /etc/environment
    ```

5. 이제 Tomcat 웹 서버를 다시 시작하여 아래 명령을 사용하여 웹 애플리케이션의 변경 사항을 적용합니다.

    ```bash
    cd /usr/local/tomcat8/bin/
    ./shutdown.sh
    ./startup.sh
    ```

    - 터미널에서 아래 출력을 볼 수 있습니다.

    ![source-endpoint](/db-mig/java-start-tomcat.png)

6. <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank">App2Container Cloudformation Stack</a>으로 이동하여 Output 섹션에서 "JavaWebApplicationURL" 매개 변수를 복사합니다.

7. 웹브라우저를 열고 "JavaWebApplicationURL"로 이동 합니다.

    - 아래와 같이 웹 응용 프로그램이 성공적으로 실행되는 것을 볼 수 있습니다.

        ![source-endpoint](/db-mig/java-app-url.png)

    - "BROWSE MOVIES" 탭으로 이동하면 유니콘 영화 목록이 표시됩니다.

        ![source-endpoint](/db-mig/java-url-movies.png)

#### 수고 하셨습니다.

여러분은 데이터베이스를 성공적으로 마이그레이션 하였으며 웹 애플리케이션이 마이그레이션된 RDS 데이터베이스를 사용하고 있습니다.
