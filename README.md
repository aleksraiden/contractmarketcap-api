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
- At TEST Auth token can be any non-empty string, e.g. "test" or "<your_company_domain>"
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



## System endpoints

#### /v1/public/system/time

*Query information on system time*

```javascript
data: {
	time: 1579868312476502	//System time, with microseconds (1/1000000 of sec)
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/system/time



#### /v1/public/system/status

*Query information on system status*

```javascript
{
	"status" : true|false, //
    "error"  : null|"Invalid backend connection" //or any string with error
    "data"	 : {} //no data block for this request
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/system/status 



#### /v1/public/system/limits

**Coming soon**. *Query information on current rate limits ond/or API plan limits*

```javascript
{
	"data"	 : {} //coming soon
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/system/limits 



## FX endpoints

**Disclaimer**: FX rate data only for internal use! Not for public view or redistribution. 

Fiat currency supported: USD, ZAR, BGN, BRL, CAD, CHF, CNY, CZK, DKK, EUR, GBP, HKD, HRK, HUF, IDR, ILS, INR, ISK, JPY, KRW, MXN, MYR, NOK, NZD, PHP, PLN, RON, RUB, SEK, SGD, THB, TRY, AUD. 

If you are interest other currency - feel free co contact us: raiden@contractmarketcap.com 

#### /v1/public/fx

*Fetch Fx quotes, used for internal calculations.*

Supports fiat currency, such as EUR, RUB, CNY and crypto, e.g. BTC, ETH.

**Note**: All rates if about USD as a base. 

```javascript
{
    "data": {
        ZAR: {
            price: 0.069534742,
            vol24h: 0,	//fiat currency has no vol. and mcap.
            mcap: 0,
            cur: "USD",
            type: "fiat",	//type of base asset at rate, fiat or crypto
            symbol: "ZAR",	//symbol, e.g. ISO-code
            updated: 1579871102,
            source: "exchangeratesapi"	//underlaying source of the quote
        },
        DKK: {
            price: 0.14841032,
            vol24h: 0,
            mcap: 0,
            cur: "USD",
            type: "fiat",
            symbol: "DKK",
            updated: 1579871102,
            source: "exchangeratesapi"
        },
        BEAM: {
            price: 0.627677049489,
            vol24h: 24993206.1238783,
            mcap: 33694482.33611089,
            cur: "USD",
            type: "crypto",
            symbol: "BEAM",
            updated: 1579871052,
            source: "contractmarketcap"
        }
    } 
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/fx 



#### /v1/public/fx/<SYMBOL>

*Fetch Fx quote for individual currency symbol.*

Supports fiat currency, such as EUR, RUB, CNY and crypto, e.g. BTC, ETH.

**Note**: All rates if about USD as a base. 

```javascript
{
    "data": {
        ZAR: {	//<Symbol>
            price: 0.069534742, //USD price rate
            vol24h: 0,	//fiat currency has no vol. and mcap.
            mcap: 0,
            cur: "USD",
            type: "fiat",	//type of base asset at rate, fiat or crypto
            symbol: "ZAR",	//symbol, e.g. ISO-code
            updated: 1579871102,
            source: "exchangeratesapi"	//underlaying source of the quote
        }
    } 
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/fx/ZAR 