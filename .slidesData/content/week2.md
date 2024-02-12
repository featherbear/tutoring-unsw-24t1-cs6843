---
title: "Week 2 - Auth and IAM"
layout: "bundle"
outputs: ["Reveal"]
date: 2023-02-23T02:57:21+10:00
---

{{< slide class="center" >}}

## Week 2

---

# Recon

{{% section %}}

* Active / Passive Recon
* Domain / DNS [Enumeration](https://featherbear.cc/UNSW-COMP6443/post/enumeration/)

---

* Website Recon
  * e.g. Wappalyzer
  * 🤖 Beep boop I'm a robot
  * Beep boop I'm a [sitemap](https://jbhifi.com.au/sitemap.xml)
  * Use the sauce 

---

* Website Enumeration (later!)
  * Directory Busting
  * IDOR

---

* Social Engineering

{{% /section %}}

---

# Website Basics

* URLs
* HTTP
  * Methods
  * Versions
  * Headers
  * State(lessness)
* HTTPS
* Cookies

---

# Auth[...]

`authentication != authorisation`

* Authentication - Who am I?
* Authorisation - What can I do?

---

### Identity & Session Management

{{% section %}}

> How do we store sessions?

Often as cookies

* Session IDs
* Plain-text
* JWT
* Flask session

---

> How do we steal cookies?

Idk I only have sewing kits...

* MITM
* Physical access
* XSS
* Anything else?

---

> How do I keep my cookies safe?

* Domain restriction
* Expiry
* HTTP Only flag
* Secure flag

{{% /section %}}


---

<!--

Hashes

https://github.com/featherbear/UNSW-CompClub2019Summer-SecurityWorkshop/tree/master/http_mitm

-->

<!-- 

# Report

https://docs.google.com/document/d/1dVXbABRPlAic2oNHqafXKrGmOYFSha-8_4kfLE_ilbQ/edit

-->
