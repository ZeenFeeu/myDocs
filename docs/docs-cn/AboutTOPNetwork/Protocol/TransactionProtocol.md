# 交易协议

## 概述

交易是指队账户资产发起转账。交易可以包含动作(Action)，当交易到达的时候触发动作(Action)。如果交易里面不指定动作(Action)，那么默认的动作(Action)为转账。

动作(Action)：一个动作可以是一个内置的系统功能，比如Hash和投票，或者是一个自定义的智能合约。用户可以通过发送交易或消息来触发动作。

消息(Message)：消息就是对账户属性发起操作的指令。消息的数据包含属性、动作、输入参数和输出参数。为安全起见，消息不能更改账户余额。

消息是一种特殊的交易。一个发送高频消息的账户会受到流量控制，必须支付gas费，否则账户所有者可能被迫执行工作证明(POW)。

## 交易数据结构

| 参数名称           |                      |                     |                          | 类型   | 说明                                                         |
| ------------------ | -------------------- | ------------------- | ------------------------ | ------ | ------------------------------------------------------------ |
| original_tx_info   |                      |                     |                          | Object | 原始交易信息。                                               |
|                    | authorization        |                     |                          | String | 交易体签名。                                                 |
|                    | challenge_proof      |                     |                          | String | 预留字段，默认为空字符串。                                   |
|                    | ext                  |                     |                          | String | 预留字段，用于扩展，默认为空字符串。                         |
|                    | from_ledger_id       |                     |                          | Uint16 | 预留字段，默认为"0"。                                        |
|                    | last_tx_hash         |                     |                          | Uint64 | 交易发送方上次交易的hash，用于交易的排序和去重。             |
|                    | last_tx_nonce        |                     |                          | Uint64 | 交易发送方上次交易的nonce，用于交易的排序和去重。            |
|                    | note                 |                     |                          | String | 交易备注。                                                   |
|                    | send_timestamp       |                     |                          | Uint64 | 交易发送时间戳GMT。                                          |
|                    | to_ledger_id         |                     |                          | Uint16 | 预留字段，默认为"0"。                                        |
|                    | tx_action            |                     |                          | Object | 交易action，包括"source_action"及"target_action"。           |
|                    |                      | receiver_action     |                          | Object | 交易接受方执行内容。                                         |
|                    |                      |                     | action_authorization     | String | action签名，json结构，当交易为部署合约交易时，此处会显示合约的公钥信息，公钥用来验证合约账户与交易发送方账户是否匹配。 |
|                    |                      |                     | action_ext               | String | 预留扩展字段，默认为空字符串。                               |
|                    |                      |                     | action_hash              | Uint32 | 整个action的xxhash32。默认为"0"，暂未使用。                  |
|                    |                      |                     | action_name              | String | 调用合约时，合约的函数名。<br/>其中，系统合约函数请参见[系统智能合约 API](docs-cn/Interface/SmartContractInterface/SystemContractAPI.md)。 |
|                    |                      |                     | action_param             | String | 接收方执行内容。不同action type执行内容的序列化请参见[action param序列化](docs-cn/Interface/RPC-API/sendTransaction/action-param-serialization.md)。 |
|                    |                      |                     | action_size              | Uint16 | action对象的大小。                                           |
|                    |                      |                     | action_type              | Uint16 | 接收方执行类型，不同的交易类型对应不同的action type，具体请参见[tx_type与action_type说明](docs-cn/Interface/RPC-API/sendTransaction/tx-type-and-action-type.md)。<br/>xaction_type_asset_out                = 0,    // 资产转出<br/>xaction_type_create_contract_account    = 3,    // 创建合约账户  <br/>xaction_type_run_contract              = 5,    // 调用智能合约<br/>xaction_type_asset_in                = 6,    // 资产转入<br/>xaction_type_pledge_token_vote          = 21,   //锁定TOP token兑换选票<br/>    xaction_type_redeem_token_vote          = 22,   // 赎回兑换选票的TOP token<br/>    xaction_type_pledge_token               = 23,   //锁定TOP token兑换gas<br/>    xaction_type_redeem_token               = 24,   //解锁兑换gas的TOP token |
|                    |                      |                     | tx_receiver_account_addr | String | 交易接受方账户地址。                                         |
|                    |                      | sender_action       |                          | Object | 交易发送方执行内容。                                         |
|                    |                      |                     | action_authorization     | String | action签名，json结构。                                       |
|                    |                      |                     | action_ext               | String | 预留字段，默认为空字符串。                                   |
|                    |                      |                     | action_hash              | Uint32 | 整个action的xxhash32。默认为"0"，暂未使用。                  |
|                    |                      |                     | action_name              | String | 预留字段，默认为空字符串。                                   |
|                    |                      |                     | action_param             | String | 发送方执行内容。不同action type执行内容的序列化请参见[action param序列化](docs-cn/Interface/RPC-API/sendTransaction/action-param-serialization.md)。 |
|                    |                      |                     | action_size              | Uint16 | action对象的大小。                                           |
|                    |                      |                     | action_type              | Uint16 | 发送方执行类型，不同的交易类型对应不同的action type，具体请参见[tx_type与action_type说明](docs-cn/Interface/RPC-API/sendTransaction/tx-type-and-action-type.md)。<br/>xaction_type_asset_out                  = 0,    // 资产转出。<br/>xaction_type_source_null =1,          // 源端不执行操作 |
|                    |                      |                     | tx_sender_account_addr   | String | 交易发送方账户地址。                                         |
|                    | tx_deposit           |                     |                          | Uint32 | 交易保证金，单位uTOP。                                       |
|                    | tx_expire_duration   |                     |                          | Uint16 | 交易到期时长，超过则被丢弃，默认100s。                       |
|                    | tx_hash              |                     |                          | String | 交易hash的十六进制。                                         |
|                    | tx_len               |                     |                          | Uint16 | 交易结构的长度。                                             |
|                    | tx_random_nonce      |                     |                          | Uint32 | 随机数字。默认为"0"，暂未使用。                              |
|                    | tx_structure_version |                     |                          | String | 交易结构版本号。默认为"0"，暂未使用。                        |
|                    | tx_type              |                     |                          | Uint16 | 交易类型，不同的交易类型，action中action_param（执行内容）及action type（执行类型）不同。<br/>xtransaction_type_create_contract_account      = 1,    // 创建合约账户 <br/>xtransaction_type_run_contract                           = 3,    // 调用智能合约<br/>xtransaction_type_transfer                                   = 4,    // 转账<br/>xtransaction_type_vote                                             = 20, //投票<br/>xtransaction_type_abolish_vote                               = 21,//取消投票<br/>xtransaction_type_pledge_token_gas                      = 22,   // 锁定TOP token兑换gas<br/>xtransaction_type_redeem_token_gas                    = 23,   // 赎回兑换gas锁定的TOP token<br/>xtransaction_type_pledge_token_vote                     = 27,   // 锁定TOP token兑换选票<br/>xtransaction_type_redeem_token_vote                    = 28,   // 赎回兑换选票锁定的TOP token |
| tx_consensus_state |                      |                     |                          | Object | 交易共识结果。<br>跨账户交易会进行三次共识，所以会返回三个unit的信息；单账户交易只返回只在交易发送账户下进行一次共识，所以只返回"confirm_unit_info"。 |
|                    | confirm_unit_info    |                     |                          | Object | 交易第三次共识产生的unit block。                             |
|                    |                      | exec_status         |                          | String | 交易最终共识结果：success--成功；failure--失败。             |
|                    |                      | height              |                          | Uint64 | 交易第三次共识产生的unit block高度。                         |
|                    |                      | recv_tx_exec_status |                          | String | 交易接收方共识结果：success--成功；failure--失败。<br/>交易接收方共识失败或拒绝执行，通常在执行合约交易的时候会出现拒绝共识的情况。例如，注册节点，节点保证金低于最低要求，合约将执行失败。 |
|                    |                      | tx_exec_status      |                          | String | 交易发送方共识状态：success--成功；failure--失败。           |
|                    |                      | unit_hash           |                          | Uint64 | 交易第三次共识产生的unit block对应的hash。                   |
|                    |                      | used_deposit        |                          | Uint32 | 交易第三次共识结束后，扣除发送方账户的交易保证金，单位uTOP。 |
|                    |                      | used_disk           |                          | Uint32 | 当前默认为"0"。                                              |
|                    |                      | used_gas            |                          | Uint32 | 交易第三次共识结束后，扣除的发送方账户gas资源（用户合约交易），单位Tgas。 |
|                    | recv_unit_info       |                     |                          | Object | 交易第二次共识产生的unit block。                             |
|                    |                      | height              |                          | Uint64 | 交易第二次共识产生的unit block高度。                         |
|                    |                      | unit_hash           |                          | Uint64 | 交易第二次共识产生的unit block对应的hash。                   |
|                    |                      | used_deposit        |                          | Uint32 | 当前默认为"0"。                                              |
|                    |                      | used_disk           |                          | Uint32 | 当前默认为"0"。                                              |
|                    |                      | used_gas            |                          | Uint32 | 交易第二次共识后，扣除接收方账户的gas资源，单位Tgas。        |
|                    | send_unit_info       |                     |                          | Object | 交易第一次共识产生的unit block。                             |
|                    |                      | height              |                          | Uint64 | 交易第一次共识产生的unit block高度。                         |
|                    |                      | tx_fee              |                          | Uint64 | 对于调用Beacon系统合约交易（注册节点相关、提案相关、启动节点进程入网），系统自动从交易发送方账户中扣除100*10^6 uTOP token作为交易手续费，并销毁。 |
|                    |                      | unit_hash           |                          | Uint64 | 交易第一次共识产生的unit block对应的hash。                   |
|                    |                      | used_deposit        |                          | Uint32 | 当前默认为"0"。                                              |
|                    |                      | used_disk           |                          | Uint32 | 当前默认为"0"。                                              |
|                    |                      | used_gas            |                          | Uint32 | 交易第一次共识扣除的gas，单位Tgas。                          |

## 交易生命周期

交易生命周期如下图所示。

![Snap47](TransactionProtocol.assets/Snap47-1599553864316.jpg)

1.客户端对交易签名后，交易直接被发送至Edge Network，从而保护Audit Network、Validate Network、Beacon Network及Sub-Beacon Network免受泛滥交易的攻击。

2.交易到达Edge Network后，根据交易接收方账户所属网络分片，将交易发送至对应的Audit Network。

3.交易到达Audit Network后，交易将被路由至交易接收方账户所在网络分片(Validator/Beacon/Sub-Beacon Network)。

4.交易到达接收方账户所在网络分片后，开始交易共识，具体请参见[共识协议](docs-cn/AboutTOPNetwork/Protocol/ConsensusProtocol.md)。

5.交易共识完成后，将数据打包到区块中，并将区块数据存入交易接收方账户所在分片的所有节点以及archive节点的数据库中。

