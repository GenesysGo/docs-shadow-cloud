## Overview
This Roadmap offers a high-level overview of our forthcoming development plans for $SHDW and shdwDrive v2 powered by D.A.G.G.E.R. It's important to note that features and timelines may evolve as we gain new insights and knowledge. For a more in-depth exploration of the technical intricacies of D.A.G.G.E.R., we encourage you to read the Litepaper. If you want to participate in our testnet by running a shdwNode, please visit our Operator page for instructions.

### Testnet Phase 1 (Q3-Q4 2023)
(COMPLETED)
* Released initial interactive explorer version with the base set of features
* Found and resolved numerous performance bottlenecks with D.A.G.G.E.R. core and D.A.G.G.E.R. Client RPC
* Improved memory utilization and efficiency with file uploads and gossip network message processing
* Improved multithreading in D.A.G.G.E.R. Client RPC, allowing for much more throughput than initially observed for clients not sending transactions directly to D.A.G.G.E.R. wield nodes.
* Observed a crash event that caused nodes to fall behind and become unable to catch up under specific throughput conditions. Collected metrics & logs and was able to resolve the node crashing scenario
* Improved memory management by moving finalized graph history to disk
* Collected metrics on >250 million transactions and events (excluding peer-to-peer sync events/voting), allowing for insights into detailed system tuning and improvements for Linux hosts
* Observed various network loads up to 4gbps while sustaining 20k transactions per second + file uploads
* Improved the D.A.G.G.E.R. node CLI & documentation for node operation
* Developed graph history snapshot system‍ 
* Launched the reference ShdwDrive File Management UI on portal.shdwdrive.com

(IN PROGRESS)
* Complete testing the finalized implementation of dynamic node membership
* Complete D.A.G.G.E.R. keypair management CLI
* Client RPC documentation 

(UPCOMING)
* Remote node metrics collection system 
* Typescript keypair SDK
* Improved event & state distribution system over p2p network
* Additional interactive features for the reference ShdwDrive File Management UI

### Testnet Phase 2 (Q1-Q2 2024)
(UPCOMING)
* Distribute individual node operator documentation and begin Testnet Phase 2 with decentralized operators
* Study and improve individual node operator stability
* Improve metrics and logs on network performance and stability of operators
* Build out block and graph explorer features
* Improve RPC documentation
* Add additional RPC methods
* Complete epoch testing
* Complete snapshot testing
* Run adversarial scenario simulations
* Address bugs or vulns found during individual node operator testing
* ShdwDrive v2 implementation - repair bandwidth management
* Fanout data distribution system & other improvements to comms layer/file system operations 
* ShdwDrive v2 implementation - core feature development and improvements

### Testnet Phase 3 (Q2-Q3 2024)
(UPCOMING)
* Individual node operators for ShdwDrive v2
* Observe usage / logs / etc and make further system tweaks as necessary
* Resolve any outstanding bugs or accrued technical debt
* Publish an updated version of the Roadmap with subsequent phases and features