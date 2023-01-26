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
