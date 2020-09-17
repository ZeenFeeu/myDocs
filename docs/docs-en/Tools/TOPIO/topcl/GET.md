# Get Information From the Blockchain

## Overview

The get command is used to query the information on the chain, including the account information on the chain, block information, master chain information, account transaction details.

## Get Commands

Execute `get -h` or `get --help` view get commands.

```
COMMANDS:
    account                         Get account information on chain.
    block                           Get block information based on block type and block height.
    chaininfo                       Get chain informaion.
    transaction                     Get transaction details on chain.

OPTIONS:
    -h,--help                       Show a list of commands or help for one command.
```

Execute `get account -h` or` get account --help` view help for subcommand `get account`.

```
Get account info on chain.

USAGE:
    get account [account_addr]

OPTIONS:
    -h --help                       Show help information for one command.

EXAMPLE:
    get account
    get account T-0-LQB6umTo6TZs9UjmZEgwpd7Jt3R7hGhYCS
```

## Command Instructions

### The Premise

（1）You need an account on the blockchain.You can create an account on the blockchain via transferring TOP token to the local account through an account with balance on the blockchain.

（2）The system uses the newest created account to send transactions.If restart the TOPIO，you need to reset default account to send transactions via `wallet setDefault`.

### Get Account 

**Request**

`get account`

**Request Parameters**

| Parameter Name | Required | Default Value           | Parameter Type | Description                                                  |
| -------------- | -------- | ----------------------- | -------------- | ------------------------------------------------------------ |
| account_addr   | No       | The account being used. | String         | The account address to be queried,the default is current account in use. |

**Options**

| Option Name | Default Value | Type | Description           |
| ----------- | ------------- | ---- | --------------------- |
| -h,--help   | -             | -    | Help for the command. |

**Response Parameters**

Caution:

> In TOPIO, account balance, transaction deposit,etc.The unit is uTOP, 1TOP=1*10^6 uTOP.

| Parameters Name         | Parameter  Type | Description                                                  |
| ----------------------- | --------------- | ------------------------------------------------------------ |
| account_addr            | String          | Normal user account address or contract account address.     |
| available_gas           | Uint64          | The available gas of the account address.The unit is Tgas.   |
| balance                 | Uint64          | The balance of the account address.The unit is uTOP.         |
| burned_token            | Uint64          | All burned TOP tokens of the account address.The unit is uTOP. |
| cluster_id              | Uint8           | cluster ID。                                                 |
| created_time            | Uint64          | The clock height when the account created on the blockchain. |
| disk_staked_token       | Uint64          | The amount of locked TOP tokens to exchanging disk.The unit is uTOP. |
| gas_staked_token        | Uint64          | The amount of locked TOP tokens to exchanging gas.The unit is uTOP. |
| group_id                | Uint8           | group ID。                                                   |
| latest_tx_hash          | String          | The hash of the latest successful transaction.               |
| latest_tx_hash_xxhash64 | String          | The xx64hash of the latest successful transaction.           |
| latest_unit_height      | Uint64          | The unit block height of the latest successsful transaction. |
| lock_balance            | Uint64          | Locked TOP tokens，the unit is uTOP,used for user contract transactions mainly.<br/>When running applicaiton contract, the transaction sender can transfer TOP tokens to the contract account at the same time. If the contract fails to be executed, the transfer TOP tokens needs to be returned to the sender. Therefore, the transfer money should be locked before the successful execution of the contract. |
| lock_deposit_balance    | Uint64          | Application contract transaction costs are related to the CPU time and transaction size.The transaction costs of the application contract cannot be determined at the beginning of the transaction. The method adopted is to freeze part of the transaction deposit of the transaction sender. At the third consensus of the transaction, according to the final execution of the application contract, the transaction deposit of the sender is deducted to pay for the costs.The unit is uTOP. |
| lock_gas                | Uint64          | Application contract transaction costs are related to the CPU time and transaction size.The transaction costs of the application contract cannot be determined at the beginning of the transaction. The method adopted is to freeze part of the gas of the transaction sender. At the third consensus of the transaction, according to the final execution of the application contract, the gas of the sender is deducted to pay for the costs.The unit is uTOP. |
| nonce                   | Uint64          | The nonce of the latest successful transaction.Unique in the global network. |
| total_gas               | Uint64          | Total gas of the account address.The unit is Tgas.           |
| unlock_disk_staked      | Uint64          | TOP tokens to exchange disk in unlock.After initiating the unlock, we need to wait 24 hours for the unlocked amount to arrive in the account. |
| unlock_gas_staked       | Uint64          | TOP tokens to exchange gas in unlock.After initiating the unlock, we need to wait 24 hours for the unlocked amount to arrive in the account. |
| unused_vote_amount      | Uint64          | Unused vote amount of the account.                           |
| vote_staked_token       | Uint64          | TOP tokens to exchange votes in unlock.                      |
| zone_id                 | Uint8           | zone ID。                                                    |

