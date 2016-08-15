Setup a [ChinaDNS](https://github.com/clowwindy/ChinaDNS) server that free of dns poisoning . 

##### 1. Add server info in ansible_hosts 
``` ini
mydns ansible_ssh_host=192.168.0.170 ansible_ssh_user=lex ansible_sudo=true

[chinadns]
mydns
```

##### 2. (Optional) Edit `host_vars/mydns.yml`, config upstream DNS servers. You can skip this step, the default config usually works well.
``` yaml
chinadns_upstream:
  - 114.114.114.114
  - 8.8.4.4
update_resolvconf: true
``` 

##### 3. Run the playbook 
``` bash
ansible-playbook chinadns.yml -l mydns
```