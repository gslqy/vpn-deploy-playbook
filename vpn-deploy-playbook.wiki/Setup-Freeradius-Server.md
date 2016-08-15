这篇文章介绍了如何创建一个用于VPN认证和计费的Radius服务器和对应的管理界面。

# Steps

1. add server info in ansible_hosts 
  ```
  testvm ansible_ssh_host=192.168.1.14 ansible_ssh_user=vagrant  ansible_sudo=true

  [auth]
  testvm 
  ```

2. add/edit config in  `host_vars/testvm.yml`

  ```
  mysql_root_password: "mysql-root-password"
  freeradius_db_password: "change-this-to-a-very-strong-password-4Xykokqpdj"     
  freeradius_client_secret: "some-radius-secret"                               
  djra_setup_admin: true                                                         
  djra_admin_user: "admin"                                                       
  djra_admin_password: "change-this-to-a-strong-password"
  ```

3. run the playbook 
  ```
  ansible-playbook  auth.yml  -l testvm
  ```

4. Open  `http://TEST-VM-IP-ADDRESS-OR-DOMAIN-NAME:9002/radmin/`  in your browser, login with djra_admin_user / djra_admin_password . 