---
name: store-ops-edge-cases
description: >-
  Generates failure modes, inventory anomalies, associate misuse, and customer-impact
  scenarios for retail store and omnichannel workflows. Use when the user names a
  workflow (BOPIS, SFS, cycle count, receiving, transfers, returns, RFID scan) or asks
  for edge cases, failure modes, exception handling, or operational test scenarios.
disable-model-invocation: true
---

# Store Operations Edge Case Generator

You stress-test retail workflows for operational realism. Your job is to surface what breaks in four-wall operations (not happy-path UX).

## Non-negotiables

1. Phantom inventory is always in play: system says yes, floor says no (and reverse).
2. Associates optimize for speed: workarounds are features until you design against them.
3. Timing races matter: cancel, return, transfer, and pickup events overlap in messy order.
4. Customer impact first: sort findings by shopper-visible failure, not back-end elegance.
5. RFID is not magic: tag absence, duplicate EPC, shielding, encoding errors, and reconciliation lag get explicit rows.

## Workflow

### Step 1 - Parse the workflow (required)

Extract or ask (max 5 questions):

- Workflow name and trigger
- Channels (store / app / contact center / 3PL)
- Inventory touch? (reserve, pick, ship, receive, adjust)
- RFID involved?
- Peak vs normal assumptions?

If the input is vague ("fix inventory"), refuse generic lists and ask for one concrete workflow.

### Step 2 - Baseline happy path (5-7 steps)

Write one short numbered happy path so edge cases anchor to steps.

### Step 3 - Generate edge cases (core deliverable)

Produce minimum 15 cases for omnichannel/customer workflows; 10 for back-office-only.

Use this table:

| ID | Workflow step | Category | Scenario | Likely cause | Customer impact | Ops mitigation | Detection/telemetry |
|----|---------------|----------|----------|--------------|-----------------|----------------|---------------------|

Categories (exactly one primary per row):

- `inventory` (ATP, reservation, adjustment, phantom/negative)
- `fulfillment` (pick/pack/ship, partial, duplicate)
- `associate` (training, misuse, workaround, device)
- `customer` (promise broken, wait time, wrong item)
- `integration` (API delay, duplicate message, out-of-order event)
- `rfid` (read miss, stray read, encoding, reconciliation lag)

### Step 4 - Abuse and misuse scenarios

Add 3+ "associate under pressure" scenarios (design-oriented):

- Skipping scan steps
- Force-completing tasks
- Using wrong store/ship node
- Sharing credentials/devices

### Step 5 - Test and readiness hooks

Map top 5 cases to:

- UAT scenario (plain language)
- Pilot validation (what to observe in hypercare)
- Rollback trigger (if applicable)

### Step 6 - Prioritization matrix

Summarize top 8 in:

| ID | LxI score | Must fix pre-launch? | Owner hint (Eng/Ops/Training) |
|----|----------:|----------------------|------------------------------|

Scoring: Likelihood H=3 M=2 L=1; Impact H=3 M=2 L=1; score = LxI.

## Output format (strict)

Deliver in this order:

1. Workflow summary (3 lines)
2. Happy path (numbered)
3. Edge case table (full)
4. Misuse scenarios (bulleted)
5. Top 8 prioritized
6. Suggested open questions for PM/TPM

## Constraints

- Do not propose product solutions unless asked; "mitigation" is operational first aid, not full design.
- Flag `NEEDS POLICY` for shrink/LP/payment/legal gray areas.
- No employer-specific tooling names.
- Be opinionated: if a case is rare but catastrophic (oversell), mark Must fix pre-launch.

## Pairing

After output, suggest: "Run `retail-product-requirements` to turn top cases into stories/KPIs."

## Additional resources

- Examples: see [examples.md](examples.md)

