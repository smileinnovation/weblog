---
layout: post
url: https://medium.com/@/636812f45b9d
title: "Magic Leap: Getting started on Unreal Engine"
subtitle: In this article I will be explaining you how to install and setup your computer in order to work with the Magic Leap SDK.
slug: magic-leap-getting-started-on-unreal-engine
description: In this article I will be explaining you how to install and setup your computer in order to work with the Magic Leap SDK.
tags:
- unreal-engine
- magic-leap
- augmented-reality
- setup
- development
author: segar
---

![](/assets/images/posts/1*yt2zeFdbx0OKrvwYormYow.jpg)

In this article, I will be demonstrating how to install and set up your computer itowork with the Magic Leap SDK.

# Magic Leap SDK

First, you will need to download the Magic Leap Package Manager.

You can [download it here](https://creator.magicleap.com/downloads) and sign in. It’s available both on Windows or Mac, but there‘s no Linux version available at this time of writing. Sorry 🤷‍♂️

Once downloaded, install the package manager and sign in (again) with your Magic Leap credentials. You will then be greeted with the following page.

![Magic Leap Package Manager](/assets/images/posts/1*N7KUVjPJQAMFXb7OgkWLRg.png)

Depending on which 3D engine you want to work with, you will need to know the SDK version that is compatible with the Lumin OS of your Magic Leap and your 3D engine. Check the table below:

![](/assets/images/posts/1*vA9Q3GiY-xwXEZ-Muu_wOQ.png)

To work with Unreal Engine and Lumin you will need to download the following packages:

> ❗️ Be careful not to install the SDK in a path with a white space in it or else the packaging process in Unreal Engine will fail. Also, if your username has a space in it then install the package in an other hard drive or create a new user just for it.

### Common Packages:

* C API Samples

* Lumin SDK

* Visual Studio 2017 Extension

### Unreal Packages:

* Unreal Soundfield Audio

* Unreal API Documentation

* Unreal Examples

* Visual Studio 2015 Extension

# Unreal Engine

You can now either download the Unreal Engine standalone, the Epic Game Store compiled version with the Lumin package already installed in it or compile the source engine with the Lumin SDK in it yourself.

![](/assets/images/posts/1*pewwC34p6UQK5kYMyEX5cA.png)

If you download the standalone version of the Unreal Engine make sure to select the Lumin package in the options as shown below.

![](/assets/images/posts/1*LtB0QQYHlvDa3Z0NJxnliw.png)

Make sure you also select the **Android** package as it is **required** by Lumin.

# Project Setup

Now you are ready to set up your project!

Open the Epic Games Launcher and log into your account. Launch Unreal Engine and start a new empty project.

Before jumping into anything, you should first set up your project settings:

![](/assets/images/posts/1*f9Pi6YTC5CCpRnStt4Y78Q.png)

In the *project settings*** **window, on your left, scroll down the menu and click on `Platforms > Magic Leap`** **then in `Magic Leap App Title`, click on `Configure Now`.
On the same page in Distribution Signing put your developer certificate (you can get one on the magic leap website, log in and go to `Publish > Certificates`).
Back on the left menu and select `Platforms > Magic Leap SDK` and enter the path where you installed the Magic Leap SDK.
And finally, on the left menu select `Plugins > Magic Leap Plugin` and enable Zero Iteration.

# Magic Leap Remote

Don’t have your Magic Leap with you? No worries, the *Magic leap Remote* allows you to simulate the headset with its remote controller and the all the required environment.
It is automatically installed with the Lumin SDK, you can either launch it via the Magic Leap package manager or directly from the folder where you installed the SDK (`<your path>\mlsdk\<your version>\VirtualDevice`).

Once you launched the app you will be greeted with the following window:

![](/assets/images/posts/1*UekYvBNSGQskDezCSYRZrg.png)

Click on `Start Simulator` and a new window will open with an empty map. On the top left corner of the window, click `Load a Virtual Room`, select a map and that’s it! You are ready to use it on the Unreal Engine.

To use this simulator on the Unreal Engine you will need to have done all the previous setup, then go to the drop-down menu of `Play `and select `VR Preview`.

Once you followed their tutorial for [Hello_Cube](https://creator.magicleap.com/learn/guides/hello-cube-unreal-engine) and click `VR Preview` you will be able to see the cube in your virtual world.

> ❗️ If you have a black screen on the simulator, try updating your graphics drivers or reinstall the SDK

There are some limitations on what you can do with the simulator, for example, you won’t be able to test the hand meshing feature or the image tracking feature. But for quick 3d rendering and testing, it is quite useful.

>> That’s it, you are ready to work with the Lumin SDK and Unreal Engine!

![](/assets/images/posts/1*Wn5sW-E8k6E1JZ4-OAPXVA.png)

Did you enjoy it? If so don’t hesitate to 👏 our article or [subscribe to our Innovation watch newsletter!](https://mailchi.mp/c414f1508567/techwatch) You can follow Smile on [Facebook](https://www.facebook.com/smileopensource), [Twitter ](https://www.twitter.com/GroupeSmile)& [Youtube](http://www.youtube.com/user/SmileOpenSource).


