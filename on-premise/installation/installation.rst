.. _installation:

Installation
------------

.. warning::
    All production DMCM installation will be performed by the customer service engineer. Self-service installations are not supported for production environments. These instructions are a general overview of installation requirements.

Getting Started
~~~~~~~~~~~~~~~

Before installation can begin, the customer service engineer will require certain information to configure the setup process.


IP Addressing
^^^^^^^^^^^^^

IP addresses/hostnames for all systems in the installation will be required up front before installation begins.

DNS
^^^

A properly configured, functioning and reliable DNS system is required for DMCM. All DNS entries must be prepared in advance and populated **before** installation begins.

For the installation, you will have to choose a domain name for the DMCM console.
Assuming you were to use the name ``cloud.mycompany.com``, it must be resolvable from any host trying to connect to it. DMCM will always redirect any traffic it recieves on any hostnames to the configured name via HTTPS.

Changing this name **after** the installation is done will require the interaction of a customer service engineer.

DNS Aliases
^^^^^^^^^^^
For failover purposes or planning for future migration from a 9-node to 23-node configuration, you may want to prep ADDITIONAL dns entries for services that would relocate to distinct systems.
The following is a list of public-facing and internal communication paths that might require a DNS alias:

- console (public-facing)
- api (public-facing)
- dispatcher (public-facing)
- km (internal)
- riak (internal)
- mysql (internal)
- rabbitmq (internal)

Using the above list you can use the following example mapping patterns as namings for components:

.. tabularcolumns:: |l|l|p{5cm}|p{5cm}|

+--------------------------------+--------------+-------------------+--------------------------+
| Name                           | component    |  9-node mapping   | 23-node mapping          |
+========+=======================+==============+===================+==========================+
| ``cloud.mycompany.com``        | console      | active DMCM node  | "frontend" load balancer |
+--------------------------------+--------------+-------------------+--------------------------+
| ``api.mycompany.com``          | api          | active DMCM node  | "frontend" load balancer |
+--------------------------------+--------------+-------------------+--------------------------+
| ``provisioning.mycompany.com`` | dispatcher   | active DMCM node  | "frontend" load balancer |
+--------------------------------+--------------+-------------------+--------------------------+
| ``km.mycompany.com``           | km           | 127.0.0.1         | km load balancer         |
+--------------------------------+--------------+-------------------+--------------------------+
| ``riak.mycompany.com``         | riak cluster | 127.0.0.1         | riak load balancer       |
+--------------------------------+--------------+-------------------+--------------------------+
| ``mysql.mycompany.com``        | mysql master | mysql master node | mysql master node        |
+--------------------------------+--------------+-------------------+--------------------------+
| ``mq.mycompany.com``           | rabbitmq     | 127.0.0.1         | Publisher Queue node     |
+--------------------------------+--------------+-------------------+--------------------------+

Note that in the 9-node configuration, certain components point internally as all DMCM components are running on the same system and a local loadbalancer is used to communicate with the riak cluster

Note that for "frontend" services, the same name can be used for all three services as they all listen on different ports. However if you want to plan ahead, you should create distinct entries for them (even if they are CNAMES)

Split DNS can be used such that lookups from DMCM components for communication with other DMCM components (i.e. Console or API to dispatcher) return a private internal address but a different address for public communications (agent/agent proxy to dispatcher)

SSL
^^^

A self-signed SSL cert will automatically be generated during installation and populated with the name provider as the "console" name. If you wish to use your own certificate, it must be provided in PEM form as single file with the decrypted key for the primary cert and all intermediary certs in the following order (from top to bottom):

- certificate for ``cloud.mycompany.com``
- RSA password-free private key for previous cert
- CA Certificate for the CA that signed ``cloud.mycompany.com`` (including intermediaries)

The means your the pem you provide will look something like this::

        -----BEGIN CERTIFICATE-----
        (cert contents)
        -----END CERTIFICATE-----
        -----BEGIN RSA PRIVATE KEY-----
        (key contents)
        -----END RSA PRIVATE KEY-----
        -----BEGIN CERTIFICATE-----
        (CA contents)
        -----END CERTIFICATE-----

Load Balancers
^^^^^^^^^^^^^^

In the event that the customer service engineer is provisioning a "frontend" load balancer, configuration of that load balancer will be handled for you.

If you wish to use your own load balancer, first be aware of the related support issues listed :ref:`here <supported_configuration_lb>`.

As we cannot test on every possible load balancer we might encounter, we can only provide configuration guidelines that would need to be translated by a customer subject matter expert on the load balancer product.

Regardless of WHICH load balancer is used, SSL offloading is HIGHLY recommended as it allows for more intelligent load balancing algorithims based on session ids.

Frontend Load Balancing
#######################

The following guidelines are for public facing services:

- Console

  - Redirect any public inbound unencrypted traffic on HTTP port 80 to HTTPS port 443
  - Terminate SSL at loadbalancer on HTTPS port 443
  - Communicate with console backends on HTTP port 8080 (note: unencrypted)
  - Remove the following headers if present on inbound traffic

    - ``X-Forwarded-Proto``
    - ``X-Forwarded-For``

  - Inject the following new headers

    - ``X-Forwarded-Proto: https``
    - ``X-Forwarded-For``

  - Sticky sessions to backends based on the ``JSESSIONID`` cookie

- API

  - Redirect any public inbound unencrypted traffic on HTTP port 15000 to HTTPS port 15443
  - Terminate SSL at loadbalancer on HTTPS port 15443
  - Communicate with api backends on HTTP port 15000 (note: unencrypted)
  - Remove the following headers if present on inbound traffic

    - ``X-Forwarded-Proto``
    - ``X-Forwarded-For``

  - Inject the following new headers

    - ``X-Forwarded-Proto: https``
    - ``X-Forwarded-For``

  - Sticky sessions not required. Use a ``least connections`` algorithim.

- Dispatcher

  - Terminate SSL at loadbalancer on HTTPS port 3302
  - Communicate with dispatcher backend on HTTP port 8081
  - Load balancing should be done in an active/backup (or standby) mode
  - Only one dispatcher should be recieving the traffic.

Other load balancing
####################

Riak and KM traffic will be handled by a load balancer provisioned by the customer service engineer. KM traffic will be load balanced in L4 mode so keys are never sent in the clear across the network. Riak does not support encrypted traffic at this time.


Web Browser
~~~~~~~~~~~

DMCM supports all modern web browsers. Here is a short list of support browsers.

* MSIE >= 8
* Chrome
* Firefox
* Safari

DMCM relies on javascript, so make sure it is enabled along with cookies.

For windows and MSIE users, here is a document discussing :ref:`MSIE compatibility mode <msie>`


.. toctree::
   :maxdepth: 2
   :hidden:

   centos/centos
   vagrant/vagrant
   single_node/single_node
   single_node/post_install
