Software
--------

DMCM leverages several open-source technologies as part of the overall cloud
management solution.

Use of these technologies is **required** for installing and running the Enstratius cloud
management software.

The installation and configuration of these components is handled by the configuration
management installation service provided by DMCM.

Operating System
~~~~~~~~~~~~~~~~

Enstratius installation requires the Linux operating system. 

See the :ref:`Supported Configurations <supported_configuration>` page for more information.

Riak
~~~~

DMCM is in the process of transitioning to using Riak for the database backend.

MySQL
~~~~~

DMCM was originally designed to use MySQL. The use of this software will be slowly
replaced as the transition to Riak happens.

Open JDK
~~~~~~~~

DMCM uses the Java Open JDK.

Jetty
~~~~~

DMCM uses Jetty as an HTTP and servlet container.


Rabbit MQ
~~~~~~~~~

DMCM uses Rabbit as the message queue.
