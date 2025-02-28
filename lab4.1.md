








<img width="887" alt="image" src="https://github.com/user-attachments/assets/6ca4f6c2-3c0a-49f0-a3ca-b06c8b6bbd62" />



CUSTOM RULE 10

```
set firewall name WAN-to-DMZ rule 10 action accept
set firewall name WAN-to-DMZ rule 10 description 'Allow HTTP from WAN to DMZ'
set firewall name WAN-to-DMZ rule 10 destination address 172.16.50.3
set firewall name WAN-to-DMZ rule 10 protocol 'tcp'
set firewall name WAN-to-DMZ rule 10 destination port '80'

```


RULE 1
```
set firewall name DMZ-to-WAN rule 1 action accept
set firewall name DMZ-to-WAN rule 1 state established enable

```


```

set zone-policy zone WAN from DMZ firewall name DMZ-to-WAN 
set zone-policy zone DMZ from WAN firewall name WAN-to-DMZ
```





LAN-to-MGMT
Create a LAN-to-MGMT firewall that:
Allows 1514,1515/tcp from LAN to wazuh
Allows 443/tcp from mgmt01 on LAN to wazuh
Allows 22/tcp from mgmt01 on LAN to wazuh
Allows established traffic back through the related firewall

MGMT-to-LAN
Create a MGMT-TO-LAN firewall that:
Allows MGMT to initiate any connection to the LAN
Allows MGMT to initiate any connection to the DMZ
Allows established traffic back again
If you do this right, you should be able to connect from mgmt02 to the DMZ like so.
 wget to web01
ping to mgmt01
ping outside will fail because you didn't explicitly allow MGMT to go anywhere but LAN and DMZ.

