# 🚀 Azure DevOps CI/CD Pipeline for React App

![Azure](https://img.shields.io/badge/Azure-DevOps-blue?logo=microsoftazure)
![Pipeline](https://img.shields.io/badge/CI/CD-Pipeline-green)
![React](https://img.shields.io/badge/Frontend-React-blue?logo=react)
![Status](https://img.shields.io/badge/Status-Completed-success)

---

## 📌 Project Overview

This project demonstrates a **complete CI/CD pipeline** using **Azure DevOps** to deploy a React application on **Azure App Service**.

The pipeline automates:

* ✅ Build process
* 📦 Artifact management
* 🚀 Deployment to staging slot
* ⛔ Manual approval gate
* 🔁 Slot swap for zero-downtime production deployment

---

## 🔄 CI/CD Workflow

```text
Code Push → Build → Artifact → Staging Deployment → Approval → Swap → Production
```

---

## 🏗️ Architecture

```text
Developer → Azure Repo → Pipeline → Staging Slot → Approval → Production Slot
```

---

## ⚙️ Pipeline Stages

### 🟢 1. Build Stage

* Installs dependencies (`npm install`)
* Builds React app (`npm run build`)

📂 Output:

```text
build/ (production-ready files)
```

---

### 📦 2. Artifact Management

* Publishes build output as artifact (`drop`)
* Used to transfer files between stages

---

### 🔵 3. Deploy to Staging Slot

* Deploys application to **staging slot**
* Uses Azure App Service Deployment task

---

### 🟣 4. Approval Gate

* Configured via **Azure DevOps Environments**
* Ensures controlled production release

---

### 🔴 5. Slot Swap (Zero Downtime 🚀)

* Swaps staging slot with production
* Ensures seamless deployment with no downtime

---

## 📸 Screenshots

> 📌 Add your screenshots here (replace with actual images)

### 🔹 Pipeline Success

![Pipeline](./screenshots/pipeline-success.png)

### 🔹 Staging Slot Deployment

![Staging](./screenshots/staging-slot.png)

### 🔹 Approval Gate

![Approval](./screenshots/approval.png)

### 🔹 Production Deployment

![Production](./screenshots/production.png)

---

## 🧠 Key Concepts

| Concept         | Description                         |
| --------------- | ----------------------------------- |
| CI              | Continuous Integration              |
| CD              | Continuous Deployment               |
| Artifact        | Data shared between stages          |
| Deployment Slot | Separate testing environment        |
| Slot Swap       | Zero downtime release               |
| Approval Gate   | Manual validation before production |

---

## ⚠️ Challenges & Fixes

### ❌ Build Path Issue

* Issue: Build folder not found
* Fix: Corrected artifact path

---

### ❌ React App Not Loading

* Issue: Static files not served
* Fix:

```bash
pm2 serve /home/site/wwwroot --no-daemon --spa
```

---

### ❌ 404 in Staging Slot

* Issue: No deployment
* Fix: Deploy app to staging slot

---

## 📂 Project Structure

```text
.
├── src/
├── public/
├── build/
├── azure-pipelines.yml
└── README.md
```

---

## 💬 Interview Q&A

### ❓ What is CI/CD?

CI automates build/testing, CD automates deployment.

### ❓ Why staging slot?

To validate application before production release.

### ❓ What is slot swap?

Swapping staging and production environments without downtime.

### ❓ Why artifacts?

To transfer build output between pipeline stages.

---

## 🧾 Resume Highlight

> Implemented a multi-stage CI/CD pipeline using Azure DevOps for a React application with staging deployment, approval gates, and zero-downtime production release via deployment slots.

---

## 🚀 Future Enhancements

* 🔍 Health checks before swap
* 🔁 Automated rollback strategy
* 📊 Monitoring with Azure Monitor

---

## 📎 Tech Stack

* Azure DevOps
* Azure App Service
* React.js
* Node.js

---

## ⭐ Key Takeaway

```text
Build → Artifact → Staging → Approval → Swap → Production
```
# 🔁 Blue-Green Deployment & Rollback (Azure App Service)

---

## 📌 What is Blue-Green Deployment?

Blue-Green deployment is a strategy to release applications with **zero downtime**.

### 🔹 Terminology

| Environment | Meaning                         |
| ----------- | ------------------------------- |
| 🔵 Blue     | Current live production version |
| 🟢 Green    | New version (staging slot)      |

---

## 🔄 Deployment Flow

```text
User Traffic → Production (Blue)

Pipeline Flow:
→ Deploy to Staging (Green)
→ Test / Validate
→ Approval
→ Slot Swap
→ Green becomes Production 🚀
```

---

## ⚙️ Azure Implementation

Azure uses **Deployment Slots** to implement Blue-Green:

* Production slot → Live traffic
* Staging slot → New deployment

---

## 🔁 What is Slot Swap?

Slot swap **exchanges** content between staging and production.

### 🔹 Before Swap

```text
Production → Version V1
Staging → Version V2
```

### 🔹 After Swap

```text
Production → Version V2 ✅
Staging → Version V1
```

👉 Swap = **exchange**, not copy

---

## 🚀 Benefits

* ✅ Zero downtime deployment
* ✅ Safe testing before release
* ✅ Instant rollback possible
* ✅ No traffic interruption

---

## 🔍 Swap with Preview (Advanced)

Azure provides **Swap with Preview**:

### 🔹 Flow

1. Apply config changes first
2. Validate app
3. Complete swap manually

👉 Useful when:

* App settings differ between slots
* Need validation before full swap

---

## 🔥 Rollback Strategy (VERY IMPORTANT)

---

### 🟢 Method 1: Instant Rollback (Best Practice)

👉 Simply perform **swap again**

```text
Production ↔ Staging
```

### Result:

```text
Production → Old version restored
```

✅ Fast (seconds)
✅ Zero downtime

---

### 🟢 Method 2: Redeploy Previous Version

1. Pick old artifact
2. Deploy to staging
3. Swap again

---

### 🟢 Method 3: Manual Portal Rollback

* Go to App Service
* Deployment Slots
* Click Swap

---

## ⚙️ Rollback in Pipeline (Optional)

```yaml
- stage: Rollback
  jobs:
  - job: RollbackJob
    steps:
    - task: AzureAppServiceManage@0
      inputs:
        azureSubscription: 'Azure for Students(...)'
        Action: 'Swap Slots'
        WebAppName: 'your-app-name'
        ResourceGroupName: 'your-rg'
        SourceSlot: 'staging'
```

---

## 🧠 Real-World Deployment Flow

```text
Deploy → Staging
↓
Test / Monitor
↓
Approval
↓
Swap → Production
↓
Monitor
↓
Issue detected?
↓
Swap back (Rollback) 🔁
```

---

## ⚠️ Best Practices

* Always test in staging
* Use approval before production
* Monitor logs after deployment
* Keep previous version in staging
* Avoid direct production deployment

---

## 💬 Interview Answer (Strong 🔥)

> I implemented blue-green deployment using Azure App Service deployment slots. The application is deployed to a staging slot first, and after validation and approval, I perform a slot swap to production. In case of issues, I can instantly rollback by swapping the slots again, ensuring zero downtime.

---

## 🧾 Key Takeaway

```text
Deploy → Staging → Swap → Production
Issue? → Swap Back → Rollback
```

---

---

## 🙌 Author

**Yusuf 

---
