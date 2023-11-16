# Server Setup
Ansible scripts used to automate the setup of a privilaged ssh user, docker, and portainer on new servers.

## Automation scope
This automation should be used to install the bare minimum required to get portainer and associated services running.  Individual docker containers should be managed by docker-compose (ideally stacks) from within portainer and, while they may be included inside this repository for convenience, they should not automagically be deployed.

## Install Steps
0. Ensure ansible config is setup correctly
 - Ensure id_rsa.pub coresponds to correct device
 - Ensure vars/config.yml is correctly configured

1. Login to the server as root

2. Install git and clone server-setup repo
```bash
apt install git
cd /root
git clone https://github.com/Robert-MacWha/server-setup.git
cd server-setup
```

3. Install ansible
```bash
sudo apt install python3-pip -y
pip3 install ansible
ansible -v
```

4. Run ansible
```bash
ansible-playbook setup_server.yml
```

5. Setup portainer
Navigate to `<server's ip>:9000` and create a portainer user