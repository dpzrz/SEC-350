## Routing and DMZ 
This page will serve as a complete setup of the boxes below and a detailed guide on how to recreate these setup steps. 
### Boxes + Configuration

Rw01:

Tools used:
nmtui
ip a
network manager
snapshots

![{95243A15-6A9E-4866-9678-5E01F8892328}](https://github.com/user-attachments/assets/43aed687-f77b-43b6-80f7-debc22ce723e)



![{75E86678-DB3C-40D7-A5E8-6643E9FCA077}](https://github.com/user-attachments/assets/ea34ff23-42a3-4a56-83f0-1a3579d2764a)

Fw01:
vyos:

configure

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

Web01:

nmtui

![{6B002826-2CBE-4302-A788-3637C1F4A1B4}](https://github.com/user-attachments/assets/01862bcf-24ca-46ab-82a4-cddc0514c674)


firewalld https + http

Routes gui

http

network manager





Log01:

