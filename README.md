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

For the "BEFORE" metrics, all resources were initially deployed with exposure to the internet. The Virtual Machines had their Network Security Groups and built-in firewalls fully open, and all other resources were deployed with public endpoints accessible from the internet, meaning Private Endpoints were not utilized.

For the "AFTER" metrics, Network Security Groups were tightened by blocking all traffic except from my admin workstation, and all other resources were secured using their built-in firewalls along with Private Endpoints.

## Attack Maps Before Hardening / Security Controls
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

In this project, a mini honeynet was set up in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on the ingested logs. Metrics were measured in the insecure environment before applying security controls and then again after implementing security measures. The results showed a significant reduction in security events and incidents after the security controls were applied, demonstrating their effectiveness.

It is important to note that if the network resources had been heavily utilized by regular users, more security events and alerts might have been generated during the 24-hour period following the implementation of the security controls.
