# Interactive API Testing Guide

## 🎯 Overview

Your Hyperliquid API documentation now includes **three powerful ways** to test and explore the API:

1. **📖 Redoc** - Beautiful documentation for reading
2. **🔧 Swagger UI** - Interactive API explorer with "Try it out"
3. **🧪 API Tester** - Simple testing tool with pre-built examples

## 🚀 Quick Start

### Option 1: API Tester (Easiest)
**URL**: https://bowen31337.github.io/hyperliquid-openapi/api-tester.html

**Best for**: Quick testing of public endpoints without any setup

**Features**:
- ✅ Pre-built examples (one-click testing)
- ✅ Network selection (Mainnet/Testnet)
- ✅ Real-time responses
- ✅ No authentication needed for public endpoints
- ✅ Copy-paste friendly

**How to use**:
1. Select an example from dropdown
2. Click "Send Request"
3. See instant results!

### Option 2: Swagger UI (Most Interactive)
**URL**: https://bowen31337.github.io/hyperliquid-openapi/swagger.html

**Best for**: Exploring all endpoints with full interactivity

**Features**:
- ✅ "Try it out" on every endpoint
- ✅ Auto-generated request examples
- ✅ Schema validation
- ✅ Response visualization
- ✅ Request/response logging

**How to use**:
1. Find an endpoint
2. Click "Try it out"
3. Modify parameters if needed
4. Click "Execute"
5. View response below

### Option 3: Redoc (Best for Reading)
**URL**: https://bowen31337.github.io/hyperliquid-openapi/

**Best for**: Understanding API structure and schemas

**Features**:
- ✅ Beautiful, organized documentation
- ✅ Advanced search
- ✅ Detailed schema visualization
- ✅ Code examples
- ✅ Mobile-responsive

## 📊 What You Can Test

### ✅ Public Endpoints (No Auth Required)

These endpoints work immediately - just click and test!

#### Market Data
- **Get All Mid Prices**: See current prices for all assets
  ```json
  {"type": "allMids"}
  ```

- **Get L2 Order Book**: View order book for any asset
  ```json
  {
    "type": "l2Book",
    "coin": "BTC",
    "nSigFigs": 5
  }
  ```

- **Get Candle Data**: Historical price data
  ```json
  {
    "type": "candleSnapshot",
    "req": {
      "coin": "BTC",
      "interval": "15m",
      "startTime": 1700000000000,
      "endTime": 1700086400000
    }
  }
  ```

- **Get Market Metadata**: Asset information
  ```json
  {"type": "meta"}
  ```

- **Get Spot Metadata**: Spot market info
  ```json
  {"type": "spotMeta"}
  ```

#### User Data (Public)
- **Get User's Open Orders**: View open orders for any address
  ```json
  {
    "type": "openOrders",
    "user": "0x..."
  }
  ```

- **Get User's Fills**: View trade history
  ```json
  {
    "type": "userFills",
    "user": "0x..."
  }
  ```

- **Get User State**: View account state
  ```json
  {
    "type": "clearinghouseState",
    "user": "0x..."
  }
  ```

### 🔐 Trading Endpoints (Requires Signature)

Trading operations require EIP-712 signatures and **cannot be tested directly in the browser** without your private key.

**⚠️ Important**: Never enter your private key in a web browser!

#### Recommended Approach: Use Official SDKs

**Python SDK**:
```python
from hyperliquid.exchange import Exchange

# Initialize with your private key
exchange = Exchange(private_key="YOUR_PRIVATE_KEY")

# Place a limit order
result = exchange.order(
    coin="BTC",
    is_buy=True,
    sz=0.01,
    limit_px=50000.0,
    order_type={"limit": {"tif": "Gtc"}}
)

print(result)
```

**TypeScript SDK**:
```typescript
import { Hyperliquid } from 'hyperliquid';

// Initialize with your private key
const sdk = new Hyperliquid('YOUR_PRIVATE_KEY');

// Place a limit order
const result = await sdk.exchange.placeOrder({
  coin: 'BTC',
  is_buy: true,
  sz: 0.01,
  limit_px: 50000.0,
  order_type: { limit: { tif: 'Gtc' } }
});

console.log(result);
```

## 🎓 Step-by-Step Testing Guide

### Testing Public Endpoints

#### Using API Tester

1. **Navigate to API Tester**
   - Go to: https://bowen31337.github.io/hyperliquid-openapi/api-tester.html

2. **Select Network**
   - Choose "Mainnet" for live data
   - Choose "Testnet" for testing

3. **Choose an Example**
   - Select from dropdown: "Get All Mid Prices"
   - Request body auto-fills

4. **Send Request**
   - Click "🚀 Send Request"
   - Response appears instantly below

5. **Explore Results**
   - View formatted JSON response
   - Check response time
   - Copy data as needed

#### Using Swagger UI

1. **Navigate to Swagger UI**
   - Go to: https://bowen31337.github.io/hyperliquid-openapi/swagger.html

2. **Select Server**
   - Click "Servers" dropdown at top
   - Choose Mainnet or Testnet

