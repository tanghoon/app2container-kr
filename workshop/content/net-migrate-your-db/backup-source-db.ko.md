+++
title = "소스 DB 백업"
weight = 29
pre = "<b>1.2 </b>"
+++

데이터베이스에서 Native로 지원하는 .bak 파일을 사용하여 데이터베이스를 백업 및 복원하는 것은 일반적으로 데이터베이스를 백업 및 복원하는 가장 빠른 방법입니다.

이 섹션에서는 소스 데이터베이스를 백업하고 이전에 생성 한 S3 버킷에 업로드합니다.

1. [Windows 인스턴스 연결]({{< ref "/connect-windows.ko.md" >}}) 섹션에 설명 된대로 Source-NET-DB 서버에 연결합니다.
    - 아래와 같이 EC2 서비스로 이동하여 소스 DB 서버를 찾을 수 있습니다. 소스 DB 서버 이름은 "Source-NET-DB-Server"입니다.
   ![6_db](/db-mig/net-db-ec2.png)
{{% notice note %}}
[Windows 인스턴스 연결]({{< ref "/connect-windows.ko.md" >}}) 섹션에 설명 된대로 DB 서버 암호를 가져와야합니다.
{{% /notice %}}

2. 로그인 한 후 시작 메뉴에서 "Microsoft SQL Server Management Studio"를 클릭합니다.

   ![6_db](/db-mig/net-sql-1.png)
3. 연결 창에서 아래 매개 변수를 입력하십시오.

| Parameter           | Value                    |
| ------------------- | ------------------------ |
| Server name                | .    |
| Authentication         | SQL Server Authentication |
| Login      | sa            |
| Password      | password-1            |

   ![6_db](/db-mig/net-sql-2.png)
4. 데이터베이스 엔진에 연결되면 데이터베이스 아래의 "MySiteDB"를 마우스 오른쪽 버튼으로 클릭하고 아래와 같이 "Backup"을 선택합니다.
   ![6_db](/db-mig/net-sql-3.png)
5. 백업 설정 화면에서 백업 유형 "Full"을 선택했는지 확인하고 백업 파일 위치를 기록해 둡니다. 다음 단계에서 이 위치를 사용합니다.
   ![6_db](/db-mig/net-sql-4.png)
6. 백업이 완료되면 아래와 같은 알림이 표시됩니다.
   ![6_db](/db-mig/net-sql-5.png)
7. 이제 백업 파일이 있습니다. 하지만 백업 파일을 S3 버킷에 업로드하려면 Windows 서버 AWS 자격 증명을 구성해야합니다. 

8. 시작 메뉴에서 Windows Powershell 터미널을 실행하고 "aws s3 cp"명령을 실행하여 백업 파일을 S3 버킷에 복사합니다. 아래 명령에서 S3 버킷 이름을 바꿉니다.

   {{% notice note %}}
   AWSCLI가이 머신에 이미 설치되어 있으며,  EC2 인스턴스 역할을 사용하여 S3 버킷에 액세스합니다. 실제 통합에 대한 <a href="https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html" target="_blank">AWS CLI 세부 정보</a> 를 확인할 수 있습니다.
   {{% /notice %}}

   ```powershell
   aws s3 cp “C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\MySiteDB.bak” s3://<<insert name of your s3 bucket>>
   ```

9. 명령을 실행하고 S3 버킷으로 이동하면, 이 버킷에 업로드 된 MySiteDB.bak 파일이 표시됩니다.
   ![6_db](/db-mig/net-sql-6.png)
