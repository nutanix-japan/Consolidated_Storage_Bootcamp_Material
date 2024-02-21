.. _objects_cli_scripts:

---------------------------------
Objects: Using From CLI & Scripts
---------------------------------

*The estimated time to complete this lab is 60 minutes.*

Overview
++++++++

Accessing Objects from the CLI
++++++++++++++++++++++++++++++

While tools like the Object Browser help to visualize how data is access within an object store, Buckets is primarily an object store service that is designed to be accessed and consumed over S3 APIs.

Amazon's S3 (Simple Storage Service) is the largest public cloud storage service, and has subsequently become the de-facto standard object storage API due to developer and ISV adoption. Buckets provides an S3 compliant interface to allow for maximum portability, as well as support for existing "cloud native" applications.

In this exercise you will leverage ``s3cmd`` to access your buckets using the CLI.

You will need the **Access Key** and **Secret Key** for the user created earlier in this lab.

Setting up s3cmd (CLI)
......................

This section of the lab is done on using Linux Tools VM.

If it is not already present in your HPOC, create Linux Tools VM using instructions in :ref:`linux_tools_vm`

#. Login to the *initials*-**Linux-ToolsVM**, with the following credentials

   - **Username** - root
   - **Password** - default nutanix password

#. Run ``s3cmd --configure`` and enter the following to configure access to the Object Store:

   .. note::

   For anything not specified below, just hit enter to leave the defaults. Do **NOT** set an encryption password and do **NOT** use HTTPS protocol.

   .. code-block:: bash

     s3cmd --configure

     - **Access Key**  - *Access Key*
     - **Secret Key**  - *Secret Key*
     - **Default Region [US]**  - us-east-1
     - **S3 Endpoint [s3.amazonaws.com]**  - *OBJECT-STORE-IP*
     - **DNS-style bucket+hostname:port template for accessing a bucket [%(bucket)s.s3.amazonaws.com]**  - *OBJECT-STORE-IP*\ :80
     - **Encryption password** - Leave Blank
     - **Path to GPG program [/usr/bin/gpg]**  - Leave Blank
     - **Use HTTPS protocol [Yes]**  - No
     - **HTTP Proxy server name**  - Leave Blank
     - **Test access with supplied credentials?**  - Y (Yes)

#. The output should look similar to this and match your environment:

   .. code-block:: bash

     New settings:
       Access Key: Ke2hEtehmOZoXYCrQnzUn_2EDD9Eqf0L
       Secret Key: p6sxh_FhxEyIteslQJKfDlezKrtJro9C
       Default Region: us-east-1
       S3 Endpoint: 10.20.95.51
       DNS-style bucket+hostname:port template for accessing a bucket: 10.20.95.51:80
       Encryption password:
       Path to GPG program: /usr/bin/gpg
       Use HTTPS protocol: False
       HTTP Proxy server name:
       HTTP Proxy server port: 0

     Test access with supplied credentials? [Y/n] y
     Please wait, attempting to list all buckets...
     Success. Your access key and secret key worked fine :-)

     Now verifying that encryption works...
     Not configured. Never mind.

     Save settings? [y/N] y
     Configuration saved to '/root/.s3cfg'

#. Type **Y** and press **Return** to save the configuration.

#. Type on Linux Tools VM ``nano .s3cfg`` then edit the value *signature_v2 = True*

Create A Bucket And Add Objects To It Using s3cmd (CLI)
.......................................................

#. Now let's use s3cmd to create a new bucket called *your-name*\ **-clibucket**.

#. From the same Linux command line, run the following command:

   .. code-block:: bash

     s3cmd mb s3://yourname-clibucket

#. You should see the following output:

   .. code-block:: bash

     Bucket 's3://yourname-clibucket/' created

#. List your bucket with the **ls** command:

   .. code-block:: bash

     s3cmd ls

#. You will see a list of all the buckets in the object-store.

#. To see just your buckets run the following command:

   .. code-block:: bash

     s3cmd ls | grep yourname

#. Now that we have a new bucket, let's upload some data to it.

#. If you do not already have the Sample-Pictures.zip, download it to your Linux-ToolsVM.

   .. code-block:: bash

    curl https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip -O -J -L
    mkdir sample-pictures
    #extract only the picture files
    unzip -j SampleData_Small.zip *.png -d sample-pictures

#. List images in sample-pictures folder

   .. code-block:: bash

    ls sample-pictures

#. Run the following command to upload one of the images to your bucket:

   .. note::

      Make sure to replace $IMAGENAME with an image name listed from the previous step

   .. code-block:: bash

     cd sample-pictures
     s3cmd put --acl-public --guess-mime-type $IMAGENAME s3://<your-bucket-name>/$IMAGENAME
     
#. You should see the following output:

   .. code-block:: bash

     #example command and output 
     
     s3cmd put --acl-public --guess-mime-type yahoo-identity-provider-config.png s3://lb-cli-bucket/yahoo-identity-provider-config.png

     upload: 'yahoo-identity-provider-config.png' -> 's3://lb-cli-bucket/yahoo-identity-provider-config.png'  [1 of 1]
     63937 of 63937   100% in    0s     4.51 MB/s  done
     Public URL of the object is: http://10.38.188.18/lb-clibucket/yahoo-identity-provider-config.png

#. If desired, repeat with more images.

#. Run the **la** command to list all objects in all buckets:

   .. code-block:: bash

     s3cmd la

#. To see just objects in your buckets, run the following command:

   .. code-block:: bash

     s3cmd la | grep *initials*

Creating and Using Buckets From Scripts
+++++++++++++++++++++++++++++++++++++++

