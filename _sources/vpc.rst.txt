=====================
Virtual Private Cloud
=====================

About Virtual Private Cloud
===========================

Virtual Private Cloud is a private, isolated part of Cirrus. A VPC can have its own virtual Network topology that resembles a traditional physical Network. You can launch Instances in the virtual Network that can have private addresses in the range of your choice, for example: 10.0.0.0/16. You can define Network Tiers within your VPC Network range, which in turn enables you to group similar kinds of Instances based on IP address range.

For example, if a VPC has the private range 10.0.0.0/16, its guest Networks can have the Network ranges 10.0.1.0/24, 10.0.2.0/24, 10.0.3.0/24, and so on.

Major Components of a VPC
=========================

A VPC is comprised of the following Network components:

* VPC: A VPC acts as a container for multiple isolated Networks that can communicate with each other via its virtual router.
* Network Tiers: Each Network Tier acts as an isolated Network with its own VLANs and CIDR list, where you can place groups of resources, such as Instances. The Network Tiers are segmented by means of VLANs. The NIC of each Network Tier acts as its gateway.
* Virtual Router: A virtual router is automatically created and started when you create a VPC. The virtual router connect the Network Tiers and direct traffic among the public gateway, the VPN gateways, and the NAT Instances. For each Network Tier, a corresponding NIC and IP exist in the virtual router. The virtual router provides DNS and DHCP services through its IP.
* Public Gateway: The traffic to and from the Internet routed to the VPC through the public gateway. In a VPC, the public gateway is not exposed to the end user; therefore, static routes are not support for the public gateway.
* Private Gateway: All the traffic to and from a private Network routed to the VPC through the private gateway. For more information, see “Adding a Private Gateway to a VPC”.
* VPN Gateway: The VPC side of a VPN connection.
* Site-to-Site VPN Connection: A hardware-based VPN connection between your VPC and your datacenter, home Network, or co-location facility. For more information, see “Setting Up a Site-to-Site VPN Connection”.
* Customer Gateway: The customer side of a VPN Connection. For more information, see “Creating and Updating a VPN Customer Gateway”.
* NAT Instance: An Instance that provides Port Address Translation for Instances to access the Internet via the public gateway. For more information, see “Enabling or Disabling Static NAT on a VPC”.
* Network ACL: Network ACL is a group of Network ACL items. Network ACL items are nothing but numbered rules that are evaluated in order, starting with the lowest numbered rule. These rules determine whether traffic is allowed in or out of any Network Tier associated with the Network ACL. For more information, see “Configuring Network Access Control List”.

Adding a Virtual Private Cloud
==============================

When creating the VPC, you simply provide the zone and a set of IP addresses for the VPC Network address space. You specify this set of addresses in the form of a Classless Inter-Domain Routing (CIDR) block.

#. In the left navigation, choose Network.
#. In the Select view, select VPC.
#. Click Add VPC. The Add VPC page is displayed as follows:
#. Provide the following information:

    * Name: A short name for the VPC that you are creating.
    * Description: A brief description of the VPC.
    * Zone: Choose the zone where you want the VPC to be available.
    * CIDR: Defines the CIDR range for all the Network Tiers (guest Networks) within a VPC. When you create a Network Tier, ensure that its CIDR is within the Super CIDR value you enter. The CIDR must be RFC1918 compliant.
    * VPC Offering: If the administrator has configured multiple VPC offerings, select the one you want to use for this VPC.

#. Click OK.

Adding Network Tiers
====================

Network Tiers are distinct locations within a VPC that act as isolated Networks, which do not have access to other Network Tiers by default. Network Tiers are set up on different VLANs that can communicate with each other by using a virtual router. Network Tiers provide inexpensive, low latency Network connectivity to other Network Tiers within the VPC.

#. In the left navigation, choose Network.
#. In the Select view, select VPC.
#. All the VPC that you have created for the Account is listed in the page.
#. Click the Configure button of the VPC for which you want to set up Network Tiers.
#. Click Create Network.
#. The Add new Network Tier dialog is displayed, as follows:
   If you have already created Network Tiers, the VPC diagram is displayed. Click Create Network Tier to add a new Network Tier.

#. Specify the following:
   All the fields are mandatory.

    * Name: A unique name for the Network Tier you create.
    * Network Offering: The following default Network offerings are listed: Internal LB, DefaultIsolatedNetworkOfferingForVpcNetworksNoLB, DefaultIsolatedNetworkOfferingForVpcNetworks
      
      In a VPC, only one Network Tier can be created by using LB-enabled Network offering.
    * Gateway: The gateway for the Network Tier you create. Ensure that the gateway is within the Super CIDR range that you specified while creating the VPC, and is not overlapped with the CIDR of any existing Network Tier within the VPC.
    * Netmask: The netmask for the Network Tier you create.
      
      For example, if the VPC CIDR is 10.0.0.0/16 and the Network Tier CIDR is 10.0.1.0/24, the gateway of the Network Tier is 10.0.1.1, and the netmask of the Network Tier is 255.255.255.0.

#. Click OK.
#. Continue with configuring access control list for the Network Tier.

About Network ACL Lists
=======================

