---
description: D.A.G.G.E.R. stands for Directed acyclic gossiping graph enabling replication, and this section explains the architecture on a high level.
---

# D.A.G.G.E.R

DAGGER is a distributed system with a graph based consensus meechanism. There are four componenents that make up the protocol specification. The components, in the order an income transaction would see them throughout its lifecycle are ilustrated the figure below:

<figure><img src="../.gitbook/assets/dagger-lifecycle.png" alt=""><figcaption></figcaption></figure>


### Terminology

The following list defines a few terms commonly used throughout this document:

Transaction: A write request submitted by a user. A transaction can contain raw bytes,
membership management requests, and Shadow transactions ([Shadow Drive/Cloud actions
e.g. store file, instantiate VM).

Block: A set of transactions that are packed into a [Merkle Tree]() whose root hash is included
in a node in the [DAG]().

Event: A node in the DAG, which contains the hashes of its parents, a timestamp, a
Block payload, and the creator’s signature of the aforementioned.

The DAGGER system reaches consensus on the ordering of Events via asynchronous compu-
tations on a local graph.


## Components

### Communications (Network I/O)

The communications module initializes outgoing sync requests with peers and forwards sync
responses from peers to the processor, handles incoming sync requests from peers, forwards
transactions to the processor, forwards RPC requests to the controller, and maintains a peer IP
database.

* Outgoing Sync Requests
    * To initialize a sync request, we begin by selecting a random active peer which has not recently
been synced with. Since we need the most up-to-date peer list along with a summary of our
current graph’s state, the communications module sends a request to the graph module to
summarize the current graph’s state and choose a peer 1. The communications receives the state
summary and a chosen peer, sends it to the peer and awaits a sync response, which is forwarded
to the graph module to be digested.

* Incoming Sync Requests
    * An incoming sync request is immediately forward to the graph module. We await a packaged
sync response containing all events that we have which the peer does not have, which is sent
back to the peer.

* Incoming Transactions
    * When a user submits a transaction, the communications module receives forwards it to the
processor. After verification, the transaction makes it way through the forester and the graph
module which, upon inclusion in a block, sends back the signature for the event which contains
the block in which the transaction was included. The communications module forwards this
signature back to the user. Note that this does not mean the block has been finalized.
    <figure><img src="../.gitbook/assets/dagger-lifetime-txn.png" alt=""><figcaption></figcaption></figure>

* Incoming RPC Requests
    * When a user submits one of several possible RPC requests, whether it be to read a file or inquire
about a block or transaction, it is forwarded to the controller. The controller sends back the
result of the ledger query to the communications module, which forwards it to the user.
    <figure><img src="../.gitbook/assets/dagger-lifetime-rpc-request.png" alt=""><figcaption></figcaption></figure>

### Processor (Verification)

The verifier and forester are sibling modules responsible for verification of events, blocks, and
transactions. The verification happens for events made by peers, as well as for incoming trans-
actions made by users. There are several forms of verification: signature verification for trans-
actions, signature verification for blocks, and root hash verification for blocks. When processing
peer events, the verifier is responsible for the first two of these forms of verification, while the
forester is responsible for verification of the root hash of the transaction merkle trees that form
the blocks. When processing incoming user transactions, the forester gathers transactions to
pack into a block and produces the merkle root hash which represents the block.

### Graph (Consensus)

At a high level, the graph has two core responsibilities along with some collaborative duties.
The two core responsibilities are: to digest sync responses to build up the gossip graph, and to
analyze the graph to derive the consensus ordering of events. The collaborative duties are to help
the communications module decide which peer to sync with and to inform the communications
module

### Controller (Transaction Execution)

The controller module performs reads and writes to the ledger. This is where different use cases
of DAGGER will vary the most. For filesystem applications of DAGGER (e.g. Shadow Drive),
this includes operations like shredding and erasure coding. For other use cases like oracles,
bridges, and VM orchestration, this is where external calls are made.

