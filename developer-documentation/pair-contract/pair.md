# Pair Contract Reference

The `Pair` contract is the core liquidity pool implementation for Lithos, handling token swaps, liquidity provision, and fee accrual. Each pair can be either stable (using a concentrated liquidity curve) or volatile (using a constant product formula). This reference documents all externally callable functions, their expected inputs, return values, and operational notes.

## Key Concepts

- **Pool Types** – Pairs are either `stable` (concentrated liquidity for similar assets) or `volatile` (constant product for dissimilar assets), determined at creation.
- **Token Order** – `token0` and `token1` are sorted by address to ensure deterministic pair addresses.
- **LP Tokens** – Liquidity providers receive ERC-20 tokens representing their share of the pool.
- **Fee Accumulation** – Trading fees are segregated from pool reserves and can be claimed separately by LPs.
- **TWAP Oracle** – Each pair maintains a time-weighted average price (TWAP) oracle with 30-minute observation periods.

## State Variables

### Pool Configuration
- `name` – Human-readable pool name (e.g., "StableV1 AMM - USDC/USDT")
- `symbol` – Pool symbol (e.g., "sAMM-USDC/USDT")
- `decimals` – Always 18 for LP tokens
- `stable` – Boolean indicating if this is a stable pool
- `token0`, `token1` – The two tokens in the pool (sorted by address)
- `factory` – Address of the PairFactory that created this pair
- `fees` – Address of the PairFees contract handling fee distribution

### Reserves and Balances
- `reserve0`, `reserve1` – Current token reserves in the pool
- `totalSupply` – Total LP tokens minted
- `balanceOf` – ERC-20 balance mapping for LP tokens
- `allowance` – ERC-20 allowance mapping

### Fee Tracking
- `index0`, `index1` – Global fee indexes for each token
- `supplyIndex0`, `supplyIndex1` – User-specific fee indexes
- `claimable0`, `claimable1` – Accumulated but unclaimed fees per user

### Oracle Data
- `observations` – Array of TWAP observations (timestamp, reserve0Cumulative, reserve1Cumulative)
- `periodSize` – 30 minutes (1800 seconds) between observations
- `reserve0CumulativeLast`, `reserve1CumulativeLast` – Last cumulative reserve values

## Utility Queries

### `metadata() → (uint256 dec0, uint256 dec1, uint256 r0, uint256 r1, bool st, address t0, address t1)`
Returns comprehensive pool metadata including token decimals, current reserves, stability flag, and token addresses.

### `tokens() → (address, address)`
Returns the two token addresses in the pool.

### `isStable() → bool`
Returns true if this is a stable pool, false if volatile.

### `getReserves() → (uint256 _reserve0, uint256 _reserve1, uint256 _blockTimestampLast)`
Returns current reserves and the timestamp of the last reserve update.

### `observationLength() → uint256`
Returns the number of TWAP observations recorded.

### `lastObservation() → Observation`
Returns the most recent TWAP observation.

## Price Oracle Functions

### `current(address tokenIn, uint256 amountIn) → uint256 amountOut`
Returns the current TWAP price for swapping `amountIn` of `tokenIn`. Uses the most recent observation period.

### `quote(address tokenIn, uint256 amountIn, uint256 granularity) → uint256 amountOut`
Returns an average TWAP price over multiple observation periods. `granularity` specifies how many periods to average.

### `prices(address tokenIn, uint256 amountIn, uint256 points) → uint256[] memory`
Returns an array of historical TWAP prices for the last `points` observation periods.

### `sample(address tokenIn, uint256 amountIn, uint256 points, uint256 window) → uint256[] memory`
Low-level function for sampling TWAP prices. `points` is the number of samples, `window` is the step size between observations.

### `currentCumulativePrices() → (uint256 reserve0Cumulative, uint256 reserve1Cumulative, uint256 blockTimestamp)`
Returns the current cumulative reserve values, adjusted for time elapsed since the last update.

## Swap Functions

### `getAmountOut(uint256 amountIn, address tokenIn) → uint256 amountOut`
Calculates the expected output amount for a swap, accounting for trading fees. This is a view function that doesn't execute the swap.

### `swap(uint256 amount0Out, uint256 amount1Out, address to, bytes calldata data) →`
Executes a token swap. Transfers output tokens to `to` and handles fee accrual. Supports flash loans via the `data` parameter.

**Parameters:**
- `amount0Out` – Amount of token0 to send out (0 if not swapping token0)
- `amount1Out` – Amount of token1 to send out (0 if not swapping token1)
- `to` – Recipient address for output tokens
- `data` – Optional data for flash loan callbacks

