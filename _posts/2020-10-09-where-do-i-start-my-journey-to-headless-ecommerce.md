---
layout: post
url: https://medium.com/@/27a7043f29dd
title: "Where do I start my journey to headless eCommerce?"
subtitle: You shouldn’t think about an e-commerce platform as a one-fit-all solution because there is no such thing, the devil hides in the details…
slug: where-do-i-start-my-journey-to-headless-ecommerce
description: 
tags: 
- ecommerce
- jamstack
- headless
- serverless
- headless-commerce
author: fagas
---

![An open road to create a unique way to interact with your customers.](/assets/images/posts/0*7OYfxSA16zi39izZ)

You shouldn’t think about an e-commerce platform as a one-fit-all solution because there is no such thing; the devil hides in the details and your margin and profit. As you may know, the headless approach is a clear separation of concern between the frontend and backend relying on API.

Let’s see in this article how you can design a custom-made headless commerce platform.

# You will rely on external data providers.

Building a commerce platform is a long journey, and you should put your effort into what really matters for your business: stock management, pricing, loyalty, product management with key partners.

If it’s key to your business, you should build your own solutions, but buy a COTS (Commercial off-the-shelf) if it's a commodity.

![](/assets/images/posts/1*s7cWGI1QJFmAmjbJ3YWrZQ.png)

There is always a cost: either a costly license, an internal resource needs, or the need of an external provider to complete your task! So choose wisely if you should own it or buy it.

![The age-old question for software ownership.](/assets/images/posts/1*SIOoTXXwis_FynmHTGPT6Q.png)

# Headless commerce ecosystem

This growing ecosystem provides few services to start building your application. They do one thing, and they do it well; to focus on your product, you should leverage their services.

### Emerging Commerce Services

Many solutions can answer your needs, from headless [Adobe Commerce](https://business.adobe.com/lu_fr/solutions/commerce.html) to [Commerce layer](https://commercelayer.io/) passing by [Shopify](https://www.shopify.com/) or [Crystallize](https://crystallize.com/).

**Omnichannel Order management hub
**Solutions like [Commerce Layer](https://commercelayer.io/) emerge from this ecosystem, providing our out-of-the-box integrations with payment gateways and shipping carriers to let your clients get paid safely, print shipping labels with one click, manage returns, and more, all within a unified user interface.

It’s a complete paradigm shift focusing on a best-of-breed approach, leaving monolith solutions far behind in terms of agility and time to market.

![Commerce layer user interface.](/assets/images/posts/0*beBlzO0i6Y-0BPsd)

### Commerce services are deeply linked to a content-first strategy

**Content **as a Service: from headless [Drupal ](https://www.drupal.org/)to [Strapi ](https://strapi.io/solutions/ecommerce-cms)passing by [Sanity](https://www.sanity.io/), and [Prismic](https://prismic.io/), or [Contentful](https://www.contentful.com/)… Dozens of services exist, and you have to pick the one you’re familiar with.

You’ve created a product page with your favorite CMS/headless cms; an async call to price and stock will allow getting fresh and mandatory information of the selected product.

![](/assets/images/posts/0*v7tFr9gkrZYjvTX-)

The checkout process is what a customer must go through when purchasing the items in their shopping cart. The customer can be either a *guest* — i.e., they proceed by just providing an email address — or *logged* — i.e., they authenticate themselves with a password. When logged in, the customer gains access to their private data, such as their saved addresses and credit cards, making the process faster.

![](/assets/images/posts/0*-T8z6_Lm-EXwpRFb)

### A headless commerce architecture

Unlike the traditional platforms, headless commerce doesn’t lock your creativity into any rigid templating system. Allowing you to build unique and blazing-fast eCommerce without worrying about servers, security, and all the boring stuff.

![](/assets/images/posts/1*USyFhCF_9WC_n3U8QePjUw.png)

# A land of services

Key components are obviously Commerce and Content Services, but you need additional services to **provide a unique customer experience** with glue around your services to tie them together.

* **CDN / static** hosting to host your app: [Netlify](https://www.netlify.com/), [Vercel](https://vercel.com/), S3… to host your [Next.js](https://nextjs.org/), [Gatsby ](https://www.gatsbyjs.com/)app

* **API services** to ease access to your service level: [Kong](https://konghq.com/kong/), API Management (AWS/Azure),[ WSO2 API Manager](https://wso2.com/api-manager/)

* **API / Flows** — glue around your services: [Apollo](http://www.apollographql.com) Graphql, [n8n](https://n8n.io/), [Hasura](https://hasura.io/), [AWS Amplify](https://aws.amazon.com/fr/amplify/)

* **Search as a Service**: the SAAS solution [Algolia ](https://www.algolia.com/)is a key player in the search ecosystem, a competitor like [Constructor.io](https://constructor.io/) emerges with a focus on capturing revenue rather than relevance. Others are based on open-source Elastic Search engines such as [Swiftype](https://swiftype.com/).
A major flex can be to index content and serve it directly with the search engine API.

* **Customer Identity and Access Management**: overtime managing customer information is getting harder, from social login to legal compliance (GDPR or other privacy legal toolkits/requirements), so dedicated solutions emerged. 
Many players offer different solutions from simple authentication services to access management: [Auth0](https://auth0.com/), [Cognito](https://aws.amazon.com/fr/cognito/), [Akamai identity](https://www.akamai.com/fr/fr/products/security/identity-cloud.jsp), and [WSO2 identity](https://wso2.com/identity-and-access-management/)…

* Payment / Checkout services are either included in a commerce platform or outsource to [Stripe](https://stripe.com/), [Checkout.com ](https://www.checkout.com/)with additionnal fees.

* **Forms**: capturing user data easily has always been a challenge: [FormKeep](https://formkeep.com/), [FormSpree.io](https://formspree.io/), [Typeform](https://www.typeform.com/), [SurveyMonkey](https://www.surveymonkey.com/)

* **Notification**: [Twillio](https://www.twilio.com/), [Batch](https://batch.com/), Mailchimp, Adobe Campaign

# Picking the right solution for your needs

We can help you choose!

* Do you want to build a headless platform?

* What components are going to build/buy?

* Is your use case-ready, or do you need help demonstrating it?

>> We are here to [help you build the future](https://www.smile.eu/en/contact)

In our next article, we will deep dive into the commerce services ecosystem and the Jamstack ecosystem to have a clearer view.

Coming next :

* Part 1 — [Headless commerce](https://medium.com/smileinnovation/headless-commerce-187fbe19f075)

* **You are here** — Part 2 — Where do I start my journey to headless eCommerce?

* Part 3 — I want my CDN application!

* Part 4 — Legacy management and progressive migration

*Authors*

[**Fabien Gasser — Innovation Advisor](https://www.linkedin.com/in/fgasser/) @ [Smile Innovation dpt.](https://innovation.smile.eu/)
**We explore, test, and recommend new technologies, creating new offers and value propositions for our clients.

We work on artificial intelligence focusing on conversational assistants and computer vision using service design.

[**Jean-Charles Bordes — VP e-commerce](https://www.linkedin.com/in/jean-charles-bordes-75693221/) @ [Smile](https://www.smile.eu/fr)
**With 15 years of XP and 300 projects, we support our customers in their activity's business and operational performance.

Experienced explorers, we are the partner of the first projects or technically complex projects with high business stakes.


