#  Getting Started

Welcome to the Nutanix Consolidated Storage Bootcamp for CX offsite 2023.

This workbook introduces Nutanix Files, Data Lens, Objects, Volumes and many common management tasks. Each section has a lesson to give you hands-on practice. The instructor is onsite to answers any additional questions that you may have.

Traditionally, SAN, NAS and object storage have created silos within IT, introducing un-necessary complexity and suffering from the same issues of scale and lack of continuous innovation seen in SAN storage. Nutanix believes there is no room for silos in the Enterprise Cloud. By approaching file storage as an app, running in software on top of a proven HCI core, Nutanix Files delivers high performance, scalability, and rapid innovation through One Click management.

Please ensure you read the whole lab before starting

**In this lab you will step through managing SMB shares and NFS exports, scale out the environment, and explore upcoming Files features. The lab will provide key considerations around deployment, configuration, and use cases.**

## What's New

- Workshop updated for the following software versions:

  - AOS 5.20.3
  - Prism Central pc.2021.9.0.4
  - Files 4.2.1.1
  - Objects Manager 3.4.0.2
  - Objects Service 3.4.0.2
  

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

- Data Lens - Data Security Management
    - Deploy Data Lens
    - Anomaly Detection
    - Ransomware Protection

- Data Protection & Lifecycle Management
    - Files - Replication
    - Files - Tiering


- Optional Labs
    - Files - Create NFS Export
    - Files - Expand Files Cluster
    - Files - Enable Files Multi-protocol
    - Objects - Access Objects from CLI and Scripts

## Initial Setup

-   Take note of the *Passwords* being used in this lab
    -   Prism Element, Prism Central, CVMs, PCVM password will be provided by instructor.
    -   FSVM, all AD users share the same password of **nutanix/4u**
-   Login to Global Protect VPN, select gateway without (ST). This is required to access Data Lens lab
-   Log into your virtual desktops if required (connection info below)

## Cluster Assignment

The instructor will inform the attendees their assigned clusters.

!!!note

        If these are Single Node Clusters (SNCs) pay close attention on the
        networking part. The SNCs are setup and configured differently
        to the 3 or 4 node clusters

## Environment Details

Nutanix Workshops are intended to be run in the Nutanix Hosted POC
environment. Your cluster will be provisioned with all necessary images,
networks, and VMs required to complete the exercises.

### Networking

As we are able to provide three/four node clusters and single node
clusters in the HPOC environment, we need to describe each sort of
cluster separately. The clusters are setup and configured differently.

#### Three/Four node HPOC clusters

Three or four node Hosted POC clusters follow a standard naming
convention:

-   **Cluster Name** - POC*XYZ*
-   **Subnet** - 10.**42**.*XYZ*.0
-   **Cluster IP** - 10.**42**.*XYZ*.37

For example:

-   **Cluster Name** - POC055
-   **Subnet** - 10.42.55.0
-   **Cluster IP** - 10.42.55.37 for the VIP of the Cluster

Throughout the Workshop there are multiple instances where you will need
to substitute *XYZ* with the correct octet for your subnet, for example:

| IP Address     |   Description |
| -------------- | --------------- |
| 10.42.*XYZ*.37 |  Nutanix Cluster Virtual IP   |
| 10.42.*XYZ*.39 |  **PC** VM IP, Prism Central |
| 10.42.*XYZ*.41  |  **DC** VM IP, NTNXLAB.local Domain Controller   |


Each cluster is configured with 2 VLANs which can be used for VMs:


|Network Name     | Address             | VLAN    | DHCP Scope|
|-----------------| ------------------- |-------- | -----------|
|Primary          | 10.42.*XYZ*.1/25    | 0       | 10.42.*XYZ*.50-10.42.*XYZ*.124|
|Secondary        | 10.42.*XYZ*.129/25  | *XYZ1*  | 10.42.*XYZ*.132-10.42.*XYZ*.253|


#### Single Node HPOC Clusters (SPOC)

For some workshops we are using Single Node Clusters (SPOC). The reason
for this is to allow more people to have a dedicated cluster but still
have enough free clusters for the bigger workshops including those for
customers.

The network in the SPOC config is using a /26 network. This splits the
network address into four equal sizes that can be used for workshops.
The below table describes the setup of the network in the four
partitions. It provides essential information for the workshop with
respect to the IP addresses and the services running at that IP address.

Naming convention: 

