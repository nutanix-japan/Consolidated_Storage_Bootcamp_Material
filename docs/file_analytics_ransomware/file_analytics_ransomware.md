# File Analytics: Ransomware {#file_analytics_ransomware}

## Ransomware Protection

In this exercise you will enable File Analytics ransomware protection
and trigger a ransomware event.

1.  In Prism Element, go File Server

2.  Select your BootcampFS file server and click on File Analytics

    ![](images/ransomware_connect.png)

    This will open in a new tab

3.  Go to **Files Analytics** \> **Main Menu Bar** \> **Ransomware**

    ![](images/ransomware_menu.png)

4.  Select **Enable Ransomware Protection** then click **Enable**

    ![](images/ransomware_enable.png)

5.  In your Windows Tools VM, open a PowerShell window by clicking on
    the **PowerShell icon** on the taskbar. Enter the following command
    where you will see an access denied error message:

    ``` PowerShell
    new-item \\BootcampFS.ntnxlab.local\xyz-marketing\ransomware.aaa -ItemType file
    ```

6.  In **File Analytics** review the **Vulnerabilities** table to see
    the blocked operation. Note you may need to refresh the
    **Ransomware** page.

    ![](images/ransomware_attempts.png)

Note: You can view the permission denied event in the **Audit Trails**
as well.
