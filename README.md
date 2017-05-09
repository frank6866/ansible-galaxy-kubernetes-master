Kubernetes Master Standalone
============================
This role is used to deploy kubernetes master in standalone mode. Supporting features as follows:

* deploying kubernetes master
* kubernetes configuration for connecting to etcd with TLS


Inventory file demo
-------------------

```
k8s-1 ansible_ssh_host=10.10.10.11 ansible_ssh_port=22 ansible_ssh_user=centos kube_master_ip=10.10.10.11


[kubernetes-master]
k8s-1

```

Role Variables
--------------

The whole example of group_vas/kubernetes-master.yml file as follows:

```
ip_hostname_in_etc_hosts:
  - ip: 10.10.10.11
    hostname: k8s-1
    state: present
  - ip: 10.10.10.12
    hostname: k8s-2
    state: present
  - ip: 10.10.10.13
    hostname: k8s-3
    state: present

kube_master_etcd_prefix: /k8s
kube_master_service_cluster_ip_range: 10.254.0.0/16
kube_master_etcd_servers: https://10.10.10.11:2379,http://10.10.10.12:2379,http://10.10.10.13:2379
kube_etcd_tls_enabled: "true"
kube_etcd_cert_file_path: etcd.crt
kube_etcd_key_file_path: etcd.key
kube_etcd_cacert_file_path: cacert.pem

```

The Kubernetes cluster need to visit each other by hostname, if the hosts is not registered in DNS server, you can set **ip_hostname_in_etc_hosts** variable to add the id and hostname pair in /etc/hosts file.


Example Playbook
----------------

```
- hosts: kubernetes-master
  become: true
  roles:
    - frank6866.linux_basic
    - frank6866.kubernetes-master
```


License
-------

MIT

