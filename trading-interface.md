# 3.1 查询账户信息 {#accountinfo}

```
GET /v1/api/account/{account}
```

查询交易账户`account`的账户信息

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| account | yes | string | 交易帐号 |

##### Headers Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| x-api-vendor | yes | string | 交易平台：`MT4`,`MT5` |
| x-api-serverid | yes | string | 服务器ID |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _object_ | _数据内容_ |
| accountId | string | 交易账号 |
| accountName | string | 账户名称 |
| balance | double | 账户余额 |
| credit | double | 信用 |
| currency | string | 货币 |
| floatProfit | double | 浮动盈亏 |
| group | string | MT组 |
| leverage | int | 杠杆 |
| marginFree | double | 可用保证金 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/account/2090002986
```

**返回示例**

```json
{
  "data": {
    "accountId": "2090002986",
    "accountName": "Demo",
    "balance": 51092.55,
    "credit": 277.78,
    "currency": "USD",
    "floatProfit": -1.164,
    "group": "demoTB",
    "leverage": 20,
    "marginFree": 51370.33
  },
  "mcode": "m0000000",
  "result": true
}
```

# 3.2 账户入金申请 {#depositApply}

```
POST /v2/api/account/{userId}/{serverId}/{login}/deposit
```

提交交易账户`login`的入金申请

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | 用户ID |
| login | yes | string | 交易帐号 |
| depositDTO | yes | Object | 入金申请信息 |
| serverId | yes | string | 服务器编号 |

###### depositDTO

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| depositAmount | yes | double | 入金金额， 对应交易账户的金额 |
| payCurrency | yes | string | 支付货币：`CNY`,`NZD` |
| comment | no | string | 申请备注 |
| signature | yes | string | 签名，具体见下方签名规则 |

签名规则：  
签名方式：SHA1WithRSA  
参与签名的数据：userId+login+payCurrency+depositAmount  
depositAmount 必须保留两位小数

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | object | 申请信息 |
| payAmount | double | 需支付金额，根据Support Center 设置的汇率计算得出 |
| jobId | string | 申请ID |
| accountId | string | 交易账号 |

#### Example

**请求示例**

```
POST http://twapi.lwork.com/v2/api/account/980cf1bb-82e1-42b1-89b0-9120f798cd91/428/2090001464/deposit
```

```json
{
  "depositAmount":"100.00",
  "payCurrency":"CNY",
  "comment":"备注",
  "signature": "T1m2b7rAn8QHB3Y3VKX+VtbUq3fRmkIzWj69GmV2TWepMWi74wYP1V22RUNbnNacs3wG+Onfd9HLtNK1uZ8aRpeOy5yrdd02uddNYVkPQpef6a68FTcj70npbQi00RE1qSyZgmTYodeb+LodxfTWff/n4/Kz5uI2PjpzvrP5QW8="
}
```

**返回示例**

```json
{
  "data": {
    "payAmount": 682.34,
    "jobId": "63b63daa-3c1e-4fe2-a4bb-8133704f9697",
    "accountId": "2090001464"
  },
  "mcode": "m0000000",
  "result": true
}
```

# 3.3 更新入金支付状态 {#depostCallback}

```
POST /v1/api/account/deposit/callback
```

客户支付成功后，更新支付状态

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| jobId | yes | string | 入金申请ID |
| orderId | yes | string | 支付订单号 |
| status | yes | string | 支付状态，1-支付成功。支付成功才会更新，否则返回失败 |
| platform | yes | string | 支付平台 |
| payAmount | yes | string | 实际支付金额，保留两位小数 |
| payCurrency | yes | string | 实际支付货币 |
| signature | yes | string | 签名，具体见下方签名规则 |

签名规则：  
签名方式：SHA1WithRSA  
参与签名的数据：jobId+orderId+status+payCurrency+payAmount  
payAmount 必须保留两位小数

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | string | 更新信息：1：代表更新状态成功，0：代表失败 |

#### Example

**请求示例**

```
POST http://twapi.lwork.com/v1/api/account/deposit/callback
```

```json
{
  "jobId":"63b63daa-3c1e-4fe2-a4bb-8133704f9697",
  "orderId":"17051110011",
  "status":"1",
  "platform":"twAPI",
  "payAmount":"682.34",
  "payCurrency":"CNY",
  "signature":"T1m2b7rAn8QHB3Y3VKX+VtbUq3fRmkIzWj69GmV2TWepMWi74wYP1V22RUNbnNacs3wG+Onfd9HLtNK1uZ8aRpeOy5yrdd02uddNYVkPQpef6a68FTcj70npbQi00RE1qSyZgmTYodeb+LodxfTWff/n4/Kz5uI2PjpzvrP5QW8="
}
```

**返回示例**

```json
{
  "data": "1",
  "mcode": "m0000000",
  "result": true
}
```

# 3.4 账户出金申请 {#withdraw}

```
POST /v2/api/account/{userId}/{serverId}/{login}/withdraw
```

提交交易账户`login`的出金申请

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | 用户ID |
| login | yes | string | 交易帐号 |
| withdrawDTO | yes | Object | 出金信息 |
| serverId | yes | string | 服务器编号 |

###### withdrawDTO

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| currency | yes | string | 账户货币 |
| payCurrency | no | string | 出金货币 |
| withdrawAmount | yes | double | 出金金额 |
| bankAccountName | yes | string | 收款人 |
| bankAccountNumber | yes | string | 收款银行帐号 |
| bankName | yes | string | 收款银行名称 |
| bankBranchName | no | string | 分支行名称 |
| SWIFT | no | string | SWIFT代码 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |

#### Example

**请求示例**

```
POST http://twapi.lwork.com/v2/api/account/4e22daee-3618-4426-b159-85fdc68c33c5/428/2090002865/withdraw
```

```json
{
"currency":"USD",
"withdrawAmount":"100",
"bankAccountName":"Demo",
"bankAccountNumber":"6226",
"bankName":"ICBC",
"bankBranchName":""
}
```

**返回示例**

```json
{
"result": true,
"mcode":"m0000000"
}
```

# 3.5 创建真实账户 {#openaccount}

```
POST /v1/api/account/{userId}/open
```

给用户`userId` 开立真实账户

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | 用户ID |
| accountDTO | yes | object | 账户资料 |

###### accountDTO

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| vendor | yes | string | 平台名称 |
| serverId | yes | string | 服务器ID |
| group | yes | string | MT组 |
| leverage | yes | int | 杠杆 |
| accountName | yes | string | 交易账户名称 |
| email | yes | string | 邮箱 |
| phone | yes | object | 电话 |
| agentAccount | no | string | 代理账号 |
| countryCode | yes | string | 国家/地区编号 |
| phone | yes | string | 电话号码 |
| password | no | string | 客户开户时设置的账户密码 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _array_ | _数据内容_ |
| address | string | 地址 |
| city | string | 城市 |
| state | string | 省份 |
| country | string | 国家 |
| currency | string | 货币 |
| email | string | 邮箱 |
| enable | boolean | 启用状态 |
| enableChangePass | boolean | 是否允许修改密码 |
| group | boolean | MT组 |
| leverage | int | 杠杆 |
| login | int | 帐号 |
| name | string | 账户名称 |
| phone | string | 电话 |

#### Example

**请求示例**

```
POST http://twapi.lwork.com/v1/api/account/4b07e7af-798a-4f68-9b2c-561a8300eac2/open

