#  Getting Started

Welcome to the Nutanix Unified Storage APJ TTT workshop 2024.

This lab introduces Nutanix Files, Data Lens, Objects, Volumes and many common management tasks. Each section is a lesson with hands-on practice. The instructor is onsite to answers any additional questions that you may have.

The goal of this workshop is to adequate you with the relevant techniques to conduct similar training to your partners in your own country. 

Traditionally, SAN, NAS and object storage have created silos within the Datacentre. This introduces un-necessary complexity and the ability to scale and the lack of continuous innovation seen in SAN storage. Nutanix believes in eliminating silos in the Enterprise Cloud by approaching file storage as an app, i.e running as software define storage on top of a proven HCI core, Nutanix Files delivers high performance, scalability, and rapid innovation through One Click management.

Please ensure you read the whole lab before starting

**In this lab you will step through managing SMB shares and NFS exports, scale out the environment, and explore upcoming Files features. The lab will provide key considerations around deployment, configuration, and use cases.**

## What's New

- Workshop updated for the following software versions:

  - AOS 6.5.2.5
  - Prism Central pc.2022.6.0.3
  - MSP 2.4.2.1
  - Files 4.3
  - Files Manager 4.3
  - Objects Manager 3.6
  - Objects Service 3.6
  

## Agenda


- Initial NUS Deployment
    - File - Deploy Nutanix Files
    - Object = Deploy Nutanix Objects
    - Files & Objects - RBAC on Files and Objects

- Start Consuming Storage Services
    - Files - Create SMB Share
    - Volumes - Deploy Nutanix Volumes
    - Objects - Buckets & Users Management

- Migrating from Existing File Shares
    - Files - Share Migration

- Data Visibility & Intelligence - The lab will use either Data Lens or File Analytics, the instructor will let you know which lab you should do.
    - Data Visibility & Intelligence (A) - Data Lens 
        - Deploy Data Lens
        - Anomaly Detection
        - Ransomware Protection

    - Data Visibility & Intelligence (B) - File Analytics
        - Enable File Analytics
        - Anomaly Detection
        - Ransomware Protection

- Data Protection & Lifecycle Management
    - Files - Replication
    - Files - Tiering
    - Objects - Replication


- Optional Labs
    - Files - Create NFS Export
    - Files - Expand Files Cluster
    - Files - Enable Files Multi-protocol
    - Objects - Access Objects from CLI and Scripts

The labs are designed to run on any Nutanix AHV cluster provided the following are present:

- AHV IPAM Network
- DHCP Scope
- Gateway
- DNS server (this lab uses a Microsoft DNS configuration)
- IP address allocations for the OCP nodes and other infrastructure elements
- Internet connectivity (no dark site setup information is documented yet)
- A Linux VM (any cloud distribution is ok. CloudInit file is provided in the LinuxToolsVM) section.