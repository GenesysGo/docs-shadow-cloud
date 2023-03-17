# **Contents**
* **[Install Shadow Drive CLI](#install-the-shadow-drive-cli)**
    * **[Video Walkthrough](#video-guide-and-walkthrough)**
    * **[Install Solana CLI](#install-the-solana-cli)**
    * **[Create a Storage Account](#create-a-storage-account)**
    * **[Upload a FIle](#upload-file-to-shadow-drive)**
    * **[Upload Multiple FIles](#upload-multiple-files-to-shadow-drive)**
    * **[Edit a File](#edit-a-file-aka-overwrite-a-file-aka-replace-a-file)**
    * **[Delete a File](#delete-a-file)**
    * **[Add Storage](#add-storage-to-storage-account)**
    * **[Reduce Storage](#reduce-storage-account-size)**
    * **[Make File Immutable](#make-storage-account-immutable)**
* **[Install Rust CLI - Experimental!](#the-rust-cli)**

## **Introduction**
The CLI is the easiest way to interact with Shadow Drive. You can use your favorite shell scripting language, or just type the commands one at a time. For test driving Shadow Drive, this is the best way to get started.

# **Install the Shadow Drive CLI**

Prerequisites: Install [NodeJS LTS 16.17.1](https://nodejs.org/en/download/) on any OS.

Then run the following command

```bash
npm install -g @shadow-drive/cli
```

### **[Video Guide and Walkthrough](https://www.youtube.com/watch?v=MfSuzFDDQ30)**

### **Install the Solana CLI**

In order to interact with Shadow Drive, we're going to need a Solana wallet and CLI to interact with the Solana blockchain. 

*NOTE: The Shadow Drive CLI uses it's own RPC configuration. It does not use your Solana environment configuration.*

Check [HERE for the latest version](https://docs.solana.com/cli/install-solana-cli-tools).

```bash
sh -c "$(curl -sSfL https://release.solana.com/v1.14.3/install)"
```

Upon install, follow that up immediately with:

```bash
export PATH="/home/sol/.local/share/solana/install/active_release/bin:$PATH"
```

### **Create a Keypair file**

We need to have a keypair in .json format to use the Shadow Drive CLI. This is going to be the wallet that owns the storage account. If you want, you can convert your browser wallet into a .json file by exporting the private keys. Solflare by default exports it in a .json format (it looks like a standard array of integers, \[1,2,3,4...]. Phantom, however, needs some help and [we have just the tool to do that](https://gist.github.com/tracy-codes/f17e7ed8acfdd1be442f632f5b80763c).

If you want to create a new wallet, just use

```
solana-keygen new -o ~/shdw-keypair.json
```
You will see it write a new keypair file and it was display the `pubkey` which is your wallet address.


**You'll need to send a small amount of SOL and SHDW to that wallet address to proceed! The SOL is used to pay for transaction fees, the SHDW is used to create (and expand) the storage account!**

### **Context-Sensitive Help**

Shadow Drive CLI comes with integrated help. All shadow drive commands begin with `shdw-drive`.

```
shdw-drive help
```

The above command will yield the following output

<figure><img src="../../.gitbook/assets/2022-10-11_13-17-13.png" alt=""><figcaption></figcaption></figure>

You can get further help on each of these commands by typing the full command, followed by the `--help` option.

```
shdw-drive create-storage-account --help
```

### **Create a Storage Account**

This is one of the few commands where you will need SHDW. Before the command executes, it will prompt you as to how much SHDW will be required to reserve the storage account. There are three required options:

`-kp, --keypair`  
* Path to wallet that will create the storage account

`-n, --name`
* What you want your storage account to be named. (Does not have to be unique)

`-s, --size`            
* Amount of storage you are requesting to create. This should be in a string like '1KB', '1MB', '1GB'. Only KB, MB, and GB storage delineations are supported.

**Example:**
```
shdw-drive create-storage-account -kp ~/shdw-keypair.json -n "pony storage drive" -s 1GB
```

### **Upload File to Shadow Drive**

There are only two required options for this command:

`-kp, --keypair`
* Path to wallet that will upload the file

`-f, --file`               
* File path. Current file size limit is 1GB through the CLI.

If you have multiple storage accounts it will present you with a list of owned storage accounts to choose from. You can optionally provide your storage account address with:

`-s, --storage-account`     
* Storage account to upload file to.

**Example:**
```
shdw-drive upload-file -kp ~/shdw-keypair.json -f ~/AccountHolders.csv
```

### **Upload Multiple Files to Shadow Drive**

A more realistic use case is to upload an entire directory of, say, NFT images and metadata. It's basically the same thing, except we point the command to a directory.

Options:

`-kp, --keypair`     
* Path to wallet that will upload the files

`-d, --directory`    
* Path to folder of files you want to upload.

`-s, --storage-account`     
* Storage account to upload file to.

`-c, --concurrent`             
* Number of concurrent batch uploads. (default: "3")

**Example:**
```
shdw-drive upload-multiple-files -kp ~/shdw-keypair.json -d ~/ponyNFT/assets/
```

### **Edit a File (aka Overwrite a File aka Replace a File)**

This command is used to replace an existing file _that has the exact same name._ If you attempt to upload this file using `edit-file` and an existing file with the same name is not already there, the request will fail.

There are three requirements for this command:

`-kp, --keypair`     
* Path to wallet that will upload the file

`-f, --file`              
* File path. Current file size limit is 1GB through the CLI. File must be named the same as the one you originally uploaded

`-u, --url`               
* Shadow Drive URL of the file you are requesting to delete

**Example:**
```
shdw-drive edit-file --keypair ~/shdw-keypair.json --file ~/ponyNFT/01.json --url https://shdw-drive.genesysgo.net/abc123def456ghi789/0.json
```

### **Delete a File**

This is straightforward and it's important to note once it's deleted, it's gone for good.

There are two requirements and there aren't any options outside of the standard ones:

`-kp, --keypair`    
* Path to the keypair file for the wallet that owns the storage account and file

`-u, --url`             
Shadow Drive URL of the file you are requesting to delete

**Example:**
```
shdw-drive delete-file --keypair ~/shdw-keypair.json --url https://shdw-drive.genesysgo.net/abc123def456ghi789/0.json
```

### **Add Storage to Storage Account**

You can expand the storage size of a storage account. This command consumes SHDW tokens.

There are only two requirements for this call

`-kp, --keypair`     
* Path to wallet that will upload the files

`-s, --size`          
* Amount of storage you are requesting to add to your storage account. Should be in a string like '1KB', '1MB', '1GB'. Only KB, MB, and GB storage delineations are supported currently

If you have more than one account, you'll get to pick which storage account you want to add storage to.

**Example:**
```
shdw-drive add-storage -kp ~/shdw-keypair.json -s 100MB
```

### **Reduce Storage Account Size**

You can reduce your storage account and reclaim your unused SHDW tokens. _**This is a two part operation**_ where you first reduce your account size, and then request your SHDW tokens. First, let's reduce the storage account size.

There are two requirements

`-kp, --keypair`     
* Path to wallet that will upload the files

`-s, --size`            
* Amount of storage you are requesting to remove from your storage account. Should be in a string like '1KB', '1MB', '1GB'. Only KB, MB, and GB storage delineations are supported currently

**Example:**
```
shdw-drive reduce-storage -kp ~/shdw-keypair.json -s 500MB
```

### **Claim Stake (aka Claim Unused SHDW Tokens after Reduction)**

Since you reduced the amount of storage being used in the previous step, you are now free to claim your unused SHDW tokens. The only requirement here is a keypair.

**Example:****
```
shdw-drive claim-stake -kp ~/shdw-keypair.json
```

### **Delete a Storage Account**


You can entirely remove a storage account from Shadow Drive. Upon completion, your SHDW tokens will be returned to the wallet.

*NOTE: You have a grace period upon deletion that lasts until the end of the current Solana epoch. [Go HERE to see](https://explorer.solana.com/) how much time is remaining in the current Solana epoch to know how much grace period you will get.*

All you need here is a keypair, and it will prompt you for the specific storage account to delete.

**Example:**
```
shdw-drive delete-storage-account ~/shdw-keypair.json
```

### **Undelete a Storage Account**

Assuming the epoch is still active, you can undelete your storage account. You only need a keypair. You will be prompted to select a storage account when running the command. This removes the deletion request.

```
shdw-drive undelete-storage-account -kp ~/shdw-keypair.json
```

### **Make Storage Account Immutable**

One of the most unique and useful features of Shadow Drive is that you can make your storage truly permanent. With immutable storage, no file that was uploaded to the account can ever be deleted or edited. They are solidified and permanent, as is the storage account itself. You can still continue to upload files to an immutable account, as well as add storage to an immutable account.

The only requirement is a keypair. You will be prompted to select a storage account when running the command.

**Example:**

```
shdw-drive make-storage-account-immutable -kp ~/shdw-keypair.json
```


# **The Rust CLI**

## **(This section is under development)

## Getting Started
```rust
/// Perform Shadow Drive operations on the command-line.
/// This CLI is written in Rust, and conforms to the interfaces
/// for signer specification available in the official
/// Solana command-line binaries such as `solana` and `spl-token`.
#[derive(Debug, Parser)]
pub struct Opts {
    #[clap(flatten)]
    pub cfg_override: ConfigOverride,
    #[clap(subcommand)]
    pub command: Command,
}

#[derive(Debug, Parser)]
pub enum Command {
    ShadowRpcAuth,
    /// Create an account on which to store data.
    /// Storage accounts can be globally, irreversibly marked immutable
    /// for a one-time fee.
    /// Otherwise, files can be added or deleted from them, and space
    /// rented indefinitely.
    CreateStorageAccount {
        /// Unique identifier for your storage account
        name: String,
        /// File size string, accepts KB, MB, GB, e.g. "10MB"
        #[clap(parse(try_from_str = parse_filesize))]
        size: Byte,
    },
    /// Queues a storage account for deletion. While the request is
    /// still enqueued and not yet carried out, a cancellation
    /// can be made (see cancel-delete-storage-account subcommand).
    DeleteStorageAccount {
        /// The account to delete
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
    },
    /// Cancels the deletion of a storage account enqueued for deletion.
    CancelDeleteStorageAccount {
        /// The account for which to cancel deletion.
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
    },
    /// Redeem tokens afforded to a storage account after reducing storage capacity.
    ClaimStake {
        /// The account whose stake to claim.
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
    },
    /// Increase the capacity of a storage account.
    AddStorage {
        /// Storage account to modify
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
        /// File size string, accepts KB, MB, GB, e.g. "10MB"
        #[clap(parse(try_from_str = parse_filesize))]
        size: Byte,
    },
    /// Increase the immutable storage capacity of a storage account.
    AddImmutableStorage {
        /// Storage account to modify
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
        /// File size string, accepts KB, MB, GB, e.g. "10MB"
        #[clap(parse(try_from_str = parse_filesize))]
        size: Byte,
    },
    /// Reduce the capacity of a storage account.
    ReduceStorage {
        /// Storage account to modify
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
        /// File size string, accepts KB, MB, GB, e.g. "10MB"
        #[clap(parse(try_from_str = parse_filesize))]
        size: Byte,
    },
    /// Make a storage account immutable. This is irreversible.
    MakeStorageImmutable {
        /// Storage account to be marked immutable
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
    },
    /// Fetch the metadata pertaining to a storage account.
    GetStorageAccount {
        /// Account whose metadata will be fetched.
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
    },
    /// Fetch a list of storage accounts owned by a particular pubkey.
    /// If no owner is provided, the configured signer is used.
    GetStorageAccounts {
        /// Searches for storage accounts owned by this owner.
        #[clap(parse(try_from_str = pubkey_arg))]
        owner: Option<Pubkey>,
    },
    /// List all the files in a storage account.
    ListFiles {
        /// Storage account whose files to list.
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
    },
    /// Get a file, assume it's text, and print it.
    GetText {
        /// Storage account where the file is located.
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
        /// Name of the file to fetch
        filename: String,
    },
    /// Get basic file object data from a storage account file.
    GetObjectData {
        /// Storage account where the file is located.
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
        /// Name of the file to examine.
        file: String,
    },
    /// Delete a file from a storage account.
    DeleteFile {
        /// Storage account where the file to delete is located.
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
        /// Name of the file to delete.
        filename: String,
    },
    /// Has to be the same name as a previously uploaded file
    EditFile {
        /// Storage account where the file to edit is located.
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
        /// Path to the new version of the file. Must be the same
        /// name as the file you are editing.
        path: PathBuf,
    },
    /// Upload one or more files to a storage account.
    StoreFiles {
        /// Batch size for file uploads, default 100, only relevant for large
        /// numbers of uploads
        #[clap(long, default_value_t=FILE_UPLOAD_BATCH_SIZE)]
        batch_size: usize,
        /// The storage account on which to upload the files
        #[clap(parse(try_from_str = pubkey_arg))]
        storage_account: Pubkey,
        /// A list of one or more filepaths, each of which is to be uploaded.
        #[clap(min_values = 1)]
        files: Vec<PathBuf>,
    },
    /// Creates an archive of metadata (runes) that can be used to summon data using the Shadow Drive Portal. Uploads data, and returns
    /// the metadata to be compiled into a smart contract.
    StoreAndCreateRunes {
        directory: PathBuf,
        target: PathBuf,
    },
}
```

### **reateStorageAccount**

```rust
Command::CreateStorageAccount { name, size } => {
                let client = shadow_client_factory(signer, rpc_url, auth);
                println!("Create Storage Account {}: {}", name, size);
                wait_for_user_confirmation(skip_confirm)?;
                let response = client
                    .create_storage_account(&name, size.clone(), StorageAccountVersion::v2())
                    .await;
                let resp = process_shadow_api_response(response)?;
                println!("{:#?}", resp);
            }
```

Response:
```rust
{
        name: String,
        size: Byte,
}
```
