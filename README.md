# [Beta version] ContractMarketCap REST API
We are crypto derivative market data provider. 55 exchanges, 500+ traded contracts, 100% market coverage.
Official API docs and examples for ContractMarketCap site. Visit to: https://contractmarketcap.com


## URI

- All endpoints use HTTPS scheme
- **Test** integration: https://sandbox.contractmarketcap.com 
- **Prod**: https://api.contractmarketcap.com  (or individual endpoint at your location: api-us, api-eu, api-sg etc., *coming soon*)
- All endpoints return content with mime-type: application/json with HTTP Status code 200
- Use version at URI path as "v1" for all request
- Non-200 OK HTTP status code indicate Error at our server side.



## Auth

- Any API plan uses Auth token for each request (except /public/ URLs)
- Auth token has limited timelife - 1Y (one year or 365 * 24 * 3600 seconds) from generation. Contact us, if you need special offer.
- Auth token usages: 
  - cookie (not recommended)
  - http header with name X-CMC-KEY (best practice) 
  - http request param, named "**x-cmc-key**"
- Get your key (or revoke previous) at client area (https://clients.contractmarketcap.com, *coming soon*) or request vie e-mail: apikey@contractmarketcap.com 
- **Endpoints, started with /v1/public/ - are FREE without any Auth key**
- **Keep your key private and safe**!  



## REST-API

Universal response format (JSON):

```javascript
{
    "status" : true|false, 	//true if request processed OK, false if error
    "error"  : null|string, //description of the error or null
    "data"   : {}, 			//object with data
    "debug"  : null 		//optional, debug info (can be available at test enviropment)
}
```



## Endpoints list

|  Group   |                     Endpoint                      |                         Description                          | Auth |
| :------: | :-----------------------------------------------: | :----------------------------------------------------------: | :--: |
|  System  |              /v1/public/system/time               |                 *Get a current system time*                  |  NO  |
|  System  |             /v1/public/system/status              |             *Query information on system status*             |  NO  |
|  System  |             /v1/public/system/limits              |    *Gate a rate limits and/or API plan limits and usages*    |  NO  |
|    FX    |                    /v1/data/fx                    |         *FX quotes, used for internal calculations*          | YES  |
|    FX    |             /v1/data/fx/**%SYMBOL%**              |       *Fetch FX quote for individual currency symbol.*       | YES  |
|  Index   |                 /v1/data/indices                  |            *Fetch indices info and latest price*             | YES  |
|  Index   |             /v1/data/indices/symbols              | *Coming soon. Fetch all indices symbols - unique, validate and  full-specified indices info* | YES  |
|  Index   |           /v1/data/indices/**%EXCODE%**           | *Fetch indices info from specify exchange (using unique exchange code)* | YES  |
|  Index   |            /v1/data/index/**%SYMBOL%**            | *Fetch one index data (using unique index symbol. All available symbols see at*  **/indices/symbols** ) | YES  |
|  Index   |        /v1/data/index/**%SYMBOL%**/history        | *Fetch index value history (using unique index symbol. All available symbols see at  **/indices/symbols** )* | YES  |
| Exchange |                /v1/data/exchanges                 |    *Fetch global info about active (connected) exchanges*    | YES  |
| Exchange |          /v1/data/exchange/**%EXCODE%**           |          *Fetch global info about specify exchange*          | YES  |
| Exchange |        /v1/data/exchange/**%EXCODE%**/info        |          *Fetch global info about specify exchange*          | YES  |
| Exchange |     /v1/data/exchange/**%EXCODE%**/contracts      |       *Fetch all contract info  from specify exchange*       | YES  |
| Exchange |      /v1/data/exchange/**%EXCODE%**/indices       |                *Fetch exchange indices info*                 | YES  |
| Exchange |       /v1/data/exchange/**%EXCODE%**/stats        |          *Fetch stats info about specify exchange*           | YES  |
| Exchange |      /v1/data/exchange/**%EXCODE%**/history       |      *Fetch stats history info about specify exchange*       | YES  |
| Exchange |      /v1/data/exchange/**%EXCODE%**/reports       |                        *Coming soon*                         | YES  |
| Exchange |        /v1/data/exchange/**%EXCODE%**/news        |                        *Coming soon*                         | YES  |
| Contract |                /v1/data/contracts                 |                *All contracts specifications*                | YES  |
| Contract |             /v1/data/contracts/traded             |               *All currently traded contracts*               | YES  |
| Contract |            /v1/data/contracts/expired             |              *All currently expired contracts*               | YES  |
| Contract |           /v1/data/contracts/perpetual            |     *All perpetual (without expiration date) contracts*      | YES  |
| Contract |              /v1/data/contracts/tod               |                *All contract, expired TODAY*                 | YES  |
| Contract |              /v1/data/contracts/tom               |               *All contract, expired TOMORROW*               | YES  |
| Contract |          /v1/data/contracts/**%EXCODE%**          |              *All contract by specify exchange*              | YES  |
| Contract |            /v1/data/contract/**%ID%**             |               *Fetch info about one contract*                | YES  |
|  Quotes  |            /v1/data/marketdata/summary            |             *All latest quotes by whole market*              | YES  |
|  Quotes  |     /v1/data/marketdata/summary/**%PERIOD%**      |        *All latest quotes, traded at specify period*         | YES  |
|  Quotes  |     /v1/data/marketdata/exchange/**%EXCODE%**     |              *Latest quotes from one exchange*               | YES  |
|  Quotes  |       /v1/data/marketdata/contract/**%ID%**       |       *Latest quotes from one contract, specify by ID*       | YES  |
|  Quotes  | /v1/data/marketdata/contract/**%ID%**/history/eod | *End-of-day history market snapshot from one contract, specify by ID*. | YES  |
|  Quotes  |           /v1/data/marketdata/contracts           |            *Latest quotes for specify contracts*             | YES  |
|  Global  |              /v1/data/global/summary              |               *Global metrics by whole market*               | YES  |
|  Global  |       /v1/data/global/summary/**%SYMBOL%**        |                  *Global metrics by symbol*                  | YES  |





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
	"data"	 : {} 	//no data block for this request
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/system/status 



#### /v1/public/system/limits

*Query information on current rate limits and/or API plan limits*

Only API endpoint usages stats at now.

**HTTP Params**: ?x-cmc-key=%YOUR_AUTH_KEY%

```javascript
{
	"data"	 : {
        "usages" : {
            "/v1/data/fx": 2		//Endpoint and successful request counter
        }
        "total" : 1		//Total request with this auth key 
    }	
}
```

Example: https://sandbox.contractmarketcap.com/v1/public/system/limits?x-cmc-key=8b5a16245f20af97932b4a655720c66b



## FX endpoints

**Disclaimer**: FX rate data only for internal use! Not for public view or redistribution. 

Fiat currency supported: USD, ZAR, BGN, BRL, CAD, CHF, CNY, CZK, DKK, EUR, GBP, HKD, HRK, HUF, IDR, ILS, INR, ISK, JPY, KRW, MXN, MYR, NOK, NZD, PHP, PLN, RON, RUB, SEK, SGD, THB, TRY, AUD. 

If you are interest other currency - feel free co contact us: raiden@contractmarketcap.com 

#### /v1/data/fx

*Fetch FX quotes, used for internal calculations.*

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

Example: https://sandbox.contractmarketcap.com/v1/data/fx 



#### /v1/data/fx/%SYMBOL%

*Fetch FX quote for individual currency symbol.*

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

Example: https://sandbox.contractmarketcap.com/v1/data/fx/ZAR 



## Indices

**Note**: This section of endpoints are under active development and will be extend to more information about index (constitution, historical data etc.)



#### /v1/data/indices

*Fetch indices info and latest price*

```javascript
{
    "data": {
        ".ETHBON2H" : {					
            excode: "bitmex",			// Unique id of exchange 
            index_name: ".ETHBON2H",	// Name of index, in some cases non-unique
            info: null,				// Reserved for future extend, Ignore it now
            contracts: [ ],			// List of Contracts, associate with index
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

Example: https://sandbox.contractmarketcap.com/v1/data/indices



#### /v1/data/indices/symbols

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

Example: https://sandbox.contractmarketcap.com/v1/data/indices/symbols 



#### /v1/data/indices/%EXCODE%

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
                	id: 1036,		//Unique id of Contract
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

Example: https://sandbox.contractmarketcap.com/v1/data/indices/binancedm



#### /v1/data/index/%SYMBOL%

*Fetch one index data (using unique index symbol. All available symbols see at  **/v1/data/indices/symbols** )*

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

