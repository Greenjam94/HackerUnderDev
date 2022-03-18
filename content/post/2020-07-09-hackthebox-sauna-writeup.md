---
author: Greenjam94
categories:
- Walkthroughs
ct_period_last_updated:
- default
date: "2020-07-09T03:53:45Z"
guid: https://www.hackerunder.dev/?p=898
id: 898
image: /wp-content/uploads/2020/07/HTB_Sauna.png
tags:
- evil-winrm
- HTB
- impacket
- kali
- kerberos
- mimikatz
- winPEAs
title: HackTheBox Sauna Writeup
url: /hackthebox-sauna-writeup/
---

Sauna is another “easy” Windows machine on HackTheBox. However I definitely fell down my fair share of rabbit holes on this one. There’s a static website hosted here, so I thought it’d start with a web shell. However, this box turned out to to be entire about domains and LDAP. Which I have very little experience with to date. While this blog may sound like a straight path, it’s well edited to be stream lined. I’ll include the blogs and posts I found most helpful while trying to get into Sauna.

## Lessons learned:

- nmap scripts for domain enumeration
- username enumeration / wordlist generation
- impacket usage
- evil-winrm usage
- windows automated enum with winPEAs
- mimikatz and lsadump

## Information Gathering

As always I started with a basic nmap scan to see what was listening.

