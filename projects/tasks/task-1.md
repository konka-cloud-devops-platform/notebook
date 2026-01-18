# Creating Platform image

Checkout
↓
Hadolint
↓
Docker Build
↓
Trivy Scan
↓
Generate SBOM
↓
Docker Push
↓
Cosign Sign


1. First create seperate repos for each and every platform image
2. Place Jenkins file on each and every platform image repo
3. Create a folder called platform-images
4. Create Jenkins pipeline for each and every platform image
5. Define Jenkins Shared lib above stages
6. Configure Global pipelines in Jenkins
    name: shared-libs
    Default Version: main
        Allow default version to be overridden   
        Include @Library changes in job recent changes 
        tick these 2 options
    Retrieval method
    Modern SCM
    Source Code Management
     git 
     repo url
