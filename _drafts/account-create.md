---
layout: post
title:  "Create Account Kazoo"
date:   2021-09-01 15:53:00 +0700
categories: kazoo provisioning
---
# Kazoo Account Creation
Request API untuk pembuatan akun baru

## Request Parameter
List nilai parameter yang perlu dikirim ke API kazoo oleh aplikasi provisioning

| No | Data Param | Type | Nilai | Sumber | Contoh |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | ```name``` | ```string()``` | {service_id} | Aplikasi | OSP-00001 |
| 2 | ```org``` | ```string()``` | {customer_name} | Aplikasi | PT. Manuk Jalak |
| 3 | ```enabled``` | ```boolean()``` | true | Aplikasi | - |
| 4 | ```language``` | ```string()``` | en-US | Aplikasi | - |
| 5 | ```timezone``` | ```string()``` | Asia/Jakarta | Aplikasi | - |
| 6 | ```ui_restrictions``` | ```object()``` | false | Aplikasi | - |

## Request URL
```
PUT https://api.ofon.io:9443/v2/accounts/{master_account_id}
```

Payload data yang harus dikirim saat pembuatan account kazoo dari aplikasi

{% highlight json %}
{
  "data": {
    "enabled": true,
    "name": {service_id},
    "org": {customer_name},
    "timezone": "Asia/Jakarta",
    "language": "en-US",
    "ui_restrictions": {
      "myaccount": {
        "user": {
          "show_tab": false
        },
        "account": {
          "show_tab": false
        },
        "billing": {
          "show_tab": false
        },
        "balance": {
          "show_tab": false,
          "show_credit": false,
          "show_minutes": false
        },
        "service_plan": {
          "show_tab": false
        },
        "transactions": {
          "show_tab": false
        },
        "inbound": {
          "show_tab": false
        },
        "outbound": {
          "show_tab": false
        },
        "twoway": {
          "show_tab": false
        },
        "errorTracker": {
          "show_tab": false
        }
      }
    }
  }
}
{% endhighlight %}

## Response Body
{% highlight json %}
{
  "data": {
      "enabled": true,
      "language": "en-US",
      "name": "OSP-00001",
      "org": "PT. Manuk Jalak",
      "timezone": "Asia/Jakarta",
      "realm": "eb8279.pbx002.ofon.biz",
      "call_restriction": {},
      "music_on_hold": {},
      "preflow": {},
      "ringtones": {},
      "id": "19af6ecd90f9c201ead3051bf9fbb71c",
      "wnm_allow_additions": false,
      "superduper_admin": false,
      "created": 63797782482,
      "reseller_id": "0f13d185ab2f26e7dc9da40eab0cad84",
      "is_reseller": false,
      "billing_mode": "normal",
      "ui_restrictions": {
      "myaccount": {
        "user": {
          "show_tab": false
        },
        "account": {
          "show_tab": false
        },
        "billing": {
          "show_tab": false
        },
        "balance": {
          "show_tab": false,
          "show_credit": false,
          "show_minutes": false
        },
        "service_plan": {
          "show_tab": false
        },
        "transactions": {
          "show_tab": false
        },
        "inbound": {
          "show_tab": false
        },
        "outbound": {
          "show_tab": false
        },
        "twoway": {
          "show_tab": false
        },
        "errorTracker": {
          "show_tab": false
        }
      }
   }
}
{% endhighlight %}

### Response Yang Harus Disimpan Oleh Aplikasi

- Simpan nilai response ```id``` atau ```account_id``` untuk dimapping ke database ```service_id``` dari Aplikasi
{% highlight json %}
"id": "0f13d185ab2f26e7dc9da40eab0cad84"
{% endhighlight %}

- Simpan nilai response ```enabled```. Nilai ```true ``` mengindikasikan akun aktif. Sedanglan nilai ```false``` menunjukkan akun non-aktif. Akan digunakan untuk enable/disable akun ketika isolir.
{% highlight json %}
"enabled": "true
{% endhighlight %}
