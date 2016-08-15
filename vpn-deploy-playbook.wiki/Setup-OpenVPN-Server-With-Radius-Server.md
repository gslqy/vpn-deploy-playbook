# Setup OpenVPN Server  (draft)

1. Setup a radius server following the instruction in [Setup Freeradius Server](/ftao/vpn-deploy-playbook/wiki/Setup-Freeradius-Server) , take note of the radius server address and secret 


2. put your openvpn server info in ansible_hosts
  ``` ini
  testvm ansible_ssh_host=192.168.1.14 ansible_ssh_user=root

  [openvpn]
  testvm
  ```

3. setup following variables in  `hosts_vars/testvm.yml` or `group_vars/openvpn.yml` 
  change radius host and secret to the right value. 
  ``` yml
  openvpn_use_radius: true
  openvpn_radius_servers:
    - host: radius.server.ip.address
      secret: "some-radius-secret"
  openvpn_client_conf_file: "/tmp/testvm-client.ovpn"
  ```

4. execute 
  ``` bash
  ansible-playbook openvpn.yml -l testvm 
  ```

5. the server should be up and running , you can now import  "/tmp/testvm-client.ovpn" to favorite openvpn client and connect to the server . 
  