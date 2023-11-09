---
description: An introduction to the ShdwDrive and its implementations
---

# Learn

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td></td><td align="center"><strong>Design</strong></td><td align="center">v1.5</td><td><a href="storage-services/design.md#shadow-drive-v1.5-is-born-building-tools-and-listening-to-developer-feedback">#shadow-drive-v1.5-is-born-building-tools-and-listening-to-developer-feedback</a></td><td><a href="../.gitbook/assets/Design1.png">Design1.png</a></td></tr><tr><td></td><td align="center"><strong>D.A.G.G.E.R.</strong></td><td align="center">Distributed Ledger Tech</td><td><a href="dagger.md#introduction">#introduction</a></td><td><a href="../.gitbook/assets/Dagger2.png">Dagger2.png</a></td></tr><tr><td></td><td align="center"><strong>Use Cases</strong></td><td align="center">ShdwDrive Capability</td><td><a href="./#shadow-cloud-use-cases">#shadow-cloud-use-cases</a></td><td><a href="../.gitbook/assets/Usecases.png">Usecases.png</a></td></tr></tbody></table>

## **Mission**

GenesysGo is committed to revolutionizing cloud storage by harnessing the power of our [Directed Acyclic Gossip Graph Enabling Replication](https://github.com/GenesysGo/dagger-litepaper/blob/main/DAGGER-Litepaper.pdf) ([D.A.G.G.E.R.)](https://github.com/GenesysGo/dagger-litepaper/blob/main/DAGGER-Litepaper.pdf) technology. Our mission is to deliver a high-performance, cost-effective, and decentralized cloud storage platform that empowers developers and businesses to build secure, scalable, and efficient Web2 and Web3 applications.

## **Objectives**

Our primary objectives at GenesysGo are strategically aligned with the core functionalities of D.A.G.G.E.R. and the expansive capabilities of ShdwDrive. We are dedicated to creating a future where decentralized data management is not only viable but superior to centralized alternatives. Our focused objectives include:

1. **Data Availability Optimization**: Elevate D.A.G.G.E.R. to become the benchmark for data availability protocols, adept at accommodating the expansive and diverse needs of both Web2 and Web3 ecosystems. By enhancing its ability to manage large datasets seamlessly, D.A.G.G.E.R. will serve as the backbone for robust decentralized applications.
2. **ShdwDrive Evolution**: Propel ShdwDrive towards its v2.0 iteration, fully powered by D.A.G.G.E.R., to provide an unparalleled decentralized storage experience. Our vision for ShdwDrive v2.0 is to be a versatile platform that excels in both static data archiving and dynamic database services, ensuring optimal performance for a myriad of use cases.
3. **Mobile Integration and Network Democratization**: Harness the latent computational power of mobile devices worldwide by integrating them as auditors within the Shdw Ecosystem. This initiative aims to democratize network participation, allowing anyone with a smartphone to contribute to and benefit from the network's growth and security.
4. **Sustainable and Inclusive Ecosystem Development**:  Build towards an energy-efficient and inclusive network that rewards participation and fosters a sense of community ownership. By incentivizing users through a well-structured reward system, we aim to cultivate a vibrant ecosystem that thrives on collective contribution and shared success.
5. **Seamless User Experience and Interoperability**: Ensure a frictionless transition for users adopting ShdwDrive by maintaining compatibility with established standards like Amazon S3 and providing comprehensive development kits in various programming languages. Our goal is to make ShdwDrive accessible and easily integrated into existing and future digital workflows.

By concentrating our efforts on these key objectives, GenesysGo is set to redefine the landscape of decentralized storage solutions, offering a platform that is secure, efficient, and aligned with the evolving needs of the digital world.

## **The Basics of ShdwDrive**

The Shdw Ecosystem is a series of trustless infrastructure layers focused on a path to decentralizing the traditional cloud storage stack. Using distributed ledger technology, ShdwDrive will take advantage of high-powered traditional and mobile compute in order reducing costs of enterprise grade data center storage. We call this core technology D.A.G.G.E.R.

Powered by Shdw Operators, the network provides builders and businesses with storage and compute solutions to optionally expand their web3 presence, or just simply use some of the most highly performant distributed ledger technology for its low cost and resiliency in business.

Now cloud makes up $400,000,000,000 (that's billion with a B) per year business, and is expected to increase by another $1T by 2030.

The ShdwDrive Platform is a stable high-performance cloud storage platform on a path to be fully powered by a decentralized network of operators. Unlike traditional cloud storage platforms, network-generated revenues are sent to Shdw Operators. The ShdwDrive Platform is the only commodity cloud storage network designed to democratize the hundreds of billions earned each year by traditional cloud storage platforms without sacrificing performance.

In order to execute this trustless decentralized network, we built our own highly performant consensus and orchestration protocol called D.A.G.G.E.R.

## **D.A.G.G.E.R.**

At the heart of the Shdw Ecosystem lies D.A.G.G.E.R., a trustless decentralized consensus protocol that extends ShdwDrive's capabilities beyond typical data-center offerings. It enables decentralized compute, mobile compute, and storage, all while maintaining high performance, quality, and security. D.A.G.G.E.R. is characterized by its asynchronous, leaderless consensus, efficient bandwidth usage, and flexibility in supporting both permissioned and permissionless deployments.

Learn about D.A.G.G.E.R.:

* Explore the full potential of D.A.G.G.E.R. through our [Litepaper](https://github.com/GenesysGo/dagger-litepaper/blob/main/DAGGER-Litepaper.pdf).
* Participate in our [Testnet](https://dagger-hammer.shadow.cloud/) and run a [Wield Node](https://docs.shdwdrive.com/wield) to experience D.A.G.G.E.R. firsthand.

## **Storage**

ShdwDrive integrates open-source, software-defined object storage with Solana’s Proof of History consensus mechanism and decentralizes it, achieving remarkable I/O speeds, scalability, and data integrity. ShdwDrive's v2 implementation of D.A.G.G.E.R. will ultimately result in a permissionless, trustless, and decentralized distributed storage network, driven by community participation.

The current [design](storage-services/design.md#present-design-considerations) of ShdwDrive makes embedding storage into applications easy, robust, and natively interfaced into the Solana blockchain. The SDKs support [Javascript](../build/shadow-drive/sdk-javascript.md), [Rust](../build/shadow-drive/sdk-rust.md), and [Python ](../build/shadow-drive/sdk-python.md)all on top of fast and reliable API. If you are building an application in need of back-end storage, secondary storage, or a client-facing storage option then our robust [Javascript SDK](../build/shadow-drive/sdk-javascript.md) has you covered. For all use cases, we are focused on making fast and reliable storage incredibly easy to build with.

[**Start Building on ShdwDrive!**](../build/)

[**Pricing**](storage-services/#pricing)

## **Compute**

Utilizing _D.A.G.G.E.R._ as an orchestration / oracle protocol, we are capable of networking together decentralized resources (Shdw Operator virtual machines) under a front end UI/UX experience that effectively decentralizes virtual machine provisioning. This is similar in concept as an Amazon EC2 instances, or Digital Ocean Droplets, except the back end resources and "network stack" are owned and operated trustlessly by individuals / entities who are Shdw Operators! This is currently in proof-of-concept and under development for the future. We revealed this technology at the Solana Breakpoint 2022 event. With over 2,000 attendees we opened showcased live demonstrations of trustless virtual machine provisioning powered by a decentralized group of micro-clouds. Developers were able to build on these virtual machines for the span of the conference, experiencing first hand the exciting direction our platform is headed.

[**Check out the Shdw VMs Demo from Breakpoint**](../reference/breakpoint-2022-demo.md)

As mobile compute power and data speeds surge, we're developing _D.A.G.G.E.R._'s permissionless mobile compute and storage through the early-stage Solana Saga Mobile. This enhancement supports our existing ecosystem of builders using ShdwDrive in mobile apps. _D.A.G.G.E.R._ Mobile will bring distributed ledger technology for compute and storage to smartphones amid growing AI and gaming demands. We aim to make _D.A.G.G.E.R._, including ShdwDrive and mobile implementations, a practical choice for mobile and edge AI solutions, aligning with our roadmap.

## **Roadmap**

GenesysGo's development roadmap is meticulously structured around key milestones that are integral to the evolution of ShdwDrive, powered by D.A.G.G.E.R. technology. Our condensed roadmap highlights the strategic phases of development:

* **Testnet Phase 1**: Successfully launched an interactive explorer and enhanced the D.A.G.G.E.R. core for efficiency. Addressed multithreading improvements and resolved critical stability issues, laying the groundwork for robust performance and memory management. Achieved significant network load capabilities and introduced the ShdwDrive File Management UI.
* **Testnet Phase 2**: Focus shifts to empowering node operators with comprehensive documentation and initiating the testnet with a decentralized operator base. Emphasis will be on improving node stability, refining network performance metrics, and enhancing the block and graph explorer features. Core development efforts on ShdwDrive v2 will concentrate on refining bandwidth management and filesystem operations.
* **Testnet Phase 3 and Beyond**: A critical phase where individual node operators will be integrated, marking a significant step towards a fully decentralized ShdwDrive v2. This phase will involve rigorous observation and fine-tuning based on real-world network engagement. Commitment to resolving bugs and technical debt will be paramount to ensure the reliability of the platform. The roadmap will be periodically updated to reflect new phases and features, signaling our ongoing commitment to technological innovation and excellence.

## **Putting it all together**

In the evolving landscape of distributed ledger technologies (DLT), GenesysGo's D.A.G.G.E.R. stands out with its focus on data availability—a key advancement that addresses the real-world practicality and usability challenges faced by earlier blockchain systems. D.A.G.G.E.R.'s architecture is built upon the principle of data scalability, ensuring that it can handle the increasing demands of modern applications without sacrificing speed, stability, or security.

The modular and versatile nature of D.A.G.G.E.R. is designed to offer seamless interoperability and composability within the Web3 ecosystem. By prioritizing data availability, D.A.G.G.E.R. enables a level of performance and flexibility that empowers developers to build decentralized applications that are not only trustless and permissionless but also resilient to censorship.

By embracing this data-centric approach, GenesysGo is paving the way for a new era of DLT that is equipped to meet the diverse and complex needs of today's digital world. This commitment to advancing data availability and scalability positions D.A.G.G.E.R. as a pioneering force in the drive towards practical, efficient, and accessible decentralized technologies.

## **ShdwDrive Use Cases**

ShdwDrive caters to a wide range of storage needs:

* Web hosting & content management
* Social media platforms
* Archival & backup solutions
* Datasets for scientific and historical research
* AI applications and model storage
* Personal & editable storage space
* Specialized distributed ledger implementations

[**Start Building on ShdwDrive!**](../build/shadow-drive/)

[**See what others are building on ShdwDrive!**](broken-reference/)

## **Shdw Operators**

**(currently in Testnet Phase 1)**

Shdw Operators are the decentralized backbone of the ShdwDrive storage platform. Operators lease their high-performance compute, storage, and bandwidth. Businesses, projects, and/or individual developers can trustlessly provision network resources. User payments are sent directly to Shdw Operators. Shdw Operators who ensure their resources are always available and performant earn more. Shdw Operators who are inconsistent earn less.

At the foundation of everything are the Shdw Operators who run the nodes that power the Shdw Platform. Operators can connect new nodes to the network, manage their existing servers, or view the overall status of other Shdw Operators – all through the one easy-to-use portal.

Currently in development are a series of Shdw Operator node roles including Wield Nodes, Metadata Nodes, and Auditor Nodes. These roles work together under the orchestration of _D.A.G.G.E.R._ to form a permissionless distributed platform that supports both ShdwDrive services and mobile storage being developed.

#### Testnet Phase 1:

[Running a Wield Node](https://docs.shdwdrive.com/wield) will allow you to trustlessly participate in the D.A.G.G.E.R. phase 1 testnet which powers the D.A.G.G.E.R. Hammer interface located [here](https://dagger-hammer.shdwdrive.com/). We encourage node operators to review our blog articles for full context on the role of Wield Nodes and the purpose of D.A.G.G.E.R. Hammer.&#x20;

Wield Node operators will be handling thousands of live user test transactions and trustlessly executing all modules within D.A.G.G.E.R. that are requires to erasure code and store files uploaded to the Hammer test interface.&#x20;

## **Our Team**

We have a talented team of senior professionals across multiple fields including software sciences, distributed ledger specialists, product development, systems and cloud engineering, business and finance. We are focused on product quality, reliability, and speed to production.

We are eagerly pushing forward our goals with excitement and focus. As we look forward and execute on our agenda, continuing to support the ecosystem of Shdw builders is a top priority. With great community engagement and feedback we frequently push versions and fixes. See how we continually improve quality, function, and security of the Shdw codebase here: [**Review change logs**](../reference/change-logs.md)
