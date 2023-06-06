# Data Lens - Deploy & Basic Configuration
## Overview

In this exercise you will explore Data Lens, auditing & ransomware protection features and trigger a ransomware event.


!!!note
        As we are using a Nutanix Dev environment, this lab is only available for **Employees**. Please consult the instructor for assistence if you are a Nutanix customers or partners.
        Please **ONLY** use your registered server

## Login to Data Lens

1. Connect to corp VPN gateway without **(ST)**
   
2. Go to https://datalens-qa.nutanix.com/ 
   
3. Please use the Datalens/password credentials provided in the **HPOC lookup**.
   
4. Choose **Common Tenant** and then **Proceed**.
   
    ![](images/dl1.png)

5. In the **Data Lens Global Dashboard**, go to **File Servers** and search the FQDN of your File Server Name (**FSxyz-a-prod**).
   
    ![](images/dl2.png)

    !!!note 
           Your File Server is already added and enabled to the Data Lens Dashboard. Contact lab instructor if you cannot find it.


6. Click the File Server Name to enter the Dashboard.

## Basic Data Lens Operations

7. Check the **Dashboard** page in your browser to see the usage for the File Server. 
   
8. Check the **Share All** on the top left corner, and select any share to see the share level usage.
    
    ![](images/dl3.png)

9. Here we will create a permission denial. Create a new directory called **RO** in your **XYZ-GSO** share

10. Create a text file in the **RO** directory called **myfile.txt** with some text like "hello world"

11. Go to the **Properties** of the **RO** folder and select the **Security** tab

12. Select **Advanced**

13. Choose **Disable inheritance** and select the **Convert...** option

14. Then add the **Everyone** permissions with the following attributes:

     -   Read & Execute
     -   List folder contents
     -   Read
   
     ![](images/43.png)

15. Choose **OK**, **OK** and **OK** again

16. Open a PowerShell window as a specific user

    -   Hold down **Shift** and **right click** on the **PowerShell
        icon** on the taskbar
    -   Select **Run as different user**

    ![](images/44.png)

17. Enter the following

    -   **User name**: devuser01
    -   **Password**: nutanix/4u

18. Change Directories to **RO** under the **xyz-GSO**

    ``` bash
    cd \\FSxyz-a-prod.ntnxlab.local\xyz-GSO\RO
    ```

19. Execute the following commands, the first should succeed, the second should fail:

    ``` bash
    more .\myfile.txt
    rm .\myfile.txt
    ```

    ![](images/45.png)

20. After a minute or so you should see **Permission Denials** in both the dashboard and the **Audit Trails** view. (You may need to refresh your browser).

    ![](images/46.png)


21. Refresh the **Dashboard** page to see the updates on **Top 5 active users**, **Top 5 accessed files** and **File Operations** panels.

22. Click on your user under **Top 5 Active Users**. This will take you to the audit trail of the user. Alternatively,  click on the **hamburger icon** then **Audit Trails** menu and search for either your user or a given file. You can use wildcards for your search, for example *.txt. 

You may continue to explore the data lens dashboard.

