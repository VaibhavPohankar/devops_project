Automated Java CI/CD Pipeline (Docker + Jenkins + AWS)
This repository demonstrates a professional, end-to-end DevOps lifecycle for a Java application. It covers everything from local environment consistency with Vagrant to automated cloud deployment on AWS EC2.

Project Architecture
The pipeline follows a standard "Build-Test-Deploy" workflow:
1.Source Control: GitHub (Version Control)
2.Local Development: Vagrant for consistent local environment testing.
3.CI/CD Orchestration: Jenkins (Declarative Pipeline).
4.Build Tool: Maven (with strict Java 17 Enforcer rules).
5.Containerization: Docker with multi-stage builds and non-root security.
6.Registry: Doker Hub for versioned image storage.
7.Production: AWS EC2 with CloudWatch performance monitoring.

Tech Stack & Specifications
*Language: Java 17 (Eclipse Temurin)
*Build Tool: Maven 3.9.8
*CI/CD: Jenkins (Pipeline-as-Code)
*Infrastructure: AWS (EC2, VPC, Security Groups)
*Container: Docker (Multi-stage, Slim JRE)

Monitoring: AWS CloudWatch
Key Engineering Features
*Multi-Stage Docker Build: Separates the Maven build environment from the final Runtime environment, significantly reducing image size and the attack surface.
*Non-Root Execution: The container runs under a custom devopsuser rather than root for enhanced security.
*Dynamic Tagging: Every deployment is uniquely tagged using ${BUILD_NUMBER}, ensuring 100% traceability from the running container back to the Jenkins build.
*Maven Enforcer: Strictly requires Java 17+ and Maven 3.8.6+ to prevent "it works on my machine" inconsistencies.
*Port Mapping: Internal service on 9090 is seamlessly mapped to standard HTTP Port 80 on the AWS host.

Proof of Deployment
The live URL is no longer accessible as the instance has been terminated to save costs. However, you can find all the deployment results and proof in the `Screenshots/` folder of this repository.

How to Run Locally
1. Prerequisites

1.Docker installed
2.Maven 3.8.6+
3.Java 17

2.Build and Test
# Compile and run JUnit 5 tests
mvn clean package

3. Containerize & Run
# Build the Docker image
docker build -t your-docker-user/vibh-app:v1 .
# Run the container (Mapping Port 80 to 9090)
docker run -d -p 80:9090 your-docker-user/vibh-app:v1

Repository Structure
```
.
├── jenkins/
│   └── Jenkinsfile         # CI/CD Logic & Credentials Management
├── src/
│   ├── main/java/...       # Java HTTP Server (Native)
│   └── test/java/...       # JUnit 5 Logic Tests
├── Dockerfile              # Multi-stage & Secure Build
├── pom.xml                 # Dependency & Plugin Management
├── Vagrantfile             # Infrastructure-as-Code for Local Dev
├── .gitignore              # Secure exclusion of secrets/binaries
└── Screenshots/            # Technical proof of deployment
```

Author
Vaibhav Pohankar Cloud & DevOps Engineer | AWS | Docker | Jenkins
