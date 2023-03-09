# Javascript Methods

## addStorage


## Definition

`addStorage` is a method of the `ShadowDrive` class defined in `index.ts` at line 121. It takes three parameters: `key`, `size`, and `version` and returns a `Promise<ShadowDriveResponse>` with the confirmed transaction ID.

## Parameters

- `key`: `PublicKey` - Public Key of the existing storage to increase size on
- `size`: `string` - Amount of storage you are requesting to add to your storage account. Should be in a string like '1KB', '1MB', '1GB'. Only KB, MB, and GB storage delineations are supported currently.
- `version`: `ShadowDriveVersion` - ShadowDrive version (v1 or v2)

{% tabs %}
{% tab title="Example" %}
```javascript
const accts = await drive.getStorageAccounts("v2")
let acctPubKey = new anchor.web3.PublicKey(accts[1].publicKey)
const addStgResp = await drive.addStorage(acctPubKey,"10MB","v2")
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
