---
layout: default
title: PowerShell Transcription Logging
parent: CyberSecurity
grand_parent: Notes
---

## PowerShell Transcription Logging

### Meaning:

Enables us to capture input and output of **Windows PowerShell** commands, allowing us to review what happened when.
Typically, they can be enabled in the **Group Policy**, or alternatively by configuring the **Windows Registry**.

![Registry](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/registry.png)

#### Can also be done in a Administartor command promt:

```
reg add HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\PowerShell\Transcription /v EnableTranscripting /t REG_DWORD /d 0x1 /f
reg add HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\PowerShell\Transcription /v OutputDirectory /t REG_SZ /d C:/ /f
reg add HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\PowerShell\Transcription /v EnableInvocationHeader /t REG_DWORD /d 0x1 /f
```

Can be enabled in this way "per-user" via the `HKEY_CURRENT_USER` registry hive, or across the entire host via the `HKEY_LOCAL_MACHINE` registry hive.
