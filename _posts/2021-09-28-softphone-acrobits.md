---
layout: post
title:  "Acrobits, Softphone SaaS untuk Personal dan UC Provider"
date:   2021-09-28 17:00:00 +0700
author: irfanrifai
categories: [ VoIP, Softphone ]
image: assets/images/acrobits-banner.png
tags: [featured]
---

### Budaya Kerja Mobile
Internet telah merevolusi cara individu dan bisnis dalam berkomunikasi. Telepon rumah tradisional sudah jarang kita jumpai lagi, bahkan telepon seluler pun beralih ke layanan Voice over Internet Protocol (VoIP) di beberapa belahan dunia.

Tren kerja jarak jauh selalu meningkat, dan COVID-19 semakin mempercepatnya. 75% pekerja di Indonesia bekerja secara WFH atau daring pada 2021, jadi masuk akal memodernisasi tempat kerja untuk mengakomodasi perubahan budaya kerja ini.

Misalnya, mengapa kita harus tetap terikat pada meja kita sepanjang waktu? Dengan mengganti telepon meja yang besar dan menggabungkan komunikasi itu ke ponsel cerdas Anda dan perangkat seluler lainnya, Anda dapat mencapai tingkat produktivitas yang lebih tinggi daripada sebelumnya di mana pun Anda berada.

Adopsi *softphone* ini menjadi lebih umum, terutama di sektor bisnis dimana manfaat dari sistem komunikasi yang dapat diakses dan efisien sangat penting dalam lingkungan yang selalu kompetitif.

