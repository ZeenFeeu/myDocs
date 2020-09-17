# 快速入门

本章将带您了解节点如何快速在链上发送交易，以及加入TOP Network网络。

流程如下图所示。

![Snap44](QuickStart.assets/Snap44.jpg)

## 配置云服务器及账号权限

根据各角色入网的硬件要求，配置云服务器，且用户类型为"root"或者拥有"sudo"权限的普通用户。

各角色节点入网硬件配置最低要求如下表所示。

| 节点角色            | 硬件配置                               |
| ------------------- | -------------------------------------- |
| 边缘节点(edge)      | 2CPU/4GB men<br/>40GB SSD<br/>100Mb/s  |
| 验证节点(validator) | 2CPU/4GB men<br/>60GB SSD<br/>100Mb/s  |
| 审计节点(auditor)   | 4CPU/8GB men<br/>100GB SSD<br/>200Mb/s |
| 存档节点(archive)   | 4CPU/8GB men<br/>100GB SSD<br/>200Mb/s |

## 下载并安装TOPIO

下载并安装TOPIO，具体请参见[安装TOPIO](docs-cn/Tools/TOPIO/InstallTOPIO.md)。

说明：

> 以下示例使用云服务器root用户账户，如您是普通用户账户（需要具有sudo权限），须在所有命令前加上"sudo"。

## 启动topcl

安装TOPIO后，在TOPIO下，执行如下命令进入链客户端topcl。

```
topio topcl
```

## 创建链上账户

在TOP Network，与链的一切交互，包括查询信息、发送交易、节点管理等，首先都需要一个链上账户。

步骤1 创建本地账户。

在topcl下，执行如下命令，创建本地账户。

```
$> wallet createaccount
Please set a password for the account keystore file. The password must consist of Numbers and Letters, 8 to 16 characters.
Or Ctrl+D skips this step.
Please Input Password Again
Please set a password hint! If don't, there will be no hint when you forget your password.
basketball
Successfully create an account locally!

You can share your public key and account address with anyone.Others need them to interact with you!
You must nerver share the private key and account keystore file with anyone!They control access to your funds!
You must backup your account keystore file!Without the file,you’ll be impossible to access account funds!
You must remember your password!Without the password,it’s impossible to use the keystore file!
Public Key: BBtwShz7qgisA4RsjpvgmijBAAPlh9m/bRh2OsRLK7erroPUD0vFQcwWh4cwVlaIRugxq9b+L67JMztdLipeygc=
Account Address: T-0-LKQULGZTa6uGPDmEtLMaCLgy922NLQntNs
Account Keystore File Path: /root/Topnetwork/keystore/T-0-LKQULGZTa6uGPDmEtLMaCLgy922NLQntNs
```

创建成功后，将在本地生成账户keystore文件，默认存储目录：/root/Topnetwork/keystore。

步骤2 创建链上账户。

通过一个已在链上创建的、有余额的账户，给您在上一步创建的本地账户中转入TOP token，即可在链上创建您的链上账户。

**确认账户余额**

TOP Network发送交易至少需要100,000 uTOP token作为交易保证金，否则交易将被丢弃。

说明：

> 在TOPIO中，账户余额、交易保证金等TOP token单位为uTOP，1TOP=1*10^6 uTOP。

在账户gas资源充足的情况下，交易保证金在交易成功后会立即退回到您的账户。如gas资源不足以支付交易费用，则需要从交易保证金中扣除一笔费用用来兑换gas资源以支付交易费用，扣除的TOP token将被销毁。

如交易保证金也不足以兑换足够的gas资源，那么交易最终将失败。

对于调用部署在Beacon上合约的交易（注册节点相关、提案相关、启动节点进程入网），除了需要最低交易保证金100,000 uTOP token，系统会自动从发送方账户里扣除100*10^6 uTOP token的服务费，并销毁。

