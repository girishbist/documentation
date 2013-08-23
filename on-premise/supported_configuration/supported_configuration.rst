.. _supported_configuration:

Supported Configurations
------------------------

The following are supported configurations and scopes for Dell Multi-Cloud Manager (DMCM - formerly Enstratius)

.. _supported_configuration_os:

Operating System
~~~~~~~~~~~~~~~~

Regardless of the type (single node, multinode) of installation, DMCM is fully supported on the following operations systems only:

- ``Ubuntu 12.04 LTS amd64``
- ``CentOS 6.x x86_64``

Additionally the systems must be current with the latest upstream packages from the distribution vendor. A check will be done during installation to verify this and systems will be upgrade prior to installation.

Systems should have no additional configuration performed on them prior to installation and the distribution's "minimal" install options should be used. This includes customer-specific software. After the installation is validated, we can work with customers to assess the impact of customer-specific operating system configurations.

An account can be created for the customer service engineer as long as it meets the root requirements listed below.

RedHat Enterprise Linux
^^^^^^^^^^^^^^^^^^^^^^^

In place of ``CentOS 6x x86_64`` ``RHEL 6.x x86_64`` can be used with the following additional requirements:

- The system has a current and active RHN subscription
- If RedHat Satellite Server is used to manage the system, any required third-party repositories must be added to the satellite server or direct access to these repositories must be allowed from the servers.

Customers should be aware of the following when choosing to use RHEL over CentOS:

- No automated testing is done of DMCM on RHEL due to licensing
- Issues arising from the use of RHEL over CentOS are dealt with on a case-by-case basis

It is our belief that CentOS is an acceptable analog for customers that are more familiar with the RedHat ecosystem than the Debian/Ubuntu ecosystem.

Installation Types
------------------

Below are the support installation types

POC
~~~

POC installations are intended for demos, proof-of-concept or development/lab environments.

System Requirements
^^^^^^^^^^^^^^^^^^^

- 4 CPU (8 CPU recommended)
- 8GB Memory (16GB recommended)
- 100GB local disk
- Can be virtualized

In this configuration, all DMCM components and datastores run on a single system. There is no redundancy or high-availability for any components in this configuration. Installations under this configuration are not eligible for support outside of a POC engagement. There is no guarantee of data redundancy.

This configuration will support minor workloads with multiple cloud accounts and can be virtualized.

.. _supported_configuration_production:

Production
~~~~~~~~~~

There are two configuration considered production quality - :ref:`an installation using 9 nodes <supported_configuration_nine_nodes>` and :ref:`a fully redundant installation using 23 nodes <supported_configuration_23_nodes>`

.. _supported_configuration_nine_nodes:

9-node Production Installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The 9 node production installation is the smallest production-quality installation available.

System Requirements
===================

- 5 Riak nodes
    - 4 CPU
    - 8GB Memory
    - 50GB Local disk
    - Virtualization not recommended

- 2 MySQL nodes (master/slave configuration)
    - 4 CPU
    - 8 GB Memory (16GB recommended)
    - 50GB Local disk
    - Virtualization not recommended

- 2 DMCM nodes (hot/cold standby)
    - 8 CPU
    - 16 GB Memory
    - 50GB Local disk
    - Virtualization okay

In this configuration all DMCM components and RabbitMQ run on a single instance. In the event of a failure with the primary DMCM instance, it would be terminated and the second instance would replace it. It is up to the customer to ensure that traffic will flow to the secondary instance under the same DNS names as the primary instance. This can be accomplished in several ways:

- An external load balancer in front of the DMCM nodes. This will be provisioned by the customer service engineer performing the installation and will require an additional instance.
- A floating IP address or VIP that is bound to the running DMCM node.

Note that in this configuration running both DMCM nodes at the same time is **NOT** supported. If the customer is running the DMCM node virtualized, the process of terminating the primary node and bringing the secondary node will need to be manual. This process only applies to a total loss of failure of all services on the primary node.

Splitting services across nodes in this configuration is **NOT** supported.

.. _supported_configuration_23_nodes:

23-node Production Installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The 23 node production installation brings full redundancy to all components (excluding load balancing and scheduling components depending on environment). This is the minimum number of nodes for this configuration but it can grow by scaling specific components horizontally. This configuration very closely matches the configuration we have in the hosted version of DMCM.

System Requirements
===================

- 5 Riak nodes
    - 4 CPU
    - 8GB Memory
    - 50GB Local disk
    - Virtualization not recommended

- 2 MySQL nodes (master/slave configuration)
    - 4 CPU
    - 8 GB Memory (16GB recommended)
    - 50GB Local disk
    - Virtualization not recommended

- 1 Frontend Load Balancer
    - 4GB Memory
    - 2 CPU
    - 10GB Local Disk
    - Virtualization okay

- 1 KM Load Balancer
    - 4GB Memory
    - 2 CPU
    - 10GB Local Disk
    - Virtualization okay

- 1 Riak Load Balancer
    - 4GB Memory
    - 2 CPU
    - 10GB Local Disk
    - Virtualization okay

- 2 Console nodes (active/active)
    - 4G Memory
    - 2 CPU
    - 50GB Local disk
    - Virtualization okay

- 2 API nodes (active/active)
    - 4GB Memory
    - 2 CPU
    - 50GB Local disk
    - Virtualization okay

- 2 KM nodes (active/active)
    - 4GB Memory
    - 2 CPU
    - 50GB Local disk
    - Virtualization okay

- 2 Dispatcher nodes (active/standby)
    - 8GB Memory
    - 4 CPU
    - 50GB Local disk
    - Virtualization okay

- 2 Worker nodes
    - 8GB Memory
    - 4 CPU
    - 50GB Local disk
    - Virtualization okay

