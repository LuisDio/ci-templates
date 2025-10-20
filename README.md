# 🔐 CI/CD Security Pipeline Template

This repository contains **reusable CI/CD workflows** for enforcing security and compliance policies across multiple product teams using **OPA/Conftest**.  

It is designed to integrate with a **centralized policy repository** (`central-policy`) and supports **multi-environment deployments** with audit-friendly artifact uploads.  

---

## 🚀 Features

- Reusable workflow for **any team repo**  
- Environment-specific deployment:
  - `staging` → automatic deployment  
  - `production` → requires **manual approval** via GitHub Environments  
- Pulls **central-policy repo** at a pinned version for consistency  
- Runs **OPA/Conftest checks** on Kubernetes manifests or Dockerfiles  
- Uploads **compliance artifacts** for auditing (SOC2, ISO27001)  
- Optional: Slack/Teams notifications on policy failure  

---

## 🏗️ Repo Structure

```text
ci-templates/
└── .github/
  └── workflows/
    └── security-pipeline.yml    # Reusable workflow
```


- `security-pipeline.yml`: reusable GitHub Actions workflow for policy checks and deployment.  
- Designed to be called from **any team repo** using `workflow_call`.

---

## 🧰 Prerequisites

- [Open Policy Agent (OPA)](https://www.openpolicyagent.org/docs/latest/)  
- [Conftest](https://www.conftest.dev/)  
- Kubernetes CLI (`kubectl`) if deploying manifests  

> **Note:** Conftest and kubectl installation are handled automatically in the workflow.

---

## ⚡ Usage

### Step 1: Call the reusable workflow from a team repo

```yaml
jobs:
  deploy:
    uses: your-org/ci-templates/.github/workflows/security-pipeline.yml@v1.0.0
    with:
      environment: staging
      manifests-path: k8s/environments/staging/*.yaml
      team: team-alpha
      policy-version: v1.0.0
