# Wallet

{% hint style="warning" %}
**IMPORTANT - You need to verify your identity by adding your node id to your Discord user before your node is eligible for rewards. Once that's done, then rewards will start accumulating for your node.**

**You need to have 100 SHDW at minimum across all wallets that are verified with the Discord verification system in order to maintain your verified status and therefore your place in the network.**
{% endhint %}

Identity security for shdwDrive is trustlessly handled by a local CLI-based shdwWallet that transparently publishes to the ledger. As development continues on shdwDrive v2 a key milestone will be the release of a full-featured shdwWallet which will greatly enhance users experiences of the network.

At this stage of development all economic features of testnets make use of the Solana blockchain for fast and cheap transaction execution. This Solana integration allows us to make use of third-party wallets native to Solana, and as a Solana native token $SHDW help seamlessly integrate shdwDrive v2 with both builders and users of Solana.

Each shdwNode uses an account consisting of two parts — a public key and a private key. This approach of having two keys is known as public key cryptography, a widely used cryptographic system. These are akin to a username and password in the web2 world. The D.A.G.G.E.R. protocol which runs on shdwNodes replaces usernames and passwords with these public and private keys.

## Third-Party Wallets

D.A.G.G.E.R. operates in parallel to the Solana blockchain, which allows any Solana-compatible wallet to be used with your account.

### Solana ecosystem wallets

Several browsers and mobile app-based wallets support Solana. You can find the right one for you on the [Solana Ecosystem](https://solana.com/ecosystem/explore?categories=wallet) page.

For advanced users and developers, command-line (CLI) wallets may be more appropriate:

* [Solana CLI](https://docs.solana.com/cli)

### Importing $SHDW Accounts into Solana Wallets

To bring a shdwAccount from the shdwWallet into another Solana wallet, us the `Private Key`method.

The private key method is the recommended method to extracting the key you will need to import into 3rd party wallets. Technically, the private key embeds the specific derivation path used by the shdwNode wallet.&#x20;

### Private Key Method

#### Get your account's private key from your shdwNode

1. Make sure you've downloaded the `shdw-keygen` binary as specified in the [install guide](install.md#id-3.-initial-node-configuration).
2. In the same directory as the `shdw-keygen` binary, run the following command:

```
/shdw-keygen privkey <path-to-keypair.json>
```

Where `<path-to-keypair.json>` is the path to your node's identity keypair file. For example, by default, it should be located at `~/id.json`.

3. You should have a string of random-looking characters. This is your node identity's private key. _<mark style="color:red;">**DO NOT SHARE THIS WITH ANYONE!**</mark>_ Copy this output; you will need it for the next step.

#### Import the private key into a Solana Wallet

For this, we'll demonstrate with [Phantom ](https://phantom.app/)wallet.

1. Open your Phantom Wallet.
2. Click on the hamburger menu icon that looks like this:\
   ![](<../.gitbook/assets/image (2).png>)
3. Once the side menu opens up, click the `+` icon for `Add / Connect Wallet`\
   ![](<../.gitbook/assets/image (3).png>)
4. You should now see a screen like this:\
   ![](<../.gitbook/assets/image (4).png>)
5. Click Import Private Key
6. Make sure Solana is selected as the network, you can name it whatever you'd like and then paste the private key from the output of [#get-your-accounts-private-key-from-your-shdwnode](wallet.md#get-your-accounts-private-key-from-your-shdwnode "mention").
7. Your shdwNode's identity should now be in your Solana Wallet! You can now go to [https://holders.genesysgo.com/](https://holders.genesysgo.com/), sign in with your Discord account, and once your node has been identified in the network, you will receive the `shdwOperator` role in discord automatically.
