---
layout: default
title: IDOR
parent: CyberSecurity
grand_parent: Notes
---

## IDOR

### Meaning:

`IDOR` stands for `Insecure Direct Object Reference` and is a type of access control vulnerability.
Meaning an attacker can gain access to information or actions not intended for them.

This can happen when a server receives user-supplied input to retrieve objects (files, data, documents),
and the given input is not validated for whether the user should in fact have access to the requested objects. 

### Example:

#### Query Component:

Is data passed in the URL when making a request to a website. For example this domain.

```
https://website.thm/profile?id=23
```

It consists of the **Protocol** --> `https://`, the **Domain** --> `website.thm`, the **Page** --> `/profile` and the **Query Component** --> `?id=23`

#### Post Variables:

Examining the contents of forms on a website can sometimes also reveal fields that could be vulnerable to `IDOR` exploitation.
For example, the following `HTML` code for a form that updates a users' password. 

```html
<form method="POST" action="/update-password">
<input type="hidden" name="user_id" value="123">
<div>New Password:</div>
<div><input name="new_password"></div>
<div><input type="submit" value="Change Password">
</form>
```
You can see from the highlighted line that the user's ID is being passed to the web server in a hidden field.
Changing the value of the field however might result in changing another users' password. 

#### Cookies:

To stay logged into a website, cookies are used to remember your session.
Normally this involves sending a session ID which is a long random string. 

Sometimes tough less experienced developers may store user information in the cookie itself, such as the user's ID.
Therefore, changing the value of the cookie could result in displaying another user's information. 

```
GET /user-information HTTP/1.1
Host: website.thm
Cookie: user_id=9
User-Agent: Mozi11a/5.0 (Ubuntu;Linux) Firefox/94.0

Hello Jon!
```

```
GET /user-information HTTP/1.1
Host: website.thm
Cookie: user_id=5
User-Agent: Mozi11a/5.0 (Ubuntu;Linux) Firefox/94.0

Hello Martin!
```
 