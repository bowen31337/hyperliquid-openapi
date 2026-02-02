# Hyperliquid API - Quick Start Guide

Get started with the Hyperliquid API in 5 minutes!

## 🚀 View Documentation

### Option 1: Open Locally (Recommended)

```bash
# Navigate to project directory
cd /Users/bowenli/development/hyperliquid_openapi

# Open index.html in your browser
open index.html
```

### Option 2: Start Local Server

```bash
# Install dependencies (first time only)
npm install

# Start server
npm start
```

Then open `http://localhost:8080` in your browser.

## 📖 Quick API Examples

### REST API

#### 1. Get All Mid Prices

```bash
curl -X POST https://api.hyperliquid.xyz/info \
  -H "Content-Type: application/json" \
  -d '{"type": "allMids"}'
```

**Response**:
```json
{
  "BTC": "50000.0",
  "ETH": "3000.0",
  "SOL": "100.0"
}
```

#### 2. Get L2 Order Book

```bash
curl -X POST https://api.hyperliquid.xyz/info \
  -H "Content-Type: application/json" \
  -d '{
    "type": "l2Book",
    "coin": "BTC",
    "nSigFigs": 5
  }'
```

#### 3. Get Candle Data

```bash
curl -X POST https://api.hyperliquid.xyz/info \
  -H "Content-Type: application/json" \
  -d '{
    "type": "candleSnapshot",
    "req": {
      "coin": "BTC",
      "interval": "15m",
      "startTime": 1700000000000,
      "endTime": 1700086400000
    }
  }'
```

#### 4. Get User's Open Orders

```bash
curl -X POST https://api.hyperliquid.xyz/info \
  -H "Content-Type: application/json" \
  -d '{
    "type": "openOrders",
    "user": "0xYOUR_ADDRESS_HERE"
  }'
```

### WebSocket API

#### Connect and Subscribe

```javascript
// Connect to WebSocket
const ws = new WebSocket('wss://api.hyperliquid.xyz/ws');

// Handle connection
ws.onopen = () => {
  console.log('Connected to Hyperliquid WebSocket');
  
  // Subscribe to all mid prices
  ws.send(JSON.stringify({
    method: 'subscribe',
    subscription: {
      type: 'allMids'
    }
  }));
};

// Handle messages
ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log('Received:', data);
};

// Handle errors
ws.onerror = (error) => {
  console.error('WebSocket error:', error);
};

// Handle close
ws.onclose = () => {
  console.log('Disconnected from Hyperliquid WebSocket');
};
```

#### Subscribe to Order Book

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

#### Subscribe to Trades

```javascript
ws.send(JSON.stringify({
  method: 'subscribe',
  subscription: {
    type: 'trades',
    coin: 'BTC'
  }
}));
```

#### Subscribe to User Events

```javascript
ws.send(JSON.stringify({
  method: 'subscribe',
  subscription: {
    type: 'userEvents',
    user: '0xYOUR_ADDRESS_HERE'
  }
}));
```

## 🔑 Trading (Requires Signature)

Trading operations require EIP-712 signatures. Use the official SDKs:

### Python Example

```python
from hyperliquid.info import Info
from hyperliquid.exchange import Exchange

# Initialize
info = Info()
exchange = Exchange(private_key="YOUR_PRIVATE_KEY")

# Get mid prices
mids = info.all_mids()
print(f"BTC mid: {mids['BTC']}")

# Place order
order_result = exchange.order(
    coin="BTC",
    is_buy=True,
    sz=0.1,
    limit_px=49000.0,
    order_type={"limit": {"tif": "Gtc"}}
)
print(f"Order placed: {order_result}")
```

### TypeScript Example

```typescript
import { Hyperliquid } from 'hyperliquid';

// Initialize
const sdk = new Hyperliquid('YOUR_PRIVATE_KEY');

// Get mid prices
const mids = await sdk.info.getAllMids();
console.log(`BTC mid: ${mids.BTC}`);

// Place order
const order = await sdk.exchange.placeOrder({
  coin: 'BTC',
  is_buy: true,
  sz: 0.1,
  limit_px: 49000.0,
  order_type: { limit: { tif: 'Gtc' } }
});
console.log(`Order placed: ${order}`);
```

## 📚 Common Use Cases

### 1. Market Data Feed

```javascript
// Real-time mid prices
const ws = new WebSocket('wss://api.hyperliquid.xyz/ws');

ws.onopen = () => {
  ws.send(JSON.stringify({
    method: 'subscribe',
    subscription: { type: 'allMids' }
  }));
};

ws.onmessage = (event) => {
  const { data } = JSON.parse(event.data);
  console.log('Mid prices:', data.mids);
};
```

### 2. Order Book Monitoring

```javascript
// L2 order book updates
ws.send(JSON.stringify({
  method: 'subscribe',
  subscription: {
    type: 'l2Book',
    coin: 'BTC',
    nSigFigs: 5
  }
}));
```

### 3. Trade Monitoring

```javascript
// Recent trades
ws.send(JSON.stringify({
  method: 'subscribe',
  subscription: {
    type: 'trades',
    coin: 'BTC'
  }
}));
```

### 4. User Activity Tracking

```javascript
// User fills, funding, liquidations
ws.send(JSON.stringify({
  method: 'subscribe',
  subscription: {
    type: 'userEvents',
    user: '0xYOUR_ADDRESS'
  }
}));
```

## 🔧 Testing

### Test with cURL

```bash
# Test mainnet
curl -X POST https://api.hyperliquid.xyz/info \
  -H "Content-Type: application/json" \
  -d '{"type": "allMids"}'

# Test testnet
curl -X POST https://api.hyperliquid-testnet.xyz/info \
  -H "Content-Type: application/json" \
  -d '{"type": "allMids"}'
```

### Test with wscat

```bash
# Install wscat
npm install -g wscat

# Connect to WebSocket
wscat -c wss://api.hyperliquid.xyz/ws

# Send subscription (paste this after connecting)
{"method":"subscribe","subscription":{"type":"allMids"}}
```

## 📊 Response Formats

### Successful Response

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

## 🚨 Rate Limits

- Rate limits are **address-based**
- Increase limits through trading volume
- Reserve additional actions: 0.0005 USDC per request

## 📖 Next Steps

1. **Explore Documentation**: Open `index.html` for complete API reference
2. **Read Examples**: Check `README.md` for more examples
3. **Use SDKs**: Install official Python or TypeScript SDK
4. **Join Community**: Visit [Hyperliquid Discord](https://discord.gg/hyperliquid)

## 🔗 Useful Links

- **Documentation**: `index.html` (local) or [Official Docs](https://hyperliquid.gitbook.io/hyperliquid-docs)
- **OpenAPI Spec**: `openapi.yaml`
- **WebSocket Spec**: `websocket-api.yaml`
- **Python SDK**: https://github.com/hyperliquid-dex/hyperliquid-python-sdk
- **TypeScript SDK**: https://github.com/nktkas/hyperliquid

## 💡 Tips

1. **Use Testnet First**: Test with `https://api.hyperliquid-testnet.xyz`
2. **Handle Disconnects**: WebSocket may disconnect periodically
3. **Check Signatures**: Trading requires proper EIP-712 signatures
4. **Monitor Rate Limits**: Track your usage to avoid throttling
5. **Read Error Messages**: Error responses contain helpful details

## ❓ Need Help?

- **Documentation**: Open `index.html` for full API reference
- **Examples**: See `README.md` for more code samples
- **Official Docs**: https://hyperliquid.gitbook.io/hyperliquid-docs
- **Discord**: https://discord.gg/hyperliquid

---

**Happy Trading! 🚀**
