---
layout: post
title:  "Microsoft Teams Direct Routing dengan FreeSWITCH"
date:   2021-09-01 13:15:57 +0700
categories: voip directrouting
---
## Pranala
Tertantang dengan kolega yang berhasil menghubungkan Asterisk  dengan Microsoft Phone System. Pada tulisan kali ini saya telah mencoba menghubungkan FreeSWITCH dengan Microsoft Phone System.
![Screenshot](https://1.bp.blogspot.com/-abyvy30FRXA/XtYpCEHjj5I/AAAAAAAADR4/MAr_NUpFmZ0oGEIO9mSfXam48WKyZMH2gCNcBGAsYHQ/s640/topo.jpg)

Artikel tentang menghubungkan Microsoft Teams Direct Routing :

1. Microsoft Teams Direct Routing With Asterisk (Issabel) by Gabriel Hermawan Wishnu.

2. Using Kamailio as SBC for Microsoft Teams by Henning Westerholt.


# Prasyarat

- Server FreeSWITCH harus terinstall dengan IP Publik. Tidak dianjurkan untuk membuka koneksi SSH ke publik. Dianjurkan menggunakan firewall yang cukup ketat dan mengizinkan koneksi dari blok IP address Microsoft Cloud ```(52.112.0.0/14)```.

-  FreeSWITCH harus diset dengan ``SIPS`` dan ```SRTP``` mengunakan cert/SSL dari SSL authority yang didukung oleh Microsoft. Dalam tulisan ini saya menggunkan DigiCert.

-  Dalam rangka menyesuaikan format contact header dari Microsoft Phone System. Perlu dilakukan patch pada kode sumber FreeSWITCH dan melakukan kompilasi modul. Patch dilakukan pada modul mod_sofia. Tanpa melakukan patch ini menggunakan FreeSWITCH dengan direct routing tidak akan berfungsi.

-  Memiliki nomor routing DID dari operator, dalam tulisan ini menggunakan DID dari Ofon Indonesia yang merupakan salah satu penyedia layanan telepon tetap berbasis IP dan menyediakan layanan Cloud Microsoft Teams Direct Routing.

-  Memiliki akses DNS Server untuk menambah TXT record dalam rangka autentikasi domain dengan Microsoft.

-  Memiliki minmal 2 lisensi E1 O365 + Phone System. Satu lisensi untuk user dan satu lisensi digunakan untuk domain SIP Trunk.


# Installasi FreeSWITCH
- Download kode sumber FreeSWITCH dari github official site.
{% highlight generic %}
 wget -O - https://files.freeswitch.org/repo/deb/debian-release/fsstretch-archive-keyring.asc | apt-key add -  
 echo "deb http://files.freeswitch.org/repo/deb/debian-release/ stretch main" > /etc/apt/sources.list.d/freeswitch.list  
 echo "deb-src http://files.freeswitch.org/repo/deb/debian-release/ stretch main" >> /etc/apt/sources.list.d/freeswitch.list  
 apt-get update  
 #Install dependencies required for the build  
 apt-get build-dep freeswitch  
 #then let's get the source. Use the -b flag to get a specific branch  
 cd /usr/src/  
 git clone https://github.com/signalwire/freeswitch.git -bv1.10 freeswitch  
 cd freeswitch
 {% endhighlight %}

- Sebelum melakukan compile, patch 3 file sumber kode yaitu ```sofia.c```,```sofia_reg.c```, dan ```mod_sofia.h``` .
- Compile dan Install FreeSWITCH.
{% highlight generic %}
 git config pull.rebase true  
 # ... and do the build  
 ./bootstrap.sh -j  
 ./configure  
 make  
 make install
 {% endhighlight %}

-  Setup startup script / systemd untuk service FreeSWITCH. Startup script akan menjalankan service FreeSWITCH secara otomatis.

-  Microsoft Phone System mengharuskan koneksi SIP Trunk dari SBC menggunakan SIPS dan SRTP. SIPS dan SRTP adalah prokotol SIP dan RTP yang dienkripsi sehingga koneksi SIP Trunk menjadi secure. FreeSWITCH harus diset menggunakan SIPS dan SRTP untuk dapat tersambung dengan Microsoft Phone System. Diperlukan perubahan pada konfigurasi global dari aplikasi FreeSWITCH, konfigurasi global pada ```vars.xml```
{% highlight xml %}
<!-- External SIP Profile -->  
 <X-PRE-PROCESS cmd="set" data="external_auth_calls=false"/>  
 <X-PRE-PROCESS cmd="set" data="external_sip_port=5080"/>  
 <X-PRE-PROCESS cmd="set" data="external_tls_port=5081"/>  
 <X-PRE-PROCESS cmd="set" data="external_ssl_enable=true"/>  
 <X-PRE-PROCESS cmd="set" data="sip_tls_version=tlsv1,tlsv1.1,tlsv1.2"/>  
 <X-PRE-PROCESS cmd="set" data="sip_tls_ciphers=ALL:!ADH:!LOW:!EXP:!MD5:@STRENGTH"/>  
 <X-PRE-PROCESS cmd="set" data="rtp_sdes_suites=AEAD_AES_256_GCM_8|AEAD_AES_128_GCM_8|AES_CM_256_HMAC_SHA1_80|AES_CM_192_HMAC_SHA1_80|AES_CM_128_HMAC_SHA1_80|AES_CM_256_HMAC_SHA1_32|AES_CM_192_HMAC_SHA1_32|AES_CM_128_HMAC_SHA1_32|AES_CM_128_NULL_AUTH"/>
 {% endhighlight %}

 - Pasang  CERT dari CA file pada FreeSWITCH. Pada contoh ini saya menggunakan DigiCert. CERT harus menggunakan provider komersial CERT yang kompatibel dengan Microsoft.

- Check validasi SSL menggunakan SSL Shopper.
![Screenshot](https://1.bp.blogspot.com/-LuXO9FCuym0/XtYiU08QzpI/AAAAAAAADQ8/nDWzxCx5wegaNGjQDTuuCZiDkCoLneajgCNcBGAsYHQ/s640/ssl%2Bcheck.jpg)

# Konfigurasi FreeSWITCH
-  Setup gateway ke Microsoft Phone System. Gateway dalam FreeSWITCH merupakan repserentasi UA untuk SIP Trunk. SIP proxy Microsoft Phone System adalah ```sip.pstnhub.microsoft.com```
```freeswitch_ofon_biz.xml``` 
{% highlight xml %}
<include>  
  <gateway name="freeswitch.ofon.biz">  
  <param name="username" value="not-used"/>  
  <param name="realm" value="freeswitch.ofon.biz"/>  
  <param name="from-domain" value="freeswitch.ofon.biz"/>  
  <param name="password" value="not-used"/>  
  <param name="proxy" value="sip.pstnhub.microsoft.com"/>  
  <param name="register" value="false"/>  
  <param name="register-transport" value="tls"/>  
  <param name="retry-seconds" value="10"/>  
  <param name="caller-id-in-from" value="true"/>  
  <!--extra sip params to send in the contact-->  
  <param name="contact-host" value="freeswitch.ofon.biz"/>  
  <param name="contact-in-ping" value="true"/>  
  <param name="ping-user-agent" value="Sonus SBC V.5.7"/>  
  <param name="ping" value="10"/>  
  </gateway>  
 </include>  
{% endhighlight %}

Parameter berwarna merah adalah parameter hasil patch sumber kode FreeSWITCH. Tanpa patch tersebut maka Microsoft Phone System tidak bisa mengidentifikasi domain dan contact header dari SBC Direct Routing.

- Jalankan gateway kearah Microsoft Phone System dan cek apakah paket ```SIP OPTIONS``` sudah terkirim dan mendapatkan balasan dari SIP Proxy Microsoft.
{% highlight generic %}
send 623 bytes to tls/[52.114.7.24]:5061 at 03:25:41.189516:  
 ------------------------------------------------------------------------  
 OPTIONS sip:sip.pstnhub.microsoft.com;transport=tls SIP/2.0  
 Via: SIP/2.0/TLS IP.AD.DR.ES:5081;rport;branch=z9hG4bKDKgetaKpUc4rc  
 Max-Forwards: 70  
 From: <sip:freeswitch.ofon.biz>;tag=4624XDZ9UtypN  
 To: <sip:freeswitch.ofon.biz>  
 Call-ID: e763d8b6-1ee8-1239-0ea5-566fbb48005e  
 CSeq: 20967190 OPTIONS  
 Contact: <sip:gw+freeswitch.ofon.biz@freeswitch.ofon.biz:5081;transport=tls;gw=freeswitch.ofon.biz>  
 User-Agent: Sonus SBC V.5.7  
 Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY  
 Supported: timer, path, replaces  
 Allow-Events: talk, hold, conference, refer  
 Content-Length: 0  
 recv 365 bytes from tls/[52.114.7.24]:5061 at 03:25:41.238211:  
 ------------------------------------------------------------------------  
 SIP/2.0 200 OK  
 FROM: <sip:freeswitch.ofon.biz>;tag=4624XDZ9UtypN  
 TO: <sip:freeswitch.ofon.biz>  
 CSEQ: 20967190 OPTIONS  
 CALL-ID: e763d8b6-1ee8-1239-0ea5-566fbb48005e  
 VIA: SIP/2.0/TLS IP.AD.DR.ES:5081;branch=z9hG4bKDKgetaKpUc4rc;rport  
 CONTENT-LENGTH: 0  
 ALLOW: INVITE,ACK,OPTIONS,CANCEL,BYE,NOTIFY  
 SERVER: Microsoft.PSTNHub.SIPProxy v.2020.5.27.2 i.ASEA.5  
{% endhighlight %}

- Setup Gateway kearah provider/carrier.

 ```ofonsbc.xml```
{% highlight xml %}
 <include>  
  <gateway name="netaccess.ofon.biz">  
  <param name="username" value="not-used"/>  
  <param name="realm" value="netaccess.ofon.biz"/>  
  <param name="from-domain" value="customer04.ofon.biz"/>  
  <param name="password" value="not-used"/>  
  <param name="proxy" value="IP.AD.DR.ES"/>  
  <param name="register" value="false"/>  
  <param name="register-transport" value="tcp"/>  
  <param name="retry-seconds" value="10"/>  
  <param name="caller-id-in-from" value="true"/>  
  <param name="contact-host" value="customer04.ofon.biz"/>  
  <param name="contact-in-ping" value="true"/>  
  <param name="ping-user-agent" value="Sonus SBC V.5.7"/>  
  <param name="ping" value="10"/>  
  </gateway>  
 </include>  
{% endhighlight %}

Setelah SIP Trunk dari Microsoft Phone System dan Carrier/Operator selesai disetup. Routing panggilan dari dan ke Microsoft Phone System harus di define pada dialplan FreeSWITCH.

DID yang digunakan adalah  ```+622430000062```.

- Microsoft Phone System ke SIP Trunk Operator
{% highlight xml %}
 <include>  
  <extension name="teams_to_ofon">  
   <condition field="${acl(${network_addr} domains)}" expression="true"/>  
   <condition field="caller_id_number" expression="^(\+622430000062)$">  
    <action application="set" data="domain_name=$${domain}"/>
    <action application="bridge" data="sofia/gateway/netaccess.ofon.biz/${destination_number}"/>  
   </condition>  
  </extension>  
 </include>  
{% endhighlight %}

- SIP Trunk Operator ke Microsoft Phone System
{% highlight xml %}
 <include>  
  <extension name="ofon_to_teams">  
   <condition field="network_addr" expression="^IP\.AD\.DR\.ES$"/>  
   <condition field="${acl(${network_addr} domains)}" expression="true"/>  
   <condition field="destination_number" expression="^(\+622430000062)$">  
    <action application="set" data="domain_name=$${domain}"/>  
    <action application="set" data="sip_secure_media=true"/>  
    <action application="export" data="nolocal:rtp_secure_media=true:AES_CM_128_HMAC_SHA1_80"/>  
    <action application="bridge" data="sofia/gateway/customer04.ofon.biz/${destination_number}"/>  
   </condition>  
  </extension>  
 </include>  
{% endhighlight %}

Pada teks berwarna merah, merupakan konfigurasi untuk force transport dari RTP menjadi SRTP.

Konfigurasi Microsoft Teams PhoneSystem

Konfigurasi pada PhoneSystem menggunakan Windows Powershell. Powershell memerlukan user Global Admin.

- Akses pada ```portal.microsoft.com``` dengan user Global Admin. Pastikan user client dan user dummy untuk SBC Direct Routing sudah diassign dengan lisensi O365 dan PhoneSystem.

Dalam contoh menggunakan user client sekaligus Global Admin ```irfan.rifai@ofon.io``` dan dummy user SBC ```sip@freeswitch.ofon.biz```
![Screenshot](https://1.bp.blogspot.com/-JbBHj0mKIgk/XtYfzO7TMHI/AAAAAAAADQo/zwepRpY2bNEdNLJ8vmRJymS3w25W2EoyACNcBGAsYHQ/s640/licensed%2Buser.jpg)

- Mulai session dengan menggunakan powershell. Mulai powershell dengan "run as administrator".

- Import module ```SkypeOnlineConnector``` dan import kredensial user Global Admin.
{% highlight generic %}
  Import-Module SkypeOnlineConnector   
  $Cred = Get-Credential   
{% endhighlight %}

- Akan muncul prompt dan masukkan kredensial.
![Screenshot](https://1.bp.blogspot.com/-ph3azb9mQU8/XtYgvz-C1MI/AAAAAAAADQw/5uNdttS46Qo-JdGhQCFxC-pPRIpJmpGJgCNcBGAsYHQ/s640/prompt.jpg)

- Mulai sesi dengan konsol PhoneSystem.
{% highlight generic %}
 $sfb = New-CsOnlineSession $Cred -OverrideAdminDomain “ofonio.onmicrosoft.com”  
 Import-PSSession $sfb -AllowClobber  
{% endhighlight %}


- Tambahkan FreeSWITCH SBC untuk Direct Routing.
{% highlight generic %}
 Set-CsOnlinePSTNGateway -Fqdn freeswitch.ofon.biz -SipSignallingPort 5081 -ForwardCallHistory $true -Enabled $true -ForwardPai $true -MaxConcurrentSessions 5
{% endhighlight %}  

- Check apakah server FreeSWITCH sudah berhasil ditambahakan dengan perintah ```Get-CsOnlinePstnGateway```.
{% highlight generic %}
 Identity              : freeswitch.ofon.biz  
 InboundTeamsNumberTranslationRules : {}  
 InboundPstnNumberTranslationRules  : {}  
 OutboundTeamsNumberTranslationRules : {}  
 OutboundPstnNumberTranslationRules : {}  
 Fqdn                : freeswitch.ofon.biz  
 SipSignalingPort          : 5081  
 FailoverTimeSeconds         : 10  
 ForwardCallHistory         : True  
 ForwardPai             : True  
 SendSipOptions           : True  
 MaxConcurrentSessions        : 5  
 Enabled               : True  
 MediaBypass             : False  
 GatewaySiteId            :  
 GatewaySiteLbrEnabled        : False  
 GatewayLbrEnabledUserOverride    : False  
 FailoverResponseCodes        : 408,503,504  
 GenerateRingingWhileLocatingUser  : True  
 PidfLoSupported           : False  
 MediaRelayRoutingLocationOverride  :  
 ProxySbc              :  
 BypassMode             : None  
{% endhighlight %} 

- Buat routing panggilan dengan policy user yang akan diberikan kepada FreeSWITCH Server.
{% highlight generic %}
 Set-CsOnlinePstnUsage -Identity Global -Usage @{Add="Default"}  
 New-CsOnlineVoiceRoute -Identity "CatchAll" -NumberPattern ".*" -OnlinePstnGatewayList ast.ofon.biz -Priority 1 -OnlinePstnUsages "Default"  
 New-CsOnlineVoiceRoutingPolicy -Identity "DefaultVoicePolicy" -OnlinePstnUsages "Default"  
{% endhighlight %}

- Tambahkan nomor DID pada user client.
{% highlight generic %}
 Set-CsUser -Identity "irfan.rifai@ofon.io" -EnterpriseVoiceEnabled $true -HostedVoiceMail $true -OnPremLineURI tel:+622713400062  
{% endhighlight %}

- Daftarkan user client dengan Voice Routing Policy yang sudah dibuat sebelumnya.
{% highlight generic %}
 Grant-CsOnlineVoiceRoutingPolicy -Identity "irfan.rifai@ofon.io" -PolicyName "DefaultVoicePolicy"  
{% endhighlight %}

Sampai disini konfigurasi untuk PhoneSystem dan FreeSWITCH sudah selesai. Login pada aplikasi Microsoft Teams pada konsol baik mobile maupun laptop. Perhatikan bagian dialplad.

# Status dialpad dari user client Microsoft Teams.

![Screenshot](https://1.bp.blogspot.com/-qfvfBksgnbA/XtYcZqUY61I/AAAAAAAADP8/6udi9AGzONMzbOdEoXNnAeSJkZQZdWGUgCNcBGAsYHQ/s640/dialpad.jpg)

Apabila dialplan sudah muncul nomor DID yang didaftarkan melalui konsol powershell, lakukan tes panggilan dari PSTN/Mobile ke DID yang sidah didaftarkan pada Microsoft Teams.

![Screenshot](https://1.bp.blogspot.com/-9xnSZB2chFA/XtYkkEXw4AI/AAAAAAAADRg/KKvXti9L75gW5vgZsBveoH7mQgRX8NyuQCEwYBhgLKs0DAL1OcqzUeYNlwWcACzNPW1kEJcdbfRMWG7VuofM7uEfJKwnPsJ2_cmFsJ7FnHbAzgfSEh2MUo9MtCRdbGQAWwg8dljOrio9ooOj6eS48qGA-BVxbo2OTnaYl6sP6jHaEUK-RHvoHfsC_KgFPSjC36aa_cg7hTQTbWrZ-Mj0fBGTjRwK2Ky7GPIJI90mAx4uK6jzAyFLegMFRhMx03Q92nmG0KC0dk8CCQEEwfeos1fJhsNwkXkNTHpkEBNd3HLMxqfEV1dgaILQco0dnaP3OMkjPdkoaKzAAH71wsk_bH8qrqzpslNLJAuCWHaEDkJk-sr2hfApwxd1GhzSu5uahxpPkT0krZUbnfmgW64tXiXp94Nj_r8yIsNDvlk272RiKhgAVNntVSkC355D6H7iBFxifq1n6qbFsR3GEnnJrdL6oCUunIHodiKAD6U5IWIii6nr9rftHnF3qDFg-AQ4UKB6d7OWMEuPRstgJFe7_KI8twjsf0qYdxxO4Ijv-B06SExlB5oWap70T2UlDTFfP9QKfztWSO9RNVAJ2KpokeHpuCg2Yp72A1QC2gFpMtvvx0V7LtBQR1tx7-LRrrwK6gYeAac8rCp79DUFw5Cwwh87Y9gU/s640/inbound%2Bteams%2Brung%2Bdiangkat.jpg)

# Log dan event panggilan dari konsol FreeSWITCH.

![Screenshot](https://1.bp.blogspot.com/-N-7MFQkHVGk/XtYclGyW20I/AAAAAAAADQA/uoT5d8n5WLYa6YmZTQCWI2DY0lLrtESxwCNcBGAsYHQ/s640/log%2Bfreeswatch.jpg)

# Panggilan outbound dari Microsoft Teams ke operator lain.

![Screenshot](https://1.bp.blogspot.com/-4FLZkS6B7Ww/XtYctDDTwzI/AAAAAAAADQU/WFKOWP225QEKpr_euFUzdjvpFWkLEoydACEwYBhgLKs0DAL1OcqzFSMD5mP-K2zpZZeD29-0cU99O-6mfJW9YpgWBpYV7N-XuBkYV2bX2q68iwU4d_V64YLlc0ELlGTzcTjLrxITcqtreAFKpu8LH5Xb1BkV7I1TYm4HgqS-lqxnEHTbx8SYaE4_FqhH9Kcb57pUxtvMUr1AYYnUNIUEjAHbUr4vSYeeR-bJvjW_XciMfUQN0uq88NSjeeLI3vISM2ZyUMQul88jDmELt1VwYFXFgcfh4WHQ8xntgvfqIX-VM0k_bdFrw7Bfnk8vkO8ZX90tDbYO5mbju64ufPGiuFu24BtwmZlGMMtLPUKMzgRUFEU3ZdImgO7nPUOlmx4Ae2QN1A8bZdu3RJOWao6KNDmpRmRJ8uV4sTYanfgDouLNIRylkma-QkiImKMpcmMNpps_f-P33bpArZvQFEzOtxfXgbwGG98vqpoYMtxZq53fPqku9mpPyhKroBm32ESK80g2Bp-uD1svAYQ7og71VuvOb37XLz0rD9CwYkhYPv7QT2CtkUG7itUgtpIEkStyAZ1sWnndIrCdzMUPzoKxdr8vUPrME-sNethoVf2ajite-p8wK6JBHAXpmtZKF5Ebb1nTrj3ChxcTzPo-UOskwnr_Y9gU/s640/outbound%2Btyeams%2Bdrung%2Bdiangkat.jpg)

![Screenshot](https://1.bp.blogspot.com/-HY3YFhUGwWE/XtYcs-0KUbI/AAAAAAAADQU/tmLRwqOkm78i_uuf2iDuXXZaissXQpOYACEwYBhgLKs0DAL1OcqzFSMD5mP-K2zpZZeD29-0cU99O-6mfJW9YpgWBpYV7N-XuBkYV2bX2q68iwU4d_V64YLlc0ELlGTzcTjLrxITcqtreAFKpu8LH5Xb1BkV7I1TYm4HgqS-lqxnEHTbx8SYaE4_FqhH9Kcb57pUxtvMUr1AYYnUNIUEjAHbUr4vSYeeR-bJvjW_XciMfUQN0uq88NSjeeLI3vISM2ZyUMQul88jDmELt1VwYFXFgcfh4WHQ8xntgvfqIX-VM0k_bdFrw7Bfnk8vkO8ZX90tDbYO5mbju64ufPGiuFu24BtwmZlGMMtLPUKMzgRUFEU3ZdImgO7nPUOlmx4Ae2QN1A8bZdu3RJOWao6KNDmpRmRJ8uV4sTYanfgDouLNIRylkma-QkiImKMpcmMNpps_f-P33bpArZvQFEzOtxfXgbwGG98vqpoYMtxZq53fPqku9mpPyhKroBm32ESK80g2Bp-uD1svAYQ7og71VuvOb37XLz0rD9CwYkhYPv7QT2CtkUG7itUgtpIEkStyAZ1sWnndIrCdzMUPzoKxdr8vUPrME-sNethoVf2ajite-p8wK6JBHAXpmtZKF5Ebb1nTrj3ChxcTzPo-UOskwnr_Y9gU/s640/outbound%2Btyeams%2Bwes%2Bdiangkat.jpg)

Menarik bukan? Meskipun dapat berfungsi baik untuk panggilan masuk dan keluar, sangat tidak disarankan menggunakan FreeSWITCH untuk Microsft Teams Direct Routing.

Ini karena Microsoft sendiri memiliki standar dan sertifikasi untuk SBC Direct Routing, adapun list dari SBC Direct Routing yang sudah didukung oleh microsoft sebagai berikut :

List terbaru SBC yang didukung Microsoft [disini](https://docs.microsoft.com/en-us/microsoftteams/direct-routing-border-controllers)

- [AudioCodes](https://www.audiocodes.com/solutions-products/solutions/skype-for-business-microsoft-teams/microsoft-teams)
- [Ribbon](https://ribboncommunications.com/solutions/enterprise-solutions/cloud-and-edge/microsoft-solutions-teams-direct-routing)
- [MetaSwitch](https://www.metaswitch.com/solutions/microsoft-teams-direct-routing)
- [Oracle](https://www.oracle.com/webfolder/technetwork/acmepacket/Microsoft/SBC-MSFTTeams-NON-MB.pdf)
- [TE-SYSTEMS](https://www.anynode.de/anynode-and-microsoft-teams/)
- [ThinkTel](https://www.thinktel.ca/services/microsoft-365-business-voice/)

Sekian tulisan mengenai Microsoft Teams Direct Routing dengan FreeSWITCH. 

Links :

- [Ofon Direct Routing Solution](https://ofon.co.id/enterprise/clouddirectrouting)
- [FreeSWITCH Open Source Softswitch](https://freeswitch.org)
- [Microsoft Direct Routing Plan](https://docs.microsoft.com/en-us/microsoftteams/direct-routing-plan)
- [Microsoft Direct Routing Guide](https://docs.microsoft.com/en-us/microsoftteams/direct-routing-configure)
