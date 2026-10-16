# GenAI Business Analyst Assistant

A small, documented **prompt workflow** that turns a plain-language business problem into BA artefacts
(stakeholders, requirements, user stories, acceptance criteria, KPIs, risks) and then **validates its own output**.

It is not an app. It is a repeatable method, tested on 2 problems (retail inventory, fitness membership).

## Why I built it
LLMs are fast at drafting BA documents but they invent facts, write vague requirements and contradict themselves.
So the workflow has two rules:
1. Every stage has a structured prompt (role, context, task, format, guardrails).
2. Nothing is final until a **validation step** and my own review have checked it.

## Workflow

```
 Business problem (plain words)
          |
          v
 1. Stakeholders  -->  2. Requirements  -->  3. User stories
                                                   |
                                                   v
 6. Validation  <--  5. KPIs + risks  <--  4. Acceptance criteria
          |
          v
 Human review: fix, tighten, log what changed  -->  lessons.md
```

| Stage | Output | Main failure I watch for |
|---|---|---|
| 1 Stakeholders | Who is affected, goals, concerns | Missing a stakeholder (e.g. compliance) |
| 2 Requirements | Objective, functional, non-functional | Vague words ("fast", "user-friendly") |
| 3 User stories | As a / I want / so that | Stories that are really solutions or tasks |
| 4 Acceptance criteria | Given / When / Then | Criteria that cannot be tested |
| 5 KPIs + risks | Measurable metrics, risks | KPIs with no baseline or formula |
| 6 Validation | Gap, contradiction, hallucination report | Reviewer repeating the author's mistakes |

## Repo contents
- `prompts.md`: the reusable prompts (copy, paste, replace `{PROBLEM}`)
- `validation-checklist.md`: my own manual checklist
- `retail-requirements-draft.md`, `retail-validation-report.md`, `retail-requirements-final.md`: retail example (draft, review, fixed version)
- `gym-requirements-draft.md`, `gym-validation-report.md`, `gym-requirements-final.md`: gym example (draft, review, fixed version)
- - `lessons Summary.md`: summary of what worked and failed, with separate notes in `lessons-retail.md` and `lessons-gym.md`

## How to read an example
Each file has three parts: **A. AI draft**, **B. Validation (AI + checklist)**, **C. My edits**.
Part C is what I changed by hand and why. That is the evidence that I did not treat AI output as final.

## 60-second explanation
"I split the BA workflow into stakeholder identification, requirement extraction, user stories, acceptance criteria
and KPIs. Each stage is a structured prompt that takes the previous stage's output. A final validation prompt, plus my
own checklist, looks for gaps, contradictions and invented facts. I then correct the output by hand and log the changes."

## Limits
- Output quality depends on how precise the problem statement is.
- The LLM has no access to a real client's data, so all numbers are placeholders until validated.
- This is a single-pass chain; a fuller agent would loop until validation passes.
