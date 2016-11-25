---
layout: post
title:  "Using the mailvelope extension to send and receive encrypted mail"
date:   2016-11-22 00:00:00 +0000
categories: encryption mail
---

__What is Mailvelope, and why should I care?__

[Mailvelope][3] is an extension for [chrome][1] or [firefox][2] that allows you to send and receive encrypted emails with relative ease

__First off - you'll need key pair__

whilst not _strictly_ true - you only _really_ need the public keys of other people to _send_ encrypted emails - you do how ever need your private key if you want to read what you sent or read the replies back

we touched on [how to generate a key pair][7] in the previous blog so we'll not repeat it here - lets get started

__Installing the extension__

the first thing we need to do is install the [Chrome extension][4] or the [Firefox extension][5]
once the extension has downloaded and installed you should see a little padlock in the top right hand corner ![mailvelopelock]({{ site.url }}/assets/using_mailvelope/01-mailvelopelock.png)

__Importing *your* keys__

click this padlock and go to options

![options]({{ site.url }}/assets/using_mailvelope/02-options.png)

seeing as we currently have no keys we need to import some so go to set up then important

![importing a key]({{ site.url }}/assets/using_mailvelope/03-importing-a-key.png)


![displaying keys]({{ site.url }}/assets/using_mailvelope/04-importing-continued.png)

we can paste it in as text - this is handy later for all the public keys (pasting them in one after the other in that text box will import them all at once) - but for now we'll imagine we have a private key file called private.key

![selecting a file]({{ site.url }}/assets/using_mailvelope/05-selecting-file.png)
once you've found the private key

![importing successful]({{ site.url }}/assets/using_mailvelope/06-import-successful.png)

you should see something like this:

![displaying keys]({{ site.url }}/assets/using_mailvelope/07-displaying-keys.png)

now we've successfully imported our private key


we can start importing some public keys so we can send encrypted emails for this example we'll use the public key from [keybase][6]

we then take the text from the link and post it into the text box in the importing screen shown in the screen shot above and click import
![a public key]({{ site.url }}/assets/using_mailvelope/08-importing-a-pub-key.png)
once that's done - go to display keys


![displaying pub key]({{ site.url }}/assets/using_mailvelope/09-public-key.png)
notice how there is only one key and it's angled to one side - this is indicating that we only have the public key for this pair meaning although we can *encrypt* messages to that key owner we cant decrypt them

__Sending an encrypted email__

Open your favorite email client (gmail, hotmail, yahoo mail, etc.) you'll see the little pad with a pencil pointing on it:

![composing]({{ site.url }}/assets/using_mailvelope/10-write-email.png)

a pop up window will appear, (1) select recipient's public keys and then (2) write your message - if you don't want it to be encrypted you can just click sign (3) and it will export the text back into the email browser window but it will be verified that what ever you typed in the compose email window (below) - if someone were to change the contents before it got to the intended recipient this signature would show as invalid (a bit like the way a wax seal on an envelope was at one time used to signify tampering)

![displaying pub key]({{ site.url }}/assets/using_mailvelope/11-plaintext.png)

once you click encrypt you'll be back in your email window but now will have an encrypted message ready to send to who ever you want to email privately
![displaying pub key]({{ site.url }}/assets/using_mailvelope/12-encrypted-message.png)

on the receipt of the email if the user is also using mailvelope they will see the following:
![displaying pub key]({{ site.url }}/assets/using_mailvelope/13-received-email.png)

clicking on the envelope will show the following prompt

![displaying pub key]({{ site.url }}/assets/using_mailvelope/14-decrypting-with-privkey.png)

depending on if you have the correct password and or correct private key you'll either see something like this

![displaying pub key]({{ site.url }}/assets/using_mailvelope/15-decrypted-message.png)

or this

![displaying without pub key]({{ site.url }}/assets/using_mailvelope/16-decrypted-without-key.png)

I hope this has been useful,
Phyu


   [1]: https://google.com/chrome "chrome"
   [2]: https://www.mozilla.org/en-GB/firefox/new/ "Firefox download page"
   [3]: https://www.mailvelope.com "mailvelope"
   [4]: https://chrome.google.com/webstore/search/mailvelope?hl=en "chrome extension"
   [5]: https://download.mailvelope.com/releases/latest/mailvelope.firefox.xpi "firefox extension"
   [6]: https://keybase.io/hackspacer123/key.asc "test pub key"
   [7]: ({{ site.url }}/_posts/2016-11-22-using-keybase.markdown)
