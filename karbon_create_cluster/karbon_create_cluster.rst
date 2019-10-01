.. _karbon_create_cluster:

---------------------
Karbon: Create Custer
---------------------

*The estimated time to complete this lab is 60 minutes.*

Overview
++++++++

Nutanix Karbon is an on-prem turnkey curated enterprise-grade Kubernetes service offering that simplifies the provisioning, operations and lifecycle management of Kubernetes.

Karbon provides a consumer-grade experience for delivering Kubernetes on-prem providing huge savings on OpEx of dedicated DevOps or SRE teams to keep Kubernetes online, up to date or integrated with 3rd party components and tooling.

**In this lab you will deploy a Kubernetes cluster using Karbon and then deploy multiple containers, referred to as Kubernetes pods, to run a sample application.**

If you already have an understanding of containers, Kubernetes, challenges, and use cases, jump to `Lab Setup`_ and `Creating a Karbon Cluster`_.\

Lab Setup
+++++++++

This lab requires applications provisioned as part of the :ref:`windows_tools_vm`.

If you have not yet deployed this VM, see the linked steps before proceeding with the lab.

It is **highly recommended** that you connect to the Tools VM using the Microsoft Remote Desktop client rather than the VM console launched via Prism. An RDP connection will allow you to copy and paste between your device and the VMs.

Creating a Karbon Cluster
+++++++++++++++++++++++++

In this exercise you will enable Karbon.

#. In **Prism Central**, select :fa:`bars` **> Services > Karbon**.

   .. figure:: images/karbon_create_cluster_0.png

   .. note::

     If Karbon has not already been enabled on your cluster, click the **Enable Karbon** button when prompted. Once clicked, the process should take approximately 2 minutes to complete. During this time Prism Central is deploying the Karbon control plane, which runs as a set of containers within the Prism Central VM.

     .. figure:: images/2.png

#. Click the provided link to launch the **Karbon Console**.

   .. note::

     If at any point your Karbon session times out, you can log in again using your Prism Central **admin** credentials.

#. Click **Download Centos** to download the required OS image to Karbon.

#. Once the download has completed, click **+ Create Kubernetes Cluster**.

#. On the **Recommended Configurations** tab, select **Development Cluster** and click **Next**.

   This will define a single worker node, master node, and etcd node.

   Worker nodes are responsible for running containers deployed onto the Kubernetes cluster. Each Worker node runs the `kubelet <https://kubernetes.io/docs/admin/kubelet/>`_ and `kube-proxy <https://kubernetes.io/docs/admin/kube-proxy/>`_ services.

   The Master node controls the Kubernetes cluster and provides the `kube-apiserver <https://kubernetes.io/docs/admin/kube-apiserver/>`_, `kube-controller-manager <https://kubernetes.io/docs/admin/kube-controller-manager/>`_. and `kube-scheduler <https://kubernetes.io/docs/admin/kube-scheduler/>`_ services.

  `etcd <https://coreos.com/etcd/>`_ nodes provide a distributed key-value store which Kubernetes uses to manage cluster state. In this lab, you will configure a single etcd node.

  .. figure:: images/karbon-configurations.png

#. On the **Name and Environment** tab, fill out the following fields:

   - **Name** - *Initials*-karbon
   - **Cluster** - Select *Your Nutanix cluster*
   - **Kubernetes Version** - 1.13.4 (Default)
   - **Host OS Image** - centos7.5.1804-ntnx-0.0 (Default)

   .. figure:: images/3.png

#. Click **Next**.

   Next you can view the resources defined for each type of container host VMs (worker, master, and etcd). If desired, you are able to edit the resource allocations by clicking **Edit** next to each type.

   For this lab, you can leave the default values.

#. Click **Next**.

   Next you will configure the networking for both the host VMs and pods. Karbon utilizes `Flannel <https://github.com/coreos/flannel#flannel>`_ to provide layer 3 IPv4 network between multiple nodes within the Karbon cluster.

   Platforms like Kubernetes assume that each pod (container) has a unique, routable IP inside the cluster. The advantage of this model is that it removes the port mapping complexities that come from sharing a single host IP.

   The **Service CIDR** defines the network range on which services (like etcd) are exposed. The **Pod CIDR** defines the network range used to IP pods. The default configuration allows for a maximum of 256 nodes with up to 256 pods per node.

