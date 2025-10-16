# Deployment-pipeline-and-cloud-platforms-GitHub-PROJECT

---
LESSON ONE
# 🚀 **Introduction to Deployment Pipelines**

### 🧭 What is a Deployment Pipeline?

A **deployment pipeline** (also known as a **CI/CD pipeline**) is an automated sequence of steps that takes your code from **development** to **production** — ensuring it’s built, tested, and deployed **quickly**, **reliably**, and **safely**.

Think of it as an **assembly line for software delivery** — each stage validates the code before it moves to the next.

---

## 🧱 **Core Stages of a Deployment Pipeline**

Each pipeline may vary slightly depending on the stack, but most share these key stages:

---

### **1️⃣ Source (Version Control Stage)**

* The pipeline begins when code is pushed or merged into a Git repository.
* This push event (on `main`, `dev`, or via pull request) **triggers** the CI/CD workflow.

🧩 **Tools:** GitHub, GitLab, Bitbucket

---

### **2️⃣ Build Stage**

* The source code is compiled or packaged into a deployable artifact (e.g., a `.jar`, `.zip`, or Docker image).
* This ensures that the application can run properly and that all dependencies are correctly installed.

🧩 **Examples:**

```bash
npm ci
npm run build
docker build -t myapp:latest .
```

🧩 **Tools:** GitHub Actions runners, Jenkins, AWS CodeBuild, Docker

---

### **3️⃣ Test Stage**

* Automated tests are executed to catch errors or regressions early.
* This includes:

  * **Unit tests** – test individual functions/components.
  * **Integration tests** – test interactions between modules.
  * **Static code analysis** – using tools like **ESLint** or **SonarQube**.

🧩 **If any test fails,** the pipeline stops immediately.

---

### **4️⃣ Deploy Stage**

Once the build and tests pass, the code is deployed to one or more environments:

* **Development / Staging** – for internal testing.
* **Production** – for end-users.

Deployments can happen to servers, containers, or cloud platforms.

🧩 **Tools:** AWS CodeDeploy, Azure Pipelines, GitHub Actions, Docker, Kubernetes, Vercel, Netlify

---

### **5️⃣ Post-Deployment Stage**

After deployment, additional checks and tasks may occur:

* **Smoke testing** (confirm app is running)
* **Monitoring and alerts** (with tools like Prometheus, Grafana, Datadog)
* **Rollback automation** (in case deployment fails)
* **Notifications** (Slack, email, Teams)

---

## ⚙️ **Example: Basic Deployment Pipeline (GitHub Actions)**

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]

jobs:
  build-test-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Deploy to Production
        if: success()
        run: echo "Deploying to production..."
