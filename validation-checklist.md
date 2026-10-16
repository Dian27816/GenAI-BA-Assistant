# My validation checklist (used after the AI validation prompt)

Tick each item per example. Write the fix in Part C of the example file.

**Facts**
- [ ] Every number is either sourced, marked [TBD], or labelled as an assumption
- [ ] No invented company, law or regulation names (check any that appear)
- [ ] Assumptions are labelled "ASSUMPTION"

**Requirements**
- [ ] Each requirement is a single, testable statement
- [ ] No vague words without a target (fast, easy, better, seamless)
- [ ] Non-functional requirements cover privacy/security where personal data is involved

**User stories**
- [ ] Each story names a real role and a benefit, not a feature
- [ ] No story is secretly two stories
- [ ] Priorities make sense (not everything "Must")

**Acceptance criteria**
- [ ] Each criterion has a clear pass/fail
- [ ] At least one edge or negative case per story

**KPIs and risks**
- [ ] Each KPI has formula, data source, baseline, target
- [ ] KPIs measure the objective (not vanity metrics)
- [ ] Every story traces to a requirement and a KPI

**Overall**
- [ ] Nothing contradicts anything else
- [ ] I can explain every line in an interview
