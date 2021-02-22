---
title: 
type: rpc
language: zh-cn
order: 1
---
# RPC API


## REST

### getLiquidityRows

返回流动池对。

```
GET http://api.qingah.com/v1/api/getLiquidityRows
```

示例：

```
curl --request GET \
  --url  http://api.qingah.com/v1/api/getLiquidityRows
```

执行结果：

```
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

### getSwappoolRows

返回Swap池底仓填充列表。

```
GET http://api.qingah.com/v1/api/getSwappoolRows
```

示例：

```
curl --request GET \
  --url  http://api.qingah.com/v1/api/getSwappoolRows
```

执行结果：

```
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

### tokenpair

返回包含交易对数据列表。

```
POST http://api.qingah.com/v1/tokenpair/batchgetSwapRankOnChain
```

示例：

```
curl --request POST \
  --url http://api.qingah.com/v1/tokenpair/batchgetSwapRankOnChain \
  --data '{"tokenpairs":["USDT@atp../ETH@atp..","ATP@atp.../USDT@atp..."]}'
```

**主体参数**

| 参数                | 类型   | 描述                                     |          |
| ------------------- | ------ | ---------------------------------------- | -------- |
| **tokenpairs** | array[string] | 交易对数组 | REQUIRED |

执行结果：

```
[
  {
    "data": [
      {
        "status": "unlocked",
        "account": "atpmy1024...",
        "tokenx": "USDT@atp1h3y5wt82z09amcd4mrhpz4jt543t5r9td9v49s",
        "tokeny": "ETH@atp16lellpkrv894hmg8am7ns3p2qny2vqj85ud8s6",
        "weight": "10000.00000000000000000",
        "tokenx_quantity": "5.884095",
        "tokeny_quantity": "0.00347109"
      },
      {
        "status": "unlocked",
        "account": "atpmy1024...",
        "tokenx": "ATP",
        "tokeny": "USDT@atp1h3y5wt82z09amcd4mrhpz4jt543t5r9td9v49s",
        "weight": "10000.00000000000000000",
        "tokenx_quantity": "5.884095",
        "tokeny_quantity": "0.00347109"
      }
    ]
  }
]                                                                                
```

### getDepthmap

返回包含交易对深度数据。

```
POST http://api.qingah.com/v1/app/getDepthmap
```

示例：

```
curl --request POST \
  --url http://api.qingah.com/v1/app/getDepthmap \
  --data '{"tokena":"atp","tokenb":"usdt","limit":24}'
```

**主体参数**

| 参数                | 类型   | 描述                                     |          |
| ------------------- | ------ | ---------------------------------------- | -------- |
| **tokena** | string | A令牌 | REQUIRED |
| **tokenb** | string | A令牌 | REQUIRED |
| **limit** | int | 限制数 | REQUIRED |


执行结果：

```
{
  "buy":[
    {
      "price":"0.006526",
      "totalamount":"309323.4682",
      "sum_quantity":"14491.7581",
      "totalprice":"94.573214"
    },
    {
      "price":"0.006574",
      "totalamount":"294831.7101",
      "sum_quantity":"14333.9045",
      "totalprice":"94.231089"
    },
    {"price":"0.006622",
    "totalamount":"280497.8056",
    "sum_quantity":"14178.8960",
    "totalprice":"93.892650"
    },
    ...
  ],
  "sell":[
    {
      "price":"0.007726",
      "totalamount":"11363.6283",
      "sum_quantity":"11363.6283",
      "totalprice":"87.795393"
    },
    {
      "price":"0.007774",
      "totalamount":"22623.3961",
      "sum_quantity":"11259.7678",
      "totalprice":"87.533435"
    },
    ...
  ],
  "lastprice":"0.007679"
}                                                                         
```

### kdata

返回包含交易对时间范围K线数据。

```
POST http://api.qingah.com/v1/deal/kdata
```

示例：