```

---

## 🧩 **Deployment Strategies**

Deployment strategy defines **how new versions of an application are released** to users.
Here are the most common ones:

---

### **1️⃣ Recreate Deployment**

* The old version is **stopped completely**, and the new version is **deployed**.
* Downtime occurs during the switch.

🧱 **Pros:** Simple to implement
⚠️ **Cons:** Causes downtime

---

### **2️⃣ Rolling Deployment**

* New versions are deployed gradually, **one instance at a time**, replacing old ones.
* Users experience a mix of old and new versions during rollout.

🧱 **Pros:** No downtime, safer
⚠️ **Cons:** Harder rollback if something goes wrong mid-rollout

---

### **3️⃣ Blue-Green Deployment**

* Two identical environments: **Blue (current production)** and **Green (new version)**.
* Once Green is fully tested, traffic is **switched instantly** from Blue to Green.
* Blue remains as backup for instant rollback.

🧱 **Pros:** Zero downtime, easy rollback
⚠️ **Cons:** Doubles infrastructure cost temporarily

---

### **4️⃣ Canary Deployment**

* The new version is rolled out to a **small subset of users** first.
* If metrics and logs look good, the rollout gradually expands to everyone.

🧱 **Pros:** Early feedback, minimal risk
⚠️ **Cons:** Requires monitoring setup and routing control

---

### **5️⃣ Shadow Deployment**

* The new version runs **in parallel** with the old one but **doesn’t serve live users yet**.
* Useful for testing real traffic safely.

🧱 **Pros:** Risk-free validation
⚠️ **Cons:** Requires duplicate infrastructure and monitoring logic

---

## 🎯 **Why Deployment Pipelines Matter**

| Benefit             | Description                                                |
| ------------------- | ---------------------------------------------------------- |
| **Speed**           | Automates repetitive steps to release faster               |
| **Quality**         | Ensures code passes tests and linting                      |
| **Reliability**     | Reduces deployment errors via automation                   |
| **Rollback safety** | Makes it easier to revert bad deployments                  |
| **Confidence**      | Developers can push updates more frequently with less fear |

---

## 🧠 **In Short**

> A **deployment pipeline** automates every step from code commit to production release — combining build, test, and deployment stages — while **deployment strategies** determine *how* updates reach users (gradually, instantly, or safely).

<img width="914" height="416" alt="image" src="https://github.com/user-attachments/assets/d10714aa-46a9-486a-8c63-dd9ad8d39ae7" />

---

LESSON TWO

# 🚀 **Automated Releases and Versioning**

---

# ⚙️ **Automatic Versioning in CI/CD**

### 🧭 What It Is

**Automatic versioning** means your CI/CD pipeline automatically **updates the version number** of your application (e.g., `1.0.0 → 1.0.1`) whenever new code changes are merged or deployed — **without manual edits**.

This helps maintain a **consistent, traceable release history** and supports automated publishing or deployments.

---

## 🧩 **Why It’s Useful**

* Keeps versioning **consistent** across developers.
* Reduces human error (no forgotten version bumps).
* Enables **automated releases** (e.g., tagging builds or publishing to npm, Docker Hub, PyPI, etc.).
* Links code changes directly to **semantic versions** for clear changelogs.

---

## 🧠 **How It Works**

Automatic versioning is often driven by:

1. **Commit messages**, or
2. **Pull request metadata**, or
3. **Git tags** automatically created by the CI/CD workflow.

---

## 🔢 **Semantic Versioning (SemVer)**

Most pipelines follow the **Semantic Versioning** format:

```
MAJOR.MINOR.PATCH
```

| Type of Change | Example       | Trigger                           |
| -------------- | ------------- | --------------------------------- |
| **MAJOR**      | 1.0.0 → 2.0.0 | Breaking changes                  |
| **MINOR**      | 1.0.0 → 1.1.0 | New features (no breaking change) |
| **PATCH**      | 1.0.0 → 1.0.1 | Bug fixes or small updates        |

---

## ⚡ **How CI/CD Automates It**

### Example Tools:

* **semantic-release** (Node.js)
* **standard-version**
* **GitVersion** (.NET)
* **bump2version** (Python)
* **npm version** (for JS projects)

---

### Example: GitHub Actions + semantic-release

```yaml
name: Release

on:
  push:
    branches: [ main ]

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm ci

      - name: Run semantic release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: npx semantic-release
