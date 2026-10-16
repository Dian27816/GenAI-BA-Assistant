# Fitness: Multi-Activity Membership (Final Requirements)

**Problem (my words):** "In gyms, people don't get what they want. People don't want to do the same thing every day. Sometimes calisthenics, yoga, fitness, boxing and other things should be there too. A prime-style membership where people can go and get different things by showing the membership."

**Version:** Final. The AI draft (Prompts 1 to 5) after the changes below, which came from the review in `gym-validation-report.md`. The original is in `gym-requirements-draft.md`.

## Changes made after validation
| # | Change | Reason (from the review) |
|---|---|---|
| 1 | Marked the core idea and the 500 member / 10 centre targets as a **hypothesis and proposed**, needing research | Business need and willingness to pay were never validated |
| 2 | Added **FR20** (partner payouts) and **US7** with acceptance criteria | Revenue share was a stated assumption but had no requirement or test |
| 3 | Added **FR21** (visit-limit enforcement) and **US8** with acceptance criteria | No rule for how plan limits work across several gyms |
| 4 | Replaced the KPI "Activity Variety Utilisation" with **Multi-Activity Adoption Rate** | The old KPI measured category coverage, not whether members try different activities |
| 5 | Defined "Paying Members" precisely (no free trials or refunded purchases) | Ambiguous counting rule |
| 6 | Rewrote FR12 and FR4 and NFR1/NFR2 into measurable wording; labelled NFR targets as proposed | "Duplicate", "agreed normal load" and similar were untestable |
| 7 | Logged **single gym vs multiple gyms** as an open question, with a pilot working assumption | The draft assumed a network without confirming it |
| 8 | Added a note that all-Must prioritisation has no method behind it | The review flagged this |

**Not changed, on purpose:** time-zone handling, emergency/safety personnel, offline check-in fallback and notification-failure cases. These matter for a real launch but are too detailed for this project stage. They stay listed in the review.

---

<!-- Prompt 1 output -->

## Stakeholder Analysis: Multi-Activity Gym Membership Platform

The proposed business idea is a Prime-style fitness membership that allows members to access different fitness activities, such as traditional gym workouts, calisthenics, yoga, and boxing, potentially across multiple fitness centres using a single membership.

### 1. Stakeholder Table

| **Stakeholder**                     | **Role**                                                                            | **Main goal**                                                               | **Main concern**                                                                  | **Influence** |
|-------------------------------------|-------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------------------------------|---------------|
| Gym Members / Customers             | Use the membership to access different fitness activities.                          | Variety, flexibility, convenience, and value for money.                     | High membership fees, limited availability, overcrowding, or hidden restrictions. | High          |
| Gym Owners / Operators              | Provide facilities and accept memberships through the platform.                     | Attract more customers, increase revenue, and improve facility utilisation. | Revenue sharing, loss of control over pricing, and overcrowding.                  | High          |
| Fitness Trainers / Instructors      | Deliver gym training, yoga, calisthenics, boxing, and other classes.                | Earn income, attract clients, and maintain a stable class schedule.         | Unpredictable bookings, compensation, and scheduling conflicts.                   | Medium        |
| Platform Owner / Business Founder   | Develop and manage the membership platform and business model.                      | Build a profitable, scalable network of fitness providers.                  | Operating costs, customer acquisition, partner retention, and profitability.      | High          |
| Payment Service Providers           | Process membership payments, renewals, refunds, and payouts.                        | Enable secure and reliable transactions.                                    | Failed payments, fraud, chargebacks, and payment disputes.                        | Medium        |
| Technology / Development Team       | Build and maintain the app or website, membership verification, and booking system. | Deliver a reliable, secure, user-friendly platform.                         | Technical failures, data security, system scalability, and integration issues.    | Medium        |
| Customer Support Team               | Handle complaints, cancellations, booking issues, and refunds.                      | Resolve issues quickly and maintain customer satisfaction.                  | High complaint volumes and unclear refund or cancellation policies.               | Medium        |
| Fitness Class / Facility Managers   | Coordinate class capacity, schedules, equipment, and access.                        | Keep operations organised and ensure a good member experience.              | Double bookings, overcrowding, safety, and resource constraints.                  | Medium        |
| Regulatory / Compliance Authorities | Oversee applicable business, consumer protection, privacy, and safety requirements. | Ensure legal compliance and protect consumers.                              | Misleading membership claims, mishandled personal data, or safety violations.     | High          |

