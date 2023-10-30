---
description: An introduction to the ShdwDrive and its implementations
---

# Learn

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td></td><td align="center"><strong>Design</strong></td><td align="center">v1.5</td><td><a href="storage-services/design.md#shadow-drive-v1.5-is-born-building-tools-and-listening-to-developer-feedback">#shadow-drive-v1.5-is-born-building-tools-and-listening-to-developer-feedback</a></td><td><a href="../.gitbook/assets/Design1.png">Design1.png</a></td></tr><tr><td></td><td align="center"><strong>D.A.G.G.E.R.</strong></td><td align="center">Distributed Ledger Tech</td><td><a href="dagger.md#introduction">#introduction</a></td><td><a href="../.gitbook/assets/Dagger2.png">Dagger2.png</a></td></tr><tr><td></td><td align="center"><strong>Use Cases</strong></td><td align="center">ShdwDrive Capability</td><td><a href="./#shadow-cloud-use-cases">#Shdw-cloud-use-cases</a></td><td><a href="../.gitbook/assets/Usecases.png">Usecases.png</a></td></tr></tbody></table>

## **Mission**

Our mission is to provide a high-performance and affordable cloud storage platform, enhanced with D.A.G.G.E.R. distributed ledger technology that empowers businesses and developers of all aspects to build secure, performant and scalable Web2/Web3 applications.

## **Objectives**

Our objectives are to continually evolve a stable high-performance ShdwDrive Storage Platform powered by a decentralized network of operators. Shdw is the only commodity cloud network designed to democratize the hundreds of billions earned each year by traditional cloud platforms without sacrificing performance. Through thoughtful design, the ShdwDrive will offer multiple service options which leverage our distributed ledger technology. The result is vertically integrated storage and compute offering that greatly improves developer experience. This will also enable the offering of distributed ledger technology "as-a-service" giving builders and businesses more granular options to customize their desired degree of Web2 or Web3 exposures.

We will focus on constantly improving the developer experience by identifying and implementing industry interoperability standards. We will maintain S3-compatibility and an open-source SDK. Being easy to use means building bridges across protocols and APIs that enable the ShdwDrive to be accessed directly and easily through popular builder tools and SDKs. Whether you are building an AI generative art program, training your enterprise NLP model, or crafting a layer-1 Rust smart-contract in need of off-chain compression, our objective is to support the popular tools that make building easier.

## **The Basics of ShdwDrive**

The Shdw Ecosystem is a series of trustless infrastructure layers focused on a path to decentralizing the traditional cloud stack. Using distributed ledger technology, ShdwDrive will take advantage of high-powered mobile storage and compute as well as rapidly reducing costs of enterprise grade data center storage. We call this core technology D.A.G.G.E.R.

Powered by Shdw Operators, the network provides builders and businesses with storage and compute solutions to optionally expand their web3 presence, or just simply use some of the most highly performant distributed ledger technology for its low cost and resiliency in business.

