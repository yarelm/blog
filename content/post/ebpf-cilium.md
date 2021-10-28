+++
title = "eBPF, Cilium, Dataplane V2 and All That Buzz (Part 1)"
description = "What's all the noise about? Is the hype around eBPF justified?"
date = 2021-10-28T02:13:50Z
author = "Yarel Maman"
+++

If you’re following the latest cloud computing trends, you’ve probably come across the eBPF term quite a lot. It’s hard not to notice it. Let’s learn why.

Looking for a modern security, observability, and monitoring solution, while maintaining optimal performance? eBPF might be the answer! This article will dive into why this Linux Kernel feature has become one of the top buzzwords for the cloud-native stack. We’ll also explore a popular off-the-shelf cloud solution that is powered by eBPF and examine how it can empower your cloud system security, observability, and monitoring. So let’s give that buzz a buzz!

![](https://cdn-images-1.medium.com/max/3840/1*sUNb0u0Jl9BgOHlv-owy0w.jpeg)

*If you are already familiar with eBPF and are interested in reading about Cilium/Dataplane V2, feel free to skip to [Part 2](https://blog.doit-intl.com/ebpf-cilium-dataplane-v2-and-all-that-buzz-part-2-8bcbee51720d)!*

[eBPF](https://ebpf.io/) belongs to the Linux Kernel space, a domain where most of us don’t hang out. So why should a person who is “up in the cloud” even care about such a low-level Linux Kernel feature? Well, the truth is that eBPF is a kind of a revolution. It has a massive impact on both low-level and high-level areas.

***For example, let’s say you want to write some custom logic that needs to closely inspect all network packets traveling between your containers and only allow HTTP packets with specific criteria to go through.***

It’s common knowledge that you should be in the kernel space if you are looking to merge, inject and limit network traffic. The Linux Kernel is the place to manipulate the inner working of the Linux OS and its massive networking layer, which is built for performance, security, and reliability.

Before eBPF hit the scene, you had to write Linux kernel code (in C programming language). That could be done by changing the kernel source code or by writing a [loadable kernel module](https://en.wikipedia.org/wiki/Loadable_kernel_module). Besides the inherent complexity, this introduced many risks because a kernel module is like a dynamic library (like a .DLL or an .so file) attached to the kernel.

A crash bug in your code could lead to kernel panic and bring down your entire OS. As a cloud user, or specifically someone who is using Kubernetes to deploy microservices, there are many other reasons to avoid writing custom kernel code: security, development velocity, complexity, or even environment infeasibility. It’s basically an overkill.

### Then Came eBPF 🐝

eBPF emerged and allowed you to inject logic into the kernel without touching the kernel code, in other words, from user-space. eBPF not only simplifies this process but also makes it a lot safer. The eBPF verification process makes sure that the eBPF code you load into your kernel is safe to execute by enforcing safety checks. For example, it enforces and makes sure that:

* Your code executes in a finite fashion (no indefinite loops)

* Your code does not crash or cause fatal bugs that can harm the system

* The process that is loading your eBPF code has the required privileges

* There is a size limit for your code

* Unreachable code is not allowed

In addition to the verification process, there are certain limits that imply to your code, such as eBPF forcing you to access certain kernel resources only by calling special eBPF helper functions. In short, eBPF gives you a safety net, ease of development, smooth deployment, and enhanced performance. Many tools and engines now take advantage of eBPF technology to bring new features for cloud-native workloads, in a transparent and performant way.
>  **If you want to take eBPF for a spin, check out [this video](https://www.youtube.com/watch?v=lrSExTfS-iQ).**

### Popular Projects Powered by eBPF 🔋

* [Cilium](https://cilium.io/) — eBPF-based Networking, Security and Observability for Kubernetes

* [Falco](https://falco.org/) — Cloud-Native Runtime Security

* [Tracee](https://github.com/aquasecurity/tracee) — Runtime Security and Forensics using eBPF

* [Pixie](https://pixielabs.ai/) — Application troubleshooting platform for Kubernetes (check it out!)

I hope you found this information useful. This [list](https://ebpf.io/projects) shows additional projects that are powered by eBPF. In [Part 2](https://blog.doit-intl.com/ebpf-cilium-dataplane-v2-and-all-that-buzz-part-2-8bcbee51720d), we’ll explore Cilium as a popular K8s eBPF solution and figure out what Dataplane V2 has to do with it.