Example: https://sandbox.contractmarketcap.com/v1/data/index/FTX:BTCUSDINDEX

#### /v1/data/index/%SYMBOL%/history

*Fetch index value history (using unique index symbol. All available symbols see at  **/v1/data/indices/symbols** )*

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

Example: https://sandbox.contractmarketcap.com/v1/data/index/FTX:BTCUSDINDEX/history



## Exchanges

Note. Only active (connecting to our platform) exchanges or CFD brokers are presents. If you need a full list of known derivative-related project, contact us: raiden@contractmarketcap.com 



#### /v1/data/exchanges

*Fetch global info about active (connected) exchanges*

```javascript
{
	"data" : {
        deribit: {
            excode: "deribit",	//Unique exchange code
            info: {
                excode: "deribit",
                exname: "Deribit",	//Name
                exsite: "https://www.deribit.com",	//Official web site
                exjurisdiction: "Netherlands",		//Main jurisdiction
                exlegalentity: "",	//Official company entity. e.g. Company Name
                exdesc: "<p>Deribit is live since June 2016 after several years of development. John Jansen, the original founder, teamed up with Marius Jansen and Sebastian Smyczýnski. Deribit started as a Bitcoin Futures and Options trading platform going live in the summer of 2016. <p>Deribit has been created as an answer to those in search of a professional fully dedicated cryptocurrencies futures and options trading platform. A service that can create a fully liquid marketplace with the same standards as a traditional derivatives market. <p><b>Technology</b> <p>In order to secure normal exchange standards, the system has been in development for more than 2 years to assure extreme performance before going live. Entire framework of the platform has been developed to assure the ability to handle very large numbers of requests with ultra low latency (<1 ms). There is simply no other platform available that can guarantee similar performance. We developed our own matching engine from scratch and all of our technology is proprietary. We believe in Bitcoin and in the future of cryptocurrencies in general. We expect millions of traders will be trading cryptocurrencies at any given moment in time soon, and our platform is built with the potential to eventually serve those millions of users at the same time with real time low latency data.",
                exlogo: "https://contractmarketcap.com/images/deribit_exchange.png",
                extags: [ ]		//Tags, coming soon
            },
            oi24h: 239966019,	//24h total Open Interest, last reported, USD
            vol24h: 229698900,	//24h total trading volume (last reported), USD
            cur: "USD",			//Currency at oi24h and vol24h fields
            updated: 1579880103,
            source: "contractmarketcap"
        },
        kumex: {
        	excode: "kumex",
       		info: {
                excode: "kumex",
                exname: "KuMEX",
                exsite: "https://www.kumex.com/",
                exjurisdiction: "Seychelles",
                exlegalentity: "",
                exdesc: "",
                exlogo: "https://contractmarketcap.com/images/kumex_exchange.png",
                extags: [ ]
            },
            oi24h: 4404358,
            vol24h: 109511607,
            cur: "USD",
            updated: 1579880103,
            source: "contractmarketcap"
        }
    }
}
```

