# Delegated Execution via Explicit Authorization Objects

**Author:** Mats Heming Julner  
**Date:** January 2, 2025  
**Status:** Technical note

---

## Abstract

This note documents a simple but previously under-specified execution primitive: **delegated execution**.  
Delegated execution allows continuous automation to occur without transferring discretionary authority to the executing system.

We describe a concrete implementation of this primitive using **explicit authorization objects** that are time-bounded, revocable, and evaluated at the moment of execution. We show that this pattern has been demonstrated publicly in an adversarial financial setting (Continuum) and is directly extensible to non-financial actions (e.g. email sending) without modification to the core invariant.

This note makes no claims about intelligence, alignment, or optimal governance. It documents a narrow execution-governance mechanism and the properties it enforces.

---

## 1. Problem Statement

Modern automated systems typically rely on **credential possession** as a proxy for authority. If a system holds a credential (API key, OAuth token, private key), it is assumed to be authorized to act until the credential is revoked or expires.

This architecture exhibits a structural flaw:

> **Authority is implicit, static, and decoupled from execution time.**

As a result:
- Executors hold open-ended power.
- Revocation is coarse or delayed.
- Scope enforcement is best-effort.
- Continuous automation requires surrendering control.

These properties are acceptable for some applications, but become problematic in domains involving:
- irreversible outcomes
- long-running automation
- high incentives to misuse authority
- low tolerance for failure

Finance and autonomous agents both exhibit these characteristics.

---

## 2. Delegated Execution (Definition)

We define **delegated execution** as follows:

> **Delegated execution is a mode of automation in which an executor performs actions on behalf of a principal, while authority remains external, explicit, bounded, revocable, and verified at the moment of execution.**

Key properties:

1. **Authority is external**  
   The executor does not own authority; it only evaluates it.

2. **Authority is explicit**  
   Permission is represented as a concrete object, not an implicit configuration.

3. **Authority is bounded**  
   Scope, time, and quantity constraints are enforced.

4. **Authority is revocable**  
   Withdrawal of consent immediately halts execution.

5. **Authority is checked at execution time**  
   Possession of credentials alone is insufficient.

6. **Executors are non-discretionary**  
   They cannot reinterpret, extend, or persist authority.

Delegated execution is an execution-layer property, independent of how decisions are made.

---

## 3. Authorization Objects

The core mechanism enabling delegated execution is the **authorization object**.

An authorization object minimally encodes:
- the granting principal
- the executor it applies to
- the resource or action class
- scope constraints
- temporal validity
- a cryptographic signature

Importantly, the authorization object:
- is inspectable
- is verifiable
- can be evaluated independently
- can be revoked without trusting the executor

Authorization objects are **not credentials**. They do not grant possession-based power. They grant **conditional permission evaluated per action**.

---

## 4. Demonstration in Finance (Continuum)

The Continuum system publicly demonstrates delegated execution in an adversarial financial environment.

Notable properties:
- No executor holds custodial control over funds.
- Execution occurs only if a live authorization permits it.
- Revocation halts execution immediately.
- Automation continues without operator intervention.
- All execution is publicly verifiable.

This demonstrates that delegated execution is feasible even where incentives to bypass controls are high.

The significance of this demonstration is not financial per se, but architectural: **continuous automation does not require surrendering authority to the automator**.

---

## 5. Extension to Non-Financial Actions

The same execution pattern applies directly to non-financial actions.

To demonstrate this, we implement a minimal system (“ActionSafe.email”) that:
- holds the real execution capability (SMTP credentials)
- accepts signed authorization objects from a principal
- attempts execution continuously
- executes only if authorization permits
- fails closed otherwise
- logs all decisions publicly

No changes to the authorization model are required. Only the payload (email instead of value transfer) differs.

This confirms that delegated execution is **domain-agnostic** with respect to the action performed.

---

## 6. Relation to Autonomous Agents

This work does **not** address:
- agent intelligence
- planning
- incentives
- value alignment

However, it establishes a critical precondition for safe agent action:

> **An agent can only act if explicit, live authorization already permits the action.**

In this architecture:
- agents propose actions
- authorization governs execution
- executors remain mechanical

This separates **decision generation** from **execution authority**, preventing agents from accumulating power through persistence or access.

---

## 7. What This Note Does *Not* Claim

This note does **not** claim that:
- this is the only valid execution model
- all automation should use authorization objects
- human decisions are inherently safe
- alignment problems are solved
- governance is easy or universal

It documents a specific execution-governance mechanism and its demonstrated properties.

---

## 8. Conclusion

Delegated execution resolves a specific structural flaw common to both financial automation and autonomous systems: the coupling of execution with implicit authority.

By making authorization explicit, bounded, revocable, and evaluated at execution time, systems can automate continuously without surrendering control to the executor.

This note documents a working implementation of this pattern and its extension beyond finance. Further exploration is left to practitioners and critics.
