---
layout: post
title: Linux'ta Windows Ve Mac OS Uyumlu USB Biçimlendirme
modified:
categories: teknik 
excerpt:
tags: [parted]
image:
  feature:
date: 2016-08-04T15:43:22+03:00
comments: true
---

#### Ortam: Debian 8.5 Jessie

#### Gereksinimler:

```bash
sudo apt install -y exfat-fuse exfat-utils
```

#### Yapılandırma:

```bash
sudo parted /dev/sdb
mkpart primary 0% 100%
set 1 msftdata
sudo mkfs.exfat -n <görünecek_isim> /dev/sdb1
```
