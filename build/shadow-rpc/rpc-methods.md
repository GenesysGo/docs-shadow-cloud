# RPC Methods

*   **[JSON RPC API Specification](https://www.jsonrpc.org/specification)**
*   **[Solana Javascript SDK](https://solana-labs.github.io/solana-web3.js/)**
*   **[HTTP Methods](#rpc-http-endpoint)**
    *   [getAccountInfo](#getaccountinfo)
    *   [getBalance](#getbalance)
    *   [getBlockHeight](#getblockheight)
    *   [getBlock](#getblock)
    *   [getBlockProduction](#getblockproduction)
    *   [getBlockCommitment](#getblockcommitment)
    *   [getBlocks](#getblocks)
    *   [getBlocksWithLimit](#getblockswithlimit)
    *   [getBlockTime](#getblocktime)
    *   [getClusterNodes](#getclusternodes)
    *   [getEpochInfo](#getepochinfo)
    *   [getEpochSchedule](#getepochschedule)
    *   [getFeeForMessage](#getfeeformessage)
    *   [getFirstAvailableBlock](#getfirstavailableblock)
    *   [getGenesisHash](#getgenesishash)
    *   [getHealth](#gethealth)
    *   [getHighestSnapshotSlot](#gethighestsnapshotslot)
    *   [getIdentity](#getidentity)
    *   [getInflationGovernor](#getinflationgovernor)
    *   [getInflationRate](#getinflationrate)
    *   [getInflationReward](#getinflationreward)
    *   [getLargestAccounts](#getlargestaccounts)
    *   [getLatestBlockhash](#getlatestblockhash)
    *   [getLeaderSchedule](#getleaderschedule)
    *   [getMaxRetransmitSlot](#getmaxretransmitslot)
    *   [getMaxShredInsertSlot](#getmaxshredinsertslot)
    *   [getMinimumBalanceForRentExemption](#getminimumbalanceforrentexemption)
    *   [getMultipleAccounts](#getmultipleaccounts)
    *   [getProgramAccounts](#getprogramaccounts)
    *   [getRecentPerformanceSamples](#getrecentperformancesamples)
    *   [getRecentPrioritizationFees](#getrecentprioritizationfees)
    *   [getSignaturesForAddress](#getsignaturesforaddress)
    *   [getSignatureStatuses](#getsignaturestatuses)
    *   [getSlot](#getslot)
    *   [getSlotLeader](#getslotleader)
    *   [getSlotLeaders](#getslotleaders)
    *   [getStakeActivation](#getstakeactivation)
    *   [getStakeMinimumDelegation](#getstakeminimumdelegation)
    *   [getSupply](#getsupply)
    *   [getTokenAccountBalance](#gettokenaccountbalance)
    *   [getTokenAccountsByDelegate](#gettokenaccountsbydelegate)
    *   [getTokenAccountsByOwner](#gettokenaccountsbyowner)
    *   [getTokenLargestAccounts](#gettokenlargestaccounts)
    *   [getTokenSupply](#gettokensupply)
    *   [getTransaction](#gettransaction)
    *   [getTransactionCount](#gettransactioncount)
    *   [getVersion](#getversion)
    *   [getVoteAccounts](#getvoteaccounts)
    *   [isBlockhashValid](#isblockhashvalid)
    *   [minimumLedgerSlot](#minimumledgerslot)
    *   [requestAirdrop](#requestairdrop)
    *   [sendTransaction](#sendtransaction)
    *   [simulateTransaction](#simulatetransaction)
*   **[Websocket Methods](#websocket-methods)**  
    *   [accountSubscribe](#accountsubscribe)
    *   [accountUnsubscribe](#accountunsubscribe)
    *   [logsSubscribe](#logssubscribe)
    *   [logsUnsubscribe](#logsunsubscribe)
    *   [programSubscribe](#programsubscribe)
    *   [programUnsubscribe](#programunsubscribe)
    *   [signatureSubscribe](#signaturesubscribe)
    *   [signatureUnsubscribe](#signatureunsubscribe)
    *   [slotSubscribe](#slotsubscribe)
    *   [slotUnsubscribe](#slotunsubscribe)
*   **[Unstable Methods](#blocksubscribe)**
    *   [blockSubscribe](#blocksubscribe)
    *   [blockUnsubscribe](#blockunsubscribe)
    *   [slotsUpdatesSubscribe](#slotsupdatessubscribe)
    *   [slotsUpdatesUnsubscribe](#slotsupdatesunsubscribe)
    *   [voteSubscribe](#votesubscribe)
    *   [voteUnsubscribe](#voteunsubscribe)
*   **[Deprecated Methods](#getconfirmedblock)**
    

JSON RPC HTTP Methods
=====================

Solana nodes accept HTTP requests using the [JSON-RPC 2.0](https://www.jsonrpc.org/specification) specification.

info

For JavaScript applications, use the [@solana/web3.js](https://github.com/solana-labs/solana-web3.js) library as a convenient interface for the RPC methods to interact with a Solana node.

For an PubSub connection to a Solana node, use the [Websocket Methods](#websocket-methods).

RPC HTTP Endpoint[​](#rpc-http-endpoint)
-----------------------------------------------------------------

**Default port:** 8899 e.g. [http://localhost:8899](http://localhost:8899), [http://192.168.1.88:8899](http://192.168.1.88:8899)

Request Formatting[​](#request-formatting "Direct link to heading")
-------------------------------------------------------------------

To make a JSON-RPC request, send an HTTP POST request with a `Content-Type: application/json` header. The JSON request data should contain 4 fields:

*   `jsonrpc: <string>` - set to `"2.0"`
*   `id: <number>` - a unique client-generated identifying integer
*   `method: <string>` - a string containing the method to be invoked
*   `params: <array>` - a JSON array of ordered parameter values

Example using curl:

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getBalance",
    "params": [
      "83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri"
    ]
  }
'
```

The response output will be a JSON object with the following fields:

*   `jsonrpc: <string>` - matching the request specification
*   `id: <number>` - matching the request identifier
*   `**result: <array|number|object|string>` - requ**ested data or success confirmation

Requests can be sent in batches by sending an array of JSON-RPC request objects as the data for a single POST.

Definitions[​](#definitions "Direct link to heading")
-----------------------------------------------------

*   Hash: A SHA-256 hash of a chunk of data.
*   Pubkey: The public key of a Ed25519 key-pair.
*   Transaction: A list of Solana instructions signed by a client keypair to authorize those actions.
*   Signature: An Ed25519 signature of transaction's payload data including instructions. This can be used to identify transactions.

Configuring State Commitment[​](#configuring-state-commitment "Direct link to heading")
---------------------------------------------------------------------------------------

For preflight checks and transaction processing, Solana nodes choose which bank state to query based on a commitment requirement set by the client. The commitment describes how finalized a block is at that point in time. When querying the ledger state, it's recommended to use lower levels of commitment to report progress and higher levels to ensure the state will not be rolled back.

In descending order of commitment (most finalized to least finalized), clients may specify:

*   `"finalized"` - the node will query the most recent block confirmed by supermajority of the cluster as having reached maximum lockout, meaning the cluster has recognized this block as finalized
*   `"confirmed"` - the node will query the most recent block that has been voted on by supermajority of the cluster.
    *   It incorporates votes from gossip and replay.
    *   It does not count votes on descendants of a block, only direct votes on that block.
    *   This confirmation level also upholds "optimistic confirmation" guarantees in release 1.3 and onwards.
*   `"processed"` - the node will query its most recent block. Note that the block may still be skipped by the cluster.

For processing many dependent transactions in series, it's recommended to use `"confirmed"` commitment, which balances speed with rollback safety. For total safety, it's recommended to use`"finalized"` commitment.

#### Example[​](#example "Direct link to heading")

The commitment parameter should be included as the last element in the `params` array:

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getBalance",
    "params": [
      "83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri",
      {
        "commitment": "finalized"
      }
    ]
  }
'
```

#### Default:[​](#default "Direct link to heading")

If commitment configuration is not provided, the node will default to `"finalized"` commitment

Only methods that query bank state accept the commitment parameter. They are indicated in the API Reference below.

#### RpcResponse Structure[​](#rpcresponse-structure "Direct link to heading")

Many methods that take a commitment parameter return an RpcResponse JSON object comprised of two parts:

*   `context` : An RpcResponseContext JSON structure including a `slot` field at which the operation was evaluated.
*   `value` : The value returned by the operation itself.

#### Parsed Responses[​](#parsed-responses "Direct link to heading")

Some methods support an `encoding` parameter, and can return account or instruction data in parsed JSON format if `"encoding":"jsonParsed"` is requested and the node has a parser for the owning program. Solana nodes currently support JSON parsing for the following native and SPL programs:

The list of account parsers can be found [here](https://github.com/solana-labs/solana/blob/master/account-decoder/src/parse_account_data.rs), and instruction parsers [here](https://github.com/solana-labs/solana/blob/master/transaction-status/src/parse_instruction.rs).

Filter criteria[​](#filter-criteria "Direct link to heading")
-------------------------------------------------------------

Some methods support providing a `filters` object to enable pre-filtering the data returned within the RpcResponse JSON object. The following filters exist:

*   `memcmp: object` - compares a provided series of bytes with program account data at a particular offset. Fields:
    
    *   `offset: usize` - offset into program account data to start comparison
    *   `bytes: string` - data to match, as encoded string
    *   `encoding: string` - encoding for filter `bytes` data, either "base58" or "base64". Data is limited in size to 128 or fewer decoded bytes.  
        **NEW: This field, and base64 support generally, is only available in solana-core v1.14.0 or newer. Please omit when querying nodes on earlier versions**
*   `dataSize: u64` - compares the program account data length with the provided data size
    

**JSON RPC API Reference**
---------------------------------------------------------------------------

**`getAccountInfo`**
-----------------------------------------------------------

Returns all information associated with the account of provided Pubkey

### **Parameters**

`string` required

Pubkey of account to query, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optional

Encoding format for Account data

Values: `base58``base64``base64+zstd``jsonParsed`

Details

*   `base58` is slow and limited to less than 129 bytes of Account data.
*   `base64` will return base64 encoded data for Account data of any size.
*   `base64+zstd` compresses the Account data using [Zstandard](https://facebook.github.io/zstd/) and base64-encodes the result.
*   `jsonParsed` encoding attempts to use program-specific state parsers to return more human-readable and explicit account state data.
*   If `jsonParsed` is requested but a parser cannot be found, the field falls back to `base64` encoding, detectable when the `data` field is type `string`.

dataSlice `string` optional

limit the returned account data using the provided "offset: <usize>" and "length: <usize>" fields*   only available for `base58`, `base64` or `base64+zstd` encodings.

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

The result will be an RpcResponse JSON object with `value` equal to:

*   `<null>` - if the requested account doesn't exist
*   `<object>` - otherwise, a JSON object containing:
    *   `lamports: <u64>` - number of lamports assigned to this account, as a u64
    *   `owner: <string>` - base-58 encoded Pubkey of the program this account has been assigned to
    *   `data: <[string, encoding]|object>` - data associated with the account, either as encoded binary data or JSON format `{<program>: <state>}` - depending on encoding parameter
    *   `executable: <bool>` - boolean indicating if the account contains a program (and is strictly read-only)
    *   `rentEpoch: <u64>` - the epoch at which this account will next owe rent, as u64
    *   `size: <u64>` - the data size of the account

### Code sample:[​](#code-sample "Direct link to heading")
```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getAccountInfo",
    "params": [
      "vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg",
      {
        "encoding": "base58"
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1
    },
    "value": {
      "data": [
        "11116bv5nS2h3y12kD1yUKeMZvGcKLSjQgX6BeV7u1FrjeJcKfsHRTPuR3oZ1EioKtYGiYxpxMG5vpbZLsbcBYBEmZZcMKaSoGx9JZeAuWf",
        "base58"
      ],
      "executable": false,
      "lamports": 1000000000,
      "owner": "11111111111111111111111111111111",
      "rentEpoch": 2,
      "space": 80
    }
  },
  "id": 1
}
```

**`getBalance`**
---------------------------------------------------

Returns the balance of the account of provided Pubkey

### **Parameters**

`string` required

Pubkey of account to query, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

`RpcResponse<u64>` - RpcResponse JSON object with `value` field set to the balance

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0", "id": 1,
    "method": "getBalance",
    "params": [
      "83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri"
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": { "context": { "slot": 1 }, "value": 0 },
  "id": 1
}
```

**`getBlock`[​](#getblock "Direct link to heading")**
-----------------------------------------------

Returns identity and transaction information about a confirmed block in the ledger

### **Parameters**

`u64` required

slot number, as `u64` integer

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optionalDefault: `finalized`

*   the default is `finalized`
*   `processed` is not supported.

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optionalDefault: `json`

encoding format for each returned Transaction

Values: `json``jsonParsed``base58``base64`

Details

*   `jsonParsed` attempts to use program-specific instruction parsers to return more human-readable and explicit data in the `transaction.message.instructions` list.
*   If `jsonParsed` is requested but a parser cannot be found, the instruction falls back to regular JSON encoding (`accounts`, `data`, and `programIdIndex` fields).

transactionDetails `string` optionalDefault: `full`

level of transaction detail to return

Values: `full``accounts``signatures``none`

Details

*   If `accounts` are requested, transaction details only include signatures and an annotated list of accounts in each transaction.
*   Transaction metadata is limited to only: fee, err, pre\_balances, post\_balances, pre\_token\_balances, and post\_token\_balances.

maxSupportedTransactionVersion `number` optional

the max transaction version to return in responses.

Details

*   If the requested block contains a transaction with a higher version, an error will be returned.
*   If this parameter is omitted, only legacy transactions will be returned, and a block containing any versioned transaction will prompt the error.

rewards `bool` optional

whether to populate the \`rewards\` array. If parameter not provided, the default includes rewards.

### **Result**

The result field will be an object with the following fields:

*   `<null>` - if specified block is not confirmed
*   `<object>` - if block is confirmed, an object with the following fields:
    *   `blockhash: <string>` - the blockhash of this block, as base-58 encoded string
    *   `previousBlockhash: <string>` - the blockhash of this block's parent, as base-58 encoded string; if the parent block is not available due to ledger cleanup, this field will return "11111111111111111111111111111111"
    *   `parentSlot: <u64>` - the slot index of this block's parent
    *   `transactions: <array>` - present if "full" transaction details are requested; an array of JSON objects containing:
        *   `transaction: <object|[string,encoding]>` - [Transaction](#transaction-structure) object, either in JSON format or encoded binary data, depending on encoding parameter
        *   `meta: <object>` - transaction status metadata object, containing `null` or:
            *   `err: <object|null>` - Error if transaction failed, null if transaction succeeded. [TransactionError definitions](https://github.com/solana-labs/solana/blob/c0c60386544ec9a9ec7119229f37386d9f070523/sdk/src/transaction/error.rs#L13)
            *   `fee: <u64>` - fee this transaction was charged, as u64 integer
            *   `preBalances: <array>` - array of u64 account balances from before the transaction was processed
            *   `postBalances: <array>` - array of u64 account balances after the transaction was processed
            *   `innerInstructions: <array|null>` - List of [inner instructions](#inner-instructions-structure) or `null` if inner instruction recording was not enabled during this transaction
            *   `preTokenBalances: <array|undefined>` - List of [token balances](#token-balances-structure) from before the transaction was processed or omitted if token balance recording was not yet enabled during this transaction
            *   `postTokenBalances: <array|undefined>` - List of [token balances](#token-balances-structure) from after the transaction was processed or omitted if token balance recording was not yet enabled during this transaction
            *   `logMessages: <array|null>` - array of string log messages or `null` if log message recording was not enabled during this transaction
            *   `rewards: <array|null>` - transaction-level rewards, populated if rewards are requested; an array of JSON objects containing:
                *   `pubkey: <string>` - The public key, as base-58 encoded string, of the account that received the reward
                *   `lamports: <i64>`\- number of reward lamports credited or debited by the account, as a i64
                *   `postBalance: <u64>` - account balance in lamports after the reward was applied
                *   `rewardType: <string|undefined>` - type of reward: "fee", "rent", "voting", "staking"
                *   `commission: <u8|undefined>` - vote account commission when the reward was credited, only present for voting and staking rewards
            *   DEPRECATED: `status: <object>` - Transaction status
                *   `"Ok": <null>` - Transaction was successful
                *   `"Err": <ERR>` - Transaction failed with TransactionError
            *   `loadedAddresses: <object|undefined>` - Transaction addresses loaded from address lookup tables. Undefined if `maxSupportedTransactionVersion` is not set in request params.
                *   `writable: <array[string]>` - Ordered list of base-58 encoded addresses for writable loaded accounts
                *   `readonly: <array[string]>` - Ordered list of base-58 encoded addresses for readonly loaded accounts
            *   `returnData: <object|undefined>` - the most-recent return data generated by an instruction in the transaction, with the following fields:
                *   `programId: <string>` - the program that generated the return data, as base-58 encoded Pubkey
                *   `data: <[string, encoding]>` - the return data itself, as base-64 encoded binary data
            *   `computeUnitsConsumed: <u64|undefined>` - number of [compute units](/developing/programming-model/runtime#compute-budget) consumed by the transaction
        *   `version: <"legacy"|number|undefined>` - Transaction version. Undefined if `maxSupportedTransactionVersion` is not set in request params.
    *   `signatures: <array>` - present if "signatures" are requested for transaction details; an array of signatures strings, corresponding to the transaction order in the block
    *   `rewards: <array|undefined>` - block-level rewards, present if rewards are requested; an array of JSON objects containing:
        *   `pubkey: <string>` - The public key, as base-58 encoded string, of the account that received the reward
        *   `lamports: <i64>`\- number of reward lamports credited or debited by the account, as a i64
        *   `postBalance: <u64>` - account balance in lamports after the reward was applied
        *   `rewardType: <string|undefined>` - type of reward: "fee", "rent", "voting", "staking"
        *   `commission: <u8|undefined>` - vote account commission when the reward was credited, only present for voting and staking rewards
    *   `blockTime: <i64|null>` - estimated production time, as Unix timestamp (seconds since the Unix epoch). null if not available
    *   `blockHeight: <u64|null>` - the number of blocks beneath this block


#### Transaction Structure[​](#transaction-structure "Direct link to heading")

Transactions are quite different from those on other blockchains. Be sure to review [Anatomy of a Transaction](https://docs.solana.com/developing/programming-model/transactions) to learn about transactions on Solana.

The JSON structure of a transaction is defined as follows:

*   `signatures: <array[string]>` - A list of base-58 encoded signatures applied to the transaction. The list is always of length `message.header.numRequiredSignatures` and not empty. The signature at index `i` corresponds to the public key at index `i` in `message.accountKeys`. The first one is used as the [transaction id](/terminology#transaction-id).
*   `message: <object>` - Defines the content of the transaction.
    *   `accountKeys: <array[string]>` - List of base-58 encoded public keys used by the transaction, including by the instructions and for signatures. The first `message.header.numRequiredSignatures` public keys must sign the transaction.
    *   `header: <object>` - Details the account types and signatures required by the transaction.
        *   `numRequiredSignatures: <number>` - The total number of signatures required to make the transaction valid. The signatures must match the first `numRequiredSignatures` of `message.accountKeys`.
        *   `numReadonlySignedAccounts: <number>` - The last `numReadonlySignedAccounts` of the signed keys are read-only accounts. Programs may process multiple transactions that load read-only accounts within a single PoH entry, but are not permitted to credit or debit lamports or modify account data. Transactions targeting the same read-write account are evaluated sequentially.
        *   `numReadonlyUnsignedAccounts: <number>` - The last `numReadonlyUnsignedAccounts` of the unsigned keys are read-only accounts.
    *   `recentBlockhash: <string>` - A base-58 encoded hash of a recent block in the ledger used to prevent transaction duplication and to give transactions lifetimes.
    *   `instructions: <array[object]>` - List of program instructions that will be executed in sequence and committed in one atomic transaction if all succeed.
        *   `programIdIndex: <number>` - Index into the `message.accountKeys` array indicating the program account that executes this instruction.
        *   `accounts: <array[number]>` - List of ordered indices into the `message.accountKeys` array indicating which accounts to pass to the program.
        *   `data: <string>` - The program input data encoded in a base-58 string.
    *   `addressTableLookups: <array[object]|undefined>` - List of address table lookups used by a transaction to dynamically load addresses from on-chain address lookup tables. Undefined if `maxSupportedTransactionVersion` is not set.
        *   `accountKey: <string>` - base-58 encoded public key for an address lookup table account.
        *   `writableIndexes: <array[number]>` - List of indices used to load addresses of writable accounts from a lookup table.
        *   `readonlyIndexes: <array[number]>` - List of indices used to load addresses of readonly accounts from a lookup table.

#### Inner Instructions Structure[​](#inner-instructions-structure "Direct link to heading")

The Solana runtime records the cross-program instructions that are invoked during transaction processing and makes these available for greater transparency of what was executed on-chain per transaction instruction. Invoked instructions are grouped by the originating transaction instruction and are listed in order of processing.

The JSON structure of inner instructions is defined as a list of objects in the following structure:

*   `index: number` - Index of the transaction instruction from which the inner instruction(s) originated
*   `instructions: <array[object]>` - Ordered list of inner program instructions that were invoked during a single transaction instruction.
    *   `programIdIndex: <number>` - Index into the `message.accountKeys` array indicating the program account that executes this instruction.
    *   `accounts: <array[number]>` - List of ordered indices into the `message.accountKeys` array indicating which accounts to pass to the program.
    *   `data: <string>` - The program input data encoded in a base-58 string.

#### Token Balances Structure[​](#token-balances-structure "Direct link to heading")

The JSON structure of token balances is defined as a list of objects in the following structure:

*   `accountIndex: <number>` - Index of the account in which the token balance is provided for.
*   `mint: <string>` - Pubkey of the token's mint.
*   `owner: <string|undefined>` - Pubkey of token balance's owner.
*   `programId: <string|undefined>` - Pubkey of the Token program that owns the account.
*   `uiTokenAmount: <object>` -
    *   `amount: <string>` - Raw amount of tokens as a string, ignoring decimals.
    *   `decimals: <number>` - Number of decimals configured for token's mint.
    *   `uiAmount: <number|null>` - Token amount as a float, accounting for decimals. **DEPRECATED**
    *   `uiAmountString: <string>` - Token amount as a string, accounting for decimals.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0","id":1,
    "method":"getBlock",
    "params": [
      430,
      {
        "encoding": "json",
        "maxSupportedTransactionVersion":0,
        "transactionDetails":"full",
        "rewards":false
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "blockHeight": 428,
    "blockTime": null,
    "blockhash": "3Eq21vXNB5s86c62bVuUfTeaMif1N2kUqRPBmGRJhyTA",
    "parentSlot": 429,
    "previousBlockhash": "mfcyqEXB3DnHXki6KjjmZck6YjmZLvpAByy2fj4nh6B",
    "transactions": [
      {
        "meta": {
          "err": null,
          "fee": 5000,
          "innerInstructions": [],
          "logMessages": [],
          "postBalances": [499998932500, 26858640, 1, 1, 1],
          "postTokenBalances": [],
          "preBalances": [499998937500, 26858640, 1, 1, 1],
          "preTokenBalances": [],
          "rewards": null,
          "status": {
            "Ok": null
          }
        },
        "transaction": {
          "message": {
            "accountKeys": [
              "3UVYmECPPMZSCqWKfENfuoTv51fTDTWicX9xmBD2euKe",
              "AjozzgE83A3x1sHNUR64hfH7zaEBWeMaFuAN9kQgujrc",
              "SysvarS1otHashes111111111111111111111111111",
              "SysvarC1ock11111111111111111111111111111111",
              "Vote111111111111111111111111111111111111111"
            ],
            "header": {
              "numReadonlySignedAccounts": 0,
              "numReadonlyUnsignedAccounts": 3,
              "numRequiredSignatures": 1
            },
            "instructions": [
              {
                "accounts": [1, 2, 3, 0],
                "data": "37u9WtQpcm6ULa3WRQHmj49EPs4if7o9f1jSRVZpm2dvihR9C8jY4NqEwXUbLwx15HBSNcP1",
                "programIdIndex": 4
              }
            ],
            "recentBlockhash": "mfcyqEXB3DnHXki6KjjmZck6YjmZLvpAByy2fj4nh6B"
          },
          "signatures": [
            "2nBhEBYYvfaAe16UMNqRHre4YNSskvuYgx3M6E4JP1oDYvZEJHvoPzyUidNgNX5r9sTyN1J9UxtbCXy2rqYcuyuv"
          ]
        }
      }
    ]
  },
  "id": 1
}
```

**`getBlockHeight`[​](#getblockheight "Direct link to heading")**
-----------------------------------------------------------

Returns the current block height of the node

### **Parameters**

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

*   `<u64>` - Current block height

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc":"2.0","id":1,
    "method":"getBlockHeight"
  }
'
```

### Response:[​](#response "Direct **link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": 1233,
  "id": 1
}
```

**`getBlockProduction`[​](#getblockproduction "Direct link to heading")**
-------------------------------------------------------------------

Returns recent block production information from the current or previous epoch.

### **Parameters**

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

identity `string` optional

Only return results for this validator identity (base-58 encoded)

range `object` optional

Slot range to return block production for. If parameter not provided, defaults to current epoch.

*   `firstSlot: <u64>` - first slot to return block production information for (inclusive)
*   (optional) `lastSlot: <u64>` - last slot to return block production information for (inclusive). If parameter not provided, defaults to the highest slot

### **Result**

The result will be an RpcResponse JSON object with `value` equal to:

*   `<object>`
    *   `byIdentity: <object>` - a dictionary of validator identities, as base-58 encoded strings. Value is a two element array containing the number of leader slots and the number of blocks produced.
    *   `range: <object>` - Block production slot range
        *   `firstSlot: <u64>` - first slot of the block production information (inclusive)
        *   `lastSlot: <u64>` - last slot of block production information (inclusive)

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getBlockProduction"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 9887
    },
    "value": {
      "byIdentity": {
        "85iYT5RuzRTDgjyRa3cP8SYhM2j21fj7NhfJ3peu1DPr": [9888, 9886]
      },
      "range": {
        "firstSlot": 0,
        "lastSlot": 9887
      }
    }
  },
  "id": 1
}
```

**`getBlockCommitment`[​](#getblockcommitment "Direct link to heading")**
-------------------------------------------------------------------

Returns commitment for particular block

### **Parameters**

`u64` required

block number, identified by Slot

### **Result**

The result field will be a JSON object containing:

*   `commitment` - commitment, comprising either:
    *   `<null>` - Unknown block
    *   `<array>` - commitment, array of u64 integers logging the amount of cluster stake in lamports that has voted on the block at each depth from 0 to `MAX_LOCKOUT_HISTORY` + 1
*   `totalStake` - total active stake, in lamports, of the current epoch

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0", "id": 1,
    "method": "getBlockCommitment",
    "params":[5]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "commitment": [
      0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
      0, 0, 0, 0, 0, 10, 32
    ],
    "totalStake": 42
  },
  "id": 1
}
```

**`getBlocks`[​](#getblocks "Direct link to heading")**
-------------------------------------------------

Returns a list of confirmed blocks between two slots

### **Parameters**

`u64` required

end\_slot, as `u64` integer

`u64` optional

start\_slot, as `u64` integer (must be no more than 500,000 blocks higher than the \`start\_slot\`)

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optionalDefault: `finalized`

*   "processed" is not supported

### **Result**

The result field will be an array of u64 integers listing confirmed blocks between `start_slot` and either `end_slot` - if provided, or latest confirmed block, inclusive. Max range allowed is 500,000 slots.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0", "id": 1,
    "method": "getBlocks",
    "params": [
      5, 10
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": [5, 6, 7, 8, 9, 10],
  "id": 1
}
```

**`getBlocksWithLimit`[​](#getblockswithlimit "Direct link to heading")**
-------------------------------------------------------------------

Returns a list of confirmed blocks starting at the given slot

### **Parameters**

`u64` required

start\_slot, as `u64` integer

`u64` optional

limit, as `u64` integer (must be no more than 500,000 blocks higher than the `start_slot`)

`object` optional

Configuration object containing the following field:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optionalDefault: `finalized`

*   "processed" is not supported

### **Result**

The result field will be an array of u64 integers listing confirmed blocks starting at `start_slot` for up to `limit` blocks, inclusive.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id":1,
    "method":"getBlocksWithLimit",
    "params":[5, 3]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": [5, 6, 7],
  "id": 1
}
```

**`getBlockTime`[​](#getblocktime "Direct link to heading")**
-------------------------------------------------------

Returns the estimated production time of a block.

info

Each validator reports their UTC time to the ledger on a regular interval by intermittently adding a timestamp to a Vote for a particular block. A requested block's time is calculated from the stake-weighted mean of the Vote timestamps in a set of recent blocks recorded on the ledger.

### **Parameters**

`u64` required

block number, identified by Slot

### **Result**

*   `<i64>` - estimated production time, as Unix timestamp (seconds since the Unix epoch)
*   `<null>` - timestamp is not available for this block

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc":"2.0", "id":1,
    "method": "getBlockTime",
    "params":[5]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": 1574721591,
  "id": 1
}
```

**`getClusterNodes`[​](#getclusternodes "Direct link to heading")**
-------------------------------------------------------------

Returns information about all the nodes participating in the cluster

### **Parameters**

**None**

### **Result**

The result field will be an array of JSON objects, each with the following sub fields:

*   `pubkey: <string>` - Node public key, as base-58 encoded string
*   `gossip: <string|null>` - Gossip network address for the node
*   `tpu: <string|null>` - TPU network address for the node
*   `rpc: <string|null>` - JSON RPC network address for the node, or `null` if the JSON RPC service is not enabled
*   `version: <string|null>` - The software version of the node, or `null` if the version information is not available
*   `featureSet: <u32|null >` - The unique identifier of the node's feature set
*   `shredVersion: <u16|null>` - The shred version the node has been configured to use

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0", "id": 1,
    "method": "getClusterNodes"
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "gossip": "10.239.6.48:8001",
      "pubkey": "9QzsJf7LPLj8GkXbYT3LFDKqsj2hHG7TA3xinJHu8epQ",
      "rpc": "10.239.6.48:8899",
      "tpu": "10.239.6.48:8856",
      "version": "1.0.0 c375ce1f"
    }
  ],
  "id": 1
}
```

**`getEpochInfo`[​](#getepochinfo "Direct link to heading")**
-------------------------------------------------------

Returns information about the current epoch

### **Parameters**

`string` required

Pubkey of account to query, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

The result field will be an object with the following fields:

*   `absoluteSlot: <u64>` - the current slot
*   `blockHeight: <u64>` - the current block height
*   `epoch: <u64>` - the current epoch
*   `slotIndex: <u64>` - the current slot relative to the start of the current epoch
*   `slotsInEpoch: <u64>` - the number of slots in this epoch
*   `transactionCount: <u64|null>` - total number of transactions processed without error since genesis

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getEpochInfo"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "absoluteSlot": 166598,
    "blockHeight": 166500,
    "epoch": 27,
    "slotIndex": 2790,
    "slotsInEpoch": 8192,
    "transactionCount": 22661093
  },
  "id": 1
}
```

**`getEpochSchedule`[​](#getepochschedule "Direct link to heading")**
---------------------------------------------------------------

Returns the epoch schedule information from this cluster's genesis config

### **Parameters**

**None**

### **Result**

The result field will be an object with the following fields:

*   `slotsPerEpoch: <u64>` - the maximum number of slots in each epoch
*   `leaderScheduleSlotOffset: <u64>` - the number of slots before beginning of an epoch to calculate a leader schedule for that epoch
*   `warmup: <bool>` - whether epochs start short and grow
*   `firstNormalEpoch: <u64>` - first normal-length epoch, log2(slotsPerEpoch) - log2(MINIMUM\_SLOTS\_PER\_EPOCH)
*   `firstNormalSlot: <u64>` - MINIMUM\_SLOTS\_PER\_EPOCH \* (2.pow(firstNormalEpoch) - 1)

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc":"2.0","id":1,
    "method":"getEpochSchedule"
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "firstNormalEpoch": 8,
    "firstNormalSlot": 8160,
    "leaderScheduleSlotOffset": 8192,
    "slotsPerEpoch": 8192,
    "warmup": true
  },
  "id": 1
}
```

**`getFeeForMessage`[​](#getfeeformessage "Direct link to heading")**
---------------------------------------------------------------

Get the fee the network will charge for a particular Message

caution

**NEW: This method is only available in solana-core v1.9 or newer. Please use [getFees](#getFees) for solana-core v1.8**

### **Parameters**

`string` required

Base-64 encoded Message

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

*   `<u64|null>` - Fee corresponding to the message at the specified blockhash

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
{
  "id":1,
  "jsonrpc":"2.0",
  "method":"getFeeForMessage",
  "params":[
    "AQABAgIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEBAQAA",
    {
      "commitment":"processed"
    }
  ]
}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": { "context": { "slot": 5068 }, "value": 5000 },
  "id": 1
}
```

**`getFirstAvailableBlock`[​](#getfirstavailableblock "Direct link to heading")**
---------------------------------------------------------------------------

Returns the slot of the lowest confirmed block that has not been purged from the ledger

### **Parameters**

**None**

### **Result**

*   `<u64>` - Slot

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc":"2.0","id":1,
    "method":"getFirstAvailableBlock"
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 250000, "id": 1 }
```

**`getGenesisHash`[​](#getgenesishash "Direct link to heading")**
-----------------------------------------------------------

Returns the genesis hash

### **Parameters**

**None**

### **Result**

*   `<string>` - a Hash as base-58 encoded string

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getGenesisHash"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": "GH7ome3EiwEr7tu9JuTh2dpYWBJK3z69Xm1ZE3MEE6JC",
  "id": 1
}
```

**`getHealth`[​](#gethealth "Direct link to heading")**
-------------------------------------------------

Returns the current health of the node.

caution

If one or more `--known-validator` arguments are provided to `solana-validator` - "ok" is returned when the node has within `HEALTH_CHECK_SLOT_DISTANCE` slots of the highest known validator, otherwise an error is returned. "ok" is always returned if no known validators are provided.

### **Parameters**

**None**

### **Result**

If the node is healthy: "ok"

If the node is unhealthy, a JSON RPC error response is returned. The specifics of the error response are **UNSTABLE** and may change in the future

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getHealth"}
'
```

### Response:[​](#response "Direct link to heading")

Healthy **Result:

```json
{ "jsonrpc": "2.0", "result": "ok", "id": 1 }
```

Unhealthy Result (generic):

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32005,
    "message": "Node is unhealthy",
    "data": {}
  },
  "id": 1
}
```

Unhealthy Result (if additional information is available)

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32005,
    "message": "Node is behind by 42 slots",
    "data": {
      "numSlotsBehind": 42
    }
  },
  "id": 1
}
```

**`getHighestSnapshotSlot`[​](#gethighestsnapshotslot "Direct link to heading")**
---------------------------------------------------------------------------

Returns the highest slot information that the node has snapshots for.

This will find the highest full snapshot slot, and the highest incremental snapshot slot _based on_ the full snapshot slot, if there is one.

caution

NEW: This method is only available in solana-core v1.9 or newer. Please use [getSnapshotSlot](/api/http#getsnapshotslot) for solana-core v1.8

### **Parameters**

**None**

### **Result**

When the node has a snapshot, this returns a JSON object with the following fields:

*   `full: <u64>` - Highest full snapshot slot
*   `incremental: <u64|undefined>` - Highest incremental snapshot slot _based on_ `full`

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1,"method":"getHighestSnapshotSlot"}
'
```

### Response:[​](#response "Direct link to heading")

Result when the node has a snapshot:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "full": 100,
    "incremental": 110
  },
  "id": 1
}
```

Result when the node has no snapshot:

```json
{
  "jsonrpc": "2.0",
  "error": { "code": -32008, "message": "No snapshot" },
  "id": 1
}
```

**`getIdentity`[​](#getidentity "Direct link to heading")**
-----------------------------------------------------

Returns the identity pubkey for the current node

### **Parameters**

**None**

### **Result**

The result field will be a JSON object with the following fields:

*   `identity` - the identity pubkey of the current node (as a base-58 encoded string)

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getIdentity"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "identity": "2r1F4iWqVcb8M1DbAjQuFpebkQHY9hcVU4WuW2DJBppN"
  },
  "id": 1
}
```

**`getInflationGovernor`[​](#getinflationgovernor "Direct link to heading")**
-----------------------------------------------------------------------

Returns the current inflation governor

### **Parameters**

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

The result field will be a JSON object with the following fields:

*   `initial: <f64>` - the initial inflation percentage from time 0
*   `terminal: <f64>` - terminal inflation percentage
*   `taper: <f64>` - rate per year at which inflation is lowered. (Rate reduction is derived using the target slot time in genesis config)
*   `foundation: <f64>` - percentage of total inflation allocated to the foundation
*   `foundationTerm: <f64>` - duration of foundation pool inflation in years

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getInflationGovernor"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "foundation": 0.05,
    "foundationTerm": 7,
    "initial": 0.15,
    "taper": 0.15,
    "terminal": 0.015
  },
  "id": 1
}
```

**`getInflationRate`[​](#getinflationrate "Direct link to heading")**
---------------------------------------------------------------

Returns the specific inflation values for the current epoch

### **Parameters**

**None**

### **Result**

The result field will be a JSON object with the following fields:

*   `total: <f64>` - total inflation
*   `validator: <f64>` -inflation allocated to validators
*   `foundation: <f64>` - inflation allocated to the foundation
*   `epoch: <u64>` - epoch for which these values are valid

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getInflationRate"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "epoch": 100,
    "foundation": 0.001,
    "total": 0.149,
    "validator": 0.148
  },
  "id": 1
}
```

**`getInflationReward`[​](#getinflationreward "Direct link to heading")**
-------------------------------------------------------------------

Returns the inflation / staking reward for a list of addresses for an epoch

### **Parameters**

`array` optional

An array of addresses to query, as base-58 encoded strings

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

epoch `u64` optional

An epoch for which the reward occurs. If omitted, the previous epoch will be used

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

The result field will be a JSON array with the following fields:

*   `epoch: <u64>` - epoch for which reward occured
*   `effectiveSlot: <u64>` - the slot in which the rewards are effective
*   `amount: <u64>` - reward amount in lamports
*   `postBalance: <u64>` - post balance of the account in lamports
*   `commission: <u8|undefined>` - vote account commission when the reward was credited

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getInflationReward",
    "params": [
      [
        "6dmNQ5jwLeLk5REvio1JcMshcbvkYMwy26sJ8pbkvStu",
        "BGsqMegLpV6n6Ve146sSX2dTjUMj3M92HnU8BbNRMhF2"
      ],
      {"epoch": 2}
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "amount": 2500,
      "effectiveSlot": 224,
      "epoch": 2,
      "postBalance": 499999442500
    },
    null
  ],
  "id": 1
}
```

**`getLargestAccounts`[​](#getlargestaccounts "Direct link to heading")**
-------------------------------------------------------------------

Returns the 20 largest accounts, by lamport balance (results may be cached up to two hours)

### **Parameters**

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

filter `string` optional

filter results by account type

Values: `circulating``nonCirculating`

### **Result**

The result will be an RpcResponse JSON object with `value` equal to an array of `<object>` containing:

*   `address: <string>` - base-58 encoded address of the account
*   `lamports: <u64>` - number of lamports in the account, as a u64

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getLargestAccounts"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 54
    },
    "value": [
      {
        "lamports": 999974,
        "address": "99P8ZgtJYe1buSK8JXkvpLh8xPsCFuLYhz9hQFNw93WJ"
      },
      {
        "lamports": 42,
        "address": "uPwWLo16MVehpyWqsLkK3Ka8nLowWvAHbBChqv2FZeL"
      },
      {
        "lamports": 42,
        "address": "aYJCgU7REfu3XF8b3QhkqgqQvLizx8zxuLBHA25PzDS"
      },
      {
        "lamports": 42,
        "address": "CTvHVtQ4gd4gUcw3bdVgZJJqApXE9nCbbbP4VTS5wE1D"
      },
      {
        "lamports": 20,
        "address": "4fq3xJ6kfrh9RkJQsmVd5gNMvJbuSHfErywvEjNQDPxu"
      },
      {
        "lamports": 4,
        "address": "AXJADheGVp9cruP8WYu46oNkRbeASngN5fPCMVGQqNHa"
      },
      {
        "lamports": 2,
        "address": "8NT8yS6LiwNprgW4yM1jPPow7CwRUotddBVkrkWgYp24"
      },
      {
        "lamports": 1,
        "address": "SysvarEpochSchedu1e111111111111111111111111"
      },
      {
        "lamports": 1,
        "address": "11111111111111111111111111111111"
      },
      {
        "lamports": 1,
        "address": "Stake11111111111111111111111111111111111111"
      },
      {
        "lamports": 1,
        "address": "SysvarC1ock11111111111111111111111111111111"
      },
      {
        "lamports": 1,
        "address": "StakeConfig11111111111111111111111111111111"
      },
      {
        "lamports": 1,
        "address": "SysvarRent111111111111111111111111111111111"
      },
      {
        "lamports": 1,
        "address": "Config1111111111111111111111111111111111111"
      },
      {
        "lamports": 1,
        "address": "SysvarStakeHistory1111111111111111111111111"
      },
      {
        "lamports": 1,
        "address": "SysvarRecentB1ockHashes11111111111111111111"
      },
      {
        "lamports": 1,
        "address": "SysvarFees111111111111111111111111111111111"
      },
      {
        "lamports": 1,
        "address": "Vote111111111111111111111111111111111111111"
      }
    ]
  },
  "id": 1
}
```

**`getLatestBlockhash`[​](#getlatestblockhash "Direct link to heading")**
-------------------------------------------------------------------

Returns the latest blockhash

caution

NEW: This method is only available in solana-core v1.9 or newer. Please use [getRecentBlockhash](#getrecentblockhash) for solana-core v1.8

### **Parameters**

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

`RpcResponse<object>` - RpcResponse JSON object with `value` field set to a JSON object including:

*   `blockhash: <string>` - a Hash as base-58 encoded string
*   `lastValidBlockHeight: <u64>` - last [block height](/terminology#block-height) at which the blockhash will be valid

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "id":1,
    "jsonrpc":"2.0",
    "method":"getLatestBlockhash",
    "params":[
      {
        "commitment":"processed"
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 2792
    },
    "value": {
      "blockhash": "EkSnNWid2cvwEVnVx9aBqawnmiCNiDgp3gUdkDPTKN1N",
      "lastValidBlockHeight": 3090
    }
  },
  "id": 1
}
```

**`getLeaderSchedule`[​](#getleaderschedule "Direct link to heading")**
-----------------------------------------------------------------

Returns the leader schedule for an epoch

### **Parameters**

`u64` optional

Fetch the leader schedule for the epoch that corresponds to the provided slot.*   If unspecified, the leader schedule for the current epoch is fetched

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

identity `string` optional

Only return results for this validator identity (base-58 encoded)

### **Result**

Returns a result with one of the two following values:

*   `<null>` - if requested epoch is not found, or
*   `<object>` - the result field will be a dictionary of validator identities, as base-58 encoded strings, and their corresponding leader slot indices as values (indices are relative to the first slot in the requested epoch)

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getLeaderSchedule",
    "params": [
      null,
      {
        "identity": "4Qkev8aNZcqFNSRhQzwyLMFSsi94jHqE8WNVTJzTP99F"
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "4Qkev8aNZcqFNSRhQzwyLMFSsi94jHqE8WNVTJzTP99F": [
      0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20,
      21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38,
      39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56,
      57, 58, 59, 60, 61, 62, 63
    ]
  },
  "id": 1
}
```

**`getMaxRetransmitSlot`[​](#getmaxretransmitslot "Direct link to heading")**
-----------------------------------------------------------------------

Get the max slot seen from retransmit stage.

### **Parameters**

**None**

### **Result**

`<u64>` - Slot number

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getMaxRetransmitSlot"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 1234, "id": 1 }
```

**`getMaxShredInsertSlot`[​](#getmaxshredinsertslot "Direct link to heading")**
-------------------------------------------------------------------------

Get the max slot seen from after shred insert.

### **Parameters**

**None**

### **Result**

`<u64>` - Slot number

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getMaxShredInsertSlot"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 1234, "id": 1 }
```

**`getMinimumBalanceForRentExemption`[​](#getminimumbalanceforrentexemption "Direct link to heading")**
-------------------------------------------------------------------------------------------------

Returns minimum balance required to make account rent exempt.

### **Parameters**

`usize` optional

the Account's data length

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

`<u64>` - minimum lamports required in the Account to remain rent free

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0", "id": 1,
    "method": "getMinimumBalanceForRentExemption",
    "params": [50]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 500, "id": 1 }
```

**`getMultipleAccounts`[​](#getmultipleaccounts "Direct link to heading")**
---------------------------------------------------------------------

Returns the account information for a list of Pubkeys.

### **Parameters**

`array` optional

An array of Pubkeys to query, as base-58 encoded strings (up to a maximum of 100)

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

dataSlice `object` optional

limit the returned account data using the provided `offset: <usize>` and `length: <usize>` fields; only available for "base58", "base64" or "base64+zstd" encodings.

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optionalDefault: `json`

encoding format for the returned Account data

Values: `jsonParsed``base58``base64``base64+zstd`

Details

*   `base58` is slow and limited to less than 129 bytes of Account data.
*   `base64` will return base64 encoded data for Account data of any size.
*   `base64+zstd` compresses the Account data using [Zstandard](https://facebook.github.io/zstd/) and base64-encodes the result.
*   [`jsonParsed` encoding](https://docs.solana.com/api/http#parsed-responses) attempts to use program-specific state parsers to return more human-readable and explicit account state data.
*   If `jsonParsed` is requested but a parser cannot be found, the field falls back to `base64` encoding, detectable when the `data` field is type `<string>`.

### **Result**

The result will be a JSON object with `value` equal to an array of:

*   `<null>` - if the account at that Pubkey doesn't exist, or
*   `<object>` - a JSON object containing:
    *   `lamports: <u64>` - number of lamports assigned to this account, as a u64
    *   `owner: <string>` - base-58 encoded Pubkey of the program this account has been assigned to
    *   `data: <[string, encoding]|object>` - data associated with the account, either as encoded binary data or JSON format `{<program>: <state>}` - depending on encoding parameter
    *   `executable: <bool>` - boolean indicating if the account contains a program (and is strictly read-only)
    *   `rentEpoch: <u64>` - the epoch at which this account will next owe rent, as u64
    *   `size: <u64>` - the data size of the account

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getMultipleAccounts",
    "params": [
      [
        "vines1vzrYbzLMRdu58ou5XTby4qAqVRLmqo36NKPTg",
        "4fYNw3dojWmQ4dXtSGE9epjRGy9pFSx62YypT7avPYvA"
      ],
      {
        "encoding": "base58"
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1
    },
    "value": [
      {
        "data": ["", "base64"],
        "executable": false,
        "lamports": 1000000000,
        "owner": "11111111111111111111111111111111",
        "rentEpoch": 2,
        "space": 16
      },
      {
        "data": ["", "base64"],
        "executable": false,
        "lamports": 5000000000,
        "owner": "11111111111111111111111111111111",
        "rentEpoch": 2,
        "space": 0
      }
    ]
  },
  "id": 1
}
```

**`getProgramAccounts`[​](#getprogramaccounts "Direct link to heading")**
-------------------------------------------------------------------

Returns all accounts owned by the provided program Pubkey

### **Parameters**

`string` required

Pubkey of program, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

withContext `bool` optional

wrap the result in an RpcResponse JSON object

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optionalDefault: `json`

encoding format for the returned Account data

Values: `jsonParsed``base58``base64``base64+zstd`

Details

*   `base58` is slow and limited to less than 129 bytes of Account data.
*   `base64` will return base64 encoded data for Account data of any size.
*   `base64+zstd` compresses the Account data using [Zstandard](https://facebook.github.io/zstd/) and base64-encodes the result.
*   [`jsonParsed` encoding](https://docs.solana.com/api/http#parsed-responses) attempts to use program-specific state parsers to return more human-readable and explicit account state data.
*   If `jsonParsed` is requested but a parser cannot be found, the field falls back to `base64` encoding, detectable when the `data` field is type `<string>`.

dataSlice `object` optional

limit the returned account data using the provided \`offset: usize\` and \`length: usize\` fields;

*   only available for "base58", "base64" or "base64+zstd" encodings.

[filters](/api/http#filter-criteria) `array` optional

filter results using up to 4 filter objects

info

The resultant account(s) must meet **ALL** filter criteria to be included in the returned results

### **Result**

By default, the result field will be an array of JSON objects.

info

If `withContext` flag is set the array will be wrapped in an RpcResponse JSON object.

The resultant response array will contain:

*   `pubkey: <string>` - the account Pubkey as base-58 encoded string
*   `account: <object>` - a JSON object, with the following sub fields:
    *   `lamports: <u64>` - number of lamports assigned to this account, as a u64
    *   `owner: <string>` - base-58 encoded Pubkey of the program this account has been assigned to
    *   `data: <[string,encoding]|object>` - data associated with the account, either as encoded binary data or JSON format `{<program>: <state>}` - depending on encoding parameter
    *   `executable: <bool>` - boolean indicating if the account contains a program (and is strictly read-only)
    *   `rentEpoch: <u64>` - the epoch at which this account will next owe rent, as u64
    *   `size: <u64>` - the data size of the account

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getProgramAccounts",
    "params": [
      "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
      {
        "filters": [
          {
            "dataSize": 17
          },
          {
            "memcmp": {
              "offset": 4,
              "bytes": "3Mc6vR"
            }
          }
        ]
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "account": {
        "data": "2R9jLfiAQ9bgdcw6h8s44439",
        "executable": false,
        "lamports": 15298080,
        "owner": "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
        "rentEpoch": 28,
        "space": 42
      },
      "pubkey": "CxELquR1gPP8wHe33gZ4QxqGB3sZ9RSwsJ2KshVewkFY"
    }
  ],
  "id": 1
}
```

**`getRecentPerformanceSamples`[​](#getrecentperformancesamples "Direct link to heading")**
-------------------------------------------------------------------------------------

Returns a list of recent performance samples, in reverse slot order. Performance samples are taken every 60 seconds and include the number of transactions and slots that occur in a given time window.

### **Parameters**

limit `usize` optional

number of samples to return (maximum 720)

### **Result**

An array of `RpcPerfSample<object>` with the following fields:

*   `slot: <u64>` - Slot in which sample was taken at
*   `numTransactions: <u64>` - Number of transactions in sample
*   `numSlots: <u64>` - Number of slots in sample
*   `samplePeriodSecs: <u16>` - Number of seconds in a sample window
*   `numNonVoteTransaction: <u64>` - Number of non-vote transactions in sample.

info

`numNonVoteTransaction` is present starting with v1.15.

To get a number of voting transactions compute:  
`numTransactions - numNonVoteTransaction`

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc":"2.0", "id":1,
    "method": "getRecentPerformanceSamples",
    "params": [4]}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "numSlots": 126,
      "numTransactions": 126,
      "numNonVoteTransaction": 1,
      "samplePeriodSecs": 60,
      "slot": 348125
    },
    {
      "numSlots": 126,
      "numTransactions": 126,
      "numNonVoteTransaction": 1,
      "samplePeriodSecs": 60,
      "slot": 347999
    },
    {
      "numSlots": 125,
      "numTransactions": 125,
      "numNonVoteTransaction": 0,
      "samplePeriodSecs": 60,
      "slot": 347873
    },
    {
      "numSlots": 125,
      "numTransactions": 125,
      "numNonVoteTransaction": 0,
      "samplePeriodSecs": 60,
      "slot": 347748
    }
  ],
  "id": 1
}
```

**`getRecentPrioritizationFees`[​](#getrecentprioritizationfees "Direct link to heading")**
-------------------------------------------------------------------------------------

Returns a list of prioritization fees from recent blocks.

info

Currently, a node's prioritization-fee cache stores data from up to 150 blocks.

### **Parameters**

`array` optional

An array of Account addresses (up to a maximum of 128 addresses), as base-58 encoded strings

note

If this parameter is provided, the response will reflect a fee to land a transaction locking all of the provided accounts as writable.

### **Result**

An array of `RpcPrioritizationFee<object>` with the following fields:

*   `slot: <u64>` - slot in which the fee was observed
*   `prioritizationFee: <u64>` - the per-compute-unit fee paid by at least one successfully landed transaction, specified in increments of 0.000001 lamports

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc":"2.0", "id":1,
    "method": "getRecentPrioritizationFees",
    "params": [
      ["CxELquR1gPP8wHe33gZ4QxqGB3sZ9RSwsJ2KshVewkFY"]
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "slot": 348125,
      "prioritizationFee": 0
    },
    {
      "slot": 348126,
      "prioritizationFee": 1000
    },
    {
      "slot": 348127,
      "prioritizationFee": 500
    },
    {
      "slot": 348128,
      "prioritizationFee": 0
    },
    {
      "slot": 348129,
      "prioritizationFee": 1234
    }
  ],
  "id": 1
}
```

**`getSignaturesForAddress`[​](#getsignaturesforaddress "Direct link to heading")**
-----------------------------------------------------------------------------

Returns signatures for confirmed transactions that include the given address in their `accountKeys` list. Returns signatures backwards in time from the provided signature or most recent confirmed block

### **Parameters**

`string` required

Account address as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

limit `number` optionalDefault: `1000`

maximum transaction signatures to return (between 1 and 1,000).

before `string` optional

start searching backwards from this transaction signature. If not provided the search starts from the top of the highest max confirmed block.

until `string` optional

search until this transaction signature, if found before limit reached

### **Result**

An array of `<object>`, ordered from **newest** to **oldest** transaction, containing transaction signature information with the following fields:

*   `signature: <string>` - transaction signature as base-58 encoded string
*   `slot: <u64>` - The slot that contains the block with the transaction
*   `err: <object|null>` - Error if transaction failed, null if transaction succeeded. See [TransactionError definitions](https://github.com/solana-labs/solana/blob/c0c60386544ec9a9ec7119229f37386d9f070523/sdk/src/transaction/error.rs#L13) for more info.
*   `memo: <string|null>` - Memo associated with the transaction, null if no memo is present
*   `blockTime: <i64|null>` - estimated production time, as Unix timestamp (seconds since the Unix epoch) of when transaction was processed. null if not available.
*   `confirmationStatus: <string|null>` - The transaction's cluster confirmation status; Either `processed`, `confirmed`, or `finalized`. See [Commitment](https://docs.solana.com/api/http#configuring-state-commitment) for more on optimistic confirmation.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getSignaturesForAddress",
    "params": [
      "Vote111111111111111111111111111111111111111",
      {
        "limit": 1
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "err": null,
      "memo": null,
      "signature": "5h6xBEauJ3PK6SWCZ1PGjBvj8vDdWG3KpwATGy1ARAXFSDwt8GFXM7W5Ncn16wmqokgpiKRLuS83KUxyZyv2sUYv",
      "slot": 114,
      "blockTime": null
    }
  ],
  "id": 1
}
```

**`getSignatureStatuses`[​](#getsignaturestatuses "Direct link to heading")**
-----------------------------------------------------------------------

Returns the statuses of a list of signatures.

info

Unless the `searchTransactionHistory` configuration parameter is included, this method only searches the recent status cache of signatures, which retains statuses for all active slots plus `MAX_RECENT_BLOCKHASHES` rooted slots.

### **Parameters**

`array` optional

An array of transaction signatures to confirm, as base-58 encoded strings (up to a maximum of 256)

`object` optional

Configuration object containing the following fields:

searchTransactionHistory `bool` optional

if `true` - a Solana node will search its ledger cache for any signatures not found in the recent status cache

### **Result**

An array of `RpcResponse<object>` consisting of either:

*   `<null>` - Unknown transaction, or
*   `<object>`
    *   `slot: <u64>` - The slot the transaction was processed
    *   `confirmations: <usize|null>` - Number of blocks since signature confirmation, null if rooted, as well as finalized by a supermajority of the cluster
    *   `err: <object|null>` - Error if transaction failed, null if transaction succeeded. See [TransactionError definitions](https://github.com/solana-labs/solana/blob/c0c60386544ec9a9ec7119229f37386d9f070523/sdk/src/transaction/error.rs#L13)
    *   `confirmationStatus: <string|null>` - The transaction's cluster confirmation status; Either `processed`, `confirmed`, or `finalized`. See [Commitment](https://docs.solana.com/api/http#configuring-state-commitment) for more on optimistic confirmation.
    *   DEPRECATED: `status: <object>` - Transaction status
        *   `"Ok": <null>` - Transaction was successful
        *   `"Err": <ERR>` - Transaction failed with TransactionError

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getSignatureStatuses",
    "params": [
      [
        "5VERv8NMvzbJMEkV8xnrLkEaWRtSz9CosKDYjCJjBRnbJLgp8uirBgmQpjKhoR4tjF3ZpRzrFmBV6UjKdiSZkQUW"
      ],
      {
        "searchTransactionHistory": true
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 82
    },
    "value": [
      {
        "slot": 48,
        "confirmations": null,
        "err": null,
        "status": {
          "Ok": null
        },
        "confirmationStatus": "finalized"
      },
      null
    ]
  },
  "id": 1
}
```

**`getSlot`[​](#getslot "Direct link to heading")**
---------------------------------------------

Returns the slot that has reached the [given or default commitment level](https://docs.solana.com/api/http#configuring-state-commitment)

### **Parameters**

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

`<u64>` - Current slot

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getSlot"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 1234, "id": 1 }
```

**`getSlotLeader`[​](#getslotleader "Direct link to heading")**
---------------------------------------------------------

Returns the current slot leader

### **Parameters**

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

`<string>` - Node identity Pubkey as base-58 encoded string

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getSlotLeader"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": "ENvAW7JScgYq6o4zKZwewtkzzJgDzuJAFxYasvmEQdpS",
  "id": 1
}
```

**`getSlotLeaders`[​](#getslotleaders "Direct link to heading")**
-----------------------------------------------------------

Returns the slot leaders for a given slot range

### **Parameters**

`u64` optional

Start slot, as u64 integer

`u64` optional

Limit, as u64 integer (between 1 and 5,000)

### **Result**

`<array[string]>` - array of Node identity public keys as base-58 encoded strings

### Code sample:[​](#code-sample "Direct link to heading")

If the current slot is `#99` - query the next `10` leaders with the following request:

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc":"2.0", "id": 1,
    "method": "getSlotLeaders",
    "params": [100, 10]
  }
'
```

### Response:[​](#response "Direct link to heading")

The first leader returned is the leader for slot `#100`:

```json
{
  "jsonrpc": "2.0",
  "result": [
    "ChorusmmK7i1AxXeiTtQgQZhQNiXYU84ULeaYF1EH15n",
    "ChorusmmK7i1AxXeiTtQgQZhQNiXYU84ULeaYF1EH15n",
    "ChorusmmK7i1AxXeiTtQgQZhQNiXYU84ULeaYF1EH15n",
    "ChorusmmK7i1AxXeiTtQgQZhQNiXYU84ULeaYF1EH15n",
    "Awes4Tr6TX8JDzEhCZY2QVNimT6iD1zWHzf1vNyGvpLM",
    "Awes4Tr6TX8JDzEhCZY2QVNimT6iD1zWHzf1vNyGvpLM",
    "Awes4Tr6TX8JDzEhCZY2QVNimT6iD1zWHzf1vNyGvpLM",
    "Awes4Tr6TX8JDzEhCZY2QVNimT6iD1zWHzf1vNyGvpLM",
    "DWvDTSh3qfn88UoQTEKRV2JnLt5jtJAVoiCo3ivtMwXP",
    "DWvDTSh3qfn88UoQTEKRV2JnLt5jtJAVoiCo3ivtMwXP"
  ],
  "id": 1
}
```

**`getStakeActivation`[​](#getstakeactivation "Direct link to heading")**
-------------------------------------------------------------------

Returns epoch activation information for a stake account

### **Parameters**

`string` required

Pubkey of stake Account to query, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

epoch `u64` optional

epoch for which to calculate activation details. If parameter not provided, defaults to current epoch.

### **Result**

The result will be a JSON object with the following fields:

*   `state: <string>` - the stake account's activation state, either: `active`, `inactive`, `activating`, or `deactivating`
*   `active: <u64>` - stake active during the epoch
*   `inactive: <u64>` - stake inactive during the epoch

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getStakeActivation",
    "params": [
      "CYRJWqiSjLitBAcRxPvWpgX3s5TvmN2SuRY3eEYypFvT",
      {
        "epoch": 4
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "active": 124429280,
    "inactive": 73287840,
    "state": "activating"
  },
  "id": 1
}
```

**`getStakeMinimumDelegation`[​](#getstakeminimumdelegation "Direct link to heading")**
---------------------------------------------------------------------------------

Returns the stake minimum delegation, in lamports.

### **Parameters**

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

The result will be an RpcResponse JSON object with `value` equal to:

*   `<u64>` - The stake minimum delegation, in lamports

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc":"2.0", "id":1,
    "method": "getStakeMinimumDelegation"
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 501
    },
    "value": 1000000000
  },
  "id": 1
}
```

**`getSupply`[​](#getsupply "Direct link to heading")**
-------------------------------------------------

Returns information about the current supply.

### **Parameters**

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

excludeNonCirculatingAccountsList `bool` optional

exclude non circulating accounts list from response

### **Result**

The result will be an RpcResponse JSON object with `value` equal to a JSON object containing:

*   `total: <u64>` - Total supply in lamports
*   `circulating: <u64>` - Circulating supply in lamports
*   `nonCirculating: <u64>` - Non-circulating supply in lamports
*   `nonCirculatingAccounts: <array>` - an array of account addresses of non-circulating accounts, as strings. If `excludeNonCirculatingAccountsList` is enabled, the returned array will be empty.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0", "id":1, "method":"getSupply"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1114
    },
    "value": {
      "circulating": 16000,
      "nonCirculating": 1000000,
      "nonCirculatingAccounts": [
        "FEy8pTbP5fEoqMV1GdTz83byuA8EKByqYat1PKDgVAq5",
        "9huDUZfxoJ7wGMTffUE7vh1xePqef7gyrLJu9NApncqA",
        "3mi1GmwEE3zo2jmfDuzvjSX9ovRXsDUKHvsntpkhuLJ9",
        "BYxEJTDerkaRWBem3XgnVcdhppktBXa2HbkHPKj2Ui4Z"
      ],
      "total": 1016000
    }
  },
  "id": 1
}
```

**`getTokenAccountBalance`[​](#gettokenaccountbalance "Direct link to heading")**
---------------------------------------------------------------------------

Returns the token balance of an SPL Token account.

### **Parameters**

`string` required

Pubkey of Token account to query, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

The result will be an RpcResponse JSON object with `value` equal to a JSON object containing:

*   `amount: <string>` - the raw balance without decimals, a string representation of u64
*   `decimals: <u8>` - number of base 10 digits to the right of the decimal place
*   `uiAmount: <number|null>` - the balance, using mint-prescribed decimals **DEPRECATED**
*   `uiAmountString: <string>` - the balance as a string, using mint-prescribed decimals

For more details on returned data, the [Token Balances Structure](#token-balances-structure) response from [getBlock](#getblock) follows a similar structure.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0", "id": 1,
    "method": "getTokenAccountBalance",
    "params": [
      "7fUAJdStEuGbc3sM84cKRL6yYaaSstyLSU4ve5oovLS7"
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1114
    },
    "value": {
      "amount": "9864",
      "decimals": 2,
      "uiAmount": 98.64,
      "uiAmountString": "98.64"
    },
    "id": 1
  }
}
```

**`getTokenAccountsByDelegate`[​](#gettokenaccountsbydelegate "Direct link to heading")**
-----------------------------------------------------------------------------------

Returns all SPL Token accounts by approved Delegate.

### **Parameters**

`string` required

Pubkey of account delegate to query, as base-58 encoded string

`object` optional

A JSON object with one of the following fields:

*   `mint: <string>` - Pubkey of the specific token Mint to limit accounts to, as base-58 encoded string; or
*   `programId: <string>` - Pubkey of the Token program that owns the accounts, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

dataSlice `object` optional

limit the returned account data using the provided `offset: <usize>` and `length: <usize>` fields; only available for `base58`, `base64` or `base64+zstd` encodings.

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optional

Encoding format for Account data

Values: `base58``base64``base64+zstd``jsonParsed`

Details

*   `base58` is slow and limited to less than 129 bytes of Account data.
*   `base64` will return base64 encoded data for Account data of any size.
*   `base64+zstd` compresses the Account data using [Zstandard](https://facebook.github.io/zstd/) and base64-encodes the result.
*   `jsonParsed` encoding attempts to use program-specific state parsers to return more human-readable and explicit account state data.
*   If `jsonParsed` is requested but a parser cannot be found, the field falls back to `base64` encoding, detectable when the `data` field is type `string`.

### **Result**

The result will be an RpcResponse JSON object with `value` equal to an array of JSON objects, which will contain:

*   `pubkey: <string>` - the account Pubkey as base-58 encoded string
*   `account: <object>` - a JSON object, with the following sub fields:
    *   `lamports: <u64>` - number of lamports assigned to this account, as a u64
    *   `owner: <string>` - base-58 encoded Pubkey of the program this account has been assigned to
    *   `data: <object>` - Token state data associated with the account, either as encoded binary data or in JSON format `{<program>: <state>}`
    *   `executable: <bool>` - boolean indicating if the account contains a program (and is strictly read-only)
    *   `rentEpoch: <u64>` - the epoch at which this account will next owe rent, as u64
    *   `size: <u64>` - the data size of the account

When the data is requested with the `jsonParsed` encoding a format similar to that of the [Token Balances Structure](#token-balances-structure) can be expected inside the structure, both for the `tokenAmount` and the `delegatedAmount` - with the latter being an optional object.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTokenAccountsByDelegate",
    "params": [
      "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
      {
        "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
      },
      {
        "encoding": "jsonParsed"
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1114
    },
    "value": [
      {
        "account": {
          "data": {
            "program": "spl-token",
            "parsed": {
              "info": {
                "tokenAmount": {
                  "amount": "1",
                  "decimals": 1,
                  "uiAmount": 0.1,
                  "uiAmountString": "0.1"
                },
                "delegate": "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
                "delegatedAmount": {
                  "amount": "1",
                  "decimals": 1,
                  "uiAmount": 0.1,
                  "uiAmountString": "0.1"
                },
                "state": "initialized",
                "isNative": false,
                "mint": "3wyAj7Rt1TWVPZVteFJPLa26JmLvdb1CAKEFZm3NY75E",
                "owner": "CnPoSPKXu7wJqxe59Fs72tkBeALovhsCxYeFwPCQH9TD"
              },
              "type": "account"
            },
            "space": 165
          },
          "executable": false,
          "lamports": 1726080,
          "owner": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
          "rentEpoch": 4,
          "space": 165
        },
        "pubkey": "28YTZEwqtMHWrhWcvv34se7pjS7wctgqzCPB3gReCFKp"
      }
    ]
  },
  "id": 1
}
```

**`getTokenAccountsByOwner`[​](#gettokenaccountsbyowner "Direct link to heading")**
-----------------------------------------------------------------------------

Returns all SPL Token accounts by token owner.

### **Parameters**

`string` required

Pubkey of account delegate to query, as base-58 encoded string

`object` optional

A JSON object with one of the following fields:

*   `mint: <string>` - Pubkey of the specific token Mint to limit accounts to, as base-58 encoded string; or
*   `programId: <string>` - Pubkey of the Token program that owns the accounts, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

dataSlice `object` optional

limit the returned account data using the provided `offset: <usize>` and `length: <usize>` fields; only available for `base58`, `base64`, or `base64+zstd` encodings.

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optional

Encoding format for Account data

Values: `base58``base64``base64+zstd``jsonParsed`

Details

*   `base58` is slow and limited to less than 129 bytes of Account data.
*   `base64` will return base64 encoded data for Account data of any size.
*   `base64+zstd` compresses the Account data using [Zstandard](https://facebook.github.io/zstd/) and base64-encodes the result.
*   `jsonParsed` encoding attempts to use program-specific state parsers to return more human-readable and explicit account state data.
*   If `jsonParsed` is requested but a parser cannot be found, the field falls back to `base64` encoding, detectable when the `data` field is type `string`.

### **Result**

The result will be an RpcResponse JSON object with `value` equal to an array of JSON objects, which will contain:

*   `pubkey: <string>` - the account Pubkey as base-58 encoded string
*   `account: <object>` - a JSON object, with the following sub fields:
    *   `lamports: <u64>` - number of lamports assigned to this account, as a u64
    *   `owner: <string>` - base-58 encoded Pubkey of the program this account has been assigned to
    *   `data: <object>` - Token state data associated with the account, either as encoded binary data or in JSON format `{<program>: <state>}`
    *   `executable: <bool>` - boolean indicating if the account contains a program (and is strictly read-only)
    *   `rentEpoch: <u64>` - the epoch at which this account will next owe rent, as u64
    *   `size: <u64>` - the data size of the account

When the data is requested with the `jsonParsed` encoding a format similar to that of the [Token Balances Structure](/api/http#token-balances-structure) can be expected inside the structure, both for the `tokenAmount` and the `delegatedAmount` - with the latter being an optional object.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTokenAccountsByOwner",
    "params": [
      "4Qkev8aNZcqFNSRhQzwyLMFSsi94jHqE8WNVTJzTP99F",
      {
        "mint": "3wyAj7Rt1TWVPZVteFJPLa26JmLvdb1CAKEFZm3NY75E"
      },
      {
        "encoding": "jsonParsed"
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1114
    },
    "value": [
      {
        "account": {
          "data": {
            "program": "spl-token",
            "parsed": {
              "accountType": "account",
              "info": {
                "tokenAmount": {
                  "amount": "1",
                  "decimals": 1,
                  "uiAmount": 0.1,
                  "uiAmountString": "0.1"
                },
                "delegate": "4Nd1mBQtrMJVYVfKf2PJy9NZUZdTAsp7D4xWLs4gDB4T",
                "delegatedAmount": {
                  "amount": "1",
                  "decimals": 1,
                  "uiAmount": 0.1,
                  "uiAmountString": "0.1"
                },
                "state": "initialized",
                "isNative": false,
                "mint": "3wyAj7Rt1TWVPZVteFJPLa26JmLvdb1CAKEFZm3NY75E",
                "owner": "4Qkev8aNZcqFNSRhQzwyLMFSsi94jHqE8WNVTJzTP99F"
              },
              "type": "account"
            },
            "space": 165
          },
          "executable": false,
          "lamports": 1726080,
          "owner": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA",
          "rentEpoch": 4,
          "space": 165
        },
        "pubkey": "C2gJg6tKpQs41PRS1nC8aw3ZKNZK3HQQZGVrDFDup5nx"
      }
    ]
  },
  "id": 1
}
```

**`getTokenLargestAccounts`[​](#gettokenlargestaccounts "Direct link to heading")**
-----------------------------------------------------------------------------

Returns the 20 largest accounts of a particular SPL Token type.

### **Parameters**

`string` required

Pubkey of the token Mint to query, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

The result will be an RpcResponse JSON object with `value` equal to an array of JSON objects containing:

*   `address: <string>` - the address of the token account
*   `amount: <string>` - the raw token account balance without decimals, a string representation of u64
*   `decimals: <u8>` - number of base 10 digits to the right of the decimal place
*   `uiAmount: <number|null>` - the token account balance, using mint-prescribed decimals **DEPRECATED**
*   `uiAmountString: <string>` - the token account balance as a string, using mint-prescribed decimals

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0", "id": 1,
    "method": "getTokenLargestAccounts",
    "params": [
      "3wyAj7Rt1TWVPZVteFJPLa26JmLvdb1CAKEFZm3NY75E"
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1114
    },
    "value": [
      {
        "address": "FYjHNoFtSQ5uijKrZFyYAxvEr87hsKXkXcxkcmkBAf4r",
        "amount": "771",
        "decimals": 2,
        "uiAmount": 7.71,
        "uiAmountString": "7.71"
      },
      {
        "address": "BnsywxTcaYeNUtzrPxQUvzAWxfzZe3ZLUJ4wMMuLESnu",
        "amount": "229",
        "decimals": 2,
        "uiAmount": 2.29,
        "uiAmountString": "2.29"
      }
    ]
  },
  "id": 1
}
```

**`getTokenSupply`[​](#gettokensupply "Direct link to heading")**
-----------------------------------------------------------

Returns the total supply of an SPL Token type.

### **Parameters**

`string` required

Pubkey of the token Mint to query, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

The result will be an RpcResponse JSON object with `value` equal to a JSON object containing:

*   `amount: <string>` - the raw total token supply without decimals, a string representation of u64
*   `decimals: <u8>` - number of base 10 digits to the right of the decimal place
*   `uiAmount: <number|null>` - the total token supply, using mint-prescribed decimals **DEPRECATED**
*   `uiAmountString: <string>` - the total token supply as a string, using mint-prescribed decimals

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0", "id": 1,
    "method": "getTokenSupply",
    "params": [
      "3wyAj7Rt1TWVPZVteFJPLa26JmLvdb1CAKEFZm3NY75E"
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1114
    },
    "value": {
      "amount": "100000",
      "decimals": 2,
      "uiAmount": 1000,
      "uiAmountString": "1000"
    }
  },
  "id": 1
}
```

**`getTransaction`[​](#gettransaction "Direct link to heading")**
-----------------------------------------------------------

Returns transaction details for a confirmed transaction

### **Parameters**

`string` required

Transaction signature, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

maxSupportedTransactionVersion `number` optional

Set the max transaction version to return in responses. If the requested transaction is a higher version, an error will be returned. If this parameter is omitted, only legacy transactions will be returned, and any versioned transaction will prompt the error.

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optionalDefault: `json`

Encoding for the returned Transaction

Values: `json``jsonParsed``base64``base58`

Details

*   `jsonParsed` encoding attempts to use program-specific state parsers to return more human-readable and explicit data in the `transaction.message.instructions` list.
*   If `jsonParsed` is requested but a parser cannot be found, the instruction falls back to regular JSON encoding (`accounts`, `data`, and `programIdIndex` fields).

### **Result**

*   `<null>` - if transaction is not found or not confirmed
*   `<object>` - if transaction is confirmed, an object with the following fields:
    *   `slot: <u64>` - the slot this transaction was processed in
    *   `transaction: <object|[string,encoding]>` - [Transaction](#transaction-structure) object, either in JSON format or encoded binary data, depending on encoding parameter
    *   `blockTime: <i64|null>` - estimated production time, as Unix timestamp (seconds since the Unix epoch) of when the transaction was processed. null if not available
    *   `meta: <object|null>` - transaction status metadata object:
        *   `err: <object|null>` - Error if transaction failed, null if transaction succeeded. [TransactionError definitions](https://docs.rs/solana-sdk/VERSION_FOR_DOCS_RS/solana_sdk/transaction/enum.TransactionError.html)
        *   `fee: <u64>` - fee this transaction was charged, as u64 integer
        *   `preBalances: <array>` - array of u64 account balances from before the transaction was processed
        *   `postBalances: <array>` - array of u64 account balances after the transaction was processed
        *   `innerInstructions: <array|null>` - List of [inner instructions](#inner-instructions-structure) or `null` if inner instruction recording was not enabled during this transaction
        *   `preTokenBalances: <array|undefined>` - List of [token balances](#token-balances-structure) from before the transaction was processed or omitted if token balance recording was not yet enabled during this transaction
        *   `postTokenBalances: <array|undefined>` - List of [token balances](#token-balances-structure) from after the transaction was processed or omitted if token balance recording was not yet enabled during this transaction
        *   `logMessages: <array|null>` - array of string log messages or `null` if log message recording was not enabled during this transaction
        *   DEPRECATED: `status: <object>` - Transaction status
            *   `"Ok": <null>` - Transaction was successful
            *   `"Err": <ERR>` - Transaction failed with TransactionError
        *   `rewards: <array|null>` - transaction-level rewards, populated if rewards are requested; an array of JSON objects containing:
            *   `pubkey: <string>` - The public key, as base-58 encoded string, of the account that received the reward
            *   `lamports: <i64>`\- number of reward lamports credited or debited by the account, as a i64
            *   `postBalance: <u64>` - account balance in lamports after the reward was applied
            *   `rewardType: <string>` - type of reward: currently only "rent", other types may be added in the future
            *   `commission: <u8|undefined>` - vote account commission when the reward was credited, only present for voting and staking rewards
        *   `loadedAddresses: <object|undefined>` - Transaction addresses loaded from address lookup tables. Undefined if `maxSupportedTransactionVersion` is not set in request params.
            *   `writable: <array[string]>` - Ordered list of base-58 encoded addresses for writable loaded accounts
            *   `readonly: <array[string]>` - Ordered list of base-58 encoded addresses for readonly loaded accounts
        *   `returnData: <object|undefined>` - the most-recent return data generated by an instruction in the transaction, with the following fields:
            *   `programId: <string>` - the program that generated the return data, as base-58 encoded Pubkey
            *   `data: <[string, encoding]>` - the return data itself, as base-64 encoded binary data
        *   `computeUnitsConsumed: <u64|undefined>` - number of [compute units](/developing/programming-model/runtime#compute-budget) consumed by the transaction
    *   `version: <"legacy"|number|undefined>` - Transaction version. Undefined if `maxSupportedTransactionVersion` is not set in request params.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getTransaction",
    "params": [
      "2nBhEBYYvfaAe16UMNqRHre4YNSskvuYgx3M6E4JP1oDYvZEJHvoPzyUidNgNX5r9sTyN1J9UxtbCXy2rqYcuyuv",
      "json"
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "meta": {
      "err": null,
      "fee": 5000,
      "innerInstructions": [],
      "postBalances": [499998932500, 26858640, 1, 1, 1],
      "postTokenBalances": [],
      "preBalances": [499998937500, 26858640, 1, 1, 1],
      "preTokenBalances": [],
      "rewards": [],
      "status": {
        "Ok": null
      }
    },
    "slot": 430,
    "transaction": {
      "message": {
        "accountKeys": [
          "3UVYmECPPMZSCqWKfENfuoTv51fTDTWicX9xmBD2euKe",
          "AjozzgE83A3x1sHNUR64hfH7zaEBWeMaFuAN9kQgujrc",
          "SysvarS1otHashes111111111111111111111111111",
          "SysvarC1ock11111111111111111111111111111111",
          "Vote111111111111111111111111111111111111111"
        ],
        "header": {
          "numReadonlySignedAccounts": 0,
          "numReadonlyUnsignedAccounts": 3,
          "numRequiredSignatures": 1
        },
        "instructions": [
          {
            "accounts": [1, 2, 3, 0],
            "data": "37u9WtQpcm6ULa3WRQHmj49EPs4if7o9f1jSRVZpm2dvihR9C8jY4NqEwXUbLwx15HBSNcP1",
            "programIdIndex": 4
          }
        ],
        "recentBlockhash": "mfcyqEXB3DnHXki6KjjmZck6YjmZLvpAByy2fj4nh6B"
      },
      "signatures": [
        "2nBhEBYYvfaAe16UMNqRHre4YNSskvuYgx3M6E4JP1oDYvZEJHvoPzyUidNgNX5r9sTyN1J9UxtbCXy2rqYcuyuv"
      ]
    }
  },
  "blockTime": null,
  "id": 1
}
```

**`getTransactionCount`[​](#gettransactioncount "Direct link to heading")**
---------------------------------------------------------------------

Returns the current Transaction count from the ledger

### **Parameters**

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

`<u64>` - the current Transaction count from the ledger

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getTransactionCount"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 268, "id": 1 }
```

**`getVersion`[​](#getversion "Direct link to heading")**
---------------------------------------------------

Returns the current Solana version running on the node

### **Parameters**

**None**

### **Result**

The result field will be a JSON object with the following fields:

*   `solana-core` - software version of solana-core
*   `feature-set` - unique identifier of the current software's feature set

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getVersion"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": { "solana-core": "1.15.0" }, "id": 1 }
```

**`getVoteAccounts`[​](#getvoteaccounts "Direct link to heading")**
-------------------------------------------------------------

Returns the account info and associated stake for all the voting accounts in the current bank.

### **Parameters**

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

votePubkey `string` optional

Only return results for this validator vote address (base-58 encoded)

keepUnstakedDelinquents `bool` optional

Do not filter out delinquent validators with no stake

delinquentSlotDistance `u64` optional

Specify the number of slots behind the tip that a validator must fall to be considered delinquent. \*\*NOTE:\*\* For the sake of consistency between ecosystem products, \_it is \*\*not\*\* recommended that this argument be specified.\_

### **Result**

The result field will be a JSON object of `current` and `delinquent` accounts, each containing an array of JSON objects with the following sub fields:

*   `votePubkey: <string>` - Vote account address, as base-58 encoded string
*   `nodePubkey: <string>` - Validator identity, as base-58 encoded string
*   `activatedStake: <u64>` - the stake, in lamports, delegated to this vote account and active in this epoch
*   `epochVoteAccount: <bool>` - bool, whether the vote account is staked for this epoch
*   `commission: <number>` - percentage (0-100) of rewards payout owed to the vote account
*   `lastVote: <u64>` - Most recent slot voted on by this vote account
*   `epochCredits: <array>` - Latest history of earned credits for up to five epochs, as an array of arrays containing: `[epoch, credits, previousCredits]`.
*   `rootSlot: <u64>` - Current root slot for this vote account

### Code sample:[​](#code-sample "Direct link to heading")

Restrict results to a single validator vote account:

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getVoteAccounts",
    "params": [
      {
        "votePubkey": "3ZT31jkAGhUaw8jsy4bTknwBMP8i4Eueh52By4zXcsVw"
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "current": [
      {
        "commission": 0,
        "epochVoteAccount": true,
        "epochCredits": [
          [1, 64, 0],
          [2, 192, 64]
        ],
        "nodePubkey": "B97CCUW3AEZFGy6uUg6zUdnNYvnVq5VG8PUtb2HayTDD",
        "lastVote": 147,
        "activatedStake": 42,
        "votePubkey": "3ZT31jkAGhUaw8jsy4bTknwBMP8i4Eueh52By4zXcsVw"
      }
    ],
    "delinquent": []
  },
  "id": 1
}
```

**`isBlockhashValid`[​](#isblockhashvalid "Direct link to heading")**
---------------------------------------------------------------

Returns whether a blockhash is still valid or not

caution

NEW: This method is only available in solana-core v1.9 or newer. Please use [getFeeCalculatorForBlockhash](#getfeecalculatorforblockhash) for solana-core v1.8

### **Parameters**

`string` required

the blockhash of the block to evauluate, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

`<bool>` - `true` if the blockhash is still valid

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "id":45,
    "jsonrpc":"2.0",
    "method":"isBlockhashValid",
    "params":[
      "J7rBdM6AecPDEZp8aPq5iPSNKVkU5Q76F3oAV4eW5wsW",
      {"commitment":"processed"}
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 2483
    },
    "value": false
  },
  "id": 1
}
```

**`minimumLedgerSlot`[​](#minimumledgerslot "Direct link to heading")**
-----------------------------------------------------------------

Returns the lowest slot that the node has information about in its ledger.

info

This value may increase over time if the node is configured to purge older ledger data

### **Parameters**

**None**

### **Result**

`u64` - Minimum ledger slot number

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"minimumLedgerSlot"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 1234, "id": 1 }
```

**`requestAirdrop`[​](#requestairdrop "Direct link to heading")**
-----------------------------------------------------------

Requests an airdrop of lamports to a Pubkey

### **Parameters**

`string` required

Pubkey of account to receive lamports, as a base-58 encoded string

`integer` required

lamports to airdrop, as a "u64"

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

`<string>` - Transaction Signature of the airdrop, as a base-58 encoded string

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0", "id": 1,
    "method": "requestAirdrop",
    "params": [
      "83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri",
      1000000000
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": "5VERv8NMvzbJMEkV8xnrLkEaWRtSz9CosKDYjCJjBRnbJLgp8uirBgmQpjKhoR4tjF3ZpRzrFmBV6UjKdiSZkQUW",
  "id": 1
}
```

**`sendTransaction`[​](#sendtransaction "Direct link to heading")**
-------------------------------------------------------------

Submits a signed transaction to the cluster for processing.

This method does not alter the transaction in any way; it relays the transaction created by clients to the node as-is.

If the node's rpc service receives the transaction, this method immediately succeeds, without waiting for any confirmations. A successful response from this method does not guarantee the transaction is processed or confirmed by the cluster.

While the rpc service will reasonably retry to submit it, the transaction could be rejected if transaction's `recent_blockhash` expires before it lands.

Use [`getSignatureStatuses`](#getsignaturestatuses) to ensure a transaction is processed and confirmed.

Before submitting, the following preflight checks are performed:

1.  The transaction signatures are verified
2.  The transaction is simulated against the bank slot specified by the preflight commitment. On failure an error will be returned. Preflight checks may be disabled if desired. It is recommended to specify the same commitment and preflight commitment to avoid confusing behavior.

The returned signature is the first signature in the transaction, which is used to identify the transaction ([transaction id](/terminology#transaction-id)). This identifier can be easily extracted from the transaction data before submission.

### **Parameters**

`string` required

Fully-signed Transaction, as encoded string.

`object` optional

Configuration object containing the following optional fields:

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` Default: `base58`

Encoding used for the transaction data.

Values: `base58` (_slow_, **DEPRECATED**), or `base64`.

skipPreflight `bool` Default: `false`

if "true", skip the preflight transaction checks

[preflightCommitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` Default: `finalized`

Commitment level to use for preflight.

maxRetries `usize`

Maximum number of times for the RPC node to retry sending the transaction to the leader. If this parameter not provided, the RPC node will retry the transaction until it is finalized or until the blockhash expires.

minContextSlot `number`

set the minimum slot at which to perform preflight transaction checks

### **Result**

`<string>` - First Transaction Signature embedded in the transaction, as base-58 encoded string ([transaction id](/terminology#transaction-id))

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "sendTransaction",
    "params": [
      "4hXTCkRzt9WyecNzV1XPgCDfGAZzQKNxLXgynz5QDuWWPSAZBZSHptvWRL3BjCvzUXRdKvHL2b7yGrRQcWyaqsaBCncVG7BFggS8w9snUts67BSh3EqKpXLUm5UMHfD7ZBe9GhARjbNQMLJ1QD3Spr6oMTBU6EhdB4RD8CP2xUxr2u3d6fos36PD98XS6oX8TQjLpsMwncs5DAMiD4nNnR8NBfyghGCWvCVifVwvA8B8TJxE1aiyiv2L429BCWfyzAme5sZW8rDb14NeCQHhZbtNqfXhcp2tAnaAT"
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": "2id3YC2jK9G5Wo2phDx4gJVAew8DcY5NAojnVuao8rkxwPYPe8cSwE5GzhEgJA2y8fVjDEo6iR6ykBvDxrTQrtpb",
  "id": 1
}
```

**`simulateTransaction`[​](#simulatetransaction "Direct link to heading")**
---------------------------------------------------------------------

Simulate sending a transaction

### **Parameters**

`string` required

Transaction, as an encoded string.

note

The transaction must have a valid blockhash, but is not required to be signed.

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optionalDefault: `finalized`

Commitment level to simulate the transaction at

sigVerify `bool` optional

if \`true\` the transaction signatures will be verified (conflicts with \`replaceRecentBlockhash\`)

replaceRecentBlockhash `bool` optional

if \`true\` the transaction recent blockhash will be replaced with the most recent blockhash. (conflicts with \`sigVerify\`)

minContextSlot `number` optional

the minimum slot that the request can be evaluated at

encoding `string` optionalDefault: `base58`

Encoding used for the transaction data.

Values: `base58` (_slow_, **DEPRECATED**), or `base64`.

accounts `object` optional

Accounts configuration object containing the following fields:

addresses `array`

An \`array\` of accounts to return, as base-58 encoded strings

encoding `string` Default: `base64`

encoding for returned Account data

Values: `base64``base58``base64+zstd``jsonParsed`

Details

*   `jsonParsed` encoding attempts to use program-specific state parsers to return more human-readable and explicit account state data.
*   If `jsonParsed` is requested but a parser cannot be found, the field falls back to binary encoding, detectable when the `data` field is type `string`.

### **Result**

The result will be an RpcResponse JSON object with `value` set to a JSON object with the following fields:

*   `err: <object|string|null>` - Error if transaction failed, null if transaction succeeded. [TransactionError definitions](https://github.com/solana-labs/solana/blob/c0c60386544ec9a9ec7119229f37386d9f070523/sdk/src/transaction/error.rs#L13)
*   `logs: <array|null>` - Array of log messages the transaction instructions output during execution, null if simulation failed before the transaction was able to execute (for example due to an invalid blockhash or signature verification failure)
*   `accounts: <array|null>` - array of accounts with the same length as the `accounts.addresses` array in the request
    *   `<null>` - if the account doesn't exist or if `err` is not null
    *   `<object>` - otherwise, a JSON object containing:
        *   `lamports: <u64>` - number of lamports assigned to this account, as a u64
        *   `owner: <string>` - base-58 encoded Pubkey of the program this account has been assigned to
        *   `data: <[string, encoding]|object>` - data associated with the account, either as encoded binary data or JSON format `{<program>: <state>}` - depending on encoding parameter
        *   `executable: <bool>` - boolean indicating if the account contains a program (and is strictly read-only)
        *   `rentEpoch: <u64>` - the epoch at which this account will next owe rent, as u64
*   `unitsConsumed: <u64|undefined>` - The number of compute budget units consumed during the processing of this transaction
*   `returnData: <object|null>` - the most-recent return data generated by an instruction in the transaction, with the following fields:
    *   `programId: <string>` - the program that generated the return data, as base-58 encoded Pubkey
    *   `data: <[string, encoding]>` - the return data itself, as base-64 encoded binary data

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "simulateTransaction",
    "params": [
      "AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAAEDArczbMia1tLmq7zz4DinMNN0pJ1JtLdqIJPUw3YrGCzYAMHBsgN27lcgB6H2WQvFgyZuJYHa46puOQo9yQ8CVQbd9uHXZaGT2cvhRs7reawctIXtX1s3kTqM9YV+/wCp20C7Wj2aiuk5TReAXo+VTVg8QTHjs0UjNMMKCvpzZ+ABAgEBARU=",
      {
        "encoding":"base64",
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 218
    },
    "value": {
      "err": null,
      "accounts": null,
      "logs": [
        "Program 83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri invoke [1]",
        "Program 83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri consumed 2366 of 1400000 compute units",
        "Program return: 83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri KgAAAAAAAAA=",
        "Program 83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri success"
      ],
      "returnData": {
        "data": ["Kg==", "base64"],
        "programId": "83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri"
      },
      "unitsConsumed": 2366
    }
  },
  "id": 1
}
```

**RPC Websocket API[](#websocket-methods)**
=================

After connecting to the RPC PubSub websocket at `ws://<ADDRESS>/`:

*   Submit subscription requests to the websocket using the methods below
*   Multiple subscriptions may be active at once
*   Many subscriptions take the optional [`commitment` parameter](https://docs.solana.com/api/http#configuring-state-commitment), defining how finalized a change should be to trigger a notification. For subscriptions, if commitment is unspecified, the default value is `finalized`.

RPC PubSub WebSocket Endpoint[​](#rpc-pubsub-websocket-endpoint "Direct link to heading")
-----------------------------------------------------------------------------------------

**Default port:** 8900 e.g. ws://localhost:8900, [http://192.168.1.88:8900](http://192.168.1.88:8900)

Methods[​](#methods "Direct link to heading")
---------------------------------------------

The following methods are supported in the RPC Websocket API:

**`accountSubscribe`[​](#accountsubscribe "Direct link to heading")**
---------------------------------------------------------------

Subscribe to an account to receive notifications when the lamports or data for a given account public key changes

### **Parameters**

`string` required

Account Pubkey, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optional

Encoding format for Account data

Values: `base58``base64``base64+zstd``jsonParsed`

Details

*   `base58` is slow.
*   `jsonParsed` encoding attempts to use program-specific state parsers to return more human-readable and explicit account state data
*   If `jsonParsed` is requested but a parser cannot be found, the field falls back to binary encoding, detectable when the `data`field is type`string`.

### **Result**

`<number>` - Subscription id (needed to unsubscribe)

### Code sample:[​](#code-sample "Direct link to heading")

```bash
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "accountSubscribe",
  "params": [
    "CM78CPUeXjn8o3yroDHxUtKsZZgoy4GPkPPXfouKNH12",
    {
      "encoding": "jsonParsed",
      "commitment": "finalized"
    }
  ]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 23784, "id": 1 }
```

#### Notification Format:[​](#notification-format "Direct link to heading")

The notification format is the same as seen in the [getAccountInfo](#getAccountInfo) RPC HTTP method.

Base58 encoding:

```json
{
  "jsonrpc": "2.0",
  "method": "accountNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5199307
      },
      "value": {
        "data": [
          "11116bv5nS2h3y12kD1yUKeMZvGcKLSjQgX6BeV7u1FrjeJcKfsHPXHRDEHrBesJhZyqnnq9qJeUuF7WHxiuLuL5twc38w2TXNLxnDbjmuR",
          "base58"
        ],
        "executable": false,
        "lamports": 33594,
        "owner": "11111111111111111111111111111111",
        "rentEpoch": 635,
        "space": 80
      }
    },
    "subscription": 23784
  }
}
```

Parsed-JSON encoding:

```json
{
  "jsonrpc": "2.0",
  "method": "accountNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5199307
      },
      "value": {
        "data": {
          "program": "nonce",
          "parsed": {
            "type": "initialized",
            "info": {
              "authority": "Bbqg1M4YVVfbhEzwA9SpC9FhsaG83YMTYoR4a8oTDLX",
              "blockhash": "LUaQTmM7WbMRiATdMMHaRGakPtCkc2GHtH57STKXs6k",
              "feeCalculator": {
                "lamportsPerSignature": 5000
              }
            }
          }
        },
        "executable": false,
        "lamports": 33594,
        "owner": "11111111111111111111111111111111",
        "rentEpoch": 635,
        "space": 80
      }
    },
    "subscription": 23784
  }
}
```

**`accountUnsubscribe`[​](#accountunsubscribe "Direct link to heading")**
-------------------------------------------------------------------

Unsubscribe from account change notifications

### **Parameters**

`number` required

id of the account Subscription to cancel

### **Result**

`<bool>` - unsubscribe success message

### Code sample:[​](#code-sample "Direct link to heading")

```bash
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "accountUnsubscribe",
  "params": [0]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": true, "id": 1 }
```

**`blockSubscribe`[​](#blocksubscribe "Direct link to heading")**
-----------------------------------------------------------

Subscribe to receive notification anytime a new block is Confirmed or Finalized.

caution

This subscription is **unstable** and only available if the validator was started with the `--rpc-pubsub-enable-block-subscription` flag.

**NOTE: The format of this subscription may change in the future**

### **Parameters**

filter `string | object` optional

filter criteria for the logs to receive results by account type; currently supported:

`string`

`all` - include all transactions in block

`object`

A JSON object with the following field:

*   `mentionsAccountOrProgram: <string>` - return only transactions that mention the provided public key (as base-58 encoded string). If no mentions in a given block, then no notification will be sent.

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

transactionDetails `string` optional

level of transaction detail to return, either "full", "signatures", or "none".

showRewards `bool` optionalDefault: `true`

whether to populate the \`rewards\` array.

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optionalDefault: `base64`

Encoding format for Account data

Values: `base58``base64``base64+zstd``jsonParsed`

Details

*   `base58` is slow
*   `jsonParsed` encoding attempts to use program-specific state parsers to return more human-readable and explicit account state data.
*   If `jsonParsed` is requested but a parser cannot be found, the field falls back to `base64` encoding, detectable when the `data` field is type `string`.

### **Result**

`integer` - subscription id (needed to unsubscribe)

### Code sample:[​](#code-sample "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "id": "1",
  "method": "blockSubscribe",
  "params": ["all"]
}
```

```json
{
  "jsonrpc": "2.0",
  "id": "1",
  "method": "blockSubscribe",
  "params": [
    {
      "mentionsAccountOrProgram": "LieKvPRE8XeX3Y2xVNHjKlpAScD12lYySBVQ4HqoJ5op"
    },
    {
      "commitment": "confirmed",
      "encoding": "base64",
      "showRewards": true,
      "transactionDetails": "full"
    }
  ]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 0, "id": 1 }
```

#### Notification Format:[​](#notification-format "Direct link to heading")

The notification will be an object with the following fields:

*   `slot: <u64>` - The corresponding slot.
*   `err: <object|null>` - Error if something went wrong publishing the notification otherwise null.
*   `block: <object|null>` - A block object as seen in the [getBlock](/api/http#getblock) RPC HTTP method.

```json
{
  "jsonrpc": "2.0",
  "method": "blockNotification",
  "params": {
    "result": {
      "context": {
        "slot": 112301554
      },
      "value": {
        "slot": 112301554,
        "block": {
          "previousBlockhash": "GJp125YAN4ufCSUvZJVdCyWQJ7RPWMmwxoyUQySydZA",
          "blockhash": "6ojMHjctdqfB55JDpEpqfHnP96fiaHEcvzEQ2NNcxzHP",
          "parentSlot": 112301553,
          "transactions": [
            {
              "transaction": [
                "OpltwoUvWxYi1P2U8vbIdE/aPntjYo5Aa0VQ2JJyeJE2g9Vvxk8dDGgFMruYfDu8/IfUWb0REppTe7IpAuuLRgIBAAkWnj4KHRpEWWW7gvO1c0BHy06wZi2g7/DLqpEtkRsThAXIdBbhXCLvltw50ZnjDx2hzw74NVn49kmpYj2VZHQJoeJoYJqaKcvuxCi/2i4yywedcVNDWkM84Iuw+cEn9/ROCrXY4qBFI9dveEERQ1c4kdU46xjxj9Vi+QXkb2Kx45QFVkG4Y7HHsoS6WNUiw2m4ffnMNnOVdF9tJht7oeuEfDMuUEaO7l9JeUxppCvrGk3CP45saO51gkwVYEgKzhpKjCx3rgsYxNR81fY4hnUQXSbbc2Y55FkwgRBpVvQK7/+clR4Gjhd3L4y+OtPl7QF93Akg1LaU9wRMs5nvfDFlggqI9PqJl+IvVWrNRdBbPS8LIIhcwbRTkSbqlJQWxYg3Bo2CTVbw7rt1ZubuHWWp0mD/UJpLXGm2JprWTePNULzHu67sfqaWF99LwmwjTyYEkqkRt1T0Je5VzHgJs0N5jY4iIU9K3lMqvrKOIn/2zEMZ+ol2gdgjshx+sphIyhw65F3J/Dbzk04LLkK+CULmN571Y+hFlXF2ke0BIuUG6AUF+4214Cu7FXnqo3rkxEHDZAk0lRrAJ8X/Z+iwuwI5cgbd9uHXZaGT2cvhRs7reawctIXtX1s3kTqM9YV+/wCpDLAp8axcEkaQkLDKRoWxqp8XLNZSKial7Rk+ELAVVKWoWLRXRZ+OIggu0OzMExvVLE5VHqy71FNHq4gGitkiKYNFWSLIE4qGfdFLZXy/6hwS+wq9ewjikCpd//C9BcCL7Wl0iQdUslxNVCBZHnCoPYih9JXvGefOb9WWnjGy14sG9j70+RSVx6BlkFELWwFvIlWR/tHn3EhHAuL0inS2pwX7ZQTAU6gDVaoqbR2EiJ47cKoPycBNvHLoKxoY9AZaBjPl6q8SKQJSFyFd9n44opAgI6zMTjYF/8Ok4VpXEESp3QaoUyTI9sOJ6oFP6f4dwnvQelgXS+AEfAsHsKXxGAIUDQENAgMEBQAGBwgIDg8IBJCER3QXl1AVDBADCQoOAAQLERITDAjb7ugh3gOuTy==",
                "base64"
              ],
              "meta": {
                "err": null,
                "status": {
                  "Ok": null
                },
                "fee": 5000,
                "preBalances": [
                  1758510880, 2067120, 1566000, 1461600, 2039280, 2039280,
                  1900080, 1865280, 0, 3680844220, 2039280
                ],
                "postBalances": [
                  1758505880, 2067120, 1566000, 1461600, 2039280, 2039280,
                  1900080, 1865280, 0, 3680844220, 2039280
                ],
                "innerInstructions": [
                  {
                    "index": 0,
                    "instructions": [
                      {
                        "programIdIndex": 13,
                        "accounts": [1, 15, 3, 4, 2, 14],
                        "data": "21TeLgZXNbtHXVBzCaiRmH"
                      },
                      {
                        "programIdIndex": 14,
                        "accounts": [3, 4, 1],
                        "data": "6qfC8ic7Aq99"
                      },
                      {
                        "programIdIndex": 13,
                        "accounts": [1, 15, 3, 5, 2, 14],
                        "data": "21TeLgZXNbsn4QEpaSEr3q"
                      },
                      {
                        "programIdIndex": 14,
                        "accounts": [3, 5, 1],
                        "data": "6LC7BYyxhFRh"
                      }
                    ]
                  },
                  {
                    "index": 1,
                    "instructions": [
                      {
                        "programIdIndex": 14,
                        "accounts": [4, 3, 0],
                        "data": "7aUiLHFjSVdZ"
                      },
                      {
                        "programIdIndex": 19,
                        "accounts": [17, 18, 16, 9, 11, 12, 14],
                        "data": "8kvZyjATKQWYxaKR1qD53V"
                      },
                      {
                        "programIdIndex": 14,
                        "accounts": [9, 11, 18],
                        "data": "6qfC8ic7Aq99"
                      }
                    ]
                  }
                ],
                "logMessages": [
                  "Program QMNeHCGYnLVDn1icRAfQZpjPLBNkfGbSKRB83G5d8KB invoke [1]",
                  "Program QMWoBmAyJLAsA1Lh9ugMTw2gciTihncciphzdNzdZYV invoke [2]"
                ],
                "preTokenBalances": [
                  {
                    "accountIndex": 4,
                    "mint": "iouQcQBAiEXe6cKLS85zmZxUqaCqBdeHFpqKoSz615u",
                    "uiTokenAmount": {
                      "uiAmount": null,
                      "decimals": 6,
                      "amount": "0",
                      "uiAmountString": "0"
                    },
                    "owner": "LieKvPRE8XeX3Y2xVNHjKlpAScD12lYySBVQ4HqoJ5op",
                    "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
                  },
                  {
                    "accountIndex": 5,
                    "mint": "iouQcQBAiEXe6cKLS85zmZxUqaCqBdeHFpqKoSz615u",
                    "uiTokenAmount": {
                      "uiAmount": 11513.0679,
                      "decimals": 6,
                      "amount": "11513067900",
                      "uiAmountString": "11513.0679"
                    },
                    "owner": "rXhAofQCT7NN9TUqigyEAUzV1uLL4boeD8CRkNBSkYk",
                    "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
                  },
                  {
                    "accountIndex": 10,
                    "mint": "Saber2gLauYim4Mvftnrasomsv6NvAuncvMEZwcLpD1",
                    "uiTokenAmount": {
                      "uiAmount": null,
                      "decimals": 6,
                      "amount": "0",
                      "uiAmountString": "0"
                    },
                    "owner": "CL9wkGFT3SZRRNa9dgaovuRV7jrVVigBUZ6DjcgySsCU",
                    "programId": "TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb"
                  },
                  {
                    "accountIndex": 11,
                    "mint": "Saber2gLauYim4Mvftnrasomsv6NvAuncvMEZwcLpD1",
                    "uiTokenAmount": {
                      "uiAmount": 15138.514093,
                      "decimals": 6,
                      "amount": "15138514093",
                      "uiAmountString": "15138.514093"
                    },
                    "owner": "LieKvPRE8XeX3Y2xVNHjKlpAScD12lYySBVQ4HqoJ5op",
                    "programId": "TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb"
                  }
                ],
                "postTokenBalances": [
                  {
                    "accountIndex": 4,
                    "mint": "iouQcQBAiEXe6cKLS85zmZxUqaCqBdeHFpqKoSz615u",
                    "uiTokenAmount": {
                      "uiAmount": null,
                      "decimals": 6,
                      "amount": "0",
                      "uiAmountString": "0"
                    },
                    "owner": "LieKvPRE8XeX3Y2xVNHjKlpAScD12lYySBVQ4HqoJ5op",
                    "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
                  },
                  {
                    "accountIndex": 5,
                    "mint": "iouQcQBAiEXe6cKLS85zmZxUqaCqBdeHFpqKoSz615u",
                    "uiTokenAmount": {
                      "uiAmount": 11513.103028,
                      "decimals": 6,
                      "amount": "11513103028",
                      "uiAmountString": "11513.103028"
                    },
                    "owner": "rXhAofQCT7NN9TUqigyEAUzV1uLL4boeD8CRkNBSkYk",
                    "programId": "TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA"
                  },
                  {
                    "accountIndex": 10,
                    "mint": "Saber2gLauYim4Mvftnrasomsv6NvAuncvMEZwcLpD1",
                    "uiTokenAmount": {
                      "uiAmount": null,
                      "decimals": 6,
                      "amount": "0",
                      "uiAmountString": "0"
                    },
                    "owner": "CL9wkGFT3SZRRNa9dgaovuRV7jrVVigBUZ6DjcgySsCU",
                    "programId": "TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb"
                  },
                  {
                    "accountIndex": 11,
                    "mint": "Saber2gLauYim4Mvftnrasomsv6NvAuncvMEZwcLpD1",
                    "uiTokenAmount": {
                      "uiAmount": 15489.767829,
                      "decimals": 6,
                      "amount": "15489767829",
                      "uiAmountString": "15489.767829"
                    },
                    "owner": "BeiHVPRE8XeX3Y2xVNrSsTpAScH94nYySBVQ4HqgN9at",
                    "programId": "TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb"
                  }
                ],
                "rewards": []
              }
            }
          ],
          "blockTime": 1639926816,
          "blockHeight": 101210751
        },
        "err": null
      }
    },
    "subscription": 14
  }
}
```

**`blockUnsubscribe`[​](#blockunsubscribe "Direct link to heading")**
---------------------------------------------------------------

Unsubscribe from block notifications

### **Parameters**

`integer` required

subscription id to cancel

### **Result**

`<bool>` - unsubscribe success message

### Code sample:[​](#code-sample "Direct link to heading")

```bash
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "blockUnsubscribe",
  "params": [0]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": true, "id": 1 }
```

**`logsSubscribe`[​](#logssubscribe "Direct link to heading")**
---------------------------------------------------------

Subscribe to transaction logging

### **Parameters**

filter `string | object` required

filter criteria for the logs to receive results by account type. The following filters types are currently supported:

`string`

A string with one of the following values:

*   `all` - subscribe to all transactions except for simple vote transactions
*   `allWithVotes` - subscribe to all transactions including simple vote transactions

`object`

An object with the following field:

*   `mentions: [ <string> ]` - array of Pubkeys (as base-58 encoded strings) to listen for being mentioned in any transaction

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

`<integer>` - Subscription id (needed to unsubscribe)

### Code sample:[​](#code-sample "Direct link to heading")

```bash
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "logsSubscribe",
  "params": [
    {
      "mentions": [ "11111111111111111111111111111111" ]
    },
    {
      "commitment": "finalized"
    }
  ]
}
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "logsSubscribe",
  "params": [ "all" ]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 24040, "id": 1 }
```

#### Notification Format:[​](#notification-format "Direct link to heading")

The notification will be an RpcResponse JSON object with value equal to:

*   `signature: <string>` - The transaction signature base58 encoded.
*   `err: <object|null>` - Error if transaction failed, null if transaction succeeded. [TransactionError definitions](https://github.com/solana-labs/solana/blob/c0c60386544ec9a9ec7119229f37386d9f070523/sdk/src/transaction/error.rs#L13)
*   `logs: <array|null>` - Array of log messages the transaction instructions output during execution, null if simulation failed before the transaction was able to execute (for example due to an invalid blockhash or signature verification failure)

Example:

```json
{
  "jsonrpc": "2.0",
  "method": "logsNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5208469
      },
      "value": {
        "signature": "5h6xBEauJ3PK6SWCZ1PGjBvj8vDdWG3KpwATGy1ARAXFSDwt8GFXM7W5Ncn16wmqokgpiKRLuS83KUxyZyv2sUYv",
        "err": null,
        "logs": [
          "SBF program 83astBRguLMdt2h5U1Tpdq5tjFoJ6noeGwaY3mDLVcri success"
        ]
      }
    },
    "subscription": 24040
  }
}
```

**`logsUnsubscribe`[​](#logsunsubscribe "Direct link to heading")**
-------------------------------------------------------------

Unsubscribe from transaction logging

### **Parameters**

`integer` required

subscription id to cancel

### **Result**

`<bool>` - unsubscribe success message

### Code sample:[​](#code-sample "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "logsUnsubscribe",
  "params": [0]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": true, "id": 1 }
```

**`programSubscribe`[​](#programsubscribe "Direct link to heading")**
---------------------------------------------------------------

Subscribe to a program to receive notifications when the lamports or data for an account owned by the given program changes

### **Parameters**

`string` required

Pubkey of the `program_id`, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

[filters](/api/http#filter-criteria) `array` optional

filter results using various [filter objects](/api/http#filter-criteria)

info

The resultant account must meet **ALL** filter criteria to be included in the returned results

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optional

Encoding format for Account data

Values: `base58``base64``base64+zstd``jsonParsed`

Details

*   `base58` is slow.
*   [`jsonParsed`](https://docs.solana.com/api/http#parsed-responses%22%3E) encoding attempts to use program-specific state parsers to return more human-readable and explicit account state data.
*   If `jsonParsed` is requested but a parser cannot be found, the field falls back to `base64` encoding, detectable when the `data` field is type `string`.

### **Result**

`<integer>` - Subscription id (needed to unsubscribe)

### Code sample:[​](#code-sample "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "programSubscribe",
  "params": [
    "11111111111111111111111111111111",
    {
      "encoding": "base64",
      "commitment": "finalized"
    }
  ]
}
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "programSubscribe",
  "params": [
    "11111111111111111111111111111111",
    {
      "encoding": "jsonParsed"
    }
  ]
}
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "programSubscribe",
  "params": [
    "11111111111111111111111111111111",
    {
      "encoding": "base64",
      "filters": [
        {
          "dataSize": 80
        }
      ]
    }
  ]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 24040, "id": 1 }
```

#### Notification format[​](#notification-format "Direct link to heading")

The notification format is a **single** program account object as seen in the [getProgramAccounts](/api/http#getprogramaccounts) RPC HTTP method.

Base58 encoding:

```json
{
  "jsonrpc": "2.0",
  "method": "programNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5208469
      },
      "value": {
        "pubkey": "H4vnBqifaSACnKa7acsxstsY1iV1bvJNxsCY7enrd1hq",
        "account": {
          "data": [
            "11116bv5nS2h3y12kD1yUKeMZvGcKLSjQgX6BeV7u1FrjeJcKfsHPXHRDEHrBesJhZyqnnq9qJeUuF7WHxiuLuL5twc38w2TXNLxnDbjmuR",
            "base58"
          ],
          "executable": false,
          "lamports": 33594,
          "owner": "11111111111111111111111111111111",
          "rentEpoch": 636,
          "space": 80
        }
      }
    },
    "subscription": 24040
  }
}
```

Parsed-JSON encoding:

```json
{
  "jsonrpc": "2.0",
  "method": "programNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5208469
      },
      "value": {
        "pubkey": "H4vnBqifaSACnKa7acsxstsY1iV1bvJNxsCY7enrd1hq",
        "account": {
          "data": {
            "program": "nonce",
            "parsed": {
              "type": "initialized",
              "info": {
                "authority": "Bbqg1M4YVVfbhEzwA9SpC9FhsaG83YMTYoR4a8oTDLX",
                "blockhash": "LUaQTmM7WbMRiATdMMHaRGakPtCkc2GHtH57STKXs6k",
                "feeCalculator": {
                  "lamportsPerSignature": 5000
                }
              }
            }
          },
          "executable": false,
          "lamports": 33594,
          "owner": "11111111111111111111111111111111",
          "rentEpoch": 636,
          "space": 80
        }
      }
    },
    "subscription": 24040
  }
}
```

**`programUnsubscribe`[​](#programunsubscribe "Direct link to heading")**
-------------------------------------------------------------------

Unsubscribe from program-owned account change notifications

### **Parameters**

`number` required

id of account Subscription to cancel

### **Result**

`<bool>` - unsubscribe success message

### Code sample:[​](#code-sample "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "programUnsubscribe",
  "params": [0]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": true, "id": 1 }
```

**`signatureSubscribe`[​](#signaturesubscribe "Direct link to heading")**
-------------------------------------------------------------------

Subscribe to a transaction signature to receive notification when a given transaction is committed. On `signatureNotification` - the subscription is automatically cancelled.

### **Parameters**

`string` required

Transaction Signature, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

`<integer>` - subscription id (needed to unsubscribe)

### Code sample:[​](#code-sample "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "signatureSubscribe",
  "params": [
    "2EBVM6cB8vAAD93Ktr6Vd8p67XPbQzCJX47MpReuiCXJAtcjaxpvWpcg9Ege1Nr5Tk3a2GFrByT7WPBjdsTycY9b",
    {
      "commitment": "finalized"
    }
  ]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 0, "id": 1 }
```

#### Notification Format:[​](#notification-format "Direct link to heading")

The notification will be an RpcResponse JSON object with value containing an object with:

*   `err: <object|null>` - Error if transaction failed, null if transaction succeeded. [TransactionError definitions](https://github.com/solana-labs/solana/blob/c0c60386544ec9a9ec7119229f37386d9f070523/sdk/src/transaction/error.rs#L13)

Example:

```json
{
  "jsonrpc": "2.0",
  "method": "signatureNotification",
  "params": {
    "result": {
      "context": {
        "slot": 5207624
      },
      "value": {
        "err": null
      }
    },
    "subscription": 24006
  }
}
```

**`signatureUnsubscribe`[​](#signatureunsubscribe "Direct link to heading")**
-----------------------------------------------------------------------

Unsubscribe from signature confirmation notification

### **Parameters**

`number` required

subscription id to cancel

### **Result**

`<bool>` - unsubscribe success message

### Code sample:[​](#code-sample "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "signatureUnsubscribe",
  "params": [0]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": true, "id": 1 }
```

**`slotSubscribe`[​](#slotsubscribe "Direct link to heading")**
---------------------------------------------------------

Subscribe to receive notification anytime a slot is processed by the validator

### **Parameters**

**None**

### **Result**

`<integer>` - Subscription id (needed to unsubscribe)

### Code sample:[​](#code-sample "Direct link to heading")

```json
{ "jsonrpc": "2.0", "id": 1, "method": "slotSubscribe" }
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 0, "id": 1 }
```

#### Notification Format:[​](#notification-format "Direct link to heading")

The notification will be an object with the following fields:

*   `parent: <u64>` - The parent slot
*   `root: <u64>` - The current root slot
*   `slot: <u64>` - The newly set slot value

Example:

```json
{
  "jsonrpc": "2.0",
  "method": "slotNotification",
  "params": {
    "result": {
      "parent": 75,
      "root": 44,
      "slot": 76
    },
    "subscription": 0
  }
}
```

**`slotUnsubscribe`[​](#slotunsubscribe "Direct link to heading")**
-------------------------------------------------------------

Unsubscribe from slot notifications

### **Parameters**

`integer` required

subscription id to cancel

### **Result**

`<bool>` - unsubscribe success message

### Code sample:[​](#code-sample "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "slotUnsubscribe",
  "params": [0]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": true, "id": 1 }
```

**`slotsUpdatesSubscribe`[​](#slotsupdatessubscribe "Direct link to heading")**
-------------------------------------------------------------------------

Subscribe to receive a notification from the validator on a variety of updates on every slot

caution

This subscription is unstable

**NOTE: the format of this subscription may change in the future and it may not always be supported**

### **Parameters**

**None**

### **Result**

`<integer>` - Subscription id (needed to unsubscribe)

### Code sample:[​](#code-sample "Direct link to heading")

```json
{ "jsonrpc": "2.0", "id": 1, "method": "slotsUpdatesSubscribe" }
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 0, "id": 1 }
```

#### Notification Format:[​](#notification-format "Direct link to heading")

The notification will be an object with the following fields:

*   `parent: <u64>` - The parent slot
*   `slot: <u64>` - The newly updated slot
*   `timestamp: <i64>` - The Unix timestamp of the update
*   `type: <string>` - The update type, one of:
    *   "firstShredReceived"
    *   "completed"
    *   "createdBank"
    *   "frozen"
    *   "dead"
    *   "optimisticConfirmation"
    *   "root"

```json
{
  "jsonrpc": "2.0",
  "method": "slotsUpdatesNotification",
  "params": {
    "result": {
      "parent": 75,
      "slot": 76,
      "timestamp": 1625081266243,
      "type": "optimisticConfirmation"
    },
    "subscription": 0
  }
}
```

**`slotsUpdatesUnsubscribe`[​](#slotsupdatesunsubscribe "Direct link to heading")**
-----------------------------------------------------------------------------

Unsubscribe from slot-update notifications

### **Parameters**

`number` required

subscription id to cancel

### **Result**

`<bool>` - unsubscribe success message

### Code sample:[​](#code-sample "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "slotsUpdatesUnsubscribe",
  "params": [0]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": true, "id": 1 }
```

**`rootSubscribe`[​](#rootsubscribe "Direct link to heading")**
---------------------------------------------------------

Subscribe to receive notification anytime a new root is set by the validator.

### **Parameters**

**None**

### **Result**

`integer` - subscription id (needed to unsubscribe)

### Code sample:[​](#code-sample "Direct link to heading")

```json
{ "jsonrpc": "2.0", "id": 1, "method": "rootSubscribe" }
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 0, "id": 1 }
```

#### Notification Format:[​](#notification-format "Direct link to heading")

The result is the latest root slot number.

```json
{
  "jsonrpc": "2.0",
  "method": "rootNotification",
  "params": {
    "result": 42,
    "subscription": 0
  }
}
```

**`rootUnsubscribe`[​](#rootunsubscribe "Direct link to heading")**
-------------------------------------------------------------

Unsubscribe from root notifications

### **Parameters**

`number` required

subscription id to cancel

### **Result**

`<bool>` - unsubscribe success message

### Code sample:[​](#code-sample "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "rootUnsubscribe",
  "params": [0]
}
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": true, "id": 1 }
```

**`voteSubscribe`[​](#votesubscribe "Direct link to heading")**
---------------------------------------------------------

Subscribe to receive notification anytime a new vote is observed in gossip. These votes are pre-consensus therefore there is no guarantee these votes will enter the ledger.

caution

This subscription is unstable and only available if the validator was started with the `--rpc-pubsub-enable-vote-subscription` flag. The format of this subscription may change in the future

### **Parameters**

**None**

### **Result**

`<integer>` - subscription id (needed to unsubscribe)

### Code sample:[​](#code-sample "Direct link to heading")

```json
{ "jsonrpc": "2.0", "id": 1, "method": "voteSubscribe" }
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 0, "id": 1 }
```

#### Notification Format:[​](#notification-format "Direct link to heading")

The notification will be an object with the following fields:

*   `hash: <string>` - The vote hash
*   `slots: <array>` - The slots covered by the vote, as an array of u64 integers
*   `timestamp: <i64|null>` - The timestamp of the vote
*   `signature: <string>` - The signature of the transaction that contained this vote

```json
{
  "jsonrpc": "2.0",
  "method": "voteNotification",
  "params": {
    "result": {
      "hash": "8Rshv2oMkPu5E4opXTRyuyBeZBqQ4S477VG26wUTFxUM",
      "slots": [1, 2],
      "timestamp": null
    },
    "subscription": 0
  }
}
```

**`voteUnsubscribe`[​](#voteunsubscribe "Direct link to heading")**
-------------------------------------------------------------

Unsubscribe from vote notifications

### **Parameters**

`integer` required

subscription id to cancel

### **Result**

`<bool>` - unsubscribe success message

### Code sample:[​](#code-sample "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "voteUnsubscribe",
  "params": [0]
}
```
### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": true, "id": 1 }
```

JSON RPC API Deprecated Methods[​](#json-rpc-api-deprecated-methods "Direct link to heading")
---------------------------------------------------------------------------------------------

**`getConfirmedBlock`[​](#getconfirmedblock "Direct link to heading")**
-----------------------------------------------------------------

DEPRECATED

This method is expected to be removed in solana-core v2.0. **Please use [getBlock](#getblock) instead**

Returns identity and transaction information about a confirmed block in the ledger

### **Parameters**

`u64` required

slot number, as u64 integer

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optionalDefault: `finalized`

transactionDetails `string` optionalDefault: `full`

level of transaction detail to return, either "full", "signatures", or "none"

rewards `bool` optionalDefault: `true`

whether to populate the \`rewards\` array.

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optionalDefault: `json`

Encoding format for Account data

Values: `json``base58``base64``jsonParsed`

Details

*   `jsonParsed` encoding attempts to use program-specific instruction parsers to return more human-readable and explicit data in the `transaction.message.instructions` list.
*   If `jsonParsed` is requested but a parser cannot be found, the instruction falls back to regular JSON encoding (`accounts`, `data`, and `programIdIndex` fields).

### **Result**

The result field will be an object with the following fields:

*   `<null>` - if specified block is not confirmed
*   `<object>` - if block is confirmed, an object with the following fields:
    *   `blockhash: <string>` - the blockhash of this block, as base-58 encoded string
    *   `previousBlockhash: <string>` - the blockhash of this block's parent, as base-58 encoded string; if the parent block is not available due to ledger cleanup, this field will return "11111111111111111111111111111111"
    *   `parentSlot: <u64>` - the slot index of this block's parent
    *   `transactions: <array>` - present if "full" transaction details are requested; an array of JSON objects containing:
        *   `transaction: <object|[string,encoding]>` - [Transaction](#transaction-structure) object, either in JSON format or encoded binary data, depending on encoding parameter
        *   `meta: <object>` - transaction status metadata object, containing `null` or:
            *   `err: <object|null>` - Error if transaction failed, null if transaction succeeded. [TransactionError definitions](https://github.com/solana-labs/solana/blob/c0c60386544ec9a9ec7119229f37386d9f070523/sdk/src/transaction/error.rs#L13)
            *   `fee: <u64>` - fee this transaction was charged, as u64 integer
            *   `preBalances: <array>` - array of u64 account balances from before the transaction was processed
            *   `postBalances: <array>` - array of u64 account balances after the transaction was processed
            *   `innerInstructions: <array|null>` - List of [inner instructions](#inner-instructions-structure) or `null` if inner instruction recording was not enabled during this transaction
            *   `preTokenBalances: <array|undefined>` - List of [token balances](#token-balances-structure) from before the transaction was processed or omitted if token balance recording was not yet enabled during this transaction
            *   `postTokenBalances: <array|undefined>` - List of [token balances](#token-balances-structure) from after the transaction was processed or omitted if token balance recording was not yet enabled during this transaction
            *   `logMessages: <array|null>` - array of string log messages or `null` if log message recording was not enabled during this transaction
            *   DEPRECATED: `status: <object>` - Transaction status
                *   `"Ok": <null>` - Transaction was successful
                *   `"Err": <ERR>` - Transaction failed with TransactionError
    *   `signatures: <array>` - present if "signatures" are requested for transaction details; an array of signatures strings, corresponding to the transaction order in the block
    *   `rewards: <array>` - present if rewards are requested; an array of JSON objects containing:
        *   `pubkey: <string>` - The public key, as base-58 encoded string, of the account that received the reward
        *   `lamports: <i64>`\- number of reward lamports credited or debited by the account, as a i64
        *   `postBalance: <u64>` - account balance in lamports after the reward was applied
        *   `rewardType: <string|undefined>` - type of reward: "fee", "rent", "voting", "staking"
        *   `commission: <u8|undefined>` - vote account commission when the reward was credited, only present for voting and staking rewards
    *   `blockTime: <i64|null>` - estimated production time, as Unix timestamp (seconds since the Unix epoch). null if not available

#### For more details on returned data:[​](#for-more-details-on-returned-data "Direct link to heading")

*   [Transaction Structure](#transaction-structure)
*   [Inner Instructions Structure](#inner-instructions-structure)
*   [Token Balances Structure](#token-balances-structure)

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0", "id": 1,
    "method": "getConfirmedBlock",
    "params": [430, "base64"]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "blockTime": null,
    "blockhash": "3Eq21vXNB5s86c62bVuUfTeaMif1N2kUqRPBmGRJhyTA",
    "parentSlot": 429,
    "previousBlockhash": "mfcyqEXB3DnHXki6KjjmZck6YjmZLvpAByy2fj4nh6B",
    "rewards": [],
    "transactions": [
      {
        "meta": {
          "err": null,
          "fee": 5000,
          "innerInstructions": [],
          "logMessages": [],
          "postBalances": [499998932500, 26858640, 1, 1, 1],
          "postTokenBalances": [],
          "preBalances": [499998937500, 26858640, 1, 1, 1],
          "preTokenBalances": [],
          "status": {
            "Ok": null
          }
        },
        "transaction": [
          "AVj7dxHlQ9IrvdYVIjuiRFs1jLaDMHixgrv+qtHBwz51L4/ImLZhszwiyEJDIp7xeBSpm/TX5B7mYzxa+fPOMw0BAAMFJMJVqLw+hJYheizSoYlLm53KzgT82cDVmazarqQKG2GQsLgiqktA+a+FDR4/7xnDX7rsusMwryYVUdixfz1B1Qan1RcZLwqvxvJl4/t3zHragsUp0L47E24tAFUgAAAABqfVFxjHdMkoVmOYaR1etoteuKObS21cc1VbIQAAAAAHYUgdNXR0u3xNdiTr072z2DVec9EQQ/wNo1OAAAAAAAtxOUhPBp2WSjUNJEgfvy70BbxI00fZyEPvFHNfxrtEAQQEAQIDADUCAAAAAQAAAAAAAACtAQAAAAAAAAdUE18R96XTJCe+YfRfUp6WP+YKCy/72ucOL8AoBFSpAA==",
          "base64"
        ]
      }
    ]
  },
  "id": 1
}
```

**`getConfirmedBlocks`[​](#getconfirmedblocks "Direct link to heading")**
-------------------------------------------------------------------

DEPRECATED

This method is expected to be removed in solana-core v2.0 **Please use [getBlocks](#getblocks) instead**

Returns a list of confirmed blocks between two slots

### **Parameters**

`u64` required

start\_slot, as u64 integer

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

The result field will be an array of u64 integers listing confirmed blocks between `start_slot` and either `end_slot` - if provided, or latest confirmed block, inclusive. Max range allowed is 500,000 slots.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc": "2.0","id":1,"method":"getConfirmedBlocks","params":[5, 10]}
'
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": [5, 6, 7, 8, 9, 10], "id": 1 }
```

**`getConfirmedBlocksWithLimit`[​](#getconfirmedblockswithlimit "Direct link to heading")**
-------------------------------------------------------------------------------------

DEPRECATED

This method is expected to be removed in solana-core v2.0 **Please use [getBlocksWithLimit](#getblockswithlimit) instead**

Returns a list of confirmed blocks starting at the given slot

### **Parameters**

`u64` required

start\_slot, as u64 integer

`u64` required

limit, as u64 integer

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

The result field will be an array of u64 integers listing confirmed blocks starting at `start_slot` for up to `limit` blocks, inclusive.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0", "id": 1,
    "method": "getConfirmedBlocksWithLimit",
    "params": [5, 3]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": [5, 6, 7], "id": 1 }
```

**`getConfirmedSignaturesForAddress2`[​](#getconfirmedsignaturesforaddress2 "Direct link to heading")**
-------------------------------------------------------------------------------------------------

DEPRECATED

This method is expected to be removed in solana-core v2.0 **Please use [getSignaturesForAddress](#getsignaturesforaddress) instead**

Returns signatures for confirmed transactions that include the given address in their `accountKeys` list. Returns signatures backwards in time from the provided signature or most recent confirmed block

### **Parameters**

`string` required

account address, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optionalDefault: `finalized`

limit `number` optional

maximum transaction signatures to return (between 1 and 1,000, default: 1,000).

before `string` optional

start searching backwards from this transaction signature. (If not provided the search starts from the top of the highest max confirmed block.)

until `string` optional

search until this transaction signature, if found before limit reached.

### **Result**

The result field will be an array of `<object>`, ordered from newest to oldest transaction, containing transaction signature information with the following fields:

*   `signature: <string>` - transaction signature as base-58 encoded string
*   `slot: <u64>` - The slot that contains the block with the transaction
*   `err: <object|null>` - Error if transaction failed, null if transaction succeeded. [TransactionError definitions](https://github.com/solana-labs/solana/blob/c0c60386544ec9a9ec7119229f37386d9f070523/sdk/src/transaction/error.rs#L13)
*   `memo: <string|null>` - Memo associated with the transaction, null if no memo is present
*   `blockTime: <i64|null>` - estimated production time, as Unix timestamp (seconds since the Unix epoch) of when transaction was processed. null if not available.

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getConfirmedSignaturesForAddress2",
    "params": [
      "Vote111111111111111111111111111111111111111",
      {
        "limit": 1
      }
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": [
    {
      "err": null,
      "memo": null,
      "signature": "5h6xBEauJ3PK6SWCZ1PGjBvj8vDdWG3KpwATGy1ARAXFSDwt8GFXM7W5Ncn16wmqokgpiKRLuS83KUxyZyv2sUYv",
      "slot": 114,
      "blockTime": null
    }
  ],
  "id": 1
}
```

**`getConfirmedTransaction`[​](#getconfirmedtransaction "Direct link to heading")**
-----------------------------------------------------------------------------

DEPRECATED

This method is expected to be removed in solana-core v2.0 **Please use [getTransaction](#gettransaction) instead**

Returns transaction details for a confirmed transaction

### **Parameters**

`string` required

transaction signature, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

[encoding](https://docs.solana.com/api/http#parsed-responses) `string` optionalDefault: `json`

Encoding format for Account data

Values: `json``base58``base64``jsonParsed`

Details

*   `base58` is slow and limited to less than 129 bytes of Account data.
*   `jsonParsed` encoding attempts to use program-specific instruction parsers to return more human-readable and explicit data in the `transaction.message.instructions` list.
*   If `jsonParsed` is requested but a parser cannot be found, the instruction falls back to regular `json` encoding (`accounts`, `data`, and `programIdIndex` fields).

### **Result**

*   `<null>` - if transaction is not found or not confirmed
*   `<object>` - if transaction is confirmed, an object with the following fields:
    *   `slot: <u64>` - the slot this transaction was processed in
    *   `transaction: <object|[string,encoding]>` - [Transaction](#transaction-structure) object, either in JSON format or encoded binary data, depending on encoding parameter
    *   `blockTime: <i64|null>` - estimated production time, as Unix timestamp (seconds since the Unix epoch) of when the transaction was processed. null if not available
    *   `meta: <object|null>` - transaction status metadata object:
        *   `err: <object|null>` - Error if transaction failed, null if transaction succeeded. [TransactionError definitions](https://docs.rs/solana-sdk/VERSION_FOR_DOCS_RS/solana_sdk/transaction/enum.TransactionError.html)
        *   `fee: <u64>` - fee this transaction was charged, as u64 integer
        *   `preBalances: <array>` - array of u64 account balances from before the transaction was processed
        *   `postBalances: <array>` - array of u64 account balances after the transaction was processed
        *   `innerInstructions: <array|null>` - List of [inner instructions](#inner-instructions-structure) or `null` if inner instruction recording was not enabled during this transaction
        *   `preTokenBalances: <array|undefined>` - List of [token balances](#token-balances-structure) from before the transaction was processed or omitted if token balance recording was not yet enabled during this transaction
        *   `postTokenBalances: <array|undefined>` - List of [token balances](#token-balances-structure) from after the transaction was processed or omitted if token balance recording was not yet enabled during this transaction
        *   `logMessages: <array|null>` - array of string log messages or `null` if log message recording was not enabled during this transaction
        *   DEPRECATED: `status: <object>` - Transaction status
            *   `"Ok": <null>` - Transaction was successful
            *   `"Err": <ERR>` - Transaction failed with TransactionError

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getConfirmedTransaction",
    "params": [
      "2nBhEBYYvfaAe16UMNqRHre4YNSskvuYgx3M6E4JP1oDYvZEJHvoPzyUidNgNX5r9sTyN1J9UxtbCXy2rqYcuyuv",
      "base64"
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "meta": {
      "err": null,
      "fee": 5000,
      "innerInstructions": [],
      "postBalances": [499998932500, 26858640, 1, 1, 1],
      "postTokenBalances": [],
      "preBalances": [499998937500, 26858640, 1, 1, 1],
      "preTokenBalances": [],
      "status": {
        "Ok": null
      }
    },
    "slot": 430,
    "transaction": [
      "AVj7dxHlQ9IrvdYVIjuiRFs1jLaDMHixgrv+qtHBwz51L4/ImLZhszwiyEJDIp7xeBSpm/TX5B7mYzxa+fPOMw0BAAMFJMJVqLw+hJYheizSoYlLm53KzgT82cDVmazarqQKG2GQsLgiqktA+a+FDR4/7xnDX7rsusMwryYVUdixfz1B1Qan1RcZLwqvxvJl4/t3zHragsUp0L47E24tAFUgAAAABqfVFxjHdMkoVmOYaR1etoteuKObS21cc1VbIQAAAAAHYUgdNXR0u3xNdiTr072z2DVec9EQQ/wNo1OAAAAAAAtxOUhPBp2WSjUNJEgfvy70BbxI00fZyEPvFHNfxrtEAQQEAQIDADUCAAAAAQAAAAAAAACtAQAAAAAAAAdUE18R96XTJCe+YfRfUp6WP+YKCy/72ucOL8AoBFSpAA==",
      "base64"
    ]
  },
  "id": 1
}
```

**`getFeeCalculatorForBlockhash`[​](#getfeecalculatorforblockhash "Direct link to heading")**
---------------------------------------------------------------------------------------

DEPRECATED

This method is expected to be removed in solana-core v2.0 **Please use [isBlockhashValid](#isblockhashvalid) or [getFeeForMessage](#getfeeformessage) instead**

Returns the fee calculator associated with the query blockhash, or `null` if the blockhash has expired

### **Parameters**

`string` required

query blockhash, as a base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

minContextSlot `number` optional

The minimum slot that the request can be evaluated at

### **Result**

The result will be an RpcResponse JSON object with `value` equal to:

*   `<null>` - if the query blockhash has expired; or
*   `<object>` - otherwise, a JSON object containing:
    *   `feeCalculator: <object>` - `FeeCalculator` object describing the cluster fee rate at the queried blockhash

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getFeeCalculatorForBlockhash",
    "params": [
      "GJxqhuxcgfn5Tcj6y3f8X4FeCDd2RQ6SnEMo1AAxrPRZ"
    ]
  }
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 221
    },
    "value": {
      "feeCalculator": {
        "lamportsPerSignature": 5000
      }
    }
  },
  "id": 1
}
```

**`getFeeRateGovernor`[​](#getfeerategovernor "Direct link to heading")**
-------------------------------------------------------------------

DEPRECATED

This method is expected to be removed in solana-core v2.0

Returns the fee rate governor information from the root bank

### **Parameters**

**None**

### **Result**

The result will be an RpcResponse JSON object with `value` equal to an `object` with the following fields:

*   `burnPercent: <u8>` - Percentage of fees collected to be destroyed
*   `maxLamportsPerSignature: <u64>` - Largest value `lamportsPerSignature` can attain for the next slot
*   `minLamportsPerSignature: <u64>` - Smallest value `lamportsPerSignature` can attain for the next slot
*   `targetLamportsPerSignature: <u64>` - Desired fee rate for the cluster
*   `targetSignaturesPerSlot: <u64>` - Desired signature rate for the cluster

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getFeeRateGovernor"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 54
    },
    "value": {
      "feeRateGovernor": {
        "burnPercent": 50,
        "maxLamportsPerSignature": 100000,
        "minLamportsPerSignature": 5000,
        "targetLamportsPerSignature": 10000,
        "targetSignaturesPerSlot": 20000
      }
    }
  },
  "id": 1
}
```

**`getFees`[​](#getfees "Direct link to heading")**
---------------------------------------------

DEPRECATED

This method is expected to be removed in solana-core v2.0 **Please use [getFeeForMessage](#getfeeformessage) instead**

Returns a recent block hash from the ledger, a fee schedule that can be used to compute the cost of submitting a transaction using it, and the last slot in which the blockhash will be valid.

### **Parameters**

`string` required

Pubkey of account to query, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

The result will be an RpcResponse JSON object with `value` set to a JSON object with the following fields:

*   `blockhash: <string>` - a Hash as base-58 encoded string
*   `feeCalculator: <object>` - FeeCalculator object, the fee schedule for this block hash
*   `lastValidSlot: <u64>` - DEPRECATED - this value is inaccurate and should not be relied upon
*   `lastValidBlockHeight: <u64>` - last [block height](/terminology#block-height) at which the blockhash will be valid

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  { "jsonrpc":"2.0", "id": 1, "method":"getFees"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1
    },
    "value": {
      "blockhash": "CSymwgTNX1j3E4qhKfJAUE41nBWEwXufoYryPbkde5RR",
      "feeCalculator": {
        "lamportsPerSignature": 5000
      },
      "lastValidSlot": 297,
      "lastValidBlockHeight": 296
    }
  },
  "id": 1
}
```

**`getRecentBlockhash`[​](#getrecentblockhash "Direct link to heading")**
-------------------------------------------------------------------

DEPRECATED

This method is expected to be removed in solana-core v2.0 **Please use [getLatestBlockhash](#getlatestblockhash) instead**

Returns a recent block hash from the ledger, and a fee schedule that can be used to compute the cost of submitting a transaction using it.

### **Parameters**

`string` required

Pubkey of account to query, as base-58 encoded string

`object` optional

Configuration object containing the following fields:

[commitment](https://docs.solana.com/api/http#configuring-state-commitment) `string` optional

### **Result**

An RpcResponse containing a JSON object consisting of a string blockhash and FeeCalculator JSON object.

*   `RpcResponse<object>` - RpcResponse JSON object with `value` field set to a JSON object including:
*   `blockhash: <string>` - a Hash as base-58 encoded string
*   `feeCalculator: <object>` - FeeCalculator object, the fee schedule for this block hash

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getRecentBlockhash"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{
  "jsonrpc": "2.0",
  "result": {
    "context": {
      "slot": 1
    },
    "value": {
      "blockhash": "CSymwgTNX1j3E4qhKfJAUE41nBWEwXufoYryPbkde5RR",
      "feeCalculator": {
        "lamportsPerSignature": 5000
      }
    }
  },
  "id": 1
}
```

**`getSnapshotSlot`[​](#getsnapshotslot "Direct link to heading")**
-------------------------------------------------------------

DEPRECATED

This method is expected to be removed in solana-core v2.0 **Please use [getHighestSnapshotSlot](#gethighestsnapshotslot) instead**

Returns the highest slot that the node has a snapshot for

### **Parameters**

**None**

### **Result**

`<u64>` - Snapshot slot

### Code sample:[​](#code-sample "Direct link to heading")

```bash
curl http://localhost:8899 -X POST -H "Content-Type: application/json" -d '
  {"jsonrpc":"2.0","id":1, "method":"getSnapshotSlot"}
'
```

### Response:[​](#response "Direct link to heading")

```json
{ "jsonrpc": "2.0", "result": 100, "id": 1 }
```

Result when the node has no snapshot:

```json
{
  "jsonrpc": "2.0",
  "error": { "code": -32008, "message": "No snapshot" },
  "id": 1
}
```