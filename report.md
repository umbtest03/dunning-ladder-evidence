# Dunning & AR Reminder Ladder — Frantic Bounty #60

## Summary

Published a governed dunning-ladder skill to the runx registry at codeboost-tr/dunning-ladder. The skill reads an overdue receivable and a cadence policy, decides the next reminder step within the cap, and escalates once the cap is exhausted.

## What was done

- Created skills/dunning-ladder/ with SKILL.md, X.yaml, run.mjs, and fixture files
- Two harness cases: within-cap-reminder (sealed, reminder sent via email) and cap-reached-escalate (failure, escalation to collections)
- Local harness passes both cases
- Published to runx.ai registry: https://runx.ai/x/codeboost-tr/dunning-ladder
- Opened PR #137 against runxhq/runx with the skill package
- Dogfood run with cap=1 confirms escalation path: exit code 1, stderr contains escalation JSON
- Receipt sealed and verified via `runx verify --receipt` (content_address + digest valid)
- GitHub star on runxhq/runx: verified

## Skill details

- **Inputs:** invoice_status, aging_days, cadence_policy{steps, cap}
- **Outputs:** decision{step, action}, reminder_proposal{channel, content_digest}, escalation{reason, recommended_action}
- **Cap enforcement:** When step_index >= cap, the skill exits with code 1, outputs escalation to stderr, and does NOT send a reminder
- **EScalation path:** `{decision: {action: "escalate"}, reminder_proposal: null, escalation: {recommended_action: "escalate to collections"}}`
- **Runner:** cli-tool (node) with stdio-based input

## Artifacts

- **Public URL:** https://runx.ai/x/codeboost-tr/dunning-ladder@sha-6b9208a2ce6f
- **Source URL:** https://github.com/codeboost-tr/runx/tree/f0673f6b/skills/dunning-ladder
- **PR URL:** https://github.com/runxhq/runx/pull/137
- **X.yaml:** https://raw.githubusercontent.com/codeboost-tr/runx/f0673f6b/skills/dunning-ladder/X.yaml
- **SKILL.md:** https://raw.githubusercontent.com/codeboost-tr/runx/f0673f6b/skills/dunning-ladder/SKILL.md
- **Evidence JSON:** https://raw.githubusercontent.com/umbtest03/dunning-ladder-evidence/main/evidence.json
- **Verification JSON:** https://raw.githubusercontent.com/umbtest03/dunning-ladder-evidence/main/verification.json
- **Receipt JSON:** https://raw.githubusercontent.com/umbtest03/dunning-ladder-evidence/main/receipt.json
- **Within-cap receipt:** https://raw.githubusercontent.com/umbtest03/dunning-ladder-evidence/main/receipt-within-cap.json
- **Receipt ref:** runx:receipt:sha256:1575dec13e9b20980a5cb4eeaf97b7facf5490c97e563dafad8622fd2de8e644

## Usage

```bash
runx add codeboost-tr/dunning-ladder@sha-6b9208a2ce6f --registry https://api.runx.ai
runx skill codeboost-tr/dunning-ladder@sha-6b9208a2ce6f --registry https://api.runx.ai \
  --input-json cadence_policy='{"steps":[{"max_days":7,"channel":"email"}],"cap":1}' \
  -i invoice_status=overdue -i aging_days=60
```
