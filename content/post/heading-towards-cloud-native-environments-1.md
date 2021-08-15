+++
title = "Heading Towards Cloud-Native Developer Environments [Part 1 — The Why]"
description = "Common local development methods are going the way of the dinosaur. Cloud-native is the future. Here's why."
date = 2021-03-22T02:13:50Z
author = "Yarel Maman"
+++

In many cloud-based software development projects, a CI/CD process is designed and maintained for deploying applications in an efficient, safe, and productive manner to cloud environments.

The main focus around the CI/CD process is often on the cloud/remote side, but seldom we talk about the phase that comes right before CI/CD, which is the local development and testing done on the developer’s laptop.

In this article, I’ll show why many common methods for local development are **far from ideal**, and I’ll present a demo for an alternate, cloud-native approach which will improve your (or your developers’) productivity. 🌤

![Photo by [Christina @ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://cdn-images-1.medium.com/max/12032/0*uOwC8XfHT4_yAex9)

## 👨🏼‍💻 Local Development

While there is not a one-size-fits-all best practice — and it varies by the organization/project requirements and needs — I would say the typical CI/CD flow looks something like this:

![](https://cdn-images-1.medium.com/max/5594/1*91ycDZVuHY0R_qqZKcmcVA.png)

Let’s examine what comes *right* before step #1: Local Development.

In most cases, this gets done on the developer’s machine. That is, planning, designing, writing code, writing tests, running tests, and perhaps a POC — and in some cases, manually making sure things are working as expected.

Many development projects are heavily invested in local setups that mimic the cloud environment in order to test whether the new feature/fix you introduce works properly — without causing regression for other features and logic.

This kind of setup is mainly used to run certain kinds of automated tests (such as system tests, integration tests, or end-to-end tests). Heck, if you want, you can even manual test 😏.

Additionally, local environments are often used to debug your code. Some developers are firm adopters of what I call “**debug-driven development**”, but it can be more than just that. Debugging can be useful when you want to investigate a tough bug or better understand a complex flow.

### Common Methods For Local Development

Assume you’re working on a project which involves a bunch of microservices that are deployed to Kubernetes. Here are a couple of common industry methods that I’ve witnessed:

* **Docker-compose Method**: Writing and maintaining docker-compose manifests (which are some sort of a duplicate of the existing K8s YAML manifests) that bring up your services + 3rd party dependencies for local testing.

* **Local K8s Method: **Running a local Kubernetes cluster using minikube/k3s/kind/other, and deploying your services locally for testing, combined with a bunch of scripts.

* **Test Suites Method: **Makefiles/bash scripts that start a per-service integration tests suites, where 3rd party dependencies are being launched locally as containers via Docker ([dockertest](https://github.com/ory/dockertest) for example, is a popular choice for such a setup).

These are all possible options for how to run and test your application locally.

### Disadvantages of common local development methods

However, the methods mentioned above come with a few disadvantages as well, including:

**Lack of local support from 3rd-parties**

There are some 3rd-party cloud dependencies that do not offer a way to run locally. In some cases, you can emulate by “[mocking/stubbing](https://blog.pragmatists.com/test-doubles-fakes-mocks-and-stubs-1a7491dfa3da)” the 3rd-party APIs, but that requires a lot of work maintaining it and does not truly represent the reality. It’s a kind of cheat. What if, for example, one of your DB queries is faulty. How can you figure that out through your testing/debugging if you are not “talking” to a real DB instance?

**Resource-heavy issues**

If your application is heavy in terms of resources, your laptop may not be up to the task. For example, you run a stress test to reproduce a memory leak bug. Or, for example, a bug is only reproduced when there are 20 replicas of your application running simultaneously. Eventually, you find out your laptop is getting unresponsive 😅. This hurts productivity.

**Shutdowns while debugging**

If you want to test or debug a tough/rare bug, which may happen once every few days, your local machine might go shutdown/sleep/hibernate, forcing you to start all over again.

**No single source of truth**

In the **Docker-compose Method**, you find yourself violating the [“single source of truth](https://en.wikipedia.org/wiki/Single_source_of_truth)” principle by maintaining multiple representations of your environments.

**Running integration tests ≠ actual integration**

In the **Test Suites Method, **while running integration tests is a good practice with a lot of benefits (even when running locally), it still isn’t the same as integrating your change on a real cloud environment. In some cases, there are integration/deployment bugs and failures it might not detect (such as network connectivity issues to other services, configuration issues, boot crashes, etc.)

The disadvantages described above hurt your development experience by making you less productive. And in many cases, they do not reflect reality (A classic case of “[*It works on my machine!](https://hackernoon.com/it-works-on-my-machine-f7a1e3d90c63)”) *and by that, they bring little value*.*

Is there a better, more modern way of approaching the local development phase, while improving productivity and usability? Yes, yes there is.

## ☁️ ***Cloud-Native Developer Environments***

Now let’s explore remote, cloud-native environments.

By transforming your local environment into a remote environment, the issues above don’t exist. Plus, it’ll upgrade your development experience into a modern, reliable, cloud-native experience.

### How do cloud-native developer environments work?

Allocate a **separate** cloud environment **for each** of your developers, which is a replica (not quite, but close enough) of your development/production environments.

By using a cloud-native personal environment approach, each developer on a team has the autonomy to use their environment as they see fit. Whether it's for testing new features and bug fixes, reproducing bugs, or even tinkering for self-learning. All done in an environment very similar to production. ✌️

You could then adapt your tooling around this to make these environments more valuable by, for example, running manual/automated tests, “mirroring” traffic from production towards the personal environments, etc.

Another valuable thing that comes to mind is conducting live, remote debugging in these environments, in complete isolation without interfering with other environments. You can make use of existing off-the-shelf tooling built for this purpose (we’ll see an example of such a tool in the next chapter).

### Winning Points For Productivity, Maintainability, And Reliability

There are many reasons why you should switch to cloud-native dev environments, but I’ll share my top eight.

You Will:

* Re-use your existing K8s YAML manifests, without having to maintain duplicate deployment configurations. (Keeping it DRY 🌵)

* Deploy to a cloud environment is done the same way as you deploy to dev/prod, but with added isolation.

* Stop relying on your local hardware. Cloud infra can easily scale up if you want to run tests that demand heavy resources and scale down on the weekend.

* Launch long-running tests in your environment without the fear of your laptop shutting down or going into hibernate mode.

* Detect integration issues quickly, since you’re using real cloud service APIs, and not mocks (for example, you can communicate with PubSub, Cloud SQL, Datastore, and many other cloud services).

* Take advantage of the built-in infrastructure that the cloud offers for monitoring, alerting, profiling, tracing, and log aggregation, just as you benefit from it for production.

* Save time by not having to run fancy setup scripts all the time. The cloud is rapid. Your environment is ready for you when you need it.

* Use these environments to learn Kubernetes by practice (if you’re a beginner), without affecting other environments.

Finally — in addition to developers — DevOps/SRE engineers can benefit from a personal environment by running infra configuration changes on their environment, before hitting environments shared by others.

## 🙋‍♂️ Hi! I Have a Question!

### ***Will my cloud bill explode?***

The initial thought is to provision an entirely separate independent replica of your cloud instances for each developer, isolated by projects, where each project contains a replica of your infrastructure.

This is generally a good practice. Especially security-wise, as it provides good isolation. However, the cost of this approach can become a problem, especially as your company/team scales.

Since we are talking about development environments, which are isolated from the production environment in many cases, you can make the tradeoff of costs over security.
>  Side-note: This tradeoff between security and costs varies between different organizations and requirements — make sure you understand the implications. This [GKE Security guide](https://gkesecurity.guide/project_organization/) can help you.

***So, how do we make that tradeoff?*** You can make use of multi-tenant features of your cloud services in order to save costs by **sharing resources between environments**.

This is often called “Soft Multi-Tenancy”, whereby “Soft” you limit the risk by allowing multiple users from the same “trusted” organization to share the resources.

For example, all developers in a team working on an application can share a K8s cluster, where each developer has the entire application stack replicated in its own K8s namespace.

Another example is that all developers can share a single DB instance, where each developer will have a dedicated DB user, schema, and tables.

This is the approach we are going to demo, in the next chapter.

### *Is this a pain to set up and maintain? 🤕🥸*

Not really! You can (and should) make it automated and easily reproducible with Infrastructure as Code and GitOps principles.

Once all of this configuration is described as code, creating/destroying a personal environment is a matter of a few clicks. Less room for human error as well, as the process is automated.

### Why isn’t it in the development environment scope? 🤔

The classic development environment isn’t isolated. It’s shared between all the developers. You merge your code, and your friend merges their code at the same time, so it’s essentially a melting pot. It’s not feasible debugging and running experiments in a shared environment once your team grows.

In addition, The development environment stage is a bit too late. You have already created a PR, you passed the code review, you merged the PR. Now you discover your changes immediately crash the service. This is now considered a blocker and requires a revert/rollback/hotfix. It is time-consuming for you and your fellow developers.

### Personal environments or ephemeral environments? 👀

The simple answer is that it’s up to you.

Basically, with personal environments, you are dedicating an environment for a specific person for a long-term purpose. While with ephemeral environments, you are creating an environment for a specific **change** (which gets destroyed after you merge that change).

My colleague Mike Sparr wrote a great [article](https://blog.doit-intl.com/recreating-heroku-ci-cd-review-apps-on-google-cloud-platform-845fa0957232?source=friends_link&sk=f84712c1579d96be64edd4ceb6f2ff2f) on ephemeral environments and you should check that out as well.

You could definitely do both — though that might be an overkill for your needs.

In [Part 2](https://www.yarelm.com/heading-towards-cloud-native-environments-2/), I will illustrate such a setup using an example architecture, using Terraform, ArgoCD, and Telepresence. [Go to Part 2!](https://www.yarelm.com/heading-towards-cloud-native-environments-2/)
