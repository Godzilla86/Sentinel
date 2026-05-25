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
- Microsoft Entra ID (You may have to add thie seperate)

There is a good table in the Github link found at the end of the pages that gives you a good view of what connectors and free or are billed.



**Step 2: Configuration**

Now that we have completed onboarding in the Defender portal navigate to Systen > Settings > Microsoft Sentinel and then connect your workspace that you created during the onboarding proccess. Once connector it should look similar to this.
<img width="1446" height="425" alt="image" src="https://github.com/user-attachments/assets/c404ea39-5e9b-4084-86ca-6f955510449b" />
