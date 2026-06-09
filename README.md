# 🛡️ Microsoft Sentinel — Knowledge Base & Detection Library
 
> A practical, hands-on reference built from real-world deployments across managed security environments.
 
---
 
## 👋 About This Repository
 
This repo documents my personal knowledge base for **Microsoft Sentinel**.
 
Everything here comes from real deployments I have configured. I've used these guides and queries while managing security for organisations.
 
**What you'll find here:**
- 📖 Step-by-step Sentinel setup and configuration guides
- 🔍 Custom KQL (Kusto Query Language) detection rules
- 🚨 Real-world threat scenarios with detection logic explained
- 
**Who this is for:**
- Security engineers and IT professionals working with Microsoft Sentinel
- Anyone evaluating or onboarding to Sentinel for the first time
- Hiring managers wanting to see how I think and work *(yes, I see you 👋)*
---
 
## 📂 Contents
 
| File | Description |
|------|-------------|
| [SentinelSetupGuide.md](./SentinelSetupGuide.md) | End-to-end setup guide for Microsoft Sentinel — workspace creation, data connectors, analytics rules, and SOAR playbooks |
| [User Sign-In from Different Countries Within 1 Hour](./User%20Sign-In%20from%20Different%20Countries%20Within%201%20Hour) | KQL detection rule for impossible travel — flags logins from geographically impossible locations within a 1-hour window |
| [Successful Sign In from High-Risk Countries](./Successful%20Sign%20In%20from%20High-Risk%20Countries) | KQL rule to alert on successful authentications originating from high-risk or sanctioned regions |
 
---
 
## 📊 Detection Rules
 
> *More alerts ≠ better security. Signal-to-noise ratio is everything.*
 
When I implemented Sentinel across customer environments, I moved away from out-of-the-box alerts that were creating noise (outdated av defenition update alerts, generic risky user flags) toward **high-fidelity, context-rich detections**
 
Examples of what good detection looks like:
 
| ❌ Low Value Alert | ✅ High Value Detection |
|---|---|
| "Risky user detected" | Impossible travel sign-in within 1 hour |
| "Antivirus alert" | Multiple files copied to USB in short timeframe |
| "Failed login" | 10+ failed sign-ins across multiple devices in 5 mins |
| "Policy change" | Conditional Access Policy modified by x user|
 
The KQL files in this repo reflect that philosophy — each one targets a **specific, meaningful threat behaviour.**
 
---
 
## 🗺️ Roadmap — What's Coming
 
This repo is actively growing. Planned additions include:
 
- [ ] Additional KQL detection rules (Tested from on the job)
- [ ] SOAR playbook setup guides
- [ ] Data connector configuration guides
---
 
## 🧰 Tech Stack Referenced
 
![Microsoft Sentinel](https://img.shields.io/badge/Microsoft%20Sentinel-0078D4?style=flat&logo=microsoft&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-0089D6?style=flat&logo=microsoftazure&logoColor=white)
![KQL](https://img.shields.io/badge/KQL-Kusto%20Query%20Language-blue?style=flat)
![Microsoft 365](https://img.shields.io/badge/Microsoft%20365-D83B01?style=flat&logo=microsoftoffice&logoColor=white)
![Entra ID](https://img.shields.io/badge/Entra%20ID-0078D4?style=flat&logo=microsoft&logoColor=white)
 
---
