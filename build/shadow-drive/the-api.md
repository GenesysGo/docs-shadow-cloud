---
description: >-
  Shadow Drive exposes an API that you can interact directly without the need of
  the CLI or SDK. You may build on top of these methods.
---
# **API**

## **Contents**
* **[Example - Sign and upload a file](#example---sign-and-upload-a-file)**
* **[Example - Edit a file](#example---edit-a-file)**
* **[Example - Delete a file](#example---delete-a-file)**


{% swagger method="post" path="" baseUrl="https://shadow-storage.genesysgo.net" summary="storage-account" expanded="true" %}
{% swagger-description %}
Creates a new storage account

\




\


Request content type: application/json
{% endswagger-description %}

{% swagger-parameter in="body" required="true" name="transaction" type="" %}
Serialized create storage account transaction that's partially signed by the storage account owner
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "shdw_bucket": String,
    "transaction_signature": String
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="https://shadow-storage.genesysgo.net" summary="storage-account-info" expanded="true" %}
{% swagger-description %}
Gets on-chain and Shadow Drive Network data about a storage account

\




\


Request content type: application/json
{% endswagger-description %}

{% swagger-parameter in="body" name="storage_account" required="true" %}
Publickey of the storage account you want to get information for
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Response for V1 Storage Account" %}
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
{% endswagger-response %}

{% swagger-response status="200: OK" description="Response for V2 Storage Account" %}
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
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="https://shadow-storage.genesysgo.net" summary="upload" expanded="true" %}
{% swagger-description %}
Uploads a single file or multiple files at once\
\
Request content type: multipart/form-data\
\
[**Example Implementation**](the-api.md#example-sign-and-upload-a-file)****

\
Parameters (FormData fields)
{% endswagger-description %}

{% swagger-parameter in="body" name="file" type="" required="true" %}
The file you want to upload. You may add up to 5 files each with a field name of 

`file`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="message" required="true" %}
Base58 message signature.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="signer" required="true" %}
Publickey of the signer of the message signature and owner of the storage account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="storage_account" required="true" %}
Key of the storage account you want to upload to
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "finalized_locations": [String],
    "message": String
    "upload_errors": [{file: String, storage_account: String, error: String}] or [] if no errors
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="https://shadow-storage.genesysgo.net" summary="edit" expanded="true" %}
{% swagger-description %}
Edits an existing file

\




\


Request content type: multipart/form-data

\




\




[**Example Implementation**\
****](the-api.md#example-edit-a-file)



\


Parameters (FormData fields)
{% endswagger-description %}

{% swagger-parameter in="body" name="file" required="true" %}
The file you want to upload. You may add up to 5 files each with a field name of 

`file`

.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="message" required="true" %}
Base58 message signature.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="signer" required="true" %}
Publickey of the signer of the message signature and owner of the storage account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="storage_account" required="true" %}
Key of the storage account you want to upload to
{% endswagger-parameter %}

{% swagger-parameter in="body" name="url" required="true" %}
Url of the original file you want to edit. Example: `https://shdw-drive.genesysgo.net/<storage-account>/<file-name>`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "finalized_location": String,
    "error": String or not provided if no error
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="https://shadow-storage.genesysgo.net" summary="list-objects" expanded="true" %}
{% swagger-description %}
Get a list of all files associated with a storage account

\




\


Request content type: application/json
{% endswagger-description %}

{% swagger-parameter in="body" name="storageAccount" %}
String version of the storage account PublicKey that you want to get a list of files for
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "keys": [String]
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="https://shadow-storage.genesysgo.net" summary="list-objects-and-sizes" expanded="true" %}
{% swagger-description %}
Get a list of all files and their size associated with a storage account

\




\


Request content type: application/json
{% endswagger-description %}

{% swagger-parameter in="body" name="storageAccount" required="true" %}
String version of the storage account PublicKey that you want to get a list of files for
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "files": [{"file_name": String, size: Number}]
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="https://shadow-storage.genesysgo.net" summary="get-object-data" expanded="true" %}
{% swagger-description %}
Get information about an object

\




\


Request content type: application/json
{% endswagger-description %}

