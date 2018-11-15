# 2.1 获取手机验证码 {#getCode}

```
GET https://account.api.lwork.com/v1/api/sys/mobile/code?phone={phone}&type={type}&sendSms={sendsms}
```

获取验证码，包括注册验证码`Register`、重置密码验证码`ResetPassword`、修改手机验证码 `UpdatePhoneNumber`

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| phone | yes | string | 手机号 |
| type | yes | string | 类型：`Register`, `ResetPassword`, `UpdatePhoneNumber` |
| sendSms | no | boolean | 是否发送短信，默认 `true` |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _string_ | _验证码，sendSms=true时不返回_ |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/sys/mobile/code?phone=15921433385&type=Register&sendSms=false
```

**返回示例**

```json
{
"data": "380086",
"mcode": "m0000000",
"result": true
}
```

# 2.2 验证手机验证码 {#verifyCode}

```
GET /v1/api/sys/phone/code/verify?phone={phone}&code={code}

校验手机验证码，成功后，返回一个ticket值，手机重置密码时PwdResetDTO需要传入
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| phone | yes | string | 手机号 |
| code | yes | string | 验证码 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | string | ticket |

#### Example

**请求示例**

```json
GET http://twapi.lwork.com/v1/api/sys/phone/code/verify?phone=15912345678&code=931484
```

**返回示例**

```json
{
"data": "VDAwMTExN1RXMTM5MTgxMTM2MzQ5MzE0ODQ=",
"mcode": "m0000000",
"result": true
}
```

# 2.3 重置用户密码-手机 {#resetbyphone}

```
POST /v1/api/user/pwd/reset/{phoneNo}/phone
```

通过手机号`phoneNo` 重置密码

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| phoneNo | yes | string | 手机 |
| PwdResetDTO | yes | Object | 重置密码信息 |

