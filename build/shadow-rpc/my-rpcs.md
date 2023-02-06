# My RPCs

### Cool! You've got an RPC reserved! Now what?

The answer lies within the My RPCs page. The My RPCs page is arranged into three sections:

* Active RPCs - this will show you all of your connection details for each RPC broken down by the account name you selected during Reserve an RPC
* Add-Ons - as the name implies, a place where you can add on some nifty features to your purchase
* Metrics - a spot where you can track your RPC usage

### Active RPCs

![](<../.gitbook/assets/Screen Shot 2022-10-03 at 3.50.13 PM.png>)

Active RPCs is going to be the place where you spend the most amount of time. Some key fields to pay attention to:

**Expiration (aka Next Billing Date) - **_****_ this field indicates the date that your account balance needs to be topped up BEFORE.

**Increase Balance -** in order to top up your account, you need to first type in the amount you want to increase to, and then click the "+" to submit the transaction to your wallet.

**Account ID** - this will be used in two places. _First_, it is used to retrieve your JWT. _Second_, this is the endpoint for your RPC which you will find in the field called...

**RPC URL** - this is where your RPC is located. This is where you tell your code to send it's read requests or transactions to.

**Token -** you can request a JWT directly from the portal, right here.

### Add-Ons

![](<../.gitbook/assets/Screen Shot 2022-10-03 at 3.52.29 PM.png>)

There are two options on the add-ons page currently (but more to come!).&#x20;

**Global Server Load Balancing -** This is eligible for Premium and Premium+ subscribers with multiple RPCs. This solution is ideal when the client-side of your dApp handles the JWT, and a client sends the request in directly to the endpoint. With just a simple click, we will route client traffic to the closest RPC as traffic is originated from your dApp. There are 2 tiers for load balancing to choose from. **Prices are PER NODE that traffic is being load balanced across. Payment is debited from the wallet account balance under Active RPCs tab.**

* Tier 1 - Load balance across 3 RPC servers in different regions (example: NA, EU, APAC). Note this requires no more, no less than 3 Premium reservations.
* Tier 2 - Load balance across 6 RPC servers in different regions (every region). Note this requires no more, no less than 6 Premium reservations.

**Account Vanity URL** - _important: does not work if GSLB option has been selected._ This allows you to create a 7-character subdomain within the genesysgo.net domain for your RPC. Example: I could reserve https://knoxrpc.genesysgo.net&#x20;

Click in the field and just start typing the subdomain that you would like to reserve. But seriously - 7 characters. Make em count.

#### Metrics

![](<../.gitbook/assets/Screen Shot 2022-10-03 at 7.33.23 PM.png>)

A very straightforward dashboard where you can see your most used and most recent RPC calls.&#x20;

**Response Time**- time in milliseconds that elapsed to respond to your request

**Response payload** - size in bytes of the RPC response payload

**Requests** - the number of times that the RPC request has arrived on the node since first seen

_**The last time the RPC was seen**_
