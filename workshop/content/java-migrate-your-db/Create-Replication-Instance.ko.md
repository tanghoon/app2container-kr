+++
title = "복제 인스턴스 생성"
weight = 20
pre = "<b>1.2 </b>"
+++

### 복제 서브넷 그룹 생성

**AWS DMS**를 사용하기 위한 사전 요구 사항 중 하나는 **DMS Replication 인스턴스**에서 사용할 서브넷 모음인 **서브넷 그룹**을 구성한 것입니다.

1. **AWS Console > Services > Database Migration Service > Subnet groups**으로 이동하여 **Create subnet group**  버튼을 클릭합니다.
2. **Create replication subnet group**  다음 파라미터 값을 입력하십시오.

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Name                | dms-subnet-group     |
    | Description         | Target VPC Subnet Group for DMS |
    | VPC                 | TargetVPC   |
    | Add subnets         | **TargetVPC-private-a-db**, **TargetVPC-private-b-db** 선택|

    ![Replication-instance-networ](/db-mig/java-subnet-group.png)

3. **Create subnet group** 버튼을 클릭 합니다.

### AWS DMS Replication 인스턴스 생성

이 단계에서는 소스 데이터베이스와 타겟 데이터베이스 간의 연결을 시작하고, 데이터를 전송하고, 초기 데이터 로드 중에 소스 데이터베이스에서 발생한 모든 변경 사항을 캐시하는 <a href="https://aws.amazon.com/dms/" target="_blank">AWS Database Migration Service</a> 복제 인스턴스를 생성합니다.



복제 인스턴스를 클릭한 다음 복제 인스턴스 생성 버튼을 클릭합니다.

1. **AWS Console**에서 **Services** 및 **Database Migration Service**로 이동합니다.

2. **Replication instances** 클릭한 다음 **Create replication instance** 버튼을 클릭합니다.

    ![Replication-instance-create](/db-mig/Replication-instance-create.png)

3. **Create replication instance** 화면에서 다음 파라미터 값으로 새 복제 인스턴스를 구성하십시오.

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Name                | replication-instance     |
    | Description         | DMS replication instance |
    | Instance Class      | dms.t3.medium            |
    | Engine version      | 3.4.3                    |
    |Allocated storage (GB)| 50                      |
    | VPC                 | TargetVPC                |
    | Multi-AZ            | Unchecked                |
    | Publicly accessible | Checked                  |

    입력값은 다음의 화면과 같습니다.

    ![replication-instance-conf](/db-mig/replication-instance-conf.png)

    **Advanced security and network configuration**에서 이전에 생성한 복제 서브넷 그룹, 가용성 영역(us-west-2a) 및 복제 인스턴스 보안 그룹을 선택해야 합니다.

    ![Replication-instance-conf](/db-mig/advanced-security.png)

4. **Create** 버튼을 클릭합니다.

{{% notice note %}}
"SYSTEM ERROR MESSION: adms.t2.medium 복제 인스턴스를 생성할 수 없음" 오류가 발생하면 DMS 복제 인스턴스를 다시 생성하되 **us-west-2b** **Availability Zone**을 선택하고 이 옵션이 도움이 되지 않는 경우 더 큰 **인스턴스 클래스**를 선택하십시오.
{{% /notice %}}  
