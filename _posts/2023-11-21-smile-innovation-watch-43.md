---
layout: post
title: "Smile’s Innovation Watch #43"
subtitle:
slug: smile-innovation-watch-43
description:
tags:
- NVIDIA
- GPU
- Apple
- Health
- OpenAI
- Twitter
- X
- Meta
- Facebook
- Instagram
- 23andMe
- Terraform
- OpenTofu
- Mistral
- AI
- OpenSource
- Privacy
- Licensing
category: techwatch
author: thmil
featured: false
image: assets/images/posts/2023/11/DALL·E 2023-11-16 15.26.51 - 1. A scene depicting the aftermath of a data breach at 23andMe. The illustration shows a chaotic office environment with concerned employees and compu.png
---
Welcome to our 43rd Tech Watch! This edition brings you right into the heart of some thrilling tech advancements and pivotal shifts in the digital world. Imagine the power of AI getting a turbo boost with Nvidia's H200 GPU - it's like giving ChatGPT a supercharged brain! And then there's Apple, navigating the tricky waters of health tech. They're dreaming big with features like blood glucose monitoring, but also treading carefully to make sure they get it right.

We're also seeing a bold move by OpenAI. They've set up a new team called Preparedness, diving into the deep end to tackle the scary but important risks of AI, like nuclear threats. It's like having a superhero team for the AI world! On the open-source front, things are buzzing with excitement. Mistral AI's new language model is out for everyone to tinker with, and OpenTofu is standing up for the open-source spirit by forking Terraform in response to licensing changes.

Privacy is a hot topic too. Twitter's X network is stirring up some controversy with its plan to collect biometric data, and 23andMe’s dealing with the challenges of keeping our genetic data safe. And finally, Meta’s shaking things up in Europe with a new ad-free subscription for Facebook and Instagram - a fresh twist in the tale of digital privacy and user choice. So, strap in and enjoy this ride through the ever-evolving, always fascinating world of tech!

⏳ Reading time: 11 minutes

## 💡 Innovation

![The new H200 monster GPU from NVIDIA](/assets/images/posts/2023/11/nvidia_h200_hero_2-760x380.jpg)

### [Nvidia introduces the H200, an AI-crunching monster GPU that may speed up ChatGPT](https://arstechnica.com/information-technology/2023/11/nvidia-introduces-its-most-powerful-gpu-yet-designed-for-accelerating-ai/)

Nvidia has announced the HGX H200 Tensor Core GPU, which uses the Hopper architecture to accelerate AI applications. The H200 is the first GPU to offer HBM3e memory, providing 141GB of memory and 4.8 terabytes per second bandwidth, which is 2.4 times the memory bandwidth of the Nvidia A100 released in 2020. Lack of computing power has been a major bottleneck of AI progress this past year, hindering deployments of existing AI models and slowing the development of new ones. The H200 may help alleviate the compute bottleneck and lead to more powerful AI models and faster response times for existing ones.

**The H200 is ideal for AI applications** because it performs vast numbers of parallel matrix multiplications, which are necessary for neural networks to function. It is essential in the training portion of building an AI model and the "inference" portion, where people feed inputs into an AI model and it returns results. The H200 will be available in several form factors, including Nvidia HGX H200 server boards in four- and eight-way configurations, compatible with both hardware and software of HGX H100 systems. **Amazon Web Services, Google Cloud, Microsoft Azure, and Oracle Cloud Infrastructure will be the first cloud service providers to deploy H200-based instances starting next year**.

![](/assets/images/posts/2023/11/STK071_apple_K_Radtke_02.jpg)

### [Apple keeps exploring (and scrapping) new ways to detect users’ health](https://www.theverge.com/2023/11/1/23941627/apple-watch-healthcare-clinics-health-app-iphone-airpods)

**Apple has been working on expanding its healthcare offerings** for years, but concerns from CEO Tim Cook and COO Jeff Williams have slowed its progress, according to a Bloomberg report. The company's biggest ambitions have been held back by worries that mistakes in the healthcare field could damage Apple's reputation. **The report reveals that the Apple Watch was supposed to monitor blood glucose from the start, but various challenges have held it back, including accuracy across skin tones and blood types**. Apple is still working on the feature, spending "in the high tens of millions of dollars a year," and development is being handled by the in-house Mac and iPhone chip design teams. The feature may use AI to predict if someone could become diabetic, but it is likely a long way off.

**Apple created a secret company called Avolonte Health**, which reportedly kicked off the work **to realize the Apple Watch blood glucose monitor** in 2011. The Watch was also supposed to launch with blood oxygen and EKG capabilities, but "component sourcing issues, battery, and reliability concerns" pushed the features out. Apple reportedly plans to add limited blood pressure monitoring to the Watch, which will identify trends rather than specific measurements. The company has also explored health-focused smartwatch accessories, including a bathroom scale and a non-inflating blood pressure cuff. **Apple has considered health clinics inside Apple Stores** and separate ones that would stand on their own, but the idea was tested as the employee-only AC Wellness clinics that were plagued by cost and management issues.

