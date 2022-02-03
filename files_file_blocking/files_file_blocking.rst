.. _files_file_blocking:

------------------------
Files: File Blocking
------------------------

Selective File Blocking
+++++++++++++++++++++++

In this exercise you will configure Files to block specific file extensions for the file server and the Marketing share.

#. In **Prism Element > File Server**, select your file server and click **Launch Files Console**.

#. The **Files Console** will open in a new browser tab

#. Click on **Configuration > Authentication**

#. Under **Blocked File Types** enter a comma separated list of extensions like .flv,.mov and click **Save**

   .. figure:: images/48.png

#. In your Windows Tools VM, open a PowerShell window by clicking on the **PowerShell icon** on the taskbar. Enter the following command where you will see an access denied error message:

   .. code-block:: PowerShell

	   new-item \\BootcampFS.ntnxlab.local\xyz-marketing\MyMovie.flv -ItemType file

   .. figure:: images/49.png

#. In **Prism Element > File Server**, click on your BootcampFS File Server

#. Click on **Launch Files Console**

#. Click on three dots on the right hand corner of the *Initials*-Marketing share and select **Edit**

   .. figure:: images/50.png

#. Select **Next** to get to the **Settings** page.

#. Check **Blocked File Types** and enter .none as a file extension.

   .. figure:: images/51.png

#. Select **Next** then **Update** on the **Summary** page to complete the update.

#. Blocked file type settings at the share level override the server level setting.  Using PowerShell issue the same command as the previous step.  The command will now complete successfully.

   .. figure:: images/52.png
