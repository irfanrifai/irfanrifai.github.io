---
layout: post
title:  "Blacklist Aplikasi Monster-UI"
date:   2021-09-01 15:53:00 +0700
categories: kazoo provisioning
---
# Blacklist aplikasi yang tidak di-order pelanggan
Blacklist Aplikasi yang tidak di-subscribe oleh pelanggan

### 1. Aplikasi Yang Tidak Dibuka Ke Pelanggan

| No | Data Param | Type | Nilai | Sumber | Contoh |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | ![icon](https://smartpbx.ofon.biz/apps/developer/metadata/icon/developer.png) | ```string()``` | {service_id} | Aplikasi | OSP-00001 |
| 2 | ```org``` | ```string()``` | {customer_name} | Aplikasi | PT. Manuk Jalak |
| 3 | ```enabled``` | ```boolean()``` | true | Aplikasi | - |
| 4 | ```language``` | ```string()``` | en-US | Aplikasi | - |
| 5 | ```timezone``` | ```string()``` | Asia/Jakarta | Aplikasi | - |
| 6 | ```ui_restrictions``` | ```object()``` | false | Aplikasi | - |

### Basic Apps
List aplikasi yang gratis didapatkan oleh customer disemua paket layanan
| No | Friendly Name | appName | appId | purchaseable |
| :---: | :---: | :---: | :---: | :---: |
| 1 | SmartPBX | voip | ```370de21e6c46424f82aaa18291e89d45``` | Yes |
| 2 | Voicemails | voicemails | ```27129932318a17ae1e89f1e3cf2fa2ac``` | Yes |
| 3 | Debug | debug | ```06d9900ae056799f81e6d717fe27984d``` | Yes |
| 4 | Easy User Dashboard | userdashboard | ```0a71a2178b76e6bfeddaa8b02f4b942c``` | Yes |

### Pro Apps
List aplikasi berbayar yang dibundling dengan service plan
| No | Friendly Name | appName | appId | purchaseable |
| :---: | :---: | :---: | :---: | :---: |
| 1 | Storage Connector | storagemgmt | ```7d76ea2d0f5e16607d1c170537d59f6f``` | Yes |
| 2 | AWS Connector | integration-aws | ```59aeffb1fa848c530668f455adb7e55d``` | Yes |
| 3 | Google Drive Connector | integration-google-drive | ```3189319fa3950f5e6e03632173d15cbf``` | Yes |
| 4 | Call Recording Basic | recordingsce | ```3017625eeb86a8826707411c5bf27902``` | Yes |
| 5 | Call Recording | call-recording | ```6c8d42b0b3f9f2d8023c54fe79c42f48``` | Yes |
| 6 | Simple Callcenter | callcenter | ```ead9f62c115562513bf0a1d693bfad3e``` | Yes |
| 7 | Dialplans | dialplans | ```9a02f21ac624c5fbfdd96ffd4c94664b``` | Yes |
| 8 | Utilities Tools | utilities | ```ee7b18b21ac7b3e5c916769bff116106``` | Yes |
| 9 | User Portal | userportal | ```1345d47066aeb54ba3cd40aa93e059fd``` | Yes |
| 10 | Trunking Manager | trunking-manager | ```4005a9aead2451aacdc9d5546db3823a``` | Yes |
| 11 | CSV Jobs | tasks | ```958e1b37e35dc63f50aa5a48013b34fa``` | Yes |
| 12 | PBX Conncetor | pbxs | ```03d3171a0aadf93fa683cc4d159abac4``` | Yes |
| 13 | Operator Pro | operator-pro | ```0088cd91551e58567bcd5211115e8f9a``` | Yes |
| 14 | Operator | operator | ```801fe44d1e6fdb3fbdb623bc6bb69a1a``` | Yes |
| 15 | Locations | locations | ```a6e9defa51c7580d26ca7d5be31f8456``` | Yes |
| 16 | Faxes | fax | ```e378c619a8d6380b0d4e58d96a5e574d``` | Yes |
| 17 | CSV Onboarding | csv-onboarding | ```5723227f96252344f1fcd5f7bb34474f``` | Yes |
| 18 | Conferences | conferences | ```f57ea8851f7829c0f68b197d4f4d5d04``` | Yes |
| 19 | CDRs | cdrs | ```0db9313fe4b924714f9ad95fb054e48f``` | Yes |
| 20 | Call Center Pro | callqueues | ```3985e5a4258f2cb79c4bee7db4e8f94a``` | Yes |
| 21 | Callflows | callflows | ```cededf86e30470fc7f74c6ccefcf6ae5``` | Yes |
| 22 | Branding Manager | branding | ```8d276192ef5bd039e8a6b2503f873df1``` | Yes |
| 23 | Blacklist | blacklist | ```4ca92746a38ebf9c877682ce2f1d0e47``` | Yes |
| 24 | WebRTC Phone | webrtc | ```141ebee5e7225b87676296f5514c01fd``` | Yes |



### API Apps
List aplikasi berbayar terkait integrasi dan API yang dibundling dengan service plan
| No | Friendly Name | appName | appId | purchaseable |
| :---: | :---: | :---: | :---: | :---: |
| 1 | Websockets | websockets | ```4599982df687de12517facf26715c1af``` | Yes |
| 2 | Developer | developer | ```18dab78d10aa9ae65d2ac43baecf40c5``` | Yes |
| 3 | Auth Security | auth-security | ```44046daa51f055700913699022132e10``` | Yes |
| 4 | Duo Integration | duo | ```3dadfd4c8106ca2b7a49fe3806e8b5a6``` | Yes |
| 5 | Pivot | pivot | ```faa735bcb64c6ef94cb7b55e9c72295e``` | Yes |
| 6 | Webhooks | webhooks | ```d6c8e27fdbb8a7b40c2725871e3f305a``` | Yes |



## Format Request
Format request berikut merupakan contoh untuk memblacklist aplikasi yang tidak dipurchase oleh customer.
Aplikasi yang dipurchase oleh customer tidak masuk dalam list body json dibawah

```
POST http://kzdev.voip.my.id:8000/v2/accounts/{{customer_account_id}}/apps_store/blacklist
```


```
{
  "data": {
    "blacklist": [
      "fba3eae761856511557ddaba637e21c9",
      "44046daa51f055700913699022132e10",
      "4ca92746a38ebf9c877682ce2f1d0e47",
      "8d276192ef5bd039e8a6b2503f873df1",
      "6c8d42b0b3f9f2d8023c54fe79c42f48",
      "cededf86e30470fc7f74c6ccefcf6ae5",
      "3985e5a4258f2cb79c4bee7db4e8f94a",
      "c6333356b2ad1651e2e7f751d40fa05b",
      "0db9313fe4b924714f9ad95fb054e48f",
      "f57ea8851f7829c0f68b197d4f4d5d04",
      "5723227f96252344f1fcd5f7bb34474f",
      "18dab78d10aa9ae65d2ac43baecf40c5",
      "9a02f21ac624c5fbfdd96ffd4c94664b",
      "3dadfd4c8106ca2b7a49fe3806e8b5a6",
      "e378c619a8d6380b0d4e58d96a5e574d",
      "59aeffb1fa848c530668f455adb7e55d",
      "3189319fa3950f5e6e03632173d15cbf",
      "71b7f69eb041ba1481bdc3b89dfb42ee",
      "a6e9defa51c7580d26ca7d5be31f8456",
      "0d1c52810210d4341907e7f232784e74",
      "44669a444f604661fadb5723fc5332a6",
      "801fe44d1e6fdb3fbdb623bc6bb69a1a",
      "0088cd91551e58567bcd5211115e8f9a",
      "03d3171a0aadf93fa683cc4d159abac4",
      "faa735bcb64c6ef94cb7b55e9c72295e",
      "f3e55182e6e19bb1c315505c0b0ad4d7",
      "fbe3ad6168455494fff30b805e673534",
      "fc20e55a62cef92f6e5732e7c717a5b8",
      "958e1b37e35dc63f50aa5a48013b34fa",
      "5d24f5d2b130330e8701e9a13cd0a706",
      "4005a9aead2451aacdc9d5546db3823a",
      "1345d47066aeb54ba3cd40aa93e059fd",
      "ee7b18b21ac7b3e5c916769bff116106",
      "d6c8e27fdbb8a7b40c2725871e3f305a",
      "141ebee5e7225b87676296f5514c01fd",
      "4599982df687de12517facf26715c1af"
    ]
  }
}
```