Example: https://sandbox.contractmarketcap.com/v1/data/exchanges



#### /v1/data/exchange/%EXCODE%

*Fetch global info about specify exchange*

```javascript
{
    "data": {
        excode: "bitmex",
        info: {
            _id: 4,
            excode: "bitmex",
            exname: "BitMEX",
            exsite: "https://www.bitmex.com",
            exjurisdiction: "Seychelles",
            exlegalentity: "",
            exdesc: "",
            exlogo: "https://contractmarketcap.com/images/bitmex_exchange.png",
            extags: ""
        },
        oi24h: 1371416433,
        vol24h: 2825731416,
        cur: "USD",
        updated: 1579881302,
        opendata: {		//Base metrics from open day, week, month and year
       		now: {
                vol24h: 2825731416,
                oi24h: 1371416433,
                updated: 1579882203
            },
            day: {
                vol24h: 3237955191,
                oi24h: 1369357234,
                updated: 1579824003
            },
            week: {
                vol24h: 4291953892,
                oi24h: 1324226567,
                updated: 1579478403
            },
            month: {
                vol24h: 1285347354,
                oi24h: 1065915172,
                updated: 1577836802
            },
            year: {
                vol24h: 1285347354,
                oi24h: 1065915172,
                updated: 1577836802
            }
        },
        source: "contractmarketcap"
    }
}
```

