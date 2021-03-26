---
title: Swap exchange
type: tutorials
language: en
order: 251
---

> If you are just an ordinary user rather than a developer, you can exchange it through qingah wallet. [download qingah wallet]（ https://wallet.qingah.com/ ）, first select the source smart token you want to redeem, enter the quantity you want to redeem, and then select the target token you want to redeem. Click exchange and wait for a moment to exchange successfully.



## Token exchange

The whole exchange process is executed in the dpos program, so the wallet plug-in is first called to sign the transaction method and parameters, and then sent to the dpos push service program.


```javascript

var QinGah = require('qingah');
var qingah_client = new QinGah();

```

**Call method:**

```javascript
let result = qingah_client.dpos.exchange(accountId, amountA, tokenA, amountB, tokenB, price, id);
```

**方法说明:**

使用 `exchange()` 方法，在 QinGah 使用 Bancor / uniswap 进行令牌之间兑换

该接口既支持 Bancor 协议，也支持 uniswap 协议。

**参数说明:**

| Params     | Description             |
| -------- | ---------------- |
| accountId    | 兑换账号         |
| amountA | 兑换令牌数量     |
| tokenA | 兑换令牌数量     |
| amountB| 兑换令牌目标数量 |
| tokenB | 兑换目标令牌     |
| price| 价格(仅在 uniswap 限价交易中填值) |
| id | 渠道商 id (仅在 uniswap 交易中使用) |

**需要说明的是，在 Bancor 兑换中，`to` 填入目标令牌为0的值，假设我需要兑换 ATP，则填入`0.0000 ATP`；`price`填`0.0`；`id`填`owner`即可。**

假设用户 `atp1t88x1...` 发行了一个智能令牌叫做 `aBTC` ，用户 `atp1t88x2...` 发行了一个叫做了 `bETH` 的智能令牌。那么用户 `atp1t88x1...` 想要用 `aBTC` 兑换得到一定数量的 `bETH` 是怎么实现的呢？调用接口 `exchange` 接口进行兑换。

```javascript
// 初始化 QinGah 客户端
...
let ctx = qingah_client.contract('qingah-swap');
let result = ctx.exchange('atp1t88x1...', 'atpaBTC...', 'atpbETH...', 1.0000, 0.0000, '0.0', 'atp1t88x2...');
```

如果想要兑换成 `QinGah` 中的 `bETH` 令牌，那么只需将 `0.0000 aETH` 换成 `0.0000 bETH` 即可。

## uniswap exchange

👉 read【[ uniswap exchange](token-uniswap.html)】part.


## One click exchange of wallet

[下载 QinGah 钱包](https://wallet.qingah.com/) ，先选择你要兑换的源智能令牌，输入要兑换的数量，再选择你要兑换的目标令牌，点击兑换，稍等片刻即可兑换成功。