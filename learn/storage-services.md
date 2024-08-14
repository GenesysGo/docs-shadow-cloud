---
description: A resource for those exploring GenesysGo's shdwDrive as a storage solution.
---

# Storage

The shdwDrive is a new approach to S3-compatible cloud storage with the enhancements of on-chain storage. We have completed full integration with the Solana blockchain, while retaining the ability to become a poly-chain service in the future. For this reason we define the combination of shdwDrive and its evolving [D.A.G.G.E.R. ](dagger.md)technology as a data-availability layer for all of web2 and web3. Other web3 storage providers have attempted to integrate with Solana in the past and have had only marginal success. shdwDrive solves this by integrating directly with the Proof of History consensus mechanism, passing on-chain events for consensus approval by the Solana validator network which prove the continued existence and integrity of the stored data.

The idea for shdwDrive was founded on the premise that the Solana network is largely misunderstood in terms of what it was built to achieve. By understanding what it truly means to be the world’s most performant state machine, you realize that Solana can be used to reach consensus and maintain state on nearly anything.

Whereas other decentralized storage providers require multiple layers in order to make their solution viable (i.e. Arweave does storage, but to scale uploads faster you need to use Bundlr for example, and then in order to serve the data in any kind of a useful way you need to subscribe to a front-end UI for basic high-speed CDN functionality), shdwDrive builds everything into one platform.

When a user commits their [$SHDW](https://docs.shadow.cloud/reference/shdw-token) to shdwDrive for storage, they are not only provided with storage space, but also with incredibly fast batched uploads and a robust CDN. This ensures their content is stored quickly, efficiently, and delivered at speeds that makes shdwDrive a truly viable solution for projects that need speed, stability, and efficiency.

The following pages are intended to outline what shdwDrive is, how it works, and why it works. These pages will do so by explaining things in practical terms as our goal is for a wide range of people to fully understand what is being built.

## **Why use shdwDrive?**

If you're a developer and are wondering why you should use shdwDrive, here's a comprehensive list of reasons and strengths that cater to a wide range of applications:

1. **User choice!**
   * shdwDrive is built to empower users to decide what should happen with their data. Immutable or mutable data, you choose. We don't take that choice away from you.
2. **GDPR Compliance!**
   * This is a big one... developers are given all the tools they need to comply with GDPR and can show records that prove that they have deleted a user's personal data. Legal grey areas and loopholes are a risk to your business, shdwDrive helps you stay safe. For example, in the case of a project or business needing to produce records to prove GDPR compliance with a user's request to delete personal data... all the records to prove GDPR compliance live on-chain and have been trustlessly verified by the Solana validator network. shdwDrive then encrypts and erasure codes the data and the fragments are algorithmically distributed in triplicate across the distributed network. Everything is handled trustlessly via smart contract and requires signed #Solana transactions ensuring a publicly verifiable on-chain log.
3. **Deterministic Naming Structure!**
   * Did you know that when you upload files to shdwDrive you will know the URL for your file before the upload is complete? In combination with your wallet's pub key and the file name you choose your building doesn't have to stop to wait for a URL. A deterministic naming structure eliminates the need for a central index, which can become a bottleneck as more objects are added to the system. This structure also makes it easier to scale the system, since all objects are named in the same way. Finally, a deterministic naming structure can be used to speed up queries, since the exact location of an object is known and can be easily located.
4. **Community designed UI's!**
   * The Solana community is incredibly talented and have really shown this by designing multiple user interface's that make uploading files to shdwDrive easy for non-developers. Web3 online storage should be accessible to everyone, not just developers.
5. **Great tools and easy to use!**
   * shdwDrive has a great CLI and comes with a robust SDK (JS, Rust and Python). This more easily enables applications to embed Storage directly into their Web App allow the end user to upload files. The CLI is great and all, but we want to bake this functionality to use the shdwDrive directly into our app. We got you covered with SDKs in both [JavaScript/Typescript](../build/the-sdk/sdk-javascript.md) as well as [Rust](../build/the-sdk/sdk-rust.md) and [Python](../build/the-sdk/sdk-python.md). The SDK code is open source and so both the core team and community are constantly improving the developer experience and the shdwDrive tool suite.
6. **Performance & Scalability**
   * shdwDrive is just as fast if not faster than standard cloud storage providers and has been design for massive horizontal scalability in preparation for rapid mass adoption.

### Pricing

shdwDrive is more affordable than any other similar storage platform currently on the market! Users can easily and affordably pay for both storage and the bandwidth needed to serve requests at a fraction of the cost of using other providers.

Mutable storage fees target a specific USD price:

* **$0.05 USD per gibibyte per year**
* **$0.0002739726 USD per gib per Solana Epoch**

Currently, that is **$0.05 USD per gibibyte per year**. This comes out to **$0.0002739726 USD per gib per Solana Epoch** (the interval for which mutable storage fees are collected). This price target is converted to $SHDW/$USDC at the time of fee collection.

You can also set up [S3-compatible](https://docs.shadow.cloud/build/s3-compatible-client-access) access. Details for setting up S3 access are [here](https://docs.shadow.cloud/build/s3-compatible-client-access).

For common questions check out the [General](../build/support-and-faq.md) section of our FAQ.

### [**Get started with shdwDrive!**](../build/shadow-drive.md)
