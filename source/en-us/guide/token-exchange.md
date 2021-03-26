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

**Method statement:**

Using the `exchange()` method, qingah uses Bancor / uniswap to exchange tokens.

The interface supports both Bancor protocol and uniswap protocol.

**Parameter description:**

| Params     | Description             |
| -------- | ---------------- |
| accountId    | Exchange account number |
| amountA | Number of tokens exchanged    |
| tokenA | Exchange token     |
| amountB| Target number of exchange token |
| tokenB | Exchange target Token     |
| price| Price (only fill in the value in uniswap price limit transaction) |
| id | Channel ID (only used in uniswap transactions) |

**It should be noted that in Bancor exchange, ` to 'fills in the value of 0 for the target Token. Assuming that I need to exchange ATP, I can fill in' 0.0000 ATP ','price' fills in '0.0','id 'fills in' owner '.**

Suppose that the user `atp1t88x1...` issues a smart token called `ABTC`, and the user `atp1t88x2...` issues a smart token called ` BETH `. So how does the user `atp1t88x1...` want to get a certain amount of `BETH` with `ABTC`? Call the exchange interface to exchange.



```javascript
// Init QinGah client
...
let ctx = qingah_client.contract('qingah-swap');
let result = ctx.exchange('atp1t88x1...', 'atpaBTC...', 'atpbETH...', 1.0000, 0.0000, '0.0', 'atp1t88x2...');
```

If you want to exchange for the `bETh` token in `QinGah`, you just need to exchange `0.0000 a ETH` for `0.0000 bETH`.

## uniswap exchange

👉 read【[ uniswap exchange](token-uniswap.html)】part.


## One click exchange of wallet

[Download QinGah Wallet](https://wallet.qingah.com/). First select the source smart token you want to redeem, enter the quantity you want to redeem, then select the target token you want to redeem, click redeem, and wait for a moment to redeem successfully.