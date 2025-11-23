# 🚀 Pump.fun Bonding Curve Logic — Custom Implementation with $TAI Base Token

This repository contains a **custom Pump.fun-style bonding-curve token launch system**, upgraded to support **$TAI (SPL token)** as the base asset instead of SOL.
It explains the logic behind minting, buying, price calculation, market cap tracking, virtual liquidity, Raydium migration, and fee distribution.

---

## 📌 **Pump.fun Flow (Original Model)**

1. **Mint token & buy initial supply** (uses SOL)
2. **Users buy on the bonding curve**
3. **Users sell anytime and receive SOL**
4. **Once the bonding curve reaches 100k MC**, the token reaches migration threshold
5. **$17k liquidity is deposited into Raydium LP and LP is burned**

---

## 🔄 **Our Updated Requirements**

### ✅ Replace SOL with **$TAI (SPL Token)**

* All buys & sells use **$TAI**
* Price discovery is based on **TAI reserves**
* LP deposited into Raydium becomes **TAI–TOKEN LP**

### 🧮 **Price Calculation Using TAI**

* Bonding curve price updates dynamically:

  * When users buy → token supply decreases, TAI increases
  * When users sell → token supply increases, TAI decreases

### 📊 **Market Cap**

* Original Pump.fun uses **$100K market cap target**
* MC is computed as:

```
MC = Remaining Token Supply * Current Token Price
```

* Price is derived from bonding curve state:

```
price = TAI_reserve / token_reserve
```

---

## 🧰 **Technical Logic Explained**

You launched a token with:

* **Total supply:** 10,000,000,000 tokens (10B)
* You bought initial tokens with **30 SOL** (virtual liquidity)

Pump.fun calculates starting price as:

```
starting_price = (virtual_solana + real_solana) / tokens_in_curve
```

Example from your case:

* Initial bought SOL: **30 SOL**
* This sets token price ≈ **3e-8 per token**

As users buy:

* Token supply in curve decreases
* TAI/SOL reserve increases
* Price increases
* Market cap approaches $100k threshold

### When MC reaches $100k:

Your pool looks like:

```
11M (GOAT) : 10K SOL
10K = 9970 REAL + 30 VIRTUAL
```

### Fee Model (Pump.fun Style):

* From **real 9970 SOL (or TAI)**:

  * A % fee goes to platform treasury
  * Remaining SOL/TAI becomes Raydium liquidity

### Raydium Step:

Deposit:

```
(TOKEN amount, TAI amount)
```

Burn LP to ensure decentralization.

---

# 📦 **Features of This Repo**

* Full bonding curve implementation
* Virtual liquidity configuration
* Custom base-token support (TAI instead of SOL)
* Buy & Sell functions
* Market cap tracking
* Raydium migration logic
* Fee extraction logic
* Curve state storage
* Developer-friendly TypeScript/Anchor structure

---

# 🧮 **Bonding Curve Formula Summary**

```
price = TAI_reserve / token_reserve
MC = circulating_supply * price
```

Price always increases as more TAI enters the contract.

---

# 🛠 **Installation**

```bash
git clone https://github.com/yourrepo
cd pumpfun-tai
npm install
```

---

# ⚙️ **Environment Setup**

Create `.env`:

```
RPC_URL=
WALLET_PRIVATE_KEY=
TAI_TOKEN_MINT=
```

---

# ▶️ **Usage Examples**

### Buy

```ts
await program.methods.buy(amountTAI).accounts({...}).rpc();
```

### Sell

```ts
await program.methods.sell(amountToken).accounts({...}).rpc();
```

### Get Curve Price

```ts
const price = await program.account.curve.fetch(curvePk);
```

---

# 🌊 **Raydium Liquidity Migration**

When MC hits target:

* Curve closes
* Fees extracted
* Remaining TAI + token deposited into Raydium
* LP burned
* Trading moves to Raydium AMM

---

# 📞 Contact & Support

If you want a custom Pump.fun fork, bonding curve, or token-launch infrastructure:

📨 **Telegram: [@terauss](https://t.me/terauss)**

---

# ⭐ Support This Project

Please star ⭐ this repository to support development.



