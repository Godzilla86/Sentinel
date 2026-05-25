This guide is a collection of my hands on personal experience configuring Microsoft Sentinel.

The guide is aimed at configuring Microsoft Sentinel for a low cost small business environment by using the free Microsoft connectors. I would expect to see less than $100 of ingestion cost a month at the time of writing this guide as the targeted environment would be a small business inbetween 5 - 15 users.

Please note it is assumed before following this guide that you have a created/already have a Azure subscription. In my experience I found that it is best to have a seperate subscription as its easier to track your spend/consumption.

1. Deploy the ARM template found in the offical Azure Github repository https://github.com/Azure/Azure-Sentinel/tree/master/Tools/Sentinel-All-In-One by Microsoft. I found this template is great as this is a barebones setup.
<img width="1828" height="941" alt="image" src="https://github.com/user-attachments/assets/0c8e8fec-0a18-427d-a0b6-0502e905bcdb" />

Select your subscription for settings suitable for you. I reccomend using a naming scheme to keep your environment readable as this will help make scripting and configuration for the future easier.
<img width="725" height="641" alt="image" src="https://github.com/user-attachments/assets/b15d3257-dc76-4cea-abdc-3af6875f1f65" />
If you are concerned about spend you can adjust your daily ingestion limit but in my experience I would evaluate the first 30 days and adjust from there to know your realistic ingestion cost. My experience for tier pricing is that using a data lake instance to ingest the data is cheaper however the trade off is the speed that you can query data is reduced. If you have a relationship with a reseller for licensing I have found there could be options to commit to a monthly cost of data storage for a discounted price.

<img width="716" height="793" alt="image" src="https://github.com/user-attachments/assets/7923e9b8-34fb-4313-9e63-6a6cc5f5dc7f" />
Select your Essentials. I would reccomend SOC Handbook and UBEA Essentials as there a some playbooks in there we can use later on.

<img width="819" height="789" alt="image" src="https://github.com/user-attachments/assets/6ea9c65f-2455-4716-a6e9-0647b3f50832" />
Select your onboarding data connectors. I would reccomend starting off with 365, Azure Activity and Defender for Endpoint (Even if you don't have Defender as your primary EDR)
