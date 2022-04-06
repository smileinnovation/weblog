---
layout: post
url: https://medium.com/@/4adf0f7080be
title: Smile FOSDEM 2020 report
subtitle: The major free software and open-source event in Brussels was, as usual, full of interesting presentations and folks from all corners of…
slug: smile-fosdem-2020-report
description:
tags:
- fosdem
- embedded-systems
- internet
- open-source
author: mafli
---

The primary free software and open-source event in Brussels was, as usual, full of exciting presentations and folks from all corners of the computing world.

![](/assets/images/posts/1*_kG7mUZZw-mKLDdz6F3Mdw.png)

The FOSDEM celebrated its twentieth birthday this year, and whoever is familiar with the event can say without hesitation that it is still getting stronger.

Like each year, Smile was there both to snatch cool presentations and provide some content ourselves. Pierre Ficheux talked in-depth about the tooling in the Yocto Project, with some practical examples. At the same time, David Garriou gave some advice on how to develop software for transiently-powered embedded systems.

As you can guess, this year, Smile’s Embedded and Connected Systems team was very present, so expect me mentioning a lot of subjects touching on Embedded, IoT, the Internet, and a touch of Security.

Some warning: as of the time of this writing, few video records of the talks are online on FOSDEM’s website or their YouTube channel. If the video you want is not there yet, check again in a few days.

https://www.linkedin.com/feed/update/urn:li:activity:6630466806836412417

# Embedded Solutions to Embedded Problems

This year marked the return of the “Embedded, Mobile and Automotive” devroom, which was sadly absent during the edition of 2019. This time, we heard about robots, VoIP, video manipulation, and tooling.

### Building Homebridge with the Yocto Project

[Homebridge](https://homebridge.io/) is a lightweight emulation of Apple’s Homekit server, which is the thing used to plug new APIs to Siri, the voice assistant.

Being able to use this means having complete control over a Linux distribution in whatever board you’re using, so why not integrate Homebridge as a Yocto package and upstream that? That’s precisely the point of this presentation, which provides yet another example of how easy Yocto building blocks are reusable, even for techies not to inclined into playing with Linux distributions.

https://fosdem.org/2020/schedule/event/ema_homebridge_with_yocto/

### ROS2: The evolution of Robot Operative System

The first thing to understand about the Robot Operating System is that it is not an Operating System. Instead, it’s a very diverse set of GUI tools and frameworks made to ease the way between your dream robot idea and its realization.

I wasn’t familiar with this project and was amazed by the richness of it. Illustrating design choices with real issues and why some things were to be rewritten from ROS1, the talk keeps it pragmatic. Now that even NASA is starting to use it, it’s no wonder the contributors to ROS2 are living exciting times.

https://fosdem.org/2020/schedule/event/ema_ros2_evolution/

(Also check out this other presentation, from other contributors, on fast IPC for robots and autonomous vehicles: [https://fosdem.org/2020/schedule/event/ema_iceoryx/](https://fosdem.org/2020/schedule/event/ema_iceoryx/))

### PipeWire in the Automotive Industry

PipeWire was already present at the latest Embedded Recipes conference in Paris, last November, and here it was again, to show its recent adoption by Automotive Grade Linux. AGL is a modern distribution trying to compete with Android Auto, which, while perfectly functional, still imposes massive efforts on any team wanting to fit it in their car.

PipeWire wanted to be “the PulseAudio for Video”, and ended up being a generic framework for media content sourcing/sinking. For automotive, it’s essential to be able to multiplex several sounds (and sometimes even video) flows, which previous open-source tools for Linux did not allow for.

An excellent presentation, heavily oriented towards design issues, and where previous solutions were lacking. The project is very active, allowing the talk to open up to some future improvements at the end.

https://fosdem.org/2020/schedule/event/ema_pipewire/

# Internet & Networking

### HTTP/3 for everyone

Daniel Stenberg, the author of curl, came to speak about HTTP/3 and explain, very broadly, what its impact is going to be on Internet services.

If you’re still wondering why HTTP/2 was not enough and what this weird QUIC protocol you keep hearing about is for, this is the presentation for you! Daniel keeps it lively and straightforward, offering free programs and use-cases examples.

https://fosdem.org/2020/schedule/event/http3/

### SCION: Future Internet that you can use today

It’s one of those half-technical, half-political talks about why the Internet is broken by design. But this one strives to keep it pragmatic, and even provides a live demo to deliver its point.

The subject is how packet routing can lead your Internet traffic to pass by some areas of the world, like China or Pakistan, which you may sensibly want to avoid if you could. It turns out, with current means (i.e., the BGP protocol & friends), you cannot; your Internet Service Provider is free to route however it wants.

This opinionated talk depicts SCION, a partial redesign of the Internet’s most large-scale protocols while trying to remain compatible with existing routing logic. A vast subject, that a single hour cannot aim to cover, but the presentation remains quite educational.

https://fosdem.org/2020/schedule/event/scion/

# Security

### Improving protection against speculative execution side-channel

A senior security techie at Intel’s came to FOSDEM to deliver a very informative talk about a subject that recently shone some light on the dangers of modern hardware trickeries: speculative execution in processors.

Mr. Stewart covers both the technical basics needed to understand what can go wrong (prepare for some low-level stuff), and what you can do about it (or not) if you’re a developer.

Good news: thanks to his and other folks’ work, Linux users don’t have to disable hyperthreading anymore, to stay away from this kind of nastiness!

https://fosdem.org/2020/schedule/event/speculative_execution/

### SaBRE: Load-time selective binary rewriting

Okay, that might be the nerdiest conference title mentioned in this article. It just so happens it is also very technical. :)

In short, we want to be able to add, remove, or replace some binary code in an existing program without rebuilding it. While the subject is not new, the tool, SaBRE, aims at providing some nice toolbox where previously skill, patience, and one’s crafting existed.

SaBRE reminds us very clearly of Strace in some ways, and it’s no surprise: the speaker often compares it to the well-known syscall tracer, especially on how their filtering abilities differ. SaBre allows “whitelisting” some syscalls, while Strace has to examine them all; allowing for a very tangible speed gain for some use cases, explained at the end of the session.

https://fosdem.org/2020/schedule/event/sabre/

This Medium post was co-authored by [Geoffrey Le Gourriérec]()

# That’s all, folks!

Did you enjoy it? If so, don’t hesitate to 👏 our article or [subscribe to our Innovation watch newsletter!](https://mailchi.mp/c414f1508567/techwatch) You can follow Smile on [Facebook](https://www.facebook.com/smileopensource), [Twitter](https://www.twitter.com/GroupeSmile) & [Youtube.](http://www.youtube.com/user/SmileOpenSource)


