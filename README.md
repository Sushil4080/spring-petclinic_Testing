Spring PetClinic -- DevSecOps CI/CD Project

A hands-on DevOps and DevSecOps portfolio project based on a fork of
the Spring PetClinic sample application, extended with CI/CD
automation using Jenkins and GitHub Actions.











📌 Project Background

This repository is a fork of the official Spring PetClinic sample
application.

The original application source code and functionality are based on the
upstream Spring PetClinic project:

Upstream repository:
https://github.com/spring-projects/spring-petclinic

This fork is used for hands-on DevOps and DevSecOps learning,
experimentation, CI/CD implementation, and portfolio demonstration.

What this project focuses on

The main focus of this repository is the automation implemented around
the application:

Jenkins CI/CD

GitHub Actions CI/CD

Self-hosted GitHub Actions runner on Ubuntu/Linux

Maven build and test automation

SonarQube code-quality analysis

Trivy security scanning

Docker image build and runtime validation

Kubernetes/Kind workflow files already present in the repository

Attribution: The application itself is based on the upstream
Spring PetClinic project. The DevOps/DevSecOps automation described
below should be considered separately from the original application.

🚀 Project Overview

The project demonstrates a CI/CD workflow that moves an application
through:

Source Code
    ↓
Build
    ↓
Unit Test
    ↓
Code Quality
    ↓
Security Scan
    ↓
Docker Build
    ↓
Docker Image Scan
    ↓
Application Run
    ↓
Health Check
    ↓
Cleanup

Two CI/CD implementations are represented in the repository:

Jenkins

GitHub Actions

🏗️ CI/CD Architecture

Jenkins Pipeline

The Jenkins implementation is used to demonstrate the following CI/CD
and DevSecOps flow:

GitHub
  ↓
Jenkins
  ↓
Checkout
  ↓
Build
  ↓
Unit Test
  ↓
SonarQube Analysis
  ↓
Trivy Security Scan
  ↓
Docker Build
  ↓
Docker Image Scan
  ↓
Docker Run
  ↓
Health Check
  ↓
Cleanup

GitHub Actions DevSecOps Pipeline

The custom devsecops.yml workflow runs on a self-hosted Linux runner:

GitHub
  ↓
GitHub Actions
  ↓
Self-Hosted Ubuntu Runner
  ↓
Checkout Source Code
  ↓
Java 21
  ↓
Maven Build
  ↓
Unit Test
  ↓
SonarQube Analysis
  ↓
Trivy Filesystem Scan
  ↓
Docker Build
  ↓
Trivy Docker Image Scan
  ↓
Docker Run
  ↓
Health Check
  ↓
Cleanup

🛠️ Technologies & Tools

Category             Technology

Source Control       Git, GitHub
CI/CD                Jenkins, GitHub Actions
Runner               Self-hosted Ubuntu/Linux
Application          Spring Boot / Java
Build                Maven
Code Quality         SonarQube
Security Scanning    Trivy
Containerization     Docker
Kubernetes Testing   Kubernetes / Kind
Operating System     Ubuntu / Linux
Scripting            Shell

📁 Repository Structure

The repository currently contains application code, CI/CD workflows,
Kubernetes-related files, Docker configuration, and build configuration.

spring-petclinic_Testing/
│
├── .github/
│   └── workflows/
│       ├── devsecops.yml
│       ├── deploy-and-test-cluster.yml
│       ├── gradle-build.yml
│       └── maven-build.yml
│
├── .devcontainer/
│
├── .mvn/
│   └── wrapper/
│
├── gradle/
│   └── wrapper/
│
├── k8s/
│
├── src/
│   ├── main/
│   └── test/
│
├── Dockerfile
├── docker-compose.yml
├── pom.xml
├── build.gradle
├── settings.gradle
├── mvnw
├── mvnw.cmd
├── gradlew
├── gradlew.bat
├── Jenkinsfile
└── README.md

If the Jenkinsfile is maintained outside the repository in your
Jenkins configuration, remove that entry. If it is committed in the
repository, keep it.

⚙️ GitHub Actions Workflows

The repository contains multiple workflows under:

.github/workflows/

1. devsecops.yml

File:

