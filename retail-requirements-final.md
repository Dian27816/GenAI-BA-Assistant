# Retail: Slow-Moving Inventory (Final Requirements)

**Problem:** "In retail, inventory is moving slowly."
**Version:** Final. This is the AI draft (Prompts 1 to 5) after the changes below, which came from the validation review in `retail-validation-report.md`. The original is in `retail-requirements-draft.md`.

## Changes made after validation
| # | Change | Reason (from the review) |
|---|---|---|
| 1 | Rewrote vague tests for FR1, FR3, FR4, FR9 and NFR1 into measurable wording | "Successfully", "standard queries" and similar cannot be tested |
| 2 | Added **US7** and AC7.1 to AC7.3 for recommendations (FR9) | A Must requirement had no user story |
| 3 | Added a **success rule** with margin and availability guardrails; write-offs do not count | Stock value can fall without real success |
| 4 | Defined inventory age at **batch level** and logged the method as an open client question | A single receipt date hides old stock |
| 5 | Labelled the 20% target as **proposed, to be validated with baseline data** | No data supports the number |
| 6 | Clarified that the system recommends only and cannot act without approval; added zero-baseline edge case | Boundary between recommendation and execution was unclear |
| 7 | Converted formulas from LaTeX to plain text | Readability |

**Not changed, on purpose:** the Given/When/Then formatting of the requirement-level tests (minor), and the open items the client must approve (thresholds, tolerances, security standards, export formats).

---

<!-- Prompt 1 output -->

## Stakeholder Analysis: Slow-Moving Inventory in Retail

Business problem: “In retail, inventory is moving slowly.”

This means products are selling more slowly than expected, causing stock to remain in storage or on shelves for longer periods. This can lead to higher storage costs, blocked working capital, potential markdowns, and obsolete inventory.

### 1. Stakeholder Identification

| **Stakeholder**                      | **Role**                                                                   | **Main goal**                                                         | **Main concern**                                                              | **Influence** |
|--------------------------------------|----------------------------------------------------------------------------|-----------------------------------------------------------------------|-------------------------------------------------------------------------------|---------------|
| Business Owner / Senior Management   | Makes strategic decisions and allocates resources to solve the problem     | Increase profitability and improve business performance               | Falling revenue, reduced profits, and capital stuck in unsold stock           | High          |
| Inventory / Warehouse Manager        | Monitors stock levels, storage, and inventory movement                     | Maintain optimal stock levels and reduce excess inventory             | Overstocking, storage costs, and products becoming obsolete                   | High          |
| Sales Manager                        | Monitors sales performance and develops strategies to increase sales       | Increase sales volume and meet sales targets                          | Low demand, missed targets, and slow product turnover                         | High          |
| Marketing Team                       | Creates promotions, advertising campaigns, and offers to attract customers | Generate customer interest and increase product demand                | Ineffective campaigns, low customer engagement, and promotion costs           | Medium        |
| Purchasing / Procurement Team        | Decides which products to buy, in what quantities, and when                | Purchase the right products in the right quantities at the right time | Incorrect demand forecasts and excessive purchasing                           | High          |
| Store Managers / Retail Staff        | Manage day-to-day store operations and interact directly with customers    | Sell products efficiently and provide a good customer experience      | Products not selling, poor product placement, and customer objections         | Medium        |
| Finance Team / CFO                   | Tracks cash flow, inventory costs, margins, and financial performance      | Improve cash flow and protect profit margins                          | Working capital tied up in inventory, storage costs, and markdown losses      | High          |
| Customers                            | Purchase products and influence demand through their preferences           | Find suitable products at the right price and quality                 | High prices, irrelevant product selection, or lack of preferred products      | Medium        |
| Suppliers / Vendors                  | Supply merchandise and may support returns, exchanges, or revised orders   | Maintain sales relationships and predictable orders                   | Cancellations, reduced future orders, and unsold merchandise returned         | Medium        |
| Data / Business Intelligence Analyst | Analyses sales, inventory, pricing, and customer data to identify causes   | Provide evidence-based insights and actionable recommendations        | Incomplete data, inaccurate forecasts, and difficulty identifying root causes | Medium        |

