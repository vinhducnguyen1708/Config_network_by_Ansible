# Config_network_by_Ansible

*Playbook này dùng để thực hiện cấu hình IP tĩnh cho Ubuntu16 và Centos7*

- Bước 1: Chỉnh sửa file inventory
```ini
[ubuntu]
192.168.30.36 ansible_python_interpreter=/usr/bin/python3 #Tùy chọn do Ansible của tôi cài không giao tiếp được với python2 trên Ubuntu
[centos]
192.168.20.39 
```
- Bước 2: Chỉnh sửa file main playbook `config-network-interfaces`
Đối với Ubuntu
```yml
- hosts: ubuntu
  gather_facts: True
  gather_subset: network
  become: yes
  roles: 
    - role: networkint
      network_ifaces:
      - device: ens3
        bootproto: static 
        ip: 192.168.30.36
        netmask: 255.255.255.0
        gateway: 192.168.30.1
        dns:
         - 8.8.8.8
         - 8.8.4.4 
      - device: ens4
        bootproto: static
        ip: 192.168.20.199
        netmask: 255.255.255.0
```
Đối với centos 7
```yml
- hosts: ubuntu
  gather_facts: True
  gather_subset: network
  become: yes
  roles: 
    - role: networkint
      network_ifaces:
      - device: eth0
        bootproto: static 
        ip: 192.168.20.39
        netmask: 255.255.255.0
        type: ethernet
        gateway: 192.168.30.1
        dns:
         - 8.8.8.8
         - 8.8.4.4 
      - device: eth2
        bootproto: static
        ip: 192.168.30.99
        netmask: 255.255.255.0
        type: ethernet
```

Bước 3: thực hiện chạy lệnh
```
 ansible-playbook -i <path>/network_config  <path>/config-network-interfaces.yml
```