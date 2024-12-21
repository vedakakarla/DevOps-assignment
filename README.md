# CI/CD Pipeline for Node.js Application

This repository contains a CI/CD pipeline for automating the following tasks:

1. **Run tests automatically on pull requests.**
2. **Build and deploy a Docker image to a Kubernetes cluster.**
3. **Send Slack notifications for deployment status.**

## Prerequisites

Add these GitHub secrets:
- `DOCKER_USERNAME` and `DOCKER_PASSWORD`: Docker Hub credentials.
- `KUBECONFIG`: Base64 encoded Kubernetes configuration file.
- `SLACK_WEBHOOK_URL`: Slack webhook for notifications.

## Usage

1. Push changes to the `main` branch or create a pull request.
2. The pipeline will:
   - Run tests (`npm test`).
   - Build and push a Docker image.
   - Deploy to Kubernetes (`nodejs-app` deployment).
   - Notify via Slack.

## Tools Used

- **GitHub Actions**
- **Docker**
- **Kubernetes**
- **Slack**

For local development, use `npm install` to install dependencies and `npm test` to run tests. Build the Docker image with `docker build -t nodejs-app .`.