Note: Influence levels are preliminary assumptions. They may change depending on the retailer's size, organisational structure, and decision-making authority.

### 2. Three Clarifying Questions to Ask the Client

## 1

Which products or product categories are moving slowly, and in which stores or locations?

Purpose: Identify whether the problem affects the entire business, specific categories, individual stores, or particular products.

## 2

When did the slowdown begin, and how does current sales or inventory turnover compare with previous periods or targets?

Purpose: Establish the scale and timing of the problem and determine whether it is a recent change or a long-standing issue.

## 3

What does the client believe is causing the slowdown, and what data is available to investigate it?

Purpose: Explore possible causes such as pricing, changing customer preferences, weak marketing, seasonality, competition, or over-purchasing, and identify the available sales, stock, and pricing data.

### Key takeaway

The stakeholder analysis should help you identify who experiences the problem, who can help solve it, and who has the authority to act.

A useful starting point is to collaborate with senior management, inventory, sales, procurement, and finance teams. Together, they can help determine whether slow-moving inventory is primarily a demand problem, a purchasing problem, a pricing problem, or a combination of these factors.

<!-- Prompt 2 output -->

# Business Requirements Document (BRD): Slow-Moving Inventory in Retail

## 1. Business Objective

Within six months of implementation, reduce the value of slow-moving inventory by at least 20% compared with the baseline established during the initial assessment, while maintaining product availability and protecting gross profit margins.

Note: The 20% target and six-month timeline are **proposed targets, to be validated with the client's baseline data**. No historical data supports them yet.

**Success rule (added after validation):** The reduction counts as achieved only if gross profit margin and product availability stay within limits approved by the client (gross margin not below [CLIENT TO DEFINE]%, availability not below [CLIENT TO DEFINE]%). Reductions caused only by write-offs do not count, and the same classification and valuation rules must be used at the baseline and the final measurement.

## 2. In Scope / Out of Scope

| **In Scope**                                                                    | **Out of Scope**                                                          |
|---------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| Analysing historical sales and inventory data                                   | Opening new retail stores or closing existing stores                      |
| Identifying slow-moving and non-moving products                                 | Replacing the entire inventory management system                          |
| Monitoring stock levels, inventory age, and turnover                            | Managing supplier manufacturing operations                                |
| Analysing pricing, discounts, seasonality, and sales trends                     | Redesigning the company's entire marketing strategy                       |
| Providing dashboards and reports for stakeholders                               | Automatically changing prices, placing purchase orders or moving stock without an authorised approval (the system recommends only) |
| Recommending actions such as promotions, stock transfers, or reduced purchasing | Guaranteeing sales increases regardless of market conditions              |
| Tracking progress against inventory reduction targets                           | Making final business decisions on behalf of management                   |

## 3. Functional Requirements

These requirements describe what the proposed system must do. Each requirement should be testable.

| **ID** | **Functional requirement**                                                                                                                                         | **Acceptance criteria / Test**                                                                             |
|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| FR1    | The system shall import sales, inventory, product, and pricing data from the agreed data sources.                                                                  | Given a dataset with valid, missing and invalid records, when an import runs, the system reports records received, accepted, rejected and flagged, with a reason for each rejected or flagged record.                |
| FR2    | The system shall allow users to view inventory by product, category, store, and time period.                                                                       | Users can filter the inventory report using each specified dimension.                                      |
| FR3    | The system shall identify slow-moving and non-moving inventory using client-approved thresholds.                                                                   | Given an approved classification rule per category, every eligible product is classified using sales in the approved window; products with insufficient data are flagged for review.                              |
| FR4    | The system shall calculate inventory age, inventory turnover, and days of inventory on hand using agreed formulas.                                                 | Calculated values match independently calculated results for an approved sample of [N] products within the client-defined tolerance.                           |
| FR5    | The system shall compare current sales performance with previous periods and established sales targets.                                                            | Users can view sales differences in both absolute values and percentages.                                  |
| FR6    | The system shall identify products with high inventory levels relative to their sales performance.                                                                 | The report flags products that meet the agreed excess-stock criteria.                                      |
| FR7    | The system shall analyse sales trends by product and category to identify possible causes of slow movement.                                                        | Users can view historical sales trends and relevant indicators for selected products.                      |
| FR8    | The system shall provide a dashboard showing slow-moving stock value, inventory age, turnover, and affected product counts.                                        | All agreed metrics appear on the dashboard and update when the underlying data is refreshed.               |
| FR9    | The system shall recommend potential corrective actions, such as discounts, promotions, stock transfers, or reduced future orders, based on agreed business rules. | Each eligible flagged product shows the recommendation, the rule that triggered it and any approval needed; if required data is missing, it shows "review needed" instead of a recommendation. |
| FR10   | The system shall allow authorised users to export reports for further analysis and management review.                                                              | Authorised users can export filtered results in the agreed formats, such as Excel or CSV.                  |
| FR11   | The system shall track inventory performance against the baseline and business objective.                                                                          | Users can view progress toward the agreed inventory reduction target over time.                            |
| FR12   | The system shall display the date and time of the latest successful data refresh.                                                                                  | The dashboard shows the correct refresh timestamp and indicates when a refresh fails.                      |

