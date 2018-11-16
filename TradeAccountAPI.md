# 3.1 Query account info {#accountinfo}

```
GET /v1/api/account/{account}
```

Query trading account `account` account info

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| account | yes | string | Trading account |

##### Headers Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| x-api-vendor | yes | string | Trading platform：`MT4`,`MT5` |
| x-api-serverid | yes | string | Platform ID |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _object_ | _Data Content_ |
| accountId | string | Trading account |
| accountName | string | Account name |
| balance | double | Account balance |
| credit | double | Credit |
| currency | string | Currency |
| floatProfit | double | float P/L |
| group | string | MT Group |
| leverage | int | Leverage |
| marginFree | double | Available Balance |

#### Example

**Request Sample**

```
GET http://twapi.lwork.com/v1/api/account/2090002986
```

**Return Sample**

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

# 3.2 Account deposit application {#depositApply}

```
POST /v2/api/account/{userId}/{serverId}/{login}/deposit
```

Submit trading account `login`deposit application

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | User ID |
| login | yes | string | Trading account |
| depositDTO | yes | Object | Deposit application |
| serverId | yes | string | Server Number |

###### depositDTO

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| depositAmount | yes | double | Deposit Amount， Amount corresponding to the trading account |
| payCurrency | yes | string | Payment currency：`CNY`,`NZD` |
| comment | no | string | Application comment |
| signature | yes | string | Signature，signature as below |

Signature rule：  
Signature method：SHA1WithRSA  
Data in Signature：userId+login+payCurrency+depositAmount  
depositAmount: Must keep two decimals 

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | Request success or not |
| mcode | string | Error info |
| data | object | App info |
| payAmount | double | Need pay amount，based on currency rate setting in Support Center |
| jobId | string | Application ID |
| accountId | string | Trading account |

#### Example

**Request Sample**

```
POST http://twapi.lwork.com/v2/api/account/980cf1bb-82e1-42b1-89b0-9120f798cd91/428/2090001464/deposit
```

```json
{
  "depositAmount":"100.00",
  "payCurrency":"CNY",
  "comment":"Comment",
  "signature": "T1m2b7rAn8QHB3Y3VKX+VtbUq3fRmkIzWj69GmV2TWepMWi74wYP1V22RUNbnNacs3wG+Onfd9HLtNK1uZ8aRpeOy5yrdd02uddNYVkPQpef6a68FTcj70npbQi00RE1qSyZgmTYodeb+LodxfTWff/n4/Kz5uI2PjpzvrP5QW8="
}
```

**Return Sample**

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

# 3.3 New deposit payment status {#depostCallback}

```
POST /v1/api/account/deposit/callback
```

After pay success, renew the status of payment 


#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| jobId | yes | string | Deposit apply ID |
| orderId | yes | string | Order ID |
| status | yes | string | payment status，1-pay success。Only renew when pay success，otherwise return failure |
| platform | yes | string | payment platform |
| payAmount | yes | string | actual payment ，keep two decimals |
| payCurrency | yes | string | Actual pay currency |
| signature | yes | string | Signature，rule shows as below |

Signature rule：  
Signature method：SHA1WithRSA  
Data in signature：jobId+orderId+status+payCurrency+payAmount  
payAmount must keep two decimals

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | Request if success or not |
| mcode | string | Failure or not |
| data | string | update info：1：success，0：failed |

#### Example

**Request Sample**

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

**Return Sample**

```json
{
  "data": "1",
  "mcode": "m0000000",
  "result": true
}
```

# 3.4 Account withdraw application {#withdraw}

```
POST /v2/api/account/{userId}/{serverId}/{login}/withdraw
```

Provide trading account `login`withdraw application

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | User ID |
| login | yes | string | Trading Account |
| withdrawDTO | yes | Object | withdraw info |
| serverId | yes | string | Server ID |

###### withdrawDTO

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| currency | yes | string | Account currency |
| payCurrency | no | string | Withdraw currency |
| withdrawAmount | yes | double | Withdraw amount |
| bankAccountName | yes | string | Payee |
| bankAccountNumber | yes | string | Bank account number |
| bankName | yes | string |Bank name |
| bankBranchName | no | string | Bank branch name |
| SWIFT | no | string | SWIFT code |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | Request if success or not |
| mcode | string | Error info |

#### Example

**Request Sample**

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

**Return Sample**

```json
{
"result": true,
"mcode":"m0000000"
}
```

# 3.5 Create real account {#openaccount}

