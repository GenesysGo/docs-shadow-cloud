---
description: >-
  ShdwDrive exposes an API that you can interact directly without the need of
  the CLI or SDK. You may build on top of these methods.
---

# API

## **Contents**

* [**Example - Sign and upload a file**](the-api.md#example-sign-and-upload-a-file)
* [**Example - Edit a file**](the-api.md#example-edit-a-file)
* [**Example - Delete a file**](the-api.md#example-delete-a-file)

## storage-account

<mark style="color:green;">`POST`</mark> `https://shadow-storage.genesysgo.net`

Creates a new storage account

Request content type: application/json

#### Request Body

| Name                                          | Type | Description                                                                                        |
| --------------------------------------------- | ---- | -------------------------------------------------------------------------------------------------- |
| transaction<mark style="color:red;">\*</mark> |      | Serialized create storage account transaction that's partially signed by the storage account owner |

{% tabs %}
{% tab title="200: OK " %}
```json
{
    "shdw_bucket": String,
    "transaction_signature": String
}
```
{% endtab %}
{% endtabs %}

## storage-account-info

<mark style="color:green;">`POST`</mark> `https://shadow-storage.genesysgo.net`

Gets on-chain and ShdwDrive Network data about a storage account

Request content type: application/json

#### Request Body

| Name                                               | Type   | Description                                                      |
| -------------------------------------------------- | ------ | ---------------------------------------------------------------- |
| storage\_account<mark style="color:red;">\*</mark> | String | Publickey of the storage account you want to get information for |

{% tabs %}
{% tab title="200: OK Response for V1 Storage Account" %}
```json
{
  storage_account: PublicKey,
  reserved_bytes: Number,
  current_usage: Number,
  immutable: Boolean,
  to_be_deleted: Boolean,
  delete_request_epoch: Number,
  owner1: PublicKey,
  owner2: PublicKey,
  accountCoutnerSeed: Number,
  creation_time: Number,
  creation_epoch: Number,
  last_fee_epoch: Number,
  identifier: String
  version: "V1"
}
```
{% endtab %}

{% tab title="200: OK Response for V2 Storage Account" %}
```json
{
  storage_account: PublicKey,
  reserved_bytes: Number,
  current_usage: Number,
  immutable: Boolean,
  to_be_deleted: Boolean,
  delete_request_epoch: Number,
  owner1: PublicKey,
  accountCoutnerSeed: Number,
  creation_time: Number,
  creation_epoch: Number,
  last_fee_epoch: Number,
  identifier: String,
  version: "V2"
}json
```
{% endtab %}
{% endtabs %}

## upload

<mark style="color:green;">`POST`</mark> `https://shadow-storage.genesysgo.net`

Uploads a single file or multiple files at once\
\
Request content type: multipart/form-data\
\
[**Example Implementation**](the-api.md#example-sign-and-upload-a-file)

\
Parameters (FormData fields)

#### Request Body

| Name                                               | Type   | Description                                                                                                             |
| -------------------------------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------- |
| file<mark style="color:red;">\*</mark>             |        | <p>The file you want to upload. You may add up to 5 files each with a field name of</p><p><code>file</code></p><p>.</p> |
| message<mark style="color:red;">\*</mark>          | String | Base58 message signature.                                                                                               |
| signer<mark style="color:red;">\*</mark>           | String | Publickey of the signer of the message signature and owner of the storage account                                       |
| storage\_account<mark style="color:red;">\*</mark> | String | Key of the storage account you want to upload to                                                                        |

{% tabs %}
{% tab title="201: Created " %}
```json
{
    "finalized_locations": [String],
    "message": String
    "upload_errors": [{file: String, storage_account: String, error: String}] or [] if no errors
}
```
{% endtab %}
{% endtabs %}

## edit

<mark style="color:green;">`POST`</mark> `https://shadow-storage.genesysgo.net`

Edits an existing file

Request content type: multipart/form-data

[**Example Implementation**](the-api.md#example-edit-a-file)

Parameters (FormData fields)

#### Request Body

| Name                                               | Type   | Description                                                                                                                                            |
| -------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| file<mark style="color:red;">\*</mark>             | String | <p>The file you want to upload. You may add up to 5 files each with a field name of</p><p><code>file</code></p><p>.</p>                                |
| message<mark style="color:red;">\*</mark>          | String | Base58 message signature.                                                                                                                              |
| signer<mark style="color:red;">\*</mark>           | String | Publickey of the signer of the message signature and owner of the storage account                                                                      |
| storage\_account<mark style="color:red;">\*</mark> | String | Key of the storage account you want to upload to                                                                                                       |
| url<mark style="color:red;">\*</mark>              | String | <p>Url of the original file you want to edit. Example:</p><p><code>https://shdw-drive.genesysgo.net/&#x3C;storage-account>/&#x3C;file-name></code></p> |

{% tabs %}
{% tab title="200: OK " %}
```json
{
    "finalized_location": String,
    "error": String or not provided if no error
}
```
{% endtab %}
{% endtabs %}

## list-objects

<mark style="color:green;">`POST`</mark> `https://shadow-storage.genesysgo.net`

Get a list of all files associated with a storage account

Request content type: application/json

#### Request Body

| Name           | Type   | Description                                                                              |
| -------------- | ------ | ---------------------------------------------------------------------------------------- |
| storageAccount | String | String version of the storage account PublicKey that you want to get a list of files for |

{% tabs %}
{% tab title="200: OK " %}
```json
{
    "keys": [String]
}
```
{% endtab %}
{% endtabs %}

## list-objects-and-sizes

<mark style="color:green;">`POST`</mark> `https://shadow-storage.genesysgo.net`

Get a list of all files and their size associated with a storage account

Request content type: application/json

#### Request Body

| Name                                             | Type   | Description                                                                              |
| ------------------------------------------------ | ------ | ---------------------------------------------------------------------------------------- |
| storageAccount<mark style="color:red;">\*</mark> | String | String version of the storage account PublicKey that you want to get a list of files for |

{% tabs %}
{% tab title="200: OK " %}
```json
{
    "files": [{"file_name": String, size: Number}]
}
```
{% endtab %}
{% endtabs %}

## get-object-data

<mark style="color:green;">`POST`</mark> `https://shadow-storage.genesysgo.net`

Get information about an object

Request content type: application/json

#### Request Body

| Name                                       | Type   | Description                                     |
| ------------------------------------------ | ------ | ----------------------------------------------- |
| location<mark style="color:red;">\*</mark> | String | URL of the file you want to get information for |

{% tabs %}
{% tab title="200: OK " %}
```
JSON object of the file's metadata in the ShdwDrive Network or an error
```
{% endtab %}
{% endtabs %}

## delete-file

<mark style="color:green;">`POST`</mark> `https://shadow-storage.genesysgo.net`

Deletes a file from a given Storage Account

Request content type: application/json

[**Example Implementation**](the-api.md#example-delete-a-file)

#### Request Body

| Name     | Type   | Description                                                                       |
| -------- | ------ | --------------------------------------------------------------------------------- |
| message  | String | Base58 message signature.                                                         |
| signer   | String | Publickey of the signer of the message signature and owner of the storage account |
| location | String | URL of the file you want to delete                                                |

{% tabs %}
{% tab title="200: OK " %}
```json
{
    "message": String,
    "error": String or not passed if no error
}
```
{% endtab %}
{% endtabs %}

## add-storage

<mark style="color:green;">`POST`</mark> `https://shadow-storage.genesysgo.net`

Adds storage

Request content type: application/json

#### Request Body

| Name                                           | Type   | Description                                                                          |
| ---------------------------------------------- | ------ | ------------------------------------------------------------------------------------ |
| transaction <mark style="color:red;">\*</mark> | String | Serialized add storage transaction that is partially signed by the ShdwDrive network |

{% tabs %}
{% tab title="200: OK " %}
```json
{
    message: String,
    transaction_signature: String,
    error: String or not provided if no error
}
```
{% endtab %}
{% endtabs %}

## reduce-storage (updated)

<mark style="color:green;">`POST`</mark> `https://shadow-storage.genesysgo.net`

Reduces storage

Request content type: application/json

#### Request Body

| Name                                           | Type   | Description                                                                             |
| ---------------------------------------------- | ------ | --------------------------------------------------------------------------------------- |
| transaction <mark style="color:red;">\*</mark> | String | Serialized reduce storage transaction that is partially signed by the ShdwDrive network |

{% tabs %}
{% tab title="200: OK " %}
```json
{
    message: String,
    transaction_signature: String,
    error: String or not provided if no error
}
```
{% endtab %}
{% endtabs %}

## make-immutable (updated)

<mark style="color:green;">`POST`</mark> `https://shadow-storage.genesysgo.net`

Makes file immutable

Request content type: application/json

#### Request Body

| Name        | Type   | Description                                                                             |
| ----------- | ------ | --------------------------------------------------------------------------------------- |
| transaction | String | Serialized make immutable transaction that is partially signed by the ShdwDrive network |

{% tabs %}
{% tab title="200: OK " %}
```json
{
    message: String,
    transaction_signature: String,
    error: String or not provided if no error
}
```
{% endtab %}
{% endtabs %}

### **Example - Secure Sign and Upload File** to ShdwDrive using API

This example demonstrates how to securely upload files to the ShdwDrive using the provided API. It includes the process of hashing file names, creating a signed message, and sending the files along with the necessary information to the ShdwDrive endpoint.

```javascript
import bs58 from 'bs58'
import nacl from 'tweetnacl'
import crypto from 'crypto'

// `files` is an array of each file passed in.
const allFileNames = files.map(file => file.fileName)
const hashSum = crypto.createHash("sha256")
// `allFileNames.toString()` creates a comma-separated list of all the file names.
const hashedFileNames = hashSum.update(allFileNames.toString())
const fileNamesHashed = hashSum.digest("hex")
// `storageAccount` is the string representation of a storage account pubkey
let msg = `Shadow Drive Signed Message:\nStorage Account: ${storageAccount}\nUpload files with hash: ${fileNamesHashed}`;
const fd = new FormData();
// `files` is an array of each file passed in
for (let j = 0; j < files.length; j++) {
    fd.append("file", files[j].data, {
        contentType: files[j].contentType as string,
        filename: files[j].fileName,
    });
}
// Expect the final message string to look something like this if you were to output it
// ShdwDrive Signed Message:
// Storage Acount: ABC123
// Upload files with hash: hash1

// If the message is not formatted like above exactly, it will fail message signature verification
// on the ShdwDrive Network side.
const encodedMessage = new TextEncoder().encode(message);
// Uses https://github.com/dchest/tweetnacl-js to sign the message. If it's not signed in the same manor,
// the message will fail signature verification on the ShdwNetwork side.
// This will return a base58 byte array of the signature.
const signedMessage = nacl.sign.detached(encodedMessage, keypair.secretKey);
// Convert the byte array to a bs58-encoded string
const signature = bs58.encode(signedMessage)
fd.append("message", signature);
fd.append("signer", keypair.publicKey.toString());
fd.append("storage_account", storageAccount.toString());
fd.append("fileNames", allFileNames.toString());
const request = await fetch(`${SHDW_DRIVE_ENDPOINT}/upload`, {
    method: "POST",
    body: fd,
});
```

### **Example -** Editing a File in ShdwDrive using API and Message Signature Verification

In this example, we demonstrate how to edit a file in ShdwDrive using the API and message signature verification. The code imports necessary libraries, constructs a message to be signed, encodes and signs the message, and sends an API request to edit the file on ShdwDrive.

```javascript
import bs58 from 'bs58'
import nacl from 'tweetnacl'

// `storageAccount` is the string representation of a storage account pubkey
// `fileName` is the name of the file to be edited
// `sha256Hash` is the sha256 hash of the new file's contents
const message = `ShdwDrive Signed Message:\n StorageAccount: ${storageAccount}\nFile to edit: ${fileName}\nNew file hash: ${sha256Hash}`
// Expect the final message string to look something like this if you were to output it
// ShdwDrive Signed Message:
// Storage Acount: ABC123
// File to delete: https://shadow-drive.genesysgo.net/ABC123/file.png

// If the message is not formatted like above exactly, it will fail message signature verification
// on the ShdwDrive Network side.
const encodedMessage = new TextEncoder().encode(message);
// Uses https://github.com/dchest/tweetnacl-js to sign the message. If it's not signed in the same manor,
// the message will fail signature verification on the Shdw Network side.
// This will return a base58 byte array of the signature.
const signedMessage = nacl.sign.detached(encodedMessage, keypair.secretKey);
// Convert the byte array to a bs58-encoded string
const signature = bs58.encode(signedMessage)


const fd = new FormData();
fd.append("file", fileData, {
    contentType: fileContentType as string,
    filename: fileName,
});
fd.append("signer", keypair.publicKey.toString())
fd.append("message", signature)
fd.append("storage_account", storageAccount)

const uploadResponse = await fetch(`${SHDW_DRIVE_ENDPOINT}/edit`, {
    method: "POST",
    body: fd,
});
```

### **Example -** Deleting a File from ShdwDrive using Signed Message and API

In this example, we demonstrate how to delete a file from the ShdwDrive using a signed message and the ShdwDrive API. The code first constructs a message containing the storage account and the file URL to be deleted. It then encodes and signs the message using the tweetnacl library. The signed message is then converted to a bs58-encoded string. Finally, a POST request is sent to the ShdwDrive API endpoint to delete the file.

```javascript
import bs58 from 'bs58'
import nacl from 'tweetnacl'

// `storageAccount` is the string representation of a storage account pubkey
// `url` is the link to the ShdwDrive file, just like the previous implementation needed the url input
const message = `ShdwDrive Signed Message:\nStorageAccount: ${storageAccount}\nFile to delete: ${url}`
// Expect the final message string to look something like this if you were to output it
// ShdwDrive Signed Message:
// Storage Acount: ABC123
// File to delete: https://shadow-drive.genesysgo.net/ABC123/file.png

// If the message is not formatted like above exactly, it will fail message signature verification
// on the ShdwDrive Network side.
const encodedMessage = new TextEncoder().encode(message);
// Uses https://github.com/dchest/tweetnacl-js to sign the message. If it's not signed in the same manor,
// the message will fail signature verification on the Shdw Network side.
// This will return a base58 byte array of the signature.
const signedMessage = nacl.sign.detached(encodedMessage, keypair.secretKey);
// Convert the byte array to a bs58-encoded string
const signature = bs58.encode(signedMessage)
const deleteRequestBody = {
    signer: keypair.publicKey.toString(),
    message: signature,
    location: options.url
}
const deleteRequest = await fetch(`${SHDW_DRIVE_ENDPOINT}/delete-file`, {
    method: "POST",
    headers: {
        "Content-Type": "application/json"
    },
    body: JSON.stringify(deleteRequestBody)
})
```