<figure class="wp-block-image size-large">![nmap 10.10.10.175](https://www.hackerunder.dev/wp-content/uploads/2020/07/sauna_1_nmap-1024x641.png)<figcaption>Nmap basic scan results</figcaption></figure>Of the listening services, I immediately took notice of:

- domain
- http
- msrpc (metasploit target?)
- ldap

Scanning dns on port 53 didn’t return much so I move right onto HTTP. The website looked like the company homepage / marketing for a “Egotistical Bank”. I spun up GoBuster to run in the background enumerating files and directories as I manually explored the site. The target was an employee login portal, or some identifying information of a framework in use that may be vulnerable. After no such luck, I gave up and had to look at the other services on the box.

The remaining services were all windows based and to be honest, I still don’t know the best way to attack most of them. port 88 was the next available one and nmap said it involved kerberos. The first google result for “nmap 88 kerberos” is the [krb5-enum-users NSE script](https://nmap.org/nsedoc/scripts/krb5-enum-users.html). Running the script requires a few parameters: a list of possible users and the domain to check against.

The website on the box had an about page that included employee names. I created a text file of the names mentioned and googled “windows domain username conventions”. There was a [company blog post](https://activedirectorypro.com/active-directory-user-naming-convention/) that listed common conventions, so I converted the full names to match what was possible.

The common conventions were as follows:

- First name and last name with a hyphen or period: john.smith
- First Initial and last name: jsmith
- Three letters of each name: johsmi
- Three random letters, Three random numbers: abc123

While it’s possible to build a likely wordlist from the about page, if it was the last option, we could generate a massive wordlist using `crunch 6 6 -t @@@%%% -o possible_usernames.txt`

To find the domain, I ran `enum4linux 10.10.10.175` and found it was **EGOTISTICALBANK**.  
Since LDAP is another service on the machine, another option to find more information about the domain would be to also use a tool called ldapsearch.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/sauna_2_ldapsearch_find_domain-1024x214.png)</figure>By running `nmap -p 88 --script krb5-enum-users --script-args krb5-enum-users.realm='EGOTISTICALBANK',userdb=userenum.txt 10.10.10.175` I was able to find that **fsmith** was a user.

## Exploitation

Now that we had a domain user, I had to figure out how that was useful. Thanks to [TJnull’s Pentesting Template](https://github.com/tjnull/TJ-JPT), which I’m expanding into a personal playbook of enumeration and attack techniques, I learned about impacket. One of the features of impacket is to check for Kerberoasting. There is a file called getNPUsers.py that takes domain users as a parameter.  
  
After a quick google, the github shows the source of the impacket script and it says:

<div class="wp-container-6233af706166e wp-block-group"><div class="wp-block-group__inner-container">> Description:  
> This script will attempt to list and get TGTs for those users that have the property  
> ‘Do not require Kerberos preauthentication’ set (UF\_DONT\_REQUIRE\_PREAUTH).  
> For those users with such configuration, a John The Ripper output will be generated  
> so you can send it for cracking.
> 
> <cite><https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetNPUsers.py></cite>

</div></div>By running the following script, it is possible to generate a hash file and crack it with john using rockyou.txt.  
`python3 /usr/share/doc/python3-impacket/examples/GetNPUsers.py EGOTISTICAL-BANK.LOCAL/ -usersfile foundusers.txt -format john -outputfile gotnpusers.txt -dc-ip 10.10.10.175`  
  
`john --wordlist=/usr/share/wordlists/rockyou.txt --format=krb5asrep gotnpusers.txt`

We see that the password for jsmith is **Thestrokes23**.

Now that we have creds, we need to find a way to login to the box with them. Googling for “kali kerberos remote shell” one of the results on the first page points to a github repo that allows linux machines to log into windows boxes using WinRM (Windows Remote Management). Also known as the Microsoft implementation of WS-Management Protocol.

`evil-winrm -i 10.10.10.175 -u FSmith -p Thestrokes23`

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/sauna_4_evil-winrm_user_shell-1024x236.png)<figcaption>Getting a user level shell on Sauna</figcaption></figure>With this shell we are able to explore the machine, get the user.txt flag, and attempt to gain higher privileges.

## Privilege Escalation

There are many ways to get higher privileges on a Windows box. In my last writeup, I learned how to with [DLL Hijacking](https://www.hackerunder.dev/hackthebox-remote-writeup/). All good privesc should start with solid enumeration. The more we know about the box, the better we can exploit it.

I started exploring the filesystem, like seeing what users were listed in c:\\Users. Simple commands like `set`, `net users`, and `systeminfo` can give a lot of context. First, by attempting to list DLLs but didn’t have permissions. Secondly, by creating a payload with msfvenom to connect with a meterpreter shell, running the exploitsuggester module also didn’t give me any leads. Thirdly, even automated scripts like JAWs or Sherlock were not giving much. Unfortunately, I was stuck here for a while.

While googling I found <https://book.hacktricks.xyz/windows/windows-local-privilege-escalation#useful-tools> and the first line says it all: the best tool is **winPEAs**.

After downloading the executable onto Sauna with certutil, I ran the script and it gave a lot of information. One section showed that the machine had stored credentials for another user.

```
<pre class="wp-block-preformatted">...
[+] Looking for AutoLogon credentials(T1012)
Some AutoLogon credentials were found!!
DefaultDomainName : 35mEGOTISTICALBANK
DefaultUserName : 35mEGOTISTICALBANK\svc_loanmanager
DefaultPassword : Moneymakestheworldgoround!
...
```

From user enumeration, we know the user is **svc\_loanmgr** and the password from above is **Moneymakestheworldgoround!**

We can again use evil-winrm to login as this user.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/sauna_5_svc_loanmgr_shell-1024x255.png)</figure>This user is likely to have different permissions, so again I downloaded and reran winPEAs. The results mentioned attempting to use lsadump. The first google result for “lsadump” led me to mimikatz. So I downloaded those files onto Kali, and transferred the executable onto Sauna.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/sauna_6_upload_mimikatz_exe-1024x346.png)</figure>Running `./mimikatz.exe "lsadump::dcsync /user:administrator" "exit"` as the svc\_loanmgr user gave me the NTLM hash for the Administrator

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/sauna_7_lsadump_find_ntlmhash-1024x727.png)</figure>The hash can be used with evil-winrm again to login as Administrator. From there we have full control of the computer. Grabbing the root.txt file completes our exercise.

<figure class="wp-block-image size-large">![](https://www.hackerunder.dev/wp-content/uploads/2020/07/sauna_8_evil-winrm_admin_shell-1024x227.png)</figure>