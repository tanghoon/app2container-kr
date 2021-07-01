+++
title = "복제 타스크 생성 및 실행"
weight = 40
pre = "<b>1.5 </b>"
+++

### 복제 타스크 생성 및 실행

**AWS DMS** 콘솔에서 **Database migration tasks**로 이동한 다음 **Create Task** 버튼을 클릭합니다.

1. **Create database migration task** 화면에서 다음 파라미터 변수 값을 입력하십시오.

    | Parameter              | Value                                               |
    | ---------------------- | --------------------------------------------------- |
    | Task identifier        | replication-cdc                                     |
    | Replication instance   | replication-instance                                |
    | Source endpoint        | source-postgre-endpoint                                     |
    | Target endpoint        | unicorn                                     |
    | Migration type         | Migrate existing data and replicate ongoing changes |

    ![Create-task-1](/db-mig/Create-task-1.png)

2. **Task settings** 패널에서는 다른 모든 항목을 다음과 같이 둡니다.

    ![create-task-2](/db-mig/java-create-task-2.png)

3. **Table mappings** 패널에서 **Wizard** 모드를 선택하고 **Add new selection rule** 버튼을 누른 다음 "Enter a schema"를 선택합니다.
    - 기본으로 입력된 값은 변경 하지 않습니다.

    ![Create-task-3](/db-mig/Create-task-3.png)

4. 화면 맨 아래로 스크롤하고 **Create task** 버튼을 클릭하여 태스크를 생성하고 데이터 복제를 시작합니다.

태스크가 실행되는 동안 해당 태스크의 세부 정보를 검토하여 복제된 테이블 및 행 수에 대한 정보를 볼 수 있습니다.

![create-task-4](/db-mig/Create-task-5.png)
