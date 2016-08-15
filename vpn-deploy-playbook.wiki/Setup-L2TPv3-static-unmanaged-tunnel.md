本文介绍如何在两台机器之间建立一个 L2TPv3 static unmanaged tunnel。
实现主要参考文档 @klzgrad 的这个Gist: [朴素VPN：一个纯内核级静态隧道](https://gist.github.com/klzgrad/5661b64596d003f61980)

## 说明
运行 `l2tp-eth.yml` 这个playbook 会
  * 在国内服务器配置 l2tp eth tunnel 的客户端，安装 chinadns 解决DNS， 配置 chnroutes 让国外流量走 tunnel . 
  * 在国外服务器配置 l2tp eth tunnel 的服务端，配置 IP Forward . 

由于会更改路由表，请谨慎运行，在一些情况下面，可能会导致无法登录到服务器。 
目前 tunnel 配置部分没有持久话下来，所以如果网络出现问题，无法访问，可以重启一下机器就可以恢复了。 

## 步骤

1. 在 ansible_hosts 中加入你的服务器信息, 修改 ｀test_cn1.example.com｀ , `test_us1.example.com` 为对应的服务器的IP 地址。 
  ```
  test_cn1 ansible_ssh_host=test_cn1.example.com ansible_ssh_port=22 ansible_ssh_user=root l2tp_eth_client_remote_ip=test_us1.example.com
  test_us1 ansible_ssh_host=test_us1.example.com ansible_ssh_port=22 ansible_ssh_user=root

  [l2tp-eth-client]
  test_cn1

  [l2tp-eth-server]
  test_us1
  ```

2. 执行playbook 
  ```
  ansible-playbook  l2tp-eth.yml
  ```