**Request Sample**

`get account`

**Response Schema**

* Successful

```
{
   "data" : {
      "account_addr" : "T-0-LaN5M2j2p4neS4TsN6WAjo6vf1yKrUoKtv",
      "available_gas" : 25000,
      "balance" : 100000000000000,
      "burned_token" : 0,
      "cluster_id" : 1,
      "created_time" : 1596520429,
      "disk_staked_token" : 0,
      "group_id" : 64,
      "gas_staked_token" : 0,
      "latest_tx_hash" : "0xfcd8843c36b1c8fee81bcac7e7cf2b38682deef723e9a237918b70b3a6dfc4c9",
      "latest_tx_hash_xxhash64" : "0xdb73d04d0f5daa84",
      "latest_unit_height" : 1,
      "lock_balance" : 0,
      "lock_deposit_balance" : 0,
      "lock_gas" : 0,
      "nonce" : 1,
      "total_gas" : 25000,
      "unlock_disk_staked" : 0,
      "unlock_gas_staked" : 0,
      "unused_vote_amount" : 0,
      "vote_staked_token" : 0,
      "zone_id" : 0
   },
   "errmsg" : "ok",
   "errno" : 0,
   "sequence_id" : "5"
}
```

* Failed

The account does not exit on the blockchain.

```
{
   "errmsg" : "account not found on chain",
   "errno" : 11,
   "sequence_id" : "38"
}
```

### Get Chaininfo

**Request**

`get chaininfo `

**Request Parameters**

None.

**Options**

| Option Name | Default Value | Type | Description           |
| ----------- | ------------- | ---- | --------------------- |
| -h,--help   | -             | -    | Help for the command. |

**Response Parameters**

| Parameter Name         | Parameter Type | Description                            |
| ---------------------- | -------------- | -------------------------------------- |
| first_timerblock_hash  | String         | Hash of the first clock block.         |
| first_timerblock_stamp | Uint64         | The first clock block generation time. |
| version                | String         | Mainchain version.                     |

**Request Sample**

`get chaininfo`

**Response Schema**

* Successful

```
 {
   "data" : {
      "first_timerblock_hash" : "c69eb7878e4f50f4b2db58956c841a33f354778c2ed86c5406675fbb27668d2e",
      "first_timerblock_stamp" : 946713601,
      "version" : "0.0.0.1"
   },
   "errmsg" : "ok",
   "errno" : 0,
   "sequence_id" : "3"
}
```

* Failed

None.

### Get Block By Account Address

Get block by account address.

The premise of querying block information is that the account has sent the transaction and the transaction was consensused successfully.

**Request**

`get block`

**Request Parameters**

| Parameter Name | Required | Default Value | Parameter Type | Description                                                  |
| -------------- | -------- | ------------- | -------------- | ------------------------------------------------------------ |
| account_addr   | Yes      | -             | String         | Use the normal account address to query the unit block，such as"T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT".<br/>Use the table block account address to query the unit block，such as"T-a-gRD2qVpp2S7UpjAsznRiRhbE1qNnhMbEDp@0". |
| height         | Yes      | -             | String/Uint64  | The latest block height(String) or the specific block height(Uint64). |

**Options**

| Option Name | Default Value | Type | Description           |
| ----------- | ------------- | ---- | --------------------- |
| -h,--help   | -             | -    | Help for the command. |

**Response Parameters**

* unit block

