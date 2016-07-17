Ansible Red Hat Gluster 3 Install
===================================

Playbook to install and configure Red Hat Gluster Storage Server

INFORMATION
-----------

This playbook can do

* Subscribe to Red Hat Network
* Enable repositories to install Red Hat Gluster Storage
* Configure network interfaces (see network)
* Install and configure Red Hat Gluster Storage


Requirements
------------

You will need ansible and all the required subscriptions for RHEL 7 and Red Hat Gluster Storage 3. 


Role Variables
--------------

All the variables are on group_vars/all

Each Red Hat Gluster Storage host vars on host_vars/"hostname"


Host Variables
------------------

Each host has a var on host_vars dir

# Network management 
mgmt_interface: eth0

# Install Gluster Server
glusterfs_install_server: True
 
# Install Gluster Client
glusterfs_install_client: False


**Network Interfaces**

See host_vars/glusterstore01 for more examples

This playbook suport any types of network configuration 

**IMPORTANT**

network_reset var reset all network interfaces configured using with caution

Red Hat Subscribe
-----------------

Set use_rhel to True and configure rhel_subscription_user and rhel_subscription_pass
to playbook subscribe to RHN

Set rhel_pool_id to respository ID to Red Hat Gluster Storage

Red Hat Repositories
--------------------

If you want you can add more Red Hat repositories with variable rhel_repos

LVM
---

**CAUTION** 
if reset_lvm is True, remove all LV, VG and LV from devices


Set options to Physical Disks

pv_disks:
  - vg_name: stg1-internal # VG Name
    thin_name: tp1 # Thin VG Name
    disk: /dev/vdb # Disk
    pv_dataalignment: 1280 # Data Alignment
    vg_extentsize: 128 # Extent Size 
    lv_chunk: 1280 # Chunk Size
    lv_metadatasize: 16G # Metadata Size

Gluster Bricks
--------------

gluster_bricks:
  - brick_name: stg1-internal
    virtualsize: 100G
    vg_name: stg1-internal
    thin_name: tp1
  - brick_name: stg1-external
    virtualsize: 100G
    vg_name: stg1-external
    thin_name: tp2

Gluster Volumes
---------------

glusterfs_volumes:
  - name: Volume1
    owner: root
    group: root
    replicas: 2
    force: True
    brick:
      - "{{ mount_point }}stg1-internal"
  - name: Volume2
    owner: root
    group: root
    replicas: 2
    force: True
    brick:
      - "{{ mount_point }}stg1-external"

License
-------

MIT

Author Information
------------------

Marcos Amorim <mamorim@redhat.com>
=======
# rh-gluster
