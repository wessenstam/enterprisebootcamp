.. title:: Karbon Logging & Visualization

.. _karbon_logging_visual:

-------------------------------
Karbon: Logging & Visualization
-------------------------------

Overview
++++++++

Karbon provides a plug-in architecture to continually add additional functionality on top of vanilla Kubernetes. The firsts plug-ins Karbon provides are an integrated logging services stack called **EFK**, short for `Elasticsearch <https://github.com/elastic/elasticsearch>`_, `fluentd <https://www.fluentd.org/>`_ and `Kibana <https://github.com/elastic/kibana>`_; and monitoring and alerting with Prometheus.

Elasticsearch is a real-time, distributed, and scalable search engine which allows for full-text and structured search, as well as analytics. It is commonly used to index and search through large volumes of log data, but can also be used to search many different kinds of documents.

Elasticsearch is commonly deployed alongside Kibana, a powerful data visualization frontend and dashboard for Elasticsearch. Kibana allows you to explore your Elasticsearch log data through a web interface, and build dashboards and queries to quickly answer questions and gain insight into your Kubernetes applications.

Fluentd is a popular data collector that runs on all Kubernetes nodes to tail container log files, filter and transform the log data, and deliver it to the Elasticsearch cluster, where it will be indexed and stored.

Exploring Logging & Visualization
+++++++++++++++++++++++++++++++++

#. Return to the **Karbon Console** and select your *Initials*\ **-karbon** cluster.

#. Select **Add-on** from the sidebar to view and manage available Karbon plugins.

   .. figure:: images/21.png

#. Select **Logging** to launch the Kibana user interface.

#. Select **Discover** from the sidebar and define ``*`` as the **Index Pattern**.

   This wildcard will retrieve all available indices within Elastisearch, including **etcd**, **kubernetes**, and **systemd**.

   .. figure:: images/22.png

#. Click **Next Step**.

#. Select **@timestamp** from the **Time Filter field name** drop down menu to allow you to sort logging entries by their respective timestamps.

#. Click **Create index pattern**.

#. Select **Discover** again from the sidebar to view all logs from the Karbon cluster. You can reduce the amount of Kubernetes metadata displayed by adding the **log** entry under **Available Fields**.

   .. figure:: images/23.png

   Advanced Kibana usage, including time series data visualization that can answer questions such as "What is the difference in service error rates between our last 3 application upgrades," is covered in the `Kibana User Guide <https://www.elastic.co/guide/en/kibana/6.2/index.html>`_.

Takeaways
+++++++++

What are the key things you should know about **Nutanix Karbon**?

- Karbon can provide additional functionality to Kubernetes over time through its plugin architecture.

- Karbon is a certified Kubernetes distribution and has passed the `Kuberentes Conformance Certification <https://landscape.cncf.io/landscape=certified-kubernetes-hosted&selected=nutanix-karbon>`_.

- Karbon is listed on the official `Kubernetes Solutions <https://kubernetes.io/docs/setup/pick-right-solution/>`_ and `Cloud Native Computing Foundation Landscape <https://landscape.cncf.io/category=certified-kubernetes-hosted&selected=nutanix-karbon>`_ pages.
