Setup SigmaVPN tunnel between two linux box . 


1. add servers in  ansible_hosts 
  ```
  vm1 ansible_ssh_host=192.168.1.14 ansible_ssh_user=vagrant  ansible_sudo=true
  vm2 ansible_ssh_host=10.8.1.14 ansible_ssh_user=vagrant  ansible_sudo=true
  [sigmavpn]
  vm1
  vm2
  ```

2. install keygen tool in one of the machine 
   ```
   ansible-playbook sigmavpn.yml -l vm1 -t install 
   ```

2. generate keypair for vm1 & vm2 
   ```
   ansible  vm1 -m shell -a "naclkeypair"
   ``` 
   result looks like 
   ```
   vm1 | FAILED | rc=10 >>
   PRIVATE KEY: RPIVATE_KEY_FOR_VM1
    PUBLIC KEY: PUBLIC_KEY_FOR_VM1
   ```

   ```
   ansible  vm1 -m shell -a "naclkeypair"
   ``` 
   result looks like 
   ```
   vm1 | FAILED | rc=10 >>
   PRIVATE KEY: RPIVATE_KEY_FOR_VM2
    PUBLIC KEY: PUBLIC_KEY_FOR_VM2
   ```

3. config client (vm1)
   edit  hosts/vm1.yml 
   ```
   sigmavpn_peers:
      - name: p1
        proto_publickey:  PUBLIC_KEY_FOR_VM2
        proto_privatekey: RPIVATE_KEY_FOR_VM1
        peer_remoteaddr: 10.8.1.14
        peer_remoteport: 7654
        peer_localaddr: 0.0.0.0
        peer_localport: 4567
        virtual_ip: 10.6.0.2
        peer_ip: 10.6.0.1
   ```

4. config server (vm2)
   edit  hosts/vm2.yml 
   ```
   sigmavpn_peers:
      - name: p1
        proto_publickey:  PUBLIC_KEY_FOR_VM1
        proto_privatekey: RPIVATE_KEY_FOR_VM2
        peer_remotefloat: 1
        peer_localaddr: 10.8.1.14
        peer_localport: 7654
        virtual_ip: 10.6.0.1
        peer_ip: 10.6.0.2
   ```

5. setup peers
   ```
   ansible-playbook sigmavpn.yml -l vm1,vm2
   ```

6. you can login into vm1, and try ping 10.6.0.1 