Note: This is an initial stakeholder map based on the client's description. Some roles may overlap—for example, a gym owner may also be a trainer or facility manager. The exact stakeholders will depend on the final business model.

### 2. Three Clarifying Questions to Ask the Client

## 1

What exactly do you mean by a Prime-style membership?

Should customers access multiple activities at one gym, visit different partner gyms using one membership, or both?

## 2

Who will pay for the membership, and how will the business earn revenue?

Will customers pay a monthly subscription, pay per visit, or choose different membership tiers? How will partner gyms and trainers be compensated?

## 3

What is the biggest problem you want to solve for customers?

Is it boredom from repetitive workouts, the high cost of joining multiple fitness centres, lack of access to different activities, or difficulty finding and booking classes?

### Key business analysis insight

The most important stakeholders to validate first are customers, gym owners, and fitness instructors. Customers need variety and convenience, gym owners need a commercially attractive partnership, and instructors need workable schedules and fair compensation.

The third clarifying question is especially important because it identifies the actual problem the business should solve before requirements or features are defined.

<!-- Prompt 2 output -->

# Business Requirements Document (BRD): Multi-Activity Fitness Membership Platform

## 1. Business objective

Within 12 months of launch, establish a multi-activity fitness membership platform that enables customers to access at least four types of fitness activities through a single membership, with a target of 500 paying members and at least 10 participating fitness centres, subject to validation of market demand and business feasibility.

Note (added after validation): the core idea, that members want variety and partners will accept one shared membership, is a **hypothesis to be validated with customer and partner research**, not an established fact. The targets of 500 members and 10 centres are **proposed, with no market data behind them yet**.

## 2. In scope / Out of scope

| **In scope**                                                                                 | **Out of scope**                                                                 |
|----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| Customer registration and login                                                              | Owning or constructing fitness centres                                           |
| Membership plans and subscriptions                                                           | Manufacturing fitness equipment                                                  |
| Access to multiple fitness activities, including gym workouts, yoga, boxing and calisthenics | Providing medical diagnosis or treatment                                         |
| Discovering participating fitness centres and available activities                           | Developing a personalised AI fitness coach in the initial release                |
| Viewing class schedules and booking sessions                                                 | Selling supplements, clothing or fitness products                                |
| Membership verification at participating centres                                             | International expansion in the initial release                                   |
| Membership payments, renewals and cancellations                                              | Managing the complete payroll of partner gyms                                    |
| Partner gym registration and activity management                                             | Guaranteeing specific fitness results                                            |
| Booking history, notifications and customer support                                          | Unrestricted access to every facility regardless of capacity or membership terms |

## 3. Functional requirements

Each functional requirement defines a system capability that can be verified through testing.

### Customer and membership management

FR1 — Customer registration and login

The system shall allow customers to register using a supported email address or mobile number and authenticate before accessing account-specific features.

FR2 — View membership plans

The system shall display available membership plans, including their prices, validity periods, included activities, usage limits and applicable restrictions before purchase.

FR3 — Purchase a membership

The system shall allow customers to purchase an available membership plan and display confirmation after successful payment.

FR4 — Manage membership

The system shall allow customers to view their membership status, validity, renewal date and usage history, and initiate cancellation or renewal. Before the customer confirms cancellation, the system shall display the effective cancellation date, renewal status and any applicable fees under the approved policy.

### Fitness activity discovery and booking

FR5 — Discover fitness activities

The system shall allow customers to search and filter participating facilities by activity type, location and available session time.

Supported activity categories shall include gym workouts, yoga, boxing and calisthenics.

FR6 — View facility and class details

The system shall display each participating facility's address, available activities, class schedules, applicable membership restrictions and booking requirements.

FR7 — Book a fitness session

The system shall allow eligible members to book available sessions and provide a booking confirmation containing the facility, activity, date and time.

FR8 — Prevent overbooking

The system shall prevent confirmed bookings from exceeding the capacity configured for a session and shall communicate when a session is full.

