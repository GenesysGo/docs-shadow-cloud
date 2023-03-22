# Smart Contracts

The Shadow Drive smart contracts are engineered for diverse use cases, showcasing the versatility inherent in smart contract design. This adaptability is a significant advantage when constructing a storage layer, rather than developing an entirely new blockchain. After all, why create a new blockchain when we can leverage the world's most efficient state machine already at our disposal?

### The Shadow Drive Smart Contracts

#### Gossip-driven Consensus

The Paxos consensus mechanism is pretty amazing and is, arguably, the forerunner of the Proof of History consensus mechanism. It is a widely influential approach to gossip-driven consensus networks.

Gossip-driven consensus mechanisms are remarkable, providing an efficient and fault-tolerant means of achieving agreement in distributed systems. This approach, along with the distributed Shadow Drive architecture, forms the foundation of the Shadow Drive smart contract interaction.

Within the Shadow Drive, multiple nodes work together to maintain the storage layer, ensuring data integrity and guarding against unauthorized changes. These nodes are akin to Solana validators and can be considered the guardians of the state of all data stored in Shadow Drive.

These nodes also enable the decentralization of Shadow Drive, as they can be hosted on any machine and participate in consensus voting, which is recorded on-chain. Future Shadow Drive operators wishing to become part of the network will be subject to higher slashing and collateral requirements, akin to the Serum/Mega-Serum node structure utilized by Project Serum.

The Shadow Drive smart contract serves as an administrator, controlling access and managing uploads or edits to the storage layer. Furthermore, nodes are configured to report their state and votes to the Solana validator network for validation and inclusion on-chain. This on-chain mechanism allows for the recording and slashing of nodes attempting malicious activity or failing to maintain the integrity of Shadow Drive's data store. This process closely resembles how the Solana validator network records and reports on the vote history of Solana validators, ensuring that Shadow Drive remains a trustless, permissionless network.

In summary, nodes within the Shadow Drive run a lightweight gossip-driven consensus mechanism, while the smart contract guarantees that the activity of this consensus is trustless and transparent to all on-chain.

Please note that data uploads and edits are distinct from data access. Projects using Shadow Drive for storage can control who can read the uploaded data, ensuring the desired level of privacy and access control.

**Mutable vs. Immutable Storage**

We have learned from the experience of existing storage providers and feel that it is important we ensure users have choice in how their data is stored. Some data should be stored perpetually, but not all data requires this. In fact, one could argue that very little data actually needs to be stored forever. This is likely one of the reasons why Arweave only stores \~60TB (as of the time of writing) of data in total.

With all that said, users will select how they want their data stored, and that will impact the overall cost of storage and the way that cost is assessed.

The SHDW token is broken up into fundamental units called “Shades”. Shadow Drive focuses on the fundamentals of storage and, at the most fundamental level, Shadow Drive stores bytes. Therefore, in the spirit of simplicity, the following conversions are applied:

1,000,000,000 Shades = 1 SHDW

_This is in line with 1b lamports equaling 1 SOL as a measure of the cost computational units_

1,000,000,000 bytes = 1 gigabyte

_Therefore…_

1 byte stored = 1 Shade

**Immutable storage** is relatively simple… If you upload 1gb of data to Shadow Drive, then a cost of .25 SHDW is applied and the smart contract sends that SHDW to the Shadow Operator smart contract to be sent out to Shadow Operators as emissions for operating Shadow Nodes. The data is flagged as “immutable” by the Shadow Drive smart contract and is therefore unable to be edited or deleted.

**Mutable storage** is a little more complex (but not overly so) and relies on staking and rent mechanics very similar to the ones used by Solana in their on-chain accounts design. If you upload 1gb of data to Shadow Drive, then you are required to **stake** .25 SHDW. On a per epoch basis, rent is assessed against mutable storage at the rate of 1 SHDW per GB per year. Therefore, uploading 1gb of data to Shadow Drive would look something like this…

1. A user uploads 1gb of data into Shadow Drive. In order to do so, the user is required to stake 1 SHDW with the Shadow Drive smart contract.
2. On a per epoch basis, rent is assessed and removed from the staked SHDW (unless the user has flagged the data as immutable) in the form of Shades. By the end of a 1 year period, the storage user would need to add additional SHDW to their account or risk the data being deleted.
3. The rent assessed each epoch is sent to the Shadow Operator Smart Contract and adds to the pool the smart contract pays out to Shadow Operators as emissions.
4. If, after six months, the storage user un-stakes their SHDW (signaling that they no longer need this data to be stored), then they would have paid .5 SHDW in rent and would receive .5 SHDW back.
5. If, after six months, the storage user decided to flag their data as immutable, then they would need to add the difference between what is currently staked in the smart contract and what it would cost to immutably store their data. So, a user with 1gb of data stored and .5 SHDW staked in the smart contract (after six months of staking) would need to add .5 SHDW in order to make their data immutable.

The rent mechanic is designed to take into account the increased bandwidth usage that typically comes along with mutable storage as it has an increased amount of reads and writes relative to immutable storage.

The benefit to users of mutable storage is that their short-term storage needs are met; and once those needs have been met, the mutable storage user can un-stake their SHDW and be returned whatever amount of SHDW they did not use. The benefit to those who use immutable storage is that they can rest assured that their account data will be retained for continual access and usage into perpetuity. This also means that as the price of $SHDW fluctuates, users of immutable storage will likely have paid a cheaper price for storage as years pass.

In the end, we believe immutable storage to be the best overall value, but we also recognize that short-term data storage needs should not be forced into a singular regime.

_Please note, the team reserves the right to change any and all aspects of storage design and costs in order to ensure the long term viability of Shadow Drive and Shadow Operators. We would rather make tweaks to one protocol along the way instead of building a new protocol each time to address any unforeseen issues._
