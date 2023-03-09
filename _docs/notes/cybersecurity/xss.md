---
layout: default
title: XSS
parent: CyberSecurity
grand_parent: Notes
---

## XSS

### Meaning:

`XSS` means `Cross-Site Scripting` and is classified as an injection attack where malicious `JavaScript` code gets injected into a web application with the intention of being executed by other users.

### Example:

For example, if you can get `JavaScript` to run on another computer, there are numerous things you can achieve. Ranging from stealing cookies to taking over sessions, running a keylogger --> logs every key a user presses on their keyboard while visiting the given website, redirecting to a completely different website or performing an action on the site like placing an order, resetting the password etc.

### Vulnerability types:

**TYPE** | **FULL NAME** | **DESCRIPTION** |
-------- | ------------- | --------------- |
DOM      | Document Object Model | Programming interface for `HTML` and `XML` (*Extensible Markup Language is a markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable*) documents. Represents the page so that programs can change the document structure, style and content. Happens directly in the browser without any new pages being loaded or data submitted. Execution occurs when the website JavaScript code acts on input or user interaction. **Example**: Getting the contents from the window.location.hash parameter then writing that onto the page |
Reflected | - | Happens when user-supplied data in a `HTTP` request is included in the webpage source without any validation. **Example**: Error message which is in a query string of a `URL` that is reflected onto the webpage <https://website.thm/login?error=Username%20Is%20Incorrect>
Stored | - | Payload is stored on the web application (`DB`) and then gets run when other users visit the site. Particularly damaging due to the high number of victims that may be affected. **Example**: User input in a comment forum 
Blind  | - | Similar to the stored `XSS` but can not be seen working or tested against ourselves first. **Example**: Contact form, which might contain a `XSS` payload, which executes when a member of staff views the message. |
