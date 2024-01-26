---
description: >-
  The current testnet phase now allows for trustless shdwOperator to join, help
  test, and earn rewards!
---

# Operate

<table data-view="cards"><thead><tr><th></th><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td></td><td align="center"><strong>Install</strong></td><td align="center"></td><td><a href="install.md">install.md</a></td><td><a href="../.gitbook/assets/SHDW-15.png">SHDW-15.png</a></td></tr><tr><td></td><td align="center"><strong>Wallet</strong></td><td align="center"></td><td><a href="wallet.md">wallet.md</a></td><td><a href="../.gitbook/assets/SHDW-16.png">SHDW-16.png</a></td></tr><tr><td></td><td align="center"><strong>Monitor</strong></td><td align="center"></td><td><a href="monitoring-stack.md">monitoring-stack.md</a></td><td><a href="../.gitbook/assets/SHDW-17.png">SHDW-17.png</a></td></tr><tr><td></td><td align="center">FAQ</td><td align="center"></td><td><a href="node-faq.md">node-faq.md</a></td><td><a href="../.gitbook/assets/SHDW-19.png">SHDW-19.png</a></td></tr></tbody></table>

## To operate a shdwNode you must:

1. [Install ](https://docs.shdwdrive.com/operate/install)the latest version GenesysGo's Directed Acyclic Gossiping Graph Enabling Replication (D.A.G.G.E.R.).
2. [Create ](https://docs.shdwdrive.com/operate/wallet)a wallet
3. Acquire 100 SHDW in your shdwNode wallet connected to your Discord user. [Read this Note](https://docs.shdwdrive.com/operate/node-faq#q-how-are-earnings-paid-out-and-how-do-i-claim-my-shdwoperator-earnings).
4. [Verify ](https://docs.shdwdrive.com/operate#discord-verification)your identity to the network (required to earn rewards).
5. [Monitor ](https://docs.shdwdrive.com/operate/monitoring-stack)and maintain your shdwNode and stay up to date with announcements. View the [leaderboard](https://testnet.shdwdrive.com/status-dashboard).
6. [Read FAQ](node-faq.md#testnet-2) about the current testnet.
7. [Claim](https://testnet.shdwdrive.com/operator-rewards) rewards.

{% hint style="warning" %}
**IMPORTANT - You need to add your node id to your Discord user before your node is eligible for rewards. Once that's done, then rewards will start accumulating for your node.**

**You need to have 100 SHDW at minimum across all wallets that are verified with the Discord verification system in order to maintain your verified status and therefore your place in the network.**
{% endhint %}

## Testnet Information:

[Testnet 2: Release Overview](https://www.shdwdrive.com/blog/shdwdrive-v2-incentivized-testnet)

[Testnet 1: Release Overview](https://www.shdwdrive.com/blog/dagger-testnet-release)

[D.A.G.G.E.R. Hammer](https://dagger-hammer.shdwdrive.com/)

Running a shdwNode will allow you to trustlessly participate in the testnets which powers the Hammer interface for shdwDrive located [here](https://dagger-hammer.shdwdrive.com/). We encourage shdwOperators to review our blog articles and Discord channels for full context on their role.

ShdwNodes will be handling thousands of live user test transactions and trustlessly executing all modules within D.A.G.G.E.R protocol which forms consensus for shdwDrive v2. For more information on the decentralized consensus performed by shdwNodes please review the [litepaper](https://github.com/GenesysGo/dagger-litepaper/blob/main/DAGGER-Litepaper.pdf) and [blogs](https://www.shdwdrive.com/blog).

## Hardware Requirements:

**Operating system requirements are Ubuntu 22.04 LTS kernel 5.15.0. Other Linux x86 distributions may work but are not officially supported at this time.**

**Minimum hardware requirements to operate a shdwNode for Testnets are as follows:**

* **16 CPU threads (8 cores)**
* **32 GB RAM**
* **2 TB SSD (or NVMe) for data storage and high speed i/o operations\*\***
* **100mbps up/down network connection is the bare minimum**

{% hint style="info" %}
\*\*Note: Using SSDs and NVMe that are less than 2 TB will not prevent a shdwNode from running, however as we increase the file transaction volumes during testnet 2 smaller storage drives will fill up more quickly requiring more frequent restarts. Storage drives 2 TB or greater are now recommended.
{% endhint %}

## Discord Verification

Follow this guide if you'd like to connect your node identity to your Discord user. This will unlock the shdwOperator role and the wield operators channel in the GenesysGo Discord Server.

* Follow this [guide ](https://docs.shdwdrive.com/operate/wallet#importing-usdshdw-accounts-into-solana-wallets)to import your shdwNode identity keypair into a Solana UI wallet.
* Go to [https://holders.genesysgo.com/](https://holders.genesysgo.com/)
* Log in with the Discord user that you'd like to connect your shdwNode identity to
* Connect your wallet, then click "Add wallet" and sign + submit the message.
* After a few seconds, you should see a notification that the request was submitted and you should now see your shdwNode wallet in the list of wallets. The role update may take a few minutes to reflect in Discord.
