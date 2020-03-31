change-cluster
=========

This role aims to change the nodes that are active within the same pool. In a traditional scenario, this could be controlled through the balancer "monitors", in the case of BIG IP, but, due to the particularity of the application, this switching is necessary.

Requirements
------------

Role tested in Ansible 2.9.2, Python 2.7, BIG-IP 12.1.3.5 and BIG-IP 13.1.0.1

Role Variables
--------------

defaults/main.yml:
```
- balancerName: bigip_device
- balancerUser: bigip_user
- balancerPass: bigip_password
- poolApp1: pool_app1
- poolApp2: pool_app2
- poolServerPort: member_port
- app1ClusterA:
  - list_ip_or_fqdn_pool_members
- app1ClusterB:
  - list_ip_or_fqdn_pool_members
- app2ClusterA:
  - list_ip_or_fqdn_pool_members
- app2ClusterB:
  - list_ip_or_fqdn_pool_members
```

Example Playbook
----------------

```
- name: "Change Cluster App1 and App2"
  hosts: your_inventory
  gather_facts: true
  connection: local
  vars_prompt:
     - name: "disablingClusterApp1"
       prompt: "Which APP1 cluster do you want to DISABLE? [ A | B ]"
       private: no
       register: disablingClusterApp1
     - name: "disablingClusterApp2"
       prompt: "Which APP2 cluster do you want to DISABLE? [ A | B ]"
       private: no
       register: disablingClusterApp2              
#---> Invalid condition
  pre_tasks:
    - name: Invalid condition
      fail:
        msg: "The option typed is invalid, use only A or B"
      when: disablingClusterApp1 != "A" and disablingClusterApp1 != "B"
    - name: Invalid condition
      fail:
        msg: "The option typed is invalid, use only A or B"
      when: disablingClusterApp2 != "A" and disablingClusterApp2 != "B"      
  roles:
  - change-cluster
```

License
-------

BSD

Author Information
------------------

Github: [vinealvees](#https://github.com/vinealvees/)