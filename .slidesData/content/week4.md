---
title: "Week 4"
layout: "bundle"
outputs: ["Reveal"]
date: 2024-03-04T10:00:00+11:00
---

{{< slide class="center" >}}

## Week 4

---

{{< slide content="secedu.goodfaith" >}}

---

## Anything to go over?

---

## Topic 2 Review

{{% section %}}

> User Identity, AuthN, AuthZ

---

* Authentication
  * Validating the user is the identity they claim
* Authorisation
  * Giving the user access based on their identity

---

Don't do this.  
Information disclosure baaaad.

![](https://memeguy.com/photos/images/password-already-in-use-262036.png)

---

#### Hashing

* Hashing is not encryption
* One way function

&nbsp;  

#### Salts (from 6[84]41) 🧂

Random bytes that are mixed into a passphrase to modify the hash values.

* Increases entropy
* 🌈 Mitigates rainbow table attacks

---

#### Preventing Spam

* Captcha
* Account Lockout
* Rate Limiting

---

C is for ______

![](https://miro.medium.com/max/1230/0*vwtmE6kZFO0rIq9o.)

---

Don't use cookies to store important stuff.  

If you have to, secure it.

* HTTP Only
* "Secure" mode
* Encrypt the cookie
* JWTs?

{{% /section %}}

---

### SQL

{{% section %}}

<div class='a'>

* Structured Query Language
* MySQL, SQLite, PostgreSQL, Microsoft SQL, NoSQL
* Statements
  * `SELECT _ FROM _ ...`
  * `INSERT INTO _ (COLn, ...) VALUES (VALn, ...)`
  * `UPDATE _ SET _ = _ ...`
  * `DELETE FROM _ ...`
  * `... -- this is a comment`
  * ...

</div>

<style>
    div.a code {
        font-size: 0.7em; 
    }
</style>

---

* WHERE
  * `>` - greater than
  * `<` - less than
  * `=` - equal to
  * `<>` - not equal to
  * ...
* LIKE
  * `%` - wildcard
* UNION
* ...

---

* `ORDER BY`
* `GROUP BY`
* `DISTINCT`
* `LIMIT`
* `OFFSET`

---

> [Demo](https://github.com/featherbear/demo-sqli): `stock` table

* `SELECT`, `INSERT`, `UPDATE`, `DELETE`
* Fingerprinting
  * `@@Version` - Microsoft SQL
  * `Version()` - MySQL
  * `sqlite_version()` - SQLite
* Functions
* Schema / Meta

<!-- sqlite_master -->

{{% /section %}}

---

## SQL<span style="text-transform: lowercase">i</span>

{{% section %}}

> user input = dangerous

```
'";<lol/>../--#`ls`
```

* What?
  * User input contains control characters that interfere with the syntax of the SQL statement
* Why?
  * Bad programming
  * User input is 'trusted' to be valid and reasonable

---

###### How

```sql
SELECT a FROM b WHERE a = '$userInput'
```

&nbsp;  

Using `' OR '1'='1`

```sql
                           vvvvvvvvvvvvv
SELECT a FROM b WHERE a = '' OR '1' = '1'
                           ^^^^^^^^^^^^^
```

---

> Demo: login 1, login 2

Step 0: Figure out the syntax, and fingerprint if needed

* Bypass logins
* Extract data
* Spoof a user - `USER_DOESNT_EXIST`

{{% /section %}}

---

## Mitigating SQL<span style="text-transform: lowercase">i</span>

* Disable debug logging
  * No error messages, maybe just a blank screen?
* WAF - Web Application Firewalls
  * Strip out malicious payloads™
  * ...or completely block access
* Parameterised Queries

> Demo: login 3

---

## Defeating mitigations

* Payload stripped? Embed dummies
* Blank response? Side channel attacks
    * Timing Attacks
    * Out of Band Attacks
        * i.e inbuilt functions (therefore fingerprint!) <!-- i.e. concat, outfile -->
* Error-based extraction
* Boolean-based extraction <!-- i.e. substr comparison -->
* Subqueries
  * `SELECT a,b FROM c WHERE d UNION SELECT (SELECT ...), 2`

---

### Note

* Sometimes you need to big brain the payload
* Reporting a vulnerability `!=` extracting data
* SQLi payload list
* Big database? - `COUNT()` it instead

![](https://featherbear.cc/tutoring-unsw-21t2-cs6443-cs6843//week4-shared/Snipaste_2021-06-23_22-49-24.png)

---

### Other Injections

{{% section %}}

##### MongoDB (NoSQL)

* JSON
* Data is transmitted as strings
* Escape the string?

---



##### Local File Inclusion

* `http://website.com/getImage.php?file=image.png`

* `http://website.com/getImage.php?file=/etc/passwd`

---

##### Server Side Template Injection

e.g. Jinja templating (Python + Flask)

```
{{ "hello " + "world" }} => "hello world"
```

```sql
{{ "".__class__.__mro__[1].__subclasses__() }}
                     ^ the `object` class

You now have a handle to every function. welp.
```

{{% /section %}}

---

## Server Side Request Forgery



{{% section %}}

> Think back to HaaS

* Only access to `kb` from `haas`
* We could send network requests that appeared to originate from the `haas` server
* Corporate / internal network?

---

#### Main Idea

Utilising functionalities of a server to access resources

* Information retrieval
* Information disclosure - i.e. cse login servers
* Can lead to RCE
* Server Side Includes

---

#### Mitigation

* URL parsing is hard
* Whitelist domains and IPs!
* Lower the access control of services
* Set limits! exec time, file sizes, recursion depth
* Local devices should NOT be assumed to be safe

{{% /section %}}
