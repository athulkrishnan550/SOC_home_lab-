# SOC Home Lab – Splunk SIEM Security Monitoring
## Project Overview

This project demonstrates the implementation of a Security Operations Center (SOC) home lab using Splunk Enterprise to simulate real-world security monitoring and log analysis.

The goal of this lab is to collect Windows event logs, forward them to a Splunk SIEM server, and visualize security events through a monitoring dashboard.

This project demonstrates practical experience with:

* Security Information and Event Management (SIEM)
* Windows log collection and forwarding
* Threat detection using Splunk queries
* Security monitoring dashboards
* Attack simulation and investigation
* Troubleshooting SIEM deployment issues

## Lab Environment

The SOC lab environment consists of three virtual machines running inside VirtualBox.

Machine          |      Role
Ubuntu Server	 |      Splunk Enterprise SIEM
Windows 10	 |      Victim machine generating logs
Kali Linux	 |      Attacker machine used for testing
        
All logs generated from the Windows machine are sent to Splunk and stored in the index:

wolfindex

## Virtual Network Configuration

A NAT Network was created in VirtualBox to allow communication between all virtual machines.

Network Range
192.168.30.0/24

This allows all machines to communicate internally within the lab.

Steps to Create NAT Network

* Open VirtualBox
* Navigate to File → Tools → Network Manager
* Select NAT Networks
* Click Create
* Configure the following settings

        | Setting      | Value           |
        | ------------ | --------------- |
        | Network Name | SOC-Lab-Network |
        | Network CIDR | 192.168.30.0/24 |
        | DHCP         | Enabled         |

Then attach each virtual machine to this NAT network.

## IP Address Configuration
Static IP address is asssigned to each of the systems 

        | Machine              | IP Address    |
        | -------------------- | ------------- |
        | Kali Linux           | 192.168.30.8  |
        | Windows 10           | 192.168.30.9  |
        | Ubuntu Splunk Server | 192.168.30.10 |

## Network Architecture

        Kali Linux (192.168.30.8)
      |
      | Attack simulation
      | Nmap scan
      |
        Windows 10 (192.168.30.9)
      |
      | Windows Event Logs
      | Sysmon Logs
      | Splunk Universal Forwarder
      |
        Ubuntu Server (192.168.30.10)
      |
      | Splunk Enterprise SIEM
      | Security Monitoring Dashboard


## Technologies Used

* Splunk Enterprise – SIEM platform for monitoring security events
* Splunk Universal Forwarder – Sends logs from Windows to Splunk
* Sysmon – Advanced Windows system monitoring
* Ubuntu Server – Host system for Splunk SIEM
* Windows 10 – Endpoint generating security logs
* Kali Linux – Attack simulation environment
* Nmap – Network scanning tool
* VirtualBox – Virtualization platform


## System Requirement

        | Resource       | Requirement       |
        | -------------- | ----------------- |
        | CPU            | Minimum 4 cores   |
        | RAM            | 16 GB recommended |
        | Storage        | 200 GB            |
        | Virtualization | Enabled           |

virtual machines used

        | Machine              | RAM  | CPU     | Storage |
        | -------------------- | ---- | ------- | ------- |
        | Ubuntu Splunk Server | 8 GB | 2 cores | 80 GB   |
        | Windows Victim       | 4 GB | 2 cores | 60 GB   |
        | Kali Linux Attacker  | 4 GB | 2 cores | 40 GB   |


## Installing Splunk Entriprise 
Splunk entriprise was installed on the ubuntu server.

Install Splunk 

* After download the SPLUNK run the command
  
          sudo dpkg -i splunk*.deb
* Start Splunk
  
          sudo /opt/splunk/bin/splunk start
* Access splunk web server
  
          http://192.168.30.10:8000


## Configuring Windows Log Forwarding 
The Splunk Universal Forwarder was installed on the Windows machine to send logs to the Splunk server.

Forwarding port used:

