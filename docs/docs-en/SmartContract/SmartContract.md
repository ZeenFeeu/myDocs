# 智能合约

## 概述

现实世界的合约简而言之是一种给定一套输入规定行为产出的协议。一份合约可以是一份正式的法律合约（比如金融交易），也可以简单如某种游戏规则。

TOP Network智能合约是一种软件，定义了接口以及实现接口的代码， 并将代码编译成节点能获取和执行的标准字节码格式。

## 合约类型

TOP Network链上有两种合约，一是原生系统合约，采用 C++ 语言开发，实现TOP Network链的政治经济制度及链上治理功能，另一种是用户合约，由Lua脚本语言开发，实现特定的应用功能。

## 系统合约

TOP Network系统合约提供的功能包括节点入网、发起链上治理提案、节点注册、节点投票等。

系统合约如下表所示：

| 系统合约         | 部署位置 | 合约地址                                   | 说明                                                      | 是否时钟触发 |
| ---------------- | -------- | ------------------------------------------ | --------------------------------------------------------- | ------------ |
| 集群选举相关合约 | Beacon   | T-21-38PMFdqf8CxBgBAXrYtWuqd7i23z3C85KDq@0 | REC elect REC：REC选举REC集群                             | 是           |
|                  |          | T-21-38S9CXF7EFLUfk4ZTnwtjuLJgNu4Jf8Go4A@0 | REC elect ZEC：REC选举ZEC集群                             | 是           |
|                  |          | T-21-38NN9R9DoXrEhaEGKjGDzSmY1ZfgLqSetvQ@0 | REC elect edge：edge集群选举                              | 是           |
|                  |          | T-21-38TWCPmtZNavRWgTp9KF4CFt6kdCfFxP6EP@0 | REC elect archive：archive集群选举                        | 是           |
|                  |          | T-21-38Mjkx1r1ciN6wABk8RBF9KirKUcW9WJiMy@0 | REC进行节点推荐                                           | 是           |
|                  | ZEC      | T-22-4uS5rwwKTQeeLPcNwb5VAeXbQhVAq5TiR3d@3 | ZEC elect auditor/validator：ZEC选举auditor/validator集群 | 是           |
|                  |          | T-22-4uN3e6AujFyvDXY4h5t6or3DgKpu5rTKELD@3 | 分片关联关系合约。                                        | 否           |
|                  |          | T-22-4uCQ5Di2vZmPURNYVUuvWm5p7EaFQrRLs76@3 | ZEC备选池合约。                                           | 是           |
| 链上治理合约     | Beacon   | T-21-38QMHWxXshXyZa1E48JU1LREu3UrT5KGD2U@0 | 链上治理。                                                | 否           |
| 注册合约         | Beacon   | T-21-38DSqqwBWkHKxkVuCq3htW47BGtJRCM2paf@0 | 节点注册。                                                | 否           |
|                  | ZEC      | T-22-4uAt8Na2U1GUtWSHXJSqaJJBXunUX9WU9kB@0 | 审计工作量统计。                                          | 是           |
|                  |          | T-22-4uDhihoPJ24LQL4znxrugPM4eWk8rY42ceS@2 | 审计惩罚。                                                | 是           |
| shard合约        | shard    | T-2-MFGB3gFWSsMfEo9LrC7JEaj1gJTXaYfXny     | 分片工作量统计                                            | 是           |
|                  |          | T-2-ML7oBZbitBCcXhrJwqBhha2MUimd6SM9Z6     | 分片惩罚                                                  | 是           |
|                  |          | T-2-MVfDLsBKVcy1wMp4CoEHWxUeBEAVBL9ZEa     | 投票合约                                                  | 否           |
| 增发合约         | ZEC      | T-22-4uBPYNoyqWBgKzNFnhjtFH5E5fuPZJAapji@1 | 增发TOP token作为奖励池                                   | 否           |

## 用户智能合约

### 合约开发

目前用户智能合约只支持Lua脚本语言开发，Lua API使用指南请参见[Lua API](docs-cn/SmartContract/LuaAPI.md)。

### 合约部署

使用TOPIO、RPC API或者TOP Network官方SDK部署用户智能合约。

TOPIO是TOP Network面向社区用户提供的集客户端操作(topcl)与节点管理(xnode)的一款工具，使用指南请参见[TOPIO使用指南](docs-cn/Tools/TOPIO/Overview.md)。

RPC API是TOP Network提供给社区使用的与链交互的接口，包括发送交易，查询交易信息、节点信息、链信息等，RPC API使用指南请参见[RPC API](docs-cn/Interface/RPC-API/Overview.md)。

TOP Network软件开发工具包(SDK)封装了加密算法、RPC交互和智能合约，是应用程序与TOP Network网络之间交互的桥梁，SDK使用指南请参见[SDK使用指南](docs-cn/Interface/SDKs/00-overview.md)。

## 合约调用

可通过TOPNetwork提供的RPC API、SDK及TOPIO客户端或者其他客户端（钱包等）调用合约。

系统合约函数详细请参见[系统合约函数](docs-cn/SmartContract/SystemContractFunction.md)。