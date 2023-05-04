# Role Base Access Control for NUS

## Overview

XXX

## Lab Setup

The lab has enabled MSP on Prism Central which is a pre-requisite of using RBAC. In real deployment, you should first enable microservices infrastruture before starting this lab.

## Define Roles

1. Login to Prism Central as **admin**, select :fontawesome-solid-bars: **> Administration > Roles**, click **Create Role**

    ![](images/1.png)

2. In this menu, fill out the fields:


    -   Put **Role Name**: **NUS User**
    -   Expand **Object Store**, click **View Access**
    -   Expand **File Server**, click **View Access**
        ![](images/2.png)
    -   Click **Change** next to **Set custom permissions** in **File Server**
    -   Click **Create File Server Share**, then click **Save**
        ![](images/3.png)
    -   Click **Save**

3. You will find **NUS user** is showing in the Role.
    ![](images/4.png)

## Assign Users to Roles

1. In Prism Central as **admin**, select :fontawesome-solid-bars: **> Administration > Roles**, click **the checkbox next to NUS user**

2. Click **Action** > **Manage Assignment** > **Add New**

3. In the **Search User** box, put **user01** and select **user01@ntnxlab.local**
    ![](images/5.png)

4. In the right side, choose **File Server** as the Entity Type, then select **Individual entity**

5. In the search box, find and select **FS*XYZ*-*A*-prod**

6. Choose **Object Store** as the Entity Type, in the search box. find and select **ntnx-objects**
    ![](images/6.png)

7. Click **Save**

!!!note
        You can add multiple users / file servers / object stores into the same assignment.

## Verify User Access Right

1. Logout from admin to **Prism Central**

    !!!note
            If you do not want to logout, you can use another browser or go to Incognito Mode of the browser so you can use different users to login at the same time.

2. Make sure you are on the page showing **Login with your Company ID**

3. Login as user01@ntnxlab.local, and click Log In
    ![](images/7.png)

4. Click :fontawesome-solid-bars:, you will see the list of service is shorter.

5. Click **Services**, you can only see **Files** and **Objects**

6. Click **Files**, and click **FS*XYZ*-*A*-prod**








## Takeaway