# **Shadow Drive SDKs**

## **Contents**
* **[Javascript SDK](#getting-started-javascript-sdk)**
* **[Rust SDK](#getting-started-rust-sdk)**
* **[Python](#getting-started-python-sdk)**

### **Getting Started: Javascript SDK**

Let's scaffold a React app and add our dependencies

```bash
npx create-react-app shdwapp
cd shdwapp/
yarn add @shadow-drive/sdk @project-serum/anchor \
    @solana/wallet-adapter-base \
    @solana/wallet-adapter-react \
    @solana/wallet-adapter-react-ui \
    @solana/wallet-adapter-wallets \
    @solana/web3.js \
    @solana-mobile/wallet-adapter-mobile 
```

### Instantiate the Wallet and Connection the ol fashion way

Use the [Solana docs and examples here](https://github.com/solana-labs/wallet-adapter) if you need help. We're going to focus on Shadow Drive SDK in these docs, so if you need a primer on how to build a React site with Solana, we can refer you to other resources.

Solana example code:

<details>

<summary>Sample Code provided by Solana</summary>



```javascript
import { WalletNotConnectedError } from '@solana/wallet-adapter-base'; 
import { useConnection, useWallet } from '@solana/wallet-adapter-react';
 import { Keypair, SystemProgram, Transaction } from '@solana/web3.js'; 
 import React, { FC, useCallback } from 'react';
export const SendSOLToRandomAddress: FC = () => { 
const { connection } = useConnection(); 
const { publicKey, sendTransaction } = useWallet();
const onClick = useCallback(async () => {
    if (!publicKey) throw new WalletNotConnectedError();

    // 890880 lamports as of 2022-09-01
    const lamports = await connection.getMinimumBalanceForRentExemption(0);

    const transaction = new Transaction().add(
        SystemProgram.transfer({
            fromPubkey: publicKey,
            toPubkey: Keypair.generate().publicKey,
            lamports,
        })
    );

    const {
        context: { slot: minContextSlot },
        value: { blockhash, lastValidBlockHeight }
    } = await connection.getLatestBlockhashAndContext();

    const signature = await sendTransaction(transaction, connection, { minContextSlot });

    await connection.confirmTransaction({ blockhash, lastValidBlockHeight, signature });
}, [publicKey, sendTransaction, connection]);

return (
    <button onClick={onClick} disabled={!publicKey}>
        Send SOL to a random address!
    </button>
);
```

};

</details>

#### Now you can focus on building components for various Shadow Drive operations

Let's start by instantiating the Shadow Drive connection class object. This will have all the methods to do all the things, and it implements the signing wallet within the class for all the transactions.

At the simplest level, it is recommend for a React app to immediately try to load a connection to a user's Shadow Drives upon wallet connection. This can be done with the useEffect React hook.

```javascript
import React, { useEffect } from "react";
import * as anchor from "@project-serum/anchor";
import {ShdwDrive} from "@shadow-drive/sdk";
import { AnchorWallet, useAnchorWallet, useConnection } from "@solana/wallet-adapter-react";

export default function Drive() {
    const { connection } = useConnection();
    const wallet = useAnchorWallet();
    useEffect(() => {
        (async () => {
            if (wallet?.publicKey) {
                const drive = await new ShdwDrive(connection, wallet).init();
            }
        })();
    }, [wallet?.publicKey])
    return (
        <div></div>
```

Also this can be done in a simple script using Node; however, you will want to use TypeScript for your Node scripts which you will realize when we get to the uploadFile method.

```javascript
const anchor = require("@project-serum/anchor")
const {
    Connection,
    clusterApiUrl,
    Keypair,
 } = require("@solana/web3.js")
 const {ShdwDrive} = require("@shadow-drive/sdk");
 const key = require('./shdwkey.json')
 

 async function main(){
    let secretKey = Uint8Array.from(key)
    let keypair=Keypair.fromSecretKey(secretKey);
    const connection = new Connection(clusterApiUrl("mainnet-beta"),"confirmed")
    const wallet = new anchor.Wallet(keypair);
    const drive = await new ShdwDrive(connection, wallet).init();
 }

 main()
```

**NOTE: the wallet object WILL need to be an AnchorWallet object in either instance.**

#### Create a Storage Account

This implementation is effectively the same for both Web and Node implementations. There are three params that are required to create a storage account:

* name: a friendly name for your storage account
* size: The size of your storage accounts with a human readable ending containing "KB", "MB", or "GB"
* version: can be either "v1" or "v2" but you should really stick to v2 if you are creating new accounts

```javascript
//create account
const newAcct = await drive.createStorageAccount("myDemoBucket","10MB", "v2")
console.log(newAcct)
```

#### Get a list of Owned Storage Accounts

This implementation is effectively the same for both Web and Node implementations. The only parameter required is either "v1" or "v2" for the version of storage account you created in the previous step.

```javascript
const accts = await drive.getStorageAccounts("v2")
// handle printing pubKey of first storage acct
let acctPubKey = new anchor.web3.PublicKey(accts[0].publicKey)
console.log(acctPubKey.toBase58())
```

Full Response:

<details>

<summary>Output</summary>

```javascript
[
  {
    publicKey: PublicKey {
      _bn: <BN: c9217d175c7257f52a64cb9faa203e65487343720441cef2d9abcb3f8c706053>
    },
    account: {
      immutable: false,
      toBeDeleted: false,
      deleteRequestEpoch: 0,
      storage: <BN: a00000>,
      owner1: [PublicKey],
      accountCounterSeed: 0,
      creationTime: 1665690481,
      creationEpoch: 359,
      lastFeeEpoch: 359,
      identifier: 'myDemoBucket'
    }
  },
  {
    publicKey: PublicKey {
      _bn: <BN: 502aa038c1b95a8bc36c301f3ae06beda929ec3a044b0543e70d4378d4c574ea>
    },
    account: {
      immutable: false,
      toBeDeleted: false,
      deleteRequestEpoch: 0,
      storage: <BN: a00000>,
      owner1: [PublicKey],
      accountCounterSeed: 1,
      creationTime: 1665690563,
      creationEpoch: 359,
      lastFeeEpoch: 359,
      identifier: 'myDemoBucket'
    }
  }
]
```

</details>

#### Get a Specific Storage Account

This implementation is effectively the same for both Web and Node implementations. The only parameter required is either a PublicKey object or a base-58 string of the public key.&#x20;

```javascript
const acct = await drive.getStorageAccount("EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN")
console.log(acct)
```

Full Response:

<details>

<summary>Output</summary>

```javascript
{
  storage_account: 'EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN',
  reserved_bytes: 10485760,
  current_usage: 0,
  immutable: false,
  to_be_deleted: false,
  delete_request_epoch: 0,
  owner1: 'GXE2RHkdivb7pdBykiAr9mpKy1kHF1Qe7oZqnX4NwkoD',
  account_counter_seed: 0,
  creation_time: 1665690481,
  creation_epoch: 359,
  last_fee_epoch: 359,
  identifier: 'myDemoBucket',
  version: 'V2'
}
```

</details>

#### Upload a File

This is where things start to get hairy. Let's take a close look at why:

The uploadFile method requires two parameters:

* key: A PublicKey object representing the public key of the Shadow Storage Account
* data: A file - and here's the kicker - of either the File object type or ShadowFile object type

Check the intellisense popup below when hovering over the method

<figure><img src="../.gitbook/assets/Screen Shot 2022-10-13 at 7.40.37 PM.png" alt=""><figcaption></figcaption></figure>

So why is this so tricky? Because File objects are implemented in web browsers, and ShadowFile is a custom type created in TypeScript. So either you are using File in the web, or you are scripting in TS. There is not currently an implementation for uploading files written in JS using only Node.

Here is an example of how to pull it off as a React component:

<details>

<summary>React Upload component</summary>



```javascript
import React, { useState } from "react";
import { ShdwDrive } from "@shadow-drive/sdk";
import { AnchorWallet, useAnchorWallet, useConnection } from "@solana/wallet-adapter-react";
import FormData from "form-data";

export default function Upload() {
    const [file, setFile] = useState<File>(undefined)
    const [uploadUrl, setUploadUrl] = useState<String>(undefined)
    const [txnSig, setTxnSig] = useState<String>(undefined)
    const { connection } = useConnection();
    const wallet = useAnchorWallet();
    return (
        <div>
            <form onSubmit={async event => {
                event.preventDefault()
                const drive = await new ShdwDrive(connection, wallet as AnchorWallet).init();
                const accounts = await drive.getStorageAccounts();
                const acc = accounts[0].publicKey;
                const getStorageAccount = await drive.getStorageAccount(acc);

                const upload = await drive.uploadFile(acc, file);
                console.log(upload);
                setUploadUrl(upload.finalized_location)
                setTxnSig(upload.transaction_signature)
            }}>
                <h1>Shadow Drive File Upload</h1>
                <input type="file" onChange={e => setFile(e.target.files[0])} />
                <br />
                <button type="submit">Upload</button>
            </form>
            <span>You may have to wait 60-120s for the URL to appear</span>
            <div>
                {(uploadUrl) ? (
                    <div>
                        <h3>Success!</h3>
                        <h4>URL: {uploadUrl}</h4>
                        <h4>Sig: {txnSig}</h4>
                    </div>
                ) : (<div></div>)
                }
            </div>
        </div>
    )
}
```

\


</details>

And a TypeScript implementation would look like:

<details>

<summary>TS UploadFile</summary>

```javascript
const {ShdwDrive, ShadowFile} = require("@shadow-drive/sdk");
...
...
...
const fileBuff = fs.readFileSync('./mytext.txt')
const acctPubKey = new anchor.web3.PublicKey("EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN")
const fileToUpload: ShadowFile = {
    name: 'mytext.txt',
    file: fileBuff
}
const uploadFile = await drive.uploadFile(acctPubKey, fileToUpload)
console.log(uploadFile)
```

</details>

#### Upload Multiple Files

This is a nearly identical implementation to uploadFile, except that it requires a FileList or array of ShadowFiles. But one of the interesting things about this is the optional concurrency parameter.

<figure><img src="../.gitbook/assets/Screen Shot 2022-10-13 at 8.12.08 PM.png" alt=""><figcaption></figcaption></figure>

Recall that the default setting is to attempt to upload 3 files concurrently. Here you can override this and specify how many files you want to try to upload based on the cores and bandwith of your infrastructure. Don't worry - Shadow Drive can handle the heat. Can you?

#### Delete a File

The implementation of deleteFile is the same between web and Node. There are three required parameters to delete a file:

* key: the storage account's public key
* url: the current URL of the file to be deleted
* version: can be either "v1" or "v2"

```javascript
const url = 'https://shdw-drive.genesysgo.net/4HUkENqjnTAZaUR4QLwff1BvQPCiYkNmu5PPSKGoKf9G/fape.png'
const acctPubKey = new anchor.web3.PublicKey("EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN")
const delFile = await drive.deleteFile(acctPubKey,url,"v2")
console.log(delFile)
```

#### Edit a File (aka Replace a file)

<figure><img src="../../.gitbook/assets/Screen Shot 2022-10-13 at 8.28.30 PM.png" alt=""><figcaption><p></p></figcaption></figure>

The editFile method is a combo of uploadFile and deleteFile. Let's look at the params:

* key: the Public Key of the storage account
* url: the URL of the file that is being replaced
* data: the file that is replacing the current file. **It must have the exact same filename and extension, and it must be a File or ShadowFile object**
* version: either "v1" or "v2" (prolly v2)

```typoscript
const fileToUpload: ShadowFile = {
        name: 'mytext.txt',
        file: fileBuff
    }
const url = 'https://shdw-drive.genesysgo.net/4HUkENqjnTAZaUR4QLwff1BvQPCiYkNmu5PPSKGoKf9G/fape.png'
const acctPubKey = new anchor.web3.PublicKey("EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN")
const editFile = await drive.editFile(acctPubKey,url,"v2", fileToUpload)
```

#### List Storage Account Files (aka List Objects)

This is a simple implementation that only requires a public key to get the file names of a storage account.

```javascript
const acctPubKey = new anchor.web3.PublicKey("EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN")
const listItems = await drive.listObjects(acctPubKey)
console.log(listItems)
```

And the response payload:

```
{ keys: [ 'index.html' ] }
```

#### Increase Storage Account Size

This is a method that simply increase the storage limit of a storage account. It requires three params:

* key: storage account public key
* size: amount to increase by, must end with "KB", "MB", or "GB"
* version: storage account version, must be "v1" or "v2"

```javascript
const accts = await drive.getStorageAccounts("v2")
let acctPubKey = new anchor.web3.PublicKey(accts[1].publicKey)
const addStgResp = await drive.addStorage(acctPubKey,"10MB","v2")
```

#### Reduce Storage Account Size

So you didn't use all of your storage? Shrink it down so that you can get some SHDW back. This implementation only requires three params - the storage account key, the amount to reduce it by, and the version.

```javascript
const acctPubKey = new anchor.web3.PublicKey("EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN")
const shrinkAcct = await drive.reduceStorage(acctPubKey,"10MB", "v2")
```

#### Next you'll want to claim your unused SHDW

Here you can claim your SHDW that is no longer being used. This method only requires a storage account public key and a version.

```javascript
const acctPubKey = new anchor.web3.PublicKey("EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN")
const claimStake = await drive.claimStake(acctPubKey,"v2")
```

#### Delete a Storage Account

As the name implies, you can delete a storage account and all of its files. The storage account can still be recovered until the current epoch ends, but after that, it's toast. This implementation only requires two params - a storage account key and a version.

```javascript
const acctPubKey = new anchor.web3.PublicKey("EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN")
const delAcct = await drive.deleteStorageAccount(acctPubKey,"v2")
```

#### Undelete the Deleted Storage Account

You can still get your storage account back if the current epoch hasn't elapsed. Same drill - just need an account public key and a version.

```javascript
const acctPubKey = new anchor.web3.PublicKey("EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN")
const cancelDelStg = await drive.cancelDeleteStorageAccount(acctPubKey,"v2")
```

# **Getting Started: Rust SDK**

Available on [crates.io](https://crates.io/crates/shadow-drive-sdk).

#### **Preerquisites**

`sudo apt-get install pkg-config`

`sudo apt-get install libudev-dev`


#### **Install**

Add the crate to your `Cargo.toml`.

```toml
shadow-drive-rust = "0.4.0"
```

#### **Examples**

```rust
    //init tracing.rs subscriber
    tracing_subscriber::fmt()
        .with_env_filter("off,shadow_drive_rust=debug")
        .init();

    //load keypair from file
    let keypair = read_keypair_file(KEYPAIR_PATH).expect("failed to load keypair at path");

    //create shdw drive client
    let shdw_drive_client = ShadowDriveClient::new(keypair, "https://ssc-dao.genesysgo.net");

    //derive the storage account pubkey
    let pubkey = keypair.pubkey();
    let (storage_account_key, _) =
        shadow_drive_rust::derived_addresses::storage_account(&pubkey, 0);

    // read files in directory
    let dir = tokio::fs::read_dir("multiple_uploads")
    .await
    .expect("failed to read multiple uploads dir");

    // create Vec of ShadowFile structs for upload
    let mut files = tokio_stream::wrappers::ReadDirStream::new(dir)
        .filter(Result::is_ok)
        .and_then(|entry| async move {
            Ok(ShadowFile::file(
                entry
                    .file_name()
                    .into_string()
                    .expect("failed to convert os string to regular string"),
                entry.path(),
            ))
        })
        .collect::<Result<Vec<_>, _>>()
        .await
        .expect("failed to create shdw files for dir");

    // Bytes are also supported
    files.push(ShadowFile::bytes(
        String::from("buf.txt"),
        &b"this is a buf test"[..],
    ));

    // kick off upload
    let upload_results = shdw_drive_client
        .upload_multiple_files(&storage_account_key, files)
        .await
        .expect("failed to upload files");

    //profit
    println!("upload results: {:#?}", upload_results);
```

## **Rust Methods**


### **`add_immutable_storage`**
Adds storage capacity to the specified immutable` StorageAccount`. This will fail if the `StorageAccount` is not immutable.
* **storage_account_key** - The public key of the immutable `StorageAccount`.
* **size** - The additional amount of storage you want to add. E.g if you have an existing StorageAccount with 1MB of storage but you need 2MB total, size should equal 1MB. When specifying size, only KB, MB, and GB storage units are currently supported.

#### **Example**
```rust
let add_immutable_storage_response = shdw_drive_client
    .add_immutable_storage(storage_account_key, Byte::from_str("1MB").expect("invalid byte string"))
    .await?;
```

### **Response**
```json
{
    pub message: String,
    pub transaction_signature: String,
    pub error: Option<String>,
}
```

### **`add_storage`**
Adds storage capacity to the specified StorageAccount.
* **storage_account_key** - The public key of the StorageAccount.
* **size** - The additional amount of storage you want to add. E.g if you have an existing StorageAccount with 1MB of storage but you need 2MB total, size should equal 1MB. When specifying size, only KB, MB, and GB storage units are currently supported.

#### **Example**
```rust
let add_storage_response = shdw_drive_client
    .add_storage(storage_account_key, Byte::from_str("1MB").expect("invalid byte string"))
    .await?;
```

#### **Response**
```json
{
    pub message: String,
    pub transaction_signature: String,
    pub error: Option<String>,
}
```
### **`cancel_delete_storage_account`**

Unmarks a StorageAccount for deletion from the Shadow Drive. To prevent deletion, this method must be called before the end of the Solana epoch in which `delete_storage_account` is called.

*   **storage\_account\_key** - The public key of the `StorageAccount` that you want to unmark for deletion.

#### **Example**

```rust
let cancel_delete_storage_account_response = shdw_drive_client
    .cancel_delete_storage_account(&storage_account_key)
    .await?;
```

#### **Response**

```json
{
    pub txid: String,
}
```

### **`claim_stake`**

Claims any available stake as a result of the `reduce_storage` command. After reducing storage amount, users must wait until the end of the epoch to successfully claim their stake.

*   **storage\_account\_key** - The public key of the StorageAccount that you want to claim excess stake from.

#### **Example**

```rust
let claim_stake_response = shdw_drive_client
    .claim_stake(&storage_account_key)
    .await?;
```

#### **Response**

```json
{
    pub txid: String,
}
```

### **`create_storage_account`**

Creates a `StorageAccount` on the Shadow Drive. `StorageAccount`s can hold multiple files and are paid for using the SHDW token.

*   **name** - The name of the `StorageAccount`. Does not need to be unique.
*   **size** - The amount of storage the `StorageAccount` should be initialized with. When specifying size, only KB, MB, and GB storage units are currently supported.

**Example**

*examples/end\_to\_end.rs (lines 24-28)*

```rust
async fn main() {
    // Get keypair
    let keypair_file: String = std::env::args()
        .skip(1)
        .next()
        .expect("no keypair file provided");
    let keypair: Keypair = read_keypair_file(keypair_file).expect("failed to read keypair file");
    println!("loaded keypair");

    // Initialize client
    let client = ShadowDriveClient::new(keypair, SOLANA_MAINNET_RPC);
    println!("initialized client");

    // Create account
    let response = client
        .create_storage_account(
            "test",
            Byte::from_bytes(2_u128.pow(20)),
            shadow_drive_sdk::StorageAccountVersion::V2,
        ).await?;
    Ok(())
}
```

#### **Response**

```json
{
    pub message: String,
    pub transaction_signature: String,
    pub storage_account_address: String,
    pub error: Option<String>,
}
```

### **`delete_file`**

Marks a file for deletion from the Shadow Drive. Files marked for deletion are deleted at the end of each Solana epoch. Marking a file for deletion can be undone with cancel\_delete\_file, but this must be done before the end of the Solana epoch.

*   **storage\_account\_key** - The public key of the `StorageAccount` that contains the file.
*   **url** - The Shadow Drive url of the file you want to mark for deletion.

#### **Example**

```rust
let delete_file_response = shdw_drive_client
    .delete_file(&storage_account_key, url)
    .await?;
```

#### **Examples Found in Repository**

```rust
async fn main() {
    // Get keypair
    let keypair_file: String = std::env::args()
        .skip(1)
        .next()
        .expect("no keypair file provided");
    let keypair: Keypair = read_keypair_file(keypair_file).expect("failed to read keypair file");
    println!("loaded keypair");

    // Initialize client
    let client = ShadowDriveClient::new(keypair, SOLANA_MAINNET_RPC);
    println!("initialized client");

    // Create account
    let response = client
        .create_storage_account(
            "test",
            Byte::from_bytes(2_u128.pow(20)),
            shadow_drive_sdk::StorageAccountVersion::V2,
        )
        .await
        .expect("failed to create storage account");
    let account = Pubkey::from_str(&response.shdw_bucket.unwrap()).unwrap();
    println!("created storage account");

    // Upload files
    let files: Vec<ShadowFile> = vec![
        ShadowFile::file("alpha.txt".to_string(), "./examples/files/alpha.txt"),
        ShadowFile::file(
            "not_alpha.txt".to_string(),
            "./examples/files/not_alpha.txt",
        ),
    ];
    let response = client
        .store_files(&account, files.clone())
        .await
        .expect("failed to upload files");
    println!("uploaded files");
    for url in &response.finalized_locations {
        println!("    {url}")
    }

    // Try editing
    for file in files {
        let response = client
            .edit_file(&account, file)
            .await
            .expect("failed to upload files");
        assert!(!response.finalized_location.is_empty(), "failed edit");
        println!("edited file: {}", response.finalized_location);
    }

    // Delete files
    for url in response.finalized_locations {
        client
            .delete_file(&account, url)
            .await
            .expect("failed to delete files");
    }
}
```

### **`delete_storage_account`**

This function marks a StorageAccount for deletion from the Shadow Drive. If an account is marked for deletion, all files within the account will be deleted as well. Any stake remaining in the StorageAccount will be refunded to the creator. Accounts marked for deletion are deleted at the end of each Solana epoch.

#### Parameters

*   `storage_account_key` - The public key of the StorageAccount that you want to mark for deletion.

#### Return

*   This method returns success if it can successfully mark the account for deletion and refund any remaining stake in the account before the end of the current Solana epoch.

#### Example

```rust
let delete_storage_account_response = shdw_drive_client
    .delete_storage_account(&storage_account_key)
    .await?;
```

An example use case for this method can be found in the same [github repository](https://github.com/phantom-labs/shadow_sdk/blob/master/examples/end_to_end.rs) on line 71.


### **`edit_file`**

Replace an existing file on the Shadow Drive with the given updated file.

*   **storage\_account\_key** - The public key of the `StorageAccount` that contains the file.
*   **url** - The Shadow Drive url of the file you want to replace.
*   **data** - The updated `ShadowFile`.

#### **Example**

```rust
let edit_file_response = shdw_drive_client
    .edit_file(&storage_account_key, url, file)
    .await?;
```

#### **Response**

```json
{
    pub finalized_location: String,
    pub error: String,
}
```

**Examples found in repository**

File: examples/end\_to\_end.rs, Line 53

```rust
async fn main() {
    // Get keypair
    let keypair_file: String = std::env::args()
        .skip(1)
        .next()
        .expect("no keypair file provided");
    let keypair: Keypair = read_keypair_file(keypair_file).expect("failed to read keypair file");
    println!("loaded keypair");

    // Initialize client
    let client = ShadowDriveClient::new(keypair, SOLANA_MAINNET_RPC);
    println!("initialized client");

    // Create account
    let response = client
        .create_storage_account(
            "test",
            Byte::from_bytes(2_u128.pow(20)),
            shadow_drive_sdk::StorageAccountVersion::V2,
        )
        .await
        .expect("failed to create storage account");
    let account = Pubkey::from_str(&response.shdw_bucket.unwrap()).unwrap();
    println!("created storage account");

    // Upload files
    let files: Vec<ShadowFile> = vec![
        ShadowFile::file("alpha.txt".to_string(), "./examples/files/alpha.txt"),
        ShadowFile::file(
            "not_alpha.txt".to_string(),
            "./examples/files/not_alpha.txt",
        ),
    ];
    let response = client
        .store_files(&account, files.clone())
        .await
        .expect("failed to upload files");
    println!("uploaded files");
    for url in &response.finalized_locations {
        println!("    {url}")
    }
    // Try editing
    for file in files {
        let response = client
            .edit_file(&account, file)
            .await
            .expect("failed to upload files");
    }    
}
```

### **`get_storage_account`**

Returns the `StorageAccount` associated with the pubkey provided by a user.

*   **key** - The public key of the `StorageAccount`.

#### **Example**

```rust
let storage_account = shdw_drive_client
    .get_storage_account(&storage_account_key)
    .await
    .expect("failed to get storage account");
```

#### **Response for V1 StorageAccount**

```json
{

    pub storage_account: Pubkey,
    pub reserved_bytes: u64,
    pub current_usage: u64,
    pub immutable: bool,
    pub to_be_deleted: bool,
    pub delete_request_epoch: u32,
    pub owner_1: Pubkey,
    pub owner_2: Pubkey,
    pub account_counter_seed: u32,
    pub creation_time: u32,
    pub creation_epoch: u32,
    pub last_fee_epoch: u32,
    pub identifier: String,
}
```
#### **Response for V2 StorageAccount**
```json
{
    pub storage_account: Pubkey,
    pub reserved_bytes: u64,
    pub current_usage: u64,
    pub immutable: bool,
    pub to_be_deleted: bool,
    pub delete_request_epoch: u32,
    pub owner_1: Pubkey,
    pub account_counter_seed: u32,
    pub creation_time: u32,
    pub creation_epoch: u32,
    pub last_fee_epoch: u32,
    pub identifier: String,
}
```

### **`get_storage_accounts`**

Returns all `StorageAccounts` associated with the public key provided by a user.

*   `owner` - The public key that is the owner of all the returned `StorageAccounts`.

#### **Example**

```rust
let storage_accounts = shdw_drive_client
    .get_storage_accounts(&user_pubkey)
    .await
    .expect("failed to get storage account");
```

#### **Response for V1 StorageAccount**

```json
{

    pub storage_account: Pubkey,
    pub reserved_bytes: u64,
    pub current_usage: u64,
    pub immutable: bool,
    pub to_be_deleted: bool,
    pub delete_request_epoch: u32,
    pub owner_1: Pubkey,
    pub owner_2: Pubkey,
    pub account_counter_seed: u32,
    pub creation_time: u32,
    pub creation_epoch: u32,
    pub last_fee_epoch: u32,
    pub identifier: String,
}
```
#### **Response for V2 StorageAccount**
```json
{
    pub storage_account: Pubkey,
    pub reserved_bytes: u64,
    pub current_usage: u64,
    pub immutable: bool,
    pub to_be_deleted: bool,
    pub delete_request_epoch: u32,
    pub owner_1: Pubkey,
    pub account_counter_seed: u32,
    pub creation_time: u32,
    pub creation_epoch: u32,
    pub last_fee_epoch: u32,
    pub identifier: String,
}
```

### **`list_objects`** 
Gets a list of all files associated with a `StorageAccount`. The output contains all of the file names as strings.

*   **storage\_account\_key** - The public key of the `StorageAccount` that owns the files.

#### **Example**

```rust
let files = shdw_drive_client
    .list_objects(&storage_account_key)
    .await?;
```

### **Response**

Note: The response is a vector containing all of the file names as strings.


### **`make_storage_immutable`**

Permanently locks a `StorageAccount` and all contained files. After a `StorageAccount` has been locked, a user will no longer be able to delete/edit files, add/reduce storage amount, or delete the `StorageAccount`.

*   **storage\_account\_key** - The public key of the `StorageAccount` that will be made immutable.

#### **Example**

```rust
let make_immutable_response = shdw_drive_client  
    .make_storage_immutable(&storage_account_key)  
    .await?;  
```

#### **Response**

```json
{
    pub message: String,
    pub transaction_signature: String,
    pub error: Option<String>,
}
```


### **`migrate`**

Migrates a v1 StorageAccount to v2. This requires two separate transactions to reuse the original pubkey. To minimize chance of failure, it is recommended to call this method with a commitment level of Finalized.

*   **storage\_account\_key** - The public key of the StorageAccount to be migrated.

#### **Example**

```rust
let migrate_response = shdw_drive_client
    .migrate(&storage_account_key)
    .await?;
```

####  **Result**
```json
{
    pub txid: String,
}
```
#### **`migrate_step_1`**

First transaction step that migrates a v1 `StorageAccount` to v2. Consists of copying the existing account’s data into an intermediate account, and deleting the v1 `StorageAccount`.

#### **`migrate_step_2`**

Second transaction step that migrates a v1 `StorageAccount` to v2. Consists of recreating the `StorageAccount` using the original pubkey, and deleting the intermediate account.


### **`new`**

Creates a new ShadowDriveClient from the given `Signer` and URL.

*   **wallet** - A `Signer` that for signs all transactions generated by the client. Typically this is a user’s keypair.
*   **rpc\_url** - An HTTP URL of a Solana RPC provider.  
    The underlying Solana RPC client is configured with 120s timeout and a commitment level of confirmed.

To customize RpcClient settings see `new_with_rpc`.

#### **Example**

```rust
use solana_sdk::signer::keypair::Keypair;    

let wallet = Keypair::generate();
let shdw_drive = ShadowDriveClient::new(wallet, "https://ssc-dao.genesysgo.net");
```

Examples found in repository?  
`examples/end_to_end.rs` (line 19)

```rust
async fn main() {
    // Get keypair
    let keypair_file: String = std::env::args()
        .skip(1)
        .next()
        .expect("no keypair file provided");
    let keypair: Keypair = read_keypair_file(keypair_file).expect("failed to read keypair file");
    println!("loaded keypair");

    // Initialize client
    let client = ShadowDriveClient::new(keypair, SOLANA_MAINNET_RPC);
    println!("initialized client");
}
```

### **`new_with_rpc`**

Creates a new ShadowDriveClient from the given `Signer` and `RpcClient`.

*   **wallet** - A `Signer` that for signs all transactions generated by the client. Typically this is a user’s keypair.
*   **rpc\_client** - A Solana `RpcClient` that handles sending transactions and reading accounts from the blockchain.  
    Providng the `RpcClient` allows customization of timeout and committment level.

#### **Example**

```rust
use solana_client::rpc_client::RpcClient;
use solana_sdk::signer::keypair::Keypair;    
use solana_sdk::commitment_config::CommitmentConfig;

let wallet = Keypair::generate();
let solana_rpc = RpcClient::new_with_commitment("https://ssc-dao.genesysgo.net", CommitmentConfig::confirmed());
let shdw_drive = ShadowDriveClient::new_with_rpc(wallet, solana_rpc);
```


### **`redeem_rent`**

Reclaims the Solana rent from any on-chain file accounts. Older versions of the Shadow Drive used to create accounts for uploaded files.

*   **storage\_account\_key** - The public key of the StorageAccount that contained the deleted file.
*   **file\_account\_key** - The public key of the File account to be closed.

#### **Example**

```rust
let redeem_rent_response = shdw_drive_client
    .redeem_rent(&storage_account_key, &file_account_key)
    .await?;
```

#### **Response**

```json
{
  pub message: String,
  pub transaction_signature: String,
  pub error: Option<String>,
}
```

### **`reduce_storage`**

Reduces the amount of total storage available for the given `StorageAccount`.

*   **storage\_account\_key** - The public key of the `StorageAccount` whose storage will be reduced.
*   **size** - The amount of storage you want to remove. E.g if you have an existing `StorageAccount` with 3MB of storage but you want 2MB total, size should equal 1MB. When specifying size, only KB, MB, and GB storage units are currently supported.

#### **Example**

```rust
let reduce_storage_response = shdw_drive_client
    .reduce_storage(&storage_account_key, reduced_bytes)
    .await?;
```

### **Response**

```json
{
  pub message: String,
  pub transaction_signature: String,
  pub error: Option<String>,
}
```

### **`store_files`**

Stores files in the specified `StorageAccount`.

*   **storage\_account\_key** - The public key of the `StorageAccount`.
*   **data** - Vector of `ShadowFile` objects representing the files that will be stored.

#### **Example**

```rust
let files: Vec<ShadowFile> = vec![
    ShadowFile::file("alpha.txt".to_string(), "./examples/files/alpha.txt"),
    ShadowFile::file(
        "not_alpha.txt".to_string(),
        "./examples/files/not_alpha.txt",
    ),
];
let store_files_response = shdw_drive_client
    .store_files(&storage_account_key, files)
    .await?;
```

#### **Response**

```json
{
  pub message: String,
  pub transaction_signature: String,
  pub error: Option<String>,
}
```


-------------------------------------------------------------------

## **Getting Started: Python SDK**


#### **isntalling shadow-drive**

`pip install shadow-drive`

(for running the examples)

`pip install solders`

Check out the [`examples/`](https://github.com/GenesysGo/shadow-drive-rust/tree/main/py) directory for a demonstration of the functionality.

https://shdw-drive.genesysgo.net/[STORAGE_ACCOUNT_ADDRESS]

#### **full.py**
```python
from shadow_drive import ShadowDriveClient
from solders.keypair import Keypair
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('--keypair', metavar='keypair', type=str, required=True,
                    help='The keypair file to use (e.g. keypair.json, dev.json)')
args = parser.parse_args()

# Initialize client
client = ShadowDriveClient(args.keypair)
print("Initialized client")

# Create account
size = 2 ** 20
account, tx = client.create_account("full_test", size, use_account=True)
print(f"Created storage account {account}")

# Upload files
files = ["./files/alpha.txt", "./files/not_alpha.txt"]
urls = client.upload_files(files)
print("Uploaded files")

# Add and Reduce Storage
client.add_storage(2**20)
client.reduce_storage(2**20)

# Get file
current_files = client.list_files()
file = client.get_file(current_files[0])
print(f"got file {file}")

# Delete files
client.delete_files(urls)
print("Deleted files")

# Delete account
client.delete_account(account)
print("Closed account")
```

#### **About the [Github Repo](https://github.com/GenesysGo/shadow-drive-rust/tree/main/py)**
This package uses PyO3 to build a wrapper around the official Shadow Drive Rust SDK. For more information, see the [Rust SDK documentation](https://github.com/GenesysGo/shadow-drive-rust/tree/main/sdk).
