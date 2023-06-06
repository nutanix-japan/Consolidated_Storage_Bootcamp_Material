# Data Lens - Ransomware Protection

## Overview
One of the core capability of Data Lens is the enhanced ransomware protection feature. Data Lens maintains a list of blocked signatures of file name or extension patterns of known ransomware variants and automatically applies the list to
your file servers. An attack is prevented by locking the files
when file extensions matches in the blocked signature list.

Furthermore, the activity is bubbled into ransomware vulnerability dashboard reporting the client, user and the files impacted. In case of an attack, Data Lens performs remediation by blocking clients that trigger the attack or set the File Server to read-only mode. It will also track the infected files and recommend the best snapshot to recover the data.

## Lab Setup

In this lab, we will simulate a ransomware attack and verify how remediation works in Data Lens. 

## Login to Data Lens (if you are not already there)

1. Connect to corp VPN, select the gateway without **(ST)**
   
2. Go to https://datalens-qa.nutanix.com/ 
   
3. Your instructor will give you a my portal account to login
   
4. Choose **Common Tenant** and then **Proceed**.
   
    ![](images/dl1.png)

5. In the **Data Lens Global Dashboard**, go to **File Servers** and search the FQDN of your File Server Name (**FS*xyz*-*a*-prod**).

    ![](images/dl2.png)

    !!!note 
           Your File Server is already added and enabled to the Data Lens Dashboard. Contact lab instructor if you cannot find it.


6. Click the File Server Name to enter the Dashboard.

## Enable Ransomware Protection in Data Lens

1. Click on :fontawesome-solid-bars: > **Ransonware Protection**

2. If **Ransomware Protection** is not enabled, click **Enable Ransomware Protection**
   
   ![](images/dl3.png)

3. If **Ransomware Protection** is enabled, go to **Settings** > **Edit Policy Configuration**

4.  From **Detect and Act on Ransomware Threats**, select **Make File Server Read-Only**. Put your email address in **Email Recipients**, then **Enable**.
      
    ![](images/dl4.png)

## Ransomware Protection Simulation

1.    Go to **Settings** > **Update Signature List**, search for ****.satana*,*.AngryDuck** in the search box. If it is not in the signature list, click **Add** to add it, otherwise just click **Close**.
            ![](images/dl5.png)

2.    Login to **WinToolsVM** using username : **administrator@ntnxlab.local**

3.    Check the following 2 shares in **File Explorer**:

      - ``FS*XYZ-A*-prod.ntnxlab.local\DLtest-prod``
      - ``FS*XYZ-A*-dr.ntnxlab.local\DLtest-dr``

4.    Copy all the files from **DLtest-prod** to **DLtest-dr**. We will use these TWO shares to compare the result with and without Data Lens ransomware protection.

5.    Create a new file **ransomware_test.ps1** in Downloads, open the file and put the following content in the file. **Save** it.
    
    === "Template commands"
    
        ```bash
        new-item \\fsXYZ-a-prod.ntnxlab.local\DLtest-prod\IamAngry.AngryDuck -ItemType file
        Get-ChildItem \\fsXYZ-a-prod.ntnxlab.local\DLtest-prod\*.txt | Rename-Item -NewName { $_.Name -replace '.txt','.satana' }
        new-item \\fsXYZ-a-dr.ntnxlab.local\DLtest-dr\IamAngry.AngryDuck -ItemType file
        Get-ChildItem \\fsXYZ-a-dr.ntnxlab.local\DLtest-dr\*.txt | Rename-Item -NewName { $_.Name -replace '.txt','.satana' }
        ```
    
    === "Sample commands"
    
        ``` bash
        new-item \\fs002-3-prod.ntnxlab.local\DLtest-prod\IamAngry.AngryDuck -ItemType file
        Get-ChildItem \\fs002-3-prod.ntnxlab.local\DLtest-prod\*.txt | Rename-Item -NewName { $_.Name -replace '.txt','.satana' }
        new-item \\fs002-3-dr.ntnxlab.local\DLtest-dr\IamAngry.AngryDuck -ItemType file
        Get-ChildItem \\fs002-3-dr.ntnxlab.local\DLtest-dr\*.txt | Rename-Item -NewName { $_.Name -replace '.txt','.satana' }
        ```

6. Right click the :simple-powershell: (PowerShell icon) and select **Run as Administrator**.

7. Run the script, which has some file creation and changing file type to both shares.

    ```bash
    C:\Users\Administrator\Downloads\ransomware_test.ps1
    ```

8.   Check the share ``FSxyz-a-prod.ntnxlab.local\DLtest-prod,`` you can see all files are .txt, meaning Data Lens is protecting against the operation of creating new or changing ransomware files according to the signature list. Check ``FSxyz-a-dr.ntnxlab.local\DLtest-dr``, you will see files were created and modified without Data Lens' protection.

9.  Wait until you receive email about ransomware attack alert.
            ![](images/dl8.png)

    !!!note
            It may take up to 15 minutes to receive this email. You can proceed to other labs while you are waiting.

10. Go back to **Data Lens** > **FSxyz-a-prod.ntnxlab.local** > :fontawesome-solid-bars: > **Ransomware Protection**, you can see the threats are recorded and the File Server is set to Read-Only mode automatically.
   
      ![](images/dl9.png)

11. Login to your WinToolsVM and go to **\\\FSxyz-a-prod.ntnxlab.local\DLtest-prod\\** from File Explorer; try to create a folder, you will see access denied as Data Lens has set it to read-only.
    
    ![](images/dl10.png)
    
    !!!note
           Data Lens sets the whole File Server to Read-Only mode. You can use another users to test on any shares in the same File Server. It should give you the same result.


12. Go back to **Data Lens** > **FSxyz-a-prod.ntnxlab.local** > :fontawesome-solid-bars: > **Ransomware Protection**. Under **Blocked Entities**, click **Unblock** > **Confirm** to resume to read-write access.
    
    !!!note
             You can always click the green circle ![](images/greencircle.png) to check the status of the tasks.

13.   Return to **Data Lens** sometimes later to check if it is done. Verify the access from your **WinToolsVM**.
