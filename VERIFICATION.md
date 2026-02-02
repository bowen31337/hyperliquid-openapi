# Hyperliquid OpenAPI Verification Report

## MCP Server Status

**Hyperliquid MCP** (`user-hyperliquid-mcp` / `@hyperliquid-ai/mcp-server`) is configured in Cursor but **reported an error** at verification time. The project's MCP folder (`mcps/user-hyperliquid-mcp/`) contains no tool descriptors and `STATUS.md` states: "The MCP server errored."

**Implication:** Verification could not be performed by calling the MCP server directly. This report is based on comparison against the **official Hyperliquid GitBook** (Info endpoint, Exchange endpoint) and the repo's own `openapi.yaml`.

---

## Verification Method

- **Source:** [Hyperliquid API – Info endpoint](https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/api/info-endpoint), [Exchange endpoint](https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/api/exchange-endpoint).
- **Spec:** `openapi.yaml` (paths, request/response schemas, oneOf lists).

---

## Info Endpoint (`POST /info`)

### Official request types (GitBook)

| Type | In our spec (schema) | In path oneOf |
|------|----------------------|----------------|
| allMids | Yes | Yes |
| openOrders | Yes | Yes |
| frontendOpenOrders | Yes | **No** (missing from oneOf) |
| userFills | Yes | Yes |
| userFillsByTime | Yes | Yes |
| userRateLimit | Yes | **No** (missing from oneOf) |
| orderStatus | Yes | Yes |
| l2Book | Yes | Yes |
| candleSnapshot | Yes | Yes |
| maxBuilderFee | **No** | No |
| historicalOrders | Yes | **No** (missing from oneOf) |
| userTwapSliceFills | **No** | No |
| subAccounts | Yes | **No** (missing from oneOf) |
| vaultDetails | Yes | **No** (missing from oneOf) |
| userVaultEquities | **No** | No |
| userRole | Yes | **No** (missing from oneOf) |
| portfolio | Yes | **No** (missing from oneOf) |
| referral | Yes | **No** (missing from oneOf) |
| userFees | Yes | **No** (missing from oneOf) |
| delegations | Yes | **No** (missing from oneOf) |
| delegatorSummary | **No** | No |
| delegatorHistory | **No** | No |
| delegatorRewards | **No** | No |
| userDexAbstraction | **No** | No |
| alignedQuoteTokenInfo | **No** | No |
| borrowLendUserState | **No** | No |
| borrowLendReserveState | **No** | No |
| allBorrowLendReserveStates | **No** | No |
| meta / spotMeta | Yes (MetaRequest) | Yes |
| clearinghouseState / spotClearinghouseState | Yes (UserStateRequest) | Yes |

### Action taken

- **Add to `/info` oneOf:** All Info request schemas that exist in `openapi.yaml` but were not referenced in the path: `FrontendOpenOrdersRequest`, `UserRateLimitRequest`, `HistoricalOrdersRequest`, `SubAccountsRequest`, `VaultDetailsRequest`, `UserRoleRequest`, `PortfolioRequest`, `ReferralRequest`, `UserFeesRequest`, `DelegationsRequest`.

### Still missing (schemas not in spec)

- maxBuilderFee, userTwapSliceFills, userVaultEquities, delegatorSummary, delegatorHistory, delegatorRewards, userDexAbstraction, alignedQuoteTokenInfo, borrowLendUserState, borrowLendReserveState, allBorrowLendReserveStates. These can be added in a follow-up if desired.

---

## Exchange Endpoint (`POST /exchange`)

### Official action types (GitBook)

| Action | In our spec (schema) | In path oneOf |
|--------|----------------------|----------------|
| order | Yes (PlaceOrderRequest) | Yes |
| cancel | Yes (CancelOrderRequest) | Yes |
| cancelByCloid | Yes (CancelByCloidRequest) | **No** (missing from oneOf) |
| scheduleCancel | **No** | No |
| modify | Yes (ModifyOrderRequest) | Yes |
| batchModify | **No** | No |
| updateLeverage | Yes (UpdateLeverageRequest) | Yes |
| updateIsolatedMargin | **No** | No |
| usdSend | Yes (UsdTransferRequest) | Yes |
| spotSend | **No** | No |
| withdraw3 | **No** | No |
| usdClassTransfer | **No** | No |
| sendAsset | **No** | No |
| cDeposit | **No** | No |
| cWithdraw | **No** | No |
| tokenDelegate | **No** | No |
| vaultTransfer | **No** | No |
| approveAgent | **No** | No |
| approveBuilderFee | **No** | No |
| twapOrder | Yes (TwapOrderRequest) | **No** (missing from oneOf) |
| twapCancel | **No** | No |
| reserveRequestWeight | **No** | No |
| noop | **No** | No |
| userDexAbstraction | **No** | No |
| agentEnableDexAbstraction | **No** | No |
| validatorL1Stream | **No** | No |

### Action taken

- **Add to `/exchange` oneOf:** `CancelByCloidRequest`, `TwapOrderRequest` (schemas already exist).

### Still missing (schemas not in spec)

- scheduleCancel, batchModify, updateIsolatedMargin, spotSend, withdraw3, usdClassTransfer, sendAsset, cDeposit, cWithdraw, tokenDelegate, vaultTransfer, approveAgent, approveBuilderFee, twapCancel, reserveRequestWeight, noop, userDexAbstraction, agentEnableDexAbstraction, validatorL1Stream. Can be added in a follow-up.

---

## WebSocket API

- `websocket-api.yaml` (AsyncAPI) was not re-verified in this pass. Official docs: [Websocket](https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/api/websocket).

---

## Summary

1. **MCP:** Hyperliquid MCP server could not be used (errored); verification was done against official GitBook and local `openapi.yaml`.
2. **Info:** Several Info request types had schemas in the spec but were not in the `/info` request body oneOf; they have been added so Swagger/Redoc show them.
3. **Exchange:** CancelByCloid and TwapOrder had schemas but were not in the `/exchange` oneOf; they have been added.
4. **Gaps:** Multiple Info and Exchange types from the official docs are still not modeled in the spec; they are listed above for future work.

---

*Generated from official Hyperliquid API docs and openapi.yaml. Re-run verification after fixing or re-enabling the Hyperliquid MCP server if you want MCP-based checks.*
