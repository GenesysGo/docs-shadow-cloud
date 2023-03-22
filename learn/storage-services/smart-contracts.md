# Smart Contracts

The Shadow Drive smart contracts are engineered for diverse use cases, showcasing the versatility inherent in smart contract design. This adaptability is a significant advantage when constructing a storage layer, rather than developing an entirely new blockchain. After all, why create a new blockchain when we can leverage the world's most efficient state machine already at our disposal?

## The Shadow Drive Smart Contracts

### Gossip-driven Consensus

The Paxos consensus mechanism is pretty amazing and is, arguably, the forerunner of the Proof of History consensus mechanism. It is a widely influential approach to gossip-driven consensus networks.

Gossip-driven consensus mechanisms are remarkable, providing an efficient and fault-tolerant means of achieving agreement in distributed systems. This approach, along with the distributed Shadow Drive architecture, forms the foundation of the Shadow Drive smart contract interaction.

Within the Shadow Drive, multiple nodes work together to maintain the storage layer, ensuring data integrity and guarding against unauthorized changes. These nodes are akin to Solana validators and can be considered the guardians of the state of all data stored in Shadow Drive.

These nodes also enable the decentralization of Shadow Drive, as they can be hosted on any machine and participate in consensus voting, which is recorded on-chain. Future Shadow Drive operators wishing to become part of the network will be subject to higher slashing and collateral requirements, akin to the Serum/Mega-Serum node structure utilized by Project Serum.

The Shadow Drive smart contract serves as an administrator, controlling access and managing uploads or edits to the storage layer. Furthermore, nodes are configured to report their state and votes to the Solana validator network for validation and inclusion on-chain. This on-chain mechanism allows for the recording and slashing of nodes attempting malicious activity or failing to maintain the integrity of Shadow Drive's data store. This process closely resembles how the Solana validator network records and reports on the vote history of Solana validators, ensuring that Shadow Drive remains a trustless, permissionless network.

In summary, nodes within the Shadow Drive run a lightweight gossip-driven consensus mechanism, while the smart contract guarantees that the activity of this consensus is trustless and transparent to all on-chain.

Please note that data uploads and edits are distinct from data access. Projects using Shadow Drive for storage can control who can read the uploaded data, ensuring the desired level of privacy and access control.

### Balancing Mutable and Immutable Storage Needs: Adapting to Evolving Storage Requirements with Shadow Drive

Understanding the diversity of user requirements, we strive to offer a flexible storage solution that caters to both mutable and immutable data storage. Our approach acknowledges the fact that not all data needs to be stored perpetually and provides users with the freedom to choose the storage method that best suits their needs.

The Shadow Drive token, SHDW, is divisible into fundamental units called "Shades," with the focus on storage fundamentals. This relationship between Shades and bytes is maintained for simplicity:

* 1,000,000,000 Shades = 1 SHDW
* 1,000,000,000 bytes = 1 gigabyte

Thus, 1 byte stored = 1 Shade.

#### **Immutable Storage**

Immutable storage offers users the peace of mind that their data will be retained permanently and cannot be edited or deleted. The cost calculation for immutable storage is simple, as the smart contract sends the appropriate amount of SHDW to the Shadow Operator smart contract as emissions for operating Shadow Nodes.

#### **Mutable Storage**

Mutable storage, on the other hand, involves a more complex process that addresses short-term storage needs and the increased bandwidth requirements typically associated with mutable storage. This process incorporates staking and rent mechanics similar to Solana's on-chain accounts design.

Users are required to stake a specific amount of SHDW when they upload data to Shadow Drive. Rent is assessed on a per-epoch basis for mutable storage, with the rate subject to change as the system evolves. The rent assessed each epoch is sent to the Shadow Operator Smart Contract, contributing to the pool that the smart contract pays out to Shadow Operators as emissions.

### **Adapting to Evolving Requirements with DAGGER and Shadow Drive v2**

Our smart-contract design is tailored to accommodate both mutable and immutable storage options effectively. As DAGGER and Shadow Drive v2 continue to evolve, the pricing dynamics and overall system will adapt to provide the most efficient and flexible storage solutions for our users.

We are committed to ensuring that our users can make informed choices about their data storage preferences and that they have access to a system that can adapt to their diverse storage requirements.

_Please note, the team reserves the right to change any and all aspects of storage design and costs in order to ensure the long term viability of Shadow Drive and Shadow Operators. We would rather make tweaks to one protocol along the way instead of building a new protocol each time to address any unforeseen issues._
