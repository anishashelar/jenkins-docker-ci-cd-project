# CI/CD Pipeline with Jenkins, Docker & GitHub on AWS EC2

In this project, we build a complete **CI/CD pipeline** where application code is automatically built, packaged into a Docker container, and deployed using Jenkins on an AWS EC2 instance.

The goal is to automate building and deploying a web application whenever changes are pushed to the repository.

---

## Architecture Diagram

![CI/CD Architecture](architecture.png)

---

## Project Overview

The pipeline integrates:

- **GitHub** for source code management
- **Jenkins** for automation
- **Docker** for containerization
- **AWS EC2** as the deployment environment


## Technologies Used

- **AWS EC2**: Jenkins server + Docker host + Git server
- **Jenkins**: CI/CD automation
- **Git & GitHub**: version control
- **Docker**: containerization
- **Linux (Amazon Linux)**: operating system
- **Shell scripting**: build and deployment steps

---

## How the Pipeline Works

1. Developer pushes code to the GitHub repository.
2. Jenkins detects the change (SCM trigger).
3. Jenkins pulls the latest code from GitHub.
4. Jenkins executes the build steps.
5. Docker builds the application image.
6. A Docker container is created and run on EC2.
7. The application is accessible via the EC2 public IP and port.

---

## Setup Steps

### Step 1: Set Up the EC2 Instance

A single EC2 instance (Amazon Linux) acts as the Jenkins server, the Docker host and the Git server. Everything in the pipeline runs on this one machine.

### Step 2: Install and Configure Docker

- Docker is installed on the same EC2 instance where Jenkins runs.
- The application is containerized using a **Dockerfile**.
- Jenkins executes Docker commands during the build.

### Step 3: Install and Configure Jenkins

- Jenkins is installed on the EC2 instance.
- The **Git plugin** is configured for repository access.

### Step 4: Create the Jenkins Freestyle Job

A Freestyle job is created to:

- Pull code from GitHub
- Execute shell commands
- Build and run the Docker container

The build steps include:

- Git SCM configuration
- Execute Shell commands for Docker build and run

**Screenshot: Jenkins Job Configuration**

![Jenkins Job Configuration](Screenshots/step18-jenkins-dashboard.png)

> The job is configured to pull code from GitHub and run the deployment steps through an Execute Shell build step.

### Step 5: Add the Build Commands (Execute Shell)

```bash
cd /myproject
docker stop mycontainer || true
docker rm mycontainer || true
docker build -t myapp .
docker run -d -p 80:80 --name mycontainer myapp
```

What each command does:

| Command | Purpose |
|---------|---------|
| `docker stop mycontainer \|\| true` | Stops the existing container (if running) without failing the build |
| `docker rm mycontainer \|\| true` | Removes the old container (if it exists) |
| `docker build -t myapp .` | Builds a new Docker image from the Dockerfile |
| `docker run -d -p 80:80 --name mycontainer myapp` | Runs the new container on port 80 |

### Step 6: Fix the Access & Permissions Issue

During setup, Docker permission issues appeared because the **Jenkins user did not have access to the Docker daemon**.

This was resolved by:

1. Adding the Jenkins user to the Docker group
2. Restarting the Jenkins service

### Step 7: Run the Build

Trigger the job (or push a change to GitHub). Jenkins clones the repository, stops and removes the old container, builds the new image and runs the new container.

**Screenshot: Successful Jenkins Build**

![Jenkins Build Success](screenshots/jenkins-build-success.png)

> The console output shows the build finishing with **SUCCESS**, confirming the Docker image was built and the container started.

### Step 8: Verify the Running Container

On the EC2 instance, confirm the container is up:

**Screenshot: Running Docker Container**

![Docker Container Running](screenshots/docker-container-running.png)

> `docker ps` shows `mycontainer` running with port 80 mapped.

### Step 9: Check the Application in the Browser

Open the EC2 public IP in a browser. The updated application is deployed and accessible.

**Screenshot: Application Output**

![Application Output](screenshots/application-output.png)

> The website is served from the Docker container on EC2. It works! 🚀

---

## Deployment Highlights

- Automated build and deployment pipeline
- Dockerized application for consistency
- Jenkins-based orchestration
- GitHub as a single source of truth
- EC2 used as a production-like environment

---

## Key Learnings

- CI/CD pipeline automation using Jenkins
- Docker container lifecycle management
- GitHub integration with Jenkins
- Handling permission issues in Linux environments
- Running services inside containers on EC2
- Deploying applications without manual intervention

---

## Outcome

- Successfully implemented a working CI/CD pipeline
- Application gets deployed automatically after code changes
- Reduced manual deployment effort
- Gained hands-on experience with DevOps tools and workflows

---

## Future Improvements

- Implement pipeline as code (Jenkinsfile)
- Use Docker Hub or a private registry for image storage
- Add monitoring and logging
- Integrate with Kubernetes for orchestration


