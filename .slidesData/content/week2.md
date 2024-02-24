---
title: "Week 2"
layout: "bundle"
outputs: ["Reveal"]
date: 2024-02-20T10:00:00+11:00
---

{{< slide class="center" >}}

## Week 2

---

{{< slide content="secedu.goodfaith" >}}

---

{{% section %}}

## HTTP - Overview

> `GET / HTTP/1.1`

* Methods
* Path
* HTTP Version
  * HTTP/1 vs HTTP/2 vs HTTP/3

---

## Inside a HTTP Request

* Headers
  * e.g `Host` (why)
  * e.g `Content-Type` and `Content-Length`
* Body
  * Body types - (forms, JSON, Protobuf, gRPC)
  * What happens if Content-Length is...
    * Too little?
    * Too big?
  * `\r\n` is important!

{{% /section %}}

---

## Proxies and Jumphosts

Let's look at `haas.quoccabank.com`!

{{% note %}}
Talk about how knowing a DNS record doesn't necessarily mean that you can access the site (IP).
Give examples of private addresses, VPNs, SDNs

KB is only accessible inside of the QuoccaBank network.
HAAS is inside the QuoccaBank network, and can access KB!
{{% /note %}}

---

## How2Burp

* HTTP History
* Repeater
* Interceptor
* Extensions

> Pages not loading?  
> Turn off intercept

---

## Source code vs DOM

* Source code - The actual server response
* DOM - Browser assumes the content

&nbsp;  

Should we look at both?

> When is one preferred over the other?

{{% note %}}
Good to mention that malformed HTML will be re-interpreted by the browser.
Try show a `<div><b><i>something</b></div>` with a missing closing `</i>` tag and see how the DOM throws the `</i>` everywhere
{{% /note %}}

---

## AuthN vs AuthZ

* AuthN - Who am I?  
* AuthZ - What can I do?

---

## Server, this is me!

* Sessions
* Cookies
* JWT
  * <s>I should store my password in my JWT</s>
    * Claims are public (if you have the token)
  * [jwt.io](https://jwt.io/)
  * Is this a JWT?
* [CyberChef](https://gchq.github.io/CyberChef/)
  
---

## Report

* Group Projects (3 people)
* Vulnerability Programs
  * [CVE](https://www.cve.org/)
  * [JVN](https://jvn.jp/en/) (Japan)
* Vulnerability scoring
  * CVSS
  * NIST
  * OCTAVE
  * Make up your own?

[Sample Report](https://docs.google.com/document/d/1dVXbABRPlAic2oNHqafXKrGmOYFSha-8_4kfLE_ilbQ/edit)

{{% note %}}
Don't need a summary...
Mention numbering of the sections
Order summary by critical vuln
{{% /note %}}
