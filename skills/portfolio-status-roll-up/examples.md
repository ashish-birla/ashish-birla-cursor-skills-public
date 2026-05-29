# Examples - portfolio-status-roll-up

All examples are fictional and safe to share publicly.

## Example 1: Team sync roll-up from messy paste (abbreviated)

### Input

```text
Week ending May 23.

Fulfillment: Still working on routing rules for Program-A. Dev says code done but QA blocked on test data. Target go-live June 15 maybe.

WMS: Green. Pick path optimization pilot started in 3 stores. Next gate is expand to 10 stores June 1.

Inventory: Yellow - reconciliation job failing nightly for 3PL-vendor-B feed. Eng investigating. Ops worried about ATP accuracy before cutover.

3PL: Vendor says API change delayed 2 weeks. We're still Green on our side per PM-J.

Data: Migration dry run moved to May 30. Need Ops sign-off on rollback script.
```

### Output (abbreviated)

#### 1. Portfolio snapshot

| Workstream | RAG | Milestone (next) | Target date | WoW delta | Top blocker |
|------------|-----|------------------|-------------|-----------|-------------|
| Fulfillment | Yellow | Program-A routing go-live | 2026-06-15 | AT RISK | QA blocked on test data |
| Warehouse / WMS | Green | Expand pick-path pilot to 10 stores | 2026-06-01 | UNCHANGED | None |
| Inventory | Yellow | 3PL feed reconciliation stable | TBD | NEW | Nightly job failures (3PL-vendor-B) |
| 3PL / MCF | Yellow | Vendor API change delivered | TBD | SLIPPED | Vendor delay ~2 weeks |
| Data / Integration | Yellow | Migration dry run | 2026-05-30 | UNCHANGED | Ops rollback sign-off pending |

**RAG mismatch flagged:** 3PL workstream claimed Green internally while vendor API slip affects Inventory and Fulfillment cutover.

#### 2. Week-over-week highlights

- WMS pick-path pilot live in 3 stores; expansion gate on track for June 1.
- Inventory reconciliation regressions new this week — ATP risk before cutover.
- Fulfillment go-live date at risk: Eng "code done" but QA blocked; June 15 not credible without test data fix.
- 3PL vendor API delay ~2 weeks — conflicts with internal Green status.
- Migration dry run scheduled May 30; Ops rollback sign-off still open.

#### 5. Accountability items

| # | Item | Owner | Due | Status | Escalation |
|---|------|-------|-----|--------|------------|
| 1 | Provide QA test data for Program-A routing | Fulfillment Eng | 2026-05-27 | BLOCKED | Yes — go-live at risk |
| 2 | Root-cause and fix plan for 3PL-vendor-B nightly reconciliation | Inventory Eng | 2026-05-28 | OPEN | Yes if no plan by due |
| 3 | Confirm revised vendor API date and downstream impact | 3PL PM | 2026-05-24 | OPEN | Yes — RAG mismatch |
| 4 | Ops sign-off on migration rollback script | Ops readiness | 2026-05-29 | OPEN | Yes — blocks dry run |

#### 6. Leadership asks

1. **3PL vendor slip:** Accept revised API date and push Fulfillment go-live, or escalate to vendor exec sponsor? *(Recommend: confirm date by May 24 before committing June 15.)*

---

## Example 2: SteerCo variant (same input, leadership audience)

### Output (abbreviated)

#### 1. Decisions needed

1. **Program-A go-live (Jun 15):** Proceed with date hold given QA blockage, or communicate slip now? *(Recommend: 1-week decision deadline — May 27 — pending test data.)*
2. **3PL vendor API delay:** Escalate to vendor exec sponsor or absorb slip across Inventory + Fulfillment? *(Recommend: escalate; internal Green status is stale.)*

#### 2. Portfolio RAG summary

| Workstream | RAG | Why |
|------------|-----|-----|
| Fulfillment | Yellow | Go-live at risk; QA blocked, no credible recovery dated |
| Inventory | Yellow | Nightly reconciliation failing; ATP accuracy before cutover |
| 3PL / MCF | Yellow | Vendor API delay ~2 weeks; not reflected in prior reporting |
| Data / Integration | Yellow | Dry run May 30; Ops rollback sign-off open |
| Warehouse / WMS | Green | Pilot on track; June 1 expansion gate achievable |

#### 3. Escalations

- **Fulfillment:** QA test data blocker threatens June 15 — no owner due date on fix until this roll-up.
- **3PL:** Vendor delay impacts Inventory reconciliation and Fulfillment routing — portfolio treated as Green incorrectly last week.

---

## Example 3: Status-request message (optional follow-up)

### Input

"Draft the message I send to workstream leads to collect next week's status."

### Output (sample)

```text
Subject: Weekly status — week ending May 30 (due Tue May 27 EOD)

Please reply with bullets under your workstream:

1. RAG (G/Y/R) and one-line why
2. Next milestone + target date + delta vs last week (on track / slipped / at risk / done)
3. Top blocker (owner + date to resolve)
4. Cross-team dependency you need from another workstream
5. Any leadership decision needed

Workstreams: Fulfillment, WMS, Inventory, 3PL, Data/Integration, Ops readiness

Keep to half a page. If Red/Yellow, include mitigation plan with dates.
```
