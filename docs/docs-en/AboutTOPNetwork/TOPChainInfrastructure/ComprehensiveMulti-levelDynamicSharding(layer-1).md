# Multi-level Dynamic Sharding(Layer-1)

## Overview

Layer-1 scaling refers to direct performance improvements in the blockchain itself. Of all the on-chain scalability solutions, sharding has gained the most attention, and is generally considered the best route for a permissionless public blockchain. However, most sharding attempts thus far have only sharded in terms of computation, and not state and network capacity.
TOP Network has developed a comprehensive multi-level dynamic sharding paradigm which shards in all three aspects. This is facilitated by the underlying network infrastructure in layer-0, along with a layered consensus network, VRF-based sortition algorithms, a two-layer Lattice-DAG data structure, and a parallel pBFT-PoS* consensus mechanism. These innovations allow TOP Network to achieve linear scalability while remaining secure and totally permissionless.

## Overall Design

TOP Network adopts a multi-chain architecture. The entire structure of TOP Chain consists of up to 255 level-1 chains, which include the main chain and service chains. Each level-1 chain can itself support up to 255 level-2 sub-chains, and so on. For instance, a developer could branch their own level-2 chain off of the main VPN service chain specifically to support their business.

Level-1 chains are distinct loosely coupled chains. Transactions are routed to the appropriate level-1 chain based on the purpose of the transaction. If it’s for asset transfer, the transaction is routed to the main chain, while if it’s for a type of service transaction, it is routed to the relevant service chain.
Each level-1 Beacon Chain is populated from a common set of nodes. Communication between each level-1 chain network is supported by TOP Network’s P2P Internet architecture, allowing for efficient communication between nodes across chain networks.

![mainchain](ComprehensiveMulti-levelDynamicSharding(layer-1).assets/mainchain.jpg)

A chain at any Level can have a maximum of 16consensus zones, and 255 consenseus clusters. Currently, the number of auditor groups per cluster on the main chain is limited to 2, which means there can be a total of 4 validator groups on the main chain.

## the Beacon

Like some other sharding architectures, we use a Beacon Chain as part of our design. However, unlike other Beacon Chain implementations, TOP Network’s Beacon Chain is very lightweight, and is not involved in confirming the result of the consensus process through methods such as cross-linking. This is the case for Beacon Chains in other sharding projects, which results in them becoming bottlenecks in the system,as all Shards ultimately rely on a single chain to help confirm transactions.

### Beacon Chain Purposes

TOP Network’s Beacon Chain has numerous roles, acting as the coordinator and archive for the system. The Beacon Chain handles node registration and elections, along with staking, voting, slashing, and work-load logging. These parameters and processes are logged and handled through Beacon Chain smart contracts. The Beacon Chain also acts as a global clock for the whole system through the production of timing blocks at regular intervals.

### Beacon Chain Nodes

In many sharding systems, the Beacon Chain is run by nodes who must specifically register to act as Beacon Chain nodes. This can result in a shortage of Beacon Chain nodes, as usually most of the mining rewards are distributed to nodes processing transactions. This can ultimately lead to centralization of
the Beacon Chain.
On TOP Network, every node is a potential Beacon Chain node by default. The nodes with the highest stake have a higher probability of selection via VRF-FTS algorithm, which means Beacon Chain operations are generally the most secure.
To add Beacon Chain blocks and execute Beacon Chain smart contracts, a subset of 256 nodes from the entire pool of nodes is randomly selected for each consensus round.

## VRF-FTS Random Partitioning

Security is one of the biggest challenges involved with sharding. By dividing the network into smaller groups, it becomes easier for malicious entities to take control of shards in what’s called a Single-Shard Takeover Attack. To prevent this, the sharding process is done randomly. With random sharding, malicious entities are not able to direct their nodes into any specific shard, making an attack much more
difficult.
To generate randomness in a decentralized manner, TOP Network makes use of a Verifiable Random Function.VRFs are cryptographic constructs which allow for the creation of unbiasable and publicly verifiable random seeds. These random seeds generated via VRF are used along with a weighted Follow-The-
Satoshi (FTS) algorithm to sort Validators into shards, and Advanced Nodes into Clusters.

## Dynamic Sharding

