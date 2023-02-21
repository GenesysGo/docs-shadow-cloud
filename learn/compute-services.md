---
description: Truly decentralized RPC read-access to the Solana Blockchain for individual and enterprise clients.
---

# **Shadow RPC**

## **Introduction**

You're off to a great start by choosing the only highly performant decentralized RPC network. Other RPC networks force you to choose between performance or decentralization. The Shadow RPC network provides all the benefits of decentralization without sacrificing speed, stability, or security.

One of the things that you will find out really quickly is that when your end user clicks the "Submit" button in their wallet, your app will need to build a transaction and send it to an RPC. Furthermore, if your application is thriving with users you will find their activity generates huge read-loads and consistly high throughput that needs horizontal back-end elasticity. If you try to rely on public, free RPCs, your dApp will crash and burn the first time it comes under any sort of of load.

**Built on a network of independent operators, Shadow’s Solana RPCs truly decentralize your back-end API with payments for compute going directly to ecosystem operators.**

**[Try it out for free!](shadow-rpc/README.md)**

[Shadow Operator Leaderboard:]() live view of operators and their metrics


## **What is an RPC and why is "read-access" important?**

One of the most interesting aspects of Solana's architecture is the RPC. An RPC, on its own, stores and performs all of the same tasks as an actual Solana validator with the exception of actual voting on new blocks. It also handles the bulk of data lookups for the Solana blockchain. Because it handles all of the same traffic as a validator but the additional traffic of just looking up data (like every time you open Phantom and it retrieves your balances), Solana RPCs get clobbered and they have to be much, much bigger machines than typical Solana validators.

They should be reliable, redundant, and performant without sacrificing key aspects of decentralization. The kicker - there is no reward built into the Solana protocol for running an RPC. If you stand up an RPC right now, you will get nothing for doing that on its own.

What's the result? There aren't many Solana RPCs in existence. And the catch? People still need RPCs. App developers _NEED_ RPCs, and they need those RPCs to be rock solid.

Oh sure, there are free RPCs out there, but you will find out quickly (if you haven't already) that these poor servers are under such heavy load all of the time that they provide a poor, inconsistent experience - the kind of experience that will make YOUR end users stay far away from you dApp.

As a dApp makes a call into the Solana blockchain, the preference of the developer is that the RPC is dedicated to handling nothing more than their app's requests. This is where Premium RPC comes in.

## **Premium RPC**

Premium RPC is the method and market by which app developers can contract directly with an RPC operator to handle their dedicated traffic.

The Shadow Operators must comply with high standards of hardware and uptime. A custom health monitoring protocol runs across the network ensuring only the healthiest operators are performing client tasks. This ensures reliability, stability, and offers an automated handling of machine downtime and planned maintenance.

Operators are rewarded based on overall health and uptime, and slashes if not serving request or reporting sub-par metrics to the health monitoring protocol. 

Shadow Operators register and manage their nodes through the Shadow PortaL. The current "alpha" pool of Operators have been running reliably for over half a year, serve over 200 registered accounts, and have proven themselves as reliable and performant. 

**As the Shadow RPC offering grows in demand, the pool will re-open for additional operators to join.**

**[Become and Shadow Operator!]()**

## **Access to Enhancements**

In effort to make building an deploying as easy as possible, Premium RPC users have access to the following enhancement.

* Global Server Load Balancing
* Geo-DNS routing and failover
* RPC health monitoring and auto-routing
* Web3 friendly payment integration

Additional enhancements releasing soon:

* indexing
* custom API routes


# **Future Compute Services**

Utilizing DAGGER as an orchestration / oracle protocol, we are capable of networking together decentralized commodity clouds (Shadow Operator virtual machines) under a frond end UI/UX experience that effectively decetralizes virtual machine provisioning. This is the same as Amazon EC2 instances, or Digital Ocean Droplets, except the back end resources and "network stack" are owned and operates trustlessly by individuals / entities who are Shadow Operators! This is currenlty in proof-of-concept and under development for the future. We revealed this technology as the Solana Breakpoint 2022 event.

**[Check out the Shadow VMs Demo from Breakpoint]()**