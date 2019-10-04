.. _objects_access_tools:

---------------------------------------------------------
Objects: Access & Create Buckets with Client Applications
---------------------------------------------------------

Overview
++++++++

In this exercise you will use `Cyberduck <https://cyberduck.io/>`_ to create and use buckets in the object store using your generated access key. Cyberduck is a multi-platform GUI application that supports multiple protocols including FTP, SFTP, WebDAV, and S3.

.. note::

  Cyberduck ver 7.1.0 is already installed on the Windows Tools VM you deployed earlier. If it is not already pinned to the taskbar, you can find it in the Windows menu.

Accessing & Creating Buckets
++++++++++++++++++++++++++++

Download the Sample Images
..........................

#. Login to *Initials*\ **-Windows-ToolsVM** via RDP using the following credentials:

   - **Username** - NTNXLAB\\Administrator
   - **password** - nutanix/4u

#. `Click here <https://s3.amazonaws.com/get-ahv-images/sample-pictures.zip>`_ to download the sample images to your Windows-ToolsVM. Once the download is complete, extract the contents of the .zip file.

Use Cyberduck to Create A Bucket
................................

#. Launch **Cyberduck** (Click the Window icon > Down Arrow > Cyberduck).

   If you are prompted to update Cyberduck, click **Skip This Version**.

#. Click on **Open Connection**.

   .. figure:: images/buckets_06.png

#. Select **Amazon S3** from the dropdown list.

#. Enter the following fields for the user created earlier, and click **Connect**:

   - **Server**  - *Objects Client Used IP*
   - **Port**  - 443
   - **Access Key ID**  - *Generated When User Created*
   - **Password (Secret Key)** - *Generated When User Created*

   .. figure:: images/buckets_08.png

#. Check the box **Always Trust** and then click **Continue** on the **The certificate is not valid** dialog box.

   .. figure:: images/invalid_certificate.png

#. Click **Yes** to continue installing the self-signed certificate.

#. Once connected, right-click anywhere inside the pane and select **New Folder**.

#. Enter the following name for your bucket, and click **Create**:

   - **Bucket Name** - *your-name*-bucket

   .. note::

     Bucket names must be lower case and only contain letters, numbers, periods and hyphens.

     Additionally, all bucket names must be unique within a given Object Store. Note that if you try to create a folder with an existing bucket name (e.g. *your-name*-my-bucket), creation of the folder will not succeed.

   Creating a bucket in this fashion allows for self-service for entitled users, and is no different than a bucket created via the Prism Buckets UI.

#. Double-click into the bucket, and right click and select **Upload**.

#. Navigate to your downloads directory and find the Sample Pictures folder. Upload one or more pictures to your bucket.

Working with Object Versioning
++++++++++++++++++++++++++++++

Object versioning allows the upload of new versions of the same object for required changes, without losing the original data. Versioning can be used to preserve, retrieve and restore every version of every object stored within a bucket, allowing for easy recovery from unintended user action and application failures.

Object Versioning
.................

#. Return to Cyberduck and re-connect using your user's access and secret keys. If you are already connected, make sure you are on the bucket listing page (the root folder in Cyberduck).

   .. figure:: images/root_folder.png

#. Select your bucket and and click **Get Info**.

   .. figure:: images/buckets_12.png

#. Under the **S3** tab, select **Bucket Versioning** and then close the window. This is equivalent to enabling versioning through Prism.

   .. figure:: images/buckets_13.png

#. Leave the Cyberduck connection open, and open Notepad in *Initials*\ **-Windows-ToolsVM**.

#. Type “version 1.0” in Notepad, then save the file.

#. In Cyberduck, upload the text file to your bucket.

#. Make changes to the text file in Notepad and save it with the same name, overwriting the original file.

#. Upload the modified file to your bucket. Click **Continue** when prompted to overwrite the existing file.

   If desired, you can update and upload the file multiple times.

#. In Cyberduck, click **View > Show Hidden Files**.

   .. figure:: images/buckets_14.png

#. Notice that all versions are shown with their individual timestamps. Toggle **View > Column > Version** to view the version number associated with each object.

   .. figure:: images/buckets_15.png

..  +++++++++++++++++++++++++++++++++++++++++++++++

  User Access Control
  +++++++++++++++++++

  In this exercise we will demonstrate user access controls and how to apply permissions so that other users or applications can access your bucket. For programmatic access to object storage, it is common for each application or service accessing the bucket to have its own access/secret key pair, so that access can be controlled granularly.

  Verify Current Access
  .....................

  In Cyberduck, click **Open Connection** and provide the Access and Secret Keys created for your second user account.

  Note that you will not see the bucket created using your first user's credentials, even if you had read access to the bucket. This is because the LIST operation to list buckets only shows buckets owned by the authenticated user.

  Click **Go > Go To Folder…**

  .. figure:: images/buckets_16.png

  Type in the name of User 1's bucket and click **Go**.

  .. figure:: images/buckets_17.png

  You should receive an Access Denied error.

  Leave your Cyberduck connection open for the following exercises.

  Grant Access to Another Bucket
  ..............................

  Access policy configuration will be in the UI in Buckets GA. In the early access software, we will use the following Linux command line ``mc`` tool to modify access to buckets.

  From the *Initials*\ **-Linux-ToolsVM**, run the following command to authenticate **MC** and allow the tool to configure the Object Store instance:

  .. code-block:: bash

    ./mc config host add NutanixBuckets http://<OBJECT-STORE-IP>:7200 USER-1-ACCESS-KEY USER-1-SECRET-KEY

  Replacing **YOUR-NAME**, run the following command to grant User 2 full access to User 1’s bucket.

  .. code-block:: bash

    ./mc policy --user=YOUR-NAME-2 grant public NutanixBuckets/YOUR-NAME-bucket

  Example output:

  .. code-block:: bash

    ./mc policy --user=John-Smith-2 grant public NutanixBuckets/john-smith-bucket
    Running grant command for bucket NutanixBuckets/john-smith-bucket Permission public User John-Smith-2 Policy public
    Setting policy readwrite public

  Buckets supports the following policies, which can be configured on a per user, per bucket basis:

    - **download** - Grants read only access to configured users.
    - **upload** - Grants write only access to configured users.
    - **public** - Grants read/write access to configured users.
    - **worm** - Enables write once, read many access. This supersedes all other policies.
    - **none** - Users have no access.

  View Bucket with Different Users Credentials
  ............................................

  In Cyberduck, notice that User 1’s bucket still does not show up in the directory listing. However, you can now navigate directly to the bucket.

  Click **Go > Go To Folder…**

  Type in the name of User 1's bucket and click **Go**.

  Verify you can now read and write to User 1's bucket.

