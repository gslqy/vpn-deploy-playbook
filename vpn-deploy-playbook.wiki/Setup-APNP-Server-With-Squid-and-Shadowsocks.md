这篇文章介绍了如何安装 APNP Server . 

# 系统要求
1. 国内一台服务器 
2. 国外一台服务器 
3. ubuntu 14.04 或者更高版本
4. 拥有root权限

# 系统架构

国内服务器 (HTTP 代理服务器) -> 国内服务器 Shadowsocks 客户端  -> 国外服务器 shadowsocks 服务端 -> 国外服务器 Squid -> 目的网站  


# 步骤

1. 在 ansible_hosts 中加入你的服务器信息.  testvm1 应该是国内的服务器， testvm2 应该是国外的服务器.  
  ```
  testvm1 ansible_ssh_host=192.168.1.14 ansible_ssh_user=vagrant  ansible_sudo=true
  testvm2 ansible_ssh_host=192.168.1.15 ansible_ssh_user=vagrant  ansible_sudo=true

  [apnp-frontend]
  testvm1 
  [apnp-backend]
  testvm2 
  ```

2. 配置**国内服务器**的参数，在 `hosts_vars/testvm1.yml` 配置连接到国外服务器的 shadowsocks 连接信息
   ```
   ss_server_host: "192.168.1.15"  #修改为国外服务器 的ip 地址                                                        
   ss_server_port: 8838                                                             
   ss_server_password: "some-password" 
   ```
3. 配置**国外服务器**的参数, 在 `hosts_vars/testvm2.yml` 配置要提供的shadowsocks 服务的端口和密码. 和第二步中配置的端口和密码应该要一致。 
   ```
   shadowsocks_provider: 'libev'   # 一定要用这个版本，因为用到转发模式                                                                                                                     
   shadowsocks_servers:                                                                                                                                    
     default:                                                                       
       port: 8838                                                                 
       password: "some-password"                                 
   ```

4. 执行 
  ```
  ansible-playbook  apnp-backend.yml  -l testvm2
  ansible-playbook  apnp-frontend.yml  -l testvm1
  ```

5. 用浏览器打开 国内服务器的30080 端口, 比如  http://192.168.1.14:30080/ , 页面里面会提供PAC文件地址. 