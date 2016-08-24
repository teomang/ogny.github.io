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
mkdir /var/lib/grafana_yeni
mkdir /var/log/grafana_yeni
cp -r /etc/grafana /etc/grafana_yeni
cp -r /usr/share/grafana /usr/share/grafana_yeni
cp /etc/init.d/grafana-server /etc/init.d/grafana-server_yeni
cp /usr/sbin/grafana-server /usr/sbin/grafana-server_yeni
cp /etc/sysconfig/grafana-server /etc/sysconfig/grafana-server_yeni

# Konfigurasyon dosyalarındaki parametler yeni uzantıya göre güncellenir
# ek olarak, grafana.ini dosyasindaki port degistirilir.
vim /etc/grafana_yeni/grafana.ini \
/etc/sysconfig/grafana-server /etc/init.d/grafana-server

/etc/init.d/grafana-server_yeni start
chkconfig grafana-server_yeni on
```
