---
layout: default
title: Yara
parent: CyberSecurity
grand_parent: Notes
---

## Yara

### Meaning:

`YARA` is a multi-platform tool for matching patterns of interest in files, to achieve that purpose [Yara rules](https://github.com/InQuest/awesome-yara) are utilized.
Is is most often used to perform research on malware families and identify malware with similar patterns.
It can help in categorizing malware in different malware families, and can also be used as a dection aid for malware analysis.

### Yara rules: 

The basic syntax of a [Yara rule](https://github.com/InQuest/awesome-yara) is as follows:

```yara
rule rulename {
    meta:
        author = "tryhackme"
        description = "test rule"
        created = "11/12/2021 00:00"
    strings:
        $textstring = "text"
        $hexstring = {4D 5A}
    conditions:
        $textstring and $hexstring
}
```

It start with the world rule followed by the name of the rule.

### Strings: 

This sections contains all the **strings** we watnt to match in a [Yara rule](https://github.com/InQuest/awesome-yara).
A declaration start with a `$` sign, followed by the name we want to assign the **string**.

**Strings** can be either text or a hexadecimal value.
To define text **strings**, we use double quotes and for hex **strings** we use curly brackets.
Text **strings** can additionaly use `regular expression` or `regex`, for more complex pattern matching.

### Conditions: 

This section defines the conditions that the file should meet for the rule to detect it. Conditions are boolean expression, and they use the strings defined in the strings sections as variables. 

**OPERATOR** | **CONDITION** |
------------ | ------------- |
`and` | All statements are true |
`or` | Any one statement is true |
`not` | The statement is false |

### Metadata: 

This section is optional. It starts witht the `meta` keyword.
It can be used to add additonal information about the rule to help the analyst in their analysis.
Generally it contains arbitrarily defined indentifiers, and their values, which are universally understood.

**Example**: `author`,` description`, and `created`

### Running rules:

```
yara [options] rule_file [target]
```

Giving the `options` here is optional.
If we run this command as it is, it will return us with the rule name and the file name if the rule is hit.
If the rule is not hit, it will not return anything.

Additionaly `options` for the [Yara rule](https://github.com/InQuest/awesome-yara) call: 

**CALL** | **DESCRIPTION** |
-------- | --------------- |
`-t tag --tag=tag` | Print rules tagged as tag and ignore the rest. This option can be used multiple times |
`-i identifier --identifier=identifier` | Print rules named identifier and ignore the rest. This option can be used multiple times |
`-n --negate` | Print rules that doesn't apply (negate) |
`-D --print-module-data` | Print module data |
`-g --print-tags` | Print the tags associated to the rule |
`-m --print-meta` | Print metadata associated to the rule |
`-s --print-strings` | Print strings found in the file |
`-p number --threads=number` | Use the specified number of threads to scan a directory |
`-l number --max-rules=number` | Abort scanning after a number of rules matched |
`-a seconds --timeout=seconds` | Abort scanning after a number of seconds has elapsed |
`-k slots --stack-size=slots` | Set maximum stack size to the specified number of slots |
`-d identifier=value` | Define an external variable. This option can be used multiple times |
`-x module=file` | Pass file's content as extra data to module. This option can be used multiple times |
`-r --recursive` | Scan files in directories recursively |
`-f --fast-scan` | Speeds up scanning by searching only for the first occurrence of each pattern |
`-w --no-warnings` | Disable warnings |
`-v --version` | Show version information |
