# 2.1 Get mobile verify code {#getCode}

```
GET https://account.api.lwork.com/v1/api/sys/mobile/code?phone={phone}&type={type}&sendSms={sendsms}
```

Get verification code，includes register verification code `Register`、rest passwords verification code`ResetPassword`、modify mobile verification code `UpdatePhoneNumber`

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| phone | yes | string | Mobile number |
| type | yes | string | Type：`Register`, `ResetPassword`, `UpdatePhoneNumber` |
| sendSms | no | boolean | If send SMS or not，custom `true` |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _string_ | _verify code，when sendSms=true not return_ |

#### Example

**Request sample**

```
GET http://twapi.lwork.com/v1/api/sys/mobile/code?phone=15921433385&type=Register&sendSms=false
```

**Return sample**

```json
{
"data": "380086",
"mcode": "m0000000",
"result": true
}
```

# 2.2 Verify mobile verification code {#verifyCode}

```
GET /v1/api/sys/phone/code/verify?phone={phone}&code={code}


Verify the cell phone verification code, and when successful, return a ticket value. PwdResetDTO needs to be passed in when the phone resets its password
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| phone | yes | string | Mobile number |
| code | yes | string | verification code |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | Request success or not |
| mcode | string | Error info |
| data | string | ticket |

#### Example

**Request sample**

```json
GET http://twapi.lwork.com/v1/api/sys/phone/code/verify?phone=15912345678&code=931484
```

**Return sample**

```json
{
"data": "VDAwMTExN1RXMTM5MTgxMTM2MzQ5MzE0ODQ=",
"mcode": "m0000000",
"result": true
}
```

# 2.3 Reset user password-mobile {#resetbyphone}

```
POST /v1/api/user/pwd/reset/{phoneNo}/phone
```

through mobile number `phoneNo` rest password

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| phoneNo | yes | string | Mobile |
| PwdResetDTO | yes | Object | Reset password info |

###### PwdResetDTO

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| newPwd | yes | string | new password |
| verified | yes | string | Confirm password |
| ticket | yes | string | The phone verification code was successfully obtained [ticket](/methods.md#verifyCode) |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _request success or not_ |
| _mcode_ | _string_ | _Error info_ |

#### Example

**Request sample**

```json
POST /v1/api/user/pwd/reset/15912345678/phone

{
"newPwd":"Abcd1234!",
"verified":"Abcd1234!",
"ticket":"VDAwMTExN1RXMTg4MTc1NTgwMjEyNzY5Njg="
}
```

**Return sample**

```json
{
"result": true,
"mcode":"m0000000",
}
```

# 2.4 User register-Mobile {#registerbyphone}

```
POST /v1/api/user/phone/register
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| name | yes | string | Name |
| phone | yes | string | Mobile |
| password | yes | string | Password |
| recommendCode | no | string | Recommend code |
| validateCode | yes | string | Verification code |
| customSource | no | string | customer source |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _array_ | _Data content_ |

#### Example

**Request sample**

```json
POST http://twapi.lwork.com/v1/api/user/phone/register

{
"name":"Demo",
"phone":"15912345678",
"password":"Abcd1234~",
"validateCode":"602843",
}
```

**Return sample**

```json
{
"result": true,
"mcode":"m0000000"
}
```

# 2.5 User register-Email {#registerbyemail}

```
    POST /v1/api/user/email/register"
```

Through Email register Trader Work User

#### Request Parameters

| Name | Required | Type | Description |
| --- | --- | --- | --- |
| mail | yes | string | User Email |
| password | yes | string | User password |

#### Response

| Name | Type | Description |
| --- | --- | --- |
| result | boolean | Request success or not |
| mcode | string | Error info |
| data | array | Data content |

#### Example

**Request sample**

```
POST /v1/api/user/email/register
{
"mail":"12938449@qq.com",
"password":"Abcd1234~"
}
```

**Return sample**

```json
{
    "mcode": "m0000000",
    "result": true
}
```

# 2.6 User Login {#login}

```
POST /v1/api/user/login
```

Verify User's account password

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| loginName | yes | string | Login Name |
| password | yes | string | Password |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _array_ | _Data content_ |
| userId | string | User ID |
| phone | string | Mobile |
| language | string | User Language：`en-US`, `zh-CN` |
| lastLoginTime | long | Recent login timestamp ，units：milliseconds |

#### Example

**Request sample**

```json
POST http://twapi.lwork.com/v1/api/user/login

{
"loginName":"15912345678",
"password":"Abcd1234~"
}
```

**Return sample**

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