- **Cluster Name** - PHX-SPOC*XYZ*-*A*
  - *XYZ* is the SPOC number
  - *A* is the partition number
- **Subnet** - 10.**38**.*XYZ*.0
- **Cluster IP** - 10.**42**.*XYZ*.37

|Partition 1     |Partition 2      | Partition 3      | Partition 4    | Service     |  Comment
|--------------- |---------------  |----------------- |----------------- |------------- |---------------
|10.38.x.1       |10.38.x.65        |10.38.x.129       |10.38.x.193      |Gateway           |     
|10.38.x.5       |10.38.x.69        |10.38.x.133       |10.38.x.197      |AHV Host          |   
|10.38.x.6       |10.38.x.70        |10.38.x.134       |10.38.x.198      |CVM IP            |
|10.38.x.7       |10.38.x.71        |10.38.x.135       |10.38.x.199      | Cluster IP       |
|10.38.x.8       |10.38.x.72        |10.38.x.136       |10.38.x.200      | Data Services IP |           
|10.38.x.9       |10.38.x.73        |10.38.x.137       |10.38.x.201      | Prism Central IP |           
|10.38.x.11      |10.38.x.75        |10.38.x.139       |10.38.x.203      | AutoDC IP(DC)    |
|10.38.x.32-37   |10.38.x.96-101    |10.38.x.160-165   |10.38.x.224-229  | Objects 1        | 
|10.38.x.38-58   |10.38.x.102-122   |10.38.x.166-186   |10.38.x.230-250  | Primary          | 6 Free IPs for static assignment|
|10.38.x.0       |10.38.x.64        |10.38.x.128       |10.38.x.192      | Subnet           | 


### Credentials

!!!note

        The *Cluster Password* is unique to each cluster and will be
        provided by the leader of the Workshop.

| Credential        | Username                 | Password           |
|------------------ |------------------------- |--------------------|
| Prism Element     | admin                    | *Cluster Password* |
| Prism Central     | admin                    | *Cluster Password* |
| Controller VM     | nutanix                  | *Cluster Password* |
| Prism Central VM  | nutanix                  | *Cluster Password* |

Each cluster has a dedicated domain controller VM, **DC**, responsible
for providing AD services for the **NTNXLAB.local** domain. The domain
is populated with the following Users and Groups:


| Group            | Username(s)              | Password |
|-----------------| ------------------------- |------------|
| Administrators    | Administrator             | nutanix/4u | 
| SSP Admins        | adminuser01-adminuser25   | nutanix/4u | 
| SSP Developers    | devuser01-devuser25       | nutanix/4u | 
| SSP Consumers     | consumer01-consumer25     | nutanix/4u |
| SSP Operators     | operator01-operator25     | nutanix/4u |
| SSP Custom        | custom01-custom25         | nutanix/4u |
| Bootcamp Users    | user01-user25             | nutanix/4u |


## Access Instructions

The Nutanix Hosted POC environment can be accessed a number of different
ways:

### Lab Access User Credentials

PHX Based Clusters: 

- **Username:** PHX-POCxxx-User01 (up to PHX-POCxxx-User20), 
- **Password:** *Provided by Instructor*

RTP Based Clusters: 

- **Username:** RTP-POCxxx-User01 (up to RTP-POCxxx-User20), 
- **Password:** *Provided by Instructor*

### Frame VDI

Login to: <https://console.nutanix.com/x/labs>

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

### Parallels VDI

PHX Based Clusters Login to: <https://xld-uswest1.nutanix.com>

RTP Based Clusters Login to: <https://xld-useast1.nutanix.com>

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

### Employee Pulse Secure VPN

Download the client:

PHX Based Clusters Login to: <https://xld-uswest1.nutanix.com>

RTP Based Clusters Login to: <https://xld-useast1.nutanix.com>

**Nutanix Employees** - Use your **NUTANIXDC** credentials
**Non-Employees** - Use **Lab Access User** Credentials

Install the client.

In Pulse Secure Client, **Add** a connection:

For PHX:

-   **Type** - Policy Secure (UAC) or Connection Server
-   **Name** - X-Labs - PHX
-   **Server URL** - xlv-uswest1.nutanix.com

For RTP:

-   **Type** - Policy Secure (UAC) or Connection Server
-   **Name** - X-Labs - RTP
-   **Server URL** - xlv-useast1.nutanix.com

## Nutanix Version Info

-   AOS 5.20.3.x & 
-   PC pc.2022.4.0.1
-   Calm 3.3.1
