Linux --> Scripting --> Git --> Ansible

AWS --> Terraform --> Jenkins/GithubActions

Docker --> Kubernetes --> Observability

# Day 0 – Design & Architecture
- What problem am I solving?
- Which AWS services?
- Why ECS vs EKS?
- Public or private?
- Cost, security, scalability
📌 No code yet. Only thinking + drawing.

# Day 1 – Develop
- App code
- Dockerfile
- Configs
- Feature flags, env vars
📌 Local testing, Docker, basic validation.

# Day 2 – Implement
- Terraform / CloudFormation
- CI pipeline
- Deploy to AWS
- Networking, IAM, secrets
📌 This is pure DevOps muscle.

# Operate – Everyday
- Logs (CloudWatch / ELK)
- Metrics (Prometheus / Grafana)
- Alerts
- Scaling
- Incidents
- Cost optimization
📌 This is where seniors are made.

Must follow this rules
1. What problem am I solving?
2. How company will use this?
3. How I implemented (steps)
4. What went wrong / lessons

**Example**
***Problem:***
Need to run backend app without managing servers.

Design:
Use ALB + ECS + RDS.
Private subnets for ECS.
Public for ALB.

Implementation:
- Docker build
- Push image to ECR
- Create task definition
- Create service with ALB

Operate:
- Logs in CloudWatch
- Auto scaling CPU based
- Rolling deployments

*Interview is a discussion about my work*

# Interviews
## Rule 1: Never go unstructured
1. Problem
2. Decision
3. Implementation
4. Result / Issue
# notebook
