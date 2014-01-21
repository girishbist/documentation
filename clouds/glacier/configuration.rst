:orphan:

Configuration
-------------

Glacier Support in DCM
~~~~~~~~~~~~~~~~~~~~~~

DCM Glacier support is found in the Platform -> Archives section of the console. Currently Amazon is the only cloud with archive support.  

============  ========
Glacier Term  DCM Term
============  ========
Vault         Directory
Archive       Archive
Directory     Directory
=========     =========

An archive is a single unchangeable file. A directory contains archives. There is only a single layer of directories, no hierarchy (you cannot create a directory within another directory).

Glacier itself is meant for "cold storage" -- data which is infrequently needed. It is much cheaper, but as a tradeoff it can be very slow to access your data.

*It is not possible to either upload or download archives using DCM*. This must be done with tools that talk directly to Glacier. Glacier is not supported in the Singapore and São Paulo regions and therefore will not appear in DCM within those regions.

There is no support for managing or querying Glacier through the DCM API. 

Directories
~~~~~~~~~~~
DCM supports the following operations on directories:

* Create a new directory in DCM. it will show up immediately in the DCM console. You may choose a budget code for a new directory. A directory will contain archives once you connect it with the Glacier vault. However, you cannot add archives directly from DCM, they must come through Glacier.
* Delete an EMPTY directory. Glacier does not support deleting directories which still contain archives. Empty directories can be deleted immediately.
* Access directory metadata: creation date, last update date, size
* DCM automatically discovers all directories in every Glacier region. If directories are created or deleted outside of DCM, they will appear in the console eventually, with the default budget code.

Archives
~~~~~~~~
DCM also auto-discovers all archives within each directory. Due to the nature of Glacier, it can take more than 24 hours for new archives to be discovered. Querying the contents of a directory involves creating a Glacier "job" to retrieve the information, which can take up to four hours. Glacier only updates this data once per day.

DCM supports deleting archives from the console. Deleted archives disappear immediately from the console. Due to the nature of Glacier, they may still be visible to clients of the Glacier API for up to a day. Likewise, if an archive is deleted from outside of DCM, it may still appear in the console for up to a day.