{
"vendor":"MT4",
"serverId":"428",
"group":"demoforex",
"leverage":50,
"accountName":"demo",
"phone":{
"countryCode":"86",
"phone":"18602920465"
},
"email":"demo@lwork.com"
}
```

**返回示例**

```json
{
"data": {
"address": "",
"city": "",
"country": "",
"currency": "USD",
"email": "demo@lwork.com",
"enable": 1,
"enableChangePass": 1,
"group": "demoforex",
"leverage": 50,
"login": 2090005510,
"name": "demo",
"phone": "86 123456",
"state": ""
},
"mcode": "m0000000",
"result": true
}
```

# 3.6 创建模拟账户-手机 {#opendemoaccount}

```
    POST /v1/api/account/{pubUserId}/sms/open/demo
```

创建模拟账户并发送手机短信

#### Request Parameters

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| demoAccountType | yes | string | 模拟账户类型ID\(通过基础接口-&gt;查询模拟账户类型设置获取\) |
| vendor | yes | string | 平台名称 MT4,MT5 |
| accountName | yes | string | 账户名称 |
| phone | yes | string | 用户手机号 |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | array | 数据内容 |

#### Example

**请求示例**

```
POST /v1/api/account/37848f89-257a-4005-b0b4-88f1a31005a5/sms/open/demo
{
    "demoAccountType": "2e852672-dd6e-4ede-a6dc-7108c234dcd7",
    "vendor": "MT4",
    "accountName": "SmsDemo01",
    "phone": "1879342xxxx"
}
```

**返回示例**

```json
{
    "mcode": "m0000000",
    "result": true
}
```

# 3.7 查询资金流水 {#fund}

```
GET /v1/api/account/{userId}/{login}/cashflow?from={dateFrom}&to={dateTo}&type={type}&page={pageIndex}&size={pageSize}
```

查询交易账户`login`的资金流水

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | 用户ID |
| login | yes | string | 交易帐号 |
| dateFrom | yes | string | 起始日期，格式：YYYY-MM-DD |
| dateTo | yes | string | 截止日期，格式：YYYY-MM-DD |
| type | yes | string | 类型：`Deposit`、`Withdraw`、`Transfer` |
| pageIndex | yes | int | 页码 |
| pageSize | yes | int | 每页条数 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _object_ | _数据内容_ |
| list | array | 资金流水记录 |
| transactionType | string | 类型 |
| depositTransaction | object |  |
| accountId | string | 交易账号 |
| comment | string | 备注 |
| createTime | int | 创建时间timestamp |
| depositAmount | double | 存款金额 |
| depositCurrency | string | 存款货币 |
| id | string | 申请ID |
| payAmount | double | 支付金额 |
| payCurrency | string | 支付货币 |
| payStatus | string | 支付状态 |
| state | string | 申请状态 |
| offset | int |  |
| pager | int | 当前页数 |
| pages | int | 总页数 |
| size | int | 每页条数 |
| total | int | 总条数 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/account/4b07e7af-798a-4f68-9b2c-561a8300eac2/2090001252/cashflow?from=2016-01-01&to=2017-09-01&type=Deposit&page=1&size=10
```

