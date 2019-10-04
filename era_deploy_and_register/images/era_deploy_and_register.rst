.. _era_deploy_and_register:

------------------------
Era: Deploy and Register
------------------------

Overview
++++++++

.. note::

  Estimated time to complete: **15 MINUTES**

In this exercise you will deploy Nutanix Era and register your Nutanix cluster.

Nutanix Era is a software suite that automates and simplifies database administration – bringing One Click simplicity and invisible operations to database provisioning and lifecycle management (LCM).

With One Click database provisioning and Copy Data Management (CDM) as its first services, Nutanix Era enables DBAs to provision, clone, refresh, and backup their databases to any point in time. Line of business applications in every vertical depend on databases, providing use cases in both production and non-production environments.

**In this lab you will explore how Era can be used to standardize database deployment, allow for rapid cloning from a production database to a clone used for application development, updating that clone based on production data, and finally leveraging the REST API to understand how Era can integrate with a customer's existing automation tools.**

Deploying Era
+++++++++++++

Era is distributed as a virtual appliance that can be installed on either AHV or ESXi. In this lab you will deploy Era to your AHV cluster.

In **Prism Central**, select :fa:`bars` **> Virtual Infrastructure > VMs**.

.. figure:: images/2a.png

Click **Create VM**.

Fill out the following fields:

- **Name** - Era-*Initials*
- **Description** - (Optional) Description for your VM.
- **vCPU(s)** - 4
- **Number of Cores per vCPU** - 1
- **Memory** - 16 GiB

- Select **+ Add New Disk**
    - **Type** - DISK
    - **Operation** - Clone from Image Service
    - **Image** - Era.qcow2
    - Select **Add**

- Select **Add New NIC**
    - **VLAN Name** - Primary
    - Select **Add**

Click **Save** to create the VM.

Select your Era VM and click **Power On**.

.. figure:: images/2b.png

Registering a Cluster
+++++++++++++++++++++

In **Prism Central > VMs > List**, identify the IP address assigned to your Era VM using the **IP Addresses** column.

Open \https://*ERA-VM-IP:8443*/ in a new browser tab.

.. note::

  It may take up to 2 minutes for the Era interface to initialize after booting the VM.

Select **I have read and agree to terms and conditions** and click **Continue**.

Enter **techX2019!** as the **admin** password and click **Set Password**.

Login using the following credentials:

- **Username** - admin
- **Password** - techX2019!

On the **Welcome to Era** page, fill in the following information:

- **Name** - *Your Cluster Name*
- **Description** - (Optional) Description
- **Address** - *Your Prism Element Cluster IP*
- **Prism Element Administrator** - admin
- **Password** - techX2019!

.. figure:: images/3b.png

.. note::

  Era requires a Prism Element account with full administrator access. For ESXi clusters, vCenter must also be registered with Prism Element.

Click **Next**.

Select the **Default** storage container and click **Next**.

.. figure:: images/3c.png

Select the **Primary** VLAN. This is the default network profile that Era will use when provisioning new databases. Do **not** select **Manage IP Address Pool**, as your AHV cluster already has IPAM (DHCP) configured for that network.

.. figure:: images/3d.png

Click **Next**.

Once Era setup has completed, click **Get Started**.

.. figure:: images/3e.png

Takeaways
++++++++++
