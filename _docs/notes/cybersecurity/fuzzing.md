---
layout: default
title: Fuzzing
parent: CyberSecurity
grand_parent: Notes
---

## Fuzzing

### Meaning:

Fuzzing is the automated means of testing an element of a web application until it gives a vulnerability or valuable information.
When we are fuzzing, we provide information as we would typically when interacting with it, just at a much faster rate.
This means that we can use extensive lists known as wordlists to test a web applications response to various information.

### Example:

For example, rather than testing a login form for a valid set of credentials one-by-one, we can use a tool for fuzzing this login form to test hundreds of credentials at a much faster rate and more reliably.

### Tools: 

One of these tools to simplify the process of fuzzing is [BurpSuite](https://portswigger.net/burp/communitydownload).

First we open [BurpSuite](https://portswigger.net/burp/communitydownload) and navigate to the **Proxy** Tab, we click on Use Burps embedded browser and open the webpage we want to fuzz with it.

![Proxy](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/proxy.png)

Additionaly `Intercept is on` has to be actice, after that we enter the login credentials in the given website and return to [BurpSuite](https://portswigger.net/burp/communitydownload), we should now see the data that has been returned from the webserver.

We now select the portion with the `username` and `password` that was sent to us and send it to the with `Send to Intruder`, know that we've done that we have to switch to the `Intruder` tab. 

![Intercept](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/intercept.png)

In the `Intruder` tab under `Positions` we first of all use the `Clear $` to remove all $ signs, because this would fuzz for each of the marked settings. After that we select the settings we really want to fuzz, which depends which information we already have.

After we selected the items to fuzz and the attack type, we can switch to the `Payload` and select how many Payloads should be used and what `Payload type` we want. If simple list is choosen we need to provide a list of passwords which could be take from [SecLists](https://github.com/danielmiessler/SecLists) and one of their wordlists.

![Payload](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/payload.png)

After everything is set we can start the attack and see all possible payloads being tested and we get returned a `Length`. We sort by this `Length` and choose the one item that has a different `Length` that should be our solution.
