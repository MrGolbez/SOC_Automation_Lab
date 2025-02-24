# SOC Automation Lab

## Objective

The Detection Lab project aimed to establish a controlled environment for simulating and detecting cyber attacks. The primary focus was to ingest and analyze logs within a Security Information and Event Management (SIEM) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies.

### Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in understanding rule and index creation using Wazuh. 
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of SIEM configuration and navigation.
- Development of critical thinking and problem-solving skills in cybersecurity.
- Configuration of Sysmon for log ingestion into Security Information and Event Management (SIEM) system

### Tools Used

- Security Information and Event Management (SIEM) system for log ingestion and analysis with Wazuh.
- Security Operation Center Case Management and SOAR workflow using TheHive and Shuffler.io workflow.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Workflow Example Depicted in Our Logical Diagram

1. **Wazuh** detects potential malware activity on an endpoint.
2. An automated task in **Shuffler** initiates a predefined response.
3. **Shuffler** isolates the affected endpoint and blocks associated IPs.
4. **Shuffler** logs incident details and response actions into **TheHive**.
5. **TheHive** creates a new case based on the alert's severity and type.
6. Security analysts collaborate within **TheHive** to investigate and respond.
7. **TheHive** archives the case, documenting the incident and actions taken.

## Visual Diagram of Our SOC Automation Lab