**Requirements:**
- At least one output amount must be > 0
- Output amounts must be less than current reserves
- Pool must not be paused (checked via factory)
- `to` cannot be either token address

**Events:**
- Emits `Swap` event with input/output amounts and recipient

## Liquidity Management

### `mint(address to) → uint256 liquidity`
Mints LP tokens for the caller based on tokens transferred to the pool. Called by the router after token transfers.

**Parameters:**
- `to` – Recipient of minted LP tokens

**Returns:**
- `liquidity` – Amount of LP tokens minted

**Logic:**
- For initial liquidity: `liquidity = sqrt(amount0 * amount1) - MINIMUM_LIQUIDITY`
- For additional liquidity: `liquidity = min((amount0 * totalSupply) / reserve0, (amount1 * totalSupply) / reserve1)`
- Burns `MINIMUM_LIQUIDITY` tokens to address(0) on initial mint

### `burn(address to) → (uint256 amount0, uint256 amount1)`
Burns LP tokens and returns underlying tokens to `to`. Called by the router.

**Parameters:**
- `to` – Recipient of underlying tokens

**Returns:**
- `amount0`, `amount1` – Amounts of each token returned

**Logic:**
- Calculates proportional amounts based on current pool balances
- Requires both amounts > 0

## Fee Management

### `claimFees() → (uint256 claimed0, uint256 claimed1)`
Claims accumulated trading fees for the caller. Fees are accrued based on the user's LP token balance and the global fee indexes.

**Returns:**
- `claimed0`, `claimed1` – Amounts of each token claimed

**Events:**
- Emits `Claim` event with amounts and recipient

### `claimStakingFees() →`
Withdraws accumulated staking fees to the staking fee handler configured in the factory. Can only be called by the staking fee handler.

## ERC-20 Functions

### `approve(address spender, uint256 amount) → bool`
ERC-20 approve function. Updates fee tracking for both owner and spender.

### `permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) →`
EIP-2612 permit function for gasless approvals. Updates fee tracking for the owner.

### `transfer(address dst, uint256 amount) → bool`
ERC-20 transfer function. Updates fee tracking for both sender and recipient.

### `transferFrom(address src, address dst, uint256 amount) → bool`
ERC-20 transferFrom function. Handles allowance updates and fee tracking for all parties.

## Administrative Functions

### `skim(address to) →`
Transfers any excess tokens (balance > reserves) to `to`. Used to recover tokens accidentally sent to the pair.

### `sync() →`
Forces reserve updates to match current token balances. Used to recover from accounting discrepancies.

## Fee Calculation Details

Trading fees are split into two components:
1. **Referral Fee** – 12% of trading fees sent to the Dibs contract as protocol revenue
2. **LP Fee** – 88% of trading fees distributed to liquidity providers via fee indexes

The fee calculation process:
1. Calculate total fee: `amountIn * feeRate / 10000`
2. Subtract referral fee (12%) and transfer to Dibs
3. Update LP fee index with remaining fee (88%): `index += (lpFee * 1e18) / totalSupply`

**Note:** NFT fees are currently set to 0% and are not collected.

## TWAP Oracle Implementation

The oracle records cumulative reserve values every 30 minutes:
- `reserve0Cumulative += reserve0 * timeElapsed`
- `reserve1Cumulative += reserve1 * timeElapsed`

Price calculations use the difference between observations:
- `avgReserve0 = (currentCumulative0 - observationCumulative0) / timeElapsed`
- `avgReserve1 = (currentCumulative1 - observationCumulative1) / timeElapsed`

## Integration Notes

- **Initial Liquidity** – First liquidity providers must account for `MINIMUM_LIQUIDITY` (1000 tokens) being burned
- **Fee Claims** – Users must call `claimFees()` to receive accumulated trading fees
- **Flash Loans** – Supported via the `data` parameter in `swap()` function
- **Reentrancy Protection** – All state-changing functions are protected by a reentrancy guard
- **Balance Updates** – Fee tracking is updated on all balance changes (mint, burn, transfer)
- **Oracle Usage** – TWAP observations are only recorded when more than 30 minutes have elapsed since the last observation

## Error Codes

- `ILM` – INSUFFICIENT_LIQUIDITY_MINTED
- `ILB` – INSUFFICIENT_LIQUIDITY_BURNED
- `IOA` – INSUFFICIENT_OUTPUT_AMOUNT
- `IL` – INSUFFICIENT_LIQUIDITY
- `IT` – INVALID_TO
- `IIA` – INSUFFICIENT_INPUT_AMOUNT
- `K` – Constant product invariant check failed
- `EXPIRED` – Permit deadline expired
- `INVALID_SIGNATURE` – Invalid EIP-712 signature