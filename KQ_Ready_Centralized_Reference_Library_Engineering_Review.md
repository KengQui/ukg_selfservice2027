# Centralized Earning & Deduction Reference Library

**Engineering design review**  
**Source:** Ready Shared Services  
**Today's goal:** Validate the proposed schema before implementation.

## The problem: today's fragmented state

The current state shows three separate pods, each maintaining its own approach:

`POD  |  POD  |  POD`

## The future state: a reference layer

Three pods use a shared master reference library:

`POD  ↓`  
`POD  ↓  → MASTER REFERENCE LIBRARY`  
`POD  ↓`

The library contains **canonical concepts, aliases, classifications, and validated attributes**.

Production codes remain client-configurable. The library standardizes their meaning and evidence.

## AI-generated question flow

**From reference information to guided decisions**

1. **Reference Library** provides configuration conditions, required clarification, and decision criteria.
2. **AI constructs the questions** by determining what information is missing and formulating understandable questions.
3. **Application displays the questions** so the user can answer questions about the company's reimbursement practices.
4. **AI evaluates the answers** using verified decision criteria to determine whether more information is needed or which approved guidance applies.

## Initial data model: agenda

1. **Current Data & Mapping Capabilities** — Nishant walks through the data currently available and the existing mapping system.
2. **Draft Reference Library Requirements** — Review the information to collect and how the application will use it.
3. **Engineering Alignment & Next Steps** — Identify data gaps.

## Start of flow: Question 0 — Payment method

**Question:** How does the company calculate mileage payments?

**Fields used:** Canonical Concept; Configuration Conditions; Required Clarification.

**What the information does:** Recognizes the mileage concept and identifies whether the payment is per mile, fixed, or based on actual expenses.

**Options shown in the prototype:**

- A set amount for each business mile driven.
- A fixed amount regardless of miles driven.
- Reimbursement of actual eligible vehicle expenses.
- Something else.

Branch to per-mile follow-ups only when the user selects a per-mile payment.

## Follow-up question 1: Rate source

**Question:** How is the reimbursement rate determined?

**Fields used:** Configuration Conditions; Required Clarification; Decision Criteria.

**Options shown in the prototype:**

- The applicable IRS standard mileage rate.
- A company-established rate.
- Another rate or method.
- Not sure.

**What the information does:** Asks where the rate comes from. A company-established rate may require its amount and applicable period.

Rate source alone does not establish payroll tax treatment.

## Follow-up question 2: Mileage records and substantiation

The example flow shows the user selected **the applicable IRS standard mileage rate** in the previous question.

**Question:** How does your company document and verify reimbursable business mileage?

The records need to show business miles, dates, and business purpose—not only how trips are tracked. Confirm whether the process also requires people to account for business mileage and return excess reimbursement within the required time limits. These facts support the review; they do not establish tax treatment on their own.

**Options shown in the prototype:**

- We verify business miles, dates and business purpose, and require excess reimbursement to be returned. People account for business mileage and return any excess within the required time limits.
- We verify the mileage records, but haven't confirmed how excess reimbursement is handled.
- Our records or review process don't cover all of these details.
- We're not sure.
- Something else.

**What the information does:** Uses the selected answer to record whether miles, dates and business purpose are verified and excess reimbursements are returned within required time limits.

A trip log, manager approval or GPS report alone does not prove accountable-plan qualification.

## Information contributors: who contributes what?

| Contributor | Proposed contribution |
| --- | --- |
| Implementation Specialists | Help define configuration conditions, required clarification, and decision criteria. |
| Engineering | Review whether the proposed schema contains the information the application needs. |

These contributions are proposed for discussion; team ownership is not finalized.

## Schema review: feedback

| Topic | Review question |
| --- | --- |
| Missing information | Are there any fields or details missing from the proposed reference library? |
| Unnecessary fields | Are we collecting anything the application does not actually need? |
| Overlapping fields | Do any proposed fields capture the same information? |
