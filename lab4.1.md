








<img width="887" alt="image" src="https://github.com/user-attachments/assets/6ca4f6c2-3c0a-49f0-a3ca-b06c8b6bbd62" />



CUSTOM RULE

```
set firewall name WAN-to-DMZ rule 10 action accept
set firewall name WAN-to-DMZ rule 10 description 'Allow HTTP from WAN to DMZ'
set firewall name WAN-to-DMZ rule 10 destination address 172.16.50.3
set firewall name WAN-to-DMZ rule 10 protocol 'tcp'
set firewall name WAN-to-DMZ rule 10 destination port '80'

```
