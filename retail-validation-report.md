> Prompt 6 output, run in a new chat. Input was the full draft in `retail-requirements-draft.md`.

# Skeptical BA Review: Slow-Moving Inventory in Retail

Overall assessment: The document is a solid draft with good structure, useful negative test cases, and clear separation between proposed targets and client-approved requirements. However, it is not yet fully implementation-ready. Its biggest weaknesses are incomplete traceability, undefined calculation rules, gaps between functional requirements and user stories, and insufficiently measurable acceptance criteria.

The review below is based on the supplied document. I have distinguished genuine contradictions from missing details and assumptions that still require client validation.

## 1. Gaps — missing stakeholders, requirements, and edge cases

1\. Missing or underrepresented stakeholders

- IT / Data Engineering: FR1, FR12 and NFR2 depend on data integration, refresh scheduling, and failure handling, but IT is not explicitly included in the user stories.

- Merchandising / Category Managers: They may own assortment decisions, markdowns, and category-level stock actions. Their involvement is not established.

- E-commerce / Omnichannel Operations: The stakeholder list does not clarify whether online sales, warehouse stock, and store stock are included.

- Data Governance / Security: NFR3–NFR5 and NFR9 require access, privacy, and compliance decisions, but no accountable stakeholder is named.

- Store-level decision-makers: Store Managers appear in the stakeholder analysis but have no corresponding user story or explicit reporting needs.

Why it matters: The document identifies stakeholders who may be affected by the solution but does not assign clear ownership for data quality, security approval, or executing recommendations.

2\. No explicit requirement for data reconciliation and duplicate handling.

FR1 requires missing and invalid records to be reported, but the document does not specify how to handle duplicate transactions, duplicate product identifiers, conflicting stock quantities, cancelled sales, returns, or negative inventory.

Suggested addition: Define validation rules and expected system behaviour for each identified data-quality condition, including whether affected records are rejected, quarantined, or included with a warning.

3\. Insufficient handling of inventory batches and replenishment.

The KPI notes acknowledge that inventory age becomes difficult when a product is received in multiple batches (lines 344–347), but there is no agreed method for calculating age.

For example, a product may have 100 units received 120 days ago and 50 units received 10 days ago. Treating all 150 units as 10 days old would hide aged inventory.

Suggested addition: Define whether age is calculated at SKU level or batch level and how the system handles partial sales, returns, and replenishment. Confirm whether FIFO, another costing/stock-flow method, or a separate age-bucketing rule is appropriate.

4\. No detailed corrective-action workflow.

FR9 recommends discounts, promotions, transfers, and reduced purchasing, but the requirements do not specify:

- Who receives each recommendation.

- Who approves or rejects it.

- Whether the decision and reason are recorded.

- How overdue recommendations are escalated.

- How the system tracks whether an action actually reduced slow-moving stock.

Suggested addition: Define recommendation ownership, approval status, action tracking, and outcome measurement. Keep automated execution out of scope unless explicitly approved.

5\. Insufficient edge cases in acceptance criteria.

Existing criteria cover missing data and missing baselines well, but several important cases are absent:

- Zero baseline inventory value, making percentage reduction undefined.

- Zero previous-period sales, making sales growth undefined.

- New products with insufficient sales history.

- Discontinued products and seasonal products.

- Stock held across multiple stores or warehouses.

- Returns, transfers, stock adjustments, and negative stock.

- A failed refresh after a previously successful refresh.

- Users without export permissions.

- Products with missing cost or selling-price data.

- A reduction in inventory value caused by markdowns or write-offs rather than genuine stock movement.

Suggested addition: Add Given/When/Then cases for the relevant scenarios and specify the expected result instead of leaving behaviour to developer interpretation.

6\. No explicit audit trail or change history.

NFR3 covers access control, but the document does not establish whether changes to thresholds, business rules, approvals, or recommendations must be logged.

Suggested addition: Confirm audit-log requirements, including which changes are recorded, who can view them, and how long records must be retained.

7\. No agreed data retention, recovery, or availability targets.

NFR7 covers refresh failures, and NFR9 refers to retention requirements, but there are no defined recovery time, recovery point, dashboard availability, or retention targets.

Suggested addition: Obtain client-approved operational targets and define what happens when the system or a source data feed remains unavailable for an extended period.

