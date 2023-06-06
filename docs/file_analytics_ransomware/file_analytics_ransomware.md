# File Analytics: Ransomware 

## Ransomware Protection

In this exercise you will enable File Analytics ransomware protection
and trigger a ransomware event.

1.  In Prism Element, go File Server

2.  Select your **FSxyz-prod** file server and click on File Analytics

    ![](images/ransomware_connect.png)

    This will open in a new tab

3.  Go to **Files Analytics** > **Main Menu Bar** > **Ransomware**

    ![](images/ransomware_menu.png)

4.  Select **Enable Ransomware Protection**, put your email address in the **Ramsomware Email Receipts** then click **Enable**

    !!!note
        If someone put his email address in the same box, add your email address at the end and separate the email address by comma.
    !!!

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
