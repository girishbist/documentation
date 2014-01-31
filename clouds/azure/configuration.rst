

Configuration
-------------

Azure Support in DCM
~~~~~~~~~~~~~~~~~~~~

DCM supports the following resource types in Azure: Servers, Machine Images, Volumes, Object Store/Files and Networks. The section below identifies any non-standard behavior for these resources as managed in DCM.

============================= ========================
Azure Terms                   Dell Cloud Manager Terms
============================= ========================
Cloud Service/Hosted Service  Machine Image
OS Image                      Machine Image
Disk                          Disk
Server                        Server
Volume                        Volume
Gateway                       VPN
Network                       Network
Object Store/Files            Storage Files
============================= ========================

Azure Disks
~~~~~~~~~~~

All disks are created from a .vhd file held in the storage account for the associated subscription. A disk cannot simply be created by giving it a name and size. A medialink to the location of the BLOB must also be provided. See Windows Azure Add Disk for more information.

An empty disk can be attached to a virtual machine to automatically create the data disk in the default storage account. See Windows Azure Add Data Disk. Set the HostCaching, the device the disk should be attached to, and the logical disk size. The disk size is passed in as an attribute. At the moment it is hardcoded to 1GB. 

A disk is created for each virtual machine. A disk with a configured operating system is called an image. This image is an OSVirtual disk and not a volume, so it is not returned in the listVolumes call. This disk is deleted when the associated VM is terminated.

Azure OS Image
~~~~~~~~~~~~~~

Servers must be stopped for imaging. If the server is running, DCM stops it, then captures an image.

All VMs must be sysprepped before you can make an image of them Please see the following information: Windows - Capture windows image, Linux - Capture linux image

When an image is made of a VM, that VM will be deleted!

Each image also has a .vhd file held in storage. When an image is deleted, the .vhd BLOB is also  removed from the storage account.

Azure Virtual Machine
~~~~~~~~~~~~~~~~~~~~~

Azure has a three-level architecture, as described in this hosted `Azure service overview <http://www.windowsazure.com/en-us/documentation/articles/fundamentals-application-models/#concepts>`. The levels are:
* Windows Azure Cloud Service (maximum of 20 cores per account, see `Usage Quotas <https://www.windowsazure.com/en-us/offers/commitment-plans#header-6>`.)
* Deployment
* Role
When a VM is created using DCM, all three levels are given the same name, but for purposes of correctly identifying VMs that have been created from the web console, all three are stored as tags. When you delete a VM, the deletion goes up the chain and deletes the deployment and hosted service too.

..note::Note
  * VMs are added to Azure subnets which are themselves part of VLANs configured in the Azure environment. The VM can only be added to an Azure network at the time of creation. For more information, read `Windows Azure Virtual Network <http://www.windowsazure.com/en-us/services/virtual-network/>`.
  * All VMs have a disk and associated .vhd file attached to them. Wherever the image used for the VM comes from, you can persistently store any changes made while a VM is running. The next time you create a VM from that .vhd, things pick up where you left off. It's also possible to copy the changed .vhd out of Windows Azure, then run it locally.

General Tips for Managing Azure Servers in DCM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* The Provider ID of a running server lists the server name three times and this name is a portion of the DNS of the server.  
  For example, the name of a test server DCMTest1 has a Provider ID of dcmtest1:dcmtest1:dcmtest1.  The DNS is then dcmtest1.cloudapp.net. 
* You cannot create standalone volumes.

  Azure has a default restriction of 20 Cloud Services per account, such as VMs. This is based on the number of cores used. For further explanation, see Usage Quotas.

* If launching a VM into a network, that network must have a subnet that can accept the VM.

Specifics for Ubuntu Servers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* When choosing a public Ubuntu Machine Image: We recommend those published by Canonical. RightScale public images have been prepared in a way that does not provide password authentication, so DCM has no way to authenticate using our public keys.  

* For Ubuntu servers, the username is "dasein".  

* To SSH to a linux VM the DNS name is 'name'.cloudapp.net, such as in the example above: dcmtest1.cloudapp.net.

Specifics for Windows Servers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* When choosing a public Windows Machine Image: We recommend those published by Canonical or Microsoft. RightScale public images have been prepared in a way that does not provide password authentication, so DCM has no way to authenticate using our public keys.  

* Windows takes longer to provision and therefore longer for DCM to show it as running. 

* Azure does not support the concept of a traditional firewall, and port 3389 for RDP is not publicly open by default.  Therefore, to access a Windows server through RDP you will either need to access using the default port of 59013, or you will have to access the direct Azure console and update the public endpoint for your server to 3389.

* For Windows servers, the username is "administrator"

Azure Networks
--------------
Gateways are not currently implemented in DCM

Provider Credentials
--------------------

Dell Cloud Manager expects the following provider context values to be
present:

-  accountNumber
-  X509 certificate
-  X509 key

Account Number
~~~~~~~~~~~~~~

Account number - subscription id from the Azure console

X509 Credentials
~~~~~~~~~~~~~~~~

Created a self-signed certificate by whatever method seems appropriate. Here are some options:

`Creating X509 Certificates (Linux) <http://www.ipsec-howto.org/x595.html>`_
`Creating X509 Certificates (Windows) <http://msdn.microsoft.com/en-us/library/vstudio/bfsktky3(v=vs.100).aspx>`_

Log in to your azure account at https://manage.windowsazure.com/#Workspace/All/dashboard

Go to Settings > Management Certificates and upload your certificate.

Now that your certificate has been linked to your account we need to set up the
account in Enstratius.  First of all we need to export the certificate and key
so that we can get the plain text contents.

Export the certificate file as .pem file.

Export the private key file as a .p12 file

.. code-block:: bash

   openssl pkcs12 -in <name of .p12 file> -out <name of .pem file you wish to create> -nodes

Example:

.. code-block:: bash

   openssl pkcs12 -in mykey.p12 -out mykey.pem -nodes

Open the .pem file that has been created and copy and paste the private key section to a new .pem file.

Within the Enstratius console you can then add your account credentials.

We had to hack the account in as it seems the existing console code is not
happy to just have x509 cert and key parameters and falls over when trying to
get the apiSecretKey (as it is null).  So Andy hardcoded a secretkey parameter
into a new jar file which I then used in order to link my Enstratius login to
the new azure cloud account.

X509 Certificate - open the certificate.pem file within TextEdit and copy+paste the contents into the text box.
X509 Key - open the key.pem file within TextEdit and copy+paste the contents into the text box.