While tools like the Objects Browser help to visualize how data is access within an object store, Nutanix Objects is primarily an object store service that is designed to be accessed and consumed over S3 APIs.

Amazon Web Services's S3 (Simple Storage Service) is the largest public cloud storage service, and has subsequently become the de-facto standard object storage API due to developer and ISV adoption. Objects provides an S3 compliant interface to allow for maximum portability, as well as support for existing "cloud native" applications.

In this exercise you will use **Boto 3**, the AWS SDK for Python, to manipulate your buckets using Python scripts.

You will need the **Access Key** and **Secret Key** for the user account created earlier in this lab.

Listing and Creating Buckets with Python
........................................

In this exercise, you will modify a sample script to match your environment, which will list all the buckets available to that user. You will then modify the script to create a new bucket using the existing S3 connection.

#. From the *Initials*\ **-Linux-ToolsVM**, run ``vi list-buckets.py`` and paste in the script below. You will need to modify the **endpoint_ip**, **access_key_id**, and **secret_access_key** values before saving the script.


   .. note::

    If you are not comfortable with ``vi`` or alternative command line text editors, you can modify the script in a GUI text editor then paste the completed script into ``vi``.

    In ``vi``, type ``i`` and then right-click to paste into the text file.

    Press **Ctrl + C** then type ``:wq`` and press **Return** to save the file.

   .. code-block:: python

      #!/usr/bin/python

      import boto3
      import warnings
      warnings.filterwarnings("ignore")

      endpoint_ip="OBJECT-STORE-IP" #Replace this value
      access_key_id="ACCESS-KEY" #Replace this value
      secret_access_key="SECRET-KEY" #Replace this value
      endpoint_url= "https://"+ endpoint_ip +":443"

      session = boto3.session.Session()
      s3client = session.client(service_name="s3", aws_access_key_id=access_key_id, aws_secret_access_key=secret_access_key, endpoint_url=endpoint_url, verify=False)

      # list the buckets
      response = s3client.list_buckets()

      for b in response['Buckets']:
        print (b['Name'])

#. Execute ``python list-buckets.py`` to run the script. Verify that the output lists any buckets you have created for using your first user account.

#. Using the previous script as a base, and the `Boto 3 documentation <https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3-examples.html>`_, can you modify the script to create a **new** bucket and then list all buckets?

Uploading Multiple Files to Buckets with Python
...............................................

#. From the *Initials*\ **-Linux-ToolsVM**, run the following to create 100 1KB files to be used as sample data for uploading:

   .. code-block:: bash

     cd ..;mkdir sample-files
     for i in {1..100}; do dd if=/dev/urandom of=sample-files/file$i bs=1024 count=1; done

   While the sample files contain random data, these could just as easily be log files that need to be rolled over and automatically archived, surveillance video, employee records, and so on.

#. Create a new script based on the example below:

   .. code-block:: python

    #!/usr/bin/python

    import boto3
    import glob
    import re
    import warnings
    warnings.filterwarnings("ignore")

    # user defined variables
    endpoint_ip= "OBJECT-STORE-IP" #Replace this value
    access_key_id="ACCESS-KEY" #Replace this value
    secret_access_key="SECRET-KEY" #Replace this value
    bucket="BUCKET-NAME-TO-UPLOAD-TO" #Replace this value
    name_of_dir="sample-files"

    # system variables
    endpoint_url= "https://"+endpoint_ip+":443"
    filepath = glob.glob("%s/*" % name_of_dir)

    # connect to object store
    session = boto3.session.Session()
    s3client = session.client(service_name="s3", aws_access_key_id=access_key_id, aws_secret_access_key=secret_access_key, endpoint_url=endpoint_url, verify=False)

    # go through all the files in the directory and upload
    for current in filepath:
        full_file_path=current
        m=re.search('sample-files/(.*)', current)
        if m:
          object_name=m.group(1)
        print("Path to File:",full_file_path)
        print("Object name:",object_name)
        response = s3client.put_object(Bucket=bucket, Body=full_file_path, Key=object_name)

   The `put_object <https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html?highlight=put_object#S3.Bucket.put_object>`_ method is used for the file upload. Optionally this method can be used to define the metadata, content type, permissions, expiration, and other key information associated with the object.

   Core S3 APIs resemble RESTful APIs for other web services, with PUT calls allowing for adding objects and associated settings/metadata, GET calls for reading objects or information about objects, and DELETE calls for removing objects.

   Execute the script and use the following command to verify that the sample files are available.

   .. code-block:: bash
    
    s3cmd ls s3://yourname-clibucket/

    # example command and output

    s3cmd ls s3://lb-clibucket/

    2021-12-20 05:56        18   s3://lb-clibucket/file1
    2021-12-20 05:56        19   s3://lb-clibucket/file10
    2021-12-20 05:56        20   s3://lb-clibucket/file100
    2021-12-20 05:56        19   s3://lb-clibucket/file11
    2021-12-20 05:56        19   s3://lb-clibucket/file20
    2021-12-20 05:56        19   s3://lb-clibucket/file2
    <output snipped>
  
Similar S3 SDKs are available for languages including Java, JavaScript, Ruby, Go, C++, and others, making it very simple to leverage Nutanix Buckets using your language of choice.

Takeaways
+++++++++

What are the key things you should know about **Nutanix Objects**?

- Nutanix Objects provides a simple and scalable S3-compatible object storage solution, optimized for DevOps, Long Term Retention and Backup Target use cases.

- Nutanix Objects can be deployed on an AHV cluster, with ESXi support on the roadmap.

- Nutanix Objects will be enabled and deployed from Prism Central.
