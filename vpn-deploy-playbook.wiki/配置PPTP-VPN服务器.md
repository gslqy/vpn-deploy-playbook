这篇文章介绍了如何安装 PPTP 服务器。

# 系统要求
1. ubuntu 12.04 或者更高版本
2. 拥有root权限

# 步骤

1. 在 ansible_hosts 中加入你的服务器信息.
  ```
  testvm ansible_ssh_host=192.168.1.14 ansible_ssh_user=vagrant  ansible_sudo=true

  [pptp]
  testvm 
  ```

2. 如果不使用radius 服务器做认证，在 `host_vars/testvm.yml` 配置如下变量

  ```
  pptp_use_radius: false
  pptp_users:                                                                     
    - username: "user1"                                                                                                            
      password: "password1"                                                           
    - username: "user2"                                                           
      password: "password2" 
  ```

3. 如果你使用radius 服务器做认证，在 `host_vars/testvm.yml` 中配置如下变量
  ```
  pptp_use_radius: true
  pptp_radius_servers:
    - host:  ip.of.radius-server-1   #请填写radius 服务器的IP 地址或者域名
      secret: some-radius-secret   #请填写radius 服务器的密钥                                                                
    - host:  ip.of.radius-server-2 
      auth_port: 5678               #如果服务器使用自定义的端口，请配置auth_port 和 acct_port 
      acct_port: 5679
      secret: some-radius-secret     
  ```
3. 执行
  ```
  ansible-playbook  pptp.yml  -l testvm
  ```

