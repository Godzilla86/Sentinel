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
---
 
## 📂 Contents
 
| File | Description |
|------|-------------|
| [SentinelSetupGuide.md](./SentinelSetupGuide.md) | End-to-end setup guide for Microsoft Sentinel — workspace creation, data connectors, analytics rules, and SOAR playbooks |
| [User Sign-In from Different Countries Within 1 Hour](./User%20Sign-In%20from%20Different%20Countries%20Within%201%20Hour) | KQL detection rule for impossible travel — flags logins from geographically impossible locations within a 1-hour window |
| [Successful Sign In from High-Risk Countries](./Successful%20Sign%20In%20from%20High-Risk%20Countries) | KQL rule to alert on successful authentications originating from high-risk or sanctioned regions |
| [BaslineRules](./BaselineRules.json) | Baseline rules I deploy for new sentinel instances around user detections and conditional access
 
## 🧰 Tech Stack Referenced
 
![Microsoft Sentinel](https://img.shields.io/badge/Microsoft%20Sentinel-0078D4?style=flat&logo=microsoft&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-0089D6?style=flat&logo=microsoftazure&logoColor=white)
![KQL](https://img.shields.io/badge/KQL-Kusto%20Query%20Language-blue?style=flat)
![Microsoft 365](https://img.shields.io/badge/Microsoft%20365-D83B01?style=flat&logo=microsoftoffice&logoColor=white)
![Entra ID](https://img.shields.io/badge/Entra%20ID-0078D4?style=flat&logo=microsoft&logoColor=white)
 
---