8\. The business outcome is not fully operationalised.

The document proposes a 20% reduction in slow-moving inventory value within six months, but does not define:

- The baseline date and measurement window.

- Whether the target applies to the entire retailer or selected categories/stores.

- Whether the same product population must be used in both periods.

- How new products, discontinued products, write-offs, and changes in inventory valuation are treated.

- The acceptable deterioration, if any, in gross profit margin and product availability.

Suggested addition: Approve a baseline and measurement protocol before development and testing. Define guardrails for availability and margin so that simply writing off stock or stopping purchases cannot falsely appear to demonstrate success.

## 2. Contradictions and inconsistencies

1\. Business objective versus KPI targets — partially inconsistent.

- Evidence: Lines 54–56 propose reducing slow-moving inventory value by at least 20% within six months. Lines 328–343 mark the KPI baselines and targets as \[TBD\].

- Assessment: This is not necessarily a contradiction because the 20% target is explicitly provisional. However, the KPI table does not operationally identify which KPI must achieve 20% or how its baseline will be fixed.

- Fix: Label the 20% reduction as a provisional business objective and explicitly link it to KPI 2, with an approved baseline, measurement window, and scope.

2\. Protecting gross profit margins versus the absence of a margin guardrail.

- Evidence: The business objective at line 54 requires protecting gross profit margins. FR9 at line 84 allows discounts and promotions, while KPI 7 at line 336 measures gross profit margin without specifying an acceptable minimum.

- Assessment: The requirements promise a business outcome without defining the conditions under which it is preserved.

- Fix: Ask the client to define the permitted margin change or minimum margin threshold. Test recommendations against the approved rule.

3\. Maintaining product availability versus the absence of a measurable constraint.

- Evidence: Lines 54 and 127 require maintaining availability or avoiding unacceptable stockouts. KPI 9 measures product availability but has no approved target.

- Assessment: The solution cannot objectively determine whether inventory reduction has harmed availability.

- Fix: Define the availability metric, its target, measurement period, and any category-specific exceptions.

4\. FR9 requires recommendations, but no user story implements FR9.

- Evidence: FR9 at line 84 requires corrective-action recommendations. US1–US6 at lines 156–161 do not explicitly request recommendations, and none links to FR9.

- Assessment: This is a traceability gap, not a direct contradiction. A Must Have business capability is not represented in the user-story set.

- Fix: Add a recommendation-focused user story or explicitly defer FR9 and revise the scope and priority accordingly.

5\. FR10 and FR12 have no corresponding user stories.

- Evidence: FR10 requires report exports; FR12 requires the latest successful refresh timestamp and failure indication. Neither is linked to US1–US6.

- Assessment: These requirements could be implemented without appearing in the current story-level completion criteria.

- Fix: Create user stories for export and data freshness, or identify them as separately managed technical requirements with their own acceptance tests.

6\. The document says every criterion follows Given/When/Then, but not all criteria consistently do so.

- Evidence: The introduction at line 175 states that each criterion follows Given/When/Then. The six user stories largely follow that format, but this format is not applied consistently to the FR1–FR12 and NFR1–NFR10 test descriptions.

- Assessment: The claim is broader than the implementation. The functional and non-functional requirement tests are mostly concise test statements rather than full Given/When/Then criteria.

- Fix: Either restrict the claim to the user-story acceptance criteria or standardise the format across all requirement-level criteria.

7\. Inventory reduction can appear successful without actual operational improvement.

- Evidence: KPI 2 measures the percentage reduction in slow-moving inventory value (line 331). KPI 7 measures gross profit margin, while KPI 9 measures product availability (lines 336–338).

- Assessment: The document recognises this risk in its overall success measure at line 388, but does not specify how the three measures are combined into a pass/fail decision. A markdown or write-off may reduce the measured inventory value while hurting profitability.

- Fix: Define an explicit success rule requiring the approved inventory reduction target to be met alongside agreed margin and availability guardrails.

8\. Inventory age definition is not aligned with the inventory valuation and movement model.

- Evidence: KPI 5 defines age as current date minus the date inventory was received (line 334), while the measurement notes acknowledge multiple replenishment batches (line 347).

- Assessment: The simple definition is ambiguous for mixed-age stock. It may not represent the age of the units that remain unsold.

