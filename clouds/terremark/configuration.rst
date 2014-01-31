:orphan:


Terremark Enterprise Cloud
--------------------------
DCM supports Terremark Enterprise Cloud resources. The section below identifies how Terremark cloud resources are mapped to DCM cloud resources.

======================= ========
Terremark Term          DCM Term
======================= ========
Organization            Account
Environment             Region
Compute Pool            Data Center
Virtual Machine         Server
Disk				            Volume
Template	   		        Public Machine Image
Catalog Entry 	        Private Machine Image
Network	      	        Network
Public IP			          IP Address / Public IP
IP Address			        Private IP
Firewall Rule		        Firewall Rule
Internet Service + Node Service Forwarding Rule
SSH Key				          Keypair
======================= ========

Organization
~~~~~~~~~~~~
A Terremark organization can have one or more environments. Each environment has a particular location (such as Miami), but there may be more than one environment in a location. 

Networking resources (Networks, IPs, Firewall Rules) live at the environment level, so they can be used by any of the compute pools associated with that environment.

Each environment may have one or more compute pools. Virtual machines, disks, and templates live at the compute pool level.

Networks
~~~~~~~~
There are two types of networks:
DMZ – network where servers are behind the firewall but permit limited access to and from the public Internet. 
Internal – network where the servers are isolated from the public internet because they contain sensitive information.

The current version of DCM does not differentiate between network types. Networks cannot be created or deleted with the Terremark API, so DCM does not support these operations.

IP Addresses and Forwarding Rules
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Private IPs are associated with networks. Public IPs are independent of networks. New Terremark accounts start with one active public IP. Using the Terremark API via DCM, you can activate/reserve additional public IPs until you reach the limit on your account. You cannot deactivate/release public IPs in Terremark.

In Terremark Enterprise Cloud you can create internet services for a public IP. An internet service defines a publicly routable port and protocol on a public IP. For each internet service, you can create one or more node services which tie the internet service to a port and protocol on a private IP address associated with a VM.

Firewall rules for internet services are created automatically when a node service is created for the internet service. DCM abstracts out the combination of an internet service and a node service and refers to it as a forwarding rule on an IP address. So when you create a forwarding rule in DCM you are actually creating both an internet service and a node service in Terremark. If you tie more than one node service to the same internet service traffic will be load balanced between the node services. In DCM terms that means creating two forwarding rules for the same IP Address that use the same port, but point to different servers.

Firewalls & Firewall Rules
~~~~~~~~~~~~~~~~~~~~~~~~~~

Each Terremark environment has one firewall. Firewall rules have two types, Internet Service and Custom. Internet Service rules are generated automatically by the creation of a node service. They are also removed automatically when a node service is deleted. They cannot be manually created or revoked. Custom rules are created and revoked manually. 

DCM supports listing of firewall rules and creating Internet Service type firewall rules automatically by adding forwarding rules to an IP. Internet Service rules will allow traffic from any source to the port of the VM as defined by the node service. DCM also supports creating Custom-type firewall rules. Custom rules can allow or deny traffic. Deny rules deny traffic from a specified external CIDR to all internal destinations. Allow rules allow traffic from an internal private IP address or network to an internal private IP address, network, or external CIDR.

These are the possible firewall rule combinations in Terremark and how they map to DCM firewall rules:

================= ========== ================ ================ ========== =========== ========= ================
Terremark                                                      Dell Cloud Manager
-------------------------------------------------------------- -------------------------------------------------
Type              Permission Source           Destination      Permission SourceType  Direction Destination Type
================= ========== ================ ================ ========== =========== ========= ================
Custom            Deny       External IP      Any              Deny       CIDR        Ingress   Global
Custom            Deny       External Network Any              Deny       CIDR        Ingress   Global
Custom            Allow      IP Address       IP Address       Allow      CIDR        Ingress   CIDR
Custom            Allow      IP Address       Network          Allow      CIDR        Ingress   VLAN
Custom            Allow      IP Address       External IP      Allow      CIDR        Egress    CIDR
Custom            Allow      IP Address       External Network Allow      CIDR        Egress    CIDR
Custom            Allow      IP Address       Any              Allow      CIDR        Egress    Global
Custom            Allow      Network          IP Address       Allow      VLAN        Ingress   CIDR
Custom            Allow      Network          Network          Allow      VLAN        Ingress   VLAN
Custom            Allow      Network          External IP      Allow      VLAN        Egress    CIDR
Custom            Allow      Network          External Network Allow      VLAN        Egress    CIDR
Custom            Allow      Network          Any              Allow      VLAN        Egress    Global
Internet Service  Allow      Any              IP Address       Allow      Global      Ingress   CIDR
================= ========== ================ ================ ========== =========== ========= ================

Machine Images
~~~~~~~~~~~~~~

There are two types of machine images in Terremark: Catalog Entries and Templates.

Templates are public images created by Terremark that live at the compute pool level. 

Catalog Entries are custom images you can create by cloning a virtual machine or by uploading OVF and VMDK files. Catalog entries live at the organization level and can be imported into any compute pool that supports the operating system defined in the catalog entry. 

DCM supports creating catalog entries (Machine Images) from VMs (Servers). When you create a catalog entry it clones the configuration of the VM including the private IP address. This results in an IP address conflict if you have the original VM and a VM created from the catalog running simultaneously. 

To fix this you need to configure the IP address at the operating system level on the VM. When DCM launches a catalog entry it makes an API call to Terremark to tie a new private IP to the new VM, but this does not make any functional changes to the machine, it just updates the metadata Terremark has about the VM.

You can identify the machine image type and the compute pool a template is associated with based on its provider ID. Provider IDs follow this format: <imageId>:<computePoolId>:<image_type>

Server Configuration
~~~~~~~~~~~~~~~~~~~~

When launching a new server you should specify the network you want to launch into. You should also specify an SSH key when launching new Linux Servers. New Terremark SSH keys can be created using DCM.

Rows and Groups are Terremark organizational constructs for grouping virtual machines within an environment. They have no functional impact and are hidden by DCM.

Servers can be powered on (started), powered off (stopped/paused) and rebooted and they will not lose their hardware or networking configuration. The hardware configuration can be changed using the DCM API while the server is stopped. 

Server Product IDs are of the form cpu_count:ram_size_mb:root_disk_size_gb.
A server must have at least one but no more than fifteen disks/volumes. Volume capacity can be increased to a maximum of 512GB but can not be reduced. Non-system volumes can be detached from servers and attached to other servers, but you can not create a new volume independent of a server.
