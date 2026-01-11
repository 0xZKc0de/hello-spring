# Atelier CI/CD: Spring Boot with Jenkins & Docker Hub

## Project Overview
This repository contains a Spring Boot application configured with a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline. It uses Jenkins to automatically build, containerize, and deploy the application to Docker Hub and a runtime environment upon every code commit.

## Key Features
* **Java Application:** A "Hello World" Spring Boot application managed with Maven.
* **Containerization:** The app is packaged in a lightweight Docker image.
* **Registry:** Docker images are automatically pushed to Docker Hub.
* **Automation:** A Jenkins pipeline detects GitHub pushes via Webhooks.
* **Continuous Deployment:** The pipeline automatically stops the old container, pulls the new image, and starts the new version.

## Prerequisites
To run this project or the pipeline, you need:
* **Java JDK:** Version 17 or higher
* **Maven:** Version 3.6+
* **Docker:** Docker Desktop or Docker Engine
* **Jenkins:** A running Jenkins instance with Docker access
* **Accounts:** GitHub and Docker Hub

## Project Structure
* `src/main/java`: Source code (`App.java`, `HelloController.java`)
* `pom.xml`: Maven configuration
* `Dockerfile`: Instructions for building the container image
* `Jenkinsfile`: The CI/CD pipeline script

## Pipeline Stages
The `Jenkinsfile` defines three main stages:
1. **Build JAR:** Compiles the code using `mvn clean package`.
2. **Build & Push Docker:** Builds the image and pushes it to Docker Hub using stored credentials.
3. **Deploy:** Stops the existing container and runs the new one on port 8080.

## Usage
Once deployed, the application is accessible at:
```bash
http://localhost:9090/hello
