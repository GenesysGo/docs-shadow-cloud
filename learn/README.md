---
description: An introduction to the shdwDrive and its implementations
---

# Learn

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td></td><td align="center"><strong>D.A.G.G.E.R.</strong></td><td align="center">Distributed Ledger Tech</td><td><a href="dagger.md#introduction">#introduction</a></td><td><a href="../.gitbook/assets/Dagger2.png">Dagger2.png</a></td></tr><tr><td></td><td align="center"><strong>Use Cases</strong></td><td align="center">shdwDrive Capability</td><td><a href="./#shadow-cloud-use-cases">#shadow-cloud-use-cases</a></td><td><a href="../.gitbook/assets/Usecases.png">Usecases.png</a></td></tr><tr><td></td><td align="center"><strong>Design</strong></td><td align="center">shdwDrive Evolution</td><td><a href="design.md#shadow-drive-v1.5-is-born-building-tools-and-listening-to-developer-feedback">#shadow-drive-v1.5-is-born-building-tools-and-listening-to-developer-feedback</a></td><td><a href="../.gitbook/assets/Design1.png">Design1.png</a></td></tr></tbody></table>

## **Mission**

GenesysGo is committed to revolutionizing cloud storage by harnessing the power of our [Directed Acyclic Gossip Graph Enabling Replication](https://github.com/GenesysGo/dagger-litepaper/blob/main/DAGGER-Litepaper.pdf) ([D.A.G.G.E.R.)](https://github.com/GenesysGo/dagger-litepaper/blob/main/DAGGER-Litepaper.pdf)[ ](https://github.com/GenesysGo/dagger-litepaper/blob/main/DAGGER-Litepaper.pdf)technology. Our mission is to deliver a high-performance, cost-effective, and decentralized cloud storage platform that empowers developers and businesses to build secure, scalable, and efficient Web2 and Web3 applications.

## **Objectives**

Our primary objectives at GenesysGo are strategically aligned with the core functionalities of D.A.G.G.E.R. and the expansive capabilities of shdwDrive. We are dedicated to creating a future where decentralized data management is not only viable but superior to centralized alternatives. Our focused objectives include:

1. **Data Availability Optimization**: Elevate D.A.G.G.E.R. to become the benchmark for data availability protocols, adept at accommodating the expansive and diverse needs of both Web2 and Web3 ecosystems. By enhancing its ability to manage large datasets seamlessly, D.A.G.G.E.R. will serve as the backbone for robust decentralized applications.
2. **shdwDrive Evolution**: Propel shdwDrive towards its v2.0 iteration, fully powered by D.A.G.G.E.R., to provide an unparalleled decentralized storage experience. Our vision for shdwDrive v2.0 is to be a versatile platform that excels in both static data archiving and dynamic database services, ensuring optimal performance for a myriad of use cases.
3. **Mobile Integration and Network Democratization**: Harness the latent computational power of mobile devices worldwide by integrating them as auditors within the SHDW Ecosystem. This initiative aims to democratize network participation, allowing anyone with a smartphone to contribute to and benefit from the network's growth and security.
4. **Sustainable and Inclusive Ecosystem Development**: Build towards an energy-efficient and inclusive network that rewards participation and fosters a sense of community ownership. By incentivizing users through a well-structured reward system, we aim to cultivate a vibrant ecosystem that thrives on collective contribution and shared success.
5. **Seamless User Experience and Interoperability**: Ensure a frictionless transition for users adopting shdwDrive by maintaining compatibility with established standards like Amazon S3 and providing comprehensive development kits in various programming languages. Our goal is to make shdwDrive accessible and easily integrated into existing and future digital workflows.

By concentrating our efforts on these key objectives, GenesysGo is set to redefine the landscape of decentralized storage solutions, offering a platform that is secure, efficient, and aligned with the evolving needs of the digital world.

## **The Basics of shdwDrive**

The SHDW Ecosystem is a series of trustless infrastructure layers focused on a path to decentralizing the traditional cloud storage stack. Using distributed ledger technology, shdwDrive will take advantage of high-powered traditional and mobile compute in order reducing costs of enterprise grade data center storage. We call this core technology D.A.G.G.E.R.

Powered by shdwOperators, the network provides builders and businesses with storage and compute solutions to optionally expand their web3 presence, or just simply use some of the most highly performant distributed ledger technology for its low cost and resiliency in business.

Now cloud makes up $400,000,000,000 (that's billion with a B) per year business, and is expected to increase by another $1T by 2030.

The shdwDrive Platform is a stable high-performance cloud storage platform on a path to be fully powered by a decentralized network of operators. Unlike traditional cloud storage platforms, network-generated revenues are sent to shdwOperators. The shdwDrive Platform is the only commodity cloud storage network designed to democratize the hundreds of billions earned each year by traditional cloud storage platforms without sacrificing performance.

In order to execute this trustless decentralized network, we built our own highly performant consensus and orchestration protocol called D.A.G.G.E.R.

## **D.A.G.G.E.R.**

At the heart of the SHDW Ecosystem lies D.A.G.G.E.R., a trustless decentralized consensus protocol that extends shdwDrive's capabilities beyond typical data-center offerings. It enables decentralized compute, mobile compute, and storage, all while maintaining high performance, quality, and security. D.A.G.G.E.R. is characterized by its asynchronous, leaderless consensus, efficient bandwidth usage, and flexibility in supporting both permissioned and permissionless deployments.

Learn about D.A.G.G.E.R.:

* Explore the full potential of D.A.G.G.E.R. through our [Litepaper](https://github.com/GenesysGo/dagger-litepaper/blob/main/DAGGER-Litepaper.pdf).

## **Storage**

shdwDrive integrates open-source, software-defined object storage with Solana’s Proof of History consensus mechanism and decentralizes it, achieving remarkable I/O speeds, scalability, and data integrity. shdwDrive's v2 implementation of D.A.G.G.E.R. will ultimately result in a permissionless, trustless, and decentralized distributed storage network, driven by community participation.

The current [design](design.md#present-design-considerations) of shdwDrive makes embedding storage into applications easy, robust, and natively interfaced into the Solana blockchain. The SDKs support [Javascript](../build/the-sdk/sdk-javascript.md), [Rust](../build/the-sdk/sdk-rust.md), and [Python ](../build/the-sdk/sdk-python.md)all on top of fast and reliable API. If you are building an application in need of back-end storage, secondary storage, or a client-facing storage option then our robust [Javascript SDK](../build/the-sdk/sdk-javascript.md) has you covered. For all use cases, we are focused on making fast and reliable storage incredibly easy to build with.

[**Start Building on shdwDrive!**](../build/)

[**Pricing**](storage-services.md#pricing)

## **Compute**

Utilizing _D.A.G.G.E.R._ as an orchestration / oracle protocol, we are capable of networking together decentralized resources (shdwOperator virtual machines) under a front end UI/UX experience that effectively decentralizes virtual machine provisioning. This is similar in concept as an Amazon EC2 instances, or Digital Ocean Droplets, except the back end resources and "network stack" are owned and operated trustlessly by individuals / entities who are shdwOperators! This is currently in proof-of-concept and under development for the future. We revealed this technology at the Solana Breakpoint 2022 event. With over 2,000 attendees we opened showcased live demonstrations of trustless virtual machine provisioning powered by a decentralized group of micro-clouds. Developers were able to build on these virtual machines for the span of the conference, experiencing first hand the exciting direction our platform is headed.

As mobile compute power and data speeds surge, we're developing _D.A.G.G.E.R._'s permissionless mobile compute and storage through the early-stage Solana Saga Mobile. This enhancement supports our existing ecosystem of builders using shdwDrive in mobile apps. _D.A.G.G.E.R._ Mobile will bring distributed ledger technology for compute and storage to smartphones amid growing AI and gaming demands. We aim to make _D.A.G.G.E.R._, including shdwDrive and mobile implementations, a practical choice for mobile and edge AI solutions, aligning with our roadmap.



## **Putting it all together**

In the evolving landscape of distributed ledger technologies (DLT), GenesysGo's D.A.G.G.E.R. stands out with its focus on data authenticity, availability and scalability — key advancements that addresses the real-world practicality and usability challenges faced by earlier blockchain systems. D.A.G.G.E.R.'s architecture is built upon the principle of data scalability, ensuring that it can handle the increasing demands of modern applications without sacrificing speed, stability, or security.

The modular and versatile nature of D.A.G.G.E.R. is designed to offer seamless interoperability and composability within the Web3 ecosystem. By prioritizing data efficacy as a design principal, D.A.G.G.E.R. enables a level of performance and flexibility that empowers developers to build decentralized applications that are not only trustless and permissionless but also resilient to censorship.

By embracing this data-centric approach, GenesysGo is paving the way for a new era of DLT that is equipped to meet the diverse and complex needs of today's digital world. This commitment to advancing data availability and scalability positions D.A.G.G.E.R. as a pioneering force in the drive towards practical, efficient, and accessible decentralized technologies.

## **shdwDrive Use Cases**

shdwDrive caters to a wide range of storage needs:

* Web hosting & content management
* Social media platforms
* Archival & backup solutions
* Datasets for scientific and historical research
* AI applications and model storage
* Personal & editable storage space
* Specialized distributed ledger implementations
* Blockchain archival

[**Start Building on shdwDrive!**](../build/shadow-drive.md)

## **Our Team**

We have a talented team of senior professionals across multiple fields including software sciences, distributed ledger specialists, product development, systems and cloud engineering, business and finance. We are focused on product quality, reliability, and speed to production.

We are eagerly pushing forward our goals with excitement and focus. As we look forward and execute on our agenda, continuing to support the ecosystem of Shdw builders is a top priority. With great community engagement and feedback, we frequently push versions and fixes. See how we continually improve quality, function, and security of the Shdw codebase here: [**Review change logs**](../reference/change-logs.md)
