+++
title = "1.3 RDS에 DB 복원"
chapter = true
weight = 63

+++
1. RDS 서비스로 이동하여 이전에 생성한 데이터베이스를 선택합니다.

2. **Connectivity & Secuirity** 탭에서 DB의 엔드 포인트 URL을 복사하고 기록해 둡니다.

   <img src="/images/c/net-create-db-5 (1).png" alt="net-create-db-5 (1).png" style="zoom:100%;" />

3. 원본 DB 서버에서 SQL Studio의 왼쪽 위 모서리에있는 연결을 클릭합니다.

   <img src="/images/c/net-restore-1.png" alt="net-restore-1.png" style="zoom:100%;" />

4. 연결 창에서 아래 매개 변수를 입력하십시오.

   | 파라메타       | 값                                                           |
   | :------------- | :----------------------------------------------------------- |
   | Server name    | RDS DB Endpoint URL                                          |
   | Authentication | SQL Server Authentication                                    |
   | Login          | unicorn (If you set a different user while creating your RDS DB, enter it here instead.) |
   | Password       | awsrocks2021 (If you set a different password while creating your RDS DB, enter it here instead.) |

   <img src="/images/c/net-restore-2.png" alt="net-restore-2.png" style="zoom:100%;" />

5. 연결되면 아래와 같이 왼쪽 창에 RDS DB가 표시됩니다.

   <img src="/images/c/net-restore-3.png" alt="net-restore-3.png" style="zoom:100%;" />

6. 왼쪽 창에서 데이터베이스를 클릭하고 아래와 같이 "New Query"를 클릭합니다.

   <img src="/images/c/net-restore-8.png" alt="net-restore-8.png" style="zoom:100%;" />

   

7. Query 화면에서 아래 명령어를 붙여넣고“Execute”버튼을 클릭합니다.

   1. Replace the S3 bucket name below with your bucket name you’ve created in previous section.

      {{% notice note%}}이전에 생성 한 DB 서브넷 그룹이 자동으로 선택됩니다.{{% /notice %}}
      
      ```
      exec msdb.dbo.rds_restore_database
          @restore_db_name='unicorn',
          @s3_arn_to_restore_from='arn:aws:s3:::<<insert name of your s3 bucket>>/MySiteDB.bak';
      ```
      
      쿼리를 실행하면 RESTORE_DB 작업이 생성 된 것을 볼 수 있습니다.
      
      <img src="/images/c/net-restore-4.png" alt="net-restore-4.png" style="zoom:100%;" />
      
      

8. 다른 쿼리 화면을 열고 아래 명령을 실행하여 작업 상태를 확인하십시오. 아래와 같이 RESTORE_DB 작업이 성공한 것을 볼 수 있습니다.

   1. 상태가 "PROGRESS"로 표시되면 몇 분 후에 쿼리를 다시 실행하십시오.

      ```
      exec msdb.dbo.rds_task_status @db_name='unicorn';
      ```

      <img src="/images/c/net-restore-5.png" alt="net-restore-5.png" style="zoom:100%;" />

9. 왼쪽 창에 생성 한 백업 파일에서 unicorn DB가 복원 된 것을 볼 수 있습니다.
   <img src="/images/c/net-restore-6.png" alt="net-restore-6.png" style="zoom:100%;" />

## 축하합니다.

대상 RDS 데이터베이스에서 원본 DB를 성공적으로 복원했습니다. 이제 다음 섹션으로 이동하여 트래픽을 RDS 데이터베이스로 전달하도록 애플리케이션 연결 문자열을 업데이트합니다.