```

**semantic-release**:

* Reads commit messages (e.g., `fix:`, `feat:`, `BREAKING CHANGE:`)
* Calculates new version automatically
* Creates a new **Git tag** (e.g., `v1.2.0`)
* Generates a **changelog**
* Publishes to npm or GitHub Releases

---

### Example Commit Message Triggers:

| Commit message               | Result                     |
| ---------------------------- | -------------------------- |
| `fix: corrected login error` | Patch bump (1.0.0 → 1.0.1) |
| `feat: added user profile`   | Minor bump (1.0.1 → 1.1.0) |
| `feat!: changed API schema`  | Major bump (1.1.0 → 2.0.0) |

---

## ✅ **Summary**

Automatic versioning in CI/CD:

* Uses **commit history** to infer code change types.
* **Increments version numbers** automatically (SemVer rules).
* **Tags releases** and updates changelogs or package manifests.
* Keeps the release process **consistent, fast, and auditable**.

----------------------

### Example: GitHub Actions + automated versioning
Images below show the workflow breakdown
<img width="943" height="434" alt="image" src="https://github.com/user-attachments/assets/a0d7e0e5-c36d-43d1-88f2-e95597592662" />
Defines one job named "Create Tag", running on a Linux runner (Ubuntu).

<img width="953" height="424" alt="image" src="https://github.com/user-attachments/assets/83a0167f-96e9-43c3-8298-2aea045efaa9" />
This is the key action:
It reads your latest tag (e.g., v1.0.0)
Then automatically increments the version (v1.0.1 if patch bump)
Then creates a new Git tag and pushes it back to the repo.
DEFAULT_BUMP decides what to bump:
patch → 1.0.0 → 1.0.1
minor → 1.0.0 → 1.1.0
major → 1.0.0 → 2.0.0


```yaml
name: Bump version and tag

on:
  push:
    branches:
      - main

jobs:
  build:
    name: Create Tag
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2
        # The checkout action checks out your repository under $GITHUB_WORKSPACE, so your workflow can access it.

      - name: Bump version and push tag
        uses: anothrNick/github-tag-action@1.26.0
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          DEFAULT_BUMP: patch
        # This action automatically increments the patch version and tags the commit.
        # 'DEFAULT_BUMP' specifies the type of version bump (major, minor, patch).

```

To create a new release upon events like a new tagging, the release code on workflow would look like this 
```
on:
  push:
    tags:
      - '*'

jobs:
  build:
    name: Create Release
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
        # Checks out the code in the tag that triggered the workflow.

      - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{" secrets.GITHUB_TOKEN "}}
        with:
          tag_name: ${{" github.ref "}}
          release_name: Release ${{" github.ref "}}
          # This step creates a new release in GitHub using the tag name.
```

------


LESSON THREE
Deploying to cloud platforms

1. <img width="413" height="251" alt="image" src="https://github.com/user-attachments/assets/d8d2cb08-326a-4105-a94b-b830d151ac96" />
2. <img width="417" height="266" alt="image" src="https://github.com/user-attachments/assets/47084a83-c932-45d0-afb4-bacef9dfaca7" />
   <img width="905" height="391" alt="image" src="https://github.com/user-attachments/assets/d5bdd5bc-0b80-4821-a04f-f8e6a1c43bbe" />
   <img width="647" height="394" alt="image" src="https://github.com/user-attachments/assets/9a91051c-3233-412a-bf6c-a30326eb8d3d" />



```
name: Deploy to AWS

on:
  push:
    branches:
      - main
  # This workflow triggers on a push to the 'main' branch.

jobs:
  deploy:
    runs-on: ubuntu-latest
    # Specifies the runner environment.

    steps:
      - name: Checkout code
        uses: actions/checkout@v2
        # Checks out your repository under $GITHUB_WORKSPACE.

      - name: Set up AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-west-2
        # Configures AWS credentials from GitHub secrets.

      - name: Deploy to AWS
        run: |
          # Add your deployment script here.
          # For example, using AWS CLI commands to deploy.
