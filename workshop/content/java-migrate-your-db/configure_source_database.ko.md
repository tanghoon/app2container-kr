+++
title = "소스 데이터베이스 구성 검토"
weight = 25
pre = "<b>1.3 </b>"
+++

{{% notice warning %}}
이 섹션은 정보 제공용으로만 선택 사항입니다. 이 워크샵에 대한 모든 설정이 이미 업데이트되었습니다. 그러나 DB 서버에 로그인하여 아래 나열된 구성을 검토할 수 있습니다.
{{% /notice %}}  

### CDC(변경 데이터 캡처)를 사용하여 DMS 복제 작업 실행

데이터베이스 마이그레이션의 다운타임을 최소화하기 위해 소스 데이터베이스에서 타겟 데이터베이스로 변경 사항(CDC-**Change Data Capture**)이라고도 함)을 지속적으로 복제합니다. CDC 및 **AWS DMS**의  CDC 지원에 대한 자세한 내용은 이 <a href="https://aws.amazon.com/blogs/database/aws-dms-now-supports-native-cdc-support/" target="_blank">문서</a>를 참조하십시오.

#### 소스 PostgreSQL 구성 변경

PostgreSQL에서 **AWS DMS** 연속 복제를 수행하는 경우에는 postgresql.conf 구성 파일에서 다음 파라미터 변수를 업데이트해야 합니다.

- (설정) wal_level = logical

- (설정) max_replication_slots 1보다 큰값
  - max_replication_slots 값은 실행할 태스크 수에 따라 설정해야 합니다. 예를 들어 태스크를 5개 이상 실행하려면 최소 5개의 슬롯을 설정해야 합니다. 슬롯은 태스크가 시작되는 즉시 자동으로 열리고 태스크가 더 이상 실행되지 않는 경우에도 열린 상태를 유지합니다. 열려 있는 슬롯을 수동으로 삭제해야 합니다.

- (설정) max_wal_senders 1보다 큰값
  - max_wal_senders 파라미터 변수는 실행할 수 있는 동시 태스크 수를 설정합니다.

- (설정) wal_sender_timeout =0
  - wal_sender_timeout 파라미터 변수는 지정된 시간(밀리초)보다 오랫동안 비활성 상태인 복제 연결을 종료합니다. 기본값은 60초이지만 이 파라미터 변수를 0으로 설정하여 시간 초과 메커니즘을 사용하지 않도록 설정하는 것이 좋습니다.

--------------

DB 서버에 로그인한 후 아래 명령을 사용하여 PostgreSQL 구성 설정을 검토할 수 있습니다.

1. 소스 Java 소스 데이터베이스 서버 정보를 가져옵니다. 이 정보는 <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/" target="_blank">CloudFormation</a>의 Output 섹션에서 확인할 수 있습니다. **자신의 AWS 계정에서 워크샵 실행** 방식으로 수행 하는 경우, **App2ContainerWorkshop**의 Output(출력) 섹션에서 확인이 가능 합니다.

    | Parameter              | Value                                               |
    | ---------------------- | --------------------------------------------------- |
    | Public DNS Name        | JavaDBServerDNSName                                    |
    | Username   | ubuntu                                |
    | SSHKey  | SSHKeyURL      (이 키로 .pem 파일을 생성해야 합니다.)                          |

{{% notice note %}}
SSH 키의 경우 URL에서 텍스트 편집기로 키를 복사하여 .pem 파일을 생성하고 .pem 파일로 저장해야 합니다.
{{% /notice %}}  

2. 소스 데이터베이스가 실행되는 **Source Java DB 서버**에 연결합니다.

    Linux 인스턴스에 연결하는 방법에 대한 자세한 내용은 <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html">문서</a>를 참조하십시오. 

3. 아래 명령을 실행하여 postgresql 구성 파일을 살펴봅니다.

    ```bash
    cat /etc/postgresql/10/main/postgresql.conf
    ```
