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
    "status" : true|false, 	//true if request processed OK, false if error
    "error"  : null|string, //description of the error or null
    "data"   : {}, 			//object with data
    "debug"  : null 		//optional, debug info (can be available at test enviropment)
}
```



## System endpoints

#### /v1/public/system/time

*Query information on system time*

```javascript
{
	"time": 1579868312476502	//System time, with microseconds (1/1000000 of sec)
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/system/time



#### /v1/public/system/status

*Query information on system status*

```javascript
{
	"status" : true|false,
	"error"  : null|"Invalid backend connection" 	//or any string with error
	"data"	 : {} 									//no data block for this request
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/system/status 



#### /v1/public/system/limits

**Coming soon**. *Query information on current rate limits ond/or API plan limits*

```javascript
{
	"data"	 : {}	//coming soon
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
            vol24h: 0,		//fiat currency has no vol. and mcap.
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
            price: 0.069534742,		//USD price rate
            vol24h: 0,				//fiat currency has no vol. and mcap.
            mcap: 0,
            cur: "USD",
            type: "fiat",			//type of base asset at rate, fiat or crypto
            symbol: "ZAR",			//symbol, e.g. ISO-code
            updated: 1579871102,
            source: "exchangeratesapi"	//underlaying source of the quote
        }
    } 
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/fx/ZAR 



## Indices

**Note**: This section of endpoints are under active development and will be extend to more information about index (constitution, historacal data etc.)



#### /v1/public/indices

*Fetch indices info and latest price*

```javascript
{
    "data": {
        ".ETHBON2H" : {					
            excode: "bitmex",			// Unique id of exchange 
            index_name: ".ETHBON2H",	// Name of index, in some cases non-unique
            info: null,					// Reserved for future extend, Ignore it now
            contracts: [ ],				// List of Contracts, associate with index
            constituents: null,			// Constituents of index. Coming soon.
            latest: 0.0003,				// Latest value
            updated_at: 1579874329		// Updated at, UNIX timestamp
        },
        "Binance JEX BTC Index" : {
            excode: "jex",
            index_name: "Binance JEX BTC Index",
            info: null,
            contracts: [				// List o f associated contracts	
            	{
            		id: 369,			//Unique id of Contract
            		ccode: "BTCUSDT"	//Contract code, in some cases can be non-unique
                }
            ],
            constituents: null,
            latest: 8458.216,
            updated_at: 1579874328
        },
        "FTX ADA Index" : {
            excode: "ftx",
            index_name: "FTX ADA Index",
            info: null,
            contracts: [
                {
                	id: 684,
               		ccode: "ADA-PERP"
                },
                {
                	id: 1224,
                	ccode: "ADA-0327"
                },
                {
                	id: 685,
                	ccode: "ADA-1227"
                }
            ],
            constituents: null,
            latest: 0.0440531,
            updated_at: 1579874318
        }
    }
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/indices



#### /v1/public/indices/symbols

*Coming soon. Fetch all indices symbols - unique, validate and  full-specified indices info*

```javascript
{
    "data" : [
        "OKE:ETCUSDINDEX",	//Array of index symbol by internal spec
        "HB:EOSUSDINDEX",
        "FTX:BTCUSDINDEX",
        "BBT:BTCUSDINDEX",
        "BMX:BXBT",
        "HB:LTCUSDINDEX",
        "HB:TRXUSDINDEX"
     ]
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/indices/symbols 



#### /v1/public/indices/<excode>

*Fetch indices info from specify exchange (using unique exchange code)*

```javascript
{
    "data": {
        "Binance ETHUSDT Index" : {
            excode: "binancedm",					// Unique id of exchange
            index_name: "Binance ETHUSDT Index",	// Name of index, non-unique
            info: null,						// Reserved for future. Ignore it now
            contracts: [					// List of Contracts, associate with index
                {
                	id: 1036,			//Unique id of Contract
                	ccode: "ETHUSDT"	//Contract code, in some cases can be non-unique
                }
            ],
            constituents: null,				// Constituents of index. Coming soon.
            latest: 162.74128669,			// Latest value
            updated_at: 1579876803			// Updated at, UNIX timestamp
        },
        "Binance XRPUSDT Index" : {
            excode: "binancedm",
            index_name: "Binance XRPUSDT Index",
            info: null,
            contracts: [
                {
               		id: 1558,
                	ccode: "XRPUSDT"
                }
            ],
            constituents: null,
            latest: 0.22515185,
            updated_at: 1579876805
        },
        "Binance LINKUSDT Index" : {
            excode: "binancedm",
            index_name: "Binance LINKUSDT Index",
            info: null,
            contracts: [
                {
                	id: 1706,
                	ccode: "LINKUSDT"
                }
            ],
            constituents: null,
            latest: 2.52759621,
            updated_at: 1579876804
        }        
    }
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/indices/binancedm



#### /v1/public/index/<symbol>

*Fetch one index data (using unique index symbol. All available symbols see at  /v1/public/indices/symbols )*

```javascript
{
	"data" : {
        excode: "ftx",
        index_name: "FTX BTC Index",
        info: {
            excode: "ftx",
            index_code: "FTX BTC Index",	//Old index name, for backword-compartability
            index_symbol: "FTX:BTCUSDIndex",	//Unique Symbol
            index_name: "FTX BTC Index",
            index_desc: "All elements of each index are weighted equally. All elements are capped at 30bp divergence from the median.",
            index_components_count: 10,
            index_updates_policy: "5s",
            index_methodology_url: "https://help.ftx.com/hc/en-us/articles/360027668812-Index-Calculation"
        },
		contracts: [
            {
            	id: 11,
            	ccode: "BTC-PERP"
            },
            {
            	id: 1460,
                ccode: "BTC-MOVE-2020Q4"
            },
            {
                id: 1041,
                ccode: "BTC-MOVE-2020Q3"
            },
            {
                id: 1459,
                ccode: "BTC-0626"
            }
		],
        constituents: null,
        latest: 8448.68772087,
        updated_at: 1579878635
	}
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/index/FTX:BTCUSDINDEX

#### /v1/public/index/<symbol>/history

*Fetch index value history (using unique index symbol. All available symbols see at  /v1/public/indices/symbols )*

At now, returns up to 500 latest values. K-lines and full-range historical values are coming.

```javascript
{
	"data" : [
        {
            latest: 8448.68772087,	//latest value of the index
            updated_at: 1579878635	//Unix datetime
        },
        {
            latest: 8466.7503925,
            updated_at: 1579878495
        },
        {
            latest: 8467.5909925,
            updated_at: 1579878421
        },
        {
            latest: 8462.14096174,
            updated_at: 1579878263
        }
    ]
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/index/FTX:BTCUSDINDEX/history