```
✅ Save as: .github/workflows/deploy.yml

You’ll also need to set up your AWS credentials in your GitHub repository:

Go to Settings → Secrets and variables → Actions → New repository secret
and add:

AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY

Then, this workflow will run automatically on every push to the main branch and execute your AWS deployment commands. See images below for demo

i. <img width="947" height="437" alt="image" src="https://github.com/user-attachments/assets/75277032-7db8-47bc-b25d-4c86b9896685" />
ii. <img width="954" height="434" alt="image" src="https://github.com/user-attachments/assets/5d04987d-75c3-4cf1-989e-adeb909ab4e8" />
iii. <img width="949" height="440" alt="image" src="https://github.com/user-attachments/assets/375f4165-76b7-4b97-9a37-7b5bedd1ffd2" />
iv. <img width="890" height="392" alt="image" src="https://github.com/user-attachments/assets/7a11f71a-20d8-4de6-8f80-90b88cc20272" />
v. <img width="953" height="485" alt="image" src="https://github.com/user-attachments/assets/bccce56e-a2a9-4a07-8d44-96b9045c5190" />



----------------------------------------------------------------
----------------------------------------------------------------

Perfect — this is the **real-world version** of what teams actually do in CI/CD.
You want each workflow to **build**, **package**, and **deploy** — regardless of whether it’s going to **EC2**, **S3**, or **CodeDeploy**.

I’ll rewrite all 3 workflows cleanly, with correct indentation, `build` and `package` stages included, and full explanations inline in comments.

---

## 🚀 **1️⃣ Full Workflow: Build, Package, and Deploy to EC2 (via SSH)**

This pipeline:

* Builds your app
* Packages it into a `.zip`
* Uploads it to the EC2 instance
* Extracts and restarts the app

```yaml
name: Build and Deploy to EC2
on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout source code
      - name: Checkout code
        uses: actions/checkout@v4

      # Step 2: Set up Node.js (adjust version as needed)
      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      # Step 3: Install dependencies and build
      - name: Install dependencies and build
        run: |
          npm ci
          npm run build

      # Step 4: Package the application
      - name: Package app
        run: |
          zip -r app.zip . -x '*.git*'

      # Step 5: Set up SSH agent for EC2
      - name: Set up SSH
        uses: webfactory/ssh-agent@v0.8.0
        with:
          ssh-private-key: ${{ secrets.EC2_SSH_KEY }}

      # Step 6: Copy package to EC2 and deploy
      - name: Deploy to EC2
        run: |
          scp -o StrictHostKeyChecking=no app.zip ubuntu@${{ secrets.EC2_PUBLIC_IP }}:/home/ubuntu/app.zip
          ssh -o StrictHostKeyChecking=no ubuntu@${{ secrets.EC2_PUBLIC_IP }} << 'EOF'
            cd /home/ubuntu
            rm -rf app
            mkdir app
            unzip app.zip -d app
            cd app
            npm install --production
            pm2 restart all || pm2 start index.js --name "my-app"
          EOF
```

✅ **Result:** Your app is built, zipped, uploaded to the EC2 instance, and started/restarted automatically.

---

## 🌐 **2️⃣ Full Workflow: Build, Package, and Deploy to S3**

This pipeline:

* Builds your frontend
* Packages it (optional)
* Deploys the `build` folder to S3

```yaml
name: Build and Deploy to S3
on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout repo
      - name: Checkout code
        uses: actions/checkout@v4

      # Step 2: Set up Node.js
      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      # Step 3: Install dependencies and build
      - name: Install dependencies and build
        run: |
          npm ci
          npm run build

      # Step 4: Package the build output (optional)
      - name: Package build artifacts
        run: |
          zip -r build.zip build

      # Step 5: Configure AWS credentials
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-west-2

      # Step 6: Deploy build to S3
      - name: Sync build folder to S3
        run: |
          aws s3 sync ./build s3://${{ secrets.S3_BUCKET_NAME }} --delete