```
curl --request POST \
  --url http://api.qingah.com/v1/deal/kdata \
  --data '{"duration":"24h","token_names":["ETH/USDT","ATP/USDT"],"stime":1613823575412,"etime":1613909975412}'
```

**主体参数**

| 参数                | 类型   | 描述                                     |          |
| ------------------- | ------ | ---------------------------------------- | -------- |
| **duration** | string | 时间范围 y,m,w,d,h,m | REQUIRED |
| **token_names** | array[string] | 交易对 | REQUIRED |
| **stime** | timestamp | 开始时间戳 | REQUIRED |
| **etime** | timestamp | 结束时间戳 | REQUIRED |

执行结果：

```
[
  {
    "dateLists": {
      "24h": {
        "nprice": "1693.301424819160677378",
        "quantitys": "0.00910037",
        "oprice": "1689.520005034077784808",
        "cprice":"1693.301424819160677378",
        "hprice":"1693.301424819160677378",
        "lprice":"1689.520005034077784808"
      }
    }
  },
  {
    "dateLists": {
      "24h": {
        "nprice": "0.007675344",
        "quantitys": "2000",
        "oprice": "0.007809735",
        "cprice":"0.007675344",
        "hprice": "0.007809735",
        "lprice":"0.007675344"
      }
    }
  }
]                                                                             
```

## CHAIN

### get_balance

返回链上AccountID的token balances。

```
POST http://api.qingah.com/v1/chain/get_balance
```
示例：

```
curl --request POST \
  --url http://api.qingah.com/v1/chain/get_balance \
  --data '{"account_id": "5"}'
```

**主体参数**

| 参数                | 类型   | 描述                                     |          |
| ------------------- | ------ | ---------------------------------------- | -------- |
| **account_id** | string | Provide a `account_id` | REQUIRED |

执行结果：

```
[
  {
    tid: 1,
    balance: 1000,
  },
    {
    tid: 2,
    balance: 1000,
  }
]

```

## GRAPHQL

> 注意：需要按照需求自行定制查询

### find_order

根据订单列表。

```
POST http://api.qingah.com/v1/graphql
```

示例：

```
curl --request POST \
  --url http://api.qingah.com/v1/graphql \
  --data `{
    find_order(
      where:{
        tokenpair_id:{
          eq: "1"
        }
        
        account_id: {
            eq: "1"
          }
        
        status:{
          eq: "waiting"
        }
      },
      order: "-created"
    ){
      id,
      direction,
      status,
      price,
      account {
        id
      },
      tokenpair {
        id,
        tokena {
          id,
          precision,
          cw_name,
          connector_weight,
          position,
          created,
        },
        tokenb {
          id,
          precision,
          cw_name,
          connector_weight,
          position,
          created,
        }
        type,
        created,
      },
      tokena_quantity,
      tokenb_quantity,
      dealled,
      created,
    }
  }`
```

**主体参数**

| 参数           | 类型  |      |
| -------------- | ----- | ---- |
| **data** | GRAPHQL | /    |

执行结果：

```
{
  "data": {
    "find_order":
    [
      {
        "id":"11705881998367995034",
        "direction":
        "buy","status":
        "waiting",
        "price":"1",
        "account": {"id":"1"},
        "tokenpair":
        {
          "id":1,
          "tokenx": {
            "id": "2",
            "precision":"100000000000.000000",
            "cw_name":"USDT",
            "connector_weight":"0",
            "position":"1",
            "created":"2019-08-28T08:11:20.000Z"
          },
          "tokenb": {
            "id":"1",
            "precision":"1059467298.6730",
            "cw_name":"ATP",
            "connector_weight":"0",
            "position":"1",
            "created":"2019-08-28T08:11:20.000Z"
          },
          "type":"uniswap",
          "created":"2019-08-19T01:49:22.000Z"
        },
        "tokena_quantity":"0.000000",
        "tokenb_quantity":"100.0000",
        "dealled":"0",
        "created":"2021-02-17T14:58:30.000Z"
      }
    ]
  }
}

```

