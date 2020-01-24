# contractmarketcap-api
Official API docs and examples for ContractMarketCap site. Visit to: https://contractmarketcap.com



## URI

- All endpoints use HTTPS scheme
- **Test** integration: https://sandbox.contractmarketcap.com 
- **Prod**: https://api.contractmarketcap.com  (or individual endpoint at your location: api-us, api-eu, api-sg etc., *coming soon*)
- All endpoints return content with mime-type: application/json with HTTP Status code 200
- Use version at URI path as "v1" for all request
- Non-200 OK HTTP status code indicate Error at our server side.



## Auth

- Any API plan uses Auth token for each request
- Auth token has limited timelife - 1Y (one year or 365 * 24 * 3600 seconds) from generation. Contact us, if you need special offer.
- Auth token usages: 
  - cookie (not recomended)
  - http header with name X-CMC-KEY (best practice) 
  - http request param, named X-CMC-KEY
- Get your key (or revoke previos) at client area (https://clients.contractmarketcap.com, *coming soon*) or request vie e-mail: apikey@contractmarketcap.com 
- **Keep your key private and safe**!  



## REST-API

Universal responce format (JSON):

```javascript
{
    "status" : true|false, //true if request processed OK, false if error
    "error"  : null|string, //description of the error or null
    "data"   : {}, //object with data
    "debug"  : null //optional, debug info (can be available at test enviropment)
}
```



### 