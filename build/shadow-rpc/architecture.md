---
description: Useful Design Ideas for using your RPC
---

# Before You Begin - Designs

### Before you get started, there are a couple things we want you to consider...

1. You need to consider how many front ends you will have and how many RPCs you will have (need), and how traffic will be directed to the RPCs from your app. That's what this page is.
2. You need to be aware that access to your RPC will expire every 24 hours unless you refresh your access token. That's what the next page is all about.

### **First, let's think about your dApp design and how traffic is balanced.**

#### **Basic - 1 Front End web app and 1 Premium RPC**

****![](<../.gitbook/assets/image (5).png>)****

There's not much to this design. Your end users from all over the world will connect to your website to submit a transaction. In your dApp, you define the [Connection](https://solana-labs.github.io/solana-web3.js/classes/Connection.html) - for endpoint param, you will simply provide it with our endpoint URL.

#### Ideal, but more complicated - Multiple RPCs to Provide GeoRedundancy and Load Balancing

![](<../.gitbook/assets/image (1) (1) (1).png>)

In this scenario, your goal is to have traffic load balanced and also provide your end users with the lowest latency (and possibly faster confirmation times). There are a few ways to accomplish this.

Here, you would place a DNS-enabled load balancer in the front. There are many resources to do this, notably Azure Traffic Manager or AWS Route 53. You create an A record for your application that resolves to the DNS router. For the DNS router, you identify what your backend resources are.

As client traffic arrives on the DNS router, the DNS router inspects the source packet to determine what geography it is coming from, and then redirects the client to the nearest backend resource which would be your web servers, located in disparate regions.

Your webserver would then serve identical websites with one exception - the RPC connections would be different based on the closest Premium RPCs you have reserved.&#x20;

#### For critical workloads, there is The Kennedy Package - Let us handle the plumbing.

![](<../.gitbook/assets/image (3) (1).png>)

When you purchase services from GenesysGo, there are add-on services available. One of those is Global Server Load Balancing (GSLB). If you purchase RPC services in different global regions (ie, NA, EU, and APAC), you can add the add-on GSLB package and we will take care of the rest. There are two tiers of global load balancing offered (3 regions or 6 regions) which we will cover when it comes time to reserve the RPC.

Why do this? Because you need to focus on your app's code; not the internet plumbing. And this is reliable way to give your end users the best possible experience.

