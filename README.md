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

![Security Operations Center Lab Diagram](SOC Automation Lab/SOC Lab Diagram.png)

## Project Walkthrough

### Step 1: SOC Automation Lab Diagram
I started by sketching out my SOC Automation Lab using draw.io. This diagram was crucial for logically planning the information flow and server setup.

### Step 2: Setting up Sysmon on Windows Machines
- Downloaded Sysmon v15.14 from Microsoft.
- Replaced the default config.xml with sysmonconfig.xml from [this GitHub repo](https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml).
- Installed Sysmon using PowerShell: `.\Sysmon64.exe`.

![Sysmon Installation](path/to/screenshot_sysmon_installation.png)

### Step 3: Choosing a Cloud Provider
To host Wazuh & TheHive servers, I took advantage of Digital Ocean's $200 credit for new users.

![Digital Ocean Droplet Setup](path/to/screenshot_droplet_setup.png)

### Step 4: Creating Wazuh & TheHive Servers
- Created Droplets with the following specs:
  - **Droplet Type**: Basic (Shared CPU)
  - **CPU**: Premium Intel
  - **Disk**: NVMe SSD
  - **8 GB RAM / 2 Intel CPUs, 160 GB NVMe SSDs, 5 TB Transfer
- Configured server access via SSH.

![Droplet Creation](path/to/screenshot_droplet_creation.png)

### Step 5: Creating a Firewall
I configured a firewall to initially allow all TCP and UDP traffic for setup purposes, ensuring our servers are protected.

![Firewall Rule Configuration](path/to/screenshot_firewall_configuration.png)

### Step 6: Adding Droplets to the Firewall
Added the Wazuh & TheHive Droplets to the firewall for enhanced security.

![Droplets Added to Firewall](path/to/screenshot_droplets_firewall.png)

### Step 7: Preparing for Configuration
Noted down public IP addresses and SSHed into each server to prepare for configuration.

### Step 8: Configuring Wazuh Server
- Updated and upgraded packages.
- Installed Wazuh using: `curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`.

![Wazuh Installation](path/to/screenshot_wazuh_installation.png)

### Step 9: Configuring TheHive Server
- Installed necessary dependencies.
- Configured Java, Cassandra, and Elasticsearch.
- Installed TheHive.

![TheHive Installation](path/to/screenshot_thehive_installation.png)

### Step 10-14: Installing Dependencies on TheHive Server
- Installed Java, Cassandra, Elasticsearch, and TheHive with specified commands.

![Cassandra and Elasticsearch Installation](path/to/screenshot_cassandra_elasticsearch_installation.png)

### Step 15: TheHive Credentials
Default credentials for TheHive dashboard are `admin@thehive.local` / `secret`.

### Step 16-18: Configuration Adjustments
- Edited Cassandra and Elasticsearch configuration files.
- Ensured network settings and authentication methods are correctly set.

### Step 19-20: TheHive Ownership and Configuration
- Changed ownership of `/opt/thp` to TheHive.
- Edited application.conf for database and index configurations.

### Step 21: Login to TheHive Dashboard
Logged in and explored TheHive dashboard. It was rewarding to see the fruits of my labor!

![TheHive Dashboard](path/to/screenshot_thehive_dashboard.png)

### Step 22-24: Configuring Wazuh Dashboard and Ingesting Telemetry
- Added a Windows Agent.
- Configured log ingestion from Sysmon and PowerShell.

### Step 25: Creating Custom Wazuh Rule
Created a rule to detect Mimikatz using Sysmon event ID.

### Step 26: Testing Custom Rule
Validated Mimikatz detection.

![Mimikatz Detection](path/to/screenshot_mimikatz_detection.png)

### Step 27: Integrating SOAR with Shuffler
- Created an account on Shuffler.
- Integrated Shuffler with Wazuh Manager for automated response.

![Shuffler Integration](path/to/screenshot_shuffler_integration.png)

### Step 28: Automated Task Creation
- Defined and configured automated tasks in Shuffler.
- Tested tasks for functionality.

### Step 29: Integrating Case Management with TheHive
- Integrated Shuffler alerts with TheHive for case management.
- Configured criteria for case creation and workflows.

![TheHive Case Management](path/to/screenshot_thehive_case_management.png)

---

## Conclusion

By following these steps, you can establish a fully functional SOC Automation Lab with advanced detection, analysis, and response capabilities. Continuous updates and refinements are crucial to stay ahead of evolving threats. Stay tuned for updates and screenshots of the automation workflow.

---

This project was a long and tedious process but extremely rewarding. Special thanks to the YouTuber MYDFIR for the project idea and workflow information. I am still working on setting up the automation using Shuffler and TheHive, but have had to proceed slowly due to my Senior Project for the Computer Technology BA. Stay tuned for updates with screenshots of my automation workflow! Thank you for reading and going through this project with me.


Conclusion

By following these detailed steps, you can establish a fully functional SOC Automation Lab equipped with advanced detection, analysis, and response capabilities.
Continuously update and refine your configurations and strategies to stay ahead of evolving threats in the cybersecurity landscape.

This was a long and tedious process but extremely amazing project and I appreciate the youtuber MYDFIR for this project idea and information they gave the workflow. 
I am still working on setting up the automation using Shuffler and TheHive, but have had to do it little slower because of working on my Senior Project for Computer 
Technology BA. Be sure to stay tuned in for updates with screenshots my automation workflow!
I appreciate you taking the time and reading and going through this project with me. 

