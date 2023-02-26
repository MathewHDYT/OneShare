---
layout: default
title: File Analysis
parent: CyberSecurity
grand_parent: Notes
---

## File Analysis

### Initial analysis:

To check the actual file type of a file the command `file <filename>` can be used.
Which returns the actual file type regardless of extension.

Additionaly we can use the `strings <filename>` command to extract and print the printable character sequences from a given file.
Especially for executable files, the **strings utility** can give pointers to the different functions that are being called, any **`IP` addresses** or **domain names**, **URLs**, etc.

If the output is to long it can be saved into a file with the addition of the filename `strings <filename> > strings_output.txt`.

### Virus Total: 

The functionality that the inital analysis showed could most of the time be used in a non-malicious program.
To make sure, we can upload it to [VirusTotal](https://www.virustotal.com/gui/home/upload), which will scan **files**, **URLs**, **IP addresses**, **domains**, or a **file hash** you provide using 60+ different Antivirus software produicts and display a summary of the scan results.

To get the file hash we can use the command `md5sum <filename>`.
