---
layout: post
url: https://medium.com/@/4bb374dc4c02
title: A Serverless 2.0 point of view
subtitle: A major evolution in the serverless movement is arriving. Let’s see why you should care about it
slug: a-serverless-2-0-point-of-view
description: 
tags: serverless,paas,devops,aws-lambda,single-page-applications
author: alrou
---

![Photo by Steve Johnson on Unsplash](/assets/images/posts/1*PflLUH-l_1gXzycOfdiLhQ.jpeg)

### A major evolution in the serverless movement is arriving. Let’s see why it is important to understand it and how it will change the way we are building apps

Microservices, PaaS, Containers, Continuous Delivery are the new day-to-day for a modern IT organization. But the next step in this evolution started a few years ago, is knocking on the door. Cloud providers (AWS, Google, Azure) are already pushing their solutions, and Open Source solutions are also ready. Let’s dig into the details of this next step.

The rise of serverless concepts was motivated by the growing need for digital apps, shorter product development time and agility to adapt a product to the market. The purpose of these technological concepts is also to ease the transition to [cloud-native architecture principles](https://thenewstack.io/10-key-attributes-of-cloud-native-applications/).

One core concept is Microservice. This technique aims to build an app using a collection of loosely coupled services. It’s like applying the Unix philosophy “*Do One Thing and Do It Well” *to the Web. This principle is directly linked to the DevOps approach and all related tooling.

# D.O.T.A.D.I.W.

In digital app age, “*Do One Thing”* can be a complex task like triggering a machine learning process, looking for some records in a database, adding an item in your cart, executing a transaction in a bank back-office system, reaching another microservice, etc.

A good practice to build Microservices is to rely on well-known frameworks to develop specific business logic. These frameworks exist for different languages and platforms (e.g. Sprint Boot, Play Framework/Akka, Flask, ExpressJs, Goa, …)

These frameworks give you access to common ways to expose your services and getting access to different middleware, filesystem or database system. In addition to business logic development, this also means that developers still need to tackle various technical topics like data serialization/de-serialization, HTTP routing, database system constraints and the packaging of the microservice for its deployment.

Of course, adding more and more “microservices” requires more and more automation and control for testing, integration and deployment steps of these services. This is where DevOps, containers, and PaaS step in.

The DevOps approach aims to deliver to the developer the required tooling to automate all development steps.

A PaaS aims to abstract server management, low-level infrastructure, to orchestrate environments, apps availability and resources provisioning, etc.

This is a serverless “1.0”: as a developer, you don’t need to think anymore about underlying servers. You just focus on delivering a container that contains your microservice. A complex app will be defined as a collection of containers.

# ︎︎√(Dev + Ops)²

But it would be also a mistake to consider that operational concerns will be out of scope for developers. They will be in fact even more important for them. Because the border between “Dev” and “Ops” is shifting, developers need to have a clear understanding of their infrastructure/middleware specifications and limits. In example: a relational database does not scale like a key-value data store but offer another kind of service.

And in that context, the good news is it is now very simple to choose/change infrastructure/middleware components thanks to PaaS and Cloud providers: agility is also possible here, we can try and change easily.

At the same time Ops, people should evolve to support all these new infrastructure services but also become an advisor for topics like security, scalability and compliance issues: to prevent common pitfalls.

This is a new equation to solve here to find the right balance in your team and to help your team members evolve in their new duty.

# Serverless++

Ok, thanks to this serverless concept, we are able to build a solution that can be deployed in a serverless environment. This solution can be defined by the following components :

* A client app (single page app, mobile app, …), if the solution requires it

* A set of microservices for the business logic

* Storage services (DB, files, queues) and 3rd party services (IAM, logs, analytics, …)

But to produce the microservices, developers still need to manage these tasks :

* Framework selection (language, services compatibility) and maintenance (security update, technical debt, …)

* Framework usage to produce and maintain the expected business logic

* The packaging of the business logic (framework + container)

* Applying container security best practices

What about reducing this list, to keep developers focus on delivering the business logic?

The main idea would be to reduce the amount of code while keeping the ability to use 3rd party services.

Producing less amount of code to deliver a solution means to build things faster and to reduce maintenance cost.

This is where the concept of “Function as a Service” (“FaaS”) step in.

# Serverless “2.0” = Serverless “1.0” + FaaS

Well, the story is about business logic, data, and infrastructure.

Data and infrastructure should be seen “as services”, but under control of the DevOps approach.

Business logic should be seen as a set of functions. Functions that could take the benefits of the “data and infrastructure as a service”.

That’s the “FaaS” definition. Instead of building your set of functions thanks to frameworks, we integrate them into a platform dedicated to their execution and their connection to a serverless ecosystem: We produce the same or more with less code.

From an architecture state point, this also brings on the table a set of patterns to manage data like [CQRS](https://martinfowler.com/bliki/CQRS.html), [Event-sourcing](https://martinfowler.com/eaaDev/EventSourcing.html), or [GraphQL](https://en.wikipedia.org/wiki/GraphQL). These patterns are not new, but there are becoming even more relevant in such a context.

These FaaS platforms connect your functions to external services thanks to event handlers to receive incoming requests and to data source provider to reach external data store.

And of course, the purpose of these platforms is also to manage the auto-scaling of resources to execute your functions, thanks to their deep interaction with the underlying infrastructure.

# So why is it so important?

We can reasonably say that, in software engineering, each past evolution increased developers efficiency thanks to new abstraction layers.

This is exactly what this concept aims to do again. But this time it also brings a new paradigm with the idea of “function” (a.k.a. nanoservice).

On one hand, it allows:

* to focus on business logic first

* to have scalability at “function” level

* to reduce the amount of code to maintain

* to leverage from a large ecosystem of cloud services

But on the other hand, it means to tackle new challenges like:

* keeping a consistent solution architecture based on a significant variety of “functions”

* managing the significant increase of 3rd party services used

* keeping control of the solution quality: CI/CD and DevOps are key!

# Ok, any examples of what can be done?

Yes!

* Event streaming, like in IoT projects. There are perfect candidates: a huge amount of small data to collect or to serve, but with simple logic for each of them

* Domain-specific API, like images or video manipulation, ETL, custom analytic, or time-based batch

And not to forgot Web apps or mobile apps back-end: it’s time to disrupt monolithic business apps.

I think there is an opportunity to build more affordable and more flexible e-commerce platform. That’s where payment platforms are already ready (i.e. Stripe, Square, …)

There are also opportunities for the open-source community to build Serverless SDK for business silo (e-commerce, CRM, ERP), and possible new business models to develop: leverage the business knowledge of well-established platforms to build ready to use set of functions.

# And what kind of tools do I need?

There is now plenty of available solutions on the market with different pros/cons, but to name some of them :

* From public cloud providers: AWS Lambda, Google Cloud Functions, Azure Functions

* From the open-source community: Nuclio, Fn, OpenFaaS (to be deployed on your favourite PaaS (e.g. private/public cloud Kubernetes)

* Or some agnostic solutions, like the “[Serverless Framework](https://serverless.com/)”, which is an “SDK” to package your functions in a way that the same code can be deployed on different kind of FaaS/PaaS platforms (AWS, Azure, Google, K8S, …)

# Key takeaways

To sum up :

* Serverless is more than just FaaS

* Serverless with FaaS is a new opportunity to build cloud-native apps faster

* DevOps approach is even more important

* Serverless apps require to take into consideration “nearly” new architecture patterns

* There are new business/product opportunities to explore/create :)

# That’s all folks!

Did you enjoy it? If so don’t hesitate to 👏 our article or s[ubscribe to our Innovation watch n](https://www.getrevue.co/profile/smileinnovation)ewsletter!
You can follow Smile on F[acebook,](https://www.facebook.com/smileopensource) T[witter ](https://www.twitter.com/GroupeSmile)& Y[outube.](http://www.youtube.com/user/SmileOpenSource)


