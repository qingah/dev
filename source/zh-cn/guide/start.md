---
title: 项目规划
type:  tutorials
language: zh-cn
order: 5
---

合约工程师开发三个合约，代理合约、存储合约和业务逻辑合约。代理合约主要功能为迭代和更新合约，把权限多签授予给社区节点监督和审计，并可由社区和安全公司审核和批准。存储合约，为整个逻辑DApp的提供业务数据逻辑数据结构存储和计算(图示1，主要为MerkleTree为合约逻辑提供链上验证。业务合约功能为核心算法逻辑，提供链下压缩数据验证和多方计算。

代码的目录结构：

```
qingah-swap/
├── src
│   ├── call.js  
│   ├── initClient.js  // 连接文件
│   ├── deploy.js  //加载、发布合约脚本文件
│   ├── abi
│   │   ├── qingah.abi  //合约abi文件
│   │   └── qingah.sol  // 合约代码文件
│   └── package.json
└── server
    └── node.js
```

## 部署节点

**开启RPC服务**

部署Alaya节点和增加业务所需的数据API功能服务

制定初步业务订数据接口规范。

## DAppUi

初步产品业务逻辑和界面功能展示

【[Draft](../ui/introduction.html#UI-设计)】

## ZkSnark算法实现和交互代码库

【[代码库](http://github.com/qingah/zkroll)】

## 代码合约

代理合约、存储合约和业务逻辑合约。

## DPOS投票批量打包程序

推送交易验证程序，实现方法为通过拍卖确定一个排序者，如果该行为人表现不佳，那么代币持有者可以通过投票将其驱逐，并发起新的拍卖。

## 后端业务API

为前端提供速度和优化的API接口。

本章项目规划，下面为大家Swap使用教程可以在【[教程](./tutorials-instructions.html)】章节中查阅。