- Fix: Specify the stock-flow or batch-ageing method and verify the calculation using a product with multiple receipts and partial sales.

9\. Scope excludes automated actions, but FR9 does not define the boundary between recommendation and execution.

- Evidence: Line 66 excludes automatically changing prices or placing purchase orders without approval. FR9 allows the system to recommend discounts and reduced purchasing (line 84).

- Assessment: These statements can coexist, but the system's permitted actions are not explicitly defined.

- Fix: State that the system may generate recommendations but cannot change prices, create orders, or execute transfers without an authorised approval workflow.

## 3. Hallucination risks and unsupported claims

Here, “hallucination risk” means an assertion that could be mistaken for a verified client fact when the document does not provide evidence for it. It does not mean the assertion is necessarily false.

1\. The 20% reduction target and six-month deadline.

- Evidence: Lines 54–56.

- Risk: No historical data, financial model, or client approval supports the target.

- Status: Correctly identified as proposed, but must not be presented as an agreed commitment.

2\. The numerical dashboard performance target.

- Evidence: NFR1, lines 95–106, requires a load time of five seconds and a 95% query-success threshold.

- Risk: There is no workload definition, dataset size, concurrency assumption, or documented performance benchmark.

- Fix: Validate the target through stakeholder agreement and performance testing under a defined workload. Do not present it as an existing system capability.

3\. Likelihood and impact ratings in the project risk table.

- Evidence: Lines 351–365.

- Risk: Ratings such as “High” and “Medium” are qualitative judgments, not measured probabilities or documented client assessments.

- Status: The document correctly calls them initial assessments at line 352. Preserve that qualification in any presentation.

4\. Stakeholder influence classifications.

- Evidence: Lines 9–22.

- Risk: Assigning “High” influence to Procurement, Finance, or Sales and “Medium” to customers and suppliers may be reasonable, but it has not been validated against the actual organisation.

- Fix: Confirm decision rights and organisational responsibilities with the client.

5\. Claims about the causes and consequences of slow-moving stock.

- Evidence: Lines 5, 42, and 48.

- Risk: Storage costs, blocked working capital, markdowns, seasonality, competition, and weak marketing are plausible explanations, not established causes for this retailer.

- Fix: Present these as hypotheses to investigate rather than confirmed root causes.

6\. Data availability and integration assumptions.

- Evidence: Lines 112–118 and 123–126.

- Risk: The document assumes historical data can be supplied and linked through sufficiently consistent product identifiers, but there is no source-system inventory, data dictionary, sample extract, or data-quality assessment.

- Fix: Validate available fields, historical coverage, source ownership, refresh mechanisms, and data quality before committing to FR1–FR4.

7\. Regulatory and security claims are underspecified.

- Evidence: NFR4, NFR5, NFR9 and constraints 8 at lines 98–103 and 129–130.

- Risk: “Approved security controls,” “applicable laws,” and “record-retention requirements” do not identify the actual standards or obligations relevant to the retailer.

- Fix: Have the client’s security, privacy, and legal representatives identify the applicable requirements. Do not claim compliance until it has been assessed and verified.

8\. The KPI formulas are proposed definitions, not necessarily the retailer's approved accounting definitions.

- Evidence: Lines 330–347.

- Risk: Turnover, days on hand, inventory value, and gross profit margin depend on agreed costing, timing, returns, and net-sales definitions. The formulas alone do not establish which conventions the retailer uses.

- Fix: Obtain Finance approval of the metric definitions and reconcile sample outputs to the source records.

9\. The inventory-age formula may overstate certainty.

- Evidence: Line 334.

- Risk: The formula is straightforward, but assigning a single receipt date to an SKU with multiple batches may misrepresent the age of remaining stock.

- Fix: Treat the formula as provisional until the stock-flow method is approved.

10\. No external evidence is supplied for the stated targets, ratings, or company-specific facts.

- Assessment: The document is primarily a proposed requirements artefact, not a researched industry benchmark report. It does not cite external studies or actual client records.

- Fix: External sources are not required for every proposed business rule, but industry benchmarks or regulatory claims should be cited if they are later introduced as factual justification.

## 4. Vague or untestable wording — with measurable rewrites

