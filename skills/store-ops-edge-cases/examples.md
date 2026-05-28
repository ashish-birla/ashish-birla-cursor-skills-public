# Examples - store-ops-edge-cases

All examples are fictional and safe to share publicly.

## Example 1: BOPIS pickup workflow

### Input

"BOPIS pickup workflow."

### Output (abbreviated)

1. Workflow summary  
Customer places an order for pickup; store picks and stages it; customer arrives and completes pickup. Inventory is reserved/held and must reconcile with store reality under time pressure.

2. Happy path  
1) Customer places BOPIS order  
2) System allocates to store and reserves inventory  
3) Associate picks items  
4) Associate stages in pickup area  
5) Customer arrives; associate verifies and completes pickup  
6) Inventory decrements and order closes

3. Edge case table (sample rows)

| ID | Workflow step | Category | Scenario | Likely cause | Customer impact | Ops mitigation | Detection/telemetry |
|----|---------------|----------|----------|--------------|-----------------|----------------|---------------------|
| EC-01 | 2 | inventory | Reservation created but item not actually on floor | phantom inventory | pickup delayed/cancelled | substitute item policy or cancel with apology | high "pick fail" rate by SKU |
| EC-02 | 3 | fulfillment | Partial pick (1 of 2 units) | low stock / mispick | customer gets incomplete order | offer split pickup or partial cancel | `pick_partial` events |
| EC-03 | 3 | associate | Associate marks picked without physically confirming | speed workaround | customer arrives and item missing | manager audit + retrain | mismatch: `picked` then `cannot_find` |
| EC-04 | 4 | customer | Customer arrives before staging complete | early arrival | wait time, frustration | dedicated pickup queue + comms | arrival-before-ready rate |
| EC-05 | 5 | customer | Wrong customer claims order | weak verification | fraud / wrong item | ID/OTP verification | overrides logged |
| EC-06 | 5 | integration | Pickup completion event delayed | network/API lag | inventory still held; future oversell | manual close fallback | completion latency metric |
| EC-07 | 2 | integration | Duplicate allocation messages | retry without idempotency | double reservation / oversell | idempotency key | duplicate allocation detection |
| EC-08 | 6 | inventory | Cancel arrives after pickup completion | race condition | refund/charge confusion | reconciliation job | cancel-after-close count |
| EC-09 | 3 | rfid | RFID indicates item in store but not on shelf | stray read / shielding | pick fail persists | search protocol; flag EPC | EPC seen but not resolved |
| EC-10 | 3 | rfid | Duplicate EPC appears across two items | encoding/commissioning issue | wrong item picked | quarantine + re-tag | duplicate EPC detector |
| EC-11 | 4 | associate | Staged item placed in wrong bin | process lapse | long retrieval time | bin labeling + scan-to-bin | stage-to-bin scan rate |
| EC-12 | 5 | customer | Customer no-show; hold expires | behavior pattern | inventory locked too long | auto-unreserve + restock task | hold-expired backlog |

4. Misuse scenarios

- Associate "force completes" pickup to clear queue, planning to fix later.
- Associate uses a coworker's login on a shared device, blurring audit trails.
- Associate picks from sales floor but stages in unsecured area, increasing misplacement/shrink risk.

5. Top 8 prioritized (illustrative)

| ID | LxI score | Must fix pre-launch? | Owner hint (Eng/Ops/Training) |
|----|----------:|----------------------|------------------------------|
| EC-01 | 9 | Yes | Eng + Ops |
| EC-05 | 6 | Yes | Ops + Training |
| EC-06 | 6 | Yes | Eng |
| EC-08 | 6 | Yes | Eng |
| EC-11 | 4 | No | Ops |
| EC-02 | 4 | No | Eng |
| EC-12 | 4 | No | Ops |
| EC-07 | 3 | Yes | Eng |

6. Open questions for PM/TPM

1) What is the substitution / partial fulfillment policy by category?  
2) What is the allowed hold time and customer comms sequence?  
3) What verification standard is required (ID, OTP, signature)?  
4) What is the inventory authority for reservation conflicts (and audit requirements)?

