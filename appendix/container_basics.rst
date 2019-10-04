.. title:: Container Basics

----------------
Container Basics
----------------

What are Containers?
....................

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. Unlike traditional virtualization, containers are substantially more light-weight than VMs, with many containers capable of sharing the same host OS instance.

.. figure:: images/overview1.png

Containers provide two key features: a packaging mechanism and a runtime environment.

At the runtime level, the container allows an application to run as an isolated process with its own view of the operating system. While VMs provide isolation via virtualized hardware, containers leverage the ability of the Linux kernel to provide isolated namespaces for individual processes. This lightweight nature means each application gets its own container, preventing dependency conflicts.

As a packaging mechanism, a container is typically just a tarball: a way to bundle the code, configuration and dependencies of an application into a single file. This eliminates the problem of “It worked on my environment, why doesn’t it work on yours,” because everything necessary to run the application consistently is transported with the container. Ideally, applications produce the same output regardless of environment, and containerization makes that ideal a lot easier to reach. The result is a containerized application that will start, stop, make requests and log the same way.

Container Benefits and Issues
.............................

For any business, containers represent a large opportunity:

- Developers will spend less time debugging environment issues and more time writing code. 
- Server bills will shrink, because more applications can fit on a server using containers than in traditional deployments. 
- Containers can run anywhere, increasing the available deployment options. For complex applications consisting of multiple components, containers vastly simplify updates. Placing each component in a container makes it simple to make changes without having to worry about unintended interactions with other components.

Use cases for containerized workloads include:

- Continuous Integration Continuous Delivery (CI/CD) development
- Application modernization/Microservices
- API, web, and backend app development/delivery
- Application cost containment
- Enabling hybrid cloud

While breaking down applications into microservices, or discrete functional parts, has clear benefits, having more parts to manage can introduce complexities for configuration, service discovery, load balancing, resource scaling, and discovering and fixing failures. Managing this complexity manually isn't scalable.

Introducing Kubernetes
......................

`Kubernetes <https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/>`_ is an container orchestration platform, open sourced by Google in 2014, that helps manage distributed, containerized applications at massive scale. According to Redmonk, 54 %of Fortune 100 companies are running Kubernetes in some form, with adoption coming from every sector.

.. figure:: images/overview2.jpg

Kubernetes delivers production-grade container orchestration, automating container configuration, simplifying scaling, lifecycle management, and managing resource allocation. Kubernetes can run anywhere. Whether you want your infrastructure to run on-premise, on a public cloud, or a hybrid configuration of both.

However, vanilla Kubernetes presents its own challenges that make it difficult for organizations to adopt the technology. Building your own production-ready deployment that ensures the Kubernetes environment itself is robust, easy to maintain (automated), with integrated features for things like networking, logging, analytics, and secret management, could take months.

Kubernetes aggressive, quarterly release cycle (where releases are deprecated by the community after 3 quarters) can create adoption challenges for enterprises. Finally, from a supportability and risk perspective, maintaining your own custom Kubernetes stack for production applications would be analogous to using a custom-made distribution of Linux - virtually unheard of in the enterprise.

As previously stated, Nutanix Karbon provides a turn-key solution to address these critical Kubernetes challenges.

Additional Kubernetes Training Resources
++++++++++++++++++++++++++++++++++++++++

- `Introduction to Kubernetes <https://www.edx.org/course/introduction-to-kubernetes>`_ - Free introductory training by The Linux Foundation

- `Play with Kubernetes <https://training.play-with-kubernetes.com/>`_ - Free introductory training and lab environment by Docker

- `Scalable Microservices with Kubernetes <https://www.udacity.com/course/scalable-microservices-with-kubernetes--ud615>`_ - Free intermediate training by Google
