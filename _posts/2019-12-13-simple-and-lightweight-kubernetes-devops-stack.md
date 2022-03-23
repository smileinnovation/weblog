---
layout: post
url: https://medium.com/@/88ab032868fa
title: Simple and lightweight Kubernetes DevOps stack
subtitle: We have chosen Rancher, Harbor, Gitea, and Drone on a real production Kubernetes cluster. We reduced CPU and memory usage as storage, and…
slug: simple-and-lightweight-kubernetes-devops-stack
description: 
tags: paas,devops,ci-cd-pipeline,kubernetes,rancher
author: Patrice Ferlet
username: patrice.ferlet
---

# Simple and lightweight Kubernetes DevOps stack

### We have chosen Rancher, Harbor, Gitea, and Drone on a real production Kubernetes cluster. We reduced CPU and memory usage as well as the storage, and of course carbon footprint.

![https://pixabay.com/fr/photos/devops-affaires-3155972/](/assets/images/posts//images/posts/0*yuJaY3WauFTB6GnN.jpg)

Kubernetes is now the most used Container platform. It is complete, effective, productive and offers a lot of control. Furthermore, while Kubernetes is a “low-level platform”, it is possible to append tools like service mesh, ingress controllers, plugins to add more objects (e.g. Velero add Backup object), storage classes, and many more. And Kubernetes is an orchestrator, so you can manage “Deployment” objects to control application version, service links, and replications. Of course, you can control that deployment with a Continuous Delivery tool.

So, Kubernetes is a perfect platform for DevOps.

Nowadays, DevOps is strongly oriented over CI/CD (Continuous Integration/Delivery) and there is plenty of solutions to work. To be honest, DevOps generally work with:

* GitLab or GitHub for source control

* Jenkins and/or GitLab runners for integration and/or deployment

And there are several reasons for those solutions to be so widespread. They are complete and very well documented.

> But… there are flip sides: weight, complexity, resources

Gitlab takes a lot of memory, and you probably don’t need the full functionalities and add-ons that are included, sometimes you only want to manage sources (with git), issues, pull requests, and so without the scrum board, reporting, or complex right management.

Gitlab runners are very well designed but can be a bit tricky to use.

Jenkins is also a bit heavy and offers many ways to build and deploy applications on several platforms, but I ask the question: do you need all the options?

A bit more of context — As a member of the AI4EU project consortium, with the responsibility of system integrator, we had to set up an environment that would allow this project to quickly evolve, to onboard OSS software and custom development from many 3rd parties, while keeping a standard process for development lifecycle and deployment

So, we need:

* to store applications sources in a Git control server

* be able to build the image, store the image, launch tests and deploy the application when a pull-request is created (testing)

* be able to be deployed when a merge is made from PR (staging)

* be able to be deployed when a tag is released (production)

* to store Docker images somewhere and to have control about vulnerability checks, storage and access rights on repositories

Spoiler alert! That’s the stack we decided to use:

![General installation](/assets/images/posts//images/posts/1*6TfzVBOziBI3if6k-ny-PQ.png)

The stack is composed by:

* Rancher to manage application catalog (Helm charts), monitoring, project access rights, and Kubernetes nodes

* Gitea to manage the sources of applications and the Helm charts versioning

* Harbor to store Docker images, check vulnerabilities, storage controls

* Drone to build the image, launch tests, and deploy on different environments

We needed to create two Drone plugins, but we will see that point following the article. It’s time to give you some precision.

# Rancher — it’s more than a UI

Rancher is mainly used because it eases the installation and it proposes a nice view. That’s right, it is nice for those topics.

But Rancher is more than a simple UI, it proposes a very efficient monitoring installation, and it can manage *Helm charts* catalog with the option to use a `questions.yaml` file that yields forms to install applications.

Helm is a fantastic solution to create application templates and to manage the deployments with “variable” parameters. This is one of the most used solutions with Kubernetes, and there is a very consequent “official” catalog to deploy in one command some services like *Nginx-Controller*, *NFS-provisioner*, *registry*, *Magento*, *Drupal*… and it’s simple to access other repositories (custom or official one).

**That’s not a gadget. **Helm is very well designed to build an application installation stack, Rancher adds a real user experience with that form generator, and we finally have more readability on what to tweak.

So, the “catalog” is graphically displayed, we can see information and form to drive the launch of the application.

As the catalog can be served from a “charts museum” or a simple Git repository, Rancher offers a way to declare custom helm charts repositories from the interface.

Rancher runs in a simple Docker container, and it has got a real advantage:

> You can import already installed Kubernetes clusters, and manage several cluster in one place! And, Rancher is not mandatory to leave the cluster to work alone. If Rancher crashes, if you want to abandon, your cluster will live anyway.

Rancher is very impressive, adaptable, useful, complete, easy to use.

# Then, let’s talk about Harbor Docker Registry

For years, we were using `registry` , proposed by Docker Inc, that does the job. Yes, it is simple and it doesn’t consume CPU or memory.

It works as expected, but it lakes of options. For example, it’s complicated to make it accessible outside the local environment, it has no good authentication and right separation, and it has no vulnerability check, storage cleanup automation…

That’s the only tool to change that will grow up the memory and CPU usage. No panic, the solution we propose is not so greedy.

After several tries, using GitLab and Nexus, for example, we gave a chance to “Go Harbor”.

https://goharbor.io/

*Harbor* is very exciting. It is free, open-source, and provides absolutely everything we need:

* Graphical Web interface (one more time, as for Rancher, it is not mandatory but it helps a lot to manage the registry)

* Docker registry (of course)

* Authentication and Authorization on “projects” level

* Certificate management + daemon to install the certificate on Kubernetes nodes

* Image signature (notary server)

* Helm chart museum

* Vulnerability checks on images with an optional tweak to refuse to deploy that images

* Replication, storage cleanup, and many other options…

It also proposes an API to get information. For example, on our pipelines, we can push images on the registry, then call API to have vulnerability check results… Awesome! 💚

And, you know what? Rancher proposes a Harbor installation from the default catalog! Installation took 2 minutes.

In our environment, we didn’t need Chart Museum because we use a git repository to manage our helm charts.

# Git repository service

GitLab is very complete, proposes a long list of tools. It also proposes CI options like using “runners”. And it can also manage the Kubernetes cluster, which means that GitLab can be used as a PaaS. Right…

But, what we need is to be able to choose a CI/CD tool. If you prefer Jenkins, that means that you will probably not use the GitLab option. Then, Rancher is already there to manage your clusters, so GitLab will be not used as PaaS…

And, unfortunately, even if we love GitLab, it is huge. It consumes a lot of memory and CPU, it is not that easy to scale… GitLab is very good if it is your main source control system deployed on a dedicated server. But here, we want to reduce the impact on the planet 🌍

So, is there a git server that offers everything we need, like issue tracker, webhooks, several authentication systems, LFS management…

Yes, there is one (at least), named Gitea!

https://gitea.io

Gitea is a fork of an initial project named “Gogs” — it is developed in Go (like Rancher, Docker, Drone, OKD/Openshift…) and benefits to the reduced CPU and memory impact usage allowed by that language.

It is fast, lightweight, and very closed to the GitHub interface. Gitea is compatible with a lot of CI/CD solutions by using webhooks. That’s a chance, Drone.io can contact Gitea and that’s what we will use.

# And then, CI/CD with Drone

We love Jenkins, really we love it. Nowadays, this is probably the best CI service that exists. It is not restrictive, proposes pipeline integration in a lot of cluster managers, and we appreciate the “pipeline syntax” language.

![A finalized Docker image build and deployment on Rancher with Drone](/assets/images/posts//images/posts/1*pGATVjReN4a1-KpWMCRDAQ.png)

But, we have discussed a lot about the usage of Jenkins and about what we “really” need. That’s a bit philosophic, but let’s take a second to think about that:

> You’re using pipeline syntax, jobs and so on… because Jenkins proposes those tools, and because “common”. OK… but if it didn’t exist, how would you like to launch tests and deployments inside Kubernetes?

Some of you will answer “actually, I would like what Jenkins propose”. We respect that.

Others will answer “I love Travis or GitLab runners, I describe pipelines in YAML format, and that’s all.” And now, we can propose other solutions.

So, you’ve got the choice, deploy Jenkins that is a bit huge in Memory (Java is not known to conserve memory 😜 ), or you take a look at “alternatives”. Of course, alternatives should be s**ustainable.

In the CI/CD universe, one tool emerges:

https://drone.io

Drone is ***ideal ***when you’re working with containers.

It uses a simple YAML syntax in a file that is default named `.drone.yml` — inside that file, you describe steps that can be triggered with several event types (merge, pull-request, tag…)

Drone proposes a lot of plugins, for example, you can build images, deploy on Kubernetes, send a message in Slack, contact servers in SSH… And you can use any Docker image to launch commands. Let’s take a look at plugins:

http://plugins.drone.io

For example, to launch Python tests with “nose”, you don’t need a special plugin:

```
kind: pipeline
```

```
steps:
- name: launch-tests
  image: python:3-alpine
  commands:
  - pip install nose
  - nose -v
```

About plugins, that’s very easy to create. You only need to build your image that launches a default command. To get parameters, *Drone* will simply give each one in an environment variable named `PLUGIN_<NAME>` — for example:

```
- name: my-plugin-test
  image: your/image/here
  settings:
    foo: bar
```

There, in my image, I will have an environment variable named `PLUGIN_FOO` that contains the value `bar` . So, let’s think about that, you will be able to create plugins in any language like Bash, Python, Go, Javascript…

That’s how we provided 2 plugins that we needed: a plugin to use “source to image” ([https://github.com/openshift/source-to-image](https://github.com/openshift/source-to-image)) and another one to contact Rancher API (to change image version in answers, that deploys the image we’ve just built, for example)

That one is made in Bash:

https://github.com/metal3d/drone-plugin-sti

And this one is built with Python:

https://github.com/metal3d/drone-plugin-rancher

*Drone* can launch image using “docker in docker”, and since several months now it can launch the steps inside Kubernetes (in real Pods)

*Drone* is compatible with a lot of Git servers, like GitHub, GitLab, BitBucket… and of course Gitea!

# More details

Finally, the following diagram gives a more detailed point of view.

![Detailed diagram of the full CI/CD stack](/assets/images/posts//images/posts/1*98mgc2nj-DYBpPFxXhqDiw.png)

Note that even if the previous diagram shows “Kubernetes” as a separate block, Harbor, Drone, and Gitea can (and are) deployed inside Kubernetes. You can see the Dev/Ops parts.

# What to say to conclude?

It’s not a problem to use Jenkins, Gitlab, Github, other Docker registries (like the Docker hub). Actually, on our stack, it’s possible to remove some parts to replace them with others. And it’s possible to use several tools in parallel as well.

The choices we made are driven by the necessity of simplicity and ease of control. We prefer to use progressive enhancement instead of searching to simplify something complex from the beginning.

That stack is used in a production environment with a large platform (more than 100 Pods across 5 environments like production, staging, and testing…) for 10 months and we don’t need to change anything at this time.

# That’s all folks!

Did you enjoy it? If so don’t hesitate to 👏 our article or s[ubscribe to our Innovation watch newsletter!](https://mailchi.mp/c414f1508567/techwatch) You can follow Smile on F[acebook,](https://www.facebook.com/smileopensource) T[witter ](https://www.twitter.com/GroupeSmile)& Y[outube.](http://www.youtube.com/user/SmileOpenSource)


