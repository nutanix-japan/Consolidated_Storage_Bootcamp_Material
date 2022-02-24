.. _objects_multiprotocol:

------------------------
Objects: Multi-Protocol
------------------------

In this section, we will see how to create a bucket and let a NFS client access this to be able to do regular folder and files operations.

- Objects multi-protocol is a new feature with Objects 3.3.1
- Multi-protocol feature only works with a fresh install of Object 3.3.1 onwards
- Upgrade Objects 3.2.1 and below to 3.3.1 doesn't allow for this functionality
- Depending on your lab environment you may need to do the following:
  
  - Upgrade Objects version to 3.3.1 or above using LCM
  - Deploy new Objects store version 3.3.1 or above  
  - Implement multi-protocol functionality with Objects and NFSv3 clients 


Lab Agenda
++++++++++

We will do the following in this lab:

- Create Objects store version 3.3.1 if not already present
- Configure NFS access to a specific NFS client (your *Initials*-**Linux-ToolsVM**)
- Create bucket
- Mount the share in the NFS client
- Perform file and folder level operation in the NFS client 
- Perform file and folder level operation in the objects browser

Create Objects Store 
++++++++++++++++++++

Make sure your objects store **ntnx-objects** is of version 3.3.x or newer.

If your **ntnx-objects** is not of version 3.3.x or newer,

- Update LCM by running an inventory
- Deploy a new two node (1 node loadbalancer and 1 node worker) objects store **initials-objects** using the instructions here :ref:`objects_deploy`
- Use the **initials-objects** as the the object store for the rest of the lab

Configure NFS Allow Client 
++++++++++++++++++++++++++

In this section we will allow your *LinuxToolsVM* to be able to access buckets using NFS3 protocol.

#. Go to Prism Central

#. Go to :fa:`bars` > Services > Objects 

#. Click on **ntnx-objects** Objects Store

#. The Objects Store management will open in a new browser tab 

#. Click on **NFS Clients Allowlist**

#. Click on ** Add Client**

   .. figure:: images/objects_nfs_allow.png

#. Enter your *Initials*-**Linux-ToolsVM** IP address followed by ``/32`` to specify access only to this client 

   .. figure:: images/objects_nfs_client.png

#. Click on **+ Add** and **Save** at the bottom of the pop-up window

Create Bucket In Prism
+++++++++++++++++++++++

A bucket is a sub-repository within an object store which can have policies applied to it, such as versioning, WORM, etc. By default a newly created bucket is a private resource to the creator. The creator of the bucket by default has read/write permissions, and can grant permissions to other users.

#. Go to Prism Central

#. Go to :fa:`bars` > Services > Objects 

#. Click on **ntnx-objects** Objects Store

#. The Objects Store management will open in a new browser tab 

#. Click **Create Bucket**, and fill out the following fields:

   - **Name**  - *Intials*-multi
   - **Enable NFS v3 access**  - checked
   - **Owner UID** - 8888
   - **Owner GID** - 8888

#. For **Default Access Permissions** select in the following: 

   .. note:: 

     We are configuring all permissions and priveleges here. However in a production environment you would be careful about giving the appropriate permissions. 

   - **Files**

     - **Owner** - read, write, execute
     - **Group** - read, write, execute
     - **Others** - read, write, execute

   - **Directory**

     - **Owner** - read, write, execute
     - **Group** - read, write, execute
     - **Others** - read, write, execute
   
   - **Advanced Settings** - default (leave as-is)

   .. figure:: images/objects_multi_bucket.png

#. Click **Create**

Buckets User Management
+++++++++++++++++++++++

.. note::

  Perform these steps only if you **have not** created a user before and **have not** downloaded the access keys in the previous :ref:`objects_buckets_users_access_control` lab.

In this exercise you will create generate access and secret keys to access the object store, that will be used throughout the lab.

#. Go to Prism Central

#. Go to :fa:`bars` > Services > Objects 

#. From the Objects UI, click on **Access Keys** and click **Add People**.

   .. figure:: images/objects_add_people.png

#. Select **Add people not in a directory service** and enter your e-mail address.

   .. figure:: images/objects_add_people_02.png

#. Click **Next**.

#. Click **Generate Keys** to generate a ket.

   .. figure:: images/objects_add_people_04.png

#. Click **Download Keys** to download a .txt file containing the **Access Key** and **Secret Key**.

   .. figure:: images/buckets_add_people3.png

#. Click **Close**.

#. Open the file with a text editor.

   .. figure:: images/buckets_csv_file.png

Adding Users to Buckets Share
+++++++++++++++++++++++++++++

In this section, we will add user to the *Intials*-multi bucket, so we can access the bucket to upload/create files and folders.

#. Go to Prism Central

#. Go to :fa:`bars` > Services > Objects 

#. Click on **ntnx-objects** Objects Store

#. The Objects Store management will open in a new browser tab (if it not already open)

#. Click on *Intials*-multi bucket, and click on **Share** 

   .. figure:: images/buckets_share_option.png

#. Click on **Edit User Access** button

   This is where you will be able to share your bucket with other users. You can configure read access (download), write access (upload), or both, on a per user basis.

#. Add the user (email address)you created earlier, with *Read* and *Write* permissions

   .. figure:: images/buckets_share.png

#. Click on **Save**

Accessing Bucket on NFS Client
++++++++++++++++++++++++++++++

In this section we will mount the *Intials*-multi bucket as a NFSv3 share on the *initials*-**Linux-ToolsVM** to create files and folders.

If it is not already present in your HPOC, create Linux Tools VM using instructions in :ref:`linux_tools_vm`

#. Login to the *initials*-**Linux-ToolsVM**, with the following credentials

   - **Username** - root
   - **Password** - default nutanix password

