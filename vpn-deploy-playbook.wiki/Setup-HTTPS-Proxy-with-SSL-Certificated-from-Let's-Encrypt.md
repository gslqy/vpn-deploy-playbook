这个教程描述了如何使用 [Let's Encrypt](https://letsencrypt.org/) 的服务来自动生成SSL 证书，并且用于 HTTPS Proxy 服务器。 
HTTPS Proxy 服务器基于 `nghttp2` 和 `squid` , 认证采用用户名和密码， 在 squid 层次做。 

### 准备工作
1. 一台公开访问到的服务器， 安装 Ubuntu 14.04
2. 将一个域名指向这台服务器
3. 如果服务器上已经运行着 Web 服务器(占用了443 端口), 请先暂时停止该服务。 
4. 阅读并接受 Let’s Encrypt Subscriber Agreement，你可以下面的地址找到最新版本的协议 https://letsencrypt.org/repository/ . 

### 控制机准备工作

请仔细阅读 [准备控制机](/ftao/vpn-deploy-playbook/wiki/准备控制机)，需要下载安装一些第三方的 Role 。

### 添加服务器，设置服务器变量

inventory file `ansible_hosts`
```
test.vpndeploy.com ansible_ssh_user=root 
```

host_var file `host_vars/test.vpndeploy.com.yml`
```
letsencrypt_email: "webmaster@test.vpndeploy.com"

https_proxy_using_lets_encryt: true                                              
https_proxy_domain: "{{ inventory_hostname }}"   

nghttp2_enable_nghttpx: true                                                     
nghttpx_listen_port: 9443                                                        
nghttpx_backend_port: 3128                                                       
                                                                                 
nghttpx_cert_source: "remote"                                                    
nghttpx_remote_private_key_file: '/etc/letsencrypt/live/{{ https_proxy_domain }}/privkey.pem'
nghttpx_remote_certificate_file: '/etc/letsencrypt/live/{{ https_proxy_domain }}/fullchain.pem'
                                                                                 
squid_require_auth: true                                                         
squid_auth_method: 'basic'                                                       
squid_users:                                                                                                                     
  - name: demo                                                                   
    password: demopass    
                                                                                                         
#if you want to use radius as authentication backend 
#squid_auth_method: 'radius'                                                                        
squid_radius_server:                                                             
  host: some.radius.server 
  secret: 'soem.radius.secret'
```

### run the playbook 
  ```
  ansible-playbook  https-proxy.yml  -l test.vpndeploy.com
  ```
  the https proxy should be up and running on port 9443