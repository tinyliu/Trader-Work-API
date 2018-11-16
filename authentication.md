When calling the API, you must provide the tenant number and API Key as the authentication for each request. You can manage your API Keys within the Management Platform in Support Center. 

The API Key is the identity of the tenant in the Account API system. Please store it securely to ensure it is not compromised. If update is needed, it can be renewed in the management platform.



Calling address：`https://account.api.lwork.com`，using HTTPS style to access


All values for the Vendor parameter MT4, MT5 must be capitalized.
For all interface requests, please include parameters in Headers


`x-api-tenantId`： `Tenant ID`，Query in product details in `SupportCenter` 
<br  />
`x-api-token`：`API Key`，Query in product details in `SupportCenter`
<br  />
`content-type` :`application/json`
<br  />

**Sample**

```http
GET https://account.api.lwork.com/v1/config/access HTTP/1.1

Accept: application/json
Accept-Encoding: gzip, deflate
Connection: keep-alive
Content-Length: 188
User-Agent: HTTPie/1.0.0-dev
cache-control: no-cache
content-type: application/json
x-api-token: myapikey
x-api-tenantid: mytennatid
```


