---
coverY: 0
---

# Methods

## **Contents**

* [**add\_immutable\_storage**](./#add\_immutable\_storage)

## **ad**

## **ad**

### ad

## **add\_storage**

{% tabs %}
{% tab title="Javascript" %}
{% code overflow="wrap" lineNumbers="true" %}
```javascript
const accts = await drive.getStorageAccounts("v2") let acctPubKey = new anchor.web3.PublicKey(accts[1].publicKey) const addStgResp = await drive.addStorage(acctPubKey,"10MB","v2")
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code overflow="wrap" lineNumbers="true" %}
```rust
let add_storage_response = shdw_drive_client
    .add_storage(storage_account_key, Byte::from_str("1MB").expect("invalid byte string"))
    .await?;
```
{% endcode %}
{% endtab %}
{% endtabs %}

## **Javascript/TS**

## **Rust**

### **Rust Methods**

#### **`add_immutable_storage`**

Adds storage capacity to the specified immutable `StorageAccount`. This will fail if the `StorageAccount` is not immutable.

* **storage\_account\_key** - The public key of the immutable `StorageAccount`.
* **size** - The additional amount of storage you want to add. E.g if you have an existing StorageAccount with 1MB of storage but you need 2MB total, size should equal 1MB. When specifying size, only KB, MB, and GB storage units are currently supported.

**Example**

```rust
let add_immutable_storage_response = shdw_drive_client
    .add_immutable_storage(storage_account_key, Byte::from_str("1MB").expect("invalid byte string"))
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

#### **`add_storage`**

Adds storage capacity to the specified StorageAccount.

* **storage\_account\_key** - The public key of the StorageAccount.
* **size** - The additional amount of storage you want to add. E.g if you have an existing StorageAccount with 1MB of storage but you need 2MB total, size should equal 1MB. When specifying size, only KB, MB, and GB storage units are currently supported.

**Example**

```rust
let add_storage_response = shdw_drive_client
    .add_storage(storage_account_key, Byte::from_str("1MB").expect("invalid byte string"))
    .await?;
```

**Response**

```json
{
    pub message: String,
    pub transaction_signature: String,
    pub error: Option<String>,
}
```

#### **`cancel_delete_storage_account`**

Unmarks a StorageAccount for deletion from the Shadow Drive. To prevent deletion, this method must be called before the end of the Solana epoch in which `delete_storage_account` is called.

* **storage\_account\_key** - The public key of the `StorageAccount` that you want to unmark for deletion.

**Example**

```rust
let cancel_delete_storage_account_response = shdw_drive_client
    .cancel_delete_storage_account(&storage_account_key)
    .await?;
```

**Response**

```json
{
    pub txid: String,
}
```

#### **`claim_stake`**

Claims any available stake as a result of the `reduce_storage` command. After reducing storage amount, users must wait until the end of the epoch to successfully claim their stake.

* **storage\_account\_key** - The public key of the StorageAccount that you want to claim excess stake from.

**Example**

```rust
let claim_stake_response = shdw_drive_client
    .claim_stake(&storage_account_key)
    .await?;
```

**Response**

```json
{
    pub txid: String,
}
```

#### **`create_storage_account`**

Creates a `StorageAccount` on the Shadow Drive. `StorageAccount`s can hold multiple files and are paid for using the SHDW token.

* **name** - The name of the `StorageAccount`. Does not need to be unique.
* **size** - The amount of storage the `StorageAccount` should be initialized with. When specifying size, only KB, MB, and GB storage units are currently supported.

**Example**

_examples/end\_to\_end.rs (lines 24-28)_

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

**Response**

```json
{
    pub message: String,
    pub transaction_signature: String,
    pub storage_account_address: String,
    pub error: Option<String>,
}
```

#### **`delete_file`**

Marks a file for deletion from the Shadow Drive. Files marked for deletion are deleted at the end of each Solana epoch. Marking a file for deletion can be undone with cancel\_delete\_file, but this must be done before the end of the Solana epoch.

* **storage\_account\_key** - The public key of the `StorageAccount` that contains the file.
* **url** - The Shadow Drive url of the file you want to mark for deletion.

**Example**

```rust
let delete_file_response = shdw_drive_client
    .delete_file(&storage_account_key, url)
    .await?;
```

**Examples Found in Repository**

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

#### **`delete_storage_account`**

This function marks a StorageAccount for deletion from the Shadow Drive. If an account is marked for deletion, all files within the account will be deleted as well. Any stake remaining in the StorageAccount will be refunded to the creator. Accounts marked for deletion are deleted at the end of each Solana epoch.

**Parameters**

* `storage_account_key` - The public key of the StorageAccount that you want to mark for deletion.

**Return**

* This method returns success if it can successfully mark the account for deletion and refund any remaining stake in the account before the end of the current Solana epoch.

**Example**

```rust
let delete_storage_account_response = shdw_drive_client
    .delete_storage_account(&storage_account_key)
    .await?;
```

An example use case for this method can be found in the same [github repository](https://github.com/phantom-labs/shadow\_sdk/blob/master/examples/end\_to\_end.rs) on line 71.

#### **`edit_file`**

Replace an existing file on the Shadow Drive with the given updated file.

* **storage\_account\_key** - The public key of the `StorageAccount` that contains the file.
* **url** - The Shadow Drive url of the file you want to replace.
* **data** - The updated `ShadowFile`.

**Example**

```rust
let edit_file_response = shdw_drive_client
    .edit_file(&storage_account_key, url, file)
    .await?;
```

**Response**

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

#### **`get_storage_account`**

Returns the `StorageAccount` associated with the pubkey provided by a user.

* **key** - The public key of the `StorageAccount`.

**Example**

```rust
let storage_account = shdw_drive_client
    .get_storage_account(&storage_account_key)
    .await
    .expect("failed to get storage account");
```

**Response for V1 StorageAccount**

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

**Response for V2 StorageAccount**

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

#### **`get_storage_accounts`**

Returns all `StorageAccounts` associated with the public key provided by a user.

* `owner` - The public key that is the owner of all the returned `StorageAccounts`.

**Example**

```rust
let storage_accounts = shdw_drive_client
    .get_storage_accounts(&user_pubkey)
    .await
    .expect("failed to get storage account");
```

**Response for V1 StorageAccount**

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

**Response for V2 StorageAccount**

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

#### **`list_objects`**

Gets a list of all files associated with a `StorageAccount`. The output contains all of the file names as strings.

* **storage\_account\_key** - The public key of the `StorageAccount` that owns the files.

**Example**

```rust
let files = shdw_drive_client
    .list_objects(&storage_account_key)
    .await?;
```

#### **Response**

Note: The response is a vector containing all of the file names as strings.

#### **`make_storage_immutable`**

Permanently locks a `StorageAccount` and all contained files. After a `StorageAccount` has been locked, a user will no longer be able to delete/edit files, add/reduce storage amount, or delete the `StorageAccount`.

* **storage\_account\_key** - The public key of the `StorageAccount` that will be made immutable.

**Example**

```rust
let make_immutable_response = shdw_drive_client  
    .make_storage_immutable(&storage_account_key)  
    .await?;  
```

**Response**

```json
{
    pub message: String,
    pub transaction_signature: String,
    pub error: Option<String>,
}
```

#### **`migrate`**

Migrates a v1 StorageAccount to v2. This requires two separate transactions to reuse the original pubkey. To minimize chance of failure, it is recommended to call this method with a commitment level of Finalized.

* **storage\_account\_key** - The public key of the StorageAccount to be migrated.

**Example**

```rust
let migrate_response = shdw_drive_client
    .migrate(&storage_account_key)
    .await?;
```

**Result**

```json
{
    pub txid: String,
}
```

**`migrate_step_1`**

First transaction step that migrates a v1 `StorageAccount` to v2. Consists of copying the existing account’s data into an intermediate account, and deleting the v1 `StorageAccount`.

**`migrate_step_2`**

Second transaction step that migrates a v1 `StorageAccount` to v2. Consists of recreating the `StorageAccount` using the original pubkey, and deleting the intermediate account.

#### **`new`**

Creates a new ShadowDriveClient from the given `Signer` and URL.

* **wallet** - A `Signer` that for signs all transactions generated by the client. Typically this is a user’s keypair.
* **rpc\_url** - An HTTP URL of a Solana RPC provider.\
  The underlying Solana RPC client is configured with 120s timeout and a commitment level of confirmed.

To customize RpcClient settings see `new_with_rpc`.

**Example**

```rust
use solana_sdk::signer::keypair::Keypair;    

let wallet = Keypair::generate();
let shdw_drive = ShadowDriveClient::new(wallet, "https://ssc-dao.genesysgo.net");
```

Examples found in repository?\
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

#### **`new_with_rpc`**

Creates a new ShadowDriveClient from the given `Signer` and `RpcClient`.

* **wallet** - A `Signer` that for signs all transactions generated by the client. Typically this is a user’s keypair.
* **rpc\_client** - A Solana `RpcClient` that handles sending transactions and reading accounts from the blockchain.\
  Providng the `RpcClient` allows customization of timeout and committment level.

**Example**

```rust
use solana_client::rpc_client::RpcClient;
use solana_sdk::signer::keypair::Keypair;    
use solana_sdk::commitment_config::CommitmentConfig;

let wallet = Keypair::generate();
let solana_rpc = RpcClient::new_with_commitment("https://ssc-dao.genesysgo.net", CommitmentConfig::confirmed());
let shdw_drive = ShadowDriveClient::new_with_rpc(wallet, solana_rpc);
```

#### **`redeem_rent`**

Reclaims the Solana rent from any on-chain file accounts. Older versions of the Shadow Drive used to create accounts for uploaded files.

* **storage\_account\_key** - The public key of the StorageAccount that contained the deleted file.
* **file\_account\_key** - The public key of the File account to be closed.

**Example**

```rust
let redeem_rent_response = shdw_drive_client
    .redeem_rent(&storage_account_key, &file_account_key)
    .await?;
```

**Response**

```json
{
  pub message: String,
  pub transaction_signature: String,
  pub error: Option<String>,
}
```

#### **`reduce_storage`**

Reduces the amount of total storage available for the given `StorageAccount`.

* **storage\_account\_key** - The public key of the `StorageAccount` whose storage will be reduced.
* **size** - The amount of storage you want to remove. E.g if you have an existing `StorageAccount` with 3MB of storage but you want 2MB total, size should equal 1MB. When specifying size, only KB, MB, and GB storage units are currently supported.

**Example**

```rust
let reduce_storage_response = shdw_drive_client
    .reduce_storage(&storage_account_key, reduced_bytes)
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

#### **`store_files`**

Stores files in the specified `StorageAccount`.

* **storage\_account\_key** - The public key of the `StorageAccount`.
* **data** - Vector of `ShadowFile` objects representing the files that will be stored.

**Example**

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

**Response**

```json
{
  pub message: String,
  pub transaction_signature: String,
  pub error: Option<String>,
}
```
