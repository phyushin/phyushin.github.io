---
layout: default
title:  "Getting Started With Mobile Testing - iOS"
date:   2025-09-11 00:00:00 +0000
categories: Mobile Testing iOS 
---

Hello everyone I've decided to kick the cobwebs off my blog and write something for the first time in... too long, but hey I've been pretty busy with life we all get that way sometimes I suppose; but this isn't a recipe for your granny's best tomato soup so lets get into it.

## Motivation
This blog series is intended for me to cement my iOS testing knowledge as well as put out some (hopefully) interesting content that isn't behind a damn medium paywall.


## Step One, iPhone
For my testing device I've gone with the iPhone 8+ (iOS 16.7.11) as it was what I had laying around after I upgraded and handily, it can still be jailbroken using the [palera1n][1] method unfortunately though I can't use trollstore to install the app. I can; however, use [Sideloadly][3] it's a simple process, connect the device up to your device - I'm using mac here but i've used windows too - drag the IPA over the the `IPA` box and then click start you'll it will ask you to log in and then drop the app on the device
![sideloadly.png]({{ site.url }}/assets/ios_mobile_app_testing_pt1/sideloadly.png)


## iGoat-Swift
OWASP have a nice handy vulnerable iOS application [iGoat-Swift][4] we're going to use for this series. Using Sideloadly, we push the iGoat app to the the device. Next we need to push the frida-server binary up to device too.

Note: You'll need to jailbreak your device in order to _properly_ run the frida server.
Additional note: There's currently an issue running Objection with the latest version of frida - I've found [version 15.6.2][2] works well the following script should get you started

```bash
#!/bin/bash

export IPHONE_IP="192.168.0.101"
## create mobile testing Venv
python3 -m venv ~/.mobile_testing_venv
source ~/.mobile_testing_venv/bin/activate
pip3 install frida-tools==13.7.1 frida==16.7.19 objection
wget https://github.com/frida/frida/releases/download/16.5.2/frida_16.5.2_iphoneos-arm64.deb # grab 64-bit frida-server for iPhone

wget https://github.com/frida/frida/releases/download/16.5.2/frida_16.5.2_iphoneos-arm.deb   # 32-bit frida-server for iPhone

# push the frida_servers to the home directory of the "mobile" user which is the default for jailbroken iphones

scp frida_16.5.2_iphoneos-arm64.deb "mobile"@$IPHONE_IP:~/frida_server64.deb
scp frida_16.5.2_iphoneos-arm.deb "mobile"@$IPHONE_IP:~/frida_server.deb

echo "connecting to iPhone at ${IPHONE_IP}"
ssh mobile@$IPHONE_IP -L 27042:localhost:27042 ## ssh into the phone forwarding frida port

## issue these commands in the ssh session
# sudo su
# dpkg -i frida_server64.deb ## frida_server.deb for 32bit
```


Once this script has run it should have downloaded the relevant frida-server and pushed it up to the device.
Next we check that we can connect to the frida-server using the following command:
`frida-ps -Rai | grep -i "goat"`
This will list all running processes on the device (the -R flag is for remote host) and grep the results for anything with the word `goat` in:
![frida-ps.png]({{ site.url }}/assets/ios_mobile_app_testing_pt1/frida-ps.png)
Now we know the gadget we can launch objection with the `explore` option: 
![objection.png]({{ site.url }}/assets/ios_mobile_app_testing_pt1/objection.png)


Finally to round it off here, issue the `env` command in the REPL to display application environment information such as where the app is on the device and and folders used to store data:
![app-env.png]({{ site.url }}/assets/ios_mobile_app_testing_pt1/app-env.png)

Hopefully, This was useful

Phyu


[1]: https://palera.in/download/?tab=macos
[2]: https://github.com/frida/frida/releases?page=6
[3]: https://sideloadly.io/
[4]: https://github.com/OWASP/iGoat-Swift