## 4. Non-Functional Requirements

Non-functional requirements specify how well and how securely the system must operate.

| **ID** | **Category**    | **Requirement**                                                                                                              | **Acceptance criteria / Test**                                                                                       |
|--------|-----------------|------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| NFR1   | Performance     | The dashboard shall load within five seconds for standard queries under the agreed normal workload.                          | Under the approved test workload ([dataset size], [concurrent users], [specified queries]), at least 95% of dashboard requests load within five seconds.                                 |
| NFR2   | Performance     | The system shall refresh inventory and sales data at the frequency agreed with the client.                                   | A refresh completes within the agreed time window, and failures are reported.                                        |
| NFR3   | Security        | The system shall restrict access to dashboards and data based on user roles.                                                 | Unauthorised users cannot access restricted reports or perform restricted actions.                                   |
| NFR4   | Security        | Data shall be protected during transmission and storage using approved security controls.                                    | Security testing verifies the required encryption and access controls.                                               |
| NFR5   | Privacy         | The system shall use only the customer or employee data necessary for the approved business purpose.                         | Unnecessary personal data is excluded or masked, and access is restricted where applicable.                          |
| NFR6   | Usability       | The dashboard shall provide clear labels, filters, definitions, and visual indicators for key inventory metrics.             | Representative business users can complete agreed tasks using the dashboard without specialist technical assistance. |
| NFR7   | Reliability     | The system shall detect and report failed data imports or refreshes without silently presenting incomplete data as current.  | Simulated failures generate a visible warning and preserve the last successful refresh timestamp.                    |
| NFR8   | Accuracy        | Inventory and sales calculations shall follow documented, client-approved definitions.                                       | Reconciliation against approved source records falls within the agreed tolerance.                                    |
| NFR9   | Compliance      | The solution shall comply with applicable data protection, information security, and record-retention requirements.          | A compliance review confirms that applicable requirements have been documented and addressed.                        |
| NFR10  | Maintainability | Business users or authorised administrators shall be able to update agreed thresholds without rewriting the entire solution. | An authorised administrator changes a threshold and verifies that subsequent reports use the updated value.          |

The numerical performance targets, refresh frequency, accuracy tolerance, and security standards should be confirmed during requirements validation.

## 5. Assumptions and Constraints

### A. Assumptions

1.  The retailer can provide historical sales, inventory, product, and pricing data.

2.  Product identifiers are sufficiently consistent across the available datasets to link sales with inventory.

3.  The client will define what qualifies as slow-moving or non-moving inventory, potentially using days without a sale, inventory turnover, or product-specific thresholds.

4.  The retailer will nominate stakeholders from management, sales, inventory, procurement, finance, and IT to validate the requirements.

5.  Historical data is sufficiently reliable to support meaningful analysis.

6.  Management will review and approve recommendations before significant pricing, purchasing, or stock-transfer decisions are made.

7.  The client will establish a baseline period and approve the proposed 20% reduction target.