| Parameter Name |                     |                 |                     |  | Parameter Name | Description                                              |
| ---------- | ------------------- | --------------- | ------------------- | ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| body       |                     |                 |                     |                     | Object |                                                              |
|            | fullunit            |                 |                     |                     | Object | To save data storage space, every 21 lightunits are packaged as a fullunit, and the 21 lightunits are cleared. |
|            | lightunit           |                 |                     |                     | Object |                   |
|            |                     | lightunit_input |                     |                     | Object | Transaction information in lightunit. |
|            |                     |                 | txs          |  | Object | The transaction information packaged in this block,the structured is a map array, and the key of the map is a transaction hash, such as: 6e734fed40b907bb64d257968e6a46a79c4ca144088d330b674cc8b545350324 |
| | | |  | recv_tx_exec_status | Uint8 | Execution results of the transaction receiver: 1-- Successful; 2-- The second consensus fails or refuses to execute, which usually occurs when the contract transaction is executed. |
|            |                     |                 |                     | send_tx_lock_gas | Uint64 | In a application contract transaction, the gas that the transaction sender can pay for the transaction receiver. |
|            |                     |                 |                     | tx_exec_status | Uint8  | Sender's transaction consensus status, 1-- Success; 2 - fail. |
|            |                     |                 |                     | tx_consensus_phase | Uint8 | Transaction consensus phase:1--self；2--send；3--recv。<br/>enum_transaction_subtype_self      = 1,  // self operate<br/>enum_transaction_subtype_send      = 2,  // send to other account<br/>enum_transaction_subtype_recv      = 3,  // receive from other account<br/>enum_transaction_subtype_recv_ack  = 4(confirm),  // receive ack from other account |
|            |                     |                 |                     | used_tx_deposit | Uint32 | Consumed transaction deposit，the unit is uTOP。 |
|            |                     |                 |                     | used_disk | Uint32 | Consumed disk resource. |
|            |                     |                 |                     | used_gas | Uint32 | Consumed gas resource，the unit is Tgas。 |
|            |                     | lightunit_state |                     |                     | Object | Changes in status caused by transactions in the lightunit block. |
|            |                     |                 | balance_change |  | Uint64 | Balance change.                                    |
|            |                     |                 | native_property |  | Key-value Array | Account native property. If the property is null, it indicates that the property has not been set this time, and is equivalent to 0 in calculation. See "Account Native properties." |
|            |                     |                 | custom_property_key |  |  String Array  | Get he list of custom property names with changes,only  returning  key. |
| hash       |                     |                 |                     |                     | String | The hexadecimal string of this block hash. |
| header     |              |                 |                     |                     | Object |                                                  |
|  | auditor_xip | | | | String | Auditor leader node of this block(xip). |
|            | timerblock_height |                 |                     |  | Uint64 | Clock block height.               |
|            | validator           |                 |                     |                     | String | Validator leader node of this block. |
| | validator_xip | | | | String | Validator leader node of this block(xip). |
| height     |                     |                 |                     |  | Uint64 | Block height.                                         |
| owner      |                     |                 |                     |  | String | The unit block owner. |
| prev_hash  |                     |                 |                     |                     | String | The hexadecimal of the hash of the previous block.           |
| timestamp  |                     |                 |                     |                     | Uint64 | Block time stamp(GMT).                           |

Account Native Property：