![](/assets/images/posts/2023/11/DALL·E 2023-11-16 15.00.35 - 1. An illustration of Aleksander Madry, depicted as a Caucasian male with short brown hair, wearing glasses and a lab coat, leading a diverse team of .png)

### [OpenAI forms team to study ‘catastrophic’ AI risks, including nuclear threats](https://techcrunch.com/2023/10/26/openai-forms-team-to-study-catastrophic-risks-including-nuclear-threats/)

OpenAI has created a new team called Preparedness, which will be responsible for assessing, evaluating and probing AI models to protect against "catastrophic risks". The team will be led by Aleksander Madry, the director of MIT's Center for Deployable Machine Learning. **Preparedness will track, forecast and protect against the dangers of future AI systems, ranging from their ability to persuade and fool humans to their malicious code-generating capabilities**. OpenAI CEO Sam Altman is known for his fears that AI "may lead to human extinction". The company is open to studying "less obvious" areas of AI risk, too, and is soliciting ideas for risk studies from the community, with a $25,000 prize and a job at Preparedness on the line for the top ten submissions.

Preparedness will also be charged with formulating a "risk-informed development policy", which will detail OpenAI's approach to building AI model evaluations and monitoring tooling, the company's risk-mitigating actions and its governance structure for oversight across the model development process. It's meant to complement OpenAI's other work in the discipline of AI safety, with focus on both the pre- and post-model deployment phases. The unveiling of Preparedness comes after OpenAI announced that it would form a team to study, steer and control emergent forms of "superintelligent" AI.

## 🔓 OpenSource

![Mistral Logo with a stylished 7 representing the "7b" version](/assets/images/posts/2023/11/mistral-7b-v0.1.jpeg)

### [Mistral AI makes its first large language model free for everyone](https://techcrunch.com/2023/09/27/mistral-ai-makes-its-first-large-language-model-free-for-everyone/)

French AI startup Mistral has released its first model, Mistral 7B, which it claims outperforms other models of its size and is free to use without restrictions. The model is available to download via a 13.4-gigabyte torrent, and Mistral has also created a GitHub repository and Discord channel for collaboration and troubleshooting. The model has been released under the Apache 2.0 license, which has no restrictions on use or reproduction beyond attribution, making it available to hobbyists, corporations and the Pentagon, as long as they have a system capable of running it locally or are willing to pay for the requisite cloud resources.

Mistral 7B is a refinement of other “small” large language models like Llama 2, offering similar capabilities at a considerably smaller compute cost. The model was developed privately, using private money, and the datasets and weights are likewise private. Mistral's business model appears to be based on the free model being free to use, but if users want to dig in, they will need to purchase the company's paid product. Mistral's commercial offering will be distributed as white-box solutions, making both weights and code sources available, and the company is actively working on hosted solutions and dedicated deployment for enterprises.

![OpenTofu logo (a block of Tofu) jumping out a megaphone with a fork in its right hand](/assets/images/posts/2023/11/fork-announcement.png)

### [OpenTofu Announces Fork of Terraform](https://opentofu.org/blog/opentofu-announces-fork-of-terraform/)

HashiCorp recently announced that they are **changing the license to all their core products, including Terraform, to the Business Source License (BSL)**. In response, the OpenTofu manifesto was published, and over 100 companies, 10 projects, and 400 individuals pledged their time and resources to keep Terraform open-source. Since no reversal has been done, and no intent to do one has been communicated, **the OpenTofu initiative has created a fork of Terraform called OpenTofu**. The project is community-driven, impartial, layered and modular, and backwards-compatible. The end goal is to have OpenTofu as part of the Cloud Native Computing Foundation to ensure the tool stays truly open-source and vendor-neutral.

![Meta logo](/assets/images/posts/2023/11/acastro_211101_1777_meta_0001.jpg)

### [Meta’s AI research head wants open source licensing to change](https://www.theverge.com/2023/10/30/23935587/meta-generative-ai-models-open-source)

Meta has released its large language model Llama 2 relatively openly and for free, which is a stark contrast to its biggest competitors. However, some still see the company’s openness with an asterisk. Meta’s license makes Llama 2 free for many, but it’s still a limited license that doesn’t meet all the requirements of the Open Source Initiative (OSI). **Meta’s limits include requiring a license fee for any developers with more than 700 million daily users and disallowing other models from training on Llama**. Meta vice president for AI research Joelle Pineau, who heads the company’s Fundamental AI Research (FAIR) center, is aware of the limits of Meta’s openness. But, she argues that **it’s a necessary balance between the benefits of information-sharing and the potential costs to Meta’s business**.

Meta’s AI division has worked on more open projects before. One of Meta’s biggest open-source initiatives is PyTorch, a machine learning coding language used to develop generative AI models. The company released PyTorch to the open source community in 2016, and outside developers have been iterating on it ever since. Pineau hopes to foster the same excitement around its generative AI models, particularly since PyTorch “has improved so much” since being open-sourced. Meta’s approach to openness feels novel in the world of big AI companies. OpenAI began as a more open-sourced, open-research company. But OpenAI co-founder and chief scientist Ilya Sutskever told The Verge it was a mistake to share their research, citing competitive and safety concerns.

