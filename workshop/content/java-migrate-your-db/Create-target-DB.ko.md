+++
title = "타겟 DB 생성"
weight = 15
pre = "<b>1.1 </b>"
+++

### 데이터베이스 마이그레이션

데이터베이스 마이그레이션은 다양한 방법으로 수행할 수 있으며, 이 워크샵에서는 <a href="https://aws.amazon.com/dms/" target="_blank">AWS Database Migrations Service (DMS)</a>를 사용하여 **지속적인 데이터 복제 마이그레이션 (continuous data replication)**을 수행할 예정입니다.

**AWS DMS**를 구성하기 전에 제공된 AWS 계정에 타겟 데이터베이스를 생성해야 합니다. **AWS RDS(Relation Database Service)**를 사용하여 이 작업을 수행하면 클라우드에서 관계형 데이터베이스를 쉽게 설정, 운영 및 확장할 수 있습니다.

필수 구성 요소로 소스 환경과 타겟 환경 간에 네트워크 연결이 필요합니다. 이 워크샵에서는 실제 시나리오를 복제하기 위해 소스 환경과 타겟 환경 간에 VPC Peering을 구축 했습니다.

{{% notice note %}}
사용자 환경에 필요한 Security Group이 이 워크샵에서 생성됩니다.
{{% /notice %}}
![Replication Instance Architecture](/db-mig/vpc-peering.png)

### 타겟 데이터베이스에 대한 서브넷 그룹 ( Subnet Group ) 생성



DB 서브넷 그룹 생성에서 다음 정보를 입력하십시오.

1. **AWS Console**로 이동하여 **Services** 에서 **RDS** 를 선택 합니다. 왼쪽 메뉴에서 서브넷 그룹을 선택한 다음 **Create DB Subnet Group**을 클릭합니다.

2. **Create DB subnet group** 에서 다음 정보를 입력하십시오.

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Name                | database-subnet-group     |
    | Description         | Subnets where RDS will be deployed |
    | VPC      | TargetVPC            |

    **Add subnets** 패널에서 CIDR 10.1.101.0/24 및 10.1.201.0/24를 사용하는 각 가용성 영역(us-west-2a 및 us-west-2b)에서 서브넷 하나를 추가한 다음 **Create** 버튼을 누르십시오.

    ![RDS Subnet group creation](/db-mig/net-db-subnet-group.png)

### 타겟 데이터베이스에 대한 파라미터 변수 그룹 ( Parameter Group ) 생성

PostgreSQL DB에서는 외래 키(참조 무결성 제약 조건)는 트리거를 사용하여 구현됩니다. 데이터베이스 전체 로드 단계 (full load) 를 수행하는 동안 AWS DMS는 데이터 적재 작업을 각 테이블당 한 번에 하나씩 로드합니다. 전체 로드 중에는 다음 방법 중 하나를 사용하여 외부 키 제약 조건을 사용하지 않도록 설정하는 것이 좋습니다.

- 인스턴스에서 모든 트리거를 일시적으로 사용하지 않도록 설정하고 전체 로드를 완료합니다.
- PostgreSQL에서 **session_replication_role** 파라미터 변수를 사용 합니다.

이 워크샵에서는  **session_replication_role** 파라미터 변수를 "replica"로 업데이트하겠습니다. 이를 위해 타겟 DB에 대한 새로운 Parameter Group을 생성하고 타겟 RDS DB를 생성하는 시점에 해당 파라미터를 적용하겠습니다.

보다 자세한 사항은 <a href="https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Target.PostgreSQL.html" target="_blank">PostgreSQL 데이터베이스를 타겟으로 사용 하기</a>를 참조 하십시오.


1. 왼쪽 메뉴에서 **Parameter groups**을 선택하고 **Create Parameter Group**을 클릭합니다.

2. **Parameter group family**에서 postgres10을 선택 합니다.
3. Name과 Description을 입력 하고 Create를 클릭 합니다. (예. unicorn-db-parameter-group)

    ![1](/db-mig/db-parameter-group.png)

