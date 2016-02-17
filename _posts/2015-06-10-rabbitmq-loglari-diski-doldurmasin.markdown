---
layout: post
excerpt:
title: "Rabbitmq Log'lari Diski Doldurmasin"
modified:
categories: sistem
tags: [rabbitmq]
image:
  feature:
date: 2015-06-10T11:06:33+03:00
---

Centos6.6'da rabbitmq paket yöneticisiyle kurulduğunda, logrotate için hazır
yapılandırmayla geliyor. Ancak hazır yapılandırma, rabbitmq'ya herhangi bir
erişim sorunu olduğunda log dosyasının diski doldurmasına mani olacak şekilde
oluşturulmamış. Bu sorunu şöyle çözdüm;

* Kurulum

```
rpm --import http://www.rabbitmq.com/rabbitmq-signing-key-public.asc
curl -L -O http://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm
curl -L -O http://www.rabbitmq.com/releases/rabbitmq-server/current/rabbitmq-server-3.5.2-1.noarch.rpm
yum install -y erlang-solutions-1.0-1.noarch.rpm
rabbitmq-server-3.5.1-1.noarch.rpm erlang
chkconfig rabbitmq-server on
```



* Centos'ta logrotate uygulamasi log'lari gunluk olarak rotate edecek sekilde
  konumlandirilmis. Saatlik logrotate yaptirmak istersek
  /etc/cron.daily/logrotate dosyasini /etc/cron.hourly/ altina tasiyabiliriz.

```
mv /etc/cron.daily/logrotate /etc/cron.hourly/ 
```

* Ancak diskteki bos yer saatlik kullanimda bile dolma riski tasiyorsa 15 dk.da
  bir calisacak sekilde cron dosyasi olusturabiliriz.

```
crontab -e
*/15 * * * * /usr/sbin/logrotate /etc/logrotate.d/rabbitmq-server
*/15 * * * * /usr/sbin/logrotate /etc/logrotate.d/rabbitmq-server_1
```

* rabbitmq-server dosyaları:

```
mv /etc/logrotate.d/rabbitmq-server{,.org}
cat << EOF >> /etc/logrotate.d/rabbitmq-server
/var/log/rabbitmq/startup_log {
        size 100k
        create 0644 rabbitmq rabbitmq
        missingok
        rotate 5
        compress
        compresscmd /usr/bin/gzip
        compressext .gz
        delaycompress
        notifempty
        sharedscripts
        maxage 10
        postrotate
            /sbin/service rabbitmq-server rotate-logs > /dev/null
        endscript
}
EOF
```
```
cat << EOF >> /etc/logrotate.d/rabbitmq-server_1
/var/log/rabbitmq/*.log {
        size 100k
        create 0644 rabbitmq rabbitmq
        missingok
        rotate 5
        compress
        compresscmd /usr/bin/gzip
        compressext .gz
        delaycompress
        notifempty
        sharedscripts
        maxage 10
        postrotate
            /sbin/service rabbitmq-server rotate-logs > /dev/null
        endscript
}
EOF
```

* log dosyalari sildiniz ancak `df -h` ciktisinda halen disk dolu gorunuyor. Bu
  durum inode'lardan (dugumler) kaynaklaniyor. Silinen dosyalari bulalim; log
  dosyalarini truncate edelim. Ornek olarak;

```
lsof | grep deleted
find /proc/*/fd -ls 2> /dev/null | grep '(deleted)'
150797741    0 l-wx------   1 root     root           64 Feb  9 17:18 /proc/6102/fd/1 -> /var/log/rabbitmq/startup_log.1\ (deleted)
150797745    0 l-wx------   1 root     root           64 Feb  9 17:18 /proc/6104/fd/1 -> /var/log/rabbitmq/startup_log\ (deleted)

:>/proc/6102/fd/1
:>/proc/6104/fd/1
```

* Tum rabbitmq log'larini kapatabiliriz. bunun icin;

```
vi /etc/rabbitmq/rabbitmq.config
# heartbeat'in altina ekleyebilirsiniz.
{log_levels,[{channel,none}]}
```
