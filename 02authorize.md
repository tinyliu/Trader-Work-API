在

调用 API 时，必须提供租户编号 和 API Key 作为每个请求的身份验证。你可以在管理平台 Support Center 内管理你的 API Key。API Key 是租户在Account API 系统中的身份标识，请安全存储，确保其不要被泄露。如需更新，也可以在管理平台内进行更新。



调用地址：\`https://account.api.lwork.com\`，HTTPS方式访问。



所有获取 Vendor 参数的值 MT4、MT5 必须大写。

所有的接口请求，请在 Headers 里面加入参数

\`x-api-tenantId\`： \`租户 ID\`，在 \`SupportCenter\` 中的产品详情中查询

&lt;br /&gt;

\`x-api-token\`：\`API Key\`，可在 \`SupportCenter\` 的产品详情中查询

&lt;br /&gt;

\`content-type\` 为 \`application/json\`

&lt;br /&gt;

\*\*示例\*\*



\`\`\`http

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

\`\`\`

