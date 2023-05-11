# Data Lens - Deploy & Basic Configuration
## Overview

In this exercise you will enable Data Lens and experience auditing & ransomware protection; and trigger a ransomware event.

## Login to Data Lens

1. Connect to corp VPN, select the gateway without **(ST)**
   
2. Go to https://datalens-qa.nutanix.com/ 
   
3. Your instructor will give you a my portal account to login
   
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

9.  Create some audit trail activity by going to share **dltest** and open one of the .txt file.

10. Refresh the **Dashboard** page to see the updates on **Top 5 active users**, **Top 5 accessed files** and **File Operations** panels.

11. Click on your user under **Top 5 Active Users**. This will take you to the audit trail of the user. Or click on the **hamburger icon** then **Audit Trails** menu and search for either your user or a given file. You can use wildcards for your search, for example .txt. 

12.   