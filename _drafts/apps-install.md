---
layout: post
title:  "Install Aplikasi Monster-UI"
date:   2021-09-01 15:53:00 +0700
categories: kazoo provisioning
---
# Install Monster UI Apps pada akun customer
Install app bertujuan untuk membuat aplikasi terpasang pada app_store milik customer.

Bertujuan agar ketika user login maka semua list aplikasi di-allow akan muncul seperti gambar dibawah.

![apps_strore](https://1.bp.blogspot.com/-JbTpy3K6r-U/X2UZuKZJ97I/AAAAAAAADas/Z2JmJjdpZk4w_wt14MjSUT930eOqLbNZQCNcBGAsYHQ/w640-h298/BYOC%2B1.png)

Installasi app pada portal Monster UI diklasifikasikan berdasarkan hak akses aplikasi.

### App-ID dari cluster Kazoo PBX002
Contoh basic apps yang ditambahkan pada saat account creation beserta klasifikasi hak aksesnya oleh admin atau user.

| No | Friendly Name | appName | appId | allowed_user |
| :---: | :---: | :---: | :---: | :---: |
| 1 | SmartPBX | voip | ```6a06ce6d8c08e2441648ab9b102e800e``` | admins |
| 2 | Voicemails | voicemails | ```e5087bfa6eeb7837bdb553fbd265be7d``` | admins |
| 3 | Debug | debug | ```9b20b5c70db880ff1e11fe6507c94317``` | admins |
| 4 | Easy User Dashboard | userdashboard | ```e796555d3342142c58dac9a9b066eefc``` | all |


### Format Request
1. Install App ***SmartPBX*** untuk administrator

```
PUT https://api.ofon.io:9443/v2/accounts/{customer_account_id}/apps_store/6a06ce6d8c08e2441648ab9b102e800e
```
Request Body
{% highlight json %}
{
    "data": {
        "allowed_users": "admins"
    }
}
{% endhighlight %}

2. Install App ***Easy User Dashboard*** untuk semua user

```
PUT https://api.ofon.io:9443/v2/accounts/{customer_account_id}/apps_store/e796555d3342142c58dac9a9b066eefc
```
Request Body
{% highlight json %}
{
    "data": {
        "allowed_users": "all"
    }
}
{% endhighlight %}




## Format Body
#### Allow Semua User
```
{
    "data": {
        "allowed_users": "all"
    }
}
```
#### Allow Hanya Admin
```
{
    "data": {
        "allowed_users": "admins"
    }
}
```
