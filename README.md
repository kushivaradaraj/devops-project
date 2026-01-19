# DevOps CI/CD Project

A CI/CD pipeline with Kubernetes deployment using GitHub Actions.

---

## What This Project Does

This project shows a complete CI/CD pipeline that:

- Builds a Java Spring Boot app
- Runs automated tests
- Checks code quality with Checkstyle
- Scans code for security issues (CodeQL)
- Scans dependencies for vulnerabilities (OWASP)
- Builds Docker images and scans them (Trivy)
- Pushes to Docker Hub
- Deploys to Kubernetes

---

## Requirements

- Java 17
- Maven
- Docker
- GitHub account
- Docker Hub account

---

## Running Locally

```bash
# Clone
git clone https://github.com/kushivaradaraj/devops-project.git
cd devops-project

# Build and test
mvn clean verify

# Run
mvn spring-boot:run

# Test endpoints
curl http://localhost:8080/health    # Returns: OK
curl http://localhost:8080/hello     # Returns: Hello, World!
curl http://localhost:8080/version   # Returns: 1.0.0
```

---

## Running with Docker

```bash
# Build
docker build -t devops-project .

# Run
docker run -p 8080:8080 devops-project

# Test
curl http://localhost:8080/health
```

---

## GitHub Secrets Configuration

Go to: Repository → Settings → Secrets and variables → Actions

Add these secrets:

| Secret Name | What to Put |
|-------------|-------------|
| `DOCKERHUB_USERNAME` | Your Docker Hub username |
| `DOCKERHUB_TOKEN` | Access token from Docker Hub (Account Settings → Security → New Access Token) |

Don't put these values directly in your code - always use secrets!

---

## CI/CD Pipeline Explanation

The pipeline runs automatically when you push code. It's defined in `.github/workflows/ci.yml`.

### Pipeline Flow:

```
Code Push
    │
    ▼
┌──────────────────────────────────────┐
│           CI (Continuous Integration) │
│                                      │
│  Checkout → Java Setup → Lint → Test │
│              → Build → Security Scans │
│              → Docker Build → Trivy   │
│              → Container Test → Push  │
└──────────────────────────────────────┘
    │
    ▼
┌──────────────────────────────────────┐
│           CD (Continuous Deployment)  │
│                                      │
│  Start Minikube → Deploy to K8s      │
│              → Create Service         │
│              → Verify Deployment      │
└──────────────────────────────────────┘
```

### Stages Explained:

| Stage | Tool | What It Does |
|-------|------|--------------|
| Checkout | actions/checkout | Gets code from repo |
| Setup Java | actions/setup-java | Installs Java 17 |
| Lint | Checkstyle | Checks code style |
| Test | JUnit | Runs unit tests |
| Build | Maven | Creates JAR file |
| SAST | CodeQL | Scans code for security issues |
| SCA | OWASP | Scans libraries for known vulnerabilities |
| Docker Build | Docker | Creates container image |
| Image Scan | Trivy | Scans container for CVEs |
| Container Test | curl | Tests if container works |
| Push | Docker Hub | Uploads image |
| K8s Deploy | Minikube | Deploys to Kubernetes |

### Why This Order?

Quick checks (lint, test) run first. If they fail, no point running slow security scans. Security scans happen before pushing, so vulnerable code never gets deployed.

---

## Kubernetes Deployment

The pipeline deploys to Kubernetes using Minikube (a local K8s cluster).

### Files:

- `k8s/deployment.yaml` - Creates 2 pods running our app
- `k8s/service.yaml` - Exposes the app so it can be accessed

### What Happens in CD Stage:

1. Installs and starts Minikube
2. Deploys the app (2 replicas for high availability)
3. Creates a service to expose the app
4. Tests that the deployment works

### Running K8s Locally:

```bash
# Start minikube
minikube start

# Deploy
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

# Check pods
kubectl get pods

# Get app URL
minikube service devops-demo-service --url
```

---

## Project Structure

```
devops-project/
├── .github/
│   └── workflows/
│       └── ci.yml              # Pipeline definition
├── k8s/
│   ├── deployment.yaml         # K8s deployment config
│   └── service.yaml            # K8s service config
├── src/
│   ├── main/java/              # App code
│   └── test/java/              # Tests
├── Dockerfile                  # How to build container
├── pom.xml                     # Dependencies
├── checkstyle.xml              # Code style rules
└── README.md
```

---

## How to Trigger Pipeline

**Automatically:** Just push to main or master

**Manually:** Actions tab → Select workflow → Run workflow

---

## Where to See Results

- **Pipeline status:** Actions tab in GitHub
- **Security issues:** Security tab in GitHub
- **Docker images:** Your Docker Hub account

---

Kushi Varadaraj | Roll No: 10035 | January 2026