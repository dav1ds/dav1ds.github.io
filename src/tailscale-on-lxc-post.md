---
title: "2025 09 07 Proxmox LXC config for tailscale "
date: 2025-09-07T13:44:28+09:00
---
Ajouter les lignes ci-dessous dans etc/pve/lxc/CTID(112).conf:

```
lxc.cgroup2.devices.allow: c 10:200 rwm
lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file


```
