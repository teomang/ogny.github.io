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
```

* rabbitmq-server dosyasi:

```
mv /etc/logrotate.d/rabbitmq-server{,.org}
cat << EOF >> /etc/logrotate.d/rabbitmq-server
/var/log/rabbitmq/*.log {
        copytruncate
        missingok
        rotate 5
        size 100k
        compress
        delaycompress
        notifempty
        sharedscripts
        postrotate
            /sbin/service rabbitmq-server rotate-logs > /dev/null
        endscript
}
EOF
```

* Tum rabbitmq log'larini kapatabiliriz. bunun icin;

```
vi /etc/rabbitmq/rabbitmq.config
# heartbeat'in altina ekleyebilirsiniz.
{log_levels,[{channel,none}]}
```
