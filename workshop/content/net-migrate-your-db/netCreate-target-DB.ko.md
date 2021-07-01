+++
title = "타겟 DB 생성"
weight = 15
pre = "<b>1.1 </b>"
+++


이 시나리오에서는 소스 SQL 서버에 대해 "기본 백업 복원 (Native Backup Restore)" 마이그레이션 방식을 사용합니다.

전제 조건으로 네트워크 연결이 필요합니다. 실제 시나리오에서도 복제하기 위해소스 및 타겟 환경 간의 네트워크 연결이 필요합니다. 이 워크숍에서는 소스 및 타겟 환경간에 VPC 피어링을 구축했습니다.

{{% notice note %}}
워크샵 수행을 위해서 필요한 Security Group은 생성이 되었습니다.
{{% /notice %}}
![Replication Instance Architecture](/db-mig/vpc-peering.png)

기본 백업 및 복원을 설정하려면 다음 세 가지 구성 요소가 필요합니다.

1. 백업 파일을 저장할 Amazon S3 버킷
2. 버킷에 액세스 하기위한 AWS Identity and Access Management (IAM) 역할
3. DB 인스턴스의 SQLSERVER_BACKUP_RESTORE 옵션이 추가된 옵션 그룹

### 1. 백업 파일을 저장할 S3 버킷 생성

- S3 콘솔로 이동하여 버킷의 고유 이름 (rds-backup-your initials-date)을 입력하고 create를 클릭하고 메모장에 버킷 이름을 저장합니다. (다음 단계에서 필요함)

 ![1](/db-mig/net-create-s3-rds.png)

### 2. 타겟 데이터베이스를 위한 옵션 그룹 생성

DB 인스턴스에서 기본 백업 및 복원을 활성화하려면, DB 인스턴스 옵션 그룹에 SQLSERVER_BACKUP_RESTORE 옵션을 추가합니다. 자세한 내용 및 지침은 <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.SQLServer.Options.BackupRestore.html" target="_blank">SQL Server의 기본 백업 및 복원 지원</a>을 참조하세요.

1. RDS 콘솔로 이동
2. 왼쪽 메뉴에서 **Option groups**을 선택하고 **Create Group**을 클릭합니다.
3. name (예 : unicorndb-sql-option)을 지정하고 description을 동일하게 지정합니다.
4. **Engine**에서 **sqlserver-ex**를 선택 하십시오.
5. "Major Engine Version"에서 "13.00"을 선택 합니다.
6. **Create** 을 클릭합니다.
    ![1](/db-mig/net-option-1.png)
7. Option Groups에서 생성된 Option Group을 선택하고 아래와 같이 "Add option"를 클릭합니다.
    ![1](/db-mig/net-option-2.png)
8. Add Option 화면의 드롭 다운 메뉴에서 “SQLSERVER_BACKUP_RESTORE” 옵션을 선택합니다.
9. IAM Role 섹션에서 "Create new role"을 선택하고 역할 이름을 지정합니다 (예 : SqlBackupRestoreIamRole).
10. S3 Destination 섹션에서 아래와 같이 메뉴에서 이전 단계에서 생성 한 S3 버킷 (rds-backup-xxxxxx)을 선택합니다.
11. "Add Option"을 클릭 합니다.
    ![1](/db-mig/net-option-3.png)

### 타겟 데이터베이스를 위한 서브넷 그룹 생성

{{% notice note %}}
Java 실습 영역의 DB 실습을 수행 하셨다면 ( 이미 서브넷 그룹을 생성 한 경우 ) 동일한 서브넷 그룹을 사용할 수 있습니다.
{{% /notice %}}

1. **AWS Console**로 이동하여 **Services**에서 **RDS**를 선택하고 왼쪽 메뉴에서 **Subnet Group**을 선택한 다음 **Create DB Subnet Group**을 클릭합니다.

2. **Create DB subnet group** 에 다음 정보를 입력합니다. 

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Name                | database-subnet-group     |
    | Description         | Subnets where RDS will be deployed |
    | VPC      | TargetVPC            |

    **Add subnets** 패널에서 각 가용 영역 (us-west-2a 및 us-west-2b)의 서브넷 CIDR 10.1.101.0/24 및 10.1.201.0/24를 추가하고, **Create** 버튼을 클릭합니다.

    ![RDS Subnet group creation](/db-mig/net-db-subnet-group.png)

