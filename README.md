# DevOps CI/CD Demo Project

[![CI/CD Pipeline](https://github.com/kushivaradaraj/devops-project/actions/workflows/ci.yml/badge.svg)](https://github.com/kushivaradaraj/devops-project/actions/workflows/ci.yml)

A complete CI/CD pipeline using GitHub Actions. Shows how to automate building, testing, and deploying code with proper security checks.

## What This Does

Every time you push code, this pipeline automatically:
- Builds your application
- Runs all tests
- Checks for security issues
- Creates a Docker container
- Deploys it to Docker Hub

No manual work needed.

## How It Works

```
Push Code → Build & Test → Security Scans → Docker Build → Deploy
```

## What You Need

**On your computer:**
- Java 17+
- Maven 3.6+
- Docker Desktop
- Git

**Online accounts:**
- GitHub
- Docker Hub

## Quick Start

**1. Get the code**
```bash
git clone https://github.com/kushivaradaraj/devops-project.git
cd devops-project
```

**2. Build it**
```bash
mvn clean verify
```

**3. Run it**
```bash
mvn spring-boot:run
```

**4. Test it**
```bash
curl http://localhost:8080/health
curl http://localhost:8080/hello
```

**5. Try with Docker**
```bash
docker build -t devops-cicd-demo .
docker run -p 8080:8080 devops-cicd-demo
```

## GitHub Setup

**Create your repo:**
1. Make a new GitHub repo called `devops-project`
2. Make it public (for free GitHub Actions)

**Add secrets (important!):**

Go to Settings → Secrets and variables → Actions, then add:
- `DOCKERHUB_USERNAME` - your Docker Hub username
- `DOCKERHUB_TOKEN` - your Docker Hub token (not password!)

To get a token: Docker Hub → Account Settings → Security → New Access Token

## Pipeline Stages

Here's what happens at each step:

**Code Quality**
- Checkstyle checks your code style
- JUnit runs all tests
- Maven builds everything

**Security Scans**
- CodeQL scans your code for vulnerabilities
- OWASP checks your dependencies for known issues

**Docker**
- Builds a container with your app
- Trivy scans the container for security problems
- Tests that it actually works
- Pushes to Docker Hub

## Running the Pipeline

**Automatic:** Runs on every push to main/master

**Manual:** 
1. Go to Actions tab
2. Click "Run workflow"
3. Pick your branch and run

## Security Tools

**CodeQL** - Finds security bugs in your code (SQL injection, XSS, etc.)

**OWASP Dependency Check** - Finds vulnerable libraries you're using

**Trivy** - Scans your Docker image for security issues

## What's In Here

```
devops-cicd-demo/
├── .github/workflows/ci.yml          # The pipeline
├── src/main/java/                    # Your code
├── src/test/java/                    # Your tests
├── Dockerfile                        # Container recipe
├── pom.xml                          # Maven config
└── README.md                        # This file
```

## Learn More

- [GitHub Actions](https://docs.github.com/en/actions)
- [Docker](https://docs.docker.com/)
- [Spring Boot](https://spring.io/projects/spring-boot)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

---

**Author:** Kushi Varadaraj  
**Student ID:** 10035  
