+++
title = "검색 및 분석"
weight = 50
pre = "<b>2.3 </b>"
+++

모더나이제이션 프로세스는 시스템에서 실행 중인 Java 애플리케이션을 검색하여 인벤토리를 생성한 다음 분석하는 것으로 시작합니다.

![app2container-discover](/discover-and-analyze/discover-overview-diagram.png)

#### 1. 애플리케이션 인벤토리 작성

1. 다음 명령을 실행하여 서버에서 실행 중인 응용 프로그램을 나열합니다.

    ```bash
    app2container inventory
    ```

    - Output 내용은 각 애플리케이션에 대한 노드가 있는 JSON 개체 모음입니다. 각 개체에는 키/값 쌍이 포함되며 아래와 같이 Java-app-id로 시작합니다.

    ```JSON
    "java-app-id": {
        "processId": pid,
        "cmdline": "/user/bin/java ...",
        "applicationType": "java-apptype"
    }
    ```

2. 수행 결과는 아래와 유사하게 표시됩니다. "java-tomcat-6c144e52"는 Java 애플리케이션 ID입니다. 다음 단계에서 사용할 수 있도록 애플리케이션 ID를 메모장에 복사합니다.

    ```JSON
    {
      "java-tomcat-21fb1eb0": {
        "processId": 1033,
        "cmdline": "/usr/bin/java ... -Dcatalina.home=/usr/local/tomcat8 -Djava.io.tmpdir=/usr/local/tomcat8/temp org.apache.catalina.startup.Bootstrap start ",
        "applicationType": "java-tomcat",
        "webApp": "ROOT"
      }
    }
    ```

#### 2. 애플리케이션 분석

1. 인벤토리 명령의 JSON 출력에서 확인한 애플리케이션 ID로 <java-app-id>를 변경한 후 "app2container analyze  --application-id \<java-app-id\>" 명령을 실행합니다.

    ```bash
    app2container analyze --application-id <java-app-id>
    ```

2. "Analyze" 명령의 결과는 app2container에서 완료된 작업에 대한 정보와 다음 단계를 위한 권장 사항을 제공합니다.

    ```bash
    root@ip-10-0-0-129:/home/ubuntu# app2container analyze --application-id java-tomcat-21fb1eb0
    ✔ Created artifacts folder /root/app2container/java-tomcat-21fb1eb0
    ✔ Generated analysis data in /root/app2container/java-tomcat-21fb1eb0/analysis.json
    👍 Analysis successful for application java-tomcat-21fb1eb0

    💡 Next Steps:
    1. View the application analysis file at /root/app2container/java-tomcat-21fb1eb0/analysis.json.
    2. Edit the application analysis file as needed.
    3. Start the containerization process using this command: app2container containerize --application-id java-tomcat-21fb1eb0
    ```

3. 출력 결과에서 analysis.json 의 위치로 이동하여 화일을 열어 확인 합니다.

    - (NOTE: 실습자 화면의 결과에서 ​​analysis.json 파일 위치를 업데이트하여 수행 하십시오.)

      ```bash
      cat /root/app2container/<java-app-id>/analysis.json
      ```

    - 파일을 열면 analysis.json 파일에 EDITABLE 및 NON-EDITABLE의 2개 섹션이 있는 것을 볼 수 있습니다.

      - EDITABLE 섹션에는 ContainerBaseImage 또는 Imagetag와 같은 기본 설정을 지정할 수 있는 containerParameters 섹션이 있습니다. 이러한 매개변수는 나중에 컨테이너화 프로세스 중에 사용됩니다. 이 연습에서는 Ubuntu:18.04가 containerBaseImage에 사용됩니다.
        ![java-analysis](/discover-and-analyze/java-analysis-1.png)

      - NON-EDITABLE 섹션에는 OS 데이터, 사용 중인 포트, 종속성, 소프트웨어 라이브러리 등과 같은 컨테이너화 중에 app2container가 사용할 애플리케이션 수준 분석 정보가 포함됩니다.

{{% notice note %}}
이 분석 단계는 레거시 애플리케이션으로 작업하거나 애플리케이션 문서가 충분하지 않은 경우 특히 중요합니다.
{{% /notice %}}

  ![java-analysis](/discover-and-analyze/java-analysis-2.png)

이 워크샵에서는 EDITABLE 매개변수를 변경하지 않지만 실제 시나리오에서는 필요에 따라 이 섹션을 업데이트할 수 있습니다. 예를 들어 containerBaseImage를 변경하거나 DynamicLogging을 활성화합니다.

이 단계를 통해 분석을 완료하고 다음 섹션에서 애플리케이션의 컨테이너화를 계속할 수 있습니다.
