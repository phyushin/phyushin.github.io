---
layout: post
title:  "Is this what I think it is - using GPG to verify a file's integrity"
date:   2017-02-28 00:00:00 +0000
categories: Encryption Checksums Cyber-Buzzwords
---
Is this what I think it is?
----
When downloading a file from the internet, it's always important to check that the file you've downloaded is the file you were actually intending to download , there are a number of reasons you'd want check :
- To stop [things like this][1] from compromising your system
- To make sure you're getting the file you intend to get - for example if you've downloaded software from a website you can check it's the latest version by comparing the relevant _checksums_
- It can even be used to check a file has completely downloaded

In Linux there is a tool installed by default called [GPG][2] or _GNU Privacy Guard_ which allows us to verify a files integrity by using two things:
- The file
- The detached signature (filename.sig)

Once you have both of these it's just a case of running the following command

`gpg --verify filename.sig filename`

Initially, this probably won't work as you'll see something like

![linux message after verifying]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/5.0_linux_message_after_verifying.png)

more info can be found about this in [the manual][3].

We're going to be installing [GPG4Win][5] so download the latest version for windows in this example its 2.3.3 - once it's downloaded open powershell by opening the run dialog box `Win+r` and typing `powershell`)
navigate to the downloads folder (where you downloaded GPG4Win to and then enter the following command)

`$(CertUtil -hashfile .\gpg4win-2.3.3.exe SHA1)[1] -replace " ",""`

![using certutil]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/3.0_using_certutil.png)

This will give you the SHA1 check sum of the file verify that it matches(1)

![checksum]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/2.0_checksum_and_detached_sig.png)

Once verified we can install it - be sure to tick the GPA box as this will make things easier later

![make sure GPA is enabled]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/4.0_make_sure_to_enable_GPA.png)


Once the installation has finished open GPA - we need to generate a key pair or import one if we followed the [earlier post][4] about setting one up using [Keybase][6] - as that post mentions you need to keep your private key VERY safe ... don't share it with anyone - not even your _bestest friend_ in the world... my recommendation would be to put it on an encrypted USB drive somewhere safe.

But any way once you've imported a private key you should see something like this

![key manager]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/7.0_keymanager.png)

So lets go verify something - how about [veracrypt][7] then we can create that encrypted USB drive later

Download the latest Veracrypt setup and its corresponding PGP signature

![download]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/8.0_veracypt_and_detached_sig.png)

right click the exe file and you should see some GPG options - select `verify`

![verifying]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/9.0_verifying.png)

But oh no! we get the same message we did when we tried to verify in Linux - that being that the key is unknown

![unknown key]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/10.0_unknown_key.png)

No worries though! we installed GPA so it's really easy to add this

![receiving]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/11,0_receiving_keys.png)

Enter the Key ID in this case `54DDD393` you should then get a prompt saying this key has been imported and it will show up in GPA

![importing]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/12,0_new_key_imported.png)

Now that the key is imported we need to tell GPA that we trust it - we do this by signing it with our key right click the imported key and go to `sign keys...`

![signing key]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/12.1_signing_key.png)

You will then be prompted for your key password once you enter that you're good to go - go back to the exe file and right click and verify again you should see something like this:

![signing key]({{ site.url }}/assets/checking_file_checksums_to_verify_integrity/13.0_valid.png)


A few caveats though:
- Don't just sign keys, only sign it if you trust it
- Encryption might be illegal where you live, don't break the law
- This is only intended to be a guide to *hopefully* make file signature checking less intimidating


I hope this has been useful,

Phyu

   [1]: http://blog.linuxmint.com/?p=2994 "Linux Mint blog"
   [2]: https://www.gnupg.org "Gnu Privacy Guard"
   [3]: https://www.gnupg.org/gph/en/manual/x135.html "GPG manual"
   [4]: http://blog.phyubox.com/encryption/keys/2016/11/22/using-keybase.html "Using Keybase"
   [5]: https://gpg4win.org/ "GPG4Win"
   [6]: https://keybase.io "KeyBase"
   [7]: https://veracrypt.codeplex.com/releases/view/629329 "veracrypt"
   [8]: http://www.cryptolaw.org/cls-sum.htm
