# Azure-Honeynet-Project

# Building a SOC + Honeynet in Azure (Live Traffic)
![Untitled Diagram drawio](https://github.com/user-attachments/assets/4b32c70b-7aad-4b01-b6e1-b2a9250860e3)

## Introduction

In this project, I created a mini honeynet in Azure and integrated log sources from various resources into a Log Analytics workspace. This workspace was then used by Microsoft Sentinel to generate attack maps, trigger alerts, and create incidents. I recorded specific security metrics in an unsecured environment for 24 hours, applied security controls to enhance the environment, measured the metrics again for another 24 hours, and presented the results below. The metrics include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Public internet drawio](https://github.com/user-attachments/assets/55da37de-ef17-4d92-b2d4-e71912709278)

## Architecture After Hardening / Security Controls
![After drawio](https://github.com/user-attachments/assets/163657b4-a13a-4a83-920f-6f253bf2ccba)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

# Phase I: Honeynet Deployment
In this initial phase, I set up both Windows and Linux virtual machines within a controlled environment designed to be intentionally vulnerable. To create an enticing target for cyberattacks, I configured the environment to allow unrestricted traffic by deliberately disabling firewalls and Network Security Groups (NSGs). This setup simulated a real-world scenario where systems are inadequately protected, thereby attracting potential attackers.

# Phase II: Simulated Attacks and Monitoring
To test the security posture of the vulnerable environment, I used PowerShell to simulate various cyberattack scenarios. These included brute-force attempts to gain unauthorized access, privilege escalation attacks to obtain higher-level permissions, and other common attack vectors. Throughout this phase, I continuously monitored the environment using Log Analytics Workspace, where I captured logs and alerts generated by these simulated attacks. By crafting and running Kusto Query Language (KQL) queries, I was able to analyze these events in detail, identifying patterns and correlating data to understand the attack dynamics.

# Phase III: Incident Analysis and Response
Over a 24-hour period, I closely monitored and assessed the incidents that occurred within the honeynet. This involved a deep dive into the details of each attack, including identifying the attackers' IP addresses, understanding the techniques they employed, and mapping out the timeline of their activities. Through this analysis, I was able to distinguish between true positives—genuine threats—and false positives—benign events mistakenly flagged as malicious. This phase provided critical insights into the attackers' behaviors and the vulnerabilities within the system.

# Phase IV: Environment Hardening and Compliance
Based on the findings from the incident analysis, I implemented a series of security enhancements to harden the environment. These included disabling public access to the virtual machines, creating private endpoints to ensure that only trusted sources could access the systems, and reconfiguring NSGs to restrict traffic exclusively to known and trusted IP addresses. All security measures were aligned with the NIST 800-53 guidelines, ensuring that the environment not only became more secure but also compliant with industry best practices.

![68747470733a2f2f692e696d6775722e636f6d2f365054473763306c2e706e67](https://github.com/user-attachments/assets/3a92f4b4-b5d0-4b03-8e1c-240eff72f95a)

# Phase V: Results and Metrics Comparison
In the final phase, I conducted a comparative analysis of the environment's security metrics before and after implementing the hardening measures. Initially, the honeynet attracted a substantial amount of malicious traffic, with various attack attempts recorded. However, after the security controls were put in place, the attack surface was significantly reduced, and the malicious activity ceased entirely. This demonstrated the effectiveness of the hardening measures in mitigating risks and protecting the environment from potential threats.



## Attack Maps Before Hardening / Security Controls
For the "BEFORE" metrics, all resources were initially deployed with exposure to the internet. The Virtual Machines had their Network Security Groups and built-in firewalls fully open, and all other resources were deployed with public endpoints accessible from the internet, meaning Private Endpoints were not utilized.

For the "AFTER" metrics, Network Security Groups were tightened by blocking all traffic except from my admin workstation, and all other resources were secured using their built-in firewalls along with Private Endpoints.

 <b>This attack map showcases failures against the MSSQL server. </b>
![mssql-auth-fail](https://github.com/user-attachments/assets/704ae5f3-f1fb-4fa1-ba0a-a392e8285ca1)
<br />
<br />
 
 <b>This attack map highlights the incidents for syslog authentication failures experienced by the Linux server. </b>
![linux-ssh-auth-fail](https://github.com/user-attachments/assets/ad5044b1-8653-4e02-a26f-ec1aa5bdd1db)
<br />
<br />
 
<b>This attack map showcases the number of incidents generated by leaving the Network Security Group (NSG) open.</b>
![nsg-malicious-allowed-in](https://github.com/user-attachments/assets/194c0c06-1d46-4f37-b2d2-4003d53a0fca)
<br />
<br />
 
<b>This attack map showcases RDP and SMB failures against the Window machine.</b>
![windows-rdp-auth-fail](https://github.com/user-attachments/assets/0e8f6898-36e9-43e4-8e17-19894b2bd55b)



## Metrics Before Hardening / Security Controls

The table below displays the metrics we measured in our insecure environment over a 24-hour period:
Start Time 8/15/2024 14:45:52<br/>
Stop Time 8/16/2024 14:45:52

| Metric                                       | Count
| -------------------------------------------- | -----
| SecurityEvent (Windows VMs)                  | 49973
| Syslog (Linux VMs)                           | 17988
| SecurityAlert (Microsoft Defender for Cloud) | 2
| SecurityIncident (Sentinel Incidents)        | 420
| AzureNetworkAnalytics_CL                     | 1660

## Attack Maps Before Hardening / Security Controls

```All map queries returned no results due to the absence of malicious activity during the 24-hour period following the hardening.```

## Metrics After Hardening / Security Controls

The table below displays the metrics we measured in our environment over another 24-hour period, this time after applying security controls:
Start Time 8/17/2024 0:57:33<br/>
Stop Time	8/18/2024 0:57:33

| Metric                                       | Count
| -------------------------------------------- | -----
| SecurityEvent (Windows VMs)                  | 18323
| Syslog (Linux VMs)                           | 1
| SecurityAlert (Microsoft Defender for Cloud) | 0
| SecurityIncident (Sentinel Incidents)        | 0
| AzureNetworkAnalytics_CL                     | 0

### Impact of Security Controls 

| Metric                                       | Change post-hardening
| -------------------------------------------- | -----
| SecurityEvent (Windows VMs)                  | 63.33%
| Syslog (Linux VMs)                           | 99.99%
| SecurityAlert (Microsoft Defender for Cloud) | 100%
| SecurityIncident (Sentinel Incidents)        | 100%
| AzureNetworkAnalytics_CL                     | 100%

## Conclusion

In this project, a mini honeynet was deployed in Microsoft Azure, with various log sources integrated into a Log Analytics workspace. Microsoft Sentinel was configured to trigger alerts and automatically create incidents based on the logs ingested from the honeynet environment. Metrics were collected in two stages: first, in the insecure environment prior to the application of any security controls, and then again after implementing a series of security measures.

The results showed a marked reduction in security events and incidents following the application of security controls, highlighting their effectiveness in mitigating potential threats. This included a significant decrease in unauthorized access attempts, suspicious activities, and other malicious behaviors that were prevalent in the unsecured state.

It's important to note that if the network resources had been actively utilized by regular users during the 24-hour monitoring period after the security controls were implemented, there could have been an increase in security events and alerts. This would have provided additional data points for assessing the effectiveness of the controls under real-world usage conditions.
