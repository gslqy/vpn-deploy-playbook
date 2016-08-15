Setup a sane dns server that free of dns poisoning . 

# Steps

1. add server info in ansible_hosts 
  ```
  testvm ansible_ssh_host=192.168.1.14 ansible_ssh_user=vagrant  ansible_sudo=true

  [sane-dns]
  testvm 
  ```

2. (Optional) Edit host_vars/testvm.yml , config sane upstream dns servers & china upstream dns server . You can skip this step, the default config usually works well . 
  ```
  sane_upstream:
    - 8.8.8.8
    - 8.8.4.4
    - 208.67.222.222:5353

  china_upstream:
    - 114.114.114.114
    - 115.115.115.115
  ``` 

3. run the playbook 
  ```
  ansible-playbook  sane-dns.yml  -l testvm
  ```