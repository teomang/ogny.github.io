---
layout: post
title: Swap Alanı Yönetimi
author: Orkun Gunay
modified:
categories: teknik
excerpt:
tags: [linux, ram]
image:
  feature:
comments: true
date: 2017-09-29T16:09:07+03:00
patat:
  incrementalLists: true
  slideLevel: 3
...

### Swap alani ne ise yarar, gerekli mi?
Linux fiziksel RAM'i page adindaki yiginlara bolerek kullanir. Swap islemi, bu
page'lerin diske kopyalanmasi ve page'lerin bosaltilmasi islemine denir. 

### Swap islemini 2 sebepten oturu gereklidir
- Sistem fiziksel ram miktarindan fazlasina ihtiyac duydugunda, kernel ram'deki
  az kullanilan page'leri swap alanina yazar ve ihtiyac olan hafizayi
  uygulamaya tahsis eder.

- Uygulamalar ayaga kalkarken yuksek miktarda page'e ihtiyac duyar, ancak bu
  alanlara sonrasinda ihtiyac duymaz. Sistem bunlari swap'a atar.

### Swap Turleri

#### Disk Swap
- Disk bolumu (Partition)

```bash
# Fdisk ile
fdisk /dev/hdb
Command: a
Partition number (1-4): 1
#And I make the second partition of type swap:
Command: t
Partition number (1-4): 2
Hex code: 82
#Changed system type of partition 2 to 82 (Linux swap)      
Command: p
```
- Dosya

```bash
fallocate -l 1G /mnt/1GB.swap
dd if=/dev/zero of=/mnt/1GB.swap bs=1024 count=1048576
mkswap /mnt/1GB.swap
swapon /mnt/1GB.swap
vi /etc/fstab 
/mnt/1GB.swap  none  swap  sw 0  0
# Check that the swap file was created.
swapon -s
```

#### Ram swap
- zRam: Cpu cache'inde kullanilmayan alan buyukse tercih edilebilir. 
Kaynak: https://wiki.voidlinux.eu/Zram
