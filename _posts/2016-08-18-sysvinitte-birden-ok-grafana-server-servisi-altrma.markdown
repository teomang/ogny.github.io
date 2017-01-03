---
layout: post
title: SysVinit'te Birden çok Grafana-server Servisi çalıştırma
modified:
categories: teknik 
excerpt:
tags: [centos6, SysVinit]
image:
  feature:
date: 2016-08-18T11:02:21+03:00
comments: true
---

#### Ortam
* Centos 6.7
* grafana-2.5.0-1

```bash
mkdir /var/lib/grafana_ver2
mkdir /var/log/grafana_ver2
cp -r /etc/grafana /etc/grafana_ver2
cp -r /usr/share/grafana /usr/share/grafana_ver2
cp /etc/init.d/grafana-server /etc/init.d/grafana-server_ver2
cp /usr/sbin/grafana-server /usr/sbin/grafana-server_ver2
cp /etc/sysconfig/grafana-server /etc/sysconfig/grafana-server_ver2

# Konfigurasyon dosyalarındaki parametler ver2 uzantıya göre güncellenir
# ek olarak, grafana.ini dosyasindaki port degistirilir.
vim /etc/grafana_ver2/grafana.ini \
/etc/sysconfig/grafana-server /etc/init.d/grafana-server

/etc/init.d/grafana-server_ver2 start
chkconfig grafana-server_ver2 on
```
