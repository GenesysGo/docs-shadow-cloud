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

### **Getting Started: Rust SDK**

Available on [crates.io](https://crates.io/crates/shadow-drive-sdk).

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


### **Getting Started: Python SDK**

Check out the [`examples/`](https://github.com/GenesysGo/shadow-drive-rust/tree/main/py) directory for a demonstration of the functionality.

```python
from shadow_drive import ShadowDriveClient

# Initialize client
client = ShadowDriveClient("test.json")

# Create account
size = 2 ** 20
account, tx = client.create_account("test", size, use_account=True)

# Upload files
files = ["./files/alpha.txt", "./files/not_alpha.txt"]
urls = client.upload_files(files)

# Delete files
client.delete_files(urls)

# Delete account
client.delete_account(account)
```

#### **About the [Github Repo](https://github.com/GenesysGo/shadow-drive-rust/tree/main/py)**
This package uses PyO3 to build a wrapper around the official Shadow Drive Rust SDK. For more information, see the [Rust SDK documentation](https://github.com/GenesysGo/shadow-drive-rust/tree/main/sdk).