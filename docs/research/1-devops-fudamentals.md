# Research: DevOps Fundamentls

## 1. Introduction

What is this technology or concept?
Basic fundamentals of DevOps Metodologie and his tools.
DevOps its a work culture. Its a set of practices, philosophies and tools that combine software development and it operations.
Born for improve the team workout for software delivery, having his own key elements like

- Collab, transparency and shared responsability
- Agility, Automation and secure integration


## 2. Core Concepts

Their core principle are CALMS
C -> Culture
A -> Automation
L -> LEAN
M -> MEASUREMENT
S -> SHARING

## 3. How It Works

DevOps have multiple stages like:

### Plan Stage
We define goals, requirements and task breakdown here and with this definitions we can align the development and operations process

### Code Stage
Here we write, review and manage the source application code and configurations using version control like Git and his Hubs like Github, GitLab and another provider.
The version control allow us to mantain the quality of the develop code, stability and traceability of the project

### Build Stage
In this stage we can automaticallyh compile and packaging code into artifact to be deploy

### Test Stage
We prove the quality, invest in the security and check the performance of the deployed artifact also identify bugs if the app have

### Release Stage
Finalizing and documenting approved builds for deployment through version tagging and strategy planning.
We can use tools like Git tags, jenkins, etc.

### Deploy Stage
Push the application into production enviroments, we can use deployment strategies for mount or rolling up broke images

### Operate and Monitor Stage
Maintaining system health and gathering real-world performance data to drive continuous improvement.

## 4. Practical Applications

How can this concept be applied in real infrastructure environments?
This part involves turn de manual operation into automated one with collaborative workflows

We also have some practices for make this happen like Infrastructure as Code (IaC), Containerization, Orchest and Automated Scaling, CI/CD Pipelines, Monitoring, etc.

## 5. Advantages and Limitations

Identify its strengths, weaknesses, and potential trade-offs.
We can improve the code and software delivery performance with a better collaboration using DevOps Tools. Improving the releases time and MVPs delivery using CI/CD Strategies and automated process
Automated testing helps detect defects early in the development lifecycle, reducing human errors and improving software reliability.

But DevOps are complex. The implementation requires and expert and if the team doesnt have the workflow and tools probably turns into hard work to apply it. DevOps means work faster than traditional and the security could be affected if we doesnt invest time and test in ci/cd pipelines

To sum up some trade off of devops:
##### - Speed deliver vs security: The pressure of delivery code continuously could skip some securiry test
##### - High complex tooling and cost: Build and mantaing the chains of ci/cd, IaC and Cloud tools needs an economic high investment
##### - Cultural breakdown: Break down traditional workflow between devs and ops could show resistance and a lack of clear definitions. Probably need a strong senior leadership
##### - Automatition overload: Automating broken or complex processes without clear metrics can create a false sense of security and multiply errors in production on a large scale.

## 6. Comparison and Alternatives

When comparing DevOps with traditional software development and operations, we can identify several improvements in collaboration, automation and delivery frequency.

CI/CD pipelines allow teams to reduce manual intervention and shorten software delivery cycles. Instead of relying on large and infrequent releases, organizations can deliver smaller and more frequent updates.

However, DevOps requires organizational changes, technical knowledge and continuous investment in automation and monitoring.

## 7. Application in ForgeOps

ForgeOps aims to implement a centralized DevOps workflow using different devops philosophies, culture and automation tooling

We will use ansible for allow us to configure the infrastructure automation and manage them. Allowing to maintaing multiple resources

Docker will be or containarization tool combining with Git For control and collab development

Aim to future implementation technology like terraform for IaC and jenkins for CI/CD Pipelines and testing

## 8. Personal Conclusions

What did you learn? - I learned the basics and cost of have a DevOps expert in my team and all the advantages of the work culture. Not only of the practical tooling, i learned how to improve my way of development and changing it. I understood the importance of this role and have and expertise. This will improve the quality and speed delivery of my team

## 9. References

- [Microsoft DevOps Fundamentals](https://learn.microsoft.com/es-es/training/paths/devops-foundations-core-principles-practices/)
- [What is DevOps?](https://www.youtube.com/watch?v=kBV8gPVZNEE)
- [Geeks For Geeks - DevOps](https://www.geeksforgeeks.org/devops/introduction-to-devops/)
- [Advantages and Limitations](https://vivasoftltd.com/advantages-and-disadvantages-of-devops/)