Example: https://sandbox.contractmarketcap.com/v1/data/exchange/bitmex



#### /v1/data/exchange/%EXCODE%/info

*Fetch global info about specify exchange*

**Note**: Now are identical as a **/v1/data/exchange/%excode%**

Example: https://sandbox.contractmarketcap.com/v1/data/exchange/bitmex/info



#### /v1/data/exchange/%EXCODE%/contracts

*Fetch all contract info  from specify exchange*

```javascript
{
	"data": {
        31: {
            id: 31,						//Contract ID
            excode: "bitmex",			//Exchange code
            ccode: "XRPU19",			//Contract symbol, not-unique at all cases
            ctype: "FUTURES",			//Type of contract (Futures, Swap, CFD, ETP etc.)
            cbase_asset: "COIN",		//Type of underlaying asset (Coin, Toke, Index)
            expiration: "27/Sep/2019",	//Date of expiration, if exists
            settlement_type: "CASH",	//Type of settlement (Cash or Delivery)
            nominated_type: "VANILLA",	//Type of nomination (Vanilla, Quanto, Inverse)
            index_name: ".BXRPXBT30M",	//Underlaying index code
            base: "XRP",				//Base asset code
            nominated: "BTC",			//Currency code
            contract_asset: "XRP",		//Lot asset
            contract_size: 1,			//Lot size (minimal contract size, not a minimal traded size)
            price_tick: 1e-8,			//Minimal price tick
            tradable: 0,				//Trading status, 0 - non-tradable, 1 - active
            updated: 1569584522			//Unix timestamp 
        },
        32: {
            id: 32,
            excode: "bitmex",
            ccode: "BCHU19",
            ctype: "FUTURES",
            cbase_asset: "COIN",
            expiration: "27/Sep/2019",
            settlement_type: "CASH",
            nominated_type: "VANILLA",
            index_name: ".BBCHXBT30M",
            base: "BCH",
            nominated: "BTC",
            contract_asset: "BCH",
            contract_size: 1,
            price_tick: 0.00001,
            tradable: 0,
            updated_at: "2019-09-27 11:42:02"
        }
    }
}
```

Example: https://sandbox.contractmarketcap.com/v1/data/exchange/bitmex/contracts



#### /v1/data/exchange/%EXCODE%/indices

*Fetch exchange indices info*

**Note**: Now are identical as a **/v1/data/%excode%/indices**

Example: https://sandbox.contractmarketcap.com/v1/data/exchange/bitmex/indices



#### /v1/data/exchange/%EXCODE%/stats

*Fetch stats info about specify exchange*

```javascript
{
    "data": {
        //Base exchange metrics from open day, week, month and year
        now: {
            vol24h: 2825731416,
            oi24h: 1371416433,
            updated: 1579882203
        },
        day: {
            vol24h: 3237955191,
            oi24h: 1369357234,
            updated: 1579824003
        },
        week: {
            vol24h: 4291953892,
            oi24h: 1324226567,
            updated: 1579478403
        },
        month: {
            vol24h: 1285347354,
            oi24h: 1065915172,
            updated: 1577836802
        },
        year: {
            vol24h: 1285347354,
            oi24h: 1065915172,
            updated: 1577836802
        }
    }
}
```

Example: https://sandbox.contractmarketcap.com/v1/data/exchange/bitmex/stats



#### /v1/data/exchange/%EXCODE%/history

*Fetch stats history (with 5 min interval) info about specify exchange*

