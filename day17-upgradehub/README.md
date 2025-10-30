# Day 17 – UpgradeHub.sol

## Concept

Upgradeable Smart Contracts using Proxy Pattern and Delegatecall.

## Objective

The goal of this task is to implement an upgradeable subscription manager using the Proxy Pattern in Solidity. The proxy holds the data, and the external logic contracts (V1 and V2) handle the logic. Using `delegatecall`, we can upgrade the logic without losing stored data.

---


## Description of Contracts

**UpgradeHub.sol**
Proxy contract that:

* Stores user subscription data (plan IDs and expiry dates)
* Delegates function calls to the current logic contract using `delegatecall`
* Allows the owner to upgrade logic implementation

**SubscriptionLogicV1.sol**
Initial version of the logic contract that:

* Manages subscriptions and renewals
* Writes to proxy storage through delegatecall

**SubscriptionLogicV2.sol**
Upgraded logic contract that:

* Retains all V1 functionality
* Adds pause and resume features for user subscriptions

---

## Foundry Commands

### Build the project

```bash
forge build
```

### Deploy Logic V1

```bash
forge create src/SubscriptionLogicV1.sol:SubscriptionLogicV1 \
  --rpc-url http://127.0.0.1:8545 \
  --private-key <PRIVATE_KEY> \
  --broadcast
```

### Deploy Proxy

```bash
forge create src/UpgradeHub.sol:UpgradeHub \
  --constructor-args <LOGIC_V1_ADDRESS> \
  --rpc-url http://127.0.0.1:8545 \
  --private-key <PRIVATE_KEY> \
  --broadcast
```

### Interact with Proxy (Subscribe Function)

```bash
cast send <PROXY_ADDRESS> "subscribe(uint256,uint256)" 1 30 \
  --rpc-url http://127.0.0.1:8545 \
  --private-key <PRIVATE_KEY>
```

### Deploy Logic V2

```bash
forge create src/SubscriptionLogicV2.sol:SubscriptionLogicV2 \
  --rpc-url http://127.0.0.1:8545 \
  --private-key <PRIVATE_KEY> \
  --broadcast
```

### Upgrade Logic Implementation

```bash
cast send <PROXY_ADDRESS> "upgradeTo(address)" <LOGIC_V2_ADDRESS> \
  --rpc-url http://127.0.0.1:8545 \
  --private-key <PRIVATE_KEY>
```

---

## Key Learnings

1. Separation of storage and logic using Proxy Pattern.
2. Using `delegatecall` to execute logic in another contract while preserving storage.
3. Safely upgrading smart contracts without losing user data.
4. Understanding storage layout consistency between proxy and logic contracts.

---

## Summary

This project demonstrates how to design upgradeable contracts using the Proxy Pattern in Solidity. The `UpgradeHub` contract serves as a persistent data holder, while `SubscriptionLogicV1` and `SubscriptionLogicV2` define upgradable logic layers. This architecture ensures flexibility and maintainability for real-world decentralized applications.

---

# End of the Project.
