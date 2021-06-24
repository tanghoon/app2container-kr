+++
title = "2.4. 추출과 컨테이너화"
chapter = true
weight = 74

+++
이 모듈에서는 app2container를 사용하여 애플리케이션을 변환합니다. 변환 단계에서는 애플리케이션 서버에서 모든 단계를 실행하는지 또는 분석을 위해 애플리케이션 서버를 사용하고 컨테이너화 및 배포를 위해 Worker machine을 사용하는지에 따라 다릅니다.

<img src="/images/d/extract-containerize-overview.png" alt="extract-containerize-overview.png" style="zoom:100%;" />

이 시나리오 (.NET 앱 컨테이너화)에서는 Worker machine에서 아래 단계를 모두 실행합니다.

{{% notice note%}}
작업자 머신을 사용하여 컨테이너화 환경을 분리하는 것이 모범 사례이며 보안 및 기능적 이점을 제공하고 컨테이너화 프로세스를 표준화하여 대규모 컨테이너화를 지원할 수 있습니다.
{{% /notice %}}

1. #### 소스 서버에서 Worker machine 으로 아티팩트 추출

   - "app2container analyze" 명령 실행 후  출력된  화면을 참고하여, 다음 단계에 따라 "app2container remote extract"명령을 실행하여 모든 애플리케이션 아티팩트를 Worker machine 으로 가져옵니다. 환경에 따라 애플리케이션 ID 및 대상 IP 주소를 업데이트하십시오.

     ```
     app2container remote extract --target <private-IP of your source server> --application-id <net-app-id>
     ```

   - 추출 프로세스는 몇 분 정도 소요되며 App2Container는 모든 애플리케이션 아티팩트를 포함하는 zip 파일을 생성합니다. 이 시점 이후에는 app2container가 이제 작업자 컴퓨터에 모든 애플리케이션 관련 아티팩트를 가지므로 소스 애플리케이션 서버와 통신 할 필요가 없습니다.

{{% notice note%}}
이 시점에서 zip 파일 내의 아티팩트를 업데이트하고 업데이트 된 zip 파일로 컨테이너화를 계속할 수도 있습니다.
{{% /notice %}}

<img src="/images/d/net-extract-1.png" alt="net-extract-1.png" style="zoom:100%;" />

   -   컨테이너화 프로세스가 완료되면 아래와 같은 출력이 표시됩니다.

<img src="/images/d/net-extract-2 (1).png" alt="net-extract-2 (1).png" style="zoom:100%;" />

2. #### Worker Machine 에서 애플리케이션 컨테이너화

   1. **PrivateIPAddress**를 웹 서버의 IP 주소로 바꾸고 작업자 컴퓨터에서 "app2container containerize –input-archive"를 실행합니다.

      - 입력 아카이브 매개 변수에 대한 zip 파일 위치를 제공하십시오. 이 명령을 사용하면 소스 머신에서 컨테이너화를 수행하는 대신 Worker macine에서 추출된 파일을 기반으로 컨테이너화 할 수 있습니다.

{{% notice note%}}
CLI 출력에서 제안된 명령을 복사해서 사용할 수 있습니다.
{{% /notice %}}

```
app2container containerize --input-archive C:\Users\Administrator\AppData\Local\app2container\remote\<PrivateIPAddress>\<net-app-id>\<net-app-id>.zip
```

Worker machine은 컨테이너용 Windows 2019 기본 이미지를 다운로드해야 하므로,  이 프로세스는 몇 분 정도 걸립니다. 프로세스가 완료되면 아래 출력이 표시됩니다.

<img src="/images/d/net-containerize-1.png" alt="net-containerize-1.png" style="zoom:100%;" />

   2. 이 단계에서 컨테이너 이미지를 준비해야합니다. Worker machine에서 "docker images"를 실행하여 이미지가 빌드되었는지 확인합니다.

      ```
      docker images
      ```

      - 선택한 containerBaseImage (Windowsservercore-ltsc-2019) 및 imageTag (최신)를 사용하여 .NET 애플리케이션에 대한 컨테이너 이미지가 생성 된 것을 볼 수 있습니다.

        <img src="/images/d/net-containerize-2.png" alt="net-containerize-2.png" style="zoom:100%;" />

   {{% notice note%}}
   이 단계에서 운영 체제를 업데이트했습니다. 이는 애플리케이션이 EOL (End Of Life) OS 버전에서 실행되는 경우 특히 유용 할 수 있습니다.
   {{% /notice %}}

   3. “App2container containerize”명령을 실행하면, 응용 프로그램 폴더 내에 deployment.json 파일이 자동으로 생성됩니다 (아래 그림 참조).

      - 이 파일에는 AWS 배포 구성이 포함되어 있습니다.

      - 이 파일을 업데이트하여 AWS 환경에 맞게 배포 할 수 있습니다.

      - 예 : 대상 환경에 대한 특정 VPC를 선택하거나 컨테이너에 대한 CPU / 메모리 할당 설정을 정의할 수 있습니다. 

        <img src="/images/d/net-containerize-3.png" alt="net-containerize-3.png" style="zoom:100%;" />

   4. 파일을 열고 설정을 확인합니다.

      - 파일을 열면 각 AWS 서비스에 대한 다양한 구성 설정을 확인할 수 있습니다. App2Container는 이러한 설정을 사전에 구축(pre-built) 했지만 환경을 채택하도록 업데이트 할 수 있습니다.

        - 이 워크샵에서는 두 곳을 확인해야 합니다.

        <img src="/images/d/net-containerize-4.png" alt="net-containerize-4.png" style="zoom:100%;" />

        1. “createEcsArtifacts” 가 “true” 로 설정되어야 합니다. 

           1. ecsParameters 섹션은 ECS (Elastic Container Service)가 대상 환경으로 결정될 때의 설정입니다. ECS에 배포하지 않으려면 "createEcsArtifacts"를 "false"로 설정해야합니다.

        2. 애플리케이션을 배포할 타겟 네트워크의 vpc-id를 입력해야 합니다. 

           1. AWS 콘솔에서 VPC 서비스로 이동합니다. 

           2. VPC에서 “TargetVPC”를 찾은 후 아래 그림에서 처럼 vpc-id를 복사합니다. 

              <img src="/images/d/java-deployment-vpc.png" alt="java-deployment-vpc.png" style="zoom:100%;" />

           3. 위의 그림처럼 deployment.json 파일에서 "reuseResources"부분을 찾아 "vpcId" 값에 복사한 내용을 붙여 넣습니다. 

   {{% notice note%}}
   deployment.json 파일에서 VPC-ID를 업데이트하지 않으면 app2container가 애플리케이션을 기본 VPC에 배포합니다.
   {{% /notice %}}

              

모든 타겟 AWS 구성 설정을 정의했으며 애플리케이션을 배포할 준비가 되었습니다.
다음 섹션에서는 배포를 위한 배포 아티팩트를 생성합니다.