**返回示例**

```json
{
"data": {
"list": [
{
"depositTransaction": {
"accountId": "2090001252",
"comment": "",
"createTime": 1491494693521,
"depositAmount": 100,
"depositCurrency": "USD",
"id": "00000139",
"payAmount": 693.4,
"payCurrency": "CNY",
"payStatus": "Pending",
"state": "Submited"
},
"transactionType": "Deposit"
}
],
"offset": 0,
"pager": 1,
"pages": 1,
"size": 10,
"total": 1
},
"mcode": "m0000000",
"result": true
}
```

# 3.8 查询交易历史 {#history}

```
GET /v1/api/account/{userId}/{login}/history?from={dateFrom}&to={dateTo}&page={pageIndex}&size={pageSize}
```

查询交易账户`login`的交易历史

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | 用户ID |
| login | yes | string | 交易帐号 |
| dateFrom | yes | string | 起始日期，格式：YYYY-MM-DD |
| dateTo | yes | string | 截止日期，格式：YYYY-MM-DD |
| pageIndex | yes | int | 页码 |
| pageSize | yes | int | 每页条数 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _object_ | _数据内容_ |
| list | array | 交易历史记录 |
| account | string | 交易账号 |
| closePrice | double | 平仓价格 |
| closeTime | string | 平仓时间timestamp |
| commission | double | 佣金 |
| id | string | 订单号 |
| openPrice | double | 开仓价格 |
| openTime | string | 开仓时间 |
| profit | double | 盈利 |
| serverId | string | 服务器ID |
| stopLoss | double | 止损价格 |
| swap | double | 利息 |
| symbol | string | 品种 |
| takeProfit | double | 止盈价格 |
| type | string | 交易类型 |
| vendor | string | 交易平台 |
| volume | double | 交易量 |
| offset | int |  |
| pager | int | 当前页数 |
| pages | int | 总页数 |
| size | int | 每页条数 |
| total | int | 总条数 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/account/4e22daee-3618-4426-b159-85fdc68c33c5/2090002986/history?from=2017-01-01&to=2017-09-01&page=1&size=10
```

**返回示例**

```json
{
  "data": {
    "list": [
      {
        "account": "2090002986",
        "closePrice": 1.07709,
        "closeTime": "2017-03-24T07:49:27.000",
        "commission": -13,
        "id": "12132047",
        "openPrice": 1.07703,
        "openTime": "2017-03-24T07:49:19.000",
        "profit": -6,
        "serverId": "428",
        "stopLoss": 0,
        "swaps": 0,
        "symbol": "EURUSD",
        "takeProfit": 0,
        "type": "Sell",
        "vendor": "MT4",
        "volume": 1
      }
    ],
    "offset": 0,
    "pager": 1,
    "pages": 1,
    "size": 10,
    "total": 1
  },
  "mcode": "m0000000",
  "result": true
}
```

# 3.9 查询挂单记录 {#order}

```
GET /v1/api/account/{userId}/{login}/pending?from={dateFrom}&to={dateTo}&page={pageIndex}&size={pageSize}
```

查询交易账户`login`的挂单记录

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | 用户ID |
| login | yes | string | 交易帐号 |
| dateFrom | yes | string | 起始日期，格式：YYYY-MM-DD |
| dateTo | yes | string | 截止日期，格式：YYYY-MM-DD |
| pageIndex | yes | int | 页码 |
| pageSize | yes | int | 每页条数 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _object_ | _数据内容_ |
| list | array | 挂单记录 |
| account | string | 交易账号 |
| executionTime | string |  |
| id | string | 订单号 |
| orderPrice | double | 挂单价格 |
| orderTime | string | 挂单时间 |
| serverId | string | 服务器ID |
| stopLoss | double | 止损价格 |
| symbol | string | 品种 |
| takeProfit | double | 止盈价格 |
| type | string | 挂单类型 |
| vendor | string | 交易平台 |
| volume | double | 交易量 |
| offset | int |  |
| pager | int | 当前页数 |
| pages | int | 总页数 |
| size | int | 每页条数 |
| total | int | 总条数 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/account/4e22daee-3618-4426-b159-85fdc68c33c5/2090002986/pending?from=2017-01-01&to=2017-09-01&page=1&size=10
```