A Network ACL is a group of Network ACL rules. Network ACL rules are processed by their order, starting with the lowest numbered rule. Each rule defines at least an affected protocol, traffic type, action and affected destination / source Network.

Creating ACL Lists
==================

#. In the left navigation, choose Network.
#. In the Select view, select VPC.
#. All the VPCs that you have created for the Account is listed in the page.
#. Click the Configure button of the VPC.
   
   For each tier, the following options are displayed:

    * Internal LB
    * Public LB IP
    * Static NAT
    * Instances
    * CIDR

   The following router information is displayed:

    * Private Gateways
    * Public IP Addresses
    * Site-to-Site VPNs
    * Network ACL Lists

#. Select Network ACL Lists.
   
   The following default rules are displayed in the Network ACLs page: default_allow, default_deny.

#. Click Add ACL Lists, and specify the following:

    * ACL List Name: A name for the ACL list.
    * Description: A short description of the ACL list that can be displayed to users.

Creating an ACL Rule
====================

#. In the left navigation, choose Network.
#. In the Select view, select VPC.

   All the VPCs that you have created for the Account is listed in the page.

#. Click the Configure button of the VPC.
#. Select Network ACL Lists.

   In addition to the custom ACL lists you have created, the following default rules are displayed in the Network ACLs page: default_allow, default_deny.

#. Select the desired ACL list.
#. Select the ACL List Rules tab.

   To add an ACL rule, fill in the following fields to specify what kind of network traffic is allowed in the VPC.

    * Rule Number: The order in which the rules are evaluated.
    * CIDR: The CIDR acts as the Source CIDR for the Ingress rules, and Destination CIDR for the Egress rules. To accept traffic only from or to the IP addresses within a particular address block, enter a CIDR or a comma-separated list of CIDRs. The CIDR is the base IP address of the incoming traffic. For example, 192.168.0.0/22. To allow all CIDRs, set to 0.0.0.0/0.
    * Action: What action to be taken. Allow traffic or block.
    * Protocol: The networking protocol that sources use to send traffic to the tier. The TCP and UDP protocols are typically used for data exchange and end-user communications. The ICMP protocol is typically used to send error messages or network monitoring data. All supports all the traffic. Other option is Protocol Number.
    * Start Port, End Port (TCP, UDP only): A range of listening ports that are the destination for the incoming traffic. If you are opening a single port, use the same number in both fields.
    * Protocol Number: The protocol number associated with IPv4 or IPv6. For more information, see Protocol Numbers.
    * ICMP Type, ICMP Code (ICMP only): The type of message and error code that will be sent.
    * Traffic Type: The type of traffic: Incoming or outgoing.

#. Click Add. The ACL rule is added.
   
   You can edit the tags assigned to the ACL rules and delete the ACL rules you have created. Click the appropriate button in the Details tab.

Creating a Tier with Custom ACL List
====================================

#. Create a VPC.
#. Create a custom ACL list.
#. Add ACL rules to the ACL list.
#. Create a tier in the VPC.
   
   Select the desired ACL list while creating a tier.

#. Click OK.

Assigning a Custom ACL List to a Tier
=====================================

#. Create a VPC.
#. Create a tier in the VPC.
#. Associate the tier with the default ACL rule.
#. Create a custom ACL list.
#. Add ACL rules to the ACL list.
#. Select the tier for which you want to assign the custom ACL.
#. Click the Replace ACL List icon. button to replace an ACL list

   The Replace ACL List dialog is displayed.

#. Select the desired ACL list.
#. Click OK.

Deploying Instances to the Tier
===============================

#. In the left navigation, choose Network.
#. In the Select view, select VPC.
   
   All the VPCs that you have created for the Account is listed in the page.

#. Click the Configure button of the VPC to which you want to deploy the Instances.

   The VPC page is displayed where all the tiers you have created are listed.

#. Click Instances tab of the tier to which you want to add an Instance.

   The Add Instance page is displayed.

   Follow the on-screen instruction to add an Instance. For information on adding an Instance, see the Installation Guide.

Acquiring a New IP Address for a VPC
====================================

When you acquire an IP address, all IP addresses are allocated to VPC, not to the guest networks within the VPC. The IPs are associated to the guest network only when the first port-forwarding, load balancing, or Static NAT rule is created for the IP or the network. IP can't be associated to more than one network at a time.

#. In the left navigation, choose Network.
#. In the Select view, select VPC.

   All the VPCs that you have created for the Account is listed in the page.

#. Click the Configure button of the VPC to which you want to deploy the Instances.

   The VPC page is displayed where all the tiers you created are listed in a diagram.

   The following options are displayed.

    * Internal LB
    * Public LB IP
    * Static NAT
    * Instances
    * CIDR

   The following router information is displayed:

    * Private Gateways
    * Public IP Addresses
    * Site-to-Site VPNs
    * Network ACL Lists

#. Select IP Addresses.

   The Public IP Addresses page is displayed.

#. Click Acquire New IP, and click Yes in the confirmation dialog.

   You are prompted for confirmation because, typically, IP addresses are a limited resource. Within a few moments, the new IP address should appear with the state Allocated. You can now use the IP address in port forwarding, load balancing, and static NAT rules.

