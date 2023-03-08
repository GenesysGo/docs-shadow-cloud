---
layout: landing
---

# add\_storage

{% swagger method="post" path="" baseUrl="" summary="Adds storage capacity to the specified StorageAccount." expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" required="true" name="storage_account_key" %}
The public key of the StorageAccount.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="size" required="true" %}
The additional amount of storage you want to add. E.g if you have an existing StorageAccount with 1MB of storage but you need 2MB total, size should equal 1MB. When specifying size, only KB, MB, and GB storage units are currently supported.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    pub message: String,
    pub transaction_signature: String,
    pub error: Option<String>,
}
```
{% endswagger-response %}
{% endswagger %}

### Example

```javascript
const accts = await drive.getStorageAccounts("v2")
let acctPubKey = new anchor.web3.PublicKey(accts[1].publicKey)
const addStgResp = await drive.addStorage(acctPubKey,"10MB","v2")
```
