---
description: Intructions on becoming a Shadow Operator.
---

# Shadow RPC Set Up

[1 - Configure Hardware](1-prep-your-drives.md)

[2 - Configure Solana Environment](2-prep-your-solana-env.md)

[3 - Start and Monitor Your Node](3-start-and-monitor-your-node.md)

[4 - Join the Shadow RPC Network](4-join-the-shadow-rpc-network.md)

## Prerequisite Knowledge

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

<details><summary>Slashing</summary>

Slashing is a mechanism by which a network punishes bad actors or negligent operators. The Shadow Operator smart contract has a slashing mechanism in place as well.

Health checks are sent to Shadow Operators in the form of RPC pings. Shadow Operators whose machines fail to respond to the health check pings or are behind by higher than a certain amount of slots are considered "down" and are potentially subject to slashing if they persist in that state for too long.

{% hint style="info" %}
All instances of slashing and failed health checks are handled on-chain and have been validated for consensus by the Solana Mainnet validators. Slashing is recorded on-chain and callable for review to ensure the integrity of the mechanic.
{% endhint %}

The slashing order flow looks something like this:

1. A health check is sent to the Shadow Operator's machine.
2. The Shadow Operator's machine responds back showing that it is 1550 slots behind. This Shadow Node is considered to be in a "down" state because it is not capable of properly serving network requests.
3. The Shadow Operator's machine persists in this state for several minutes.
4. The results of this health check are recorded on-chain in the Shadow Operator Staking smart contract's program account and that Shadow Node is slashed.
5. The smart contract references the current epoch on-chain and how many times that Shadow Node has been slashed during that epoch.
6. $SHDW emissions are slashed first. If a Shadow Node continually shows as being in a "down" state then it is possible for the Shadow Operator's emissions to be slashed to zero and they will earn no rewards for that epoch.
7. If no emissions are available for that epoch because the Shadow Node has consistently been responding poorly to the health check then the staking smart contract will slash the Shadow Operator's collateral.
8. If a Shadow Operator's collateral has been slashed and they fall below a minimum threshold then they are considered "underwater" on their collateral and must replenish it in order to begin earning rewards again.
9. All $SHDW tokens removed as a result of slashing are deposited into the Shadow Operator Emissions smart contract to contribute to future emissions for the Shadow Operators.
</details>

<details><summary> Staking & Collateral</summary>

{% hint style="warning" %}
Initial collateral requirements are set at 10,000 $SHDW. In order to earn rewards collateral must be staked and emissions are no longer earned if the collateral is unstaked.&#x20;

This is subject to change as the Shadow Operator Program leaves the Devnet period and goes fully live on Mainnet.

A warming up and cooling off period exists and exactly mirrors the warming up and cooling off periods of Solana Validators.
{% endhint %}

As said, the stability of the Solana network and the execution of requests made by Solana users are the highest priorities for Shadow Operators.

To that end, the Shadow Operator smart contract employs a staking mechanism by which Shadow Operators are required to put up "collateral". This was a concept we borrowed from the Filecoin network. It is important, especially in the early Mainnet beta phases of the Shadow Operator program, that Shadow Operators understand the importance of the role they play in the ecosystem.

The collateral mechanism also allows new Shadow Operators to come online and immediately start earning rewards without having to go through a period of earning little to no rewards because they have to do something like attract enough stake first.

Shadow Operators who fail to keep up with their machine, consistently serve requests poorly, or attempt malicious activities, will not only earn less in $SHDW emissions but will also potentially have their collateral slashed.

For more on slashing please see the "Slashing" section.
</details>

<details><summary>Emissions</summary>

Shadow Operators receive emissions of USDC every 30 days. This directly corresponds to the client billing cycle. Since smart contracts power the billing and emissions the entire process is transparent and verifiable on chain.

Payments from clients are aggregates on a regional basis and so therefore emissions to operators are based on that same region. For example, if there are 100 clients in the New York region being served by 10 operators. The emissions to operators would in theory be [100 client payments]/[10 operators]. Since not all 10 operators would have 100% uptime, the formula is slightly more complicated to account for operator delinquency. The delinquency will differ per operator depending on the performance of their machine and its overall stability in serving requests. This is tracked and provided on the [Shadow Operator Leaderboard]().

{% hint style="warning" %}
$SHDW payments are not guaranteed. Being a Shadow Operator is no different than running a Solana Validator in terms of needing to contribute to the network.

Solana Validators that cannot attract stake and are consistently offline will see themselves earning fewer and fewer rewards. Similarly, Shadow Operators whose machines are not consistently online or are serving requests poorly will also earn fewer and fewer rewards.
{% endhint %}

</details>
