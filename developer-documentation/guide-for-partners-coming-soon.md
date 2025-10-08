# Guide For Partners

This guide provides essential information for partners looking to integrate with the Lithos protocol, including contract references, integration patterns, and best practices.

## Overview

Lithos is a **3,3 DEX** built on the Plasma blockchain, serving as the central liquidity layer for the ecosystem. Partners can integrate with Lithos to provide trading services, access deep liquidity, and participate in governance.

## Core Contracts

### RouterV2
The `RouterV2` contract is the primary integration surface for swapping assets and managing liquidity. All external integrations should use this contract as the main entry point.

**Key Features:**
- Multi-hop swaps with customizable routes
- Liquidity provision and removal
- Support for both stable and volatile pools
- ETH wrapping/unwrapping
- Fee-on-transfer token support

**Documentation:** [RouterV2 Contract Reference](router-v2.md)

**Mainnet Address:** `0xD70962bd7C6B3567a8c893b55a8aBC1E151759f3`

### Pair Contract
The `Pair` contract represents individual liquidity pools, handling token swaps, liquidity provision, and fee accrual. Each pair can be either stable (concentrated liquidity) or volatile (constant product).

**Key Features:**
- Stable and volatile pool types
- TWAP oracle for price feeds
- Segregated fee system
- Flash loan support
- ERC-20 LP tokens

**Documentation:** [Pair Contract Reference](pair-contract/pair.md)

## Integration Patterns

### Basic Swap Integration

```solidity
// Example: Swap token A for token B
function swapTokens(
    address tokenIn,
    address tokenOut,
    uint256 amountIn,
    uint256 amountOutMin,
    address recipient,
    bool useStablePool
) external {
    // Approve router to spend tokens
    IERC20(tokenIn).approve(routerAddress, amountIn);
    
    // Execute swap
    IRouterV2(routerAddress).swapExactTokensForTokensSimple(
        amountIn,
        amountOutMin,
        tokenIn,
        tokenOut,
        useStablePool,
        recipient,
        block.timestamp + 300 // 5 minute deadline
    );
}
```

### Liquidity Provision

```solidity
// Example: Add liquidity to a pool
function addLiquidity(
    address tokenA,
    address tokenB,
    uint256 amountADesired,
    uint256 amountBDesired,
    uint256 amountAMin,
    uint256 amountBMin,
    bool useStablePool
) external returns (uint256 liquidity) {
    // Approve tokens
    IERC20(tokenA).approve(routerAddress, amountADesired);
    IERC20(tokenB).approve(routerAddress, amountBDesired);
    
    // Add liquidity
    (uint256 amountA, uint256 amountB, liquidity) = IRouterV2(routerAddress).addLiquidity(
        tokenA,
        tokenB,
        useStablePool,
        amountADesired,
        amountBDesired,
        amountAMin,
        amountBMin,
        msg.sender,
        block.timestamp + 300
    );
}
```

### Price Oracle Integration

```solidity
// Example: Get TWAP price from a pair
function getTWAPPrice(
    address pairAddress,
    address tokenIn,
    uint256 amountIn
) external view returns (uint256 amountOut) {
    IPair pair = IPair(pairAddress);
    amountOut = pair.current(tokenIn, amountIn);
}
```

## Fee Structure

Lithos implements a transparent fee structure:

- **Trading Fees:** Variable rate set by the factory (typically 0.2% - 0.3%)
  - 12% referral fees (protocol revenue)
  - 88% LP fees (distributed to liquidity providers)
  - 0% NFT fees (currently disabled)

- **Fee Claims:** Liquidity providers must actively claim accumulated fees using the `claimFees()` function on the Pair contract.

## Pool Types

### Stable Pools
- Designed for similar assets (e.g., USDC/USDT, WBTC/renBTC)
- Use concentrated liquidity formula: `x³y + y³x >= k`
- Lower slippage for correlated assets
- Better capital efficiency for stablecoin pairs

### Volatile Pools
- Designed for dissimilar assets (e.g., ETH/USDC, tokenA/tokenB)
- Use constant product formula: `xy >= k`
- Standard AMM behavior
- Suitable for assets with uncorrelated price movements

## Best Practices

### 1. Pool Selection
- Use stable pools for assets with similar value and high correlation
- Use volatile pools for assets with different value propositions
- Check pool existence before attempting swaps or liquidity operations

### 2. Slippage Protection
- Always set appropriate `amountOutMin` values for swaps
- Consider using multi-hop routes for better pricing
- Use the router's quoting functions to estimate outputs

### 3. Deadline Management
- Set reasonable deadlines for all transactions
- Consider network congestion and block times
- Avoid extremely long deadlines that could expose users to MEV

### 4. Gas Optimization
- Batch operations where possible
- Use `swapExactTokensForTokensSimple` for single-hop swaps
- Consider the trade-off between gas cost and price improvement

### 5. Error Handling
- Handle common revert reasons gracefully
- Provide clear error messages to users
- Implement retry logic for failed transactions

## Security Considerations

### Contract Verification
- All Lithos contracts are verified on PlasmaScan
- Always verify contract addresses before integration
- Check for any recent contract upgrades

### Reentrancy Protection
- All Pair contracts include reentrancy guards
- Router contracts follow checks-effects-interactions pattern
- Flash loans are supported but properly isolated

### Access Control
- Critical functions are protected by timelock and multisig
- Factory can pause trading in emergency situations
- Governance changes require multi-signature approval

## Testing and Deployment

### Testnet Integration
- Test all integrations on Plasma testnet first
- Use testnet tokens for initial testing
- Verify contract interactions with small amounts

### Mainnet Deployment
- Start with small transaction sizes
- Monitor gas usage and transaction success rates
- Implement monitoring for contract upgrades

## Support and Resources

### Documentation
- [Deployed Contracts](deployed-contracts.md) - Mainnet contract addresses
- [RouterV2 Contract Reference](router-v2.md) - Detailed router documentation
- [Pair Contract Reference](pair-contract/pair.md) - Detailed pair documentation
- [Security](../security.md) - Security information and audit history

### Tools and Utilities
- PlasmaScan for contract verification and transaction monitoring
- Block explorers for real-time data
- Community forums for integration support

### Contact
- For technical integration questions, reach out to the Lithos development team
- Security concerns should be reported through the official security channels
- Partnership inquiries can be directed to the business development team

## Upcoming Features

This section will be updated as new features are added to the Lithos protocol:

- Advanced liquidity management tools
- Cross-chain integration capabilities
- Enhanced governance features
- Additional pool types and strategies

---

*This guide will be continuously updated as the Lithos protocol evolves. Check back regularly for the latest integration information and best practices.*