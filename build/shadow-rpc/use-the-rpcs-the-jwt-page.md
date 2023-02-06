---
description: Let's review a step by step walkthrough (including examples) on using the Shadow RPC
---

# Use the RPCs (the JWT Page)

## Let's Get the Party Started

You read the [Before You Begin - Authentication](before-you-begin-authentication.md) page, correct?

No?

![](../.gitbook/assets/giphy.gif)

Go read it.&#x20;

Ok, welcome back. See? It was worth your time wasn't it? Now that you've wrapped your head around the architecture, we can explore how to actually make this happen in code.

### Here is a Sample Docker Container that you can either use or just explore the code. Let's break it down.

👉 [https://github.com/GenesysGo/premium-rpc-nodejs-example](https://github.com/GenesysGo/premium-rpc-nodejs-example)

The basics of this container are:

* If you control the underlying infra that your app runs on, you can deploy this container alongside it.&#x20;
* Once you have updated the .env file with the correct params, this container will retrieve your JWT token every 5 minutes.
* You can then send RPC requests to the container, which listens on an /rpc endpoint (ie, https://1.2.3.4/rpc or http://localhost/rpc). The container will add the correct JWT token to the request and forward it to your Premium RPC endpoint. It serves as a proxy\
  dApp ---> Container ---> Premium RPC
* Note this does NOT need to run on the same box as your web app. This can be a standalone container that serves as a proxy for your RPC handling.&#x20;

### Here's another community created sample script that you can borrow some code from to test out your RPC

{% embed url="https://github.com/CodeableAS/simple-guide-to-genesysgo-premium-rpc" %}

### Let's dig into the code in case you need to Do It Yourself

Chances are pretty darn high that you don't have total control over your box and you want to handle the JWT issuance on your own. You can borrow some of the code used in the container if you so choose. NOTE - I am NOT going to focus on the entire code sample in the expandable snippet below. I am just going to focus in on handling the JWT. Let's explore:

<details>

<summary>Full Code Snippet</summary>



```javascript
import dotenv from "dotenv";
dotenv.config();
import * as solana from "@solana/web3.js";
import nacl from "tweetnacl";
import bs58 from "bs58";
import { createClient } from "redis";
import fetch from "node-fetch";

async function main() {
  console.log("Starting...");
  const client = createClient({
    url: "redis://redis",
    password: process.env.REDIS_PASSWORD,
  });
  await client.connect();
  console.log("Connected to redis...");
  // Get wallet from local keypair file a keypair variable passed in
  // const key = JSON.parse(
  //   fs.readFileSync("/path/to/your/wallet/keypair/file/id.json").toString()
  // );
  const key = JSON.parse(process.env.WALLET_KEY as string);
  const wallet = solana.Keypair.fromSecretKey(new Uint8Array(key));
  console.log("Read wallet ", wallet.publicKey.toString());
  // Build and sign message
  const msg = new TextEncoder().encode(`Sign in to GenesysGo Shadow Platform.`);
  const message = bs58.encode(nacl.sign.detached(msg, wallet.secretKey));
  // Send auth request with wallet pubkey and signed message payload
  console.log("Sending auth request", process.env.API_URL_BASE);
  let body = {
    message,
    signer: wallet.publicKey.toString(),
  };
  console.log({ body });
  const authRequest = await fetch(`${process.env.API_URL_BASE}/signin`, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(body),
  });
  console.log("Validating auth request");
  // Validate that the request is ok
  if (!authRequest.ok || authRequest.status !== 200) {
    console.error("Error occurred:", authRequest.status);
    return;
  }
  // Convert response into json
  const authResponse = await authRequest.json();
  // Validate that a token is in the response body
  if (typeof authResponse?.token !== "string") {
    console.log("No valid auth token returned.");
    return;
  }
  console.log(authResponse);
  // Get JWT Auth Token
  // const token = authResponse.token;
  const tokenRequest = await fetch(
    `${process.env.API_URL_BASE}/premium/token/${process.env.RPC_ID}`,
    {
      method: "POST",
      headers: {
        Authorization: `Bearer ${authResponse.token}`,
      },
    }
  );
  if (!tokenRequest.ok || tokenRequest.status !== 200) {
    console.error("Error occurred:", tokenRequest.status);
    return;
  }
  // Convert response to json
  const tokenResponse = await tokenRequest.json();
  if (typeof tokenResponse?.token !== "string") {
    console.log("No valid jwt token returned");
    return;
  }
  // Send token to Redis
  client.set("RPC_TOKEN", tokenResponse.token);
  console.log("Set RPC_TOKEN to", tokenResponse.token);
}

main();
// Update token every 5 minutes
// In production, you should only have to refresh this token every 24 hours.
const INTERVAL = 300_000;
setInterval(() => main(), INTERVAL);
```

</details>

**First up are the dependencies.**

I'm NOT going to focus on all of the dependencies used in this code. I'm just going to focus on what would be needed to handle a JWT request.

```javascript
import dotenv from "dotenv";
dotenv.config();
import * as solana from "@solana/web3.js";
import nacl from "tweetnacl";
import bs58 from "bs58";
```

For the most part, there are a few important libraries here. We have dotenv primarily because we want to separate secrets and other variables needed into environment variables. We use web3.js because... well, Solana. And we use nacl and bs58 to handle the encoding of our messages.

**Next, we'll get ahold of your keypair.**

**The code needs to use a keypair to sign the encoded message.** If you don't have your keypair in a .json file, you can export your secret key from your wallet provider. If you are using Solflare, the export secret key button will actually export the JSON array (ie, \[1,2,3,4,...]) that you need. Just save it as a .json file and you're set. Something like Phantom will need a little extra work to take their secret key and turn it into a JSON file, and [we have the tool for exactly that.](https://gist.github.com/tracy-codes/f17e7ed8acfdd1be442f632f5b80763c)

You can either store your keypair in a .json file on your dApp, or you can add it to the .env file and access it through an environment variable (like what is done in the docker container code).

```javascript
// Get wallet from local keypair file
// const key = JSON.parse(
//   fs.readFileSync("/path/to/your/wallet/keypair/file/id.json").toString()
// );
// OR
// Get it from environment variable called WALLET_KEY
  const key = JSON.parse(process.env.WALLET_KEY as string);
  const wallet = solana.Keypair.fromSecretKey(new Uint8Array(key));
```

**With our wallet in hand, we can now begin to build our message in order to sign in**

The way this works is we will build an encoded message that simply reads "Sign in to GenesysGo Shadow Platform." No, really. That's what the message is. That's the requirement.

The first line in the below snippet encodes the text message. The real magic happens on the second line. The nacl.sign.detached() method signs the message that we just created with our wallet's private key. The resulting signed object is THEN base58 encoded by the bs58.encode() method.

So, we have an encoded plain-text message, which gets signed by our wallet key, which gets base58 encoded. That base58 encoded message is one of the values we have to provide in our authentication request's body. The other value is our wallet's public key.

The final step is to construct the payload for the auth request. In my opinion, the hardest part is over at this point.&#x20;

```javascript
const msg = new TextEncoder().encode(`Sign in to GenesysGo Shadow Platform.`);
const message = bs58.encode(nacl.sign.detached(msg, wallet.secretKey));
let body = {
  message,
  signer: wallet.publicKey.toString(),
};
```

**Now we can get signed in.**

This part is pretty straightforward. We will make a POST request to the /signin endpoint, then validate that the response was successful as well as contains an authentication token. Recall that the authentication token is what is used as a Bearer token in the JWT request.

```javascript
const authRequest = await fetch("https://portal.genesysgo.net/api/signin", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(body),
  });

  // Validate that the request is ok
  if (!authRequest.ok || authRequest.status !== 200) {
    console.error("Error occurred:", authRequest.status);
    return;
  }
  
  // Convert response into json
  const authResponse = await authRequest.json();
  
  // Validate that a token is in the response body
  if (typeof authResponse?.token !== "string") {
    console.log("No valid auth token returned.");
    return;
  }

```

**And Upon Sign-in, we can request our JWT**&#x20;

Another pretty straightforward step. We simply make our post request to the correct JWT endpoint. Note - you will need to get your account ID from the My RPCs portal in order to complete the URL. This can also be handled in environment variables as it is done in the docker container under the name "RPC\_ID".

Note the authorization in the POST request - it is using the authResponse.token object from the previous step as a Bearer token.

```javascript
  const tokenRequest = await fetch(
    "https://portal.genesysgo.net/api/premium/token/<ACCOUNT_ID_HERE>",
    {
      method: "POST",
      headers: {
        Authorization: `Bearer ${authResponse.token}`,
      },
    }
  );
  
  //confirm succeeded
  if (!tokenRequest.ok || tokenRequest.status !== 200) {
    console.error("Error occurred:", tokenRequest.status);
    return;
  }
  
  // Convert response to json
  const tokenResponse = await tokenRequest.json();
  
  //confirm a valid token exists
  if (typeof tokenResponse?.token !== "string") {
    console.log("No valid jwt token returned");
    return;
  }
  
  //store JWT in a var or wherever else you want
  const JWT = tokenResponse.token
```

**With your JWT in hand, you can now make RPC requests. Try it out!**

```javascript
const payload = {
	"jsonrpc": "2.0",
	"id": 1,
	"method": "getBlockHeight"
}

const blockHeight = await fetch(
    "https://portal.genesysgo.net/<ACCOUNT_ID_HERE>",
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${JWT}`,
      },
      body: JSON.stringify(payload)
    });
```

**NOTE:** A final thing we'll add here is that the web3.js Connection class object also supports headers in the form of a ConnectionConfig as the second param. Here is an example

```javascript
const conn = new Connection('https://us-east-1.genesysgo.net/xxxxx', {"commitment": "confirmed", "httpHeaders": {"Authorization": `Bearer ${jwt}`,
        "Content-Type": "application/json"}});
```
