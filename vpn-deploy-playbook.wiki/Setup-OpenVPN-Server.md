这篇文章介绍了如何安装 OpenVPN Server 

## 系统要求
1. Ubuntu 14.04 / Debian 7.0 或者更高版本
2. 拥有 root 权限

## 步骤

1. 在 ansible_hosts 中加入你的服务器信息.
  ``` ini
  test.vpndeploy.com  ansible_ssh_user=root

  [openvpn]
  test.vpndeploy.com
  ```

2. 在 `host_vars/test.vpndeploy.com.yml` 或 `group_vars/openvpn.yml` 配置如下变量

  ``` yaml
  openvpn_cert_source: "gen"  #auto generate the certificates, this is the default value 
  openvpn_client_conf_file: "./client-config.ovpn"
  ```
  如果你希望使用 `radius` 认证的方式, 你可以配置 `radius server`
  ```yaml
  openvpn_use_radius: true
  openvpn_radius_servers:
    - host: some.radius.server
      secret: "some-radius-secret"
  ```

3. 执行
  ``` bash
  ansible-playbook openvpn.yml -l test.vpndeploy.com 
  ```
  注意 `openvpn_client_conf_file` 参数，客户端配置文件会下载到这里，你可以导入到你的OpenVPN 客户端中。 