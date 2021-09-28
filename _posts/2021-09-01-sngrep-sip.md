---
layout: post
title:  "SNGREP SIP Ladder"
date:   2021-09-01 13:34:00 +0700
author: irfanrifai
categories: [ VoIP, Linux, SIP]
image: assets/images/sngrep.png
tags: [featured]
---
Seringkali ketika lagi troubleshoot VoIP nemu obstacle gimana caranya narik log SIP nya yang mudah dan gampang. Dulu awal - awal pas troubleshoot pake tools sejuta umat, wireshark. Cuma lama - lama jadi kesulitan sendiri karena wireshark yang berat dan harus nunggu sesi call selesai utuk troubleshoot.

Akhirnya dapat masukkan dari kawan sekantor buat pake tools baru yaitu NGREP dan SNGREP. Pertama - pertama  disuruh pake NGREP dulu,  lama - lama baru pake SNGREP. Karena emang keluarnya duluan NGREP. 

NGREP sendiri singkatan dari "Network Grep". Disini saya menggunakan distro Centos 7, untuk installasinya sendiri tinggal menggunakan RPM.

Cara installnya sebagai berikut :

 ```yum install ngrep```  


Setelah terinstall, anda bisa menggunakan NGREP melalui command line server anda

 ```ngrep -W byline -d eth0 port 5060```  


Hasilnya kurang lebih sebagai berikut :
{% highlight generic %}
 U 185.53.88.40:58777 -> 10.10.10.56:5060 #3  
 INVITE sip:900442038076746@10.10.10.56:5060 SIP/2.0.  
 Via: SIP/2.0/UDP 185.53.88.40:58777;branch=z9hG4bK1763926594.  
 Max-Forwards: 70.  
 From: <sip:61@10.10.10.56:5060>;tag=1590280767.  
 To: <sip:900442038076746@10.10.10.56:5060>.  
 Call-ID: 1842141676-1666533667-1306961472.  
 CSeq: 1 INVITE.  
 Contact: <sip:61@185.53.88.40:58777>.  
 Content-Type: application/sdp.  
 Allow: ACK, BYE, CANCEL, INFO, INVITE, MESSAGE, NOTIFY, OPTIONS, PRACK, REFER, REGISTER, SUBSCRIBE, UPDATE, PUBLISH.  
 User-Agent: sdthsrhstuksz.  
 Content-Length: 206.  
 .  
 v=0.  
 o=61 16264 18299 IN IP4 192.168.1.83.  
 s=call.  
 c=IN IP4 192.168.1.83.  
 t=0 0.  
 m=audio 25282 RTP/AVP 0 101.  
 a=rtpmap:0 pcmu/8000.  
 a=rtpmap:8 pcma/8000.  
 a=rtpmap:101 telephone-event/8000.  
 a=fmtp:101 0-11.  
 #  
 U 10.10.10.56:5060 -> 185.53.88.40:58777 #4  
 SIP/2.0 100 Trying.  
 Via: SIP/2.0/UDP 185.53.88.40:58777;branch=z9hG4bK1763926594.  
 From: <sip:61@10.10.10.56:5060>;tag=1590280767.  
 To: <sip:900442038076746@10.10.10.56:5060>.  
 Call-ID: 1842141676-1666533667-1306961472.  
 CSeq: 1 INVITE.  
 User-Agent: VoIP My ID Mediaserver.  
 Content-Length: 0.  
 .  
 #  
 U 10.10.10.56:5060 -> 185.53.88.40:58777 #5  
 SIP/2.0 403 Forbidden.  
 Via: SIP/2.0/UDP 185.53.88.40:58777;branch=z9hG4bK1763926594.  
 From: <sip:61@10.10.10.56:5060>;tag=1590280767.  
 To: <sip:900442038076746@10.10.10.56:5060>;tag=U3gB9U86j2a6Q.  
 Call-ID: 1842141676-1666533667-1306961472.  
 CSeq: 1 INVITE.  
 User-Agent: VoIP My ID Mediaserver.  
 Accept: application/sdp.  
 Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY.  
 Supported: timer, path, replaces.  
 Allow-Events: talk, hold, conference, refer.  
 Content-Length: 0.  
 .  
 #  
{% endhighlight %}

Sekarang ngetrap trafik SIP yang masuk ke server jadi lebih mudah dan trubleshoot juga bisa jadi lebih gercep.

Namun menggunakan NGREP ada kekurangannya, jadi kita agak kerepotan jika trafik call yang masuk ke server sudah berjibun. Maka dari itu akhirnya sama temen dianjurin menggunakan SNGREP kalau saja trafik SIP udah banyak.

SNGREP sendiri sebenarnya adalah kembangan dari NGREP, namun dengan tambahan S didepannya. S sendiri adalah singakatan dari SIP. Yang mana berarti SNGREP ini khsusus dibuat untuk mentrap trafik SIP.

Untuk installasinya lakukan sebagai berikut :
Menambahkan file di ```/etc/yum.repos.d/irontec.repo``` dengan isi :
{% highlight generic %}
 [irontec]  
 name=Irontec RPMs repository  
 baseurl=http://packages.irontec.com/centos/$releasever/$basearch/  
{% endhighlight %}


Tambahkan public key untuk repo irontec
{% highlight generic %}
 rpm --import http://packages.irontec.com/public.key  
{% endhighlight %}

Jalankan update lalu install SNGREP
{% highlight generic %}
 yum check-update -y  
 yum install sngrep -y  
{% endhighlight %}


Setelah berhasil terinstall, untuk menjalankannya cukup dengan perintah ```sngrep```

![Screenshot](https://www.otakudang.org/wp-content/uploads/2018/05/sngrep-00.jpg)

Selanjutnya tinggal klik - klik aja pada bagian transaction yag mau dilihat, contohnya :

![Screenshot](https://www.otakudang.org/wp-content/uploads/2018/05/sngrep-01.jpg)

Jadi kira - kira seperti itu untuk penggunaan SNGREP dan NGREP, penggunaanya sama - sama mudah tergantung kebutuhan. Kalau trafik sedikit dan mau ambil plain teks bisa langsung pake NGREP. Kalau trafiknya sudah banyak, SNGREP juga bisa jadi opsi.

Semoga bermanfaat. 