FR9 — Cancel or modify bookings

The system shall allow members to cancel or modify eligible bookings according to the facility's configured booking policy and display the resulting booking status.

FR10 — View booking history

The system shall allow members to view their upcoming, completed and cancelled bookings.

### Membership verification and facility operations

FR11 — Verify membership

The system shall enable participating facilities to verify whether a customer's membership is valid and whether it permits access to the requested activity.

FR12 — Record facility check-ins

The system shall record successful check-ins against the customer's membership and applicable usage limits. It shall reject a second successful check-in for the same member and session unless an authorised override is recorded, and reject check-ins when the membership is ineligible or the session-access rules are not satisfied.

FR13 — Manage facility listings

The system shall allow authorised facility managers to maintain facility information, supported activities, operating hours, class schedules and session capacities.

FR14 — Manage bookings at facilities

The system shall allow authorised facility managers to view bookings for their sessions, update availability and manage session cancellations.

### Payments and communication

FR15 — Process membership payments

The system shall integrate with a supported payment provider to process membership purchases and renewals, and display the transaction status.

FR16 — Manage payment failures and refunds

The system shall record failed transactions and eligible refund requests, display their statuses and prevent a failed payment from being treated as a completed purchase.

FR17 — Send notifications

The system shall notify customers of successful bookings, booking cancellations, membership expiry and relevant payment status changes through supported communication channels.

FR18 — Handle customer support requests

The system shall allow customers to submit support requests concerning memberships, bookings, payments and facility access, and track their status.

FR19 — Generate business reports

The system shall provide authorised platform administrators with reports on active memberships, subscription revenue, bookings, check-ins, cancellations and participating facilities.

FR20 — Partner compensation and payout statements *(added after validation)*

The system shall calculate each partner facility's share from recorded check-ins using the approved revenue-share rule, generate a monthly statement listing every visit, date and calculated payout, and allow discrepancies to be flagged and reconciled.

FR21 — Visit-limit enforcement *(added after validation)*

The system shall enforce the visit limits of the member's plan across all participating facilities (for example a monthly cap or a fair-use rule), count each valid check-in once, and tell the member how many visits remain. Plan limits themselves are [TIER DETAILS TBD by the client].

## 4. Non-functional requirements

The following are proposed acceptance targets for the initial release. They should be validated with the client and technical team before being treated as contractual requirements.

| **ID** | **Category**  | **Requirement**                                                                                                                                                                               |
|--------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NFR1   | Performance   | At least 95% of standard search, membership and booking requests shall receive a response within 3 seconds under a documented test workload of [X] concurrent users and [Y] requests per second over a [Z]-minute test (proposed target, not yet validated).                                            |
| NFR2   | Availability  | The platform shall target 99.5% monthly availability (proposed, not yet validated), measured with agreed external health checks and excluding only maintenance windows defined in the approved service-level agreement.                                                                                                   |
| NFR3   | Security      | The system shall enforce role-based access controls so customers, facility managers and platform administrators can access only authorised functions and data.                                |
| NFR4   | Privacy       | The system shall collect only personal information necessary for its defined purposes and restrict access, retention and sharing in accordance with applicable privacy requirements.          |
| NFR5   | Usability     | A first-time user shall be able to locate an available activity, understand its access conditions and complete a booking without assistance in a usability test with agreed success criteria. |
| NFR6   | Compatibility | The customer-facing platform shall support the agreed target browsers and mobile devices.                                                                                                     |
| NFR7   | Reliability   | The system shall prevent duplicate successful bookings and duplicate membership charges caused by repeated submissions or retries.                                                            |
| NFR8   | Compliance    | The business shall identify and meet applicable consumer protection, payment, tax, privacy and fitness-facility safety obligations before launching relevant services.                        |
| NFR9   | Scalability   | The platform shall support the forecast launch workload and an agreed growth scenario without breaching the performance targets.                                                              |
| NFR10  | Accessibility | The customer interface shall follow an agreed accessibility standard, with WCAG 2.2 Level AA proposed as the target. The legal accessibility obligations for the launch jurisdiction are still to be confirmed.                                                                          |

## 5. Assumptions and constraints

### Assumptions

1.  Partner participation: Independent gyms and fitness centres will agree to participate and accept the platform's membership verification process.

