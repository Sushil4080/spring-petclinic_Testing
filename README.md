 Spring PetClinic -- DevSecOps CI/CD Project

**A hands-on DevOps and DevSecOps portfolio project based on a fork of
the Spring PetClinic sample application, extended with Jenkins and
GitHub Actions CI/CD automation.**

[GitHub
Repository](https://github.com/Sushil4080/spring-petclinic_Testing) \|
[GitHub
Actions](https://github.com/Sushil4080/spring-petclinic_Testing/actions)
\|
[Workflows](https://github.com/Sushil4080/spring-petclinic_Testing/tree/main/.github/workflows)

------------------------------------------------------------------------

## 📌 Project Background

This repository is a **fork of the official Spring PetClinic sample
application**.

The original application source code and functionality are based on the
upstream Spring PetClinic project:

**Upstream:**
[spring-projects/spring-petclinic](https://github.com/spring-projects/spring-petclinic)

This fork is used for hands-on **DevOps and DevSecOps learning,
experimentation, CI/CD implementation, and portfolio demonstration**.

### What was implemented for this project

-   Jenkins CI/CD pipeline
-   GitHub Actions DevSecOps workflow
-   Self-hosted GitHub Actions runner on Ubuntu/Linux
-   Maven build and unit-test automation
-   SonarQube code-quality analysis
-   Trivy filesystem vulnerability scanning
-   Trivy Docker image vulnerability scanning
-   Docker image build and runtime validation
-   Application health checks
-   Automated cleanup
-   Kubernetes/Kind workflow files are present in the repository

------------------------------------------------------------------------

## 🚀 Project Overview

The project demonstrates a CI/CD workflow that moves a Java/Spring Boot
application through build, test, quality analysis, security scanning,
containerization, runtime validation, and cleanup.

### Overall Flow

    Source Code
        ↓
    Build
        ↓
    Unit Test
        ↓
    SonarQube
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

------------------------------------------------------------------------

## 🏗️ CI/CD Architecture

### Jenkins Pipeline

    GitHub Repository
           │
           ▼
        Jenkins
           │
           ▼
       Checkout
           │
           ▼
     Maven Build
           │
           ▼
      Unit Test
           │
           ▼
     SonarQube
           │
           ▼
    Trivy FS Scan
           │
           ▼
     Docker Build
           │
           ▼
    Trivy Image Scan
           │
           ▼
      Docker Run
           │
           ▼
     Health Check
           │
           ▼
       Cleanup

### GitHub Actions DevSecOps Pipeline

    GitHub Repository
           │
           ▼
     GitHub Actions
           │
           ▼
    Self-Hosted Ubuntu Runner
           │
           ▼
    Checkout Source Code
           │
           ▼
        Java 21
           │
           ▼
      Maven Build
           │
           ▼
       Unit Test
           │
           ▼
     SonarQube
           │
           ▼
    Trivy FS Scan
           │
           ▼
     Docker Build
           │
           ▼
    Trivy Image Scan
           │
           ▼
      Docker Run
           │
           ▼
     Health Check
           │
           ▼
       Cleanup

------------------------------------------------------------------------

## 🛠️ Technologies & Tools

  Category             Technology
  -------------------- --------------------------
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

------------------------------------------------------------------------

## 📁 Repository Structure

    spring-petclinic_Testing/
    ├── .github/
    │   └── workflows/
    │       ├── devsecops.yml
    │       ├── deploy-and-test-cluster.yml
    │       ├── gradle-build.yml
    │       └── maven-build.yml
    │
    ├── .devcontainer/
    ├── .mvn/
    │   └── wrapper/
    ├── gradle/
    │   └── wrapper/
    ├── k8s/
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

------------------------------------------------------------------------

## ⚙️ GitHub Actions Workflows

The repository contains multiple workflows under `.github/workflows/`.

### 1. devsecops.yml --- Custom DevSecOps Workflow

**File:** `.github/workflows/devsecops.yml`

This is the custom DevSecOps workflow implemented and tested for this
project.

    Checkout
       ↓
    Setup Java 21
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

## 🧪 Pipeline Validation

The custom GitHub Actions DevSecOps pipeline has been successfully executed end-to-end on the self-hosted Ubuntu runner.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/86f500b4-af8f-40c8-982d-898250f8d251" />

Validated stages:

...

#### Runner

The workflow uses:

    runs-on: [self-hosted, Linux, X64]

#### Maven Build

    chmod +x mvnw
    ./mvnw clean package -DskipTests

#### Unit Test

    ./mvnw test

#### SonarQube

The workflow uses the repository configuration:

    SONAR_TOKEN
    SONAR_HOST_URL

Project configuration:

  Setting         Value
  --------------- ----------------------------
  Project Key     `petclinic-github-actions`
  Project Name    `petclinic-github-actions`
  Sources         `src/main/java`
  Tests           `src/test/java`
  Java Binaries   `target/classes`

#### Trivy Filesystem Scan

    trivy fs \
      --severity HIGH,CRITICAL \
      --exit-code 0 \
      --no-progress \
      .

#### Docker Build

    docker build \
      -t petclinic-app:${GITHUB_RUN_NUMBER} \
      -t petclinic-app:latest \
      .

#### Trivy Docker Image Scan

    trivy image \
      --severity HIGH,CRITICAL \
      --exit-code 0 \
      --no-progress \
      ${IMAGE_NAME}:${GITHUB_RUN_NUMBER}

#### Docker Run

    docker run -d \
      --name petclinic-test \
      -p 8081:8080 \
      ${IMAGE_NAME}:${GITHUB_RUN_NUMBER}

#### Health Check

    curl -f http://localhost:8081/

#### Cleanup

    docker rm -f petclinic-test 2>/dev/null || true

### 2. maven-build.yml

**File:** `.github/workflows/maven-build.yml`

This is the repository\'s Maven-oriented GitHub Actions workflow. It is
separate from the custom `devsecops.yml` workflow.

### 3. gradle-build.yml

**File:** `.github/workflows/gradle-build.yml`

This is the repository\'s Gradle-oriented GitHub Actions workflow. It is
separate from the custom DevSecOps workflow.

### 4. deploy-and-test-cluster.yml

**File:** `.github/workflows/deploy-and-test-cluster.yml`

This workflow is present in the repository for Kubernetes cluster
deployment/testing using Kind.

> **Attribution note:** This workflow is documented because it exists in
> the fork. It should only be presented as newly implemented personal
> work if it has actually been created, modified, and tested by you.

------------------------------------------------------------------------

## 🤖 Jenkins Implementation

The repository includes the Jenkins pipeline configuration:

    Jenkinsfile

The Jenkins implementation demonstrates:

-   GitHub source-code checkout
-   Maven build
-   Unit testing
-   SonarQube integration
-   Trivy security scanning
-   Docker image build
-   Docker image scanning
-   Docker runtime validation
-   Application health check
-   Cleanup

### Jenkins Pipeline Flow

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

  ### Jenkins Pipeline Execution

The Jenkins pipeline successfully executes the CI/CD workflow from source checkout through container verification and cleanup.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3d315318-5488-406e-89f9-d001e33a8fbb" />


The `Jenkinsfile` keeps the Jenkins pipeline configuration
version-controlled with the application.

------------------------------------------------------------------------

## 🔐 DevSecOps Implementation

### SonarQube

SonarQube is used for automated code-quality analysis of the Java
application.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b1f1c219-1a19-4447-be15-acee085f4e78" />


### Trivy Filesystem Scan

Trivy scans the project filesystem for vulnerabilities, with the current
workflow focusing on:

-   HIGH
-   CRITICAL

### Trivy Docker Image Scan

After the Docker image is built, Trivy scans the container image for
vulnerabilities.

> **Current learning configuration:** `--exit-code 0` reports findings
> without failing the workflow. A production-style implementation can
> use a non-zero exit code to enforce a security gate.

------------------------------------------------------------------------

## 🐳 Docker Implementation

Application flow:

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

### Dockerfile

    FROM eclipse-temurin:21-jre

    WORKDIR /app

    COPY target/*.jar app.jar

    EXPOSE 8080

    ENTRYPOINT ["java", "-jar", "app.jar"]

------------------------------------------------------------------------

## 🧪 Pipeline Validation

The custom GitHub Actions DevSecOps pipeline has been successfully
executed end-to-end on the self-hosted Ubuntu runner.

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

------------------------------------------------------------------------

## 🖥️ Self-Hosted GitHub Actions Runner
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/df29bbee-ef74-4671-b5b7-fd8e6e854a01" />


The custom DevSecOps workflow uses a self-hosted runner with:

    self-hosted
    Linux
    X64

The runner provides access to locally installed tools such as:

-   Java
-   Maven
-   Docker
-   Trivy
-   SonarQube

------------------------------------------------------------------------

## 🔗 SonarQube Configuration

The GitHub Actions workflow expects:

    SONAR_TOKEN
    SONAR_HOST_URL

Secrets and connection information should never be committed to Git.

> **Security:** Store tokens and credentials using GitHub Actions
> Secrets, Jenkins Credentials, or another appropriate
> secrets-management system.

------------------------------------------------------------------------

## ☸️ Kubernetes

The repository contains:

    k8s/

and an existing Kubernetes/Kind workflow:

    .github/workflows/deploy-and-test-cluster.yml

### Extendable Deployment Flow

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

------------------------------------------------------------------------

## 🧰 Local Prerequisites

-   Git
-   Java 21
-   Docker
-   Trivy
-   Jenkins, for Jenkins pipeline testing
-   SonarQube, for local SonarQube testing
-   kubectl / Kind, for Kubernetes workflow testing

The Maven wrapper is included in the repository.

------------------------------------------------------------------------

## 🚀 Run the Application Locally

    git clone https://github.com/Sushil4080/spring-petclinic_Testing.git
    cd spring-petclinic_Testing

    chmod +x mvnw

    ./mvnw clean package

    ./mvnw spring-boot:run

Open:

    http://localhost:8080/

------------------------------------------------------------------------

## 🐳 Build and Run with Docker

    ./mvnw clean package -DskipTests

    docker build -t petclinic-app:latest .

    docker run -d \
      --name petclinic-test \
      -p 8081:8080 \
      petclinic-app:latest

    docker ps

    curl -f http://localhost:8081/

    docker logs petclinic-test

    docker rm -f petclinic-test

------------------------------------------------------------------------

## 🔍 Run Trivy Locally

### Filesystem Scan

    trivy fs \
      --severity HIGH,CRITICAL \
      --no-progress \
      .

### Docker Image Scan

    trivy image \
      --severity HIGH,CRITICAL \
      --no-progress \
      petclinic-app:latest

------------------------------------------------------------------------

## 📸 Project Screenshots

Recommended screenshots can be stored under:

    docs/images/
    ├── jenkins-pipeline.png
    ├── github-actions-pipeline.png
    └── sonarqube-dashboard.png

Then add them to this README:

    ![Jenkins Pipeline](docs/images/jenkins-pipeline.png)

    ![GitHub Actions Pipeline](docs/images/github-actions-pipeline.png)

    ![SonarQube Dashboard](docs/images/sonarqube-dashboard.png)

> Before committing screenshots, make sure passwords, tokens,
> credentials, private URLs, and other sensitive information are not
> visible.

------------------------------------------------------------------------

## 🎯 Key Learning Outcomes

-   CI/CD pipeline design
-   Jenkins Pipeline automation
-   GitHub Actions workflow automation
-   Self-hosted GitHub Actions runners
-   Linux-based CI/CD environments
-   Maven build automation
-   Java application packaging
-   Unit-test automation
-   SonarQube integration
-   Trivy filesystem scanning
-   Trivy Docker image scanning
-   Docker image creation
-   Docker container execution
-   Application health checks
-   CI/CD troubleshooting and failure analysis
-   DevSecOps practices
-   Kubernetes deployment/testing concepts

------------------------------------------------------------------------

## 🚧 Future Improvements

-   Docker image push to Docker Hub
-   Amazon ECR integration
-   Automated Kubernetes deployment
-   Helm charts
-   Argo CD / GitOps
-   Kubernetes readiness and liveness probes
-   Prometheus monitoring
-   Grafana dashboards
-   Deployment rollback
-   Slack/email notifications
-   AWS deployment
-   Stronger vulnerability gates
-   Container image signing
-   SBOM generation
-   Dependency scanning
-   Centralized secrets management
-   Separate development, staging, and production environments

------------------------------------------------------------------------

## 🔗 Repository Links

-   **Main Repository:**
    [spring-petclinic_Testing](https://github.com/Sushil4080/spring-petclinic_Testing)
-   **GitHub Actions Workflows:**
    [.github/workflows](https://github.com/Sushil4080/spring-petclinic_Testing/tree/main/.github/workflows)
-   **DevSecOps Workflow:**
    [devsecops.yml](https://github.com/Sushil4080/spring-petclinic_Testing/blob/main/.github/workflows/devsecops.yml)
-   **Jenkinsfile:**
    [Jenkinsfile](https://github.com/Sushil4080/spring-petclinic_Testing/blob/main/Jenkinsfile)

------------------------------------------------------------------------

## 🙏 Credits & Attribution

This project is based on the official **Spring PetClinic** sample
application.

**Original upstream project:**\
<https://github.com/spring-projects/spring-petclinic>

The original application source code and functionality are attributed to
the upstream Spring PetClinic project and its contributors.

This repository is a fork created for personal DevOps/DevSecOps
learning, experimentation, CI/CD implementation, and portfolio
demonstration.

The CI/CD and DevSecOps work described in this README is separate from
the original application\'s purpose and documentation.

------------------------------------------------------------------------

## 📄 License

The underlying Spring PetClinic application is distributed under the
**Apache License 2.0**.

Please refer to `LICENSE.txt` and the upstream Spring PetClinic
repository for complete licensing information.

------------------------------------------------------------------------

## ⭐ Final Summary

This repository demonstrates a practical CI/CD and DevSecOps workflow
around a Spring Boot application.

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

The goal of this repository is to demonstrate **practical DevOps and
DevSecOps engineering through a working CI/CD project**, while clearly
attributing the underlying Spring PetClinic application to its original
open-source project.
