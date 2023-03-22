# Javascript

## **Contents**

* [**Getting Started**](sdk-javascript.md#getting-started-javascript-sdk)
* [**Create a Storage Account**](sdk-javascript.md#create-a-storage-account)
* [**Upload a File**](sdk-javascript.md#upload-a-file)
* [**Edit a File**](sdk-javascript.md#edit-a-file-aka-replace-a-file)
* [**Delete a File**](sdk-javascript.md#delete-a-file)
* [**Methods**](sdk-javascript.md#methods)
  * [**constructor**](sdk-javascript.md#constructor)
  * [**addStorage**](sdk-javascript.md#addstorage)
  * [**cancelDeleteStorageAccount**](sdk-javascript.md#canceldeletestorageaccount)
  * [**claimStake**](sdk-javascript.md#claimstake)
  * [**createStorageAccount**](sdk-javascript.md#createstorageaccount)
  * [**deleteFile**](sdk-javascript.md#deletefile)
  * [**deleteStorageAccount**](sdk-javascript.md#deletestorageaccount)
  * [**editFile**](sdk-javascript.md#editfile)
  * [**getStorageAccount**](sdk-javascript.md#getstorageaccount)
  * [**getStorageAccounts**](sdk-javascript.md#getstorageaccount)
  * [**listObjects**](sdk-javascript.md#listobjects)
  * [**makeStorageImmutable**](sdk-javascript.md#makestorageimmutable)
  * [**migrate**](sdk-javascript.md#migrate)
  * [**redeemRent**](sdk-javascript.md#redeemrent)
  * [**reduceStorage**](sdk-javascript.md#reducestorage)
  * [**storageConfigPDA**](sdk-javascript.md#storageconfigpda)
  * [**uploadFile**](sdk-javascript.md#uploadfile)
  * [**uploadMultipleFiles**](sdk-javascript.md#uploadmultiplefiles)
  * [**userInfo**](sdk-javascript.md#userinfo)
* [**Example - POST request via SDK**](#example---post-request-via-sdk-make-immutable)
* []()
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

Review the [Solana Web3.js](https://solana-labs.github.io/solana-web3.js/) SDK and [Solana API](https://docs.solana.com/developing/clients/javascript-api) resources.

#### **Instantiate the Wallet and Connection**

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

#### **Building components for various Shadow Drive operations**

Let's start by instantiating the Shadow Drive connection class object. This will have all Shadow Drive methods and it implements the signing wallet within the class for all transactions.

At the simplest level, it is recommend for a React app to immediately try to load a connection to a user's Shadow Drives upon wallet connection. This can be done with the `useEffect` React hook.

```javascript
import React, { useEffect } from "react";
import * as anchor from "@project-serum/anchor";
import {ShdwDrive} from "@shadow-drive/sdk";
import { useWallet, useConnection } from "@solana/wallet-adapter-react";

export default function Drive() {
    const { connection } = useConnection();
    const wallet = useWallet();
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

This can be done with a NodeJS + TypeScript program as well.

```javascript
const anchor = require("@project-serum/anchor");
const { Connection, clusterApiUrl, Keypair } = require("@solana/web3.js");
const { ShdwDrive } = require("@shadow-drive/sdk");
const key = require("./shdwkey.json");

async function main() {
    let secretKey = Uint8Array.from(key);
    let keypair = Keypair.fromSecretKey(secretKey);
    const connection = new Connection(
        clusterApiUrl("mainnet-beta"),
        "confirmed"
    );
    const wallet = new anchor.Wallet(keypair);
    const drive = await new ShdwDrive(connection, wallet).init();
}

main();
```

**Create a Storage Account**

This implementation is effectively the same for both Web and Node implementations. There are three params that are required to create a storage account:

* `name`: a friendly name for your storage account
* `size`: The size of your storage accounts with a human readable ending containing `KB`, `MB`, or `GB`
* `version`: can be either `v1` or `v2`. Note - `v1` is completely deprecated and you shuold only use `v2` moving forward.

```javascript
//create account
const newAcct = await drive.createStorageAccount("myDemoBucket", "10MB", "v2");
console.log(newAcct);
```

#### **Get a list of Owned Storage Accounts**

This implementation is effectively the same for both Web and Node implementations. The only parameter required is either `v1` or `v2` for the version of storage account you created in the previous step.

```javascript
const accts = await drive.getStorageAccounts("v2");
// handle printing pubKey of first storage acct
let acctPubKey = new anchor.web3.PublicKey(accts[0].publicKey);
console.log(acctPubKey.toBase58());
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

#### **Get a Specific Storage Account**

This implementation is effectively the same for both Web and Node implementations. The only parameter required is either a PublicKey object or a base-58 string of the public key.

```javascript
const acct = await drive.getStorageAccount(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
console.log(acct);
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

#### **Upload a File**

The `uploadFile` method requires two parameters:

* `key`: A PublicKey object representing the public key of the Shadow Storage Account
* `data`: A file of either the `File` object type or `ShadowFile` object type

Check the intellisense popup below when hovering over the method

<figure><img src="../../.gitbook/assets/Screen Shot 2022-10-13 at 7.40.37 PM.png" alt=""><figcaption></figcaption></figure>

`File` objects are implemented in web browsers, and `ShadowFile` is a custom type we implemented in TypeScript. So either you are using `File` in the web, or you are scripting in TS.

Here is an example with a React Component:

<details>

<summary>React Upload Component</summary>

```javascript
import React, { useState } from "react";
import { ShdwDrive } from "@shadow-drive/sdk";
import { useWallet, useConnection } from "@solana/wallet-adapter-react";
import FormData from "form-data";

export default function Upload() {
    const [file, setFile] = useState < File > undefined;
    const [uploadUrl, setUploadUrl] = useState < String > undefined;
    const [txnSig, setTxnSig] = useState < String > undefined;
    const { connection } = useConnection();
    const wallet = useWallet();
    return (
        <div>
            <form
                onSubmit={async (event) => {
                    event.preventDefault();
                    const drive = await new ShdwDrive(
                        connection,
                        wallet
                    ).init();
                    const accounts = await drive.getStorageAccounts();
                    const acc = accounts[0].publicKey;
                    const getStorageAccount = await drive.getStorageAccount(
                        acc
                    );

                    const upload = await drive.uploadFile(acc, file);
                    console.log(upload);
                    setUploadUrl(upload.finalized_location);
                    setTxnSig(upload.transaction_signature);
                }}
            >
                <h1>Shadow Drive File Upload</h1>
                <input
                    type="file"
                    onChange={(e) => setFile(e.target.files[0])}
                />
                <br />
                <button type="submit">Upload</button>
            </form>
            <span>You may have to wait 60-120s for the URL to appear</span>
            <div>
                {uploadUrl ? (
                    <div>
                        <h3>Success!</h3>
                        <h4>URL: {uploadUrl}</h4>
                        <h4>Sig: {txnSig}</h4>
                    </div>
                ) : (
                    <div></div>
                )}
            </div>
        </div>
    );
}
```

</details>

And a NodeJS + TypeScript implementation would look like:

<details>

<summary>TS UploadFile</summary>

```javascript
const { ShdwDrive, ShadowFile } = require("@shadow-drive/sdk");
/**
 * ...
 * Initialize storage account here...
 * ...
 */
const fileBuff = fs.readFileSync("./mytext.txt");
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const fileToUpload: ShadowFile = {
    name: "mytext.txt",
    file: fileBuff,
};
const uploadFile = await drive.uploadFile(acctPubKey, fileToUpload);
console.log(uploadFile);
```

</details>

#### **Upload Multiple Files**

This is a nearly identical implementation to uploadFile, except that it requires a `FileList` or array of `ShadowFiles` and an optional concurrency parameter.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-10-13 at 8.12.08 PM.png" alt=""><figcaption></figcaption></figure>

Recall that the default setting is to attempt to upload 3 files concurrently. Here you can override this and specify how many files you want to try to upload based on the cores and bandwith of your infrastructure.

#### **Delete a File**

The implementation of `deleteFile` is the same between web and Node. There are three required parameters to delete a file:

* `key`: the storage account's public key
* `url`: the current URL of the file to be deleted
* `version`: can be either `v1` or `v2`

```javascript
const url =
    "https://shdw-drive.genesysgo.net/4HUkENqjnTAZaUR4QLwff1BvQPCiYkNmu5PPSKGoKf9G/fape.png";
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const delFile = await drive.deleteFile(acctPubKey, url, "v2");
console.log(delFile);
```

#### **Edit a File (aka Replace a file)**

<figure><img src="../../.gitbook/assets/Screen Shot 2022-10-13 at 8.28.30 PM.png" alt=""><figcaption></figcaption></figure>

The editFile method is a combo of `uploadFile` and `deleteFile`. Let's look at the params:

* `key`: the Public Key of the storage account
* `url`: the URL of the file that is being replaced
* `data`: the file that is replacing the current file. **It must have the exact same filename and extension, and it must be a `File` or `ShadowFile` object**
* `version`: either `v1` or `v2`

```typescript
const fileToUpload: ShadowFile = {
    name: "mytext.txt",
    file: fileBuff,
};
const url =
    "https://shdw-drive.genesysgo.net/4HUkENqjnTAZaUR4QLwff1BvQPCiYkNmu5PPSKGoKf9G/fape.png";
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const editFile = await drive.editFile(acctPubKey, url, "v2", fileToUpload);
```

#### **List Storage Account Files (aka List Objects)**

This is a simple implementation that only requires a public key to get the file names of a storage account.

```javascript
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const listItems = await drive.listObjects(acctPubKey);
console.log(listItems);
```

And the response payload:

```
{ keys: [ 'index.html' ] }
```

#### **Increase Storage Account Size**

This is a method to simply increase the storage limit of a storage account. It requires three params:

* `key`: storage account public key
* `size`: amount to increase by, must end with `KB`, `MB`, or `GB`
* `version`: storage account version, must be `v1` or `v2`

```javascript
const accts = await drive.getStorageAccounts("v2");
let acctPubKey = new anchor.web3.PublicKey(accts[1].publicKey);
const addStgResp = await drive.addStorage(acctPubKey, "10MB", "v2");
```

#### **Reduce Storage Account Size**

This is a method to decrease the storage limit of a storage account. This implementation only requires three params - the storage account key, the amount to reduce it by, and the version.

```javascript
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const shrinkAcct = await drive.reduceStorage(acctPubKey, "10MB", "v2");
```

#### **Next you'll want to claim your unused SHDW**

This method allows you to reclaim the SHDW that is no longer being used. This method only requires a storage account public key and a version.

```javascript
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const claimStake = await drive.claimStake(acctPubKey, "v2");
```

#### **Delete a Storage Account**

As the name implies, you can delete a storage account and all of its files. The storage account can still be recovered until the current epoch ends, but after that, it will be removed. This implementation only requires two params - a storage account key and a version.

```javascript
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const delAcct = await drive.deleteStorageAccount(acctPubKey, "v2");
```

#### **Undelete the Deleted Storage Account**

You can still get your storage account back if the current epoch hasn't elapsed. This implementation only requires two params - an account public key and a version.

```javascript
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const cancelDelStg = await drive.cancelDeleteStorageAccount(acctPubKey, "v2");
```

## **Methods**

### **`constructor`**

#### **Definition**

This method is used to create a new instance of the ShadowDrive class. It accepts a web3 connection object and a web3 wallet. It returns an instance of the ShadowDrive class.

#### **Parameters**

* `connection`: `Connection` - initialized web3 connection object
* `wallet`: `any` - Web3 wallet

#### **Returns**

It returns an instance of the ShadowDrive class.

{% tabs %}
{% tab title="Example" %}
```javascript
const shadowDrive = new ShadowDrive(connection, wallet).init();
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// this creates a new instance of the ShadowDrive class and initializes it with the given connection and wallet parameters
const shadowDrive = new ShadowDrive(connection, wallet).init();
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`addStorage`**

#### **Definition**

`addStorage` is a method of the `ShadowDrive` class defined in `index.ts` at line 121. It takes three parameters: `key`, `size`, and `version` and returns a `Promise<ShadowDriveResponse>` with the confirmed transaction ID.

#### **Parameters**

* `key`: `PublicKey` - Public Key of the existing storage to increase size on
* `size`: `string` - Amount of storage you are requesting to add to your storage account. Should be in a string like '1KB', '1MB', '1GB'. Only KB, MB, and GB storage delineations are supported currently.
* `version`: `ShadowDriveVersion` - ShadowDrive version (v1 or v2)

#### **Returns**

Confirmed transaction ID

```json
{
  message: string;
  transaction_signature?: string
}
```

{% tabs %}
{% tab title="Example" %}
```javascript
const accts = await drive.getStorageAccounts("v2");
let acctPubKey = new anchor.web3.PublicKey(accts[1].publicKey);
const addStgResp = await drive.addStorage(acctPubKey, "10MB", "v2");
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// This line retrieves the storage accounts with version "v2" using the `getStorageAccounts` method of the `drive` object and stores them in the `accts` variable.
const accts = await drive.getStorageAccounts("v2")

// This line creates a new `PublicKey` object using the public key of the second storage account retrieved in the previous line and stores it in the `acctPubKey` variable.
let acctPubKey = new anchor.web3.PublicKey(accts[1].publicKey)

// This line adds a new storage allocation of size "10MB" and version "v2" to the storage account identified by the public key in `acctPubKey`. The response is stored in the `addStgResp` variable.
const addStgResp = await drive.addStorage(acctPubKey,"10MB","v2"ca
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`cancelDeleteStorageAccount`**

#### **Definition**

Implementation of ShadowDrive.cancelDeleteStorageAccount defined in index.ts:135 This method is used to cancel a delete request for a Storage Account on ShadowDrive. It accepts a Public Key of the Storage Account and the Shadow Drive version (v1 or v2). It returns a Promise<{ txid: string }> containing the confirmed transaction ID of the undelete request.

#### **Parameters**

* `key`: `PublicKey` - Publickey

#### **Returns**

Confirmed transaction ID

{% tabs %}
{% tab title="Example" %}
```javascript
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const cancelDelStg = await drive.cancelDeleteStorageAccount(acctPubKey, "v2");
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Create a new public key object from a string representation of a Solana account public key
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);

// Call the "cancelDeleteStorageAccount" function of the Shadow Drive API, passing in the account public key object and a string indicating the storage account version to cancel deletion for
const cancelDelStg = await drive.cancelDeleteStorageAccount(acctPubKey, "v2");
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`claimStake`**

#### **Definition**

This method is used to request a Stake on ShadowDrive. It accepts a PublicKey of the Storage Account and the ShadowDrive version (v1 or v2). It returns a Promise<{ txid: string }> containing the confirmed transaction ID of the claimStake request.

#### **Parameters**

* `key`: `PublicKey` - Publickey of Storage Account
* `version`: \`ShadowDrive

#### **Returns**

Confirmed transaction ID

{% tabs %}
{% tab title="Example" %}
```javascript
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const claimStake = await drive.claimStake(acctPubKey, "v2");
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Create a new public key object with the specified value
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);

// Call the 'claimStake' function on the 'drive' object with the account public key and 'v2' as parameters, and wait for its completion before proceeding
const claimStake = await drive.claimStake(acctPubKey, "v2");
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`createStorageAccount`**

#### **Definition**

Implementation of ShadowDrive.createStorageAccount defined in index.ts:120 This method is used to create a new Storage Account on ShadowDrive. It accepts the name of the Storage Account, the size of the requested Storage Account, and the ShadowDrive version (v1 or v2). It also accepts an optional secondary owner for the Storage Account. It returns a Promise containing the created Storage Account and the transaction signature.

#### **Parameters**

* `name`: `string` - What you want your storage account to be named. (Does not have to be unique)
* `size`: `string` - Amount of storage you are requesting to create. Should be in a string like '1KB', '1MB', '1GB'. Only KB, MB, and GB storage delineations are supported currently.
* `version`: `ShadowDriveVersion` - ShadowDrive version(v1 or v2)
* `owner2` (optional): `PublicKey` - Optional secondary owner for the storage account.

#### **Returns**

```json
{
    "shdw_bucket": String,
    "transaction_signature": String
}
```

{% tabs %}
{% tab title="Example" %}
```javascript
//create account
const newAcct = await drive.createStorageAccount("myDemoBucket", "10MB", "v2");
console.log(newAcct);
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Calls the 'createStorageAccount' function on the 'drive' object with "myDemoBucket", "10MB", and "v2" as parameters, and waits for its completion before proceeding. The result of the function call is assigned to the 'newAcct' variable.
const newAcct = await drive.createStorageAccount("myDemoBucket", "10MB", "v2");

// Logs the value of the 'newAcct' variable to the console
console.log(newAcct);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`deleteFile`**

#### **Definition**

This method is used to delete a file on ShadowDrive. It accepts a Public Key of your Storage Account, the Shadow Drive URL of the file you are requesting to delete and the ShadowDrive version (v1 or v2). It returns a Promise containing the confirmed transaction ID of the delete request.

#### **Parameters**

* `key`: `PublicKey` - Publickey of Storage Account
* `url`: `string` - Shadow Drive URL of the file you are requesting to delete.
* `version`: \`ShadowDrive
* Version\` - ShadowDrive version (v1 or v2)

#### **Returns**

```json
{
    "message": String,
    "error": String or not passed if no error
}
```

{% tabs %}
{% tab title="Example" %}
```javascript
const url =
    "https://shdw-drive.genesysgo.net/4HUkENqjnTAZaUR4QLwff1BvQPCiYkNmu5PPSKGoKf9G/fape.png";
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const delFile = await drive.deleteFile(acctPubKey, url, "v2");
console.log(delFile);
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Assigns a string value containing the URL of the file to be deleted to the 'url' variable
const url =
    "https://shdw-drive.genesysgo.net/4HUkENqjnTAZaUR4QLwff1BvQPCiYkNmu5PPSKGoKf9G/fape.png";

// Creates a new public key object with a specific value and assigns it to the 'acctPubKey' variable
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);

// Calls the 'deleteFile' function on the 'drive' object with the account public key, URL, and "v2" as parameters, and waits for its completion before proceeding. The result of the function call is assigned to the 'delFile' variable.
const delFile = await drive.deleteFile(acctPubKey, url, "v2");

// Logs the value of the 'delFile' variable to the console
console.log(delFile);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`deleteStorageAccount`**

#### **Definition**

Implementation of ShadowDrive.deleteStorageAccount defined in index.ts:124 This method is used to delete a Storage Account on ShadowDrive. It accepts a Public Key of the Storage Account and the Shadow Drive version (v1 or v2). It returns a Promise<{ txid: string }> containing the confirmed transaction ID of the delete request.

#### **Parameters**

* `key`: `PublicKey` - Publickey of a Storage Account
* `version`: `ShadowDriveVersion` - ShadowDrive version (v1 or v2)

#### **Returns**

Confirmed transaction ID

{% tabs %}
{% tab title="Example" %}
```javascript
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const delAcct = await drive.deleteStorageAccount(acctPubKey, "v2");
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Creates a new public key object with a specific value and assigns it to the 'acctPubKey' variable
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);

// Calls the 'deleteStorageAccount' function on the 'drive' object with the account public key and "v2" as parameters, and waits for its completion before proceeding. The result of the function call is assigned to the 'delAcct' variable.
const delAcct = await drive.deleteStorageAccount(acctPubKey, "v2");
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`editFile`**

#### **Definition**

This method is used to edit a file on ShadowDrive. It accepts a Public Key of your Storage Account, the URL of the existing file, the File or ShadowFile object, and the ShadowDrive version (v1 or v2). It returns a Promise containing the file location and the transaction signature.

#### **Parameters**

* `key`: `PublicKey` - Publickey of Storage Account
* `url`: `string` - URL of existing file
* `data`: `File | ShadowFile` - File or ShadowFile object, file extensions should be included in the name property of ShadowFiles.
* `version`: `ShadowDriveVersion` - ShadowDrive version (v1 or v2)

#### **Returns**

```json
{
  finalized_location: string;
}
```

{% tabs %}
{% tab title="Example" %}
```javascript
const fileToUpload: ShadowFile = {
    name: "mytext.txt",
    file: fileBuff,
};
const url =
    "https://shdw-drive.genesysgo.net/4HUkENqjnTAZaUR4QLwff1BvQPCiYkNmu5PPSKGoKf9G/fape.png";
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const editFile = await drive.editFile(acctPubKey, url, "v2", fileToUpload);
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Creates an object containing the name and content of the file to upload and assigns it to the 'fileToUpload' variable
const fileToUpload: ShadowFile = {
    name: "mytext.txt",
    file: fileBuff,
};

// Assigns a string value containing the URL of the file to be edited to the 'url' variable
const url =
    "https://shdw-drive.genesysgo.net/4HUkENqjnTAZaUR4QLwff1BvQPCiYkNmu5PPSKGoKf9G/fape.png";

// Creates a new public key object with a specific value and assigns it to the 'acctPubKey' variable
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);

// Calls the 'editFile' function on the 'drive' object with the account public key, URL, "v2", and the file object as parameters, and waits for its completion before proceeding. The result of the function call is assigned to the 'editFile' variable.
const editFile = await drive.editFile(acctPubKey, url, "v2", fileToUpload);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`getStorageAccount`**

#### **Definition**

This method is used to get the details of a Storage Account on ShadowDrive. It accepts a Public Key of the Storage Account and returns a Promise containing the Storage Account details.

#### **Parameters**

* `key`: `PublicKey` - Publickey of a Storage Account

#### **Returns**

```json
{
  storage_account: PublicKey;
  reserved_bytes: number;
  current_usage: number;
  immutable: boolean;
  to_be_deleted: boolean;
  delete_request_epoch: number;
  owner1: PublicKey;
  account_counter_seed: number;
  creation_time: number;
  creation_epoch: number;
  last_fee_epoch: number;
  identifier: string;
  version: `${Uppercase<ShadowDriveVersion>}`;
}
```

{% tabs %}
{% tab title="Example" %}
```javascript
const acct = await drive.getStorageAccount(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
console.log(acct);
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Calls the 'getStorageAccount' function on the 'drive' object with the account public key as a parameter, and waits for its completion before proceeding. The result of the function call is assigned to the 'acct' variable.
const acct = await drive.getStorageAccount(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);

// Logs the resulting object to the console
console.log(acct);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`getStorageAccounts`**

#### **Definition**

This method is used to get a list of all the Storage Accounts associated with the current user. It accepts a ShadowDrive version (v1 or v2). It returns a Promise\<StorageAccountResponse\[]> containing the list of storage accounts.

#### **Parameters**

* `version`: `ShadowDriveVersion` - ShadowDrive version (v1 or v2)

#### **Returns**

```json
{
  publicKey: anchor.web3.PublicKey;
  account: StorageAccount;
}
```

{% tabs %}
{% tab title="Example" %}
```javascript
ShadowDrive.getStorageAccounts(shadowDriveVersion)
    .then((storageAccounts) =>
        console.log(`List of storage accounts: ${storageAccounts}`)
    )
    .catch((err) => console.log(`Error getting storage accounts: ${err}`));
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Calls the 'getStorageAccounts' function on the 'drive' object with the version parameter "v2", and waits for its completion before proceeding. The result of the function call is assigned to the 'accts' variable.
const accts = await drive.getStorageAccounts("v2");

// Uses the 'let' keyword to declare a variable 'acctPubKey', which is assigned the value of the publicKey of the first object in the 'accts' array. This value is converted to a string in Base58 format.
let acctPubKey = new anchor.web3.PublicKey(accts[0].publicKey);
console.log(acctPubKey.toBase58());
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`listObjects`**

#### **Definition**

This method is used to list the Objects in a Storage Account on ShadowDrive. It accepts a Public Key of the Storage Account and returns a Promise containing the list of Objects in the Storage Account.

#### **Parameters**

* `storageAccount`: `PublicKey`

#### **Returns**

```json
{
  keys: string[];
}
```

{% tabs %}
{% tab title="Example" %}
```javascript
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const listItems = await drive.listObjects(acctPubKey);
console.log(listItems);
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Creates a new 'PublicKey' object using a specific public key string and assigns it to the 'acctPubKey' variable.
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);

// Calls the 'listObjects' function on the 'drive' object with the 'acctPubKey' variable as a parameter, and waits for its completion before proceeding. The result of the function call is assigned to the 'listItems' variable.
const listItems = await drive.listObjects(acctPubKey);

// Logs the resulting object to the console.
console.log(listItems);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`makeStorageImmutable`**

#### **Definition**

This method is used to make a Storage Account immutable on ShadowDrive. It accepts a Public Key of the Storage Account and the Shadow Drive version (v1 or v2). It returns a Promise containing the confirmed transaction ID of the makeStorageImmutable request.

#### **Parameters**

* `key`: `PublicKey` - Publickey of Storage Account
* `version`: `ShadowDriveVersion` - ShadowDrive version (v1 or v2)

#### **Returns**

```json
{
  message: string;
  transaction_signature?: string;
}
```

{% tabs %}
{% tab title="Example" %}
```javascript
const key = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const result = await drive.makeStorageImmutable(key, "v2");
console.log(result);
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Create a new PublicKey object using a public key string.
const key = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);

// Call the makeStorageImmutable function with the PublicKey object and a version string, and wait for it to complete.
const result = await drive.makeStorageImmutable(key, "v2");

// Log the resulting object to the console.
console.log(result);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`migrate`**

#### **Definition**

This method is used to migrate a Storage Account on ShadowDrive. It accepts a PublicKey of the Storage Account. It returns a Promise<{ txid: string }> containing the confirmed transaction ID of the migration request.

#### **Parameters**

* `key`: `PublicKey` - Publickey of Storage Account

#### **Returns**

Confirmed transaction ID

{% tabs %}
{% tab title="Example" %}
```javascript
const result = await drive.migrate(key);
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Call the migrate function on the drive object, passing in the PublicKey object as a parameter.
const result = await drive.migrate(key);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`redeemRent`**

#### **Definition**

This method is used to redeem Rent on ShadowDrive. It accepts a Public Key of the Storage Account and the Public Key of the file account to close. It returns a Promise<{ txid: string }> containing the confirmed transaction ID of the redeemRent request.

#### **Parameters**

* `key`: `PublicKey` - Publickey of Storage Account
* `fileAccount`: `PublicKey` - PublicKey of the file account to close

#### **Returns**

Confirmed transaction ID

{% tabs %}
{% tab title="Example" %}
```javascript
const fileAccount = new anchor.web3.PublicKey(
    "3p6U9s1sGLpnpkMMwW8o4hr4RhQaQFV7MkyLuW8ycvG9"
);
const result = await drive.redeemRent(key, fileAccount);
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Create a new PublicKey object using a public key string for the file account.
const fileAccount = new anchor.web3.PublicKey(
    "3p6U9s1sGLpnpkMMwW8o4hr4RhQaQFV7MkyLuW8ycvG9"
);

// Call the redeemRent function on the drive object, passing in both PublicKey objects as parameters.
const result = await drive.redeemRent(key, fileAccount);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`reduceStorage`**

#### **Definition**

This method is used to reduce the storage of a Storage Account on ShadowDrive. It accepts a Public Key of the Storage Account, the amount of storage you are requesting to reduce from your storage account, and the Shadow Drive version (v1 or v2). It returns a Promise containing the confirmed transaction ID of the reduce storage request.

#### **Parameters**

* `key`: `PublicKey` - Publickey of Storage Account
* `size`: `string` - Amount of storage you are requesting to reduce from your storage account. Should be in a string like '1KB', '1MB', '1GB'. Only KB, MB, and GB storage delineations are supported currently.
* `version`: `ShadowDriveVersion` - ShadowDrive version (v1 or v2)

#### **Returns**

```json
{
  message: string;
  transaction_signature?: string;
}
```

{% tabs %}
{% tab title="Example" %}
```javascript
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const shrinkAcct = await drive.reduceStorage(acctPubKey, "10MB", "v2");
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Create a new public key object with the given string
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);

// Reduce the storage size of the storage account with the given public key
// to 10MB using the version specified
const shrinkAcct = await drive.reduceStorage(acctPubKey, "10MB", "v2");
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`storageConfigPDA`**

#### **Definition**

This exposes the PDA account in case developers have a need to display / use the data stored in the account.

#### **Parameters**

* `key`: `PublicKey` - Publickey of Storage Account
* `data`: `File | ShadowFile` - File or ShadowFile object, file extensions should be included in the name property of ShadowFiles.

#### **Returns**

Public Key

{% tabs %}
{% tab title="Example" %}
```javascript
storageConfigPDA: PublicKey;
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
//storageConfigPDA is a method in the Shadow SDK that returns the public key of the program derived account (PDA) for the Shadow storage program's config. A program derived account is a special account on the Solana blockchain that is derived from a program's public key and a specific seed. The purpose of this method is to provide a convenient way to obtain the PDA for the Shadow storage program's config. The config contains important information such as the current storage rent exemption threshold and the data size limits for storage accounts. This public key can be used to interact with the Shadow storage program's config account, allowing the user to retrieve and modify the program's global configuration settings.
storageConfigPDA: PublicKey;
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`uploadFile`**

#### **Definition**

This method is used to upload a file to ShadowDrive. It accepts a Public Key of your Storage Account and a File or ShadowFile object. The file extensions should be included in the name property of ShadowFiles. It returns a Promise containing the file location and the transaction signature.

#### **Parameters**

* `key`: `PublicKey` - Publickey of Storage Account.
* `data`: `File | ShadowFile` - File or ShadowFile object, file extensions should be included in the name property of ShadowFiles.

#### **Returns**

```json
{
  finalized_locations: Array<string>;
  message: string;
  upload_errors: Array<UploadError>;
}
```

{% tabs %}
{% tab title="Example" %}
```javascript
const uploadFile = await drive.uploadFile(acctPubKey, fileToUpload);
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// This line calls the uploadFile method of the drive object and passes in two parameters:
// 1. acctPubKey: A PublicKey object representing the public key of the storage account where the file will be uploaded.
// 2. fileToUpload: A ShadowFile object containing the file name and file buffer to be uploaded.
const uploadFile = await drive.uploadFile(acctPubKey, fileToUpload);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`uploadMultipleFiles`**

#### **Definition**

This method is used to upload multiple files to a Storage Account on ShadowDrive. It accepts the Storage Account's PublicKey, a data object containing the FileList or ShadowFile array of files to upload, an optional concurrent number for the number of files to concurrently upload, and an optional callback function for every batch of files uploaded. It returns a Promise\<ShadowBatchUploadResponse\[]> containing the file names, locations and transaction signatures for uploaded files.

#### **Parameters**

* `key`: `PublicKey` - Storage account PublicKey to upload the files to.
* `data`: `FileList | ShadowFile[]` -
* `concurrent` (optional): `number` - Number of files to concurrently upload. Default: 3
* `callback` (optional): `Function` - Callback function for every batch of files uploaded. A number will be passed into the callback like callback(num) indicating the number of files that were confirmed in that specific batch.

#### **Returns**

```json
{
  fileName: string;
  status: string;
  location: string;
}
```

{% tabs %}
{% tab title="Example" %}
```javascript
const drive = new ShadowDrive();
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);
const files = [
    {
        name: "file1.txt",
        file: new File(["hello"], "file1.txt"),
    },
    {
        name: "file2.txt",
        file: new File(["world"], "file2.txt"),
    },
    {
        name: "file3.txt",
        file: new File(["!"], "file3.txt"),
    },
];
const concurrentUploads = 2;
const callback = (response) => {
    console.log(`Uploaded file ${response.fileIndex}: ${response.fileName}`);
};
const responses = await drive.uploadMultipleFiles(
    acctPubKey,
    files,
    concurrentUploads,
    callback
);
console.log(responses);
```
{% endtab %}

{% tab title="Explanations" %}
{% code overflow="wrap" %}
```javascript
// Create an instance of the Shadow Drive client
const drive = new ShadowDrive();

// Define the public key of the storage account where the files will be uploaded
const acctPubKey = new anchor.web3.PublicKey(
    "EY8ZktbRmecPLfopBxJfNBGUPT1LMqZmDFVcWeMTGPcN"
);

// Define an array of files to upload
const files = [
    {
        name: "file1.txt",
        file: new File(["hello"], "file1.txt"),
    },
    {
        name: "file2.txt",
        file: new File(["world"], "file2.txt"),
    },
    {
        name: "file3.txt",
        file: new File(["!"], "file3.txt"),
    },
];

// Define the maximum number of concurrent uploads (optional)
const concurrentUploads = 2;

// Define a callback function to be called after each file is uploaded (optional)
const callback = (response) => {
    console.log(`Uploaded file ${response.fileIndex}: ${response.fileName}`);
};

// Call the uploadMultipleFiles method to upload all the files
const responses = await drive.uploadMultipleFiles(
    acctPubKey,
    files,
    concurrentUploads,
    callback
);

// Print the responses returned by the server for each file uploaded
console.log(responses);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### **`userInfo`**

#### **Definition**

userInfo: PublicKey

### **Example - POST request via SDK (make immutable)**
```java
import * as anchor from "@project-serum/anchor";
import { getStakeAccount, findAssociatedTokenAddress } from "../utils/helpers";
import {
  emissions,
  isBrowser,
  SHDW_DRIVE_ENDPOINT,
  tokenMint,
  uploader,
} from "../utils/common";
import {
  ASSOCIATED_TOKEN_PROGRAM_ID,
  TOKEN_PROGRAM_ID,
} from "@solana/spl-token";
import { ShadowDriveVersion, ShadowDriveResponse } from "../types";
import fetch from "node-fetch";
/**
 *
 * @param {anchor.web3.PublicKey} key - Publickey of a Storage Account
 * @param {ShadowDriveVersion} version - ShadowDrive version (v1 or v2)
 * @returns {ShadowDriveResponse} - Confirmed transaction ID
 */
export default async function makeStorageImmutable(
  key: anchor.web3.PublicKey,
  version: ShadowDriveVersion
): Promise<ShadowDriveResponse> {
  let selectedAccount;
  try {
    switch (version.toLocaleLowerCase()) {
      case "v1":
        selectedAccount = await this.program.account.storageAccount.fetch(key);
        break;
      case "v2":
        selectedAccount = await this.program.account.storageAccountV2.fetch(
          key
        );
        break;
    }
    const ownerAta = await findAssociatedTokenAddress(
      selectedAccount.owner1,
      tokenMint
    );
    const emissionsAta = await findAssociatedTokenAddress(emissions, tokenMint);
    let stakeAccount = (await getStakeAccount(this.program, key))[0];
    let txn;
    switch (version.toLocaleLowerCase()) {
      case "v1":
        txn = await this.program.methods
          .makeAccountImmutable()
          .accounts({
            storageConfig: this.storageConfigPDA,
            storageAccount: key,
            stakeAccount,
            emissionsWallet: emissionsAta,
            owner: selectedAccount.owner1,
            uploader: uploader,
            ownerAta,
            tokenMint: tokenMint,
            systemProgram: anchor.web3.SystemProgram.programId,
            tokenProgram: TOKEN_PROGRAM_ID,
            associatedTokenProgram: ASSOCIATED_TOKEN_PROGRAM_ID,
            rent: anchor.web3.SYSVAR_RENT_PUBKEY,
          })
          .transaction();
      case "v2":
        txn = await this.program.methods
          .makeAccountImmutable2()
          .accounts({
            storageConfig: this.storageConfigPDA,
            storageAccount: key,
            owner: selectedAccount.owner1,
            ownerAta,
            stakeAccount,
            uploader: uploader,
            emissionsWallet: emissionsAta,
            tokenMint: tokenMint,
            systemProgram: anchor.web3.SystemProgram.programId,
            tokenProgram: TOKEN_PROGRAM_ID,
            associatedTokenProgram: ASSOCIATED_TOKEN_PROGRAM_ID,
            rent: anchor.web3.SYSVAR_RENT_PUBKEY,
          })
          .transaction();
        break;
    }
    txn.recentBlockhash = (
      await this.connection.getLatestBlockhash()
    ).blockhash;
    txn.feePayer = this.wallet.publicKey;
    let signedTx;
    let serializedTxn;
    if (!isBrowser) {
      await txn.partialSign(this.wallet.payer);
      serializedTxn = txn.serialize({ requireAllSignatures: false });
    } else {
      signedTx = await this.wallet.signTransaction(txn);
      serializedTxn = signedTx.serialize({ requireAllSignatures: false });
    }
    const makeImmutableResponse = await fetch(
      `${SHDW_DRIVE_ENDPOINT}/make-immutable`,
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          transaction: Buffer.from(serializedTxn.toJSON().data).toString(
            "base64"
          ),
        }),
      }
    );
    if (!makeImmutableResponse.ok) {
      return Promise.reject(
        new Error(`Server response status code: ${
          makeImmutableResponse.status
        } \n
			Server response status message: ${(await makeImmutableResponse.json()).error}`)
      );
    }
    const responseJson = await makeImmutableResponse.json();
    return Promise.resolve(responseJson);
  } catch (e) {
    return Promise.reject(new Error(e));
  }
}
```



#### **Help improve this** [**resource**](sdk-javascript.md) **or provide us with** [**feedback**](discord/)**.**