```

✅ **Result:** The workflow builds your app and pushes the output to your S3 bucket — ready for static website hosting or CloudFront.

---

## ⚙️ **3️⃣ Full Workflow: Build, Package, and Deploy Using AWS CodeDeploy**

This pipeline:

* Builds and packages your app
* Uploads it to an S3 bucket
* Triggers a CodeDeploy deployment

```yaml
name: Build and Deploy with CodeDeploy
on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout code
      - name: Checkout code
        uses: actions/checkout@v4

      # Step 2: Set up Node.js
      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      # Step 3: Install dependencies and build
      - name: Install dependencies and build
        run: |
          npm ci
          npm run build

      # Step 4: Package for deployment
      - name: Create deployment package
        run: |
          zip -r deployment-package.zip . -x '*.git*'

      # Step 5: Configure AWS credentials
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-west-2

      # Step 6: Upload package to S3 (for CodeDeploy)
      - name: Upload package to S3
        run: |
          aws s3 cp deployment-package.zip s3://${{ secrets.CODEDEPLOY_BUCKET }}/

      # Step 7: Trigger CodeDeploy
      - name: Create deployment with CodeDeploy
        run: |
          aws deploy create-deployment \
            --application-name ${{ secrets.CODEDEPLOY_APP_NAME }} \
            --deployment-group-name ${{ secrets.CODEDEPLOY_DEPLOYMENT_GROUP }} \
            --s3-location bucket=${{ secrets.CODEDEPLOY_BUCKET }},bundleType=zip,key=deployment-package.zip
```

✅ **Result:** This workflow handles everything from build → package → CodeDeploy → EC2/ECS deployment — all automatically.
CodeDeploy uses your `appspec.yml` in the repo to define what happens during install/start/stop phases.

---

## 📦 Example `appspec.yml` (For CodeDeploy)

Put this file in the **root of your repo**:

```yaml
version: 0.0
os: linux
files:
  - source: /
    destination: /home/ubuntu/app
hooks:
  BeforeInstall:
    - location: scripts/stop_server.sh
      timeout: 60
      runas: ubuntu
  AfterInstall:
    - location: scripts/start_server.sh
      timeout: 60
      runas: ubuntu
```

And you’d create those scripts in `/scripts/` to handle restarts using `pm2`, `systemctl`, or `docker`.

---

## ⚖️ **Summary Table**

| Deployment Target | Build Step        | Packaging        | Deploy Command                 | AWS Service      | Best For                 |
| ----------------- | ----------------- | ---------------- | ------------------------------ | ---------------- | ------------------------ |
| **EC2 (SSH)**     | ✅ `npm run build` | ✅ `.zip` via SCP | SSH & PM2                      | None             | Simple backend apps      |
| **S3 (Static)**   | ✅ `npm run build` | ✅ (optional)     | `aws s3 sync`                  | S3               | Static websites          |
| **CodeDeploy**    | ✅ `npm run build` | ✅ `.zip` to S3   | `aws deploy create-deployment` | CodeDeploy + EC2 | Scalable production apps |

---

Would you like me to add **Docker-based build and deploy versions** too (for when you containerize your app and push to ECR before deployment)?
That’s the next logical step up from these workflows.

------------------------------------------------------Perfect 👌 — now we’re stepping into **containerized CI/CD**, which is what most production-grade AWS workflows use today.

Here’s a **comprehensive expansion** of what we just covered — this time, including **Docker build + package + push + deploy** workflows for:

* EC2 (manual Docker run via SSH)
* ECS (using AWS Elastic Container Service)
* and ECR (container registry + versioning hub).

Let’s go step-by-step.

---

## 🐳 1️⃣ Build, Package, and Deploy a Docker App to **EC2 (via SSH)**

In this flow:

* GitHub Actions builds a Docker image
* Pushes it to **Amazon ECR** (AWS container registry)
* SSHs into the EC2 instance
* Pulls and runs the new image

```yaml
name: Build and Deploy Docker App to EC2
on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout code
      - name: Checkout code
        uses: actions/checkout@v4

      # Step 2: Configure AWS credentials
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-west-2

      # Step 3: Log in to Amazon ECR
      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      # Step 4: Build and push Docker image
      - name: Build, tag, and push Docker image
        env:
          ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          ECR_REPOSITORY: my-app
          IMAGE_TAG: ${{ github.sha }}
        run: |
          docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
          docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

      # Step 5: Connect to EC2 and deploy
      - name: Deploy to EC2 instance
        uses: appleboy/ssh-action@v1.1.0
        with:
          host: ${{ secrets.EC2_PUBLIC_IP }}
          username: ubuntu
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            docker pull ${{ steps.login-ecr.outputs.registry }}/my-app:${{ github.sha }}
            docker stop my-app || true
            docker rm my-app || true
            docker run -d --name my-app -p 80:3000 ${{ steps.login-ecr.outputs.registry }}/my-app:${{ github.sha }}
