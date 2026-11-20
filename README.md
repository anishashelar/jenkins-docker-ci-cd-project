CI/CD Pipeline with Jenkins, Docker & GitHub on AWS EC2
Project Overview
This project demonstrates a complete CI/CD pipeline where application code is automatically built, packaged into a Docker container, and deployed using Jenkins on an AWS EC2 instance.

The pipeline integrates: • GitHub for source code management • Jenkins for automation • Docker for containerization • EC2 as the deployment environment The goal is to automate the process of building and deploying a web application whenever changes are pushed to the repository.

Source Code
The application code for this project is maintained in a separate repository: https://github.com/anishashelar/project2

This repository contains the website files and Docker configuration used in the CI/CD pipeline.

Architecture Overview
Developer pushes code to GitHub repository.
Jenkins detects changes (SCM trigger)
Jenkins pulls the latest code from GitHub.
Jenkins executes build steps.
Docker builds the application image.
Docker container is created and run on EC2.
Application is accessible via EC2 public IP/port.
Technologies Used
• AWS EC2 (Jenkins server + Docker host+ git server) • Jenkins (CI/CD automation) • Git & GitHub (version control) • Docker (containerization) • Linux (Amazon linux) • Shell scripting (build & deployment steps)

Architecture Diagram
Architecture Diagram (architecture.png)

Workflow
Code is pushed to GitHub repository
Jenkins job is triggered
Jenkins clones the repository
Jenkins executes build steps: • Stops existing container (if running) • Removes old container • Builds new Docker image • Runs new container on specified port
Updated application is deployed and accessible via browser
Docker Setup
• Docker is installed on the EC2 instance where Jenkins server is also present. • Application is containerized using a Dockerfile . • Jenkins executes Docker commands during build.

Jenkins Configuration
• Jenkins installed on EC2 instance • Git plugin configured for repository access • Freestyle job created to: o Pull code from GitHub o Execute shell commands o Build and run Docker container • Build steps include: o Git SCM configuration o Execute shell commands for Docker build & run

Jenkins Job Configuration
The Jenkins job was configured to pull code from GitHub and execute deployment steps using shell commands.

Build Commands (Execute Shell)
cd /myproject

docker stop mycontainer || true docker rm mycontainer || true

docker build -t myapp . docker run -d -p 80:80 --name mycontainer myapp

Access & Permissions Issue Resolution
During setup, Docker permission issues were encountered due to Jenkins user not having access to the Docker daemon This was resolved by: • Adding Jenkins user to Docker group • Restarting Jenkins service

Key Learnings
• CI/CD pipeline automation using Jenkins • Docker container lifecycle management • GitHub integration with Jenkins • Handling permission issues in Linux environments • Running services inside containers on EC2 • Deploying applications without manual intervention

Deployment Highlights
• Automated build and deployment pipeline • Dockerized application for consistency • Jenkins-based orchestration • GitHub as a single source of truth • EC2 used as a production-like environment

Outcome
• Successfully implemented a working CI/CD pipeline • Application gets deployed automatically after code changes • Reduced manual deployment effort • Gained hands-on experience with DevOps tools and workflows

Future Improvements
• Implement pipeline as code (Jenkinsfile) • Use Docker Hub or private registry for image storage • Add monitoring and logging • Integrate with Kubernetes for orchestration
