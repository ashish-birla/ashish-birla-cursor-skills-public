# Examples - retail-product-requirements

All examples are fictional and safe to share publicly.

## Example 1: RFID cycle counting feature (abbreviated)

### Input

"Build an RFID cycle counting feature for stores."

### Output (abbreviated PRD)

1. Problem Statement  
Store inventory on-hand drifts from reality due to receiving errors, transfers, shrink, and missed system adjustments. Existing cycle counts are slow and inconsistent, causing replenishment misses and broken omnichannel promises.

2. Outcome and Non-Goals  
Outcome: Increase inventory accuracy for counted categories while reducing associate time per count.  
Non-goals: Financial inventory close process replacement; full-store annual inventory.

3. Personas  
- Store associate: completes cycle count during trading hours with minimal disruption.  
- Store manager: ensures compliance and resolves exceptions.  
- Inventory analyst: monitors exceptions and drives follow-up actions.

4. Happy-Path Workflow  
| Step | Actor | Action | System state change |
|------|-------|--------|---------------------|
| 1 | Manager | Schedules count for category/area | Count job created |
| 2 | Associate | Opens count on handheld | Job "in progress" |
| 3 | Associate | Scans area with RFID | Raw reads captured |
| 4 | System | Applies read filters and mapping | Read set normalized |
| 5 | System | Compares to expected on-hand | Exceptions computed |
| 6 | Associate | Resolves exceptions (search, rescan) | Exceptions updated |
| 7 | Manager | Approves and submits | Count completed; audit logged |

5. Edge Cases (sample)  
- `rfid`: tags present but not encoded/commissioned -> items appear as "unknown EPC" bucket. (L=M, I=M)  
- `inventory`: expected on-hand includes in-transit/reserved units -> false exceptions unless reservation model is stated. (L=H, I=M)  
- `associate`: count interrupted by customer support -> resume/timeout behavior needed. (L=H, I=L)  
- `integration`: delayed inventory feed causes mismatch between expected list and store reality. (L=M, I=M)

6. User Stories and Acceptance Criteria (sample)

Story: As a store associate, I can start an RFID cycle count for an assigned area so I can validate on-hand quickly.

AC:
- Given an assigned job, when I start it, then the device shows the target area and the list of expected items by category/SKU.
- Given I scan, when reads stream in, then the app shows progress and flags unknown EPCs separately.
- Operational AC: If the reader fails, associate can switch to manual scan mode and still complete a count with "lower confidence" label.

7. Integrations and APIs (sample)

- Device -> Count service: async read upload, idempotent batch upload by (jobId, batchId), retries with backoff.
- Count service -> Inventory service: fetch expected on-hand snapshot (include reservations/holds flags).

8. Telemetry (sample)

- `cycle_count_started` (jobId, storeId, areaId, deviceType)  
- `rfid_reads_uploaded` (jobId, readCount, unknownEpcCount, durationMs)  
- `cycle_count_exception_resolved` (jobId, exceptionType, resolutionType)

9. KPIs (sample)

| KPI | Definition | Formula/source | Target direction | Measurement lag |
|-----|------------|----------------|------------------|-----------------|
| Count time per area | Median time to complete a count job | app timestamps | down | same day |
| Exception rate | % counted items that become exceptions | count service | down | same day |
| Post-count accuracy | On-hand agreement vs follow-up validation | analytics | up | 1-7 days |

10. Rollout and Operational Readiness (sample)

- Pilot: 5 stores (high volume + low volume + known shrink risk) with trained champions.
- Gates: training complete, device firmware validated, hypercare schedule set.
- Hypercare: daily review of unknown EPC bucket; rollback if app crash rate > threshold.

11. Assumptions and Open Questions (sample)

Assumptions:
- A1: Stores have supported RFID handhelds and approved read power settings.
- A2: Expected on-hand snapshot includes reservation states.

Open questions:
1. Do we allow system on-hand adjustments automatically, or only after manager approval?
2. How are unknown EPCs resolved (lookup service, quarantine, or manual investigation)?

