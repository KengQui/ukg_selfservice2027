# Topic 1: Additional Earning & Deduction Configuration Requirements (WAOT Workshops)

## Goal

Agree with PMs on two linked things before we approach engineering. Topic 3 has been merged into this page.

1. The additional earning and deduction configuration requirements from the weighted average overtime (WAOT) discovery sessions, and who
writes each setup question (a person, a person-written bank where AI picks, or AI).
2. What information to capture during earning, deduction and tax setup so downstream workflows work: pay calculation, parallel testing and
historical testing. This needs input from the domain teams.

They are on one page because they share the same open item: for several areas we don't yet know which facts to collect at setup, and the same domain
teams hold the answer.

Evidence status: WAOT has had discovery sessions with service advisors and solution consultants. Imputed income has not and needs discovery (Part
B). Anything about imputed income below is a question for discovery, not a finding.

## Part A: Who writes each setup question

### Proposal: three tiers

Tier                       Who writes the question                                      Use when

1. Fixed                   A person writes the wording, options and order.              Exact wording or a mandatory sequence matters, and the questions
are known in advance.

2. Authored bank, AI       A person writes every question. AI only picks which          The facts to collect are known, but which ones apply depends on
selects                    one to ask next.                                             earlier answers.

3. AI-written follow-      AI writes one targeted question.                             The missing fact is open-ended and can't be listed in advance.
up

Decision rule: use the most controlled tier that any one criterion requires. Tier 2 is the default for structured payroll questions. Tier 1 is the exception. Tier
3 is a narrow fallback.

### Five criteria for placing a topic

Criterion                             Tier 1                  Tier 2                                          Tier 3

Wording sensitivity                   Exact wording matters   Approved wording matters                       Flexible wording is safe

Answer structure                      Controlled choices      Known questions, conditional relevance         Open-ended explanation

Question sequence                     Mandatory order         Conditional order                              No required order

Risk of misunderstanding              High                    Moderate, manageable with approved questions   Low

Are the questions known in advance?   All known               Known, relevance varies                        The missing fact can't be anticipated

Whether an answer affects payroll is not the deciding factor on its own. A question can influence pay and still be AI-written if it only collects a factual
explanation. A harmless-looking question needs fixed wording if ambiguity could cause a wrong setup.

### Controls for AI-written questions

1. One missing fact per question, no compound or leading questions.
2. Grounded in a real uncertainty in the customer's data.
3. No legal or tax conclusions. AI never decides whether an earning is taxable or counts toward the regular rate.
4. A maximum number of AI questions per code, then the case goes to review.
5. A vague answer is never treated as sufficient.
6. Save the question, answer, triggering evidence, model version and resulting decision.

For unknown codes, start with a neutral authored question ("What does this code pay for?"), then use AI follow-ups only for unexpected answers. The
prototype uses this pattern for MISC7, ADJ2 and cellphone.

### Where each area lands

Area                             Tier                    Basis

WAOT screening questions         1                       Expert-reviewed wording and order, shown exactly as approved with no runtime AI rewriting.
The questions encode Ready rules customers don't know. See Part A2.

WAOT: split into straight-       1                       Not a choice. We tell the customer what we set up. Wording and example are authored.
time and half-time codes

WAOT: how overtime               1                       Two fixed layouts with a sample pay statement.
appears on the pay
statement

WAOT: pay period eligibility     1, as a rule-based      A hard rule decides whether lookback is possible.
(weekly or biweekly)             check

WAOT: which earnings             1                       Customer decides, with no preset or recommendation. Wording refers to legal guidance.
count toward overtime

Overtime rate (0.5×, 1.5×,       1                       Known options. A wrong answer double-counts or underpays.
2×, flat)

Tax confirmation questions       1                       AI never confirms tax treatment.

Retroactive pay                  1                       Original period and earning type matter.

Imputed income (fringe           Needs discovery         No discovery yet. See Part B.
earnings)