#. On the **Network** tab, fill out the following fields:

   - **Network Provider** - Flannel (Default)
   - **Service CIDR** - 172.19.0.0/16 (Default)
   - **Pod CIDR** - 172.20.0.0/16 (Default)

   .. figure:: images/6.png

#. Click **Next**.

#. On the **Storage Class** tab, fill out the following fields:

   - **Storage Class Name** - default-storageclass-*Initials* (initials in lowercase)
   - **Prism Element Cluster** - *Your Nutanix Cluster IP*
   - **Nutanix Cluster Username** - admin
   - **Nutanix Cluster Password** - *Your Nutanix Cluster Password*
   - **Reclaim Policy** - Delete (Default)
   - **Storage Container Name** - Default
   - **File System** - ext4 (Default)

   .. figure:: images/7.png

#. Click **Create**.

   Deployment of the cluster should take approximately 10 minutes. During this time, Karbon is pulling images from public image repositories for the **master**, **etcd**, and **worker** nodes, as well as **flannel**, the Nutanix Volumes plugin, and any additional Karbon plugins. Support for authenticated proxy and dark site image repositories will be added post-GA.

   Filtering VMs for *Initials*\ **-karbon** in **Prism Central** will display the master, etcd, and worker VMs provisioned by Karbon.

   .. figure:: images/8.png

   In **Prism Element > Storage > Volume Group**, Karbon has created the **pvc-...** Volume Group, used as persistent storage for logging. Karbon leverages the Nutanix Kubernetes Volume Plug-In to present Nutanix Volumes to Kubernetes pods via iSCSI. This allows containers to take advantage of native Nutanix storage capabilities such as thin provisioning, zero suppression, compression, and more.

   .. figure:: images/9.png

   The Karbon cluster has finished provisioning when the **Status** of the cluster is **Healthy**.

   .. figure:: images/10.png

#. Click on your cluster name (*Initials*\ **-karbon**) to access the Summary Page for your cluster.

   .. figure:: images/11.png

#. Explore this view and note the ability to create and add additional storage classes and persistent storage volumes to the cluster.

   Additional persistent storage volumes could be leveraged for use cases such as containerized databases.

   In 15 minutes or less, you have deployed a production-ready Kubernetes cluster with logging (EFK), networking (flannel), and persistent storage services.

Getting Started with Kubectl
++++++++++++++++++++++++++++

`Kubectl <https://kubernetes.io/docs/reference/kubectl/overview/>`_ is the  command line interface for running commands against Kubernetes clusters. `Kubeconfig <https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/>`_ files contain information about clusters, users, namespaces, and authentication. The ``kubectl`` tool uses **kubeconfig** files to find and communicate with a Kubernetes cluster.

In this exercise you will use ``kubectl`` to perform basic operations against your newly provisioned Karbon cluster.

#. From within your *Initials*\ **-Windows-ToolsVM** VM, browse to **Prism Central** and open **Karbon**.

#. Select your *Initials*\ **-karbon** cluster and click **Actions > Download kubeconfig**.

   .. figure:: images/12.png

#. Click **Download**, then click **Close**.

#. Open **PowerShell**.

   .. note::

     If installed, you can also use a local instance of ``kubectl``. The Tools VM is provided to ensure a consistent experience.

     Instructions for setting up ``kubectl`` in Windows and macOS can be found `here <https://kubernetes.io/docs/tasks/tools/install-kubectl/>`_.

#. From PowerShell, run the following commands to configure ``kubectl``:

   .. code-block:: PowerShell

     cd ~
     mkdir .kube
     cd .kube
     mv ~\Downloads\*kubectl* ~\.kube\config
     kubectl get nodes

   .. note::

     By default, ``kubectl`` looks like a file named ``config`` in the ``~/.kube`` directory. Other locations can be specified using environment variables or by setting the ``--kubeconfig`` flag.

#. Verify that the output of the last command shows 1 master node and 1 worker node as **Ready**.

#. Next you will check the versions of the Kubernetes client and server by running the following command:

   .. code-block:: PowerShell

   	kubectl version

Takeaways
+++++++++

What are the key things you should know about **Nutanix Karbon**?

- Karbon is a great use case for internal development, CI/CD, digital transformation or application modernization initiatives
- Karbon is included in all AOS software editions at no additional cost.
- The primary benefit of Karbon is reduced CapEX and OpEX of managing and operating Kubernetes environments, reducing learning curve and enabling DevOps/ITOps teams to quickly support their development teams to start deploying containerized workloads.
