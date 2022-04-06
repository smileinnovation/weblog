---
layout: post
url: https://medium.com/@/c3c5dd224058
title: I want my CDN Application!
subtitle: Performance and time to interaction are key to increase conversion and retention. And for that, PWA has done an amazing job promoting a new…
slug: i-want-my-cdn-application
description:
tags:
- pwa-for-ecommerce
- headless-commerce
- jamstack
- api
- graphql
author: fagas
---

![Access your application around the world!](/assets/images/posts/0*rqiej0HCGx0WpDyW.png)

*Performance* and *time to interaction* are key to increase conversion and retention. And for that, [PWA](https://en.wikipedia.org/wiki/Progressive_web_application) has done an amazing job promoting a new way of building applications. But pages can be rendered way faster using static page generation, fetching only what’s need to be fresh.

Imagine a **product’s category page loading in 30ms **instead of 1,5 seconds on your current e-commerce platform.** That is 50 times faster!**

A static site allows better performance, SEO, and scalability. I think we forgot how blazingly fast the display of a static site is. 😅

**We start to speak of* CDN A*pplications.**

<iframe src="/assets/images/posts/2aa8d54b25d71c9d953c95c2bc598cd1.html"></iframe>

# Jamstack, the cool gang

![](/assets/images/posts/0*B5exEvFSbgGXL3Xa.png)

[JAMstack ](https://jamstack.org/)stands for *JavaScript*, *APIs*, and *Markup*. Jamstack.org defines each component as the following.

* JavaScript (J) running entirely on the client handles any dynamic programming during the request/response cycle (e.g., [Vue.js](https://vuejs.org/), [React.js](https://reactjs.org/)).

* APIs (A) accessed over HTTP with JavaScript abstract all server-side processes or database actions (e.g., [Twilio](https://www.twilio.com/), [Stripe](https://stripe.com/)).

* Markup (M) should be pre-built at deploy time, usually using a site generator for content sites or a build tool for web apps (e.g., [Gatsby.js](https://www.gatsbyjs.com/), [Webpack](https://webpack.js.org/)).

Let’s see now how we can migrate from using it.

# *Migration from Solutions to Services*

The JAMstack is a complete paradigm shift focusing on a best-of-breed approach, leaving monolith solutions far behind in terms of agility and time to market.

![](/assets/images/posts/0*Gp4grzvBHUbVZWDU.png)

**Pro**

* Focused on the front-end build and delivery, Better developer experience

* Cheap hosting and maintenance: improve scaling, heightened security, instant cache invalidation with [Netlify](https://www.netlify.com/), for example.

**Cons**

* Highly dependent on third-party systems

* Complex eco-system and services coordination

# Frontend Framework and Static Site Generators

JAMstack's approach relies on HTML being rendered at build time or in the web browser to serve static content to the client; different frameworks exist to do so.

![Adam Sturrock analysis of the market.](/assets/images/posts/0*BcC2LgVLplVvHbaJ.jpg)

Some are more business-oriented such as [*vuestorefront.io*](https://www.vuestorefront.io/), based on the e-commerce domain (Category, Product list page, Product detail page).

* [Vue Storefront](https://www.vuestorefront.io/), [Shogun](https://getshogun.com/), [Front Commerce](https://www.front-commerce.com/en/), [Commerce-UI](https://www.commerce-ui.com/), [Next.js Commerce](https://nextjs.org/commerce)

Some Server Site Generator is more framework oriented like:

* [Gatsby](https://www.gatsbyjs.com/) combines static site generation and client-side rendering with third-party APIs. In addition, Gatsby now offers incremental build to reduce by 4000 the build time for your static website.

* [Next.js](https://nextjs.org/) & [Vercel](https://vercel.com/)

* [Nuxtjs.org](https://nuxtjs.org/) for *vue.js* static page generation

# JAMstack ecosystem

This growing ecosystem provides few services to start building your application: they do one thing, and they do it well.

![Jamstack conference partner networks.](/assets/images/posts/0*S4ZBsRXTs1xQL5lm.png)

To focus on your product, you should leverage their services; let’s pick the right product for you.

### Frontend

A typical frontend stack doesn’t exist; it’s like a teenager’s room: changing from time to time.

<iframe src="/assets/images/posts/229d9d7ac93177f72399b5e0b8c749bd.html"></iframe>

### Asset hosting

To display the content with a minimum size, you will need an image or video content provider solution; CDN can do this. Nevertheless, your reverse proxy configuration can also be addressed to serve resized images according to the consuming device (desktop/mobile/tv..) and the user bandwidth.

Dedicated tools :

* Open Source On-Premise: [Thumbor](http://thumbor.org/)

* CDN offers the capacity to serve adapted media: *Netlify*, *Vercel*, Cloud providers services…

* Or you can pick your own tool with a Saas approach: [Imgix](https://imgix.com/), [Cloudinary](https://www.cloudimage.io/), [Cloudflare](https://www.cloudflare.com/), [TwicPics](https://www.twicpics.com/).

# API and services

It’s now time to interact with hot content with API for authentication, inventory, cart, and order management.

### API Gateway and API management

Cloud services will provide those tools to route your query to the right endpoint, either an application, a lambda, or even a media.

Dedicated solutions exist to build your own realm, such as *WSO2* or *Mulesoft*, on top of your information system.

* Open Source — On-Premise Solutions: [Kong](https://konghq.com/), [WSO2 API Gateway](https://wso2.com/api-manager/)

* SAAS or OnPremise: [Zapier](https://zapier.com/), [Mulesoft](https://www.mulesoft.com/)

https://medium.com/smileinnovation/kong-manage-your-flask-apis-d58ff4ea808d

### IAM

User management in modern applications is mostly management outside the application; authentication is hard. So is managing a fine-grained authorization and access policy. I’m not even talking about compliance to GDPR or any other law for managing user data.

* Open Source — On-premise: [WSO2 Identity](https://wso2.com/identity-and-access-management/)

* Saas Solutions: [Akamai Identity](https://www.akamai.com/fr/fr/products/security/identity-cloud.jsp), [Okta](https://www.okta.com/)/[Auth0](https://auth0.com/fr), [AWS IAM](https://aws.amazon.com/fr/iam/), [Google Cloud Identity](https://cloud.google.com/identity)

A [NextAuth.js](https://next-auth.js.org/) integration could also do authentication.

### Backend For Frontend

The first time I saw a BFF was at the beginning of the mobile application market. The new need to provide a clear technical interface with a REST API hiding the ugly truth of the internal complexity to this fresh generation of consumers search for speed and mobility.

A few years later, this approach became an obvious pattern to deal with legacy systems and migrate them graceful to microservices,

One approach to implement Backend For Frontend could be to a GraphQL endpoint.

**GraphQL, when REST is not enough for your Developer eXperience
**A new devil or a new standard GraphQL is becoming a key approach when building an API. Integrated with tools like *AWS AppSync* and you have a native online/offline real-time data update tool.

* On-Premise Open Source: [API-Platform](https://api-platform.com/), [Apollo Server](https://www.apollographql.com/), [Hasura](https://hasura.io/)

**AWS Amplify, the back-office killer.
**[Amplify](https://aws.amazon.com/amplify/) features a data store that syncs data between your site and the cloud, authentication, machine learning, analytics, and more. *AWS Amplify* works natively with *AWS* but is designed to be open and pluggable for any custom back-end or service.

### Caching

Behind the battle of API, one key thing is data sourcing in application performance and caching.

> **Remember that reducing the return to the source allows us to avoid calculation time and so reduce the ecological footprint of our applications.**

Inducing fine cache management on several levels is crucial and a key to your performance and your ability to deploy in several countries: consumer cache (local storage, cache), API cache (TTL), in-memory cache (Redis), database cache…

Solutions :

1. Local Storage

1. CDN: Cloud services, *Akamai*, *Netlify*, *Vercel*

1. Reverse proxy: *nginx*, *apache*

1. In-memory storage: *Redis*

# Serverless, still a server but not yours

We are now at the infrastructure level, and most of you have migrated from bare metal to cloud providers by commodity; most developers will tell you: “*I have absolutely no desire to run, maintain, update and monitor servers or server processes.*”.

We have few answers for them.

**PaaS **has seen a revolution with the emergence of *Kubernetes* and the implementation of machine management through services like *Kapsule* to create, configure, and run a cluster of pre-configured machines for containerized applications.

https://www.metal3d.org/blog/2020/kapsule-scaleway/

**Lambda functions **could be seen as the ultimate way to implement microservices or microlith, depending on how critical you are on tech trends. Nevertheless, Lambda functions are the key for a fast time to market delivering features. However, they could also be toxic for your IT if set aside from your organization, like a realm with no king to rule over data and API endpoints.

💁‍♂️ Be aware that a cold start can add seconds to start your service.

Also, if your Lambda function writes, by example, in a Dynamo DB, your bottleneck will be your database, not your lambda, so be sure to load test to deduce your real capacity (Request Per Second), consider 5k to 10k RPS where monolith applications reach their own limitations.

**Services marketplace** such as [*Code.Store*](https://code.store/) exposes GraphQL endpoints served by Lambda functions, allowing developers to rely on ready-to-market business components.

### How much do you have to manage?

Comparison between different as-a-Service models and traditional IT, where the whole stack is hosted on-premise. The longer the arrow, the more management is required by and/or on behalf of the client.

![Pablo Iorio accurate vision of infrastructure](/assets/images/posts/0*-rLczLcR2MRsuIyb.png)

# Let’s wrap it up!

Now we are aligned on the requirement to start composable commerce with a CDN application; you can start building your own ecosystem based on the challenge inherent to your industry: stocks, delivery, services…

![Large overview of a classic headless commerce ecosystem.](/assets/images/posts/1*HfDX1J2Lk7oGp6gE8Jjr9A.png)

# **🙋‍♂️ Since you’re here

Do you have headless commerce? Or maybe you want to jump on the train of CDN Application for your activity? Do you ask yourself :

* Do you want to build a headless platform?

* What components should I build?

* What components should I buy?

* Is my use case ready, or do I need help demonstrating it?

‍💪 We are here to h[elp you build the future ](https://www.smile.eu/en/contact)of your headless commerce stack.

Coming next :

* Part 1 — [Headless commerce](https://medium.com/smileinnovation/headless-commerce-187fbe19f075)

* Part 2 —[ Where do I start my journey to headless eCommerce?](https://medium.com/smileinnovation/where-do-i-start-my-journey-to-headless-ecommerce-27a7043f29dd)

* **You are here** — Part 3 — I want my CDN application!

* Part 4 — Legacy management and progressive migration

*Author*

[**Fabien Gasser — Head of innovation](https://www.linkedin.com/in/fgasser/) @ [Smile ](https://innovation.smile.eu/)
**We explore, test, and recommend new technologies, creating new offers and value propositions for our clients.


