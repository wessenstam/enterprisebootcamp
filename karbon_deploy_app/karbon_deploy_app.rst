.. title:: Karbon Deploy Application

.. _karbon_deploy_app:

--------------------------
Karbon: Deploy Application
--------------------------

Overview
++++++++

Deploying an Application
++++++++++++++++++++++++

Now that you have successfully run commands against your Kubernetes cluster using ``kubectl``, you are now ready to deploy an application. In this exercise you will be deploying the popular open-source content management system used for websites and blogs, Wordpress.

#. Using *Initials*\ **-Windows-ToolsVM**, open **PowerShell** and create a **wordpress** directory using the following command:

   .. code-block:: PowerShell

   	mkdir ~\wordpress
   	cd ~\wordpress

   Kubernetes depends on YAML files to provision applications and define dependencies. YAML files are a human-readable text-based format for specifying configuration information. This application requires two YAML files to be stored in the **wordpress** directory.

   .. note::

     To learn more about Kubernetes application deployment and YAML files, click `here <https://www.mirantis.com/blog/introduction-to-yaml-creating-a-kubernetes-deployment/>`_.

#. Using your *Initials*\ **-Windows-ToolsVM** web browser, download the following YAML files for Wordpress and the MySQL deployment used by Wordpress:

   - https://raw.githubusercontent.com/nutanixworkshops/ts2019/master/karbon/mysql-deployment.yaml
   - https://raw.githubusercontent.com/nutanixworkshops/ts2019/master/karbon/wordpress-deployment.yaml

   Kubernetes depends on YAML files to provision applications and define dependencies. YAML files are a human-readable text-based format for specifying configuration information. This application requires two YAML files to be stored in the **wordpress** directory.

  .. note::

  If attempting to download the script results in an Access Denied error, log out of any AWS accounts from your browser or open the download link in **Incognito (Private Browsing)** mode.

#. Move both files to the **wordpress** directory using the following command:

   .. code-block:: PowerShell

   	mv ~\Downloads\*.yaml ~\wordpress\
   	cd ~\wordpress\

#. Open the **wordpress-deployment.yaml** file with your preferred text editor.

   .. note::

     **Sublime Text** has been pre-installed on *Initials*\ **-Windows-ToolsVM**.

   .. figure:: images/13.png

#. Under **spec: > type:**, change the value from **LoadBalancer** to **NodePort** and save the file. (Using a LoadBalancer is outside the scope of this lab.)

   .. figure:: images/14.png

   .. note::

     You can learn more about Kubernetes publishing service types `here <https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types>`_.

#. Open the **mysql-deployment.yaml** file and note that it requires an environmental variable to define the **MYSQL_ROOT_PASSWORD** as part of deployment. **No changes are required to this file.**

   .. figure:: images/14b.png

#. Define the **secret** to be used as the MySQL password by running the following command:

   .. code-block:: bash

   	kubectl create secret generic mysql-pass --from-literal=password=Nutanix/4u!

   Verify the command returns ``secret/mysql-pass created``.

   You can also verify the secret has been created by running the following command:

   .. code-block:: bash

   	kubectl get secrets

   Verify **mysql-pass** appears in the **NAME** column.

#. You will now provision the MySQL database by running the following command:

   .. code-block:: bash

   	kubectl create -f mysql-deployment.yaml.txt

   .. figure:: images/karbon_deploy_application_31.png

#. In addition to the MySQL service, the **mysql-deployment.yaml** also specifies that a persistent volume be created as part of the deployment. You can get additional details about the volume by running:

   .. code-block:: bash

   	kubectl get pvc

   You will note that the **STORAGECLASS** matches the **default-storageclass-**\ *Initials* provisioned by Karbon.

   The volume also appears in **Karbon** under *Initials*\ **-karbon > Volume**.

   .. figure:: images/16.png

#. To view all running pods on the cluster, which should currently only be your Wordpress MySQL database, run the following command:

   .. code-block:: bash

   	kubectl get pods

#. To complete the application, deploy Wordpress by running the following command:

   .. code-block:: bash

   	kubectl create -f wordpress-deployment.yaml.txt

#. Verify both pods are displayed as **Running** using ``kubectl get pods``.

Accessing Wordpress
+++++++++++++++++++

You have confirmed the Wordpress application and its MySQL database are running. Configuration of Wordpress is done via web interface, but to access the web interface you must first determine the IP addresses of our worker VMs and the port on which the pod is running.

#. The IP addresses of all cluster VMs is returned by the ``kubectl describe nodes`` command. You can run this and search for the **InternalIP** of any of your **worker** VMs, or run the following command to return only the hostnames and IP addresses:

   .. code-block:: PowerShell

   	kubectl describe nodes | Select-String -Pattern "Hostname:","InternalIP"

   .. figure:: images/17.png

#. To determine the port number of the Wordpress application, run the following command and note the TCP port mapped to port 80:

   .. code-block:: bash

   	kubectl get services wordpress

   .. figure:: images/18.png

#. Open \http://*WORKER-VM-IP:WORDPRESS SERVICE PORT*/ in a new browser tab to access to Wordpress installation.

   .. note::

     In the example shown, you would browse to http://10.21.78.72:32160. You environment will have a different IP and port.

   .. figure:: images/19.png

#. Click **Continue** and fill out the following fields:

   - **Site Title** - *Initials*\ 's Karbon Blog
   - **Username** - admin
   - **Password** - Nut@nixLabs19
   - **Your Email** - noreply@nutanix.com

#. Click **Install Wordpress**.

#. After setup completes (a few seconds), click **Log In** and provide the credentials just configured.

   Congratulations! Your Wordpress application and MySQL database setup is complete.

   .. figure:: images/20.png

Takeaways
+++++++++

What are the key things you should know about **Nutanix Karbon**?

- Karbon delivers One-Click operations for Kubernetes provisioning and lifecycle management, enabling enterprises to provide a private-cloud Kubernetes solution with the simplicity and performance of public clouds.
