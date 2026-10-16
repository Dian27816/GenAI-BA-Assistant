# Lessons: Retail example (slow-moving inventory)

Files: `retail-requirements-draft.md`, `retail-validation-report.md`, `retail-requirements-final.md`

## What worked
- Splitting the work into stages (stakeholders, requirements, user stories, acceptance criteria, KPIs) gave cleaner output than one big prompt.
- Running the validation in a **new chat** found real problems the drafting chat missed.
- The "[TBD]" guardrail stopped the AI inventing baseline numbers.
- Validation showed a Must requirement (recommendations, FR9) had **no user story**. I added US7 and its acceptance criteria.
- Validation showed the 20% stock reduction could look successful through write-offs alone. I added a **success rule** with margin and availability guardrails.
- It flagged that inventory age is ambiguous when a product arrives in several batches. I defined age at batch level and logged the method as an open question for the client.

## What failed
- The AI draft was very long (12 functional requirements, 10 non-functional requirements, 10 KPIs). Longer was not clearer, and I had to prioritise.
- The validation over-reported small issues (for example the Given/When/Then formatting point), so I had to decide which findings mattered. I left minor ones unchanged on purpose.
- The 20% reduction target and the 5-second load time had no data behind them. I relabelled them as proposed targets.
- Even the fixed version depends on client decisions (slow-moving thresholds, ageing method, tolerances) that AI cannot supply.

## What I would change next
- Ask the AI to keep the first draft shorter (for example, at most 6 requirements) so it is easier to review.
- Add a prompt that asks the client clarifying questions before drafting.
- Loop the workflow: re-run validation on the fixed version until no major gaps remain.
