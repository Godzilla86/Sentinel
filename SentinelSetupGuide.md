This guide is a collection of my hands on personal experience configuring Microsoft Sentinel.
This guide explains how to configure Microsoft Sentinel for a low-cost small business environment using free Microsoft connectors. At the time of writing, I would expect monthly ingestion costs to stay below $100 for a small business with 5 to 15 users.
**Please note:** Before following this guide, make sure you already have an Azure subscription. Based on my experience, using a separate subscription makes it easier to track costs.



**Step 1: Onboarding**

Deploy the ARM template found in the official Azure Github repository https://github.com/Azure/Azure-Sentinel/tree/master/Tools/Sentinel-All-In-One by Microsoft. This is a great template as this is a barebones setup.
<img width="1828" height="941" alt="image" src="https://github.com/user-attachments/assets/0c8e8fec-0a18-427d-a0b6-0502e905bcdb" />

Select your desired subscription and the region settings that are suitable for you. I recommend using a naming scheme to keep your environment readable as this will make scripting and configuration easier.
<img width="725" height="641" alt="image" src="https://github.com/user-attachments/assets/b15d3257-dc76-4cea-abdc-3af6875f1f65" />

**Tip:** If cost is a concern, you can set a daily ingestion limit. However, I recommend reviewing your first 30 days of ingestion to get a realistic view of your monthly Microsoft Sentinel costs. Tiered pricing may be cheaper if you ingest data into a Data Lake instead of the default Analytics Workspace, but the trade-off is slower query performance. If you work with a Microsoft licensing reseller, you may also be able to secure lower pricing through a monthly commitment.

Select your Essentials. I would recommend SOC Handbook and UBEA Essentials as there are some playbooks we can use later on.
<img width="706" height="778" alt="image" src="https://github.com/user-attachments/assets/4917baec-e03d-4608-adcf-f60f8bfd9065" />

Select your onboarding data connectors. I would recommend starting off with 
- Office 365
- Azure Activity
- Defender for Endpoint (Even if you don't have Defender as your primary EDR)
- Microsoft Entra ID (You will have to add this later and there is also cost)
<img width="819" height="789" alt="image" src="https://github.com/user-attachments/assets/6ea9c65f-2455-4716-a6e9-0647b3f50832" />

**Tip:** Below are the free Microsoft Data Connectors

- Azure Active Directory Identity Protection
- Azure Activity - No license needed
- Microsoft 365 Defender for Endpoint - Microsoft 365 Defender License
- Microsoft Defender for Cloud - Microsoft Defender for Cloud License
- Microsoft Insider Risk Management - Insider Risk Management
- Office 365 - No License

If you aren't sure what you current Microsoft 365 licensing includes I advise checking out m365maps.com

**Step 2: Configuration**

Navigate to the Defender portal. System > Settings > Microsoft Sentinel. Connect your workspace that you created during the onboarding proccess. Once connect it should look similar to this.
<img width="1446" height="425" alt="image" src="https://github.com/user-attachments/assets/c404ea39-5e9b-4084-86ca-6f955510449b" />

Once the workspace is connected the data connectors will create the required table and begin to ingest data.
<img width="563" height="905" alt="image" src="https://github.com/user-attachments/assets/6a958063-f066-4dfc-9650-3a9bac7599ef" />

You will want to enable User and Entity Behavior Analytics as this feature uses AI to help corrolate potential anomolies in your environment. 
<img width="911" height="817" alt="image" src="https://github.com/user-attachments/assets/1e276e63-722f-476a-9ce9-117450c0ea8a" />

Playbook permissions need to be configured in order for Microsoft Sentinel to run any playbooks created.
<img width="628" height="694" alt="image" src="https://github.com/user-attachments/assets/a3a25c3d-d95a-45de-8078-b53d6c336348" />


**Step 3: Data Connectors**

Now we will want to ensure that all our Data connectors are configured. Going into Microsoft Sentinel > Configuration > Data connectors. You will see a list of current connectors. If the connect is green this means it enable but this dosen't mean data is nesscarlly ingesting. To check open the connector
<img width="1829" height="911" alt="image" src="https://github.com/user-attachments/assets/59d52438-8e5a-4f6f-a47b-16ee632de48b" />

Here we can see that Microsoft Entra ID is enabled but isn't gathering any logs. To ensure data is being collected we must enable the logs we want. After applying the changes this will start ingesting the data into the tables we selected.
<img width="883" height="625" alt="image" src="https://github.com/user-attachments/assets/e6dc3793-8237-45c2-80fd-b32c715b22a5" />

**Tip:** All Data connectors are free to install however the ingestion of the data will most likely cost. In this case Entra ID will cost however if you have one of the 5 tier licenses e.g. A5, E5, etc you can get 5MB free each user per day.


**Step 4: Create Playbooks/Email Alerting**

Now will configure our first Playbook by going to Microsoft Sentinel > Automation > Playbooks. From here you can create a playbook from scratch which are 'Azure Logic Apps'. However in this example I will setup a playbook to send email with infromation from an incident.
<img width="1802" height="897" alt="image" src="https://github.com/user-attachments/assets/a5d8e7ea-6543-4947-aafd-032929ed7fd5" />

**Tip:** I would advise looking up documentation for your ITSM platform as there is most likely an API intergration that will allow the email to come through on your ticketing board with enriched infromation before the initial ticket triaged e.g. Customer name, severity, hyperlinks, enhanced formatting etc.

Now search for the 'Send Incident Email' and you should come across the playbook as this is apart of the SOAR Essentials module we installed earlier during the onboarding process.
<img width="1474" height="877" alt="image" src="https://github.com/user-attachments/assets/468b17b3-bb58-41ad-b068-2a3da9305b08" />

Create the playbook and enter details relevant to you e.g. phone number, email etc
<img width="1145" height="642" alt="image" src="https://github.com/user-attachments/assets/ea72686d-e19d-4129-abdc-859d2da6ffd8" />

You will notice as you are about to create the playbook that two new connections will be made. One is a managed identitiy Sentinel can connect to. Two is a Office 365 Outlook connection which will allow to the email of the incident to be sent.
<img width="1070" height="893" alt="image" src="https://github.com/user-attachments/assets/51d6db2d-436a-4c9e-ad0a-6281b7ab7682" />

Enter the Logic App and configure your choice of email for the Outlook 365 connection
<img width="1897" height="912" alt="image" src="https://github.com/user-attachments/assets/679d02e0-104d-4c52-8997-55d917b89b9c" />
<img width="619" height="364" alt="image" src="https://github.com/user-attachments/assets/6f004c0d-9d77-4888-ab4b-eb4c5ed50741" />

Next we need to authorize the managed identity running the HTTP request to pull data from Defender and enrich the email. Run the script found in the documentation for this playbook (https://github.com/Azure/Azure-Sentinel/blob/master/Solutions/SentinelSOARessentials/Playbooks/Send-Incident-Email-XDRPortal/readme.md)
