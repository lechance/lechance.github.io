---
title: Virtualization using KVM + QEMU + libvirt
date: 2022-03-27
categories: Linux
tags:
- kvm
- qemu
- libvirt
- linux
---
Last updated on 2022-03-27 • Tagged under  [# virtualization](https://www.dwarmstrong.org/tags/virtualization/) [# debian](https://www.dwarmstrong.org/tags/debian/) [# linux](https://www.dwarmstrong.org/tags/linux/)

Setup a stack of virtualization tools on a Linux host (Debian) for creating and managing virtual machines (VMs).

## Let's go!

[KVM](https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine) (Kernel-based Virtual Machine) is built into the Linux kernel and handles the CPU and memory details. [QEMU](https://en.wikipedia.org/wiki/QEMU) (Quick EMUlator) emulates the various hardware components of a physical machine. Finally, [libvirt](https://wiki.archlinux.org/title/Libvirt) provides the tools for creating and managing VMs. I use `virt-manager` and `virsh` as graphical and console interfaces respectively.

First, check whether the host computer's CPU supports virtualization ...

```bash
$ egrep -c '(vmx|svm)' /proc/cpuinfo
```

A result of `1` or more means the CPU supports virtualization extensions. A result of `0` means it does not, though double-check the BIOS and see if the extensions are available and just need to be enabled.

## Install

Install packages and add username (example: `foo`) to the `kvm` and `libvirt` groups ...

```bash
$ sudo apt install qemu-system-x86 libvirt-clients libvirt-daemon libvirt-daemon-system virtinst virt-manager bridge-utils
$ sudo usermod -aG kvm foo
$ sudo usermod -aG libvirt foo
```

Log out and back in.

Default directory to hold VM images is `/var/lib/libvirt/images`. Since I have `root` and `home` on separate partitions, and I have much more storage space in `home`, I create an `images` directory there, plus an `isos` directory to hold Linux install images, and create symbolic links to them in `/var/lib/libvirt` ...

```bash
$ mkdir /home/foo/{images,isos}
$ chown :kvm /home/foo/images
$ sudo rmdir /var/lib/libvirt/images
$ sudo ln -s /home/foo/images /var/lib/libvirt/images
$ sudo ln -s /home/foo/isos /var/lib/libvirt/isos
```

## Permissions

Create `~/.config/libvirt/libvirt.conf` ...

```bash
$ mkdir ~/.config/libvirt
$ sudo cp -rv /etc/libvirt/libvirt.conf ~/.config/libvirt/
$ sudo chown foo: ~/.config/libvirt/libvirt.conf
```

Open the file and set ...

```bash
uri_default = "qemu:///system"
```

For storage file permissions, open `/etc/libvirt/qemu.conf` and set the `user` to your username and `group` to `libvirt` ...

```bash
user = "foo"
group = "libvirt"
```

Restart `libvirt` service ...

```bash
$ sudo systemctl restart libvirtd
$ systemctl status libvirtd
```

## Create VM

Create a VM using `virt-manager`. Click the icon to add a new VM, and work through the series of dialog boxes to configure.

Here I've created two: `bullseye` and `mint` ...

![virt-manager](https://www.dwarmstrong.org/img/virt-manager.png)

Note during the creation of a VM, the network selection option defaults to `Virtual network 'default': NAT`. If the `default` network is not active, virt-manager will prompt to start it. Otherwise, start manually with ...

```bash
$ virsh net-start default
```

Each VM (in `default` network) will be a member of `192.168.122.0/24`, with an IP address in the range of `192.168.122.2` to `192.168.122.254`, and are accessible via SSH from the host.

## Resize a VM guest window

In the virt-manager console window, navigate to `Edit->Preferences->Console` and set:

- ```
  Graphical console scaling
  ```

   

  to

   

  ```
  Always
  ```

  - `Resize guest with window` to `On`

Inside the VM, install a **spice agent** ...

```bash
$ sudo apt install spice-vdagent
```

Reboot the VM.

In the VM guest window, navigate to `View->Scale display` and check `Always` and `Auto resize VM with window`.

Now click-and-drag the window edge to resize display, or use `xrandr` to set display size.

## Video acceleration

A Linux Mint VM in `virt-manager` complained about lack of video acceleration and was *very* slow.

Using *VirtIO* its possible to create a virtual 3D accelerated GPU and pass through the hardware capabilities of the VM host's graphic card (which, in my case, is integrated Intel graphics).

With the VM shutdown, open the VM hardware details window. Click on `Video QXL` and set `Model: Virtio`, check the box for `3D acceleration`, and `Apply` the modifications.

![virtio](https://www.dwarmstrong.org/img/libvirt-virtio.png)

Click on `Display Spice`, set `Type: Spice Server` and `Listen type: None`, check the box for `OpenGL`, and `Apply` the modifications.

![opengl](https://www.dwarmstrong.org/img/libvirt-opengl.png)

Start the VM. Much improved!

## Virsh and virt-clone

Some useful commands:

- start network - `virsh net-start <network_name>`
- list networks - `virsh net-list`
- list of VMs - `virsh list --all`
- start, reboot, and shutdown VM - `virsh start <VM>`, `virsh reboot <VM>`, and `virsh shutdown <VM>`
- show IP addresses - `virsh net-dhcp-leases <network_name>`
- clone VM and create storage image - `virt-clone -o <VM> -n <new_VM> -f /var/lib/libvirt/images/<new_VM>.qcow2`

## Helpful

- [Linux Hypervisor Setup](https://octetz.com/docs/2020/2020-05-06-linux-hypervisor-setup/)
- [libvirt](https://wiki.archlinux.org/title/Libvirt)
- [VirtIO 3D Acceleration](https://ryan.himmelwright.net/post/virtio-3d-vms/)