8.  Sales performance may be affected by seasonality, competitor activity, pricing, and customer preferences, so inventory reduction cannot be attributed solely to the system.

### B. Constraints

1.  Data availability: Missing, outdated, or inconsistent sales and inventory records may reduce the accuracy of the analysis.

2.  Budget: The solution must operate within the client's approved budget for software, development, and maintenance.

3.  Technology: Integration depends on the retailer's existing systems, data formats, and available interfaces.

4.  Timeline: Delivery and implementation must fit the agreed project schedule.

5.  Business operations: Recommendations must not cause unacceptable stockouts or damage customer satisfaction.

6.  Profitability: Discounts and clearance sales must consider product margins and must not undermine the retailer's profitability targets.

7.  Decision authority: The system provides insights and recommendations; authorised business stakeholders retain responsibility for final decisions.

8.  Regulatory requirements: Data handling and retention must comply with applicable laws and company policies.

### How the requirements connect to the business problem

Business problem

Inventory is moving slowly and capital is tied up in unsold products.

Analyse and identify

FR1–FR7: Integrate data, measure stock movement, and identify problem products.

Recommend and monitor

FR8–FR12: Visualise insights, suggest corrective actions, and track progress.

Business outcome

Target a 20% reduction in slow-moving inventory value within six months, subject to client approval.

Important distinction: These are proposed business requirements, not confirmed client requirements. Before development begins, validate the target, slow-moving inventory definition, data sources, stakeholder permissions, and reporting frequency with the client.

<!-- Prompt 3 output -->

## User Stories: Slow-Moving Inventory in Retail

| **ID** | **User story**                                                                                                                                                                                                                | **Priority (MoSCoW)** | **Linked requirement** |
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|------------------------|
| US1    | As an Inventory Manager, I want to identify slow-moving and non-moving products, so that I can take action to reduce excess stock and prevent inventory from becoming obsolete.                                               | Must Have             | FR3                    |
| US2    | As a Business Owner, I want to view key inventory metrics, including slow-moving stock value, inventory age, and turnover, so that I can understand the scale of the problem and make informed business decisions.            | Must Have             | FR8                    |
| US3    | As a Sales Manager, I want to compare current sales performance with previous periods and sales targets, so that I can identify products and categories with declining sales.                                                 | Must Have             | FR5                    |
| US4    | As a Procurement Manager, I want to identify products whose inventory levels are high relative to their sales, so that I can adjust future purchasing decisions and avoid overstocking.                                       | Must Have             | FR6                    |
| US5    | As a Business Analyst, I want to analyse historical sales trends by product and category, so that I can investigate potential reasons for slow-moving inventory and support evidence-based decisions.                         | Should Have           | FR7                    |
| US6    | As a Finance Manager, I want to track inventory performance against the established baseline and reduction target, so that I can measure progress toward reducing slow-moving inventory value while protecting profitability. | Must Have             | FR11                   |
| US7    | As a Category Manager, I want recommended actions (markdown, store-to-store transfer or reduced buying) for each slow-moving SKU, so that I can clear stock without guessing. | Must Have             | FR9                    |

### Why these priorities?

- Must Have: Essential capabilities needed to identify the problem, understand its financial impact, and track progress toward the business objective.

- Should Have: Valuable capabilities that support deeper analysis but may follow the essential features.

- Could Have: Useful enhancements that can be delivered if time and budget permit.

- Won't Have (for now): Features explicitly excluded from the current project scope.

These seven stories focus on what each stakeholder needs and why, rather than prescribing how the system must be built.

<!-- Prompt 4 output -->

# Acceptance Criteria: Slow-Moving Inventory in Retail

Acceptance criteria define the conditions that must be met for each user story to be considered complete. Each criterion follows Given / When / Then and includes a negative or edge case that a tester can verify.

## US1 — Identify Slow-Moving and Non-Moving Products

Linked requirement: FR3 · Priority: Must Have

AC1.1 — Identify slow-moving products

- Given inventory data is available and the client-approved slow-moving threshold is configured,

- When the Inventory Manager views the inventory report,

- Then all products meeting the slow-moving criteria are identified as slow-moving.