XINLINE_CONSTEXPR char const * XPROPERTY_CONTRACT_CODE                   = "@1";<br/>
XINLINE_CONSTEXPR char const * XPROPERTY_GLOBAL_NAME                     = "@2";<br/>
XINLINE_CONSTEXPR char const * XPROPERTY_ALIAS_NAME                      = "@3";<br/>
XINLINE_CONSTEXPR char const * XPROPERTY_LOCK_TOKEN_KEY                  = "@4";<br/>
XINLINE_CONSTEXPR char const * XPROPERTY_LOCK_TOKEN_SUM_KEY              = "@5";<br/>
XINLINE_CONSTEXPR char const * XPROPERTY_AUTHORIZE_KEY                   = "@6";<br/>
XINLINE_CONSTEXPR char const * XPROPERTY_ACCOUNT_KEYS                    = "@7";<br/>
XINLINE_CONSTEXPR char const * XPROPERTY_ACCOUNT_VOTE_KEY                = "@8";<br/>
XINLINE_CONSTEXPR char const * XPROPERTY_ACCOUNT_TRANSFER_KEY            = "@9";<br/>
XINLINE_CONSTEXPR char const * XPROPERTY_ACCOUNT_DATA_KEY                = "@10";<br/>
XINLINE_CONSTEXPR char const * XPROPERTY_ACCOUNT_CONSENSUS_KEY           = "@11";<br/>
XINLINE_CONSTEXPR char const * XPORPERTY_PARENT_ACCOUNT_KEY              = "@12";<br/>
XINLINE_CONSTEXPR char const * XPORPERTY_SUB_ACCOUNT_KEY                 = "@13";<br/>
XINLINE_CONSTEXPR char const * XPORPERTY_CONTRACT_SUB_ACCOUNT_KEY        = "@14";<br/>
XINLINE_CONSTEXPR char const * XPORPERTY_CONTRACT_PARENT_ACCOUNT_KEY     = "@15";<br/>
XINLINE_CONSTEXPR const char * XPORPERTY_CONTRACT_NATIVE_TOKEN_KEY       = "@16";<br/>
XINLINE_CONSTEXPR const char * XPORPERTY_CONTRACT_NATIVE_TOKEN_OWERN_KEY = "@17";<br/>XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_VOTE_KEY                = "@18";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_VOTE_OUT_KEY            = "@19";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_VOTE_STAT_KEY           = "@20";<br/>XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_BEC_RESULT_KEY          = "@21";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_BEC_ZEC_RESULT_KEY    ="@22";<br/>XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_BEC_VERSION_KEY         = "@23";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_BEC_ZEC_VERSION_KEY     = "@23_1";<br/>XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_RECMD_RESULT_KEY        = "@24";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_EDGE_RESULT_KEY         = "@25";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_ARCHIVE_RESULT_KEY      = "@26";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_ZONE_POOL_KEY           = "@27";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_BEC_POOL_KEY            = "@28";<br/>XINLINE_CONSTEXPR const char* XPROPERTY_PLEDGE_TOKEN_gas_KEY            = "@29";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_USED_gas_KEY                    = "@30";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_FROZEN_gas_KEY                  = "@31";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_LAST_TX_HOUR_KEY                 = "@32";<br/>XINLINE_CONSTEXPR const char* XPROPERTY_PLEDGE_TOKEN_DISK_KEY            = "@33";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_USED_DISK_KEY                    = "@34";<br/>
XINLINE_CONSTEXPR const char* XPROPERTY_CONTRACT_gas_LIMIT_KEY          = "@37";<br/>
XINLINE_CONSTEXPR const char* XPORPERTY_CONTRACT_BLOCK_CONTENT_KEY       = "@40";<br/>

Property value calculation method:
"@30" : "3234333733"
Every two characters represents the ASCII hexadecimal of the corresponding integer, such that the hexadecimal ASCII of 2 is 0x32, so the actual value of @30 is 24373.

* table block

| Parameter Name |           |                   |                      | Parameter Type | Description                                                  |
| -------------- | --------- | ----------------- | -------------------- | -------------- | ------------------------------------------------------------ |
| tableblock     |           |                   |                      | Object         | The tableblock contains multiple units. When there is no new transaction on the blockchain for a long time, the latest height data of the tableblock is "null". |
|                | units     | lightunit_input   |                      | Object         | Tableblock stores information of unit blocks.                |
|                |           |                   | is_contract_create   | Bool           | Transactions created by contract.                            |
|                |           |                   | last_tx_nonce        | String         | Nonce of previous transaction.                               |
|                |           |                   | tx_sender_locked_gas | Uint64         | In a application contract transaction, the gas that the transaction sender can pay for the transaction receiver. |
|                |           |                   | tx_consensus_phase   | Uint8          | Transaction consensus phase:1（self）,2（send）,3（recv）<br/>enum_transaction_subtype_self      = 1,  // self operate<br/>enum_transaction_subtype_send      = 2,  // send to other account<br/>enum_transaction_subtype_recv      = 3,  // receive from other account<br/>enum_transaction_subtype_recv_ack  = 4(confirm),  // receive ack from other account |
|                |           | lightunit_state   |                      | Object         | Please refer to unit block parameter description.            |
|                |           | unit_height       |                      | Uint64         | Unit block height.                                           |
|                | hash      |                   |                      | String         | The hexadecimal string of this block hash.                   |
|                | header    |                   |                      | Object         |                                                              |
|                |           | auditor_xip       |                      | String         | Auditor leader node of this block(xip).                      |
|                |           | timerblock_height |                      | Uint64         | Clock block height.                                          |
|                |           | auditor           |                      | String         | Auditor leader node of this block.                           |
|                |           | validator_xip     |                      | String         | Validator leader node of this block(xip).                    |
|                | height    |                   |                      | Uint64         | Block height.                                                |
|                | owner     |                   |                      | String         | The table block owner.                                       |
|                | prev_hash |                   |                      | String         | The hexadecimal of the hash of the previous block.           |
|                | timestamp |                   |                      | Uint64         | Block time stamp(GMT).                                       |

