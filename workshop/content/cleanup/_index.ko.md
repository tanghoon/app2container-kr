+++
title = "Cleanup"
weight = 300
pre = "<b></b>"
+++

자신의 AWS 계정에서 워크샵 실행의 경우, 다음을 포함하여 생성한 *모든* 리소스를 삭제해야 합니다.

1. <a href="https://us-west-2.console.aws.amazon.com/rds/home?region=us-west-2#databases:" target="_blank">AWS RDS 콘솔</a> 작업
   - **databases** 삭제
    ![RDS Confirm Deletion](/cleanup/db-delete-confirm.en.png)

2. <a href="https://us-west-2.console.aws.amazon.com/dms/v2/home?region=us-west-2#replicationInstances" target="_blank">AWS DMS 콘솔</a> 작업
   -  중단 ( Stopped 상태가 될때 ) 후,  **tasks**. *삭제 - 삭제 완료 될때까지 대기*.
   -  **endpoints**. *삭제 - 삭제 완료 될때까지 대기*.
   -  **replication instances**. *삭제 - 삭제 완료 될때까지 대기*.
   -  **subnet group** 삭제

     <a href="https://us-west-2.console.aws.amazon.com/dms/v2/home?region=us-west-2#dashboard" target="_blank">AWS DMS 대시보드</a> 로 이동하여 DMS관련 모든 자원이 삭제 되었는지 확인을 수행 합니다.  (이전에 DMS를 사용하지 않았다고 가정하면 아래 스크린샷처럼 모든 곳에 0이 표시됩니다.)
     ![DMS Dashboard confirmation](/cleanup/dms-dashboard-final.en.png)

3. <a href="https://us-west-2.console.aws.amazon.com/efs/home?region=us-west-2" target="_blank">Amazon EFS Console</a> 작업
   - **Elastic File Systems** 삭제

4. <a href="https://us-west-2.console.aws.amazon.com/ecs/home?region=us-west-2#/getStarted" target="_blank">AWS ECS</a> 작업
   -  **services** 삭제
   -  **task definitions** 삭제
   -  **clusters** 삭제
   -  **ECR registries** 삭제

5. <a href="https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#Home:" target="_blank">AWS EC2 Console</a> 작업
   -  **EC2 machines** 삭제(터미네이트)
   -  **Load balancers** 삭제

6. <a href="https://s3.console.aws.amazon.com/s3/" target="_blank">AWS S3</a> 작업
   -  **S3 buckets** 삭제

7. <a href="https://console.aws.amazon.com/iam/" target="_blank">AWS IAM console</a> 작업
   -  **IAM users/roles** 삭제

8. <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks" target="_blank">AWS CloudFormation</a> 작업
   - **app2container workshop** stack 삭제

**App2ContainerWorkshop** 스택이 삭제 되면, 실습자의 AWS 계정을 다시 확인하고 **이 실습 과정에서 생성 된 모든 나머지 자원을 삭제합니다 .**.

추가적으로 <a href="https://eventbox.dev/survey/QRHSVTI">익명의 워크샵 피드백</a>을 남겨주시면 감사하겠습니다.
