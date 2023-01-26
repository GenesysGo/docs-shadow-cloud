# Support and FAQ

If you've got questions, we've got answers. Probably.

**👉Where can I go to reach out for technical assistance?**

Our discord server is the best place to get in touch with us. [https://discord.gg/p7GzDfzyU3](https://discord.gg/p7GzDfzyU3)\
We have a dedicated support section.

**👉How many RPCs can I reserve?**

As many as you can afford.

**👉Can I reserve RPCs in different tiers?**

You sure can. You can reserve a Premium+ in NYC and a Premium in Amsterdam if you choose.

**👉When I reserve multiple RPCs, are my resources and limits pooled?**

No, they are per RPC|per reservation. If you reserve a Premium+ in NYC and a Premium in Amsterdam, the limits of each tier apply to the corresponding RPC.

**👉Will you add more regions later?**

You can count on it.

**👉When do I need to deposit USDC into my account wallet?**

Before the current billing cycle ends.&#x20;

**👉Can I withdraw USDC from the account wallet?**

Not at this time.

**👉How do I know the full USDC amount is going directly to Shadow RPC Operators?**

It is all governed by smart contract. You can simply follow the money on-chain.

**👉How do I pay for add-ons like Global Server Load Balancing?**

Payments for add-ons are debited from the account wallet. Just top up the account wallet before purchasing add-ons.&#x20;

**👉Can I get an Unlimited Package?**

We currently enforce rate and data limits to protect the shadow operators, but still believe we are providing the best possible experience. These rate limits are also subject to change with a likelihood of scaling up our limits as more compute becomes available.

**👉Are you monitoring the health of the RPCs that Shadow Operators provide?**

Sure are. We have an in-house monitoring protocol built that tracks status, health, response times, and delinquency (among other things). If a node is below our health threshold for a given amount of time, it is automatically removed from the Shadow network, and it's USDC emissions are slashed.&#x20;

👉**What is a good transaction timeout threshold (ie, what is a good retry timer)?**

No less than 60s with 120s being preferred. Here is an additional resource that can help you [https://solanacookbook.com/guides/retrying-transactions.html#facts](https://solanacookbook.com/guides/retrying-transactions.html#facts)

👉**How can I tell if the RPC is failing or if the transaction itself is failing?**

There's a lot at play here. Let's take a look at some common response codes.

* If you receive a 200 OK response, that means the RPC accepted the call, and anything that happens after that is up to Toly and Raj.&#x20;
* If you get a 500 ERROR response, there's a decent chance something is wrong with the validator.
* 400 - chances are your payload is busted here. Something is wrong with your request
* 403 - Chances are you have a request that violates your payment tier (i.e., batching requests with a Basic tier 0 account), but it could also just be a busted payload, or you are calling a deprecated RPC.
* 429 - You have exceeded the limits of your tier. Consider either scaling your tier up, or scaling your resources out by adding more RPCs and global load balancing.

👉**Do I HAVE to use the JWT?**

Yes.

👉**What if I don't want to?**

Too bad.

👉**Seriously bro?**

Yah brah.

We gave you the sample code to drop in as well as a Docker container that handles it all for you. Just hop right back over [to the JWT page](use-the-rpcs-the-jwt-page.md) and copy pasta. If you struggle with any of it, just drop into Discord and ask for help.&#x20;

**👉There is no mention about `getTokenLargestAccounts`, does this mean they are included in the base RPS limit ?**

gTLA is included

**👉For the `getMultipleAccounts` there is a max limit of 100 accounts, is that honored or is it lower ?**

100 account max is honored

**👉How are websocket subscriptions counted ?**

1 request for the POST call asking for 'connection: upgrade'

**👉Is there a penalty for the paid tiers when limits are exceeded (besides the 429 throttling) ?**

There are not

__