2.  Membership model: Customers will use a single membership to access multiple fitness activities, subject to the plan's terms, facility participation and availability.

3.  Digital access: Customers will have access to a supported smartphone or web browser and an internet connection.

4.  Facility information: Participating facilities will maintain accurate schedules, capacities, activity listings and access rules.

5.  Payment processing: A supported payment provider will be available for membership transactions and applicable refunds.

6.  Business targets: The proposed target of 500 paying members, 10 participating centres and four activity categories within 12 months is provisional and requires commercial validation.

7.  Operating policies: Booking limits, cancellations, refunds, membership usage limits and partner compensation will be defined before implementation.

### Constraints

1.  Budget and timeline: Development scope will depend on the approved budget, available staff and agreed launch date.

2.  Partner coverage: Customers can access only participating facilities that offer the relevant activity and have available capacity.

3.  Commercial viability: Membership pricing and revenue-sharing arrangements must be financially sustainable for both the platform and participating facilities.

4.  Technology dependencies: Payment processing, notifications and other third-party integrations depend on provider availability and capabilities.

5.  Regulatory requirements: The business must comply with applicable laws and contractual obligations in its launch jurisdiction.

6.  Safety and capacity: Facility access and class bookings must respect capacity limits, facility rules and applicable safety requirements.

8.  Customer demand and willingness to pay (added after validation): No survey, interviews or competitor research support the idea yet. The pilot must test this before the solution scope is treated as settled.

Important BA note: The client's original problem statement is broad and does not yet confirm whether customers want access to multiple activities at a single gym or access to multiple gyms through one membership. This BRD assumes a network of participating facilities. That distinction, along with the pricing model, geographic launch area and booking rules, should be confirmed before finalising requirements.

**Open question (not resolved):** single gym with many activities, or many gyms on one pass? Pricing, partner payouts, visit limits and location filters all change depending on the answer. The working assumption for the pilot is a network of participating facilities.

<!-- Prompt 3 output -->

## Eight User Stories — Multi-Activity Fitness Membership Platform

| **ID** | **User story**                                                                                                                                                                                                             | **Priority (MoSCoW)** | **Linked requirement**                                          |
|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|-----------------------------------------------------------------|
| US1    | As a fitness customer, I want to view available membership plans, prices, included activities and usage limits, so that I can choose a membership that suits my needs and budget.                                          | Must Have             | FR2 — View membership plans                                     |
| US2    | As a fitness customer, I want to discover fitness activities such as gym workouts, yoga, boxing and calisthenics across participating facilities, so that I can enjoy variety instead of doing the same workout every day. | Must Have             | FR5 — Discover fitness activities                               |
| US3    | As a fitness customer, I want to view available sessions and book a fitness class, so that I can plan my workouts around my schedule.                                                                                      | Must Have             | FR7 — Book a fitness session                                    |
| US4    | As a fitness customer, I want to verify my membership status and permitted activities when visiting a participating facility, so that I can access the services included in my membership.                                 | Must Have             | FR11 — Verify membership                                        |
| US5    | As a gym owner or facility manager, I want to manage my facility's activities, schedules and session capacities, so that customers can discover accurate information and book available sessions.                          | Must Have             | FR13 — Manage facility listings                                 |
| US6    | As a fitness customer, I want to purchase a membership and receive confirmation of successful payment, so that I can activate my membership and start using the available fitness services.                                | Must Have             | FR3 — Purchase a membership; FR15 — Process membership payments |
| US7    | As a partner gym owner, I want a monthly statement of visits and payouts, so that I can trust the revenue share. | Must Have | FR20 — Partner compensation and payout statements |
| US8    | As a fitness customer, I want my visits counted against my plan limit across all partner gyms, so that I know how many visits I have left. | Must Have | FR21 — Visit-limit enforcement |

### Why these eight? (US7 and US8 were added after validation)

These stories cover the core customer journey and the essential business operations:

1.  Choose a suitable membership.

2.  Discover different fitness activities.

3.  Book a session.

4.  Access facilities using the membership.

5.  Manage facility availability.

6.  Purchase the membership.

