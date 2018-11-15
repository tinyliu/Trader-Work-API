# 1.1 查询城市编码 {#country}

```
GET /v1/api/sys/countryCity?lang={lang}&pid={pid}
```

获取指定语言`lang`、指定国家或省份`pid`的城市列表

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| lang | no | string | 语言：`zh-CN`，`en-US`，`zh-HK` |
| pid | no | string | 上级城市id |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _array_ | _数据内容_ |
| pid | string | 上级城市id |
| id | string | 城市id |
| value | string | 国家/省份/城市名称 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/sys/countryCity?lang=zh-CN
```

**返回示例**

```json
{
"result": true,
"mcode":"m0000000",
"data":[
{
"pid":"0",
"id":"1",
"value":"中国大陆"
},
{
"pid":"1",
"id":"2",
"value":"北京"
},
{
"pid":"2",
"id":"3",
"value":"东城"
}]
}
```

# 1.2 查询访问设置 {#access}

```
GET /v1/api/sys/config/access
```

获取 Support Center - Trader Work 访问设置中所设规则

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _array_ | _数据内容_ |
| allowEmail | boolean | 是否允许邮箱注册 |
| allowPhone | boolean | 是否允许手机注册 |
| lockLoginFailTimes | int | 密码输错锁定次数 |
| logoutTime | int | 超时登出时间 |
| pwdRegexMap | object | 密码强度正则表达式 |
| pwdStrength | string | 密码强度 |
| registrable | boolean | 是否开放注册 |
| verificationLoginFailTimes | int | 登录出现滑动验证码的密码输错次数 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/sys/config/access
```

**返回示例**

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

# 1.3 查询入金设置 {#depositconfig}

```
GET /v1/api/sys/config/deposit?platform={platform}
```

查询Support Center中交易平台`platform`的入金配置

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| platform | no | string | 交易平台：`MT4`, `MT5` |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _array_ | _数据内容_ |
| charge | double | 手续费 |
| showCharge | boolean | 是否展示手续费 |
| exchangeRateSettings | array | 汇率设置 |
| exchange | double | 当前汇率 |
| exchangeFloat | double | 汇率上浮 |
| exchangeMode | string | 汇率类型 |
| payCurrency | string | 支付货币 |
| showExchange | boolean | 是否展示汇率 |
| transactionCurrency | string | 交易货币 |
| minDeposit | double | 最低入金金额 |
| maxDeposit | double | 最高入金金额 |
| payList | array | 支付平台列表 |
| providerId | string | 支付平台id |
| providerName | string | 支付平台名称 |
| name | string | 支付平台别名 |
| currency | string | 支付货币 |
| telegraphic | string | 电汇入金信息 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/sys/config/deposit?platform=MT4
```

**返回示例**

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
"name": "LW支付通道",
"providerId": "3",
"providerName": "sina支付"
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

# 1.4 查询账户字段设置 {#fields}

```
GET /v1/api/account/info/fields
```

查询Support Center中的账户所有人资料的字段配置

#### Headers Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| x-language | yes | string | 语言：`en-US`,`zh-CN` |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _array_ | _数据内容_ |
| t\_account\_profiles | array | 账户基本信息 |
| columns | string | 占列 |
| enable | boolean | 启用状态 |
| fieldType | string | 字段类型 |
| key | string | 字段key |
| label | boolean | 标签 |
| orderNo | int | 排序 |
| overuse | boolean |  |
| readonly | boolean | 只读 |
| searchable | boolean |  |
| size | int | 字段长度 |
| sysDefault | boolean |  |
| unique | boolean | 是否唯一 |
| validateType | object | 校验类型 |
| phone | boolean | 是否手机 |
| email | boolean | 是否邮箱 |
| required | boolean | 是否必填 |
| t\_account\_finacial | array | 账户财务信息 |
| columns | string | 占列 |
| enable | boolean | 启用状态 |
| fieldType | string | 字段类型 |
| key | string | 字段key |
| label | boolean | 标签 |
| orderNo | int | 排序 |
| overuse | boolean |  |
| readonly | boolean | 只读 |
| searchable | boolean |  |
| size | int | 字段长度 |
| sysDefault | boolean |  |
| unique | boolean | 是否唯一 |
| validateType | object | 校验类型 |
| phone | boolean | 是否手机 |
| email | boolean | 是否邮箱 |
| required | boolean | 是否必填 |
| t\_account\_id\_info | array | 账户证件信息 |
| columns | string | 占列 |
| enable | boolean | 启用状态 |
| fieldType | string | 字段类型 |
| key | string | 字段key |
| label | boolean | 标签 |
| orderNo | int | 排序 |
| overuse | boolean |  |
| readonly | boolean | 只读 |
| searchable | boolean |  |
| size | int | 字段长度 |
| sysDefault | boolean |  |
| unique | boolean | 是否唯一 |
| validateType | object | 校验类型 |
| phone | boolean | 是否手机 |
| email | boolean | 是否邮箱 |
| required | boolean | 是否必填 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/account/info/fields
```

**返回示例**

```json
{
  "data": {
    "t_account_profiles": [
      {
        "columns": "1",
        "enable": true,
        "fieldType": "text",
        "key": "accountName",
        "label": "姓名",
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
        "label": "性别",
        "optionList": [
          {
            "label": "男",
            "value": "0"
          },
          {
            "label": "女",
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
        "label": "手机",
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
        "label": "邮箱",
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
        "label": "身份证明类型",
        "optionList": [
          {
            "label": "营业执照",
            "value": "1"
          },
          {
            "label": "组织机构代码证",
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
        "label": "身份证明号码",
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
        "label": "总资产",
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
        "label": "净资产",
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

# 1.5 查询错误编码 {#errorcode}

```
GET /v1/api/sys/error/code?lang={lang}
```

获取指定语言`lang`的错误信息

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| lang | no | string | 语言：`zh-CN`，`en-US`，`zh-HK` |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _array_ | _数据内容_ |
| code | string | 错误码 |
| value | string | 错误信息 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/sys/error/code?lang=zh-CN
```

