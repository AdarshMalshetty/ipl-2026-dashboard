# IPL 2026 Dashboard

A live IPL 2026 points table and fixtures dashboard — deployed automatically to **Azure Static Web Apps** via **Azure DevOps Pipelines** on every push to `main`.

🔗 **Live URL:** *(your Azure Static Web Apps URL here)*

---

## What It Shows

- **Points Table** — live standings with NRR, wins, losses, and playoff qualification status
- **Fixtures** — upcoming matches with venues and times
- **Results** — completed match outcomes
- **Orange & Purple Cap** — leading run-scorer and wicket-taker

## How It's Deployed

```
Push to main (index.html updated)
        │
        ▼
Azure DevOps Pipeline triggers
        │
        ▼
AzureStaticWebApp@0 task runs
        │
        ▼
Live on Azure Static Web Apps
   in under 2 minutes
```

The pipeline is defined in `/pipeline/azure-pipelines.yml`. Every time standings or fixtures are updated and pushed to `main`, the site redeploys automatically — no manual steps.

## Setup

### 1 — Create Azure Static Web App
1. Go to [portal.azure.com](https://portal.azure.com)
2. Search **Static Web Apps** → Create
3. Select **Free** tier
4. Source: **Other** (we'll deploy via Azure DevOps, not GitHub Actions)
5. Once created, go to **Manage deployment token** and copy the token

### 2 — Add the token to Azure DevOps
1. Open your Azure DevOps pipeline → **Edit → Variables**
2. Add variable: `azureStaticWebAppsApiToken`
3. Paste the token — mark it as **secret**

### 3 — Create the pipeline
1. Azure DevOps → **Pipelines → New Pipeline**
2. Source: **GitHub** → select this repo
3. Choose **Existing Azure Pipelines YAML file**
4. Select `/pipeline/azure-pipelines.yml`
5. Save and run

### 4 — Push any change to trigger a deploy
Update the points table data in `index.html` after each match and push — the pipeline redeploys automatically.

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML, CSS, Vanilla JS (no framework, no build step) |
| Hosting | Azure Static Web Apps (Free tier) |
| CI/CD | Azure DevOps Pipelines |
| Source control | GitHub |

## Project Structure

```
ipl-2026-dashboard/
├── index.html                  # The entire app — points table, fixtures, results
├── staticwebapp.config.json    # Azure SWA routing config
├── pipeline/
│   └── azure-pipelines.yml     # Deploy to Azure Static Web Apps on push
└── README.md
```
