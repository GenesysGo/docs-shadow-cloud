---
description: A deeper dive into the design of Shadow Drive and what problems it solves.
---

# stuff
# (probably the why and what summary go here)
# (add an outline to the following sections which are about design)

If you're a #Solana dev and are wondering why you should use $SHDW Drive, here's a few thoughts:

1) User choice! Shdw Drive is built  to empower users to decide what should happen with their data.
Immutable or mutable data, you choose. We don't take that choice away from you.

2) GDPR Compliance! 
This is a big one... developers are given all the tools they need to comply with GDPR and can show records that prove that they have deleted a user's personal data.
Legal grey areas and loopholes are a risk to your business, $SHDW Drive helps you stay safe.

3) Deterministic Naming Structure! 
Did you know that when you upload files to $SHDW Drive you will know the URL for your file before the upload is complete?
In combination with your wallet's pub key and the file name you choose your BUIDL doesn't have to stop to wait for a URL.

4) Community designed UI's!
The #Solana community is incredibly talented and have really shown this by designing multiple user interface's that make uploading files to $SHDW Drive easy for non-developers. #web3 online storage should be accessible to everyone, not just developers.



$SHDW Drive then encrypts and erasure codes the data and the fragments are algorithmically distributed in triplicate across the distributed network. Everything is handled trustlessly via smart contract and requires signed #Solana txns ensuring a publicly verifiable on-chain log.

For example, in the case of a project or business needing to produce records to prove GDPR compliance with a user's request to delete personal data... all the records to prove GDPR compliance live on-chain and have been trustlessly verified by the #Solana validator network.



# this is a dump of data to be organized into subfolders

# Shadow Drive

Everyone begins their cloud journey working with storage. Maybe that's Dropbox, or OneDrive, or Google Drive, or iCloud drive. But that's definitely where everyone's journey starts. So what is the web3 equivalent to those? Shadow Drive.&#x20;