- 2 Monitor nodes
    - 8GB Memory
    - 4 CPU
    - 50GB Local disk
    - Virtualization okay

- 1 Publisher Queue node
    - 8GB Memory
    - 2 CPU
    - 50GB Local disk
    - Virtualization okay

In this configuration all horizontally scalable components have been provided at least one additional instance. The following components are horizontally scalable and support either load balancing or running multiple instances.

- Workers
- Monitors
- KM (with load balancer)
- Console (with load balancer)
- API (with load balancer)

The following subsystem supports multiple instances running but only one can actively service traffic:

- Dispatcher

The following subsystem supports only one running instance and requires external coordination to ensure that only one instance is running at a time:

- Publisher Queue

Root Access
-----------

Root access is required during the installation for the customer service engineer performing the installation. The installation is done as root. Once the installation is complete root access can be revoked for the engineer.

When engaging support from a customer service engineer, root access will be required for troubleshooting.

It is not neccessary to provide the customer service engineer with the root password. If sudo is in use on the system (and sudo is one of the required packages), the engineer will need to be able to run ``sudo su -l`` and gain a full root shell.

Remote Installations
--------------------

Installation can be done remotely however installation over remote desktop solutions (e.g. RDP, VNC, GoToMeeting, WebEx, Citrix) is not supported. The engineer will need direct SSH access to the systems as well as HTTPS access to the console post-installation.

SSH and HTTPS access can be provided over the internet, restricted from a dedicated VPN source address or over a customer VPN.

Customer VPN
------------

If providing the engineer with VPN access for the installation, we will provide you with a list of users who need VPN access. VPN access should meet the following criteria:

- Access to external resources (e.g. websites, skype) must **not** be restricted as this is how our team engages internal support systems.
- Access to internal resources (e.g. the systems being used for installation) can be restricted to the systems used for installation provided the SSH and HTTPS requirements are met.

.. note::
    "Remote Application"-style VPNs such as those from Citrix are **not** supported. The engineer will need to use tooling on her local system connected to the VPN to interact with the installation targets.

.. _supported_configuration_network:

Network Access
--------------

All DMCM components must have unrestricted access to communicate between them. Additionally, access between DMCM and managed instances is required if the agent or automation is being used.

Internet access during installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

DMCM requires full unrestricted access to the internet during installation. This is for several reasons:

- Installation pulls required dependencies transitively from third-part distribution repositories
- Installation automatically updates the system to the most current revision of the distribution supported (i.e. Ubuntu 12.04 will be brought current but it will **not** be upgraded to Ubuntu 13.04)

This access can be done through a proxy but it is recommended that the proxy access for these systems not require a username or password.

Internet access after installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Internet access for the sysems can be revoked after installation as long as DMCM has no need to communicate with the Internet for cloud access (i.e. access to AWS cloud endpoints)

.. _supported_configuration_lb:

Load Balancing
--------------
Customer-provided load balancers are ONLY supported for front-end services (console, api and dispatcher). As we do not have access to all loadbalancers, we will need to work closely with the team responsible for the load balancer on the customer side. We will provide general guidelines as to the configuration but it will be up to the responsible person on the customer side to translate that to the appropriate load balancer configuration.

Load balancing for KM and Riak will be provisioned by the DMCM customer service engineer. Ensuring that these load balancers are redundant depends entirely on the customer environment. The customer service engineer can provision multiple instances of load balancers but only one should service traffic at a time. These must be reachable by the same DNS name used in the registration process regardless of which load balancer is active.

Hearbeating
-----------
Hearbeating software (such as Linux-HA) in conjunction with a floating IP address can be utilized for singleton systems on in the the event that there is a reliable STONITH method and heartbeat path.

.. _supported_configuration_virtualization:

Virtualization
--------------
Note that in the 9 and 23 node configurations HA only extends as far as the hypervisor. The assumption is that no HA components will be running on the same underlying hypervisor. For instance, if both console nodes are on the same hypervisor there are no guarantees that console services will be available should that hypervisor fail.

DMCM uses the network very heavily and as such there are points of diminishing returns based on the network capacity between hypervisor nodes. It is up to the end user to ensure that there are not only redundant network paths between hypervisors but also enough capacity for DMCM traffic between components.

Heartbeats over virtualized networks are **not** supported.

Running DMCM on the same platform it is managing is **not** supported. For example, if DMCM is managing AWS resources then DMCM should not be installed on AWS. If AWS becomes unavailable DMCM cannot manage resources in AWS and the user will be unable to access DMCM (to migrate workloads between clouds) because AWS is unavailable.

Additionally if the user is running DMCM in a public cloud, system sizing requirements may need to be increased to account for the realities of a multi-tenant public cloud (i.e. "noisy neighbor syndrome"). Some public clouds may also not provide full functionality to meet the HA requirements of DMCM. When running in a public cloud, all components must be in the same "data center" (i.e. us-west-2a if using AWS).

Running DMCM in public clouds for production configurations will require additional review and may have additional requirements and recommendations.

.. danger:: Live Motion/Migration

   If you run DMCM virtualized, be aware that LiveMotion/migration is not supported on ANY DMCM instance or database instance. The services must be stopped and the instance taken fully offline before instances are transfered between hypervisors.

Multi Data Center Redundancy
----------------------------
There is no supported configuration for multi datacenter installations of DMCM. Only dedicated DMCM installations are supported in each datacetner. Additionally no two DMCM installations can point to the same cloud account. This means that two full DMCM installations in different datacenters cannot talk to a cloud endpoint with the same set of credentials but they can talk to the same cloud endpoint with two different sets of credentials **provided** they see different sets of resources.

Support Contracts
-----------------
Only the 9 and 23+ node configurations are allowed for any DMCM support contract.

.. toctree::
   :maxdepth: 2
   :hidden:
