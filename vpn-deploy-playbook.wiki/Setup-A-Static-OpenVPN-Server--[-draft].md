**NOT WORKING YET , DONT READ **


# System Requirement
1. ubuntu 14.04 

# Steps

1. add your server info in ansible_hosts 
  ```
  testvm ansible_ssh_host=192.168.1.14 ansible_ssh_user=root 

  [openvpn-static-server]
  testvm 
  ```

2. copy `group_vars/openvpn-static-server.yml.example` to `group_vars/openvpn-static-server.yml`
3. edit `group_vars/openvpn-static-server.yml` to fit your need . The default value should work just ok. 
   ```
   #generate the client config to this location
   openvpn_client_config_file: 'static-client.ovpn'

   #advanced options
   openvpn_conf_name: 'static-ovpn-server'
   openvpn_port: 9194
   openvpn_proto: udp
   openvpn_local_ip: '10.11.0.1'
   openvpn_peer_ip: '10.11.0.2'
  ```

4. if your server is behind firewall , you may need to set `openvpn_public_ip` to the right public ip.
   create a file named , `host_vars/testvm.yml`  (match the hostname defined in your ansible_hosts) 
   ```
   #filename: host_vars/testvm.yml 
   openvpn_public_ip: '1.2.3.4'  #change this to the public ip of the server
   ```

5. run the playbook 
  ```
  ansible-playbook  openvpn-static.yml  -l testvm
  ```

6. you can now import  'static-client.ovpn' to your openvpn client, and connect to it . 