# 2.7 Query user {#user}

```
GET /v1/api/user/list?type={type}&value={value}&page={pageIndex}&size={pageSize}
```

according to `type`Query user

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| type | yes | string | Query type: `UserName`,`Email`,`Phone`,`TradeAccount` |
| value | yes | string | keyword |
| pageIndex | yes | int | Pages |
| pageSize | yes | int | Items per page |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _object_ | _Data content_ |
| list | array | Query result |
| accounts | array | trading account |
| email | string | Email |
| isEnable | boolean | Enable or not |
| lastLoginTime | int | Recent login time |
| phone | string | Phone |
| pubUserId | string | User ID |
| realName | string | Register name |
| registerTime | int | Register time |
| offset | int |  |
| pager | int | Current pages |
| pages | int | Total pages |
| size | int | Items per page|
| total | int | Total items |

#### Example

**Request sample**

```
GET http://twapi.lwork.com/v1/api/user/list?type=Email&value=demo@lwork.com&page=1&size=10
```

**Return sample**

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

# 2.8 Query user basic info{#userinfo}

```
GET /v1/api/user/{userId}/detail
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | int | User ID |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _array_ | _Data content_ |
| username | string | User name |
| email | string | Email |
| phone | string | Mobile |
| nickname | string | Nick name |
| avatar | string | Icon URL |
| accounts | array | trading account list |
| accountGroup | string | account list ID |
| accountGroupName | string | account list name |
| accountId | string | trading account  |
| accountName | string | account name |
| accountType | string | account type |
| balance | double | account balance|
| currency | string | account trading currency |
| leverage | int | account leverage |
| platformName | string | platform Name |
| serverName | string | trading server name |
| status | boolean | Enable status |

#### Example

**Request sample**

```
GET http://twapi.lwork.com/v1/api/user/4b07e7af-798a-4f68-9b2c-561a8300eac2/detail
```

**Return sample**

```json
{
  "data": {
    "accounts": [
      {
        "accountGroup": "9",
        "accountGroupName": "account group test 99",
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

# 2.9 Modify user login password {#modifypwd}

```
POST /v1/api/user/pwd/set
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | User ID |
| origin | yes | string | Original password |
| newPwd | yes | string | New password |
| verifyPwd | yes | string | Confirm password |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |

#### Example

**request sample**

```json
POST http://twapi.lwork.com/v1/api/user/pwd/set
{
"userId":"657d717a-241f-46d8-a5da-a4d80a8b095f",
"origin":"Abcd1234!",
"newPwd":"Abcd1234~",
"verifyPwd":"Abcd1234~"
}
```

**Return sample****

```json
{
"result": true,
"mcode":"m0000000",
}
```

# 2.10 Change profile Icon{#profilepic}

```
POST /v1/api/user/{userId}/avatar
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | int | User ID |
| avatarDTO | yes | object |  |

###### avatarDTO

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| avatar | yes | string | New profile icon url |

#### Response

Return result [Query user basic info](/methods.md#userinfo)

#### Example

**Request sample**

```
POST http://twapi.lwork.com/v1/api/user/4b07e7af-798a-4f68-9b2c-561a8300eac2/avatar

{
"avatar":"//broker-upload.oss-cn-hangzhou.aliyuncs.com/test/45d952ed-8ca9-4fac-8302-6e735e4d105e.png"
}
```

**Return sample **

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

# 2.11 Query account info {#account}

```
GET /v1/api/account/{userId}/info
```

Query user `userId`account info，return parameters according to Support Center - Broker Work fields setting

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | User ID |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _object_ | _Data content_ |
| base | object | Account basic info |
| accountName | string | Account name |
| address | string | Address |
| gender | string | Gender |
| ... | ... | ... |
| cert | object | ID info |
| idType | string | ID type |
| idNum | string | ID number |
| idUrl1 | string | ID Url address |
| ... | ... | ... |
| finance | object |account finance info |
| investmentPurpose | string | investor purpose |
| investmentYear | string | investor years |
| knowledgeLevel | string | knowledgeLevel level |
| ... | ... | ... |

#### Example

**Request sample**

```
GET http://twapi.lwork.com/v1/api/account/4b07e7af-798a-4f68-9b2c-561a8300eac2/info
```

**Return sample**

```json
{
    "data": {
        "base": {                       // basic info
            "accountName": "test",      // account name
            "birthday": "2017-03-01",   // birthday
            "email": "demo@lwork.com",  // email
            "gender": "0",              // gender
            "homePlace": {              // birth place
                "city": "",
                "country": "3225",
                "province": ""
            },
            "idAddress": "",            // ID address
            "nationality": "",          // Nationality
            "phones": {                 // Mobile
                "countryCode": "+886",
                "phone": "12312",
                "phoneStr": "+886 12312"
            },
            "residence": {              // birth place
                "city": "",
                "country": "",
                "province": ""
            },
            "standbyTelephone": {       // Alternate phone
                "countryCode": "",
                "phone": "",
                "phoneStr": " "
            }
        },
        "cert": {                       // ID info
            "addressDocType": "",       // Address certificate type 
            "addressFile1": "",         // Address certificate A
            "addressFile2": "",         // Address certificate B
            "bankCardFile1": "",        // Bank certificate A
            "bankCardFile2": "",        // Bank certificate B
            "idNum": "11111111111",     // ID number
            "idType": "",               // Type of identification
            "idUrl1": "",               // Identification A
            "idUrl2": ""                // Identification B
        },
        "finance": {                    // financial information
            "aba": "",                  // ABA number 
            "accountNo": "",            // bank account 
            "bankAccount": "",          // bank of deposit 
            "bankAddress": {            // bank address
                "city": "",
                "country": "",
                "province": ""
            },
            "incomeSource": "",         // Income source
            "investmentExperience": [], // investor experience 
            "investmentPurpose": "",    // investor purpose
            "investmentQuota": "",      // investor quota 
            "investmentYear": "",       // investor years in total
            "knowledgeLevel": "",       // knowledgeLevel
            "netAssets": "",            // netAssets
            "remark": "",               // comments
            "swift": "",                // SWIFT code
            "totalAssets": "",          // total assets
            "bankBranch": ""            // bank branch name 
        }
    },
    "mcode": "m0000000",
    "result": true
}
```

# 2.12 Modify account info {#modifyaccountinfo}

```
POST /v1/api/account/{userId}/update
```

Modify user `userId`account info 

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| userId | yes | string | User ID |
| accountDTO | yes | object | account info|

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| result | boolean | request success or not |
| mcode | string | Error info |

#### Example

**Request sample**

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

**return sample**

```json
{
"mcode": "m0000000",
"result": true
}
```

# 2.13 Query taks status {#task}

```
GET /v1/api/account/{userId}/{taskType}/task/state
```

Query user `userId` task status 

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| pubUserId | yes | string | user ID |
| taskType | yes | string | task type：`JOB_TYPE_TA_OPEN` - open account，`JOB_TYPE_TA_SAME_OPEN` - the same name account, `JOB_TYPE_TA_BIND` - binding account，`JOB_TYPE_TA_UPDATE_OWNER` - modify account owner info |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |
| _data_ | _array_ | _Data conent_ |
| createdTime | string | Task created time |
| id | string | Task ID |
| jobType | string | Task type |
| pubUserId | string | user ID |
| state | string | task status |

#### Example

**request sample**

```
GET http://twapi.lwork.com/v1/api/account/39444997-6082-400a-b2be-7ecb8a67722d/JOB_TYPE_TA_SAME_OPEN/task/state
```

**return sample**

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
# 2.14 user register-sending email verification code{#getVerifiyCode}

```
POST https://account.api.lwork.com/v1/api/user/email/sendRegisterValidateCode?email={email}
```

#### Request Parameters

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| email | yes | string | Email |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info_ |

#### Example

**Request sample**

```
GET http://twapi.lwork.com/v1/api/user/email/sendRegisterValidateCode?email=tomcat@lwork.com

```

**Return sample **

```json
{
"mcode": "m0000000",
"result": true
}
```

# 2.15 user register-using email verification code to register{#useVerifiyCode}

```
POST /v1/api/user/email/registerWithValidateCode
```

#### Request Body

| Name | Required | Type | Description |
| :--- | :--- | :--- | :--- |
| name | yes | string | Name  |
| email | yes | string | email |
| password | yes | string | password |
| validateCode | yes | string | verification code  |

#### Response

| Name | Type | Description |
| :--- | :--- | :--- |
| _result_ | _boolean_ | _Request success or not_ |
| _mcode_ | _string_ | _Error info _ |
| _data_ | _array_ | _Data content_ |

#### Example

**Request sample**

```json
POST http://twapi.lwork.com/v1/api/user/email/registerWithValidateCode
{
 "name": "steven",
 "email": "steven@lwork.com",
 "password": "Abcd1234",
 "validateCode": "302454"
} 
```

**Return sample**

```json
{
"result": true,
"mcode":"m0000000"
}
```





