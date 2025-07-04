# Cloud-based SOC deployment in Azure

> 🛠️ **This project is a work in progress.** It will continue to evolve as new information is updated.

This project used two virtual machines to simulate a network in a controlled environment. One virtual machine ran Windows, and the other ran Linux, both configured to be exposed to the internet. This was achieved by disabling their respective firewalls and configuring the Network Security Groups (NSGs) to allow unfiltered traffic.

Before hardening the system and applying security controls, initial metrics were collected to establish a baseline. These metrics helped identify patterns and types of traffic in the unrestricted environment, providing a clear comparison of the impact of the implemented security measures.

Log collection was enabled from multiple sources on both virtual machines, and the data was sent to a Log Analytics Workspace in Azure. In addition to the events generated on the virtual machines, log capture was configured for key resources such as Microsoft Entra ID, Blob Storage, Key Vault, and the Activity Log in Azure. The types of logs collected include:

        - SecurityEvent: Security event logs from Windows.
        - Syslog: Event logs from Linux.
        - SecurityAlert: Alerts triggered in Log Analytics.
        - SecurityIncident: Incidents created by Microsoft Sentinel.
        - AzureNetworkAnalytics_CL: Malicious flows allowed into the honeynet.

Using this information, the capabilities of Microsoft Sentinel were leveraged to analyze and visualize the collected data. Interactive maps, custom alerts, and simulated security incidents were created, providing a robust platform to practice and develop incident response skills.
