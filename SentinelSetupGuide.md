This guide is a collection of my hands on personal experience configuring Microsoft Sentinel.

The guide is aimed at configuring Microsoft Sentinel for a low cost small business environment by using the free Microsoft connectors. I would expect to see less than $100 of ingestion cost a month at the time of writing this guide as the targeted environment would be a small business inbetween 5 - 15 users.

**Please note**: it is assumed before following this guide that you have a created/already have a Azure subscription. In my experience I found that it is best to have a seperate subscription as its easier to track your costs.



**Step 1: Onboarding**


Deploy the ARM template found in the official Azure Github repository https://github.com/Azure/Azure-Sentinel/tree/master/Tools/Sentinel-All-In-One by Microsoft. I found this template is great as this is a barebones setup.
<img width="1828" height="941" alt="image" src="https://github.com/user-attachments/assets/0c8e8fec-0a18-427d-a0b6-0502e905bcdb" />




Select your desired subscription and the region settings that are suitable for you. I reccomend using a naming scheme to keep your environment readable as this will make scripting and configuration easier.
<img width="725" height="641" alt="image" src="https://github.com/user-attachments/assets/b15d3257-dc76-4cea-abdc-3af6875f1f65" />

Tip: If you are concerned about cost you can adjust your daily ingestion limit but in my experience I would evaluate the first 30 days of ingestions and configure a limit as this will help you understand your realistic monthly cost. Tier pricing can be cheaper if you ingest data into a 'Data lake' instance rather then the default 'Analytics Workspace' however the trade off is query lookup speed is reduced if you don't use a Analytic Workspace. If you have a relationship with a reseller for your Microsoft licensing there could be options to recieve reduced pricing if you commit monthly.


<img width="706" height="778" alt="image" src="https://github.com/user-attachments/assets/4917baec-e03d-4608-adcf-f60f8bfd9065" />
Select your Essentials. I would reccomend SOC Handbook and UBEA Essentials as there a some playbooks we can use later on.

<img width="819" height="789" alt="image" src="https://github.com/user-attachments/assets/6ea9c65f-2455-4716-a6e9-0647b3f50832" />

Select your onboarding data connectors. I would reccomend starting off with 
- Office 365
- Azure Activity
- Defender for Endpoint (Even if you don't have Defender as your primary EDR)
- Microsoft Entra ID (You may have to add this seperate)

There is a table in the Github link found at the end of the page that gives you a good view of what connectors and free or are billed.



**Step 2: Configuration**

Now that we have completed onboarding in the Defender portal navigate to Systen > Settings > Microsoft Sentinel and then connect your workspace that you created during the onboarding proccess. Once connector it should look similar to this.
<img width="1446" height="425" alt="image" src="https://github.com/user-attachments/assets/c404ea39-5e9b-4084-86ca-6f955510449b" />

Once you have connected your workspace your data connectors will start pull from your tenant. We will need to configure specific setting to enhance Sentinel.



<img width="563" height="905" alt="image" src="https://github.com/user-attachments/assets/6a958063-f066-4dfc-9650-3a9bac7599ef" />
You will want to enable User and Entity Behavior Analytics as this feature uses AI to help corrolate potential anomolies/incidents in your environment. 


<img width="911" height="817" alt="image" src="https://github.com/user-attachments/assets/1e276e63-722f-476a-9ce9-117450c0ea8a" />
Next Playbook permissions need to be configured by selecting your sentinel resource group inside your subscription. Enabling this gives Microsoft Sentinel permissions to run the playbooks you have configured.


<img width="628" height="694" alt="image" src="https://github.com/user-attachments/assets/a3a25c3d-d95a-45de-8078-b53d6c336348" />
Now Sentinel has full permissions to run and UBEA is starting to learn your environment.

**Step 3: Data Connectors**

Now we will want to ensure that all our Data connectors are configured. Going into Microsoft Sentinel > Configuration > Data connectors. You will see a list of current connectors. If the connect is green this means it enable but this dosen't mean data is nesscarlly ingesting. To check open the connector

<img width="1829" height="911" alt="image" src="https://github.com/user-attachments/assets/59d52438-8e5a-4f6f-a47b-16ee632de48b" />


Here we can see that Microsoft Entra ID is enabled but isn't gathering any logs. To ensure data is being collected we must enable the logs we want. After applying the changes this will start ingesting the data into the tables we selected.

<img width="883" height="625" alt="image" src="https://github.com/user-attachments/assets/e6dc3793-8237-45c2-80fd-b32c715b22a5" />

**Tip** All Data connectors are free to install however the ingestion of the data will most likely cost. In this case Entra ID will cost however if you have one of the 5 tier licenses e.g. A5, E5, etc you can get 5MB free each user per day.