```

✅ **Result:** The new Docker image is automatically built, pushed to ECR, and deployed on your EC2 instance.

---

## 🚀 2️⃣ Build, Package, and Deploy a Docker App to **ECS (Elastic Container Service)**

This workflow:

* Builds the Docker image
* Pushes it to ECR
* Updates an existing ECS service to pull and run the latest image automatically

```yaml
name: Build and Deploy Docker App to ECS
on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-west-2

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build and push Docker image to ECR
        env:
          ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          ECR_REPOSITORY: my-ecs-app
          IMAGE_TAG: ${{ github.sha }}
        run: |
          docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
          docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

      - name: Update ECS service
        run: |
          aws ecs update-service \
            --cluster ${{ secrets.ECS_CLUSTER_NAME }} \
            --service ${{ secrets.ECS_SERVICE_NAME }} \
            --force-new-deployment
```

✅ **Result:** ECS automatically replaces old containers with new ones, no SSH needed.
It pulls the new image from ECR and handles scaling, load balancing, and health checks.

---

## 🗂️ 3️⃣ Build, Package, and Push to **ECR Only**

You can use this as a reusable workflow to build and publish Docker images (no deploy).
Other workflows can “call” this one using `workflow_call`.

```yaml
name: Build and Push to ECR
on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-west-2

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build and push Docker image
        env:
          ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          ECR_REPOSITORY: my-docker-app
          IMAGE_TAG: ${{ github.sha }}
        run: |
          docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
          docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
```

✅ **Result:** Your app is packaged into a Docker image, versioned by commit SHA, and stored in ECR.

---

## 📦 Packaging Explained (In Context of Docker & CI/CD)

| Stage       | Description                                      | Example Tool                                     |
| ----------- | ------------------------------------------------ | ------------------------------------------------ |
| **Build**   | Compile / bundle your source code                | `npm run build`, `webpack`, `javac`              |
| **Package** | Wrap the build output into a deployable artifact | `.zip`, `.jar`, `.tar`, or Docker image          |
| **Deploy**  | Deliver artifact to the target environment       | `scp`, `aws s3 sync`, `aws deploy`, `docker run` |

In Docker-based systems:

* Your **package** is the Docker image itself
* The **registry (ECR)** stores those versions
* The **deploy step** pulls the latest image and runs it in your target environment (EC2 or ECS)

---

## ⚖️ Summary of All Workflows

| Workflow Type | Build | Package           | Deploy Method      | Service Used | Target              |
| ------------- | ----- | ----------------- | ------------------ | ------------ | ------------------- |
| Classic EC2   | ✅     | `.zip`            | SSH + PM2          | EC2          | VM (manual setup)   |
| Static S3     | ✅     | `.zip` (optional) | AWS CLI `s3 sync`  | S3           | Static website      |
| CodeDeploy    | ✅     | `.zip` → S3       | CodeDeploy agent   | CodeDeploy   | EC2 or On-prem      |
| Docker on EC2 | ✅     | Docker image      | SSH + Docker CLI   | ECR + EC2    | VM (Dockerized app) |
| Docker on ECS | ✅     | Docker image      | ECS rolling deploy | ECS + ECR    | Managed containers  |

---

Would you like me to **combine all of them into one YAML workflow file** (where you can choose deployment type via `workflow_dispatch` input — e.g., `"ec2"`, `"ecs"`, `"s3"`)?
That’s called a **multi-environment CI/CD pipeline**, and it’s how real DevOps teams manage flexible deployments.


