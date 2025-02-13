## Wazuh + Agent Instalaltion

### Wazuh Installation

    curl -sO https://packages.wazuh.com/4.3/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i


-i to ignore minimum requirements of 2CPU and 4 GB RAM

### Agent Installation


Find the groups screen in Wazuh, create a new group called linux:

![image](https://github.com/user-attachments/assets/088abe12-5e3d-487b-9580-26f53e0d99c0)

From the group screen you can click the add new group and specify a name:

![image](https://github.com/user-attachments/assets/38fdcd1e-6b05-45c0-9eda-6bc1bc6a3a8b)


Find the agents screen in Wazuh:

![image](https://github.com/user-attachments/assets/44dd6c79-9e64-47cc-93ed-f7977d1bb2d0)

 Deploy a new agent with the following configuration:

- Redhat/CentoS
- CentOS 6 or higher (Note, it will work on rocky 8)
- x86_64
- 172.16.200.10
- Linux
This will give you the specifcally crafted syntax command to install on your remote system. In this case its web01.

Run the command on your web01 server

```
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

### Navigating Security Events

In the Agents tab click on web01. From there navigate to Security events:

![image](https://github.com/user-attachments/assets/acdadb14-360b-42e0-b7d9-c9b12a112ca1)

From Secuirty events you'll see dahsboard then you need to move to the Events tab:


![image](https://github.com/user-attachments/assets/81f9cabf-b9c0-46b1-bfe7-54c5d01225fe)


