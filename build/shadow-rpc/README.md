---
description: This section provides you with options and guides on getting everything quickly set up and secured to access the Solana blockchain.
---

# Service Options

|[Free](reserve-shadow-rpc-account.md) |[Premium](reserve-shadow-rpc-account.md) |[Dedicated](reserve-shadow-rpc-account.md)|
|:---:|:---:|:---:|
|**$0**|**$39.99**<br>every 30 days</br>|**$325**<br>every 30 days</br>|
|&check; No credit caps<br>&check; Global Routing</br>&check; Transaction Archive |&check; No credit caps<br>&check; Global Routing</br>&check; Transaction Archival<br>&check; Auth security|&check; No credit caps<br>&check; Regional Routing</br>&check; Transaction Archival</br>&check; Auth Security</br>&check; High Compute<br>&check; High Data Cap   |
|1 req / sec  |50 req / sec|Unlimited   |
|[Sign Up]()    |[Sign up]()    |[Sign up]()    |
|<details><summary>Details & Limits</summary>gPA = not avail.<br>gMA = 6 RPM</br>Txn History = 1 RPS<br>sendTxn = 1 RPS</br></details>   |<details><summary>Details & Limits</summary>gPA = 5 RPS<br>gMA = 25 RPS</br>Txn History = 10 RPS<br>sendTxn = 5 RPS</br></details>   </details>   |<details><summary>Details & Limits</summary>gPA = 10 RPS<br>gMA = 100 RPS</br>Txn History = 40 RPS<br>sendTxn = 50 RPS</br></details> 

## We can help you make a decision:
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