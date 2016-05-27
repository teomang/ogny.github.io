---
layout: post
title: Ruby Öğreniyorum
modified: true
categories: teknik
comments: true
excerpt:
tags: [ruby]
image:
  feature:
date: 2016-05-26T11:00:15+03:00
---

Hayatım son zamanlarda oldukça zevkli bir yöne doğru gidiyor.

Bugün ilk kodumu yazdım. Ruby'de fonksiyonları öğrenirken, kendi kendime bir
seyler yapabileceğim aklıma geldi.  Ne kadar basit olursa olsun, bana kendime
göre ve yeni bir şey yapma hislerini yaşattığı için bugünümü özel kıldı.

```ruby
def turgut_uyar_unutulmaz_dortlukleri(*dizeler)
  dize1, dize2, dize3, dize4 = dizeler
  puts "Şimdi dolaşıp duruyor #{dize1}
#{dize2} bir duygu olarak
Doğudan batıya bir #{dize3} halinde
Çılgın ve #{dize4}"
end
turgut_uyar_unutulmaz_dortlukleri("aramızda","kıpkırmızı","güz","hüzünlü")
```

* İkinci günde fonksiyonda yaptığım toplama işleminin sonucunu `return` ile dönüyorum.

```ruby
def ikinci_fonksiyonum(x, y)
  puts "Bu bir toplama işlemi, sayılar: #{x} ve #{y}"
  return x + y
end
toplam_deger = ikinci_fonksiyonum(1, 2)
puts "fonksiyon değerlerin toplamını, #{toplam_deger}'ü döndürüyor"
```
