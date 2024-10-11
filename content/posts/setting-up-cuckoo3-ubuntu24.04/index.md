---
title: "Setup Cuckoo3 in Ubuntu 24.04 LTS"
date: 2024-10-10T08:06:25+06:00
description: Setup Cuckoo3 in Ubuntu 24.04 LTS
menu:
  sidebar:
    name: Setup Cuckoo3 in Ubuntu 24.04 LTS
    identifier: cuckoo
    weight: 30
author:
  name: Pinku Deb Nath
  image: /images/author/pinku.png
math: true
---

Cuckoo is a malware analysis tool using sandboxed environments. 

Cuckoo2 is stable, has a big community, lots of Github stars, but it is not actively maintained since 2022 and requires Python2.

While Cuckoo3 is relatively new but is actively developed and supports Python 3.10 (preferably in Ubuntu 22.04 LTS for easier setup).

Hence, we will try out how to setup Cuckoo3.

Cuckoo3 provides a setup script but it is for Ubuntu 22.04 LTS and Python 3.10 by default. \

Since we will be using Ubuntu 24.04 LTS, We need to modify the setup script a bit to make it run. 
We also need to install missing system library headers (since they are used by a dependent package) and install Python 3.10 manually.


