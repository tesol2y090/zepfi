# ZepFi — Cross-Chain TWAP Swap Executor with Best-Price Routing

**ZepFi** enables decentralized, gasless, cross-chain TWAP (Time-Weighted Average Price) swaps by combining the power of:

- 🧠 **1inch Fusion+** (intent-based execution)
- 🔐 **Limit Order Protocol v3** (on-chain settlement)
- ⚙️ **Smart contract hooks** (custom logic: TWAP, slippage, oracle checks)
- 🌉 **Axelar SDK** (cross-chain bridging + GMP)
- 🤖 **Custom resolver engine** (off-chain price routing & fill logic)

With ZepFi, users can split large trades into scheduled swaps that execute across **Ethereum, Moonbeam, NEAR, or Sui**, depending on the best price at each interval — all without paying gas or interacting with multiple chains.

## Use Case

> “Swap 100 ETH to USDC over 5 hours, every 1 hour, on whichever chain offers the best price.”

ZepFi:

- Splits the trade into **5 off-chain signed limit orders** (chunks)
- Every hour, finds the **best cross-chain execution path**
- Bridges ETH to that chain (via Axelar)
- Swaps ETH → USDC on the local DEX
- Sends USDC to the user on the chosen chain

## 🧠 How It Works

ZepFi enables automated, gasless cross-chain execution by combining **1inch’s intent-based architecture** with a custom **TWAP strategy resolver** and **Axelar-powered bridging**.

### Step-by-Step Flow

1. **User creates a TWAP swap**

   - Inputs: `100 ETH → USDC`, over `5 hours`, in `5 chunks`
   - Signs `5 EIP-712 orders` using the 1inch Fusion SDK
   - Orders include:
     - `makingAmount` = 20 ETH per chunk
     - `minReturn` = USDC floor
     - `validAfter` = timestamp when that chunk is eligible
     - Optional `interactionTarget` for TWAPHook

2. **Orders are stored off-chain**

   - Stored in ZepFi backend (or IPFS, DB, etc.)
   - The user doesn’t need to submit anything on-chain

3. **Every hour, resolver activates**

   - ZepFi’s off-chain **resolver bot** wakes up on a cron schedule
   - It queries swap quotes across chains:
     - 1inch API (Ethereum)
     - Beamswap (Moonbeam)
     - Ref Finance (NEAR)
     - Cetus (Sui)
   - Compares `ETH → USDC` prices and slippage

4. **Resolver chooses best chain**

   - If the best price is **not on Ethereum**:
     - It uses **Axelar** to bridge 20 ETH to that chain
     - Waits for confirmation + executes local swap

5. **Order is filled on-chain**

   - Resolver calls `fillOrder()` on the **Limit Order Protocol** on the best chain
   - If `interactionTarget` is set, `TWAPHook` verifies:
     - Enough time has passed
     - Oracle price is within safe bounds

6. **USDC is delivered to the user**

   - Either directly on the destination chain
   - Or bridged back to user’s chain via Axelar GMP

7. **This repeats until all TWAP chunks are executed**

### Execution Timeline Example

| Time    | Action                                               |
| ------- | ---------------------------------------------------- |
| T = 0   | Chunk #1: Swapped on Ethereum                        |
| T = +1h | Chunk #2: Bridged to Moonbeam → Swapped via Beamswap |
| T = +2h | Chunk #3: Swapped on NEAR via Ref Finance            |
| T = +3h | Chunk #4: Skipped (price out of bounds)              |
| T = +4h | Chunk #5: Swapped on Sui via Cetus                   |

This gives the user a **blended average price** and reduced market impact.

---

## Features

- Split Execution — Break large swaps into timed, smaller chunks

- Cross-Chain Routing — Execute each chunk on the chain with best rate

- UX — Users only sign; resolvers pay gas

- Price Guardrails — Optional Chainlink oracle enforcement

- Composable Hooks — Smart contracts validate TWAP intervals/slippage

- Modular Bridging Layer — Axelar used for token + message transport

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
