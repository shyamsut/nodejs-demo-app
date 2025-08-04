# Node.js CI/CD Pipeline with GitHub Actions & Docker

This repository demonstrates a working setup of a CI/CD pipeline for a Node.js application using GitHub Actions and Docker.
The aim is to automate the entire process —from code push to container deployment on DockerHub — with minimal manual steps.

---

## 🔧 What This Project Does

- Automatically installs dependencies and runs tests
- Builds the Docker image using a Dockerfile
- Pushes the image to DockerHub using GitHub Secrets
- All triggered by a simple push to the `main` branch

This setup is perfect for small to mid-sized applications that need simple and reliable automation.

---

## 🛠 Tech Used

- **Node.js** – Server-side JavaScript
- **Express.js** – Lightweight web framework
- **Docker** – Containerization
- **DockerHub** – Hosting Docker images
- **GitHub Actions** – CI/CD automation
- **GitHub Secrets** – Secure credential handling

---

## 📁 Folder Structure

.
├── .github/workflows/main.yml # GitHub Actions workflow
├── Dockerfile # Build instructions for Docker
├── index.js # Entry point of the app
├── package.json # Project config and scripts
└── .dockerignore # Ignore unnecessary files during Docker build

---

## ▶️ Running the App Locally

To test the app outside of CI/CD:

```bash
npm install
npm start
Then visit http://localhost:3000 to see the result.

🐳 Docker Image
Once the workflow runs, the image will be available on DockerHub under your account:


docker pull yourusername/nodejs-demo-app
docker run -p 3000:3000 yourusername/nodejs-demo-app
Browser output:
Hello from Node.js App!

🔐 Secrets to Configure in GitHub
Go to your GitHub repo > Settings > Secrets > Actions, and add:

DOCKER_USERNAME = Your DockerHub username

DOCKER_PASSWORD = Your DockerHub password or access token

These secrets are injected into the workflow during DockerHub login.

🧪 Sample GitHub Actions Workflow
Below is the .github/workflows/main.yml that handles the full pipeline:


name: Build & Deploy Node.js App

on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Login to DockerHub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/nodejs-demo-app .

      - name: Push image to DockerHub
        run: docker push ${{ secrets.DOCKER_USERNAME }}/nodejs-demo-app
