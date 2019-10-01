.. _era_provision_postgresdb:

--------------------------
Era: Provision Postgres DB
--------------------------

Overview
++++++++

.. note::

  Estimated time to complete: **30 MINUTES**

This lab will show you how to provision, connect, and view a Postgres Database.

Provisioning a PostgreSQL Database
++++++++++++++++++++++++++++++++++

The initial release of Era supports the following Operating Systems and Database Servers:

- CentOS 6.9, 7.2, and 7.3
- Oracle Linux 7.3
- RHEL 6.9, 7.2, and 7.3
- Windows Server 2012, Windows Server 2012 R2, and Windows Server 2016
- Oracle 11.2.0.4.x, 12.1.0.2.x, and 12.2.0.1.x
- PostgreSQL 9.x and 10.x
- SQL Server 2008 R2, SQL Server 2012, SQL Server 2014, SQL Server 2016, and SQL Server 2017

Era can be used to provision database servers and databases on the registered Nutanix cluster, or you can register an existing source database running on the cluster. In this lab, you will provision a new PostgreSQL database server and database.

Era makes it even simpler to provision a simple PostgreSQL database by providing sample profiles for **Software**, **Compute**, and **Database Parameters**. You will explore each of these profiles to understand how they are configured.

#. Select the **Era > Getting Started** drop down menu and click **Profiles**.

   .. figure:: images/3g.png

#. Select **Software** and note there are included profiles for **PostgreSQL 10.4** and **MariaDB 10.3** shipped with Era.

   Additional PostgreSQL, MariaDB, SQL Server, and Oracle profiles can be created by registering database server VMs with Era.

#. Select **Compute > DEFAULT_OOB_COMPUTE** and note the default Compute Profile creates a 4 core, 32GiB RAM VM to host the database. To reduce memory consumption in the shared lab environment, you will create a custom Compute Profile.

#. Click **+ Create** and fill out the following fields:

   - **Name** - Lab
   - **Description** - Lab Compute Profile
   - **vCPUs** - 1
   - **Cores per CPU** - 2
   - **Memory (GiB)** - 16

   .. figure:: images/3f2.png

#. Click **Create**.

#. Select **Database Parameters > DEFAULT_POSTGRES_PARAMS** and note the default parameters for a PostgreSQL database provisioned by Era.

#. Select the **Era > Profiles** drop down menu and click **Getting Started**.

#. On the **Getting Started** page, click the **PostgreSQL** button under **Provision a Database**.

   .. figure:: images/4b2.png

#. Click **Provision a Database**.

   .. figure:: images/4c.png

#. Select the **PostgreSQL** engine and click **Next**.

#. Fill out the following **Database Server** fields:

   - **Database Server** - Select **Create New Server**
   - **Database Server Name** - *Initials*-DBServer
   - **Compute Profile** - Lab
   - **Network Profile** - DEFAULT_OOB_NETWORK
   - **Software Profile** - POSTGRES_10.4_OOB
   - **Description** - (Optional) Description
   - **SSH Public Key for Node Access** -

   .. code-block:: text

     ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoQRdEfm8ZJNGlYLQ2iw08eVk/Wyj0zl3M5KyqKmBTpUaS1uxj0K05HMHaUNP+AeJ63Qa2hI1RJHBJOnV7Dx28/yN7ymQpvO1jWejv/AT/yasC9ayiIT1rCrpHvEDXH9ee0NZ3Dtv91R+8kDEQaUfJLYa5X97+jPMVFC7fWK5PqZRzx+N0bh1izSf8PW0snk3t13DYovHFtlTpzVaYRec/XfgHF9j0032vQDK3svfQqCVzT02NXeEyksLbRfGJwl3UsA1ujQdPgalil0RyyWzCMIabVofz+Czq4zFDFjX+ZPQKZr94/h/6RMBRyWFY5CsUVvw8f+Rq6kW+VTYMvvkv

   .. note::

     The above SSH public key is provided as an example and is configured as an authorized key for the operating system provisioned by Era. In a non-lab setting you would create your own SSH private/public keypair and provide the public key during this step.

   .. figure:: images/4d2.png

#. Click **Next**.

