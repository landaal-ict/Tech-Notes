
### vlans op specifieke poorten


Assuming vlan10 trusted-home 172.31.234.0/24  
vlan20 - unk  
vlan30 - unk  
vlan40 hotspot  
  
Assign all vlans to the bridge THEN..............  
  
/bridge ports  
add bridge=bridge interface=ether2 pvid=10 ingress-filtering=yes  
add bridge=bridge interface=ether3 (hybrid) pvid=10  
add bridge=bridge interface=ether4 pvid=10  
add bridge=bridge interface=ether5 pvid=20  
add bridge=bridge interface=ether6 pvid=30  
/bridge vlans  
add bridge=bridge tagged=bridge untagged=ether2,ether3,ether 4 pvid=10  
add bridge=bridge tagged=bridge untagged=ether5 pvid=20  
add bridge=bridge tagged=bridge untagged=ether6 pvid=30  
add bridge=bridge tagged=bridge,ether3 pvid=40  
  
Need addresses, dhcp-server, dhcp-server-network, IP pool, and addresses for all vlans  
  
/ip interface  
add name=WAN  
add name=LAN  
add name=Manage  
  
/ip interface members  
ether1 list=LAN (unless its a vlan or pppoe and then that name need also to be added to WAN interface)  
vlan10 list=LAN  
vlan20 list=LAN  
vlan30 list=LAN  
vlan40 list=LAN  
vlan10 list=Manage  
  
/ip neighbours discovery  
interface-list=Manage  
/ip tools  
mac server  
win mac server interface-list=Manage  
  
/ip firewall filter  
{Input Chain}  
add action=accept chain=input comment="defconf: accept established,related,untracked" connection-state=established,related,untracked  
add action=drop chain=input comment="defconf: drop invalid" connection-state=invalid  
add action=accept chain=input comment="defconf: accept ICMP" protocol=icmp  
add action=accept chain=input in-interface-list=Manage {optional to add a source address firewall list of Admin devices)  
add action=accept chain=input in-interface-list=LAN dst-port=53  
add action=drop chain=input comment="drop all else"  
{forward chain}  
add action=fasttrack-connection chain=forward comment="defconf: fasttrack" connection-state=established,related  
add action=accept chain=forward comment="defconf: accept established,related, untracked" connection-state=established,related,untracked  
add action=drop chain=forward comment="defconf: drop invalid" connection-state=invalid  
add action=accept chain=forward comment="allow internet traffic" in-interface-list=LAN out-interface-list=WAN  
add action=accept chain=forward comment="allow port forwarding" connection-nat-state=dstnat  
add action=drop chain=forward  
/ip firewall nat  
add action=masquerade chain=srcnat comment="defconf: masquerade" out-interface-list=WAN