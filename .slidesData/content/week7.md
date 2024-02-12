---
title: "Week 7 - Client Side"
layout: "bundle"
outputs: ["Reveal"]
date: 2023-03-28T02:57:21+10:00
---

{{< slide class="center" >}}

## Week 7

<!-- NOTE: You should run the XSS demo now and put it in the background for effect -->

---


### Origin vs Site

{{% section %}}

What is an origin?

> <span style="color: #f1c232">http://</span><span style="color: #00b050">www\.website\.com</span><span style="color: #0000ff">:80</span>

origin = <span style="color: #f1c232">scheme</span> + <span style="color: #00b050">host</span> + <span style="color: #0000ff">port</span>

---

What is a site?

> <span style="color: #f1c232">http://</span><span style="color: #80ffba">www.</span><u><span style="color: #00b050">website</span><span style="color: #007133">.com</span></u><span style="color: #0000ff">:80</span>  
> <span style="color: #f1c232">https://</span><span style="color: #80ffba">mobile.</span><u><span style="color: #00b050">website</span><span style="color: #007133">.com</span></u><span style="color: #0000ff">:443</span>  
> <span style="color: #f1c232">ftp://</span><span style="color: #80ffba">f.</span><u><span style="color: #00b050">website</span><span style="color: #007133">.com</span></u><span style="color: #0000ff">:21</span>  

site = <span style="color: #00b050">private_domain</span> + <span style="color: #007133">public_suffix</span>

* <span style="color: #f1c232">Scheme</span> doesn't matter
* <span style="color: #0000ff">Port</span> doesn't matter
* Just the <u><span style="color: #00b050">domain</span></u>!

---

> <span style="color: #f1c232">http://</span><span style="color: #80ffba">www.</span><u><span style="color: #00b050">website</span><span style="color: #007133">.com</span></u><span style="color: #0000ff">:80</span>  

&nbsp;  

origin = <span style="color: #f1c232">scheme</span> + <span style="color: #80ffba">h</span><span style="color: #00b050">os</span><span style="color: #007133">t</span> + <span style="color: #0000ff">port</span>

&nbsp;  

site = <span style="color: #00b050">private_domain</span> + <span style="color: #007133">public_suffix</span>  
site = <u><span style="color: #00b050">dom</span><span style="color: #007133">ain</span></u>

{{% /section %}}

---

### Cross-Origin Policy

{{% section %}}

* Blocks external resource requests from/to a site
    * Why?
        * Security
        * Enforce usage limits
* Enforces a "Same Origin Policy"
    * Only pages from the same origin <span style="font-size: 0.7em">(<span style="color: #f1c232">scheme</span> + <span style="color: #00b050">host</span> + <span style="color: #0000ff">port</span>)</span> are allowed to <span style="color: #ffb0ef">use</span> the response content
    * Unless the remote resource has the right `Access-Control-*` headers

