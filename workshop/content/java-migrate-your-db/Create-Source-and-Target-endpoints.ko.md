+++
title = "소스 / 타겟 엔드포인트 생성"
weight = 30
pre = "<b>1.4 </b>"
+++




AWS 콘솔의 **AWS Database Migration Service** 화면으로 돌아가서 **Endpoints**를 선택 하고 **Create endpoint** 버튼을 클릭합니다.

### 1. Source Endpoint 생성

- 다음의 파라미터를 설정하여 endpoint를 구성 합니다.

    | Parameter           | Value                                          |
    | ------------------- | ---------------------------------------------- |
    | Endpoint type       | Source endpoint                                |
    | Endpoint identifier | source-PostgreSQL-endpoint                                |
    | Source engine       | PostgreSQL                                          |
    | Server name         | 10.0.0.140                                            |
    | Port                | 5432                                           |
    | SSL mode            | none                                           |
    | User name           | unicorn                                 |
    | Password            | awsrocks2021                                   |
    | Database Name            | unicorn                                   |

    ![source-endpoint](/db-mig/source-endpoint-1.png)

    ![source-endpoint](/db-mig/source-endpoint-2-privateip.png)

    **Test endpoint connection (옵션사항)** 섹션을 연 다음 **VPC** 드롭다운에서 **TargetVPC**를 선택하고 **Run test** 버튼을 클릭하여 endpoint 구성이 올바른지 확인합니다.

    ![test-source-endpoint](/db-mig/source-endpoint-3.png)

    테스트는 짐시 실행되며(약 1분) **Status**컬럼에 **successful** 메시지가 표시됩니다. **Create endpoint**  버튼을 클릭하여 endpoint를 생성합니다.

    오류가 발생 하는 경우, endpoint의 파라미터 변수가 올바르게 구성 되었는지를 확인하고 **Publicly Accessible** 파라미터 변수를 선택한 상태로 복제 인스턴스를 생성했는지 확인합니다.

### 2. Target Endpoint 생성

- 모든 단계를 반복하여 다음 파라미터 변수 값으로 Target Endpoint를 생성합니다.
  

    | Parameter           | Value                                                 |
    | ------------------- | ----------------------------------------------------- |
    | Endpoint type       | Target endpoint                                       |
    | Select RDS DB instance | 체크                                            |
    | RDS Instance        | 드롭다운에서 RDS 인스턴스를 선택합니다(값이 표시되지 않는 경우 수동으로 값 입력).          |
    | Endpoint Identifier | unicorn                                       |
    | Target Engine       | PostgreSQL (사전 입력)                                                |
    | Server Name         | DNS name of your RDS database (사전 입력된 값을 유지)                             |
    | Port                | 5432     (사전 입력)                                             |
    | SSL mode            | none                                                  |
    | User name           | (사전 입력 값 유지)                                                 |
    | Password            | 사용하게 될 RDS 데이터베이스 암호| (awsrocks2021)

![source-endpoint](/db-mig/target-endpoint-1.png)

- "Provide access information manually"를 선택 합니다.
![source-endpoint](/db-mig/target-endpoint-2.png)

- **Test endpoint connection (옵션사항)**의 **VPC** 드롭다운에서 **TargetVPC**를 선택하고 **Run test** 버튼을 클릭하여 endpoint가 유효한지 확인합니다.

![source-endpoint](/db-mig/target-endpoint-3.png)
    테스트는 짐시 실행되며(약 1분) **Status**컬럼에 **successful** 메시지가 표시됩니다. **Create endpoint**  버튼을 클릭하여 endpoint를 생성합니다.

{{% notice note %}}
**Actions** 버튼을 클릭한 다음 **Test connection**을 클릭하여 **Endpoints** 목록에서 추가 엔드포인트 연결 테스트를 수행할 수 있습니다.
{{% /notice %}}
