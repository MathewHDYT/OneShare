---
layout: default
title: Cookie Manipulation / Authentication Bypass
parent: CyberSecurity
grand_parent: Notes
---

## Cookie Manipulation / Authentication Bypass

HTTP is a stateless protocol, so when you send a request to a web server, it can not distinguish your request from someone else's. 

To solve this problem and identify different users and access levels, the web server will assign cookies to create and manage a stateful session between client and server.

Cookies are tiny pieces of data (metadata) or information locally stored on your computer that are sent to the server when you make a request.

Cookies can be assigned any name and any value allowing the web server to store any information it wants.

### Example:

Authentication or session cookies are used to identify you and what level of access is attached to your session.

![Assigning / Using Cookie](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/cookies.png)

When you send a request such as a login, your browser will send that information typically as a POST request. The web server will then verify that it received it and set a unique cookie.

Once the cookie is assigned as long as it stays locally stored in the browser all future GET requests will be automatically sent with a cookie to identify the user and access level. Additionally, once the server receives the GET request it will locate and deserialize (Process of taking a data format such as JSON and rebuilding it as an object) the session, if successful it returns with an HTTP 200 OK response.

### Cookie Components:

**COMPONENT** |	**PURPOSE** | **EXAMPLE** |
--------- | ------- | ------- |
Name	  | Unique cookie identifier (arbitrarily set by web-server). Always paired with the value component. | `SessionID`
Value	  | Unique cookie value (arbitrarily set by web-server). Always paired with the name component | `sty1z3kz11mpqxjv648mqwlx4ginpt6c`
Domain	  | Originating domain of the cookie. Sets the scope of the cookie | `.tryhackme.com`
Path	  | Local path to cookie. Sets the scope of the cookie | `/`
Expires / Max-age | Date / time at which cookies expire | `2022-11-11T15:39:04.166Z`
Size	  | Disk size of the cookie in bytes. This is typically {Name + Value} | `91`
HttpOnly  | Cookie cannot be accessed through client-side scripts | `(indicated by a checkmark)`
Secure	  | Cookie is only sent over HTTPS | `(indicated by a checkmark)`
SameSite  | Specifies when a cookie is sent through cross-site requests | `none`
SameParty |	Extends functionality of SameSite attribute to First-Party sets | `(indicated by a checkmark)`
Priority  | Defines the importance of a cookie. Determines whether it should be removed or held on to | `High`

The most important part of the cookie are `Name` and `Value`, because all the others are handled by the web server itself.

Important is that each value in a cookie consists of key value pairs.

```
Set-Cookie: <cookie-name>=<cookie-value>; Domain=<domain-value>; Secure; HttpOnly
```

### Cookie Manipulation:

Cookie Manipulation is taking a cookie and modifying it to obtain unintended behavior. This is possible because cookies are stored locally on the system, meaning you have complete control over them and can modify them as you please.

To start modifying a cookie you need to open the DevTools `F12` or `CTRL` + `SHIFT` + `I` then navigate to the `Storage` or `Application` tab and select the `Cookies` dropdown.

![Firefox Storage Tab / Cookies](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/storage.png)

Cookie values may seem random at first, however they often have an encoded value or meaning behind them that can be decoded to a non-arbitrary value such as a JavaScript object.

```javascript
{firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}
```

1. Obtain a cookie value from registering or signing up for an account.
2. Decode the cookie value.
3. Identify the object notation or structure of the cookie.
4. Change the parameters inside the object to a different parameter with a higher privilege level, such as admin or administrator.
5. Re-encode the cookie and insert the cookie into the value space; this can be done by double-clicking the value box.
6. Action the cookie; this can be done by refreshing the page or logging in.

To decode the cookie a tool like [CyberChef](https://gchq.github.io/CyberChef/) can be used.
