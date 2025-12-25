# Execution Responsibility Inversion  
## Why Gas Optimization Failed and How Permissioned Pull Fixes a Root Cause

**Author:** Mats Heming Julner  
**Date:** December 25, 2025

---

## Abstract

Blockchain systems have spent over a decade attempting to reduce transaction fees and improve user experience through gas optimization, rollups, account abstraction, and execution batching. While these approaches reduced per-transaction cost, they failed to eliminate fee volatility, failed transaction loss, and persistent UX degradation under demand.

This paper argues that these failures are not the result of insufficient optimization, but of a misassigned execution responsibility inherent to push-based transaction models. In push-based systems, asset owners are required to pay execution costs at the moment of state change, directly exposing end users to network congestion and execution risk.

We introduce **Execution Responsibility Inversion**, a design principle in which authorization and execution are separated via permissioned pull semantics. Under this model, users pre-authorize bounded outcomes, while professional executors perform execution and bear gas costs. Scarcity is not eliminated, but routed away from end users and absorbed by actors capable of optimizing, batching, and amortizing execution costs.

We show that this model resolves longstanding gas UX failures, reduces custodial pressure, improves composability, and can be adopted on existing blockchains without protocol changes. The gas problem, we conclude, was never about price; it was about responsibility.

---

## 1. Introduction

Transaction fees (“gas”) have remained one of the most persistent usability barriers in blockchain systems. Despite substantial progress in execution efficiency, users continue to experience:

- Sudden fee spikes during congestion  
- Failed transactions that still incur cost  
- Uncertainty around execution timing  
- Cognitive overhead when interacting with decentralized applications  

Layer-2 rollups, calldata compression, and account abstraction have reduced base fees, but have not resolved these issues at scale. As demand increases, fees inevitably rise, re-exposing users to volatility and execution risk.

This persistence suggests that the problem is not merely technical, but structural.

This paper proposes that gas UX failures stem from an unexamined assumption embedded in push-based transaction models:

> **The asset owner must pay the execution cost at the moment of state change.**

We argue that this assumption is neither fundamental nor necessary, and that its removal explains why gas optimization has repeatedly failed to deliver stable user experience.

---

## 2. Push-Based Execution Models

In push-based blockchains, state changes occur when users broadcast transactions that directly mutate state. These transactions require:

- The user to sign intent  
- The user to submit the transaction  
- The user to pay execution cost (gas)  
- The user to bear failure risk  

All four responsibilities are collapsed onto a single actor: the end user.

This model tightly couples:

- Ownership  
- Authorization  
- Execution  
- Cost responsibility  

While simple, this coupling produces several systemic effects.

### 2.1 Direct Exposure to Scarcity

Blockspace is a scarce resource. When demand exceeds supply, prices rise. In push systems, this scarcity is experienced directly by end users, who must decide when to transact and how much to pay.

This creates:

- Fee shock during congestion  
- Timing anxiety  
- Adversarial competition with automated actors  

### 2.2 Failed Transaction Loss

Because execution cost is paid upfront, failed transactions still incur loss. Users must reason about nonce ordering, state changes, MEV risk, and timing; responsibilities typically reserved for infrastructure operators.

### 2.3 UX Degradation Under Load

As demand increases, UX degrades non-linearly:

- Wallets become harder to use  
- Applications become unreliable  
- Users retreat to custodial services  

This pattern has repeated across all push-based chains, regardless of performance improvements.

---

## 3. Why Gas Optimization Cannot Solve UX

Gas optimization attempts to reduce the cost per unit of execution. While beneficial, optimization does not alter who bears the cost.

Even with:

- Faster block times  
- Cheaper calldata  
- Higher throughput  

Scarcity remains fundamental.

As long as users are directly exposed to execution markets, fee volatility will resurface whenever demand increases.

> **Gas optimization reduces cost, but does not reduce exposure.**

This explains why:

- Layer-2 fees rise once popular  
- Account abstraction fails to eliminate user anxiety  
- Relayers and sponsorship systems remain fragile  

These approaches treat symptoms while preserving the underlying responsibility assignment.

---

## 4. Execution as an Economic Responsibility

Gas is often framed as a technical or performance concern. In reality, gas represents execution cost under scarcity.

In mature systems, scarcity is not eliminated. It is routed.

Other infrastructure domains consistently assign scarcity exposure to actors capable of managing it:

- ISPs absorb bandwidth spikes  
- Payment processors absorb network fees  
- Cloud providers absorb compute volatility  

Push-based blockchains uniquely assign this responsibility to retail users.

This misassignment explains both poor UX and the persistent re-emergence of custody as a “solution.”

---

## 5. Permissioned Pull and Consent-Based Execution

Permissioned pull introduces a separation between authorization and execution.

### 5.1 Consent as a First-Class Object

Users create durable, revocable authorization objects that define:

- Who may execute  
- What may be executed  
- Under what constraints  
- For what duration  

These consents define **allowed outcomes**, not specific actions.

### 5.2 Executors as Distinct Actors

Executors:

- Monitor active consents  
- Choose when and how to execute  
- Pay gas  
- Bear execution risk  

Users do not initiate transactions per action. They manage permissions.

---

## 6. Execution Responsibility Inversion

This separation produces a fundamental inversion:

