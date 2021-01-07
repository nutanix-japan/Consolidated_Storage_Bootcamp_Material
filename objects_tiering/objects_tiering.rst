.. _objects_tiering:

------------------------------
Objects: Tiering
------------------------------

*The estimated time to complete this lab is 30 minutes.*

Overview
++++++++

Data storage is for your applications can get quite expensive for applications using Objects store.

To enable applications to have data that is infrequently used to be tiered to another Objects stores which may be cheaper and in a different region, Nutanix Objects provides Tiering feature.

This will enable customers to do the following:

- Tier to third-party objects based cloud storage
- Reduce storage consumption and cost for storing data that are old and infrequently used
- Take advantage of Nutanix Objects' features to reduce cost on PUT and GET requests by grouping Objects by application in a big chunk
- Decouple the application from managing storage and have Nutanix's proven HCI Storage features to effectively manage data
- Use industry standards for Objects like storage

**In this lab, you will walk through a Nutanix Objects Tiering feature to AWS S3 bucket. The configuration procedure remains the same for any S3 based storage tiering**

Lab Setup
++++++++++

At high level we will go through the following:

- Create AWS bucket as tiering destination
- Setup Endpoint in Object Store configuration
- Setup Lifecycle policies in Buckets configuration

This lab requires applications provisioned as part of the :ref:`windows_tools_vm`.

**Google Chrome is recommended for this lab.**

Creating AWS S3 Bucket as Tiering Destination
................................................................

In this section you will configure AWS S3 bucket, set up access permissions and get access and secret keys.

To Create an AWS S3 Bucket
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Sign in to the AWS Management Console and open the Amazon S3 console at https://console.aws.amazon.com/s3/
#. Choose **Create bucket**
#. In Bucket name, enter your initials (**lnb** here is an example) **lnb-bucket** DNS-compliant name for your bucket name

   .. figure:: images/tiering1.png
#. Choose your preferred region (in most cases this is automatically selected). Choose the closest region for tiering
#. Choose **Block all public access**
#. Choose to **Disable** Bucket versioning
#. Choose to **Disable** Server-side encryption
#. Click on **Create Bucket** button at the bottom of the page
#. You will now see your bucket in the list

   .. figure:: images/tiering2.png

To Setup Access for AWS S3 Bucket
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Go to your AWS IAM Management Console https://console.aws.amazon.com/iam/
#. Select **Users > Add User**
#. Enter **lnb-bucket-user** as the user name
#. Select **Programmatic access**
#. In the next window, select **Add user to group**
#. Since we don't have group yet, click on **Create group**

   .. figure:: images/tiering3.png

#. Enter **s3access** as the user group name (could be any name that is easy to identify)

#. In the **Filter Policies** input type **s3** and choose **AmazonS3FullAccess** as the policy. This provides all permissions. Feel free to explore other permission policies as well.

   .. figure:: images/tiering4.png

#. Click on **Create group**

#. Choose the **s3access** as the user group name and click on **Next: Tags** at the bottom of the screen

   .. figure:: images/tiering5.png

#. Click on **Next: Review**

#. Click on **Create user**

#. You will now a success message followed by download options for the access and secret key

#. Download access and secret key CSV file

   .. note::

   	 Make sure to download this CSV file and store it securely, as it will be only possible to do this once

   .. figure:: images/tiering6.png

#. Click on **Close**

You have successfully setup access to your AWS S3 bucket

Setup Endpoint in Object Store configuration
................................................................

In this section you will setup endpoints for tiering from Nutanix Objects that you created in :ref:`objects_deploy` to AWS S3.

Configuring Endpoint
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Login into your Prism Central instance.

#. In Prism Central, select :fa:`bars` **> Services > Objects**

#. Choose your Objects Store

   .. figure:: images/tiering7.png

#. This will open a new browser tab with additional settings for your chosen objects store

#. Select **Tiering Endpoint** and click on **Create**

   .. figure:: images/tiering8.png

#. In the add enpoint wizard, enter the following details

   - Name of the Endpoint - **AWS Tiering Endpoint** (give an easily identifiable name)
   - Service Host - **s3.ap-southeast-2.amazonaws.com**  (this will change depending on your AWS region)
   - Bucket Name - **lnb-bucket** (this is the name of the bucket you created in previous section in AWS)
   - Access Key - **AKIAWQKDDII6SHOQGYMV**
   - Secret Key - **secret key from CSV you downloaded** in the previous section

   .. figure:: images/tiering9.png

#. Click on **Save**

#. You will now be able to see the endpoint in your Object Store configuration

   .. figure:: images/tiering10.png

You have successfully setup a tiering endpoint which resides in AWS.


Configuring Lifecycle Policies
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Lifecycle policies allows to schedule tiering from source bucket to target bucket irrespective of the location.

In this section we will create a lifecycle policy to tier data from Nutanix Object's bucket that you created in :ref:`objects_versioning_access_control` to the AWS bucket you created earlier.

#. In Prism Central, select :fa:`bars` **> Services > Objects**

#. Choose your Objects Store

#. Click your source bucket *intials*-**Test-Bucket** (the one you created in here :ref:`objects_versioning_access_control`)

   .. figure:: images/tiering11.png

#. Click on **Lifecycle** and click on **Create Rule**

   .. figure:: images/tiering12.png

#. Enter a meaningful name that you can identify, for example **tier-to-aws-ap-southeast-2.amazonaws.com** which specifies the region of tiered data

#. Choose **All Objects**

   .. note::

   	Note that you are also able to use **tags** as an option to select the objects to replicate. Make sure to explain this selection feature to a end-user.

   .. figure:: images/tiering13.png

#. Click on **Next**

#. Select **AWS Tiering Endpoint**

#. Set tiering to **1** days after objects creation date in the source bucket

#. You can select expiration to 1 day as well in the destination storage as an example. Just to make sure you don't run into a huge bill in the public cloud.

#. Click on **Add Action** and choose another expire Action

#. Choose **Multipart Uploads** and **1** days after last creation date on destination bucket
