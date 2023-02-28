---
layout: default
title: Oledump
parent: CyberSecurity
grand_parent: Notes
---

## Oledump

### Meaning:

`Oledump` ([oledump.py](https://blog.didierstevens.com/programs/oledump-py/)) is a tool written in python, that can analyze `OLE` (`Compound File Binary Format`).
They are similar to a `Zip` archive, and are more commonly known as the `MS Office` extensions (`.doc`, `.xls`, `.ppt`).

Malicious actors may use macro to hide command / scripts within these documents.

### Running:

You can run `Oledump` with the `oledump.py [Filename]` command. 

```console
C:\Desktop\Tools>oledump.py Document1.doc
1: 114 '\x01CompObj'
2: 4096 '\x05DocumentSummaryInformation'
3: 4096 '\x05SummaryInformation'
4: 13859 '1Table'
5: 33430 'Data'
6: 365 'Macros/PROJECT'
7: 41 'Macros/PROJECTwm'
8: M 9852 'Macros/VBA/ThisDocument'
9: 5460 'Macros/VBA/_VBA_PROJECT'
10: 513 'Macros/VBA/dir'
```

The letter `M` next to a stream indicates that the stream contains a **VBA Macro**.
Each stream additionaly has a number and a name to easily select one file for analysis. 

### Options: 

**COMMAND** | **DESCRIPTION** |
`-A` | Does an **ASCII** dump similar to option `-a`, but duplicate lines are removed |
`-S` | Dumps strings |
`-d` | Produces a **RAW** dump of the stream content |
`-s STREAM NUMBER` or `--select=STREAM NUMBER` | Allows you to select the stream number to analyze (`-s a` to select all streams) |
`-d`, `--dump` | Perform a **RAW** dump |
`-x`, `--hexdump` | Perform a **HEX** dump |
`-a`, `--asciidump` | Perform an **ASCII** dump |
`-S`, `--strings` | Perform a strings dump |
`-v`, `--vbadecompress` | **VBA** decompression |
