---
title: uniswap
type: tutorials
language: en
order: 250
---

## uniswap deposit
> Recharge the native token ATP or arc20 on the chain to the contract.

#### Call example:

```javascript
  // Deposit ATP
  const mainContract = "atpcontract...";
  const account = "atpmyaddress..."
  const Web3 = require('web3')
  let web3 = new Web3()

  var privateKey = Buffer.from(pk, 'hex')
  const contract = new web3.platon.Contract(abi, mainContract, {
    from: account, // default from address
  })

  const contractFunction = contract.methods.depositETH(account)
  const functionAbi = contractFunction.encodeABI()
  const gasAmount = await contractFunction.estimateGas({ from: account })
  const estimatedGas = (gasAmount).toString(16)
  const _nonce = await web3.platon.getTransactionCount(account)
  const nonce = _nonce.toString(16)

  const rawTx = {
    gasLimit: web3.utils.toHex('200000'),
    value: web3.utils.toHex(web3.utils.toWei(amount, 'ether')),
    gasPrice: '0x' + estimatedGas,
    to: mainContract,
    data: functionAbi,
    from: account,
    nonce: '0x' + nonce,
  }
  var tx = new Tx(rawTx)
  tx.sign(privateKey)
  var serializedTx = tx.serialize()
  web3.platon.sendSignedTransaction('0x' + serializedTx.toString('hex')).on('receipt', console.log)
  const res = await contract.methods.depositATP(account).call()


  // Recharge token, according to ARC20 token the transfer method directly to contract
  const resp = contract.transfer(amount, mainContract).call()

```
## uniswap exchange
> In all interfaces of uniswap, the order of token a and token B does not affect the execution result. For example, `ATP-USDT` and `USDT-ATP` belong to the same market.

### Add liquidity

The whole exchange process is executed in the dpos program, so the wallet plug-in is first called to sign the transaction method and parameters, and then sent to the dpos push service program.


```javascript

var QinGah = require('qingah');
var qingah_client = new QinGah();

```

#### Call method:

```javascript
let result = await qingah_client.dpos.addLiquidity(liquidityID, 
    amountADesired, amountBDesired, 
    amountAMin, amountBMin, 
    to, accountId, 
    feeA, feeB
  );
```

#### Method statement:

Use `addLiquidity()` method，You can add positions for the uniswap market. In particular, if there is no such market, the market will be created.

#### Parameter description:

| params     | description             |
| -------- | ---------------- |
| liquidityID | 流动对ID liquidity_a_b    |
| amountADesired | 期望得到的 A amount uint    |
| amountBDesired|  期望得到的 B amount uint   |
| amountAMin|  最小 A amount uint   |
| amountBMin|  最小 B amount uint   |
| to    |  流动对 address    |
| nonce | 用户交易nonce  |
| accountId|  用户address对应合约自增ID    |
| feeA |  feeA    |
| feeB |  feeB    |

#### 限制：

以下所有的限制均不包含常规限制，例如权限不对，精度不对等。本文提到的限制都是特定的限制。

- 已成为`bancor`交易市场的，不能再成为`uniswap`的交易市场。否则报错。
- 加仓后正负价格不能超过 0.1%。否则报错。
- 加仓后个人权重增加必须大于0。否则报错。

该行为会对 A-B 的 uniswap 令牌市场进行加仓，如果该市场不存在，则会创建该市场。

```javascript
...
let result = qingah_client.addLiquidity("liquidity_0_1", 1, 2, 0.995, 1.99, to, 1, 0, 0);
```
在上面的例子中，我们为`ATP-USDT`的 uniswap 市场进行加仓，数额分别是`1.0000 ATP`和`2.0000 USDT`。同样的，如果没有该市场，则会创建`ATP-USDT`的 uniswap 市场。

#### 市场份额表查询：
```javascript
qingah_client.api.getLiquidityRows()
// output:
{
  "rows": [
    {
      "primary": 0,
      "tokena": {
        "quantity": 140.00000000000,
        "contract": "atpaapt..."
      },
      "tokenb": {
        "quantity": 10030.000000,
        "contract": "atpausdt..."
      },
      "total_weights": "11849.89451429842847574"
    }
  ],
  "more": false
}
```

```js
qingah.api.getSwappoolRows("liquidity_a_b") // 此处的"0"对应的对应币币对的 primary
// output:
{
  "rows": [
    {
      "owner": "aptuser1..",
      "weights": "11356.56329871202615323"
    },
    {
      "owner": "aptuser2...",
      "weights": "493.33121558640181092"
    }
  ],
  "more": false
}
```
上例的情况是，`USDT-ATP`的`uniswap`的市场中，`USDT`的总额为`10030.0000 APT`，`ATP`的总额为`140.0000 FO`，所有持仓者总权重为`11849.89451429842847574`。

在该市场中，有两个持仓者，分别为`aptuser1...`和`aptuser2...`。他们在总市场中的权重分别为`11356.56329871202615323`和`493.33121558640181092`。

### Remove Liquidity

**Call method:**

```javascript
let result = await qingah_client.dpos.removeLiquidity(liquidityID, rate, amountAMin, amountBMin, to, nonce, accountId, feeA, feeB);
```

**Method statement:**

Use `removeLiquidity()` method, which can be used to raise positions in a specific uniswap market and put forward our share in the uniswap market.

**参数说明:**

| 参数     | description            |
| -------- | ---------------- |
| liquidityID | 流动对ID liquidity_a_b |
| rate | 提回流动性rate    |
| amountAMin | 最小 A amount uint  |
| amountBMin | 最小 B amount uint  |
| nonce | 用户交易nonce  |
| to    |  流动对 address    |
| accountId|  用户address对应合约自增ID    |
| feeA |  feeA    |
| feeB |  feeB    |

