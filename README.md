SOC Home Lab – Splunk SIEM Security Monitoring Project
Project Overview

This project demonstrates the creation of a Security Operations Center (SOC) home lab using Splunk Enterprise to simulate real-world security monitoring and log analysis. The lab collects logs from a Windows system, forwards them to a centralized SIEM platform, and visualizes security events through a monitoring dashboard.

The primary objective of this project is to gain practical experience with:

Security Information and Event Management (SIEM)

Log collection and forwarding

Threat detection using Windows event logs

Security monitoring dashboards

Attack simulation and investigation

The environment consists of three virtual machines that simulate a small SOC monitoring infrastructure.

Machine	Role
Ubuntu Server	Splunk Enterprise SIEM
Windows 10	Victim system generating logs
Kali Linux	Attacker machine used for testing

All logs generated from the Windows machine are forwarded to Splunk and stored in the index:

wolfindex

Virtual Network Configuration (VirtualBox NAT Network)

To allow communication between the virtual machines, a NAT Network was created in VirtualBox.

The network was configured with the following address range:

192.168.30.0/24

This allows all machines in the lab environment to communicate with each other while remaining isolated from the host network.

Steps to Create NAT Network in VirtualBox

Open VirtualBox

Navigate to File → Tools → Network Manager

Select NAT Networks

Click Create

Configure the network settings.

Setting	Value
Network Name	SOC-Lab-Network
Network CIDR	192.168.30.0/24
DHCP	Enabled

Attach each virtual machine to this NAT network.

IP Address Configuration

The following static IP addresses were assigned to each system.

System	IP Address
Kali Linux (Attacker)	192.168.30.8
Windows 10 (Victim)	192.168.30.9
Ubuntu Splunk Server	192.168.30.10
Network Communication Flow
Kali Linux (192.168.30.8)
        |
        | Attack simulation (Nmap)
        |
Windows 10 (192.168.30.9)
        |
        | Windows logs + Sysmon logs
        | Splunk Universal Forwarder
        |
Ubuntu Splunk Server (192.168.30.10)
        |
        | Splunk SIEM Analysis
Lab Architecture

The architecture of this SOC lab replicates how enterprise security monitoring systems collect and analyze endpoint telemetry.

                +---------------------+
                |     Kali Linux      |
                |   192.168.30.8      |
                |     (Attacker)      |
                +----------+----------+
                           |
                           | Attack Simulation
                           | Nmap Scanning
                           |
                +----------v----------+
                |      Windows 10     |
                |   192.168.30.9      |
                | Sysmon + Forwarder  |
                +----------+----------+
                           |
                           | Windows Event Logs
                           | Splunk Forwarder
                           |
                +----------v----------+
                |   Ubuntu Server     |
                |   192.168.30.10     |
                | Splunk Enterprise   |
                |       (SIEM)        |
                +---------------------+
Technologies Used
Technology	Purpose
Splunk Enterprise	Security monitoring and log analysis
Splunk Universal Forwarder	Sends logs from Windows to Splunk
Sysmon	Advanced Windows event logging
Ubuntu Server	Host system for Splunk SIEM
Windows 10	Endpoint generating event logs
Kali Linux	Attack simulation platform
Nmap	Network scanning tool
VirtualBox	Virtualization environment
System Requirements

Recommended system resources for running the lab environment.

Resource	Requirement
CPU	Minimum 4 cores
RAM	16 GB recommended
Disk Space	200 GB
Virtualization	Enabled in BIOS

Virtual machines used:

Machine	RAM	CPU	Storage
Ubuntu Splunk Server	8 GB	2 cores	80 GB
Windows Victim	4 GB	2 cores	60 GB
Kali Linux Attacker	4 GB	2 cores	40 GB
Installing Splunk Enterprise

Splunk Enterprise was installed on the Ubuntu server.

Installation command:

sudo dpkg -i splunk*.deb

Start the Splunk service:

sudo /opt/splunk/bin/splunk start

Splunk web interface:

http://192.168.30.10:8000
Configuring Windows Log Forwarding

The Splunk Universal Forwarder was installed on the Windows machine to send logs to the Splunk server.

