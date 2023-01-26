# The CLI

The CLI is probably the easiest way to interact with Shadow Drive on an a la carte basis. You can certainly use your favorite shell scripting language, or just type the commands one at a time. Whatever floats your boat. For test driving Shadow Drive, this is the best way to get started.

## Install the Shadow Drive CLI

Prerequisites: Install [NodeJS LTS 16.17.1](https://nodejs.org/en/download/) on any OS. And that's it. Then run the following command

```bash
npm install -g @shadow-drive/cli
```

Your output will look like this:

![](<../.gitbook/assets/image (4).png>)

_As an alternative, a community member created and open-sourced their own CLI that is based on Rust instead of TS:_ [_https://github.com/ebrightfield/shadow-drive-rust-cli_](https://github.com/ebrightfield/shadow-drive-rust-cli) __&#x20;

_Also enjoy this video-based guide if docs aren't quite your style_

{% embed url="https://www.youtube.com/watch?v=MfSuzFDDQ30" %}

### Install the Solana CLI

In order to interact with Shadow Drive, we're going to need a Solana wallet and a way to interact with the Solana blockchain. NOTE: The Shadow Drive CLI uses it's own RPC configuration. It does not use your Solana environment configuration.

Check [HERE for the latest version](https://docs.solana.com/cli/install-solana-cli-tools), as well as install methods for Windows if you are working with Windows (but seriously, no WSL? C'mon now).

```bash
sh -c "$(curl -sSfL https://release.solana.com/v1.14.3/install)"
```

Upon install, follow that up immediately with:

```bash
export PATH="/home/sol/.local/share/solana/install/active_release/bin:$PATH"
```

### Create a Keypair file

We need to have a keypair in .json format to use the Shadow Drive CLI. This is going to be the wallet that owns the storage account. If you want, you can convert your browser wallet into a .json file by exporting the private keys. Solflare by default exports it in a .json format (it looks like a standard array of integers, \[1,2,3,4...]. Phantom, however, needs some help and [we have just the tool to do that](https://gist.github.com/tracy-codes/f17e7ed8acfdd1be442f632f5b80763c).&#x20;

If you want to create a new wallet, just use

```
solana-keygen new -o ~/shdw-keypair.json
```

<figure><img src="../.gitbook/assets/2022-10-11_13-00-03.png" alt=""><figcaption></figcaption></figure>

You'll need to send a small amount of SOL and SHDW to that wallet address to proceed! The SOL is used to pay for transaction fees, the SHDW is used to create (and expand) the storage account!

### Context-Sensitive Halp

Like most CLIs, Shadow Drive comes with help. All shadow drive commands begin with `shdw-drive`.

```
shdw-drive help
```

The above command will yield the following output

<figure><img src="../.gitbook/assets/2022-10-11_13-17-13.png" alt=""><figcaption></figcaption></figure>

You can get further help on each of these commands by typing the full command, followed by the `--help` option.

```
shdw-drive create-storage-account --help
```

### Create a Storage Account

This is one of the few commands where you will need SHDW. Don't worry - before the command executes, it will prompt you as to how much SHDW will be required to reserve the storage account. There are three required options (which is a really weird thing to type, ya know? How can they be options if they're required?):

* \-kp, --keypair     Path to wallet that will create the storage account
* \-n, --name          What you want your storage account to be named. (Does not have to be unique)
* \-s, --size             Amount of storage you are requesting to create. Should be in a string like '1KB', '1MB', '1GB'. Only KB, MB, and GB storage delineations are supported currently.

Used in a sentence like a spelling bee?&#x20;

```
shdw-drive create-storage-account -kp ~/shdw-keypair.json -n "pony storage drive" -s 1GB
```

### Yeet a File to Shadow Drive

There are only two required options (again with the required options thing) for this command:

* \-kp, --keypair     Path to wallet that will upload the file
* \-f, --file               File path. Current file size limit is 1GB through the CLI.

Now you may be thinking to yourself "what if I have more than one storage account? How does it know which storage account to put it on?" If you weren't thinking that already, now you are. Don't worry - upon firing off the command, it will present you with a list of owned storage accounts to choose from. You can optionally provide your storage account address with:

* \-s, --storage-account     Storage account to upload file to.

Used in a sentence

```
shdw-drive upload-file -kp ~/shdw-keypair.json -f ~/CelsiusAccountHolders.csv
```

### Yeet Multiple Files to Shadow Drive

A more realistic use case is to upload an entire directory of, say, NFT images and metadata. It's basically the same thing, except we point the command to a directory.

Requirements (I'm not saying options anymore. That's just stupid):

* \-kp, --keypair     Path to wallet that will upload the files
* \-d, --directory    Path to folder of files you want to upload.

Options (because these are optional. See? Doesn't that make more sense? Why is everything an \[option] on command line?)

* \-s, --storage-account     Storage account to upload file to.
* \-c, --concurrent              Number of concurrent batch uploads. (default: "3")

Used in a sentence

```
shdw-drive upload-multiple-files -kp ~/shdw-keypair.json -d ~/ponyNFT/assets/
```

### Edit a File (aka Overwrite a File aka Replace a File)

This command is used to replace an existing file _that has the exact same name._ If you attempt to upload this file using `edit-file` and an existing file with the same name is not already there, the request will fail.&#x20;

There are three requirements for this command:

* \-kp, --keypair     Path to wallet that will upload the file
* \-f, --file               File path. Current file size limit is 1GB through the CLI. File must be named the same as the one you originally uploaded
* \-u, --url               Shadow Drive URL of the file you are requesting to delete

Used in a sentence

```
shdw-drive edit-file --keypair ~/shdw-keypair.json --file ~/ponyNFT/0.json --url https://shdw-drive.genesysgo.net/abc123def456ghi789/0.json
```

### Delete a File (aka unYeet a file)

This should seem pretty straightforward but let's clarify something - there is no grace period or rollback for a file deletion. Once it's deleted, it's gone for good.

There are two requirements and there aren't any options outside of the standard ones:

* \-kp, --keypair    Path to the keypair file for the wallet that owns the storage account and file
* \-u, --url              Shadow Drive URL of the file you are requesting to delete

Used in a sentence

```
shdw-drive delete-file --keypair ~/shdw-keypair.json --url https://shdw-drive.genesysgo.net/abc123def456ghi789/0.json
```

### Add Storage to Storage Account

Men only think about one thing and it involves enlargement. Look, everyone feels insecure about the size of their Shadow Drive storage account sometimes. That's why we can pay to expand it. This is the other command that consumes SHDW tokens.

There are only two requirements for this call

* \-kp, --keypair     Path to wallet that will upload the files
* \-s, --size             Amount of storage you are requesting to add to your storage account. Should be in a string like '1KB', '1MB', '1GB'. Only KB, MB, and GB storage delineations are supported currently

If you have more than one account, you'll get to pick which storage account you want to add storage to.

Used in a sentence

```
shdw-drive add-storage -kp ~/shdw-keypair.json -s 100MB
```

### Reduce Storage Account Size

Sounds like blasphemy but hear me out - what if you reserved a 1GB storage account, and then only used 500MB of it? Turns out your size is not size. You can reduce your storage account and reclaim your unused SHDW tokens this way. _**This is a two part operation**_ where you first reduce your account size, and then request your SHDW tokens. First, let's reduce the storage account size.&#x20;

There are two requirements

* \-kp, --keypair     Path to wallet that will upload the files
* \-s, --size            Amount of storage you are requesting to remove from your storage account. Should be in a string like '1KB', '1MB', '1GB'. Only KB, MB, and GB storage delineations are supported currently

Used in a sentence

```
shdw-drive reduce-storage -kp ~/shdw-keypair.json -s 500MB
```

### Claim Stake (aka Claim Unused SHDW Tokens after Reduction)

So glad I got to use that gif.&#x20;

Since you reduced the amount of storage being used in the previous step, you are now free to claim your unused SHDW tokens. The only requirement here is a keypair.

Used in a sentence

```
shdw-drive claim-stake -kp ~/shdw-keypair.json
```

### Delete a Storage Account

Can we talk about how Paul Rudd doesn't age for a second?&#x20;

\When you're ready to go scorched earth, you can tear down your storage account entirely. Upon total completion, your SHDW tokens will be returned to the wallet.&#x20;

NOTE: You have a grace period upon deletion that lasts until the end of the current Solana epoch. [Go HERE to see](https://explorer.solana.com/) how much time is remaining in the current Solana epoch to know how much grace period you will get.&#x20;

All you need here is a keypair, and it will prompt you for the specific storage account to delete.

Used in a sentence

```
shdw-drive delete-storage-account ~/shdw-keypair.json
```

### Undelete a Storage Account

They always come back. Unless the epoch has elapsed. Then they don't. But if the epoch is still active, you can undelete your storage account. Same drill as before, only need a keypair. You will be prompted to select a storage account when running the command. This removes the deletion request and it's business as usual.

```
shdw-drive undelete-storage-account -kp ~/shdw-keypair.json
```

### Make Storage Account Immutable

One of the coolest things about Shadow Drive is that you can make your storage truly permanent. With immutable storage, no file that was uploaded to the account can ever be deleted or edited. They are solidified and permanent, as is the storage account itself. You CAN still continue to upload files to an immutable account, as well as add storage to an immutable account.&#x20;

As before, the only requirement is a keypair. You will be prompted to select a storage account when running the command.

Used in a sentence&#x20;

```
shdw-drive make-storage-account-immutable -kp ~/shdw-keypair.json
```
