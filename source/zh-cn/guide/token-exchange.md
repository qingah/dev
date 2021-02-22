---
title: Swap兑换
type: tutorials
language: zh-cn
order: 251
---

> 如果你只是普通用户而不是开发者，可以通过 QinGah 钱包兑换。[下载 QinGah 钱包](https://wallet.qingah.com/) ，先选择你要兑换的源智能令牌，输入要兑换的数量，再选择你要兑换的目标令牌。点击兑换，稍等片刻即可兑换成功。

## 令牌互兑

整个兑换过程是在 DPOS 程序执行，所以是首先调用钱包插件对交易方法和参数进行签名，然后发送到DPOS推送服务程序。

```javascript

var QinGah = require('qingah');
var qingah_client = new QinGah();

```

**调用方法:**

```javascript
let result = qingah_client.dpos.exchange(accountId, amountA, tokenA, amountB, tokenB, price, id);
```

**方法说明:**

使用 `exchange()` 方法，在 QinGah 使用 Bancor / uniswap 进行令牌之间兑换

该接口既支持 Bancor 协议，也支持 uniswap 协议。

**参数说明:**

| 参数     | 含义             |
| -------- | ---------------- |
| accountId    | 兑换账号         |
| amountA | 兑换令牌数量     |
| tokenA | 兑换令牌数量     |
| amountB| 兑换令牌目标数量 |
| tokenB | 兑换目标令牌     |
| price| 价格(仅在 uniswap 限价交易中填值) |
| id | 渠道商 id (仅在 uniswap 交易中使用) |

**需要说明的是，在 Bancor 兑换中，`to` 填入目标令牌为0的值，假设我需要兑换 FO，则填入`0.0000 ATP`；`price`填`0.0`；`id`填`owner`即可。**

假设用户 `atp1t88x1...` 发行了一个智能令牌叫做 `aBTC` ，用户 `atp1t88x2...` 发行了一个叫做了 `bETH` 的智能令牌。那么用户 `atp1t88x1...` 想要用 `aBTC` 兑换得到一定数量的 `bETH` 是怎么实现的呢？调用接口 `exchange` 接口进行兑换。

```javascript
//初始化 QinGah 客户端
...
let ctx = qingah_client.contract('qingah-swap');
let result = ctx.exchange('atp1t88x1...', 'atpaBTC...', 'atpbETH...', 1.0000, 0.0000, '0.0', 'atp1t88x2...');
```

如果想要兑换成 `QinGah` 中的 `bETH` 令牌，那么只需将 `0.0000 aETH` 换成 `0.0000 bETH` 即可。

## uniswap 交易

👉 见【[ uniswap 交易](token-uniswap.html)】部分


## 钱包一键兑换

[下载 QinGah 钱包](https://wallet.qingah.com/) ，先选择你要兑换的源智能令牌，输入要兑换的数量，再选择你要兑换的目标令牌，点击兑换，稍等片刻即可兑换成功。