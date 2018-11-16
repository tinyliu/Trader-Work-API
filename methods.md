#
1.1 Query City Code {#country}

```
GET /v1/api/sys/countryCity?lang={lang}&pid={pid}
```

Acquire language`lang`、Counry or province`pid` 

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| lang | no | string | Language ：`zh-CN`，`en-US`，`zh-HK` |
| pid | no | string | Superior City id |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _array_ | _Data content_ |
| pid | string | Superior City id |
| id | string | City id |
| value | string | Country/Province/City Name |

#### Example

**Request Sample**

```
GET http://twapi.lwork.com/v1/api/sys/countryCity?lang=zh-CN
```

**Return Sample**

```json
{
"result": true,
"mcode":"m0000000",
"data":[
{
"pid":"0",
"id":"1",
"value":"China"
},
{
"pid":"1",
"id":"2",
"value":"Beijing"
},
{
"pid":"2",
"id":"3",
"value":"DongCheng"
}]
}
```

# 1.2 Query Access Setting{#access}

```
GET /v1/api/sys/config/access
```

Acquire Support Center - Trader Work Query Access Setting

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error Info_ |
| _data_ | _array_ | _Data Content_ |
| allowEmail | boolean | Permit register by email or not |
| allowPhone | boolean | Permit register by mobile nor not |
| lockLoginFailTimes | int | Password input error lock number |
| logoutTime | int | Timeout time |
| pwdRegexMap | object | Password Strength Regular Expression|
| pwdStrength | string | Password Strength |
| registrable | boolean | Open register or not |
| verificationLoginFailTimes | int | The number of password errors entered in the login verification code |

#### Example

**Request Sample**

```
GET http://twapi.lwork.com/v1/api/sys/config/access
```

**Return Sample**

```json
{
"result": true,
"mcode":"m0000000",
"data": {
"allowEmail": true
"allowPhone": true,
"lockLoginFailTimes": 10,
"logoutTime": 30,
"pwdRegexMap": {
"Strong": "^(?=.*[\\p{Digit}])(?=.*[\\p{Lower}])(?=.*[\\p{Upper}])[\\d\\p{Upper}\\p{Lower}\\p{Punct}]{8,20}$",
"Middle": "^(?=.*[\\p{Digit}])(?=.*[\\p{Alpha}])[\\d\\p{Alpha}\\p{Punct}]{6,20}$",
"SuperStrong": "^(?=.*[\\p{Digit}])(?=.*[\\p{Lower}])(?=.*[\\p{Upper}])(?=.*[\\p{Punct}])[\\d\\p{Upper}\\p{Lower}\\p{Punct}]{8,20}$"
},
"pwdStrength": "Middle",
"registrable": true,
"verificationLoginFailTimes": 3
}
}
```

# 1.3 Query deposit setting {#depositconfig}

```
GET /v1/api/sys/config/deposit?platform={platform}
```

Query Support Center's reading platform`platform` deposit setting

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| platform | no | string | trading platform：`MT4`, `MT5` |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _array_ | _Data content_ |
| charge | double | Charge Fee |
| showCharge | boolean | Shows charge fee or not |
| exchangeRateSettings | array | Currency rate setting |
| exchange | double | Current currency |
| exchangeFloat | double | Curreny rate floating |
| exchangeMode | string | Currency rate style |
| payCurrency | string | Payment currency|
| showExchange | boolean | Shows the currency rate or not |
| transactionCurrency | string | Trading Currency |
| minDeposit | double | Minimal deposit |
| maxDeposit | double | Highest deposit amount |
| payList | array | Payment platform list |
| providerId | string | Payment platform id |
| providerName | string | Payment platform Name |
| name | string | Payment platform another Name |
| currency | string | Payment currency |
| telegraphic | string | Wire transfer information |

#### Example

**Request Sample**

```
GET http://twapi.lwork.com/v1/api/sys/config/deposit?platform=MT4
```

**Return Sample**

```json
{
"data": {
"charges": 10,
"exchangeRateSettings": [
{
"exchange": 6.9271,
"exchangeFloat": 0.1,
"exchangeMode": "Automatic",
"payCurrency": "CNY",
"showExchange": true,
"transactionCurrency": "USD"
},
{
"exchange": 1.43902439,
"exchangeFloat": 0,
"exchangeMode": "Automatic",
"payCurrency": "NZD",
"showExchange": true,
"transactionCurrency": "USD"
}
],
"maxDeposit": 200000,
"minDeposit": 0.01,
"payList": [
{
"currency": "CNY",
"name": "LW Payment Channel",
"providerId": "3",
"providerName": "sina Payment"
},
{
"currency": "NZD",
"name": "UNIONPAY",
"providerId": "27",
"providerName": "UNIONPAY"
}
],
"showCharge": true,
"telegraphic": "<html>test</html>\n"
},
"mcode": "m0000000",
"result": true
}
```

# 1.4 Query account field setting {#fields}

```
GET /v1/api/account/info/fields
```

Query Support Center account owner's info fields setting

#### Headers Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| x-language | yes | string | Language：`en-US`,`zh-CN` |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request Success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _array_ | _Data content_ |
| t\_account\_profiles | array | Account baseInfo |
| columns | string | Occupy columns |
| enable | boolean | Enable status |
| fieldType | string | field Type |
| key | string | field key |
| label | boolean | label |
| orderNo | int | Order |
| overuse | boolean | |
| readonly | boolean | Read Only |
| searchable | boolean | |
| size | int | field size |
| sysDefault | boolean | |
| unique | boolean | If one and only |
| validateType | object | valid Type |
| phone | boolean | If mobile |
| email | boolean | If email |
| required | boolean | If required |
| t\_account\_finacial | array | Financial account info |
| columns | string | Occupy columns |
| enable | boolean | Enable Status |
| fieldType | string | Field Type |
| key | string | field key |
| label | boolean | Label |
| orderNo | int | Order No |
| overuse | boolean | |
| readonly | boolean | Read Only |
| searchable | boolean | |
| size | int | Field size |
| sysDefault | boolean | |
| unique | boolean | If one and only |
| validateType | object | validate Type |
| phone | boolean | If mobile |
| email | boolean | If email |
| required | boolean | If required |
| t\_account\_id\_info | array | account ID info |
| columns | string | Occupy columns |
| enable | boolean | Enable status |
| fieldType | string | Field Type |
| key | string | Field key |
| label | boolean | Label |
| orderNo | int | Order No|
| overuse | boolean | |
| readonly | boolean | Read Only |
| searchable | boolean | |
| size | int | Field size |
| sysDefault | boolean | |
| unique | boolean | If one and only |
| validateType | object | validate Type |
| phone | boolean | If mobile  |
| email | boolean | If email |
| required | boolean | If required |

#### Example

**Request sample**

```
GET http://twapi.lwork.com/v1/api/account/info/fields
```

**Return Sample**

```json
{
"data": {
"t_account_profiles": [
{
"columns": "1",
"enable": true,
"fieldType": "text",
"key": "accountName",
"label": "Name",
"orderNo": 1,
"overuse": true,
"readonly": false,
"searchable": false,
"size": 40,
"sysDefault": true,
"unique": false,
"validateType": {
"required": true
}
},
{
"columns": "1",
"defaultValue": "0",
"enable": true,
"fieldType": "select",
"key": "gender",
"label": "Gender",
"optionList": [
{
"label": "Male",
"value": "0"
},
{
"label": "Female",
"value": "1"
}
],
"orderNo": 2,
"overuse": false,
"readonly": false,
"searchable": false,
"size": 50,
"sysDefault": false,
"unique": false,
"validateType": {}
},
{
"columns": "1",
"enable": true,
"fieldType": "phone",
"key": "phones",
"label": "Mobile",
"orderNo": 5,
"overuse": false,
"readonly": false,
"searchable": false,
"size": 50,
"sysDefault": true,
"unique": false,
"validateType": {
"phone": true,
"required": true
}
},
{
"columns": "1",
"enable": true,
"fieldType": "text",
"key": "email",
"label": "Email",
"orderNo": 6,
"overuse": false,
"readonly": false,
"searchable": false,
"size": 50,
"sysDefault": true,
"unique": false,
"validateType": {
"required": true,
"email": true
}
},
...
],
"t_account_id_info": [
{
"columns": "1",
"defaultValue": "1",
"enable": true,
"fieldType": "select",
"key": "idType",
"label": "Identity Type",
"optionList": [
{
"label": "Business License",
"value": "1"
},
{
"label": "Organization Code Certificate",
"value": "2"
},
...
],
"orderNo": 1,
"overuse": true,
"readonly": false,
"searchable": false,
"size": 50,
"sysDefault": false,
"unique": false,
"validateType": {
"required": true
}
},
{
"columns": "1",
"enable": true,
"fieldType": "text",
"key": "idNum",
"label": "ID number",
"orderNo": 2,
"overuse": true,
"readonly": false,
"searchable": false,
"size": 50,
"sysDefault": false,
"unique": false,
"validateType": {
"required": true
}
},
...
],
"t_account_finacial": [
{
"columns": "1",
"defaultValue": "1",
"enable": true,
"fieldType": "select",
"key": "totalAssets",
"label": "Total Assets",
"optionList": [
{
"label": "0-500,000",
"value": "1"
},
{
"label": "500,001-1,000,000",
"value": "2"
},
...
],
"orderNo": 1,
"overuse": false,
"readonly": false,
"searchable": false,
"size": 50,
"sysDefault": false,
"unique": false,
"validateType": {}
},
{
"columns": "1",
"defaultValue": "1",
"enable": true,
"fieldType": "select",
"key": "netAssets",
"label": "Net Assets",
"optionList": [
{
"label": "0-500,000",
"value": "1"
},
{
"label": "500,001-1,000,000",
"value": "2"
},
...
],
"orderNo": 2,
"overuse": false,
"readonly": false,
"searchable": false,
"size": 50,
"sysDefault": false,
"unique": false,
"validateType": {}
},
...
]
},
"mcode": "m0000000",
"result": true
}
```

# 1.5 Query Error Code {#errorcode}

```
GET /v1/api/sys/error/code?lang={lang}
```

Access language`lang`Error Info

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| lang | no | string | Language：`zh-CN`，`en-US`，`zh-HK` |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request Success nor not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _array_ | _Data content_ |
| code | string | Error code |
| value | string | Error Info_ |

#### Example

**Request Sample**

```
GET http://twapi.lwork.com/v1/api/sys/error/code?lang=zh-CN
```

**Return Sample**

```json
{
"result": true,
"mcode":"m0000000",
"data": [
{
"code": "-1",
"value": "System Error"
},
{
"code": "PUB_AUTH_0000001",
"value": "Email Style Error"
},
{
"code": "PUB_AUTH_0000002",
"value": "Mobile Style Error"
},
...
]
}
```

# 1.6 Upload Photo File {#upload}

```
POST /v1/api/upload/file
```

Upload Photo file

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| file | yes | file | Upload file smaller than 5M，format：`jpg`,`png`,`bmp`,`gif`,`jpeg`,`pdf` |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | Request success or not |
| mcode | string | Error info |
| data | array | File access address |

#### Example

**Request Sample**

```
POST http://twapi.lwork.com/v1/api/upload/file
```

```html
<form action="/v1/api/upload/file" enctype="multipart/form-data" method="POST">
<input name="image" type="file"></input><br/>
<input name="submit" type="submit">Submit</input>
</form>
```

**Return Sample**

```json
{
"result": true,
"mcode":"m0000000",
"data": "/ali/oss/preview/leanwork-fs?fName=tw/dev/T001117/0384062a-368a-4992-9197-55b675e11586.jpg"
}
```

# 1.7 Query Demo account setting {#demotype}

```
GET /v1/api/sys/conf/demo/account/type?platform={platform}
```

Acquire Support Center - Trader Work setting platform MT4 or MT5 -&gt; Open account setting -&gt; Rule set by demo account style 

#### Request Parameters

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| platform | yes | string | Trading Platform：MT4, MT5 |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | Request Success or not |
| mcode | string | Error Info |
| data | array | Data Content |
| accountGroup | string | MT Group |
| initAmount | int | Initial Amount |
| leverage | int | Leverage |
| typeId | string | Demo account style |
| typeName | string | Demo account style Name |

#### Example

**Request Sample**

```
GET /v1/api/sys/conf/demo/account/type?platform=MT4
```

**Return Sample**

```json
{
"data": [
{
"accountGroup": "D04DemoLWVIR",
"initAmount": 10000,
"leverage": 100,
"typeId": "2e852672-dd6e-4ede-a6dc-7108c234dcd7",
"typeName": "General User"
},
{
"accountGroup": "D04DemoLWVIR",
"initAmount": 20000,
"leverage": 100,
"typeId": "cdc44f28-0a3c-4569-90e9-ba56f73be7c1",
"typeName": "VIP user"
},
{
"accountGroup": "D04DemoLWVIR",
"initAmount": 50000,
"leverage": 400,
"typeId": "fa17962a-30ff-43f5-9e33-2a7ba25060ec",
"typeName": "VIP user"
}
],
"mcode": "m0000000",
"result": true
}
```

# 1.8 Ade new banks in bank list {#bank}

```
POST /v1/api/sys/set/bank/name
```

#### Request Body

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| bankName | yes | string | New bank names |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | Request success or not |
| mcode | string | Error info |
| data | Void | Data Content  |

#### Example

#### Request Sample

```
POST /v1/api/sys/set/bank/name
{
"bankName":"ICBC"
}
```

#### Return Sample

```
{
"mcode": "m0000000",
"result": true
}
```

# 1.9 Get real account account opening field settings from SC {#scField}

```
GET /v1/api/account/open/info/{vendor}/fields
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| vendor | yes | string | Platform name |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | Request success or not |
| mcode | string | Error info |
| firstStepFieldList | array | Open account 1st step  baseInfo Required field setup information |
| key | string | field key |
| validateType.required | bool | If required |
| fieldType | string | Field Type |
| enable | bool | If enable |
| secondStepFieldList | array | Open account 2nd step financialInfo Required field setup information |
| key | string | Field key |
| validateType.required | bool | If required |
| fieldType | string | Field Type |
| enable | bool | If enable |
| thirdStepFieldList | array | Open account 3rd step financialInfo Required field setup information |
| key | string | Field key |
| validateType.required | bool | If required |
| fieldType | string | Field Type |
| enable | bool | If enable |

#### Example

#### Request Sample

```
GET /v1/api/account/open/info/MT4/fields

curl -v \
-H "x-api-token:6WuPfOEXToAif3ie" \
-H "x-language:zh-CN" \
-H "x-api-tenantId:T001117" \
-H "content-type:application/json" \
http://trader.tamsc.lwork.com:10703/release/info

#-l -X POST -d '{"tenantId":"T001117", "serverId":"428", "vendor":"MT4", "accountInfo":{"group":"amity-online", "leverage":10, "enable":1, "readOnly":1, "email":"333@qq.com", "phone":{"phone":"12345689", "countryCode":"+86"}}, "baseInfo":{"lastName":"ee", "phones":{"phone":"12345689", "countryCode":"+86"}, "accountName":"wak", "email":"39344@qq.com"}, "financialInfo":{"bankBranch":"dkdf"}, "certificatesInfo":{"idUrl2":"xxx", "idType":"dd"}}' http://trader.tamsc.lwork.com:10703/v2/api/account/934fabf1-213e-4cef-96ba-ea69f43a0639/open
```



