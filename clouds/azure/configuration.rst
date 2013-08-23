:orphan:

Configuration
-------------

Dasein Cloud expects the following provider context values to be
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
