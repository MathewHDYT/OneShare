---
layout: default
title: HTTP(S)
parent: CyberSecurity
grand_parent: Notes
---

## HTTP(S)

### Meaning:

`HTTP` stands for `Hypertext Transfer Protocol` and is a client-server protocol that allows communication between a client and web server.
Requests are similar to a standard `TCP` network request, however `HTTP` adds specific headers to identify the protocol and other information.

When a `HTTP` request is crafted the **method** --> `how to retreive` and **target header** --> `what to retrieve` will always be included.

When retrieving information from a web server, it is common to use the `GET` method, such as loading a picture.

When sending data to a web server, it is common to use the `POST` method, such as sending login information.

### Example Request:

```
GET / HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com/
```

The status code tells the client browser how the web server interpreted the request. Most common success status code is `HTTP 200 OK `.

### Example Response: 

```
HTTP/1.1 200 OK
Server: nginx/1.15.8
Date: Wednesday, 24 Nov 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98

<html>
<head>
    <title>Advent of Cyber</title>
</head>
<body>
    Welcome To Advent of Cyber!
</body>
</html>
```

The protocol itself is only a small part, once it is retrieved from the web server, your browser needs a way to interpret and render the information sent.
Web applications are commonly formatted in `HTML` (`HyperText Markup Language`), rendered, and styled in `CSS` (`Cascading Style Sheets`).
`JavaScript` is also commonly used to provide additional functionality. 
