# DevSecOps-CI-CD-Pipeline-on-AWS-EKS
This project engineers a complete DevSecOps pipeline, integrating security and best practices at every stage to automatically build, scan, and securely deploy a multi-tier application to a production-ready AWS EKS Kubernetes cluster provisioned with Terraform.


## Key Features
1. Provisioning the AWS EKS cluster and all necessary infrastructure using Terraform (IaC).

2. Setting up and configuring a robust CI/CD toolchain with Jenkins, Nexus, and SonarQube.

3. Automating the build, test, and security analysis of the multi-tier application.

4. Orchestrating secure deployments to the EKS cluster with Kubernetes RBAC (Role-Based Access Control).

5. Automating the build, test, artifact management, and deployment of a multi-tier application.

6. Establishing automated pipeline triggers using GitHub webhooks.

## Technologies Used

* **AWS (Amazon Web Services):** The cloud provider used to host all infrastructure and services (EC2, EKS, IAM).
* **Terraform:** Used for **Infrastructure as Code (IaC)** to provision the **AWS EKS Kubernetes cluster** and EC2 instances in a repeatable and automated manner.
* **Jenkins:** The central **CI/CD automation server** that orchestrates the entire pipeline, from code build to deployment.
* **Kubernetes (EKS):** The container orchestration platform used to deploy, manage, and scale the multi-tier application.
* **Maven:** The build automation tool used for compiling the Java application, running tests, and managing artifacts.
* **SonarQube:** Integrated for **Static Application Security Testing (SAST)** to analyze code quality, bugs, and vulnerabilities.
* **Trivy:** A versatile vulnerability scanner used for scanning the **file system**, **artifacts**, and **Docker images** for security issues.
* **Nexus Repository Manager:** Used for **artifact management** (both releases and snapshots), serving as a central repository for the built application's JAR files.
* **Docker:** Used to containerize the application and its dependencies into a portable image.
* **Groovy:** The language used to write the Jenkins Pipeline (`Jenkinsfile`).
* **Kubectl:** The command-line tool used to interact with the **Kubernetes** cluster for deployments and verification.
* **RBAC (Role-Based Access Control):** A **Kubernetes** security feature used to grant a service account the least privilege needed for deployment.
* **GitHub:** The version control system for source code management, used to trigger the pipeline via **webhooks**.



## Architecture / Workflow

The pipeline is designed to automate the software delivery lifecycle with integrated security.

1.  **Infrastructure Provisioning (IaC):** The entire **AWS** infrastructure, including a powerful **EKS Kubernetes cluster**, is provisioned from a central **VM** using **Terraform**. This **IaC** approach ensures a repeatable and consistent environment.
2.  **Toolchain Setup:** **Jenkins**, **SonarQube**, and **Nexus Repository Manager** are installed on dedicated **EC2** instances, forming the core of the **CI/CD** toolchain.
3.  **Continuous Integration (CI) Stages:**
    * A developer pushes code to the main branch of the **GitHub** repository. A **webhook** automatically triggers the **Jenkins** pipeline.
    * The pipeline performs a **Git checkout** to clone the repository.
    * **Maven** compiles the code, runs unit tests, and packages it into a **JAR** artifact.
    * **SonarQube** performs a static code analysis to check for bugs and code smells.
    * **Trivy** performs a file system scan to check for exposed credentials and configuration vulnerabilities.
    * The application's **JAR** artifact is published to the **Nexus Repository Manager**.
4.  **Continuous Deployment (CD) Stages:**
    * A **Docker image** of the application is built and pushed to **Docker Hub**.
    * **Trivy** performs a vulnerability scan on the **Docker image** itself.
    * The application is securely deployed to the **EKS cluster**.
    * **Kubernetes RBAC** is used to ensure the deployment is performed by a dedicated service account with minimal privileges, not a root user.
5.  **Deployment Verification:** The pipeline verifies that the pods and services are running as expected in the **Kubernetes** cluster.
