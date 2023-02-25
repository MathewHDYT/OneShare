---
layout: default
title: OSINT
parent: CyberSecurity
grand_parent: Notes
---

## OSINT

### Meaning:

`OSINT` stands for `Open Source Intelligence` meaning information that can be obtained from free and public sources.
Information is at the core of `OSINT` and that information can be obtained by typically 2 places.

- **Clearnet**: This refers to anything you can publicly access from your traditional web browser, including
  - Facebook
  - Twitter
  - GitHub
- **Darknet**: The darknet is accessed using special software and requires additional configuration, it is most commonly used by privacy-minded individuals, whistleblowers, censored people, criminals, journalists, and government law enforcement agencies. Below are a few examples of what the darknet has to offer:
  - TOR
  - Freenet
  - I2P
  - IPFS
  - Zeronet

Information used in `OSINT` originates from your digital footprint.
When conduting `OSINT`, we look at data the target left behind to lead us to the information / objective we are seeking. 

### OSINT Process Cycle: 

When conduting `OSINT`, each individual and team will have their methodology to approach the task.
The approach to `OSINT` can be quantified through models and systematic steps to follow.

![OSINT](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/osint.png)

**Intelligence** is obtained from information that is obtained from data, it will ultimately lead you our your customer to a decision.
This decision will then determine where in the model you will move.

![OSINT2](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/osint2.png)

**RSI PHASE** | **DEFINITION** | **EXAMPLE** |
------------- | -------------- | ----------- |
Client | What is the question/objective? | Email |
Source | What is available on the objective? | Email servers, etc. |
Monitoring | What is happening with the objective? | Email has been inactive |
Selecting / Finding | Where is the objective? How can we find/identify the objective? | Clearnet, Gmail, etc. |
Acquisition | How can we get the objective? | Leaked database |
Indexing | How is the objective retrievable? | Dehashed |
Syntheses | How can we combine this objective with others? | Identify information from email |
Dissemination | How can you action/quantify this objective? | Report / Plan |

### Account Discovery & Analysis 

Accounts are a prevalent and well-known part of a target's footprint. Accounts can include any public accounts linked to the target or a target persona, including but not limited to *Facebook*, *Twitter*, *Reddit*, etc. 

When analyzing an account, we are commonly looking at the following objectives: 

**OBJECTIVE** | **PURPOSE** |
------------- | ----------- |
Identify real or personas | A target will often use a persona. Depending on initial information, we are looking for their real name or persona to find other accounts. Our end objective is to identify further information and accounts owned by our target |
Identify email | This is less common to find openly but can help identify further information of the target and other sources |
Locate linked accounts | Targets will often link other public accounts leading you to further information or their real name / persona |
History | The importance of a target's post history will depend on your objective. This could be crucial to your investigation and lead you to what they're doing or pivot to another resource |
Information from posts | Continuing from a target's post history, you can obtain various information from a target's posts. This can include location, other accounts, real name, interests, etc. |

### Google Dorking

To even begin searching for information, we need to first index and quickly find these sources. To help us in that process Google has a known feature called **google dorks**, that allows us to use specific syntax in a search query to filter further and make the search more granular.

Small smaple list of google dorks a complete list can be found [here](https://www.sans.org/posters/google-hacking-and-defense-cheat-sheet/) or [here](https://gist.github.com/sundowndev/283efaddbcf896ab405488330d1bbc06).

**TERM** | **PURPOSE** | **EXAMPLE** |
-------- | ----------- | ----------- |
site | Specifically searches that particular site and lists all the results for that site | `site:"www.google.com"` |
filetype | Searches for a particular filetype mentioned in the query | `filetype:"pdf"` |
link | Searches for external links to pages | `link:"keyword"` |
inurl | Searches for a URL matching one of the keywords | `inurl:"keyword"` |
before/after | Used to search within a particular date range | `(before:2000-01-01 after:2001-01-01)` |

### OSINT & The Blockchain 

A core principle of blockchain technology and decnentralization is anonymity.
How can you gather information on a target if they are anonymous? 

Blockchain technology is completly open while still remaining anonymous, this comes with its pros and cons from an `OSINT` perspective, we can quickly identify specific identifiert but linking them can become difficlut. 

Multiple tools aid in exploring the blockain including but not limited to:
- Blocktrail
- Bitcoin Who's Who
- Graphsense
- Block Explorer

### Going Deeper 

Each plattform has its unique functionality and tricks that can be used, to get further information and links that may still be publicy accesible but are not easily found. 

For example **GitHub**, with its version control, which can lead to potential information leaks when not adequately sanitized or monitored. 

An example of an unintentional information leak from **GitHub** could be a company that forgot to delete its `API` keys from a `JSON` file. 

![Leak](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/leak.png)