**返回示例**

```json
{
  "data": {
    "list": [
      {
        "account": "2090002986",
        "executionTime": "1970-01-01T08:00:00.000",
        "orderPrice": 1.05984,
        "orderTime": "2017-04-08T01:21:54.000",
        "id": "12132047",
        "serverId": "428",
        "stopLoss": 0,
        "symbol": "EURUSD",
        "takeProfit": 0,
        "type": "BuyLimit",
        "vendor": "MT4",
        "volume": 1
      }
    ],
    "offset": 0,
    "pager": 1,
    "pages": 1,
    "size": 10,
    "total": 1
  },
  "mcode": "m0000000",
  "result": true
}
```

# 3.10 查询持仓记录 {#position}

```
GET /v1/api/account/{userId}/{login}/position?from={dateFrom}&to={dateTo}&page={pageIndex}&size={pageSize}
```

查询交易账户`login`的持仓记录

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | 用户ID |
| login | yes | string | 交易帐号 |
| dateFrom | yes | string | 起始日期，格式：YYYY-MM-DD |
| dateTo | yes | string | 截止日期，格式：YYYY-MM-DD |
| pageIndex | yes | int | 页码 |
| pageSize | yes | int | 每页条数 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _object_ | _数据内容_ |
| list | array | 挂单记录 |
| account | string | 交易账号 |
| commission | double | 佣金 |
| id | string | 订单号 |
| openPrice | double | 开仓价格 |
| openTime | string | 开仓时间 |
| profit | double | 盈亏 |
| serverId | string | 服务器ID |
| stopLoss | double | 止损价格 |
| swap | double | 利息 |
| symbol | string | 品种 |
| takeProfit | double | 止盈价格 |
| type | string | 交易类型 |
| vendor | string | 交易平台 |
| volume | double | 交易量 |
| offset | int |  |
| pager | int | 当前页数 |
| pages | int | 总页数 |
| size | int | 每页条数 |
| total | int | 总条数 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/account/4e22daee-3618-4426-b159-85fdc68c33c5/2090002986/position?from=2017-01-01&to=2017-09-01&page=1&size=10
```

**返回示例**

```json
{
  "data": {
    "list": [
      {
        "account": "2090001479",
        "commission": 0,
        "id": "12212503",
        "openPrice": 1255.28,
        "openTime": "2017-04-08T00:41:49.000",
        "profit": -0.1,
        "serverId": "428",
        "stopLoss": 0,
        "swap": 0,
        "symbol": "XAUUSD",
        "takeProfit": 0,
        "tenantId": "T001117",
        "type": "Buy",
        "vendor": "MT4",
        "volume": 0.1
      },
      {
        "account": "2090001479",
        "commission": -9.8,
        "id": "12212502",
        "openPrice": 1.05984,
        "openTime": "2017-04-08T00:41:38.000",
        "profit": -7.84,
        "serverId": "428",
        "stopLoss": 0,
        "swap": 0,
        "symbol": "EURUSD",
        "takeProfit": 0,
        "tenantId": "T001117",
        "type": "Buy",
        "vendor": "MT4",
        "volume": 0.98
      },
      {
        "account": "2090001479",
        "commission": -0.1,
        "id": "12212501",
        "openPrice": 1.05984,
        "openTime": "2017-04-08T00:41:12.000",
        "profit": -0.08,
        "serverId": "428",
        "stopLoss": 0,
        "swap": 0,
        "symbol": "EURUSD",
        "takeProfit": 0,
        "tenantId": "T001117",
        "type": "Buy",
        "vendor": "MT4",
        "volume": 0.01
      },
      {
        "account": "2090001479",
        "commission": -0.1,
        "id": "12212500",
        "openPrice": 1.05984,
        "openTime": "2017-04-08T00:41:03.000",
        "profit": -0.08,
        "serverId": "428",
        "stopLoss": 0,
        "swap": 0,
        "symbol": "EURUSD",
        "takeProfit": 0,
        "tenantId": "T001117",
        "type": "Buy",
        "vendor": "MT4",
        "volume": 0.01
      }
    ],
    "offset": 0,
    "pager": 1,
    "pages": 1,
    "size": 10,
    "total": 4
  },
  "mcode": "m0000000",
  "result": true
}
```

# 3.11 账户转账申请 {#accountTransfer}

```
    POST /v1/api/account/{pubUserId}/{accountId}/transfer
