---
description: Directed Acyclic Gossip Graph Enabling Replication
---

# Digging DAGGER

### DAGGER Defined

Dagger stands for **Directed Acyclic Gossip Graph Enabling Replication.** Yes, you need to know that because it is required to create every VM. If you get the answer wrong, we'll drain your wallet.

Not really. But seriously, it's badass tech and this doc breaks it down.

At the highest, simplest level, DAGGER is the Quarterback. It's what makes all of the gears turn and switches flip. DAGGER is a **decentralized cloud orchestration protocol.** We're fans of that whole decentralized thing, and think a Web3 solution should be decentralized through and through. So what makes it decentralized?

**First, DAGGER has a consensus mechanism**. A supermajority consensus of 2/3 vote among the nodes allows events within DAGGER to be carried out. Those events could be adding new operators to the network, or the creation of a new virtual machine.&#x20;

**Second, because there is a trustless consensus mechanism, machines can be globally distributed.** Physical machines can be added or removed from the cluster ad hoc. A gossip layer handles the exchange of information about new or existing physical machines, as well as the handling of virtual machines requested by customers. It is all event-driven and message-based.

The nerdy shit: We created something called the ShadowGraph. You could think of the ShadowGraph as a kind of Merkle DAG (or **Directed acyclic graph). A DAG is basically a mathematical phenomenon where a series of events or objects can form a cluster with no closed loop.**&#x20;

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

**Ok but what does it meeeeeeeeannnnn?** It means the nodes that operate and own the cluster (trustlessly) form a Byzantine fault-tolerant, consistent, verifiable, concurrent, parallel processing protocol. Or put in smooth brain, the code that runs the nodes and orchestrates the provisioning of VMs is seriously decentralized and fast. _You can think of the ShadowGraph as the equivalent of a ledger. It is an ordered list of the events that have happened on the platform, managed by a decentralized consensus of independent servers._

I know. I had to read it a few times myself. Take a moment to digest it because on the next page we're going to dig into the anatomy of DAGGER.
