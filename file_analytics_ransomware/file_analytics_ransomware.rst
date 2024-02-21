.. _file_analytics_ransomware:

--------------------------
File Analytics: Ransomware
--------------------------

Ransomware Protection
+++++++++++++++++++++++

In this exercise you will enable File Analytics ransomware protection and trigger a ransomware event.

#. In Prism Element, go File Server

#. Select your BootcampFS file server and click on File Analytics 

   .. figure:: images/ransomware_connect.png

   This will open in a new tab

#. Go to **Files Analytics** > **Main Menu Bar** > **Ransomware**

   .. figure:: images/ransomware_menu.png

#. Select **Enable Ransomware Protection** then click **Enable**

   .. figure:: images/ransomware_enable.png

#. In your Windows Tools VM, open a PowerShell window by clicking on the **PowerShell icon** on the taskbar. Enter the following command where you will see an access denied error message:

   .. code-block:: PowerShell

	   new-item \\BootcampFS.ntnxlab.local\xyz-marketing\ransomware.aaa -ItemType file

#. In **File Analytics** review the **Vulnerabilities** table to see the blocked operation.  Note you may need to refresh the **Ransomware** page. 

   .. figure:: images/ransomware_attempts.png

Note: You can view the permission denied event in the **Audit Trails** as well.
