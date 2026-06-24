# Dunning & AR Reminder Ladder — Frantic Bounty #60

## Summary

Published a governed dunning-ladder skill to the runx registry at umbtest03/dunning-ladder. The skill reads an overdue receivable and a cadence policy, decides the next reminder step within the cap, and escalates (exit 1) once the cap is exhausted.

## What was done

- Created skills/dunning-ladder/ with SKILL.md, X.yaml, run.mjs, and fixture files
- Two harness cases: within-cap-reminder (sealed, reminder sent via email) and cap-reached-escalate (failure, escalation to collections)
- Local harness passes both cases
- Published to runx.ai registry: https://runx.ai/x/umbtest03/dunning-ladder
- Opened PR #139 from umbtest03/runx against runxhq/runx with the skill package
- Post-publish dogfood run: sealed receipt captured and verified
- GitHub star on runxhq/runx: verified

## Skill details

- **Inputs:** invoice_status, aging_days, cadence_policy{steps, cap}
- **Outputs:** decision{step, action}, reminder_proposal{channel, content_digest}, escalation{reason, recommended_action}
- **Cap enforcement:** When step_index >= cap, the skill exits with code 1, outputs escalation to stderr, and does NOT send a reminder
- **Escalation path:** decision.action=escalate, reminder_proposal=null, escalation.recommended_action=escalate to collections
- **Runner:** cli-tool (node) with stdio-based input

## Artifacts

- **Public URL:** https://runx.ai/x/umbtest03/dunning-ladder@sha-5a9176c1fd75
- **Source URL:** https://github.com/umbtest03/runx/tree/f0673f6b/skills/dunning-ladder
- **PR URL:** https://github.com/runxhq/runx/pull/139
- **X.yaml:** https://raw.githubusercontent.com/umbtest03/runx/f0673f6b/skills/dunning-ladder/X.yaml
- **SKILL.md:** https://raw.githubusercontent.com/umbtest03/runx/f0673f6b/skills/dunning-ladder/SKILL.md
- **Evidence JSON:** https://raw.githubusercontent.com/umbtest03/dunning-ladder-evidence/main/evidence.json
- **Verification JSON:** https://raw.githubusercontent.com/umbtest03/dunning-ladder-evidence/main/verification.json
- **Receipt JSON:** https://raw.githubusercontent.com/umbtest03/dunning-ladder-evidence/main/receipt.json
- **Receipt ref:** runx:receipt:sha256:610b8d05021ac3ef321cf588251bec879295c918bef9aed1dd26cec756c1ac0e

## Usage

```bash
runx add umbtest03/dunning-ladder@sha-5a9176c1fd75 --registry https://api.runx.ai
runx skill umbtest03/dunning-ladder@sha-5a9176c1fd75 --registry https://api.runx.ai \
  --input-json cadence_policy='{"steps":[{"max_days":7,"channel":"email"}],"cap":1}' \
  -i invoice_status=overdue -i aging_days=60
```
