# Hyperliquid OpenAPI Documentation

Comprehensive OpenAPI 3.1.0 specification and AsyncAPI 3.0.0 documentation for the Hyperliquid DEX API.

## 📚 Documentation

This repository contains complete API documentation for Hyperliquid, including:

- **REST API**: Complete OpenAPI 3.1.0 specification with all endpoints
- **WebSocket API**: AsyncAPI 3.0.0 specification for real-time data streaming
- **Interactive Documentation**: Redoc-powered beautiful API documentation

## 🚀 Quick Start

### View Documentation Locally

1. **Install dependencies**:
   ```bash
   npm install
   ```

2. **Start local server**:
   ```bash
   npm start
   ```

3. **Open browser**:
   Navigate to `http://localhost:8080`

### Alternative: Python HTTP Server

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080` in your browser.

## 📁 Files

- `openapi.yaml` - Complete REST API specification (OpenAPI 3.1.0)
- `websocket-api.yaml` - WebSocket API specification (AsyncAPI 3.0.0)
- `index.html` - Redoc documentation viewer
- `README.md` - This file

## 🔗 API Endpoints

### Base URLs

- **Mainnet**: `https://api.hyperliquid.xyz`
- **Testnet**: `https://api.hyperliquid-testnet.xyz`

### WebSocket URLs

- **Mainnet**: `wss://api.hyperliquid.xyz/ws`
- **Testnet**: `wss://api.hyperliquid-testnet.xyz/ws`

## 📖 API Overview

### Info Endpoint (`POST /info`)

Query various types of information:

- **Market Data**: Mid prices, order books, candles
- **User Data**: Open orders, fills, positions
- **Account Data**: Balances, fees, referrals
- **Staking Data**: Delegations, rewards

### Exchange Endpoint (`POST /exchange`)

Execute trading and account operations:

- **Orders**: Place, cancel, modify orders
- **Leverage**: Update leverage and margin
- **Transfers**: Send USDC, spot tokens
- **Withdrawals**: Bridge to L1
- **Staking**: Delegate/undelegate
- **Vaults**: Deposit/withdraw

### WebSocket Subscriptions

Real-time data streams:

- `allMids` - All mid prices
- `l2Book` - Level 2 order book
- `trades` - Recent trades
- `candle` - Candlestick data
- `userEvents` - User fills, funding, liquidations
- `orderUpdates` - Order status updates
- `webData2` - Comprehensive user data

## 🔐 Authentication

Trading operations require EIP-712 signatures. See the [official documentation](https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/api/signing) for signing details.

## 📚 SDKs

Official and community SDKs:

- **Python**: [hyperliquid-python-sdk](https://github.com/hyperliquid-dex/hyperliquid-python-sdk)
- **TypeScript**: 
  - [nktkas/hyperliquid](https://github.com/nktkas/hyperliquid)
  - [nomeida/hyperliquid](https://github.com/nomeida/hyperliquid)
- **Rust**: [infinitefield/hypersdk](https://github.com/infinitefield/hypersdk)

## 🛠️ Validation

### Validate OpenAPI Spec

```bash
npm run validate
```

Or using Docker:

```bash
docker run --rm -v $(pwd):/spec redocly/cli lint openapi.yaml
```

### Validate AsyncAPI Spec

```bash
npm run validate:asyncapi
```

## 📝 Examples

### REST API Examples

#### Get All Mid Prices

```bash
curl -X POST https://api.hyperliquid.xyz/info \
  -H "Content-Type: application/json" \
  -d '{"type": "allMids"}'
```

#### Get User's Open Orders

```bash
curl -X POST https://api.hyperliquid.xyz/info \
  -H "Content-Type: application/json" \
  -d '{
    "type": "openOrders",
    "user": "0x0000000000000000000000000000000000000000"
  }'
```

#### Get L2 Order Book

```bash
curl -X POST https://api.hyperliquid.xyz/info \
  -H "Content-Type: application/json" \
  -d '{
    "type": "l2Book",
    "coin": "BTC",
    "nSigFigs": 5
  }'
```

### WebSocket Examples

#### Subscribe to All Mids

```javascript
const ws = new WebSocket('wss://api.hyperliquid.xyz/ws');

ws.onopen = () => {
  ws.send(JSON.stringify({
    method: 'subscribe',
    subscription: {
      type: 'allMids'
    }
  }));
};

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log('Mid prices:', data);
};
```

#### Subscribe to L2 Order Book

```javascript
ws.send(JSON.stringify({
  method: 'subscribe',
  subscription: {
    type: 'l2Book',
    coin: 'BTC',
    nSigFigs: 5
  }
}));
```

#### Subscribe to User Events

```javascript
ws.send(JSON.stringify({
  method: 'subscribe',
  subscription: {
    type: 'userEvents',
    user: '0x0000000000000000000000000000000000000000'
  }
}));
```

## 🔄 Rate Limits

Rate limits are address-based and can be increased through:

1. **Trading Volume**: Higher volume increases limits
2. **Reserve Actions**: Pay 0.0005 USDC per request to reserve additional actions

## 📊 Response Formats

All responses are in JSON format. Successful responses have `status: "ok"`, errors have `status: "err"`.

### Successful Order Response

```json
{
  "status": "ok",
  "response": {
    "type": "order",
    "data": {
      "statuses": [{
        "resting": {
          "oid": 123456
        }
      }]
    }
  }
}
```

### Error Response

```json
{
  "status": "ok",
  "response": {
    "type": "order",
    "data": {
      "statuses": [{
        "error": "Order must have minimum value of $10."
      }]
    }
  }
}
```

## 🐛 Error Handling

### Common Error Statuses

- `rejected` - Order rejected at placement
- `marginCanceled` - Insufficient margin
- `openInterestCapCanceled` - Open interest cap reached
- `reduceOnlyCanceled` - Reduce-only order doesn't reduce position
- `liquidatedCanceled` - Canceled due to liquidation

See [Error Responses](https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/api/error-responses) for complete list.

## 📖 Additional Resources

- [Official Documentation](https://hyperliquid.gitbook.io/hyperliquid-docs)
- [API Reference](https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/api)
- [WebSocket Documentation](https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/api/websocket)
- [Signing Documentation](https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/api/signing)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## 📄 License

MIT License - see LICENSE file for details.

## ⚠️ Disclaimer

This is unofficial documentation. Always refer to the [official Hyperliquid documentation](https://hyperliquid.gitbook.io/hyperliquid-docs) for the most up-to-date information.

## 🔗 Links

- [Hyperliquid Website](https://hyperliquid.xyz)
- [Hyperliquid App](https://app.hyperliquid.xyz)
- [Hyperliquid Documentation](https://hyperliquid.gitbook.io/hyperliquid-docs)
- [Hyperliquid GitHub](https://github.com/hyperliquid-dex)

---

**Built with ❤️ for the Hyperliquid community**
