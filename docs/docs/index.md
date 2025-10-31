# ⚙️ GitHub Actions Automation Documentation

This documentation explains how GitHub Actions workflows were implemented to automate various aspects of a web application project at **XYZ Corp** — including testing, deployment, maintenance, and documentation.

---

## 🧱 Project Overview

The goal was to use **GitHub Actions** to automate development tasks such as building, testing, deploying, and maintaining the project.  
Automation improves efficiency, code quality, and ensures smooth deployment cycles.

---

## 🗂️ Project Structure

```plaintext
.github/workflows/
│
├── ci.yml               # Continuous Integration workflow
├── deploy.yml           # Deployment pipeline to Heroku
├── schedule.yml         # Scheduled maintenance workflow
├── dependency.yml       # Auto dependency update
├── review.yml           # Code review checks
├── docs_deploy.yml      # Documentation deployment (MkDocs)
└── custom_task.yml      # Custom analytics task
```
## 🪜 Steps Followed During the Assignment
## Repository Setup

Created a GitHub repository named task-manager-app.

Added application source code and project files.

Created .github/workflows/ directory for YAML workflow files.

Enabled GitHub Actions in the repository settings.

---

## Step 1: Continuous Integration Workflow (ci.yml)

Purpose: Run automated tests on every push or pull request.
Configuration:
name: Continuous Integration

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Run lint
        run: npm run lint --if-present

      - name: Run tests
        run: npm test --if-present

      - name: Build project
        run: npm run build --if-present

---

## step 2: Deployment Workflow (deploy.yml)

Purpose: Automate deployment to Vercel.
Configuration:
name: CI/CD Pipeline - Deploy to Vercel

on:
  push:
    branches:
      - main

jobs:
  build-test-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test

      - name: Build Project
        run: npm run build

      - name: Deploy to Vercel
        run: |
          curl -X POST https://api.vercel.com/v13/deployments \
          -H "Authorization: Bearer ${{ secrets.VERCEL_TOKEN }}" \
          -H "Content-Type: application/json" \
          -d '{
            "name": "heaven-above",
            "projectId": "${{ secrets.VERCEL_PROJECT_ID }}",
            "orgId": "${{ secrets.VERCEL_ORG_ID }}",
            "target": "production"
          }'


---

## step 3: Scheduled Maintenance (schedule.yml)

Purpose: Automate recurring maintenance tasks.
Configuration:
name: Scheduled Tasks

## Trigger: Runs on a schedule (daily at 2:00'o clock)
on:
  schedule:
    - cron: '0 2 * * *'  # minute_hour_day_month_day-of-week

jobs:
  maintenance:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout the repository
      - name: Checkout Repository
        uses: actions/checkout@v3

      # Step 2: Run a script or command
      - name: Run Maintenance Script
        run: |
          echo "Running daily maintenance..."
         

---

## step 4: Dependency Updates (dependency.yml)

Purpose: Keep all dependencies up-to-date automatically using Dependabot.
Configuration:
name: Test Dependency Updates

on:
  pull_request:
    branches:
      - main

jobs:
  test-updates:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repo
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test




---

## Step 5: Code Review Workflow (review.yml)

Purpose: Ensure code quality and security before merging.
Configuration:
name: Code Review Workflow

## Trigger: Run this workflow on pull requests to the main branch
on:
  pull_request:
    branches:
      - main

jobs:
  code-review:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout repository
      - name: Checkout Repository
        uses: actions/checkout@v3

      # Step 2: Set up Node.js environment (for JavaScript/Node projects)
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      # Step 3: Install dependencies
      - name: Install Dependencies
        run: npm install

      # Step 4: Lint code (check coding standards)
      - name: Run ESLint
        run: npx eslint . --max-warnings=0

      # Step 5: Run tests (basic quality check)
      - name: Run Tests
        run: npm test

      # Step 6: Security check (scan for vulnerabilities)
      - name: Run npm audit
        run: npm audit --audit-level=moderate



---

## Step 6: Documentation Deployment (docs_deploy.yml)

Purpose: Automatically build and deploy MkDocs documentation to GitHub Pages.
Configuration:
name: Deploy Documentation

## Trigger workflow on push to main branch or changes in docs folder
on:
  push:
    branches:
      - main
    paths:
      - 'docs/**'  # Only trigger if files in docs folder change

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      # Step 1: Checkout repository
      - name: Checkout Repository
        uses: actions/checkout@v3

      # Step 2: Install dependencies (optional, if using a static site generator)
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install Dependencies
        run: npm install

      # Step 3: Build documentation (example: using MkDocs or another tool)
      - name: Build Documentation
        run: |
          # Example using MkDocs
          # pip install mkdocs mkdocs-material
          # mkdocs build
          echo "Building documentation..."  # Replace with actual build command

      # Step 4: Deploy to GitHub Pages
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v6
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site  # Replace with your built docs folder

---         

## Step 7: Custom Workflow Integration (custom_task.yml)

Purpose: Add a custom analytics and report generation workflow.
Configuration:

---