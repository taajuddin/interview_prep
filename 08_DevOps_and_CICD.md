# DevOps & CI/CD

## Q1: What AWS services are used in CI/CD pipelines?
- **CodeCommit** → Git repository.  
- **CodeBuild** → Build service.  
- **CodeDeploy** → Deployment automation.  
- **CodePipeline** → CI/CD orchestration.  

---

## Q2: Hands-on – Create CodePipeline (YAML).
```yaml
version: 1
phases:
  build:
    commands:
      - npm install
      - npm test
artifacts:
  files:
    - '**/*'
```

---

## Q3: Blue/Green Deployment in AWS.
- Two environments (Blue = current, Green = new).  
- Traffic gradually shifted using **Elastic Beanstalk**, **CodeDeploy**, or **ALB**.  

---

## Q4: Scenario – Auto-deploy container app.
**Answer:**  
- Store code in **CodeCommit**.  
- Build Docker image with **CodeBuild**.  
- Push to **ECR**.  
- Deploy to **ECS/EKS** using **CodePipeline**.  
