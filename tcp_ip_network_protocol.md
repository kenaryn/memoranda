### Network submasks            

`/24`: regular network susbmask specify that the 24 first bits hold 1. Hence, 8 bits are remaining (*i.e.* 256 possible adresses) to administer the subnetwork.

But the machine assigned to the 0 based address at the last byte belongs to the network and the one ending by 255 is a "broadcast" machine to communicate *via* ARP (*viz* Address Resolution Protocol) through the data link layer from the OSI model.
256 - 1 - 1 = 254. Therefore, 254 adresses are left to set up many hosts..

Network submask is a cornerstone to administer a company network.

DHCP (*viz* Dynamic Host Configuration Protocol) leases commonly expire every couple of hours by default if regular settings not are altered.


### Gateways or how to communicate to another IP outside the local network.
`ip route` to check out the default gateway address. It dispatches packets send by a machine to another one on another network.
The default gateway are usually set up at 1 or 254 to the fourth byte (*i.e.* host part of the IP).


### Diagnostics
- `traceroute <IP> / <hostname>` to trace a route taken by a packet over a IPv4/IPv6 network.
- `netstat -a` to print active connexions and listening ports.
- `nslookup <domain_name>` resolves a domain's name into an IP.
- `ip link show` to reveal some info related to network settings
`cat /sys/class/net/<network_codename>/address` to check out the device's MAC address.

### Help's command  - windows
On Windows, prepend `/?` to a command to access help menu, *e.g.* `dir /?`

