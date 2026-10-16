# Lessons: Fitness example (multi-activity membership)

Files: `gym-requirements-draft.md`, `gym-validation-report.md`, `gym-requirements-final.md`

## What worked
- The validation in a new chat caught a mistake in the KPI table. "Activity Variety Utilisation" counted how many activity categories were booked, not whether **members** try different activities. I replaced it with **Multi-Activity Adoption Rate**, which matches the original idea.
- It found a requirement with no owner: the draft assumed partner gyms get a revenue share, but nothing calculated payouts. I added FR20 and US7 (payout statements).
- It found no rule for how plan visit limits work across different gyms. I added FR21 and US8.
- It showed "Paying Members" had no clear counting rule, so I defined it (no free trials, no refunded purchases).

## What failed
- The AI draft treated "a multi-gym membership" as the chosen solution, but nothing showed customers want it or partners will join. I relabelled it as a **hypothesis to test**.
- All six user stories were marked "Must Have" with no prioritisation method.
- The review listed 34 gaps. Many were too detailed for this stage (time zones, emergency staff, offline check-in), so I had to choose which ones to act on and left the rest on purpose.
- The single-gym vs many-gyms question is still unresolved. I logged it as an open question with a pilot assumption.

## What I would change next
- Ask the client clarifying questions before drafting (single gym or many gyms?).
- Ask the validation prompt to rank findings by importance, so I do not have to sort 30+ points myself.
- Add customer and partner research before the requirements stage, because a business-model problem needs evidence first.
