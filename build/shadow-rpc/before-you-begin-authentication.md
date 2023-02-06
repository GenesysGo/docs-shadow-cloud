---
description: How to authenticate to Premium RPC services
---

# Before You Begin - Authentication

### Ok before you freak out, we're going to give you code samples for you to use

We just want to make it abundantly clear that without setting up basic authentication, the RPCs will reject reuqests for security. An important part to making sure YOUR dedicated RPCs work best for you is that they _remain_ dedicated to your app. It would be a mega bummer if some (dare I say) Shadowy Super Coder were to find your RPC's endpoint and use it for their own and hog your precious resources.

SO. We built an authentication mechanism that will restrict usage of the RPC to your dApp. This is done by a proxy that you run (which we provide an example container solution for!) requests a JWT token. There are basically 2 phases to authentication and usage.

1.  Authenticate

    When you register for a Premium RPC, you will have to identify a wallet keypair that is eligible to request a token. You will send an authentication request including a signed message with the wallet keypair.\
    \
    The authentication response will return an authentication token - NOT the JWT token.
2. Using the authentication token, you will then be able to request a JWT token. Upon receiving a JWT token, you can then send all requests to your RPC endpoint using the standard Authorization Bearer \<token> header.

**The catch: The JWT expires every 24 hours. You will have to request it again before expiry or your entire world will come crashing down. It's ugly. We've seen it.**

### Here's a UML Sequence Diagram

![](<../.gitbook/assets/image (3) (2).png>)

See? It sounds worse than it really is. And like I said, you're gonna get code samples and so much more in the following pages.&#x20;
