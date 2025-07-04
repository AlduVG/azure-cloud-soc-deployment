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

