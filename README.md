# ACMP2400 DevOps Final Project

## Overview

This project is a simple Django web application that returns a “Hello World” response, combined with a full DevOps pipeline. The goal is to demonstrate containerization, CI/CD, security scanning, and cloud deployment using modern tools.

The application itself is small, but the focus of this project is the automated workflow that builds, tests, scans, deploys, and cleans up the application.

---

## Tech Stack

* Python (Django)
* Docker
* GitHub Actions (CI/CD)
* Terraform / OpenTofu (Infrastructure as Code)
* Microsoft Azure (Container Registry + Container Instances)
* Syft & Grype (SBOM + vulnerability scanning)

---

## Project Structure

```
ACMP2400/
│── final_app/              # Django project
│── hello_final/            # Django app (returns Hello World)
│── stage1/                 # Terraform - Azure Container Registry
│── stage2/                 # Terraform - Container Instance deployment
│── stage3/                 # Terraform - cleanup/destroy
│── .github/workflows/      # CI/CD pipeline
│── Dockerfile              # Container setup
│── requirements.txt        # Python dependencies
│── README.md
```

---

## How It Works (Pipeline Overview)

1. **Build Docker Image**
   The app is containerized using Docker

2. **Run Container Locally**
   The container is started and tested using `curl`

3. **Security Scan (SBOM)**

   * SBOM generated with Syft
   * Vulnerabilities scanned with Grype
   * Pipeline fails on critical issues

4. **Push to Azure Container Registry**
   The image is tagged and pushed to ACR

5. **Deploy to Azure Container Instances**
   Terraform/OpenTofu provisions infrastructure and deploys the container

6. **Test Deployment**
   A `curl` request verifies the deployed app is running

7. **Cleanup**
   Infrastructure is destroyed to avoid unnecessary costs

---

## Running Locally

### 1. Clone the repo

```bash
git clone https://github.com/xKingPanda/ACMP2400.git
cd ACMP2400
```

### 2. Build Docker image

```bash
docker build -t app-test .
```

### 3. Run container

```bash
docker run -p 8000:8000 app-test
```

### 4. Test

Open browser or run:

```bash
curl http://localhost:8000
```

Expected output:

```
Hello World
```

---

## CI/CD Pipeline

The GitHub Actions workflow automatically:

* Builds the Docker image
* Runs the container
* Tests with curl
* Generates SBOM
* Scans for vulnerabilities
* Deploys to Azure
* Tests the live deployment
* Cleans up resources

---

## Security Notes

* Secrets are stored in GitHub Secrets
* Docker images and actions are pinned to specific versions
* SBOM scanning helps detect vulnerabilities early
* `.env` files should not be committed in real projects
