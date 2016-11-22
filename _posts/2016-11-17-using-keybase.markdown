---
layout: post
title:  "Generating a private/public key pair using Keybase.io"
date:   2016-11-18 10:00:00 +0000
categories: encryption
---

__What is a key pair?__

You've probably heard of encryption - depending on where you've heard about it you might think it's something that only "bad guys" use - in actual fact encryption is everywhere, it protects your sensitive information when shopping online - any website where you see that little green padlock in the browser (a website that uses HTTPS) uses encryption - there's a whole host of things that encryption can be used for!
In this post however we're only really interested in secure* email communication, when I say secure I mean to a reasonable standard - but it should be noted that encryption doesn't make the messages invisible just *VERY* hard to read for the average adversary

Coding messages have been around since the roman times - look at the Caeser cipher for an example of one of the earliest methods of securing communications
but anyway back to keys (it should also be noted that this post is designed to be a brief introduction to using public/private key cryptography, not an exhaustive account of how it works) public key cryptography (also known as asymmetric key cryptography) is particularly useful as it allows the user to distribute their public key to anyone they wish to communicate privately with but only _they_ can decrypt messages that are encrypted with the public key - so anyone and everyone can use the users public key (well call it pub1) to encrypt a message where the intended recipient has the complimentary private key (pk1)

for example [Alice and Bob][1] want to communicate but don't want Charles to be able to eavesdrop
so they devise a method to communicate securely:
they have a box with two lock hasps on Alice writes a message on a piece of paper in the box and locks her pad lock on it and sends it to bob
he then puts his pad lock on and sends it back to Alice who finally unlocks it and sends it back - when bob receives it this time he unlocks his lock and then can read the note - the processes is repeated for the communication
 note: this isn't EXACTLY how it works but it helps visualise the basic process involved and the idea that only Alice can unlock Alice's key and bob can only unlock bob's key

So, lets get on with it goto the [Keybase website][2]
and create an account
![creating new keybase user]({{ site.url }}/assets/generating a key using keybase\01-joiningkeybase.png)

![adding pgp key]({{ site.url }}/assets/generating a key using keybase\02-keybaseaddpgpkey.png)

![need a pgp key]({{ site.url }}/assets/generating a key using keybase\03-addakey.png)

![generating pgp key]({{ site.url }}/assets/generating a key using keybase\04-generating.png)

![generating pgp key cont.]({{ site.url }}/assets/generating a key using keybase\05-generating2.png)

![finished generating pgp key]({{ site.url }}/assets/generating a key using keybase\06-generating3.png)

![generating pgp key]({{ site.url }}/assets/generating a key using keybase\07-editkey.png)

![generating pgp key]({{ site.url }}/assets/generating a key using keybase\08-public key.png)


   [1]: https://en.wikipedia.org/wiki/Alice_and_Bob "Alice and Bob"
   [2]: https://keybase.io "Keybase"
