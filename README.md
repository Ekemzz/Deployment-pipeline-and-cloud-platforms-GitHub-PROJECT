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

Then, this workflow will run automatically on every push to the main branch and execute your AWS deployment commands.