#. Fill out the following **Database** fields:

   - **Database Name** - *Initials*\_LabDB
   - **Description** - (Optional) Description
   - **POSTGRES Password** - nutanix/4u
   - **Database Parameter Profile** - DEFAULT_POSTGRES_PARAMS
   - **Listener Port** - 5432
   - **Size (GiB)** - 200

   .. note::

     Era also offers to ability to run scripts or commands both before and after database creation . These can be used to further customize an environment based on specific enterprise needs.

   .. figure:: images/4e2.png

#. Click **Next**.

#. Fill out the following **Time Machine** fields:

   - **Name** - *Initials*\_LabDB_TM
   - **Description** - (Optional) Description
   - **SLA** - Gold
   - **Schedule** - Default

   .. figure:: images/4f2.png

#. Click **Provision**.

#. Click **Operations** in the upper right-hand corner to view the provisioning progress. Provisioning should take approximately 5 minutes.

   .. note::

     All operations within Era have unique IDs are fully visible for logging/auditing.

   .. figure:: images/4g2.png

#. Upon completion, select **Dashboard** from the drop down menu and note your new **Source Database**.

   .. figure:: images/4i2.png

   You should also be able to see the *Initials*-**DBServer** VM running within Prism.

Connecting to the Database
++++++++++++++++++++++++++

Now that Era has successfully provisioned a database instance, you will connect to the instance and verify the database was created.

#. Select **Era > Databases** from the drop down menu.

#. Under **Sources**, click the name of your database.

   .. figure:: images/5a2.png

#. Note the IP Address of your **Database Server**.

   .. figure:: images/5b.png

#. Using *Initials*\ **-Windows-ToolsVM**, open **pgAdmin**.

   .. note::

     If installed, you can also use a local instance of pgAdmin. The Tools VM is provided to ensure a consistent experience.

#. Under **Browser**, right-click **Servers** and select **Create > Server...**.

   .. figure:: images/5c.png

#. On the **General** tab, provide your database server name (e.g. *Initials*-**DBServer**).

#. On the **Connection** tab, fill out the following fields:

   - **Hostname/IP Address** - *Initials*-DBServer IP Address
   - **Port** - 5432
   - **Maintenance Database** - postgres
   - **Username** - postgres
   - **Password** - nutanix/4u

   .. figure:: images/5d2.png

#. Expand *Initials*\ **-DBServer > Databases** and note an empty database has been created by Era.

   .. figure:: images/5h2.png

..  Now you will create a table to store data regarding Names and Ages.

  Expand *Initials*\_**labdb** **> Schemas > public**. Right-click on **Tables** and select **Create > Table**.

  .. figure:: images/5e.png

  On the **General** tab, enter **table1** as the **Name**.

  On the **Columns** tab, click **+** and fill out the following fields:

  - **Name** - Id
  - **Data type** - integer
  - **Primary key?** - Yes

  Click **+** and fill out the following fields:

  - **Name** - Name
  - **Data type** - text
  - **Primary key?** - No

  Click **+** and fill out the following fields:

  - **Name** - Age
  - **Data type** - integer
  - **Primary key?** - No

  .. figure:: images/5f.png

  Click **Save**.

  Using your **Tools VM**, open the following link to download a .CSV file containing data for your database table: http://ntnx.tips/EraTableData

  Using **pgAdmin**, right-click **table1** and select **Import/Export**.

  Toggle the **Import/Export** button to **Import** and fill out the following fields:

  - **Filename** - C:\\Users\\Nutanix\\Downloads\\table1data.csv
  - **Format** - csv

  .. figure:: images/5g.png

  Click **OK**.

  You can view the imported data by right-clicking **table1** and selecting **View/Edit Data > All Rows**.

  Takeaways
  +++++++++

  - Era 1.0 supports Oracle, SQL Server, and PostgreSQL. MySQL will be supported in an upcoming release.

  - Era supports One Click operations for registering, provisioning, cloning and refreshing supported databases.

  - Era enables the same type of simplicity and operating efficiency that you would expect from a public cloud while allowing DBAs to maintain control.

  - Era automates complex database operations – slashing both DBA time and the cost of managing databases with traditional technologies and saving immensely on enterprise OpEx.

  - Era enables database admins to standardize their database deployments across database engines and automatically incorporate database best practices.