```javascript
{
    "data": [
        {
            vol24h: 2905612398,
            oi24h: 1372123527,
            updated: 1579886402
        },
        {
            vol24h: 2904404168,
            oi24h: 1371779791,
            updated: 1579886102
        },
        {
            vol24h: 2903491181,
            oi24h: 1372187444,
            updated: 1579885802
        },
        {
            vol24h: 2900333483,
            oi24h: 1371022486,
            updated: 1579885503
        }
    ]
}
```

Example: https://sandbox.contractmarketcap.com/v1/data/exchange/bitmex/history

**Coming soon**: *at a future release, we are added %excode%/kline endpoint with UDF (TradingView) data for charts.*



#### /v1/data/exchange/%EXCODE%/reports

*Coming soon*



#### /v1/data/exchange/%EXCODE%/news

*Coming soon*





## Contracts



#### /v1/data/contracts

*All contracts specifications*

**Note**: Endpoint returns ALL of registered contracts, traded and expired. A lot of data (500+ Kb)!  If you need only active contracts, please, use **/contracts/traded** endpoint. 

```javascript
{
    "data": {
        1: {
            id: 1,						//unique ID of this contract
            excode: "ftx",				//Exchange code
            ccode: "ALGO-PERP",			//Symbol or exchange name of contract
            origital_id: "",			//If provide, internal exchange id of contract
            contract_type: "FUTURES",	//Type of contract (FUTURES, SWAP, CFD, ETP)
            asset_type: "COIN",			//Type of base asset: COIN, TOKEN, INDEX etc.
            expiration_date: "",		//Date of contract expiration (empty for perpetual)
            settlement_type: "CASH",	//Type of settlement (CASH, DELIVERY)
            nominated_type: "VANILLA",	//Type of nominal (VANILLA, INVERSE, QUANTO)
            index_name: "FTX ALGO Index", //Underlaying index name
            base: "ALGO",				//Base asset symbol
            nominated: "USD",			//Nominal currency symbol
            contract_asset: "ALGO",		//Asset of contract size
            contract_size: 1,			
            price_tick: 0.0001,			//Minimal price tick (in nominal currency)
            tradable: 1,				//1 - contract traded (live), 0 - non-traded
            symbol: ""					//Reserve, symbol for replace id in future
        },
        2: {
            id: 2,
            excode: "ftx",
            ccode: "ALGO-0927",
            origital_id: "",
            contract_type: "FUTURES",
            asset_type: "COIN",
            expiration_date: "27/Sep/2019",	//Date of contract expiration
            settlement_type: "CASH",
            nominated_type: "VANILLA",
            index_name: "FTX ALGO Index",
            base: "ALGO",
            nominated: "USD",
            contract_asset: "ALGO",
            contract_size: 1,
            price_tick: 0.0001,
            tradable: 0,
            symbol: ""
        }
    }
}
```

Example: https://sandbox.contractmarketcap.com/v1/data/contracts



#### /v1/data/contracts/traded

*All currently traded contracts*. All fields identical to **/contracts** endpoint

```javascript
{
    "data" : {
        1: {
            id: 1,						//unique ID of this contract
            excode: "ftx",				//Exchange code
            ccode: "ALGO-PERP",			//Symbol or exchange name of contract
            origital_id: "",			//If provide, internal exchange id of contract
            contract_type: "FUTURES",	//Type of contract (FUTURES, SWAP, CFD, ETP)
            asset_type: "COIN",			//Type of base asset: COIN, TOKEN, INDEX etc.
            expiration_date: "", //Date of contract expiration (empty or d/M/Y, eg. 21/Mar/2020)
            settlement_type: "CASH",	//Type of settlement (CASH, DELIVERY)
            nominated_type: "VANILLA",	//Type of nominal (VANILLA, INVERSE, QUANTO)
            index_name: "FTX ALGO Index", //Underlaying index name
            base: "ALGO",				//Base asset symbol
            nominated: "USD",			//Nominal currency symbol
            contract_asset: "ALGO",		//Asset of contract size
            contract_size: 1,			
            price_tick: 0.0001,			//Minimal price tick (in nominal currency)
            tradable: 1,				//1 - contract traded (live), 0 - non-traded
            symbol: ""					//Reserve, symbol for replace id in future
        }
    }
}
```

