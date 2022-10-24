---
author: Greenjam94
categories:
- Hacker101
tags:
- CAM2022
- NationalCybersecurityAlliance
- PasswordManagement
ct_period_last_updated:
- default
date: "2022-10-23T00:00:00Z"
title: Introduction to Password Management
url: /intro-to-passwords
---

Every computer, social media platform, or online tool requries some level of authentication. This usually requires a username and password. Correctly managing these credentials can be a defining point in defending yourself from an online attacker. What if I told you that a hand written log of passwords is not the most insecure means of password management?

# Key requirements of credentials

Lets start with the basics. A username is a value that is used to identify a user and a password is a secret that is used to verify a user is who they claim to be. Many systems use this as the first line of authentication, which means that between work and personal accounts there are a lot of secrets to remember.

These secrets would ideally be long and difficult to guess, **passwords should be complex**, meaning it contains multiple kinds of characters such as letters, numbers, and special characters. When these requirements are the first focus of passwords, the second factor is easily forgotten. **Passwords should be unique**, in case one password is stolen, it can't be used to pivot from one computer or account to another. The final requirement is that **passwords should be rotated**. When a provider has it's database stolen, or a company requires regular password changes every quarter or year, it is important to change passwords to a new value that is still unique and complex.

# Why a journal is not the worst idea

The three factors mentioned above are not always followed with passwords. Users will repeat the same password everywhere because they have memorized it, or use passwords that are 70% the same value which is easier for hackers to brute force when trying to pivot. Other times, sites will have tecnical limitations and not require the best standards when creating a password and allow weak ones to be created.

In order to make sure passwords are the highest quality, I would rather see users keep a hand written journal to track long, unqiue passwords instead of having users repeatedly use a few strong passwords. A journal provides a way to remember long complex passwords without having to memorize them. Comparing various entries provide a way to confirm entries are unique, even when updating passwords from previous entries.

The only downside to physical journals is the need to secure them. Journals should not be left unattended at work, or out in the open at home. By placing all account passwords in a single place creates a high value target to burglars or insider threats such as unhappy coworkers.

# Benefits of digital journals

This is where digital password managers can shine. On top of the benefits that physical journals have, password managers provide additional features that allow for a better user experience. Firstly, digital managers solve the physical security issue of requiring to lock away the journal. With encryption techniques today, your collections of passwords can be encrypted and secured before being sent online for storage which ensures only you will be able to decrypt the actual password values.

Online managers provide redundancy, availabilty, and easier usage than a journal. Imagine spilling coffee on a password journal or suffering a house fire. Storing collections online allows providers to replicate them across their servers and in case of emergencies or losing your local devices, the passwords can be easily recovered by downloading the collections to a new device and decrypting them.
Digital journals are easier to update and sort as well, providers have different ways to sort passwords or group them with custom tags that allow users to quickly find a certain password. Different providers also have means to automate certain features such as checking for password complexing, reuse, or tracking last edit or creation dates to suggest updates.

# My experiece with 1Password

My parents originally used physical journals for passwords, which worked well for them. Growing up, I gained more and more responsibility to managing the house IT and eventually it was not uncommon for me to be reading from the password journal. As I started my career as a student developer, I was introducted to local digital password managers which was slightly easier to use than journals and were definitely more secure. While I adopted these applications, it was not user friendly enough where the benefits outweighed the costs for my parents.

At a previous employer, I was given access to [1Password](https://1password.com/) which was much better than a local manager. Being able to encrypt and store credential online provided for collections to be shared between devices and integrated into login forms on applications and websites. 1Password's automation won me over and I started using it for my personal accounts.

The pricing made it worthwhile to offer to my family, and after some convincing, they have replaced their physical journals with the online collections. An additional feature that is great about 1password is being able to share collections with other coworkers or family memebers. This is great for shared credentials like WiFi passwords or joint accounts. 1Password has worked so well for my family that they have suggested it to extended family as well, the multiple layers of referrals is proof enough of the value in online managers like 1Password.

# Conclusion

Credentials are a key part of accessing computers and applications. Passwords should be complex, unique, and rotated as needed. Physically writting passwords is more secure than compromising those requirements. Digital password managers provide better means to store, sort, and use secure passwords. I highly recommend you check out a password manager such as 1Password which my family, coworkers, and I have had great experience with so far.