
# 概述

## 什么是 Arseeding

Arseeding 是 Arweave 生态中的一个强大的基础设施，它不仅提供了 Arweave 网关功能，还可以用它来上传和下载和广播任何 AR 原生交易和数据，这也就意味着你只需要关心你自己的业务数据而不用同步 Arweave 全量数据，这将节省大量的服务器运维成本。

Arseeding 支持 bundle（ANS-104）交易类型。相比于 AR 原生交易，bundle 交易类型更加适用于文件小数量多的存储场景。在使用 bundle 进行文件存储时，Arseeding 支持使用 everPay 进行存储费用支付，这意味着持有 ETH、USDC 等代币也可以使用 Arweave 的文件存储服务；同时，Arseeding 能 100% 保证文件上传 Arweave 节点，提升开发者的存储体验。

## 为什么要有 Arseeding

目前对于 arweave 生态开发者面临两个严重的问题：

1. 生态项目严重依赖 [arweave.net](http://arweave.net/) 网关，存在单点故障的问题。
2. arweave 节点交易广播服务存在已知 bug。[arweave.net](http://arweave.net/) 网关接收到的 txs 不能及时广播给其他出块节点。导致 [arweave.net](http://arweave.net/) 网关 pending pool 累积大量的 txs，但其他出块节点没有交易进行打包。后果就是 pending pool 中大量的交易会过期。
3. 生态中没有开源的 ANS-104 bundler 协议实现。
4. Arweave 生态代币获取渠道少，原生 Arweave 交易速度慢，使用 everPay 进行支付的 [web3infura.io](http://web3infura.io) 网关更便于开发者进入 Arweave 生态。

为了解决以上问题，everFinance 团队开发了
[Arseeding](https://github.com/everFinance/arseeding) 节点。

## Arseeding 实现功能

### 交易广播服务

Arseeding 完全兼容 Arweave 原生节点，开发者可以直接将交易发送到 Arseeding 节点，**Arseeding 会自动把交易广播到网络中所有的 Arweave 节点**。此功能确保所有 Arweave 节点的 pending pool 中能及时接收到这笔交易，提高交易被打包的速率。

### 数据做种服务

Arweave 是通过概率存储机制确保链上数据大概率不会被丢失，由于部分网关可用性不稳定，可能导致数据上传失败甚至丢失。在实际应用中，Bob 通过 [arweave.net](http://arweave.net/) 节点发送一笔存储交易，交易被正常打包上链。但是如果 [arweave.net](http://arweave.net/) 没有及时把 Bob 的数据广播给网络中的其他节点，可能出现数据丢失的问题，此时是无法从 Arweave 网络中获取该数据。

开发者使用 Arseeding 时，所有的数据都将被存储到 Arseeding 节点，**开发者可以使用 Arseeding 提供的特殊接口对数据进行全网广播，让数据副本被全网节点存储**。

### 数据获取服务

Arweave 是一套概率存储系统，意味着每个节点不需要存储所有的数据，即使是 [arwave.net](http://arwave.net/) 网关也不一定存储了所有的链上数据。在实践过程中，开发者在下载数据可能需要轮询所有的 Arweave 节点。通过 Arseeding 节点，**开发者可以调用特殊接口向全网自动请求指定数据，简单快速接入 Arweave 生态**。

同时，**Arseeding 也可以作为开发者的 CDN 网关，为应用提供高速的下载服务**。这将降低对 [arweave.net](http://arweave.net) 的单点依赖，为应用提供更好的数据服务。

### ANS-104 协议实现

完全支持 bundle 交易。支持上传多份 bundle items 自动批量打包成 bundle，100% 上传至 Arweave 网络。支持自动解析 bundle，拆分为 item 供开发者请求 bundle item 数据。

### 使用 everPay 进行无 GasFee 实时支付

开发者部署的 Arseeding 节点不仅可以为自有的业务服务，也可以公开给所有的加密生态用户使用！公开的 Arseeding 节点将使用 everPay 进行支付。用户把数据上传到公共的 Arseeding 节点后，只需要使用 everPay 进行一笔无 gas fee 的支付就可以获得 Arweave 的永存服务。everPay 目前已经支持 4 条公链和数十种数字资产，用户无需担心没有 AR 原生代币，也不需要担心其他公链高昂的支付矿工费，**用户可以使用任何 everPay 支持的代币将文件永存在 Arweave 上**。
