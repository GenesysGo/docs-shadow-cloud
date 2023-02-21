---
description: A resource for those exploring GenesysGo's Shadow Drive as a storage solution.
---

# Shadow Drive Overview

The Shadow Drive is a new approach to on-chain storage and is built specifically for the Solana blockchain, while retaining the ability to become a polychain service in the future. Other web3 storage providers have attempted to integrate with Solana in the past and have had only marginal success. Shadow Drive solves this by integrating directly with the Proof of History consensus mechanism, passing on-chain events for consensus approval by the Solana validator network which prove the continued existence and integrity of the stored data.

The idea for Shadow Drive was founded on the premise that the Solana network is largely misunderstood in terms of what it was built to achieve. By understanding what it truly means to be the world’s most performant state machine, you realize that Solana can be used to reach consensus and maintain state on nearly anything.

Whereas other decentralized storage providers require multiple layers in order to make their solution viable (i.e. Arweave does storage, but to make uploads faster you need to use Bundlr, and then in order to serve the data in any kind of a useful way you need to use ArDrive), Shadow Drive builds everything in to one platform.

When a user commits their $SHDW to Shadow Drive for storage they are not only provided with storage space, but also with incredibly fast batched uploads and a robust CDN. This ensures their content is stored quickly, efficiently, and delivered at speeds that makes Shadow Drive a truly viable solution for projects that need speed, stability, and efficiency.

The following pages are intended to outline what Shadow Drive is, how it works, and why it works. These pages will do so by explaining things in practical terms as our goal is for a wide range of people to fully understand what is being built.

<details><summary>Please note</summary> 

*This paper assumes some basic knowledge of GenesysGo and the Solana architecture as a whole. If you aren’t familiar with Solana’s architecture, then it is highly recommended to spend some time learning about how Solana validators store “Account State”, what “AccountsDB” is, and what goes into the creation of “on-chain accounts.” Please see the Solana Discord (discord.gg/solana) and check out the dev-resources channel to learn more.*
</details>

## Contents
 * **[Design]()**
    * Learn how the Shadow Drive is built to support an entire ecosystem of new developers
 * **[On-Chain Events as Proof of Storage]()**
    * Learn how the Shadow Drive utilizes the world’s most performant state machine to ensure the validity and integrity of the state of its storage network via on-chain change events.
 * **[Smart-Contracts]()**
    * Learn how smart-contracts securely interact and orchestrate client storage

## Why use Shadow Drive?

If you're a Web3 developer and are wondering why you should use $SHDW Drive, here's a few thoughts:

1) **User choice!**
    * Shdw Drive is built  to empower users to decide what should happen with their data.
Immutable or mutable data, you choose. We don't take that choice away from you.

2) **GDPR Compliance!**
    * This is a big one... developers are given all the tools they need to comply with GDPR and can show records that prove that they have deleted a user's personal data. Legal grey areas and loopholes are a risk to your business, $SHDW Drive helps you stay safe. For example, in the case of a project or business needing to produce records to prove GDPR compliance with a user's request to delete personal data... all the records to prove GDPR compliance live on-chain and have been trustlessly verified by the #Solana validator network. $SHDW Drive then encrypts and erasure codes the data and the fragments are algorithmically distributed in triplicate across the distributed network. Everything is handled trustlessly via smart contract and requires signed #Solana transactions ensuring a publicly verifiable on-chain log.

3) **Deterministic Naming Structure!**
    * Did you know that when you upload files to $SHDW Drive you will know the URL for your file before the upload is complete?
In combination with your wallet's pub key and the file name you choose your BUIDL doesn't have to stop to wait for a URL.

4) **Community designed UI's!**
    * The Solana community is incredibly talented and have really shown this by designing multiple user interface's that make uploading files to $SHDW Drive easy for non-developers. Web3 online storage should be accessible to everyone, not just developers.

5) **Great tools and easy to use!**
    * Shadow Drive has a great CLI and comes with a robust SDK (JS or Rust). Both the core team and community are constantly improving the developer experience.

6) **Performance & Scalability**
    * Shadow Drive is just as fast if not faster than standard cloud storage providers and has been design for massive horizontal scalability in preparation for rapid mass adoption.

**[Get started with Shadow Drive!](/build/shadow-drive/README.md)**
