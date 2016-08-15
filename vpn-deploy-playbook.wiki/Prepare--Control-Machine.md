# Prepare Control Machine 

## install ansible

for Ubuntu 
```
sudo apt-get install python-pip
sudo pip install -U ansible
```

## clone this repo, install dependencies 
```
git clone https://github.com/ftao/vpn-deploy-playbook.git
cd vpn-deploy-playbook
ansible-galaxy install -r requirements.yml 
```

## prepare inventory file   
```
cp ansible_hosts.example ansible_hosts
``` 
Then open `ansible_hosts` with your favorite editor , add your server to the file . 
Please read the document at  [http://docs.ansible.com/intro_inventory.html](http://docs.ansible.com/intro_inventory.html)  for syntax for the inventory file. 

To add a server , add something like below 
```
servername.example.com  ansible_ssh_host=ip.of.server ansible_ssh_user=some-user 
```

To add server to a group, for example , if the server is act as a pptp server 
```
[pptp]
servername.example.com 
```

## customize group or server variable 
In mostcases , there will be default values for a certain variables, located in `group_vars/`
please copy the example file from `group_vars/some-group.yml.example` to `group_vars/some-group.yml` . 
If may want to change the variable in the file . 
If you need to change the variable only for a certain server , not the whole group. 
Please create a file in  `host_vars/servername.yml` , `servername` should match the name in inventory file, and add the variable there . 
 
