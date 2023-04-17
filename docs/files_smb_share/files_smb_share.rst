.. _files_smb_share:

----------------------------
Files: Create SMB Share
----------------------------

Overview
++++++++

In this exercise you will create and test a SMB share, used to support home directories, user profiles, and other unstructured file data such as departmental shares commonly accessed by Windows clients.

Lab Setup
+++++++++

This lab requires applications provisioned as part of the :ref:`windows_tools_vm`.

If you have not yet deployed this VM, see the steps in :ref:`windows_tools_vm` to deploy Windows Tools VM before proceeding with the lab.

#. Connect to the Windows Tools VM via RDP or console

#. Download the sample files for File Analytics to the Tools VM:

   - `https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip <https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip>`_

Using SMB Shares
++++++++++++++++

Creating the Share
..................

#. In **Prism Element > File Server**, click on your BootcampFS File Server

#. Click on **Launch Files Console**

   This will open in a new browser tab

   .. figure:: images/13.png

#. Select **Shares** and click on **Create a New Share**

#. Fill out the following fields:

   - **Name** - *Initials*-Marketing  (e.g. XYZ-Marketing)
   - **Description (Optional)** - Departmental share for marketing team
   - **Share Path (Optional)** - Leave blank. This field allows you to specify an existing path in which to create the nested share.
   - **Max Size (Optional)** - Leave blank. This field allows you to set a hard quota for the individual share.
   - **Primary Protocol Access** - SMB

   .. figure:: images/14.png

#. Click **Next**.

#. Select the following options:
  
   - **Enable Self Service Restore**, 
   - **Enable Compression** and
   - **Enable Access Based Enumeration** 

   .. figure:: images/15.png

   As you are creating a departmental share, it should be created as a **Standard** share. This means that all top level directories and files within the share, as well as connections to the share, are served from a single Files VM.

   **Distributed** shares are appropriate for home directories, user profiles, and application folders. This type of share shards top level directories across all Files VMs and load balances connections across all Files VMs within the Files cluster.

   **Access Based Enumeration (ABE)** ensures that only files and folders which a given user has read access are visible to that user. This is commonly enabled for Windows file shares.

   **Self Service Restore** allows users to leverage Windows Previous Version to easily restore individual files to previous revisions based on Nutanix snapshots.

#. Click **Next**.

#. Review the **Summary** and click **Create**.

   .. figure:: images/16.png

Testing the Share
.................

#. Connect to your *Initials*\ **-ToolsVM** via RDP or console using **NTNXLAB\\Administrator** user

   .. note::

     The Tools VM has already been joined to the **NTNXLAB.local** domain. You could use any domain joined VM to complete the following steps.

#. Open ``\\BootcampFS.ntnxlab.local\`` in **File Explorer**.

   .. figure:: images/17.png

#. Test accessing the Marketing share by extracting the SampleData_Small.zip files downloaded in the previous step into the share.

   .. figure:: images/18.png

   - The **NTNXLAB\\Administrator** user was specified as a Files Administrator during deployment of the Files cluster, giving it read/write access to all shares by default.
   - Managing access for other users is no different than any other SMB share.

#. Right-click **Marketing > Properties**.

#. Select the **Security** tab and click **Advanced**.

   .. figure:: images/19.png

#. Select **Users (BootcampFS\\Users)** and click **Remove**.

#. Click **Add**.

#. Click **Select a principal** and specify **Everyone** in the **Object Name** field. Click **OK**.

   .. figure:: images/20.png

#. Fill out the following fields and click **OK**:

   - **Type** - Allow
   - **Applies to** - This folder only
   - Select **Read & execute**
   - Select **List folder contents**
   - Select **Read**
   - Select **Write**

   .. figure:: images/21.png

#. Click **OK > OK > OK** to save the permission changes.

   All users will now be able to create folders and files within the Marketing share.

   It is common for shares utilized by many people to leverage quotas to ensure fair use of resources. Files offers the ability to set either soft or hard quotas on a per share basis for either individual users within Active Directory, or specific Active Directory Security Groups.

Adding Share Level Quota
..........................

#. In **Prism Element > File Server**, click on your BootcampFS File Server

#. Click on **Launch Files Console**

   .. figure:: images/13.png

#. Select **Shares** > **Initials-Marketing** share
 
   .. figure:: images/21-1.png

#. This will open the share details

#. Click on **Quota Policies** > **New Quota Policy**
 
   .. figure:: images/21-3.png

#. Fill out the following fields and click **Save**:

   - Select **User Group**
   - **User or Group** - SSP Developers
   - **Quota** - 10 GiB
   - **Enforcement Type** - Hard Limit

   .. figure:: images/22.png

#. Click **Add**.

#. This will enforce quota limits on the shares for AD user group **SSP Developers** to stay within limit

#. With the *Initials*-Marketing > **Summary** selected, review the **Capacity Summary**, **Performance Summary** and **Share Properties** tabs to understand the available on a per share basis, including the number of files & connections, storage utilization over time, latency, throughput, and IOPS.

   .. figure:: images/23.png
