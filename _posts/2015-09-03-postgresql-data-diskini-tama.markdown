---
layout: post
title: "Postgresql Veri Diskini Taşıma"
modified:
categories: teknik
excerpt:
tags: [pg]
image:
  feature:
date: 2015-09-03T14:22:47+03:00
---

Test edilen ortam: Centos 6.7 postgresql-9.4.1 

#### adımlar;

* screen/tmux gibi bir terminal çoklayıcı kullan.
* postgresql kullanıcısının ev dizinini değiştir. Veriyi taşıyacağın disk
  bölümü, postgre kullanıcısının ev dizini olsun.
* veriyi kullanıcının yeni ev dizinine rsync'le eşleştir. Böylece yeni dizinden
  başlatmazsan hızlıca eski çalışan yerden geri başlatabilirsin.

* * * 
* Çalışan postgresql servisini kapat. veri eşleştirmesine başla.

~~~
su - postgres 
pg_ctl -D /var/lib/pgsql/9.4/data -m immediate stop
vi /var/lib/pgsql/9.4/data/postgresql.conf
data_directory = '/home/pgsql/9.4/data'
exit 
mkdir -p /home/pgsql/9.4/data
chown  postgres: /home/pgsql/ -R
rsync -ah --exclude='base/pgsql_tmp' /var/lib/pgsql/9.4/data/ /home/pgsql/9.4/data/
~~~

* Yeni bir screen'de postgres kullanıcısına ait ev dizinini değiştir
~~~
cp /etc/passwd{,.org}
vi /etc/passwd
  postgres:x:26:26:PostgreSQL Server:/home/pgsql:/bin/bash
~~~

* postgres kullanıcısına tanımlı değişkenleri düzenle

~~~
su - postgres
vi ~/.bash_profile
  :%s/\/var\/lib/\/home/gc
source ~/.bash_profile
echo $PGDATA
~~~

* Eşleştirme bittiğinde postgre'yi başlat;

~~~
pg_ctl -D /home/pgsql/9.4/data/ -l logfile -w start
pg_ctl status -m fast
ps -efd | grep postgres
netstat -tan |grep 5432
stat $PGDATA/postmaster.pid
~~~

* Aksi bir durum olduğunda postgre'yi eski dizinden başlat

~~~
pg_ctl -D /home/pgsql/9.4/data/ -m immediate stop
pg_ctl -D /var/lib/pgsql/9.4/data -l logfile -w start
~~~
