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
| liquidityID |  Liquidity pair ID liquidity_a_b    |
| amountADesired | Expected A amount uint    |
| amountBDesired|  Expected B amount uint   |
| amountAMin|  The minimum A amount uint   |
| amountBMin| The minimum B amount uint   |
| to    |   Liquidity pair address    |
| nonce | nonce  |
| accountId|  Account Id    |
| feeA |  feeA    |
| feeB |  feeB    |

#### Limit：

All the following restrictions do not include general restrictions, such as incorrect permissions and unequal precision. The restrictions mentioned in this article are specific.

- What has become a `Bancor` market can no longer be a `uniswap` market. Otherwise, an error will be reported.
- The positive and negative prices after the increase in the warehouse shall not exceed 0.1%. Otherwise, an error will be reported.
- Personal weight increase must be greater than 0 after position increase. Otherwise, an error will be reported.

This behavior will add positions to the uniswap token market of a-b. if the market does not exist, it will create the market.

```javascript
...
let result = qingah_client.addLiquidity("liquidity_0_1", 1, 2, 0.995, 1.99, to, 1, 0, 0);
```
In the above example, we have increased positions for the uniswap market of `ATP-USDT`, with the amounts of `1.0000 ATP` and `2.0000 USDT` respectively. Similarly, if there is no such market, the `ATP-USDT` uniswap market will be created.

#### Market share table query：
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
qingah.api.getSwappoolRows("liquidity_a_b") // The "0" here corresponds to the corresponding currency pair primary
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
In the case of the above example, in the `uniswap` market of `USDT-ATP`, the total amount of `USDT` is `10030.0000 ATP`, the total amount of  `ATP` is `140.0000 ATP`, and the total weight of all positions is `11849.89451429842847574`.

There are two positions in the market, one is "aptuser1..." and the other is "aptuser2...". Their weight in the total market is `11356.56329871202615323` and `493.33121558640181092`.
### Remove Liquidity

**Call method:**

```javascript
let result = await qingah_client.dpos.removeLiquidity(liquidityID, rate, amountAMin, amountBMin, to, nonce, accountId, feeA, feeB);
```

**Method statement:**

Use `removeLiquidity()` method, which can be used to raise positions in a specific uniswap market and put forward our share in the uniswap market.

**Parameter description:**

| Params     | description            |
| -------- | ---------------- |
| liquidityID | Liquidity pair ID liquidity_a_b |
| rate | Remove of liquidity rate    |
| amountAMin | The minimum A amount uint  |
| amountBMin | The minimum B amount uint  |
| nonce | nonce  |
| to    |  Liquidity pair address    |
| accountId|  address ID    |
| feeA |  feeA    |
| feeB |  feeB    |

#### Limit：

- `0<rate<1`. Otherwise, an error will be reported.
- The `uniswap` must exist. Otherwise, an error will be reported.
- The user must have a share in the `uniswap` market. Otherwise, an error will be reported.
- The number of `tokens` extracted by the user must be greater than the minimum precision of the currency. Otherwise, an error will be reported.
- After user extraction, the remaining weight must be greater than 0.1% of the total weight. Otherwise, an error will be reported.

This behavior will raise the position of A-B's uniswap token market and put forward our own token share through the `rate` ratio.
```javascript
...
qingah_client.dpos.removeLiquidity('liquidity_0_1', 0.5, 0.4975, 0.995, 'aptcontractaddress', nonce, 1, 0, 0);
```
In the above example, we raised our position in the uniswap market of `ATP-USDT`, with a ratio of `0.1`. After the operation, we will extract the number of `ATP` tokens and `USDT` tokens in the market according to the proportion of 'individual weight / total weight * rate'



#### Formula description：

In the `uniswap` market, the user's share of `token` is displayed in the form of weight. When reducing positions * * can't * * set the 'token' of a certain amount to be extracted, only the extraction proportion can be set, and the corresponding currency pair can be obtained according to the corresponding proportion. The detailed formula is as follows:


{% raw %}
$$
\begin{gather}
a\times{b}=S\\
Filling warehouse:\; a_{new}\times{b_{new}}=S_{new}\\
Weight calculation：\;\frac{\sqrt{S_{new}}-\sqrt{S}}{\sqrt{S}}\times{weights}\\
Reduction ratio：\;r=\frac{weigehts_{个人}}{weights_{全部}}\times{rate}_{传入值}\\
Withdrawal from reduced warehouse：(a_{new}\times{r})\quad(b_{new}\times{r})
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

- In the market price transaction, the value of * * `amountA` must not be 0, and the values of `amountA` and `price` must be 0 * *. Otherwise, an error will be reported.
- In a price limit transaction, the value of * * price must not be zero, and both `amountA` and 'amountb' must have one and only one value * *. Otherwise, an error will be reported.
- The 'uniswap' market must exist. Responsible for reporting errors.
- The number of `tokens` exchanged by the user must be greater than the minimum precision of the currency. Otherwise, an error will be reported.

Market exchange:
```javascript
...
qingah_client.dpos.exchange(1, '200.000000', 'aUSDT', 0, 'ATP', 0, 0);
```
In the above example, the user ID `1` wants to exchange `200.000000 aUSDT` for `ATP`. No matter how much the price is, the exchange will last until `200.000000 aUSDT` is finished.


Limit exchange:
```javascript
...
qingah_client.dpos.exchange(1, "0.0000000", "aUSDT", "100.00000000", "ATP", 1.2, 0);
```
In the above example, user ID `1` needs to exchange '100 million ATP' at a price no higher than `1.2`. If the price is higher than `1.2', the part that has not been exchanged will be charged.

#### Example of list query：
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