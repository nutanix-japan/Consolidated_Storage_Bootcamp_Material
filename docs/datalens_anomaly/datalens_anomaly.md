# Data Lens - Anomaly Detection
## Overview

In this exercise, we will explore anomaly rules and trigger some events against the rules.

## Login to Data Lens (if you are not already there)

1. Connect to corp VPN, select the gateway without **(ST)**
   
2. Go to https://datalens-qa.nutanix.com/ 
   
3. Your instructor will give you a my portal account to login
   
4. Choose **Common Tenant** and then **Proceed**.
   
      ![](images/dl1.png)

5. In the **Data Lens Global Dashboard**, go to **File Servers** and search the FQDN of your File Server Name(**FSxyz-a-prod**).

      ![](images/dl2.png)

    !!!note 
           Your File Server is already added and enabled to the Data Lens Dashboard. Contact lab instructor if you cannot find it.


6. Click the File Server Name to enter the Dashboard.

## Define Anomaly Rules

1.  Click on :fontawesome-solid-bars: > **Anomalies**

2.  Click **Define Anomaly Rules**

3.  Add your email address to the **Anomaly Email Recipients**, then click **Save**

4.  Click + Define Anomaly Rules, then create the first rule with the following settings:
    - Operations: **Delete**
    - Minimum Operation %: **1**
    - Minimum Operation Count: **1**
    - User: **All Users**
    - Monitors: **Hourly**
    - Interval: **1**
5.  Click ![](images/tick.png) to save the rule

6.  Click **+ Configure new anomaly** and create a second rule with the following settings:
    - Operations: **Create**
    - Minimum Operation %: **1**
    - Minimum Operation Count: **1**
    - User: **All Users**
    - Monitors: **Hourly**
    - Interval: **1**

7.  Click ![](images/tick.png) to save the rule

8.  Click **Close** to go back to the dashboard
        ![](images/dl3.png)

9.  From the dashboard you are able to see number of anomaly alerts. top users and top folders that trigger anomaly rules on the past period.

## Trigger anomaly rules

1.  While waiting for the Anomaly Alerts to populate,  we'll create a permission denial.

2.  Create a new directory called **RO** in the Marketing share

3.  Create TWO text files in the **RO** directory called **myfile01.txt** & **myfile02.txt** with some text.

4.  Delete the TWP files. The creation and deletion of files should trigger the anomaly rules.

5. Continue on the next lab until you receive email warning about anomalies detected. 
        ![](images/dl4.png)

6.  Go back to Data Lens Anomaly Detection dashboard and check the charts.
        ![](images/dl5.png)



