---
description: Intructions on becoming a Shadow Operator.
---

# Shadow RPC Set Up

[1 - Configure Hardware](1-prep-your-drives.md)

[2 - Configure Solana Environment](2-prep-your-solana-env.md)

[3 - Start and Monitor Your Node](3-start-and-monitor-your-node.md)

[4 - Join the Shadow RPC Network](4-join-the-shadow-rpc-network.md)

## Prerequisites

<details><summary>Review Skills</summary>

* Define what SSH is
* What is the difference between baremetal and a virtual machine
* What is latency in terms of a network? What is latency in terms of a disk?
* What is the difference between an HDD, SSD, and nVME?
* What Ubuntu utility is used to create disk partitions?
* What Ubuntu utility is used to create filesystems on top of disk partitions?
* What would you pipe an Ubuntu log file to if you wanted to search the log file for a specific keyword?
* Nano or Vim?
* What is swap (related to memory)?

Running a Solana RPC is best suited for a systems administrator with at least 1 year of experience working with cloud and Linux technologies. Sure, some unicorns could get by with less experience, and yes, we are here to help!
</details>

<details><summary>Review Hardware Requirements</summary>

### I have a really powerful Desktop PC - can I run it on that?

No.

Solana RPCs are nothing to mess with. They do almost all of the things that a Solana validator nodes do PLUS they handle almost all of the lookup requests. Ever opened your Phantom wallet and waited for the balances to load? That's because it was blowing up an RPC requesting all of the balances and SPL tokens and NFTs in your wallet.

Now imagine that happening for all the wallets everywhere in the world. That's an RPC, and that's why your desktop PC will commit seppuku if you try to run a Solana RPC on it.

### So what's it take?

* More important than anything is redundancy. You run servers in data centers because you get:
  * Dual power circuits from 2 separate power companies
  * Dual battery backups
  * Dual ISPs
  * Dual cooling and air conditioning flows

You don't have those things at home, so just know going into this that if someone is paying you for a - and this is a keyword here - _**\*Premium\***_ service such as _**\*Premium\***_ RPC, you have an obligation to provide just that.

So what data center should you use and where to begin this journey? 👇

**Apply for access to data centers through the Solana Server Program (requires KYC!)** [**https://solana.org/server-program**](https://solana.org/server-program)****

**You can alternatively explore other bare metal providers without going through Solana Server program, like** [**Latitude**](https://latitude.sh)**.**

Once you've gone through that, you will be given a catalog of servers and data centers to pick from. I'm here to tell you right now, most of those offerings work great for validators - they do NOT work great for RPCs. Never forget, RPCs require a hoss of a server.

We recommend:

* AMD EPYC 7502P 32 Core CPU _**or better**_ (AMD EPYC is preferred over Intel, and the 7443 will work)
* 512 GB RAM minimum, _**1 TB RAM preferred**_
* Disk layout:
  * 1x OS Drive that will also hold Solana logs. 128GB min, _**256GB+ preferred**_
  * 2x 1TB nVME drive minimum, _**2x 3.9TB nVME preferred**_
* And another thing to watch out for is bandwith usage. Depending on the region, some of our nodes can transmit up to 10 TB outbound per day, which can rack up a big bill if your hosting provider charge by the TB.

_**The preferred node that most operators have been using is currently the Equinix EQ-6 or Latitude s3.large.**_

_**NOTE: IN ADDITION to the hardware requirements above, it is also required that all operators stake 10,000 SHDW per node that they want to operate!!**_

</details>

{% hint style="danger" %}
_**PLEASE NOTE: We are currently pausing Shadow Operator registration as we kickstart the Premium RPC program with our long time alpha testers. Please check back SOON**_
{% endhint %}


## Why Shadow RPCs on Solana?

One of the most interesting aspects of Solana's architecture is the RPC. It's weird.

An RPC, on its own, stores and performs all of the same tasks as an actual Solana validator with the exception of actual voting on new blocks. It also handles the bulk of data lookups for the Solana blockchain. Because it handles all of the same traffic as a validator but the additional traffic of just looking up data (like every time you open Phantom and it retrieves your balances), Solana RPCs get clobbered and they have to be much, much bigger machines than typical Solana validators.

The kicker - there is no reward built into the Solana protocol for running an RPC. If you stand up an RPC right now, you will get nothing for doing that on its own.

What's the result? There aren't many Solana RPCs in existence. And the catch? People still need RPCs. App developers _NEED_ RPCs, and they need those RPCs to be rock solid.

Other RPC networks force you to choose between performance or decentralization. The Shadow RPC network provides all the benefits of decentralization without sacrificing speed, stability, or security.