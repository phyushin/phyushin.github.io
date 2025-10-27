---
layout: default
title:  "Generating a private/public key pair using Keybase.io"
date:   2016-11-22 10:00:00 +0000
categories: Encryption Keys
---

__What is a key pair?__

You've probably heard of encryption - depending on where you've heard about it you might think it's something that only "bad guys" use - in actual fact encryption is everywhere, it protects your sensitive information when shopping online - any website where you see that little green padlock in the browser (a website that uses HTTPS) uses encryption - there's a whole host of things that encryption can be used for!
In this post however we're only really interested in secure* email communication, when I say secure I mean to a reasonable standard - but it should be noted that encryption doesn't make the messages invisible just *VERY* hard to read for the average adversary

__Cryptography, a brief introduction__

Coding messages have been around since the roman times - look at the Caeser cipher for an example of one of the earliest methods of securing communications
but anyway back to keys (it should also be noted that this post is designed to be a brief introduction to using public/private key cryptography, not an exhaustive account of how it works) public key cryptography (also known as asymmetric key cryptography) is particularly useful as it allows the user to distribute their public key to anyone they wish to communicate privately with but only _they_ can decrypt messages that are encrypted with the public key - so anyone and everyone can use the users public key (well call it pub1) to encrypt a message where the intended recipient has the complimentary private key (pk1)

For example [Alice and Bob][1] want to communicate but don't want Charles to be able to eavesdrop
so they devise a method to communicate securely:
they have a box with two lock hasps on Alice writes a message on a piece of paper in the box and locks her pad lock on it and sends it to Bob
he then puts his pad lock on and sends it back to Alice who finally unlocks it and sends it back - when Bob receives it this time he unlocks his lock and then can read the note - the processes is repeated for the communication
 note: this isn't EXACTLY how it works but it helps visualise the basic process involved and the idea that only Alice can unlock Alice's key and Bob can only unlock Bob's key (this is the basics of how key exchange works)

So, lets get on with it go to the [Keybase website][2]
and create an account:
![creating new keybase user]({{ site.url }}/assets/generating_a_key_using_keybase/01-joiningkeybase.png)

once you've picked a username, put your email address in and choose a *secure* password *one you haven't used elsewhere*

Once we've logged in we will see something like this:
![adding pgp key]({{ site.url }}/assets/generating_a_key_using_keybase/02-keybaseaddpgpkey.png)

*but what is this PGP key?* well, it's out asymmetric key pair we mentioned earlier - click add a PGP key as shown in the screen shot - as this post is titled _Generating a private/public key pair using Keybase.io__ that's what we're going to do

![need a pgp key]({{ site.url }}/assets/generating_a_key_using_keybase/03-addakey.png)

we're going to select I "need a public key" which will then greet us with the following screen:

![generating pgp key]({{ site.url }}/assets/generating_a_key_using_keybase/04-generating.png)

fill in the details accordingly and then put your keybase password in and away it goes:
![generating pgp key cont.]({{ site.url }}/assets/generating_a_key_using_keybase/05-generating2.png)

once it's done it will show you the public key it's generated in the text box, it's important for us at the moment to make sure the "Host encrypted private key, too" option is checked as it will allow us to encrypt and decrypt using the website (for the particularly paranoid you can export the key and delete it later (I will show you at further down how this is done ))
![finished generating pgp key]({{ site.url }}/assets/generating_a_key_using_keybase/06-generating3.png)

Now that the keys have been generated you'll something similar to the screen shot below
![generating pgp key]({{ site.url }}/assets/generating_a_key_using_keybase/07-editkey.png)

if you want to share your public key with people who don't have keybase you can click the edit button and give them the link below:
![generating pgp key]({{ site.url }}/assets/generating_a_key_using_keybase/08-public key.png)

__exporting private key__

**WARNING - Here be dragons!**

do *NOT* share this file with anyone - also keep it safe, if anyone has the private key they can encrypt messages as you so - only do this if you want to use it else where (like pgp locally or [mailvelope][3] etc.) click edit and select export private key

![exporting]({{ site.url }}/assets/generating_a_key_using_keybase/09-exportingprivatekey.png)

this will generate the private key into the text box:

![exporting]({{ site.url }}/assets/generating_a_key_using_keybase/10-exported.png)
keep this somewhere safe and as mentioned above don't share it

I hope this has been useful,
Phyu

   [1]: https://en.wikipedia.org/wiki/Alice_and_Bob "Alice and Bob"
   [2]: https://keybase.io "Keybase"
   [3]: https://www.mailvelope.com "mailvelope"