The following are the highest-priority examples. The suggested rewrites are templates; values marked as client-approved must be agreed before testing.

1\. “The system successfully imports the agreed datasets and reports missing or invalid records.” — FR1, line 76.

Problem: “Successfully” and “invalid” are undefined. No completeness threshold or record-level behaviour is specified.

Suggested rewrite: “Given an agreed source dataset containing valid, missing, and invalid records, when an import runs, then the system shall report the number of records received, accepted, rejected, and flagged for review, with a reason for each rejected or flagged record.”

2\. “The system shall identify slow-moving and non-moving inventory using client-approved thresholds.” — FR3, line 78.

Problem: The thresholds, measurement window, and classification rules are unspecified.

Suggested rewrite: “Given an approved classification rule for each applicable product category, when the report runs, then every eligible product shall be classified according to its recorded sales activity during the approved window, with products having insufficient data flagged for review.”

3\. “Calculated values match independently verified results for a sample of products.” — FR4, line 79.

Problem: Neither sample size nor acceptable calculation tolerance is stated.

Suggested rewrite: “Given an approved test dataset of \[N\] products and independently calculated expected results, when the system calculates inventory age, turnover, and days on hand, then each result shall match the approved calculation within the client-defined tolerance.”

4\. “The report flags products that meet the agreed excess-stock criteria.” — FR6, line 81.

Problem: “Excess stock” lacks a defined formula and does not distinguish high stock from strategically necessary safety stock.

Suggested rewrite: “Given approved excess-stock rules and complete stock and sales data, when the report is generated, then each product meeting those rules shall be flagged with the calculated measure and threshold that triggered the flag.”

5\. “The system shall recommend potential corrective actions … based on agreed business rules.” — FR9, line 84.

Problem: There is no test for recommendation eligibility, suitability, margin impact, or missing inputs.

Suggested rewrite: “Given an approved recommendation rule and all required input data, when an eligible product is flagged, then the system shall display the applicable recommendation, the rule that triggered it, and any required approval. When required data is unavailable, it shall request review rather than issue an unsupported recommendation.”

6\. “The dashboard shall load within five seconds for standard queries under the agreed normal workload.” — NFR1, line 95.

Problem: “Standard queries” and “normal workload” are undefined.

Suggested rewrite: “Under the approved test workload of \[dataset size\], \[concurrent users\], and \[specified queries\], at least 95% of dashboard requests shall complete within five seconds during the defined performance test.”

7\. “The system shall refresh inventory and sales data at the frequency agreed with the client.” — NFR2, line 96.

Problem: There is no agreed frequency, completion deadline, or freshness expectation.

Suggested rewrite: “The system shall attempt refreshes every \[approved interval\], complete each successful refresh within \[approved duration\], and display a warning when a refresh fails or the data exceeds the approved freshness limit.”

8\. “Representative business users can complete agreed tasks using the dashboard without specialist technical assistance.” — NFR6, line 100.

Problem: “Representative,” “agreed tasks,” and “without specialist assistance” cannot be tested without a defined protocol.

Suggested rewrite: “At least \[approved percentage\] of \[approved number\] representative users shall complete each of \[specified tasks\] without assistance during user acceptance testing.”

9\. “Reconciliation against approved source records falls within the agreed tolerance.” — NFR8, line 102.

Problem: The tolerance and reconciliation method are missing.

Suggested rewrite: “For each approved test case, the reported inventory quantities and values shall reconcile with independently calculated source-record results within the approved tolerance of \[value and unit\].”

10\. “Within six months of implementation, reduce the value of slow-moving inventory by at least 20% compared with the baseline.” — Business objective, line 54.

Problem: The measurement start date, inventory population, baseline method, and exclusions are not defined.

Suggested rewrite: “Within six calendar months of \[approved start date\], reduce the value of inventory classified as slow-moving by at least 20% relative to the approved baseline valuation as of \[baseline date\], using the same approved classification and valuation rules at both measurement points, subject to the agreed gross-margin and product-availability guardrails.”

11\. “The system shall allow authorised users to export reports for further analysis and management review.” — FR10, line 85.

Problem: “Authorised” and the supported export formats are not finalised. The examples Excel and CSV are not necessarily approved requirements.

