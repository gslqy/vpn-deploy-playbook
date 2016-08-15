本项目是一些安装配置 VPN 服务或者代理服务的 [Ansible Playbook](http://docs.ansible.com/playbooks.html)。 

如果不是很熟悉 `ansible`, 请先阅读 [准备控制机](/ftao/vpn-deploy-playbook/wiki/%E5%87%86%E5%A4%87%E6%8E%A7%E5%88%B6%E6%9C%BA) 和 [Ansible](http://docs.ansible.com/) 的文档。 

目前包含的模块如下，大部分模块都处于基本可用，但是没有经过详细测试的状态。

* [PPTP](https://github.com/ftao/vpn-deploy-playbook/wiki/配置PPTP-VPN服务器)
* [L2TP](https://github.com/ftao/vpn-deploy-playbook/wiki/Setup-L2TP-VPN-Server--(Using-Strongswan))
* [IKEV1](https://github.com/ftao/vpn-deploy-playbook/wiki/Setup-IKEv1-VPN-Server)
* [IKEV2](https://github.com/ftao/vpn-deploy-playbook/wiki/Setup-IKEv2-VPN-Server)
* [IKEV2 With Let's Encrypt](/ftao/vpn-deploy-playbook/wiki/Setup-IKEv2-VPN-Server-with-SSL-Certs-from-Let's-Encrypt)
* [OpenConnect With Let's Encrypt](/ftao/vpn-deploy-playbook/wiki/Setup-OpenConnect-VPN-Server-with-SSL-Certs-from-Let's-Encrypt)
* [SigmaVPN](https://github.com/ftao/vpn-deploy-playbook/wiki/Setup-SigmaVPN--Tunnel)
* [OpenVPN](/ftao/vpn-deploy-playbook/wiki/Setup-OpenVPN-Server)
* Shadowsocks
* [DNS](https://github.com/ftao/vpn-deploy-playbook/wiki/Setup-DNS-Server)
* [ChinaDNS](https://github.com/ftao/vpn-deploy-playbook/wiki/Setup-ChinaDNS)
* COW
* [HTTPS Proxy](/ftao/vpn-deploy-playbook/wiki/Setup-HTTPS-Proxy-with-SSL-Certificated-from-Let's-Encrypt)
* [APNP (squid + shadowsocks tunnel)](https://github.com/ftao/vpn-deploy-playbook/wiki/Setup-APNP-Server-With-Squid-and-Shadowsocks)