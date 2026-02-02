# Hyperliquid MCP Debug & Configuration

## Issue Identified

The original `hyperliquid-mcp` package (v0.0.1-1) had a broken bin configuration - the executable file was missing the proper Node.js shebang (`#!/usr/bin/env node`), causing it to be executed as a shell script instead of a Node.js module.

## Solution

Switched to `@hyperliquid-ai/mcp-server` (v1.1.1) which:
- ✅ Has proper bin configuration
- ✅ Includes Claude Code/Cursor support via HTTP transport
- ✅ More mature and actively maintained
- ✅ Comprehensive trading and market data tools

## Current Configuration

**File:** `~/.cursor/mcp.json`

```json
{
  "mcpServers": {
    "hyperliquid-mcp": {
      "command": "npx",
      "args": [
        "-y",
        "@hyperliquid-ai/mcp-server",
        "claude-code",
        "--port",
        "3001"
      ],
      "env": {
        "PRIVATE_KEY": ""
      }
    }
  }
}
```

## Setup Steps

### 1. Set Your Private Key

You have two options:

**Option A: Environment Variable (Recommended for security)**
```bash
export PRIVATE_KEY="your_wallet_private_key_here"
```

Add to your `~/.zshrc` or `~/.bashrc`:
```bash
echo 'export PRIVATE_KEY="your_private_key"' >> ~/.zshrc
source ~/.zshrc
```

**Option B: Direct in mcp.json (Dev only - NOT for production)**
```json
"env": {
  "PRIVATE_KEY": "0x1234567890abcdef..."
}
```

### 2. Restart Cursor

After setting the private key, restart Cursor completely (Cmd+Q and reopen) to load the new MCP server.

### 3. Verify Installation

Check MCP status in Cursor:
- Open Cursor Settings
- Navigate to MCP section
- Verify `hyperliquid-mcp` shows as "Running"

## Available Tools

The `@hyperliquid-ai/mcp-server` provides comprehensive Hyperliquid API access:

### Account Management
- `get_spot_clearinghouse_state` - Get spot account state
- `get_perp_clearinghouse_state` - Get perpetual account state
- `get_user_state` - Get complete user state

### Order Management
- `place_order` - Place limit/market orders
- `cancel_order` - Cancel specific order
- `cancel_all_orders` - Cancel all open orders
- `modify_order` - Modify existing order
- `get_open_orders` - List all open orders
- `get_order_status` - Check specific order status
- `get_order_history` - Get historical orders

### Market Data
- `get_all_mids` - Get mid prices for all assets
- `get_candle_snapshot` - Historical candlestick data
- `get_l2_book` - L2 order book data
- `get_spot_meta` - Spot market metadata
- `get_perp_meta` - Perpetual market metadata

### Transfers
- `transfer_spot_perp` - Transfer between spot and perp accounts
- `withdraw` - Withdraw funds

### Technical Analysis
- `calculate_indicators` - Technical indicators (RSI, MACD, etc.)
- `analyze_market` - Market analysis

## Requirements

- **Node.js:** v22.0.0+ (you have v24.12.0 ✅)
- **Package Manager:** npm/npx (installed ✅)
- **Private Key:** Hyperliquid wallet private key

## Testing

Test the MCP server manually:
```bash
npx -y @hyperliquid-ai/mcp-server --help
```

Test Claude Code mode:
```bash
PRIVATE_KEY="your_key" npx -y @hyperliquid-ai/mcp-server claude-code --port 3001
```

## Troubleshooting

### MCP Server Not Starting
1. Check Cursor logs: `~/Library/Logs/Cursor/`
2. Verify private key is set: `echo $PRIVATE_KEY`
3. Test package manually: `npx -y @hyperliquid-ai/mcp-server --help`

### Invalid Private Key Error
- Ensure private key is in correct format (0x prefixed hex)
- Verify it's a valid Hyperliquid wallet key

### Port Already in Use
- Change port in mcp.json: `"--port", "3002"`
- Check what's using port 3001: `lsof -i :3001`

## Alternative Packages

If you need different features, other Hyperliquid MCP servers:

1. **@mektigboy/server-hyperliquid** - Market data only (no trading)
2. **@mseep/hyperliquid-mcp-server** - Comprehensive SDK wrapper
3. **hyperliquid-mcp** (Impa-Ventures) - Original but has bin issues

## Next Steps

Once the MCP is working, you can:
1. Query Hyperliquid API directly through Cursor
2. Generate comprehensive OpenAPI documentation
3. Build automated trading tools
4. Create market analysis dashboards

## Security Notes

⚠️ **NEVER commit your private key to git**
⚠️ **Use environment variables for production**
⚠️ **Test with small amounts first**
⚠️ **Verify all transactions before confirming**
