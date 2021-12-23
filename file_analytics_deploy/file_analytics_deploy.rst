.. _file_analytics_deploy:

----------------------
File Analytics: Deploy
----------------------

Overview
++++++++

In this exercise you will walk through deploying the File Analytics VM and scan the existing shares to build out the dashboard.  You will also create anomaly alerts and view the audit details for your file server instance.

.. note:: 

 If you are using HPOC/Single Node POC cluster, Files Analytics VM is already deployed for you based on the staging. 

Deploy File Analytics
+++++++++++++++++++++

#. In **Prism** > **File Server** > click **Deploy File Analytics**

   .. figure:: images/31.png

#. Choose the Files Analytics latest software. 
   
   For the purpose of saving time, the File Analytics 3.0 (or latest) package has already been uploaded to your cluster. Files binaries can be downloaded from the Nutanix Portal and uploaded manually.
   
   .. figure:: images/31-1.png

#. Click **Next**

#. Input the following details:

   - **Name** - *Intials*-FA 
   - **Server Size** - Small
   - **Storage Container** - Self Service Container
   - **Network** - Primary-Managed

#. Select **Show Advanced Settings**

#. Ensure **DNS Resolver IP** is set to your Active Directory, ntnxlab.local, domain controller/DNS IP address and **ONLY** that address.

#. Choose **Deploy**

#. You can monitor the deployment from the **Tasks** page.  The Analytics VM deployment should take ~5 minutes.

#. In **Prism** > **File Server** > click **File Analytics**

   .. figure:: images/33.png

   This will open in a new tab.

#. On the Enable File Analytic, choose the audit data retention period and click on **Enable**
   
   .. figure:: images/34.png