Example: https://sandbox.contractmarketcap.com/v1/data/contracts/traded



#### /v1/data/contracts/expired

*All currently expired contracts*. Data fields are identical to **/contracts** endpoint

Example: https://sandbox.contractmarketcap.com/v1/data/contracts/expired 



#### /v1/data/contracts/perpetual

*All perpetual (without expiration date) contracts*. Data fields are identical to **/contracts** endpoint

Note: Not of all perpetual contracts are traded! 

Example: https://sandbox.contractmarketcap.com/v1/data/contracts/perpetual



#### /v1/data/contracts/tod

*All contract, expired TODAY*. Data fields are identical to **/contracts** endpoint

Example: https://sandbox.contractmarketcap.com/v1/data/contracts/tod



#### /v1/data/contracts/tom

*All contract, expired TOMORROW*. Data fields are identical to **/contracts** endpoint

Example: https://sandbox.contractmarketcap.com/v1/data/contracts/tom



#### /v1/data/contracts/%EXCODE%

*All contract by specify exchange*. Data fields are identical to **/contracts** endpoint

**Note**: Returns all contracts, not only traded now. 

Example: https://sandbox.contractmarketcap.com/v1/data/contracts/huobidm



#### /v1/data/contract/%ID%

*Fetch info about one contract*. Data fields are identical to **/contracts** endpoint

Example: https://sandbox.contractmarketcap.com/v1/data/contract/1797



## Market data

Latest data for all contracts. Note, 5-min data updated currently. Real-time data streaming will be released at Q2'2020. 

**Note**: Some metrics available only for Enterprise plan: Open Interest and Volumes in contracts, currency, latest amount, Spread, raw price without rounding, 24h, 7day, 30day changes etc. 

Contact us for more info: raiden@contractmarketcap.com 



#### /v1/data/marketdata/summary

*All latest quotes by whole market*. Only traded now contracts are displayed.

Note: Most of fields identical to /v1/data/contracts endpoint. 

```javascript
{
    "data" : {
        1: {
            id: 1,
            excode: "ftx",
            ccode: "ALGO-PERP",
            contract_type: "FUTURES",
            asset_type: "COIN",
            expiration_date: "",
            nominated_type: "VANILLA",
            base: "ALGO",
            nominated: "USD",
            vol24h_base: 526456,			//24h trading volume (in base asset)
            vol24h_usd: 127122.36,			//24h volume in USD, internal calculated
            last_price: 0.2423,				//Last trade price
            best_bid: 0.2408,				//Best bid price
            best_ask: 0.2414,				//Best ask price
            open_price: 0.2420,				//Open price (from 00:00 UTC)
            oi24h_base: 3389647,			//Open Interest, in base asset
            oi24h_usd: 818491.8,			//Open Interest in USD, internal calculated
            fund_rate: 0					//Current funding rate, if available
        },
        3: {
            id: 3,
            excode: "ftx",
            ccode: "ALT-PERP",
            contract_type: "FUTURES",
            asset_type: "INDEX",
            expiration_date: "",
            nominated_type: "VANILLA",
            base: "ALT",
            nominated: "USD",
            vol24h_base: 9100.963,
            vol24h_usd: 3573847.81,
            last_price: 634.45,
            best_bid: 636.65,
            best_ask: 637,
            open_price: 634.01, 
            oi24h_base: 2219.149,
            oi24h_usd: 871435.34,
            fund_rate: 0
        }
    }
}
```

Example: https://sandbox.contractmarketcap.com/v1/data/marketdata/summary



#### /v1/data/marketdata/summary/%PERIOD%

*All latest quotes by whole market, traded at specify period are returned.* 

Period: **daily** (open day data, 00:00 by GMT), **weekly** and **monthly**.

**Note**: All of fields identical to /v1/data/contracts endpoint.  

**Note**: For any un-applicable period returns latest data

Example: https://sandbox.contractmarketcap.com/v1/data/marketdata/summary/daily

Example: https://sandbox.contractmarketcap.com/v1/data/marketdata/summary/weekly

