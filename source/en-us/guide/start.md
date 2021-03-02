---
title: Project planning
type:  tutorials
language: en
order: 5
---

Contract engineer develops three contracts: agent contract, storage contract and business logic contract. The main function of the agent contract is to iterate and update the contract. The authority is multi signed to the community node for supervision and audit, and can be reviewed and approved by the community and the security company. Storage contract provides business data and logical data structure storage and calculation for the whole logical DAPP (Figure 1, mainly merkletree provides on Chain Verification for contract logic). The service contract function is the core algorithm logic, which provides compressed data verification and multi-party calculation under the chain.


Directory structure of code：

```
qingah-swap/
├── src
│   ├── call.js  
│   ├── initClient.js  // init file
│   ├── deploy.js  // deploy shell file
│   ├── abi
│   │   ├── qingah.abi  // abi
│   │   └── qingah.sol  // sol file
│   └── package.json
└── server
    └── node.js
```

## Deployment node

**Start RPC service**

- Deploy Alaya node and add data API function service required by business.

- Formulate preliminary business data interface specification.

## DApp Ui

Preliminary product business logic and interface function display.

【[Draft](../ui/introduction.html#UI-设计)】

## Zksnark algorithm implementation and interactive code base

【[Repository](http://github.com/qingah/zkDpos)】

## Code contract

Agent contract, storage contract and business logic contract.

## Dpos voting batch packer

The implementation method of push transaction verification program is to determine a sequencer by auction. If the actor does not perform well, the token holder can vote to expel him and initiate a new auction.

## Backend API

Provide speed and optimized API interface for front end.

This chapter project planning, the following for you to use swap tutorial, you can refer to in 【[Guide](./tutorials-instructions.html)】.


