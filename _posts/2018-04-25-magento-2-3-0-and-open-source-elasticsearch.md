---
layout: post
canonical_url: https://medium.com/@/ab1a3b3007d5
title: "Magento 2.3.0 and Open Source ElasticSearch"
subtitle: "It has been announced during Magento Imagine that ElasticSearch support will be moved in the open-source version of Magento for the 2.3.0…"
slug: magento-2-3-0-and-open-source-elasticsearch
description:
tags:
- ecommerce
- magento
- elasticsearch
- search-engines
author: aufou
image: assets/images/posts/1*WATj-MWqAZ-fljJEEIZqVA.png
---

It has been announced during [Magento Imagine](https://imagine.magento.com/) that [ElasticSearch](https://www.elastic.co/fr/) support will be moved in the open-source version of Magento for the 2.3.0 release, while previously reserved for Magento Commerce.

<iframe src="/assets/images/posts/f222b4e422fd9c637290ea42ece84764.html"></iframe>

As a long-time supporter of ElasticSearch, we are very happy with this decision. We are also proud that **our contributions to the ElasticSearch Magento module were included**.

# Why ElasticSearch ?

It is well known that MySQL search engine was weak and suffer a lot of relevance issues that are very hard to solve. Not to talk about performances issues caused by using MySQL to develop features like full-text search, faceting or product list sorting …

Using a specialized search technology just like ElasticSearch is the way to go to solve these issues. Magento search will be more relevant out-of-the-box.

Until now, Magento considered those integrations as enterprise feature only. I am quite sure it was slowing down the adoption of those technologies and had caused a lot of problems for the merchants. One of the most recurrent problems is the number of third-party modules that were not fully compatible with the ElasticSearch implementation provided by Magento.

# What about existing (Elastic)Search extensions ?

There are several ElasticSearch extensions for Magento Open Source like [**Wyomind**](https://www.wyomind.com/fr/magento2/elastic-search-magento.html), [**Mirasvit**](https://mirasvit.com/magento-2-extensions/elastic-search-ultimate.html) or [**Bubbleshop**](https://www.bubbleshop.net/fr/magento2-elasticsearch.html).

In my opinion, those extensions were a good option for people that did want to buy Magento Commerce. But there are not so many differences with what will be proposed by the Magento integration of ElasticSearch. In some way, those extensions can’t stay relevant on the market.

A lot of search engine extensions will become less relevant because Magento now embeds a search technology that performs better. I think about all those SolR or Sphinx integrations but also to some commercial SaaS engines.

![](/assets/images/posts/1*WATj-MWqAZ-fljJEEIZqVA.png)

# What about ElasticSuite ?

Our Innovation team has matured our own search and merchandising solution for quite a long years now, it’s **ElasticSuite **which is available in **open source** in our GitHub repo

https://github.com/Smile-SA/elasticsuite

The solution gained a very strong traction in the community with **more than 1000 websites using it** and will reach **100 000 downloads in few days**.

The question of the future of ElasticSuite is on the table with Magento releasing its ElasticSearch implementation in open source. But there are many reasons to be optimistic about it.

For example, unlike many other search extensions, our main goal has never been to dodge Magento Commerce but to build an integrated search and merchandising solution that is complementary of its functionalities and enhance it.

When analyzing ElasticSuite setup, we realized that more than **20% of the websites using ElasticSuite are Magento Commerce** one. This tends to show that **our module is still relevant even for people that could use Magento ElasticSearch** implementation. And here are the main reasons our community members invoke :

* The unique feature set proposed by ElasticSuite (virtual categories, visual merchandiser, rule-based optimization, faceting improvement, …)

* Our full-text search relevance model is better than what proposed by Magento ElasticSearch implementation. It is also more tunable from the admin to fit any business case and it is easier to support new languages

* ElasticSuite performances are better than Magento ElasticSearch both for indexing and query. It is a big deal for a retailer with a huge catalog or many store views.

# The Future

Now we have to be smart and open minded. **We’ll try to work closely with Magento to study the merging possibility** of some ElasticSuite features in Magento in order to make the project maintenance easier. We think it may be valuable for the entire Magento ecosystem.

MSI community project shows us Magento is now more **open to community contribution**. The instant purchase feature recently merged in Magento is another great example of what could be done. So why not imagine the future of ElasticSuite in the same way?

We have not yet a clear idea of how ElasticSuite will be shipped in the future, but reasons for using ElasticSuite have not vanished with Magento open sourcing its ElasticSearch implementation and ElasticSuite unique feature set stay ahead of other solution. **We’ll continue to actively develop** and keep our effort on ElasticSuite. **A 2.6.0 release will be out in few weeks with a lot of new features**.