Suggested rewrite: “Given a user with the approved export permission, when the user exports a filtered report, then the system shall generate a file in each configured supported format containing the selected filters and the expected report data. A user without export permission shall be denied the action.”

12\. “The system shall comply with applicable data protection, information security, and record-retention requirements.” — NFR9, line 103.

Problem: The applicable obligations, owners, and verification evidence are not named.

Suggested rewrite: “Before release, the designated compliance owner shall verify the documented controls against the client-approved list of applicable legal, security, privacy, and retention requirements, with each requirement recorded as passed, failed, or requiring remediation.”

13\. “The report displays sales trends for the selected period.” — US5, AC5.1, line 272.

Problem: “Sales trends” could mean units, revenue, growth, moving averages, or another measure.

Suggested rewrite: “When a user selects a product or category and date range, the report shall display sales quantity and net sales value by the configured reporting interval, using the approved treatment of returns and cancelled transactions.”

14\. “The report displays baseline value, current slow-moving inventory value, absolute reduction, percentage reduction, and target.” — US6, AC6.1, line 295.

Problem: The output is clear, but the criterion does not define what happens if the baseline is zero or the inventory valuation rules change.

Suggested rewrite: “When a valid positive baseline and current valuation are available under the same approved valuation rules, the report shall display both values and calculate absolute and percentage reduction. When the baseline is zero or invalid, the percentage reduction shall be displayed as unavailable with an explanatory message.”

15\. “The dashboard shall load within five seconds” and “at least 95% of standard queries.” — NFR1, line 95.

Problem: Even with the proposed rewrite above, the measurement environment, test duration, and whether failed requests count in the denominator need agreement.

Suggested rewrite: Define the test environment, run duration, request population, percentile calculation, and failure treatment in the performance-test specification.

## 5. Traceability — user stories, requirements, and KPIs

The table below maps each user story to its explicitly linked functional requirement and the KPIs it can reasonably support. The distinction is important: a KPI being relevant to a story does not mean that the story directly measures or achieves that KPI.

| **User story**                                     | **Linked requirement** | **Relevant KPI(s)**                                                                                                                 | **Traceability assessment**                                                                                                                                                     |
|----------------------------------------------------|------------------------|-------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| US1 — Identify slow-moving and non-moving products | FR3                    | KPI 1: Slow-Moving Inventory Value; KPI 2: Slow-Moving Inventory Reduction                                                          | Direct requirement link. Classification is foundational to both KPIs, but the story does not define valuation or reduction measurement.                                         |
| US2 — View key inventory metrics                   | FR8                    | KPI 1: Slow-Moving Inventory Value; KPI 3: Inventory Turnover Ratio; KPI 4: Days of Inventory on Hand; KPI 5: Inventory Age         | Direct requirement link. The story covers these metrics, but not KPI calculation definitions or data-quality rules.                                                             |
| US3 — Compare sales performance                    | FR5                    | KPI 6: Sales Growth                                                                                                                 | Direct requirement link. The story supports comparisons, but KPI 6's growth formula and zero-sales handling must be consistent with the report.                                 |
| US4 — Identify products with excess inventory      | FR6                    | KPI 8: Excess Inventory Value; potentially KPI 9: Product Availability                                                              | Direct requirement link. The story supports excess-stock identification. Availability is a guardrail requiring separate measurement and is not implemented by this story alone. |
| US5 — Analyse historical sales trends              | FR7                    | KPI 6: Sales Growth; indirectly KPI 1 and KPI 2                                                                                     | Direct requirement link. Trend analysis may help explain slow movement, but the story does not itself reduce stock or prove causation.                                          |
| US6 — Track inventory reduction targets            | FR11                   | KPI 2: Slow-Moving Inventory Reduction; KPI 1: Slow-Moving Inventory Value; KPI 7: Gross Profit Margin; KPI 9: Product Availability | Direct requirement link. Reduction tracking is explicit; margin and availability are relevant guardrails but are not adequately specified as pass/fail conditions.              |

### 5.1 Requirement orphans — functional requirements not linked to a user story

High

FR9 — Recommend corrective actions

No explicit user story. No KPI measures recommendation adoption or action outcomes.

Add a recommendation-focused story and define approval, ownership, and outcome tracking.

Medium

FR10 — Export reports