###### PwdResetDTO

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| newPwd | yes | string | 新密码 |
| verified | yes | string | 确认密码 |
| ticket | yes | string | 手机验证码成功后得到的 [ticket](/methods.md#verifyCode) |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |

#### Example

**请求示例**

```json
POST /v1/api/user/pwd/reset/15912345678/phone

{
"newPwd":"Abcd1234!",
"verified":"Abcd1234!",
"ticket":"VDAwMTExN1RXMTg4MTc1NTgwMjEyNzY5Njg="
}
```

**返回示例**

```json
{
"result": true,
"mcode":"m0000000",
}
```

# 2.4 用户注册-手机 {#registerbyphone}

```
POST /v1/api/user/phone/register
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| name | yes | string | 姓名 |
| phone | yes | string | 手机 |
| password | yes | string | 密码 |
| recommendCode | no | string | 推荐码 |
| validateCode | yes | string | 验证码 |
| customSource | no | string | 客户来源 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _array_ | _数据内容_ |

#### Example

**请求示例**

```json
POST http://twapi.lwork.com/v1/api/user/phone/register

{
"name":"Demo",
"phone":"15912345678",
"password":"Abcd1234~",
"validateCode":"602843",
}
```

**返回示例**

```json
{
"result": true,
"mcode":"m0000000"
}
```

# 2.5 用户注册-邮件 {#registerbyemail}

```
    POST /v1/api/user/email/register"
```

通过邮件注册Trader Work 用户

#### Request Parameters

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| mail | yes | string | 用户邮箱 |
| password | yes | string | 用户密码 |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |
| data | array | 数据内容 |

#### Example

**请求示例**

```
POST /v1/api/user/email/register
{
"mail":"12938449@qq.com",
"password":"Abcd1234~"
}
```

**返回示例**

```json
{
    "mcode": "m0000000",
    "result": true
}
```

# 2.6 用户登录 {#login}

```
POST /v1/api/user/login
```

验证用户的账号密码

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| loginName | yes | string | 登录名 |
| password | yes | string | 密码 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _array_ | _数据内容_ |
| userId | string | 用户ID |
| phone | string | 手机 |
| language | string | 用户语言：`en-US`, `zh-CN` |
| lastLoginTime | long | 最近登录时间戳，单位：毫秒 |

#### Example

**请求示例**

```json
POST http://twapi.lwork.com/v1/api/user/login

{
"loginName":"15912345678",
"password":"Abcd1234~"
}
```

**返回示例**

```json
{
"data": {
"language": "en-US",
"lastLoginTime": 1490630315366,
"phone": "15912345678",
"userId": "657d717a-241f-46d8-a5da-a4d80a8b095f"
},
"mcode": "m0000000",
"result": true
}
```

# 2.7 查询用户 {#user}

```
GET /v1/api/user/list?type={type}&value={value}&page={pageIndex}&size={pageSize}
```

根据`type`查询用户

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| type | yes | string | 查询类型: `UserName`,`Email`,`Phone`,`TradeAccount` |
| value | yes | string | 关键词 |
| pageIndex | yes | int | 页码 |
| pageSize | yes | int | 每页条数 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _object_ | _数据内容_ |
| list | array | 查询结果 |
| accounts | array | 交易账号 |
| email | string | 电子邮件 |
| isEnable | boolean | 是否启用 |
| lastLoginTime | int | 最近登录时间 |
| phone | string | 电话 |
| pubUserId | string | 用户ID |
| realName | string | 注册名称 |
| registerTime | int | 服注册时间 |
| offset | int |  |
| pager | int | 当前页数 |
| pages | int | 总页数 |
| size | int | 每页条数 |
| total | int | 总条数 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/user/list?type=Email&value=demo@lwork.com&page=1&size=10
```

**返回示例**

```json
{
  "data": {
    "list": [
      {
        "accounts": [
          "2090004028",
          "2090004033",
          "2090004045",
          "2090004047",
          "2090004048",
          "2090004050",
          "2090004056",
          "2090004059"
        ],
        "email": "bruce@lwork.com",
        "isEnable": true,
        "lastLoginTime": 1491403147458,
        "phone": "18602920465",
        "pubUserId": "ac59a7d8-7860-49a7-b8dc-d3cce3196146",
        "realName": "bruce",
        "registerTime": 1490780106812
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

# 2.8 查询用户基本资料 {#userinfo}

```
GET /v1/api/user/{userId}/detail
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | int | 用户ID |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _array_ | _数据内容_ |
| username | string | 用户名 |
| email | string | Email |
| phone | string | 手机 |
| nickname | string | 昵称 |
| avatar | string | 头像URL |
| accounts | array | 交易账户列表 |
| accountGroup | string | 账户组ID |
| accountGroupName | string | 账户组名称 |
| accountId | string | 交易账号 |
| accountName | string | 账户名称 |
| accountType | string | 账户类型 |
| balance | double | 账户余额 |
| currency | string | 账户交易货币 |
| leverage | int | 账户杠杆 |
| platformName | string | 交易平台名称 |
| serverName | string | 交易服务器名称 |
| status | boolean | 激活状态 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/user/4b07e7af-798a-4f68-9b2c-561a8300eac2/detail
```

**返回示例**

```json
{
  "data": {
    "accounts": [
      {
        "accountGroup": "9",
        "accountGroupName": "账户组测试99",
        "accountId": "2090001252",
        "accountName": "demo",
        "accountType": "Live",
        "balance": 1014.07,
        "currency": "USD",
        "leverage": 100,
        "platformName": "MT4",
        "serverName": "MT4-Live",
        "status": true
      },
      {
        "accountId": "2090005469",
        "accountName": "demo",
        "accountType": "Demo",
        "balance": 0,
        "currency": "USD",
        "leverage": 100,
        "platformName": "MT4",
        "serverName": "MT4-Demo",
        "status": true
      }
    ],
    "email": "demo@lwork.com",
    "nickname": "demo",
    "username": "Test"
  },
  "mcode": "m0000000",
  "result": true
}
```

# 2.9 修改用户登录密码 {#modifypwd}

```
POST /v1/api/user/pwd/set
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | 用户ID |
| origin | yes | string | 原始密码 |
| newPwd | yes | string | 新密码 |
| verifyPwd | yes | string | 确认密码 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |

#### Example

**请求示例**

```json
POST http://twapi.lwork.com/v1/api/user/pwd/set
{
"userId":"657d717a-241f-46d8-a5da-a4d80a8b095f",
"origin":"Abcd1234!",
"newPwd":"Abcd1234~",
"verifyPwd":"Abcd1234~"
}
```

**返回示例**

```json
{
"result": true,
"mcode":"m0000000",
}
```

# 2.10 修改用户头像 {#profilepic}

```
POST /v1/api/user/{userId}/avatar
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | int | 用户ID |
| avatarDTO | yes | object |  |

###### avatarDTO

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| avatar | yes | string | 新头像url |

#### Response

返回结果同 [查询用户基本资料](/methods.md#userinfo)

#### Example

**请求示例**

```
POST http://twapi.lwork.com/v1/api/user/4b07e7af-798a-4f68-9b2c-561a8300eac2/avatar

{
"avatar":"//broker-upload.oss-cn-hangzhou.aliyuncs.com/test/45d952ed-8ca9-4fac-8302-6e735e4d105e.png"
}
```

**返回示例**

```json
{
"data": {
"accounts": [
{
"accountId": "2090001252",
"accountName": "demo",
"accountType": "Live",
"balance": 1014.07,
"currency": "USD",
"platformName": "MT4",
"serverName": "MT4-Live",
"status": true
},
{
"accountId": "2090005469",
"accountName": "demo",
"accountType": "Demo",
"balance": 0,
"currency": "USD",
"platformName": "MT4",
"serverName": "MT4-Demo",
"status": true
}
],
"email": "demo@lwork.com",
"nickname": "demo",
"username": "Test",
"avatar": "//broker-upload.oss-cn-hangzhou.aliyuncs.com/test/45d952ed-8ca9-4fac-8302-6e735e4d105e.png",
},
"mcode": "m0000000",
"result": true
}
```

# 2.11 查询账户资料 {#account}

```
GET /v1/api/account/{userId}/info
```

查询用户`userId`的账户资料，返回的参数信息根据Support Center - Broker Work 字段设置

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | 用户ID |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _object_ | _数据内容_ |
| base | object | 账户基本资料 |
| accountName | string | 账户名称 |
| address | string | 地址 |
| gender | string | 性别 |
| ... | ... | ... |
| cert | object | 账户证件信息 |
| idType | string | 证件类型 |
| idNum | string | 证件号码 |
| idUrl1 | string | 证明Url地址 |
| ... | ... | ... |
| finance | object | 账户财务资料 |
| investmentPurpose | string | 投资目的 |
| investmentYear | string | 投资年数 |
| knowledgeLevel | string | 知识水平 |
| ... | ... | ... |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/account/4b07e7af-798a-4f68-9b2c-561a8300eac2/info
```

**返回示例**

```json
{
    "data": {
        "base": {                       // 基本资料
            "accountName": "test",      // 账户名称
            "birthday": "2017-03-01",   // 生日
            "email": "demo@lwork.com",  // 邮箱
            "gender": "0",              // 性别
            "homePlace": {              // 出生地
                "city": "",
                "country": "3225",
                "province": ""
            },
            "idAddress": "",            // 身份证地址
            "nationality": "",          // 国籍
            "phones": {                 // 手机
                "countryCode": "+886",
                "phone": "12312",
                "phoneStr": "+886 12312"
            },
            "residence": {              // 居住地
                "city": "",
                "country": "",
                "province": ""
            },
            "standbyTelephone": {       // 备用电话
                "countryCode": "",
                "phone": "",
                "phoneStr": " "
            }
        },
        "cert": {                       // 证件信息
            "addressDocType": "",       // 地址证明类型
            "addressFile1": "",         // 地址证明A
            "addressFile2": "",         // 地址证明B
            "bankCardFile1": "",        // 银行卡证明A
            "bankCardFile2": "",        // 银行卡证明B
            "idNum": "11111111111",     // 身份证明号码
            "idType": "",               // 身份证明类型
            "idUrl1": "",               // 身份证明A
            "idUrl2": ""                // 身份证明B
        },
        "finance": {                    // 财务信息
            "aba": "",                  // ABA号码
            "accountNo": "",            // 银行账号
            "bankAccount": "",          // 开户银行
            "bankAddress": {            // 银行地址
                "city": "",
                "country": "",
                "province": ""
            },
            "incomeSource": "",         // 收入来源
            "investmentExperience": [], // 投资经验
            "investmentPurpose": "",    // 投资目的
            "investmentQuota": "",      // 投资额度
            "investmentYear": "",       // 投资年数
            "knowledgeLevel": "",       // 知识水平
            "netAssets": "",            // 净资产
            "remark": "",               // 备注
            "swift": "",                // SWIFT代码
            "totalAssets": "",          // 总资产
            "bankBranch": ""            // 银行支行名称
        }
    },
    "mcode": "m0000000",
    "result": true
}
```

# 2.12 修改账户资料 {#modifyaccountinfo}

```
POST /v1/api/account/{userId}/update
```

修改用户`userId`的账户资料，请求的参数信息参数查询账户资料返回的账户资料

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | 用户ID |
| accountDTO | yes | object | 账户资料 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | 请求是否成功 |
| mcode | string | 错误信息 |

#### Example

**请求示例**

```
POST http://twapi.lwork.com/v1/api/account/4b07e7af-798a-4f68-9b2c-561a8300eac2/update

{
"base": {
"accountName": "test",
"birthday": "2017-03-01",
"email": "demo@lwork.com",
"gender": "0",
"homePlace": {
"city": "",
"country": "3225",
"province": ""
},
"idAddress": "",
"nationality": "",
"phones": {
"countryCode": "+886",
"phone": "12312",
"phoneStr": "+886 12312"
},
"residence": {
"city": "",
"country": "",
"province": ""
},
"standbyTelephone": {
"countryCode": "",
"phone": "",
"phoneStr": " "
}
},
"cert": {
"addressDocType": "",
"addressFile1": "",
"addressFile2": "",
"bankCardFile1": "",
"bankCardFile2": "",
"idNum": "11111111111",
"idType": "",
"idUrl1": "",
"idUrl2": ""
},
"finance": {
"aba": "",
"accountNo": "",
"bankAccount": "",
"bankAddress": {
"city": "",
"country": "",
"province": ""
},
"floatingAssets": "",
"incomeSource": "",
"investmentExperience": [],
"investmentPurpose": "",
"investmentQuota": "",
"investmentYear": "",
"knowledgeLevel": "",
"netAssets": "",
"netIncome": "",
"remark": "",
"swift": "",
"totalAssets": "",
"transactionsPerYear": ""
}

}
```

**返回示例**

```json
{
"mcode": "m0000000",
"result": true
}
```

# 2.13 查询任务状态 {#task}

```
GET /v1/api/account/{userId}/{taskType}/task/state
```

查询用户`userId` 的任务状态

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| pubUserId | yes | string | 用户ID |
| taskType | yes | string | 任务类型：`JOB_TYPE_TA_OPEN` - 开户，`JOB_TYPE_TA_SAME_OPEN` - 同名账户, `JOB_TYPE_TA_BIND` - 绑定账户，`JOB_TYPE_TA_UPDATE_OWNER` - 修改账户所有人资料 |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _请求是否成功_ |
| _mcode_ | _string_ | _错误信息_ |
| _data_ | _array_ | _数据内容_ |
| createdTime | string | 任务创建时间 |
| id | string | 任务ID |
| jobType | string | 任务类型 |
| pubUserId | string | 用户ID |
| state | string | 任务状态 |

#### Example

**请求示例**

```
GET http://twapi.lwork.com/v1/api/account/39444997-6082-400a-b2be-7ecb8a67722d/JOB_TYPE_TA_SAME_OPEN/task/state
```

**返回示例**

```json
{
  "result": true,
  "mcode":"m0000000",
  "data": {
    "createdTime": "2017-04-19T18:07:43.218",
    "id": "e431c3aa-b1e2-4d5a-9321-7e9909eba5e2",
    "jobType": "JOB_TYPE_TA_SAME_OPEN",
    "pubUserId": "39444997-6082-400a-b2be-7ecb8a67722d",
    "state": "Submited"
  }
}
```



