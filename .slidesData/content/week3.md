---
title: "Week 3"
layout: "bundle"
outputs: ["Reveal"]
date: 2024-02-26T10:00:00+11:00
---

{{< slide class="center" >}}

## Week 3

---

{{< slide content="secedu.goodfaith" >}}

---

## Anything to go over?

---

## I got hired by GitHub - a story

{{% section %}}

![](scam-github1.png)

---

[githubtalentcommunity [dot] githubcareers [dot] online)](./scam-github.html)

---

Looks like a scam?

Probably is... let's look at the source code?

---

### The login page

```
https://github.com/login?client_id=a541835be80a5d13734d&return_to=%2Flogin%2Foauth%2Fauthorize%3Fclient_id%3Da541835be80a5d13734d%26redirect_uri%3Dhttps%253A%252F%252Fgithubtalentcommunity.githubcareers.online%252Fauth%252Fcallback%26scope%3Drepo%2Buser%2Bread%253Aorg%2Bread%253Adiscussion%2Bgist%2Bwrite%253Adiscussion%2Bdelete_repo
```

```
Client ID: a541835be80a5d13734d
Scopes: repo user read:org read:discussion gist write:discussion delete_repo
```

[GitHub Scopes](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/scopes-for-oauth-apps)

---

### WHOIS you?

```bash
$> whois githubcareers.online

Domain Name: GITHUBCAREERS.ONLINE
Registry Domain ID: D435320268-CNIC
Registrar WHOIS Server: whois.godaddy.com
Registrar URL: https://www.godaddy.com/
Creation Date: 2024-02-23T03:15:47.0Z
Registry Expiry Date: 2025-02-23T23:59:59.0Z
Registrar: Go Daddy, LLC
Registrar IANA ID: 146
```

* Domain is registered through GoDaddy

---

### DIGing around

```bash
$> dig githubtalentcommunity.githubcareers.online

githubtalentcommunity.githubcareers.online. 3600 IN CNAME cryptic-narwhal-zql3j4yj4oqewyd5ne2d9uwh.herokudns.com.
cryptic-narwhal-zql3j4yj4oqewyd5ne2d9uwh.herokudns.com.	5 IN A 18.205.222.128
cryptic-narwhal-zql3j4yj4oqewyd5ne2d9uwh.herokudns.com.	5 IN A 52.202.168.65
cryptic-narwhal-zql3j4yj4oqewyd5ne2d9uwh.herokudns.com.	5 IN A 54.161.241.46
cryptic-narwhal-zql3j4yj4oqewyd5ne2d9uwh.herokudns.com.	5 IN A 54.237.133.81
```

* Site is hosted on Heroku

---

### Abuse Report

* Let GitHub know (affected platform)
* Let Heroku know (host of malicious software)
* Let GoDaddy know (domain provider of malicious site)

{{% /section %}}

---

## Important things to know

* Certificate chain of trust
* HTTPS process
* HSTS and the HSTS preload

---

## Authentication

* Username / Email + Password
* Phone Number + Code
* Email + Magic Link
* QR Code

---

## More on Sessions

* HTTP is stateless!
* Cookies pass state data
  * Information as state data
  * Identifiers as state data
* Session management ([demo](https://github.com/featherbear/demo-broken-session-management))

---

{{% section %}}

## Secrets / DevSecO(o)ps

* Q: Are JWTs good for passing secrets?
* Secrets via command line
* Secrets via environment
* Secrets via `.env`
* External secrets

---

## Externalising Secrets

* Key Servers / Key Stores
  * i.e. AWS Parameter Store
  * i.e. Google Secrets Manager
  * i.e. HashiCorp Vault
  * i.e. Apple Keychain
* Centralised management
* Rotation + deployment at scale
* Access control

{{% /section %%}

---

<!-- 

# Let's make an app

---

# Scripting

https://featherbear.cc/tutoring-unsw-23t1-cs6443/week3/#/7

-->
