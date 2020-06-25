Ansible Role: ocp_modign_ova_deploy
=========

This role was put together to help assist performing Openshift 4 deployments on VMWare ESXi where static IPs are required but where DHCP is allowed to give out temporary addresses. The role creates a base64 encoded ignition file for each Red Hat CoreOS server that contains the static specific IP and network configuration as required and then provisions the VMs in the cluster injecting the created base64 encoded ignition file. 

**FYI:** this role has not been fully tested yet.

Requirements
------------

The role expects that 'vanilla' igniton files have already been created as elements of them will be consumed. 

Role Variables
--------------

Available variables are listed below, along with example values (see vars/main.yml):

The ign directory is where the previously created 'vanilla' ignition files are, and the output directory is where you want the files that will be created to go. 
```yaml
ign_directory: /home/vagrant/ocp4
output_directory: /home/vagrant/ignition/roles/createigns/files 
``` 

Path to OVA
```yaml
ova_path: /home/vagrant/rhcos-4.4.3-x86_64-vmware.x86_64.ova 
```

The webserver host and port that hosts the bootstrap ignition file.
```yaml
webserver: 10.80.158.5:8080
``` 

Network configuration of the estate
```yaml
netmask: 255.255.255.240
cidr: 28
gateway: 10.80.158.1
dns: 10.80.158.5
domain: jftest.com
```

ESXi information.
```yaml
esxi_host: 139.178.72.66
esxi_user: root
esxi_pass: n4{D2^U5Gg
esxi_dc: ha-datacenter
esxi_datastore: datastore1
```

Openshift cluster name
```yaml
cluster_name: ocp4
```

Hostname and required IP address of the bootstrap node.
```yaml
bootstrap:
  - hostname: bootstrap.ocp4.jftest.com
    ip: 10.80.158.6
```

Hostname, IP and function (worker or master) of the required nodes in the cluster.
```yaml
nodes: 
  - hostname: master0.ocp4.jftest.com
    ip: 10.80.158.7
    function: master
```

Dependencies
------------

pyVmomi is required to be installed on the host where the role is ran for the VMware modules to work.

Example Playbook
----------------

```yaml
---
- name: Generate Modified Configuration Files 
  hosts: localhost
  roles: 
   - ocp_modign_ova_deploy
```

License
-------

MIT License

Author Information
------------------

Thie role was created in 2020 by James Force

https://sysadm.life/
