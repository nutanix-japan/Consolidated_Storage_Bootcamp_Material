# Objects: Creating Buckets, Users, and Access Control

## Overview

## Create Bucket In Prism Central

A bucket is a sub-repository within an object store which can have
policies applied to it, such as versioning, WORM, etc. By default a
newly created bucket is a private resource to the creator. The creator
of the bucket by default has read/write permissions, and can grant
permissions to other users.

1.  Go to Prism Central, click **Infrastructure > Objects**

2.  Click on **ntnx-objects** Objects Store

3.  The Objects Store management will open in a new browser tab

4.  Go to **Bucket**, click **Create Bucket**, and fill out the following fields:

    -   **Name** - **your-name**-my-bucket
    -   **Enable Versioning** - Checked
    -   **Multiprotocol Access** - disabled (selecting **Enable
        versioning** will disable this)

    ![](images/objects_05.png)

5.  Click **Create**.

    !!!note

           Buckets created via Prism Central are owned by the Prism Central
           admin.

    If versioning is enabled, new versions can be uploaded of the same
    object for required changes, without losing the original data.

    Lifecycle policies define how long to keep data in the system.

    Once the bucket is created, it can be configured with WORM.

    WORM (Write Once, Read Many) storage prevents the editing,
    overwriting, renaming, or deleting data and is crucial in heavily
    regulated industries (finance, healthcare, public agencies, etc.)
    where sensitive data is collected and stored. Examples include
    e-mails, account information, voice mails, and more.

    !!!note

           If WORM is enabled on the bucket, this will supersede any lifecycle policy.

6.  Check the box next to your *your-name*-**my-bucket** bucket, and click on **Actions** dropdown and choose **Configure WORM**

7.  Check the box next to **Enable WORM** bucket,

8.  Enter **1** month as the retention periodand click **Enable WORM**.

    !!!note

           You have the ability to define a WORM data retention period on a per bucket basis.

9. Create the another bucket in **ntnx-objects-dr** with the name : **your-name**-my-bucket-dr and follow the same configuration as **your-name**-my-bucket.

## Buckets User Management

In this exercise you will create generate access and secret keys to access the object store, that will be used throughout the lab.

1.  Go to Prism Central, click **Infrastructure > Objects**

2.  From the left panel of Objects UI, click on **Access Keys** and click **Configure Directories**

3.  Check if domain **ntnxlab.local** is added to the directory service. If not, add that in by clicking **+ Add Directory**.
    -   **Directory Type** - Active Directory
    -   **Name** - ntnxlab
    -   **Domain** - ntnxlab.local
    -   **Directory URL** - ldap://[domain controller IP]:389
    -   **Username** - ntnxlab\administrator
    -   **Password** - nutanix/4u

    ![](images/objects_add_people_0.png)

4.  If directory is added, go to **Add People** from **Access Keys**.

    ![](images/objects_add_people.png)

5.  Select **Search for people in a directory service** and search for **adminuser##** where ## is your assigned number.

    ![](images/objects_add_people_02.png)

6.  Click **Next**.

7.  Click **Generate Keys** to generate a ket.

    ![](images/objects_add_people_04.png)

8.  Click **Download Keys** to download a .txt file containing the
    **Access Key** and **Secret Key**.

    ![](images/buckets_add_people3.png)

9.  Click **Close**.

10. Open the file with a text editor.

    ![](images/buckets_csv_file.png)

    !!!note 

           Keep the text files open so that you have the access and secret keys
           readily available for future labs.
## Adding Users to Buckets Share

1.  Go to Prism Central, click **Infrastructure > Objects**

2.  Click on **ntnx-objects** Objects Store

3.  The Objects Store management will open in a new browser tab (if it
    not already open)

4.  Click on **your-name**-**my-bucket** bucket, and click on **User
    Access**

5.  Click on **Edit User Access** button

    This is where you will be able to share your bucket with other
    users. You can configure read only, full access
    , or custom, on a per user basis.

6.  Add the user (email address) you created earlier, click **Full Access**

    ![](images/buckets_share.png)

7.  Click on **Save**

8.  Do the same user access setting to **your-name**-my-bucket-dr in **ntnx-objects-dr**

## Accessing & Creating Buckets With Objects Browser

In this exercise you will use Objects Browser to create and use buckets
in the object store using your generated access key.

### Download the Sample Images

1.  Login to **Initials-Windows-ToolsVM** via RDP using the following
    credentials:
    -   **Username** - NTNXLAB\\Administrator
    -   **password** - ask your Instructor
2.  Use this
    [link](http://10.42.194.11/workshop_staging/peer/SampleData_Small.zip)
    to download the sample files to your Windows-ToolsVM. Once the
    download is complete, extract the contents of the .zip file.

### Use Object Browser to Create A Bucket

1.  From the Objects UI, Locate the **ntnx-objects** object store.

    ![](images/obj_01.png)

2.  The objects browser will open in a new tab

3.  Enter the following fields from the downloaded key file (while you
    created user access) and click **Login** button:

    -   **Access Key** - *Generated When User Created*
    -   **Secret Key** - *Generated When User Created*

    ![](images/obj_02.png)

4.  Click the **+ Create Bucket**.

5.  Enter the following name for your bucket, and click **Create**:

    -   **Bucket Name** - *intials*-**test-bucket**
    -   **Enable versioning** - checked

    ![](images/obj_03.png)

    !!!note

           Bucket names must be lower case and only contain letters, numbers,
           periods and hyphens.
       
           Additionally, all bucket names must be unique within a given Object
           Store. Note that if you try to create a folder with an existing
           bucket name (e.g. *your-name*-my-bucket), creation of the folder
           will not succeed.

    Creating a bucket in this fashion allows for self-service for
    entitled users, and is no different than a bucket created via the
    Prism Buckets UI.

6.  Click into your **intials*-**test-bucket** bucket, and Click the **+** and **Upload Objects**.

7.  Click on **Select Folder**

    ![](images/obj_04.png)

8.  Navigate to your downloads directory and find the Sample Pictures
    folder. Upload one or more pictures to your bucket.

9.  That is how easy it is to use the Objects Browser.

## Object Versioning

Object versioning allows the upload of new versions of the same object
for required changes, without losing the original data. Versioning can
be used to preserve, retrieve and restore every version of every object
stored within a bucket, allowing for easy recovery from unintended user
action and application failures.

1.  Open Notepad in *Initials***-Windows-ToolsVM**.

2.  Type "version 1.0" in Notepad, then save the file with a name (e.g.
    A.txt)

3.  In Objects Browser, upload the text file to your
    **intials*-**test-bucket** bucket.

4.  Make changes to the text file (e.g. A.txt) in Notepad and save it
    with the same name, overwriting the original file.

5.  Upload the modified file to your bucket. If desired, you can update
    and upload the file multiple times.

6.  Back in Prism Cental \> Services \> Objects, click on **Object
    Stores**.

7.  In Objects Browser of your **intials*-**test-bucket** bucket

8.  Select your **A.txt** file

9.  Under the **Actions** menu, scroll down to select **Versions**

10. You should be able to see the different versions of the file A.txt

    ![](images/obj_05.png)



## Takeaways

What are the key things you should know about **Nutanix Objects**?

-   Nutanix Objects provides a simple and scalable S3-compatible object
    storage solution, optimized for DevOps, Long Term Retention and
    Backup Target use cases.
-   Nutanix Objects can be deployed on an AHV cluster, with ESXi support
    on the roadmap.
-   Nutanix Objects will be enabled and deployed from Prism Central.
