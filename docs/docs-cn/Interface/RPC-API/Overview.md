# RPC API

## 概述

RPC API是TOP Network提供给社区使用的与链交互的接口，包括发送交易，查询交易信息、节点信息、主链信息等。

## 方法列表

| 方法名             | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| requestToken       | 在与链交互前需要先获取身份令牌。<br/>根据账户获取身份令牌(identity token)，每个账户身份令牌不同。 |
| sendTransaction    | 发送交易，包括转账、节点staking、节点注册、节点投票、节点领取奖励、调用合约等。 |
| getAccount         | 查询链上账户信息。                                           |
| getTransaction     | 查询账户交易信息。                                           |
| getBlock           | 查询区块信息。                                               |
| getChainInfo       | 查询主链信息。                                               |
| queryNodeInfo      | 查询节点信息。                                               |
| queryNodeReward    | 查询节点奖励。                                               |
| listVoteUsed       | 查询节点投票分布。                                           |
| queryVoterDividend | 查询投票者分红。                                             |
| queryProposal      | 查询提案信息。                                               |

## 请求结构说明

### 公共请求参数

| 参数名称            | 是否必选 | 默认值 | 类型   | 说明                                                         |
| ------------------- | -------- | ------ | ------ | ------------------------------------------------------------ |
| target_account_addr | 是       | -      | String | 扩展字段，当前未使用，您可填入任意一个账户地址。             |
| body                | 是       | -      | Object | body里为具体业务参数，需将json格式序列化成String格式。<br/>业务参数说明请参见各接口“请求参数”。 |
| method              | 是       | -      | String | 请求方法。                                                   |
| sequence_id         | 是       | -      | String | 客户端会话次数，递增。                                       |
| identity_token      | 是       | -      | String | 与链交互前首先需要获取链访问身份令牌(identity token)，身份令牌因账户不同而不同。当前未对该字段进行校验。 |
| version             | 是       | 1.0    | String | RPC API版本。                                                |

### 请求体示例

以查询账户交易信息为例：

```
target_account_addr=T-0-LPiPwUsQK8A7qeLaByLcfk57khRTM9XTpn&
body={
"params" : {
    "account_addr" : "T-0-LPiPwUsQK8A7qeLaByLcfk57khRTM9XTpn",
    "tx_hash" : 0x8aa1e7082af07bf22840a1526745c484a5a20115d8e92cff2d9ed413128ac2b4
   }
}&
method=getTransaction&
sequence_id=22&
identity_token=&
version=1.0
```

说明：

> 请求体"body"中的参数为各个接口具体业务参数。

## 错误码

| 错误码 | 错误类型                    | 说明                                                         |
| ------ | --------------------------- | ------------------------------------------------------------ |
| 1      | rpc_param_json_parser_error | 请求时json输入错误。<br/>例如：<br/>body json parse error；<br/>json parse error。 |
| 2      | rpc_param_param_lack        | 字段缺失。<br/>例如：miss param method 。                    |
| 3      | rpc_param_param_error       | 错误参数。<br/>例如：send_transaction sign error             |
| 5      | rpc_param_unknown_error     | 未知错误。<br/>例如：unknown exception                       |
| 11     | rpc_shard_exec_error        | shard相关错误，例如链上不存在此账户信息、交易hash对应的交易不存在。<br/>例如：account not find、transaction not find。 |
| 97     | rpc_error                   | 未知错误。<br/>例如：unknow error。                          |
| 98     | rpc_excepiton               | 异常错误。                                                   |
| 99     | rpc_timeout                 | 请求超时。<br/>例如：request time out。                      |