```
POST /v1/api/account/{userId}/open
```

Help user `userId` open real account 

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | User ID |
| accountDTO | yes | object | Account Info |

###### accountDTO

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| vendor | yes | string | Platform Name |
| serverId | yes | string | Server ID |
| group | yes | string | MT Group |
| leverage | yes | int | Leverage |
| accountName | yes | string | Trading account name |
| email | yes | string | Email |
| phone | yes | object | Phone |
| agentAccount | no | string | Agent account |
| countryCode | yes | string | Country/Province Code |
| phone | yes | string | Mobile |
| password | no | string | The password that the client sets when opening an account |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _array_ | _Data Content_ |
| address | string | Address |
| city | string | City |
| state | string | Province |
| country | string | Country |
| currency | string | Currency |
| email | string | Email |
| enable | boolean | Enable status |
| enableChangePass | boolean | If allowed to modify the password |
| group | boolean | MT Group |
| leverage | int | Leverage |
| login | int | Account |
| name | string | Account Name |
| phone | string | Phone |

#### Example

**Request Sample**

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

**Return Sample**

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

# 3.6 Create Demo Account-Mobiile {#opendemoaccount}

```
    POST /v1/api/account/{pubUserId}/sms/open/demo
```

Create demo account and send SMS

#### Request Parameters

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| demoAccountType | yes | string | Demo account type ID\(Through the base interface -&gt;Query demo account setting \) |
| vendor | yes | string | platform name  MT4,MT5 |
| accountName | yes | string | account name  |
| phone | yes | string | user mobile |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean |Request success or not|
| mcode | string |Error info |
| data | array |Data content|

#### Example

**Request Sample**

```
POST /v1/api/account/37848f89-257a-4005-b0b4-88f1a31005a5/sms/open/demo
{
    "demoAccountType": "2e852672-dd6e-4ede-a6dc-7108c234dcd7",
    "vendor": "MT4",
    "accountName": "SmsDemo01",
    "phone": "1879342xxxx"
}
```

**Return Sample**

```json
{
    "mcode": "m0000000",
    "result": true
}
```

# 3.7 Query journal account of capital  {#fund}

```
GET /v1/api/account/{userId}/{login}/cashflow?from={dateFrom}&to={dateTo}&type={type}&page={pageIndex}&size={pageSize}
```

Query trading account `login`journal account 

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | User ID |
| login | yes | string |Trading account |
| dateFrom | yes | string | Start Date，format：YYYY-MM-DD |
| dateTo | yes | string | End Date，format：YYYY-MM-DD |
| type | yes | string | Type：`Deposit`、`Withdraw`、`Transfer` |
| pageIndex | yes | int | Page Number |
| pageSize | yes | int | Items per page |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not _ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _object_ | _Data content_ |
| list | array | Journal account of capital |
| transactionType | string | Type |
| depositTransaction | object |  |
| accountId | string | Trading account |
| comment | string | Comment |
| createTime | int | Create time timestamp |
| depositAmount | double | Deposit amount |
| depositCurrency | string |Deposit currency |
| id | string | Application ID |
| payAmount | double | Pay amount |
| payCurrency | string | Pay currency |
| payStatus | string | Pay status |
| state | string | Apply status |
| offset | int |  |
| pager | int | Current page |
| pages | int | Total pages|
| size | int | Items per page |
| total | int | Total items |

#### Example

**Request sample**

```
GET http://twapi.lwork.com/v1/api/account/4b07e7af-798a-4f68-9b2c-561a8300eac2/2090001252/cashflow?from=2016-01-01&to=2017-09-01&type=Deposit&page=1&size=10
```

**Return Sample**

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

# 3.8 Query trading history{#history}

```
GET /v1/api/account/{userId}/{login}/history?from={dateFrom}&to={dateTo}&page={pageIndex}&size={pageSize}
```

Query trading account`login`trading history

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | User ID |
| login | yes | string | Trading account |
| dateFrom | yes | string | Start Date，format：YYYY-MM-DD |
| dateTo | yes | string | End Date，format：YYYY-MM-DD |
| pageIndex | yes | int | Page Number |
| pageSize | yes | int | Items per page |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _object_ | _Data content_ |
| list | array | Trading history record |
| account | string | Trading account |
| closePrice | double | Close price |
| closeTime | string | Close time timestamp |
| commission | double | Commission |
| id | string | Order ID |
| openPrice | double | Open position price |
| openTime | string | Open position time |
| profit | double | Profit |
| serverId | string | Server ID |
| stopLoss | double | Stop loss price |
| swap | double | Swap |
| symbol | string | Symbol |
| takeProfit | double | Take profit price |
| type | string | Trading type |
| vendor | string | Trading platform |
| volume | double | Trading volume |
| offset | int |  |
| pager | int | Current page |
| pages | int | Total page |
| size | int | Item per page |
| total | int | Total Items |

