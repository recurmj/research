# Continuum: Observations from a Live Experiment in Governed Automation Without Custody

**Author:** Mats Heming Julner  
**Status:** Experimental / Observational  
**Deployment:** Live system on Base Sepolia  
**Keywords:** permissioned pull, automation, time, consent, custody, on-chain systems

---

## Abstract

This paper documents observations from a live, continuously running on-chain system (“Continuum”) designed to explore whether automation, economic continuity, and fault tolerance can be achieved without custodial control or discretionary execution authority.

Rather than proposing a new financial model a priori, this work reports properties that *emerged unintentionally* from the system’s architecture during extended live operation, including repeated interruptions, restarts, and adversarial conditions.

Key observations include the separation of time advancement from action execution, the viability of automation without custody, tolerance to pauses without recovery logic, and the absence of timing advantage or execution races. These properties suggest a different design space for automated economic systems; one in which rules are primary, time is explicit, and action is strictly derivative.

---

## 1. Introduction

Most automated financial systems implicitly conflate three concepts:

1. **Time** (when something should happen)
2. **Action** (what happens)
3. **Authority** (who causes it to happen)

In practice, this coupling manifests as:
- cron jobs triggering payments,
- bots executing strategies,
- schedulers holding private keys,
- operators performing reconciliations after failure.

Such systems often require trust in executors, continuous uptime, and manual repair when interrupted.

Continuum was not initially designed to “solve” these issues. It was built to test a narrower question:

> Can value movement be automated *only* through explicit, revocable consent without granting any agent discretionary transfer authority?

The results of running this system live revealed several properties that were not fully anticipated at design time. This paper documents those observations.

---

## 2. System Overview

Continuum is a closed-loop on-chain economic system composed of:

- **Agents** (employer, workers, merchants, treasury, subscribers)
- **Permissioned pull authorizations** (bounded, revocable)
- **A public time advancement mechanism** (“tick”)
- **No push-based transfers**
- **No scheduler with authority**

### 2.1 Permissioned Pulls

All value movement occurs via permissioned pulls, where:
- the *grantor* authorizes a pull,
- the *grantee* executes it,
- bounds (amount, frequency, time window) are enforced on-chain.

No agent may transfer funds arbitrarily.

### 2.2 Time Advancement

Time advances discretely via a public function (`tickSafe()`):
- anyone may call it,
- it does not itself cause transfers,
- it only makes certain rules *eligible* to be evaluated.

If rules permit action, action may occur.  
If not, nothing happens.

---

## 3. Experimental Setup

- Deployed on Base Sepolia
- All contracts verified on-chain
- Runner publicly advances time at a fixed interval
- System observed continuously over many hours
- Includes:
  - runner crashes,
  - VPS reboots,
  - network interruptions,
  - rate limiting,
  - manual restarts.

No off-chain reconciliation or repair logic was used.

---

## 4. Observed Properties

The following properties were *not* explicitly targeted in the design, but emerged during live operation.

### 4.1 Separation of Time from Action

**Observation:**  
Time can advance without forcing any action.

Advancing a tick:
- does not execute transfers,
- does not trigger payments,
- only updates eligibility.

Action occurs *only if* rules already allow it.

This contrasts with traditional automation, where time *triggers* action.

In Continuum:
> Time enables evaluation; rules decide outcomes.

---

### 4.2 Automation Without Custody

**Observation:**  
The system operates without any agent holding discretionary custody.

In practice:
- intermediary agents (e.g., employer, treasury) frequently hold zero balance,
- funds pass through roles without being retained,
- no agent accumulates float or control.

This demonstrates that economic continuity does not require a custodian, only enforceable permissions.

---

### 4.3 Tolerance to Pauses Without Recovery Logic

**Observation:**  
The system tolerates pauses without requiring repair.

When the runner stops:
- time stops advancing,
- no state becomes inconsistent,
- no backlog accumulates,
- no reconciliation is required.

When the runner resumes:
- time continues from the last tick,
- rules resume evaluation,
- the system proceeds normally.

This is fault tolerance without recovery procedures.

---

### 4.4 Absence of Timing Advantage

**Observation:**  
No participant can act “faster” than another.

Because:
- time advances globally,
- rules are evaluated synchronously,
- execution authority is bounded.

There is:
- no race condition,
- no MEV window,
- no “right moment” to exploit,
- no advantage to anticipation.

Action is derivative, not competitive.

---

### 4.5 Intermediary Balance Convergence

**Observation:**  
Certain agents converge to zero balance as a stable outcome.

Notably, the employer role:
- pulls only what it is authorized to pull,
- forwards funds immediately,
- retains no balance.

This demonstrates that roles can exist purely as logical conduits, not economic actors.

---

## 5. Comparison to Existing Automation Models

| Model | Time | Action | Authority |
|-----|-----|-------|-----------|
| Cron-based | Implicit | Forced | Scheduler |
| Bot-driven | Implicit | Discretionary | Bot |
| Keeper networks | Competitive | Incentivized | Executor |
| Continuum | Explicit | Conditional | Rules |

Continuum does not replace these systems, but occupies a distinct design space.

---

## 6. Limitations

This experiment has clear limitations:

- Requires explicit time advancement
- Depends on well-defined rules
- Not suitable for all financial domains
- Does not optimize for throughput or latency
- Economic semantics remain human-defined

These observations should not be generalized beyond their context.

---

## 7. Implications and Open Questions

The experiment raises several questions:

- Can time itself be governed?
- Can automation be non-competitive by design?
- Which domains benefit from rule-first execution?
- Where does this model break down?

These remain open areas for exploration.

---

## 8. Conclusion

Continuum demonstrates that it is possible to construct a live, automated economic system where:

- time is explicit and discrete,
- rules are primary,
- action is strictly derivative,
- custody is unnecessary,
- pauses are tolerable,
- and timing advantage is eliminated.

These properties were not fully anticipated, but emerged through live operation.

Rather than presenting a finished model, this paper documents a set of observations that suggest a broader design space for governed automation; one in which systems adapt to reality, rather than requiring reality to adapt to hidden machinery.

---

## References

- ERC-8102: Permissioned Pull 
- ERC-8103: Permissioned Authorization Objects  
- Verified contracts and live dashboard available at: *pullbased.finance*
