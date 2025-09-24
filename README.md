# 📖 SatoshiAnchor Protocol

**A revolutionary Bitcoin-collateralized stablecoin ecosystem built natively on Stacks Layer 2.**

SatoshiAnchor enables users to unlock liquidity against their Bitcoin holdings through an over-collateralized, trust-minimized stablecoin protocol. By combining Bitcoin’s unmatched security with Stacks’ programmability, SatoshiAnchor establishes a foundation for a robust, decentralized financial ecosystem.

---

## 🚀 System Overview

SatoshiAnchor transforms Bitcoin into a productive asset by allowing users to mint a fully on-chain stablecoin backed by BTC collateral.

Key components include:

* **Vaults** – User-owned non-custodial contracts securing BTC collateral.
* **Stablecoin Engine** – Mints and burns SAT (SatoshiAnchor Token) in accordance with collateralization rules.
* **Oracle System** – Multi-oracle BTC/USD price feeds with Byzantine fault tolerance.
* **Risk Management Layer** – Automated liquidation when vault collateralization falls below safety thresholds.
* **Governance Framework** – Protocol owner (DAO/guardian) can adjust system parameters.

---

## ⚙️ Core Features

* **150% Minimum Collateral Ratio** – Ensures robust over-collateralization.
* **125% Liquidation Threshold** – Automatic vault liquidation to maintain solvency.
* **Oracle-Based Price Discovery** – Multi-oracle BTC/USD feed with bounds checking.
* **SIP-010 Stablecoin Compliance** – Fully interoperable with Stacks DeFi ecosystem.
* **Dynamic Governance** – Protocol parameters tunable by governance contract.

---

## 🏗 Contract Architecture

The protocol is implemented as a **single Clarity contract** with modular components:

* **Trait Compliance**

  * Implements `sip-010-token` for SAT stablecoin interoperability.

* **Vault System**

  * Each vault is mapped to `(owner, id)` pair.
  * Stores collateral amount, minted supply, and creation metadata.
  * Managed via `create-vault`, `mint-stablecoin`, and `redeem-stablecoin`.

* **Oracle System**

  * Authorized oracles submit BTC price updates (`update-btc-price`).
  * Prices validated against bounds (`MAX-BTC-PRICE`, `MAX-TIMESTAMP`).
  * Latest BTC/USD value retrieved via `get-latest-btc-price`.

* **Risk Management**

  * Automatic liquidation when collateralization < `liquidation-threshold`.
  * Removes insolvent vaults and burns outstanding stablecoin.

* **Governance**

  * Owner-only functions to adjust parameters like collateral ratio or fees.

---

## 🔄 Data Flow

1. **Vault Creation**

   * User deposits BTC (represented via wrapped or referenced collateral).
   * Vault initialized with collateral amount and zero minted SAT.

2. **Minting Stablecoin**

   * User requests mint amount.
   * System calculates maximum mintable SAT = `(collateral × BTC price) / collateralization ratio`.
   * Protocol mints SAT and increases total supply.

3. **Redeeming Stablecoin**

   * User repays SAT to reclaim BTC collateral.
   * Stablecoin supply decreases accordingly.

4. **Liquidation**

   * If `collateralization < liquidation-threshold`, third-party liquidators can liquidate vault.
   * Vault deleted, minted SAT burned, system solvency preserved.

---

## 📊 Protocol Parameters

| Parameter               | Default       | Purpose                                        |
| ----------------------- | ------------- | ---------------------------------------------- |
| Collateralization Ratio | 150%          | Minimum safety margin for vaults               |
| Liquidation Threshold   | 125%          | Collateralization level triggering liquidation |
| Mint Fee (bps)          | 50            | Fee charged on mint operations                 |
| Redemption Fee (bps)    | 50            | Fee charged on redemption                      |
| Max Mint Limit          | 1,000,000 SAT | Protocol-wide cap on minted stablecoin         |

---

## 🔐 Security Considerations

* **Oracle Validation** – BTC price bounded within safe maximum values.
* **Authorization Control** – Oracle management & governance restricted to contract owner.
* **Invariant Enforcement** – Collateral ratios strictly enforced during mint/redeem.
* **Vault Isolation** – Each vault scoped to `(owner, id)` ensuring separation of user positions.

---

## 📡 Read-Only Functions

* `get-latest-btc-price` – Returns the most recent BTC/USD price.
* `get-vault-details` – Returns vault information (collateral, minted supply, creation block).
* `get-total-supply` – Returns circulating supply of SAT.

---

## 🛠 Development & Deployment

### Requirements

* [Clarinet](https://github.com/hirosystems/clarinet) ≥ v1.0.0
* Stacks blockchain testnet/mainnet environment

### Build & Test

```bash
clarinet check
clarinet test
```

### Deployment

Deploy via Clarinet or directly using Stacks CLI:

```bash
clarinet deploy
```

---

## 📖 License

This protocol is released under the **MIT License**.

---

⚡ **SatoshiAnchor Protocol** – Unlocking Bitcoin’s liquidity for the decentralized economy.