MoSCoW note: All eight are classified as Must Have because they represent the proposed minimum viable product. No prioritisation method has been applied yet, so this is a proposal. US1 (plan viewing) and US2 (discovery) are the best candidates to downgrade if the first release must be smaller. Validate with the client before development begins.

<!-- Prompt 4 output -->

# Acceptance Criteria — Multi-Activity Fitness Membership Platform

## US1 — View Membership Plans

AC1.1 — Display available plans (Positive)

- Given membership plans are available in the system

- When a customer opens the membership plans page

- Then the system displays each available plan's price, validity period, included activities, usage limits and applicable restrictions.

AC1.2 — Display accurate plan details (Positive)

- Given a membership plan has configured pricing, validity and usage limits

- When a customer views that plan

- Then the displayed details match the plan information stored in the system.

AC1.3 — No available plans (Negative / Edge case)

- Given no membership plans are available for purchase

- When a customer opens the membership plans page

- Then the system displays a message stating that no plans are currently available and does not display any unavailable plan as purchasable.

## US2 — Discover Fitness Activities

AC2.1 — Discover activities by type (Positive)

- Given participating facilities offer gym workouts, yoga, boxing or calisthenics

- When a customer searches for one of these activity types

- Then the system displays participating facilities offering the selected activity.

AC2.2 — Filter by location and session time (Positive)

- Given participating facilities have configured locations and session schedules

- When a customer applies a location and available-time filter

- Then the system displays only facilities matching both selected filters.

AC2.3 — No matching activities (Negative / Edge case)

- Given no participating facility offers the selected activity in the chosen location

- When a customer searches using that activity and location

- Then the system displays a no-results message and does not display non-matching facilities as results.

## US3 — Book a Fitness Session

AC3.1 — Book an available session (Positive)

- Given a customer has a valid membership that permits the selected activity and the session has available capacity

- When the customer books the session

- Then the system creates a confirmed booking showing the facility, activity, date and time.

AC3.2 — Prevent overbooking (Negative / Edge case)

- Given a session has reached its configured booking capacity

- When another customer attempts to book that session

- Then the system rejects the booking, displays that the session is full and does not create a confirmed booking.

AC3.3 — Membership does not permit the activity (Negative / Edge case)

- Given a customer has a membership that does not include the selected activity

- When the customer attempts to book a session for that activity

- Then the system rejects the booking and explains that the activity is not covered by the membership.

## US4 — Verify Membership at a Facility

AC4.1 — Valid membership (Positive)

- Given a customer has an active membership that permits access to the requested activity at the selected facility

- When an authorised facility manager verifies the customer's membership

- Then the system displays a valid status and confirms that the requested activity is covered.

AC4.2 — Expired membership (Negative)

- Given a customer's membership has expired

- When an authorised facility manager verifies the membership

- Then the system displays an invalid or expired status and does not authorise access under that membership.

AC4.3 — Activity not included (Negative / Edge case)

- Given a customer has an active membership that does not permit the requested activity

- When an authorised facility manager verifies access for that activity

- Then the system indicates that the membership is active but the requested activity is not covered, and does not authorise access to that activity.

## US5 — Manage Facility Listings

AC5.1 — Update facility information (Positive)

- Given an authorised facility manager is logged in and their facility listing exists

- When the manager updates the facility's activities, schedule or session capacity with valid information and saves the changes

- Then the system saves the changes and displays the updated information in the facility listing.

AC5.2 — Reject invalid session capacity (Negative)

- Given an authorised facility manager is editing a session's capacity

- When the manager enters a capacity of zero or a negative number

- Then the system rejects the value, displays a validation message and does not save the invalid capacity.

AC5.3 — Unauthorised modification (Negative / Edge case)

- Given a user is not authorised to manage the facility listing

- When that user attempts to modify or save the facility's activities, schedule or capacity

- Then the system denies the modification and leaves the existing facility information unchanged.

## US6 — Purchase a Membership

AC6.1 — Successful membership purchase (Positive)

- Given a customer has selected an available membership plan and the payment provider confirms successful payment

- When the customer completes the purchase

- Then the system activates the membership according to the plan's validity terms and displays a purchase confirmation containing the plan details and payment status.

AC6.2 — Failed payment (Negative)

- Given a customer has selected an available membership plan

