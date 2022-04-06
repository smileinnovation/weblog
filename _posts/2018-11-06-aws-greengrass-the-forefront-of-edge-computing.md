---
layout: post
url: https://medium.com/@/8ec2098a33b7
title: "AWS Greengrass the forefront of edge computing"
subtitle: |-
    This articles main goal is to resume briefly what AWS Greengrass is. If you want a more thorough presentation you can find it on the AWS…
slug: aws-greengrass-the-forefront-of-edge-computing
description: |-
    This articles main goal is to resume briefly what AWS Greengrass is. If you want a more thorough presentation you can find it on the AWS …
tags:
- aws
- greengrass
- machine-learning
- lambda
- embedded-systems
author: segar
---

![Credits: Amazon](/assets/images/posts/1*7LaVsLtTgUnkcuRblDFKkg.png)

This articles main goal is to resume briefly what AWS Greengrass is. If you want a more thorough presentation you can find it on the AWS [Greengrass documentation page](https://docs.aws.amazon.com/greengrass/latest/developerguide/what-is-gg.html)

# IoT

First, we need to understand what is the Internet of Things before we can talk about the *AWS Greengrass* solution from Amazon.

![Internet of Things](/assets/images/posts/1*cIyH9nTwK5NU0c7HhbjbGA.png)

The IoT, in brief, is a term used to identify a network of connected devices called Things that are connected to a cloud server (your fridge could be a Thing). From this server, you can monitor the data sent by the Things in real time and take actions immediately if needed. It is a great tool to improve our day to day life. But not everything is perfect and there are some flaws.

One of the flaws is connectivity, your Things must always be connected to the cloud in order for you to collect data and control your objects. If you lose the connection between your cloud and your object, no data will be saved and/or collected and you will lose all control over your network. This is the main problem AWS Greengrass wants to solve.

The other flaw is security, nowadays, is the most important part of any project. Are the communications secured? Is the hardware safe and hasn’t been tampered with? AWS Greengrass also tries to maximize security with its solution.

# Edge computing

Edge computing is a distributed computing paradigm, meaning that most of the operations are done on a device instead of a server in the cloud. The word edge comes from its position in the network, it’s on the “edge” of the enterprise or network to the cloud. Its goal is to bring data collection, analysis, artificial intelligence and server resources closer to Things. It’s also considered as environmentally friendlier because it would use less network electricity and thus, less cooling is needed. AWS Greengrass is part of the edge computing.

# AWS Greengrass

Before we start talking about Greengrass, at the time of writing this article, Greengrass was at version 1.7. This service evolves rapidly so some information might not be up to date.

> AWS Greengrass is a software that extends AWS Cloud capabilities to local devices, making it possible for those devices to collect and analyze data closer to the source of information, while also securely communicating with each other on local networks.

AWS Greengrass provides the core software that you can install on your device ([list of compatible devices](https://aws.amazon.com/greengrass/faqs/#Greengrass_Core_Platform_Compatibility)), an SDK and an API.

This service offers several features such as :

* A lambda runtime that allows you to execute serverless instructions if needed.

* A shadow implementation, this means that a Thing has a `JSON` file (the shadow) where all of its parameters/variables are set and can be modified with lambda functions from the core or from the cloud.

* A message manager, for instance, when the core needs to restart, the Things can still send messages and the core will save them until the core has restarted again.

* A group management, a group is comprised of the Things and the Greengrass Core.

* A discovery service, it is a service mainly used by the Things to get certificates to connect to a Greengrass Core.

* An Over-the-air update agent that allows updating one or more Greengrass Cores on a network at the same time or on predefined schedules. The devices with the Greengrass core will need to have the WiFi activated for this feature to work.

* Local resources access on the Greengrass Core device if it is needed, the resources can be anything.

* A machine learning inference, meaning that the training is done on the cloud servers but the model (the “brain” of the AI) is on the Greengrass device and can perform in real time what it was trained to do. We have made a use case that uses machine learning inference, you can find out more at the bottom of this article.

* Since version 1.7, Connectors help you implement reusable business logic, interact with cloud and local services (including AWS and third-party services), ingest and process device data, enable device-to-device calls using MQTT topic subscriptions and user-defined Lambda functions. This module works as packages that you can deploy on your core without the hassle of learning new APIs or protocols. It’s meant to make your life easier.

As the technology advances in time, more features will probably be added.

# Connectivity

![Credits: Amazon](/assets/images/posts/1*UYH-y5giSyQsZi2J0yK7fQ.png)

So, as we can see, Greengrass is a software that will enable you to perform, in a local network, basically the same process as if your Things were connected to a cloud but this time they are connected to a local device.

This solves the connectivity issue with the cloud servers. If your Greengrass device loses connection to the cloud, it won’t stop working. It still gather data from the Things and perform automated actions (that you made before deploying Greengrass, we will see that later on).

Once the Greengrass device reconnects to the cloud it can perform updates to its core and send you back the data it collected when it couldn’t reach the cloud.

# Security

![Credits: Amazon](/assets/images/posts/1*f_9hNqsTzPHQc4oLRetVWw.png)

> AWS Greengrass protects user data through the secure authentication and authorization of devices. Through secure connectivity in the local network. Between local devices and the cloud. Device security credentials function in a group until they are revoked, even if connectivity to the cloud is disrupted, so that the devices can continue to securely communicate locally.

A — Greengrass service role: A customer-created IAM role that allows AWS Greengrass access to your AWS IoT and Lambda resources.

B — Core device certificate: An X.509 certificate used to authenticate an AWS Greengrass core.

C — Device certificate: An X.509 certificate used to authenticate an AWS IoT device.

D — Group role: A role assumed by AWS Greengrass when calling into the cloud from a Lambda function on an AWS Greengrass core.

E — Group CA: A root CA certificate used by AWS Greengrass devices to validate the certificate presented by an AWS Greengrass core device during TLS mutual authentication.

If you want a more detailed information on how the security works, here’s a [link to the documentation page](https://docs.aws.amazon.com/greengrass/latest/developerguide/gg-sec.html).

# Competition

To this day (day of publishing this article) Amazon Greengrass is the only complete and easy to install solution. But there are other businesses that are working on edge computing services:

* [Microsoft Azure IoT Edge](https://azure.microsoft.com/en-us/services/iot-edge/) is currently working on the same service with similar features but with the core edge service open sourced.

* [Google Cloud IoT Edge](https://cloud.google.com/iot-edge/) is working on it too but you need to send a request in order to work with them as the project is still in alpha.

* [EdgeX Foundry](https://www.edgexfoundry.org/) is a Linux Foundation Project that created an open source edge computing core.

* Alibaba and Intel are working on a Joint Edge Computing Platform too.

# Use case: Capturing your dinner, a deep learning story II

This use case is in the continuity of our previous article written by our R&D engineer for Innovation dept. [Patrice Ferlet]().

https://medium.com/smileinnovation/capturing-your-dinner-a-deep-learning-story-bf8f8b65f26f

Our goal here is to use the Machine Learning inference feature that offers AWS Greengrass in order to detect if, in the image the camera captures, it can identify the food whilst not being connected to the cloud.

![Credits: Amazon](/assets/images/posts/1*-D3_7QlYYvbohJ6OhJ2qVg.png)

![Use case](/assets/images/posts/1*omhouHqQUDep8mCqs1t4yQ.png)

For our use case, we will be using the following services:

* *AWS IoT*

* *AWS Greengrass*

* *AWS SageMaker*

* *AWS S3*

And for the hardware:

* 1 *Raspberry PI 3 Model B V1.2 (2015)*

* *Raspberry Black Camera V2*.

![Setup](/assets/images/posts/1*uVg8aEpRBpc1OfpOx5kKgA.jpg)

Although the Raspberry Pi is a very powerful hardware its capabilities on machine learning inference isn’t great especially because it doesn’t have a dedicated GPU so everything has to go to the processor. If you want to do real-time image prediction we recommend to switch to a more powerful hardware such as a Nvidia Jetson TX2. But for our use case, the Raspberry Pi was enough.

We won’t be explaining here how to install and set up the services that we will use for our prototype as it would take too much time and it is not our goal, but here are the tutorials that will help you start and that helped us:

* [AWS Greengrass](https://docs.aws.amazon.com/greengrass/latest/developerguide/gg-gs.html): To learn how to create a group install a core, get the certificates, update lambdas etc…

* [AWS Machine Learning Inference](https://docs.aws.amazon.com/greengrass/latest/developerguide/ml-console.html): A tutorial that shows you how to install a machine learning inference in a few steps. Our use case is mainly based on this tutorial.

When installing the MXNet package from [this tutorial](https://docs.aws.amazon.com/greengrass/latest/developerguide/ml-console.html), if you use a Raspberry Pi like us, you will probably need to increase your swap memory (if your MicroSD card allows you to). For that, just do the following commands:

```
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
sudo /sbin/mkswap /var/swap.1
sudo chmod 600 /var/swap.1
sudo /sbin/swapon /var/swap.1
```

Once you finished the installation of the packages, you can remove the temporary swap memory by doing the following commands:

```
sudo swapoff /var/swap.1
sudo rm /var/swap.1
```

When you have installed and put the necessary certificates in the Raspberry Pi you can proceed by creating a Greengrass group.

Next step is to add a new Lambda

![Adding a Lambda](/assets/images/posts/1*xlCktxZgXkHO8yU6ueyHhQ.png)

Then create a Lambda and follow the steps that the previously mentioned AWS tutorial showed.

Make sure you have your MXNet model available on an S3 bucket. Just to warn you, at the time of writing this article, MXNet 1.2.1 is the only version compatible with Greengrass. So is Tensorflow version 1.4.

Don’t forget to set it to long-lived and enable read access to the`/sys` directory. Finally set the key `MXNET_ENGINE_TYPE` to `NaiveEngine`.

Then create the ressources `videoCoreInterface`, `videoCoreSharedMemory` and `Food_Model` so that in our Lambda instance we will have our model accessible.

![Resources](/assets/images/posts/1*6DH7aODXLVKn0tJ0sQxnwA.png)

In our subscriptions, we subscribed our IoT Cloud to our Lambda with the topic `gg/inference/food/trigger`, so that our core does a prediction only when we call this topic (on demand). And finally, the IoT Cloud to our Lambda with the topic `gg/inference/food`, so that our core goes in an infinite loop (long-lived) so it makes predictions as often as it can.

![Subscriptions](/assets/images/posts/1*3PrpzWlDJ2NpkrAHlq0o-Q.png)

Finally we deploy our data to our core and everything will run on its own.

To test if everything works, click on the test button at the bottom left-hand side of your screen. Subscribe to `gg/inference/food` and you should see your predictions.

![Predicts if it sees a burger/other/salad](/assets/images/posts/1*vmsBRpqiNwXZgcj8GOws5w.png)

I hope everything was clear. We just wanted to explain and have a little demo on what is Greengrass and the Machine learning inference capabilities. The prediction process takes about 1 to 1.2 seconds, but we are at this time looking into some hardware accelerators that would help improve the predictions speed.

Have you enjoyed it? If so don’t hesitate to 👏 our article or s[ubscribe to our Innovation watch newsletter ](https://www.getrevue.co/profile/smileinnovation)!
You can follow Smile on F[acebook,](https://www.facebook.com/smileopensource) T[witter ](https://www.twitter.com/GroupeSmile)& Y[outube.](http://www.youtube.com/user/SmileOpenSource)