```

同一用户下相同平台账户之间转账

#### Request Parameters

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| pubUserId | yes | string | 转出用户userId |
| accountId | yes | string | 转出账户ID |

#### Request Body

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| receiptAccount | yes | string | 转入账户ID |
| receiptAccountName | yes | string | 转入账户名字 |
| receiptServerId | yes | string | 转入账户所在服务器ID |
| receiptUserId | yes | string | 转入用户userId |
| receiptVendor | yes | string | 平台名称 MT4,MT5 |
| currency | yes | string | 转出账户的交易货币 |
| transferAmount | yes | double | 转出账户转出金额 |
| comment | no | string | 附加信息 |
| signature | yes | string | 签名信息 |

签名规则： 签名方式：SHA1WithRSA 参与签名的数据：accountId + currency  + pubUserId + receiptAccount + receiptAccountName +  receiptServerId +  receiptVendor + transferAmount   其中transferAmount 必须保留两位小数

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | array | 数据内容 |

#### Example

**请求示例**

```
POST /v1/api/account/37848f89-257a-4005-b0b4-88f1a31005a5/1234580040/transfer
{
    "receiptAccount": "1234580207",
    "receiptAccountName": "想回家回家~",
    "receiptServerId": "429",
    "receiptUserId": "37848f89-257a-4005-b0b4-88f1a31005a5",
    "receiptVendor": "MT4",
    "currency": "USD",
    "transferAmount": "100.01",
    "comment": "",
    "signature" :"xxxxxxxxx"
}
```

**返回示例**

```
{
    "mcode": "m0000000",
    "result": true
}
```

# 3.12 修改账户所有人资料基础信息 {#modifyBasicInfo}

```
POST /v1/api/account/{pubUserId}/update/diff/base
```

#### Request Body

Name为账户所有人baseInfo中对应的key

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| accountName | no | string | 账户名称 |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | Void | 数据内容 |

#### Example

#### 请求示例

```
POST /v1/api/account/934fabf1-213e-4cef-96ba-ea69f43a0639/update/diff/base
{
"accountName":"test"
}
```

#### 返回示例

```
{
    "mcode": "m0000000",
    "result": true
}
```

# 3.13 修改账户所有人资料财务信息 {#modifyFinancialInfo}

```
POST /v1/api/account/{pubUserId}/update/diff/finance
```

#### Request Body

Name为账户所有人financeInfo中对应的key

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| accountNo | no | string | 银行账号 |
| bankAccount | no | string | 开户银行 |
| bankBranch | no | string | 银行支行名称 |
| bankAddress | no | json | 银行地址 |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | Void | 数据内容 |

#### Example

#### 请求示例

```
POST /v1/api/account/934fabf1-213e-4cef-96ba-ea69f43a0639/update/diff/finance
{
accountNo : "xxxx"
}
```

#### 返回示例

```
{
    "mcode": "m0000000",
    "result": true
}
```

# 3.14 修改账户所有人资料账户证件信息 {#modifyIdentityInfo}

```
POST /v1/api/account/{pubUserId}/update/diff/certificate
```

#### Request Body

Name为账户所有人certificateInfo中对应的key

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| idNum | no | string | 身份证号吗 |
| idType | no | string | 身份证类型 |
| idUrl1 | no | string | 身份证明A |
| idUrl2 | no | json | 身份证明B |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | Void | 数据内容 |

#### Example

#### 请求示例

```
POST /v1/api/account/934fabf1-213e-4cef-96ba-ea69f43a0639/update/diff/certificate
{
idNo : "xxxx"
}
```

#### 返回示例

```
{
    "mcode": "m0000000",
    "result": true
}
```

# 3.15 根据SC配置开真实账户 {#scConfiguration}

```
POST /v2/api/account/{userId}/open
给用户userId 开立真实账户
```

#### Request Body

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| serverId | yes | string | 服务器ID |
| vendor | yes | string | 平台名称 |
| accountInfo | yes | object | 账户资料 |
| baseInfo | yes | object | 账户基础信息（字段来自1.9接口获取都第1步数据） |
| financialInfo | yes | object | 账户财务信（字段来自1.9接口获取都第2步数据） |
| certificatesInfo | yes | object | 账户身份信息（字段来自1.9接口获取都第3步数据） |

#### accountInfo

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| group | yes | string | MT组 |
|  |  |  |  |
| leverage | yes | string | 杠杆 |
| login | no | object | 交易账号 |
| enable | yes | object | 启用状态 0/1 |
| readOnly | yes | object | 只读状态 0/1 |
| password | no | string | 主密码 |
| investorPassword | no | string | 投资密码 |
| comment | no | string | 备注 |
| email | yes | string | 邮箱 |
| phone | yes | object | 电话 |