Using any Solana wallet, and a little bit of $SHDW, users can carve out permanent, dedicated storage to store their files (and it doesn't even hash the filename!).&#x20;

Beyond that, any software developer that wants to build a dApp that requires some sort of file storage (because the app creates data, or because the users upload data - whatever!), Shadow Drive comes with a full fledged SDK (JS or Rust) for baking storage capability straight into your dApp.


# Before You Begin

### What Makes Shadow Drive so great?

Don't worry, we're going to get to the How To's next. For now, let's talk about _why_ you should use Shadow Drive for your storage.

As a developer, we want you to see Shadow Drive as a direct competitor and alternative to AWS S3, Azure Blob, Arweave, and IPFs. Here's why Shadow Drive is better.

#### It's insanely fast.

No, really. It's faster than any other cloud storage I've ever used. We regularly see file transfers in excess of 2Gbps. When the Solana mainnet has been halted in the past, Genesys Go has been hosting the restoration snapshots on Shadow Drive so that RPC and Validator operators can download it as quickly as possible to get back online.

_So what's the secret sauce? How does it pull off these breakneck speeds?_

I'm going to try not to get tooooooo deep in the weeds here. At the highest level, there are two mechanisms that make up Shadow Drive - the actual storage network of servers that host the files (including CDN and caching), and the Solana blockchain. The storage network relies on the Solana blockchain for instructions.&#x20;

At the end of the day, blockchain is enabling peer-to-peer transactions and services without middlemen, in a self-healing and trustless nature. People want to buy storage. Operators want to provide it. How can we link these two up? By using the fastest, trustless ledger around - Solana.

As consumers want to purchase storage, or change their storage, or make their storage immutable, they interact with our Solana smart contract to issue their request. Our storage network reacts to the Solana transaction which on average sees a 2 second confirmation and 12 second finality. So the creation of a storage account happens in seconds, using Solana for decentralized consensus. These on-chain events bring you Proof of Storage.

And as I mentioned, the storage network itself is bonkers fast. We can ingest and serve data faster than Solana can produce it now. Under the hood, we are using our own modified version of Ceph, a distributed database system that even CERN uses. And that kind of leads to the next point

#### The same thing that makes it fast also makes it resilient

Ceph by design does not have a single point of failure. Solana by design (for the most part) does not have a single point of failure. This means data storage and creation is highly available. Stored data, therefore, cannot be altered by someone who doesn't own the storage account. Ever. And because of Ceph's design to distribute data in triplicate, as well as integrity check, resiliency is constantly enforced.

#### Did I mention it was fast?

Ceph scales both horizontally and vertically better than it reasonably should. Our decentralized cluster consistently handled 2,000 concurrent connections, each uploading 10,000 individual objects measuring 2mb in size, and sustained an upload speed of 2.7Gbps with zero packet loss for extended periods of time. This means that the cluster is so fast that when Solana validators finish block #99 we can ingest it, store it, and serve live requests against it **before** block #100 is finished and propagated.

#### Sounds expensive.

It's not.

![](<../.gitbook/assets/Screen Shot 2022-10-13 at 9.22.16 AM.png>)

This is a request to create a 1GB storage account, permanently. Not monthly or hourly or yearly. Forever. It costs 0.25 SHDW, which equates to \~$0.08 at the time of this writing.&#x20;

# The Shadow Drive Platform

The Shadow Drive platform is designed to support entire ecosystems being built on top of it. Shadow Drive storage is deterministic to allow for ease of use. While other storage protocols require the user to wait for data to be uploaded in order to generate a URI, Shadow Drive has a deterministic scheme:

https://shdw-drive.genesysgo.net/<storage-account-pubkey>/<file-name>

Info can be prepped for uploads, index, the creation of custom RPC calls, etc in advance. Meaning, application developers will find it much easier to pre-plan their builds in advance.

Builders can interact directly with Shadow Drive using the Shadow Drive CLI or use the Shadow Drive SDK to build front-end applications directly on top of the drive.

It is our goal to empower developers to integrate Shadow Drive directly into their builds and to support this incredibly talented community of designers who will absolutely come up with better platforms for Shadow Drive than we could possibly come up with on our own!

# Shadow Drive v1.5 Overview

## Trustless Permissionless Solana Optimized Storage

Shadow Drive was created to prove that a decentralized storage network can deliver perform equal or greater to cloud providers while still retaining the ability to scale horizontally and avoid bottlenecks.

The Shadow Drive has been designed as the solution to Solana’s growing need for an on-chain ecosystem native storage solution. The following pages are intended to outline what Shadow Drive is, how it works, and why it works. These pages will do so by explaining things in practical terms as our goal is for a wide range of people to fully understand what is being built, rather than a select few. The hard-core academic types with a deep love for algorithms will likely find these pages to be largely disappointing.

Please note, this paper assumes some basic knowledge of GenesysGo, $SHDW, and the Solana architecture as a whole. This paper is, by design, not meant as an introductory resource to GenesysGo or Solana. Instead, this paper should be viewed as a resource for those considering GenesysGo’s Shadow Drive as a potential storage solution or for those attempting to vet the Shadow Drive’s long term viability.

If you aren’t familiar with Solana’s architecture, then it is highly recommended to spend some time learning about how Solana validators store “Account State”, what “AccountsDB” is, and what goes into the creation of “on-chain accounts.” Please see the Solana Discord (discord.gg/solana) and check out the dev-resources channel if none of these things are familiar to you.

* Contents
    * Summary
    * What is Shadow Drive?
    * Under the hood of Shadow Drive
    * Solana On-Chain Events and Proof of Storage
    * The Shadow Drive Smart Contract(s)
    * The Shadow Drive Enhanced GenesysGo Network Architecture


# The Shadow Drive Storage Stack
The Shadow Drive Stack is broken into two parts: the underlay and the overlay. The following serves as a very basic primer to help one envision Shadow Drive's architecture and is written with the non-technical user in mind.

Certain terms, such as "Underlay" and "Overlay", are far more nuanced than the description in these documents and we would very much encourage anyone with the desire to learn more to use these definitions as their jumping off point into learning more!

For the highly technical, all of our live code is open source and available on our Github to deep dive at your leisure.

# Summary
https://medium.com/@genesysgo/the-shadow-drive-decentralized-storage-optimized-for-solana-11f2f257aaf2
The Shadow Drive is a new approach to on-chain storage and is built specifically for the Solana blockchain, while retaining the ability to become a polychain service in the future. Other web3 storage providers have attempted to integrate with Solana in the past and have had only marginal success. Shadow Drive solves this by integrating directly with the Proof of History consensus mechanism, passing on-chain events for consensus approval by the Solana validator network which prove the continued existence and integrity of the stored data.
The idea for Shadow Drive was founded on the premise that the Solana network is largely misunderstood in terms of what it was built to achieve. By understanding what it truly means to be the world’s most performant state machine, you realize that Solana can be used to reach consensus and maintain state on nearly anything.
Whereas other decentralized storage providers require multiple layers in order to make their solution viable (i.e. Arweave does storage, but to make uploads faster you need to use Bundlr, and then in order to serve the data in any kind of a useful way you need to use ArDrive), Shadow Drive builds everything in to one platform.
When a user commits their $SHDW to Shadow Drive for storage they are not only provided with storage space, but also with incredibly fast batched uploads and a robust CDN. This ensures their content is stored quickly, efficiently, and delivered at speeds that makes Shadow Drive a truly viable solution for projects that need speed, stability, and efficiency.

# What is Shadow Drive?
Shadow Drive takes open source software-defined object storage and integrates it with Solana’s Proof of History consensus mechanism, and then decentralizes it. This allows it to achieve extremely fast I/O speeds, massive scalability, and data integrity. At the same time it retains the benefits of the trustlessness, enhanced security, full transparency, and decentralization that blockchain technology brings to the table.

Ultimately, Shadow Drive’s final iteration will be a permissionless, trustless, decentralized distributed storage network that exists in perpetuity without needing to rely on the direct efforts of a centralized team to grow and expand. It will take time, as the path for open sourcing hardware infrastructure is different from that for software, but Shadow Drive’s final form will be one in which the community drives the direction, enhancements, and future of Shadow Drive.

# Submitting Bugs

If you experience any issues or think you have found a bug with Shadow Drive then please submit it via the github link below!

Submit a PR or, if you need immediate help join our discord (discord.gg/genesysgo) and check out the #shdw-drive-support channel! Though, be warned, if you're popping in to submit a bug report then we'll likely thank you very much for your help and then direct you to the github to submit the pug so that way we can properly document everything on our end!
There are A LOT of moving parts when you build a storage network that is designed to interface with the Solana Mainnet chain and so we want to make sure our processes for investigating and solving bugs are well maintained.
Thank you for your help!