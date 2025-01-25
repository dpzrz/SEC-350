




## Routing and DMZ 

This page will serve as a complete setup of the boxes below and a detailed guide on how to recreate these setup steps. 

![{38447B48-64E5-4AFA-A57E-6D9346681553}](https://github.com/user-attachments/assets/ff1cb289-3901-4e81-88f7-3a22c812d75c)
## Boxes + Configuration

### Rw01:

#### Creating a user and defining them as a sudo user

```
sudo adduser username
# this command will prompt a user stup terminal process just type Y for all options

sudo usermod -aG sudo username

```
#### Setting hostname through terminal
`sudo hostnamectl set-hostname hostname`

Road warrior can be set up graphically or through a config file. The route taken herre was using nmtui. Nmtui can be triggered in the terminal and provides the user with a GUI to configure their box. 

#### Nmtui Process

The nessecarry things to check in nmtui:

***Edit a connection***
- Add the IP address with the /** notation. Add gateway and DNS servers to `10.0.17.2`
- Use the X to check off require IPv4 addressing
- Make sure to set IPv4 config to manual amnd IPv6 to Disabled
- 
***Activate a connection***
- Sometimes your connection cna be deactivated so make sure its activated
Set system hostname
- This will prompt a swystem hostname prompt you can change. Nessecary for our logs to be more organized.

![{95243A15-6A9E-4866-9678-5E01F8892328}](https://github.com/user-attachments/assets/43aed687-f77b-43b6-80f7-debc22ce723e)



![{75E86678-DB3C-40D7-A5E8-6643E9FCA077}](https://github.com/user-attachments/assets/ea34ff23-42a3-4a56-83f0-1a3579d2764a)

Once you have httpd running and properly opened, we need to add our routes. I accomplished this through gui. 

Edit Wired connection ----> IPv4 Sttings -----> Routes:

- IP Adresss (.50)
- Netmask (/29)
- Gateway (WAN .163)

### Fw01:

vyos:

#### Configuring FW

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


### Web01:
Web01 is a Rocky Web Server runnig CentOS that should be placed on the DMZ Network with the IP Address of 172.16.50.3/29. Make sure web01’s network adapter is on the SEC350-DMZ Network.

Utilizing the tool nmtui for a graphical config setting hostname and IP addresses is the same as before. 

The sudo user process differs, the screenshot shown below shows the proper way to add a sudo user. 
![{6B002826-2CBE-4302-A788-3637C1F4A1B4}](https://github.com/user-attachments/assets/01862bcf-24ca-46ab-82a4-cddc0514c674)

While our web01 is IP assigned and able to ping fw01 it wont be able to reach the internet. This is becasue our fw01 isnt configured to forward or translate IP and DNS.

Since this is our web box we'll need to install http. This starts with a simple `sudo yum install http`. After a successful install we'll need to permanently open those ports.

The firewall service can be modified using `firewall-cmd`
```
sudo firewall-cmd --zone=public --add-service=http
sudo firewall-cmd --zone=public --add-service=https
sudo systemctl restart firewalld
```
You can also confirm these changes by using the command `sudo firewall-cmd --zone=public --list-services`

After adding the routes you should be able to retrieve the httpd test page. 

### Log01:

Opeing traffic for ports 514 UPD and TCP is relativley easy. Were using the same `firewall-cmd` command scheme:

```
sudo firewall-cmd --zone=public -addport-514/tcp --permanent
sudo firewall-cmd --zone=public -addport-514/udp --permanent
```




<img width="864" alt="image" src="https://github.com/user-attachments/assets/8a32a0d9-fb19-4606-ae7b-16e2a26b1a61" />


