这个教程描述了如何使用 [Let's Encrypt](https://letsencrypt.org/) 的服务来自动生成SSL 证书，并且用于 IKEv2 VPN 服务器。 

### 准备工作
1. 一台公开访问到的服务器， 安装 Ubuntu 14.04
2. 将一个域名指向这台服务器, 并且这个域名要和下面设置的 `ipsec_domain` 一致
3. 如果服务器上已经运行着 Web 服务器(占用了443 端口), 请先暂时停止该服务。 
4. 阅读并接受 Let’s Encrypt Subscriber Agreement，你可以下面的地址找到最新版本的协议 https://letsencrypt.org/repository/ . 

### 控制机准备工作

请仔细阅读 [准备控制机](/ftao/vpn-deploy-playbook/wiki/准备控制机)，需要下载安装一些第三方的 Role 。

### 添加服务器，设置服务器变量

inventory file `ansible_hosts`
```
test.vpndeploy.com ansible_ssh_user=root 
[ipsec]
test.vpndeploy.com
```

host_var file `host_vars/test.vpndeploy.com.yml`

```
letsencrypt_email: "webmaster@test.vpndeploy.com"
ipsec_domain: "test.vpndeploy.com"
ipsec_enable_ikev2: true
ikeve2_users: 
  - username: demo
    password: dmeo
```
当然你如果使用 radius 认证，这里也可以配置成
```
ipsec_use_radius:  true                                                       
ipsec_radius_servers:                                                                                                        
   - host: some.radius.server                                                            
     secret: some-radius-secret                                                 
```

### 执行Playbook `ipsec-lte.yml`
```
ansible-playbook ipsec-lte.yml 
``` 