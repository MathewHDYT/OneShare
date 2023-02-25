---
layout: default
title: Content Discovery
parent: CyberSecurity
grand_parent: Notes
---

## Content Discovery

### Meaning:

Content is the assets and inner workings of an application. It can be files, folders, or pathways that weren't meant to be accessed.

### Example:

1. Configuration files
2. Passwords and secrets
3. Backups
4. Content management systems
5. Administrator dashboards or portals

These are just some of the examples we may be able to find. They can be found because they are stored either in a **folder** (which has a name) or a **file** (which has both a name and an extension). This means we can search for files by their extension. For example text files with the extension `.txt`

### Tools:

One of theee tools to simplify the process of checking if they are possible hidden files is [Dirbuster](https://www.kali.org/tools/dirbuster/). Which works by accepting a **wordlist** --> `file containg everything we want to search for` and a few other arguments.

![Dirbuster](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/dirbuster.png)

To get useful wordlists a collection of open-source wordlists like [SecLists](https://github.com/danielmiessler/SecLists) can be used to discover content with the right combination of content and the correct wordlist.

### Default credentials:

Web services and applications often come with a default pair of credentials. Developers leave these in place to get quickly started with the plattform (in hopes you change them later).

However not everyone does in fact, therefore applications often include other accounts that are not well documented. [SecLists](https://github.com/danielmiessler/SecLists) also provideds a list of default credentials.

Sometimes these credentials are stored in the web applications configuration file or public documentation, which then means that anyone will be able to log in using them.
