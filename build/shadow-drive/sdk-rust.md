# **Contents**
* **[Install](#install)**
* **[Example](#example)**
* **[Methods](#methods)**
    * **[add_immutable_storage](#add_immutable_storage)**
    * **[add_storage](#add_storage)**
    * **[cancel_delete_storage_account](#cancel_delete_storage_account)**
    * **[claim_stake](#claim_stake)**
    * **[create_storage_account](#create_storage_account)**
    * **[delete_file](#delete_file)**
    * **[delete_storage_account](#delete_storage_account)** 
    * **[edit_file](#edit_file)**
    * **[get_object_data](#get_object_data)**
    * **[get_storage_account](#get_storage_account)**
    * **[get_storage_accounts](#get_storage_accounts)**
    * **[list_objects](#list_objects)**
    * **[make_storage_immutable](#make_storage_immutable)**
    * **[migrate](#migrate)**
    * **[migrate_step_1](#migrate_step_1)**
    * **[migrate_step_2](#migrate_step_2)**
    * **[new](#new)**
    * **[new_with_rpc](#new_with_rpc)**
    * **[redeem_rent](#redeem_rent)**
    * **[reduce_storage](#reduce_storage)**
    * **[store_files](#store_files)**

## **Install**

The Rust SDK is available on [crates.io](https://crates.io/crates/shadow-drive-sdk) and Rust SDK [Github](https://github.com/GenesysGo/shadow-drive-rust)

Run the following Cargo command in your project directory:  
`cargo add shadow-drive-sdk`

Or add the following line to your Cargo.toml  
`shadow-drive-sdk = "0.6.1"`

#### **You can find more examples on our [Github](https://github.com/GenesysGo/shadow-drive-rust/blob/main/sdk/examples/end_to_end.rs)**

## **Example**

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

# **Methods**

## **`add_immutable_storage`**

### **Definition**
Adds storage capacity to the specified immutable` StorageAccount`. This will fail if the `StorageAccount` is not immutable.

### **Parameters**
* `storage_account_key` - The public key of the immutable `StorageAccount`.
* `size` - The additional amount of storage you want to add. E.g if you have an existing StorageAccount with 1MB of storage but you need 2MB total, size should equal 1MB. When specifying size, only KB, MB, and GB storage units are currently supported.

### **Example**
```rust
let add_immutable_storage_response = shdw_drive_client
    .add_immutable_storage(storage_account_key, Byte::from_str("1MB").expect("invalid byte string"))
    .await?;
```

### **Response**
```json
{
    message: String,
    transaction_signature: String,
    error: Option<String>,
}
```

## **`add_storage`**

### **Definition**
Adds storage capacity to the specified StorageAccount.

### **Parameters**

* `storage_account_key` - The public key of the StorageAccount.
* `size` - The additional amount of storage you want to add. E.g if you have an existing StorageAccount with 1MB of storage but you need 2MB total, size should equal 1MB. When specifying size, only KB, MB, and GB storage units are currently supported.

### **Example**
```rust
let add_immutable_storage_response = shdw_drive_client
    .add_immutable_storage(storage_account_key, Byte::from_str("1MB").expect("invalid byte string"))
    .await?;
```

### **Response**
```json
{
    message: String,
    transaction_signature: String,
    error: Option<String>,
}
```

## **`cancel_delete_storage_account`**

### **Definition**

Unmarks a StorageAccount for deletion from the Shadow Drive. To prevent deletion, this method must be called before the end of the Solana epoch in which `delete_storage_account` is called.

### **Parameters**
*   `storage_account_key` - The public key of the `StorageAccount` that you want to unmark for deletion.

### **Example**

```rust
let cancel_delete_storage_account_response = shdw_drive_client
    .cancel_delete_storage_account(&storage_account_key)
    .await?;
```
### **Response**

```json
{
    txid: String,
}
```

## **`claim_stake`**

### **Definition**
Claims any available stake as a result of the `reduce_storage` command. After reducing storage amount, users must wait until the end of the epoch to successfully claim their stake.

### **Parameters**
*   `storage_account_key` - The public key of the StorageAccount that you want to claim excess stake from.

### **Example**
```rust
let claim_stake_response = shdw_drive_client
    .claim_stake(&storage_account_key)
    .await?;
```

### **Response**

```json
{
    txid: String,
}
```

## **`create_storage_account`**

### **Definition**

Creates a `StorageAccount` on the Shadow Drive. `StorageAccount`s can hold multiple files and are paid for using the SHDW token.

### **Parameters**
*   `name` - The name of the `StorageAccount`. Does not need to be unique.
*   `size` - The amount of storage the `StorageAccount` should be initialized with. When specifying size, only KB, MB, and GB storage units are currently supported.

### **Example**

An example use case for this method can be found in the same [github repository](https://github.com/phantom-labs/shadow_sdk/blob/master/examples/end_to_end.rs)

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

### **Response**

```json
{
    message: String,
    transaction_signature: String,
    storage_account_address: String,
    error: Option<String>,
}
```

## **`delete_file`**

### **Definition**

Marks a file for deletion from the Shadow Drive. Files marked for deletion are deleted at the end of each Solana epoch. Marking a file for deletion can be undone with cancel_delete_file, but this must be done before the end of the Solana epoch.

### **Parameters**

*   `storage_account_key` - The public key of the `StorageAccount` that contains the file.
*   `url` - The Shadow Drive url of the file you want to mark for deletion.

### **Example**

```rust
let delete_file_response = shdw_drive_client
    .delete_file(&storage_account_key, url)
    .await?;
```

An example use case for this method can be found in the same [github repository](https://github.com/phantom-labs/shadow_sdk/blob/master/examples/end_to_end.rs)

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

## **`delete_storage_account`**

### **Definition**

This function marks a StorageAccount for deletion from the Shadow Drive. If an account is marked for deletion, all files within the account will be deleted as well. Any stake remaining in the StorageAccount will be refunded to the creator. Accounts marked for deletion are deleted at the end of each Solana epoch.

### **Parameters**
*   `storage_account_key` - The public key of the StorageAccount that you want to mark for deletion.

### **Response**

*   This method returns success if it can successfully mark the account for deletion and refund any remaining stake in the account before the end of the current Solana epoch.

### Example

```rust
let delete_storage_account_response = shdw_drive_client
    .delete_storage_account(&storage_account_key)
    .await?;
```
An example use case for this method can be found in the same [github repository](https://github.com/phantom-labs/shadow_sdk/blob/master/examples/end_to_end.rs) on line 71.


## **`edit_file`**

### **Definition**

Replace an existing file on the Shadow Drive with the given updated file.

### **Parameters**

*   `storage_account_key` - The public key of the `StorageAccount` that contains the file.
*   `url` - The Shadow Drive url of the file you want to replace.
*   `data` - The updated `ShadowFile`.

### **Example**

```rust
let edit_file_response = shdw_drive_client
    .edit_file(&storage_account_key, url, file)
    .await?;
```

### **Response**

```json
{
    finalized_location: String,
    error: String,
}
```

Examples found in [repository](https://github.com/GenesysGo/shadow-drive-rust/blob/main/sdk/examples/end_to_end.rs)

File: examples/end_to_end.rs, Line 53

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
## **`get_object_data`**

Retrieve object data

## **`get_storage_account`**

### **Definition**

Returns the `StorageAccount` associated with the pubkey provided by a user.

### **Parameters**

*   `key` - The public key of the `StorageAccount`.

### **Example**

```rust
let storage_account = shdw_drive_client
    .get_storage_account(&storage_account_key)
    .await
    .expect("failed to get storage account");
```

### **Response for V1 StorageAccount**

```json
{
    storage_account: Pubkey,
    reserved_bytes: u64,
    current_usage: u64,
    immutable: bool,
    to_be_deleted: bool,
    delete_request_epoch: u32,
    owner_1: Pubkey,
    owner_2: Pubkey,
    account_counter_seed: u32,
    creation_time: u32,
    creation_epoch: u32,
    last_fee_epoch: u32,
    identifier: String,
}
```
### **Response for V2 StorageAccount**
```json
{
    storage_account: Pubkey,
    reserved_bytes: u64,
    current_usage: u64,
    immutable: bool,
    to_be_deleted: bool,
    delete_request_epoch: u32,
    owner_1: Pubkey,
    account_counter_seed: u32,
    creation_time: u32,
    creation_epoch: u32,
    last_fee_epoch: u32,
    identifier: String,
}
```

## **`get_storage_accounts`**

### **Definition**

Returns all `StorageAccounts` associated with the public key provided by a user.

### **Parameters**

*   `owner` - The public key that is the owner of all the returned `StorageAccounts`.

### **Example**

```rust
let storage_accounts = shdw_drive_client
    .get_storage_accounts(&user_pubkey)
    .await
    .expect("failed to get storage account");
```

### **Response for V1 StorageAccount**

```json
{

    storage_account: Pubkey,
    reserved_bytes: u64,
    current_usage: u64,
    immutable: bool,
    to_be_deleted: bool,
    delete_request_epoch: u32,
    owner_1: Pubkey,
    owner_2: Pubkey,
    account_counter_seed: u32,
    creation_time: u32,
    creation_epoch: u32,
    last_fee_epoch: u32,
    identifier: String,
}
```
### **Response for V2 StorageAccount**
```json
{
    storage_account: Pubkey,
    reserved_bytes: u64,
    current_usage: u64,
    immutable: bool,
    to_be_deleted: bool,
    delete_request_epoch: u32,
    owner_1: Pubkey,
    account_counter_seed: u32,
    creation_time: u32,
    creation_epoch: u32,
    last_fee_epoch: u32,
    identifier: String,
}
```

## **`list_objects`**

### **Definition**
Gets a list of all files associated with a `StorageAccount`. The output contains all of the file names as strings.

### **Parameters**

*   `storage_account_key` - The public key of the `StorageAccount` that owns the files.

### **Example**

```rust
let files = shdw_drive_client
    .list_objects(&storage_account_key)
    .await?;
```

### **Response**
Note: The response is a vector containing all of the file names as strings.

## **`make_storage_immutable`**

### **Definition**

Permanently locks a `StorageAccount` and all contained files. After a `StorageAccount` has been locked, a user will no longer be able to delete/edit files, add/reduce storage amount, or delete the `StorageAccount`.

### **Parameters**

*   `storage_account_key` - The public key of the `StorageAccount` that will be made immutable.

### **Example**

```rust
let make_immutable_response = shdw_drive_client  
    .make_storage_immutable(&storage_account_key)  
    .await?;  
```

### **Response**

```json
{
    message: String,
    transaction_signature: String,
    error: Option<String>,
}
```

## **`migrate`**

### **Definition**

Migrates a v1 StorageAccount to v2. This requires two separate transactions to reuse the original pubkey. To minimize chance of failure, it is recommended to call this method with a commitment level of Finalized.

### **Parameters**

*   `storage_account_key` - The public key of the StorageAccount to be migrated.

### **Example**

```rust
let migrate_response = shdw_drive_client
    .migrate(&storage_account_key)
    .await?;
```

###  **Result**
```json
{
    txid: String,
}
```

## **`migrate_step_1`**

### **Definition**

First transaction step that migrates a v1 `StorageAccount` to v2. Consists of copying the existing account’s data into an intermediate account, and deleting the v1 `StorageAccount`.

## **`migrate_step_2`**

### **Definition**

Second transaction step that migrates a v1 `StorageAccount` to v2. Consists of recreating the `StorageAccount` using the original pubkey, and deleting the intermediate account.


## **`new`**

### **Definition**

Creates a new ShadowDriveClient from the given `Signer` and URL.

### **Parameters**

*   `wallet` - A `Signer` that for signs all transactions generated by the client. Typically this is a user’s keypair.
*   `rpc_url` - An HTTP URL of a Solana RPC provider.  
    The underlying Solana RPC client is configured with 120s timeout and a commitment level of confirmed.

To customize RpcClient settings see `new_with_rpc`.

### **Example**

```rust
use solana_sdk::signer::keypair::Keypair;    

let wallet = Keypair::generate();
let shdw_drive = ShadowDriveClient::new(wallet, "https://ssc-dao.genesysgo.net");
```

Examples found in [repository](https://github.com/GenesysGo/shadow-drive-rust/blob/main/sdk/examples/end_to_end.rs)

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

## **`new_with_rpc`**

### **Definition**

Creates a new ShadowDriveClient from the given `Signer` and `RpcClient`.

### **Parameters**

*   `wallet` - A `Signer` that for signs all transactions generated by the client. Typically this is a user’s keypair.
*   `rpc_client` - A Solana `RpcClient` that handles sending transactions and reading accounts from the blockchain.  
    Providng the `RpcClient` allows customization of timeout and committment level.

### **Example**

```rust
use solana_client::rpc_client::RpcClient;
use solana_sdk::signer::keypair::Keypair;    
use solana_sdk::commitment_config::CommitmentConfig;

let wallet = Keypair::generate();
let solana_rpc = RpcClient::new_with_commitment("https://ssc-dao.genesysgo.net", CommitmentConfig::confirmed());
let shdw_drive = ShadowDriveClient::new_with_rpc(wallet, solana_rpc);
```

## **`redeem_rent`**

### **Definition**

Reclaims the Solana rent from any on-chain file accounts. Older versions of the Shadow Drive used to create accounts for uploaded files.

### **Parameters**

*   `storage_account_key` - The public key of the StorageAccount that contained the deleted file.
*   `file_account_key` - The public key of the File account to be closed.

### **Example**

```rust
let redeem_rent_response = shdw_drive_client
    .redeem_rent(&storage_account_key, &file_account_key)
    .await?;
```

### **Response**
```json
{
  message: String,
  transaction_signature: String,
  error: Option<String>,
}
```

## **`reduce_storage`**

### **Definition**

Reduces the amount of total storage available for the given `StorageAccount`.

### **Parameters**

*   `storage_account_key` - The public key of the `StorageAccount` whose storage will be reduced.
*   `size` - The amount of storage you want to remove. E.g if you have an existing `StorageAccount` with 3MB of storage but you want 2MB total, size should equal 1MB. When specifying size, only KB, MB, and GB storage units are currently supported.

### **Example**

```rust
let reduce_storage_response = shdw_drive_client
    .reduce_storage(&storage_account_key, reduced_bytes)
    .await?;
```

### **Response**

```json
{
  message: String,
  transaction_signature: String,
  error: Option<String>,
}
```

## **`store_files`**

### **Definition**

Stores files in the specified `StorageAccount`.

### **Parameters**

*   `storage_account_key` - The public key of the `StorageAccount`.
*   `data` - Vector of `ShadowFile` objects representing the files that will be stored.

### **Example**

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

### **Response**

```json
{
  finalized_locations: Array<string>;
  message: string;
  upload_errors: Array<UploadError>;
}
```