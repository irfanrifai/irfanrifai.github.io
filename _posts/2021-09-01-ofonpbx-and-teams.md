---
layout: post
title:  "Panggilan Antar Ekstensi Microsoft Teams dan Ofon CloudPBX"
date:   2021-09-01 13:15:57 +0700
categories: voip directrouting
--- 

Sebenarnya penambahan nomor ekstensi pada Microsoft Teams masih belum memungkinkan. Sebagai cara lain, hal tersebut dapat diakali pada SBC Direct Routing. SBC Direct Routing lah yang akan mentranlasikan mapping dari nomer ekstensi pada Phone System dengan DID pada setiap usernya.

Salah satu contoh integrasi yang memungkinkan adalah menghubungkan Microsoft Teams dengan Ofon SmartPBX. Dimana baik SmartPBX dan Microsoft Teams sama sama sistem teleponi berbasis cloud.

![Screenshot](https://1.bp.blogspot.com/-BpCtjhwsfyg/XuE4pLUSR0I/AAAAAAAADUs/fnz7SH-Sm2Yd35e71KCGYNTmh-z4-bg6QCEwYBhgLKtMDAL1OcqzyixlnIJDhLMZgXpKLKJ34-Noa7anvNC3DhUuWFwm9ewX6hbseBcJCrLN7D6Rfd2ZrVHDhE4MYsKUeMVETrcsnMCDdWlcUXB-Yj-4Qa1xga8fRFRUieyyFbmYG0HKAckYe2OEBCYEbC0bxCbDZBJRdqGJNDEAUlZwk55wNKiApW5Gp5ud-PnO3vbXvjnlQ-rL1WkBa6hCBgs0eQ8i0y1Vndx5WUYWhdQp3jpcQpE7HNrxaNDunAgRMUE2vJ8z6zrSXVlZm2f1VngFWK00yVY32N1YNEBEO1PJg0rD8AU0cnX2a5nkGYyOcjxgxiL_hMY9mZJ7DK9tcDDy9WYGlCtqJlrcjXn_XRGcyI3UdoisWBbI2JoEtsCqYF-4UVoMopcSK-e4CM2zX_t6oNpI1dB4LQLBxSPpoicTmRHSsgPINSIxhQKbJlddUluvU1AJR8A6xa9q1Q6whx1TesjkKffMkAscaxQgz6vwqtYOSy8C_CW6aQJNA6os4j1rS4KC2S5t0CW8z-P_vgLv7JiiIk3lZbwM5osDc-Y8YGGiYgwRws6K0DPcNwDKEx6YNAVRS4LEUnIzKxCW0Z_Pzl3GmbH-JlmRGMDnNiNlZWAcivScwyPmE9wU/s640/Integrasi%2BMSTeams%2Bdan%2BSmartPBX.png)

## Prasyarat :
-  Blok ekstensi dari SmartPBX dan Microsoft Teams sudah didefinisikan sebelum dilakukan integrasi. Hal ini bertujuan agar memudahkan pengaturan routing pada kedua sistem teleponi. Pada contoh, blok ekstensi pada SmartPBX adalah ```11XX``` dan blok ekstensi pada Microsoft Teams dalah ```10XX``` 
- SBC Direct Routing berfungsi menghubungkan SmartPBX dan Microsoft Teams.
- Microsoft Teams tidak mengenali nomer ekstensi, semua panggilan pada nomer ekstensi menuju Microsoft Teams akan ditranslasikan oleh SBC Direct Routing menjadi DID Line-URI.


## Dialplan Rules:
- SmartPBX ke Microsoft Teams : ```11XX``` atau ```11\d{2}``` dengan prioty ```0```
- Microsoft Teams ke SmartPBX : ```10XX``` atau ```10\d{2}``` dengan priority ```0```

Disini saya tidak menjelaskan detail bagaimana dialplan menuju PSTN atau carrier. Praktisnya tergantung masing masing penerapan. Sebagai contoh sederhana dapat menggunakan dialplan dengan rules CatchAll dengan priority 1 atau sesudah priority 0. Ini bertujuan agar dialplan antar ekstensi SmartPBX dan Microsoft Teams dapat dieksekusi terlebih dahulu. Bisa dilihat pada tulisan saya sebeulmnya di sini.

# SIP Trunk dari SmartPBX ke SBC Direct Routing
- Tambahkan device dan ekstensi pada SmartPBX. Buat dengan nomor ekstensi sesuai aturan yang sudah didefinisikan. Sebagai contoh dibuat user dengan device dan nomer ekstensi ```1017```.

![Screenshot](https://1.bp.blogspot.com/-4zc1494OCrk/XuFA9vd8F_I/AAAAAAAADVA/MSWvo6wrMrwkUI1guY9MePuwJLRU3a4igCNcBGAsYHQ/s320/add%2Bdevice.jpg)

Ektensi ```1017``` ini akan mewakili satu user pada Microsoft Teams. Jika ada beberapa user maka perlu ditambahkan masing masing satu device dan ekstension untuk mewakilinya. Atau dalam cara praktis lainnya dapat dibuatkan modifikasi routing untuk bisa mengakomodasi kebutuhan routing panggilan dengan hanya user dan device saja.

# SIP Trunk dari SBC Direct Routing ke SmartPBX
Tambahkan SIP trunk baru dari SBC Direct Routing menuju SmartPBX. SBC Direct Routing perlu mengenali panggilan dari SmartPBX lalu melakukan routing menuju Microsoft Teams. Masukkan kredensial dari username, password, dan realm pada device ekstensi yang sebelumnya sudah diset pada SmartPBX.

# SIP Trunk SBC Direct Routing - SmartPBX.
{% highlight xml %}
<gateway name="smartpbx">  
  <param name="username" value="username"/>  
  <param name="password" value="password"/>  
  <param name="realm" value="smartpbx.domain.tld"/>  
  <param name="from-user" value="username"/>  
  <param name="from-domain" value="smartpbx.domain.tld"/>  
  <param name="proxy" value="smartpbx.domain.tld"/>  
  <param name="expire-seconds" value="800"/>  
  <param name="register" value="true"/>  
  <param name="register-transport" value="tcp"/>  
  <param name="retry-seconds" value="30"/>  
  <param name="ping" value="30"/>  
  <param name="context" value="public"/>  
  <param name="caller-id-in-from" value="true"/>  
   <variables>  
   </variables>  
</gateway>  
 {% endhighlight %}

Asumsinya adalah SBC Direct Routing sudah terhubung dengan Microsoft Teams, hal ini karena sudah dikonfigurasikan pada step sebelumnya ketika menghubungkan nomer DID dari PSTN menuju Microsoft Teams.

Tahap selanjutnya adalah mengkonfigurasikan routing panggilan pada SBC Direct Routing untuk panggilan ekstensi antara SmartPBX dan Microsoft Teams.

Pada SmartPBX tidak diperlukan perubahan routing panggilan, dikarenakan secara otomatis SmartPBX sudah mengenali Microsoft Teams dari blok ekstensi yang sudah disepakati sebelumnya. Hal ini berbeda dengan Microsoft Teams yang memerlukan mapping dari nomor ekstensi menjadi nomor DID.

# Routing SmartPBX ke Microsoft Teams

{% highlight xml %}
 <extension name="smartpbx_to_teams">   
   <condition field="${acl(${network_addr} domains)}" expression="true"/>  
   <condition field="destination_number" expression="^(1017)$">  
    <action application="set" data="domain_name=$${domain}"/>  
    <action application="set" data="sip_secure_media=true"/>  
    <action application="export" data="nolocal:rtp_secure_media=true:AES_CM_128_HMAC_SHA1_80"/>  
    <action application="bridge" data="sofia/gateway/sip.pstnhub.microsoft.com/+622713400062"/>  
   </condition>  
  </extension>  
{% endhighlight %}

Perhatikan konfigurasi diatas. Pada routing panggilan diatas dilakukan translasi nomor tujuan dari ```1017 ``` menjadi ```+622713400062```. Hal ini agar panggilan dapat termapping pada user yang sudah ditentukan.

# Routing Microsoft Teams ke SmartPBX

{% highlight xml %}
 <extension name="teams_to_smartpbx">  
   <condition field="${acl(${network_addr} domains)}" expression="true"/>  
   <condition field="destination_number" expression="^(\+62)(11\d{2})$">  
    <action application="set" data="domain_name=$${domain}"/>  
    <action application="bridge" data="sofia/gateway/smartpbx.domain.tld/$2"/>  
   </condition>  
  </extension>  
{% endhighlight %}

Pada konfigurasi dari Microsoft Teams ke SmartPBX dilakukan pula normalisasi panggilan. Hal ini karena secara default Microsoft Teams akan membawa format E.164 yang akan selalu membawa awalan ```+62```. Sebenarnya hal tersebut dapat dilakukan melalui normalisasi pada Microsoft Teams. Pada CsOnlineVoiceRoute. SmartPBX tidak akan mengenali panggilan tersebut apabila tidak dilakukan normalisasi.

Setelah trunking dan routing selesai disetup, dapat dilanjutkan melakukan test panggilan ke antar ekstensi. 

# Test Call dari SmartPBX ke Microsoft Teams.
Panggilan dial dari softphone yang teregister ke Ofon SmartPBX dengan ekstensi ```1123``` menuju ekstensi Microsoft Teams ```1017```

![Screenshot](https://1.bp.blogspot.com/-GY0aaFOFNUw/XuFMaGmQliI/AAAAAAAADVg/GlckAxHQNT46gB2qgT2MBogkPBe5PadmACNcBGAsYHQ/s640/SmartPBX%2Bke%2BMS%2BTeams.png)

# Panggilan berjalan dari SmartPBX ke Microsoft Teams

![Screenshot](https://1.bp.blogspot.com/--9CjKRWQNhE/XuFMpKinL5I/AAAAAAAADVk/pYwrQBQtuyk5hSXhlCN7iawJQLm1dckZQCNcBGAsYHQ/s640/Active%2Bcall%2Bsmartpbx%2Bke%2Bmsteams.jpg)

# Test Call dari Microsoft Teams ke SmartPBX
Panggilan dari user Microsoft Teams dekstop app ke ekstensi ```1123``` yang ada di SmartPBX

![Screenshot](https://1.bp.blogspot.com/-pXtZvZUCVsE/XuFSpgUJKMI/AAAAAAAADW0/ZnOtOtDanMg26mB-yIdqaLcUHXzNzj3nQCNcBGAsYHQ/s640/MS%2BTeams%2Bto%2BSmartPBX.jpg)

# Panggilan berjalan dari Microsoft Teams ke SmartPBX

![Screenshot](https://1.bp.blogspot.com/-bQbnTokAHME/XuFS1PQkG5I/AAAAAAAADW4/V4Tlgy16P1E4-8-SVmiXz92hLd0oci2tACNcBGAsYHQ/s640/MS%2BTeams%2Bto%2BSmartPBX%2Baktipp.jpg)

Integrasi antara Microsoft Phone System tidak terbatas hanya pada SmartPBX saja. Hal serupa juga dapat dilakukan dengan menggunakan eksiting on premise IPPBX seperti ```Grandstream```, ```Sangoma```, ```PBXAct```, ```Yeastar```, ```CUCM dll```. Perbedaan requirement dari masing - masing vendor masih perlu dicoba dan ditest dikarenakan perbedaan pada cara konfigurasi.

Integrasi antara SmartPBX dan Microsoft Teams dapat dilakukan secara seamlessly dengan layanan Cloud Direct Routing yang memungkinkan menghubungkan kedua sistem teleponi yang berbeda.


Links :

- [Ofon Indonesia](https://www.ofon.co.id) 
- [Ofon Enterprose Solution](https://ofon.co.id/enterprise)
- [Microsoft Doccs for Direct Rotuing](https://docs.microsoft.com/en-us/microsoftteams/direct-routing-connect-the-sbc)



