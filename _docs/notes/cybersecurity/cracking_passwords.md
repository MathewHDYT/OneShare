---
layout: default
title: Cracking Windows Passwords
parent: CyberSecurity
grand_parent: Notes
---

## Cracking Windows Passwords

### Meaning:

A standard tool used to retrieve password hashes from memory is called `mimikatz` ([GitHub](https://github.com/gentilkiwi/mimikatz)) is a tool written in C and has various modules that can be used to extract different kinds of credentials from memory and use these credentials to access user accounts.

### Running:

You can run `mimikatz`, when you open the downloaded folder and navigate to the x64 binary folder.
Then run the `mimikatz` program.

To check if we have the appropriate privileges to run the program, we can use `privilege::debug`.
This should return `Privilege '20' OK`, if succesfull.

Once the privilege has been verified, we can dump the passwords from `LSASS` (`Local Security Authority Subsystem Service`) process memory.
Which stores users credential in memory when a user succesfully logs on, so we don't have to reenter the credentials when we access other resources.

To dump the passwords we use the `sekurlsa` module, which is used to extract various credentials from the `LSASS` memory.
You need atleast **Administartor** access or a **SYSTEM account** to use this module.

We then can use the `logonpasswords` function to extract the credentials for currently logged in users.

We then see all logged in users credentials including a `NTLM` and `SHA1` hash of their password.

### Password Cracking: 

Because we've got the password hashed we can use a tool like [John the Ripper](https://www.openwall.com/john/), to read the hash from a file and return a potential input.

We can create a with a given string in the current directory with `echo 'PASSWORD-HASH-HERE'> hash.txt`.

After that we can use the tool to bruteforce the input text  with `john --format=NT -w=/usr/share/wordlists/rockyou.txt hash.txt --pot=output.txt`.

- `--format=NT`: Is used to represent the type of the hash. In this case, we're specifying that we want to crack an `NTLM` hash. You can find a full list of formats supported by **john** using the `john --list=formats command`
- `/usr/share/wordlists/rockyou.txt`: Is the wordlist containing the input passwords that will be hashed by john and compared against our hash
- `hash.txt`: Represents the text file containing the hash
- `--pot=output.txt`: Represents the output file that the clear text retrieved password will be stored in

The output is then saved into the file with the given format being:

```
$HASH-TYPE$PASSWORD-HASH:RECOVERED-PASSWORD
```
