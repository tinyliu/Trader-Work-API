# Update log

## 2017-09-02 {#20170902}

### Add API

1. [3.10 account transfer application](TradeAccountAPI.md#accountTransfer)：允许对同一 TW User 下的交易账户发起转账申请；

## 2017-08-17 {#20170817}

### 新增 API

1. [1.7 基础接口：查询模拟账户配置](methods.md#demotype)：获取当前租户对模拟账户的配置信息；
2. [2.5 用户接口：用户注册-邮件](TWUserAPI.md#registerbyemail)：使用邮件通过 TW API 来注册用户；
3. [3.6 账户接口：创建模拟账户-手机](TradeAccountAPI.md#opendemoaccount)：支持使用手机号开通模拟账户，账户信息通过短信通知；

### 修改 API

1. 删除 _账户入金_ API；

### 其它

1. 修正原容易产生误解的签名方式描述（RSA -&gt; SHA1WithRSA）；
2. 补充描述：所有获取 Vendor 参数的值 MT4、MT5 必须大写；

## 2018-08-17 {#20180817}

### 修改 API

1. [3.2 账户入金申请](TradeAccountAPI.md#depositApply)和[3.4 账户出金申请](TradeAccountAPI.md#withdraw)：增加请求参数serverID；


## 2018-09-30 {#20180930}
### 新增 API
1. [1.8 银行列表中新增银行](methods.md#bank)：银行列表中新增银行；
2. [1.9 从SC获取真实账户开户字段设置](methods.md#scField)：从SupportCenter获取真实账户开户字段设置；
3. [3.12 修改账户所有人资料基础信息](TradeAccountAPI.md#modifyBasicInfo)：修改账户所有人资料基础信息；
4. [3.13 修改账户所有人资料财务信息](TradeAccountAPI.md#modifyFinancialInfo)：修改账户所有人资料财务信息；
5. [3.14 修改账户所有人资料账户证件信息](TradeAccountAPI.md#modifyIdentityInfo)：修改账户所有人资料账户证件信息；
6. [3.15 根据SC配置开真实账户](TradeAccountAPI.md#scConfiguration)：根据SC配置开真实账户；

### 修改 API
1. [3.4 账户出金申请](TradeAccountAPI.md#withdraw)：withdrawDTO增加字段payCurrency（出金货币）；
2. [3.5 创建真实账户](TradeAccountAPI.md#openaccount)：accountDTO增加字段password；

## 2018-11-15 {#20181115}
### 新增 API
1. [2.14 用户注册-发送邮件验证码](TWUserAPI.md#getVerifiyCode)：注册TW用户时发送验证码到注册邮箱；
2. [2.15 用户注册-使用邮件验证码注册](TWUserAPI.md#useVerifiyCod)：使用邮箱收到的验证码完成注册；
3. [3.16 根据SC配置提交真实开户任务](TradeAccountAPI.md#openAccountTask)：根据SC配置的字段提交真实开户任务






















