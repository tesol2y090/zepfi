# ZepFi — Cross-Chain TWAP Swaps with Best Execution

**ZepFi** is a decentralized intent-based trading engine that enables users to execute large token swaps across multiple blockchains using a **Time-Weighted Average Price (TWAP)** strategy. At every interval, ZepFi finds the **best price across chains like Ethereum, Polkadot (Moonbeam), NEAR, and Sui**, and executes the trade — minimizing slippage, avoiding MEV, and maximizing execution efficiency.

Built on top of **1inch Fusion+**, ZepFi unifies cross-chain liquidity into a single intelligent trading flow.

## Features

- ⏱ **TWAP Execution**: Break down large swaps into timed intervals for better average pricing.
- 🌉 **Cross-Chain Routing**: Bridge tokens and execute swaps across Ethereum, Moonbeam, NEAR, and Sui.
- 💸 **Best Price Discovery**: Resolver scans liquidity sources across chains to find the most optimal swap path.
- 🧠 **Intent-Based Architecture**: Users sign intents off-chain; resolvers execute them on-chain.
- 📊 **Visual Progress Dashboard**: Track each executed chunk and monitor TWAP performance.

## How It Works

1. User submits a TWAP swap (e.g., 10 ETH → USDC over 5 rounds)

2. ZepFi signs 5 separate intents with timing and price constraints

3. At each interval:

   - Resolver fetches swap prices across all supported chains

   - Finds best execution path

   - Swaps and sends USDC to the user

4. Final report shows TWAP achieved vs market price

## Team ZepFi

@tesol2y090 — Full-stack + Smart Contract

## Built for ETHGlobal Unite DeFi Hackathon

This project is an entry to the ETHGlobal Unite DeFi hackathon.
It leverages 1inch Fusion+, Limit Order Protocol, and cross-chain infrastructure to deliver next-gen intent-based trading experiences.