.github/workflows/devsecops.yml

This is the custom DevSecOps workflow implemented for this project.

Runner

The workflow uses:

runs-on: [self-hosted, Linux, X64]

The workflow therefore executes on the configured self-hosted
Ubuntu/Linux runner.

Pipeline stages

Checkout
   ↓
Setup Java
   ↓
Maven Build
   ↓
Unit Test
   ↓
SonarQube
   ↓
Trivy Filesystem Scan
   ↓
Docker Build
   ↓
Trivy Docker Image Scan
   ↓
Docker Run
   ↓
Health Check
   ↓
Cleanup

Maven Build

chmod +x mvnw
./mvnw clean package -DskipTests

The application is packaged first so that the JAR is available to the
Docker build.

Unit Test

./mvnw test

SonarQube

The workflow uses:

SONAR_TOKEN
SONAR_HOST_URL

and analyzes:

Sources:
src/main/java

Tests:
src/test/java

Java Binaries:
target/classes

Project key:

petclinic-github-actions

Trivy Filesystem Scan

trivy fs \
  --severity HIGH,CRITICAL \
  --exit-code 0 \
  --no-progress \
  .

Docker Build

docker build \
  -t petclinic-app:${GITHUB_RUN_NUMBER} \
  -t petclinic-app:latest \
  .

Trivy Docker Image Scan

trivy image \
  --severity HIGH,CRITICAL \
  --exit-code 0 \
  --no-progress \
  ${IMAGE_NAME}:${GITHUB_RUN_NUMBER}

Docker Run

docker run -d \
  --name petclinic-test \
  -p 8081:8080 \
  ${IMAGE_NAME}:${GITHUB_RUN_NUMBER}

Health Check

curl -f http://localhost:8081/

If the health check fails, the workflow prints the container logs and
exits with an error.

Cleanup

docker rm -f petclinic-test 2>/dev/null || true

The cleanup step runs with:

if: always()

so the test container is removed even when an earlier stage fails.

2. maven-build.yml

File:

.github/workflows/maven-build.yml

This is the repository's Maven-oriented GitHub Actions workflow.

It is separate from the custom devsecops.yml workflow.

3. gradle-build.yml

File:

.github/workflows/gradle-build.yml

This is the repository's Gradle-oriented GitHub Actions workflow.

It is separate from the custom DevSecOps workflow.

4. deploy-and-test-cluster.yml

File:

.github/workflows/deploy-and-test-cluster.yml

This workflow is present in the repository for Kubernetes cluster
deployment/testing using Kind.

The repository history indicates that this workflow was added for
Kubernetes deployment testing.

This workflow is documented here because it exists in the fork. It
should only be presented as newly implemented personal work if it has
actually been created, modified, and tested by you.

🤖 Jenkins Implementation

The repository includes a Jenkins pipeline configuration:

Jenkinsfile

The Jenkins implementation demonstrates CI/CD automation using Jenkins
with the following concepts:

GitHub source-code checkout

Maven build

Unit testing

SonarQube integration

Trivy security scanning

Docker image build

Docker image scanning

Docker runtime validation

Application health check

Cleanup

Jenkins Pipeline Flow

Checkout
   ↓
Build
   ↓
Unit Test
   ↓
SonarQube Analysis
   ↓
Trivy Security Scan
   ↓
Docker Build
   ↓
Trivy Docker Image Scan
   ↓
Docker Run
   ↓
Health Check
   ↓
Cleanup

Jenkinsfile

The pipeline definition is maintained as:

Jenkinsfile

This allows the CI/CD configuration to be version-controlled alongside
the application.

🔐 DevSecOps Implementation

Security and quality checks are integrated into the CI/CD process.

SonarQube

SonarQube is used for automated code-quality analysis.

The pipeline analyzes:

Java source code

Test source configuration

Java compiled classes

Project code-quality information

Trivy Filesystem Scan

Trivy scans the project filesystem for vulnerabilities.

Current severity focus:

HIGH
CRITICAL

Trivy Docker Image Scan

After the Docker image is built, Trivy scans the container image for
vulnerabilities.

This provides a security check before the image is executed.

