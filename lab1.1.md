## Routing and DMZ 

This page will serve as a complete setup of the boxes below and a detailed guide on how to recreate these setup steps. 

![{38447B48-64E5-4AFA-A57E-6D9346681553}](https://github.com/user-attachments/assets/ff1cb289-3901-4e81-88f7-3a22c812d75c)
### Boxes + Configuration

Rw01:

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


nmtui
ip a
network manager
snapshots

![{95243A15-6A9E-4866-9678-5E01F8892328}](https://github.com/user-attachments/assets/43aed687-f77b-43b6-80f7-debc22ce723e)



![{75E86678-DB3C-40D7-A5E8-6643E9FCA077}](https://github.com/user-attachments/assets/ea34ff23-42a3-4a56-83f0-1a3579d2764a)

Fw01:

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

commit

save

exit

show network interfaces

![{496FD017-29E1-4759-9A6C-E919E5334D81}](https://github.com/user-attachments/assets/76f23317-4c76-4d4b-9752-0983edbedbae)


```
configure
set system host-name
commit
save
exit
```
Once thstd complete we need to organize our open ethernet ports. This is down by adding a description before assignment:

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

Web01:

nmtui

![{6B002826-2CBE-4302-A788-3637C1F4A1B4}](https://github.com/user-attachments/assets/01862bcf-24ca-46ab-82a4-cddc0514c674)


firewalld https + http

Routes gui

http

network manager





Log01:

<img width="864" alt="image" src="https://github.com/user-attachments/assets/8a32a0d9-fb19-4606-ae7b-16e2a26b1a61" />


