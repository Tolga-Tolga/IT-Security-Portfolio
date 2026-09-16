# IT Security Portfolio

This repository is a collection of practical security work and hands-on learning notes. It serves as a portfolio of my practical work in IT security.

The repository documents how I approach technical problems: I examine the available information, form hypotheses, test them in a controlled environment, and record the reasoning behind the result.

## Featured practical work

### Hack The Box

- [Cap](htb/ctfs/Cap/Readme.md) — Linux machine involving service and web enumeration, an IDOR vulnerability, PCAP analysis, credential discovery, SSH access, and privilege escalation.
- [DevHub](htb/ctfs/DevHub/Readme.md) — Linux machine involving service enumeration, remote code execution, Jupyter API interaction, credential discovery, and multiple privilege-escalation steps.

These writeups show the complete process from initial enumeration to obtaining root access, including the observations and decisions made along the way.

## pwn.college — Web Security

The [Web Security](pwn.college/Intro%20to%20Cybersecurity/Web%20Security/Web%20Security.md) section contains notes and solutions from hands-on pwn.college exercises. It documents my progression through common web-security concepts, including:

- path traversal;
- command injection;
- authentication bypasses; and
- SQL injection.

The complete challenge set is available in the [challenges](pwn.college/Intro%20to%20Cybersecurity/Web%20Security/challenges/) directory. Recommended starting points are:

- [Path Traversal 1](pwn.college/Intro%20to%20Cybersecurity/Web%20Security/challenges/1.%20Path%20Traversal%201.md) — understanding path resolution and URL encoding;
- [CMDi 3](pwn.college/Intro%20to%20Cybersecurity/Web%20Security/challenges/5.%20CMDi%203.md) — analysing command injection in a quoted argument; and
- [SQLi 1](pwn.college/Intro%20to%20Cybersecurity/Web%20Security/challenges/11.%20SQLi%201.md) — analysing application logic and constructing a SQL-injection proof of concept in Python.

## Topics covered

- Linux and command-line workflows
- Network and service enumeration
- Web application security
- Source-code and binary analysis
- HTTP requests and APIs
- Credential discovery and responsible redaction
- Privilege escalation
- Technical documentation and structured problem solving

## Profiles

- [Hack The Box](https://profile.hackthebox.com/profile/019d95ce-2372-71d5-97b4-cdcdce26c46d)
- [pwn.college](https://pwn.college/hacker/193560)

## Repository structure

```text
htb/
└── ctfs/
    ├── Cap/
    └── DevHub/

pwn.college/
└── Intro to Cybersecurity/
    └── Web Security/
        ├── Web Security.md
        └── challenges/
```

The HTB writeups are machine reports. The pwn.college files are personal notes based on practical platform exercises and are included to document the learning process behind the practical work.
