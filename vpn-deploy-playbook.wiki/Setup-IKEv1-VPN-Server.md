这篇文章介绍了如何安装 IKEv1 VPN Server (基于 StrongSwan)

# 系统要求
1. Ubuntu 14.04 / Debian 7.0 或者更高版本
2. 拥有 root 权限

# 步骤

1. 在 ansible_hosts 中加入你的服务器信息.
  ``` ini
  testvm ansible_ssh_host=192.168.1.14 ansible_ssh_user=vagrant ansible_sudo=true

  [ipsec]
  testvm
  ```

2. 在 `hosts_vars/testvm.yml` 或 `group_vars/ipsec.yml` 配置如下变量

  ``` yml
  ipsec_enable_ikev1: true 
  ipsec_enable_ikev2: false
  ipsec_enable_l2tp: false
  ipsec_psk: ipsecworks

  ipsec_use_radius: false
  ikev1_users:
    - username: "testuser"
      password: "changeme"
  ```

3. 执行
  ``` bash
  ansible-playbook ipsec.yml -l testvm
  ```
