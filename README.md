markdown# CI/CD Pipeline with GitHub Actions + Docker + AWS EC2

End-to-end automation project: Code push → Docker build → Docker Hub push → Auto deploy to EC2

## Day 1: Dockerize the Application
**Goal:** Create Docker image of static website

**Steps:**
1. Created `Dockerfile` using nginx:alpine base image
2. Copied static HTML files to `/usr/share/nginx/html`
3. Exposed port 80
4. Tested locally:
   ```bash
   docker build -t day1-repo-site .
   docker run -p 80:80 day1-repo-siteVerified at http://localhost:80Day 2: Push to Docker Hub
Goal: Manually build and push image to Docker HubSteps:Created Docker Hub account: rajuraichalwarLogged in via CLI: docker loginTagged image: docker tag day1-repo-site rajuraichalwar/day1-repo-site:latestPushed: docker push rajuraichalwar/day1-repo-site:latestVerified image at Docker Hub registryDay 3: GitHub Actions CI/CD Pipeline
Goal: Automate Docker build + push on every commitSteps:Created .github/workflows/docker.ymlConfigured workflow triggers on push to main branchAdded steps:Checkout codeSet up Docker BuildxLogin to Docker Hub using GitHub SecretsBuild and push image automaticallyAdded secrets: DOCKER_USERNAME, DOCKER_PASSWORDTested: Pushed code → Workflow ran → Image updated on Docker HubWorkflow file: .github/workflows/docker.ymlDay 4: Auto Deploy to AWS EC2
Goal: SSH into EC2 and deploy new image automaticallySteps:Launched Ubuntu 22.04 EC2 instance on AWSOpened Security Group: Port 22 SSH, Port 80 HTTPInstalled Docker on EC2Added GitHub Secrets:EC2_HOST = EC2 Public IPEC2_USERNAME = ubuntuEC2_SSH_KEY = .pem key contentUpdated workflow with SSH deploy step using appleboy/ssh-actionDeploy commands:bash   docker pull rajuraichalwar/day1-repo-site:latest
   docker stop myapp || true
   docker rm myapp || true
   docker run -d --name myapp -p 80:80 rajuraichalwar/day1-repo-site:latestTested: Git push → Full pipeline ran → Site live at http://EC2_PUBLIC_IPTech Stack
CI/CD: GitHub ActionsContainerization: Docker, Docker HubCloud: AWS EC2 UbuntuWeb Server: Nginx AlpineAutomation: SSH via appleboy/ssh-actionLive Demo
http://13.232.201.21What I Learned
Docker image creation and containerizationDocker Hub registry managementGitHub Actions YAML workflow syntaxSecrets management in GitHubEC2 instance setup and Docker installationAutomated deployment via SSH in CI/CDEnd-to-end DevOps pipeline designjavascript
### **Bonus: Add this badge on top**
```markdown
![CI/CD](https://github.com/rajuraichalwar/day1-repo-site/actions/workflows/docker.yml/badge.svg)
