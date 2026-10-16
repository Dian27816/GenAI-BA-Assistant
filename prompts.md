# Prompt library

Use one chat per problem. Run the prompts in order. Paste the previous output where shown.
Replace `{PROBLEM}`. Tested with a general-purpose LLM chat.

## Shared header (put at the top of Prompt 1, or in the system/custom instructions)
```
ROLE: You are a senior business analyst at a technology consulting firm.
GUARDRAILS:
- Do not invent statistics, company names or regulations. If a number is needed, write [BASELINE TBD].
- Mark any assumption with "ASSUMPTION:".
- Use plain language. No vague words such as "fast", "easy", "better" without a measurable target.
- If the problem statement is unclear, list your questions first, then continue with stated assumptions.
```

## Prompt 1: Stakeholders
```
CONTEXT: A client reports this business problem: "{PROBLEM}"
TASK: Identify the stakeholders affected by or involved in solving it.
FORMAT: A table with columns: Stakeholder | Role | Main goal | Main concern | Influence (High/Med/Low).
Then list 3 clarifying questions I should ask the client.
```

## Prompt 2: Requirements
```
CONTEXT: Problem: "{PROBLEM}". Stakeholders: <paste Prompt 1 table>.
TASK: Write the business requirements.
FORMAT:
1. Business objective (one sentence, measurable)
2. In scope / Out of scope
3. Functional requirements (numbered FR1, FR2...; each testable)
4. Non-functional requirements (NFR1...: performance, security, privacy, usability, compliance)
5. Assumptions and constraints
```

## Prompt 3: User stories
```
CONTEXT: Requirements: <paste Prompt 2 output>.
TASK: Write 6 user stories covering the highest-value functional requirements.
FORMAT: For each: ID | "As a <role>, I want <capability>, so that <benefit>" | Priority (MoSCoW) | Linked requirement.
GUARDRAIL: Each story must describe a user need, not a technical solution.
```

## Prompt 4: Acceptance criteria
```
CONTEXT: User stories: <paste Prompt 3 output>.
TASK: Write acceptance criteria for each story.
FORMAT: Given / When / Then. 2-3 criteria per story, including one negative or edge case.
GUARDRAIL: Every criterion must be verifiable by a tester with a clear pass/fail result.
```

## Prompt 5: KPIs and risks
```
CONTEXT: Objective and requirements: <paste>.
TASK: Propose success KPIs and project risks.
FORMAT:
- KPI table: KPI | Formula | Data source | Baseline | Target | Review frequency
  (Baseline and target = [TBD] unless I supply them.)
- Risk table: Risk | Likelihood | Impact | Mitigation
```

## Prompt 6: Validation (run in a NEW chat so it does not defend its own work)
```
ROLE: You are a skeptical reviewer of business analysis documents.
INPUT: <paste the full output of Prompts 1-5>
TASK: Review it and report:
1. Gaps (missing stakeholders, requirements, edge cases)
2. Contradictions (between requirements, stories, criteria, KPIs)
3. Hallucination risks (specific numbers, names, regulations, claims stated as fact without a source)
4. Vague or untestable wording (quote it, then suggest a measurable rewrite)
5. Traceability: for each user story, name the requirement and KPI it supports. Flag orphans.
FORMAT: A numbered list per section. Do NOT rewrite the whole document.
```
