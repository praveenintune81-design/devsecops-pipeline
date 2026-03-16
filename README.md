# 🔐 DevSecOps Pipeline with GitHub Actions

This project demonstrates how to build a **DevSecOps pipeline using GitHub Actions** that integrates security checks directly into the CI/CD workflow.

Instead of treating security as a final step, this pipeline automatically scans code, dependencies, Dockerfiles, and container images before deployment.

The goal of this project is to understand how modern DevOps teams integrate **security, automation, and containerization** into their software delivery pipelines.

---

# 🚀 What is DevSecOps?

DevSecOps stands for **Development + Security + Operations**.

Traditional pipelines usually look like this:

Development → Build → Test → Deploy

Security is often checked only at the end of the process.

DevSecOps changes this approach by integrating security into every stage of the pipeline.

Modern DevSecOps pipeline:

Code → Security Scan → Dependency Scan → Build → Container Scan → Deploy

This approach ensures that:

• vulnerabilities are detected early  
• secrets are not leaked into repositories  
• insecure dependencies are identified  
• container images are scanned before deployment  

In simple terms:

DevSecOps = **Secure CI/CD Pipeline**

---

# 🧰 Technologies Used

This project uses the following tools:

| Tool | Purpose |
|-----|------|
| GitHub Actions | CI/CD automation |
| Docker | Containerization |
| Gitleaks | Secret scanning |
| npm audit | Dependency vulnerability scanning |
| Hadolint | Dockerfile linting |
| Trivy | Container vulnerability scanning |
| Node.js | Example application |

---

# 📂 Project Structure
devsecops-pipeline
│
├── backend
│ ├── package.json
│ ├── app.js
│ └── Dockerfile
│
├── frontend
│ ├── package.json
│ └── Dockerfile
│
└── .github
└── workflows
└── devsecops-pipeline.yml


Explanation:

backend → backend application  
frontend → frontend service  
.github/workflows → GitHub Actions pipeline configuration

---

# ⚙️ DevSecOps Pipeline Workflow

The pipeline runs automatically whenever code is pushed to the **main branch**.

Pipeline stages:

1️⃣ Checkout Code  
2️⃣ Secret Scan  
3️⃣ Dependency Vulnerability Scan  
4️⃣ Dockerfile Security Scan  
5️⃣ Build Docker Image  
6️⃣ Container Vulnerability Scan

---

# 1️⃣ Checkout Code

This stage downloads the repository code to the GitHub runner so it can be scanned and built.

Example configuration:

```yaml
- name: Checkout Repository
  uses: actions/checkout@v4

2️⃣ Secret Scanning (Gitleaks)

Secrets like API keys, tokens, or passwords should never be committed to Git repositories.

This stage scans the repository for exposed secrets.

Tool used:

Gitleaks

Example:

- name: Run Gitleaks Secret Scan
  uses: gitleaks/gitleaks-action@v2

If any secrets are detected, the pipeline can fail to prevent exposure.

3️⃣ Dependency Vulnerability Scan

Applications often use third-party libraries.

Some dependencies may contain known vulnerabilities.

This stage scans installed packages using npm audit.

Example:

- name: Run npm audit
  run: npm audit --audit-level=high

This helps identify vulnerable packages early.

4️⃣ Dockerfile Security Scan

Before building containers, the Dockerfile itself should follow security best practices.

Common problems include:

• using the latest tag
• running containers as root
• unnecessary layers

Tool used:

Hadolint

Example:

docker run --rm -i hadolint/hadolint < backend/Dockerfile

5️⃣ Build Docker Image

After security checks, the application is containerized.

Docker images are built for both frontend and backend services.

Example:

docker build -t backend ./backend
docker build -t frontend ./frontend

Containerization ensures consistent environments across development and production.

6️⃣ Container Vulnerability Scan

Even if the Dockerfile is secure, the base image might contain vulnerabilities.

This stage scans container images using Trivy.

Trivy checks:

• OS packages
• libraries
• known CVEs (Common Vulnerabilities and Exposures)

Example:

- name: Run Trivy Scan
  uses: aquasecurity/trivy-action@0.24.0

This ensures that container images are safe before deployment.

🔄 Pipeline Flow

Complete workflow:

Developer Push Code
↓
GitHub Actions Trigger
↓
Secret Scan (Gitleaks)
↓
Dependency Scan (npm audit)
↓
Dockerfile Scan (Hadolint)
↓
Build Docker Image
↓
Container Scan (Trivy)

📊 Why This Pipeline is Important

This pipeline demonstrates how security can be integrated into CI/CD pipelines.

Benefits:

• Early vulnerability detection
• Secure container images
• Automated security checks
• Faster and safer deployments

📚 What I Learned

Through this project I learned:

• How to build CI/CD pipelines using GitHub Actions
• How to integrate security tools into pipelines
• How to scan dependencies and containers for vulnerabilities
• How to automate Docker builds and security checks

This project is part of my DevOps learning journey.
