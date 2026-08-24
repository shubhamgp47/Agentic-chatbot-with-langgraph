# Agentic-Chatbot-APP-CICD-Deployment-with-Github-Actions-on-AWS

## Description

This repository demonstrates how to deploy an **Agentic Chatbot** application using a full CI/CD pipeline powered by **GitHub Actions**, with the app containerized via **Docker** and hosted on an **AWS EC2** instance.

## About the Deployment

The deployment pipeline follows these high-level steps:

1. Build a Docker image of the source code.
2. Push the Docker image to Docker Hub.
3. Launch an EC2 instance.
4. Pull the image from Docker Hub onto the EC2 instance.
5. Launch the Docker container on EC2.

## Deployment Steps

### 1. AWS Setup

1. Log in to the AWS Console.
2. Create an IAM user for deployment.

**Required IAM Policy:**

- `AmazonEC2FullAccess`

### 2. Launch EC2 Instance

Create an EC2 machine using the **Ubuntu** AMI.

### 3. Install Docker on EC2

Connect to your EC2 instance and install Docker with the following commands.

```bash
# Optional: update system packages
sudo apt-get update -y
sudo apt-get upgrade

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker

# After running runners commands to EC2 Instance run these too
# Install and start as a service
sudo ./svc.sh install
sudo ./svc.sh start
sudo ./svc.sh status
```

> **Note:** Make sure to configure port mapping for port **8501** in your EC2 security group.

### 4. Configure EC2 as a Self-Hosted GitHub Actions Runner

1. In your GitHub repository, go to **Settings → Actions → Runners → New self-hosted runner**.
2. Choose your operating system.
3. Run the provided setup commands on your EC2 instance, one by one.

### 5. Add Secrets to GitHub Actions

In your repository, go to **Settings → Secrets and variables → Actions**, and add the following secrets:

| Secret Name | Description |
|---|---|
| `REGISTRY` | Docker registry, e.g. `docker.io` |
| `DOCKER_USERNAME` | Your Docker Hub username |
| `DOCKER_PASSWORD` | Your Docker Hub access token |
| `IMAGE_NAME` | Name of the Docker image, e.g. `agentic-chatbot` |
| `AWS_ACCESS_KEY_ID` | Your AWS access key |
| `AWS_SECRET_ACCESS_KEY` | Your AWS secret key |
| `AWS_REGION` | AWS region, e.g. `us-east-1` |
| `OPENAI_API_KEY` | Your OpenAI API key |
| `TAVILY_API_KEY` | Your Tavily API key |
| `OPENWEATHER_API_KEY` | Your OpenWeather API key |
| `GOOGLE_API_KEY` | Your Google API key |
| `LANGSMITH_TRACING` | Set to `true` to enable LangSmith tracing |
| `LANGSMITH_ENDPOINT` | `https://api.smith.langchain.com` |
| `LANGSMITH_API_KEY` | Your LangSmith API key |
| `LANGSMITH_PROJECT` | Your LangSmith project name, e.g. `agentic-chatbot-project` |

> ⚠️ **Security Note:** Never commit these values directly into your codebase or `.env` files that are tracked by Git. Always store them as encrypted GitHub Actions secrets and reference them in your workflow file.