所以，注册节点，账户余额中除了节点注册需要的最低保证金（参见[注册节点](#注册节点)），还应另外至少有100.1*10^6 uTOP余额。

**重新设置默认账户**

系统默认使用您最新创建的账户作为发送交易的账户。

如您退出TOPIO后重启TOPIO，需要执行如下命令重新设置默认账户。

```
$>wallet SetDefault /root/Topnetwork/keystore/T-0-LVUuJXHbfzshf6RM44iCavc24SdMHHCQjE
T-0-LVUuJXHbfzshf6RM44iCavc24SdMHHCQjE: Set default account successfully.
```

## 注册节点

步骤1 创建公私钥对。

提醒：

> 此步可选，您可以不创建公私钥对，直接使用节点账户公私钥对作为节点注册的node key，在节点注册时，"node_sign_key"传入节点账户的公钥。

为了更好地保护您的账户资产，建议您使用`wallet createKey`创建一对无资产的公私钥作为节点的node key，在节点注册入网后，节点工作时使用该私钥为节点签名。

提醒：

> 请确保您node key的keystore文件，在当前您TOPIO所运行的节点服务器上。

```
$> wallet createkey
Please set a password for the keystore file. The password must consist of Numbers and Letters, 8 to 16 characters.
Or Ctrl+D skips this step.
Please Input Password Again
Please set a password hint! If don't, there will be no hint when you forget your password.
basketball
Successfully create an key locally!

You can share your public key with anyone.Others need it to interact with you!
You must nerver share the private key and keystore file with anyone!They can use them to make the node malicious.
You must backup your keystore file!Without the file,you may not be able to send transactions.
You must remember your password!Without the password,it’s impossible to use the keystore file!
Public Key: BBYTqmkmNksMjX/ydgnixYP1fVmd0zHQGqW1xCBo4zXNrWf3H/XXqe+NsUkvrSuZ4wtDbJqdE7NDU752gMFd5+g=
Keystore File Path: /root/Topnetwork/keystore/Lgq6CojT16wVRSCEuGcsQRPg8eRsz3auyJ
```

步骤2 注册节点。

TOP Network目前有三种类型的节点：边缘(edge)节点、验证(validator)节点、高级(advance)节点。您可以注册成为其中一种类型的节点。

高级节点可在不同的网络里同时担任多个角色：验证(validator)、审计(auditor)、存档(archive)，注册高级节点时，节点注册保证金需满足这三种角色节点中所需的最高保证金即可，即注册高级节点节点保证金最低为1,000,000*10^6 uTOP。

各角色节点注册最低保证金如下表所示。

| 节点角色            | 保证金              |
| ------------------- | ------------------- |
| 边缘节点(edge)      | 100,000*10^6 uTOP   |
| 验证节点(validator) | 500,000*10^6 uTOP   |
| 审计节点(auditor)   | 1,000,000*10^6 uTOP |
| 存档节点(archive)   | 1,000,000*10^6 uTOP |

执行如下命令，注册节点。

```
$> system registerNode 500000000000 validator qiqi BBYTqmkmNksMjX/ydgnixYP1fVmd0zHQGqW1xCBo4zXNrWf3H/XXqe+NsUkvrSuZ4wtDbJqdE7NDU752gMFd5+g=
$> {
   "errmsg" : "ok",
   "errno" : 0,
   "sequence_id" : "28",
   "tx_hash" : "0x15109aaf5f3084f66dc8767a39787abc0629d8eb66099e0712f39033af7e26c3",
   "tx_size" : 261
}
Please use command 'get transaction' to query transaction status later on!!!
```

请求参数说明如下表所示。

| 参数名称         | 是否必选 | 默认值 | 类型   | 说明                                                         |
| ---------------- | -------- | ------ | ------ | ------------------------------------------------------------ |
| register_deposit | 是       | -      | Uint64 | 节点注册保证金，单位uTOP。                                   |
| node_type        | 是       | -      | Object | 节点类型，包括edge（边缘节点）、validator（验证节点）、advance（高级节点），高级节点兼具archive（存档）、validator（验证）、auditor（审计）角色。<br/>您可以注册成为三种类型中的一种。<br/>注册成为advance节点后，节点被选举为何种工作角色取决于节点所获得的选票：<br/>advance节点若要被选举为audtior、archive、REC、ZEC角色，节点所获得的选票需要大于等于节点实际质押的保证金（此处节点保证金以TOP计算，非uTOP）。<br/>当选票低于实际质押保证金时，advance节点只能被选为validator。<br/>说明：<br/>节点选票须由接受投票而得，可以由其他节点投票，也可由本账户投票。 |
| nodename         | 是       | -      | String | 节点昵称。                                                   |
| node_sign_key    | 是       | -      | String | 您可以使用节点账户公私钥对作为节点注册的node key，此处直接传入节点账户的公钥(base64)。<br/>为了更好地保护您的账户资产，建议您使用`wallet createKey`创建一对无资产的公私钥对，在节点注册入网后，节点工作时使用该私钥为节点签名。<br/>此处请输入对应的公钥(base64)，其他节点可使用该公钥进行验签。 |

执行命令后，根据返回的交易hash查询交易是否成功、以及能否查询到节点信息判断节点注册是否成功。如查询交易成功，且可以查询到节点信息，证明节点注册成功；如查询交易失败，且无法查询到节点信息，则注册失败。

查询交易信息请参见[查询链上信息](docs-cn/Tools/TOPIO/topcl/GET.md)中“查询账户交易详情”内容。

查询节点信息请参见[系统交易](docs-cn/Tools/TOPIO/topcl/system.md)中“查询节点信息”内容。

除了注册节点，您还可以在链上进行其他交易，例如转账、兑换gas资源、兑票、投票等，具体请参见[topcl使用指南](docs-cn/Tools/TOPIO/topcl/Overview.md)。

## 退出topcl

在topcl中完成节点注册后，需要退出topcl，启动节点进程xnode，才可加入TOP Network物理网络，进入候选池等待选举。节点选举规则请参见[节点选举](docs-cn/Node/NodeElection.md)。

使用如下命令退出topcl：

`exit`、`q`、`quit`、`logout`。

## 启动节点进程xnode

提醒：

> 为了方便查询节点入网后的信息，建议您此处使用TOPIO console模式，即进入交互环境的同时启动xnode、topcl。
>
> 为了在本地运行多个节点进程，您必须确保每个节点都有一个单独的数据目录。

在TOPIO下执行如下命令。

```
topio --account_addr T-0-LVUuJXHbfzshf6RM44iCavc24SdMHHCQjE -k /root/Topnetwork/keystore/Lgq6CojT16wVRSCEuGcsQRPg8eRsz3auyJ --password 123456789jjj --datadir /home/cathy4 console
```

可选项说明如下表所示。

| 选项名称       | 默认值 | 类型   | 说明                                                         |
| -------------- | ------ | ------ | ------------------------------------------------------------ |
| --account_addr | -      | String | 节点账户地址。                                               |
| -k,--keystore  | -      | String | 此节点注册时设置的node key的keystore文件路径：如使用节点账户公私钥对作为node key，此处为节点账户keystore文件路径；如使用公私钥对作为node key，则此处为公私钥对keystore文件路径。 |
| -p,--password  | -      | String | node key对应的账户keystore或者公私钥对keystore文件密码。     |
| -d,--datadir   | -      | String | 给节点指定数据目录，如不指定，则节点数据目录为系统默认路径。如指定数据目录：/home/cathy4，则keystore文件以及链数据默认存储在此目录下。 |

## 查询节点网络信息

### 查询节点是否成功入网

执行如下命令查询节点是否成功加入物理网络。

```
xnode net joined
```

如返回结果为"true"，节点入网成功；如返回结果为"false"，节点入网失败。

### 查询节点经过选举后加入的网络ID

执行如下命令，查询节点经过选举后，加入的网络ID。

```
xnode net netID
```

运行命令后：

| ID                                                           | Network                    |
| ------------------------------------------------------------ | -------------------------- |
| xnetwork_id[0]                                               | 节点还未被选举到分片网络。 |
| xnetwork_id[0] zone_id[1]、cluster_id[0]、group_id[0]        | Beacon Network             |
| xnetwork_id[0] zone_id[2]、cluster_id[0]、group_id[0]        | ZEC Network                |
| xnetwork_id[0] zone_id[14]、cluster_id[1]、group_id[1]       | Archiver Network           |
| xnetwork_id[0] zone_id[15]、cluster_id[1]、group_id[1]       | Edger Network              |
| xnetwork_id[0] zone_id[0]、cluster_id[1]、group_id[n],n∈[1,63] | Auditor Network            |
| xnetwork_id[0] zone_id[0]、cluster_id[1]、group_id[m],m∈[64,126] | Validator Network          |

节点加入网络后的其他信息，请使用xnode命令查询，具体请参见[xnode命令说明](docs-cn/Tools/TOPIO/xnode/command.md)。