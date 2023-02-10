---
description: Exploring the Anatomy of DAGGER
---

# DAGGER Guts

### The Components of DAGGER

First, let's look at a high level at the pieces that make up DAGGER before we talk about how they all work together. The DAGGER protocol has 3 main components:

* The Controller
* Ledger
* The Payload Processor

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

**The Controller serves as point guard (which is my second sports analogy in this documentation for the astute observer).** Requests for data as well as transactions arrive here on the Controller. The controller will forward new payloads to the Ledger graph, who will handle it further. After receiving an ordered, verified series of events, the controller orchestrates provisioning or changing of virtual machines and infrastructure.

**Ledger - the ShadowGraph lives here.** The ledger is responsible for being able to house, update, and verify the graph at all times. Ledger sits in between the Controller, who sends it payloads to verify, and the Processor, who sends the verification. Ledger also manages the gossip layer and receives events through the comms bus from peering operators or from devs requesting infrastructure.

**The Payload Processor - processes and verifies payloads received from Ledger.** And that's really about it for the processor, but it's where the journey of explaining gossip, payloads, and events begins.

Let's walk through it step by step.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

Blow that image up nice and big like.&#x20;

**In phase 1,** traffic makes it's way into the DAGGER protocol. This could be in the form of a user requesting to provision a VM, or look up their account details. This could also be a peer node requesting to synchronize... requests. Either way, you can think of this as new data is coming in to change the state, or there are lookup requests to retrieve the current state.

They land on the communications layer, which is basically just a message queue. No real data will persist here, much the way most traditional message queues work.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

**In Phase 2, traffic goes to and from the communications layer.** The real magic work happens between the communications layer and the controller. This happens when the state needs to change and a request (payload) needs to be managed. But events that arrive on the communications layer can also be sent to and from the ledger.

The Ledger layer is where the graph of nodes and events lives and is changed. As the controller sends request payloads to the ledger graph to change the state of the graph, DAGGER then relies on the Payload Processor to determine the validity of the request. Once the Processor orders and validates the request, it is sent back to the ledger, who returns the ordered events back to the controller.&#x20;

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

**In Phase 3, the Controller orchestrates the infrastructure provisioning.** Once the Controller sees the order of events, it can issue commands to the operators to deploy, change, or remove compute and storage.&#x20;

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