> [test-cors.org](https://www.test-cors.org)

---

> "Only pages from the same origin are allowed to <span style="color: #ffb0ef"><u>use</u></span> the response content"

* Can I still <span style="color: #ffb0ef">make</span> the request?
  * What does the server receive?
* GET? POST? PUT?
* OPTIONS?

---

> <span style="font-size: 0.8em">"If I need to be on the allow list, then will my Python / Node.js / etc script still work?"</span>

* 🤷‍♂️ idk try it
  * Why does/doesn't it work?
* Browser
    * CORS Proxy - i.e. CORS Anywhere 
    * JSONP

---

#### JSONP

> JSON with Padding

```js
// http://website.com/request?callbackName=response //

response({data...})
```

* A server might not have CORS headers
* Or maybe it's too difficult
* So, supply the result as a JS source file!

Let's make a server ~

---

> Q. Does every program respect CORS?  
> <span style="font-size: 0.6em">(A. no)</span>

{{% /section %}}

---


### CSRF

{{% section %}}

> "Heyyyy, look <a class="sparkle">here</a> 🥺 👉👈"

<script src="https://ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"></script>
<script src="https://evejweinberg.github.io/sparkleHover/sparkleHover.js"></script>

<script>
    for (let elem of document.getElementsByClassName('sparkle')) {
        $(elem).sparkleHover({
            colors : ['#297E97', "#2EB8D5",'#36BEC1'],
            num_sprites: 22,
            lifespan: 3000,
            radius: 500,
            sprite_size: 40,
            shape: 'circle',
            image: 'http://loganchamber.org/files/Pumplin.jpg'

        })
    }
</script>

Tricking a user into performing an operation that they don't intend

![](https://featherbear.cc/tutoring-unsw-21t2-cs6443-cs6843/week7-shared/Snipaste_2021-07-15_05-25-43.png)

---



##### CSRF Tokens

Stop CSRF attempts by supplying the user with a single-use 'nonce' value.

* When the page loads, a nonce is included
  * e.g. cookie, header, HTML form
* The nonce must be passed in the request

_Can't forge a request if you don't know the nonce before hand... sort of..._

---

##### CSRF Token Attacks

* Better hope the server is handling CSRF tokens properly
* Single. Use. grr.
* Forge?
* Override?

---

> CSRF vs XSS

* XS<u>S</u> - The JavaScript method
* CSRF - The HTML method

Generally XSS is performed in the background  
(since it's a script exploit)

{{% /section %}}

---

## HTML

{{% section %}}

* DOM
  * Virtual DOM
  * Shadow DOM
* Some element tags are paired - `<div>...</div>`
* Some are not... `<img>`
* What if just added `<s>` without closing it?

---

##### HTML + JS

* document.write()
* document.querySelector\[All\]()
* document.getElementsByTagName()
* document.getElementById()
* Script tags
  * External script tags.. [load lifecycle](https://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html)
* HTML Injection
  * `<script>` --> JS injection

{{% /section %}}

---

### Clickjacking

{{% section %}}

Click  

on

<div id="jack_elm" data-height=400 data-width=400 style="cursor: pointer; margin: 0 auto;"><span><u>Jack</u></span></div>

?

🙄

<script>
{
    let images = [
        "https://static.wikia.nocookie.net/disney/images/2/28/Profile_-_Jack_Sparrow.png",
        "https://static.wikia.nocookie.net/titanic-1997-movie/images/1/1f/AC210765-BEDB-4C59-AF64-CB6FD707E5E7.jpeg",
        "https://media.istockphoto.com/photos/floor-jack-isolated-picture-id1129827496?k=6&m=1129827496&s=612x612&w=0&h=fpxEy1euO1dlc7Zcr51G6a1hlWk523B1oB1o2jPaHzk=",
        "https://static.wikia.nocookie.net/disney/images/2/28/Profile_-_Jack_Jack_Parr.jpeg",
        "https://media.istockphoto.com/id/472368857/vector/jack-in-the-box.jpg?s=612x612&w=is&k=20&c=h04GUPLDmHrBgdz2AXCtvrXKGmd4BT-Onm0GyYDnlKQ=",
    ]
    
    images = images.map(url => {
        let elem = new Image();
        elem.src = url
        elem.style.height = "100%"
        elem.style.width = "100%"
        return elem
    }) 

    let elem = document.getElementById('jack_elm');

    const increment = 10;

    elem.addEventListener('click', function () {
        let next;
        while ((next = images[Math.floor(Math.random() * images.length)]) == elem.children[0]);
        elem.replaceChildren(next)

        elem.dataset.width = Number(elem.dataset.width) + increment
        elem.style.maxWidth = `${elem.dataset.width}px`

        elem.dataset.height = Number(elem.dataset.height) + increment
        elem.style.maxHeight = `${elem.dataset.height}px`
    })
}
</script>

---

An overlay (possibly hidden or transparent) with a higher `z-index` than a form control (i.e. submit button)

When the user "clicks" on the button, they instead click on something else.

* Trigger a script?
* Redirect?
* etc..

Or maybe the other way around?

---

A fake form that sits on top of a form. Causes the user to unintentionally interact with the legitimate form.

> i.e. switch the Yes / No buttons around?

(`pointer-events`)

Note: The legitimate form can be in an `iframe`

---

#### Defense

Some extensions / JS magic can help to prevent clickjacking attempts.  

<img src="https://img.ifunny.co/images/04e58da8a3b3b9079850479b6818ab204831656d79d4ac379fe687d46216db1c_1.jpg" height=400px>

---

#### Defense 2

> X-Frame-Options

{{% /section %}}

---

#### A Hacker's Toolkit

* HTML is cAsE INSEnSItive
* `<script>...</script>`
* `<img onerror="..." src="x">`
* `document.write(...)`
* Fetch API
* XMLHttpRequest
* jQuery

---

## XSS

{{% section %}}

* What - User content is rendered as HTML

* How - The server unsafely puts user content into the response body of a request. The browser interprets the payload as if it was legitimate HTML

---

#### Reflected XSS

The payload is part of user input  
_(i.e. URL bar, inside a cookie, etc)_

> Demo: [`Reflected XSS`](http://localhost:3000/reflected?q=%3Cimg+onerror%3D%22alert%28%27pwned%21%27%29%22+src%3D%22http%3A%2F%2Fgoogle.com%22%3E)

Who'd click on them though....  
But also like, link shorteners...  
But also like, "..."

---

#### Stored XSS

The payload is stored in some sort of database.  

Arguably more dangerous...
Anyone who opens a page that returns content from that same database may be victim to a stored XSS attack

+ Twitter Hack

> Demo: Stored XSS

---

#### DOM-based XSS

The client pieces together data which eventually becomes an exploit itself.

> i.e. `document.write(...)`

{{% /section %}}

---

#### Mitigating XSS

{{% section %}}

* Validation - Blacklisting / whitelisting of input
* Sanitisation - Remove unsafe tags and attributes
* Encode - Escape data so it's not a control character

---

### Defense

Don't use `.innerHTML` or `.outerHTML`

use `.innerText` or `.textContent`

> Demo: DOM XSS

JS Frameworks

---

> `X-XSS-Protection` header

Turn it off, it's broken 🔥 🌊🚒 

✅ `X-XSS-Protection: 0`  

* Sometimes causes client-side errors on real code
* Has its own [vulnerabilities](https://github.com/helmetjs/helmet/issues/230)

> <span style="font-size: 0.6em">"Firefox never supported X-XSS-Protection and Chrome and Edge have announced they <s>are dropping</s> have dropped support for it."</span>

---

> Sandboxing

iframes, sandbox, seamless, etc...

Link: [GitHub](https://github.com/featherbear/UNSW-CompClub2019Summer-CTF/commit/778c26087f0f6baa7b286037b4fe162aefd4ad67
)

<!--
https://github.com/featherbear/UNSW-CompClub2019Summer-CTF/commit/778c26087f0f6baa7b286037b4fe162aefd4ad67
https://github.com/featherbear/UNSW-CompClub2019Summer-CTF/commit/7b6e5875f31fc1c12da9fbd9e149e833130fd4e2
-->


{{% /section %}}

---

#### Mitigating XSS Mitigations

* Filter evasion [[1]](https://github.com/payloadbox/xss-payload-list) [[2]](https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting) [[3]](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)
* TL;DR Abuse bad sanitisation

---

## CSP

{{% section %}}

Only allow scripts / images / styles / ... from ____

> it kinda OP.

* block eval(), inline scripts, iframes
* whitelist by scheme, domain, path, ...
* nonce
* checksum
* reporting
* ...
<!-- * click jacking frame-ancestors -->

---

#### Implementation

* Define CSP inside HTTP headers, or <meta> tags
* Meta tags have a lesser CSP capability
  * (in case you can override meta tags)
  * But still has its flaws...

&nbsp;

---

#### Exploits

* Exploiting trust
  * Redirected
  * Code Execution
  * Inheritance
* Invalidating restrictions
  * Corrupt the CSP header
  * Response splitting

{{% /section %}}

---