![SOC Lab Diagram](https://github.com/user-attachments/assets/aa1344d9-7089-40ae-a3b5-438d3d177afc)

## Project Walkthrough

### Step 1: SOC Automation Lab Diagram
I started by sketching out my SOC Automation Lab using draw.io. This diagram was crucial for logically planning the information flow and server setup.

### Step 2: Setting up Sysmon on Windows Machines
- Downloaded Sysmon v15.14 from Microsoft.
- Replaced the default config.xml with sysmonconfig.xml from [this GitHub repo](https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml).
- Installed Sysmon using PowerShell: `.\Sysmon64.exe`.
- Once Sysmon is installed you will want to double check that sysmonconfig is in the same directory as sysmon.
- Sysmon will create lots of noise and collect everything.
- This is why following through Olafhartongs repo wil help ensure it does not make too much noice. 


![Installing sysmon](https://github.com/user-attachments/assets/4d135abe-c0e1-4f3a-b4dd-33c76bb0433d)
![verifying sysmon installed](https://github.com/user-attachments/assets/292cdeb8-708d-4a9f-9200-4bdb33b5abb0)
![Ensuring sysmonconfig is same directory as sysmon](https://github.com/user-attachments/assets/dacf07c1-d615-4e9b-a760-7c4a5778308f)

### Step 3: Choosing a Cloud Provider
To host Wazuh & TheHive servers, I took advantage of Digital Ocean's $200 credit for new users.
You can use any Cloud Provider you would like but as I mentioned I took advantage of Digital Ocean's new user credit and wanted to gain some experience using the platform as I have heard a lot of good things about it. 

### Step 4: Creating Wazuh & TheHive Servers
- Created Droplets with the following specs:
  - **Droplet Type**: Basic (Shared CPU)
  - **CPU**: Premium Intel
  - **Disk**: NVMe SSD
  - **8 GB RAM / 2 Intel CPUs, 160 GB NVMe SSDs, 5 TB Transfer
- Configured server access via SSH.
- I used the same droplet configuration for both of my servers shown below.

![creating Ubuntu droplet](https://github.com/user-attachments/assets/749259c1-90b7-4c89-a05a-8d64a4fd8197)


### Step 5: Creating a Firewall
I configured a firewall to initially allow all TCP and UDP traffic for setup purposes, ensuring our servers are protected.

![Firewall Rules](https://github.com/user-attachments/assets/d0b0d1a3-01f5-41c1-9d12-82b40e85a75a)

### Step 6: Adding Droplets to the Firewall
- Added the Wazuh & TheHive Droplets to the firewall for enhanced security.
- Click on Firewall --> Add Droplets --> Choose the Droplet --> Repeat for the other Droplets.
- The screenshot shows what a Droplet being added into a Firewall looks like. 

![adding Wazuh to firewall](https://github.com/user-attachments/assets/299f8d44-c709-4b09-97c7-dd39bd7e632a)
![Servers added to Firewall](https://github.com/user-attachments/assets/911edee0-4012-43e5-a39f-b9078e3bd369)

### Step 7: Preparing for Configuration
Noted down public IP addresses and SSHed into each server to prepare for configuration.
- IMPORTANT STEP! Moving forward we will be given various IPs, and credentials for our servers and services. It is important to notate these down in your documentation.
- There are ways to get something like TheHive credentials again but is extra steps and ensuring proper documentation will always help!

### Step 8: Configuring Wazuh Server
- Updated and upgraded packages.
- Installed Wazuh using: `curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`.
- Start up and run the update and upgrade on your Wazuh server: `sudo apt-get update && apt-get upgrade -y`
- This will get the Dashboard set up but we need to do some configuration to ensure proper logging and such which we will do after setting up TheHive.

![Wazuh dashboard successful login!](https://github.com/user-attachments/assets/1cf5e8ee-30ce-45cc-9829-09deb108b542)
![sysmon rules for wazuh](https://github.com/user-attachments/assets/6373520a-2421-422d-aa24-c2a168a3efd0)

### Step 9: Configuring TheHive Server
- Installed necessary dependencies.
- Configured Java, Cassandra, and Elasticsearch.
- Installed TheHive.

![ensuring thehive status is up and running](https://github.com/user-attachments/assets/e1c6ff95-a2c4-4c6c-be24-8b3aaa5d9f20)

### Step 10-14: Installing Dependencies on TheHive Server
- Installed Java, Cassandra, Elasticsearch, and TheHive with specified commands.

![Installing TheHive dependencies](https://github.com/user-attachments/assets/a9b67055-b718-45f6-a8f8-383d42618298)
![Enabling elasticsearch service](https://github.com/user-attachments/assets/f8685464-b032-4fc7-85f3-59d801bfb0b8)


### Step 15: TheHive Credentials
Default credentials for TheHive dashboard are `admin@thehive.local` / `secret`.

### Step 16-18: Configuration Adjustments
- Edited Cassandra and Elasticsearch configuration files.
- Ensured network settings and authentication methods are correctly set.

![Configuration Elasticsearch Network](https://github.com/user-attachments/assets/6e848685-2a48-49fe-8786-a278c3ec6062)
![Elasticsearch Cluster Name and Enabling Nodename](https://github.com/user-attachments/assets/4081ce01-e202-452f-bf8f-1dc6ce5c3b10)
![Cleaning up Cassandra and Restarting the Service](https://github.com/user-attachments/assets/451ad8fd-7bc7-4816-9db8-c84f6bc5776d)
![Filebeat for Log Ingestion](https://github.com/user-attachments/assets/134f2051-ba0b-4d05-9bf3-fd262b48ed26)

### Step 19-20: TheHive Ownership and Configuration
- Changed ownership of `/opt/thp` to TheHive.
- Edited application.conf for database and index configurations.
- Then a confirmation that my services were running. 

![TheHive Ownership](https://github.com/user-attachments/assets/b6a067dc-4a19-4961-b44f-f114be09f78a)
![Running Services](https://github.com/user-attachments/assets/e7d295a2-aee8-4bc6-9d78-913d03dcda3a)


### Step 21: Login to TheHive Dashboard
Logged in and explored TheHive dashboard. It was rewarding to see the fruits of my labor!


### Step 22-24: Configuring Wazuh Dashboard and Ingesting Telemetry
- Added a Windows Agent within Wazuh for Log Ingestion.
- Reviewed location of Wazuh log archives and implemented proper log ingestion. 

![wazuh log archives](https://github.com/user-attachments/assets/56ac1c76-7f0b-4b14-b00e-3f4497ef610c)
![Deploying windows wazuh agent](https://github.com/user-attachments/assets/dc5a108b-4324-40ce-94f6-fd3f57756c99)
![log ingesting for wazuh](https://github.com/user-attachments/assets/51d1b6a5-1eb0-4b21-af40-9d6ff8508d01)
![Showing our windows agent on Wazuh](https://github.com/user-attachments/assets/0b3f9edd-a0c3-4209-836a-b43c2107c516)

### Step 25: Creating Custom Wazuh Rule
Created a rule to detect Mimikatz using Sysmon event ID.

![mimikatz rule](https://github.com/user-attachments/assets/b3a347eb-51af-4981-8c7f-3d408a6e36f3)
![mimkatz event grep](https://github.com/user-attachments/assets/fc717b59-dfe4-472e-a7a1-cb8b64eec879)

### Step 26: Testing Custom Rule
Validated Mimikatz detection.

![showing our sysmon is picking up mimikatz](https://github.com/user-attachments/assets/e4f923bd-bdbb-4095-abca-cea5f4a56c8c)

### Step 27: Integrating SOAR with Shuffler
- Created an account on Shuffler.
- I was not yet able to fully integrate my SOAR solution into Wazuh as I relieved it is a big part of the project and will need more research on getting that configured fully.
- Instead I got the webhook configured so that I can test it with my mimikatz detection and start getting some experience configuring that. 

![Webhook](https://github.com/user-attachments/assets/4b01167f-4bc9-4cfc-98cb-543e81ea60f5)
![webhook for shuffle into wazuh](https://github.com/user-attachments/assets/5262fc9a-f8b7-4369-ad58-e7474e7430d1)


---

## Conclusion

By following these detailed steps, you can establish a fully functional SOC Automation Lab equipped with advanced detection, analysis, and response capabilities.
Continuously update and refine your configurations and strategies to stay ahead of evolving threats in the cybersecurity landscape.

This was a long and tedious process but extremely amazing project and I appreciate the youtuber MYDFIR for this project idea and information they gave the workflow. 
I am still working on setting up the automation using Shuffler and TheHive, but have had to do it little slower because of working on my Senior Project for Computer 
Technology BA. Be sure to stay tuned in for updates with screenshots my automation workflow!
I appreciate you taking the time and reading and going through this project with me. 

