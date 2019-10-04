.. _objects_create_scripts:

----------------------------------------
Objects: Creating Buckets Using Scripts
----------------------------------------

Overview
++++++++

While tools like Cyberduck help to visualize how data is access within an object store, Nutanix Objects is primarily an object store service that is designed to be accessed and consumed over S3 APIs.

Amazon Web Services's S3 (Simple Storage Service) is the largest public cloud storage service, and has subsequently become the de-facto standard object storage API due to developer and ISV adoption. Objects provides an S3 compliant interface to allow for maximum portability, as well as support for existing "cloud native" applications.

Creating and Using Buckets From Scripts
+++++++++++++++++++++++++++++++++++++++

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

     endpoint_ip= "OBJECT-STORE-IP" #Replace this value
     access_key_id="ACCESS-KEY" #Replace this value
     secret_access_key="SECRET-KEY" #Replace this value
     endpoint_url= "https://"+endpoint_ip+":443"

     session = boto3.session.Session()
     s3client = session.client(service_name="s3", aws_access_key_id=access_key_id, aws_secret_access_key=secret_access_key, endpoint_url=endpoint_url, verify=False)

     # list the buckets
     response = s3client.list_buckets()

     for b in response['Buckets']:
       print (b['Name'])

#. Execute ``python list-buckets.py`` to run the script. Verify that the output lists any buckets you have created for using your first user account.

#. Using the previous script as a base, and the `Boto 3 documentation <https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3-examples.html>`_, can you modify the script to create a **new** bucket and then list all buckets?

   .. note::

     Totally stuck? Take a look at the completed script :download:`here <create-bucket.py>`

     If attempting to download the script results in an Access Denied error, log out of any AWS accounts from your browser or open the download link in **Incognito (Private Browsing)** mode.

Uploading Multiple Files to Buckets with Python
...............................................

#. From the *Initials*\ **-Linux-ToolsVM**, run the following to create 100 1KB files to be used as sample data for uploading:

   .. code-block:: bash

     mkdir sample-files
     for i in {1..100}; do dd if=/dev/urandom of=sample-files/file$i bs=1024 count=1; done

   While the sample files contain random data, these could just as easily be log files that need to be rolled over and automatically archived, surveillance video, employee records, and so on.

#. Modify your existing script or create a new script based on the example below:

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

#. Execute the script and use Cyberduck to verify the sample files are available.

   .. figure:: images/buckets_18.png

   Similar S3 SDKs are available for languages including Java, JavaScript, Ruby, Go, C++, and others, making it very simple to leverage Nutanix Buckets using your language of choice.

Takeaways
+++++++++

What are the key things you should know about **Nutanix Objects**?
