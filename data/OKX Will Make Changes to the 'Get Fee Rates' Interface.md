# OKX Will Make Changes to the 'Get Fee Rates' Interface  

Cryptocurrency traders and developers relying on OKX's API must prepare for critical updates to the **"Get Fee Rates"** interface. Scheduled for May 7, 2022, these adjustments streamline fee rate categorization while introducing new parameters tailored for **USDT-margined** and **crypto-margined contracts**. This article breaks down the changes, their technical implications, and actionable steps to ensure seamless integration with OKX's upgraded system.  

## Key Adjustments to the Fee Rate System  

OKX's fee rate overhaul simplifies categorization by **removing the 'category' field** from the API response. Previously, this field distinguished fee tiers across currency pairs. Now, all spot trading pairs will share a unified fee structure. However, **futures and perpetual contracts** will diverge based on margin type:  

- **USDT-margined contracts**: Fee rates will now be represented by the new `takerU` and `makerU` parameters.  
- **Crypto-margined contracts**: The original `taker` and `maker` fields will exclusively apply to these instruments.  

This shift ensures clearer fee calculations for traders leveraging different margin types, addressing a growing need in the dynamic crypto derivatives market.  

👉 [Explore OKX's trading platform for real-time updates](https://bit.ly/okx-bonus)  

### Why This Matters for Traders and Developers  

The removal of the `category` field reduces complexity for spot traders, who no longer need to map currency pairs to specific fee tiers. However, developers integrating OKX's API must update their systems to handle:  
1. **Deprecated `category` parameter** in requests.  
2. **New `takerU`/`makerU` fields** for futures/perpetual contracts.  
3. **Revised scope of `taker`/`maker`** to apply only to crypto-margined positions.  

Failure to adapt could disrupt automated trading strategies or fee calculation tools, leading to execution errors or inaccurate cost projections.  

## Technical Details: Updated API Parameters  

### Endpoint Overview  

- **Path**: `GET /api/v5/account/trade-fee` (unchanged)  
- **Purpose**: Retrieve instrument-specific fee rates.  

### Request Parameters  

| Parameter    | Type   | Required | Description                              |  
|--------------|--------|----------|------------------------------------------|  
| `instType`   | String | Yes      | Instrument type (SPOT, MARGIN, SWAP, etc.)|  
| `instId`     | String | Optional | Instrument ID (e.g., BTC-USDT)            |  
| `uly`        | String | Optional | Underlying asset (e.g., BTC-USD)          |  
| `category`   | String | Optional | **Deprecated field** (no longer used)     |  

### Return Parameters  

| Parameter    | Type   | Description                              |  
|--------------|--------|------------------------------------------|  
| `taker`      | String | Taker fee for crypto-margined contracts  |  
| `maker`      | String | Maker fee for crypto-margined contracts  |  
| `takerU`     | String | Taker fee for USDT-margined contracts    |  
| `makerU`     | String | Maker fee for USDT-margined contracts    |  
| `delivery`   | String | Delivery fee rate                        |  
| `exercise`   | String | Option exercise fee                      |  
| `level`      | String | Fee rate tier                            |  
| `instType`   | String | Instrument type                          |  
| `ts`         | String | Timestamp (Unix in milliseconds)         |  

### Example Use Case  

A trader querying fees for a BTC-USDT perpetual contract would:  
1. Set `instType=SWAP` and `uly=BTC-USD`.  
2. Receive distinct values for `takerU`/`makerU` (USDT-margined) and `taker`/`maker` (crypto-margined).  

This dual-parameter approach enables precise cost modeling across margin types.  

👉 [Optimize your trading strategy with OKX's tools](https://bit.ly/okx-bonus)  

## FAQs: Addressing Common Concerns  

### How does removing the `category` field affect existing integrations?  
The deprecated field remains in the response for backward compatibility but will no longer influence fee calculations. Developers can safely ignore it in future updates.  

### What happens if I don’t update my API integration by May 7, 2022?  
Systems relying on outdated `taker`/`maker` values for USDT-margined contracts will return **incorrect fee rates**, potentially skewing trading strategies.  

### Are these changes applicable to options trading?  
No. The updates focus on **futures and perpetual contracts**. Options retain their existing fee structure (`exercise` parameter).  

### How can I test the updated API?  
OKX recommends using the **testnet environment** to validate API calls before the live rollout.  

### Will historical fee data reflect these changes?  
Historical responses will remain unchanged. The new structure applies only to data generated **post-May 7, 2022**.  

## Best Practices for a Smooth Transition  

1. **Audit Existing Code**: Identify dependencies on the `category` field or legacy `taker`/`maker` logic.  
2. **Update Parameter Mapping**: Integrate `takerU`/`makerU` for futures/perpetual contract calculations.  
3. **Validate Responses**: Ensure backward compatibility by handling both old and new parameter sets during migration.  
4. **Monitor Logs**: Track API errors related to deprecated fields after deployment.  

## Conclusion  

OKX's "Get Fee Rates" update enhances transparency for derivatives traders while simplifying spot fee structures. By proactively adapting to these changes, developers and traders can avoid disruptions and leverage **precise fee modeling** across USDT- and crypto-margined instruments.  

👉 [Stay ahead with OKX's evolving trading solutions](https://bit.ly/okx-bonus)  

*For detailed technical documentation, visit [OKX API Guides](https://bit.ly/okx-bonus).*