**Request Sample**

* unit block

`get block T-0-LWG4vTgckM7dKodtfCRmhiUwQsTTKT7e2d latest`

* table block

`get block T-a-gRD2qVpp2S7UpjAsznRiRhbE1qNnhMbEDp@0 264`

**Response Schema**

* Successful

unit block

```
{
   "data" : {
      "value" : {
         "body" : {
            "fullunit" : null,
            "lightunit" : {
               "lightunit_input" : {
                  "txs" : [
                     {
                        "0xe31fbabe30ae6c06bf6f5fc54bec44295bf2efa749ac82b97b63565804383e42" : {
                           "recv_tx_exec_status" : 1,
                           "send_tx_lock_gas" : 0,
                           "tx_consensus_phase" : "confirm",
                           "tx_exec_status" : 1,
                           "used_disk" : 0,
                           "used_gas" : 0,
                           "used_tx_deposit" : 0
                        }
                     }
                  ]
               },
               "lightunit_state" : {
                  "balance_change" : 0,
                  "burned_amount_change" : 0,
                  "custom_property_key" : null,
                  "native_property" : null
               }
            }
         },
         "hash" : "41098cec1fd53036f2a3ef3e38d97427b4f30bc49a5771e684fc7277fa2749c2",
         "header" : {
            "auditor_xip" : "100000000000001:f6000000000407ff",
            "timerblock_height" : 7929,
            "validator" : "T-0-LXRSDkzrUsseZmfJFnSSBsgm754XwV9SLw",
            "validator" : "100000000000001:f600000000050003"
         },
         "height" : 3,
         "owner" : "T-0-LKXjgwdL9bTwADL89cBp7L2ze3wqiNmRB4",
         "prev_hash" : "ba754df1f36e328462b98d175f4e143308c7ba307605ba947b6f21a17b4c337d",
         "timestamp" : 1594882650
      }
   },
   "errmsg" : "ok",
   "errno" : 0,
   "sequence_id" : "9"
}
```

table block

```
 {
   "data" : {
      "value" : {
         "body" : {
            "tableblock" : {
               "units" : {
                  "T-0-LKXjgwdL9bTwADL89cBp7L2ze3wqiNmRB4" : {
                     "lightunit_input" : {
                        "0xe31fbabe30ae6c06bf6f5fc54bec44295bf2efa749ac82b97b63565804383e42" : {
                           "is_contract_create" : 0,
                           "last_tx_nonce" : 0,
                           "sender_tx_lock_gas" : 0,
                           "tx_consensus_phase" : "confirm"
                        }
                     },
                     "lightunit_state" : {
                        "balance_change" : 0,
                        "burned_amount_change" : 0,
                        "custom_property_key" : null,
                        "native_property" : null
                     },
                     "unit_height" : 3
                  }
               }
            }
         },
         "hash" : "f260ae40f5ea129a7f22b36b6abe8e46bb0058114af7390d54261e0db513e462",
         "header" : {
            "auditor_xip" : "100000000000001:f6000000000407ff",
            "timerblock_height" : 7929,
            "validator" : "T-0-LXRSDkzrUsseZmfJFnSSBsgm754XwV9SLw",
            "validator_xip" : "100000000000001:f600000000050003"
         },
         "height" : 9,
         "owner" : "T-a-gRD2qVpp2S7UpjAsznRiRhbE1qNnhMbEDp@146",
         "prev_hash" : "1d69844c0efec8fb56df7bb38ff9080251fb2b3fd62e3d6a961779ca3e3820d4",
         "table_id" : 146,
         "timestamp" : 1594882650
      }
   },
   "errmsg" : "ok",
   "errno" : 0,
   "sequence_id" : "17"
}
```

