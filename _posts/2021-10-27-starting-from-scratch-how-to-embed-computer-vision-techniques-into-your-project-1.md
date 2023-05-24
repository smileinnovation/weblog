---
layout: post
canonical_url: https://medium.com/@/cb9354ee8cbe
title: Starting from scratch, how to embed computer vision techniques into your project
subtitle: 'A practical guide to answering this question: is this a screwdriver or
  a hammer?'
slug: starting-from-scratch-how-to-embed-computer-vision-techniques-into-your-project
description:
tags:
- artificial-intelligence
- machine-learning
- deep-learning
- computer-vision
- visual-search
author: alrou
image: assets/images/posts/1*wuEUDAkSJw0qIIAOK4qoiw.jpeg
---

---
A practical guide to answering this question: is this a screwdriver or a hammer?

[**Smile**](https://www.smile.eu/) is the proud editor of [**ElasticSuite**](https://elasticsuite.io/), a great **Magento** open-source extension, with **more than a million downloads** on Github and **trusted by more than 1500 top retailers worldwide**. It’s the leading solution for **intelligent search and merchandising** on Magento.

As part of this product road map, we have to test and experiment with new features.

# You need a use case

A trend in e-commerce search solutions is visual search, which offers a different search experience using real-life images instead of text inputs. There are numerous visual search usages, from fashion, home decor retailers to DIY retailers.

So it is obvious for the ElasticSuite team to experiment with a visual search feature. Our use case was targeting DIY retailers to filter search results, suggest similar products based on an image uploaded by the user.

This experimentation was also for us to understand the challenges related to such technologies and estimate, only starting from the idea, the required effort to deliver something ready for production.

We decided to go step by step, targeting a simple version of this feature, narrowing the visual search capabilities. And based on this first experience, we would decide what would be the next steps.

This first experience was focused on this simple use-case: recognize a set of common objects available in a DIY retailer from real-life images.

As a first target, we chose this object list: *screwdrivers, wheelbarrow, hammer, paintbrush, and driller*.

And according to what can be found in these images, we can shape a search result of a retailer’s e-commerce website.

This use case will rely on the latest computer vision techniques (more precisely, [object detection](https://en.wikipedia.org/wiki/Object_detection) and deep learning): we want software capable of identifying, in a real-life image, objects of interest, then reporting their presence and position in this image.

# Before anything else, what you need to know about computer vision

🤓 Here, I’m not going to explain the theoretical and practical of how computer vision techniques work. There is already a[ good amount of literature available ](https://towardsdatascience.com/search?q=computer%20vision)for you to read, and w[e have covered it already in the past.](https://medium.com/smileinnovation/tagged/machine-learning)

Instead, we will focus on the minimum required knowledge to use computer vision, especially deep learning techniques.

### Becoming a teacher

First, we need to agree that machine learning techniques are based on **learning**. This means we have to become a teacher with all the required skills to know what to explain and how to explain it. We also need to make sure that what has been learned is what we were expecting.

To teach a machine to recognize objects using deep learning, we then have :

* to know what we want to recognize

* to collect a broad set of examples of what to recognize

* to define tests of successful learning

These elements are crucial, and their outcome is the most valuable part of this entire project: they represent the **training data set**.

This training data set must be built and maintained to keep your machine training up to date and accurate. And this will be the first big step in this process.

### Finding your student

Following that, we will need a **machine learning model** trained with our training data set. Here we can get lost as machine learning models are numerous and difficult to understand.

But we will step back a bit to understand what’s going on in this area: competition. A machine learning model is based on recipes, mathematical recipes. And every model creator will try to optimize his recipe to get the best result on the task at hand. They usually compare each other using a common training data set (i.e., [Coco data set](https://cocodataset.org/#home) ). From this competition/research, we can focus on a top list of model recipes relevant for a specific task.

As someone who wants to use these technologies, we can benefit from an already well-known best-in-class model relevant to our needs.

However, there are two essential criteria to look at when choosing a model :

* capability to support what we call **transfer learning**

* software maturity and documentation

Transfer learning is an essential technique in machine learning that allows to reuse of an already trained model for a similar task but based on a more specialized training data set. This is a massive gain in time and simplifies a lot the usage of a complex model.

Software maturity and documentation are crucial to making sure you will easily use this model. Deep learning techniques are complex, and we do not want to become experts of the particular model we choose; we just want to use it.

In our case, we decided to choose a well-known model for [objects detection](https://machinelearningmastery.com/object-recognition-with-deep-learning/): [Yolo v5](https://github.com/ultralytics/yolov5) (“ in large version,” optimized for 640 pixels images)

Here is the algorithm performance claim from their authors :

![Yolo v5 team, showing their benchmark](/assets/images/posts/1*TnpzQ4Ap1P5fDg5atOBT5w.png)

### And finally a classroom

Now we know what to teach, and we have a student; we need a classroom to teach.

A classroom means an environment where :

* we can easily store and access our training data set

* we can have access to computing resources to execute the training phase

* we can use a programming language to adapt our training data set to what our model is expecting as input and to manage the different steps of the training phase

This classroom can be your computer, a big server or a cloud service, and a set of software. The choice depends on different criteria :

* the size of your training data set

* the required computing resources by the machine learning model to be trained

* the ecosystem and tooling available in this environment

> **So, to recap we have :**

> A use case

> A machine learning model candidate

> **And we now we need :**

> A training data set

> A machine learning environment

The following steps will be now to dig into the details of how to :

* build a training data set

* setup a proper machine learning environment

Coming next :

* **You are here — Part 1 — Use case and what you need to know**

* Part 2 —[ ](https://medium.com/smileinnovation/where-do-i-start-my-journey-to-headless-ecommerce-27a7043f29dd)[Training data set](https://medium.com/p/e99bbaca674e)

* Part 3 —[ Model training](https://medium.com/p/5d3c9360fbe7)

* Part 4 — [Integration and conclusion](https://medium.com/p/c8711775181f)

# **That’s all, folks!**

Did you enjoy it? If so, don’t hesitate to 👏 our article or [subscribe to our Innovation watch newsletter!](https://mailchi.mp/c414f1508567/techwatch) You can follow Smile on [Facebook](https://www.facebook.com/smileopensource), [Twitter](https://www.twitter.com/GroupeSmile) & [Youtube.](http://www.youtube.com/user/SmileOpenSource)