4. 목록에서 방금 만든 "unicorn-db-parameter-group"을 클릭합니다.
5. 검색 상자에서 "session_replication_role"을 검색하여 선택하고 해당 값을 "replica"로 편집하십시오.
6. Save를 클릭하여 저장 합니다.

    ![1](/db-mig/db-parameters-2.png)

### 타겟 데이터베이스 생성

1. 이제 왼쪽 메뉴에서 **Databases**를 선택하고 **Create database**을 클릭합니다.

2. **Engine options**에서 PostgreSQL과 Version PostgreSQL 10.15-R1을 선택 합니다.

    ![1](/db-mig/db-create-1.png)

{{% notice note %}}
다음을 입력하여 소스 PostgreSQL 데이터베이스의 SQL 버전을 확인 할 수 있습니다.: **psql -V**
{{% /notice %}}

**Template** 섹션에서 "Free Tier"를 선택합니다.

![Free tier template selection](/db-mig/db-template.png)

{{% notice note %}}
"Free Tier"를 선택하면 템플릿이 마법사의 다음 단계에서 옵션을 제한하므로 AWS Free Tier의 제한 범위를 벗어나지 않습니다.
{{% /notice %}}

**Settings** 섹션에서 새 데이터베이스 인스턴스에 대한 DB 인스턴스 식별자(unicorn), 마스터 사용자 이름(unicorn) 및 마스터 암호(awscocks2021)를 구성합니다.

![3_db](/db-mig/db-create-2.png)

{{% notice note %}}
**Master username**과 **Master password**는 나중에 사용 되기 때문에 별도로 기록해 두십시오.
{{% /notice %}}

Burstable DB 인스턴스 클래스에서 **db.t2.micro**를 선택하고,**General Purpose (SSD)**를 스토리지 타입에서 선택 합니다. 이때  "Enable storage autoscaling"는 체크를 하지 않습니다.(이 데이터베이스에는 20GB 이상의 스토리지가 필요하지 않습니다.)

![4_db](/db-mig/4_db.png)

3. **Availability & durability**의 경우 **Do not create a standby instance** 옵션을 선택 하십시오.

    ![5_db](/db-mig/5_db.png)

{{% notice note %}}
For production workloads, we recommend enabling the standby instance to enable <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html" target="_blank">Multi-AZ Deployment</a> for higher availability.
{{% /notice %}}  

4. **Connectivity** 섹션에서 다음을 수행합니다.

    - **VPC(Virtual Private Cloud)**에서 **TargetVPC**(이 랩에 대해 자동으로 생성된 <a href="https://aws.amazon.com/vpc/" target="_blank">Amazon Virtual Private Cloud</a>를 선택합니다.
    ![6_db](/db-mig/db-create-3.png)
    - **Additional connectivity configuration -> VPC Security Group**에서 **Choose existing** VPC Security Group 선택을 선택하고 사용자를 위해 생성된 "PGRDSSG" Security Group을 선택합니다. 데이터베이스 포트가 5432로 설정되어 있는지 확인합니다.
    - 이전에 생성한 DB Subnet 그룹이 자동으로 선택됩니다.

    ![6_db](/db-mig/java-db-create-4.png)

5. **Database authentication**에서 **Password authentication**를 선택 합니다.

    ![6_2_db](/db-mig/6_2_db.png)

6. Additional configuration 섹션에서 초기 데이터베이스 이름('unicorn')을 입력하고 이전에 생성한 DB Parameter Group('unicorn-db-parameter-group')을 선택합니다. Backup, Performance Insights, Monitoring 항목의 선택을 해제하십시오. 
    ![6_db](/db-mig/db-create-5.png)

7. 마지막으로 **예상 월별 비용**을 검토하고 **Create database** 버튼을 클릭합니다.
   
   ![8_2_db](/db-mig/8_2_db.png)
