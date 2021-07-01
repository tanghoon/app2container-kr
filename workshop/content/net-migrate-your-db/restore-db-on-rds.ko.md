+++
title = "RDS에서 DB 복원"
weight = 30
pre = "<b>1.3 </b>"
+++



1. RDS 서비스로 이동하여 이전에 생성한 데이터베이스를 선택합니다.
2. **Connectivity & Secuirity** 탭에서 DB의 엔드 포인트 URL을 복사하고 기록해 둡니다.
   ![6_db](/db-mig/net-create-db-5.png)
3. 소스 DB 서버에서 SQL Studio의 왼쪽 위 모서리에있는 "Connect"를 클릭합니다.
    ![6_db](/db-mig/net-restore-1.png)
4. 연결 창에서 아래 매개 변수를 입력하십시오.

    | Parameter           | Value                    |
    | ------------------- | ------------------------ |
    | Server name                | RDS DB Endpoint URL    |
    | Authentication         | SQL Server Authentication |
    | Login      | unicorn (RDS DB 생성시 다른 사용자를 사용했다면 해당 계정을 입력합니다.)             |
    | Password      | awsrocks2021  (RDS DB 생성시 다른 암호를 사용했다면 해당 암호를 입력합니다.)         |

    ![6_db](/db-mig/net-restore-2.png)

5. 연결이 되면 아래와 같이 왼쪽 창에 RDS DB가 표시됩니다.
    ![6_db](/db-mig/net-restore-3.png)
6. 왼쪽 창에서 데이터베이스를 클릭하고 아래와 같이 "New Query"를 클릭합니다.
    ![6_db](/db-mig/net-restore-8.png)
7. Query 화면에서 아래 명령어를 붙여넣고“Execute”버튼을 클릭합니다.
    - 아래의 S3 버킷 이름을 이전 섹션에서 생성 한 버킷 이름으로 바꿉니다.

{{% notice note %}}
다른 RDS DB 이름으로 정의한 경우. 아래의 명령에서 restore_db_name을 변경 하십시오. (예 : "unicornsql")
{{% /notice %}}  


```powershell
exec msdb.dbo.rds_restore_database
    @restore_db_name='unicorn',
    @s3_arn_to_restore_from='arn:aws:s3:::<<insert name of your s3 bucket>>/MySiteDB.bak';
```

쿼리를 실행하면 RESTORE_DB 작업이 생성 된 것을 볼 수 있습니다.
    ![6_db](/db-mig/net-restore-4.png)

8. 다른 쿼리 화면을 열고 아래 명령을 실행하여 작업 상태를 확인하십시오. 아래와 같이 RESTORE_DB 작업이 성공한 것을 볼 수 있습니다.

- 상태가 "in progress"로 표시되면 몇 분 후에 쿼리를 다시 실행하십시오.

  ```powershell
  exec msdb.dbo.rds_task_status @db_name='unicorn';
  ```

![6_db](/db-mig/net-restore-5.png)
9. 왼쪽 창에 생성 한 백업 파일에서 unicorn DB가 복원 된 것을 볼 수 있습니다
![6_db](/db-mig/net-restore-6.png)

### 축하합니다.

소스 DB에서 타겟 RDS 데이터베이스로 성공적으로 복원을 수행 하였습니다. 이제 다음 섹션으로 이동하여 트래픽을 RDS 데이터베이스로 전달하도록 애플리케이션 연결 문자열을 업데이트합니다.
