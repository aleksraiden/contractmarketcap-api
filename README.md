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



## Exchanges

Note. Only active (connecting to our platform) exchanges or CFD brokers are presents. If you need a full list of known derivative-related project, contact us: raiden@contractmarketcap.com 



#### /v1/public/exchanges

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

Example: https://sandbox.contractmarketcap.com/v1/public/exchanges



#### /v1/public/exchange/<excode>

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

Example: https://sandbox.contractmarketcap.com/v1/public/exchange/bitmex



#### /v1/public/exchange/<excode>/info

*Fetch global info about specify exchange*

**Note**: Now are identical as a **/v1/public/exchange/<excode>**

Example: https://sandbox.contractmarketcap.com/v1/public/exchange/bitmex/info



#### /v1/public/exchange/<excode>/contracts

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

Example: https://sandbox.contractmarketcap.com/v1/public/exchange/bitmex/contracts



#### /v1/public/exchange/<excode>/indices

*Fetch exchange indices info*

**Note**: Now are identical as a **/v1/public/<excode>/indices**

Example: https://sandbox.contractmarketcap.com/v1/public/exchange/bitmex/indices



#### /v1/public/exchange/<excode>/stats

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

Example: https://sandbox.contractmarketcap.com/v1/public/exchange/bitmex/stats



#### /v1/public/exchange/<excode>/history

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

Example: https://sandbox.contractmarketcap.com/v1/public/exchange/bitmex/history

**Coming soon**: *at a future release, we are added <excode>/kline endpoint with UDF (TradingView) data for charts.*



#### /v1/public/exchange/<excode>/reports

*Coming soon*



#### /v1/public/exchange/<excode>/news

*Coming soon*





## Contracts



#### /v1/public/contracts

*All contracts specifications*

**Note**: Endpoint returns ALL of registered contracts, traded and expired. A lot of data (500+ Kb)!  If you need only active contracts, please, use /contracts/traded. 

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

Example: https://sandbox.contractmarketcap.com/v1/public/contracts



#### /v1/public/contracts/traded

*All currently traded contracts*. All fields identical to /contracts endpoint

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
            expiration_date: "",		//Date of contract expiration (empty for perpetual or d/M/Y, eg. 21/Mar/2020)
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

Example: https://sandbox.contractmarketcap.com/v1/public/contracts/traded



#### /v1/public/contracts/expired

*All currently expired contracts*. Data fields are identical to **/contracts** endpoint

Example: https://sandbox.contractmarketcap.com/v1/public/contracts/expired 



#### /v1/public/contracts/perpetual

*All perpetual (without expiration date) contracts*. Data fields are identical to **/contracts** endpoint

Note: Not of all perpetual contracts are traded! 

Example: https://sandbox.contractmarketcap.com/v1/public/contracts/perpetual



#### /v1/public/contracts/tod

*All contract, expired TODAY*. Data fields are identical to **/contracts** endpoint

Example: https://sandbox.contractmarketcap.com/v1/public/contracts/tod



#### /v1/public/contracts/tom

*All contract, expired TOMORROW*. Data fields are identical to **/contracts** endpoint

Example: https://sandbox.contractmarketcap.com/v1/public/contracts/tom



#### /v1/public/contracts/<excode>

*All contract by specify exchange*. Data fields are identical to **/contracts** endpoint

**Note**: Returns all contracts, not only traded now. 

Example: https://sandbox.contractmarketcap.com/v1/public/contracts/huobidm



#### /v1/public/contract/<id>

*Fetch info about one contract*. Data fields are identical to **/contracts** endpoint

Example: https://sandbox.contractmarketcap.com/v1/public/contract/1797



## Market data

Latest data for all contracts. Note, 5-min data updated currently. Real-time data streaming will be released at Q2'2020. Please, contact us if you need this: raiden@contractmarketcap.com



#### /v1/public/marketdata/summary

*All latest quotes by whole market*. 

```javascript
{
    "data" : {
        
    }
}
```





Example: https://sandbox.contractmarketcap.com/v1/public/marketdata/summary



#### /v1/public/marketdata/summary/from/<period>

*Latest quotes from one exchange*. Data fields are identical to /marketdata/summary endpoint

Example: https://sandbox.contractmarketcap.com/v1/public/contract/1797



#### /v1/public/marketdata/exchange/<excode>

*Latest quotes from one exchange*. Data fields are identical to /marketdata/summary endpoint

Example: https://sandbox.contractmarketcap.com/v1/public/contract/1797



#### /v1/public/marketdata/contract/<contractid>

*Latest quotes from one contract, specify by ID*. Data fields are identical to /marketdata/summary endpoint

Example: https://sandbox.contractmarketcap.com/v1/public/contract/1797



#### /v1/public/marketdata/contracts

*Latest quotes from one exchange*. Data fields are identical to /marketdata/summary endpoint

Example: https://sandbox.contractmarketcap.com/v1/public/contract/1797