..  +++++++++++++++++++++++++++++++++++++++++++++++

  While tools like Cyberduck help to visualize how data is access within an object store, Buckets is primarily an object store service that is designed to be accessed and consumed over S3 APIs.

  Amazon's S3 (Simple Storage Service) is the largest public cloud storage service, and has subsequently become the de-facto standard object storage API due to developer and ISV adoption. Buckets provides an S3 compliant interface to allow for maximum portability, as well as support for existing "cloud native" applications.

  In this exercise you will leverage ``s3cmd`` to access your buckets using the CLI.

  You will need the **Access Key** and **Secret Key** for the first user account created earlier in this lab.

  Setting up s3cmd (CLI)
  ......................

  From the *initials*-**Linux-ToolsVM**, run ``s3cmd --configure`` and enter the following to configure access to the Object Store:

  .. note::

    For anything not specified below, just hit enter to leave the defaults. Do **NOT** set an encryption password and do **NOT** use HTTPS protocol.

  .. code-block:: bash

    s3cmd --configure

  - **Access Key**  - *First User's Access Key*
  - **Secret Key**  - *First User's Secret Key*
  - **Default Region [US]**  - us-east-1
  - **S3 Endpoint [s3.amazonaws.com]**  - *OBJECT-STORE-IP*\ :7200
  - **DNS-style bucket+hostname:port template for accessing a bucket [%(bucket)s.s3.amazonaws.com]**  - *OBJECT-STORE-IP*
  - **Encryption password** - Leave Blank
  - **Path to GPG program [/usr/bin/gpg]**  - Leave Blank
  - **Use HTTPS protocol [Yes]**  - No
  - **HTTP Proxy server name**  - Leave Blank
  - **Test access with supplied credentials?**  - Y (Yes)

  The output should look similar to this and match your environment:

  .. code-block:: bash

    New settings:
      Access Key: Ke2hEtehmOZoXYCrQnzUn_2EDD9Eqf0L
      Secret Key: p6sxh_FhxEyIteslQJKfDlezKrtJro9C
      Default Region: us-east-1
      S3 Endpoint: 10.20.95.51:7200
      DNS-style bucket+hostname:port template for accessing a bucket: 10.20.95.51
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

  Type **Y** and press **Return** to save the configuration.

  Create A Bucket And Add Objects To It Using s3cmd (CLI)
  .......................................................

  Now lets use s3cmd to create a new bucket called *your-name*\ **-clibucket**.

  From the same Linux command line, run the following command:

  .. code-block:: bash

    s3cmd mb s3://xyz-cli-bob-bucket

  You should see the following output:

  .. code-block:: bash

    Bucket 's3://xyz-cli-bob-bucket/' created

  List your bucket with the **ls** command:

  .. code-block:: bash

    s3cmd ls

  You will see a list of all the buckets in the object-store.

  To see just your buckets run the following command:

  .. code-block:: bash

    s3cmd ls | grep *initials*

  Now that we have a new bucket, lets upload some data to it.

  If you do not already have the Sample-Pictures.zip, download it and copy to your Linux-ToolsVM.

  :download:`sample-pictures <https://s3.amazonaws.com/get-ahv-images/sample-pictures.zip>`

  .. code-block:: bash

    curl https://s3.amazonaws.com/get-ahv-images/sample-pictures.zip -o sample-pictures

  Run the following command to upload one of the images to your bucket:

  .. code-block:: bash

    s3cmd put --acl-public --guess-mime-type image01.jpg s3://<your-bucket-name>/image01.jpg

  You should see the following output:

  .. code-block:: bash

    s3://xyz-cli-bob-bucket/image01.jpg
    WARNING: Module python-magic is not available. Guessing MIME types based on file extensions.
    upload: 'image01.jpg' -> 's3://xyz-cli-bob-bucket/image01.jpg'  [1 of 1]
    1048576 of 1048576   100% in    7s   142.74 kB/s  done
    Public URL of the object is: http://10.20.95.51:7200/xyz-cli-bob-bucket/image01.jpg

  If desired, repeat with more images.

  Run the **la** command to list all objects in all buckets:

  .. code-block:: bash

    s3cmd la

  To see just objects in your buckets, run the following command:

  .. code-block:: bash

    s3cmd la | grep *initials*

Takeaways
+++++++++

What are the key things you should know about **Nutanix Objects**?

- Nutanix Objects provides a simple and scalable S3-compatible object storage solution, optimized for DevOps, Long Term Retention and Backup Target use cases.