#### Example

**Request Sample**

```
GET http://twapi.lwork.com/v1/api/account/4e22daee-3618-4426-b159-85fdc68c33c5/2090002986/history?from=2017-01-01&to=2017-09-01&page=1&size=10
```

**Return Sample**

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

# 3.9 Query pending order record {#order}

```
GET /v1/api/account/{userId}/{login}/pending?from={dateFrom}&to={dateTo}&page={pageIndex}&size={pageSize}
```

Query trading account `login`pending order record

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | User ID |
| login | yes | string | Trading account |
| dateFrom | yes | string | Start Date，format：YYYY-MM-DD |
| dateTo | yes | string | End Date，format：YYYY-MM-DD |
| pageIndex | yes | int | Page number |
| pageSize | yes | int | Item per page |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | Error info |
| _data_ | _object_ | _Data content_ |
| list | array | pending order record |
| account | string | trading account |
| executionTime | string |  |
| id | string | order ID |
| orderPrice | double | pending order price |
| orderTime | string | pending order time |
| serverId | string | Server ID |
| stopLoss | double | Stop loss price |
| symbol | string | Symbol |
| takeProfit | double | Take profit price |
| type | string | pending order type |
| vendor | string | trading platform |
| volume | double | trading volume |
| offset | int |  |
| pager | int | Current pages |
| pages | int | Total pages |
| size | int | Item per page |
| total | int | Total Items |

#### Example

**Request sample**

```
GET http://twapi.lwork.com/v1/api/account/4e22daee-3618-4426-b159-85fdc68c33c5/2090002986/pending?from=2017-01-01&to=2017-09-01&page=1&size=10
```

**Return sample**

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

# 3.10 Query taking position record {#position}

```
GET /v1/api/account/{userId}/{login}/position?from={dateFrom}&to={dateTo}&page={pageIndex}&size={pageSize}
```

Query trading account `login`take position record

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | User ID |
| login | yes | string | Trading account |
| dateFrom | yes | string | Start Date，format：YYYY-MM-DD |
| dateTo | yes | string | End Date，format：YYYY-MM-DD |
| pageIndex | yes | int | Page Number |
| pageSize | yes | int | Items per page |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | Error info |
| _data_ | _object_ | _Data content_ |
| list | array | pending order record |
| account | string | trading account |
| commission | double | commission |
| id | string | Order ID |
| openPrice | double | Open position price |
| openTime | string | Open position time |
| profit | double | Profit/Loss |
| serverId | string | Server ID |
| stopLoss | double | Stop loss price |
| swap | double | Swap |
| symbol | string | symbol |
| takeProfit | double | take profit price |
| type | string | trading type |
| vendor | string | trading platform |
| volume | double | trading volume |
| offset | int |  |
| pager | int | current page |
| pages | int | Total pages |
| size | int | Items per page |
| total | int | Total Items |

#### Example

**Request sample **

```
GET http://twapi.lwork.com/v1/api/account/4e22daee-3618-4426-b159-85fdc68c33c5/2090002986/position?from=2017-01-01&to=2017-09-01&page=1&size=10
```

**Return sample**

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

# 3.11 Account transfer application {#accountTransfer}

```
    POST /v1/api/account/{pubUserId}/{accountId}/transfer
```

 Transfer between the same platform accounts under the same user

#### Request Parameters

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| pubUserId | yes | string | From the user userId |
| accountId | yes | string | From the account ID |

#### Request Body

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| receiptAccount | yes | string | To the account ID |
| receiptAccountName | yes | string | To the account name |
| receiptServerId | yes | string | To the serverID where the account is located|
| receiptUserId | yes | string | To the user userId |
| receiptVendor | yes | string | platform name MT4,MT5 |
| currency | yes | string | transfer from the account's trading symbol|
| transferAmount | yes | double | transfer from the account amount |
| comment | no | string | attach info |
| signature | yes | string | signature info |

Signature rule： 

Signature method：SHA1WithRSA 

