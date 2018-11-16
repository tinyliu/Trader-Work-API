# Update log

## 2017-09-02 {#20170902}

### Add API

1. [3.10 account transfer application](TradeAccountAPI.md#accountTransfer)：Allows a transfer request to be initiated on the same TW User trading account；

## 2017-08-17 {#20170817}

### Add API

1. [1.7 Basic interface：Query demo account configuration](methods.md#demotype)：obtain current tenant's demo account configuration info；
2. [2.5 User interface：User register-Email](TWUserAPI.md#registerbyemail)：Using email through TW API to register account；
3. [3.6 Trading account interface：Create demo account-mobile](TradeAccountAPI.md#opendemoaccount)：support using mobile number to open demo account,account info notice by SMS；

### Modify API

1. Delete _account deposit_ API；

### Others

1. Modify the misinterpreted signature description（RSA -&gt; SHA1WithRSA）；
2. Add description：All Vendor parameter value MT4、MT5 must be capitalized；

## 2018-08-17 {#20180817}

### Modify API

1. [3.2 Account depositApply](TradeAccountAPI.md#depositApply)和[3.4 account withdraw application](TradeAccountAPI.md#withdraw)：add request parameter serverID；


## 2018-09-30 {#20180930}
### add API
1. [1.8 add banks in bank list](methods.md#bank)：add banks in bank list；
2. [1.9 Get the Settings for the real account opening field from SC](methods.md#scField)：Get the Settings for the real account opening field from SC；
3. [3.12 Modify the basic information of account owner](TradeAccountAPI.md#modifyBasicInfo)：Modify the basic information of account owner；
4. [3.13 Modify the financial information of account owner](TradeAccountAPI.md#modifyFinancialInfo)：Modify the financial information of account owner；
5. [3.14 Modify the account owner's account id information](TradeAccountAPI.md#modifyIdentityInfo)：Modify the account owner's account id information；
6. [3.15 Open real account according to SC configuration](TradeAccountAPI.md#scConfiguration)：Open real account according to SC configuration；

### modify API
1. [3.4 account withdraw application](TradeAccountAPI.md#withdraw)：withdrawDTO add field payCurrency（withdraw currency）；
2. [3.5 create real account](TradeAccountAPI.md#openaccount)：accountDTO add field password；

## 2018-11-15 {#20181115}
### add API
1. [2.14 User register-Send email verification code](TWUserAPI.md#getVerifiyCode)：Send validation code to registered mailbox when registering TW user；
2. [2.15 User register-register by email verification code(TWUserAPI.md#useVerifiyCod)：Complete the registration using the verification code received by the mailbox；
3. [3.16 Submit real account opening tasks according to SC configuration](TradeAccountAPI.md#openAccountTask)：Submit real account opening tasks according to SC configuration