### Problematika 
Sebelumnya, setiap provider berusaha membuat *softphone* yang sesuai untuk produk mereka. Baik membangunnya dari *scratch* ataupun memilih library yang sudah ada.
Salah satunya berbasis kode sumber terbuka seperti library milik [linphone](https://linphone.org) yang dapat digunakan untuk membangun sebuah aplikasi *softphone*. Namun sampai disini, kompleksitas dalam membuat sebuah aplikasi *softphone* tidak berhenti sampai disini saja.

Tantangan pertama dalam pembuatan aplikasi *softphone* harus melingkupi fungsionalitas dari layanan VoIP yang diberikan oleh provider. Dimana satu provider dan provider lainnya memiliki behaviour layanan yang berbeda. Sebagai contoh, bisa saja sebuah provider memiliki fitur *conferencing* sedang satu yang lain tidak. Atau memiliki fitur lain dan yang tidak ada diprovider lainnya. Provider harus memikirkan seberapa jauh mereka akan mendeliver dan menerjemahkan setiap fitur yang mereka miliki kedalam platfrom softphone sehingga dengan mudah pelanggan dapat menggunakannya.

Selanjutnya softphone adalah bagian besar dari usaha branding. Kenapa demikian? Softphone terpasang pada perangkat pelanggan. Pelanggan menggunakan layanan dari VoIP provider melalui platform *softphone*.

Usaha untuk memberikan fungsionalitas, fitur, dan pengalaman terbaik dalam penggunaan sebuah aplikasi *softphone* menjadi faktor kunci dalam kesuksesan dalam sebuah layanan VoIP dan *UCaaS*. *Softphone* menjadi titik terminasi antara layanan dan pelanggan. Mungkin platform *softswitch* yang digunakan sudah canggih dan memiliki banyak fitur, namun tidak akan berarti apa - apa apabila fitur - fitur tersebut tidak dapat dirasakan pelanggan karena perangkat atau platfrom end-user yang mereka gunakan tidak bisa bekerja secara maksimal. Dan *softphone* adalah salah satu faktor besarnya.

Berikut adalah pendapat saya mengenai poin besar harus jadi perhatian provider dalam pembuatan sebuah *softphone* :

#### Pemilihan *underlying communication stack*. 
Apakah menggunakan teknologi *SIP*, *WebRTC*, atau *konvergen*. Ada banyak opsi library *communication stack* yang bisa dipilih oleh sebuah provider. Sebelum pergi untuk memulai membuat sebuah aplikasi *softphone* maka provider perlu memilih *commstack* yang sesuai dengan infrastruktur *softswitch* yang mereka gunakan dengan tujuan fungsionalitas dan fitur - fitur yang ada pada infrastruktur *softswitch* dapat digunakan secara maksimal melalui *softphone* yang dibuat.
![SIP Stack vs WebRTC Stack](/assets/images/sip-vs-webrtc.png)


#### Pemrosesan aplikasi *background*. 
Ini adalah poin problematik krusial dan menjadi salah satu pengalaman saya dalam memilih sebuah aplikasi *softphone*. Sebuah *softphone* terhubung dengan sebuah *softswitch* atau *SIP Server* dengan metode registrasi. Apabila registrasi ini terputus, maka *softphone* tersebut akan offline. Ketika sebuah *softphone* offline maka pengguna dari *softphone* tersebut tidak akan bisa menerima panggilan. Pengguna akan bisa menerima panggilan hanya ketika aplikasi tersebut dibuka oleh pengguna. Sesuatu yang sirna ketika anda memiliki sebuah infrastruktur solid namun pengguna tidak bisa menerima panggilan hanya karena *softphone* yang tidak bisa mengatasi situasi end-user ini.

Ada opsi dengan cara melakukan *pemaksaan* untuk membuat *softphone* selalu berjalan di background task dengan harga yang harus dibayar adalah konsumsi sumber daya perangkat menjadi bertambah. Terdengar seperti ```swiss army knife``` akan tetapi cara ini bukanlah cara yang baik.

#### Kompatibilitas perangkat.
Banyak tools yang bisa anda gunakan untuk membuat aplikasi *softphone* yang bisa membuat sekaligus untuk platfrom Android maupun IOS. Pertanyaannya adalah, aplikasi *softphone* memiliki *underlying communication stack* yang kita bahas diatas. Memasukkan fungsionalitas *commstack* tidak akan semudah ketika anda membuat sebuah aplikasi yang bersifat sistem dan data. *Real time communication* adalah ```different beasts```

### Acrobits Cloud Softphone
[Acrobits](https://acrobits.net) merupakan sebuah perusahaan asal Cekoslovakia yang menawarkan solusi *softphone* berbasis cloud dan memberikan fitur *whitelabeling* untuk user enterprise. Acrobits menyediakan per-user sekali bayar yang dapat dibeli perseorangan dari [Google Play Store](https://play.google.com/store/apps/details?id=cz.acrobits.softphone.alien) maupun [Apple App Store](https://apps.apple.com/us/app/acrobits-softphone/id314192799) dengan harga 9USD (Rp. 100.000), dan juga enterprise untuk provider dengan mekanisme *SaaS* (Software as a Service).

Yang membuat *Acrobits* berbeda adalah, mereka sudah mengcover poin besar *softphone* yang saya sebutkan diatas. Acrobits mengadopsi *commstack* yang konvergen dan menerapkannya dalam produk *softphone* mereka. Untuk masalah backgrounding aplikasi, Acrobits menggunakan *firebase* untuk android dan *APN* untuk IOS. Desain arsitektur mereka terbilang salah satu terobosan dalam menyelesaikan permasalahan *softphone*. Yang menarik bagi saya adalah mereka menerapkan proxy yang bernama [Acrobits SIP Instance Server](https://doc.acrobits.net/sipis/index.html) yang berfungsi sebagai *push notification server*. Kenapa pendekatan dan terobosan ini unik adalah karena ketika anda memasukkan fungsionalitas *push notification* kedalam *softphone* yang anda buat. Maka anda juga harus memasangkan fungsionalitas untuk trigger *push notification* didalam *softswitch* anda atau SIP Server anda. 

Saya pernah melakukan ini dengan menambahkan fungsi trigger push notification dengan menulis kustom logik kedalam SIP Server. Dimana saya harus mencatat setiap *push notification token* yang ada pada device pengguna kedalam database, lalu ketika panggilan terjadi program saya akan mencolek Google Firebase untuk membangunkan device pengguna. Fyi saya menggunakan library linphone. 

Hal ini yang dilihat sebagai peluang oleh Acrobits dan membuat terobosan untuknya. Mereka melakukan proxy registration melalui [Acrobits SIP Instance Server](https://doc.acrobits.net/sipis/index.html) sehingga setiap provider tidak perlu repot - repot merubah SIP Server atau softswitch mereka untuk menghadirkan fungsionalitas *push notification*.

Berikut adalah *softphone* Acrobits saya yang teregister ke SIP Server.

![Detail Registrasi](/assets/images/2021-09-28-acrobits-softphone/skrinsut-1.png)

Dari informasi diatas didaptkan IP Address 167.99.48.91 yang dari keterangannya merupakan *Acrobits SIPIS*, saya mencoba melihat dimanakah IP tersebut terpasang. Saya menemukan untuk versi personal subscription akan menggunakan Acrobits SIPIS yang mereka pasang di DigitalOcean.

![Detail Registrasi](/assets/images/2021-09-28-acrobits-softphone/skrinsut-2.png)

### Bagaimana Acrobits Bekerja

Berikut adalah paket INVITE yang dikirim oleh SIP Server ke *Acrobits SIPIS Proxy*.
```
2021/09/30 10:28:43.827572 167.99.48.91:29761 -> ip.add.re2:5060
SIP/2.0 180 Ringing
Via: SIP/2.0/UDP ip.add.re2;branch=z9hG4bK461.61fab67073ce613f8cdab4fc5c666727.0
Via: SIP/2.0/UDP ip.add.re1:11000;received=ip.add.re1;rport=11000;branch=z9hG4bKSU8Ut6c06NXmc
Record-Route: <sip:ip.add.re2;lr=on;ftag=pDmeSt20e8atj>
Contact: <sip:pikancutes@10.65.7.3:29761>
From: "02139710003" <sip:02139710003@irfanrifai.sipserver.com>;tag=pDmeSt20e8atj
Call-ID: 02660e02-d1cc-4c93-8405-1e4cc5ac21bd
CSeq: 41932469 INVITE
To: <sip:pikancutes@irfanrifai.sipserver.com>;tag=9770491E7AC9F0891A63660417F6F5BB
Allow: ACK, BYE, CANCEL, INFO, INVITE, NOTIFY, OPTIONS, REFER
Supported: replaces, path
X-Sipis-Push-Status: Alerting-Device
User-Agent: Acrobits SIPIS
Content-Length: 0

```
Dari informasi log tracing yang saya urutkan. Saya melakukan penalaran bagiamana Acrobits mengatur proses panggilan dengan *push notification*. Diagram panggilan dengan *push notification* oleh *Acrobits Softphone* dan *Acrobits SIPIS Proxy*.

![Acrobits Architecture](/assets/images/2021-09-28-acrobits-softphone/acrobits-diagram.png)

### Kesimpulan
Dari hasil dan perspektif yang telah saya coba. Acrobits mampu bekerja secara handal, dimana saya hampir tidak pernah melewatkan satu panggilan masuk pun. Arsitektur *push notification* yang mereka gunakan bekerja maksimal disemua jenis SIP Server dan Softphone tidak perduli jenis dan tipe dari platform infrastruktur yang digunakan. 

Saya akan berusaha berkala mengupdate tulisan ini apabila saya menemukan sesuatu hal lain yang menarik dari Acrobits ini baik dari sisi keuntungan dan kekurangannya.

Referensi :
1. [Acrobits](https://acrobits.net)
2. [WebRTC vs SIP](http://blog.kundansingh.com/2014/12/webrtc-vs-sipsdp.html)