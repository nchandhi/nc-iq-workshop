# Foundry IQ + Fabric IQ Workshop - Deployment Guide

## Prerequisites

### Required Access
- [ ] Azure subscription with **Contributor** access
- [ ] Microsoft Fabric workspace with **F2+ capacity**
- [ ] Workspace admin permissions in Fabric

### Required Tools
- [ ] [Visual Studio Code](https://code.visualstudio.com/download)
- [ ] [Azure Developer CLI (azd)](https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd)
- [ ] [Python 3.10+](https://www.python.org/downloads/)
- [ ] [Git](https://git-scm.com/downloads)

### Quick Start Options
- **GitHub Codespaces** (recommended): All tools pre-installed
- **VS Code Dev Container**: Local container with tools
- **Local Setup**: Install tools manually

---

## Deployment Steps

### Step 1: Clone Repository
```bash
git clone https://github.com/nchandhi/nc-iq-workshop.git
cd nc-iq-workshop
```

### Step 2: Login to Azure
```bash
azd auth login
```

### Step 3: Deploy Azure Resources (~7 min)
```bash
azd up
```
When prompted:
- Environment name: `iq-workshop-yourname`
- Subscription: Select your subscription
- Location: `eastus2` or `westus2`

### Step 4: Configure Fabric
1. Go to [Microsoft Fabric](https://app.fabric.microsoft.com)
2. Create or open a workspace
3. Copy Workspace ID from URL: `https://app.fabric.microsoft.com/groups/{workspace-id}/...`

### Step 5: Configure Environment
```bash
cp .env.example .env
```
Edit `.env` and set:
```
FABRIC_WORKSPACE_ID=your-workspace-id-here
DATA_FOLDER=data/default
```

### Step 6: Setup Python Environment
```bash
cd scripts
python -m venv .venv
.venv\Scripts\activate          # Windows
# source .venv/bin/activate     # macOS/Linux
pip install uv && uv pip install -r requirements.txt
```

### Step 7: Build Solution (~5 min)
```bash
python scripts/00_build_solution.py --from 02
```

### Step 8: Test the Agent
```bash
python scripts/08_test_foundry_agent.py
```

---

## What Gets Deployed

| Resource | Purpose |
|----------|---------|
| Azure AI Foundry | Hosts Orchestrator Agent and Foundry IQ |
| Azure AI Services | GPT-4o-mini and embeddings |
| Azure AI Search | Document vectors |
| Storage Account | Documents and artifacts |

---

## Sample Questions to Try

**Structured Data (Fabric IQ):**
- "How many outages occurred last month?"
- "What is the average resolution time for tickets?"

**Unstructured Data (Foundry IQ):**
- "What are the policies for notifying customers?"
- "What steps must be taken to escalate an outage?"

**Combined (Both):**
- "Which outages exceeded our policy thresholds?"

---

## Customize for Your Use Case

After testing the default scenario, build your own custom solution:

### Option 1: Command Line
```bash
python scripts/00_build_solution.py --clean \
    --industry "Insurance" \
    --usecase "Claims processing and policy management"
```

### Option 2: Environment Variables
Edit `.env`:
```
INDUSTRY=Insurance
USECASE=Claims processing and policy management
```
Then run:
```bash
python scripts/00_build_solution.py --clean
```

### Sample Industries
- Telecommunications, Insurance, Finance, Retail
- Manufacturing, Education, Hospitality
- Real Estate, Energy, Logistics

The `--clean` flag creates fresh Fabric artifacts for each new scenario.

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `azd up` fails | Check subscription permissions |
| Fabric errors | Verify Workspace ID in .env |
| Python errors | Ensure venv is activated |

> **Tip:** Use GitHub Copilot Chat (`Ctrl+I` in VS Code) for help with errors.

---

## Resources

- Workshop: https://nchandhi.github.io/nc-iq-workshop/
- Repository: https://github.com/nchandhi/nc-iq-workshop