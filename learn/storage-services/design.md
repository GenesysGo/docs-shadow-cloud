---
description: A deeper dive into the design of Shadow Drive and the problems it solves.
---

# The Shadow Drive Platform

## Deterministic Naming
The Shadow Drive platform is designed to support entire ecosystems being built on top of it. Shadow Drive storage is deterministic to allow for ease of use. While other storage protocols require the user to wait for data to be uploaded in order to generate a URI, Shadow Drive has a deterministic scheme:

**`https://shdw-drive.genesysgo.net/<storage-account-pubkey>/<file-name>`**

Information can be prepped for uploads, index, the creation of custom RPC calls, etc in advance. Meaning, application developers will find it much easier to pre-plan their builds in advance.

## Content ID Addressing *(future release)*
In the near future, content-ID (CID) addressing will be used as well. Content ID addressing is a deterministic naming structure that can be used in distributed file storage systems to identify content stored in the system. It works by generating a unique identifier for each file or piece of data stored in the system. The identifier is generated based on the content’s content-hash (SHA-256). This content-hash is used to generate the content ID, which is then used to locate the data in the system.

DAGGER's directed acyclic graph (DAG) will be used to track the files stored in the system. Each node in the DAG will contain a list of the content IDs for each file stored in the system. The gossip network will be used to share the content IDs between nodes, so that all participants in the system can access a consistent view of the content stored in the system. Finally, the peer-to-peer network can form consensus on the files that need to be stored in the system, based on the content IDs shared between the nodes.

## Developer Tools
Builders can interact directly with Shadow Drive using the Shadow Drive [CLI]() or the Shadow Drive [SDK]() to build front-end applications directly on top of the drive. Providing SDKs in Javascript, Rust, and Python provides a number of benefits and efficiencies to developers. It allows developers to access the full range of features and capabilities of the application, and helps developers to quickly get up to speed on the application, since there’s less of a learning curve when working with a language they’re already familiar with.

It is our goal to empower developers to integrate Shadow Drive directly into their builds and to support this incredibly talented community of designers who will absolutely come up with better platforms for Shadow Drive than we could possibly come up with on our own!

## Under the Hood (this section will need reworking)

At the heart of Shadow Drive lives the open source software defined storage program called Ceph.

![https://en.wikipedia.org/wiki/Ceph\_(software)](<../../.gitbook/assets/>)

**Ceph was chosen for a number of reasons…**

1. It is VERY open source. Ceph was first presented in 2006 and merged directly into the Linux kernel in 2010. Since then the Ceph GitHub has grown to 179 different repositories. These different repositories have been collectively forked over 10,000 times, have had thousands of PRs submitted, and has seen a community of tens of thousands emerge to provide support. [https://github.com/ceph](https://github.com/ceph)
2. It is extremely resilient and adaptable. Ceph is designed to not have a singular point of failure that could lead to data loss. As Shadow Drive is being designed to run in a permissionless trustless decentralized environment, having no singular point of failure is very attractive. The resiliency of how Ceph stores data and its open source design mean that Ceph can be forked and modified to be a trustless permissionless decentralized storage layer that can be integrated with smart contracts to protect the stored data against bad actors.
3. Ceph is very performant and scales exceptionally well both horizontally and vertically. Our decentralized cluster consistently handled 2,000 concurrent connections, each uploading 10,000 individual objects measuring 2mb in size, and sustained an upload speed of 2.7gbps with zero packet loss for extended periods of time. This means that the cluster is so fast that when Solana validators finish block #130188099 we can ingest it, store it, and serve live requests against it **before** block #130188100 is finished and propagated.
4. Ceph’s CRUSH map algorithm is amazing! CRUSH is a scalable pseudo-random data distribution function designed for distributed object-based storage systems that efficiently maps data objects to storage devices without relying on a central directory. The CRUSH whitepaper ([https://ceph.com/assets/pdfs/weil-crush-sc06.pdf](https://ceph.com/assets/pdfs/weil-crush-sc06.pdf)) dives deep into the algorithm but the TL;DR is that CRUSH allows for the decentralization of location for data on an individual byte level. Ceph utilizes CRUSH to literally break stored objects down into component bytes, shards/erasure codes those bytes, and then decentralizes their location in triplicate across any particular Ceph cluster.
5. Speaking of decentralization of data… Ceph runs its own consensus mechanism internally to ensure the integrity of your data. Monitor daemons are the custodians of the pieces of the CRUSH map and are responsible for verifying its accuracy and approving/recording changes to the stored data. Ceph monitors use a Paxos consensus mechanism to maintain a quorum and verify the authenticity of the data stored in the cluster. We will revisit the importance of this consensus mechanism later when we discuss Solana integrations.
6. Finally, Ceph is (theoretically) infinitely scalable without any notable decrease in performance. There is no theoretical max to how large a Ceph cluster can become. This is due to the different software daemons Ceph employs and how well the CRUSH algorithm scales. The largest Ceph cluster ever tested successfully stored 10,000,000,000 unique objects. If we think about each Solana block produced we are currently in the 120 millions (as of the time of this writing). Therefore, Ceph is uniquely positioned to be the best possible solution for a blockchain that produces a new block every 400 milliseconds.

### As a fun side note… <a href="#8f7c" id="8f7c"></a>

…the creator of the Paxos Consensus Mechanism, Leslie Lamport, is also honored as Solana’s biggest technical influence…

![https://docs.solana.com/introduction](<../../.gitbook/assets/>)

![https://martinfowler.com/articles/patterns-of-distributed-systems/paxos.html](../../.gitbook/assets/)

### If nothing else… <a href="#b15e" id="b15e"></a>

It’s kinda cool to know that we’re using the same DB software as the CERN team is! [https://indico.cern.ch/event/649159/contributions/2761965/attachments/1544385/2423339/hroussea-storage-at-CERN.pdf](https://indico.cern.ch/event/649159/contributions/2761965/attachments/1544385/2423339/hroussea-storage-at-CERN.pdf)

In fact, the CERN team submitted PRs to the main Ceph branch to have their homegrown improvements included in the main branch.

Of course, none of this is to suggest that Ceph is some kind of perfect solution that has no flaws and can do no wrong. However, for our use case, Ceph checks all the boxes of performance, reliability, durability, scalability, and its functionality can be adapted to provide the decentralized trustless data storage that Solana needs.

## The Shadow Drive Enhanced RPC Network Architecture

Data and data storage provide the ultimate foundation for many new and exciting directions for GenesysGo and our network. Our RPC network will undergo an extreme revamp with the creation of a new kind of node which we will use to serve Solana data.

As written earlier, Shadow Drive is capable of ingesting the most recent block, storing it, replicating it, and then serving RPC requests against it faster than the Solana validators can build the next block. This new capacity allows us to maximize our relationship with Solana Labs by being able to answer RPC requests using machines which are not affected by any turbulence of the Solana validator network. The current design of the RPC server is also deeply tied to the state of congestion on Solana at any given moment. What we have done with Shadow Nodes has allowed us to keep them deeply tied to Solana as an RPC node, however we’ve attached the tie in a different way and place to allow Shadow Nodes to truly maximize stability and performance.

Additionally, the vast data set we are ingesting will allow us to serve network snapshots faster and more accurately than is currently possible… which will allow new validators to come online faster after they perform software rollups, restart their machines after maintenance, or recover after a hardware failure. This will also have the added benefit of saving existing validators from needing to use their own resources to serve snapshots to other validators. Anything that maximizes a validator’s ability to focus on building blocks and building blocks alone is a huge win for the network.
