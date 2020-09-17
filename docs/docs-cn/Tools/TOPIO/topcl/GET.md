# 查询链上信息

## 概述

get命令用于查询链上信息，包括链上账户信息、区块信息、主链信息、账户交易详情。

| 命令                                 | 说明               |
| ------------------------------------ | ------------------ |
| [get account](#查询链上账户信息)     | 查询链上账户信息。 |
| [get chaininfo](#查询主链信息)       | 查询主链信息。     |
| [get block](#查询区块信息)           | 查询区块信息。     |
| [get transaction](#查询账户交易详情) | 查询账户交易详情。 |

## 查看get所有命令及帮助

使用`get -h   ` 或者` get --help`查看get所有命令。

```
COMMANDS:
    account                         Get account information on chain.
    block                           Get block information based on block type and block height.
    chaininfo                       Get chain informaion.
    transaction                     Get transaction details on chain.

OPTIONS:
    -h,--help                       Show a list of commands or help for one command.
```

使用`get account -h`或者`get account --help`查看子命令`get account`的帮助。

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

## 命令使用说明

### 使用前提

查询链上账户信息、查询区块信息、查询账户交易详情，请做好如下准备工作：

（1）您需要拥有链上账户，创建链上账户的方法：使用`wallet createAccount`创建一个本地账户，通过一个有余额的链上账户给此本地账户转账，转账成功后，即可在链上创建该本地账户对应的链上账户。

（2）系统默认使用最新创建的账户作为发送交易的账户，如您已退出TOPIO，重启TOPIO后，请使用`wallet setDefault`命令重新指定发送交易账户。

### 查询链上账户信息

**请求方式**

`get account`

**请求参数**

| 参数名称     | 是否必选 | 默认值       | 类型   | 说明                                                         |
| ------------ | -------- | ------------ | ------ | ------------------------------------------------------------ |
| account_addr | 否       | 当前使用账户 | String | 指定查询链上信息的账户地址，如不指定，则默认查询正在使用的账户。 |

**选项**

| 选项名称  | 默认值 | 类型 | 说明               |
| --------- | ------ | ---- | ------------------ |
| -h,--help | -      | -    | 查看命令帮助信息。 |

**返回参数**

说明：

> TOPIO中所有关于TOP token的单位都为uTOP，1TOP=1*10^6 uTOP。

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
| lock_balance            | Uint64 | 锁定的TOP token，单位uTOP，主要用于用户合约交易。<br/>调用用户合约的时候，交易发送方可同时给合约账户转账，如果合约执行失败，转账款需要退还给发送方，所以在合约执行成功前，先将转账款锁定。 |
| lock_deposit_balance    | Uint64 | 用户合约交易费用与执行合约交易占用的CPU时长以及交易大小相关，无法在交易开始确定合约的交易费用。采取的方法是冻结一部分发送方交易保证金，在交易第三次共识的时候，根据合约的最终执行情况，扣除发送方交易保证金以支付交易费用，单位uTOP。 |
| lock_gas                | Uint64 | 用户合约交易费用与据执行合约交易占用的CPU时长以及交易大小相关，无法在交易开始确定合约的交易费用。采取的方法是冻结发送方一部分gas，在交易第三次共识的时候，根据合约的最终执行情况，扣除发送方交易消耗的gas，单位Tgas。 |
| nonce                   | Uint64 | 该账户最新共识成功的交易序号，唯一。                         |
| total_gas               | Uint64 | 账户总gas量，单位Tgas。                                      |
| unlock_disk_staked      | Uint64 | 解锁中的兑换disk的TOP token，发起解锁后，需要等待24小时，解锁的金额才会到账。 |
| unlock_gas_staked       | Uint64 | 解锁中的兑换gas的TOP token，发起解锁后，需要等待24小时，解锁的金额才会到账。 |
| unused_vote_amount      | Uint64 | 该账户未使用选票数量。                                       |
| vote_staked_token       | Uint64 | 兑换选票锁定的TOP token，单位uTOP。                          |
| zone_id                 | Uint8  | zone ID。                                                    |

如查询用户合约账户信息，除以上参数外，返回以下两个参数。

| 参数名称                | 类型   | 说明                         |
| ----------------------- | ------ | ---------------------------- |
| contract_code           | String | 合约代码。                   |
| contract_parent_account | String | 合约父账户，部署合约的账户。 |

**请求样例**

```
get account
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

链上没有此账户信息，返回：

```
{
   "errmsg" : "account not found on chain",
   "errno" : 11,
   "sequence_id" : "38"
}
```

### 查询主链信息

**请求方式**

```
get chaininfo
```

**请求参数**

无。

**选项**

| 选项名称  | 默认值 | 类型 | 说明               |
| --------- | ------ | ---- | ------------------ |
| -h,--help | -      | -    | 查看命令帮助信息。 |

**返回参数**

| 参数名称               | 类型   | 说明                   |
| ---------------------- | ------ | ---------------------- |
| first_timerblock_hash  | String | 第一个时钟块hash。     |
| first_timerblock_stamp | Uint64 | 第一个时钟块生成时间。 |
| version                | String | 主链版本。             |

**请求样例**

```
get chaininfo
```

**返回样例**

* 成功返回

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

* 失败返回

无。

### 查询区块信息

根据账户地址查询区块信息。

查询区块信息的前提是账户已发送交易且交易共识成功。

**请求方式**

```
get block
```

**请求参数**

| 参数名称     | 是否必选 | 默认值 | 类型          | 说明                                                         |
| ------------ | -------- | ------ | ------------- | ------------------------------------------------------------ |
| account_addr | 是       | -      | String        | 查询unit block请使用普通账户地址，如"T-0-Lh2X4xGs4C88JuPqwNcBwWugZw7Hd3TytT"。<br/>查询table block请使用table block账户地址，如"T-a-gRD2qVpp2S7UpjAsznRiRhbE1qNnhMbEDp@0"。 |
| height       | 是       | -      | String/Uint64 | 最新区块高度"latest"(String)或者具体区块高度(Uint64)。       |

**选项**

| 选项名称  | 默认值 | 类型 | 说明               |
| --------- | ------ | ---- | ------------------ |
| -h,--help | -      | -    | 查看命令帮助信息。 |

**返回参数**

* unit block

| 参数名称 |                     |                 |                     |  | 类型   | 说明                                                         |
| ---------- | ------------------- | --------------- | ------------------- | ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| body       |                     |                 |                     |                     | Object |                                                              |
|            | fullunit            |                 |                     |                     | Object | 为了节约数据存储空间，每21个lightunit会打包成1个fullunit，这个21个lightunit将会被清除。 |
|            | lightunit           |                 |                     |                     | Object | 包括lightunit_input、lightunit_state两部分。                  |
|            |                     | lightunit_input |                     |                     | Object | lightunit块相关交易。 |
|            |                     |                 | txs          |  | Object | 本区块打包的交易信息，结构为map数组，map的key为交易hash，比如6e734fed40b907bb64d257968e6a46a79c4ca144088d330b674cc8b545350324 |
| | | |  | recv_tx_exec_status | Uint8 | 交易接收方执行结果：1--成功；2--二次共识失败或拒绝执行，通常在执行合约交易的时候会出现拒绝共识的情况。 |
|            |                     |                 |                     | send_tx_lock_gas | Uint64 | 用户合约交易中，交易发送方可为接收方支付的gas，单位Tgas。当前为min[gas_limit,available_gas]。 |
|            |                     |                 |                     | tx_exec_status | Uint8  | 发送方交易共识状态，1-- success；2-- fail。                   |
|            |                     |                 |                     | tx_consensus_phase | Uint8 | 交易共识阶段：1--self；2--send；3--recv。<br/>enum_transaction_subtype_self      = 1,  // self operate<br/>enum_transaction_subtype_send      = 2,  // send to other account<br/>enum_transaction_subtype_recv      = 3,  // receive from other account<br/>enum_transaction_subtype_recv_ack  = 4(confirm),  // receive ack from other account |
|            |                     |                 |                     | used_tx_deposit | Uint32 | 交易消耗的交易保证金，单位uTOP。                         |
|            |                     |                 |                     | used_disk | Uint32 | 交易消耗的账户disk资源。                                      |
|            |                     |                 |                     | used_gas | Uint32 | 交易消耗的账户gas资源，单位Tgas。                      |
|            |                     | lightunit_state |                     |                     | Object | lightunit块相关交易带来的状态变化。 |
|            |                     |                 | balance_change |  | Uint64 | 余额变化。                                                    |
|            |                     |                 | native_property |  | Key-value数组 | 账户原生属性，如果属性为空，表明本次出块该属性未被设置，计算上等价为0。参见“账户原生属性”。 |
|            |                     |                 | custom_property_key |  |  String数组  | 查询有变化的自定义属性名称列表，只返回key。      |
| hash       |                     |                 |                     |                     | String | 本块hash的十六进制字符串。                               |
| header     |              |                 |                     |                     | Object |                                                  |
|  | auditor_xip | | | | String | 本块的auditor leader节点。（xip格式） |
|            | timerblock_height |                 |                     |  | Uint64 | 时钟块高度。                                      |
|            | validator           |                 |                     |                     | String | 本块的validator leader节点账户地址。                         |
| | validator_xip | | | | String | 本块的validator leader节点。（xip格式） |
| height     |                     |                 |                     |  | Uint64 | 块高度。                                                       |
| owner      |                     |                 |                     |  | String | unit block所属账户地址。      |
| prev_hash  |                     |                 |                     |                     | String | 前一区块hash的十六进制。                                         |
| timestamp  |                     |                 |                     |                     | Uint64 | 出块时间戳GMT。                                                 |

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

| 参数名称   |           |                   |                    | 类型   | 说明                                                         |
| ---------- | --------- | ----------------- | ------------------ | ------ | ------------------------------------------------------------ |
| tableblock |           |                   |                    | Object | table block下包含多个units，当链上长时间没有新交易，查询tableblock最新高度数据为"null"。 |
|            | units     | lightunit_input   |                    | Object | tableblock存储若干unit块相关信息。                           |
|            |           |                   | is_contract_create | Bool   | 合约里创建的交易。                                           |
|            |           |                   | last_tx_nonce      | String | 上一笔交易nonce。                                            |
|            |           |                   | send_tx_lock_gas   | Uint64 | 应用合约交易中，交易发送方可为接收方支付的gas，单位Tgas。当前为min[gas_limit,available_gas]。 |
|            |           |                   | tx_consensus_phase | Uint8  | 交易共识阶段：1（self）,2（send）,3（recv）<br/>enum_transaction_subtype_self      = 1,  // self operate<br/>enum_transaction_subtype_send      = 2,  // send to other account<br/>enum_transaction_subtype_recv      = 3,  // receive from other account<br/>enum_transaction_subtype_recv_ack  = 4(confirm),  // receive ack from other account |
|            |           | lightunit_state   |                    | Object | 参见unit block参数说明。                                     |
|            |           | unit_height       |                    | Uint64 | unit block高度。                                             |
|            | hash      |                   |                    | String | 块hash的十六进制字符串。                                     |
|            | header    |                   |                    | Object |                                                              |
|            |           | auditor_xip       |                    | String | 本块的auditor leader节点(xip)。                              |
|            |           | timerblock_height |                    | Uint64 | 时钟块高度。                                                 |
|            |           | auditor           |                    | String | 本块的validator leader节点账户地址。                         |
|            |           | validator_xip     |                    | String | 本块的validator leader节点(xip)。                            |
|            | height    |                   |                    | Uint64 | 块高度。                                                     |
|            | owner     |                   |                    | String | table block所属账户地址。                                    |
|            | prev_hash |                   |                    | String | 前一区块hash的十六进制。                                     |
|            | timestamp |                   |                    | Uint64 | 出块时间戳。                                                 |

**请求样例**

* unit block

```
get block T-0-LWG4vTgckM7dKodtfCRmhiUwQsTTKT7e2d latest
```

* table block

```
get block T-a-gRD2qVpp2S7UpjAsznRiRhbE1qNnhMbEDp@146 9
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
                           "send_tx_lock_gas" : 0,
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

### 查询账户交易详情

**请求方式**

```
get transaction
```

**请求参数**

| 参数名称     | 是否必选 | 默认值 | 类型   | 说明                                        |
| ------------ | -------- | ------ | ------ | ------------------------------------------- |
| account_addr | 是       | -      | String | 发送交易或接受交易账户地址。                |
| tx_hash      | 是       | -      | String | 账户最新交易hash，可通过`get account`查询。 |

**选项**

| 选项名称  | 默认值 | 类型 | 说明               |
| --------- | ------ | ---- | ------------------ |
| -h,--help | -      | -    | 查看命令帮助信息。 |

**返回参数**

请参见“[交易数据结构说明](#交易数据结构说明)”。

**请求样例**

```
get transaction T-0-LVb72cJ9LzaQbdR41Zvx7dsAL1oDFFGQrJ 0x8aa1e7082af07bf22840a1526745c484a5a20115d8e92cff2d9ed413128ac2b4
```

**返回样例**

* 成功返回

说明：

> * 交易为单账户交易，交易只在交易发送方下进行一次共识，查询交易最终返回结果中只有"confirm_unit_info"的信息。
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

根据返回参数exec_status判断交易最终是否成功：

1.当exec_status返回值为"success"，证明交易最终成功。

2.当exec_status返回值为"failure"，则交易失败，此时recv_tx_exec_status返回值为"failure"，表明交易接收方共识失败。

提醒：

> 如查询交易返回的结果中，没有出现以上三个字段，证明此时交易共识还未完全结束，请稍后再查询。

* 失败返回

hash值不正确时，返回：

```
{
   "errmsg" : "0x38a14c80a6621ef67dc3a3311f90555bf722eeb0bac316c21261a70fe6f5e4 length is not correct",
   "errno" : 3,
   "sequence_id" : "21"
}
```

## 交易数据结构说明

| 参数名称 |  |         |                      | 类型   | 说明                                 |
| ------------------- | -------------------- | ------ | ------------------------------------ | ------------------- | ------------------- |
| original_tx_info |  |  | | Object | 原始交易信息。 |
|        | authorization |        |                      | String | 交易体签名。                          |
|  | challenge_proof |  | | String | 预留字段，默认为空字符串。 |
|  | ext |  | | String | 预留字段，用于扩展，默认为空字符串。 |
| | from_ledger_id | | | Uint16 | 预留字段，默认为"0"。 |
|      | last_tx_hash |  |                      | Uint64 | 交易发送方上次交易的hash，用于交易的排序和去重。 |
|     | last_tx_nonce |  |                      | Uint64 | 交易发送方上次交易的nonce，用于交易的排序和去重。 |
| | note | | | String | 交易备注。 |
|  | send_timestamp |  | | Uint64 | 交易发送时间戳GMT。 |
|  | to_ledger_id |  | | Uint16 | 预留字段，默认为"0"。 |
|  | tx_action |  | | Object | 交易action，包括"source_action"及"target_action"。 |
|        |        | receiver_action |                      | Object | 交易接受方执行内容。           |
|                     |                     |                     | action_authorization | String | action签名，json结构，当交易为部署合约交易时，此处会显示合约的公钥信息，公钥用来验证合约账户与交易发送方账户是否匹配。 |
|                     |                     |                     | action_ext           | String | 预留扩展字段，默认为空字符串。     |
|                     |                     |                     | action_hash          | Uint32 | 整个action的xxhash32。默认为"0"，暂未使用。   |
|                     |                     |                     | action_name          | String | 调用合约时，合约的函数名。<br/>其中，系统合约函数请参见[系统智能合约 API](docs-cn/Interface/SmartContractInterface/SystemContractAPI.md)。 |
|                     |                     |                     | action_param         | String | 接收方执行内容。不同action type执行内容的序列化请参见[action param序列化](docs-cn/Interface/RPC-API/sendTransaction/action-param-serialization.md)。 |
|                     |                     |                     | action_size          | Uint16 | action对象的大小。         |
|                     |                     |                     | action_type          | Uint16 | 接收方执行类型，不同的交易类型对应不同的action type，具体请参见[tx_type与action_type说明](docs-cn/Interface/RPC-API/sendTransaction/tx-type-and-action-type.md)。<br/>xaction_type_asset_out                = 0,    // 资产转出<br/>xaction_type_create_contract_account    = 3,    // 创建合约账户  <br/>xaction_type_run_contract              = 5,    // 调用智能合约<br/>xaction_type_asset_in                = 6,    // 资产转入<br/>xaction_type_pledge_token_vote          = 21,   //锁定TOP token兑换选票<br/>    xaction_type_redeem_token_vote          = 22,   // 赎回兑换选票的TOP token<br/>    xaction_type_pledge_token               = 23,   //锁定TOP token兑换gas<br/>    xaction_type_redeem_token               = 24,   //解锁兑换gas的TOP token |
| | | | tx_receiver_account_addr | String | 交易接受方账户地址。 |
|                     |                     | sender_action |                      | Object | 交易发送方执行内容。                |
|                     |                     |                     | action_authorization | String | action签名，json结构。 |
|                     |                     |                     | action_ext           | String | 预留字段，默认为空字符串。 |
|                     |                     |                     | action_hash          | Uint32 | 整个action的xxhash32。默认为"0"，暂未使用。 |
|                     |                     |                     | action_name          | String | 预留字段，默认为空字符串。 |
|                     |                     |                     | action_param         | String | 发送方执行内容。不同action type执行内容的序列化请参见[action param序列化](docs-cn/Interface/RPC-API/sendTransaction/action-param-serialization.md)。 |
|                     |                     |                     | action_size          | Uint16 | action对象的大小。 |
|                     |                     |                     | action_type          | Uint16 | 发送方执行类型，不同的交易类型对应不同的action type，具体请参见[tx_type与action_type说明](docs-cn/Interface/RPC-API/sendTransaction/tx-type-and-action-type.md)。<br/>xaction_type_asset_out                  = 0,    // 资产转出。<br/>xaction_type_source_null =1,          // 源端不执行操作 |
| | | | tx_sender_account_addr | String | 交易发送方账户地址。 |
| | tx_deposit | |  | Uint32 | 交易保证金，单位uTOP。 |
| | tx_expire_duration | |  | Uint16 | 交易到期时长，超过则被丢弃，默认100s。 |
| | tx_hash | |  | String | 交易hash的十六进制。 |
| | tx_len | |  | Uint16 | 交易结构的长度。 |
| | tx_random_nonce | |  | Uint32 | 随机数字。默认为"0"，暂未使用。 |
| | tx_structure_version | |  | Uint32 | 交易结构版本号。默认为"0"，暂未使用。 |
|     | tx_type |     |                      | Uint16 | 交易类型，不同的交易类型，action中action_param（执行内容）及action type（执行类型）不同。<br/>xtransaction_type_create_contract_account      = 1,    // 创建合约账户 <br/>xtransaction_type_run_contract                           = 3,    // 调用智能合约<br/>xtransaction_type_transfer                                   = 4,    // 转账<br/>xtransaction_type_vote                                             = 20, //投票<br/>xtransaction_type_abolish_vote                               = 21,//取消投票<br/>xtransaction_type_pledge_token_gas                      = 22,   // 锁定TOP token兑换gas<br/>xtransaction_type_redeem_token_gas                    = 23,   // 赎回兑换gas锁定的TOP token<br/>xtransaction_type_pledge_token_vote                     = 27,   // 锁定TOP token兑换选票<br/>xtransaction_type_redeem_token_vote                    = 28,   // 赎回兑换选票锁定的TOP token |
| tx_consensus_state |  |  | | Object | 交易共识结果。<br>跨账户交易会进行三次共识，所以会返回三个unit的信息；单账户交易只返回只在交易发送账户下进行一次共识，所以只返回"confirm_unit_info"。 |
|  | confirm_unit_info |  | | Object | 交易第三次共识产生的unit block。 |
|  |  | exec_status | | String | 交易最终共识结果：success--成功；failure--失败。 |
|  |  | height | | Uint64 | 交易第三次共识产生的unit block高度。 |
|  |  | recv_tx_exec_status | | String | 交易接收方共识结果：success--成功；failure--失败。<br/>交易接收方共识失败或拒绝执行，通常在执行合约交易的时候会出现拒绝共识的情况。例如，注册节点，节点保证金低于最低要求，合约将执行失败。 |
|  |  | tx_exec_status | | String | 交易发送方共识状态：success--成功；failure--失败。 |
|  |  | unit_hash | | Uint64 | 交易第三次共识产生的unit block对应的hash。 |
|  |  | used_deposit | | Uint32 | 交易第三次共识结束后，扣除发送方账户的交易保证金，单位uTOP。 |
|  |  | used_disk | | Uint32 | 当前默认为"0"。 |
|  |  | used_gas | | Uint32 | 交易第三次共识结束后，扣除的发送方账户gas资源（用户合约交易），单位Tgas。 |
|  | recv_unit_info |  | | Object | 交易第二次共识产生的unit block。 |
|  |  | height | | Uint64 | 交易第二次共识产生的unit block高度。 |
|  |  | unit_hash | | Uint64 | 交易第二次共识产生的unit block对应的hash。 |
|  |  | used_deposit | | Uint32 | 当前默认为"0"。 |
|  |  | used_disk | | Uint32 | 当前默认为"0"。 |
|  |  | used_gas | | Uint32 | 交易第二次共识后，扣除接收方账户的gas资源，单位Tgas。 |
|  | send_unit_info |  | | Object | 交易第一次共识产生的unit block。 |
|  |  | height | | Uint64 | 交易第一次共识产生的unit block高度。 |
| | | tx_fee | | Uint64 | 对于调用Beacon系统合约交易（注册节点相关、提案相关、启动节点进程入网），系统自动从交易发送方账户中扣除100*10^6 uTOP token作为交易手续费，并销毁。 |
|  |  | unit_hash | | Uint64 | 交易第一次共识产生的unit block对应的hash。 |
|  |  | used_deposit | | Uint32 | 当前默认为"0"。 |
|  |  | used_disk | | Uint32 | 当前默认为"0"。 |
|  |  | used_gas | | Uint32 | 交易第一次共识扣除的gas，单位Tgas。 |