| Dimension | Push Model | Pull Model |
|---------|-----------|------------|
| Who authorizes | User | User |
| Who executes | User | Executor |
| Who pays gas | User | Executor |
| Who bears failure | User | Executor |

Scarcity still exists. Fees still rise under load.

But scarcity is absorbed by professional actors capable of batching, retrying, optimizing timing and amortizing cost.   

This inversion does not eliminate gas. It removes end users from direct exposure to it.

---

## 7. Ethereum as a Case Study

Importantly, permissioned pull does not require changes to Ethereum’s protocol.

Ethereum already supports:

- Contract-based authorization  
- Signature verification  
- Delegated execution  
- Relayers and batching  

Ethereum already supports the technical primitives required for permissioned pull execution, including contract-based authorization, signature verification, and delegated execution. However, widespread adoption would require a significant shift in default wallet behavior, application design, and user mental models.

The challenge is therefore not protocol capability, but coordination and abstraction alignment.

---

## 8. Secondary Effects

Execution Responsibility Inversion produces several downstream effects:

- Custody becomes optional, not necessary  
- Wallets become permission dashboards, not transaction consoles  
- Failed transactions disappear from user experience  
- MEV shifts from extraction to competition  
- Scaling reframes from throughput to scarcity routing  

These effects are consequences of responsibility reassignment, not independent features.

---

### 8.1 Expertise Compression and the Disappearance of Failure from User Experience

One of the strongest indicators of a correct abstraction in distributed systems is the reduction of expertise required to use the system safely and efficiently. Mature infrastructure does not eliminate failure; rather, it relocates failure handling to layers and actors capable of managing it.

Push-based blockchain systems fail this test.

In push execution models, end users are required to reason about execution-level concerns, including gas pricing, execution timing, nonce ordering, state races, and failure modes. Failed transactions are surfaced directly to users, often accompanied by irreversible cost.

This requirement represents abstraction leakage. In comparable infrastructure domains, such leakage is widely recognized as a sign of immaturity. Internet users are not exposed to packet loss, TCP retries, or routing failures; database clients are not exposed to write contention resolution; payment card users are not exposed to interbank settlement failures.

Permissioned pull execution restores this separation.

Under a pull-based model, users authorize bounded outcomes through durable, revocable consent objects. Execution is performed by distinct actors, executors, who are economically incentivized to handle retries, optimize timing, batch operations, and absorb execution failures.

Failed executions continue to occur, but they no longer appear in the user’s interaction surface. Failure becomes an infrastructure concern rather than a user-facing event.

This relocation of failure handling produces a second-order effect: a dramatic reduction in the expertise required to participate efficiently in the system.

This phenomenon can be described as **expertise compression**; the systematic reduction of operational knowledge required at the user layer as abstractions mature.

From this perspective, the persistent requirement for blockchain users to understand execution failure is not an inherent property of decentralized systems, but an artifact of push-based execution semantics.

---

## 9. Discussion and Limitations

This model does not:

- Eliminate scarcity  
- Guarantee low fees  
- Remove MEV entirely  

Instead, it ensures scarcity impacts actors capable of managing it.

Adoption requires:

- Standardization of consent objects  
- Wallet-level UX changes  
- Executor competition and slashing  

This paper does not claim to fully specify executor economics, pricing models, or market equilibria. Its contribution is **diagnostic rather than prescriptive**.

While executor centralization is a legitimate concern, it differs structurally from custody. Executors do not take possession of assets, cannot exceed consent boundaries, and can be replaced without asset migration.

Centralization in execution services therefore represents **reversible coordination**, not custodial lock-in.

---

### 9.1 Executor Economics Are Explicit, Not Implicit

In push systems, users pay implicitly and chaotically via gas. In pull systems, users pay explicitly via service fees, spreads, subscriptions, or yield sharing.

This does not reintroduce gas exposure, it moves cost from per-action volatility to predictable pricing.

The question is not whether users ultimately pay for execution, but whether they are exposed to execution markets directly.

---

### 9.2 Why Push-Based Execution Became Dominant

Push-based execution was not a design mistake, but a **phase-optimal architecture**.

It offered:

- Conceptual simplicity  
- Clean cryptographic mapping  
- Synchronous composability  
- Low coordination overhead  
- Minimal trust assumptions  

In early environments with low congestion and expert users, these properties dominated.

As demand increased, congestion, adversarial dynamics, and heterogeneous users inverted these advantages into liabilities.

Push execution persists due to path dependence rather than enduring optimality.

Permissioned pull should therefore be understood not as a universal replacement, but as a corrective abstraction for systems operating under sustained demand and competitive execution environments.

---

## 10. Conclusion

Gas optimization failed not because it was insufficient, but because it addressed the wrong problem.

The persistent gas UX crisis is the result of a misassigned execution responsibility embedded in push-based transaction models.

By separating authorization from execution and routing execution cost to executors, permissioned pull corrects this misassignment.

The gas problem was never about price. It was about responsibility.

The contribution of this paper is intentionally diagnostic rather than prescriptive. While permissioned pull provides one concrete mechanism for correcting this misassignment, alternative architectures may exist.

The central claim is therefore not that permissioned pull is the only solution, but that **responsibility reassignment is the necessary axis of improvement**.