While random partitioning helps prevent attackers from directly inserting their nodes into a particular shard, it is not sufficient to prevent adaptive adversaries from slowly corrupting a shard. For instance,it is possible that given enough time, a malicious entity could bribe all the nodes in a particular shard.
If the node makeup of a shard remains static, this type of bribing attack is feasible. To prevent this,TOP Network employs dynamic sharding. Every so often, a few nodes from each shard are reshuffled into other shards. Over time, shards will have completely different nodes then they had previously. As only a few
nodes are shuffled around at a time, consensus can continue uninterrupted while the newly moved nodes sync to the state of their new shard.
This same process occurs in the Routing/Audit Network as well, where Advanced Nodes are continuously shuffled between Clusters. This two-layer dynamic sharding scheme renders adaptive adversary attacks practically impossible. 

## Multi-Level Sharding

When it comes to sharding, the goal is to achieve linear scalability. This means that scalability increases linearly with increasing node count. For this to occur, the amount of work each node must do should not strongly depend on the total number of nodes in the system, or the global volume of transactions.
To accomplish this, all of a blockchain’s resources must be sharded, including state(storage), computation(transaction validation and smart contract execution), and networking(block propagation, cross-shard communication etc). If, for instance, only computation is sharded, then storage or bandwidth will eventually become a bottleneck.
To reach the goal of a fully sharded system, we developed a novel multi-layered sharding architecture.Each blockchain resource is sharded in multiple ways, which aids in increasing scalability and keeping node requirements low.

## Two-Layer State Sharding

The state of TOP Network consists of all user accounts and all smart-contracts. Every user account and every smart-contract is represented by an account object. Each account object contains multiple properties, associated functions, and a mini NoSQL database. This state is sharded in two ways.
First, account objects are divided between shards, meaning nodes within a shard only need to store the state of a subset of the total account space. While this already achieves partitioning of the global state,it is not quite sufficient for a few reasons. Since Validator nodes only store the state of the account subspace associated with that shard, they cannot adequately verify transactions sent from other shards unless they know that the state of the sending account was properly updated.
To account for this, we use table blocks, which store the latest state information of recently changed accounts. There are 1024 table blocks which are divided into the current number of shards. The account space is mapped so that each table block is responsible for an equally sized account subspace. Finding the table block associated with an account can be quickly determined using the hash of the account number. This allows shards to quickly pull relevant state information from other shards and check if balances have been updated properly before committing a transaction.
Table blocks are also used to increase throughput by batching transactions. Multiple transactions from the same account, along with transactions from accounts within the same account subspace, can be included together in one table block. These blocks can be validated in one consensus round,potentially increasing throughput drastically. 

## Three-Layer Compute Sharding

Computational resources are needed for transaction validation and smart-contract execution, which are both pBFT consensus processes. We use a three-layer design to facilitate secure compute sharding.
When a transaction is sent to a shard for validation, a random subset within that shard is chosen to perform a pBFT consensus check. The rest of the shard monitors the subset and aids with the subsequent synchronization process. Advanced nodes in the governing cluster are also involved in the pBFT consensus through a secondary audit process. The concept of three-layer compute sharding will become more clear after reading section 4 on TOP’s consensus mechanism.

## Three Layer Network Sharding

The network is divided into three levels. At the top level is Zones, followed by Clusters within the Routing Network, and then Shards(auditor group and validator group) within the Core Network. Each Zone is a network of Clusters and Shards, and each Cluster and Shard is also a network. Network operations within each of these levels is
confined to a specific network. For instance, network communication within a Shard or Cluster always remains between the nodes in that Shard or Cluster. This three-layer partitioning of the network helps to ensure that bandwidth requirements do not grow significantly as the size of the overall network grows.

## Three Layer Consensus Network

The TOP Network consensus network is divided into three layers: the Edge Network, Routing Network,and Core Network.The Edge Network acts as the access point for clients. All transactions are first sent to Edge Nodes in the Edge Network before being relayed to the Routing Network.
The Routing Network consists of Advanced Nodes randomly partitioned into Clusters. This layer handles cross-shard communication and synchronization while also acting as a secondary audit network for transaction validation carried out in the Core Network.
The Core Network consists of shards made up of Validator Nodes. This is where transaction validation takes places. Within each shard, Validator Nodes validate and confirm transactions using a parallel pBFT algorithm.
A tiered network is used for multiple reasons. First, splitting duties among multiple types of nodes helps keep node requirements low. The Routing Network handles most of the bandwidth heavy operations such as cross-shard communication, which then allows Validator nodes to have relatively low
requirements. In addition, the Edge Network helps protect the Routing and Core Network from spam attacks, as clients can only send transactions directly to Edge Nodes.