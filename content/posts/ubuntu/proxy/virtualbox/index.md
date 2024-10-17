---
title: VirtualBox
menu:
  sidebar:
    name: VirtualBox
    identifier: ubuntu-proxy-virtualbox
    parent: proxy
    weight: 10
---


In order to access internet from a Ubuntu 22.04 LTS VM instance using VirtualBox in Ubuntu 24.04 LTS, we usually need to setup proxy and network adapter.

For setting up proxy, we need to update both the host machine and the guest VM.




# Setting up proxy

##HOST:
```bash
$ journalctl -xe
kernel: VBoxNetFlt: attached to '4026531840/eno1' / 2c:f0:5d:0c:44:74
kernel: vboxdrv: 0000000000000000 VBoxDDR0.r0
kernel: vboxdrv: 0000000000000000 VMMR0.r0
kernel: vboxnetflt: 0 out of 28552 packets were not sent (directed to host)
```

```bash
$ sudo -I
vi /etc/cntlm.conf
### Add the following line after the existing Listen statement
Listen  10.209.201.25:3128
```

```bash
service cntlm restart
```

```bash
lsof -Pni
```

## GUEST VM:

Setting up proxy for sudo apt
```bash
$ sudo -i
cd /etc/apt
cat > 90-proxy
Acquire::http::Proxy "http://10.209.201.25:3128";
Acquire::https::Proxy "http://10.209.201.25:3128";
```

```bash
vi /etc/environment
http_proxy="10.209.201.25:3128"
https_proxy="10.209.201.25:3128"
```