Logs are forwarded to the Splunk server using port:

9997

Configuration file:

inputs.conf

[WinEventLog://Application]
disabled = 0
index = wolfindex

[WinEventLog://Security]
disabled = 0
index = wolfindex

[WinEventLog://System]
disabled = 0
index = wolfindex

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = wolfindex
Verifying Log Ingestion

To verify logs were successfully received by Splunk:

index=wolfindex

Other useful searches:

Windows security logs

index=wolfindex sourcetype="WinEventLog:Security"

Application logs

index=wolfindex sourcetype="WinEventLog:Application"

Sysmon logs

index=wolfindex sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
Creating the SOC Dashboard

The monitoring dashboard was created using Splunk Dashboard Studio.

The dashboard visualizes security activity such as:

Authentication events

Process execution

Network connections

Application events

Source IP activity

Dashboard Panels
Failed Login Attempts

Detects brute-force login attempts.

Query:

index=wolfindex EventCode=4625
| stats count by Account_Name

Visualization: Bar chart

Successful Logins

Query:

index=wolfindex EventCode=4624
| stats count by Account_Name

Visualization: Pie chart

Process Creation Monitoring

Query:

index=wolfindex EventCode=1
| stats count by Image
| sort -count

Visualization: Column chart

Network Activity Monitoring

Query:

index=wolfindex EventCode=3
| stats count by SourceIp DestinationIp DestinationPort

Visualization: Table

Top Source IP Activity

Query:

index=wolfindex
| stats count by SourceIp
| sort -count

Visualization: Bar chart

Application Event Monitoring

Query:

index=wolfindex sourcetype="WinEventLog:Application"
| stats count by SourceName
| sort -count

Visualization: Bar chart

Attack Simulation

To generate security events, attacks were simulated using the Kali Linux machine.

Example Nmap scan:

nmap -sS 192.168.30.9

This scan generates network connection logs which are captured by Sysmon and forwarded to Splunk.

Security Events Detected
Event Type	Detection
Brute force attempts	Failed login events
Authentication activity	Successful login events
Port scanning	Network connection logs
Suspicious processes	Process creation logs
Application errors	Application log monitoring
Errors Encountered During Lab Setup

During the setup process several configuration issues were encountered and resolved.

PowerShell “Unexpected Token” Error

When running Splunk CLI commands:

"C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" list forward-server

PowerShell returned a syntax error due to spaces in the file path.

Solution:

& "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" list forward-server
Forwarder Showing “Configured but Inactive”

The forward server appeared as configured but inactive.

Cause:

Receiving port was not enabled on the Splunk server.

Solution:

Enable port 9997 under:

Settings → Forwarding and Receiving → Configure Receiving

Missing inputs.conf File

The inputs.conf file did not initially exist.

Location:

C:\Program Files\SplunkUniversalForwarder\etc\system\local

It had to be created manually to specify which logs should be collected.

Logs Not Appearing in Splunk

The dashboard initially displayed zero events.

Cause:

Incorrect search query and time range.

Correct search:

index=wolfindex
Incorrect Search Query Syntax

Incorrect query:

source="WinEventLog:*" index="wolfindex"

Correct query:

index=wolfindex sourcetype="WinEventLog:Security"
Network Connectivity Issues

Connectivity between systems must be verified.

Command used:

Test-NetConnection 192.168.30.10 -Port 9997

Ensuring port 9997 is open resolves this issue.

Skills Demonstrated

This project demonstrates several key SOC analyst skills:

SIEM deployment

Windows log collection

Sysmon monitoring

Security event analysis

Threat detection using Splunk queries

Dashboard development

Attack simulation

Troubleshooting SIEM environments

Future Improvements

Potential improvements include:

Implementing automated alerts

MITRE ATT&CK detection mapping

PowerShell attack detection

Additional endpoint log sources

Advanced threat hunting queries

Conclusion

This SOC home lab demonstrates how Splunk SIEM can be used to monitor endpoint activity and detect potential security threats.

By collecting Windows event logs, analyzing network behavior, and visualizing security events through dashboards, this project simulates real-world SOC monitoring workflows.
