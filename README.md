# DevOps Task – CI/CD on EC2 with Jenkins & Docker

This project demonstrates a complete **CI/CD pipeline** using **Jenkins**, **Docker**, and **AWS EC2**.  
The pipeline pulls code from GitHub, builds a Docker image, and deploys a containerized Node.js application.

---

## 🚀 Tech Stack
- **Node.js + Express** (sample app)
- **Docker** (containerization)
- **Jenkins** (CI/CD automation)
- **AWS EC2** (deployment server)
- **GitHub Webhooks** (auto trigger builds)

---

📦 DevOps-Task
├── 📂 src/                     # Application source code
│   └── app.js                  # Express server entry point
│
├── 📂 public/                   # Static assets (images, logos, etc.)
│   └── logoswayatt.png
│
├── 📂 config/                   # Configurations (optional, scalable apps)
│   └── default.json             # Example: environment configs
│
├── 📂 scripts/                  # Deployment & helper scripts
│   └── start.sh                 # Script to run container / service
│
├── Dockerfile                   # Docker build instructions
├── Jenkinsfile                  # (Optional) Declarative Jenkins pipeline
├── package.json                 # Node.js dependencies
├── package-lock.json            # Dependency lock file
├── README.md                    # Documentation
└── .gitignore                   # Git ignore file



---

## ⚙️ Jenkins Pipeline
1. **Source Code Management**  
   - Repo: [DevOps Task GitHub Repo](https://github.com/sazamansari/DevOps-Task)  
   - Branch: `main`

2. **Build Step (Execute Shell)**  
   ```bash
   docker build -t node-app .
   docker rm -f node-app-container || true
   docker run -d --name node-app-container -p 3000:3000 node-app

Webhook Trigger

Configured at: http://<EC2-Public-IP>:8080/github-webhook/

GitHub pushes trigger Jenkins automatically.

🔧 Deployment Proof

Public URL:
http://3.110.219.136:3000

If not accessible, check:

EC2 security group allows port 3000.

Container is running: 
docker ps


🛠️ Setup & Run Locally

Clone repo:

git clone https://github.com/sazamansari/DevOps-Task.git
cd DevOps-Task


Build & run Docker container:

docker build -t node-app .
docker run -d -p 3000:3000 node-app


Open in browser:

http://localhost:3000

📊 Architecture
GitHub Repo → GitHub Webhook → Jenkins (EC2) → Docker Build → Run Container → Expose App on Port 3000

✨ Improvements (Future Work)

Add Jenkinsfile (declarative pipeline).

Push Docker image to DockerHub / AWS ECR.

Deploy using AWS ECS or Kubernetes.

Add automated tests before deployment.
