### ❓ *“Explain your EKS architecture end-to-end for one application.”*

> “I designed and deployed a production-style application on **Amazon EKS** using a secure and scalable architecture.
>
> The infrastructure starts with a **custom VPC** having public and private subnets across multiple AZs. The EKS control plane is managed by AWS, and worker nodes run in private subnets using managed node groups.
>
> For traffic flow, users access the application through an **Application Load Balancer**, which is integrated with Kubernetes Ingress. The ALB routes traffic to a React frontend service, which then communicates with the Spring Boot backend service internally using Kubernetes services.
>
> The backend connects to **Amazon RDS** for MySQL and **Amazon ElastiCache** for Redis, both deployed as AWS-managed services outside the cluster for reliability and scalability.
>
> For configuration and security, I used ConfigMaps and Secrets, IAM Roles for Service Accounts for AWS access, and Kubernetes Network Policies to restrict pod-to-pod communication.
>
> CI/CD is implemented using GitHub Actions to build and push images to ECR, and deployments are handled through Helm and Argo CD following a GitOps approach. Monitoring is done using Prometheus and Grafana, with centralized logging for troubleshooting.”*

1. what type of cluster you design
2. network design
3. how applications are deployed
4. how the security was implemented[network-policies,istio,pod-identitiees]
5. observability
6. troubleshooting

- Network-isolated workloads
- GitOps-based deployments
- Service mesh security
- Full observability


