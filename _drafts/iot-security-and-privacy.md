---
layout: "post"
title: "IoT Security and Privacy"
date: "2017-02-26 00:43"
comments: true
categories:
  - IoT
  - Security
  - Privacy
---

The current state of the consumer IoT market weirds me out - and it seems that I'm in the minority.

## Privacy

My main problem with the current market is the reliance on the cloud, and while that can cause issues when a provider decides [not to support your devices any more](https://www.wired.com/2016/04/nests-hub-shutdown-proves-youre-crazy-buy-internet-things/), I think the invasion of privacy is worse. Google Home and Amazon Echo are the most convenient examples of this. Both require the use of their respective cloud services to do anything useful, which involves a microphone which is always on sending your voice to an outside party. Yes, we're told that the device doesn't do anything until 'woken up' by it's keyword - but do you trust that this will always be the case? Or even trust that Amazon or Google actually do what they say they are doing? Maybe you do, which is fine - but do you trust that these services they provide you will always be free?

It's not just Google and Amazon either - [SmartThings](https://www.smartthings.com/), [IFTTT](https://ifttt.com/), and [Nest](https://nest.com/) all require cloud services, just to name a few. In the end it boils down to this: in the corporate world nothing is free - if you're getting something for free, they're making money from your data.  
But this is the norm now, and it seems like going the other way - having a completely on-premises home automation setup - is not well documented. I recently bought a ReSpeaker which claims to support offline voice recognition, but it turns out that's basically [complete bullshit](https://community.openhab.org/t/voice-control-and-seeed-respeaker/13541/4) ([my request](https://github.com/respeaker/respeaker_python_library/issues/6) asking if it really can do 'speech recognition' and not just 'keyword recognition' has fallen on deaf ears).


## Security

I'm not going to try to convince you that IoT devices shouldn't be using default passwords and the other security 101 bulletpoints because if you're reading this you should already know. [This is a good post](https://mjg59.dreamwidth.org/45483.html) on why you need to think about security in your home if you want it.

I started writing this section about how the problem is usually not with the cloud side of things, but with the device itself. But now I'm not sure, I'm also not sure making the distinction matters, and I'm mad. Lights that can be [controlled worldwide by anyone](https://mjg59.dreamwidth.org/40397.html) willing to look at the protocol, teddy bears that [store your children's voice in public](https://arstechnica.com/security/2017/02/creepy-iot-teddy-bear-leaks-2-million-parents-and-kids-voice-messages/) databases, and [more bulbs that give away your phone details](https://mjg59.dreamwidth.org/43722.html) have made me thoroughly disgusted in humanity and those are just examples I remembered reading off the top of my head. The security problems in the 3 devices listed are a mixture of device, protocol, and cloud service - but then the end result is always bad things happening over the internet.

The media typically jumps on a story like the Yahoo hack for a day or two and then everyone kind of goes back to what they were doing. Apparently the [largest DDos in history](https://www.theguardian.com/technology/2016/oct/26/ddos-attack-dyn-mirai-botnet) was recently carried out with a bunch of IoT devices - mostly ones using default credentials - and I'm not sure it even made the TV news here in New Zealand.

I can see two ways this gets better:

- The whole world gets much more serious about internet security (everyone includes your mother, and my mother =\\ ), not buying from companies that have a poor reputation.
- We take charge of our own devices and take responsibility for our own security.

As much as it makes me sad to say it, I can't see the first point happening. Most people just don't care. I have people on my street with open wifi. In 2017.

The second option is that I build walls around my own stuff, and keep it to myself. The one great thing I've found for this is [Home Assistant](https://home-assistant.io/) - it's a home automation server you host *inside your house* (like, the same building where your other devices are). Then devices don't need to be connected to the internet. There is either no point of connection to the internet, or only a single point if you want to use it from afar, which is much less attack surface. There are also options like VLANs to protect your devices from people on your local network. Plus, this solves the privacy problem since you're in charge of your own data.

This does unfortunately rule out Google Home, Amazon Echo, Nest, Hue, SmartThings, and almost every other big player in the home automation game. But **I just don't understand why my lightbulbs need to talk to the internet** to tell other devices they're on. Why does my toaster need to talk to the internet to have an app, if they're on the same network (unless you want to cook toast while you're out. Which no, you don't)? And I'm still looking for a voice assistant that works offline.

But no one builds IoT devices like this, so fuck 'em, I'll make my own.