* Failed

The account error or the account does not exit on the blockchain.

```
{
   "data" : {
      "value" : null
   },
   "errmsg" : "ok",
   "errno" : 0,
   "sequence_id" : "19"
}
```

### Get Transaction

**Request**

`get transaction`

**Request Parameters**

| Parameter Name | Required | Default Value | Parameter Type | Descripiton                                                  |
| -------------- | -------- | ------------- | -------------- | ------------------------------------------------------------ |
| account_addr   | 是       | -             | String         | Transacton sender's account address or receiver's account address. |
| tx_hash        | 是       | -             | String         | The latest transaction hash of the account which can be queried via `get account`. |

**Options**

| Option Name | Default Name | Type | Description           |
| ----------- | ------------ | ---- | --------------------- |
| -h,--help   | -            | -    | Help for the command. |

**Response Parameters**

Please refer to “[Transaction Data Structure](#Transaction Data Structure)”。

**Request Sample**

```
get transaction T-0-LVb72cJ9LzaQbdR41Zvx7dsAL1oDFFGQrJ 0x8aa1e7082af07bf22840a1526745c484a5a20115d8e92cff2d9ed413128ac2b4
```

**Response Schema**

* Successful

Caution：

> * The transaction is a single-account transaction, there is only one round of consensus under the transaction sender. Only the information of "confirm_unit_info" is returned in the result.
> * The transaction is an across-account transaction, there are three rounds of consensuse in total. The information of the three consensus, including "confirm_unit_info (the second round of consensus under the sender) ", "recv_unit_info(consensus under the receiver)" and "send_unit_info" (the first round of consensus under the sender) are returned in the result. 

```
{
   "data" : {
      "original_tx_info" : {
         "authorization" : "0x005d19e04e77e99a0b9c029b0a247fe30009b3cc543db18bdb503b37e7d0788d50530437ef65583da62454bb4eafe2cf5e03952134a2ba08e84565e3d8e0aa893e",
         "challenge_proof" : "",
         "ext" : "",
         "from_ledger_id" : 0,
         "last_tx_hash" : 1,
         "last_tx_nonce" : 15813211746016799364,
         "note" : "",
         "send_timestamp" : 1596523599,
         "to_ledger_id" : 0,
         "tx_action" : {
            "receiver_action" : {
               "action_authorization" : "",
               "action_ext" : "",
               "action_hash" : 0,
               "action_name" : "",
               "action_param" : "0x000000001027000000000000",
               "action_size" : 78,
               "action_type" : 6,
               "tx_receiver_account_addr" : "T-0-LRyJeU1r4GqPTAmKEgZ2t1mGBj9DK2gWu2"
            },
            "sender_action" : {
               "action_authorization" : "",
               "action_ext" : "",
               "action_hash" : 0,
               "action_name" : "",
               "action_param" : "0x000000001027000000000000",
               "action_size" : 78,
               "action_type" : 0,
               "tx_sender_account_addr" : "T-0-LaN5M2j2p4neS4TsN6WAjo6vf1yKrUoKtv"
            }
         },
         "tx_deposit" : 100000,
         "tx_expire_duration" : 100,
         "tx_hash" : "0x0e4bcb020f2fdf6ed4105385a2d564b6ae33f7ae8d85563d471c7240713d8c5b",
         "tx_len" : 315,
         "tx_random_nonce" : 0,
         "tx_structure_version" : 0,
         "tx_type" : 4
      },
      "tx_consensus_state" : {
         "confirm_unit_info" : {
            "exec_status" : "success",
            "height" : 3,
            "recv_tx_exec_status" : "success",
            "tx_exec_status" : "success",
            "unit_hash" : "bd47732e8d959846c0302ab3582632e6c11e19c1d30f6578213fc4342de95b01",
            "used_deposit" : 0,
            "used_disk" : 0,
            "used_gas" : 0
         },
         "recv_unit_info" : {
            "height" : 1,
            "unit_hash" : "f75ea49c50f4fe151931bc782468e0243880994393f0a8c30e8179597aaa5389",
            "used_deposit" : 0,
            "used_disk" : 0,
            "used_gas" : 0
         },
         "send_unit_info" : {
            "height" : 2,
            “tx_fee" : 0，
            "unit_hash" : "80e4029f0ce8c2861ac89b5b9394ced6cf80161ebc899d22cb6d1a4afc8616b9",
            "used_deposit" : 0,
            "used_disk" : 0,
            "used_gas" : 774
         }
      }
   },
   "errmsg" : "ok",
   "errno" : 0,
   "sequence_id" : "8"
}
```

Determine whether the transaction is successful or not according to the parameter "exec_status":

1. When the value of exec_status is "success", it proves that the transaction is finally successful.

2. When the value of exec_status is "failure", the transaction fails. At this time, the value of recv_tx_exec_status is "failure", indicating that the consensus under the receiver is failed.

提醒：

> If the above three parameters do not appear in the results, which proves that the transaction consensus is not completely finished at this time, please query again later.

* Failed

Hash error.

```
{
   "errmsg" : "0x38a14c80a6621ef67dc3a3311f90555bf722eeb0bac316c21261a70fe6f5e4 length is not correct",
   "errno" : 3,
   "sequence_id" : "21"
}
```

## Transaction Data Structure

| Parameter Name |  |         |                      | Parameter Name | Description                      |
| ------------------- | -------------------- | ------ | ------------------------------------ | ------------------- | ------------------- |
| original_tx_info |  |  | | Object | Original transaction information. |
|        | authorization |        |                      | String | Transaction body signature. |
|  | challenge_proof |  | | String | The reserve parameter, default to empty string. |
|  | ext |  | | String | The reserve parameter, default to empty string. |
| | from_ledger_id | | | Uint16 | The reserve parameter, default to "0". |
|      | last_tx_hash |  |                      | String | The hash of the previous transaction ,used for transaction sorting and removing duplicates. |
|     | last_tx_nonce |  |                      | Uint64 | The nonce of the previous transaction ,used for transaction sorting and removing duplicates. |
| | note | | | String | Transaction note. |
|  | send_timestamp |  | | Uint64 | Transaction send timestamp. |
|  | to_ledger_id |  | | Uint16 | The reserve parameter, default to "0". |
|  | tx_action |  | | Object | Transaction action，including "source_action" and "target_action"。 |
|        |        | receiver_action |                      | Object | Transaction receiver action. |
|                     |                     |                     | action_authorization | String | Action signature, JSON structure. When the transaction is application contract deployment, the public key of the contract is displayed here. The public key is used to verify whether the contract account matches the account of the transaction sender. |
|                     |                     |                     | action_ext           | String | The reserve parameter, default to empty string. |
|                     |                     |                     | action_hash          | Uint32 | xxhash32 of the action.Default to "0".Temporary unused. |
|                     |                     |                     | action_name          | String | The name of contract function.<br/>The system smart contract function,please refer to [System Smart Contract API](docs-en/Smart Contract/SystemContractAPI.md). |
|                     |                     |                     | action_param         | String | The transaction receiver action.The serialization of different action perform content please refer to [Action Param Serialization](docs- en/Interface/RPC-API/sendTransaction/action-param-serialization.md). |
|                     |                     |                     | action_size          | Uint16 | Action size. |
|                     |                     |                     | action_type          | Uint16 | Different transaction type correspond to different receiver action type, please refer to [TX Type and Action Type](docs- en/Interface/RPC-API/sendTransaction/tx-type-and-action-type.md).<br/>xaction_type_asset_out                = 0,  <br/>xaction_type_create_contract_account    = 3,    <br/>xaction_type_run_contract              = 5,    <br/>xaction_type_asset_in                = 6,    <br/>xaction_type_pledge_token_vote          = 21,   <br/>xaction_type_redeem_token_vote          = 22,<br/>xaction_type_pledge_token               = 23,   <br/>xaction_type_redeem_token               = 24, |
| | | | tx_receiver_account_addr | String | Transaction receiver account address. |
|                     |                     | sender_action |                      | Object | Transaction sender action. |
|                     |                     |                     | action_authorization | String | Action signature, JSON structure. |
|                     |                     |                     | action_ext           | String | The reserve parameter, default to empty string. |
|                     |                     |                     | action_hash          | Uint32 | xxhash32 of the action.Default to "0".Temporary unused. |
|                     |                     |                     | action_name          | String | The reserve parameter, default to empty string. |
|                     |                     |                     | action_param         | String | The transaction sender action.The serialization of different action perform content please refer to [Action Param Serialization](docs- en/Interface/RPC-API/sendTransaction/action-param-serialization.md). |
|                     |                     |                     | action_size          | Uint16 | Action size. |
|                     |                     |                     | action_type          | Uint16 | Different transaction types correspond to different sender action types, please refer to [TX Type and Action Type](docs- en/Interface/RPC-API/sendTransaction/tx-type-and-action-type.md).<br/>xaction_type_asset_out                  = 0,    <br/>xaction_type_source_null =1, |
| | | | tx_sender_account_addr | String | Transaction sender account address. |
| | tx_deposit | |  | Uint32 | Transaction deposit.The unit is uTOP. |
| | tx_expire_duration | |  | Uint16 | If the transaction expires, it will be discarded with the default time of 100s. |
| | tx_hash | |  | String | The hexadecimal of the transaction hash. |
| | tx_len | |  | Uint16 | Transaction size. |
| | tx_random_nonce | |  | Uint32 | Random nonce.Default to "0".Temporary unused. |
| | tx_structure_version | |  | String | Transaction structure version.Default to "0".Temporary unused. |
|     | tx_type |     |                      | Uint16 | Transaction type.<br/>Different transaction types have different action param and action type in action.<br/>xtransaction_type_create_contract_account      = 1, <br/>xtransaction_type_run_contract                           = 3,<br/>xtransaction_type_transfer                                   = 4,<br/>xtransaction_type_vote                                             = 20, <br/>xtransaction_type_abolish_vote                               = 21,<br/>xtransaction_type_pledge_token_gas                      = 22,  <br/>xtransaction_type_redeem_token_gas                    = 23,   <br/>xtransaction_type_pledge_token_vote                     = 27,<br/>xtransaction_type_redeem_token_vote                    = 28, |
| tx_consensus_state |  |  | | Object | Transaction consensus status. |
|  | confirm_unit_info |  | | Object | Unit block generated in the third round of consensus. |
|  |  | exec_status | | String | Final consensus status of the transaction:success or failure. |
|  |  | height | | Uint64 | The height of the unit block which generated by the third round of consensus. |
|  |  | recv_tx_exec_status | | String | Consensus status of transaction receiver:success/failure.<br/>Fail or refuse to consensus, usually occurs in contract transaction. |
|  |  | tx_exec_status | | String | Consensus status of transaction sender:success/failure. |
|  |  | unit_hash | | String | The hash of the unit block which generated in the third round of consensus. |
|  |  | used_deposit | | Uint32 | After the third round of consensus, the transaction deposit of the sender's account will be deducted if the gas is not enough. The unit is uTOP. |
|  |  | used_disk | | Uint32 | Default to "0". |
|  |  | used_gas | | Uint32 | Deducted gas after the third round of consensus (application contract transaction).The unit is Tgas. |
|  | recv_unit_info |  | | Object | Unit block generated in the second round of consensus. |
|  |  | height | | Uint64 | The height of the unit block which generated in the second round of consensus. |
|  |  | unit_hash | | String | The hash of the unit block which generated in the second round of consensus. |
|  |  | used_deposit | | Uint32 | Default to "0". |
|  |  | used_disk | | Uint32 | Default to "0". |
|  |  | used_gas | | Uint32 | Deducted gas after the second round of consensus.The unit is Tgas. |
|  | send_unit_info |  | | Object | Unit block generated in the round of consensus. |
|  |  | height | | Uint64 | The height of the unit block which generated in the first round of consensus. |
| | | tx_fee | | Uint64 | The system will automatically deduct 100*10^6 uTOP Token from the transaction sender's account as the transaction fee for running Beacon system contract transaction (registration node related, proposal related, starting node process) and destroy it. |
|  |  | unit_hash | | String | The hash of the unit block which generated in the first round of consensus. |
|  |  | used_deposit | | Uint32 | Default to "0". |
|  |  | used_disk | | Uint32 | Default to "0". |
|  |  | used_gas | | Uint32 | Deducted gas after the first round of consensus.The unit is Tgas. |

