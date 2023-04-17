# File Analytics: Anomaly Rules {#file_analytics_anomaly}

## Overview

## Define Anomaly Rules

1.  Create two anomaly rules by going to **Define Anomaly Rules** from
    under the `cog`{.interpreted-text role="fa"} gear icon

    ![](images/39.png)

2.  Create the first rule with the following settings:

    -   **Events:** Delete
    -   **Minimum Operation %:** 1
    -   **Minimum Operation Count:** 10
    -   **User:** All Users
    -   **Type:** Hourly
    -   **Interval:** 1

3.  Choose **Save** for that anomaly table entry

4.  Choose **+ Configure new anomaly** and create a second rule with the
    following settings

    -   **Events**: Create
    -   **Minimum Operation %**: 1
    -   **Minimum Operation Count**: 10
    -   **User**: All Users
    -   **Type**: Hourly
    -   **Interval**: 1

5.  Choose **Save** for that anomaly table entry

    ![](images/40.png)

6.  Select **Save** to exit the Define Anomaly Rules window

## Load Sample Data

1.  Go to the Sample Data folder in the Marketing share and copy, then
    paste that folder to the same share.

    ![](images/42.png)

2.  Now delete the original Sample Data folder.

## Cause Error Condition

1.  While waiting for the Anomaly Alerts to populate we'll create a
    permission denial.

    ::: note
    ::: title
    Note
    :::

    The Anomaly engine runs every 30 minutes. While this setting is
    configurable from the File Analytics VM, modifying this variable is
    outside the scope of this lab.
    :::

2.  Create a new directory called **RO** in the Marketing share

3.  Create a text file in the **RO** directory with some text like
    "hello world" called **myfile.txt**

4.  Go to the **Properties** of the **RO** folder and select the
    Security tab

5.  Select **Advanced**

6.  Choose **Disable inheritance** and select the **Convert...** option

7.  Then add the **Everyone** permissions with the following:

    -   Read & Execute
    -   List folder contents
    -   Read

    ![](images/43.png)

8.  Choose **OK**, **OK** and **OK** again

9.  Open a PowerShell window as a specific user

    -   Hold down **Shift** and **right click** on the **PowerShell
        icon** on the taskbar
    -   Select **Run as different user**

    ![](images/44.png)

10. Enter the following

    -   **User name**: devuser01
    -   **Password**: nutanix/4u

11. Change Directories into the Marketing share and the **RO** directory

    > ``` bash
    > cd \\xyz-files.ntnxlab.local\marketing\RO
    > ```

12. Execute the following commands, the first should succeed, the second
    should fail:

    > ``` bash
    > more .\myfile.txt
    > rm .\myfile.txt
    > ```

    ![](images/45.png)

13. After a minute or so you should see **Permission Denials** in both
    the dashboard and the **Audit Trails** view. You may need to refresh
    your browser.

    ![](images/46.png)

    ::: note
    ::: title
    Note
    :::

    The Capacity Trend dashboard panel updates every 24 hrs.
    :::
