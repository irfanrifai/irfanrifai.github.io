---
layout: post
title:  "Call Recording dengan AWS S3"
date:   2021-09-01 13:15:57 +0700
categories: voip cloudpbx
---
## Pranala
Call Recording atau rekaman panggilan merupakan fitur dasar dan penting yang harus dimiliki perangkat IPPBX. Sebagai contoh kasus, ada kalanya seorang supervisor hendak memutar percakapan seorang staffnya dengan pelanggan untuk memastikan kualitas dukungan layanan terhadap pelanggan. Atau seorang manajer IT yang hendak melakukan evaluasi terhadap kualitas layanan telepon itu sendiri.

Perkembangan dalam transformasi telepon kantor dari on-premise menjadi cloud memberikan tantangan baru dalam memberikan fleksibilitas dan keamanan dari layanan telepon. Lalu bagaimana dengan kualitas dan keamanan dalam layanan telepon kantor berbasis cloud?

Ofon SmartPBX adalah platform teleponi berbasis cloud. SmartPBX menyediakan fitur Call Recording yang dapat terhubung dengan eksisting cloud storage milik pelanggan ataupun dengan cloud storage yang sudah disediakan oleh Ofon.

Dengan integrasi cloud storage. Kapasitas dan ketersediaan ruang penyimpanan dapat diatur sesuai dengan kebutuhan. Keamanan dan ketersedian data rekaman panggilan dapat terjaga karena tersimpan dalam penyimpanan komputasi awan. Didukung dengan web portal yang intuitif untuk memudahkan pelanggan dalam melakukan konfigurasi call recording pada setiap usernya.


## Storage Management
Storage Management adalah aplikasi web manajemen untuk menambahkan cloud storage pada portal web SmartPBX. Storage management mendukung integrasi SmartPBX dengan provider cloud storage seperti ```AWS S3``` dan layanan ```S3 kompatibel``` lain. Ofon juga menyediakan lokal S3 kompatibel storage.

![Screenshot](https://1.bp.blogspot.com/-3cMJFlvTK5Y/XtqC5ojdEeI/AAAAAAAADT4/cE67Qe-VOw4i_jqgF-Rw-rcKxLzY1bmEwCNcBGAsYHQ/s640/Storage%2Bmanagement1.jpg)

## Call Recording
Call Recording adalah aplikasi web manajemen untuk mencari, mendengarkan, dan mengunduh hasil rekaman panggilan yang sudah diset pada user SmartPBX. Pencarian rekaman panggilan dapat dilakukan dengan mudah melalui filter dan pencarian custom yang sudah tersedia pada portal web SmartPBX.

![Screenshot](https://1.bp.blogspot.com/-JZWDqzcQDKw/XtqDMyAeP9I/AAAAAAAADUA/majVbc2Q-OsZa5_y3rmF6ijLrsOeGkfCwCNcBGAsYHQ/s640/call%2Brecording1.jpg)

Mudah bukan? Pelanggan dapat memilih opsi menggunakan eksiting kompatibel S3 cloud storage ataupun menggunakan kapasitas cloud storage yang sudah disediakan oleh Ofon.

# Mengaktifkan Call Recording Pada User
 
Mengaktifkan fitur call recording pada Ofon SmartPBX dapat dilakukan dengan mudah. Administrator yang sudah memiliki login akun dapat mengoperasikan konfigurasi untuk menambah call recording pada user.

Buka pada aplikasi ```SmartPBX```, pilih menu ```Users```, lalu klik pada bagian kanan konfigurasi user. Klik pada icon ```Customized Call Recording```.

![Screenshot](https://1.bp.blogspot.com/-1GZ5TysEUSw/XtqDU5xVAVI/AAAAAAAADUE/6y6i5lgI7iMk_zW5dDppf7-F-4AXBl86QCNcBGAsYHQ/s640/mengaktifkan%2Bcall%2Brecording1.jpg)

Pembagian call recording dapat dipisahkan menjadi 4 kategori. Adapun pembagiannya sebagai berikut.

- **Inbund Internal** : Mengaktifkan call recording apabila user atau extension ditelpon oleh user lain dalam SmartPBX.
- **Inbound External** : Mengaktifkan call recording apabila user atau extension ditelpon oleh DID eksternal diluar SmartPBX.
- **Outbound Internal** : Mengaktifkan call recording apabila user atau extension melalukan panggilan menuju user atau extension lain dalam SmartPBX.
- **Outbound External** : Mengaktifkan call recording apabila user atau extension melakukan panggilan menuju DID eksternal diluar SmartPBX.

Links :
- [Ofon Indonesia](https://www.ofon.co.id)
