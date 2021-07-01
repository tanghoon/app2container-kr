+++
title = "Limpieza"
weight = 300
pre = "<b></b>"
+++

Si ejecutó el taller en su propia cuenta, al final asegúrate de borrar _todos_ los recursos que has creado, incluyendo:

1. En <a href="https://us-west-2.console.aws.amazon.com/rds/home?region=us-west-2#databases:" target="_blank">AWS RDS Console</a>
   - Eliminar las **bases de datos creadas** (sin snapshot final y sin conservar las copias de seguridad automatizadas)
    ![RDS Confirm Deletion](/cleanup/db-delete-confirm.en.png)

2. En <a href="https://us-west-2.console.aws.amazon.com/dms/v2/home?region=us-west-2#replicationInstances" target="_blank">AWS DMS Console</a>
   - Detenga y (cuando este en estado **Stopped**) - borrar las **tasks**. *Esperar a que se borre la tarea*.
   - Eliminar los **endpoints** creados. *Esperar a que los **endpoints** se borren*.
   - Borrar las **instancias de replicación**. *Esperar a que se eliminen las **instancias de replicación***.
   - Borrar el **subnet group**.

     Vaya a <a href="https://us-west-2.console.aws.amazon.com/dms/v2/home?region=us-west-2#dashboard" target="_blank">AWS DMS Dashboard</a> para confirmar que todo fue borraro (ausmiendo que no usaste DMS antes, deberías ver 0's en todas partes, como en la captura de pantalla de abajo).
     ![DMS Dashboard confirmation](/cleanup/dms-dashboard-final.en.png)

3. En <a href="https://us-west-2.console.aws.amazon.com/efs/home?region=us-west-2" target="_blank">Amazon EFS Console</a>
   - Eliminar el **Elastic File Systems** creado.

4. En <a href="https://us-west-2.console.aws.amazon.com/ecs/home?region=us-west-2#/getStarted" target="_blank">AWS ECS</a>
   - Eliminar los **services** creados.
   - Eliminar las **task definitions** creadas.
   - Eliminar los **clusters** creados.
   - Eliminar los **ECR registries** creados.

5. En <a href="https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#Home:" target="_blank">AWS EC2 Console</a>
   - Terminar las **EC2 machines** creadas.
   - Borrar los **Load balancers** creados.

6. En <a href="https://s3.console.aws.amazon.com/s3/" target="_blank">AWS S3</a>
   - Eliminar los **S3 buckets** creados.

7. En <a href="https://console.aws.amazon.com/iam/" target="_blank">AWS IAM console</a>
   - Eliminar los **IAM users/roles** creados.

8. En <a href="https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks" target="_blank">AWS CloudFormation</a>
   - Borrar el stack **app2container workshop**.

Cuando se elimina el stack **App2ContainerWorkshop**, revise su cuenta de AWS y **elimine todos los recursos restantes que se crearon durante este laboratorio**.

Además, le agradeceríamos que proporcionara <a href="https://eventbox.dev/survey/QRHSVTI">comentarios anónimos para este taller</a>.
