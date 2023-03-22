# UML for merkle tree

@startuml

class MerkleTree {
  +root: Hash
  +computeTree(data: List<Data>): void
}

class Node {
  +hash: Hash
  +left: Node
  +right: Node
  +isLeaf(): bool
}

class Data {
  +data: String
}

class Hash {
  +value: String
}

MerkleTree *--> Node
Node *--> Node
Node *--> Data
Data *--> Hash

@enduml



dagger mermaid sequence:

```
title DAGGER: Lifecycle of a Transaction

participant User
participant Communications
participant Peers
participant Verifier
participant Forester
participant Controller
participant Graph

User->Communications: New Transaction via RPC
Communications->Verifier: Transaction Forwarded for Signature Verification
note over Verifier,Forester, Controller: Processor
Verifier->Forester: Root Hash Verify
Forester->Graph: Block Packing

Note over Communications,Graph: Sync State
Graph-->Communications: Update State and Peer
Communications->Peers: Outgoing Sync Request
Peers-->Communications: Sync Response
Communications->Graph: Sync Data Forwarded

note over Controller,Graph: Consensus
Graph<-->Controller: Analyze
Controller->Graph: Write

Graph-->Peers: Sync Differences
Graph-->Communications: Signature Hash of Event
Communications-->User: Transaction Signature

User->Communications: RPC Read Inquiry
Communications->Controller: Forwarded for Ledger Read
Controller->Graph: Read
Graph-->Controller: Response
Controller-->Communications: Ledger State & Response
Communications-->User: Confirmed as Finalized

```
