---
layout: post
title: Nasıl Twitter Kullanıyorum
modified:
categories: teknik 
excerpt:
tags: [twitter,ruby]
image:
  feature:
date: 2016-08-17T16:18:50+03:00
---

Hobilerimi önemsiyorum. Herhangi bir şey uğruna, onlardan vazgeçmek durumunda
kalmak istemiyorum.  Son dönemde, telefonda internetsiz kaldım; bu sebepten boş
zamanlarımda twitter'a bakma sürem daraldı. Kısa zamanda twitter'da takip
ettiklerimden haberdar olabilmek için bir çözüm bulmam gerekiyordu.  T twitter
istemcisini kullanma alışkanlığı edindim ve bunu biraz geliştirdim. Bu yazıda
bahsetmek istiyorum.

#### Ortam
* Debian Jessie
* ruby-2.1.1
* [RVM](https://rvm.io/)
* [T Twitter CLI](http://sferik.github.io/t/)
* `gem install --user-install executable-hooks`

t client ile takip ettiğiniz listelerdeki son gönderileri belli sayıda
görüntüleyebiliyoruz. Bundan yararlanarak aşağıdaki gibi bir betik oluşturdum.

```bash
#!/bin/bash
source $HOME/.profile
TARIH=$(date +"%Y-%m-%d_%H:%M")
DOSYA="$HOME/gunun_tweetleri/$TARIH"
t="$HOME/.rvm/gems/ruby-2.1.1/bin/t"
touch $DOSYA 
$t list timeline @orkungunay/hobi -n 15 > $DOSYA; \
$t list timeline @orkungunay/sanat -n 10 >> $DOSYA; \
$t list timeline @orkungunay/tech -n 10 >> $DOSYA; \
$t list timeline @orkungunay/ekoloji -n 5 >> $DOSYA; \
$t list timeline @orkungunay/ezilenler -n 5 >> $DOSYA; \
$t list timeline @orkungunay/haber -n 5 >> $DOSYA; \
$t list timeline @orkungunay/politics -n 5 >> $DOSYA; \
$t list timeline @orkungunay/kiymetliler -n 10 >> $DOSYA
```

bu betiği cronjob'la günde 3 kere toplayacak şekilde düzenledim.
```bash
0 9,13,17 * * * <betik_adı>  >/dev/null 2>&1
```

**Not**: betiğe profil dosyasını koymazsanız, cronjob'ta aşağıdaki hata oluşuyor.
```bash
/usr/bin/env: ruby_executable_hooks: No such file or directory
```
[buradan](../../images/t_twitter_ciktisi.png) ekran görüntüsüne ulasabilirsiniz. 

