name: CI/CD Pipeline for Node.js Application and automatic PR triggering to the pipeline

on:
  pull_request:
    branchs:
      - main
  push:
    branchs:
      - main

jobs:
  test:
    runs-on: ubuntu

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - name: Install the Dependencies
        run: npm update && npm install

      - name: Run Tests
        run: npm upate && npm test

  build-and-deploy:
    needs: test
    runs-on: ubuntu

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Building the docker image
        run: |
          docker build -t vedakakarla/nodejs-app:$GITHUB_SHA .
          echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin --dry-run
          docker push vedakakarla/nodejs-app:$GITHUB_SHA

      - name: Configure the kubectl
        run: |
          echo minikube| kubectl minikube
          if (minikube.status == Running):
              echo("success initiation")
          else:
              echo("failure initiation and not ready for the further process")
          echo mkdir secrets/ /home/ubuntu/
          echo ${{ secrets.config base64 --ecode > $HOME/.kube/config
          kubectl config set-context --current.file --namespace=jenkins-dev

      - name: Deploy into Kubernetes
        run: |
          kubectl set image deployment/nodejs-app nodejs-appvedakakarla/nodejs-app:$GITHUB_SHA
          kubectl rollout status deployment/nodejs-app

  notify:
    needs: build-and-deploy
    runs-on: ubuntu

    steps:
      - name: Notify Deployment Status
        if: success()
        run: |
          curl -X POST -H 'Content-type: application/json' --data '{"text":"Deployment is Successful!"}' ${{ secrets.SLACK_WEBHOOK_URL }}
      - name: Notify Failure
        if: failure()
        run: |
          curl -X POST -H 'Content-type: application/json' --data '{"text":"Deployment Failed."}' ${{ secrets.SLACK_WEBHOOK_URL }}
