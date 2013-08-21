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

| :download:`Amazon Web Services <./images/post_install_aws.png>`
| :download:`AT&T Synaptic Cloud <./images/post_install_att.png>`
| :download:`Azure <./images/post_install_azure.png>`
| :download:`Bluelock vCloud Director <./images/post_install_bluelock.png>`
| :download:`CloudCentral <./images/post_install_cloudcentral.png>`
| :download:`CloudSigma Las Vegas <./images/post_install_cloudsigma_lv.png>`
| :download:`CloudSigma Zurich <./images/post_install_cloudsigma_z.png>`
| :download:`GoGrid <./images/post_install_gogrid.png>`
| :download:`Google <./images/post_install_google.png>`
| :download:`HP Cloud <./images/post_install_hp.png>`
| :download:`IBM SmartCloud Enterprise <./images/post_install_ibm.png>`
| :download:`Joyent <./images/post_install_joyent.png>`
| :download:`KT ucloud <./images/post_install_kt.png>`
| :download:`OpSource <./images/post_install_opsource.png>`
| :download:`Rackspace NG <./images/post_install_rackspace.png>`
| :download:`ServerExpress <./images/post_install_serverexpress.png>`
| :download:`Tata InstaCompute <./images/post_install_tata.png>`
| :download:`Terremark Enterprise Cloud <./images/post_install_terremark.png>`
    
MySQL
~~~~~

The root password for MySQL was generated at the time of installation. The password will
be located in ``/etc/mysql/grants.sql`` on Debian derivatives and
``/etc/mysql_grants.sql`` on RHEL and CentOS.
