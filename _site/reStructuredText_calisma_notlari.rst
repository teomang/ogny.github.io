================================
reStructuredText Calışma Notları
================================

:date: 2014-11-23 21:20
:comments: true
:categories: markup
:tags: reStructuredText

Altbaslik
---------

- Yeni paragraf icin 2 paragraf arasindaki bos satir

bu ilk paragraf

bu da ikincisi

- girintilemede onemli olan kac karakter iceriden basladigin degil,
  girintilenecek tum satirlarin ayni bosluk sayisindan basladigi

- baslikta belirleyici olan karakter degil, baslik uzunluguyla karakterin
  ayni uzunlukta olmasi

- italik icin *yildiz* koyu renk icin **cift yildiz**

``standard bosluklu ifadeler``

- tablolar

============	===============================
ilk satir	ilk sutun
============	===============================
ikincisi	ikinci sutun

ucuncu		ucuncu sutun
============	===============================

- Tanim listeleri

nedir
    bu soru nesnenin neligine dair sorulur. felsefede bu soru ile
    baslanir ve bu sorunun cevabi uzerinden yeni sorular aranir.

nasildir
    bu soru varolusa dair olmayip seyin epistemolojisine dair sorulan
    sorulardir. sorunun olasi cevaplari bilimin alanina girmektedir.

:yazarlar:
    orkun gunay
    ogny
    safruhani

:surum: 0.1 2014/11/23
:Adanma: kendime.

**baslik**

:Date: 22014-11-27

Altbaslik
---------

        `yorum biceminde yazildi`

.. code:: sh

        #!/bin/bash
        for i in $(ls); do 
                ffmpeg -i $i -q:a 0 -map 
        a $i.mp3
        done

.. code:: cpp
  
    #include <iostream>
    using std::cout;
    using std::endl;
    int main()
    {
        cout << "Hello";
        cout << endl;
        return 0;
    }




* Listeler

bu alt liste
bu ikinci liste nesnesi
