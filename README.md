# CI/CD Pipeline Using GitHub Actions: Deploy Node.js Project on AWS EC2 (Ubuntu)

This repository provides a step-by-step guide on how to set up a CI/CD pipeline using GitHub Actions to deploy a Node.js project on AWS EC2. The pipeline automates the deployment process, allowing you to easily deploy your Node.js application on an AWS EC2 instance. Follow the instructions below to get started.

## Documentation

### Step 01: Create and Test Your Node.js Project Locally

1. Create a new Node.js project on your local machine.
2. Confirm that your project is working perfectly on your local machine.

### Step 02: Prepare AWS Environment

1. [Login to the AWS console](https://console.aws.amazon.com/console/home?nc2=h_ct&src=header-signin).
2. Create a new IAM user role. If the role already exists, you can use that.
3. Generate a new key pair and download it to your local machine.
4. Select the region where you want to create your server.
5. Go to the EC2 instance section.
6. Create a new security group. If a security group already exists, you can use that.
   - Ensure that the inbound specific port required for your Node.js project is allowed for public traffic.
7. Create a key pair for connecting to the server via SSH. If a key pair already exists, you can use that.
8. Create your server based on your requirements.

### Step 03: Connect to Your Server via SSH Key

1. Connect to your server using an SSH key. You can use tools like PuTTYgen or any other SSH client.

### Step 04: Create a GitHub Repository

1. Go to GitHub and create a new repository for your Node.js project. If the repository already exists, you can use that.

### Step 05: Push Your Node.js Project to the Repository

1. Push your Node.js project to the newly created repository.

### Step 06: Create Workflow for CI/CD

1. Create a workflow for your project.
2. In the repository, create a file named `.github/workflows/node.js.yml` and import the following code:

```yaml
name: Node.js CI

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: self-hosted

    strategy:
      matrix:
        node-version: [12.x]

    steps:
      - uses: actions/checkout@v3
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      - run: npm ci
      - run: npm run build --if-present
      - run: pm2 restart index.js
```

### Step 07: Set Up Self-Hosted Runner and Configure AWS

1. Go to your GitHub repository and navigate to Settings.
2. Open the Actions tab from the left side navigation and click on the Runners menu. Then click on "New self-hosted runner."
3. Select Linux as the operating system and scroll down.
4. Open your server via SSH connection and follow these steps:
   - Create a folder for this project:
     ```bash
     $ mkdir actions-runner && cd actions-runner
     ```
   - Download the latest runner package:
     ```bash
     $ curl -o actions-runner-linux-x64-2.304.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.304.0/actions-runner-linux-x64-2.304.0.tar.gz
     ```
   - Extract the installer:
     ```bash
     $ tar xzf ./

actions-runner-linux-x64-2.304.0.tar.gz
     ```
   - Create the runner and start the configuration experience:
     ```bash
     $ ./config.sh --url https://github.com/jmalamin40/nodejs_deploy_in_aws_cicd_pipeline --token <copy_token_from_github>
     ```
   - Install the runner as a service:
     ```bash
     $ sudo ./svc.sh install
     ```
   - Start the runner:
     ```bash
     $ sudo ./svc.sh start
     ```
   - Install Node.js and any required database as per your project's requirements.

5. Use the following YAML in your workflow file for each job:
   ```yaml
   runs-on: self-hosted
   ```

6. Push your targeted branch to trigger the CI/CD pipeline.
7. Ensure that the port used by your project is allowed for public traffic in the AWS security group.

### Step 08: Check GitHub Actions Logs
1. Check the GitHub Actions log to monitor the progress and status of your CI/CD pipeline.