Signature Data：accountId + currency  + pubUserId + receiptAccount + receiptAccountName +  receiptServerId +  receiptVendor + transferAmount   among which transferAmount must keep two decimals

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | Request success or not |
| mcode | string | Error info |
| data | array | Data content |

#### Example

**Request sample**

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

**Return sample**

```
{
    "mcode": "m0000000",
    "result": true
}
```

# 3.12 Modify the base info of account owner {#modifyBasicInfo}

```
POST /v1/api/account/{pubUserId}/update/diff/base
```

#### Request Body

Name is account owner baseInfo corresponding key

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| accountName | no | string | account name |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | Request success or not |
| mcode | string | Error info |
| data | Void | Data content |

#### Example

#### Request sample

```
POST /v1/api/account/934fabf1-213e-4cef-96ba-ea69f43a0639/update/diff/base
{
"accountName":"test"
}
```

#### Return sample

```
{
    "mcode": "m0000000",
    "result": true
}
```

# 3.13  Modify account owner's financialInfo {#modifyFinancialInfo}

```
POST /v1/api/account/{pubUserId}/update/diff/finance
```

#### Request Body

Name is account owner's financeInfo corresponding key

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| accountNo | no | string | Bank account |
| bankAccount | no | string | Bank of deposit |
| bankBranch | no | string | Name of bank branch |
| bankAddress | no | json | Bank address |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | Request success or not |
| mcode | string | Error info |
| data | Void | Data content |

#### Example

#### Request sample

```
POST /v1/api/account/934fabf1-213e-4cef-96ba-ea69f43a0639/update/diff/finance
{
accountNo : "xxxx"
}
```

#### Return sample

```
{
    "mcode": "m0000000",
    "result": true
}
```

# 3.14 Modify account owner's ID info {#modifyIdentityInfo}

```
POST /v1/api/account/{pubUserId}/update/diff/certificate
```

#### Request Body

Name is account owner's certificateInfo corresponding key

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| idNum | no | string | ID number |
| idType | no | string | ID type |
| idUrl1 | no | string | Identify A |
| idUrl2 | no | json | Identify B |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | Request success or not |
| mcode | string | Error info |
| data | Void | Data content |

#### Example

#### Request sample 

```
POST /v1/api/account/934fabf1-213e-4cef-96ba-ea69f43a0639/update/diff/certificate
{
idNo : "xxxx"
}
```

#### Return sample

```
{
    "mcode": "m0000000",
    "result": true
}
```

# 3.15 Open real account according to SC configuration {#scConfiguration}

```
POST /v2/api/account/{userId}/open
Open a real account for the user userId
```

#### Request Body

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| serverId | yes | string | Server ID |
| vendor | yes | string | platform Name |
| accountInfo | yes | object | account  |
| baseInfo | yes | object | account base info（The field is from the 1.9 interface to get the data for step 1） |
| financialInfo | yes | object | account financeInfo（The field is from the 1.9 interface to get the data for step 2） |
| certificatesInfo | yes | object | account ID info（The field is from the 1.9 interface to get the data for step 3） |

#### accountInfo

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| group | yes | string | MT Group |
|  |  |  |  |
| leverage | yes | string | Leverage |
| login | no | object | Trading account |
| enable | yes | object | Enable status 0/1 |
| readOnly | yes | object | Read only status 0/1 |
| password | no | string | Main password |
| investorPassword | no | string | Investor password |
| comment | no | string | Comments |
| email | yes | string | Email |
| phone | yes | object | Phone |


# 3.16 Submit real account opening tasks according to SC configuration {#openAccountTask}

```
POST /v2/api/account/{userId}/task/open
Submit the actual account opening task to the user userId
```

#### Request Body

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| serverId | yes | string | Server ID |
| vendor | yes | string | Platform Name |
| accountInfo | yes | object | account info |
| baseInfo | yes | object | account base info（The field is from the 1.9 interface to get the data for step 1） |
| financialInfo | yes | object | account financialInfo（The field is from the 1.9 interface to get the data for step 2） |
| certificatesInfo | yes | object | account ID info（The field is from the 1.9 interface to get the data for step 3） |

#### accountInfo

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| group | yes | string | MT Group |
|  |  |  |  |
| leverage | yes | string | Leverage |
| login | no | object | Trading account |
| enable | yes | object | Enable status 0/1 |
| readOnly | yes | object | Read only status 0/1 |
| password | no | string | Mail password |
| investorPassword | no | string | Investor password |
| comment | no | string | Comment |
| email | yes | string | Email |
| phone | yes | object | Phone |






