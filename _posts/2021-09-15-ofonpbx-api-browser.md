---
layout: post
title:  "Bermain Programmable Voice dengan API Playground CloudPBX"
date:   2021-09-15 08:40:00 +0700
categories: voip developer api
---

### Mengenal Programmable Voice
Layanan semacam CPaaS (*Communication Platfrom as a Service*) menyediakan voice API sebagai salah satu produknya. Voice API juga sering disebut sebagai *Programmable Voice* dimana user/developer dapat melakukan kontrol dari platfrom komunikasi melalui aplikasi pihak ketiga, melalui interface REST API.

Jika pernah mendengar provider seperti [Twilio](https://www.twilio.com/docs/usage/api), [Nexmo](https://developer.nexmo.com/voice/voice-api/overview), dan [Vonage](https://www.vonage.com/communications-apis) memiliki layanan *programmable voice* yang hampir mirip - mirip.

Berikut beberapa contoh layanan voice API yang sering kita dengar :
- Click to Call
- Call Blast / Broadcast
- Conferences Dial Out
- Call Detail Records
- Call Queues
- Webhooks
- Pivot Call Control

### Mengenal API Playground
API playground adalah tools yang dapat digunakan oleh user/developer untuk mempelajari dan mencoba fungsionalitas dari sebuah sistem API secara langsung. API playground diharapkan akan mempermudah user dalam melakukan pengujian API sebelum diterjemahkan kedalam kode program aplikasi yang dibuat oleh user/developer. 

API playground akan memberikan user/developer gambaran bagaimana format permintaan dan format respon yang terjadi ketika terjadi pertukaran data antara aplikasi dan API endpoint.

Salah satu contoh API playground adalah layanan OAuth2.0 milik [Google](https://developers.google.com/oauthplayground/).

![Google API Playground](/assets/images/2021-09-15-api-playground/2.png)

### API Playground pada Programmable Voice
Diatas sudah dibahas mengenai *Programmable Vocie* dan *API Playground*. Hal selanjutnya yang akan dibahas adalah bagaimana jika belajar menggunakan Programmable Voice menggunakan API Playground?
Alih - alih menggunakan 3 provider *CPaaS* diatas, mari melirik salah satu provider lokal sekaligus tempat saya berkerja saat ini yaitu [Ofon](https://ofon.co.id).

# Ofon CloudPBX API Browser
Ofon CloudPBX API Browser adalah *add-on applications* pada layanan [OfonPBX](https://ofon.co.id/pbx). API browser pada OfonPBX berisi collection list API OfonPBX yang dapat digunakan oleh developer. 

![OfonPBX](/assets/images/2021-09-15-api-playground/1.png)

### Contoh Voice API Playground pada Ofon CloudPBX
#### 1. Fetching Call Detail Records

CDR atau *Call Detail Records* adalah record detail panggilan yang sudah terjadi. Berisi informasi nomor pemanggil, nomor tujuan, waktu dimulai panggilan, durasi panggilan, dll. 

Sebagai contoh kasusnya user/developer ingin memvisualisasikan CDR pada aplikasi yang sedang dibuat untuk melihat statistik panggilan yang terjadi pada paltfrom Ofon CloudPBX. Developer dapat menarik data dari API Ofon CloudPBX dan mengolah datanya pada aplikasi yang dibuat.

![OfonPBX](/assets/images/2021-09-15-api-playground/3.png)

#### 2. Fetching Panggilan Yang Aktif / *Active Channels*

Active channels merupakan daftar sesi panggilan aktif yang sedang berlangsung pada sistem CloudPBX Ofon. 

![OfonPBX](/assets/images/2021-09-15-api-playground/4.png)

#### 3. Click to Call dari Device ke Nomor Telepon / Ekstensi Lain

Quick Call / Click to Call adalah API trigger untuk memanggil dua endpoint dan menyambungkannya dalam satu channel panggilan. 
Sebagai contoh, dalam aplikasi yang dibuat hendak diberikan fungsionalitas click to call. 

![OfonPBX](/assets/images/2021-09-15-api-playground/5.png)


### Penutup
Bagaimana? Cukup mudah bukan? Sebenarnya ada opsi lain menggunakan aplikasi [Postman](https://postman.com). Namun dengan postman user/developer perlu menginstall aplikasi postman ke local PC terlebih dahulu menyiapkan collection url APInya terlebih dahulu.

Referensi :
1. [Ofon](https://ofon.co.id)
2. [OfonPBX](https://ofon.co.id/pbx)
3. [Twilio](https://www.twilio.com/docs/usage/api)
4. [Nexmo](https://developer.nexmo.com/voice/voice-api/overview)
5. [Vonage](https://www.vonage.com/communications-apis)
6. [API Playground](https://github.com/BestBuy/api-playground)