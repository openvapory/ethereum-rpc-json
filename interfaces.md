# interfaces

- [db](#db)
- [net](#net)
- [personal](#personal)
- [shh](#shh)
- [trace](#trace)
- [vap](#vap)
- [vapcore](#vapcore)
- [web3](#web3)


## db

- [db_getHex](#db_getHex)
- [db_getString](#db_getString)
- [db_putHex](#db_putHex)
- [db_putString](#db_putString)

### db_getHex

Returns binary data from the local database. (Deprecated and not supported, to be removed in a future version)

#### parameters

- `String` - Database name
- `String` - Key name

#### returns

- `Data` - The previously stored data

### db_getString

Returns string from the local database. (Deprecated and not supported, to be removed in a future version)

#### parameters

- `String` - Database name
- `String` - Key name

#### returns

- `String` - The previously stored string

### db_putHex

Stores binary data in the local database. (Deprecated and not supported, to be removed in a future version)

#### parameters

- `String` - Database name
- `String` - Key name
- `Data` - The data to store

#### returns

- `Boolean` - `true` if the value was stored, otherwise `false`

### db_putString

Stores a string in the local database. (Deprecated and not supported, to be removed in a future version)

#### parameters

- `String` - Database name
- `String` - Key name
- `String` - The string to store

#### returns

- `Boolean` - `true` if the value was stored, otherwise `false`


## net

- [net_listening](#net_listening)
- [net_peerCount](#net_peerCount)
- [net_version](#net_version)

### net_listening

Returns `true` if client is actively listening for network connections.

#### parameters

none

#### returns

- `Boolean` - `true` when listening, otherwise `false`.

### net_peerCount

Returns number of peers currenly connected to the client.

#### parameters

none

#### returns

- `Quantity` - Integer of the number of connected peers

### net_version

Returns the current network protocol version.

#### parameters

none

#### returns

- `String` - The current network protocol version


## personal

- [personal_listAccounts](#personal_listAccounts)
- [personal_newAccount](#personal_newAccount)
- [personal_signAndSendTransaction](#personal_signAndSendTransaction)
- [personal_signerEnabled](#personal_signerEnabled)
- [personal_unlockAccount](#personal_unlockAccount)

### personal_listAccounts

Returns a list of addresses owned by client.

#### parameters

none

#### returns

- `Array` - 20 Bytes addresses owned by the client.

### personal_newAccount

Creates new account

#### parameters

- `String` - Password

#### returns

- `Address` - The created address

### personal_signAndSendTransaction

Sends and signs a transaction given account passphrase. Does not require the account to be unlocked nor unlocks the account for future transactions. 

#### parameters

- `Object` - The transaction object
    - `data`/`Data` - The compiled code of a contract OR the hash of the invoked method signature and encoded parameters. For details see [Vapory Contract ABI](https://github.com/vaporyco/wiki/wiki/Vapory-Contract-ABI)
    - `from`/`Address` - 20 Bytes - The address the transaction is send from
    - `gas`/`Quantity` - (optional) (default: 90000) Integer of the gas provided for the transaction execution. It will return unused gas
    - `gasPrice`/`Quantity` - (optional) (default: To-Be-Determined) Integer of the gasPrice used for each paid gas
    - `nonce`/`Quantity` - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
    - `to`/`Address` - 20 Bytes - (optional when creating new contract) The address the transaction is directed to
    - `value`/`Quantity` - (optional) Integer of the value send with this transaction
- `String` - Passphrase to unlock `from` account.

#### returns

- `Data` - 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available

### personal_signerEnabled

Returns whether signer is enabled/disabled.

#### parameters

none

#### returns

- `Boolean` - true when enabled, false when disabled

### personal_unlockAccount

?

#### parameters

?
?
?

#### returns

- `Boolean` - whether the call was successful


## shh

- [shh_addToGroup](#shh_addToGroup)
- [shh_getFilterChanges](#shh_getFilterChanges)
- [shh_getMessages](#shh_getMessages)
- [shh_hasIdentity](#shh_hasIdentity)
- [shh_newFilter](#shh_newFilter)
- [shh_newGroup](#shh_newGroup)
- [shh_newIdentity](#shh_newIdentity)
- [shh_post](#shh_post)
- [shh_uninstallFilter](#shh_uninstallFilter)
- [shh_version](#shh_version)

### shh_addToGroup

(?)

#### parameters

- `Data` - 60 Bytes - The identity address to add to a group (?)

#### returns

- `Boolean` - `true` if the identity was successfully added to the group, otherwise `false` (?)

### shh_getFilterChanges

Polling method for whisper filters. Returns new messages since the last call of this method.
**Note** calling the [shh_getMessages](#shh_getmessages) method, will reset the buffer for this method, so that you won't receive duplicate messages.

#### parameters

- `Quantity` - The filter id

#### returns

- `Array` - Array of messages received since last poll

### shh_getMessages

Get all messages matching a filter. Unlike `shh_getFilterChanges` this returns all messages.

#### parameters

- `Quantity` - The filter id

#### returns

See [shh_getFilterChanges](#shh_getfilterchanges)

### shh_hasIdentity

Checks if the client hold the private keys for a given identity.

#### parameters

- `Data` - 60 Bytes - The identity address to check

#### returns

- `Boolean` - `true` if the client holds the privatekey for that identity, otherwise `false`

### shh_newFilter

Creates filter to notify, when client receives whisper message matching the filter options.

#### parameters

- `Object` - The filter options:
    - `to`/`Data` - (optional) 60 Bytes - Identity of the receiver. *When present it will try to decrypt any incoming message if the client holds the private key to this identity.*
    - `topics`/`Array` - Array of `DATA` topics which the incoming message's topics should match.  You can use the following combinations

#### returns

- `Quantity` - The newly created filter

### shh_newGroup

(?)

#### parameters

none

#### returns

- `Data` - 60 Bytes - the address of the new group. (?)

### shh_newIdentity

Creates new whisper identity in the client.

#### parameters

none

#### returns

- `Data` - 60 Bytes - the address of the new identiy

### shh_post

Sends a whisper message.

#### parameters

- `Object` - The whisper post object:
    - `from`/`Data` - (optional) 60 Bytes - The identity of the sender
    - `payload`/`Data` - The payload of the message
    - `priority`/`Quantity` - The integer of the priority in a rang from ... (?)
    - `to`/`Data` - (optional) 60 Bytes - The identity of the receiver. When present whisper will encrypt the message so that only the receiver can decrypt it
    - `topics`/`Array` - Array of `DATA` topics, for the receiver to identify messages
    - `ttl`/`Quantity` - Integer of the time to live in seconds.

#### returns

- `Boolean` - `true` if the message was send, otherwise `false`

### shh_uninstallFilter

Uninstalls a filter with given id. Should always be called when watch is no longer needed.
Additonally Filters timeout when they aren't requested with [shh_getFilterChanges](#shh_getfilterchanges) for a period of time.

#### parameters

- `Quantity` - The filter id

#### returns

- `Boolean` - `true` if the filter was successfully uninstalled, otherwise `false`

### shh_version

Returns the current whisper protocol version.

#### parameters

none

#### returns

- `String` - The current whisper protocol version


## trace

- [trace_block](#trace_block)
- [trace_filter](#trace_filter)
- [trace_get](#trace_get)
- [trace_transaction](#trace_transaction)

### trace_block

Returns traces created at given block

#### parameters

- `BlockNumber` - Integer block number, or 'latest' for the last mined block or 'pending', 'earliest' for not yet mined transactions

#### returns

- `Array` - Block traces

### trace_filter

Returns traces matching given filter

#### parameters

- `Object` - The filter object

#### returns

- `Array` - Traces matching given filter

### trace_get

Returns trace at given position.

#### parameters

- `Hash` - Transaction hash
- `Integer` - Trace position witing transaction

#### returns

- `Object` - Trace object

### trace_transaction

Returns all traces of given transaction

#### parameters

- `Hash` - Transaction hash

#### returns

- `Array` - Traces of given transaction


## vap

- [vap_accounts](#vap_accounts)
- [vap_blockNumber](#vap_blockNumber)
- [vap_call](#vap_call)
- [vap_coinbase](#vap_coinbase)
- [vap_compileLLL](#vap_compileLLL)
- [vap_compileSerpent](#vap_compileSerpent)
- [vap_compileSolidity](#vap_compileSolidity)
- [vap_estimateGas](#vap_estimateGas)
- [vap_fetchQueuedTransactions](#vap_fetchQueuedTransactions)
- [vap_flush](#vap_flush)
- [vap_gasPrice](#vap_gasPrice)
- [vap_getBalance](#vap_getBalance)
- [vap_getBlockByHash](#vap_getBlockByHash)
- [vap_getBlockByNumber](#vap_getBlockByNumber)
- [vap_getBlockTransactionCountByHash](#vap_getBlockTransactionCountByHash)
- [vap_getBlockTransactionCountByNumber](#vap_getBlockTransactionCountByNumber)
- [vap_getCode](#vap_getCode)
- [vap_getCompilers](#vap_getCompilers)
- [vap_getFilterChanges](#vap_getFilterChanges)
- [vap_getFilterChangesEx](#vap_getFilterChangesEx)
- [vap_getFilterLogs](#vap_getFilterLogs)
- [vap_getFilterLogsEx](#vap_getFilterLogsEx)
- [vap_getLogs](#vap_getLogs)
- [vap_getLogsEx](#vap_getLogsEx)
- [vap_getStorageAt](#vap_getStorageAt)
- [vap_getTransactionByBlockHashAndIndex](#vap_getTransactionByBlockHashAndIndex)
- [vap_getTransactionByBlockNumberAndIndex](#vap_getTransactionByBlockNumberAndIndex)
- [vap_getTransactionByHash](#vap_getTransactionByHash)
- [vap_getTransactionCount](#vap_getTransactionCount)
- [vap_getTransactionReceipt](#vap_getTransactionReceipt)
- [vap_getUncleByBlockHashAndIndex](#vap_getUncleByBlockHashAndIndex)
- [vap_getUncleByBlockNumberAndIndex](#vap_getUncleByBlockNumberAndIndex)
- [vap_getUncleCountByBlockHash](#vap_getUncleCountByBlockHash)
- [vap_getUncleCountByBlockNumber](#vap_getUncleCountByBlockNumber)
- [vap_getWork](#vap_getWork)
- [vap_hashrate](#vap_hashrate)
- [vap_inspectTransaction](#vap_inspectTransaction)
- [vap_mining](#vap_mining)
- [vap_newBlockFilter](#vap_newBlockFilter)
- [vap_newFilter](#vap_newFilter)
- [vap_newFilterEx](#vap_newFilterEx)
- [vap_newPendingTransactionFilter](#vap_newPendingTransactionFilter)
- [vap_notePassword](#vap_notePassword)
- [vap_pendingTransactions](#vap_pendingTransactions)
- [vap_protocolVersion](#vap_protocolVersion)
- [vap_register](#vap_register)
- [vap_sendRawTransaction](#vap_sendRawTransaction)
- [vap_sendTransaction](#vap_sendTransaction)
- [vap_sign](#vap_sign)
- [vap_signTransaction](#vap_signTransaction)
- [vap_submitHashrate](#vap_submitHashrate)
- [vap_submitWork](#vap_submitWork)
- [vap_syncing](#vap_syncing)
- [vap_uninstallFilter](#vap_uninstallFilter)
- [vap_unregister](#vap_unregister)

### vap_accounts

Returns a list of addresses owned by client.

#### parameters

none

#### returns

- `Array` - 20 Bytes - addresses owned by the client

### vap_blockNumber

Returns the number of most recent block.

#### parameters

none

#### returns

- `Quantity` - integer of the current block number the client is on

### vap_call

Executes a new message call immediately without creating a transaction on the block chain.

#### parameters

- `Object` - The transaction call object
    - `data`/`Data` - (optional) Hash of the method signature and encoded parameters. For details see [Vapory Contract ABI](https://github.com/vaporyco/wiki/wiki/Vapory-Contract-ABI)
    - `from`/`Address` - (optional) 20 Bytes - The address the transaction is send from
    - `gas`/`Quantity` - (optional) Integer of the gas provided for the transaction execution. vap_call consumes zero gas, but this parameter may be needed by some executions
    - `gasPrice`/`Quantity` - (optional) Integer of the gasPrice used for each paid gas
    - `to`/`Address` - 20 Bytes  - The address the transaction is directed to
    - `value`/`Quantity` - (optional) Integer of the value send with this transaction
- `BlockNumber` - integer block number, or the string `'latest'`, `'earliest'` or `'pending'`, see the [default block parameter](#the-default-block-parameter)

#### returns

- `Data` - the return value of executed contract

### vap_coinbase

Returns the client coinbase address.

#### parameters

none

#### returns

- `Address` - The current coinbase address

### vap_compileLLL

Returns compiled LLL code.

#### parameters

- `String` - The source code

#### returns

- `Data` - The compiled source code

### vap_compileSerpent

Returns compiled serpent code.

#### parameters

- `String` - The source code

#### returns

- `Data` - The compiled source code

### vap_compileSolidity

Returns compiled solidity code.

#### parameters

- `String` - The source code

#### returns

- `Data` - The compiled source code

### vap_estimateGas

Makes a call or transaction, which won't be added to the blockchain and returns the used gas, which can be used for estimating the used gas.

#### parameters

- `Object` - see [vap_sendTransaction](#vap_sendTransaction)

#### returns

- `Quantity` - The amount of gas used

### vap_fetchQueuedTransactions

?

#### parameters

?

#### returns

- `Boolean` - whether the call was successful

### vap_flush

?

#### parameters

none

#### returns

- `Boolean` - whether the call was successful

### vap_gasPrice

Returns the current price per gas in wei.

#### parameters

none

#### returns

- `Quantity` - integer of the current gas price in wei

### vap_getBalance

Returns the balance of the account of given address.

#### parameters

- `Address` - 20 Bytes - address to check for balance
- `BlockNumber` - integer block number, or the string `'latest'`, `'earliest'` or `'pending'`, see the [default block parameter](#the-default-block-parameter)

#### returns

- `Quantity` - integer of the current balance in wei

### vap_getBlockByHash

Returns information about a block by hash.

#### parameters

- `Hash` - Hash of a block
- `Boolean` - If `true` it returns the full transaction objects, if `false` only the hashes of the transactions

#### returns

- `Object` - A block object, or `null` when no block was found
    - `difficulty`/`Quantity` - integer of the difficulty for this block
    - `extraData`/`Data` - the 'extra data' field of this block
    - `gasLimit`/`Quantity` - the maximum gas allowed in this block
    - `gasUsed`/`Quantity` - the total used gas by all transactions in this block
    - `hash`/`Hash` - 32 Bytes - hash of the block. `null` when its pending block
    - `logsBloom`/`Data` - 256 Bytes - the bloom filter for the logs of the block. `null` when its pending block
    - `miner`/`Address` - 20 Bytes - the address of the beneficiary to whom the mining rewards were given
    - `nonce`/`Data` - 8 Bytes - hash of the generated proof-of-work. `null` when its pending block
    - `number`/`Quantity` - The block number. `null` when its pending block
    - `parentHash`/`Hash` - 32 Bytes - hash of the parent block
    - `receiptsRoot`/`Data` - 32 Bytes - the root of the receipts trie of the block
    - `sha3Uncles`/`Data` - 32 Bytes - SHA3 of the uncles data in the block
    - `size`/`Quantity` - integer the size of this block in bytes
    - `stateRoot`/`Data` - 32 Bytes - the root of the final state trie of the block
    - `timestamp`/`Quantity` - the unix timestamp for when the block was collated
    - `totalDifficulty`/`Quantity` - integer of the total difficulty of the chain until this block
    - `transactions`/`Array` - Array of transaction objects, or 32 Bytes transaction hashes depending on the last given parameter
    - `transactionsRoot`/`Data` - 32 Bytes - the root of the transaction trie of the block
    - `uncles`/`Array` - Array of uncle hashes

### vap_getBlockByNumber

Returns information about a block by block number.

#### parameters

- `BlockNumber` - integer of a block number, or the string `'earliest'`, `'latest'` or `'pending'`, as in the [default block parameter](#the-default-block-parameter)
- `Boolean` - If `true` it returns the full transaction objects, if `false` only the hashes of the transactions

#### returns

See [vap_getBlockByHash](#vap_getblockbyhash)

### vap_getBlockTransactionCountByHash

Returns the number of transactions in a block from a block matching the given block hash.

#### parameters

- `Hash` - 32 Bytes - hash of a block

#### returns

- `Quantity` - integer of the number of transactions in this block

### vap_getBlockTransactionCountByNumber

Returns the number of transactions in a block from a block matching the given block number.

#### parameters

- `BlockNumber` - integer of a block number, or the string `'earliest'`, `'latest'` or `'pending'`, as in the [default block parameter](#the-default-block-parameter)

#### returns

- `Quantity` - integer of the number of transactions in this block

### vap_getCode

Returns code at a given address.

#### parameters

- `Address` - 20 Bytes - address
- `BlockNumber` - integer block number, or the string `'latest'`, `'earliest'` or `'pending'`, see the [default block parameter](#the-default-block-parameter)

#### returns

- `Data` - the code from the given address

### vap_getCompilers

Returns a list of available compilers in the client.

#### parameters

none

#### returns

- `Array` - Array of available compilers

### vap_getFilterChanges

Polling method for a filter, which returns an array of logs which occurred since last poll.

#### parameters

- `Quantity` - The filter id

#### returns

- `Array` - Array of log objects, or an empty array if nothing has changed since last poll

### vap_getFilterChangesEx

?

#### parameters

?

#### returns

- `Boolean` - whether the call was successful

### vap_getFilterLogs

Returns an array of all logs matching filter with given id.

#### parameters

- `Quantity` - The filter id

#### returns

See [vap_getFilterChanges](#vap_getfilterchanges)

### vap_getFilterLogsEx

?

#### parameters

?

#### returns

- `Boolean` - whether the call was successful

### vap_getLogs

Returns an array of all logs matching a given filter object.

#### parameters

- `Object` - The filter object, see [vap_newFilter parameters](#vap_newfilter)

#### returns

See [vap_getFilterChanges](#vap_getfilterchanges)

### vap_getLogsEx

?

#### parameters

?

#### returns

- `Boolean` - whether the call was successful

### vap_getStorageAt

Returns the value from a storage position at a given address.

#### parameters

- `Address` - 20 Bytes - address of the storage
- `Quantity` - integer of the position in the storage
- `BlockNumber` - integer block number, or the string `'latest'`, `'earliest'` or `'pending'`, see the [default block parameter](#the-default-block-parameter)

#### returns

- `Data` - the value at this storage position

### vap_getTransactionByBlockHashAndIndex

Returns information about a transaction by block hash and transaction index position.

#### parameters

- `Hash` - hash of a block
- `Quantity` - integer of the transaction index position

#### returns

See [vap_getBlockByHash](#vap_gettransactionbyhash)

### vap_getTransactionByBlockNumberAndIndex

Returns information about a transaction by block number and transaction index position.

#### parameters

- `BlockNumber` - a block number, or the string `'earliest'`, `'latest'` or `'pending'`, as in the [default block parameter](#the-default-block-parameter)
- `Quantity` - The transaction index position

#### returns

See [vap_getBlockByHash](#vap_gettransactionbyhash)

### vap_getTransactionByHash

Returns the information about a transaction requested by transaction hash.

#### parameters

- `Hash` - 32 Bytes - hash of a transaction

#### returns

- `Object` - A transaction object, or `null` when no transaction was found:
    - `blockHash`/`Hash` - 32 Bytes - hash of the block where this transaction was in. `null` when its pending.
    - `blockNumber`/`BlockNumber` - block number where this transaction was in. `null` when its pending.
    - `from`/`Address` - 20 Bytes - address of the sender.
    - `gas`/`Quantity` - gas provided by the sender.
    - `gasPrice`/`Quantity` - gas price provided by the sender in Wei.
    - `hash`/`Hash` - 32 Bytes - hash of the transaction.
    - `input`/`Data` - the data send along with the transaction.
    - `nonce`/`Quantity` - the number of transactions made by the sender prior to this one.
    - `to`/`Address` - 20 Bytes - address of the receiver. `null` when its a contract creation transaction.
    - `transactionIndex`/`Quantity` - integer of the transactions index position in the block. `null` when its pending.
    - `value`/`Quantity` - value transferred in Wei.

### vap_getTransactionCount

Returns the number of transactions *sent* from an address.

#### parameters

- `Address` - 20 Bytes - address
- `BlockNumber` - integer block number, or the string `'latest'`, `'earliest'` or `'pending'`, see the [default block parameter](#the-default-block-parameter)

#### returns

- `Quantity` - integer of the number of transactions send from this address

### vap_getTransactionReceipt

Returns the receipt of a transaction by transaction hash.
**Note** That the receipt is not available for pending transactions.

#### parameters

- `Hash` - hash of a transaction

#### returns

- `Object` - A transaction receipt object, or `null` when no receipt was found:
    - `blockHash`/`Hash` - 32 Bytes - hash of the block where this transaction was in.
    - `blockNumber`/`BlockNumber` - block number where this transaction was in.
    - `contractAddress`/`Address` - 20 Bytes - The contract address created, if the transaction was a contract creation, otherwise `null`.
    - `cumulativeGasUsed`/`Quantity` - The total amount of gas used when this transaction was executed in the block.
    - `gasUsed`/`Quantity` - The amount of gas used by this specific transaction alone.
    - `logs`/`Array` - Array of log objects, which this transaction generated.
    - `transactionHash`/`Hash` - 32 Bytes - hash of the transaction.
    - `transactionIndex`/`Quantity` - integer of the transactions index position in the block.

### vap_getUncleByBlockHashAndIndex

Returns information about a uncle of a block by hash and uncle index position.

#### parameters

- `Hash` - Hash a block
- `Quantity` - The uncle's index position

#### returns

See [vap_getBlockByHash](#vap_getblockbyhash)

### vap_getUncleByBlockNumberAndIndex

Returns information about a uncle of a block by number and uncle index position.

#### parameters

- `BlockNumber` - a block number, or the string `'earliest'`, `'latest'` or `'pending'`, as in the [default block parameter](#the-default-block-parameter)
- `Quantity` - The uncle's index position

#### returns

See [vap_getBlockByHash](#vap_getblockbyhash)

### vap_getUncleCountByBlockHash

Returns the number of uncles in a block from a block matching the given block hash.

#### parameters

- `Hash` - 32 Bytes - hash of a block

#### returns

- `Quantity` - integer of the number of uncles in this block

### vap_getUncleCountByBlockNumber

Returns the number of uncles in a block from a block matching the given block number.

#### parameters

- `BlockNumber` - integer of a block number, or the string 'latest', 'earliest' or 'pending', see the [default block parameter](#the-default-block-parameter)

#### returns

- `Quantity` - integer of the number of uncles in this block

### vap_getWork

Returns the hash of the current block, the seedHash, and the boundary condition to be met ('target').

#### parameters

none

#### returns

- `Array` - Array with the following properties:

### vap_hashrate

Returns the number of hashes per second that the node is mining with.

#### parameters

none

#### returns

- `Quantity` - number of hashes per second

### vap_inspectTransaction

?

#### parameters

?

#### returns

- `Boolean` - whether the call was successful

### vap_mining

Returns `true` if client is actively mining new blocks.

#### parameters

none

#### returns

- `Boolean` - `true` of the client is mining, otherwise `false`

### vap_newBlockFilter

Creates a filter in the node, to notify when a new block arrives.
To check if the state has changed, call [vap_getFilterChanges](#vap_getfilterchanges).

#### parameters

none

#### returns

- `Quantity` - A filter id

### vap_newFilter

Creates a filter object, based on filter options, to notify when the state changes (logs).
To check if the state has changed, call [vap_getFilterChanges](#vap_getfilterchanges).

#### parameters

none

#### returns

- `Object` - The filter options:
    - `address`/`Address` - (optional) 20 Bytes - Contract address or a list of addresses from which logs should originate.
    - `fromBlock`/`BlockNumber` - (optional) (default: latest) Integer block number, or `'latest'` for the last mined block or `'pending'`, `'earliest'` for not yet mined transactions.
    - `toBlock`/`BlockNumber` - (optional) (default: latest) Integer block number, or `'latest'` for the last mined block or `'pending'`, `'earliest'` for not yet mined transactions.
    - `topics`/`Array` - (optional) Array of 32 Bytes `DATA` topics. Topics are order-dependent. Each topic can also be an array of DATA with 'or' options.

### vap_newFilterEx

?

#### parameters

?

#### returns

- `Boolean` - whether the call was successful

### vap_newPendingTransactionFilter

Creates a filter in the node, to notify when new pending transactions arrive.
To check if the state has changed, call [vap_getFilterChanges](#vap_getfilterchanges).

#### parameters

none

#### returns

- `Quantity` - A filter id

### vap_notePassword

?

#### parameters

?

#### returns

- `Boolean` - whether the call was successful

### vap_pendingTransactions

?

#### parameters

none

#### returns

- `Boolean` - whether the call was successful

### vap_protocolVersion

Returns the current vapory protocol version.

#### parameters

none

#### returns

- `String` - The current vapory protocol version

### vap_register

?

#### parameters

?

#### returns

- `Boolean` - whether the call was successful

### vap_sendRawTransaction

Creates new message call transaction or a contract creation for signed transactions.

#### parameters

- `Data` - The signed transaction data

#### returns

- `Hash` - 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available

### vap_sendTransaction

Creates new message call transaction or a contract creation, if the data field contains code.

#### parameters

- `Object` - The transaction object
    - `data`/`Data` - The compiled code of a contract OR the hash of the invoked method signature and encoded parameters. For details see [Vapory Contract ABI](https://github.com/vaporyco/wiki/wiki/Vapory-Contract-ABI)
    - `from`/`Address` - 20 Bytes - The address the transaction is send from
    - `gas`/`Quantity` - (optional) (default: 90000) Integer of the gas provided for the transaction execution. It will return unused gas.
    - `gasPrice`/`Quantity` - (optional) (default: To-Be-Determined) Integer of the gasPrice used for each paid gas
    - `nonce`/`Quantity` - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
    - `to`/`Address` - 20 Bytes - (optional when creating new contract) The address the transaction is directed to
    - `value`/`Quantity` - (optional) Integer of the value send with this transaction

#### returns

- `Hash` - 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available

### vap_sign

Signs data with a given address.
**Note** the address to sign must be unlocked.

#### parameters

- `Address` - 20 Bytes - address
- `Data` - Data to sign

#### returns

- `Data` - Signed data

### vap_signTransaction

?

#### parameters

?

#### returns

- `Boolean` - whether the call was successful

### vap_submitHashrate

Used for submitting mining hashrate.

#### parameters

- `Data` - a hexadecimal string representation (32 bytes) of the hash rate
- `String` - A random hexadecimal(32 bytes) ID identifying the client

#### returns

- `Boolean` - `true` if submitting went through succesfully and `false` otherwise

### vap_submitWork

Used for submitting a proof-of-work solution.

#### parameters

- `Data` - 8 Bytes - The nonce found (64 bits)
- `Data` - 32 Bytes - The header's pow-hash (256 bits)
- `Data` - 32 Bytes - The mix digest (256 bits)

#### returns

- `Boolean` - `true` if the provided solution is valid, otherwise `false`

### vap_syncing

Returns an object with data about the sync status or `false`.

#### parameters

none

#### returns

- `Object` - An object with sync status data or `FALSE`, when not syncing
    - `currentBlock`/`Quantity` - The current block, same as vap_blockNumber
    - `highestBlock`/`Quantity` - The estimated highest block
    - `startingBlock`/`Quantity` - The block at which the import started (will only be reset, after the sync reached his head)

### vap_uninstallFilter

Uninstalls a filter with given id. Should always be called when watch is no longer needed.
Additonally Filters timeout when they aren't requested with [vap_getFilterChanges](#vap_getfilterchanges) for a period of time.

#### parameters

- `Quantity` - The filter id

#### returns

- `Boolean` - `true` if the filter was successfully uninstalled, otherwise `false`

### vap_unregister

?

#### parameters

?

#### returns

- `Boolean` - whether the call was successful


## vapcore

- [vapcore_acceptNonReservedPeers](#vapcore_acceptNonReservedPeers)
- [vapcore_addReservedPeer](#vapcore_addReservedPeer)
- [vapcore_defaultExtraData](#vapcore_defaultExtraData)
- [vapcore_devLogs](#vapcore_devLogs)
- [vapcore_devLogsLevels](#vapcore_devLogsLevels)
- [vapcore_dropNonReservedPeers](#vapcore_dropNonReservedPeers)
- [vapcore_extraData](#vapcore_extraData)
- [vapcore_gasFloorTarget](#vapcore_gasFloorTarget)
- [vapcore_minGasPrice](#vapcore_minGasPrice)
- [vapcore_netChain](#vapcore_netChain)
- [vapcore_netMaxPeers](#vapcore_netMaxPeers)
- [vapcore_netPort](#vapcore_netPort)
- [vapcore_nodeName](#vapcore_nodeName)
- [vapcore_removeReservedPeer](#vapcore_removeReservedPeer)
- [vapcore_rpcSettings](#vapcore_rpcSettings)
- [vapcore_setAuthor](#vapcore_setAuthor)
- [vapcore_setExtraData](#vapcore_setExtraData)
- [vapcore_setGasFloorTarget](#vapcore_setGasFloorTarget)
- [vapcore_setMinGasPrice](#vapcore_setMinGasPrice)
- [vapcore_setTransactionsLimit](#vapcore_setTransactionsLimit)
- [vapcore_transactionsLimit](#vapcore_transactionsLimit)
- [vapcore_unsignedTransactionsCount](#vapcore_unsignedTransactionsCount)

### vapcore_acceptNonReservedPeers

?

#### parameters

none

#### returns

- `Boolean` - ?

### vapcore_addReservedPeer

?

#### parameters

- `String` - Enode

#### returns

- `Boolean` - ?

### vapcore_defaultExtraData

Returns the default extra data

#### parameters

none

#### returns

- `Data` - Extra data

### vapcore_devLogs

Returns latest logs of your node

#### parameters

none

#### returns

- `Array` - Development logs

### vapcore_devLogsLevels

Returns current log level settings

#### parameters

none

#### returns

- `String` - undefined

### vapcore_dropNonReservedPeers

?

#### parameters

none

#### returns

- `Boolean` - ?

### vapcore_extraData

Returns currently set extra data

#### parameters

none

#### returns

- `Data` - Extra data

### vapcore_gasFloorTarget

Returns current target for gas floor

#### parameters

none

#### returns

- `Quantity` - Gas Floor Target

### vapcore_minGasPrice

Returns currently set minimal gas price

#### parameters

none

#### returns

- `Quantity` - Minimal Gas Price

### vapcore_netChain

Returns the name of the connected chain.

#### parameters

none

#### returns

- `String` - chain name

### vapcore_netMaxPeers

Returns maximal number of peers.

#### parameters

none

#### returns

- `Quantity` - Maximal number of peers

### vapcore_netPort

Returns network port the node is listening on.

#### parameters

none

#### returns

- `Quantity` - Port Number

### vapcore_nodeName

Returns node name (identity)

#### parameters

none

#### returns

- `String` - Node name

### vapcore_removeReservedPeer

?

#### parameters

- `String` - Encode

#### returns

- `Boolean` - ?

### vapcore_rpcSettings

Returns basic settings of rpc (enabled, port, interface).

#### parameters

none

#### returns

- `Object` - JSON object containing rpc settings

### vapcore_setAuthor

Changes author (coinbase) for mined blocks.

#### parameters

- `Address` - 20 Bytes - Address

#### returns

- `Boolean` - whether the call was successful

### vapcore_setExtraData

Changes extra data for newly mined blocks

#### parameters

- `Data` - Extra Data

#### returns

- `Boolean` - whether the call was successful

### vapcore_setGasFloorTarget

Changes current gas floor target.

#### parameters

- `Quantity` - Gas Floor Target

#### returns

- `Boolean` - whether the call was successful

### vapcore_setMinGasPrice

Changes minimal gas price for transaction to be accepted to the queue.

#### parameters

- `Quantity` - Minimal gas price

#### returns

- `Boolean` - whether the call was successful

### vapcore_setTransactionsLimit

Changes limit for transactions in queue.

#### parameters

- `Quantity` - New Limit

#### returns

- `Boolean` - whether the call was successful

### vapcore_transactionsLimit

Changes limit for transactions in queue.

#### parameters

none

#### returns

- `Quantity` - Current max number of transactions in queue

### vapcore_unsignedTransactionsCount

Returns number of unsigned transactions when running with Trusted Signer. Error otherwise

#### parameters

none

#### returns

- `Quantity` - Number of unsigned transactions


## web3

- [web3_clientVersion](#web3_clientVersion)
- [web3_sha3](#web3_sha3)

### web3_clientVersion

Returns the current client version.

#### parameters

none

#### returns

- `String` - The current client version

### web3_sha3

Returns Keccak-256 (*not* the standardized SHA3-256) of the given data.

#### parameters

- `String` - The data to convert into a SHA3 hash

#### returns

- `Data` - The SHA3 result of the given string