3. **Find Endpoint**
   - Use search or browse by tags
   - Click on "POST /info"

4. **Try It Out**
   - Click "Try it out" button
   - Request body editor appears

5. **Modify Request**
   - Edit JSON in request body
   - Example: Change "BTC" to "ETH"

6. **Execute**
   - Click "Execute" button
   - Scroll down to see response

7. **View Results**
   - See status code
   - View response body
   - Check response headers

## 💡 Pro Tips

### API Tester Tips

1. **Quick Testing**: Use pre-built examples for instant results
2. **Custom Requests**: Modify JSON directly for custom queries
3. **Network Switching**: Test same request on both networks
4. **Error Handling**: Invalid JSON shows helpful error messages
5. **Keyboard Shortcut**: Ctrl+Enter to send request

### Swagger UI Tips

1. **Persistent Auth**: Authorization persists across page reloads
2. **Request Snippets**: Copy curl commands for CLI testing
3. **Schema Validation**: Invalid requests show validation errors
4. **Response Timing**: See exact request duration
5. **Filter Endpoints**: Use search to find specific endpoints quickly

### General Tips

1. **Start Simple**: Test "allMids" first to verify connectivity
2. **Check Network**: Ensure you're on correct network (mainnet/testnet)
3. **Read Responses**: Response structure matches OpenAPI spec
4. **Use Examples**: Pre-built examples are production-ready
5. **Console Logs**: Open browser DevTools to see detailed logs

## 🔍 Troubleshooting

### Common Issues

#### "Network Error" or "Failed to Fetch"

**Cause**: CORS or network connectivity issue

**Solution**:
- Check internet connection
- Try different network (mainnet/testnet)
- Verify API endpoint is accessible
- Check browser console for details

#### "Invalid JSON"

**Cause**: Malformed JSON in request body

**Solution**:
- Use pre-built examples as templates
- Validate JSON syntax
- Check for missing quotes or commas
- Use JSON formatter/validator

#### "Empty Response"

**Cause**: Valid request but no data available

**Solution**:
- Check if user address has activity
- Try different asset (e.g., BTC instead of obscure token)
- Verify time range for historical data
- Confirm asset exists on selected network

#### Trading Endpoints Don't Work

**Cause**: Missing or invalid signature

**Solution**:
- Trading endpoints require EIP-712 signatures
- Use official SDKs (Python/TypeScript)
- Never enter private keys in browser
- See SDK examples in documentation

## 📚 Example Use Cases

### Use Case 1: Monitor Prices

**Goal**: Get real-time prices for all assets

**Steps**:
1. Open API Tester
2. Select "Get All Mid Prices"
3. Click "Send Request"
4. Bookmark for quick access

**Result**: Instant price data for all assets

### Use Case 2: Analyze Order Book

**Goal**: View order book depth for BTC

**Steps**:
1. Open API Tester
2. Select "Get L2 Order Book"
3. Modify coin if needed
4. Click "Send Request"
5. Analyze bid/ask levels

**Result**: Detailed order book with price levels

### Use Case 3: Historical Analysis

**Goal**: Get 24h candle data for ETH

**Steps**:
1. Open API Tester
2. Select "Get Candle Data"
3. Change coin to "ETH"
4. Adjust time range
5. Click "Send Request"

**Result**: OHLCV data for analysis

### Use Case 4: Track User Activity

**Goal**: Monitor fills for a specific address

**Steps**:
1. Open Swagger UI
2. Find "userFills" endpoint
3. Click "Try it out"
4. Enter user address
5. Click "Execute"

**Result**: Complete fill history

## 🔗 Resources

### Documentation
- **Landing Page**: https://bowen31337.github.io/hyperliquid-openapi/landing.html
- **Redoc**: https://bowen31337.github.io/hyperliquid-openapi/
- **Swagger UI**: https://bowen31337.github.io/hyperliquid-openapi/swagger.html
- **API Tester**: https://bowen31337.github.io/hyperliquid-openapi/api-tester.html

### SDKs
- **Python SDK**: https://github.com/hyperliquid-dex/hyperliquid-python-sdk
- **TypeScript SDK**: https://github.com/nktkas/hyperliquid
- **Rust SDK**: https://github.com/infinitefield/hypersdk

### Official Resources
- **Hyperliquid Docs**: https://hyperliquid.gitbook.io/hyperliquid-docs
- **GitHub**: https://github.com/bowen31337/hyperliquid-openapi

## 🎉 Summary

You now have **three powerful tools** for testing Hyperliquid API:

1. **🧪 API Tester**: Quick testing with examples
2. **🔧 Swagger UI**: Full interactive exploration
3. **📖 Redoc**: Beautiful documentation

**Public endpoints** work immediately - no setup required!

**Trading endpoints** require official SDKs for security.

**Start testing now**: https://bowen31337.github.io/hyperliquid-openapi/api-tester.html

---

**Last Updated**: February 2, 2026
**Status**: ✅ Fully Functional
**Support**: https://github.com/bowen31337/hyperliquid-openapi/issues
