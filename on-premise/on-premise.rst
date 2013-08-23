.. Enstratius documentation master file, created by
   sphinx-quickstart on Mon Mar 12 21:46:44 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


On-Premise
==========

On-Premise installations of DMCM make sense for many reasons, and, due to the
flexibility of the DMCM architecture, you can deploy Enstratius as a single-node
personal development environment, single node POC, or a fully scaled high-availability cloud
management solution.

Developers
----------

.. image:: ./images/developers.png
   :height: 232 px
   :width: 922 px
   :scale: 85 %
   :alt: Developers
   :align: center

Setting up a development DMCM environment or a single-node DMCM environment for
sales/demonstration purposes just got a whole lot simpler. By leveraging vagrant, you can
install the full Enstratius cloud management software stack in less than 15 minutes,
depending on your network speed.


POC
---

.. image:: ./images/poc.png
   :height: 190 px
   :width: 869 px
   :scale: 85 %
   :alt: Customers
   :align: center

Installing DMCM cloud management software as part of a POC can be done by leveraging
a configuration management engine like Chef from Opscode.

POC installations are always performed on a single node.

Production
----------

DMCM can also be deployed in a High-Availability production architecture to support
hundreds of users and cloud accounts. There are two supported architectures for production installations:

- 9 nodes
- 23 node HA

.. image:: ./images/multinodes.png
   :scale: 50%
   :alt: 9 and 23 node configuration
   :align: center

Additional Information
----------------------
Please see the :ref:`"Supported Configurations" guide <supported_configuration>` for more details on these configurations.

.. toctree::
   :maxdepth: 3
   :hidden:
   :glob:

   architecture/architecture
   requirements/requirements
   installation/installation
   administration/administration
   help/help
   downloads