{% swagger-parameter in="body" name="location" required="true" %}
URL of the file you want to get information for
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
JSON object of the file's metadata in the Shadow Drive Network or an error
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="https://shadow-storage.genesysgo.net" summary="delete-file" expanded="true" %}
{% swagger-description %}
Deletes a file from a given Storage Account

\




\


Request content type: application/json

\




\




[**Example Implementation**](the-api.md#example-delete-a-file)

****
{% endswagger-description %}

{% swagger-parameter in="body" name="message" %}
Base58 message signature.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="signer" %}
Publickey of the signer of the message signature and owner of the storage account
{% endswagger-parameter %}

{% swagger-parameter in="body" name="location" %}
URL of the file you want to delete
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "message": String,
    "error": String or not passed if no error
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="https://shadow-storage.genesysgo.net" summary="add-storage" expanded="true" %}
{% swagger-description %}
Adds storage

\




\


Request content type: application/json
{% endswagger-description %}

{% swagger-parameter in="body" name="transaction " required="true" %}
Serialized add storage transaction that is partially signed by the shadow drive network
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    message: String,
    transaction_signature: String,
    error: String or not provided if no error
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="https://shadow-storage.genesysgo.net" summary="reduce-storage" expanded="true" %}
{% swagger-description %}
Reduces storage

\




\


Request content type: application/json
{% endswagger-description %}

{% swagger-parameter in="body" name="transaction " required="true" %}
Serialized reduce storage transaction that is partially signed by the shadow drive network
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    message: String,
    transaction_signature: String,
    error: String or not provided if no error
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="https://shadow-storage.genesysgo.net" summary="make-immutable" expanded="true" %}
{% swagger-description %}
Makes file immutable

\




\


Request content type: application/json
{% endswagger-description %}

{% swagger-parameter in="body" name="transaction" %}
Serialized make immutable transaction that is partially signed by the shadow drive network
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    message: String,
    transaction_signature: String,
    error: String or not provided if no error
}
```
{% endswagger-response %}
{% endswagger %}

### **Example -  Sign and upload a file**

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
// Shadow Drive Signed Message:
// Storage Acount: ABC123
// Upload files with hash: hash1

// If the message is not formatted like above exactly, it will fail message signature verification
// on the Shadow Drive Network side.
const encodedMessage = new TextEncoder().encode(message);
// Uses https://github.com/dchest/tweetnacl-js to sign the message. If it's not signed in the same manor,
// the message will fail signature verification on the Shadow Network side.
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

### **Example - Edit a file**

```javascript
import bs58 from 'bs58'
import nacl from 'tweetnacl'

// `storageAccount` is the string representation of a storage account pubkey
// `fileName` is the name of the file to be edited
// `sha256Hash` is the sha256 hash of the new file's contents
const message = `Shadow Drive Signed Message:\n StorageAccount: ${storageAccount}\nFile to edit: ${fileName}\nNew file hash: ${sha256Hash}`
// Expect the final message string to look something like this if you were to output it
// Shadow Drive Signed Message:
// Storage Acount: ABC123
// File to delete: https://shadow-drive.genesysgo.net/ABC123/file.png

// If the message is not formatted like above exactly, it will fail message signature verification
// on the Shadow Drive Network side.
const encodedMessage = new TextEncoder().encode(message);
// Uses https://github.com/dchest/tweetnacl-js to sign the message. If it's not signed in the same manor,
// the message will fail signature verification on the Shadow Network side.
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

### **Example - Delete a file**

```javascript
import bs58 from 'bs58'
import nacl from 'tweetnacl'

// `storageAccount` is the string representation of a storage account pubkey
// `url` is the link to the shadow drive file, just like the previous implementation needed the url input
const message = `Shadow Drive Signed Message:\nStorageAccount: ${storageAccount}\nFile to delete: ${url}`
// Expect the final message string to look something like this if you were to output it
// Shadow Drive Signed Message:
// Storage Acount: ABC123
// File to delete: https://shadow-drive.genesysgo.net/ABC123/file.png

// If the message is not formatted like above exactly, it will fail message signature verification
// on the Shadow Drive Network side.
const encodedMessage = new TextEncoder().encode(message);
// Uses https://github.com/dchest/tweetnacl-js to sign the message. If it's not signed in the same manor,
// the message will fail signature verification on the Shadow Network side.
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