Now cloud compute makes up $400,000,000,000 (that's billion with a B) per year business, and is expected to increase by another $1T by 2030.

The Shdw Platform is a stable high-performance cloud platform on a path to be fully powered by a decentralized network of operators. Unlike traditional cloud platforms, network-generated revenues are sent to Shdw Operators. The Shdw Platform is the only commodity cloud network designed to democratize the hundreds of billions earned each year by traditional cloud platforms without sacrificing performance.

In order to execute this trustless decentralized network, we built our own highly performant consensus and orchestration protocol called D.A.G.G.E.R.

## **D.A.G.G.E.R.**

The foundation of the Shdw Ecosystem is D.A.G.G.E.R., a trustless decentralized consensus protocol that expands the ShdwDrive platform beyond standard data-center enterprise compute and storage offerings, enabling optional decentralized offerings of compute, mobile compute, and decentralized storage - all while pushing the limits of performance, quality, and security.

D.A.G.G.E.R. stands for Directed Acyclic Gossip Graph Enabling Replication (of compute and storage).

[**Learn about D.A.G.G.E.R**](dagger.md)

## **Storage**

ShdwDrive takes open source software-defined object storage and integrates it with Solana’s Proof of History consensus mechanism, and then decentralizes it. This allows it to achieve extremely fast I/O speeds, massive scalability, and data integrity. At the same time, it retains the benefits of the trustlessness, enhanced security, full transparency, and decentralization that blockchain technology brings to the table.

Ultimately, ShdwDrive’s implementation of D.A.G.G.E.R. will produce a permissionless, trustless, decentralized distributed storage network that exists in perpetuity without needing to rely on the direct efforts of a centralized team to grow and expand. It will take time, as the path for open sourcing hardware infrastructure is different from that of software, but ShdwDrive’s final form will be one in which the community of businesses and builders drive the direction, enhancements, and future of ShdwDrive.

The current [design](storage-services/design.md#present-design-considerations) of ShdwDrive makes embedding storage into applications easy, robust, and natively interfaced into the Solana blockchain. The SDKs support [Javascript](../build/shadow-drive/sdk-javascript.md), [Rust](../build/shadow-drive/sdk-rust.md), and [Python ](../build/shadow-drive/sdk-python.md)all on top of fast and reliable API. Use ShdwDrive to store your 800gb model weights, 10 TB training data and all the generated images your users generate via our [Python](../build/shadow-drive/sdk-python.md) SDK. Or maybe you just want to store your historical geyser plugin output and state, and any relevant program accounts and transactions. If you are building an application in need of back-end storage, secondary storage, or a client-facing storage option then our robust [Javascript SDK](../build/shadow-drive/sdk-javascript.md) has you covered. For all use cases, we are focused on making fast and reliable storage incredibly easy to build with.

[**Start Building on ShdwDrive!**](../build/)

[**Pricing**](storage-services/#pricing)

## **Compute**

Utilizing _D.A.G.G.E.R._ as an orchestration / oracle protocol, we are capable of networking together decentralized commodity clouds (Shdw Operator virtual machines) under a front end UI/UX experience that effectively decentralizes virtual machine provisioning. This is similar in concept as an Amazon EC2 instances, or Digital Ocean Droplets, except the back end resources and "network stack" are owned and operated trustlessly by individuals / entities who are Shdw Operators! This is currently in proof-of-concept and under development for the future. We revealed this technology at the Solana Breakpoint 2022 event. With over 2,000 attendees we opened showcased live demonstrations of trustless virtual machine provisioning powered by a decentralized group of micro-clouds. Developers were able to build on these virtual machines for the span of the conference, experiencing first hand the exciting direction our platform is headed.

[**Check out the Shdw VMs Demo from Breakpoint**](../reference/breakpoint-2022-demo.md)

As mobile compute power and data speeds surge, we're developing _D.A.G.G.E.R._'s permissionless mobile compute and storage through the early-stage Solana Saga Mobile. This enhancement supports our existing ecosystem of builders using ShdwDrive in mobile apps. _D.A.G.G.E.R._ Mobile will bring distributed ledger technology for compute and storage to smartphones amid growing AI and gaming demands. We aim to make _D.A.G.G.E.R._, including ShdwDrive and mobile implementations, a practical choice for mobile and edge AI solutions, aligning with our roadmap.

## **Roadmap**

There are three areas we focus our developments:

* The ShdwDrive implementation of _D.A.G.G.E.R._
* The Mobile implementation of _D.A.G.G.E.R._ and ShdwDrive
* The combination of the above two to support AI evolution

## **Putting it all together**

"On-chain" versus "off-chain" is a concept that needs to evolve in order for Web3 technology to meaningfully gain user adoption. Blockchain is a type of decentralized ledger technology (DLT), but it's not the only DLT anymore. "On-DLT" versus "off-DLT", that's what matters. This is not to take away from the significance of blockchain or its impact on the world. Instead, it's merely to say that the subset of technologies falling under the DLT umbrella has expanded in the 14 years since blockchain first appeared. The right tool for the right job matters.

Our hope is that ShdwDrive and D.A.G.G.E.R represent a first step on the path to creating composability in Web3. Not just composability between one blockchain and another... instead, zoom out and think about composability between all the different systems underneath the DLT umbrella. By creating interoperability between Solana and _D.A.G.G.E.R._ the result is a true Web3 cloud platform that doesn't sacrifice speed, stability, and/or security in order to gain trustlessness, decentralization, permissionlessness, and/or censorship resistance.

Decentralized Ledger Technology should be thought of as a superset of technologies that can be selected and assembled in various combinations to satisfy specific user requirements. So what are some of these user requirements? Here are some real world use cases to start:

## **ShdwDrive Use Cases**

* Storage
  * Web hosting & content management
    * Managing static content at high volumes that can elastically scale with your needs makes ShdwDrive a great solution for web content, media, images, and other unstructured data types.
  * Social media
    * Temporarily or permanently store message history and embedded media, stream video content from user uploads, and store and deliver vast amounts of images and avatars to enhance the Web3 efficacy of your app.
  * Archival & back-up
    * By 3x replicating and erasure coding encrypted records across a decentralized network, ShdwDrive can preserve valuable records from bad actors.
  * Datasets
    * Some information is too important for humanity to lose. From scientific research projects to historical documentation, cryptographic proofs guarantee a dataset’s integrity and availability forever. Imagine if the world’s most important libraries couldn’t be destroyed because the information has been distributed across thousands of unique locations worldwide.
  * AI implementation
    * From training models, to backing up data sets, to virtually mounting the storage accounts, ShdwDrive will offer fast, reliable, transparent methods to support many AI data-processing applications by lowering costs of housing huge amounts of data and offering extremely high read/write more cheaply than other decentralized competitors.
  * Personal & editable storage space
    * With optional mutability, you are free to create and delete files as you see fit. With low costs and many front-end user interfaces to choose from, ShdwDrive can be your personal expanded storage space.
  * Specialized distributed ledger implementations
    * The current [design](storage-services/design.md) of ShdwDrive makes embedding storage into applications easy, robust, and natively integrated into the Solana blockchain. Use ShdwDrive to store your 800gb model weights, 100 TB training data and all the generated images your users generate via our [Python](../build/shadow-drive/sdk-python.md) SDK. Or maybe you just want to store your historical geyser plugin output and state, and any relevant program accounts and transactions. If you are building an application in need of back-end storage, secondary storage, or client-facing storage option then our robust [Javascript SDK](../build/shadow-drive/sdk-javascript.md) has you covered. For all use cases, we are focused on making fast and reliable storage incredibly easy to build with.
* Compute
  * Application Building _(future release)_
    * Delivery on-chain data to front-end business applications with speed and reliability.
  * Mobile Development _(future release)_
    * Power back-end mobile remote procedural calls to enable users to interact with their favorite blockchain or web3 mobile application.
  * Enterprise VMs _(future release)_
    * White-glove handling of bare metal and virtual provisioning for specialized distributed ledger technology implementations.

[**Start Building on ShdwDrive!**](../build/shadow-drive/)

[**See what others are building on ShdwDrive!**](broken-reference)

## **Shdw Operators**

**(currently in private alpha)**

Shdw Operators are the decentralized backbone of the Shdw commodity cloud. Operators lease their high-performance compute, storage, and bandwidth. Businesses, projects, and/or individual developers can trustlessly provision network resources. User payments are sent directly to Shdw Operators. Shdw Operators who ensure their resources are always available and performant earn more. Shdw Operators who are inconsistent earn less.

At the foundation of everything are the Shdw Operators who run the nodes that power the Shdw Platform. Operators can connect new nodes to the network, manage their existing servers, or view the overall status of other Shdw Operators – all through the one easy-to-use portal.

Currently in development are a series of Shdw Operator node roles including resolvers, ingestors, storers, cachers. These roles work together under the orchestration of _D.A.G.G.E.R._ to form a permissionless distributed platform that supports both ShdwDrive services and mobile storage being developed.

## **Our Team**

We have a talented team of senior professionals across multiple fields including software sciences, distributed ledger specialists, product development, systems and cloud engineering, business and finance. We are focused on product quality, reliability, and speed to production.

We are eagerly pushing forward our goals with excitement and focus. As we look forward and execute on our agenda, continuing to support the ecosystem of Shdw builders is a top priority. With great community engagement and feedback we frequently push versions and fixes. See how we continually improve quality, function, and security of the Shdw codebase here: [**Review change logs**](../reference/change-logs.md)
