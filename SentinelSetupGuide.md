Microsoft Sentinel Low-Cost Setup Guide

This guide is based on my hands-on experience configuring Microsoft Sentinel.

It explains how to configure Microsoft Sentinel for a low-cost small business environment using free Microsoft connectors. At the time of writing, expected monthly ingestion costs should remain below $100 AUD for a small business with 5–15 users.

**Important Note:**

Before following this guide, ensure you already have an Azure subscription. Based on experience, using a separate subscription makes cost tracking significantly easier.


**Step 1: Onboarding**

Deploy the ARM template from the official Microsoft Azure Sentinel GitHub repository. This template provides a clean, barebones starting point.
<img width="1828" height="941" alt="image" src="https://github.com/user-attachments/assets/0c8e8fec-0a18-427d-a0b6-0502e905bcdb" />

Select your Azure subscription and configure the settings as appropriate.
<img width="725" height="641" alt="image" src="https://github.com/user-attachments/assets/b15d3257-dc76-4cea-abdc-3af6875f1f65" />

**Tip:** Use a consistent naming convention to keep your environment clean and readable. This becomes extremely important for scripting and automation later.
If cost is a concern, you can configure a daily ingestion cap. However, it is recommended to review your first 30 days of ingestion to establish a realistic cost baseline before applying limits.
Tiered pricing may reduce costs if you use a Data Lake instead of the default Log Analytics workspace, but this comes with slower query performance.
If you work with a Microsoft licensing reseller, you may also obtain better pricing through capacity commitments.

Select your Essentials modules:
- SOC Handbook
- UEBA Essentials
  
These include useful playbooks for later automation.
<img width="706" height="778" alt="image" src="https://github.com/user-attachments/assets/4917baec-e03d-4608-adcf-f60f8bfd9065" />

**Recommended Initial Data Connectors**

Start with:
- Office 365
- Azure Activity
- Defender for Endpoint (even if not your primary EDR)
- Microsoft Entra ID (configured manually later; may incur cost)

<img width="819" height="789" alt="image" src="https://github.com/user-attachments/assets/6ea9c65f-2455-4716-a6e9-0647b3f50832" />

**Tip:** Common Free Microsoft Data Connectors

- Azure AD Identity Protection (requires Entra ID P2)
- Azure Activity (free)
- Microsoft Defender for Endpoint (requires Defender licensing)
- Microsoft Defender for Cloud (requires relevant licensing)
- Microsoft Insider Risk Management
- Office 365 (free connector)

If you aren't sure what your Microsoft 365 licensing entitles you to check out [m365maps.com](https://m365maps.com/)

**Step 2: Configuration**

Navigate to the **Microsoft Defender Portal:**

System → Settings → Microsoft Sentinel

Connect the workspace created during the onboarding process.
<img width="1446" height="425" alt="image" src="https://github.com/user-attachments/assets/c404ea39-5e9b-4084-86ca-6f955510449b" />

Once connected:

- Required tables will be created
- Data ingestion will begin
<img width="563" height="905" alt="image" src="https://github.com/user-attachments/assets/6a958063-f066-4dfc-9650-3a9bac7599ef" />

**Enable UEBA**

Enable User and **Entity Behavior Analytics (UEBA)**
This uses machine learning to detect anomalies and correlate suspicious activity across users and entities.

<img width="911" height="817" alt="image" src="https://github.com/user-attachments/assets/1e276e63-722f-476a-9ce9-117450c0ea8a" />

**Configure Playbook Permissions**

Ensure proper permissions are configured so that Sentinel can execute Logic App playbooks.
<img width="628" height="694" alt="image" src="https://github.com/user-attachments/assets/a3a25c3d-d95a-45de-8078-b53d6c336348" />


**Step 3: Data Connectors**

Navigate to:

Microsoft Sentinel → Configuration → Data Connectors

You will see a list of connectors.

✅ A green status indicates the connector is enabled

⚠️ This does not guarantee data is being ingested

<img width="1829" height="911" alt="image" src="https://github.com/user-attachments/assets/59d52438-8e5a-4f6f-a47b-16ee632de48b" />

**Verify Data Ingestion**
Open each connector and confirm:

- Logs are enabled
- Tables are receiving data
  
