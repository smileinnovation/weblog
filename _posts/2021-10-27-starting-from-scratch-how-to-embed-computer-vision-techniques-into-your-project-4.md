---
layout: post
url: https://medium.com/@/c8711775181f
title: "Starting from scratch, how to embed computer vision techniques into your project #4"
subtitle: Part 4— Integration
slug: starting-from-scratch-how-to-embed-computer-vision-techniques-into-your-project-4
description:
tags:
- computer-vision
- deep-learning
- visual-search
author: alrou
image: assets/images/posts/1*NUC03THTnMV6aE1EakIrXA.png
---

Part 4— Integration

How to integrate it?

The SageMaker work is done. We have a data set with labels, a trained model, and an endpoint to request objects detection.

But how do we integrate it within our product? Well, this endpoint can be seen as an API. We send a request, and as a response, we get a JSON object that contains a list of detected objects with their coordinates in the original image.

The remaining constraint is the usage of the SageMaker SDK to create this request. To remove this constraint, we can rely on a proxy that will accept a regular HTTP Post request and return the same JSON object.

We developed this proxy as a serverless service using AWS Lambda. This [lambda](https://github.com/smileinnovation/visual-search-yolov5-sagemaker/blob/master/3-predict/lambda-inference.py) is exposed to the Internet with AWS API Gateway. We can handle each POST request inside this lambda, create a SageMaker request to the model endpoint, and return the response as a JSON object.

Now inside ElasticSuite, we rely on a standard REST API to include visual search in the product UX.

Using this lambda, the typical round trip for one inference is about 2 seconds with a 640x800 jpeg image (150k) and using an entry-level CPU-based instance type (2xvCPU / 8Gb of Ram). The inference time is, on average, 800ms.

The final step was to use this API from the ElasticSuite product to allow end-users to submit their images to trigger an automatic search based on recognized objects.

![Visual search results](/assets/images/posts/1*NUC03THTnMV6aE1EakIrXA.png)

# What can we conclude?

First of all, and as a disclaimer, this is not production-ready :

* The model accuracy is not optimal

* The data set object list is minimal (5 objects)

* You should take a lot of training parameters into consideration, but to keep our proof of concept simple, we focused on the Yolo best practices (good is good enough for now)

But it works!

And to go further :

* Why not test other models than Yolo? (i.e. : EfficientDet / Faster-RCNN). Yolo is well-known for its inference speed. Other models are renowned for their accuracy. We should give it a try.

* Data is king: we should also definitely spend more effort on the data set to understand how to improve it. This is also related to which direction we want this use case must go (objects catalog). The maintenance cost of this data set is also something to consider: **80% of our effort in this proof of concept was to capture images and label them**.

* In this proof of concept, we used AWS SageMaker, but we could also have a look at their competitors: SageMaker is a great product but can be complex to understand. It is worth looking at other platforms to see how they address the topics we reviewed here (labeling, training, endpoint, etc.)

All the steps

* [Part 1 — Use case and what you need to know](http://Part 1 - Use case and what you need to know)

* Part 2 — [Training data set](https://medium.com/p/e99bbaca674e)

* Part 3 — [Model training](https://medium.com/p/5d3c9360fbe7)

* **You are here — Part 4— Integration and conclusion**

[**Smile**](https://www.smile.eu/) is the proud editor of [**ElasticSuite**](https://elasticsuite.io/), a great **Magento** open-source extension, with **more than a million downloads** on Github and **trusted by more than 1500 top retailers worldwide**. It’s the leading solution for **intelligent search and merchandising** on Magento.

As part of this product road map, we have to test and experiment with new features.

**That’s all, folks!**
Did you enjoy it? If so, don’t hesitate to 👏 our article or [subscribe to our Innovation watch newsletter!](https://mailchi.mp/c414f1508567/techwatch) You can follow Smile on [Facebook](https://www.facebook.com/smileopensource), [Twitter](https://www.twitter.com/GroupeSmile) & [Youtube.](http://www.youtube.com/user/SmileOpenSource)


