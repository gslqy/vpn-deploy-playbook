这篇文章介绍了如何安装 IKEv2 VPN Server . 

# 系统要求
1. ubuntu 14.04 或者更高版本
2. 拥有root权限

# 步骤

1. 在 ansible_hosts 中加入你的服务器信息.
  ```
  testvm.example.com ansible_ssh_host=192.168.1.14 ansible_ssh_user=vagrant  ansible_sudo=true

  [ipsec]
  testvm.example.com
  ```

3. 在 `host_vars/testvm.example.com.yml` 配置如下变量

  ```
  ipsec_enable_l2tp: false
  ipsec_enable_ikev1: false
  ipsec_enable_ikev2: true
  ikev2_users:
    - username: "user1"
      password: "pass1"
    - username: "user2"
      password: "pass2"

  ipsec_gen_ios8_profile: true   #自动生成ios8 profile
  ipsec_ios8_include_password: true  ＃在profile 中包含密码
  ```

3. 执行
  ```
  ansible-playbook  ipsec.yml  -l testvm.example.com
  ```

# 客户端 - iOS 8 

在你的 iOS 设备上使用 Safari 访问  `http://testvm.exmaple.com:9441/ios8-profile/user1.mobileconfig`  安装描述文件。 之后在VPN 列表中就会出现对应的VPN 项目， 选中后连接就可以了。 

# 客户端 - OS X
1. 下载[strongswan客户端](https://download.strongswan.org/osx/)
2. 将服务器上的 `/etc/ipsec.d/certs/server_cert.pem` 文件（http://you_server:9441/osx/server_cert.pem)下载到本地。
3. 打开 **Keychain Access.app**, 选中左侧中的【系统】，点击菜单：【文件】-> 【导入】。然后双击导入的证书，在【信任】栏下选择【始终信任】

# 注意事项
如果你没有完整的域名，想直接使用IP地址连接到服务器，请设置如下的变量. 
```
ipsec_use_ip_as_domain: true
```
如果生成的iOS 中配置文件不正确， 可以考虑设置 `ipsec_domain`, `ipsec_remote_address` 为可以正确访问的域名或者IP 地址, 并且保证两者是一致的。
来保证生成的配置文件中的地址或者域名是正确的。 
```
ipsec_domain: "ip or domain of the server"
ipsec_remote_address: "ip or domain of the server" 
```