- When the payment provider reports that the payment has failed

- Then the system displays a payment-failure message, does not activate the membership and does not record the purchase as successfully paid.

AC6.3 — Repeated payment submission (Negative / Edge case)

- Given a customer submits a membership payment and repeats the same submission because of a timeout or delayed response

- When the payment provider confirms that the original transaction succeeded

- Then the system creates only one active membership for that purchase and records no more than one successful charge for the same transaction.

## US7 — View Partner Payout Statements

Linked requirement: FR20 · Priority: Must Have

*(Added after the validation review found no requirement or test for partner compensation.)*

AC7.1 — Monthly statement

- Given a month has ended and check-ins were recorded for a partner facility

- When the partner owner opens the monthly statement

- Then the statement lists each visit with date and activity, and the calculated payout under the approved revenue-share rule.

AC7.2 — Disputed visit (Edge case)

- Given a partner owner believes a visit count is wrong

- When the owner flags a visit in the statement

- Then the visit is marked "disputed" and reviewed before the payout is finalised.

AC7.3 — Access control (Negative)

- Given a partner owner is logged in

- When the owner tries to open another facility's statement

- Then access is denied.

## US8 — Track Visits Against Plan Limit

Linked requirement: FR21 · Priority: Must Have

*(Added after the validation review found no enforcement rule for visit limits.)*

AC8.1 — Remaining visits

- Given a member has a plan with a monthly visit limit

- When the member completes a valid check-in at any partner facility

- Then exactly one visit is deducted and the remaining visits are shown.

AC8.2 — Limit reached (Negative)

- Given the member has used all visits allowed by the plan

- When the member scans at a partner facility

- Then entry is declined with the reason "visit limit reached".

AC8.3 — Duplicate scan (Edge case)

- Given the same pass is scanned twice for the same session

- When the second scan is processed

- Then no second visit is deducted.

Testing note: Each criterion has a clear expected outcome that a tester can verify. For execution, prepare test data for active and expired memberships, included and excluded activities, available and full sessions, authorised and unauthorised facility users, and successful and failed payments.

<!-- Prompt 5 output -->

## 1. Success KPIs — Multi-Activity Fitness Membership Platform

The following KPIs measure whether the platform attracts customers, provides access to varied fitness activities, operates reliably, and generates sustainable revenue. Baselines and targets are marked TBD because no measured values or approved targets have been supplied.

| **KPI**                         | **Formula**                                                                            | **Data source**                           | **Baseline** | **Target** | **Review frequency** |
|---------------------------------|----------------------------------------------------------------------------------------|-------------------------------------------|--------------|------------|----------------------|
| Paying Members                  | Distinct customers with an active paid membership at the reporting date, excluding free trials, fully refunded purchases and cancelled memberships                                      | Membership database, payment records      | TBD          | TBD        | Monthly              |
| Membership Conversion Rate      | (Customers purchasing a membership ÷ Unique visitors to membership plans) × 100        | Website/app analytics, membership records | TBD          | TBD        | Monthly              |
| Membership Renewal Rate         | (Eligible memberships renewed ÷ Total memberships eligible for renewal) × 100          | Membership and subscription records       | TBD          | TBD        | Monthly              |
| Multi-Activity Adoption Rate    | (Members who completed sessions in 2 or more activity types in the month ÷ Members who completed at least one session in the month) × 100 | Check-in and booking records | TBD | TBD | Monthly |
| Booking Success Rate            | (Successful bookings ÷ Total valid booking attempts) × 100                             | Booking system logs                       | TBD          | TBD        | Weekly               |
| Booking Cancellation Rate       | (Cancelled bookings ÷ Total confirmed bookings) × 100                                  | Booking records                           | TBD          | TBD        | Weekly               |
| Membership Access Success Rate  | (Successful eligible check-ins ÷ Total valid membership verification attempts) × 100   | Membership verification and check-in logs | TBD          | TBD        | Weekly               |
| Customer Satisfaction Score     | Sum of customer satisfaction ratings ÷ Number of valid responses                       | Customer feedback surveys                 | TBD          | TBD        | Monthly              |
| Partner Facility Retention Rate | (Facilities remaining active at period end ÷ Facilities active at period start) × 100  | Partner facility records                  | TBD          | TBD        | Quarterly            |
| Payment Success Rate            | (Successful membership payment attempts ÷ Total payment attempts) × 100                | Payment provider transaction records      | TBD          | TBD        | Weekly               |
| Monthly Recurring Revenue (MRR) | Sum of monthly recurring membership subscription revenue                               | Subscription and payment records          | TBD          | TBD        | Monthly              |
| Platform Availability           | (Total scheduled service time − Unavailable time) ÷ Total scheduled service time × 100 | Application monitoring logs               | TBD          | TBD        | Monthly              |