Releasing an IP Address Allotted to a VPC
=========================================

The IP address is a limited resource. If you no longer need a particular IP, you can disassociate it from its VPC and return it to the pool of available addresses. An IP address can be released from its tier, only when all the networking ( port forwarding, load balancing, or StaticNAT ) rules are removed for this IP address. The released IP address will still belongs to the same VPC.

#. In the left navigation, choose Network.
#. In the Select view, select VPC.

   All the VPCs that you have created for the Account is listed in the page.

#. Click the Configure button of the VPC whose IP you want to release.

   The VPC page is displayed where all the tiers you created are listed in a diagram.

   The following options are displayed.

    * Internal LB
    * Public LB IP
    * Static NAT
    * Instances
    * CIDR

   The following router information is displayed:

    * Private Gateways
    * Public IP Addresses
    * Site-to-Site VPNs
    * Network ACL Lists

#. Select Public IP Addresses.

   The IP Addresses page is displayed.

#. Click the IP you want to release.
#. In the Details tab, click the Release IP button button to release an IP.

Enabling or Disabling Static NAT on a VPC
=========================================

A static NAT rule maps a public IP address to the private IP address of an Instance in a VPC to allow Internet traffic to it. This section tells how to enable or disable static NAT for a particular IP address in a VPC.

If port forwarding rules are already in effect for an IP address, you cannot enable static NAT to that IP.

If a Guest Instance is part of more than one network, static NAT rules will function only if they are defined on the default network.

#. In the left navigation, choose Network.
#. In the Select view, select VPC.
   
   All the VPCs that you have created for the Account is listed in the page.

#. Click the Configure button of the VPC to which you want to deploy the Instances.

   The VPC page is displayed where all the tiers you created are listed in a diagram.

   For each tier, the following options are displayed.

    * Internal LB
    * Public LB IP
    * Static NAT
    * Instances
    * CIDR

   The following router information is displayed:

    * Private Gateways
    * Public IP Addresses
    * Site-to-Site VPNs
    * Network ACL Lists

#. In the Router node, select Public IP Addresses.

   The IP Addresses page is displayed.

#. Click the IP you want to work with.
#. In the Details tab,click the Static NAT button. button to enable Static NAT. The button toggles between Enable and Disable, depending on whether static NAT is currently enabled for the IP address.
#. If you are enabling static NAT, a dialog appears as follows:
#. Select the tier and the destination Instance, then click Apply.

Adding a Port Forwarding Rule on a VPC
======================================

#. In the left navigation, choose Network.
#. In the Select view, select VPC.
   
   All the VPCs that you have created for the Account is listed in the page.

#. Click the Configure button of the VPC to which you want to deploy the Instances.

   The VPC page is displayed where all the tiers you created are listed in a diagram.

   For each tier, the following options are displayed:

    * Internal LB
    * Public LB IP
    * Static NAT
    * Instances
    * CIDR

   The following router information is displayed:

    * Private Gateways
    * Public IP Addresses
    * Site-to-Site VPNs
    * Network ACL Lists

#. In the Router node, select Public IP Addresses.

   The IP Addresses page is displayed.

#. Click the IP address for which you want to create the rule, then click the Configuration tab.
#. In the Port Forwarding node of the diagram, click View All.
#. Select the tier to which you want to apply the rule.
#. Specify the following:

    * Public Port: The port to which public traffic will be addressed on the IP address you acquired in the previous step.
    * Private Port: The port on which the Instance is listening for forwarded public traffic.
    * Protocol: The communication protocol in use between the two ports.

       * TCP
       * UDP

    * Add Instance: Click Add Instance. Select the name of the Instance to which this rule applies, and click Apply.

      You can test the rule by opening an SSH session to the Instance.

Removing Tiers
==============

You can remove a tier from a VPC. A removed tier cannot be revoked. When a tier is removed, only the resources of the tier are expunged. All the network rules (port forwarding, load balancing and staticNAT) and the IP addresses associated to the tier are removed. The IP address still be belonging to the same VPC.

#. In the left navigation, choose Network.
#. In the Select view, select VPC.

   All the VPC that you have created for the Account is listed in the page.

#. Click the Configure button of the VPC for which you want to set up tiers.

   The Configure VPC page is displayed. Locate the tier you want to work with.

#. Select the tier you want to remove.
#. In the Network Details tab, click the Delete Network button. button to remove a tier

   Click Yes to confirm. Wait for some time for the tier to be removed.

Editing, Restarting, and Removing a Virtual Private Cloud
=========================================================

#. In the left navigation, choose Network.
#. In the Select view, select VPC.
   
   All the VPCs that you have created for the Account is listed in the page.

#. Select the VPC you want to work with.
#. In the Details tab, click the Remove VPC button button to remove a VPC

   You can remove the VPC by also using the remove button in the Quick View.

   You can edit the name and description of a VPC. To do that, select the VPC, then click the Edit button. button to edit.

   To restart a VPC, select the VPC, then click the Restart button.

.. note::
    Ensure that all the tiers are removed before you remove a VPC.