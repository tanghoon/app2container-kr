+++
title = "Modernization Overview"
weight = 4
+++

There is no single path that our customers take when they modernize on AWS, but once you start modernizing your applications you get many important benefits like below:

1. **Agility**: Develop and deploy faster to achieve business goals.
2. **Enterprise DevOps**: Build and operate utilizing proven ecosystem of cloud-native tooling.
3. **Portable & Isolated**: Enable portable, scalable and isolated application deployment.
4. **Operational Efficiency**: Reduce IT operational overhead and achieve optimized compute infrastructure.

For this workshop, you have identified 2 applications in your company. And thought they would be good candidates for modernization.

- A. Java Tomcat Application
- B. .NET Application

-------------
But you have few challenges:

1. The applications are old and the application owner had left the company years ago without leaving any documentation behind.
2. You/your team is not expert on containerization process, thus, you are not feeling comfortable with touching these applications.
3. You/your team is recently started your Cloud Journey with AWS and you want to be sure whether you are following the Cloud Best practices while deploying the applications into the Cloud.
4. You/your team is very busy and cant spend too much time to containerize these applications.

-------------
With these challenges in mind:

- In this workshop, you will use AWS migration and Modernization tools to help your modernization journey.
- For each application, you will use different migration and modernization tools based on your use-case and requirements:
    1. <a href="https://aws.amazon.com/dms/" target="_blank">Database Migration Service (DMS)</a> to migrate your Database to AWS RDS (Relational Database Service).
    2. <a href="https://aws.amazon.com/app2container" target="_blank">AWS App2Container (A2C)</a> to Analyze, Containerize, Deploy your Application to Amazon ECS (Elastic Container Service).

#### Now, navigate to [Getting Started]({{< ref "getting-started/_index.en.md" >}}) and choose your application (Java or .NET) to start modernizing.

{{% notice note %}}
Each Application follows similar processes, so you can follow each application independent to each other.
{{% /notice %}}
