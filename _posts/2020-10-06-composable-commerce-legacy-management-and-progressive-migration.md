---
layout: post
url: https://medium.com/@/ddb24361f7da
title: Composable commerce  —  Legacy management and progressive migration
subtitle: A spaghetti architecture with data everywhere, resources are tightly coupled, evolutions are limited due to your systems’ complexity, …
slug: composable-commerce-legacy-management-and-progressive-migration
description: 
tags: 
- microservices
- graphql
- composable-commerce
- api
- e-commerce-solution
author: fagas
---

![My last application and me.](/assets/images/posts/0*cIIwH7NKrkGhs1RP)

# Where you probably stand from

A spaghetti architecture with data everywhere, resources are tightly coupled, evolutions are limited due to your systems’ complexity, so your developer experience is terrible. Too much domain knowledge is required making the platform harder to maintain, but also, it’s getting hard for developers to onboard as a result of your velocity slowdown.

# Why do we need BFF architecture?

>> A Back End for Frontend is a single-purpose edge service for UIs and 3rd party solutions

*According to Sam Newman, author of [Building Microservices*](https://www.amazon.fr/Building-Microservices-Sam-Newman/dp/1491950358)

This approach is also an organizational change as we will rely on a clear separation of concern between teams.

### The application is faster as it’s lighter

* Remove backend-targeting logic from frontend

* Decouples frontend apps from our data

* Reduce the weight of client-side JS scripting

### The developer experience is better

* Ease onboarding of frontend developers

* Unified access to data in one place

* Accelerated development with more robust isolation of business logic and technical layers: less code and great tools, simplifying particular use-cases (SSR, WebSocket, Aggregation, Authentication, Logging)

# Legacy migration

**Back to our spaghetti plate, it’s time for progressive change, and Backend For Frontend is one approach used for legacy migration, but how does it work?**

We will have to re-architect the legacy system and switch from a legacy system to a new one more seamlessly with a [Backend For Frontend](https://samnewman.io/patterns/architectural/bff/) pattern.

This logic is instrumental in moving the business logic away from the frontend and backend by manipulating resources from the source of truth, either with the legacy or the new application to connect with.

![](/assets/images/posts/1*MdhzUWZMpxVnnJgvBol6hg.png)

An approach could be to build a GraphQL endpoint to get information from various data providers and aggregate it for the consumer without over-fetching as the query is done by the frontend.

It allows complete flexibility in building new features and dealing with legacy systems.

![Using GraphQL for API Aggregation](/assets/images/posts/1*QaimeFU5BJ-_lQPuudXaqg.png)

![Composing schemas declaratively with federation](/assets/images/posts/0*jpt9dSa9BHDLjJCb.png)

If you want to dig more into that approach, you can[ have a look at Netflix DGS](https://netflixtechblog.com/how-netflix-scales-its-api-with-graphql-federation-part-1-ae3557c187e2).

# Resource and developer experience

**How do we break a monolithic enterprise application into smaller pieces to assign teams to them, and then how do we connect those pieces?**

Adding an abstraction layer between backend systems and frontend allows to ease the developer experience and resource consumption:

>> As an user I want to see my customer informations

1. **Applications **describe their requirements:
User story: As a customer application, I want to fetch customer information.
Key advantages: Decoupled frontend apps from our data by fetching discoverable data without over fetching, so you drastically reduce the codebase by removing the business logic.

1. **BFF **— Graph functions as an auto described marketplace of data and services, 
User story: As a BFF, I fetch and aggregate data from various data sources (new or legacy)
Key advantages: unifying the business logic across defined channels, flexible migration between two sources of truth, ease to integrate new components.

1. **Resources** — Service describes their capabilities.
User story: As a resource, I display a range of information with a defined TTL.
Key advantages: Remove or keep data clean without specific per channel data or formalism

![](/assets/images/posts/1*T2oO34dafeeuQv44rV6gwg.png)

With the described architecture, you can split your development team into different layers: Frontend, BFF/Middleware, Resource.

This approach allows a progressive migration to break its functionality out one service at a time, enabling different teams to work on various products and features without interfering with one another even if the end resource is changing or not ready.

Also, splitting down an application with a domain-driven approach reduce the amount of technical & business knowledge to onboard new developers, so you increase your capacity and velocity.

### Will this increase latency?

The latency induced by the BFF is negligible compared to a messy frontend.

### Any other way to change to do it?

Orchestration vs. Choreography is an eternal question. I would say orchestration is easier to set up with legacy applications as it’s often close to the initial pattern in place, where Choreography represents a cultural change introducing an event notion.

![BFF, as you guessed rely on the orchestration pattern.](/assets/images/posts/1*z4r6JsQZgXdLgzraZtlhRg.png)

**Let’s dig a little more into Choreography.**

![](/assets/images/posts/1*G4_ZaDW4gHk9WjuGbIiFkA.png)

Here you can see that producers and consumers have to manage events, so switching to that kind of approach is higher. Still, so is implementing a circuit breaker pattern to handle load and recovery with an orchestration pattern.

So good luck picking the right approach for your need!

# 🙋‍♂️ Since you’re here

Do you have headless commerce? Or maybe you want to jump on the train of CDN Application for your activity? Do you ask yourself :

* Do you want to build a headless platform?

* What components should I build?

* What components should I buy?

* Is my use case ready, or do I need help demonstrating it?

‍💪 We are here to h[elp you create the future ](https://www.smile.eu/en/contact)of your headless commerce stack.

Previously :

* Part 1 — [Headless commerce](https://medium.com/smileinnovation/headless-commerce-187fbe19f075)

* Part 2 — [Where do I start my journey to headless eCommerce?](https://medium.com/smileinnovation/where-do-i-start-my-journey-to-headless-ecommerce-27a7043f29dd)

* Part 3 — [I want my CDN application!](https://medium.com/smileinnovation/i-want-my-cdn-application-c3c5dd224058)

* **You are here** — Composable commerce — Legacy management and a progressive migration.

*Author*

[**Fabien Gasser — Head of innovation](https://www.linkedin.com/in/fgasser/) @ [Smile](https://innovation.smile.eu/)
**We explore, test, and recommend new technologies, creating new offers and value propositions for our clients.


