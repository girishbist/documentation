.. _post_install:

Post Install
------------

.. _register:

Register
~~~~~~~~

In your browser, navigate to https://cloud.mycompany.com/page/1/register.jsp (https://cloud.mycompany.com/pages/public/register.jsp if using version h.4.2+),
replacing cloud.mycompany.com with the url you specified in in your attributes file.

.. note:: You may have to modify your DNS settings via a hosts file entry or other means
   to reach the URL.

Successful registration will direct the user to a page where cloud credentials can be
entered. Before proceeding, let's start the worker and monitor services.

.. figure:: ./images/post_install_register.png
   :height: 700px
   :width: 900 px
   :scale: 55 %
   :alt: Registration
   :align: center

   Registration

Login
~~~~~~~~

.. figure:: ./images/post_install_login.png
   :height: 700px
   :width: 900 px
   :scale: 55 %
   :alt: Login
   :align: center

   Login

Add a Cloud
~~~~~~~~~~~

.. figure:: ./images/post_install_add_cloud.png
   :height: 700px
   :width: 900 px
   :scale: 55 %
   :alt: Add
   :align: center

   Add a Cloud

List of Clouds
~~~~~~~~~~~~~~

.. raw:: html
	
	<a href="./images/post_install_aws.png">Amazon Web Services</a><br />
	<a href="./images/post_install_att.png">AT&T Synaptic Cloud</a><br />
	<a href="./images/post_install_azure.png">Azure</a><br />
	<a href="./images/post_install_bluelock.png">Bluelock vCloud Director</a><br />
	<a href="./images/post_install_cloudcentral.png">CloudCentral</a><br />
	<a href="./images/post_install_cloudsigma_lv.png">CloudSigma Las Vegas</a><br />
	<a href="./images/post_install_cloudsigma_z.png">CloudSigma Zurich</a><br />
	<a href="./images/post_install_gogrid.png">GoGrid</a><br />
	<a href="./images/post_install_google.png">Google</a><br />
	<a href="./images/post_install_hp.png">HP Cloud</a><br />
	<a href="./images/post_install_ibm.png">IBM SmartCloud Enterprise</a><br />
	<a href="./images/post_install_joyent.png">Joyent</a><br />
	<a href="./images/post_install_kt.png">KT ucloud</a><br />
	<a href="./images/post_install_opsource.png">OpSource</a><br />
	<a href="./images/post_install_rackspace.png">Rackspace NG</a><br />
	<a href="./images/post_install_serverexpress.png">ServerExpress</a><br />
	<a href="./images/post_install_tata.png">Tata InstaCompute</a><br />
	<a href="./images/post_install_terremark.png">Terremark Enterprise Cloud</a><br />

    
MySQL
~~~~~

The root password for MySQL was generated at the time of installation. The password will
be located in ``/etc/mysql/grants.sql`` on Debian derivatives and
``/etc/mysql_grants.sql`` on RHEL and CentOS.
