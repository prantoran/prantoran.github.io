---
title: KVM Notes
menu:
  notes:
    name: KVM
    identifier: notes-kvm
    weight: 10
---

# KVM Notes



## Networking

```bash
sudo virsh net-list --all
sudo virsh net-start default
sudo virsh net-autostart default
```

## Check libvirtd

```bash
kvm-ok
sudo systemctl start libvirtd
sudo systemctl enable libvirtd
virsh list --all
```

## Storage pools

```bash
sudo virsh pool-define-as --name vmpool --type dir --target /var/lib/libvirt/images
sudo virsh pool-build vmpool
sudo virsh pool-start vmpool
sudo virsh pool-autostart vmpool
```

```bash
virsh pool-list --all
virsh pool-info default
virsh pool-refresh default
```

## Virsh

domain = VM

```bash
virsh # connect to a local QEMU system to manage local KVM machines
virsh list # lists all running domains (VMs)
virsh list --all # list all configured VMs
```

### Manage VM's power state
```bash
virsh start domain|id
virsh shutdown domain|id
virsh reboot domain|id
```

## Ref
- https://idroot.us/install-kvm-ubuntu-24-04
- https://www.tecmint.com/manage-kvm-storage-volumes-and-pools/
- https://libvirt.org/storage.html
