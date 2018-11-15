When calling the API, the tenant number and APIKey must be provided as the authentication for each request. You can manage your APIKey in the SupportCenter. The APIKey is the identity of the tenant in the AccountAPI system. Please store it securely to ensure that it is not compromised. If you need to update, you can also update it in the SupportCenter.

Call address: `https://account.api.lwork.com`, HTTPS access.

All values for the Vendor parameter MT4, MT5 must be capitalized.

For all interface requests, please include parameters in Headers

  
`x-api-tenantId`：`Tenant ID`，Query in Product details in`SupportCenter`

`x-api-token`：`API Key`，Query in Product details`SupportCenter`

`content-type`:`application/json`



```
GET
 https://account.api.lwork.com/v1/config/access HTTP/1.1
Accept:
 application/json

Accept-Encoding:
 gzip, deflate

Connection:
 keep-alive

Content-Length:
 188

User-Agent:
 HTTPie/1.0.0-dev

cache-control:
 no-cache

content-type:
 application/json

x-api-token:
 myapikey

x-api-tenantid:
 mytennatid
```