#### Limit：

- $0<rate\leq1$。否则报错。
- 该`uniswap`必须存在。否则报错。
- 该用户在该`uniswap`市场中必须拥有份额。否则报错。
- 用户提取出来的`token`数必须大于该币种的最小精度。否则报错。
- 用户提取结束后剩余的权重必须大于总权重部分的0.1%。否则报错。

该行为会对 A-B 的 uniswap 令牌市场进行提仓，通过`rate`比例提出我们自己的令牌份额。
```javascript
...
qingah_client.dpos.removeLiquidity('liquidity_0_1', 0.5, 0.4975, 0.995, 'aptcontractaddress', nonce, 1, 0, 0);
```
在上面的例子中，我们在`ATP-USDT`的 uniswap 市场进行提仓，比例为`0.1`。操作过后，我们会按`个人权重/总权重 * rate`的比例等比得提取该市场中`ATP`令牌数量和`USDT`令牌数量。


#### Formula description：

在 `uniswap`市场中，用户所拥有的`token`份额以权重的形式展示。在减仓的时候**不能**设置提取某一个数额的`token`出来，只能设置提取的比例，按照对应的比例获得对应的币币对。详细公式见下：

{% raw %}
$$
\begin{gather}
a\times{b}=S\\
填仓:\; a_{new}\times{b_{new}}=S_{new}\\
计算权重：\;\frac{\sqrt{S_{new}}-\sqrt{S}}{\sqrt{S}}\times{weights}\\
减仓比例：\;r=\frac{weigehts_{个人}}{weights_{全部}}\times{rate}_{传入值}\\
减仓提取：(a_{new}\times{r})\quad(b_{new}\times{r})
\end{gather}
$$
{% endraw %}

### Market trading and limit trading

uniswap Market trading, limit trading and Bancor share the `exchange` interface.

| Params     | description            |
| -------- | ---------------- |
| accountId    | account id        |
| amountA | exchange token amount     |
| tokenA |  exchange token     |
| amountB | exchange target token amount |
| tokenB | exchange target token   |
| price | price (only in uniswap limit exchange used) |
| id | Channel business id (only in uniswap exchange used) |

#### Limit

- `bancor` exchange和 `uniswap` share the same interface，**If they are `Bancor` trading pairs, they can't do `uniswap` trading**.Otherwise, an error will be reported.

- 在市价交易的时候，**`amountA`的值必须不为0，`amountB`和`price`的值必须为0**。否则报错。
- 在限价交易的时候，**`price`的价值必须不为0，`amountA`和`amountB`二者必须有且只一个值**。否则报错。
- 该`uniswap`市场必须存在。负责报错。
- 用户兑换出来的`token`数必须大于该币种的最小精度。否则报错。

市价交易：
```javascript
// 初始化 DPOS 客户端
...
qingah_client.dpos.exchange(1, '200.000000', 'aUSDT', 0, 'ATP', 0, 0);
```
上面的例子中，用户id`1`想用`200.000000 aUSDT`兑换`ATP`，不论价格是多少，兑换完`200.000000 aUSDT`为止。

限价交易：
```javascript
...
qingah_client.dpos.exchange(1, "0.0000000", "aUSDT", "100.00000000", "ATP", 1.2, 0);
```
上面的例子中，用户id`1`需要以不高于`1.2`的价格兑换出`100.00000000 ATP`。对于高于`1.2`的价格，仍未兑换完的部分，则会挂单。

#### 挂单表查询示例：
```js
qingah.getSwaporderRows()

// output:
{
  "rows": [
    {
      "primary": 0,
      "owner": "apt...",
      "price": "4294967296", 
      "quantity": {
        "quantity": 100.000000,
        "contract": "aptusdtcontractaddress"
      }
    }
  ],
  "more": false
}
```

### Cancel order
**Call method:**

```javascript
let result = qingah_client.dpos.withdraw(name, symbola, symbolb, bid_id);
```

**Method statement:**

Use `withdraw()` method, you can cancel your registration in uniswap market

**Parameter description**

| Params     | description            |
| -------- | ---------------- |
| accountId    | cancel accountid         |
| symbola | a token   |
| symbolb|  b token   |
| bid_id|  cancel id   |

```javascript
...
let result = await cqingah_client.dpos.tx.withdraw("atpaddress...", 0,  0,  bid_id);
```
Cancellation will cancel the user's registration with this ID, and the amount of registration will be returned to the account

## uniswap Withdraw
> Directly call the contract to withdraw cash.

#### Call contract:

```javascript
  const mainContract = "atpcontract...";
  const account = "atpmyaddress..."
  const Web3 = require('web3')
  let web3 = new Web3()

  var privateKey = Buffer.from(pk, 'hex')
  const contract = new web3.platon.Contract(abi, mainContract, {
    from: account, // default from address
  })

  const contractFunction = contract.methods.withdraw(tokenId, amount)
  const functionAbi = contractFunction.encodeABI()
  const gasAmount = await contractFunction.estimateGas({ from: account })
  const estimatedGas = (gasAmount).toString(16)
  const _nonce = await web3.platon.getTransactionCount(account)
  const nonce = _nonce.toString(16)

  const rawTx = {
    gasLimit: web3.utils.toHex('200000'),
    value: web3.utils.toHex(web3.utils.toWei(amount, 'ether')),
    gasPrice: '0x' + estimatedGas,
    to: mainContract,
    data: functionAbi,
    from: account,
    nonce: '0x' + nonce,
  }
  var tx = new Tx(rawTx)
  tx.sign(privateKey)
  var serializedTx = tx.serialize()
  web3.platon.sendSignedTransaction('0x' + serializedTx.toString('hex')).on('receipt', console.log)
  const res = await contract.methods.withdraw(tokenId, amount).call()

```