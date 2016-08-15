
# System Requirement
1. ubuntu 14.04 
2. be super user 

# Steps

1. add your server info in ansible_hosts 
  ```
  testvm ansible_ssh_host=192.168.1.14 ansible_ssh_user=root 

  [spdy-proxy]
  testvm 
  ```

2. copy `group_vars/spdy_proxy.yml.example` to `group_vars/spdy_proxy.yml`
3. edit `group_vars/spdy_proxy.yml` , point `nghttpx_private_key_file` and `nghttpx_certificate_file` to the right files. 
   ```
   nghttp2_with_spdylay: true
   nghttp2_enable_nghttpx: true
   nghttpx_listen_port: 9443
   nghttpx_backend_port: 3128
   nghttpx_private_key_file: 'spdy.example.com.key'
   nghttpx_certificate_file: 'spdy.example.com.crt'
  ```

4. run the playbook 
  ```
  ansible-playbook  spdy-proxy.yml  -l testvm
  ```

5. the spdy proxy is up & running on port 9443