Example:
A connector may appear enabled (e.g., Microsoft Entra ID) but not ingesting logs until log categories are explicitly selected.
Once configured, data will begin flowing into the selected tables.
<img width="883" height="625" alt="image" src="https://github.com/user-attachments/assets/e6dc3793-8237-45c2-80fd-b32c715b22a5" />

**Tip:** Cost Considerations

- All connectors are free to enable
- Data ingestion incurs cost

For Entra ID:

- Users with E5/A5 licenses receive 5 MB free ingestion per user per day


**Step 4: Create Playbooks/Email Alerting**

Navigate to:

Microsoft Sentinel → Automation → Playbooks
Create a playbook using Azure Logic Apps. For this example, use the prebuilt playbook: **“Send Incident Email with XDR” (included in SOAR Essentials)**
<img width="1802" height="897" alt="image" src="https://github.com/user-attachments/assets/a5d8e7ea-6543-4947-aafd-032929ed7fd5" />

**Tip:** I would advise looking up documentation for your ITSM platform as there is most likely an API intergration that will allow the email to come through on your ticketing board with enriched infromation before the initial ticket triaged e.g. Customer name, severity, hyperlinks, enhanced formatting etc.

<img width="1474" height="877" alt="image" src="https://github.com/user-attachments/assets/468b17b3-bb58-41ad-b068-2a3da9305b08" />

Create the playbook and enter details.
<img width="1145" height="642" alt="image" src="https://github.com/user-attachments/assets/ea72686d-e19d-4129-abdc-859d2da6ffd8" />

You will notice as you are about to create the playbook that two new connections will be made. One is a managed identity Sentinel can connect to run the playbook and the other is a Office 365 Outlook connection to email the incident.
<img width="1070" height="893" alt="image" src="https://github.com/user-attachments/assets/51d6db2d-436a-4c9e-ad0a-6281b7ab7682" />

Enter the Logic App and configure your choice of email for the Outlook 365 connection
<img width="1897" height="912" alt="image" src="https://github.com/user-attachments/assets/679d02e0-104d-4c52-8997-55d917b89b9c" />
<img width="619" height="364" alt="image" src="https://github.com/user-attachments/assets/6f004c0d-9d77-4888-ab4b-eb4c5ed50741" />

Next we need to authorize the system identity running the HTTP request to pull data from Defender and enrich the email. Run the script found in the documentation for this playbook (https://github.com/Azure/Azure-Sentinel/blob/master/Solutions/SentinelSOARessentials/Playbooks/Send-Incident-Email-XDRPortal/readme.md)

**Tip** Use a user-assigned managed identity rather than a system-assigned identity. This allows:

- Better auditing
- Reuse across multiple playbooks
- Centralized permission management

In order to get the email sent every time there is a new incident we need to attact the book to a automation rule. 

Navigate to:
Microsoft Sentinel → Automation → Automation Rules

<img width="1066" height="1258" alt="image" src="https://github.com/user-attachments/assets/b0d2d354-6479-4c57-91af-0852be10abdb" />


Following this configuration it means that when a new incident is created the playbook will run and gather information from the incident in Defender and send the email

**Step 5: Analytic Rules**

Analytic Rules use KQL (Kusto Query Language) to detect suspicious activity.

When conditions are met:

- An alert is generated
- An incident is created 

**Getting Started**

Analytic Rules → Rule Templates
Start with prebuilt templates, then move to custom rules.

**Tip:** Use descriptive names instead of generic ones.
- Example: User Sign-In from Different Countries Within 1 Hour
- Avoid: Impossible Travel

<img width="1481" height="897" alt="image" src="https://github.com/user-attachments/assets/ca7f0005-4dd5-4660-8112-247f95ad38e1" />


**Tip:** Ensure Query runtime is greater than Lookback time

Example:

If your query reviews 1 hour of data → ensure execution runs within that window

<img width="2039" height="1233" alt="image" src="https://github.com/user-attachments/assets/e0bb2e29-d210-489b-a941-55e044d6333d" />


**Tip:**Group alerts by Account

This keeps incidents clean, organized, and user-focused
<img width="2044" height="1245" alt="image" src="https://github.com/user-attachments/assets/0de1db4c-6285-4404-8dfd-c61861b9f135" />






