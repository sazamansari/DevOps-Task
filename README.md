---

# DevOps Task – End-to-End CI/CD Pipeline on AWS EC2 with Jenkins & Docker

![Pipeline Architecture](https://github.com/sazamansari/DevOps-Task/blob/main/image/CI-CD-Pipeline.jpg)
                        +----------------+
                        |  GitHub Repo   |
                        | DevOps-Task    |
                        +--------+-------+
                                 |
                     (push / PR trigger via webhook)
                                 |
                        +--------v--------+
                        |   Jenkins CI     |
                        +--------+---------+
                                 |
              +------------------+------------------+
              |                                     |
      (Build / Dockerize)                  (Static Code / Lint / Test)
              |                                     |
   +----------v----------+              +-----------v----------+
   |  Build Docker Image |              |  Run Unit / Integration |
   |  (logo-server)      |              |  Tests, Lint, Filters   |
   +----------+----------+              +-----------+-------------+
              |                                     |
              +------------------+------------------+
                                 |
                         (push to container registry)
                                 |
               +-----------------v------------------+
               |   DockerHub  / AWS ECR / GCP AR     |
               +-----------------+------------------+
                                 |
                        +--------v--------+
                        |   Deployment     |
                        |  (Cloud Run /    |
                        |   ECS / GKE)     |
                        +--------+---------+
                                 |
                         +-------v--------+
                         |  Staging / QA   |
                         |  environment    |
                         +-------+---------+
                                 |
                      (smoke tests / approval)
                                 |
                         +-------v--------+
                         |  Production     |
                         |  Deployment     |
                         +----------------+

## 📖 Overview

This project demonstrates a **complete DevOps workflow** using:

* **Node.js + Express** sample application
* **Docker** for containerization
* **Jenkins** for CI/CD automation
* **AWS EC2** as the deployment environment

The pipeline covers the full journey: **source → build → test → containerize → deploy**.

---

## 🏗️ Architecture

The following diagram shows the high-level architecture:

![Architecture Diagram]((https://github.com/sazamansari/DevOps-Task/blob/main/image/1.png))

1. Developer pushes code to GitHub.
2. GitHub Webhook triggers Jenkins job.
3. Jenkins builds & tests app, then builds Docker image.
4. Jenkins deploys the container to AWS EC2.
5. Application exposed at `http://<EC2_PUBLIC_IP>:3000`.

---

## 📂 Repository Structure

```
DevOps-Task/
├── app.js              # Node.js Express application
├── package.json        # NPM dependencies
├── Dockerfile          # Container build definition
├── Jenkinsfile         # Jenkins pipeline (Declarative)
├── README.md           # Project documentation
├── docs/
│   └── architecture.png # Architecture diagram
├── public/
│   └── logoswayatt.png  # Static assets / sample images
└── test/
    └── smoke.test.js    # Basic smoke test
```

---

## 🚀 Features

* **Automated Build & Deploy** via Jenkins.
* **Dockerized App** → portable & consistent runtime.
* **Basic Smoke Test** to validate endpoints.
* **Healthcheck in Dockerfile** for runtime monitoring.
* **Branch Strategy** → `dev` for feature work, PRs into `main`.

---

## ⚙️ Setup & Installation

### 1. Clone the Repository

```bash
git clone https://github.com/sazamansari/DevOps-Task.git
cd DevOps-Task
```

### 2. Local Run (without Docker)

```bash
npm install
node app.js
```

App runs at: [http://localhost:3000](http://localhost:3000)

### 3. Docker Build & Run

```bash
docker build -t node-app .
docker run -d -p 3000:3000 --name node-app node-app
```

---

## 🔄 Jenkins CI/CD Pipeline

The pipeline is fully automated via **Jenkinsfile**:

![Jenkins Pipeline](https://github.com/sazamansari/DevOps-Task/blob/main/image/2.png)

### Stages:

1. **Checkout** → Pull code from GitHub.
2. **Install & Test** → Run `npm ci` & smoke tests.
3. **Docker Build** → Build image `node-app:<BUILD_NUMBER>`.
4. **Deploy** → Run container on EC2 at port 3000.

### Jenkins Setup

* Install plugins: *GitHub Integration*, *Pipeline*, *Docker Pipeline*.
* Configure credentials for GitHub & Docker registry.
* Add Webhook in GitHub → `http://<JENKINS_HOST>:8080/github-webhook/`.

---

## 🌐 Deployment

Live application URL (example):
👉 [http://3.110.219.136:3000](http://3.110.219.136:3000)

![App Screenshot](https://github.com/sazamansari/DevOps-Task/blob/main/image/3.png)

---

## 🧪 Testing

Run smoke tests locally:

```bash
npm test
```

Example smoke test (`test/smoke.test.js`):

```js
const request = require('supertest');
const app = require('../app');

describe('smoke', () => {
  it('GET / should return 200', async () => {
    await request(app).get('/').expect(200);
  });
});
```

---
## EC2 CMD
![App Screenshot](https://github.com/sazamansari/DevOps-Task/blob/main/image/4.png)

## 📈 Improvements

* Push Docker image to **DockerHub / AWS ECR**.
* Add **blue/green deployment** on EC2.
* Introduce **Terraform/CloudFormation** for IaC.
* Add monitoring (Prometheus/Grafana).
* Enable HPA if deployed on Kubernetes.

---

✅ With this setup, you now have a **complete, production-style DevOps pipeline** that runs end-to-end.

---