AC1.2 — Identify non-moving products

- Given inventory data includes products with no sales during the configured non-moving period,

- When the Inventory Manager views the inventory report,

- Then those products are identified as non-moving.

AC1.3 — Missing sales data (edge case)

- Given a product has missing sales history,

- When the system evaluates its inventory movement,

- Then the product is flagged for data review and is not incorrectly classified as non-moving solely because its sales data is missing.

## US2 — View Key Inventory Metrics

Linked requirement: FR8 · Priority: Must Have

AC2.1 — Display inventory metrics

- Given valid inventory data is available,

- When the Business Owner opens the inventory dashboard,

- Then the dashboard displays slow-moving stock value, inventory age, inventory turnover, and the number of affected products.

AC2.2 — Update metrics after data refresh

- Given the inventory data has changed and a successful refresh has completed,

- When the Business Owner views the dashboard,

- Then the displayed metrics reflect the refreshed data.

AC2.3 — No available inventory data (edge case)

- Given no valid inventory data is available,

- When the Business Owner opens the dashboard,

- Then the dashboard displays a clear data-unavailable message instead of presenting missing metrics as zero.

## US3 — Compare Sales Performance

Linked requirement: FR5 · Priority: Must Have

AC3.1 — Compare sales with previous periods

- Given sales data is available for the current and previous periods,

- When the Sales Manager selects a product or category and a comparison period,

- Then the system displays sales values for both periods and the absolute and percentage changes.

AC3.2 — Compare sales with targets

- Given a sales target and actual sales data are available for the selected period,

- When the Sales Manager views the sales performance report,

- Then the report displays actual sales, the target, and the difference between them.

AC3.3 — Missing comparison data (edge case)

- Given sales data is unavailable for the selected previous period,

- When the Sales Manager requests a comparison,

- Then the system indicates that the comparison cannot be completed and does not display a fabricated percentage change.

## US4 — Identify Products with Excess Inventory

Linked requirement: FR6 · Priority: Must Have

AC4.1 — Flag excess inventory

- Given inventory quantities, sales performance, and approved excess-stock criteria are available,

- When the Procurement Manager views the inventory report,

- Then products meeting the excess-stock criteria are flagged.

AC4.2 — Filter excess inventory

- Given the inventory report contains products with different stock levels and sales performance,

- When the Procurement Manager filters the report for excess inventory,

- Then only products meeting the configured excess-stock criteria are displayed.

AC4.3 — Incomplete product data (edge case)

- Given a product has an available stock quantity but missing sales data,

- When the system evaluates whether that product has excess inventory,

- Then the product is flagged for review and is not assigned a definitive excess-stock classification based on incomplete data alone.

## US5 — Analyse Historical Sales Trends

Linked requirement: FR7 · Priority: Should Have

AC5.1 — View historical sales trends

- Given historical sales data exists for multiple periods,

- When the Business Analyst selects a product or category and a date range,

- Then the system displays sales trends for the selected period.

AC5.2 — Compare product and category trends

- Given historical sales data is available for multiple products or categories,

- When the Business Analyst selects products or categories for comparison,

- Then the system displays their sales trends over the same selected period.

AC5.3 — Insufficient historical data (edge case)

- Given a selected product has no historical sales records for the requested date range,

- When the Business Analyst requests its sales trend,

- Then the system displays a clear message that historical data is unavailable and does not invent trend values.

## US6 — Track Progress Against Inventory Reduction Targets

Linked requirement: FR11 · Priority: Must Have

AC6.1 — Track inventory reduction

- Given an approved baseline inventory value and reduction target are recorded,

- When the Finance Manager views the performance report,

- Then the report displays the baseline value, current slow-moving inventory value, absolute reduction, percentage reduction, and target.

AC6.2 — Measure target achievement

- Given the baseline and current inventory values are available,

- When the Finance Manager reviews progress against the target,

- Then the system indicates whether the agreed reduction target has been achieved based on the calculated percentage reduction.

AC6.3 — Missing baseline (edge case)

- Given no approved baseline inventory value has been established,

- When the Finance Manager opens the progress report,

