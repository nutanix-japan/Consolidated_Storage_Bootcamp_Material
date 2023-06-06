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
        You should have set SMTP service from the previous lab on Anomaly Detection, if not, you should set SMTP here.
        If someone put his email address in the same box, add your email address at the end and separate the email address by comma.
    !!!

    ![](images/ransomware_enable.png) 

5.  In Ransomware page, click **Settings**, search for ****.satana*,*.AngryDuck** in the search box. If it is not in the signature list, click **Add** to add it, otherwise just close this box.

6.  Login to **WinToolsVM** using username : **administrator@ntnxlab.local**

7.  Check the following share in **File Explorer**: 
      - ``FS*XYZ-A*-prod.ntnxlab.local\UserXX-prod``

8.  Create a new file **ransomware_test.ps1** in Downloads, open the file and put the following content in the file. **Save** it.
      ```bash
        new-item \\FSxyz-prod.ntnxlab.local\UserXX-prod\IamAngry.AngryDuck -ItemType file
        Get-ChildItem \\FSxyz-prod.ntnxlab.local\UserXX-prod\*.txt | Rename-Item -NewName { $_.Name -replace '.txt','.satana' }
      ```

6. Right click the :simple-powershell: (PowerShell icon) and select **Run as Administrator**.

7. Run the script, which has some file creation and changing file type to both shares.

    ```bash
    C:\Users\Administrator\Downloads\ransomware_test.ps1
    ```

9.   Check the share ``FSxyz-prod.ntnxlab.local\UserXX-prod,`` you can see all files are .txt, meaning File Analytics is protecting against the operation of creating new or changing ransomware files according to the signature list.

10. Wait until you receive email about ransomware attack alert.

Note: You can view the permission denied event in the **Audit Trails**
as well.
