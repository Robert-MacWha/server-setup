# Server Setup
Ansible scripts used to automate the setup of a privilaged ssh user, docker, and portainer on new servers.

## Automation scope
This automation should be used to install the bare minimum required to get portainer and associated services running.  Individual docker containers should be managed by docker-compose (ideally stacks) from within portainer and, while they may be included inside this repository for convenience, they should not automagically be deployed.

This playbook assumes that the OS and portainer will be installed on one disk, while all media / data will be contained on another.

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

3. Setup storage partition <!-- This step could be automated, but it feels dangerous. Still - might be more dangerous to have someone attempting to setup partitions without knowing what they're doing. -->
```bash
apt install gdisk
gdisk /dev/sdb # replace `sdb` with whatever disk you're using
    n
    (default) # 1
    (default) # 2048
    (default) # large number
    (default) # 8300
    p # ensure the file system matches what you expect
    w
    y
lsblk # ensure /dev/sdb1 was created and fills the disk
mkfs.ext4 /dev/sdb1
```

4. Install ansible
```bash
sudo apt install python3-pip -y
pip3 install ansible
ansible -v
```

5. Run ansible
```bash
ansible-playbook mount_smb_share.yml
ansible-playbook setup_server.yml
```

6. Set smbuser password
```bash
smbpasswd -a rmacwha # I recommend setting to the same as the debian password for simplicity
```

6. Setup portainer
Navigate to `<server's ip>:9000` and create a portainer user

7. Setup SMB access
 - Open windows file explorer, right click "This PC", and press "Map Network Drive..."
 - Enter the network drive's ip address followed by the share folder: `\\<ip-address>\home`
 - Tick "Connect using different credentials"
 - Press finish, then enter your server user's credentials from config.yml

> If you didn't connect using different credentials when creating the mapping, you can search for "credential manager" and enter the credentials under "windows credentials"