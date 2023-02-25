---
layout: default
title: User Privilege
parent: CyberSecurity
grand_parent: Notes
---

## User Privilege

### Account Types:

On a typical Windows Server, you may find different account types. These may include:

- **Domain Administrators**: This is typically the highest account level you will find in an enterprise along with **Enterprise Administrators**. An account with this level of privilege can manage all accounts of the organization, their access levels, and almost anything you can think of. A `"domain"` is a central registry used to manage all users and computers within the organization
- **Services**: Accounts used by software to perform their tasks such as back-ups or antivirus scans
- **Domain users**: Accounts typically used by employees. These should have just enough privileges to do their daily jobs. For example, a system administrator may restrict a user's ability to install and uninstall software
- **Local accounts**: These accounts are only valid on the local system and can not be used over the domain

### Windows Privilege Escalation Vectors:

- **Stored Credentials**: Important credentials can be saved in files by the user or in the configuration file of an application installed on the target system
- **Windows Kernel Exploit**: The Windows operating system installed on the target system can have a known vulnerability that can be exploited to increase privilege levels
- **Insecure File / Folder Permissions**: In some situations, even a low privileged user can have read or write privileges over files and folders that can contain sensitive information
- **Insecure Service Permissions**: Similar to permissions over sensitive files and folders, low privileged users may have rights over services. These can be somewhat harmless such as querying the service status (`SERVICE_QUERY_STATUS`) or more interesting rights such as starting and stopping a service (`SERVICE_START` and `SERVICE_STOP`, respectively)
- **DLL Hijacking**: Applications use DLL files to support their execution. You can think of these as smaller applications that can be launched by the main application. Sometimes DLLs that are deleted or not present on the system are called by the application. This error doesn't always result in a failure of the application, and the application can still run. Finding a DLL the application is looking for in a location we can write to can help us create a malicious DLL file that will be run by the application. In such a case, the malicious DLL will run with the main application's privilege level. If the application has a higher privilege level than our current user, this could allow us to launch a shell with a higher privilege level
- **Unquoted Service Path**: If the executable path of a service contains a space and is not enclosed within quotes, a hacker could introduce their own malicious executables to run instead of the intended executable
- **Always Install Elevated**: Windows applications can be installed using Windows Installer (also known as MSI packages) files. These files make the installation process easy and straightforward. Windows systems can be configured with the `"AlwaysInstallElevated"` policy. This allows the installation process to run with administrator privileges without requiring the user to have these privileges. This feature allows users to install software that may need higher privileges without having this privilege level. If `"AlwaysInstallElevated"` is configured, a malicious executable packaged as an MSI file could be run to obtain a higher privilege level 
- **Other software**: Software, applications, or scripts installed on the target machine may also provide privilege escalation vectors 

### Information Gathering: 

To find the correct vector to escalate windows privileges one of theese commands might help

- **Users on the target system**: The `net users` command will list users on the target system
- **OS version**: The `systeminfo | "OS"` command will output information about the operating system. This should be used to do further research on whether a privilege escalation vulnerability exists for this version
- **Installed services**: the `wmic service list` command will list services installed on the target system