Example: https://sandbox.contractmarketcap.com/v1/data/marketdata/summary/monthly



#### /v1/data/marketdata/exchange/%EXCODE%

*Latest quotes from one exchange*. Data fields are identical to **/marketdata/summary** endpoint

Example: https://sandbox.contractmarketcap.com/v1/marketdata/exchange/bbx



#### /v1/data/marketdata/contract/%ID%

*Latest quotes from one contract, specify by ID*. Data fields are identical to **/marketdata/summary** endpoint

Example: https://sandbox.contractmarketcap.com/v1/data/marketdata/contract/1797



#### /v1/data/marketdata/contract/%ID%/history/eod

*End-of-day history market snapshot from one contract, specify by ID*. 

Data fields are identical to **/marketdata/summary** endpoint. Available for last 7 day.

Example: https://sandbox.contractmarketcap.com/v1/data/marketdata/contract/1797/history/eod



#### /v1/data/marketdata/contracts

*Latest quotes for specify contracts*. Data fields are identical to **/marketdata/summary** endpoint

**Note**: Contract IDs set by param ?cid=1,2,3. Max: 100 ids per request.

**Note**: You may request latest price for any contract, traded or non-tradable now.

Example: https://sandbox.contractmarketcap.com/v1/data/marketdata/contracts?ids=1797,1791,1792



## Global market

**Note**: Metrics, provided by [CoinMarketCap](https://contractmarketcap.com) available only for non-commercial, individual use, as indicative metric.



#### /v1/data/global/summary

*Global metrics by whole market*. 

**Note**: some of global metrics provided by third-party contributors - CoinMarketCap and other.

```javascript
{
    "data" : {
        open_interest: 6754109440.38,			//Open Interest, in USD
        altcoin_open_interest: 1803773013.62,	//Open Interest, in USD (exclude BTC)
        dm_vol24h: 31588066765.25,				//24h trading volume, in USD
        altcoin_dm_vol24h: 13460356306.72,		//24h trading volume, in USD (exclude BTC)
        btc_dm_oi_dominance: 73.29,		//% of BTC domination in Open Interest
        btc_dm_vol_dominance: 57.39,	//% of BTC domination in trading volume
        btc_spot_dominance: 65.66,		//% of BTC domination at Spot (via CoinMarketCap)
        dm_products: 604,				//Count of traded contracts
        dm_perpetuals: 381,				//Count of perpetual contracts
        dm_swaps: 138,					//Count of swap contracts
        dm_futures: 453,				//Count of Futures
        dm_quanto: 37,					//Count of Quanto products (Futures, Swaps)
        dm_inverse: 68,					//Inverse nominated products (Futures, Swaps)
        btc_price: 8752.08,				//Latest BTC price, spot (via CoinMarketCap)
        spot_vol24h: 98452373185.82,	//Spot trading vol, 24h (via CoinMarketCap)
        spot_altcoin_vol24h: 72785633607.05, //Spot trading, exclude BTC (via CoinMarketCap)
        updated_at: 1580137802			//UTC timestamp
    }
}
```

Example: https://sandbox.contractmarketcap.com/v1/data/global/summary



#### /v1/data/global/summary/%SYMBOL%

*Global metrics by symbol*. 

**Note**: some of global metrics provided by third-party contributors - CoinMarketCap and other.

```javascript
{
    "data" : {
        symbol: "BTC",						//base symbol
        vol24h_usd: 18505314096.25,			//24h trading volume, derivatives, in USD
        vol24h_base: 2121219.53,			//24h trading volume, derivatives, in asset
        oi24h_usd: 4969451177.08,			//Open Interest, total, in USD
        oi24h_base: 1284342199.27,			//Open Interest, total, in asset
        spot_mcap_usd: 159529029869.83,		//Spot, market cap, USD (via CoinMarketCap)
        spot_vol24h_usd: 25869515532.82,	//Spot, trade vol, USD (via CoinMarketCap)
        spot_price_usd: 8772.69410837		//Spot, last price USD (via CoinMarketCap)
        updated_at: 1580137802				//UTC timestamp
    }
}
```

Example: https://sandbox.contractmarketcap.com/v1/data/global/summary/BTC
