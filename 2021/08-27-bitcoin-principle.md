---
title: bitcoin 笔记
date: 2021-08-27
tag: [webpack]
---

# Bitcoin

## transaction-based ledger

bitcon 是一个基于交易的账本

bitcoin 会维护一个 UTXO 数据结构

UTXO：upspent trasction output，用来防止 double spending

每笔交易都要指明，从哪几个交易记录到哪几个地址，

**备注： 以太坊是 account-based ledger**



## 比特币挖矿奖励减半

比特币每 21万个区块会使挖矿奖励减半，比特币通过控制挖矿难度来控制平均每 10 分钟出一个区块，而 21万个区块差不多等于 4年。

21万 * 10分钟 / (60分钟 * 24小时 * 365天) = 约4年



## Bernoulli trial

比特币挖矿的每一次尝试  nonce 都是一次 Bernoulli trial

Bernoulli trial: a random experiment with binary outcome

Bernoulli process: a sequence of independent Bernoulli trials

memoryless: 前面的结果不会影响后面的概

比特币是 memoryless and progress free 的: 这提供了挖矿公平性的保证，每一次尝试都是一样的概率。



## 比特币网络

The BitCoin Network

appliction layer: BitCoin Block Chain

network layer: P2P Overlay Network

simple robust but not efficient



## BTC 挖矿难度

挖矿目标，使得 H(block header) <= target，其中 Header 中的 nonce 可以修改，使得结果小于 target（target 是前缀一连串 0 的二进制数）

bitcoin 挖矿的本质就是尝试出 nonce 使得结果小于 target，从而获得记账权。

SHA-256 即 Secure hash Algorithm 2^256，产出 256 位二进制数。

**出块时间太短会有什么问题？**

同一时间会有很多分叉，算力会被分散，有恶意的算力可以集中扩展一个分叉。

好人的算力被分散了，这样坏人就不需要 51%的算力进行攻击了。

以太坊将出块时间降低到了15s，这样的话就需要新的共识机制，ghost，（扩展，orphan node, uncle reward）

**调整方式？**

```
target = target * (actural time / expected time)
```

其中 actual time = time spent mining the last 2016 blocks

expected time = 2016 * 10 * 60 (秒)

```
next_dificulty = previous_diffulty * (2 weeks / time to mine last 2016 blocks)
```

target 和 expect_difficulty 成反比

每次调整最大4倍，最小1/4

**如何让大家都进行调整？**

在  block header 中有一个字段 nBits 4个字节，nBits 是 target 的压缩版本（target 有 256位）

如果有坏矿工不进行调整，那么他产生的区块将不被其他矿工承认。



### 比特币如何保证安全性？

1. 密码学上的保证：没有私钥就没办法伪造签名，并且矿工只认有正确签名的交易
2. 共识机制：工作量证明



## BTC 挖矿

从速度上来说：

CPU -> GPU -> ASIC 即 Appliction Specific Integrated Circuit

mining puzzle -> Alternative mining puzzle (目的: ASIC resistance)

有些区块链会使用可变的 mining puzzle，这样可以有效抑制 ASIC 的发展。



## 矿池

矿池：pool manager has many miner，pool manager 也就是全节点，有很多职责，而 miner 只负责计算 hash。

矿池如何分配奖励：使用 almost valid block，矿工只需要挖到矿池规定的具有一定数量前导 0 的 nonce，就可以得到一个 share，最终的奖励根据 share 分配。

矿池的弊病：让 51% Attack 更容易

51%算力的矿池可以干的坏事：

- 分叉攻击
- boycott：封锁账户



## BTC 脚本

- P2PK: pay to public key
- P2PKH: pay to public hash
- P2RSH: pay to redeem script hash