9997

inputs.conf Configuration

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

Logs collected include:

* Windows Security logs
* Windows System logs
* Windows Application logs
* Sysmon logs

# Verifying Log Ingestion 

To verify the logs are arriving in the Splunk 

        index=wolfindex

Security logs - 

        index=wolfindex sourcetype="WinEventLog:Security"

Application logs - 

        index=wolfindex sourcetype="WinEventLog:Application"

Sysmon logs - 

        index=wolfindex sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"


## SOC Monitoring Dashboard 

A monitoring dashboard was created using Splunk Dashboard Studio.

The dashboard provides visualization of security activity including:

* Authentication activity
* Process execution
* Network connections
* Application events
* Source IP activity

## Dashboard Panels Creation 

Failed Login Attempts
Detects possible brute-force attacks.
Used visualization barchart 

        index=wolfindex EventCode=4625
        | stats count by Account_Name

Successful Logins 

        index=wolfindex EventCode=4624
        | stats count by Account_Name

Process Creation Monitoring

        index=wolfindex EventCode=1
        | stats count by Image
        | sort -count

Network Activity Monitoring

        index=wolfindex EventCode=3
        | stats count by SourceIp DestinationIp DestinationPort

Application Event Monitoring

        index=wolfindex sourcetype="WinEventLog:Application"
        | stats count by SourceName
        | sort -count


## Attack Simulation 

Security events were generated by performing attacks from the Kali Linux machine.
Example Nmap Scan

        nmap -sS 192.168.30.9
This generates network connection logs which are captured by Sysmon and forwarded to Splunk.

## Security Events Detected 

        | Event Type                 | Detection                   |
        | -------------------------- | --------------------------- |
        | Brute force login attempts | Failed login events         |
        | User authentication        | Successful login monitoring |
        | Port scanning              | Network activity monitoring |
        | Malicious processes        | Process creation monitoring |
        | Application failures       | Application log monitoring  |



## Errors Encountered During Lab Setup 
During the lab setup several issues occurred and were resolved.

These troubleshooting steps helped improve understanding of Splunk forwarding and SIEM configuration.

PowerShell Unexpected Token Error

        When running Splunk CLI commands:
           "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" list forward-server

Solution:
use the powershell call operator 

        & "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" list forward-server

## Forwarder Showing “Configured but Inactive”

The forward server appeared as configured but inactive.

Cause

Receiving port was not enabled on the Splunk server.

Solution

Enable port 9997

Steps:

* Go to Settings
* Select Forwarding and Receiving
* Click Configure Receiving
* Add port 9997

# Missing inputs.conf File 
This file is did not exist initially 
Location:

        C:\Program Files\SplunkUniversalForwarder\etc\system\local
The file had to be created manually to define log sources.

Logs Not Appearing in Splunk

The dashboard initially displayed no events.

Cause

* Incorrect search query
* Incorrect time range

Correct search

        index=wolfindex

Incorrect Search Query Syntax

Incorrect query used:

        source="WinEventLog:*" index="wolfindex"

Correct query:

        index=wolfindex sourcetype="WinEventLog:Security"

## Network Connectivity Issues

Logs cannot be forwarded if the Splunk server is unreachable.

Connectivity test command:

        Test-NetConnection 192.168.30.10 -Port 9997     

## Skills Demonstrated

This project demonstrates several cybersecurity skills.

* SIEM deployment
* Windows log monitoring
* Sysmon configuration
* Threat detection using Splunk
* SOC dashboard creation
* Network attack simulation
* Troubleshooting SIEM environments

## Conclusion

This SOC home lab demonstrates how Splunk SIEM can be used to monitor endpoint activity and detect security threats.

By collecting Windows event logs, analyzing network activity, and visualizing security data through dashboards, this project simulates real-world SOC monitoring workflows.

The lab provides valuable hands-on experience with SIEM platforms, security monitoring, and threat detection techniques used by SOC analysts.