Leave, shift differential        2                       Questions are known. The reference's roll up type decides which apply.

Cellphone, reimbursements        2                       Which missing fact to ask next depends on earlier answers.

Unknown codes (ADJ,              2 for the opening,      A neutral authored opening avoids steering AI toward a guess.
SPECIAL, MISC)                   then 3 for follow-ups

Typed "Something else"           3                       A person's free text needs a tailored follow-up.
answers

Wording for the customer call    3                       Starts from approved questions and only rephrases.

This is a general product-design recommendation, not a verified Ready rule. Specific assignments need review against the real roll up types, jurisdictions
and approved questionnaires.

## Part A2: Evidence from the WAOT discovery sessions

Why WAOT questions need to be written and reviewed by people:

1. The questions encode Ready rules customers and AI wouldn't know. WAOT lookback needs overtime split into a straight-time code and a
half-time (average 0.5) code. Service advisors reported that most customers pay overtime on one time-and-a-half code and answer questions that
way, which would produce a wrong setup. Only one WAOT half-time code can exist. Cost centers track separate departments on that one code.
2. Experts changed the questions in every session. Examples: the fixed-salary question moved to last, a duplicate question was removed, the
same-period bonus question was reworded to cover employees who also earn overtime, and bonus type and discretionary questions moved into
the WAOT section. Each change came from domain knowledge that was not in the first draft.
3. Some steps are not choices. The straight-time and half-time codes are set up automatically and the customer isn't asked to name them. The
customer does choose how overtime appears on the pay statement.
4. There are hard stops. Lookback only works on weekly or biweekly pay periods. WAOT is still calculated weekly. A customer on a semi-monthly
or monthly payroll who needs lookback probably can't self-implement. Product still needs to confirm the self-implementation behaviour.
5. Two configuration paths exist. Pay earned and paid in the same period is handled on the time side. Pay earned over several periods and paid
later is handled on the payroll side by lookback. Customers should not have to learn the word "lookback". We explain that we split the code.
6. Wording is controlled, and the terminology rule is decided. We use industry terminology (for example "non-discretionary" and "premium
portion") and explain it with an example for users who are not familiar with the terms. This applies as one convention across all configuration. See
the decision log.
7. The product doesn't give legal advice. There is no preset include/exclude list for the overtime calculation. The customer decides, with guidance
to consult their legal team or state labor rules.

### Items to confirm

Does the screening stop at the first Yes? The prototype stops. In one session the group said the remaining questions should still be asked
after a Yes. This needs a decision.
What happens for semi-monthly and monthly payrolls that need lookback? A stop with a handoff to the implementation team is the working
assumption.
The bonus question is asked when the overtime answer is 0.5×, 1.5× or 2×, but not for a flat amount. Confirm this trigger.

## Part A3: Reference library alignment

Sources: Earning and Deduction Reference Library and Earning and Deduction Reference Library: PRD Context (Child), both written from meeting
recordings on 9 and 16 October 2026. Transcripts were auto-generated, so names and terms may be mis-transcribed.

### What the reference library already defines

Roll up type is "the Ready category a code maps to". It is stored in each library entry's classification, together with the code name and aliases,
earning or deduction, and W-2 box.
Roll up type is the anchor. It is identified first, because it drives taxability, reporting, limits and most other settings. If the roll up type is known,
no further questions are asked.
A payroll domain expert defines the roll up types and the questions to ask. The implementation team vets the questions. Payroll engineering
asked for the questions to be validated through the usual discovery process with implementation.
Facts are stored, not generated. The AI never decides a fact or overrides one.
Questions are stored as short text, and AI rewrites them in plain language at runtime.
AI is called only to phrase a question, choose a follow-up when the answer isn't deterministic, or explain an unknown or ambiguous code.
Matching, classification and facts are deterministic.
Tax and benefit questions ask the user to confirm. The system never gives advice, and the result is flagged "tax to be verified" until confirmed.
Codes that don't map to any roll up type are escalated to an implementation specialist or support. This is a proposal.
Scope is US only for now. State is captured at company level and used only when a question needs it.

### How this maps to the tiers

The library's model is closest to Tier 2, with one addition the tiers above don't name: AI rephrases a stored, human-written question at runtime. That is
acceptable where wording is flexible.

Decision: WAOT questions are shown exactly as the experts approved them, with no runtime AI rewriting. The WAOT discovery found that
wording must be fixed and expert-reviewed, and a paraphrase could drop the industry terms and examples the experts asked for. This is a design decision
and doesn't need PM approval. Engineering needs to know that the library must be able to mark a question as "show verbatim".

### Still open in the reference library

Roll up type source of truth. Roll up types exist in production but are not documented, and there is no clear way to extract them.
Ownership of library data. Who updates it as new codes appear, for example group term life rates.
How the questions are validated.
Whether AI can generate and evaluate questions from the library. The page lists this as an assumption and says feasibility was not
discussed.

## Part B: Imputed income needs discovery

No discovery session has been held for imputed income. The prototype's imputed income questions are sample content and are not a basis for design
decisions. No tier is proposed until discovery is done. The reference library page agrees: imputed earnings are listed as unresolved, and the
implementation team reviews them as a complex case.

### Input from the implementation team

Source: Hope Davis, posted in the team channel on Friday 6 November 2026. This is a written explainer, not a discovery session. It is reproduced here as
received.

What are fringe earnings? Fringe earnings, also known as fringe benefits or imputed income in certain situations, represent
additional compensation or benefits provided by an employer beyond an employee's regular wages. Depending on the benefit and
applicable tax regulations, these amounts may need to be reported as taxable compensation even though the employee does not
receive the amount as additional take-home pay. Common examples of fringe earnings include: - Group-Term Life Insurance: The
taxable value of employer-provided group-term life insurance coverage exceeding the applicable IRS exclusion. - Personal Use of a
Company Vehicle: The taxable value associated with an employee's personal use of an employer-provided vehicle. - Employer-
Provided Benefits: Certain taxable allowances, reimbursements, or employer-paid benefits. - Noncash Compensation: Taxable gifts,
awards, or other noncash benefits provided to employees.

How does UKG Ready handle fringe earnings? UKG Ready allows organizations to configure earning codes to track and report
different types of compensation, including taxable fringe benefits. When properly configured, these earning codes can be used to: 1.
Record the taxable value of the benefit. 2. Apply the appropriate federal, state, and local tax treatment. 3. Include applicable taxable
amounts in payroll and year-end reporting. 4. Track fringe earnings separately from regular wages.

Will fringe earnings increase an employee's paycheck? Not necessarily. Many fringe benefits are noncash compensation. For
example, if an employee receives a taxable fringe benefit valued at $200, that amount may increase the employee's taxable wages
by $200 without increasing their cash payment by $200. Depending on the earning code configuration, applicable taxes may be
withheld from the employee's other cash wages, potentially reducing their net pay.

Important considerations The tax treatment of fringe benefits varies depending on the type of benefit and applicable regulations.
Employers should confirm which benefits are taxable, how they should be valued, and how they should be reported. UKG Ready
earning codes must be configured appropriately to ensure accurate payroll calculations and year-end reporting. This means, the
taxability needs to be configured in the earning code. This is one of the steps in setting up a fringe earning.

### What this adds

Taxability is configured in the earning code, as one of the fringe earning setup steps. This is a fact to capture at setup (Part C). It also fits
the rule that AI never confirms tax treatment.
Cash versus non-cash. A fringe benefit raises taxable wages without raising take-home pay, and taxes may come out of the employee's other
cash wages. Customers outside payroll could misread this, so question wording will need to explain it with an example.
A broader list of examples than the prototype. The list covers group term life, personal use of a company vehicle, taxable allowances and
employer-paid benefits, and gifts or awards. It doesn't mention non-dependent health coverage or housing, which the prototype includes.
Discovery should settle which types customers actually have.
A naming question. The implementation team says "fringe earnings", and "imputed income" only "in certain situations". The prototype uses
"imputed income", and the reference library uses "imputed earning". The terminology rule (industry terms with examples) applies, but which of
these terms is the industry term the customer sees is still open.
The customer supplies the facts. The explainer advises employers to confirm what is taxable, how it is valued and how it is reported. The
product should not decide these.
It agrees with the reference library: imputed earnings are set up as earnings in Ready, even where another system creates them as
deductions. The library page marks this as needing confirmation.

### What it does not answer

Whether imputed income counts toward the weighted average overtime regular rate.
Which offsetting deductions are needed, and whether setup order matters.
How each benefit is valued.
Whether group term life and Blocked group term life are separate choices.
Whether the questions should be fixed or should adapt by benefit type.

### Unverified research from the support team's chat agent

These are research notes, not discovery findings, and should be re-checked against the cited articles.

Type                        What the knowledge base said

Group term life (GTL)       Roll-up earning type exists. Setup involves coverage formula and amount, age-basis date, salary basis, coverage reduction,
and the IRS age-based calculation on excess coverage above $50,000.

Blocked GTL                 Separate roll-up that raises taxable wages without withholding federal or state income tax on those wages.

Company vehicle             Listed as a taxable-fringe example. No dedicated roll-up and no valuation method documented.

Other taxable fringe        Custom Taxable Fringe roll-up exists, with a W-2 box type setting.

Non-dependent health        No dedicated imputed income setup documented.
coverage, housing

Not in the knowledge base (not the same as "false"): offsetting deductions per type, setup order, valuation for vehicle and housing, tax selections and
limits per fringe type, and the exact result of each wrong choice.

### Questions for discovery

1. Which imputed income types do customers actually have, and which must self-service setup support first?
2. For each type, which facts must be captured at setup?
3. Does imputed income count toward the WAOT regular rate? Per the WAOT discovery, the product doesn't recommend which earnings count,
so the customer decides. Discovery should still establish what guidance we give. The knowledge base says the Ready amount/hours settings can
not be changed once the feature is used, so the question needs a payroll specialist's input.
4. Are GTL and Blocked GTL distinct choices customers need to make?
5. Which offsetting deductions are required, and does setup order matter?
6. Which industry term does the customer see: "imputed income" or "fringe earnings"? (The terminology rule is decided: industry terms with
examples.)
7. Who supplies the valuation of each benefit, and how does the customer enter it?
8. How do we explain, with an example, that a fringe earning raises taxable wages without raising take-home pay?
9. Group term life is managed by the benefits module, which needs the benefits team, and its rates are maintained by hand (reference
library page). How does self-service setup handle this?
10. Do the facts to collect differ enough by type that the questions should adapt (a likely Tier 2 case), or is one fixed set enough (Tier 1)? Decide this
after questions 1 and 2 are answered.

### Discovery plan

Schedule imputed income discovery, including a payroll specialist and the benefits team
Agree who attends (payroll specialist, tax, deductions, benefits)
Run discovery using the questions above
Place imputed income in a tier using the five criteria
Update this page and the Part C capture table with the findings

## Part C: Information to capture at setup for downstream workflows

### Why this matters

Setup data is only useful if downstream workflows can use it. If we don't capture the right information at setup, customers have to return later, or testing
can't compare against prior results.

The downstream workflows: - Pay calculation - Parallel testing - Historical testing

### Downstream workflow needs

```text
Domain teams to fill in. One row per piece of information.
```

```text
     Information to capture at setup                     Earning /          Pay                Parallel     Historical    Domain       Notes
                                                         Deduction          calculation        testing      testing       team /
                                                         / Tax                                                            owner
```

Which codes were combined, split or removed            Earning                                                          TBD          Raised in discovery: history should let
during setup, and why                                                                                                                historical reconciliation avoid separate
data mapping

Straight-time and half-time codes created for          Earning                                                          TBD          From WAOT discovery
WAOT, and the pay statement layout chosen

Pay period type (weekly, biweekly, semi-               Pay setup                                                        TBD          Decides whether lookback is possible
monthly, monthly)

Taxability configured in the earning code for a        Earning / Tax                                                    TBD          From the implementation team input.
fringe earning                                                                                                                       One of the fringe earning setup steps

TBD, other imputed income items after                                                                                                Needs discovery
discovery

### Domain teams to engage

PM to confirm the list and contacts. The reference library page already lists payroll engineering, the payroll domain expert, the benefits team and the
analytics team as stakeholders.

```text
     Domain                   Team / contact        Engaged?            Date         Notes
```

Pay calculation              TBD                  No

Parallel testing             TBD                  No

Historical testing           TBD                  No

Tax                          TBD                  No

Deductions                   TBD                  No

Benefits (group term life)   TBD                  No                           Benefits-managed deductions and group term life

### Workshop outputs to capture

Fill in from the discovery sessions. One row per requirement.

#     Requirement        Earning / deduction / both          Tier (1, 2 or 3)       Fits current pattern?    Information to capture at setup       Notes

1        TBD

2        TBD

3        TBD

## Open questions for PMs

1. Does the screening stop at the first Yes, or keep asking?
2. What is the behaviour for customers on semi-monthly or monthly payrolls who need lookback? Who confirms?
3. When can imputed income discovery be scheduled, and who should attend (payroll specialist, tax, deductions, benefits)?
4. Which domain teams own each downstream workflow (pay calculation, parallel testing, historical testing), and who is the contact?
5. What is the engagement mechanism (workshop, async questionnaire, review of a draft list)?
6. For historical testing, what prior-period or legacy-system data must be mapped from setup?
7. For parallel testing, what must be captured to compare results against the customer's current system?
8. Which requirements are must-have for the first release?
9. Timeline: when do we need the capture list to unblock design and engineering?

## Proposed next steps

PM confirms domain teams and contacts
Tell engineering the library needs a "show verbatim" option for questions that must not be rewritten
Decide whether screening stops at the first Yes
Confirm the semi-monthly and monthly payroll behaviour with product
Schedule imputed income discovery (see Part B)
Design drafts a straw-man list of information to capture for the domain teams to react to
Agree where roll up type data comes from (source of truth) and who maintains it
Place each remaining requirement in a tier using the five criteria
Consolidate the capture list and agree it with PM before engineering handoff

## Decision log

Date          Decision                                           Decided by             Notes

WAOT           Straight-time and half-time codes are set up       Service advisors and   Customer is told what is set up, with a sample pay statement
discovery      automatically, not asked                           solution consultants
sessions

WAOT           No preset or recommended list of earnings that     Discovery group        Customer decides, with legal-guidance wording
discovery      count toward overtime
sessions

WAOT           Lookback is not explained to the customer          Discovery group        We say we split the code
discovery
sessions

2026-11-09     WAOT questions are shown exactly as the            Keng Qui Chia          Does not need PM approval. Engineering needs a "show verbatim" option in
experts approved them, with no runtime AI          (design)               the library
rewriting

2026-11-09     Use industry terminology, and explain unfamiliar   Keng Qui Chia          One convention across all configuration. Which term to use for imputed
terms to users with examples                       (design)               income versus fringe earnings is still open (Part B, discovery question 6)

## Related pages

Earning and Deduction Reference Library
Earning and Deduction Reference Library: PRD Context (Child)

## Parent

Back to Design & PM Alignment: Earning, Deduction & Tax Setup Next Steps
