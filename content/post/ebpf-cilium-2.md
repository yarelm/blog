+++
title = "eBPF, Cilium, Dataplane V2 and All That Buzz (Part 2)"
description = "Let's explore Cilium and Dataplane V2!"
date = 2021-10-28T02:14:50Z
author = "Yarel Maman"
+++

I hope you enjoyed reading about eBPF in [Part 1](https://www.yarelm.com/ebpf-cilium/
)! Now let’s examine Cilium as a popular K8s eBPF solution and find out how it relates to Dataplane V2.

[Cilium](https://cilium.io/) 🐝 is a “hot” technology that’s powered by eBPF. It’s often the first thing mentioned when eBPF comes up, more so in the context of K8s. Cilium is basically open-source software that acts as a [CNI plugin](https://github.com/containernetworking/cni) for Kubernetes. It provides eBPF-based networking, observability, and security with optimal scale and performance for platform teams operating Kubernetes environments on the cloud and on-premise.

![](https://cdn-images-1.medium.com/max/3840/1*S-VljUlIBFiKR30CBDA78A.jpeg)
>  **Taking advantage of Extended Berkeley Packet Filter (eBPF), Cilium brings a number of interesting features to K8s. Let’s take a closer look.**

![](https://cdn-images-1.medium.com/max/2000/0*5zwTvbuTbozpDrZV.png)

### Network Policies 🕸

It’s a good practice to implement Least Privilege security when it comes to your K8s pods communicating with each other. The basic [K8s Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/) (operate at L3/L4) do a good job, but you can build upon them with [Cilium Network Policies](https://docs.cilium.io/en/v1.10/policy/) (operate at L3-L7).

This can be very useful in the world of K8s and microservices because examining and controlling the network traffic with metadata (e.g., IPs and ports) doesn’t provide much value. IPs and ports change all the time as services come and go. **With Cilium, you can also control the traffic with Pod, HTTP, gRPC, Kafka, DNS, and other metadata.**

For example, you can define HTTP rules that allow a specific API call to be made from a certain pod via path, header, and request methods. Another example is defining DNS rules based on FQDN, so that only queries to a specific domain will be allowed. That allows us to define security policies that are more valuable and usable in our real-world use-cases.

### Multi-Cluster Connectivity 🔗

By using a [cluster mesh](https://cilium.io/blog/2019/03/12/clustermesh), Cilium allows K8s pods to communicate and be discovered across K8s clusters. Use cases include High Availability and [Multi-Cloud](https://www.youtube.com/watch?v=U34lQ8KbQow) (connecting K8s clusters across cloud providers).

### Load Balancing ⚖️

Cilium replaces kube-proxy for BPF.kube-proxy uses iptables which [is being replaced for BPF](https://cilium.io/blog/2018/04/17/why-is-the-kernel-community-replacing-iptables). This change dramatically improves performance.

### Additional Features

* [Transparent Encryption](https://docs.cilium.io/en/stable/gettingstarted/encryption/) between pods. IPsec/Wireguard support

* [Increased Network Performance](https://cilium.io/blog/2021/05/11/cni-benchmark)

* [High Infra Scalability](https://cilium.io/blog/2019/04/24/cilium-15)

* [Enriched visibility of traffic flows](https://cilium.io/blog/2019/11/19/announcing-hubble), not only by IPs and ports but also by L7 protocols. Check out [this blog](https://cilium.io/blog/2019/12/18/how-to-debug-dns-issues-in-k8s) on an interesting DNS debugging session

* [Monitoring](https://cilium.io/blog/2019/11/19/announcing-hubble#metrics--monitoring) and improved visibility to network errors between your microservices. Cilium provides Prometheus-compatible metrics
>  **A few words on the alternatives. There are other capable CNI plugins out there in the market. Here’s a [comparison](https://kubedex.com/kubernetes-network-plugins/) (might be opinionated). Calico has also recently [introduced](https://docs.projectcalico.org/maintenance/enabling-bpf) an eBPF Dataplane.**

## Dataplane V2 ✈

Google Cloud Platform is keeping GKE (Google Kubernetes Engine) in the loop by leveraging Cilium into their own mechanism, [Dataplane V2](https://cloud.google.com/blog/products/containers-kubernetes/bringing-ebpf-and-cilium-to-google-kubernetes-engine). But is Dataplane V2 really a Google-managed Cilium for GKE? We did love our managed services, right? This calls for a close inspection.

Upon inspecting Google’s [documentation of Dataplane V2 concepts](https://cloud.google.com/kubernetes-engine/docs/concepts/dataplane-v2), there is no indication or reference to the Cilium project (at the time of writing this blog post). However, in the [official blog post](https://cloud.google.com/blog/products/containers-kubernetes/bringing-ebpf-and-cilium-to-google-kubernetes-engine) and in [some documentation](https://cloud.google.com/kubernetes-engine/docs/how-to/dataplane-v2), there are *some *minor references.

Dataplane V2 control plane is deployed as a K8s DaemonSet called anetd. A quick kubectl describe daemonsets.apps -n kube-system anetd reveals that it is using the image gke.gcr.io/cilium/cilium:v1.9.4-gke.17.

So, is that really Cilium? Let’s run kubectl exec -n kube-system -ti ds/anetd — cilium version . Here is the output:

    Client: 1.9.4 609a63dfb 2021-04-12T15:01:54-07:00 go version go1.15.7 linux/amd64

Yes! It’s indeed Cilium 1.9.4 ! However, upon comparing it with the official Cilium v1.9.4 image, we get a slightly different result:

    Client: 1.9.4 **07b62884c** **2021-02-03T11:45:44-08:00** go version go1.15.7 linux/amd64

Now let’s compare the Docker images gke.gcr.io/cilium/cilium:v1.9.4-gke.17 and quay.io/cilium/cilium:v1.9.4 with a tool like [Dive](https://github.com/wagoodman/dive). It seems like there are some changes in the layers, but it’s hard for me to tell if there is any major logic change between them when it comes to Cilium’s offerings.
>  **It’s also worth mentioning that in this [blog post](https://cilium.io/blog/2020/08/19/google-chooses-cilium-for-gke-networking), it’s claimed that Google stepped in and contributed a number of meaningful features to the Cilium project. I think that shows a certain degree of commitment.**

### So, Dataplane V2 = Managed Cilium?

So far, **I cannot conclude** whether Dataplane V2 is a managed Cilium for GKE. **And without this official conclusion, we can say that at least as a product, Dataplane V2 ≠ Cilium.** It looks like Cilium is being used under the hood or behind the scenes. Google’s docs simply don’t say or guide you to Cillium’s docs. It’s a completely different offering.

From the testing I have done, some Cilium features seem to work on Dataplane V2. However, there’s no official Google support. Needless to say, “uncharted” Cilium features on Dataplane V2 could work today, but may break unexpectedly at any given time. Hence, it’s best we don’t enter uncharted waters. Just follow the official docs to be on the safe side.

## Vanilla Cilium or Dataplane V2? 🤔

Here’s a feature comparison:

{{< gist yarelm a9e9b5ab51ee2b8a79b8c16acb4444ba >}}

The common features of both Cillium and Dataplane V2 currently are:

* [K8s Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/) (**not CiliumNetworkPolicy**, although Dataplane V2 doesn’t seem to reject it at the moment),

* Replacing kube-proxy with eBPF,

* [Network Policy Logging](https://cloud.google.com/kubernetes-engine/docs/how-to/network-policy-logging). It’s not really a Cilium feature, but it is based on Cilium. It allows you to monitor the outcome of your network policies hits.

### My Opinion 💭

I always try to opt for the simplest solution possible. If it’s managed, that’s great! It saves precious time for all sides involved. Dataplane V2 seems like a simpler and easier **managed** solution if all you need is to leverage K8s Network Policies, a kube-proxy eBPF replacement for performance and scale, and the easy-to-use logging of the network policies outcome.

Just make sure you are aware of its [limitations](https://cloud.google.com/kubernetes-engine/docs/concepts/dataplane-v2#limitations). If you need the additional features from Cilium such as [Hubble](https://docs.cilium.io/en/v1.9/gettingstarted/hubble/), [Cilium L7 Network Policies](https://docs.cilium.io/en/v1.9/policy/language/#layer-7-examples), [Cluster Mesh](https://cilium.io/blog/2019/03/12/clustermesh) or using a self-hosted/different cloud provider, you’ll probably want to go with vanilla Cilium instead.

### Dataplane V2: The Pros 👍

* Firstly, it’s [easy to install](https://cloud.google.com/kubernetes-engine/docs/how-to/dataplane-v2#create-cluster). Just add the —enable-dataplane-v2flag when creating a new GKE cluster via gcloud container clusters create .

* It is based on the open-source Cilium project.

* Dataplane V2 acts as the foundation for the [Network Policy Logging](https://cloud.google.com/kubernetes-engine/docs/how-to/network-policy-logging) in GKE. This is a nifty feature that creates logs when a connection is allowed or denied by a network policy.

* The anted plugin as part of Dataplane V2 (based on Cilium) is managed by Google and is currently in [GA](https://cloud.google.com/products#product-launch-stages) (General Availability). That means it is ready for production workloads, with ongoing updates and support.

* It’s reasonable to assume Google will add more integration with GKE native features, perhaps with Cilium native features as well. This makes the choice more compelling if you are looking at the long-term.

### Dataplane V2: The Cons 👎

* As of this moment, I couldn’t find a way to use Hubble with Dataplane V2. [Hubble](https://docs.cilium.io/en/v1.10/intro/#what-is-hubble) is a very nice observability tool by Cilium that can provide important visibility by utilizing Cilium eBPF. You can track it [here](https://github.com/cilium/cilium/issues/15035).

* Officially, Dataplane V2 is not a managed Cilium solution. This means you can’t rely on some Cilium features that are working for you now with Dataplane V2. You may be looking at potential breaks going ahead.

* You can only enable it on a newly-created GKE cluster. This means you can’t use it on your existing GKE clusters.

* There are some other [limitations](https://cloud.google.com/kubernetes-engine/docs/concepts/dataplane-v2#limitations) you should be aware of.

## Getting Started 🏃🏽‍♀️

To get started with Cilium on K8 — [Click here](https://docs.cilium.io/en/stable/gettingstarted/k8s-install-default/).

To get started with Dataplane V2 on GKE — [Click here](https://cloud.google.com/kubernetes-engine/docs/how-to/dataplane-v2).
>  **Pro Tip: When you are writing or planning your K8s/Cilium Network Policy manifests, use the [Cilium Editor](https://editor.cilium.io/) for a fun and safe experience.**

## The Future of eBPF

Groundbreaking as this technology is, I can predict we will continue seeing many more solutions and interesting developments using eBPF.

One such potential area of influence is the service mesh world. Most of the existing service mesh solutions (e.g., Istio, Linkerd) rely on sidecar proxies attached to your pods. This impacts performance, adds complexity, and introduces additional points of failure. eBPF has the potential to provide service mesh capabilities by replacing the sidecar proxies with eBPF logic, potentially making service mesh accessible for additional use cases.

Stay tuned!
