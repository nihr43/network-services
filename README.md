# network-services

This repository defines a network architecture employing highly available dhcp, dns, and routing.  Automated dns record updates are performed through the use of isc-dhcp and bind.


##### Design

        net
         |
      -------
      |     |
     gw0   gw1   ns0   ns1
      |     |     |     |
      -------------------
              |       |
            dhcp0   dhcp1


Due do the increased reliance on dhcp, the dhcp servers have been configured in an active - active pair using the split-scope method.  For example, in a /24 range, dhcp0 offers .51'-.150 , dhcp1 offers .151-.250.  One or all dhcp servers will respond to dhcp discovers, but by design, a client will only acknowlege a single offer, preventing the need for any additional complexity.

The dhcp servers are aware of the master / slave dns configuration, and will send nsupdates accordingly.

The default routers are configured using common address redundancy protocol, CARP, and share an address.  NAT is performed using the PF firewall.

##### Use cases

 - providing network services to a vxlan or vlan in a virtual envrironment where High availability is required
 - consolidate gw0, dhcp0, and ns0 and so on - into two 1u, or one 1u 2 node server to be used in conjunction with a generic top-of-rack switch, in order to provide basic network services to a single layer 2 segment in a routed datacenter.