## 🗽 Privacy & Freedom

![X app, showing Elon Musk account, display on a smartphone hold in one hand](/assets/images/posts/2023/11/getty-musk-x-1-760x380.jpg)

### [X (née Twitter) wants to collect your biometric data and employment history](https://arstechnica.com/tech-policy/2023/08/x-nee-twitter-wants-to-collect-your-biometric-data-and-employment-history/)

Twitter-owned social network X is set to collect users' biometric information, employment history, and educational history, according to an updated privacy policy. **The new policy, which went into effect on September 29, says that X "may collect and use your personal information"** to recommend potential jobs for users, share with potential employers, enable employers to find potential candidates, and show more relevant advertising. **The policy does not specify what kind of biometric data X would collect**. The changes come as X plans to offer video and audio calls, which will work on iOS, Android, Mac, and PCs and will not require a phone number, according to owner Elon Musk.

However, the changes could face scrutiny from the Federal Trade Commission, as **Twitter agreed to settlements in 2011 and 2022 with the FTC over privacy violations before Musk bought the company**. Several of Twitter's top privacy and security executives resigned in November 2022, reportedly over concerns that Musk's rapid rollout of new features without full security reviews would violate the FTC consent decree. Musk's massive layoffs also fueled a new FTC investigation into whether the company has enough resources to protect users' privacy. A pending lawsuit filed by a Twitter user in July alleges that X collects biometric data without properly notifying users in violation of the Illinois Biometric Information Privacy Act.

![A scene depicting the aftermath of a data breach at 23andMe. The illustration shows a chaotic office environment with concerned employees and computers](/assets/images/posts/2023/11/DALL·E 2023-11-16 15.26.51 - 1. A scene depicting the aftermath of a data breach at 23andMe. The illustration shows a chaotic office environment with concerned employees and compu.png)

### [23andMe says private user data is up for sale after being scraped](https://arstechnica.com/security/2023/10/private-23andme-user-data-is-up-for-sale-after-online-scraping-spree/)

Genetic profiling service 23andMe is investigating a data breach after private user data was scraped off its website. **Last month, an unknown entity advertised the sale of private information for millions of 23andMe users** on an online crime forum. The posts claimed that **the stolen data included origin estimation, phenotype, health information, photos, and identification data**. 23andMe officials confirmed that private data for some of its users is up for sale, but they do not have any indication at this time that there has been a data security incident within their systems. **The cause of the leak is data scraping, a technique that essentially reassembles large amounts of data by systematically extracting smaller amounts of information available to individual users of a service**.

The DNA relative feature allows users who opt in to view basic profile information of others who also allow their profiles to be visible to DNA Relative participants. If the DNA of one opting-in user matches another, each gets to access the other’s ancestry information. The data included profile and account ID numbers, display names, gender, birth year, maternal and paternal haplogroups, ancestral heritage results, and data on whether or not each user has opted in to 23andme’s health data. Some of this data is included only when users choose to share it. While there are benefits to storing genetic information online so people can trace their heritage and track down relatives, there are clear privacy threats.

![A landscape-oriented digital collage representing Meta's new paid subscription service in the European Union. The image features prominent logos of Facebook and Instagram](/assets/images/posts/2023/11/DALL·E 2023-11-16 15.41.07 - A landscape-oriented digital collage representing Meta's new paid subscription service in the European Union. The image features prominent logos of Fa.png)

### [Facebook and Instagram launch a paid ad-free subscription](https://www.theverge.com/2023/10/30/23938283/facebook-instagram-ad-free-subscription-eu)

Meta, formerly known as Facebook, has **launched a paid subscription service to remove ads from Facebook and Instagram in the European Union**. The subscription, priced at **€9.99 per month on the web and €12.99 per month on iOS and Android**, is aimed at addressing concerns by the European Union about Meta's ad targeting and data collection practices. The company believes that by offering users the option to pay for the service to remove ad targeting or use the service for free but consent to its data collection practices, it will have more clearly met privacy requirements set by European data laws, including the Digital Markets Act and GDPR.

Meta will continue to offer free access to its products for people who do not wish to pay, with the experience for non-paying users remaining unchanged, and its existing ad preference tools remaining available. **The ad-free subscription will only be available to people aged 18 and older in the EU, EEA, and Switzerland**, and will initially apply across all linked Facebook and Instagram accounts, **with Meta eventually charging extra for linked accounts**.

## That’s all folks

Did you enjoy it? If so don’t hesitate to share our article or [subscribe to our Innovation watch newsletter!](https://smile-1.eo.page/w1xpf) You can follow Smile on [X](https://www.twitter.com/GroupeSmile) & [Youtube](http://www.youtube.com/user/SmileOpenSource).
