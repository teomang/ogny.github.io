---
layout: post
title: Pomodoro Pomodoro Dedikleri...
modified:
categories: teknik 
excerpt:
tags: []
image:
  feature:
comments: true
date: 2017-05-26T11:48:01+03:00
---

Pomodoro disipliniyle çalışmak için ( 25 dk. çalış, 5 dk. mola ver ),
terminal'den çalışan bir app aradım, python/zsh/nodejs app'larını denedim,
hiçbirisi istediğim verimlilikte değildi, sonunda kendim hazırladım, son derece
de basit oldu, crontab'ı şu şekilde düzenledim;

```
*/30 * * * *      /usr/bin/notify-send "Work"
25 * * * *      /usr/bin/notify-send "Break"
55 * * * *      /usr/bin/notify-send "Break"
```

Her yarım saatte bir Work, saati her 25 ve 55 geçtiğinde ise Break gönderiyor.
böylece pomodoro döngüsü sağlanıyor.


Kısacası, eğer işiniz basit gibi duruyorsa, önce yapılmısına bakmak yerine
kendiniz yaptığınızda daha verimli oluyormuş.

