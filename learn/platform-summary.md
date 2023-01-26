---
description: What can you do in the Shadow Cloud?
---

# Platform Summary

At it's core, Shadow offers 2 main products. Here are their summaries:

### Shadow Drive

Everyone begins their cloud journey working with storage. Maybe that's Dropbox, or OneDrive, or Google Drive, or iCloud drive. But that's definitely where everyone's journey starts. So what is the web3 equivalent to those? Shadow Drive.&#x20;

Using any Solana wallet, and a little bit of $SHDW, users can carve out permanent, dedicated storage to store their files (and it doesn't even hash the filename!).&#x20;

Beyond that, any software developer that wants to build a dApp that requires some sort of file storage (because the app creates data, or because the users upload data - whatever!), Shadow Drive comes with a full fledged SDK (JS or Rust) for baking storage capability straight into your dApp.

### Shadow RPC

One of the most interesting aspects of Solana's architecture is the RPC. It's weird.&#x20;

An RPC, on its own, stores and performs all of the same tasks as an actual Solana validator with the exception of actual voting on new blocks. It also handles the bulk of data lookups for the Solana blockchain. Because it handles all of the same traffic as a validator but the additional traffic of just looking up data (like every time you open Phantom and it retrieves your balances), Solana RPCs get clobbered and they have to be much, much bigger machines than typical Solana validators.

The kicker - there is no reward built into the Solana protocol for running an RPC. If you stand up an RPC right now, you will get nothing for doing that on its own.

What's the result? There aren't many Solana RPCs in existence. And the catch? People still need RPCs. App developers _NEED_ RPCs, and they need those RPCs to be rock solid.

Oh sure, there are free RPCs out there, but you will find out quickly (if you haven't already) that these poor servers are under such heavy load all of the time that they provide a poor, inconsistent experience - the kind of experience that will make YOUR end users stay far away from you dApp.

As a dApp makes a call into the Solana blockchain, the preference of the developer is that the RPC is dedicated to handling nothing more than their app's requests. This is where Premium RPC comes in.&#x20;

Premium RPC is the method and market by which app developers can contract directly with an RPC operator to handle their dedicated traffic.

