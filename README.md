# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/smithd2983/Cloud-SOC/assets/155127022/4af91c02-d2e4-4ccf-8c5e-b26ab6e8b4ff)



## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/smithd2983/Cloud-SOC/assets/155127022/430e58c6-651c-4176-a9ca-ed554bf9d7fa)



## Architecture After Hardening / Security Controls
![PUBLIC INTERNET](https://github.com/smithd2983/Cloud-SOC/assets/155127022/8d820d9a-60f2-43b5-a08a-b9af593b705e)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![(Before) NSG- Malicious-Allowed-in](https://github.com/smithd2983/Cloud-SOC/assets/155127022/81ebb0ed-ec4c-43f4-b09a-25c08909d0f0)
![(Before) Syslog-SSH-Auth-Fail](https://github.com/smithd2983/Cloud-SOC/assets/155127022/08fbc4c4-d5a2-4eb7-b618-02f3c4d31248)
![(Before) Windows-RDP-Auth-Fail](https://github.com/smithd2983/Cloud-SOC/assets/155127022/9504e8fd-b1bf-49dc-8374-a09ad9be9691)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-11-11T00:41:13
Stop Time  2023-11-14T00:41:13

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 33414
| Syslog                   | 4436
| SecurityAlert            | 13
| SecurityIncident         | 184
| AzureNetworkAnalytics_CL | 3346

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-11-14T03:09:17
Stop Time	2023-11-16T03:09:17

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 1206
| Syslog                   | 4
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
