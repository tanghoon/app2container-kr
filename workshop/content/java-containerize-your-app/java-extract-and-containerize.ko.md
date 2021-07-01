+++
title = "추출 및 컨테이너화"
weight = 60
pre = "<b>2.4 </b>"
+++

이 모듈에서는 app2container를 사용하여 애플리케이션을 변환합니다. 추출 및 컨테이너화 단계 활동은 애플리케이션 서버에서 모든 단계를 실행하고 있는지 또는 분석에 애플리케이션 서버를 사용하고 컨테이너화 및 배포에 작업자 시스템을 사용하는지에 따라 다릅니다.
![extract-and-containerize-intro](/extract-and-containerize/extract-containerize-overview.png)
이 시나리오에서는 ( [Java 앱 컨테이너화]({{< ref "/java-containerize-your-app/_index.ko.md" >}}) ) 애플리케이션 서버에서 아래의 모든 단계를 실행하게 됩니다.

{{% notice note %}}
Worker 머신을 사용하여 컨테이너화 환경을 분리하는 것이 모범 사례이며 보안 및 기능적 이점을 제공합니다. Worker 시스템 옵션은 [ .NET 앱 모더나이제이션]({{< ref "/net-containerize-your-app/_index.ko.md" >}}) 시나리오에서 사용됩니다.
{{% /notice %}}  

1. 다음 명령을 실행하여 컨테이너화 프로세스를 시작합니다. 실습자는 \<java-app-id\> 를 Java 애플리케이션 ID로 바꿉니다.

    ```bash
    app2container containerize --application-id <java-app-id>
    ```

    - 컨테이너화 프로세스는 몇 분 정도 소요되며 App2Container는 모든 AWS 배포 관련 구성을 포함하는 도커 파일 및 deployment.json 파일을 생성합니다.
    - 컨테이너화 프로세스가 완료되면 아래와 같은 출력이 표시되어야 합니다.

    ```bash
    root@ip-10-0-0-129:/home/ubuntu# app2container containerize --application-id java-tomcat-21fb1eb0
    ✔ AWS prerequisite check succeeded
    ✔ Docker prerequisite check succeeded
    ✔ Extracted container artifacts for application
    ✔ Entry file generated
    ✔ Dockerfile generated under /root/app2container/java-tomcat-21fb1eb0/Artifacts
    ✔ Generated dockerfile.update under /root/app2container/java-tomcat-21fb1eb0/Artifacts
    ✔ Generated deployment file at /root/app2container/java-tomcat-21fb1eb0/deployment.json
    👍 Containerization successful. Generated docker image java-tomcat-21fb1eb0

    💡 You're all set to test and deploy your container image.

    Next Steps:
    1. View the container image with "docker images" and test the application.
    2. When you're ready to deploy to AWS, please edit the deployment file as needed at /root/app2container/java-tomcat-21fb1eb0/deployment.json.
    3. Generate deployment artifacts using "app2container generate app-deployment --application-id java-tomcat-21fb1eb0"
    ```

2. 이제 도커 이미지가 준비되었습니다. 아래 명령을 실행하여 이를 확인할 수 있습니다.

    ```bash
    docker images
    ```

    - 컨테이너 이미지는 컨테이너 기본 이미지(ubuntu 18.4)와 태그(최신)를 사용하여 Java 애플리케이션용으로 생성 된것을 확인 할 수 있습니다.

    ```bash
    root@ip-10-0-0-129:/home/ubuntu# docker images
    REPOSITORY           TAG     IMAGE ID      CREATED        SIZE
    java-tomcat-21fb1eb0 latest  5e382c3f2e6a  3 minutes ago  715MB
    ubuntu               18.04   2c047404e52d  6 weeks ago    63.3MB
    ```

    이 단계를 통해 애플리케이션을 성공적으로 컨테이너화했습니다. 이제 AWS에 배포할 준비가 되었습니다.

3. "App2container containerize" 명령을 실행하면 자동으로 deployment.json 파일이 생성됩니다. 이 파일에는 AWS 배포 구성이 포함되어 있습니다. 이 파일을 업데이트하여 AWS 환경에서 배포를 사용자 지정할 수 있습니다. 예를 들어 타겟 환경에 대한 특정 VPC를 선택하거나 컨테이너에 대한 CPU/메모리 할당 설정을 정의할 수 있습니다.

- 파일을 열고 구성을 검토합니다. 참고: 명령 출력에서 ​​아래 명령의 파일 위치를 업데이트하십시오.

```bash
nano /root/app2container/<java-app-id>/deployment.json
```

- 파일을 열면 각 AWS 서비스에 대한 다양한 구성 설정이 있음을 알 수 있습니다. App2Container는 이러한 설정을 미리 빌드하지만 환경을 구성하도록 업데이트할 수 있습니다.

이 워크샵에서는 2곳을 확인해야 합니다.

1. "createEcsArtifacts"가 "true"로 설정되어 있는지 확인합니다.  [x]
    - ecsParameters 섹션은 ECS(Elastic Container Service)를 타겟 환경으로 결정할 때 설정하는 부분입니다. ECS에 배포하지 않으려면 "createEcsArtifacts"를 "false"로 설정해야 합니다.
{{% notice note %}}
아래에서 "deployTarget"이 "FARGATE"로 설정되어 있습니다. 컨트롤이 컨테이너를 호스팅하는 ec2 인스턴스를 관리하도록 하려면 이것을 "EC2"로 설정할 수 있습니다
{{% /notice %}}  
    ![java-deployment-1](/extract-and-containerize/java-deployment-1.png)

2. 타겟 네트워크에 애플리케이션을 배포하려면 타겟 vpc-id를 입력하십시오.

    - AWS console에서 <a href="https://us-west-2.console.aws.amazon.com/vpc/home?region=us-west-2#vpcs" target="_blank"> VPC 콘솔 </a>로 이동 합니다.
    - VPC 화면에서 "TargetVPC" 를 찾아 아래와 같이 vpc-id를 복사합니다.
    ![java-deployment-1](/extract-and-containerize/java-deployment-vpc.png)
    - deployment.json 파일에서 "reuseResources" 부분을 찾아 복사한 "vpcId" 값을 붙여넣습니다.
    ![java-deployment-1](/extract-and-containerize/java-deployment-2.png)

3. "ctrl+x"로 파일을 종료하고 "y"를 입력하고 Enter 키를 눌러 동일한 이름으로 파일을 저장합니다.

{{% notice note %}}
deployment.json 파일에서 VPC-ID를 업데이트하지 않으면 app2container가 애플리케이션을 기본 VPC에 배포합니다.
{{% /notice %}}  

축하합니다! 모든 타겟 AWS 구성 설정을 정의하고 애플리케이션을 배포할 준비가 되었습니다.

다음 섹션에서는 배포를 위한 배포 아티팩트를 만듭니다.
