# 01-01-2026[EKS]
[morning-slot]
- Develop endpoints in vpc module
- It will support both interface and gateway endpoints
- This feature was conditionally enabled
- Why I'm creating this endpoint resource
  - My EKS cluster pull the images from ECR repo wihtout going to internet So no NAT anf IGW
  - Backups stores in s3  so we need gate way endpoint
- need to be tested this feature
  - don't create no endpoints
  - only create Gateway Endpoint
  - only create Interface Endpoint
[night-slot]