- Then the system indicates that progress cannot yet be calculated and does not report a misleading reduction percentage. The same applies when the baseline value is zero (percentage reduction shown as unavailable with an explanation).

## US7 — Receive Recommended Actions for Slow-Moving Products

Linked requirement: FR9 · Priority: Must Have

*(Added after the validation review found FR9 had no user story.)*

AC7.1 — Show recommendation

- Given a product is flagged as slow-moving and all required data is available,

- When the Category Manager opens the product details,

- Then the system shows the recommended action, the rule that triggered it, and whether approval is required.

AC7.2 — Missing data (edge case)

- Given required data (for example cost price) is missing,

- When the Category Manager opens the product details,

- Then the system shows "review needed" and does not show a recommendation.

AC7.3 — No automatic execution

- Given a recommendation is displayed,

- When no authorised user has approved it,

- Then the system does not change prices, create purchase orders or move stock.

## Important testing note

For these criteria to be fully testable, the client must approve the following definitions before testing begins:

- The time period and thresholds for slow-moving and non-moving products.

- The formula for inventory turnover and inventory age.

- The criteria for excess inventory.

- The comparison periods and sales targets.

- The method for valuing slow-moving inventory and calculating percentage reduction.

Once these are agreed, a tester can prepare sample datasets, calculate expected results independently, and mark each criterion Pass or Fail.

<!-- Prompt 5 output -->

# Success KPIs and Project Risks: Slow-Moving Inventory in Retail

## 1. Success KPI Table

These KPIs measure whether the project is successfully reducing slow-moving inventory, improving sales and inventory management, and protecting profitability.

As requested, all baselines and targets are marked \[TBD\] until the client confirms them.

| **KPI**                                 | **Formula**                                                                                                   | **Data source**                                                   | **Baseline** | **Target** | **Review frequency** |
|-----------------------------------------|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|--------------|------------|----------------------|
| 1\. Slow-Moving Inventory Value         | Sum of the inventory value of all products classified as slow-moving                                          | Inventory records, product costs, inventory classification report | \[TBD\]      | \[TBD\]    | Weekly               |
| 2\. Slow-Moving Inventory Reduction (%) | (Baseline value − Current value) ÷ Baseline value × 100                       | Historical and current inventory valuation reports                | \[TBD\]      | \[TBD\]    | Monthly              |
| 3\. Inventory Turnover Ratio            | Cost of Goods Sold (COGS) ÷ Average Inventory Value                                                           | Sales records, cost records, inventory reports                    | \[TBD\]      | \[TBD\]    | Monthly              |
| 4\. Days of Inventory on Hand           | (Average Inventory Value ÷ COGS) × Days in period                             | Inventory valuation and COGS records                              | \[TBD\]      | \[TBD\]    | Monthly              |
| 5\. Inventory Age                       | Measured at **batch level**: current date − receipt date of each batch, in days, with remaining units tracked per batch (stock-flow method, e.g. FIFO, to be approved by Finance)                                                  | Inventory receipt records and stock data                          | \[TBD\]      | \[TBD\]    | Weekly               |
| 6\. Sales Growth (%)                    | (Current period sales − Previous period sales) ÷ Previous period sales × 100  | Point-of-sale (POS) and sales reports                             | \[TBD\]      | \[TBD\]    | Monthly              |
| 7\. Gross Profit Margin (%)             | (Net Sales − COGS) ÷ Net Sales × 100                                          | Sales and finance records                                         | \[TBD\]      | \[TBD\]    | Monthly              |
| 8\. Excess Inventory Value              | Sum of the inventory value of products meeting the approved excess-stock criteria                             | Inventory reports and sales data                                  | \[TBD\]      | \[TBD\]    | Weekly               |
| 9\. Product Availability (%)            | Availability checks with stock available ÷ Total availability checks × 100 | Store stock records and availability reports                      | \[TBD\]      | \[TBD\]    | Weekly               |
| 10\. Data Refresh Success Rate (%)      | Successful scheduled refreshes ÷ Total scheduled refreshes × 100                   | Data refresh logs                                                 | \[TBD\]      | \[TBD\]    | Weekly               |

