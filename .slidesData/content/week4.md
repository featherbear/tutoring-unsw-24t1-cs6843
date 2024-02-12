---
title: "Week 4 - SQLi"
layout: "bundle"
outputs: ["Reveal"]
date: 2023-03-08T02:57:21+10:00
---

{{< slide class="center" >}}

## Week 4

<!-- https://github.com/featherbear/tutoring-unsw-21t2-cs6443-cs6843/tree/master/sqli_demo -->

---

### SQL

{{% section %}}

* Structured Query Language
* MySQL, SQLite, PostgreSQL, Microsoft SQL, NoSQL
* Statements
  * `SELECT _ FROM _ ...`
  * `INSERT INTO _ (COLn, ...) VALUES (VALn, ...)`
  * `UPDATE _ SET _ = _ ...`
  * `DELETE FROM _ ...`
  * `... -- this is a comment`
  * ...

---

* WHERE
  * `>` - greater than
  * `<` - less than
  * `=` - equal to
  * `<>` - not equal to
  * ...
* LIKE
  * `_` - single wildcard
  * `%` - multiple wildcard
* UNION
* JOIN
* ...


---

> Demo: `stock` table

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

![](https://featherbear.cc/tutoring-unsw-21t2-cs6443-cs6843/week4-shared/Snipaste_2021-06-23_22-49-24.png)

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
