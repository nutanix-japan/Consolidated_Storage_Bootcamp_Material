# Files: Create SMB Share

## Overview

In this exercise you will create and test a SMB share, used to support
home directories, user profiles, and other unstructured file data such
as departmental shares commonly accessed by Windows clients.

## Lab Setup

This lab requires applications provisioned as part of the [Windows Tools](../tools_vms/windows_tools_vm.md)

If you have not got this deployed, see the steps in [Windows Tools](../tools_vms/windows_tools_vm.md) to deploy Windows Tools VM before proceeding with the lab.

1.  Connect to the Windows Tools VM via RDP or console
2.  Download the [sample files](<https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip>) for File Analytics to the Tools VM

## Using SMB Shares

### Creating the Share

1.  In **Prism Element > File Server**, click on your BootcampFS File Server

2.  Click on **Launch Files Console** 
    
    This will open in a new browser tab

    ![](images/13.png)

3.  Select **Shares** and click on **Create a New Share**

4.  Fill out the following fields:

    -   **Name** - *Initials*-Marketing (e.g. XYZ-Marketing)
    -   **Description (Optional)** - Departmental share for marketing
        team
    -   **Share Path (Optional)** - Leave blank. This field allows you
        to specify an existing path in which to create the nested share.
    -   **Max Size (Optional)** - Leave blank. This field allows you to
        set a hard quota for the individual share.
    -   **Primary Protocol Access** - SMB

    ![](images/14.png)

5.  Click **Next**.

6.  Select the following options:

    -   **Enable Self Service Restore**,
    -   **Enable Compression** and
    -   **Enable Access Based Enumeration**

    ![](images/15.png)

    As you are creating a departmental share, it should be created as a
    **Standard** share. This means that all top level directories and
    files within the share, as well as connections to the share, are
    served from a single Files VM.

    **Distributed** shares are appropriate for home directories, user
    profiles, and application folders. This type of share shards top
    level directories across all Files VMs and load balances connections
    across all Files VMs within the Files cluster.

    **Access Based Enumeration (ABE)** ensures that only files and
    folders which a given user has read access are visible to that user.
    This is commonly enabled for Windows file shares.

    **Self Service Restore** allows users to leverage Windows Previous
    Version to easily restore individual files to previous revisions
    based on Nutanix snapshots.

7.  Click **Next**.

8.  Review the **Summary** and click **Create**.

    ![](images/16.png)

### Testing the Share

1.  Connect to your *Initials***-ToolsVM** via RDP or console using
    **NTNXLAB\\Administrator** user

    !!!note

           The Tools VM has already been joined to the **NTNXLAB.local**
           domain. You could use any domain joined VM to complete the following
           steps.

2.  Open `\\BootcampFS.ntnxlab.local\` in **File Explorer**.

    ![](images/17.png)

3.  Test accessing the Marketing share by extracting the
    SampleData_Small.zip files downloaded in the previous step into the
    share.

    ![](images/18.png)

    -   The **NTNXLAB\\Administrator** user was specified as a Files
        Administrator during deployment of the Files cluster, giving it
        read/write access to all shares by default.
    -   Managing access for other users is no different than any other
        SMB share.

4.  Right-click **Marketing \> Properties**.

5.  Select the **Security** tab and click **Advanced**.

    ![](images/19.png)

6.  Select **Users (BootcampFS\\Users)** and click **Remove**.

7.  Click **Add**.

8.  Click **Select a principal** and specify **Everyone** in the
    **Object Name** field. Click **OK**.

    ![](images/20.png)

9.  Fill out the following fields and click **OK**:

    -   **Type** - Allow
    -   **Applies to** - This folder only
    -   Select **Read & execute**
    -   Select **List folder contents**
    -   Select **Read**
    -   Select **Write**

    ![](images/21.png)

10. Click **OK \> OK \> OK** to save the permission changes.

    All users will now be able to create folders and files within the
    Marketing share.

    It is common for shares utilized by many people to leverage quotas
    to ensure fair use of resources. Files offers the ability to set
    either soft or hard quotas on a per share basis for either
    individual users within Active Directory, or specific Active
    Directory Security Groups.

### Adding Share Level Quota

1.  In **Prism Element \> File Server**, click on your BootcampFS File
    Server

2.  Click on **Launch Files Console**

    ![](images/13.png)

3.  Select **Shares** \> **Initials-Marketing** share

    ![](images/21-1.png)

4.  This will open the share details

5.  Click on **Quota Policies** \> **New Quota Policy**

    ![](images/21-3.png)

6.  Fill out the following fields and click **Save**:

    -   Select **User Group**
    -   **User or Group** - SSP Developers
    -   **Quota** - 10 GiB
    -   **Enforcement Type** - Hard Limit

    ![](images/22.png)

7.  Click **Add**.

8.  This will enforce quota limits on the shares for AD user group **SSP
    Developers** to stay within limit

9.  With the *Initials*-Marketing \> **Summary** selected, review the
    **Capacity Summary**, **Performance Summary** and **Share
    Properties** tabs to understand the available on a per share basis,
    including the number of files & connections, storage utilization
    over time, latency, throughput, and IOPS.

    ![](images/23.png)
