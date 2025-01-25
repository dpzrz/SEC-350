# Zone-based firewall


![firewall-gral-packet-flow](https://github.com/user-attachments/assets/c79117af-64a0-4457-9dc2-67b07fac5667)




![firewall-zonebased](https://github.com/user-attachments/assets/0dcadb23-800e-433f-9494-1a70c0fbd2b9)



#### Configuring Vyos


Configuring our fw differs as changes are made to the running configuration by entering `configure” mode`.  These changes are applied to the running configuration via `commit`.  The changes persist after reload only if you `save` them.  You leave configuration mode via the `exit` command. In practice this looks like:


##### Hostname Config
```
configure
set system host-name fw1-hostname
commit
save
exit 
```

##### Interface Assignemnt

First you have to delete any dhcp assignments on our fw:
```
show interfaces
```
![{05B9226F-6722-407C-B749-48298E2D071C}](https://github.com/user-attachments/assets/c7407f2c-0441-4fca-9baa-6b0ad1c96b50)

```
delete interfaces ethernet eth0 address dhcp
delete interfaces ethernet eth1 address dhcp
commit
save
```


![{496FD017-29E1-4759-9A6C-E919E5334D81}](https://github.com/user-attachments/assets/76f23317-4c76-4d4b-9752-0983edbedbae)


```
configure
set system host-name
commit
save
exit
```
Once thats complete we need to organize our open ethernet ports. This is down by adding a description before assignment:

```
configure
set interface eth0 description SEC350-WAN
commit
save
exit
```
Assigning our descriptions for DMZ and LAN are the same

```
configure
set interface eth1 description HOSTNAME-DMZ
set interface eth2 description HOSTNAME-LAN
commit
save
```

Now assigning our IP Addresses using the vyos configuration method:
```
set interfaces ethernet ethX IPADDRESS/MASK
commit
save
exit
```
This process neeeds to be done to each of our ethernet ports; eth0, eth1, eth2

Our WAN interface needs to now be routed to the internet. This process invlves setting both the deafult gateway and DNS server.

#### NAT and DNS Forwarding

Were using the same command types with the inclusion of `source rule 10`
```
configure
set nat source rule 10 description "NAT FROM DMZ to WAN"
set nat source rule 10 outbound-interface eth0
set nat source rule 10 source address 172.16.50.0/29
set nat source rule 10 translation address masquerade
commit
save
```
To check out forwarding is a successes we run this command `show nat source rule 10`, it will output our changes and parrot back our source address and translation rules.

Next is configuring DNS Forwarding, we'll need to set services and were going to need to define threee things:
- listen-address
- forwarding allow address
- forwarding system

Those commands look like this:
```
set service dns forwarding listen-address 172.16.50.2
set service dns forwarding allow-from 172.16.50.0/29
set service dns forwarding system
commit
save
```

Refrences
[Vyos Docs](https://docs.vyos.io/en/latest/configuration/firewall/index.html)
