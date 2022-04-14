# Binance Futures Public API Connector Python
[![Python 3.6](https://img.shields.io/badge/python-3.6+-blue.svg)](https://www.python.org/downloads/release/python-360/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This is a lightweight library that works as a connector to [Binance Futures public API](https://binance-docs.github.io/apidocs/futures/en/)

- Supported APIs:
    - USDT-M Futures `/fapi/*`
    - COIN-M Delivery `/dapi/*`
    - Futures/Delivery Websocket Market Stream
    - Futures/Delivery User Data Stream
- Inclusion of examples
- Customizable base URL, request timeout
- Response metadata can be displayed

## Installation

```bash
pip install binance-futures-connector
```

## Documentation
- USDT-M Futures: https://github.com/Binance-docs/binance-futures-connector-python/wiki/USDT-M-Futures
- COIN-M Futures: https://github.com/Binance-docs/binance-futures-connector-python/wiki/COIN-M-Futures

## RESTful APIs

Usage examples:
```python
from binance.futures import Futures 

client = Futures()
print(client.time())

client = Futures(key='<api_key>', secret='<api_secret>')

# Get account information
print(client.account())

# Post a new order
params = {
    'symbol': 'BTCUSDT',
    'side': 'SELL',
    'type': 'LIMIT',
    'timeInForce': 'GTC',
    'quantity': 0.002,
    'price': 59808
}

response = client.new_order(**params)
print(response)
```
Please find `examples` folder to check for more endpoints.

### Base URL

For USDT-M Futures, if `base_url` is not provided, it defaults to `fapi.binance.com`.<br/>
For COIN-M Delivery, if `base_url` is not provided, it defaults to `dapi.binance.com`.<br/>
It's recommended to pass in the `base_url` parameter, even in production as Binance provides alternative URLs

### Optional parameters

PEP8 suggests _lowercase with words separated by underscores_, but for this connector,
the methods' optional parameters should follow their exact naming as in the API documentation.

```python
# Recognised parameter name
response = client.query_order('BTCUSDT', orderListId=1)

# Unrecognised parameter name
response = client.query_order('BTCUSDT', order_list_id=1)
```

### RecvWindow parameter

Additional parameter `recvWindow` is available for endpoints requiring signature.<br/>
It defaults to `5000` (milliseconds) and can be any value lower than `60000`(milliseconds).
Anything beyond the limit will result in an error response from Binance server.

```python
from binance.futures import Futures as Client

client = Client(key, secret)
response = client.query_order('BTCUSDT', orderId=11, recvWindow=10000)
```

### Timeout

`timeout` is available to be assigned with the number of seconds you find most appropriate to wait for a server response.<br/>
Please remember the value as it won't be shown in error message _no bytes have been received on the underlying socket for timeout seconds_.<br/>
By default, `timeout` is None. Hence, requests do not time out.

```python
from binance.futures import Futures as Client

client= Client(timeout=1)
```

### Proxy
proxy is supported

```python
from binance.futures import Futures as Client

proxies = { 'https': 'http://1.2.3.4:8080' }

client= Client(proxies=proxies)
```

### Response Metadata

The Binance API server provides weight usages in the headers of each response.
You can display them by initializing the client with `show_limit_usage=True`:

```python[
  {
    "name": "Bitcoin",
    "symbol": "₿",
    "rank": 1,
    "age": 4836,
    "color": "#fa9e32",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/btc.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/btc.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/btc.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/btc.webp",
    "exchanges": 184,
    "markets": 4346,
    "pairs": 1519,
    "allTimeHighUSD": 68780.77475755227,
    "circulatingSupply": 18999806,
    "totalSupply": 18999806,
    "maxSupply": 21000000,
    "links": {
      "website": "https://bitcoin.org/",
      "whitepaper": "https://bitcoin.org/bitcoin.pdf",
      "twitter": null,
      "reddit": "https://reddit.com/r/bitcoin",
      "telegram": null,
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "BTC",
    "rate": 45202.24845859232,
    "volume": 26572560432,
    "cap": 858833951477
  },
  {
    "name": "Ethereum",
    "symbol": "Ξ",
    "rank": 2,
    "age": 2437,
    "color": "#282a2a",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/eth.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/eth.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/eth.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/eth.webp",
    "exchanges": 242,
    "markets": 4656,
    "pairs": 2382,
    "allTimeHighUSD": 4861.288700810894,
    "circulatingSupply": 120198947,
    "totalSupply": 120198947,
    "maxSupply": null,
    "links": {
      "website": "https://www.ethereum.org/",
      "whitepaper": "https://github.com/ethereum/wiki/wiki/White-Paper",
      "twitter": "https://twitter.com/ethereum",
      "reddit": "https://reddit.com/r/ethereum",
      "telegram": null,
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "ETH",
    "rate": 3290.2314819375374,
    "volume": 16164585366,
    "cap": 395482359515
  },
  {
    "name": "Tether",
    "rank": 3,
    "age": 2831,
    "color": "#24a37c",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/usdt.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/usdt.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/usdt.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/usdt.webp",
    "exchanges": 209,
    "markets": 10485,
    "pairs": 3635,
    "allTimeHighUSD": 1.053,
    "circulatingSupply": 81978743040,
    "totalSupply": 81978743040,
    "maxSupply": null,
    "links": {
      "website": "https://tether.to",
      "whitepaper": "https://tether.to/wp-content/uploads/2015/04/Tether-White-Paper.pdf",
      "twitter": "https://twitter.com/tether_to",
      "reddit": "https://reddit.com/r/Tether",
      "telegram": "https://t.me/OfficialTether",
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "USDT",
    "rate": 1.0053197928505537,
    "volume": 72841835259,
    "cap": 82414852971
  },
  {
    "name": "BNB",
    "rank": 4,
    "age": 1735,
    "color": "#f4bc2c",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/bnb.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/bnb.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/bnb.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/bnb.webp",
    "exchanges": 105,
    "markets": 3837,
    "pairs": 3411,
    "allTimeHighUSD": 691.55924082575,
    "circulatingSupply": 165123057,
    "totalSupply": 165123057,
    "maxSupply": 197192382,
    "links": {
      "website": "https://www.binance.com/",
      "whitepaper": "https://www.binance.com/resources/ico/Binance_WhitePaper_en.pdf",
      "twitter": "https://twitter.com/binance",
      "reddit": "https://reddit.com/r/binance",
      "telegram": "https://t.me/binanceexchange\\",
      "discord": null,
      "medium": null,
      "instagram": "https://instagram.com/Binance"
    },
    "code": "BNB",
    "rate": 432.777075964748,
    "volume": 3381040719,
    "cap": 71461473783
  },
  {
    "name": "USD Coin",
    "rank": 5,
    "age": 1270,
    "color": "#eef4f9",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/usdc.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/usdc.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/usdc.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/usdc.webp",
    "exchanges": 163,
    "markets": 1157,
    "pairs": 686,
    "allTimeHighUSD": 1.087,
    "circulatingSupply": 51391437922,
    "totalSupply": 51391437922,
    "maxSupply": null,
    "links": {
      "website": "https://www.centre.io/usdc",
      "whitepaper": "https://www.centre.io/pdfs/centre-whitepaper.pdf",
      "twitter": null,
      "reddit": null,
      "telegram": null,
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "USDC",
    "rate": 1.0032977856081595,
    "volume": 2813270932,
    "cap": 51560915866
  },
  {
    "name": "Solana",
    "rank": 6,
    "age": 718,
    "color": "#68a1c9",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/sol.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/sol.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/sol.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/sol.webp",
    "exchanges": 44,
    "markets": 144,
    "pairs": 67,
    "allTimeHighUSD": 259.8360337991446,
    "circulatingSupply": 325673234,
    "totalSupply": 518596084,
    "maxSupply": null,
    "links": {
      "website": "https://solana.com/",
      "whitepaper": "https://docs.solana.com/",
      "twitter": "https://twitter.com/solana",
      "reddit": "https://reddit.com/r/solana",
      "telegram": "https://t.me/solana",
      "discord": "https://discord.com/invite/pquxPsq",
      "medium": "https://medium.com/solana-labs",
      "instagram": "https://www.instagram.com/solana_updates"
    },
    "code": "SOL",
    "rate": 125.06373188871142,
    "volume": 4032569507,
    "cap": 40729910020
  },
  {
    "name": "XRP",
    "rank": 7,
    "age": 3743,
    "color": "#040404",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/xrp.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/xrp.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/xrp.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/xrp.webp",
    "exchanges": 101,
    "markets": 398,
    "pairs": 168,
    "allTimeHighUSD": 3.91546307780967,
    "circulatingSupply": 48121609012,
    "totalSupply": 99989656524,
    "maxSupply": 100000000000,
    "links": {
      "website": "https://ripple.com/",
      "whitepaper": "https://interledger.org/interledger.pdf",
      "twitter": "https://twitter.com/Ripple",
      "reddit": "https://reddit.com/r/ripple",
      "telegram": "https://t.me/Ripple",
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "XRP",
    "rate": 0.8190793396265856,
    "volume": 2207042258,
    "cap": 39415415731
  },
  {
    "name": "Cardano",
    "rank": 8,
    "age": 1642,
    "color": "#226ed8",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ada.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ada.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ada.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ada.webp",
    "exchanges": 59,
    "markets": 171,
    "pairs": 50,
    "allTimeHighUSD": 3.0939096930263132,
    "circulatingSupply": 32066262182,
    "totalSupply": 32066262182,
    "maxSupply": 45000000000,
    "links": {
      "website": "https://www.cardano.org/",
      "whitepaper": "https://www.cardanohub.org/en/academic-papers/",
      "twitter": "https://twitter.com/Cardano",
      "reddit": "https://reddit.com/r/cardano",
      "telegram": "https://t.me/cardano",
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "ADA",
    "rate": 1.13720264425499,
    "volume": 1394020909,
    "cap": 36465838145
  },
  {
    "name": "Terra",
    "rank": 9,
    "age": 1899,
    "color": "#f3cf3a",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/luna.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/luna.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/luna.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/luna.webp",
    "exchanges": 39,
    "markets": 79,
    "pairs": 25,
    "allTimeHighUSD": 111.18875307705339,
    "circulatingSupply": 354234304,
    "totalSupply": 993680819,
    "maxSupply": null,
    "links": {
      "website": "https://terra.money/",
      "whitepaper": "https://docs.terra.money/",
      "twitter": "https://twitter.com/terra_money",
      "reddit": "https://reddit.com/r/terraluna",
      "telegram": "https://t.me/TerraLunaChat",
      "discord": "https://discord.gg/bYfyhUT",
      "medium": "https://medium.com/terra-money",
      "instagram": null
    },
    "code": "LUNA",
    "rate": 102.91609813241061,
    "volume": 2029089165,
    "cap": 36456412392
  },
  {
    "name": "Avalanche",
    "rank": 10,
    "age": 556,
    "color": "#eb4343",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/avax.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/avax.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/avax.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/avax.webp",
    "exchanges": 41,
    "markets": 280,
    "pairs": 187,
    "allTimeHighUSD": 146.46571588270493,
    "circulatingSupply": 267336881,
    "totalSupply": 377752194,
    "maxSupply": 720000000,
    "links": {
      "website": "https://www.avax.network/",
      "whitepaper": "https://docs.avax.network/",
      "twitter": "https://twitter.com/avalancheavax",
      "reddit": "https://reddit.com/r/Avax",
      "telegram": "https://t.me/avalancheavax",
      "discord": "https://discord.com/invite/RwXY7P6",
      "medium": "https://medium.com/avalancheavax",
      "instagram": null
    },
    "code": "AVAX",
    "rate": 93.71599570629708,
    "volume": 1220565357,
    "cap": 25053741992
  },
  {
    "name": "Polkadot",
    "rank": 11,
    "age": 585,
    "color": "#e4047c",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/dot.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/dot.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/dot.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/dot.webp",
    "exchanges": 48,
    "markets": 100,
    "pairs": 19,
    "allTimeHighUSD": 55.06538188283951,
    "circulatingSupply": 1032470090,
    "totalSupply": 1179274936,
    "maxSupply": null,
    "links": {
      "website": "https://polkadot.network/",
      "whitepaper": "https://polkadot.network/PolkaDotPaper.pdf",
      "twitter": "https://twitter.com/Polkadot",
      "reddit": "https://reddit.com/r/dot",
      "telegram": "https://t.me/PolkadotOfficial",
      "discord": "https://discord.com/invite/wGUDt2p",
      "medium": "https://medium.com/polkadot-network",
      "instagram": null
    },
    "code": "DOT",
    "rate": 21.196883287782324,
    "volume": 759825347,
    "cap": 21885147996
  },
  {
    "name": "Dogecoin",
    "symbol": "Ɖ",
    "rank": 12,
    "age": 3028,
    "color": "#eee0b6",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/doge.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/doge.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/doge.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/doge.webp",
    "exchanges": 75,
    "markets": 178,
    "pairs": 59,
    "allTimeHighUSD": 0.7371712183910056,
    "circulatingSupply": 132670764299,
    "totalSupply": 132670764299,
    "maxSupply": null,
    "links": {
      "website": "http://dogecoin.com/",
      "whitepaper": null,
      "twitter": "https://twitter.com/dogecoin",
      "reddit": "https://reddit.com/r/dogecoin",
      "telegram": null,
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "DOGE",
    "rate": 0.1369368539033251,
    "volume": 893970329,
    "cap": 18167517068
  },
  {
    "name": "Binance USD",
    "rank": 13,
    "age": 923,
    "color": "#f4bc0c",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/busd.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/busd.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/busd.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/busd.webp",
    "exchanges": 52,
    "markets": 1008,
    "pairs": 876,
    "allTimeHighUSD": 1.24997330944831,
    "circulatingSupply": 17477605024,
    "totalSupply": 17477605024,
    "maxSupply": null,
    "links": {
      "website": "https://www.paxos.com/busd/",
      "whitepaper": null,
      "twitter": "https://twitter.com/PaxosGlobal",
      "reddit": null,
      "telegram": null,
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "BUSD",
    "rate": 1.0089969968388244,
    "volume": 4760523919,
    "cap": 17634850981
  },
  {
    "name": "TerraUSD",
    "rank": 14,
    "age": 1465,
    "color": "#e28a8b",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ust.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ust.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ust.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ust.webp",
    "exchanges": 26,
    "markets": 159,
    "pairs": 97,
    "allTimeHighUSD": 1.0996799121413454,
    "circulatingSupply": 16422578880,
    "totalSupply": 16422591519,
    "maxSupply": null,
    "links": {
      "website": "https://www.terra.money/",
      "whitepaper": "https://www.terra.money/Terra_White_paper.pdf",
      "twitter": "https://twitter.com/terra_money",
      "reddit": "https://reddit.com/r/terraluna",
      "telegram": "https://t.me/TerraLunaChat",
      "discord": "https://discord.com/invite/e29HWwC2Mz",
      "medium": "https://medium.com/terra-money",
      "instagram": null
    },
    "code": "UST",
    "rate": 1.002527922359892,
    "volume": 477795036,
    "cap": 16464093884
  },
  {
    "name": "Shiba Inu",
    "rank": 15,
    "age": 422,
    "color": "#faa00d",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/shib.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/shib.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/shib.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/shib.webp",
    "exchanges": 59,
    "markets": 84,
    "pairs": 17,
    "allTimeHighUSD": 0.00008877244256117385,
    "circulatingSupply": 589656772081074,
    "totalSupply": 999991708924817,
    "maxSupply": null,
    "links": {
      "website": "https://shibatoken.com/",
      "whitepaper": "https://raw.githubusercontent.com/shytoshikusama/woofwoofpaper/main/SHIBA_INU_WOOF_WOOF.pdf",
      "twitter": "https://twitter.com/shibtoken",
      "reddit": "https://reddit.com/r/SHIBArmy",
      "telegram": "https://t.me/shibainuthedogecoinkiller",
      "discord": "https://discord.com/invite/shibatoken",
      "medium": "https://allhailtheshiba.medium.com",
      "instagram": null
    },
    "code": "SHIB",
    "rate": 0.000025619648207888315,
    "volume": 1450759225,
    "cap": 15106799064
  },
  {
    "name": "Polygon",
    "rank": 16,
    "age": 1068,
    "color": "#8444e4",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/matic.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/matic.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/matic.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/matic.webp",
    "exchanges": 76,
    "markets": 327,
    "pairs": 182,
    "allTimeHighUSD": 2.920098783754804,
    "circulatingSupply": 7701069511,
    "totalSupply": 10000000000,
    "maxSupply": 10000000000,
    "links": {
      "website": "https://polygon.technology/",
      "whitepaper": "https://polygon.technology/lightpaper-polygon.pdf",
      "twitter": "https://twitter.com/0xPolygon",
      "reddit": "https://reddit.com/r/0xPolygon",
      "telegram": "https://t.me/maticnetwork",
      "discord": "https://discord.gg/tuETxzrh",
      "medium": "https://polygontech.medium.com/",
      "instagram": null
    },
    "code": "MATIC",
    "rate": 1.6405758263252737,
    "volume": 616352395,
    "cap": 12634188477
  },
  {
    "name": "Cronos",
    "rank": 17,
    "age": 1115,
    "color": "#f6f7f8",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/cro.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/cro.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/cro.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/cro.webp",
    "exchanges": 26,
    "markets": 90,
    "pairs": 48,
    "allTimeHighUSD": 0.9516849148075289,
    "circulatingSupply": 25263013692,
    "totalSupply": 30263013692,
    "maxSupply": null,
    "links": {
      "website": "https://www.crypto.com/en/chain",
      "whitepaper": "https://crypto.com/images/crypto_com_whitepaper.pdf",
      "twitter": "https://twitter.com/cryptocom",
      "reddit": "https://reddit.com/r/Crypto_com",
      "telegram": "https://t.me/CryptoComOfficial",
      "discord": "https://discord.com/invite/nsp9JTC",
      "medium": "https://medium.com/@crypto.com",
      "instagram": "https://instagram.com/cryptocomofficial"
    },
    "code": "CRO",
    "rate": 0.4622545123730912,
    "volume": 191337512,
    "cap": 11677942075
  },
  {
    "name": "Dai",
    "rank": 18,
    "age": 1556,
    "color": "#fcb934",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/dai.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/dai.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/dai.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/dai.webp",
    "exchanges": 93,
    "markets": 228,
    "pairs": 89,
    "allTimeHighUSD": 1.3998062062282162,
    "circulatingSupply": 9359045406,
    "totalSupply": 10931954334,
    "maxSupply": null,
    "links": {
      "website": "http://www.makerdao.com/",
      "whitepaper": "https://makerdao.com/whitepaper/",
      "twitter": "https://twitter.com/makerDAO",
      "reddit": "https://reddit.com/r/makerDAO",
      "telegram": "https://t.me/makerdaoOfficial",
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "DAI",
    "rate": 1.0078531743959793,
    "volume": 256945488,
    "cap": 9432543622
  },
  {
    "name": "NEAR Protocol",
    "rank": 19,
    "age": 523,
    "color": "#040404",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/near.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/near.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/near.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/near.webp",
    "exchanges": 18,
    "markets": 38,
    "pairs": 17,
    "allTimeHighUSD": 20.46907836312353,
    "circulatingSupply": 659151927,
    "totalSupply": 1000000000,
    "maxSupply": 1000000000,
    "links": {
      "website": "https://near.org/",
      "whitepaper": "https://docs.near.org/docs/develop/basics/getting-started",
      "twitter": "https://twitter.com/nearprotocol",
      "reddit": "https://reddit.com/r/nearprotocol",
      "telegram": "https://t.me/cryptonear",
      "discord": "https://discord.com/invite/UY9Xf2k",
      "medium": "https://medium.com/nearprotocol",
      "instagram": "https://www.instagram.com/near_protocol"
    },
    "code": "NEAR",
    "rate": 13.373760506787978,
    "volume": 313842494,
    "cap": 8815340009
  },
  {
    "name": "Litecoin",
    "rank": 20,
    "age": 3828,
    "color": "#345c9c",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ltc.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ltc.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ltc.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ltc.webp",
    "exchanges": 124,
    "markets": 332,
    "pairs": 92,
    "allTimeHighUSD": 413.0840556096379,
    "circulatingSupply": 69971881,
    "totalSupply": 69971881,
    "maxSupply": 84000000,
    "links": {
      "website": "https://litecoin.com",
      "whitepaper": null,
      "twitter": "https://twitter.com/LitecoinDotCom",
      "reddit": "https://reddit.com/r/litecoin",
      "telegram": "https://t.me/litecoin",
      "discord": null,
      "medium": null,
      "instagram": "https://instagram.com/litecoindotcom"
    },
    "code": "LTC",
    "rate": 122.32637029484133,
    "volume": 926799605,
    "cap": 8559406225
  },
  {
    "name": "Chainlink",
    "rank": 21,
    "age": 1658,
    "color": "#2959d9",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/link.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/link.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/link.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/link.webp",
    "exchanges": 108,
    "markets": 213,
    "pairs": 40,
    "allTimeHighUSD": 52.85091701629316,
    "circulatingSupply": 467009552,
    "totalSupply": 1000000000,
    "maxSupply": 1000000000,
    "links": {
      "website": "https://chain.link/",
      "whitepaper": "https://research.chain.link/whitepaper-v2.pdf?_ga=2.202988689.147345121.1621217890-329871762.1621217890",
      "twitter": "https://twitter.com/chainlink",
      "reddit": "https://reddit.com/r/chainlink",
      "telegram": "https://t.me/chainlinkofficial",
      "discord": "https://discord.gg/aSK4zew",
      "medium": null,
      "instagram": null
    },
    "code": "LINK",
    "rate": 16.79768095891131,
    "volume": 522446260,
    "cap": 7844677459
  },
  {
    "name": "Uniswap",
    "rank": 22,
    "age": 561,
    "color": "#fc047c",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/uni.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/uni.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/uni.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/uni.webp",
    "exchanges": 58,
    "markets": 113,
    "pairs": 21,
    "allTimeHighUSD": 45.009351908031135,
    "circulatingSupply": 688112936,
    "totalSupply": 1000000000,
    "maxSupply": 1000000000,
    "links": {
      "website": "https://uniswap.org/",
      "whitepaper": "https://uniswap.org/whitepaper-v3.pdf",
      "twitter": "https://twitter.com/Uniswap",
      "reddit": "https://reddit.com/r/Uniswap",
      "telegram": null,
      "discord": "https://discord.com/invite/FCfyBSbCU5",
      "medium": null,
      "instagram": null
    },
    "code": "UNI",
    "rate": 11.224820058615112,
    "volume": 203403810,
    "cap": 7723943887
  },
  {
    "name": "Bitcoin Cash",
    "rank": 23,
    "age": 1704,
    "color": "#f1f9f7",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/bch.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/bch.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/bch.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/bch.webp",
    "exchanges": 88,
    "markets": 205,
    "pairs": 45,
    "allTimeHighUSD": 4355.62,
    "circulatingSupply": 19024493,
    "totalSupply": 19024493,
    "maxSupply": 21000000,
    "links": {
      "website": "https://www.bitcoincash.org/",
      "whitepaper": "https://www.bitcoincash.org/bitcoin.pdf",
      "twitter": "https://twitter.com/Bitcoin",
      "reddit": "https://reddit.com/r/Bitcoincash",
      "telegram": null,
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "BCH",
    "rate": 369.9625014063041,
    "volume": 450336471,
    "cap": 7038349018
  },
  {
    "name": "Cosmos",
    "rank": 24,
    "age": 1999,
    "color": "#30334c",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/atom.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/atom.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/atom.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/atom.webp",
    "exchanges": 45,
    "markets": 118,
    "pairs": 39,
    "allTimeHighUSD": 44.477623439842205,
    "circulatingSupply": 238524829,
    "totalSupply": 238524829,
    "maxSupply": null,
    "links": {
      "website": "https://cosmos.network/",
      "whitepaper": "https://cosmos.network/resources/whitepaper",
      "twitter": "https://twitter.com/cosmos",
      "reddit": "https://reddit.com/r/cosmosnetwork",
      "telegram": "https://t.me/cosmosproject",
      "discord": "https://discord.com/invite/cosmosnetwork",
      "medium": null,
      "instagram": null
    },
    "code": "ATOM",
    "rate": 28.418357479967675,
    "volume": 677431597,
    "cap": 6778483858
  },
  {
    "name": "FTX Token",
    "rank": 25,
    "age": 962,
    "color": "#5cccdc",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ftt.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ftt.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ftt.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ftt.webp",
    "exchanges": 29,
    "markets": 48,
    "pairs": 16,
    "allTimeHighUSD": 83.82453603463195,
    "circulatingSupply": 137298428,
    "totalSupply": 333167763,
    "maxSupply": null,
    "links": {
      "website": "https://ftx.com",
      "whitepaper": "https://docs.google.com/document/d/1u5MOkENoWP8PGcjuoKqRkNP5Gl1LLRB9JvAHwffQ7ec/view",
      "twitter": "https://twitter.com/FTX_Official",
      "reddit": "https://reddit.com/r/FTX_Official",
      "telegram": "https://t.me/FTX_Official",
      "discord": null,
      "medium": "https://ftx.medium.com/",
      "instagram": "https://www.instagram.com/ftx_official"
    },
    "code": "FTT",
    "rate": 49.02980840700165,
    "volume": 142083454,
    "cap": 6731715619
  },
  {
    "name": "Axie Infinity",
    "rank": 26,
    "age": 512,
    "color": "#0f48b3",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/axs.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/axs.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/axs.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/axs.webp",
    "exchanges": 32,
    "markets": 52,
    "pairs": 12,
    "allTimeHighUSD": 164.99042139105435,
    "circulatingSupply": 102896038,
    "totalSupply": 270000000,
    "maxSupply": 270000000,
    "links": {
      "website": "https://axieinfinity.com/",
      "whitepaper": "https://whitepaper.axieinfinity.com/",
      "twitter": "https://twitter.com/axieinfinity",
      "reddit": "https://reddit.com/r/axieinfinity",
      "telegram": "https://t.me/axieinfinity",
      "discord": "https://discord.com/invite/axie",
      "medium": "https://axieinfinity.medium.com/",
      "instagram": "https://www.instagram.com/axieinfinity/"
    },
    "code": "AXS",
    "rate": 62.77046573445521,
    "volume": 176863230,
    "cap": 6458832227
  },
  {
    "name": "Algorand",
    "rank": 27,
    "age": 1014,
    "color": "#efefef",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/algo.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/algo.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/algo.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/algo.webp",
    "exchanges": 37,
    "markets": 111,
    "pairs": 50,
    "allTimeHighUSD": 3.228152051480498,
    "circulatingSupply": 6638291350,
    "totalSupply": 10000000000,
    "maxSupply": 10000000000,
    "links": {
      "website": "http://algorand.foundation",
      "whitepaper": "https://www.algorand.com/resources/white-papers",
      "twitter": "https://twitter.com/AlgoFoundation",
      "reddit": "https://reddit.com/r/AlgorandOfficial",
      "telegram": "https://t.me/AlgorandFoundation",
      "discord": "https://discord.gg/84AActu3at",
      "medium": "https://medium.com/algorand",
      "instagram": null
    },
    "code": "ALGO",
    "rate": 0.926547865988032,
    "volume": 181022608,
    "cap": 6150694684
  },
  {
    "name": "Ethereum Classic",
    "rank": 28,
    "age": 2076,
    "color": "#3ab93a",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/etc.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/etc.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/etc.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/etc.webp",
    "exchanges": 57,
    "markets": 122,
    "pairs": 25,
    "allTimeHighUSD": 168.98136358110946,
    "circulatingSupply": 133926314,
    "totalSupply": 210700000,
    "maxSupply": null,
    "links": {
      "website": "https://ethereumclassic.github.io/",
      "whitepaper": "https://github.com/ethereum/wiki/wiki/White-Paper",
      "twitter": "https://twitter.com/eth_classic",
      "reddit": "https://reddit.com/r/EthereumClassic",
      "telegram": null,
      "discord": "https://discord.gg/HW4GckH",
      "medium": null,
      "instagram": null
    },
    "code": "ETC",
    "rate": 45.38022499261541,
    "volume": 1356805049,
    "cap": 6077606262
  },
  {
    "name": "Decentraland",
    "rank": 29,
    "age": 1657,
    "color": "#f64e5c",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/mana.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/mana.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/mana.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/mana.webp",
    "exchanges": 44,
    "markets": 83,
    "pairs": 15,
    "allTimeHighUSD": 5.871183877218146,
    "circulatingSupply": 2193713527,
    "totalSupply": 2193713527,
    "maxSupply": 2805886393,
    "links": {
      "website": "https://decentraland.org/",
      "whitepaper": "https://decentraland.org/whitepaper.pdf",
      "twitter": "https://twitter.com/decentraland",
      "reddit": "https://reddit.com/r/decentraland",
      "telegram": "https://t.me/dcl_market",
      "discord": "https://decentraland.org/discord/",
      "medium": "https://medium.com/@decentraland",
      "instagram": "https://www.instagram.com/decentralandoficial/"
    },
    "code": "MANA",
    "rate": 2.5707327350302838,
    "volume": 407055270,
    "cap": 5639451175
  },
  {
    "name": "Stellar",
    "rank": 30,
    "age": 2795,
    "color": "#040404",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/xlm.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/xlm.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/xlm.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/xlm.webp",
    "exchanges": 74,
    "markets": 203,
    "pairs": 57,
    "allTimeHighUSD": 0.921644339265009,
    "circulatingSupply": 24708878393,
    "totalSupply": 50001787982,
    "maxSupply": null,
    "links": {
      "website": "https://www.stellar.org",
      "whitepaper": "https://www.stellar.org/papers/stellar-consensus-protocol.pdf",
      "twitter": "https://twitter.com/StellarOrg",
      "reddit": "https://reddit.com/r/stellar",
      "telegram": null,
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "XLM",
    "rate": 0.22601868337379055,
    "volume": 364768508,
    "cap": 5584668162
  },
  {
    "name": "Waves",
    "rank": 31,
    "age": 2128,
    "color": "#36b3d5",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/waves.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/waves.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/waves.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/waves.webp",
    "exchanges": 37,
    "markets": 81,
    "pairs": 19,
    "allTimeHighUSD": 62.082895092645494,
    "circulatingSupply": 100000000,
    "totalSupply": 100000000,
    "maxSupply": null,
    "links": {
      "website": "https://wavesplatform.com/",
      "whitepaper": "https://blog.wavesplatform.com/waves-whitepaper-164dd6ca6a23",
      "twitter": "https://twitter.com/wavesprotocol",
      "reddit": "https://reddit.com/r/Wavesplatform",
      "telegram": "https://t.me/wavesnews",
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "WAVES",
    "rate": 55.6062888282353,
    "volume": 1375012482,
    "cap": 5560628883
  },
  {
    "name": "OKB",
    "rank": 32,
    "age": 1066,
    "color": "#3974ee",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/okb.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/okb.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/okb.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/okb.webp",
    "exchanges": 12,
    "markets": 24,
    "pairs": 12,
    "allTimeHighUSD": 44.237843690801675,
    "circulatingSupply": 260143734,
    "totalSupply": 300000000,
    "maxSupply": 300000000,
    "links": {
      "website": "https://www.okx.com/",
      "whitepaper": "https://okexchain-docs.readthedocs.io/en/latest/",
      "twitter": "https://twitter.com/okx",
      "reddit": "https://reddit.com/r/OKEx",
      "telegram": "https://t.me/OECofficial",
      "discord": "https://discord.com/invite/2rynEUqJxP",
      "medium": "https://medium.com/okex-blog",
      "instagram": "https://www.instagram.com/okx_official/"
    },
    "code": "OKB",
    "rate": 20.678910922433523,
    "volume": 97134351,
    "cap": 5379489102
  },
  {
    "name": "Wrapped Bitcoin",
    "rank": 33,
    "age": 1155,
    "color": "#f09444",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/wbtc.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/wbtc.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/wbtc.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/wbtc.webp",
    "exchanges": 71,
    "markets": 127,
    "pairs": 38,
    "allTimeHighUSD": 69231.28625326925,
    "circulatingSupply": 118282,
    "totalSupply": 118282,
    "maxSupply": null,
    "links": {
      "website": "https://www.wbtc.network/",
      "whitepaper": "https://wbtc.network/assets/wrapped-tokens-whitepaper.pdf",
      "twitter": "https://twitter.com/WrappedBTC",
      "reddit": null,
      "telegram": null,
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "WBTC",
    "rate": 45150.81465970353,
    "volume": 224775749,
    "cap": 5340528660
  },
  {
    "name": "VeChain",
    "rank": 34,
    "age": 1683,
    "color": "#6488e1",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/vet.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/vet.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/vet.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/vet.webp",
    "exchanges": 28,
    "markets": 92,
    "pairs": 40,
    "allTimeHighUSD": 0.26773718608293,
    "circulatingSupply": 66760741299,
    "totalSupply": 85985041177,
    "maxSupply": 86712634466,
    "links": {
      "website": "https://www.vechain.org/",
      "whitepaper": "https://cdn.vechain.com/vechain_ico_ideas_of_development_en.pdf",
      "twitter": "https://twitter.com/vechainofficial",
      "reddit": "https://reddit.com/r/vechain",
      "telegram": "https://t.me/vechain_official_english",
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "VET",
    "rate": 0.07925860265559878,
    "volume": 587180728,
    "cap": 5291363068
  },
  {
    "name": "TRON",
    "rank": 35,
    "age": 1661,
    "color": "#c43429",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/trx.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/trx.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/trx.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/trx.webp",
    "exchanges": 77,
    "markets": 235,
    "pairs": 66,
    "allTimeHighUSD": 0.2947430746031121,
    "circulatingSupply": 71659657369,
    "totalSupply": 99281283754,
    "maxSupply": null,
    "links": {
      "website": "https://tron.network/",
      "whitepaper": "https://o836fhe91.qnssl.com/tron/whitebook/TronWhitepaper_en.pdf",
      "twitter": "https://twitter.com/trondao",
      "reddit": "https://reddit.com/r/Tronix",
      "telegram": "https://t.me/tronnetworkEN",
      "discord": "https://discord.com/invite/ezNzd9W",
      "medium": "https://tronfoundation.medium.com/",
      "instagram": "https://instagram.com/tronfoundation"
    },
    "code": "TRX",
    "rate": 0.07280401694374414,
    "volume": 1022351776,
    "cap": 5217110909
  },
  {
    "name": "Internet Computer",
    "rank": 36,
    "age": 325,
    "color": "#ea594e",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/icp.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/icp.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/icp.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/icp.webp",
    "exchanges": 14,
    "markets": 29,
    "pairs": 9,
    "allTimeHighUSD": 494.5636424153892,
    "circulatingSupply": 216366875,
    "totalSupply": 471245633,
    "maxSupply": 471266671,
    "links": {
      "website": "https://dfinity.org/",
      "whitepaper": "https://sdk.dfinity.org/docs/interface-spec/index.html",
      "twitter": "https://twitter.com/dfinity",
      "reddit": "https://reddit.com/r/dfinity",
      "telegram": "https://t.me/dfinity/",
      "discord": null,
      "medium": "https://medium.com/dfinity-network-blog",
      "instagram": null
    },
    "code": "ICP",
    "rate": 20.70063561310898,
    "volume": 172286876,
    "cap": 4478931838
  },
  {
    "name": "Filecoin",
    "rank": 37,
    "age": 532,
    "color": "#0492fc",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/fil.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/fil.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/fil.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/fil.webp",
    "exchanges": 30,
    "markets": 52,
    "pairs": 12,
    "allTimeHighUSD": 237.64668979882163,
    "circulatingSupply": 185383933,
    "totalSupply": 1972610415,
    "maxSupply": 1972610415,
    "links": {
      "website": "https://filecoin.io/",
      "whitepaper": "https://docs.filecoin.io/",
      "twitter": "https://twitter.com/Filecoin",
      "reddit": "https://reddit.com/r/filecoin",
      "telegram": null,
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "FIL",
    "rate": 23.72672829080862,
    "volume": 390113139,
    "cap": 4398554208
  },
  {
    "name": "Elrond",
    "rank": 38,
    "age": 574,
    "color": "#060606",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/egld.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/egld.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/egld.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/egld.webp",
    "exchanges": 15,
    "markets": 31,
    "pairs": 15,
    "allTimeHighUSD": 548.0865877630855,
    "circulatingSupply": 22110985,
    "totalSupply": 23440985,
    "maxSupply": null,
    "links": {
      "website": "https://elrond.com/",
      "whitepaper": "https://elrond.com/assets/files/elrond-whitepaper.pdf",
      "twitter": "https://twitter.com/elrondnetwork",
      "reddit": "https://reddit.com/r/elrondnetwork",
      "telegram": "https://t.me/ElrondNetwork",
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "EGLD",
    "rate": 192.25166815666657,
    "volume": 154444755,
    "cap": 4250873751
  },
  {
    "name": "Theta Network",
    "rank": 39,
    "age": 1534,
    "color": "#25ced3",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/theta.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/theta.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/theta.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/theta.webp",
    "exchanges": 20,
    "markets": 37,
    "pairs": 10,
    "allTimeHighUSD": 15.710482367125962,
    "circulatingSupply": 1000000000,
    "totalSupply": 1000000000,
    "maxSupply": 1000000000,
    "links": {
      "website": "https://www.thetatoken.org/",
      "whitepaper": "https://s3.us-east-2.amazonaws.com/assets.thetatoken.org/Theta-white-paper-latest.pdf",
      "twitter": "https://twitter.com/theta_network",
      "reddit": "https://reddit.com/r/thetatoken",
      "telegram": "https://t.me/joinchat/Gt6mbxJb-2XukwGA_atLjg",
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "THETA",
    "rate": 4.02374783931007,
    "volume": 590033176,
    "cap": 4023747839
  },
  {
    "name": "Monero",
    "rank": 40,
    "age": 2905,
    "color": "#fc6404",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/xmr.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/xmr.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/xmr.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/xmr.webp",
    "exchanges": 37,
    "markets": 65,
    "pairs": 11,
    "allTimeHighUSD": 516.843201123428,
    "circulatingSupply": 18081370,
    "totalSupply": 18081370,
    "maxSupply": null,
    "links": {
      "website": "http://www.monero.cc",
      "whitepaper": "https://github.com/monero-project/research-lab/blob/master/whitepaper/whitepaper.pdf",
      "twitter": "https://twitter.com/monero",
      "reddit": "https://reddit.com/r/monero",
      "telegram": "https://t.me/bitmonero",
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "XMR",
    "rate": 218.02589513040988,
    "volume": 130905675,
    "cap": 3942206879
  },
  {
    "name": "The Sandbox",
    "rank": 41,
    "age": 593,
    "color": "#f3f9fb",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/sand.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/sand.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/sand.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/sand.webp",
    "exchanges": 32,
    "markets": 53,
    "pairs": 15,
    "allTimeHighUSD": 8.35315043328023,
    "circulatingSupply": 1157908753,
    "totalSupply": 3000000000,
    "maxSupply": null,
    "links": {
      "website": "https://www.sandbox.game/en/",
      "whitepaper": null,
      "twitter": "https://twitter.com/TheSandboxGame",
      "reddit": null,
      "telegram": "https://t.me/sandboxgame",
      "discord": null,
      "medium": null,
      "instagram": "https://instagram.com/thesandboxgame"
    },
    "code": "SAND",
    "rate": 3.370566007425051,
    "volume": 453712103,
    "cap": 3902807883
  },
  {
    "name": "Fantom",
    "rank": 42,
    "age": 1249,
    "color": "#e4e4e4",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ftm.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ftm.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ftm.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ftm.webp",
    "exchanges": 43,
    "markets": 373,
    "pairs": 205,
    "allTimeHighUSD": 3.463518790484801,
    "circulatingSupply": 2541152731,
    "totalSupply": 3175000000,
    "maxSupply": 3175000000,
    "links": {
      "website": "https://fantom.foundation/",
      "whitepaper": "https://www.fantom.foundation/data/FANTOM%20Whitepaper%20English%20v1.5.pdf",
      "twitter": "https://twitter.com/FantomFDN",
      "reddit": "https://reddit.com/r/FantomFoundation",
      "telegram": "https://t.me/Fantom_English",
      "discord": "https://discord.com/invite/zS4Ek4WByA/",
      "medium": "https://medium.com/fantomfoundation",
      "instagram": "https://www.instagram.com/official.fantom.foundation/"
    },
    "code": "FTM",
    "rate": 1.527140903788145,
    "volume": 1346160779,
    "cap": 3880698278
  },
  {
    "name": "ApeCoin",
    "rank": 43,
    "age": 14,
    "color": "#ecf0f6",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ape.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ape.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/ape.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/ape.webp",
    "exchanges": 19,
    "markets": 32,
    "pairs": 10,
    "allTimeHighUSD": 16.66916271276931,
    "circulatingSupply": 277500000,
    "totalSupply": 1000000000,
    "maxSupply": 1000000000,
    "links": {
      "website": "https://apecoin.com/",
      "whitepaper": null,
      "twitter": "https://twitter.com/apecoin",
      "reddit": "https://reddit.com/r/BoredApeYachtClub",
      "telegram": null,
      "discord": "https://discord.com/invite/wjH7hGz2yS",
      "medium": "https://boredapeyachtclub.medium.com/",
      "instagram": "https://www.instagram.com/boredapeyachtclub/"
    },
    "code": "APE",
    "rate": 12.683913622849452,
    "volume": 617211064,
    "cap": 3519786030
  },
  {
    "name": "Hedera Hashgraph",
    "rank": 44,
    "age": 914,
    "color": "#f8f8f8",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/hbar.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/hbar.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/hbar.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/hbar.webp",
    "exchanges": 17,
    "markets": 32,
    "pairs": 10,
    "allTimeHighUSD": 0.569398985191319,
    "circulatingSupply": 14248172433,
    "totalSupply": 14248172433,
    "maxSupply": 50000000000,
    "links": {
      "website": "https://hedera.com/",
      "whitepaper": "https://hedera.com/papers",
      "twitter": "https://twitter.com/hashgraph",
      "reddit": "https://reddit.com/r/hashgraph",
      "telegram": "https://t.me/hederahashgraph",
      "discord": null,
      "medium": null,
      "instagram": null
    },
    "code": "HBAR",
    "rate": 0.23860393468685276,
    "volume": 99080532,
    "cap": 3399670005
  },
  {
    "name": "Tezos",
    "rank": 45,
    "age": 1462,
    "color": "#2c84fc",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/xtz.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/xtz.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/xtz.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/xtz.webp",
    "exchanges": 44,
    "markets": 90,
    "pairs": 18,
    "allTimeHighUSD": 9.085601324153052,
    "circulatingSupply": 889597755,
    "totalSupply": 911285254,
    "maxSupply": null,
    "links": {
      "website": "https://www.tezos.com/",
      "whitepaper": "https://tezos.gitlab.io/",
      "twitter": "https://twitter.com/tezos",
      "reddit": "https://reddit.com/r/tezos",
      "telegram": "https://t.me/tezosplatform",
      "discord": "https://discord.gg/e6dCF7B",
      "medium": "https://medium.com/tezos",
      "instagram": null
    },
    "code": "XTZ",
    "rate": 3.676033607760901,
    "volume": 125048949,
    "cap": 3270191245
  },
  {
    "name": "Klaytn",
    "rank": 46,
    "age": 365,
    "color": "#1c1c1c",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/_klay.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/_klay.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/_klay.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/_klay.webp",
    "exchanges": 25,
    "markets": 37,
    "pairs": 7,
    "allTimeHighUSD": 4.4518074054046,
    "circulatingSupply": 2779183892,
    "totalSupply": 2779183892,
    "maxSupply": null,
    "links": {
      "website": "https://www.klaytn.com/",
      "whitepaper": "https://www.klaytn.com/Klaytn_PositionPaper_V2.1.0.pdf",
      "twitter": "https://twitter.com/klaytn_official",
      "reddit": "https://reddit.com/r/klaytn",
      "telegram": "https://t.me/Klaytn_EN",
      "discord": null,
      "medium": "https://medium.com/klaytn",
      "instagram": null
    },
    "code": "_KLAY",
    "rate": 1.161114088665135,
    "volume": 188137337,
    "cap": 3226949572
  },
  {
    "name": "Aave",
    "rank": 47,
    "age": 534,
    "color": "#688cb5",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/aave.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/aave.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/aave.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/aave.webp",
    "exchanges": 56,
    "markets": 100,
    "pairs": 21,
    "allTimeHighUSD": 663.2190116456594,
    "circulatingSupply": 13658873,
    "totalSupply": 16000000,
    "maxSupply": 16000000,
    "links": {
      "website": "https://aave.com/",
      "whitepaper": "https://github.com/aave/protocol-v2/blob/master/aave-v2-whitepaper.pdf",
      "twitter": "https://twitter.com/AaveAave",
      "reddit": "https://reddit.com/r/Aave_Official",
      "telegram": "https://t.me/Aavesome",
      "discord": "https://discord.com/invite/adPfquZDZc",
      "medium": "https://medium.com/aave",
      "instagram": "https://instagram.com/aave.aave"
    },
    "code": "AAVE",
    "rate": 222.1441199048238,
    "volume": 414912456,
    "cap": 3034238321
  },
  {
    "name": "EOS",
    "rank": 48,
    "age": 1734,
    "color": "#ebebeb",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/eos.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/eos.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/eos.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/eos.webp",
    "exchanges": 65,
    "markets": 158,
    "pairs": 33,
    "allTimeHighUSD": 23.95565521183252,
    "circulatingSupply": 989449670,
    "totalSupply": 989449670,
    "maxSupply": null,
    "links": {
      "website": "https://eos.io/",
      "whitepaper": "https://github.com/EOSIO/Documentation/blob/master/TechnicalWhitePaper.md",
      "twitter": "https://twitter.com/block_one_",
      "reddit": "https://reddit.com/r/EOS",
      "telegram": "https://t.me/EOSProject",
      "discord": null,
      "medium": "https://medium.com/eosio",
      "instagram": null
    },
    "code": "EOS",
    "rate": 2.764321307908511,
    "volume": 739455081,
    "cap": 2735156806
  },
  {
    "name": "Helium",
    "rank": 49,
    "age": 595,
    "color": "#eaf5fa",
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/hnt.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/hnt.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/hnt.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/hnt.webp",
    "exchanges": 14,
    "markets": 22,
    "pairs": 7,
    "allTimeHighUSD": 55.09568340157064,
    "circulatingSupply": 115642638,
    "totalSupply": 223000000,
    "maxSupply": 223000000,
    "links": {
      "website": "https://www.helium.com/",
      "whitepaper": "http://whitepaper.helium.com/",
      "twitter": "https://twitter.com/helium",
      "reddit": "https://reddit.com/r/HeliumNetwork",
      "telegram": "https://t.me/helium_network",
      "discord": null,
      "medium": null,
      "instagram": "https://instagram.com/helium"
    },
    "code": "HNT",
    "rate": 22.994844131037187,
    "volume": 46608653,
    "cap": 2659184436
  },
  {
    "name": "Osmosis",
    "rank": 50,
    "age": 241,
    "color": null,
    "png32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/osmo.png",
    "png64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/osmo.png",
    "webp32": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/32/osmo.webp",
    "webp64": "https://lcw.nyc3.cdn.digitaloceanspaces.com/production/currencies/64/osmo.webp",
    "exchanges": 1,
    "markets": 19,
    "pairs": 17,
    "allTimeHighUSD": 11.179822190372986,
    "circulatingSupply": 334245191,
    "totalSupply": 325000000,
    "maxSupply": null,
    "links": {
      "website": "https://osmosis.zone/",
      "whitepaper": "https://docs.osmosis.zone/",
      "twitter": "https://twitter.com/osmosiszone",
      "reddit": "https://reddit.com/r/OsmosisLab",
      "telegram": "https://t.me/osmosis_chat",
      "discord": "https://discord.com/invite/osmosis",
      "medium": "https://medium.com/osmosis",
      "instagram": null
    },
    "code": "OSMO",
    "rate": 7.943703563592597,
    "volume": 12296917,
    "cap": 2655144715
  }
]
from binance.futures import Futures as Client

client = Client(show_limit_usage=True)
print(client.time())
```
returns:

```python
{'data': {'serverTime': 1587990847650}, 'limit_usage': {'x-mbx-used-weight': '31', 'x-mbx-used-weight-1m': '31'}}
```
You can also display full response metadata to help in debugging:

```python
client = Client(show_header=True)
print(client.time())
```

returns:

```python
{'data': {'serverTime': 1587990847650}, 'header': {'Context-Type': 'application/json;charset=utf-8', ...}}
```

If `ClientError` is received, it'll display full response meta information.

### Display logs

Setting the log level to `DEBUG` will log the request URL, payload and response text.

### Error

There are 2 types of error returned from the library:
- `binance.error.ClientError`
    - This is thrown when server returns `4XX`, it's an issue from client side.
    - It has 4 properties:
        - `status_code` - HTTP status code
        - `error_code` - Server's error code, e.g. `-1102`
        - `error_message` - Server's error message, e.g. `Unknown order sent.`
        - `header` - Full response header. 
- `binance.error.ServerError`
    - This is thrown when server returns `5XX`, it's an issue from server side.

## Websocket

```python
from binance.websocket.futures.websocket_client import FuturesWebsocketClient as WebsocketClient

def message_handler(message):
    print(message)

ws_client = WebsocketClient()
ws_client.start()

ws_client.mini_ticker(
    symbol='bnbusdt',
    id=1,
    callback=message_handler,
)

# Combine selected streams
ws_client.instant_subscribe(
    stream=['bnbusdt@bookTicker', 'ethusdt@bookTicker'],
    callback=message_handler,
)

ws_client.stop()
```
More websocket examples are available in the `examples` folder

### Heartbeat

Once connected, the websocket server sends a ping frame every 3 minutes and requires a response pong frame back within
a 10 minutes period. This package handles the pong responses automatically.

