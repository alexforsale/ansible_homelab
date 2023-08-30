KVM Provision
=========

Provision Libvirt virtual machines.

Role Variables
--------------

These variables are defined in [default.yml](./defaults/main.yml)
| Variable Name | Description
| --- | ---
| debian_base_image_name | Debian base image name, part of the image filename.
| debian_base_image_url | The URL for the debian image.
| debian_base_image_sha | The image file checksum.
| ubuntu_base_image_name | Ubuntu base image name, part of the image filename.
| ubuntu_base_image_url | The URL for the ubuntu image.
| ubuntu_base_image_sha | The image file checksum.
| archlinux_base_image_name | Archlinux base image name, part of the image filename.
| archlinux_base_image_url | The URL for the archlinux image.
| archlinux_base_image_sha | The image file checksum.
| iso_dir | Location to store the base image in the remote machine.
| storage_dir | Location for the vm storage.
| libvirt.host_username | The username used in this play (in the remote machine).
| libvirt.pool.name | Name of the storage pool.
| libvirt.pool.type | Name of the storage pool type.
| libvirt.pool.path | Path for the storage pool.
| libvirt.pool.mode | The permission mode for the storage pool.
| libvirt.pool.owner | The user who owns the directory.
| libvirt.pool.group | The group who owns the directory.
| libvirt.pool.autostart | Toggle autostart mode for this pool.
| libvirt.pool.iso | <boolean> if true then this will be the storage for base images.
| libvirt.net.name | name of the virtual network.
| libvirt.net.forward_mode | the mode for network forwarding.
| libvirt.net.bridge_name | the bridge name for the network.
| libvirt.net.stp | Toggle stp for the bridge.
| libvirt.net.delay| Set the delay.
| libvirt.net.ip_address | set the ip address for the network.
| libvirt.net.netmask | set the netmask for the network.
| libvirt.net.dhcp_start | set the start DHCP IP.
| libvirt.net.dhcp_end | set the end DHCP IP.
| libvirt.net.autostart | Toggle autostart for this network.
| libvirt.vm.name | Name of the VM.
| libvirt.vm.arch | The VM architecture.
| libvirt.vm.distro | The distro used (`debian`, `ubuntu`, or `archlinux`).
| libvirt.vm.vcpus | The number of cpu used.
| libvirt.vm.machine | The machine of this vm (`pc-q35-8-1`).
| libvirt.vm.mode | the mode for this vm (`host-model`, and `host-passthrough`)
| libvirt.vm.emulator | The path for the emulator(`/usr/bin/qemu-system-x86_64`)
| libvirt.vm.default_disk_driver_type | The driver type for the first disk(`qcow2`).
| libvirt.vm.default_disk_dev | The device type for the first disk(`vda`).
| libvirt.vm.default_disk_bus | The bus type for the first disk(`virtio`).
| libvirt.vm.ram_mb | The memory for this vm in mb.
| libvirt.vm.root_password | the root password.
| libvirt.vm.network | The network used in this vm.
| libvirt.vm.network_type | The network type (`virtio`).
| libvirt.vm.graphics.type | The graphics type for this vm(`spice`).
| libvirt.vm.videos.type | The video type for this vm(`qxl`).
| libvirt.vm.memballon_model | The memballoon model(`virtio`).
| libvirt.vm.ssh_pub_key | The path for the ssh public key(in the remote machine)

Distro-specific variables:
-------------------------

Archlinux
---------
| Variable Name | Description
| --- | ---
| host_packages | list of packages required for the host.
| nsswitch_hosts | services for libvirt.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - name: Deploys VMs
      hosts: localhost
      gather_facts: yes
      become: yes
      vars:
        libvirt:
          host_username: "{{ lookup('env', 'USER') }}"
          pool:
            - name: ansible_iso_dir
              type: dir
              path: "{{ iso_dir }}"
              mode: "0755"
              owner: 0
              group: 0
              autostart: True
              iso: True
            - name: default
              type: dir
              path: "{{ storage_dir }}"
              mode: "0755"
              owner: 0
              group: 0
              autostart: True
              iso: False
          net:
            - name: ansible_network
              forward_mode: nat
              bridge_name: virt_net1
              stp: "on"
              delay: 0
              ip_address: 10.0.33.254
              netmask: 255.255.255.0
              dhcp_start: 10.0.33.100
              dhcp_end: 10.0.33.200
              autostart: True
          vm:
            - name: vm01
              arch: 'x86_64'
              distro: debian
              vcpus: 1
              machine: 'pc-q35-8.1'
              mode: 'host-passthrough'
              emulator: "/usr/bin/qemu-system-x86_64"
              default_disk_driver_type: qcow2
              default_disk_dev: vda
              default_disk_bus: virtio
              ram_mb: 1024
              root_password: test1234
              network: ansible_network
              network_type: virtio
              graphics:
                - type: "spice"
              videos:
                - type: qxl
              memballoon_model: "virtio"
              ssh_pub_key: "/home/alexforsale/.ssh/id_rsa.pub"
            - name: vm02
              arch: 'x86_64'
              distro: debian
              vcpus: 1
              machine: 'pc-q35-8.1'
              mode: 'host-passthrough'
              emulator: "/usr/bin/qemu-system-x86_64"
              default_disk_driver_type: qcow2
              default_disk_dev: vda
              default_disk_bus: virtio
              ram_mb: 1024
              root_password: test1234
              network: ansible_network
              network_type: virtio
              graphics:
                - type: "spice"
              videos:
                - type: qxl
              memballoon_model: "virtio"
              ssh_pub_key: "/home/alexforsale/.ssh/id_rsa.pub"

      tasks:
        - name: KVM Provision role
          include_role:
            name: kvm_provision
          tags: libvirt

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