**返回示例**

```json
{
  "result": true,
  "mcode":"m0000000",
  "data": [
      {
        "code": "-1",
        "value": "系统错误"
      },
      {
        "code": "PUB_AUTH_0000001",
        "value": "邮箱格式错误"
      },
      {
        "code": "PUB_AUTH_0000002",
        "value": "手机格式错误"
      },
      ...
  ]
}
```

# 1.6 上传图片文件 {#upload}

```
POST /v1/api/upload/file
```

上传图片文件

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| file | yes | file | 上传的文件，不超过5M，格式：`jpg`,`png`,`bmp`,`gif`,`jpeg`,`pdf` |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | array | 文件访问地址 |

#### Example

**请求示例**

```
POST http://twapi.lwork.com/v1/api/upload/file
```

```html
<form action="/v1/api/upload/file" enctype="multipart/form-data" method="POST">
 <input name="image" type="file"></input><br/>
 <input name="submit" type="submit">提交</input>
</form>
```

**返回示例**

```json
{
  "result": true,
  "mcode":"m0000000",
  "data": "/ali/oss/preview/leanwork-fs?fName=tw/dev/T001117/0384062a-368a-4992-9197-55b675e11586.jpg"
}
```

# 1.7 查询模拟账户配置 {#demotype}

```
    GET /v1/api/sys/conf/demo/account/type?platform={platform}
```

获取 Support Center - Trader Work 平台设置中相关platform MT4或MT5 -&gt; 开户设置 -&gt; 模拟账户类型设置中所设规则

#### Request Parameters

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| platform | yes | string | 交易平台：MT4, MT5 |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | array | 数据内容 |
| accountGroup | string | MT组 |
| initAmount | int | 初始金额 |
| leverage | int | 杠杆 |
| typeId | string | 模拟账户类型ID |
| typeName | string | 模拟账户类型名字 |

#### Example

**请求示例**

```
    GET /v1/api/sys/conf/demo/account/type?platform=MT4
```

**返回示例**

```json
{
    "data": [
        {
            "accountGroup": "D04DemoLWVIR",
            "initAmount": 10000,
            "leverage": 100,
            "typeId": "2e852672-dd6e-4ede-a6dc-7108c234dcd7",
            "typeName": "一般用户"
        },
        {
            "accountGroup": "D04DemoLWVIR",
            "initAmount": 20000,
            "leverage": 100,
            "typeId": "cdc44f28-0a3c-4569-90e9-ba56f73be7c1",
            "typeName": "贵宾客户"
        },
        {
            "accountGroup": "D04DemoLWVIR",
            "initAmount": 50000,
            "leverage": 400,
            "typeId": "fa17962a-30ff-43f5-9e33-2a7ba25060ec",
            "typeName": "VIP客户"
        }
    ],
    "mcode": "m0000000",
    "result": true
}
```

# 1.8 银行列表中新增银行 {#bank}

```
POST /v1/api/sys/set/bank/name
```

#### Request Body

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| bankName | yes | string | 新增的银行名字 |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | Void | 数据内容 |

#### Example

#### 请求示例

```
POST /v1/api/sys/set/bank/name
{
"bankName":"ICBC"
}
```

#### 返回示例

```
{
    "mcode": "m0000000",
    "result": true
}
```

# 1.9 从SC获取真实账户开户字段设置 {#scField}

```
GET /v1/api/account/open/info/{vendor}/fields
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| vendor | yes | string | 平台名称 |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| firstStepFieldList | array | 开户第一步baseInfo所需的字段设置信息 |
| key | string | 字段key |
| validateType.required | bool | 是否必需 |
| fieldType | string | 字段类型 |
| enable | bool | 是否启用 |
| secondStepFieldList | array | 开户第二步financialInfo所需的字段设置信息 |
| key | string | 字段key |
| validateType.required | bool | 是否必需 |
| fieldType | string | 字段类型 |
| enable | bool | 是否启用 |
| thirdStepFieldList | array | 开户第三步financialInfo所需的字段设置信息 |
| key | string | 字段key |
| validateType.required | bool | 是否必需 |
| fieldType | string | 字段类型 |
| enable | bool | 是否启用 |

#### Example

#### 请求示例

```
GET /v1/api/account/open/info/MT4/fields

 curl -v   \
    -H "x-api-token:6WuPfOEXToAif3ie" \
    -H "x-language:zh-CN" \
    -H "x-api-tenantId:T001117" \
    -H "content-type:application/json" \
    http://trader.tamsc.lwork.com:10703/release/info

    #-l -X POST -d '{"tenantId":"T001117", "serverId":"428", "vendor":"MT4", "accountInfo":{"group":"amity-online", "leverage":10, "enable":1, "readOnly":1, "email":"333@qq.com", "phone":{"phone":"12345689", "countryCode":"+86"}}, "baseInfo":{"lastName":"ee", "phones":{"phone":"12345689", "countryCode":"+86"}, "accountName":"wak", "email":"39344@qq.com"}, "financialInfo":{"bankBranch":"dkdf"}, "certificatesInfo":{"idUrl2":"xxx", "idType":"dd"}}' http://trader.tamsc.lwork.com:10703/v2/api/account/934fabf1-213e-4cef-96ba-ea69f43a0639/open
```