The current learning pipeline uses --exit-code 0, so the scan
reports findings without making HIGH/CRITICAL findings fail the
workflow. This can be changed to a blocking security gate in a future
production-style implementation.

🐳 Docker Implementation

The CI/CD workflow packages the application into a Docker image.

The application flow is:

Maven Build
    ↓
target/*.jar
    ↓
Docker Build
    ↓
petclinic-app:<run-number>
    ↓
Trivy Image Scan
    ↓
Docker Run
    ↓
Health Check

Dockerfile

The current Docker approach uses the generated Maven JAR:

FROM eclipse-temurin:21-jre

WORKDIR /app

COPY target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]

🧪 Pipeline Validation

The custom GitHub Actions DevSecOps pipeline has been successfully
executed end-to-end on the self-hosted Ubuntu runner.

Validated stages:

✅ Checkout Source Code
✅ Setup Java
✅ Maven Build
✅ Unit Test
✅ SonarQube Analysis
✅ Trivy Filesystem Scan
✅ Docker Build
✅ Trivy Docker Image Scan
✅ Docker Run
✅ Health Check
✅ Cleanup

This validates the complete workflow from source checkout through
application runtime verification.

🖥️ Self-Hosted GitHub Actions Runner

The custom DevSecOps workflow uses a self-hosted runner with:

self-hosted
Linux
X64

The runner is hosted on an Ubuntu/Linux environment and provides access
to locally installed tools including:

Java

Maven

Docker

Trivy

SonarQube

This setup is also useful when CI jobs need access to services running
on the local development environment.

🔗 SonarQube Configuration

The GitHub Actions workflow uses repository-level configuration for the
SonarQube connection.

The workflow expects:

SONAR_TOKEN
SONAR_HOST_URL

The SonarQube project configuration used by the custom workflow is:

Project Key:
petclinic-github-actions

Project Name:
petclinic-github-actions

Sources:
src/main/java

Tests:
src/test/java

Java Binaries:
target/classes

Security: Never commit SonarQube tokens, passwords, API keys, or
other credentials into the repository. Store secrets using GitHub
Actions Secrets or Jenkins Credentials.

☸️ Kubernetes

The repository contains:

k8s/

and an existing Kubernetes/Kind-related GitHub Actions workflow:

.github/workflows/deploy-and-test-cluster.yml

This provides a foundation for extending the current Docker-based CI/CD
workflow into automated Kubernetes deployment.

Extendable deployment flow

Source Code
    ↓
Build
    ↓
Unit Test
    ↓
SonarQube
    ↓
Trivy
    ↓
Docker Build
    ↓
Container Registry
    ↓
Kubernetes
    ↓
Deployment
    ↓
Service
    ↓
Health Verification

🧰 Local Prerequisites

For local reproduction, install the tools required for the part of the
project you want to run:

Git

Java 21

Docker

Trivy

Jenkins, for Jenkins pipeline testing

SonarQube, for local SonarQube testing

kubectl / Kind, for Kubernetes workflow testing

The Maven wrapper is included in the repository, so Maven does not have
to be installed separately for the Maven build.

🚀 Run the Application Locally

Clone the repository:

git clone https://github.com/Sushil4080/spring-petclinic_Testing.git
cd spring-petclinic_Testing

Make the Maven wrapper executable:

chmod +x mvnw

Build the application:

./mvnw clean package

Run the application:

./mvnw spring-boot:run

Access the application:

http://localhost:8080/

🐳 Build and Run with Docker

Build the application JAR:

./mvnw clean package -DskipTests

Build the Docker image:

docker build -t petclinic-app:latest .

Run the container:

docker run -d \
  --name petclinic-test \
  -p 8081:8080 \
  petclinic-app:latest

Check the container:

docker ps

Test the application:

curl -f http://localhost:8081/

View logs:

docker logs petclinic-test

Remove the test container:

docker rm -f petclinic-test

🔍 Run Trivy Locally

Filesystem Scan

trivy fs \
  --severity HIGH,CRITICAL \
  --no-progress \
  .

Docker Image Scan

trivy image \
  --severity HIGH,CRITICAL \
  --no-progress \
  petclinic-app:latest

📸 Project Screenshots

Screenshots can be stored under:

docs/images/

Recommended screenshots:

Jenkins Pipeline

docs/images/jenkins-pipeline.png

![Jenkins Pipeline](docs/images/jenkins-pipeline.png)

GitHub Actions Successful Pipeline

docs/images/github-actions-pipeline.png

![GitHub Actions Pipeline](docs/images/github-actions-pipeline.png)

SonarQube Dashboard

docs/images/sonarqube-dashboard.png

![SonarQube Dashboard](docs/images/sonarqube-dashboard.png)

Before committing screenshots, make sure no passwords, tokens, private
URLs, credentials, or other sensitive information are visible.

🎯 Key Learning Outcomes

This project provides hands-on experience with:

CI/CD pipeline design

Jenkins Pipeline automation

GitHub Actions workflow automation

Self-hosted GitHub Actions runners

Linux-based CI/CD environments

Maven build automation

Java application packaging

Unit-test automation

SonarQube integration

Trivy filesystem scanning

Trivy Docker image scanning

Docker image creation

Docker container execution

Application health checks

Pipeline troubleshooting

CI/CD failure analysis

DevSecOps practices

Kubernetes deployment/testing concepts

🚧 Future Improvements

The project can be extended with:

Docker image push to Docker Hub

Amazon ECR integration

Automated Kubernetes deployment

Helm charts

Argo CD / GitOps

Kubernetes readiness probes

Kubernetes liveness probes

Prometheus monitoring

Grafana dashboards

Deployment rollback

Slack/email notifications

AWS deployment

Stronger vulnerability gates

Container image signing

SBOM generation

Dependency scanning

Centralized secrets management

Separate development, staging, and production environments

📌 Project Highlights

                       DEVSECOPS PROJECT
                              │
             ┌────────────────┴────────────────┐
             │                                 │
          Jenkins                       GitHub Actions
             │                                 │
             └────────────────┬────────────────┘
                              ↓
                         Maven Build
                              ↓
                          Unit Test
                              ↓
                          SonarQube
                              ↓
                       Trivy FS Scan
                              ↓
                        Docker Build
                              ↓
                    Trivy Image Scan
                              ↓
                        Docker Run
                              ↓
                       Health Check
                              ↓
                          Cleanup

🔗 Repository Links

Main Repository

https://github.com/Sushil4080/spring-petclinic_Testing

GitHub Actions Workflows

https://github.com/Sushil4080/spring-petclinic_Testing/tree/main/.github/workflows

DevSecOps Workflow

https://github.com/Sushil4080/spring-petclinic_Testing/blob/main/.github/workflows/devsecops.yml

Jenkinsfile

https://github.com/Sushil4080/spring-petclinic_Testing/blob/main/Jenkinsfile

🙏 Credits & Attribution

This project is based on the official Spring PetClinic sample
application.

Original upstream project

https://github.com/spring-projects/spring-petclinic

The original application source code and functionality are attributed to
the upstream Spring PetClinic project and its contributors.

This repository is a fork created for personal DevOps/DevSecOps
learning, experimentation, CI/CD implementation, and portfolio
demonstration.

The CI/CD and DevSecOps work described in this README is separate from
the original application's purpose and documentation.

📄 License

The underlying Spring PetClinic application is distributed under the
Apache License 2.0.

Please refer to:

LICENSE.txt

and the upstream Spring PetClinic repository for complete licensing
information.

⭐ Final Summary

This repository demonstrates a practical CI/CD and DevSecOps workflow
around a Spring Boot application.

The project brings together:

Git
  +
GitHub
  +
Jenkins
  +
GitHub Actions
  +
Self-Hosted Linux Runner
  +
Java 21
  +
Maven
  +
SonarQube
  +
Trivy
  +
Docker
  +
Kubernetes / Kind

The primary DevSecOps workflow automates:

Checkout
   ↓
Build
   ↓
Test
   ↓
Code Quality
   ↓
Security Scan
   ↓
Docker Build
   ↓
Container Scan
   ↓
Runtime Validation
   ↓
Health Check
   ↓
Cleanup

The goal of this repository is to demonstrate practical DevOps and
DevSecOps engineering through a working CI/CD project, while clearly
attributing the underlying Spring PetClinic application to its original
open-source project.
