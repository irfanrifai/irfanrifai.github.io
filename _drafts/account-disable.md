---
layout: post
title:  "Disable Account Kazoo"
date:   2021-09-01 15:53:00 +0700
categories: kazoo provisioning
---
# Disable Kazoo Account
Disable Account Pelanggan

## Request URL
```
POST https:/api.ofon.io:9443/v2/accounts/{customer_account_id}
```
Payload data yang harus dikirim saat menon-aktifkan akun pelanggan pada Kazoo

{% highlight json %}
{
  "data": {
    "enabled": false
  }
}
{% endhighlight %}