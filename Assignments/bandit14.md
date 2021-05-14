# Bandit Level 14 to 15 Writeup



Author: [Kuldeep Singh Chouhan](https://github.com/kuldeep-singh-chouhan)

Problem Page: [bandit15](https://overthewire.org/wargames/bandit/bandit15.html)

## List of Commands Used
```
telnet - user interface to TELNET protocol
```

## Walkthrough
This level is slightly differernt from previous levels as this level asks us to connect to **localhost** and submit the password used for this level to obtain password for next level. By viewing the manual pages of the suggested commands **telnet** seems working. `man telnet` gives idea of the command which is `telnet host port`.

## Password
`BfMYroe26WYalil77FoDi9qh59eK5xNr`

## Bash/Python script to automate the process
```
#!/bin/bash
sshpass -p 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e ssh bandit14@bandit.labs.overthewire.org -p 2220
telnet localhost 30000
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
```

## Suggested modifications [Optional]
This level introduces TELNET, so it should be as basic as it is.
