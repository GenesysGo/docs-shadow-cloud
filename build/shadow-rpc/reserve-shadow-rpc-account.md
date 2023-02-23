# Reserve an RPC Account

![](<../../.gitbook/assets/Screen Shot 2022-10-03 at 7.37.35 PM.png>)

### Reserving any Shadow RPC service will require **USDC and SOL in your wallet.**

We begin by travelling to the reservation portal. Be ready to connect your wallet. [https://portal.genesysgo.net/premium/reserve](https://portal.genesysgo.net/premium/reserve)

**Note: when you connect your wallet for the first time, you also need to click on the wallet bubble again and click Sign In to sign a message.**

![](<../../.gitbook/assets/image (3) (3).png>)

As you can see in the screenshot above, or the site because you clicked the link, we have some options.
### **We can help you make a decision!**
<details><summary>Compare plans in greater detail</summary>

### RPCs Are Not One Size Fits All

Some devs just need a way into Solana to test some stuff out. Some devs higher RPC throughput for their dApp. And some devs need to have RPC instances in every region of the world in order to globally load balance their dApp because they have millions of transactions. We got all these situations covered.

### We offer Three Tiers of RPC

{% hint style="info" %}
Note, creating multiple RPC accounts increases your project's rate limits.

Prices below are for 30 day cycles (roughlky monthly)
{% endhint %}

#### Tier 0 - "I'm just a little curious how this works"

* Use: Low performance requirements, ideal for just exploring how Solana works and testing some new calls out
* **Cost = Free**
* 1x Request per second
  * This is monitored based on a combination of the following:
    * Your IP Address
    * UUID that Shadow generates
    * JWT token that is recycled every 24 hours
    * _**Try to spoof this and get the perma banhammer, scrubs**_
* 6x Requests/minute for getMultipleAccounts RPC call

{% hint style="danger" %}
**NOTE: IF YOU WANT TO RESERVE THE FREE TIER, YOU WILL STILL NEED A TINY AMOUNT OF USDC IN YOUR WALLET (even 0.0001) JUST TO HAVE THE ASSOCIATED TOKEN ACCOUNT CREATED.**
{% endhint %}

#### Tier 1 - "Ok, we're going to build a dApp and take it seriously this time"

* Use: A production app that is expected to have moderate, but consistent utilization. NOTE: the following figures are per node (yes, you can rent more than 1)
* **Cost = 325 USDC/mo - Paid directly to Shadow Operators**
* Unlimited transactions unless specified below
* (5 request/second for getProgramAccounts) x # of RPC instances reserved
* 100 Mbps bandwidth limit x # of RPC instances reserved

#### Tier 2 - "This thing got big, and now we have a lot of traffic from all over the world"

* Use: Quite simply, you have a large, steady flow of transactions because your dApp is something people like. NOTE: the following figures are per node (yes, you can rent more than 1)
* **Cost = 795 USDC/mo - Paid directly to Shadow Operators**
* Unlimited transactions unless specified below
* (10 requests/second getProgramAccounts) x # of RPC instances reserved
* 250 Mbps bandwidth limit

### burst - \ ˈbərst \\

_verb (used without object), burst or, often, burst·ed, burst·ing._

1. To go over your allotted requests/second for a given RPC call and still not have the RPC server fuss at you or reject your call
2. Burst refills once your requests/second falls below the standard limit

_Example used in a sentence_

* Our Tier 2 dApp bursted getMultipleAccounts calls to 200 requests/second for the first second it was live. The RPC server handled it fine. In the next second, it fell to 80 requests/second, which refilled our burst by 20 calls. If it stays at 80 requests/second, that will refill our burst by 20 requests/second up till the burst limit of 100 requests.

**What's up with rate limits on different call types? Why limit it at all?**

It comes down to how batching works, really. We don't limit batching starting in Tier 1, so 100 requests per second could easily scale up on our poor operators. Plus there are kinds of requests that can be super toxic in addition to block crawlers causing resource exhaustion.

Now, you no doubt have more questions about how this really works and how you can use an RPC (or multiple RPCs!) as an entrypoint into Solana from your dApp. That's why you need to spend some time reading the next page.
</details>

<details><summary>Design Considerations</summary>
---
description: Useful Design Ideas for using your RPC
---


### When choosing Premium vs Dedicated, there are a couple things we want you to consider...

1. You need to consider how many front ends you will have and how many RPCs you will have (need), and how traffic will be directed to the RPCs from your app. That's what this page is.
2. You need to be aware that access to your RPC will expire every 24 hours unless you refresh your access token. That's what the next page is all about.

### **First, let's think about your dApp design and how traffic is balanced.**

#### **Basic - 1 Front End web app and 1 Premium RPC**

****![](<../../.gitbook/assets/image (5).png>)****

There's not much to this design. Your end users from all over the world will connect to your website to submit a transaction. In your dApp, you define the [Connection](https://solana-labs.github.io/solana-web3.js/classes/Connection.html) - for endpoint param, you will simply provide it with our endpoint URL.

#### Ideal, but more complicated - Multiple RPCs to Provide GeoRedundancy and Load Balancing

![](<../../.gitbook/assets/image (1) (1) (1).png>)

In this scenario, your goal is to have traffic load balanced and also provide your end users with the lowest latency (and possibly faster confirmation times). There are a few ways to accomplish this.

Here, you would place a DNS-enabled load balancer in the front. There are many resources to do this, notably Azure Traffic Manager or AWS Route 53. You create an A record for your application that resolves to the DNS router. For the DNS router, you identify what your backend resources are.

As client traffic arrives on the DNS router, the DNS router inspects the source packet to determine what geography it is coming from, and then redirects the client to the nearest backend resource which would be your web servers, located in disparate regions.

Your webserver would then serve identical websites with one exception - the RPC connections would be different based on the closest Premium RPCs you have reserved.

#### For critical workloads, there is The Kennedy Package - Let us handle the plumbing.

![](<../../.gitbook/assets/image (3) (1).png>)

When you purchase services from GenesysGo, there are add-on services available. One of those is Global Server Load Balancing (GSLB). If you purchase RPC services in different global regions (ie, NA, EU, and APAC), you can add the add-on GSLB package and we will take care of the rest. There are two tiers of global load balancing offered (3 regions or 6 regions) which we will cover when it comes time to reserve the RPC.

Why do this? Because you need to focus on your app's code; not the internet plumbing. And this is reliable way to give your end users the best possible experience.

</details>

**First up is region**. An important decision. In an ideal world, you would know where your audience is and deploy your application in that region along with an RPC. But this is web3, and the whole point is to be international. Lucky for you, we have 6 options (and growing!) to start with

* NYC
* DFW
* Silicon Valley
* Amsterdam
* Frankfurt
* Hong Kong

Remember, you can reserve as many instances as you want.

**Next up is the plan.** You'll want to circle back to the [Our Offerings page](/build/shadow-rpc/README.md) and make sure you fully understand the differences between Basic, Premium, and Premium+. This impacts the resulting Stake Amount field but more on that to come.


{% hint style="danger" %}
**NOTE: IF YOU WANT TO RESERVE THE FREE TIER, YOU WILL STILL NEED A TINY AMOUNT OF USDC IN YOUR WALLET (even 0.0001) JUST TO HAVE THE ASSOCIATED TOKEN ACCOUNT CREATED.**
{% endhint %}

**Third on the list is an Account Name.** This is a clever name that you can set to organize your RPC accounts. This would be particularly handy for billing/accounting if you're sophisticated enough to think that far in advance.

**Lastly is the Stake Amount field.** Let's take a moment to wrap our heads around how billing actually works. For a single Premium+ RPC, the cost is $795/ every 30 days (or ~1 month). When you first reserve a Premium+ instance, the $795 is immediately paid to the operator.

What's interesting is this also opens an account where you can prepay any amount of monthly fees that you want. Note that the monthly payment needs to be _deposited before the expiration_ that is shown in the [My RPCs page.](my-rpcs.md)

**Create Account** submits the transactions needed to reserve the RPC. **IMPORTANT:** each RPC account has a list of authorized Public Keys. The Pubkeys added to each account via the portal can be used to sign your authentication attempts to issue your JWT token. You will need the keypair of the authorized Pubkey in a .json format (but don't worry - we've got you covered on how to do that, too).
