# 查询接口说明

查询接口提供查询链上信息查询功能，具体如下表所示。

| 方法名                                | 说明                             |
| ------------------------------------- | -------------------------------- |
| [getTransaction](#查询账户交易信息)   | 查询账户交易信息。               |
| [getAccount](#查询链上账户信息)       | 查询链上账户信息。               |
| [getBlock](#查询区块信息)             | 根据账户及区块高度查询区块信息。 |
| [getChainInfo](#查询主链信息)         | 查询主链信息。                   |
| [queryNodeInfo](#查询节点信息)        | 查询节点信息。                   |
| [queryNodeReward](#查询节点奖励)      | 查询节点奖励。                   |
| [listVoteUsed](#查询节点投票分布)     | 查询节点投票分布。               |
| [queryVoterDividend](#查询投票者分红) | 查询投票者分红。                 |
| [queryProposal](#查询提案信息)        | 查询提案信息。                   |

## 接口使用说明

### 使用前提

查询链上账户信息、查询区块信息、查询账户交易详情，请确保您已经拥有一个链上账户。

### 查询账户交易信息

**请求方式**

`getTransaction`

**请求参数**

| 参数名称     | 是否必选 | 默认值 | 类型   | 说明                                   |
| ------------ | -------- | ------ | ------ | -------------------------------------- |
| account_addr | 是       | -      | String | 发送交易或接受交易账户地址。           |
| tx_hash      | 是       | -      | String | 账户交易hash，可用于查询交易详细信息。 |

**返回参数**

| Parameter Name     |                      |                     |                          | Parameter Name | Description                                                  |
| ------------------ | -------------------- | ------------------- | ------------------------ | -------------- | ------------------------------------------------------------ |
| original_tx_info   |                      |                     |                          | Object         | Original transaction information.                            |
|                    | authorization        |                     |                          | String         | Transaction body signature.                                  |
|                    | challenge_proof      |                     |                          | String         | The reserve parameter, default to empty string.              |
|                    | ext                  |                     |                          | String         | The reserve parameter, default to empty string.              |
|                    | from_ledger_id       |                     |                          | Uint16         | The reserve parameter, default to "0".                       |
|                    | last_tx_hash         |                     |                          | String         | The hash of the previous transaction ,used for transaction sorting and removing duplicates. |
|                    | last_tx_nonce        |                     |                          | Uint64         | The nonce of the previous transaction ,used for transaction sorting and removing duplicates. |
|                    | note                 |                     |                          | String         | Transaction note.                                            |
|                    | send_timestamp       |                     |                          | Uint64         | Transaction send timestamp.                                  |
|                    | to_ledger_id         |                     |                          | Uint16         | The reserve parameter, default to "0".                       |
|                    | tx_action            |                     |                          | Object         | Transaction action，including "source_action" and "target_action"。 |
|                    |                      | receiver_action     |                          | Object         | Transaction receiver action.                                 |
|                    |                      |                     | action_authorization     | String         | Action signature, JSON structure. When the transaction is application contract deployment, the public key of the contract is displayed here. The public key is used to verify whether the contract account matches the account of the transaction sender. |
|                    |                      |                     | action_ext               | String         | The reserve parameter, default to empty string.              |
|                    |                      |                     | action_hash              | Uint32         | xxhash32 of the action.Default to "0".Temporary unused.      |
|                    |                      |                     | action_name              | String         | The name of contract function.<br/>The system smart contract function,please refer to [System Smart Contract API](docs-en/Smart Contract/SystemContractAPI.md). |
|                    |                      |                     | action_param             | String         | The transaction receiver action.The serialization of different action perform content please refer to [Action Param Serialization](docs- en/Interface/RPC-API/sendTransaction/action-param-serialization.md). |
|                    |                      |                     | action_size              | Uint16         | Action size.                                                 |
|                    |                      |                     | action_type              | Uint16         | Different transaction type correspond to different receiver action type, please refer to [TX Type and Action Type](docs- en/Interface/RPC-API/sendTransaction/tx-type-and-action-type.md).<br/>xaction_type_asset_out                = 0,  <br/>xaction_type_create_contract_account    = 3,    <br/>xaction_type_run_contract              = 5,    <br/>xaction_type_asset_in                = 6,    <br/>xaction_type_pledge_token_vote          = 21,   <br/>xaction_type_redeem_token_vote          = 22,<br/>xaction_type_pledge_token               = 23,   <br/>xaction_type_redeem_token               = 24, |
|                    |                      |                     | tx_receiver_account_addr | String         | Transaction receiver account address.                        |
|                    |                      | sender_action       |                          | Object         | Transaction sender action.                                   |
|                    |                      |                     | action_authorization     | String         | Action signature, JSON structure.                            |
|                    |                      |                     | action_ext               | String         | The reserve parameter, default to empty string.              |
|                    |                      |                     | action_hash              | Uint32         | xxhash32 of the action.Default to "0".Temporary unused.      |
|                    |                      |                     | action_name              | String         | The reserve parameter, default to empty string.              |
|                    |                      |                     | action_param             | String         | The transaction sender action.The serialization of different action perform content please refer to [Action Param Serialization](docs- en/Interface/RPC-API/sendTransaction/action-param-serialization.md). |
|                    |                      |                     | action_size              | Uint16         | Action size.                                                 |
|                    |                      |                     | action_type              | Uint16         | Different transaction types correspond to different sender action types, please refer to [TX Type and Action Type](docs- en/Interface/RPC-API/sendTransaction/tx-type-and-action-type.md).<br/>xaction_type_asset_out                  = 0,    <br/>xaction_type_source_null =1, |
|                    |                      |                     | tx_sender_account_addr   | String         | Transaction sender account address.                          |
|                    | tx_deposit           |                     |                          | Uint32         | Transaction deposit.The unit is uTOP.                        |
|                    | tx_expire_duration   |                     |                          | Uint16         | If the transaction expires, it will be discarded with the default time of 100s. |
|                    | tx_hash              |                     |                          | String         | The hexadecimal of the transaction hash.                     |
|                    | tx_len               |                     |                          | Uint16         | Transaction size.                                            |
|                    | tx_random_nonce      |                     |                          | Uint32         | Random nonce.Default to "0".Temporary unused.                |
|                    | tx_structure_version |                     |                          | String         | Transaction structure version.Default to "0".Temporary unused. |
|                    | tx_type              |                     |                          | Uint16         | Transaction type.<br/>Different transaction types have different action param and action type in action.<br/>xtransaction_type_create_contract_account      = 1, <br/>xtransaction_type_run_contract                           = 3,<br/>xtransaction_type_transfer                                   = 4,<br/>xtransaction_type_vote                                             = 20, <br/>xtransaction_type_abolish_vote                               = 21,<br/>xtransaction_type_pledge_token_gas                      = 22,  <br/>xtransaction_type_redeem_token_gas                    = 23,   <br/>xtransaction_type_pledge_token_vote                     = 27,<br/>xtransaction_type_redeem_token_vote                    = 28, |
| tx_consensus_state |                      |                     |                          | Object         | Transaction consensus status.                                |
|                    | confirm_unit_info    |                     |                          | Object         | Unit block generated in the third round of consensus.        |
|                    |                      | exec_status         |                          | String         | Final consensus status of the transaction:success or failure. |
|                    |                      | height              |                          | Uint64         | The height of the unit block which generated by the third round of consensus. |
|                    |                      | recv_tx_exec_status |                          | String         | Consensus status of transaction receiver:success/failure.<br/>Fail or refuse to consensus, usually occurs in contract transaction. |
|                    |                      | tx_exec_status      |                          | String         | Consensus status of transaction sender:success/failure.      |
|                    |                      | unit_hash           |                          | String         | The hash of the unit block which generated in the third round of consensus. |
|                    |                      | used_deposit        |                          | Uint32         | After the third round of consensus, the transaction deposit of the sender's account will be deducted if the gas is not enough. The unit is uTOP. |
|                    |                      | used_disk           |                          | Uint32         | Default to "0".                                              |
|                    |                      | used_gas            |                          | Uint32         | Deducted gas after the third round of consensus (application contract transaction).The unit is Tgas. |
|                    | recv_unit_info       |                     |                          | Object         | Unit block generated in the second round of consensus.       |
|                    |                      | height              |                          | Uint64         | The height of the unit block which generated in the second round of consensus. |
|                    |                      | unit_hash           |                          | String         | The hash of the unit block which generated in the second round of consensus. |
|                    |                      | used_deposit        |                          | Uint32         | Default to "0".                                              |
|                    |                      | used_disk           |                          | Uint32         | Default to "0".                                              |
|                    |                      | used_gas            |                          | Uint32         | Deducted gas after the second round of consensus.The unit is Tgas. |
|                    | send_unit_info       |                     |                          | Object         | Unit block generated in the round of consensus.              |
|                    |                      | height              |                          | Uint64         | The height of the unit block which generated in the first round of consensus. |
|                    |                      | tx_fee              |                          | Uint64         | The system will automatically deduct 100*10^6 uTOP Token from the transaction sender's account as the transaction fee for running Beacon system contract transaction (registration node related, proposal related, starting node process) and destroy it. |
|                    |                      | unit_hash           |                          | String         | The hash of the unit block which generated in the first round of consensus. |
|                    |                      | used_deposit        |                          | Uint32         | Default to "0".                                              |
|                    |                      | used_disk           |                          | Uint32         | Default to "0".                                              |
|                    |                      | used_gas            |                          | Uint32         | Deducted gas after the first round of consensus.The unit is Tgas. |

**请求样例**

```
account_address=T-0-LVb72cJ9LzaQbdR41Zvx7dsAL1oDFFGQrJ&
body={
"params" : {
    "account_addr" : "T-0-LVb72cJ9LzaQbdR41Zvx7dsAL1oDFFGQrJ",
    "tx_hash" : "0x8aa1e7082af07bf22840a1526745c484a5a20115d8e92cff2d9ed413128ac2b4" 
    }
}&
method=getTransaction&
sequence_id=9&
identity_token=&
version=1.0
```

**返回样例**

* 成功返回

说明：

> * 交易为单账户交易，交易只在交易发送方下进行一次共识，查询交易最终返回结果中只有"confirm_unit_info"的信息。
>
> * 交易为跨账户交易，交易总共需要进行三次共识，查询交易最终返回结果中包括三次共识的信息，包括"confirm_unit_info（发送方第二次共识）"、"recv_unit_info（接收方共识）"、"send_unit_info"（发送方第一次共识），如下所示。

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
            "tx_fee" : 0
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

一笔跨账户交易会经过三次共识，根据返回参数exec_status判断交易最终是否成功：

1.当exec_status返回值为"success"，证明交易最终成功。

2.当exec_status返回值为"failure"，则交易失败，此时recv_tx_exec_status返回值为"failure"，表明交易接收方共识失败。

提醒：

> 如查询交易返回的结果中，没有出现以上三个字段，证明此时交易共识还未完全结束，请稍后再查询。

* 失败返回

当hash错误时，返回：

```
{
   "errmsg" : "0x38a14c80a6621ef67dc3a3311f90555bf722eeb0bac316c21261a70fe6f5e4 length is not correct",
   "errno" : 3,
   "sequence_id" : "21"
}
```

### 查询链上账户信息

当一个用户发送交易至链上时，必须要使用该用户最新的"nonce"和"last_hash_xxhash64"，这两个属性可以从该方法的返回值中获取。

**请求方法**

`getAccount`

**请求参数**

| 参数名称     | 是否必选 | 默认值 | 类型   | 说明             |
| ------------ | -------- | ------ | ------ | ---------------- |
| account_addr | 是       | -      | String | 查询的账户地址。 |

**返回参数**

| 参数名称                | 类型   | 说明                                                         |
| ----------------------- | ------ | ------------------------------------------------------------ |
| account_addr            | String | 账户地址。                                                   |
| available_gas           | Uint64 | 账户现有可用gas的量，单位Tgas。                              |
| balance                 | Uint64 | 账户余额，单位uTOP。                                         |
| burned_token            | Uint64 | 该账户所有已经销毁的TOP token，单位uTOP。                    |
| cluster_id              | Uint8  | cluster ID。                                                 |
| created_time            | Uint64 | 账户在链上创建的时钟高度。                                   |
| disk_staked_token       | Uint64 | 兑换disk锁定的TOP token，单位uTOP。                          |
| gas_staked_token        | Uint64 | 兑换gas锁定的TOP token，单位uTOP。                           |
| group_id                | Uint8  | group ID。                                                   |
| latest_tx_hash          | String | 最新共识成功的交易hash。                                     |
| latest_tx_hash_xxhash64 | String | 最新共识成功的交易xx64hash。                                 |
| latest_unit_height      | Uint64 | 最新共识成功的交易的unit block高度。                         |
| lock_balance            | Uint64 | 锁定的TOP token，单位uTOP，主要用于合约交易。<br/>调用合约的时候，发送方可同时给合约转账，如果合约执行失败，转账款需要退还给发送方，所以在合约执行成功前，将转账款锁定。 |
| lock_deposit_balance    | Uint64 | 用户合约交易费用与执行合约交易占用的CPU时长以及交易大小相关，无法在交易开始确定合约的交易费用。采取的方法是冻结一部分发送方交易保证金，在交易第三次共识的时候，根据合约的最终执行情况，扣除发送方交易保证金，单位uTOP。 |
| lock_gas                | Uint64 | 用户合约交易费用与据执行合约交易占用的CPU时长以及交易大小相关，无法在交易开始确定合约的交易费用。采取的方法是冻结发送方一部分gas，在交易第三次共识的时候，根据合约的最终执行情况，扣除发送方交易消耗的gas，单位Tgas。 |
| nonce                   | Uint64 | 该账户最新共识成功的交易序号，唯一。                         |
| total_gas               | Uint64 | 账户总gas量，单位Tgas。                                      |
| unlock_disk_staked      | Uint64 | 解锁中的兑换disk的TOP token，发起解锁后，需要等待24小时，解锁的金额才会到账。 |
| unlock_gas_staked       | Uint64 | 解锁中的兑换gas的TOP token，发起解锁后，需要等待24小时，解锁的金额才会到账。 |
| unused_vote_amount      | Uint64 | 该账户未使用选票数量。                                       |
| vote_staked_token       | Uint64 | 兑换选票锁定的TOP token，单位uTOP。                          |
| zone_id                 | Uint8  | zone ID。                                                    |

**请求样例**

```
account_address=T-0-LVb72cJ9LzaQbdR41Zvx7dsAL1oDFFGQrJ&
body={
"params" : {
    "account_addr" : "T-0-LVb72cJ9LzaQbdR41Zvx7dsAL1oDFFGQrJ" 
   }
}&
method=getAccount&
sequence_id=9&
identity_token=&
version=1.0
```

**返回样例**

* 成功返回

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

* 失败返回

```
Please create new public-private key pairs or set default account first!!!
```

### 查询区块信息

根据账户地址及区块高度查询区块信息。

提醒：

> 查询区块信息的前提是账户已发送交易且交易执行成功。

**请求方法**

`getBlock`

**请求参数**

| 参数名称     | 是否必选 | 默认值 | 类型          | 说明                                                         |
| ------------ | -------- | ------ | ------------- | ------------------------------------------------------------ |
| account_addr | 是       | -      | String        | 查询unit block请使用普通账户地址，如"T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT"。<br/>查询table block请使用table block账户地址，如"T-a-gRD2qVpp2S7UpjAsznRiRhbE1qNnhMbEDp@0"。<brget/>table block账户可通过区块链浏览器查询。 |
| height       | 是       | -      | String/Uint64 | 最新区块高度"latest"(String)或者具体区块高度(Uint64)。       |

**返回参数**

* unit block

| 参数名称  |                   |                 |                     |                     | 类型          | 说明                                                         |
| --------- | ----------------- | --------------- | ------------------- | ------------------- | ------------- | ------------------------------------------------------------ |
| body      |                   |                 |                     |                     | Object        |                                                              |
|           | fullunit          |                 |                     |                     | Object        | 为了节约数据存储空间，每21个lightunit会打包成1个fullunit，这个21个lightunit将会被清除。 |
|           | lightunit         |                 |                     |                     | Object        | 包括lightunit_input、lightunit_state两部分。                 |
|           |                   | lightunit_input |                     |                     | Object        | lightunit块相关交易。                                        |
|           |                   |                 | txs                 |                     | Object        | 本区块打包的交易信息，结构为map数组，map的key为交易hash，比如6e734fed40b907bb64d257968e6a46a79c4ca144088d330b674cc8b545350324 |
|           |                   |                 |                     | recv_tx_exec_status | Uint8         | 交易接收方执行结果：1--成功；2--二次共识失败或拒绝执行，通常在执行合约交易的时候会出现拒绝共识的情况。 |
|           |                   |                 |                     | send_tx_lock_gas    | Uint64        | 交易发送方可为接收方支付的gas。                              |
|           |                   |                 |                     | tx_exec_status      | Uint8         | 发送方交易执行状态，1-- success；2-- fail。                  |
|           |                   |                 |                     | tx_consensus_phase  | Uint8         | 交易的发送方or接收方，1--self；2--send；3--recv。<br/>enum_transaction_subtype_self      = 1,  // self operate<br/>enum_transaction_subtype_send      = 2,  // send to other account<br/>enum_transaction_subtype_recv      = 3,  // receive from other account<br/>enum_transaction_subtype_recv_ack  = 4(confirm),  // receive ack from other account |
|           |                   |                 |                     | used_tx_deposit     | Uint32        | 交易消耗的交易保证金，单位uTOP。                             |
|           |                   |                 |                     | used_disk           | Uint32        | 交易消耗的账户disk资源。                                     |
|           |                   |                 |                     | used_gas            | Uint32        | 交易消耗的账户gas资源，单位tgas。                            |
|           |                   | lightunit_state |                     |                     | Object        | lightunit块相关交易带来的状态变化。                          |
|           |                   |                 | balance_change      |                     | Uint64        | 余额变化。                                                   |
|           |                   |                 | native_property     |                     | key-value数组 | 账户原生属性，如果属性为空，表明本次出块该属性未设置，计算上等价为0。参见“账户原生属性“。 |
|           |                   |                 | custom_property_key |                     | String数组    | 查询有变化的自定义属性名称列表，只返回key。                  |
| hash      |                   |                 |                     |                     | String        | 本块hash的十六进制字符串。                                   |
| header    |                   |                 |                     |                     | Object        |                                                              |
|           | auditor_xip       |                 |                     |                     | String        | 本块的auditor leader节点。（xip格式）                        |
|           | timerblock_height |                 |                     | timerblock_height   | Uint64        | 时钟块高度。                                                 |
|           | validator         |                 |                     |                     | String        | 本块的validator leader节点账户地址。                         |
|           | validator_xip     |                 |                     |                     | String        | 本块的validator leader节点。（xip格式）                      |
| height    |                   |                 |                     |                     | Uint64        | 块高度。                                                     |
| owner     |                   |                 |                     |                     | String        | unit block所属账户地址。                                     |
| prev_hash |                   |                 |                     |                     | String        | 前一区块hash的十六进制。                                     |
| timestamp |                   |                 |                     |                     | Uint64        | 出块时间戳GMT。                                              |

账户原生属性：

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

属性值计算方法：
"@30" : "3234333733"
每两个字符代表相应整数的ASCII十六进制表示，比如2的十六进制ASCII为0x32，因此@30的实际值为24373。

* table block

| 参数名称   |           |                   |                      | 类型   | 说明                                                         |
| ---------- | --------- | ----------------- | -------------------- | ------ | ------------------------------------------------------------ |
| tableblock |           |                   |                      | Object | table block下包含多个units，当链上长时间没有新交易，查询tableblock最新高度数据为"null"。 |
|            | units     | lightunit_input   |                      | Object | tableblock存储若干unit块相关信息。                           |
|            |           |                   | is_contract_create   | Bool   | 合约里创建的交易。（后续可能会删除）                         |
|            |           |                   | last_tx_nonce        | String | 上一笔交易nonce。                                            |
|            |           |                   | sender_tx_locked_gas | Uint64 | 交易发送方可为接收方支付的gas。                              |
|            |           |                   | tx_consensus_phase   | Uint8  | 交易的发送方or接收方，1（self）,2（send）,3（recv）<br/>enum_transaction_subtype_self      = 1,  // self operate<br/>enum_transaction_subtype_send      = 2,  // send to other account<br/>enum_transaction_subtype_recv      = 3,  // receive from other account<br/>enum_transaction_subtype_recv_ack  = 4(confirm),  // receive ack from other account |
|            |           | lightunit_state   |                      | Object | 参见unit block参数说明。                                     |
|            |           | unit_height       |                      | Uint64 | unit块高度。                                                 |
|            | hash      |                   |                      | String | 块hash的十六进制字符串。                                     |
|            | header    |                   |                      | Object |                                                              |
|            |           | auditor_xip       |                      | String | 本块的auditor leader节点(xip)。                              |
|            |           | timerblock_height |                      | Uint64 | 时钟块高度。                                                 |
|            |           | auditor           |                      | String | 本块的validator leader节点账户地址。                         |
|            |           | validator_xip     |                      | String | 本块的validator leader节点(xip)。                            |
|            | height    |                   |                      | Uint64 | 块高度。                                                     |
|            | owner     |                   |                      | String | table block所属账户地址。                                    |
|            | prev_hash |                   |                      | String | 前一区块hash的十六进制。                                     |
|            | timestamp |                   |                      | Uint64 | 出块时间戳。                                                 |

**请求样例**

* unit block

```
target_account_addr=T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT&
body={
"params" : {
    "account_addr" : "T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT",
    "height" : "latest" 
   }
}&
method=getBlock&
sequence_id=4&
identity_token=&
version=1.0
```

* table block

```
target_account_addr=T-a-gRD2qVpp2S7UpjAsznRiRhbE1qNnhMbEDp@0&
body={
"params" : {
    "account_addr" : "T-a-gRD2qVpp2S7UpjAsznRiRhbE1qNnhMbEDp@0",
    "height" : 264
   }
}&
method=getBlock&
sequence_id=5&
identity_token=&
version=1.0
```

**返回样例**

* 成功返回

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
                           "send_tx_lock_Tgas" : 0,
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

table blcok

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
                           "sender_tx_locked_gas" : 0,
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

* 失败返回

查询账户错误，返回：

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

### 查询主链信息

**请求方式**

`getChainInfo`

**请求参数**

| 参数名称     | 是否必选 | 默认值 | 类型   | 说明       |
| ------------ | -------- | ------ | ------ | ---------- |
| account_addr | 是       | -      | String | 账户地址。 |

**返回参数**

| 参数名称               | 类型   | 说明                   |
| ---------------------- | ------ | ---------------------- |
| first_timerblock_hash  | String | 第一个时钟块hash。     |
| first_timerblock_stamp | Uint64 | 第一个时钟块生成时间。 |
| version                | String | 主链版本。             |

**请求样例**

```
account_address=T-0-LPiPwUsQK8A7qeLaByLcfk57khRTM9XTpn&
body={
"params" : {
    "account_addr" : "T-0-LPiPwUsQK8A7qeLaByLcfk57khRTM9XTpn"
   }
}&
method=getChainInfo&
sequence_id=6&
identity_token=&
version=1.0
```

**返回样例**

* 成功返回

```
{
	"data": {
		"first_timerblock_hash": "ddba228c84b6380845adce23ced7dd0f4574c9fbb45f2221a610e869798565e3",
		"first_timerblock_stamp": 1590980774,
		"version": "0.0.0.1"
	},
	"errmsg": "ok",
	"errno": 0,
	"sequence_id": "6"
}
```

* 失败返回

无。

### 查询节点信息

**请求方法**

`queryNodeInfo`

**请求参数**

| 参数名称          | 是否必选 | 默认值 | 类型   | 说明                                                         |
| ----------------- | -------- | ------ | ------ | ------------------------------------------------------------ |
| account_addr      | 是       | -      | String | 发出查询请求账户地址。                                       |
| node_account_addr | 否       | 空     | String | node_account_addr = "",表示查所有节点；<br/>node_account_addr = T-xxx, 表示查单个节点。 |

**返回参数**

| 参数名称             | 类型    | 说明                                                         |
| -------------------- | ------- | ------------------------------------------------------------ |
| account_addr         | String  | 节点账户地址。                                               |
| auditor_credit       | String  | auditor节点信誉分。                                          |
| auditor_stake        | Uint64  | auditor节点权益：auditor stake=（节点保证金+节点得票总数）*信誉分 |
| dividend_ratio       | Integer | 分红率，百分比%，值为[0,100]。                               |
| network_id           | String  | 节点经过选举后，加入的具体的网络ID。                         |
| node_deposit         | Uint32  | 节点注册保证金。                                             |
| nodename             | String  | 节点昵称。                                                   |
| registered_node_type | String  | 节点注册类型：<br/>边缘节点：edge<br/>验证节点：validator<br/>高级节点：advance |
| node_sign_key        | String  | 注册节点时使用的公钥。                                       |
| validator_credit     | String  | validator节点信誉分。                                        |
| validator_stake      | Uint64  | validator节点权益：validator stake=（节点保证金+节点得票总数）/2 |
| vote_amount          | Uint64  | 节点得票总数。                                               |

**请求样例**

```
target_account_addr=T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT&
body={
"params" : {
    "account_addr" : "T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT",
    "node_account_addr" : "T-0-LKXjgwdL9bTwADL89cBp7L2ze3wqiNmRB4"
   }
}&
method=queryNodeInfo&
sequence_id=4&
identity_token=&
version=1.0
```

**返回样例**

* 成功返回

```
 "data" : {
      "account_addr" : "T-0-LKXjgwdL9bTwADL89cBp7L2ze3wqiNmRB4",
      "auditor_credit" : "0.100000",
      "auditor_stake" : 0,
      "dividend_ratio" : 0,
      "network_id" : "0 ",
      "node_deposit" : 1000,
      "nodename" : "qiqi",
      "registered_node_type" : "validator",
      "node_sign_key" : "BFYRfp557uqBAKA54JmFkT+UVXf74LVdHDnjtLBM/V3nyIdKiHLwX2yr7s03RCoHNfciT0Zays3oUvcmkRv5VK4=",
      "validator_credit" : "0.100000",
      "validator_stake" : 0,
      "vote_amount" : 0
   },
   "errmsg" : "ok",
   "errno" : 0,
   "sequence_id" : "10"
}
```

* 失败返回

节点未注册返回：

```
{
	"data": null,
	"errmsg": "ok",
	"errno": 0,
	"sequence_id": "8"
}
```

### 查询节点奖励

**请求方法**

`queryNodeReward`

**请求参数**

| 参数名称          | 是否必选 | 默认值 | 类型   | 说明                                                         |
| ----------------- | -------- | ------ | ------ | ------------------------------------------------------------ |
| account_addr      | 是       | -      | String | 发出查询请求账户地址。                                       |
| node_account_addr | 否       | 空     | String | node_account_addr = "",表示查所有；<br/>node_account_addr= T-xxx, 表示查单个节点。 |

**返回参数**

| 参数名称        | 类型   | 说明                     |
| --------------- | ------ | ------------------------ |
| accumulated     | Uint64 | 奖励总额，单位uTOP。     |
| last_claim_time | Uint64 | 上次领取奖励的时钟高度。 |
| unclaimed       | Uint64 | 未领取的奖励，单位uTOP。 |

**请求样例**

```
target_account_addr=T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT&
body={
"params" : {
    "account_addr" : "T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT",
    "node_account_addr" : "T-0-LKXjgwdL9bTwADL89cBp7L2ze3wqiNmRB4"
   }
}&
method=queryNodeReward&
sequence_id=4&
identity_token=&
version=1.0
```

**返回样例**

* 成功返回

```
{
   "data" : {
      "accumulated" : 40458594,
      "last_claim_time" : 0,
      "unclaimed" : 40458594
   },
   "errmsg" : "ok",
   "errno" : 0,
   "sequence_id" : "53"
}
```

* 失败返回

```
{
	"data": null,
	"errmsg": "ok",
	"errno": 0,
	"sequence_id": "10"
}
```

### 查询节点投票分布

**请求方法**

`listVoteUsed`

**请求参数**

| 参数名称          | 是否必选 | 默认值 | 类型   | 说明                                                         |
| ----------------- | -------- | ------ | ------ | ------------------------------------------------------------ |
| account_addr      | 是       | -      | String | 发出查询请求账户地址。                                       |
| node_account_addr | 否       | 空     | String | node_account_addr = "",表示查所有；<br/>node_account_addr = T-xxx, 表示查单个节点。 |

**返回参数**

| 参数名称   | 类型    | 说明                                                    |
| ---------- | ------- | ------------------------------------------------------- |
| vote_infos | Map数组 | 接收投票节点账户地址(String)；给节点投票数量(Integer)。 |

**请求样例**

```
target_account_addr=T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT&
body={
"params" : {
    "account_addr" : "T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT",
    "node_account_addr" : "T-0-LKXjgwdL9bTwADL89cBp7L2ze3wqiNmRB4"
   }
}&
method=listVoteUsed&
sequence_id=4&
identity_token=&
version=1.0
```

**返回样例**

* 成功返回

```
{
   "data" : {
      "vote_infos" : {
         "T-0-LKXjgwdL9bTwADL89cBp7L2ze3wqiNmRB4" : 200
      }
   },
   "errmsg" : "ok",
   "errno" : 0,
   "sequence_id" : "80"
}
```

* 失败返回

```
{
	"data": null,
	"errmsg": "ok",
	"errno": 0,
	"sequence_id": "12"
}
```

### 查询投票者分红

**请求方法**

`queryVoterDividend`

**请求参数**

| 参数名称          | 是否必选 | 默认值 | 类型   | 说明                                                         |
| ----------------- | -------- | ------ | ------ | ------------------------------------------------------------ |
| account_addr      | 是       | -      | String | 发出查询请求账户地址。                                       |
| node_account_addr | 否       | 空     | String | node_account_addr = "",表示查所有；<br/>node_account_addr= T-xxx, 表示查单个节点。 |

**返回参数**

| 参数名称        |                 | 类型   | 说明                                 |
| --------------- | --------------- | ------ | ------------------------------------ |
| accumulated     |                 | Uint64 | 投票者已领取的分红总额。             |
| last_claim_time |                 | Uint64 | 投票者上次领取分红的时钟高度。       |
| node_dividend   |                 | List   | 被投票节点的分红信息。               |
|                 | account_addr    | String | 被投票节点账户地址。                 |
|                 | accumulated     | Uint64 | 被投票节点分给该投票者的分红。       |
|                 | last_claim_time | Uint64 | 被投票节点分红上次被领取的时钟高度。 |
|                 | unclaimed       | Uint64 | 被投票节点分给该投票者未领取的分红。 |
| unclaimed       |                 | Uint64 | 投票者未领取的分红总额。             |

**请求样例**

```
target_account_addr=T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT&
body={
"params" : {
    "account_addr" : "T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT",
    "node_account_addr" : "T-0-LKXjgwdL9bTwADL89cBp7L2ze3wqiNmRB4"
   }
}&
method=queryVoterDividend&
sequence_id=4&
identity_token=&
version=1.0
```

**返回样例**

* 成功返回

```
{
   "data" : {
      "accumulated" : 1136005762,
      "last_claim_time" : 16632,
      "node_dividend" : [
         {
            "account_addr" : "T-0-LKogyPKkA6owYPjPGrzYSe39KLTSVJUzS8",
            "accumulated" : 1136005762,
            "last_claim_time" : 16632,
            "unclaimed" : 768988516
         }
      ],
      "unclaimed" : 768988516
   },
   "errmsg" : "ok",
   "errno" : 0,
   "sequence_id" : "82"
}
```

* 失败返回

```
{
	"data": null,
	"errmsg": "ok",
	"errno": 0,
	"sequence_id": "14"
}
```

### 查询提案信息

**请求方法**

`queryProposal`

**请求参数**

| 参数名称     | 是否必选 | 默认值 | 类型   | 说明                                 |
| ------------ | -------- | ------ | ------ | ------------------------------------ |
| account_addr | 是       | -      | String | 发出查询请求账户地址。               |
| proposal_id  | 否       | 空     | String | 提案ID。如为空，则默认查询所有提案。 |

**返回参数**

| 参数名称               | 类型   | 说明                                                         |
| ---------------------- | ------ | ------------------------------------------------------------ |
| effective_timer_height | Uint64 | 提案通过后生效时钟高度。如生效始终高度小于提案通过时的始终高度，那么提案在通过后会立刻生效。 |
| expire_time            | String | 提案失效时间，提案在259200个时钟高度内没有被TCC通过或者否决，提案将失效。 |
| priority               | Uint8  | 提案优先级：1--Normal；2--Important；3--Critical。<br/>只有TCC委员对提案有表决权，对于不同级别的提案，表决通过的规则不同：<br/>Normal：需51%委员通过。<br/>Important：需51%委员通过，且弃权委员不超过25%。<br/>Critical：需2/3的委员通过，且反对委员不超过20%。 |
| proposal_account_addr  | String | 提案发起者账户地址。                                         |
| proposal_deposit       | Uint64 | 提案保证金，最低为100*10^6 uTOP。                            |
| proposal_id            | String | 提案ID，系统自动生成，唯一。                                 |
| proposal_type          | Uint8  | 提案类型：1--修改链上治理参数提案；2--社区基金管理提案。     |
| target                 | String | 当提案类型为修改链上治理参数提案时，target为链上治理参数名称，链上治理参数请参见[链上治理参数说明]( docs-cn/On-ChainGovernance/Overview.md)；<br/>当提案类型为社区基金管理提案时，target为接受转账账户地址，销毁账户地址为：T-b-gkhLhFJXVN3ZPQYZxphja93BEUtwKRdden。 |
| value                  | String | 当target为链上治理参数时，value为修改后的值。<br/>当target为接受转账账户地址，value为转账金额，单位uTOP。 |
| voting_status          | Uint16 | 该提案投票表决状态：0--未开始；8--进行中；9--失败；10--成功。 |

**请求样例**

```
target_account_addr=T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT&
body={
"params" : {
    "account_addr" : "T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT",
    "node_account_addr" : "T-0-LKXjgwdL9bTwADL89cBp7L2ze3wqiNmRB4"
   }
}&
method=queryProposal&
sequence_id=4&
identity_token=&
version=1.0
```

**返回样例**

* 成功返回

```
{
	"chain_timer_height": 40,
	"proposal_deposit": 400,
	"end_time": "1590487691",
	"modification_description": "bbb",
	"new_value": "2",
	"orig_value": "10000",
	"parameter": "archive_deposit",
	"priority": 3,
	"proposal_account_addr": "T-0-LZ6nexZWNPwua7cagJcGuzLwe5aytnzTsk",
	"proposal_id": "aaa",
	"voting_status": 0
}
```

* 失败返回

```
{
	"data": null,
	"errmsg": "ok",
	"errno": 0,
	"sequence_id": "16"
}
```

