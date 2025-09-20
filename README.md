# Cloud-based SOC deployment in Azure

> 🛠️ **This project is a work in progress.** It will continue to evolve as new information is updated.

## Intro
This project used two virtual machines to simulate a network in a controlled environment. One virtual machine ran Windows, and the other ran Linux, both configured to be exposed to the internet. This was achieved by disabling their respective firewalls and configuring the Network Security Groups (NSGs) to allow unfiltered traffic.

Before hardening the system and applying security controls, initial metrics were collected to establish a baseline. These metrics helped identify patterns and types of traffic in the unrestricted environment, providing a clear comparison of the impact of the implemented security measures.

Log collection was enabled from multiple sources on both virtual machines, and the data was sent to a Log Analytics Workspace in Azure. In addition to the events generated on the virtual machines, log capture was configured for key resources such as Microsoft Entra ID, Blob Storage, Key Vault, and the Activity Log in Azure. The types of logs collected include:

        - SecurityEvent: Security event logs from Windows.
        - Syslog: Event logs from Linux.
        - SecurityAlert: Alerts triggered in Log Analytics.
        - SecurityIncident: Incidents created by Microsoft Sentinel.
        - AzureNetworkAnalytics_CL: Malicious flows allowed into the honeynet.

Using this information, the capabilities of Microsoft Sentinel were leveraged to analyze and visualize the collected data. Interactive maps, custom alerts, and simulated security incidents were created, providing a robust platform to practice and develop incident response skills.

## Metrics and attack maps BEFORE hardening

The following table shows the metrics we measured in our insecure environment for 24 hours.

| Metric                                 | Value                         |
|----------------------------------------|-------------------------------|
| Start Time                             | 2025-05-02T18:24:38.0926231Z |
| Stop Time                              | 2025-05-03T18:24:38.0926231Z |
| Security Events (Windows VMs)          | 94,226                        |
| Syslog (Linux VMs)                     | 6,911                         |
| SecurityAlert (Microsoft Defender for Cloud) | 70                    |
| SecurityIncident (Sentinel Incidents)  | 132                           |
| NSG Inbound Malicious Flows Allowed    | 260                           |

![linx-ssh-auth-failure](https://github.com/user-attachments/assets/36c68291-e5c5-46de-96a1-6b34e3a14cf9)
![windows-rdp-auth-fail](https://github.com/user-attachments/assets/6fa729cb-eec3-4c20-b51a-815dd30a1b92)
![nsg-malicious-allowed-in](https://github.com/user-attachments/assets/e8459151-f5f0-49a7-815f-3ddddb452f04)

## Brute Force attempts incident response

For documentation purposes, the following section focuses on a brute force attack targeting the Windows virtual machine. Although similar brute force alerts were also detected on the Linux VM, these will be closed as false positives without further analysis to avoid redundancy. The attack pattern was essentially the same, differing only in the operating system being targeted.


![IR-Azure-1](https://github.com/user-attachments/assets/800154a8-7b39-4fc8-8aed-232f4461460a)

### Summary

![IR-Azure-2](https://github.com/user-attachments/assets/8b9a8559-f241-4db2-849e-d7dc2bdfa0f9)

Incident Report – CUSTOM: Brute Force ATTEMPT - Windows

Incident ID: 72

Date/Time: May 3, 2025 – 11:44:58

Asset: Windows Virtual Machine (Windows-vm)

Source IP: 102.88.21.212

Duration: Approx. 20 minutes

A brute force attack was detected against the Administrator account on the Windows virtual machine. The attack originated from IP address 102.88.21.212 and spanned approximately 20 minutes. All login attempts failed, with no evidence of successful authentication.
Investigation Details
Security Event Logs confirm repeated Event ID 4625 (failed logon attempts) from the source IP.
The following KQL query was used to verify the activity:

     SecurityEvent
     | where EventID == 4624 
     | where IPAddress == "102.88.21.212"

Threat Intelligence Summary:

VirusTotal: 7/94 engines flagged the IP as malicious.

BrightCloud: High Risk.

Cisco Talos: Neutral IP reputation.

AbuseIPDB: Reported 81 times, Confidence of Abuse: 100%.

ScamAnalytics: Fraud Score: 79.

DNSlytics: Listed on DNS blacklist. No domains/mail servers hosted.

Correlations:

![IR-Azure-3](https://github.com/user-attachments/assets/ff01dcdb-cbb5-44ef-a488-83cdb65a3837)
![IR-Azure-4](https://github.com/user-attachments/assets/fc02b71d-75bc-489b-91dd-6a8a9ebc7f19)
![IR-Azure-5](https://github.com/user-attachments/assets/c1429ae9-94a6-413b-a7a0-e5cc55ed8949)

The IP is involved in 42 separate incidents.
The attacker profile is linked to 4 additional alerts across the environment.

### Conclusion and Response
Although the activity was clearly malicious, no compromise occurred. All authentication attempts failed. It is important to note that the firewall and NSG (Network Security Group) were intentionally disabled as part of the honeynet simulation environment, allowing this kind of traffic.

Action Taken:

- This incident has been closed as a false positive in the context of the test environment. However, security controls will be re-enabled.