### 타겟 데이터베이스 생성

1. 왼쪽 메뉴에서 **Database** 를 선택하고 **Create Database** 를 클릭합니다.

2. **Engine options**에서 Microsoft SQL Server를 선택하고 아래와 같이 버전 세부 정보를 선택합니다.

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Edition                | SQL Server Express Edition     |
    | Version         | SQL Server 2016 13.00.5820.21.v1 |

    ![1](/db-mig/net-create-db-1.png)

3. **Template** 섹센에서 "Free Tier"를 선택 합니다.

    ![Free tier template selection](/db-mig/db-template.png)

    {{% notice note %}}
"Free Tier" 템플릿을 선택하면 마법사의 다음 단계에서 옵션이 제한되므로 여러분은 AWS 프리 티어 한도 내에 유지됩니다.
{{% /notice %}}

4. **Settings** 섹션에서 새 데이터베이스 인스턴스에 대한 DB 인스턴스 식별자 (unicorn), 마스터 사용자 이름 (unicorn) 및 마스터 암호 (awsrocks2021)를 구성합니다.

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | DB Instance Identifier                | unicorn (Java 실습에서 unicorn DB를 생성했다면,  “unicornsql” 을 입력)   |
    | Master Username         | unicorn |
    | Master password         | awsrocks2021 |

    ![3_db](/db-mig/db-create-2.png)

{{% notice note %}}
해당 정보는 나중에 사용하게 됩니다. **Master username** 과 **Master password**를 기록해 둡니다.
{{% /notice %}}

5. Burstable DB 인스턴스 클래스에서 **db.t2.micro**를 선택하고 스토리지 유형으로 **General Purpose (SSD)**을 선택하고 "Enable storage autoscaling"의 선택을 취소합니다. (이 데이터베이스에 대해 20GB 이상의 스토리지가 필요하지 않습니다.)

    ![4_db](/db-mig/4_db.png)

{{% notice note %}}
프로덕션 워크로드의 경우 <a href="https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html" target="_blank">다중 AZ 배포</a>를 활성화하여 더 높은 가용성을 제공할 수 있게 대기 인스턴스를 활성화하는 것이 좋습니다.
{{% /notice %}}

6. **Connectivity** section에서
    - Virtual Private Cloud (VPC)에서  **TargetVPC** (이 실습을 위해 자동 생성된 <a href="https://aws.amazon.com/vpc/" target="_blank">Amazon Virtual Private Cloud</a>입니다.) 를 선택합니다.
        ![6_db](/db-mig/db-create-3.png)
    - **Additional connectivity configuration** >>  VPC Security Group에서 **Existing VPC security group** 아래 드롭다운 메뉴에서 “SQLRDSSG” 보안 그룹을 선택 합니다.
        **Database port**를 1433으로 지정합니다.
    - Note 이전에 생성 한 DB 서브넷 그룹이 자동으로 선택됩니다
        ![6_db](/db-mig/net-create-db-2.png)
{{% notice note %}}
Note: SQLRDSSG 보안 그룹은 이미 생성되었으며 소스 데이터베이스 및 소스 웹 서버의 트래픽을 허용합니다.
{{% /notice %}}
7. Additional configuration 섹션에서 앞서 생성 한 옵션 그룹 (‘unicorndb-sql-option-group’)을 선택합니다.
8. U래와 같이 Backup, Performance insights 및 Monitoring 의 선택을 취소합니다.
   ![6_db](/db-mig/net-create-db-3.png)

9. 마지막으로 **예상 월간 비용**을 검토하고 **Create database**  버튼을 클릭합니다.

   ![8_2_db](/db-mig/8_2_db.png)

10. 데이터베이스를 만드는 데 몇 분이 걸립니다. "Databases" 섹션에서 상태를 확인할 수 있습니다. 준비가 되면 아래와 같이 상태가 "Available"으로 표시되어야합니다.
    - 아래 화면처럼  DB이름을 클릭합니다.

    ![6_db](/db-mig/net-create-db-4.png)

11. **Connection & Security** Tab 에서 DB의 엔드 포인트 URL을 복사하고 기록해 둡니다. 다음 섹션에서이 End Point URL을 사용하여 데이터베이스를 연결합니다.
   ![6_db](/db-mig/net-create-db-5.png)
