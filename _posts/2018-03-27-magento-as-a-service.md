---
layout: post
canonical_url: https://medium.com/@/e788b6f90ed2
title: Magento as a Service
subtitle: How-to build an e-store factory
slug: magento-as-a-service
description: "As part of our continuous improvement, one particular interesting topic we had in mind for e-store projects was the ability of delivering a new store as a full fledge service : an e-store as a service"
tags:
- magento
- technology
- devops
- kubernetes
- docker
author: pafer
image: assets/images/posts/1*XlppZAstcxT6vl-68BQzzQ.png
---

*Co-written with [Romain Ruaud](), teammate at Smile, Magento expert*.

Thanks to a great [Magento expertise and partnership](https://www.smile.eu/en/technologies/magento), Smile is renowned for its [e-store offer](https://www.smile.eu/en/offers/digital).

As part of our continuous improvement, we are always looking for innovation to deliver the best to our clients. And one particular interesting topic we had in mind for e-store projects was the ability of delivering a new store as a full fledge service : an e-store as a service.

And to deliver on-demand a new store, indeed some changes are required in the way we organised the work but also to adapt our tooling. In today’s words we can speak about **DevOps **and** PaaS**.

# A traditional Magento dev environment

For a long time, we used Magento with virtual machines, then we had implemented a developer skeleton for LXC. Both worked. VMs were heavy to share and deploy, and we had complexity to make them near of the hosting platform. LXC was a revolution, more lightweight, very closed to the OpenVZ platform on the hosting side, that was a big enhancement for us.

At Smile we build complex Magento applications since the appearance of the solution, which led us to acquire a lot of experience on this product, especially about how to deploy, host and interface huge E-Commerce websites.

>> We have been four-times Europe Partner of the the Year (2010–2014), two-times Spirit of Excellence (2015–2016) and Global Elite Partner (2017) awarded by Magento, which is putting Smile to the world top 5 Magento integration partners.

Several years ago we decided to industrialize our deployment system with the following pattern :

* Production/Staging environment : [OpenVZ](https://openvz.org/Main_Page) instances provisioned by Ansible

* Developers environment : [LXC containers](https://en.wikipedia.org/wiki/LXC) provisioned by the same [Ansible](https://www.ansible.com/) recipes.

### Benefits

This was the first step in order to obtain a versioned and replicable project skeleton across all the different environments of an application. This allowed us to start a new Magento project from scratch quickly, by provisioning a new monolithic LXC container.

Developers are able to override the Ansible recipes of a given project to reflect their changes on the OpenVZ production platform.

### Disadvantages

However, there were still many drawbacks to this approach that were hard to cover :

* Ansible recipes remains tightened to Linux distribution used on the final environment target.

* Software versions (PHP, Elasticsearch… ) are not easy to change/upgrade because they may also be dependant of the distribution.

* We had a working CI with Jenkins / Gitlab Runner but building a CD pipeline was complicated.

* This does not allow us to have a proper “bookshelf” where we could find inheritable boilerplate components when starting a new project.

* Not easy to scale up or down since it requires adding a new OpenVZ container and provisioning it with the recipes.

* We were also unable to start sandboxes or ephemeral environments for parallized testing of different features.

For the developer side, LXC was a real step forward, but to be honest it’s not so easy to tweak, and we can’t deploy them as is. We still need to have deployment environments (staging, production, …) that will be different from the developer computer.

This is of course challenging for our goal : to support a Magento a service, creating new instance must be easy as few clicks. And as consequence the execution environment specifications must be the same from developer’s computer to highly-available production servers

# DevOps to the rescue

There is many different definitions of what means DevOps. Some of them focus on the tooling, or focus on a methodology, others that this is a philosophy…

Here is how at Smile we are thinking DevOps:

* The goal is to make Dev (developers) and Ops (operators, system admin) **communicate** smoothly and using the same tooling

* A way to accelerate and facilitate automation of every task that can be automated

* A software deployment impact must be predictable, and can be achieved by a developer or any person with the access right to the target environment

* Ops can provide segmented, controlled and monitored environments and support developers in their activities

We can resume that DevOps is combination of cultural philosophies, practices, and tools that increases an organisation’s ability to deliver applications and services at high velocity.

>> To create a unique channel where Dev and Ops can communicate and work, we just need to have methods, tools and **determination**.

Usually, the first tool that enter in DevOps conversation is a **CI platform**, Jenkins is a well-known one but you can also use GitLab-CI (with “runners”). We’ve got both at Smile, following project constraints we can choose either the first or the second, but sometimes we use both for one project.

Then you need to deploy your project in a testing and staging environment. Until now, we pushed the code on the server and we passed scripts to make migrations. Ok, nice but not efficient in time, in comprehension, and when it won’t work that’s hard for a developer to locally reproduce the bad behavior.

But with Docker and a container platform, it changes the situation.

### Dock me up !

Some of the concepts that come with the DevOps approach like immutability and idempotence are important for what we are trying to do :

* Immutability : to ensure that each software or infrastructure components are replaced for every deployment rather than being updated in-place

* Idempotence : to ensure that a change applied several times will always give the same result

And containers are made to support these concepts.

Today, **Docker** is the king of containers solution. The war is won. With the ease of *Docker-Compose* mixed with *Dockerfiles*, everything is more flawless, adaptable and easy to extend. Docker come also with a rich ecosystem.

Docker Containers allow us to change Linux distribution, to change version of an application (database, cache server, Nginx or Apache ?), to override and test many possibilities. Docker is not as famous just for marketing reasons, it is really the most suitable tool for this kind of work.

>> Docker is the right one to use, but what about development, tests and deployment processes ?

I’m sure you’re already heard about Docker… And since several months, you couldn’t have missed articles and demo about **PaaS** as **Kubernetes**, **OpenShift**, or **Rancher**. We are already using OpenShift for a long list of services, so why not to use it for our Magento 2 developer teams ?

We have CI platforms (Gitlab CI and/or Jenkins) so the testing part will not be a problem. What we have changed are:

* **developer** environment that is now using Docker

* **deployment** environment is now on OpenShift

Now, let’s see what we need to create to help developers.

### Strong productivity using a PaaS + CI/CD

PaaS (Platform as a Service) is not *really* a deployment tool, it is a platform that yields and setup environment for projects. That’s the purpose of Rancher, OpenShift or Kubernetes: you create projects (or namespaces) then you can spawn environment inside.

As Rancher and Kubernetes, OpenShift orchestrate a set of Docker containers according to some rules you have to define. Actually, Rancher and OpenShift integrates Kubernetes. OpenShift add the abilities to have its own private container registry and to build new container images on-demand.

Each deployed containers can have volumes and all of them are encapsulated in a *Pod*. To access a *Pod*, you need *Services* that makes port forwarding and load balancing. And the whole needs are automated and simple to use.

### Why to use a container PaaS to develop ?

At first sight, the difference between VM, LXC and the use of containers is not so evident.

As I said earlier, OpenShift has got a registry and can build images. That means that Ops can prepare images, push them on the registry, and developers will use them to develop. Ok, developer pull images, then mount source in a container, and then…

What happens when developer need to push sources ?

That’s the power of *BuildConfig* and *DeploymentConfig* which are provided by OpenShift that gives advantages. Let’s check what I mean.

![](/assets/images/posts/1*XlppZAstcxT6vl-68BQzzQ.png)

The “builder” takes sources from the right branch or tag (following the given *BuildConfig* in OpenShift), then it uses initialisation scripts, build and compile translation, templates and so on… Image is pushed in *ImageStream* and then deployed as container following the “deployment configuration” (aka *DeploymentConfig* in OpenShift) taking the right image in the internal registry. What is very important to understand now is that **the base image used to build an Application Image on OpenShift is the same one used by developpers**.

![CI-CD with Gitlab, OpenShift Containers Platform and how Developer is implied](/assets/images/posts/1*ESVn123PDX0l_fHMmZPqcg.png)

Everything is automated. Developers and Administrators can check build logs, get images, and can monitor deployment logs. All that is almost without a hitch.

But Magento 2 needs more that PHP and MySQL… so let’s speak about its containerization.

# Magento 2 containerization

We are going to speak about Magento 2 : we need it to be containerized and to be able to have high availability on the PaaS.

### Magento 2 with full services

Actually, we need to have High Availability for **the whole services list**. That means : database, cache, http server, file-system… And of course everything has to be resilient and scalable.

![Magento2 Stack on our OpenShift containers platform](/assets/images/posts/1*UtEkWRE3MkK5MX1m-MYXDQ.png)

Quickly, that’s what we can list:

* MySQL — let’s use MySQL Galera to make it HA

* Redis — let’s use Redis Sentinels

* Http Server — we will use NGinx and PHP-FPM

For the storage, Openshift provides storage volumes from different distributed FS as far as you’ve installed one. On our testing platform we are using GlusterFS, but Amazon S3 and ESB can be used, you may also use Ceph and many others.

As part of many Magento deployment and proudly develop at Smile, we also need to include [ElasticSuite](http://magento-elastic-suite.io/en/), which depends on Elastic Search.

On PHP container, we love to have monitors, so we added [BlackFire](https://blackfire.io/) and [NewRelic](https://newrelic.com/).

Ok, do you remember that I said that developer side we will have *more or less *the same architecture ? So, you are saying to yourself:

>> “He’s mad, so many services on a developer laptop…”

And you’re right ! We decided to reduce container number to only what the developer will work, that means that we only propose to developer to get php and nginx images **from our OpenShift registry**. The others containers are only the monolithic version.

![Container stack on developer computer](/assets/images/posts/1*kypxw3_hn9wfQfuoUzsiBQ.png)

### Magento2 as a ready-to-use image on a bookshelf

Since Openshift is providing us Image Streams, we are able to generate ready-to-use images.

As we have seen previously, our Magento2 container is basically a PHP-FPM container populated with the application sources.

But we needed to go further, because we wanted to have a Magento image that we are able to start **in a couple of seconds**, to achieve dynamic scaling in near-real time. For those who are familiar with Magento 2, you already know that on a production context, the Magento application cannot start immediately from the PHP sources and will need some mandatory steps to be processed before starting.

To do this, we decided to proceed most of the Magento prerequisites during the image building :

* Dependencies are installed with Magento composer.

* Environment configuration is injected into the app/etc/env.php.

* Production mode is enabled.

* Code compilation is proceeded.

* Static content is published.

At this step, this means that we have a **ready-to-start and fully configured Docker image for our Magento container**, in which php-fpm can start immediately listening to incoming connections.

We are now able to start as many containers as we need with this image, to obtain a running pool of identical Magento application ready to handle our numerous user’s requests.

Back on the developer computer !

Our skeleton is a bit “more complex” to be fully presented. But to resume, developers can locally see this:

* a `Makefile`

* a `docker-compose.yml`file

* a `.openshift` directory with scripts and Dockerfiles

To initialise the project, developers can take this skeleton, tweak Docker images (actually it’s only an override to not make it able to use another image than the provided one from our registry) and adapt scripts if needed. That all. Running “make” command will prepare environment (get images from OpenShift, call composer, install database…) with Docker.

To make it really easier and faster, we take advantage of *source to image* tool from Red Hat.

### The s2i power

Build image can be a big effort and a very long task. That’s why Red Hat creates and gives to the community a nice tool that make things a bit lighter. Its name: **Source to Image**

https://github.com/openshift/source-to-image

That tool allows to prepare environment at the “build time” (image creation) and then at “startup time” (container start). That is a big help to let developers to integrates their own tweaks.

So, what we did is pretty simple. We created a list of scripts that should be launched at certain condition (is Magento already deployed ? Are we on OpenShift ?). Locally, we detect if Magento sources are already present, if not we call “composer” to install requirements. Then the script continues and install Magento 2 with given parameters (name of the site, admin password, …)

![The full process of image building and deployment](/assets/images/posts/1*-me-MnZmKosjppFfYx8lJg.png)

After a couple of seconds we can see a local website that is ok to work.

Then, if the project is pushed on GitLab, the team can prepare a Magento 2 environment on OpenShift. The only needed task is to use the provided “application template” that displays a form to fill. Here, the values specific to the environment have to be specified (name of the site, number of MySQL nodes, password for admin, …) and this is **now that the Gitlab project address is given**. After form validation, image builder will take the given branch, build, push, and so on… Application is deployed and the init-scripts are launched. Everything that the developer saw on his computer will be executed the same way in OpenShift. Adaptations are only done to respect OpenShift conditions.

That’s all ? No… we had some problems to resolve.

# Tech problems and solutions

First, Magento 2 initialisation is not able to use Redis Sentinels — we opened issues to explain the problem and what they answer was that we can “install Magento 2 with one redis, then change configuration afterward”

The problem is that we can’t do that on OpenShift as easily as they say. The Redis masters and sentinels are started “as is”. We can’t change Pod topology afterward, we need to make deployment in “one shot”.

![](/assets/images/posts/0*PDeNAnehX08_X22n.png)

To fix that little problem, we decided to build a little “proxy” that makes the job. As soon as Magento 2 is able to let us work with Redis Sentinels at startup so we will remove that part. Magento 2 connect to that proxy as if it is Redis. The proxy find master and sentinels and knows if an election was made. That’s **fast and scaled**.

We named this project “Ellison” as from the TV show “The sentinel” where the main character name is “Jim Ellison”, you can find it here: [https://github.com/metal3d/redis-ellison](https://github.com/metal3d/redis-ellison)

Finally, we proposes Docker images to easily deploy it on OpenShift and Kubernetes from our “Redis HA” repository:

https://github.com/Smile-SA/redis-ha

# Conclusion

Working with Docker and OpenShift offers a higher productivity and help a lot to avoid deployment failures. Using Docker images that are built on our platform prevent developers to have differences between their environment and the production one.

We now have a high available solution to host that solution, which is compatible with self-hosted, Amazon AWS, Google GCE, Digital Ocean, and many other machine providers. Also, **a large base of our work can be deployed on Kubernetes** as Openshift is running over it. But in this case we loose a part that is automated by OpenShift, that is the “Image build”. You can of course do it yourself on Jenkins/Gitlab-CI or whatever the CI platform you’re using.

We now have continuous integration platform fully integrated in the process, I mean that Gitlab-CI is not only a “testing platform”, it is now part of the deployment by communicating between GitLab and OpenShift.

Also, developers need to trained to understand new environment and processes. That’s not so complicated but we need time to teach them how they should now work.

Last… We will work on a problem that everybody here should mention: what about database migration ? Should we set website in maintenance mode while we’re doing changes ? How to insure that the deployment properly makes that step ? what about migration problem and rollbacks ? That’s the questions we’re working on !


