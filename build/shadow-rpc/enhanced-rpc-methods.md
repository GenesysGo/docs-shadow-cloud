# Enhanced NFT RPC API Methods

The following methods are enhanced NFT RPC API Methods in compliance with the
Metaplex Digital Asset API which allows for querying both Compressed and
Uncompressed NFTs.

[link text](#<heading-name>) 

## Methods

-   [getAssetProof](#<getAssetProof>): Get a merkle proof for a compressed asset by its ID
-   [getAsset](#<getAsset>): get an asset (NFT) by its ID (Pubkey)
-   [getAssetsByOwner](#<getAssetsByOwner>): Get a list of assets (NFTs) owned by an address (Pubkey)
-   [getAssetsByGroup](#<getAssetsByGroup>): Get a list of assets (NFTs) by a group key and value (NFT Collection)
-   [getAssetsByCreator](#<getAssetsByCreator>): Get a list of assets (NFTs) created by an address (Pubkey)
-   [getAssetsByAuthority](#<getAssetsByAuthority>): Get a list of assets (NFTs) with a specific authority (Pubkey)
-   [searchAssets](#<searchAssets>): Search for assets (NFTs) by a variety of parameters

---

## `getAssetProof`

### Request Parameters

| Parameter name | Type   | Description                                        | Required |
| -------------- | ------ | -------------------------------------------------- | -------- |
| id             | String | ID of the asset you want to get a merkle proof for | Yes      |

### Examples

Get Asset Proof for Asset ID `DBRoaZMmTM39wRsadyajfpGgKhnPA4jTRw2yt3QRv57r`

#### Parameters

```json
{
    "jsonrpc": "2.0",
    "method": "getAssetProof",
    "params": {
        "id": "DBRoaZMmTM39wRsadyajfpGgKhnPA4jTRw2yt3QRv57r"
    },
    "id": 0
}
```

#### Result

```json
{}
```

## `getAsset`

### Request Parameters

| Parameter name | Type   | Description                     | Required |
| -------------- | ------ | ------------------------------- | -------- |
| id             | String | ID of the asset you want to get | Yes      |

Get Asset data for Asset ID `DBRoaZMmTM39wRsadyajfpGgKhnPA4jTRw2yt3QRv57r`

### Examples

#### Parameters

```json
{
    "jsonrpc": "2.0",
    "method": "getAsset",
    "params": {
        "id": "DBRoaZMmTM39wRsadyajfpGgKhnPA4jTRw2yt3QRv57r"
    },
    "id": 0
}
```

#### Result

```json
{}
```

## `getAssetsByOwner`

### Request Parameters

| Parameter name | Type   | Description                                                                                         | Required |
| -------------- | ------ | --------------------------------------------------------------------------------------------------- | -------- |
| ownerAddress   | String | Owner address you want to get assets for                                                            | Yes      |
| limit          | Number | Limit number of assets returned. Limit is 1000.                                                     | No       |
| before         | String |                                                                                                     | no       |
| after          | String |                                                                                                     | no       |
| page           | Number | Which page of assets to get based on Limit set. Cannot be combined with `before` or `after` params. | no       |
| storBy         | Object | Sorts results by a given field ascending or descending. See below for all available `sortBy` inputs | no       |

#### sortBy Object

```json
{
    "sortBy": "created"
}
```

Get Assets by Owner Address `DBRoaZMmTM39wRsadyajfpGgKhnPA4jTRw2yt3QRv57r`

### Examples

#### Parameters

```json
{
    "jsonrpc": "2.0",
    "method": "",
    "params": {},
    "id": 0
}
```

#### Result

```json
{}
```

## `getAssetsByGroup`

### Request Parameters

Get Asset data for Asset ID `DBRoaZMmTM39wRsadyajfpGgKhnPA4jTRw2yt3QRv57r`

### Examples

#### Parameters

```json
{
    "jsonrpc": "2.0",
    "method": "",
    "params": {},
    "id": 0
}
```

#### Result

```json
{}
```

## `getAssetsByCreator`

### Requset Parameters

## `getAssetsByAuthority`

### Request Parameters

Get Asset data for Asset ID `DBRoaZMmTM39wRsadyajfpGgKhnPA4jTRw2yt3QRv57r`

### Examples

#### Parameters

```json
{
    "jsonrpc": "2.0",
    "method": "",
    "params": {},
    "id": 0
}
```

#### Result

```json
{}
```

## `searchAssets`

### Request Parameters

Get Asset data for Asset ID `DBRoaZMmTM39wRsadyajfpGgKhnPA4jTRw2yt3QRv57r`

### Examples

#### Parameters

```json
{
    "jsonrpc": "2.0",
    "method": "",
    "params": {},
    "id": 0
}
```

#### Result

```json
{}
```
