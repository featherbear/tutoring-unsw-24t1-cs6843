---
title: "Week 7 - Client Side (2)"
layout: "bundle"
outputs: ["Reveal"]
date: 2023-04-04T02:57:21+10:00
---

{{< slide class="center" >}}

## Week 8

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

When the user "clicks" on the button, they instead click on something else[.](https://featherbear.cc/demo-js-clickjack/)

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

### Breaking website protections

<!-- https://github.com/featherbear/demo-response-header-splitting -->