No explicit user story and no KPI for export success or usage.

Add an export story if this is a required business capability, with permissions and format tests.

Medium

FR12 — Show latest successful refresh

No explicit user story. KPI 10 measures refresh success rate, but not freshness visibility.

Add a data-freshness story or explicitly assign the requirement to the technical backlog.

Medium

FR1 — Import agreed datasets

No explicit story for data ingestion. KPI 10 measures scheduled refresh success, not initial import completeness.

Add a data-ingestion story or a documented technical requirement with independent acceptance tests.

Additional observation: FR2, FR4, and parts of FR1 are supporting capabilities rather than complete end-user outcomes. They can be managed as enabling requirements, but their acceptance tests must still be traceable to delivery work.

### 5.2 Non-functional requirement traceability

1.  NFR1 — Performance: No user story explicitly verifies the five-second dashboard-load target.

2.  NFR2 — Refresh scheduling: No story specifies refresh frequency or completion time.

3.  NFR3 — Role-based access: No story tests access by role or restricted actions.

4.  NFR4 — Security in transit and at rest: No story or test case identifies the approved security controls.

5.  NFR5 — Data minimisation and privacy: No story covers unnecessary personal data, masking, or restricted access.

6.  NFR6 — Usability: No story explicitly covers user task completion or usability testing.

7.  NFR7 — Refresh/import failure handling: US2 AC2.3 covers unavailable inventory data, but does not fully verify failure detection and preservation of the last successful refresh timestamp.

8.  NFR8 — Calculation accuracy: US1–US6 contain relevant functional checks, but no single approved reconciliation protocol covers the required calculations and tolerance.

9.  NFR9 — Compliance and retention: No story identifies compliance sign-off or verifies the actual applicable requirements.

10. NFR10 — Editable thresholds: US1 and US4 depend on configured thresholds, but neither tests that an authorised administrator can update those thresholds without rewriting the solution.

These are not automatically defects in the user-story set: non-functional requirements may be delivered through shared technical tasks, security controls, or system-level testing. They are defects in traceability if the project has no separate mechanism to assign and verify them.

### 5.3 KPI orphans and weakly supported KPIs

1.  KPI 10 — Data Refresh Success Rate: No user story directly measures or reports the rate of successful scheduled refreshes. FR12 supports freshness visibility, but a refresh-success calculation needs explicit ownership and reporting.

2.  KPI 7 — Gross Profit Margin: Relevant to the business objective, but no story specifies a margin guardrail or a decision rule that prevents an inventory reduction from being accepted when profitability deteriorates.

3.  KPI 9 — Product Availability: Relevant to the business objective, but no story defines the required availability target or how a breach affects overall project success.

4.  KPI 6 — Sales Growth: Supported by US3 and US5, but it is a diagnostic/business-performance measure rather than direct evidence that the inventory-reduction objective has been achieved.

5.  KPI 1 — Slow-Moving Inventory Value: Supported by US1 and US2, but its definition depends on the classification rules, valuation method, and reporting scope being consistent.

6.  KPI 2 — Slow-Moving Inventory Reduction: Supported by US1 and US6, but its formula cannot be safely used when the baseline is zero, the measurement population changes, or the valuation rules differ between periods.

## Final verdict

## Recommended priorities before sign-off

1.  Resolve metric definitions first. Approve slow-moving thresholds, inventory ageing, stock valuation, turnover, excess-stock rules, and the baseline calculation.

2.  Close the traceability gaps. Cover FR1, FR9, FR10, FR12 and the non-functional requirements through user stories or a controlled technical backlog.

3.  Make business success measurable. Define the 20% reduction calculation, margin guardrail, availability guardrail, and treatment of markdowns, write-offs, new products, and discontinued stock.

4.  Strengthen data-quality and failure tests. Cover duplicates, missing costs, returns, transfers, mixed-age batches, zero denominators, and failed refreshes.

5.  Validate assumptions with the client. Confirm stakeholder authority, source systems, performance targets, security controls, compliance obligations, and project risk ratings.

Reviewer conclusion: The document is a strong requirements draft, but it should not yet be treated as a fully testable, end-to-end specification. The most serious issue is not the lack of additional prose; it is the absence of approved definitions and explicit traceability between requirements, delivery work, and measurable business outcomes.
