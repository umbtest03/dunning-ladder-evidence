# Dunning & AR Reminder Ladder — Frantic Bounty #60

## Summary

Published a governed dunning-ladder skill to the runx registry at codeboost-tr/dunning-ladder. The skill reads an overdue receivable and a cadence policy, decides the next reminder step within the cap, and emits a gated reminder-send proposal, escalating once the cadence cap is reached.

## What was done

- Created skills/dunning-ladder/ with SKILL.md, X.yaml, run.mjs, and fixture files
- Local harness passes: within-cap-reminder (sealed) + not-overdue (failure)
- Published to runx.ai registry: https://runx.ai/x/codeboost-tr/dunning-ladder
- Opened PR #137 against runxhq/runx with the skill package
- GitHub star on runxhq/runx: verified

## Skill details

- **Inputs:** invoice_status, aging_days, cadence_policy{steps, cap}
- **Outputs:** decision{step, action}, reminder_proposal{channel, content_digest}, escalation
- **Cap enforcement:** Never exceeds cadence cap, escalates rather than sending again
- **Runner:** cli-tool (node) with stdio-based input

## Artifacts

- **Public URL:** https://runx.ai/x/codeboost-tr/dunning-ladder
- **Source URL:** https://github.com/codeboost-tr/runx/tree/e583d83da68287e3c20c21c9c64274726b2e85d8/skills/dunning-ladder
- **PR URL:** https://github.com/runxhq/runx/pull/137
- **X.yaml:** https://raw.githubusercontent.com/codeboost-tr/runx/e583d83da68287e3c20c21c9c64274726b2e85d8/skills/dunning-ladder/X.yaml
- **SKILL.md:** https://raw.githubusercontent.com/codeboost-tr/runx/e583d83da68287e3c20c21c9c64274726b2e85d8/skills/dunning-ladder/SKILL.md
- **Evidence JSON:** https://raw.githubusercontent.com/umbtest03/dunning-ladder-evidence/main/evidence.json
- **Verification JSON:** https://raw.githubusercontent.com/umbtest03/dunning-ladder-evidence/main/verification.json
- **Receipt ref:** runx:receipt:sha256:51861f7d28b01578d25b3247e7f8bd701b5c474175daa27e77999d3970252378

## Usage

```bash
runx add codeboost-tr/dunning-ladder@sha-6b9208a2ce6f --registry https://api.runx.ai
runx skill codeboost-tr/dunning-ladder@sha-6b9208a2ce6f --registry https://api.runx.ai \
  --input-json cadence_policy='{"steps":[{"max_days":7,"channel":"email"}],"cap":3}' \
  -i invoice_status=overdue -i aging_days=15
```
