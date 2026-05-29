# Ashish Birla Cursor Skills (Public)

Opinionated, workflow-aware Cursor Agent Skills for retail omnichannel product, program, and store operations work (OMS, inventory, RFID, fulfillment).

## Skills

- `portfolio-status-roll-up`: Synthesize messy team updates into weekly portfolio RAG, milestone deltas, accountability items, and SteerCo-ready status.
- `retail-product-requirements`: Convert a rough idea into a PRD with personas, workflow, edge cases, stories/AC, integrations, telemetry, KPIs, and rollout gates.
- `store-ops-edge-cases`: Stress-test a retail workflow for operational exceptions, inventory anomalies, misuse patterns, and customer-impact scenarios.

## Install (personal)

This repo is designed to be installed into your personal Cursor skills folder.

```bash
git clone https://github.com/<you>/ashish-birla-cursor-skills-public.git
mkdir -p ~/.cursor/skills
cd ashish-birla-cursor-skills-public
for d in skills/*/; do ln -sf "$(pwd)/$d" ~/.cursor/skills/"$(basename "$d")"; done
```

Verify:

```bash
ls -la ~/.cursor/skills
```

## Use

Ask Cursor to apply a specific skill by name, for example:

- "Use `portfolio-status-roll-up` on: [paste weekly team updates]"
- "Use `retail-product-requirements` on: Build RFID cycle counting feature"
- "Use `store-ops-edge-cases` on: BOPIS pickup workflow"

## Notes

- All examples use fictional program names and placeholder systems to keep the repo safe to share publicly.
- These are skills (instructions), not code libraries.

