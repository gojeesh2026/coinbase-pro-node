# coinbase-pro-node

Programmatic trading needs access to market data and authenticated exchange requests without hand-written request signing. coinbase-pro-node is a JavaScript Coinbase Pro API client based on the original Node.js library, with public REST calls and a WebSocket-synced order book.

**Legacy API notice:** The donor project is deprecated. Its Coinbase Pro endpoints are historical defaults; verify endpoint availability and API compatibility before using this code. This standalone project is not an official Coinbase library.

## Features

- Fetch products, tickers, trades, historical rates, and order book snapshots through public client methods.
- Authenticate private API requests with an API key, base64 secret, and passphrase; let the client handle HMAC signing and JSON formatting.
- Maintain a customizable order book synchronized through WebSocket updates.
- Fetch a trade range with `getProductTradeStream`, which handles pagination and rate limits.
- Call API methods with promises or callbacks, and pass endpoint-specific options where supported.

## Installation

From a checkout of this repository, install the JavaScript dependencies:

```bash
cd coinbase-pro-node
npm install
```

The usage example below loads the local package, so it does not depend on a separately published npm package. The donor's npm package was named `coinbase-pro`; that is distinct from this standalone repository.

For a tagged source archive, set `GITHUB_OWNER` to this repository's owner. The following URL requires the `v0.1.0` release tag to exist:

```bash
curl -fL "https://github.com/${GITHUB_OWNER}/coinbase-pro-node/archive/refs/tags/v0.1.0.tar.gz" -o coinbase-pro-node-v0.1.0.tar.gz
```

## Usage

Save this example as `ticker.js` in the repository root. It accepts a product ID and an API endpoint, then prints the ticker response:

```js
const CoinbasePro = require('./');

const productID = process.argv[2] || 'BTC-USD';
const endpoint = process.argv[3] || 'https://api.pro.coinbase.com';
const publicClient = new CoinbasePro.PublicClient(endpoint);

async function main() {
  try {
    const ticker = await publicClient.getProductTicker(productID);
    console.log(ticker);
  } catch (error) {
    console.error(error);
    process.exitCode = 1;
  }
}

main();
```

Run it with a product ID and an endpoint compatible with the legacy API:

```bash
node ticker.js BTC-USD https://api.pro.coinbase.com
```

This is a script that uses the library, not a bundled command-line interface. The historical endpoint above may no longer accept requests.

To request an order book snapshot instead, use:

```js
const book = await publicClient.getProductOrderBook('ETH-USD', { level: 3 });
```

Callbacks receive `(error, response, data)` and must be passed last, after any options. **A call with a callback does not return a promise.**

Private endpoints use `AuthenticatedClient`:

```js
const authedClient = new CoinbasePro.AuthenticatedClient(
  process.env.COINBASE_API_KEY,
  process.env.COINBASE_API_SECRET,
  process.env.COINBASE_API_PASSPHRASE,
  process.env.COINBASE_API_URL || 'https://api.pro.coinbase.com'
);
```

Keep credentials out of source control. Confirm that your endpoint supports the client's authentication scheme before sending private requests.

## License

Licensed under Apache-2.0. This project is based on [coinbase/coinbase-pro-node](https://github.com/coinbase/coinbase-pro-node), also licensed under Apache-2.0. Preserve applicable upstream copyright, license, and attribution notices when redistributing derived code, and mark modified files as required by the license.

## Related projects

The supplied related-project list identifies these as other repositories from the same author, but provides no verified functionality descriptions:

- [khuram-bti/taskomask](https://github.com/khuram-bti/taskomask): Consult its README for its purpose and usage.
- [RNuneConnexcs/netscanner](https://github.com/RNuneConnexcs/netscanner): Consult its README for its purpose and usage.
- [puneethvital2000-png/node-chatgpt-api-xmro](https://github.com/puneethvital2000-png/node-chatgpt-api-xmro): Consult its README for its purpose and usage.