Important measurement notes:

- The proposed business objective is to reduce slow-moving inventory value by 20% within six months, while maintaining product availability and protecting gross profit margins. This is a proposed target that requires client approval.

- Inventory valuation should use a consistent, agreed costing method across the baseline and measurement periods.

- Inventory turnover and days on hand should use the same reporting period and consistent COGS and average-inventory definitions.

- If previous-period sales are zero, sales growth is undefined; it should be reported as unavailable rather than assigned a misleading percentage.

- Inventory age is defined at batch level, because a single receipt date per SKU would hide old stock when a product is replenished in several batches (e.g. 100 units received 120 days ago and 50 units 10 days ago). The stock-flow method (FIFO or other) is an **open question for the client** and the formula stays provisional until approved.

## 2. Project Risk Table

The likelihood and impact ratings below are initial qualitative assessments, not confirmed client assessments.

| **Risk**                                                                     | **Likelihood** | **Impact** | **Mitigation**                                                                                                                                                  |
|------------------------------------------------------------------------------|----------------|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1\. Inaccurate or incomplete inventory data                                  | High           | High       | Validate source data, identify missing records, reconcile inventory quantities and values, and flag unreliable records before analysis.                         |
| 2\. Incorrect classification of slow-moving inventory                        | Medium         | High       | Agree on product-specific or category-specific thresholds with the client, document the rules, and validate classifications against sample products.            |
| 3\. Inaccurate sales forecasts or misleading trend analysis                  | Medium         | High       | Analyse historical trends, seasonality, promotions, and relevant market factors; communicate uncertainty rather than treating patterns as guaranteed forecasts. |
| 4\. Excessive discounting reduces profitability                              | Medium         | High       | Require margin checks and management approval before promotions or markdowns; monitor gross profit margin alongside inventory reduction.                        |
| 5\. Inventory reduction causes stockouts                                     | Medium         | High       | Monitor product availability, distinguish excess stock from essential or fast-selling products, and require review before reducing future orders.               |
| 6\. Integration difficulties with existing retail systems                    | Medium         | High       | Confirm data sources and formats early, test data imports, document integration dependencies, and establish a fallback process for failed imports.              |
| 7\. Stakeholders do not adopt the dashboard or recommendations               | Medium         | Medium     | Involve inventory, sales, procurement, and finance teams during design; provide training and gather feedback through user acceptance testing.                   |
| 8\. Unclear ownership of corrective actions                                  | Medium         | Medium     | Assign responsible owners for reviewing recommendations, approving actions, and monitoring results, with regular progress reviews.                              |
| 9\. Delays in delivery due to scope changes or unavailable data              | Medium         | Medium     | Agree on scope, acceptance criteria, milestones, data access, and change-control procedures before implementation.                                              |
| 10\. Unauthorised access to sensitive business data                          | Low            | High       | Apply role-based access controls, approved encryption, secure data handling, and periodic access reviews.                                                       |
| 11\. Inventory reduction target is not achieved                              | Medium         | High       | Establish a reliable baseline, monitor KPIs monthly, investigate underperforming categories, and adjust approved corrective actions.                            |
| 12\. Incorrect conclusions because of seasonality or external market changes | Medium         | Medium     | Compare equivalent periods where appropriate and consider seasonal demand, competitor activity, and changing customer preferences before recommending action.   |

## 3. Recommended KPI review approach

Weekly operational review

Inventory Manager and Procurement Manager

Review slow-moving stock value, excess inventory, inventory age, and product availability. Identify products requiring immediate attention.

Monthly performance review

Business Owner, Sales Manager, and Finance Manager

Review inventory reduction, turnover, sales trends, gross profit margin, and progress against the approved target.

Six-month outcome review

Senior Management and project stakeholders

Assess whether slow-moving inventory value has decreased by the approved target, while maintaining product availability and protecting gross profit margins.

Overall success measure (see the success rule in the Business Objective): The project should not be considered successful merely because inventory value falls. It should demonstrate a measurable reduction in slow-moving inventory while avoiding unacceptable stockouts and deterioration in gross profit margins.