Change after validation: the original KPI "Activity Variety Utilisation" measured how many categories were booked out of the categories offered. That is platform coverage, not member variety, and could look high even if every member used only one activity. It is replaced by Multi-Activity Adoption Rate, which matches the original business idea (members who do not want the same workout every day).

How to use these KPIs: Establish baselines during the pilot or initial launch, then agree on measurable targets with the client. The formulas should also be finalised with clear definitions—for example, whether a booking attempt counts as valid only after membership eligibility and session availability have been checked.

## 2. Project Risks

Likelihood and impact below are preliminary qualitative assessments, not measured probabilities. Reassess them after validating the business model, partner agreements and technical design.

| **Risk**                                                       | **Likelihood** | **Impact** | **Mitigation**                                                                                                                                       |
|----------------------------------------------------------------|----------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| Insufficient customer demand                                   | Medium         | High       | Conduct customer interviews, surveys and a pilot before committing to a large rollout. Track conversion, retention and customer feedback.            |
| Too few participating gyms and fitness centres                 | High           | High       | Recruit anchor partners before launch, define partner benefits and establish minimum geographic coverage.                                            |
| Inaccurate facility schedules or activity listings             | Medium         | High       | Require partners to maintain current listings and periodically verify schedules, availability and access rules.                                      |
| Overbooking or overcrowded sessions                            | Medium         | High       | Enforce session capacity during booking, handle simultaneous booking attempts safely and monitor attendance.                                         |
| Unclear membership pricing or revenue sharing                  | Medium         | High       | Validate willingness to pay and agree on transparent pricing, payout terms, usage limits and partner compensation.                                   |
| Payment failures or duplicate charges                          | Medium         | High       | Use a reputable payment provider, reconcile transactions, implement duplicate-request protection and provide a clear refund process.                 |
| Customers cannot access activities covered by their membership | Medium         | High       | Validate membership eligibility at booking and check-in, clearly display restrictions and provide a support escalation process.                      |
| Customer dissatisfaction due to limited activity variety       | Medium         | High       | Monitor activity-category usage and customer feedback; expand the activity network based on demonstrated demand.                                     |
| Personal data breach or unauthorised access                    | Medium         | High       | Apply role-based access, minimise collected data, secure sensitive information, conduct security testing and establish incident-response procedures. |
| Legal or regulatory non-compliance                             | Medium         | High       | Review applicable privacy, consumer protection, payment, tax and facility-safety obligations with qualified advisers before launch.                  |
| Technical downtime or booking-system failure                   | Medium         | High       | Monitor service health, test peak loads, maintain backups and define recovery procedures.                                                            |
| Poor partner retention                                         | Medium         | High       | Monitor partner utilisation and satisfaction, communicate performance transparently and review commercial arrangements regularly.                    |
| Delays in development or integration                           | Medium         | Medium     | Prioritise core membership, discovery, booking, verification and payment journeys; track dependencies and test integrations early.                   |
| Fraudulent membership sharing or unauthorised access           | Medium         | Medium     | Use appropriate membership verification, restrict access to authorised users and monitor suspicious check-in patterns.                               |

## 3. Recommended priorities

For the initial launch, focus on these three outcomes:

1.  Customer adoption: paying memberships, conversion and renewal rates.

2.  Core service reliability: successful bookings, membership access and payment success.

3.  Marketplace viability: participating facility retention, multi-activity adoption and partner utilisation.

The most important business risk is the two-sided marketplace problem: customers need enough participating facilities and activities to see value, while facilities need enough paying members and bookings to justify participating.

The initial pilot should therefore test both sides together rather than measuring customer sign-ups alone.
