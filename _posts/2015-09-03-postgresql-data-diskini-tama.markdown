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

Centos 6.7 dağıtımda postgresql-9.4.1 ile çalıştım.
postgresql verisini sunucuya bağlı başka bir diske taşımak için şu yöntemi
izledim;

* postgresql kullanıcısının ev dizini değiştirilir, 
* veri kullanıcının yeni ev dizinine kopyalanır. Böylece kullanıcıya ait tüm
  değişkenler kullanılmaya devam edilir. Örneğimizde disk /home'a bağlı

* * * 
* Veri tabanı sunucusu kapatılır. veri, postgres kullanıcısının yeni ev
  dizinine kopyalanır.

~~~
su - postgres 
pg_ctl stop -m fast 
exit 
mkdir /home/pgsql
chown  postgres: /home/pgsql
rsync -ah  --progress /var/lib/pgsql/ /home/pgsql/
~~~

* Hizli olunmasi gereken durumlarda, tum veriyi tasimak iyerine ilgili db'yi
  export/import edip baslatmak daha uygun bir cozum.

~~~
PGUSER=postgres pg_dump -h "sunucu_ip" -U "veri_tabani_sahibi" "veri_tabani_adi" > db.out
psql dbname < db.out
~~~

* postgres kullanıcısına ait ev dizini değiştirilir. 

~~~
cp /etc/passwd{,.org}
vi /etc/passwd
  postgres:x:26:26:PostgreSQL Server:/home/pgsql:/bin/bash
~~~


* postgres kullanıcısında tanımlı değişkenler düzenlenir. Kullanacağımız
  `$PGDATA` değişkenini çağırıp kontrol etmekte fayda var.

~~~
su - postgres
vi ~/.bash_profile
  :%s/\/var\/lib/\/home/gc
source ~/.bash_profile
echo $PGDATA
~~~


* Taşıma bittiyse postgres başlatılır;

~~~
pg_ctl start -D $PGDATA -m fast
pg_ctl status
ps -efd | grep postgres
~~~

* veriyi replike ediyorsanız, çalışıp çalışmadığı kontrol etmek için; hızlı güncellenen bir
  tabloyu hem master'da hem de slave'de açıp son kaydın aynı olup olmadığını
  kontrol edebilirsiniz.

~~~
su - postgres
psql -d <db_adi>
select * from <tablo_adi> order by id desc limit 10;
~~~
