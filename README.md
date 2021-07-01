**App2container Migration Workshop** - https://app2container.workshop.aws/en/

Self-paced activity that allows you to learn how to migrate applications to AWS cloud:  
  - Contanierzing of your Java and .NET applications with [AWS App2Container](https://aws.amazon.com/app2container/)
  - Re-hosting of webserver with [AWS CloudEndure Migration](https://aws.amazon.com/cloudendure-migration/)  
  - Re-platforming of database with [AWS Database Migration Service](https://aws.amazon.com/dms/)  
  - Modernization of webserver to containers running on [Amazon Elastic Container Service](https://aws.amazon.com/ecs/)   
 
This repository includes:
 - **App2Container Workshops ** instructions deployed to https://app2container.workshop.aws/en/
 - **App2container** resources (in **./resources** folder) used to deploy the source environment (for the self-paced mode)   
 
   - An Amazon VPC (Virtual Private Cloud) with a NAT Gateway
   - An Amazon EC2 machine with Tomcat8 Web Server (ubuntu)  
   - An Amazon EC2 machine with PostgreSQL database (ubuntu)
   - An Amazon EC2 machine with IIS Web Server (Windows 2012)
   - An Amazon EC2 machine with SQL Server (Windows 2012)
   - An Amazon EC2 machine for operations- Worker Machine (Windows 2012)
   - Two AWS Lambda Functions (one generating EC2 Key Pair, one retrieving the EC2 Key Pair)   
   - Amazon API Gateway as the entry point for lambdas   

**Contributions**

Make sure you include the following text in your pull request "By submitting this pull request, I confirm that you can use, modify, copy, and redistribute this contribution, under the terms of your choice."
