# Reserve an RPC Account

![](<../.gitbook/assets/Screen Shot 2022-10-03 at 7.37.35 PM.png>)

### So you're ready to get started?

I'm happy to hear that. Even though I can't actually hear that. Maybe you can tweet at us and let us know you're getting started? Anyways.

We begin by travelling to the reservation portal. Be ready to connect your wallet. [https://portal.genesysgo.net/premium/reserve](https://portal.genesysgo.net/premium/reserve)

**Note: when you connect your wallet for the first time, you also need to click on the wallet bubble again and click Sign In to sign a message.**

![](<../.gitbook/assets/image (3) (3).png>)

As you can see in the screenshot above, or the site because you clicked the link, we have some options.

**First up is region**. An important decision. In an ideal world, you would know where your audience is and deploy your application in that region along with an RPC. But this is web3, and the whole point is to be international. Lucky for you, we have 6 options (and growing!) to start with

* NYC
* DFW
* Silicon Valley
* Amsterdam
* Frankfurt
* Hong Kong

Remember, you can reserve as many instances as you want.

**Next up is the plan.** You'll want to circle back to the [Our Offerings page](our-offerings.md) and make sure you fully understand the differences between Basic, Premium, and Premium+. This impacts the resulting Stake Amount field but more on that to come.&#x20;

{% hint style="danger" %}
**NOTE: IF YOU WANT TO RESERVE THE FREE TIER, YOU WILL STILL NEED A TINY AMOUNT OF USDC IN YOUR WALLET (even 0.0001) JUST TO HAVE THE ASSOCIATED TOKEN ACCOUNT CREATED.**
{% endhint %}

**Third on the list is an Account Name.** This is a clever name that you can set to organize your RPC accounts. This would be particularly handy for billing/accounting if you're sophisticated enough to think that far in advance.

**Lastly is the Stake Amount field.** Let's take a moment to wrap our heads around how billing actually works. For a single Premium+ RPC, the cost is $795/month. When you first reserve a Premium+ instance, the $795 is immediately paid to the operator.

What's interesting is this also opens an account where you can prepay any amount of monthly fees that you want. Note that the monthly payment needs to be _deposited before the expiration_ that is shown in the [My RPCs page.](my-rpcs.md)

**Create Account** submits the transactions needed to reserve the RPC. **IMPORTANT:** each RPC account has a list of authorized Public Keys. The Pubkeys added to each account via the portal can be used to sign your authentication attempts to issue your JWT token. You will need the keypair of the authorized Pubkey in a .json format (but don't worry - we've got you covered on how to do that, too).
