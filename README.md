# IPL 2026 Dashboard

> Live points table, fixtures, and results for the 2026 Indian Premier League — automatically deployed to **Azure Static Web Apps** via **Azure DevOps Pipelines** on every push to `main`.

🔗 **[Live Demo → black-tree-0f9e5e710.1.azurestaticapps.net](https://black-tree-0f9e5e710.1.azurestaticapps.net)**

![Azure Static Web Apps](https://img.shields.io/badge/Azure-Static%20Web%20Apps-0078D4?logo=microsoftazure&logoColor=white)
![Azure DevOps](https://img.shields.io/badge/Azure%20DevOps-Pipeline-0078D4?logo=azuredevops&logoColor=white)
![Deploy](https://img.shields.io/badge/deploy-auto%20on%20push-34d399)

---

## What It Does

A clean, dark-themed dashboard tracking the IPL 2026 season:

- **Points Table** — live standings with NRR, wins, losses, and playoff qualification zones
- **Fixtures** — upcoming matches with venues, dates, and times
- **Results** — completed match outcomes
- **Orange & Purple Cap** — leading run-scorer and wicket-taker
- **Season stats** — matches played, teams, and final date

---

## Architecture

```
Developer pushes to main (GitHub)
           │
           ▼
Azure DevOps Pipeline triggers
           │
           ▼
Self-hosted agent runs SWA CLI
           │
           ▼
Deployed to Azure Static Web Apps
           │
           ▼
Live at azurestaticapps.net in ~60s
```

### Planned Extension — Real-Time Data Pipeline

```
Azure Function (timer trigger, every 30 mins)
           │
           ▼
Cricket API (CricAPI)
           │
           ▼
Azure Blob Storage (data.json, public read)
           │
           ▼
index.html fetches on load + polls every 5 mins
           │
           ▼
Dashboard updates automatically during live matches
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML, CSS, Vanilla JS — no framework, no build step |
| Hosting | Azure Static Web Apps (Free tier) |
| CI/CD | Azure DevOps Pipelines (self-hosted agent) |
| Deployment | Azure Static Web Apps CLI |
| Source control | GitHub |

---

## Repo Structure

```
ipl-2026-dashboard/
├── index.html                  # Entire app — standings, fixtures, results, caps
├── staticwebapp.config.json    # Azure SWA routing config
├── pipeline/
│   └── azure-pipelines.yml     # CI/CD pipeline definition
└── README.md
```

---

## CI/CD Pipeline

The pipeline (`/pipeline/azure-pipelines.yml`) does three things on every push to `main`:

1. **Checkout** — pulls latest code from GitHub
2. **Install SWA CLI** — `npm install -g @azure/static-web-apps-cli`
3. **Deploy** — pushes static files to Azure Static Web Apps using a secret deployment token

The deployment token is stored as an encrypted pipeline variable in Azure DevOps — never in source code.

---

## Setup (Run It Yourself)

### Prerequisites
- Azure free account — [portal.azure.com](https://portal.azure.com)
- Azure DevOps organisation — [dev.azure.com](https://dev.azure.com)

### Step 1 — Create an Azure Static Web App
1. Azure Portal → **Static Web Apps** → **Create**
2. Plan: **Free**, Source: **Other**
3. Once created → **Manage deployment token** → copy the token

### Step 2 — Create the pipeline
1. Azure DevOps → **Pipelines** → **New Pipeline**
2. Source: **GitHub** → select this repo
3. Choose **Existing Azure Pipelines YAML file** → `/pipeline/azure-pipelines.yml`
4. Add variable: `azureStaticWebAppsApiToken` → paste token → mark as **secret**
5. Save and run

### Step 3 — Update scores
Edit the data arrays in `index.html` after each match and push to `main`. The pipeline redeploys automatically in ~60 seconds.

---

## Updating the Dashboard

Points table and match data live as JavaScript arrays at the top of `index.html`:

```javascript
const teams = [
  { short:"RCB", name:"Royal Challengers Bengaluru", m:1, w:1, l:0, nr:0, nrr:+1.250, pts:2 },
  // update after each match
];

const results = [
  { num:1, date:"28 Mar", t1:"RCB", t2:"SRH", result:"RCB won", venue:"M. Chinnaswamy Stadium" },
  // add each completed match here
];
```

---

## Built By

**Adarsh Malshetty** — DevOps Engineer  
[LinkedIn](https://www.linkedin.com/in/adarsh-malshetty/) · [GitHub](https://github.com/AdarshMalshetty)

*Built during IPL 2026 season opener weekend to demonstrate Azure Static Web Apps and Azure DevOps Pipelines.*