这篇文章介绍了如何安装 L2TP over IPSEC VPN Server , 基于 StrongSwan 和 xl2tpd .

# 系统要求
1. Ubuntu 14.04 / Debian 7.0 或者更高版本
2. 拥有 root 权限

# 步骤

1. 在 ansible_hosts 中加入你的服务器信息.
  ``` ini
  test.vpndeploy.com  ansible_ssh_user=root

  [ipsec]
  test.vpndeploy.com
  ```

2. 在 `hosts_vars/test.vpndeploy.com.yml` 或 `group_vars/ipsec.yml` 配置如下变量

  ``` yml
  ipsec_enable_ikev1: false 
  ipsec_enable_ikev2: false
  ipsec_enable_l2tp: true
  ipsec_psk: ipsecworks
  ipsec_use_radius: false
  l2tp_users:
    - username: "testuser"
      password: "changeme"
  ```

3. 执行
  ``` bash
  ansible-playbook ipsec.yml -l testvm
  ```
