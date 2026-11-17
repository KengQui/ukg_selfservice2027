# Use case: An AI-first design pipeline, from ticket to engineering handoff

> **DRAFT for portfolio.** Anonymized by default (no company-internal names, ticket keys, customer names or colleague names).
> Items marked **[CONFIRM]** need your input before publishing. The author-only appendix at the end lists sources and must be deleted before this goes public.

## Summary

I co-designed a gated, multi-agent design pipeline that runs inside Claude Code and takes a design project from a Jira ticket and PRD to a coded engineering handoff. It encodes a design organization's standards (design system, personas, domain rules, quality gates) once, so every project starts from the same context and clears the same bar. I then ran it on real work, including a payroll variance-review page that went from a failing first check to a handoff package engineering could apply as a patch.

## The problem

At a workforce-management and payroll software company, every design project began the same way: dig through tickets, find the PRD, check usage data, set up kick-offs, review prior research. The team's own estimate was **2-4 weeks of gathering before a single pixel was designed**, and the handoff at the end (annotated specs, grooming calls, mapped states) left room for interpretation and rework.

AI tools helped individual designers, but they ran in isolation. The designer carried context from one tool to the next, and output quality depended on who ran the tool. The company's standards weren't written down anywhere an AI could use them.

## My role  **[CONFIRM: exact scope]**

I co-designed the pipeline with another designer over **12 days (17-28 April 2026, part-time)**, starting from two open-source projects: BMAD (a markdown/TOML framework for AI agent personas) and Impeccable (design critique/audit skills). I owned **[CONFIRM: e.g. the orchestrator, the quality gates, the handoff contract, the gap and V1 analysis]**. I presented it to design leadership and, later, to the wider product, design and engineering triad.

## Design approach

1. **Encode standards once (the "harness").** Design system, design principles, personas, domain rules, terminology, product landscape and brand voice live in config files every agent reads before it does anything. Context is the project brief; the harness is the onboarding.
2. **Triage sets depth. Gates never bend.** Work is classified Light, Medium or Full, which changes how much research and how many design variations run. The three mandatory quality gates run at full rigor at every level. Triage is also partly a cost control.
3. **Handoff files, not memory.** Each agent writes a handoff file and reads only what came immediately before it. A separate checkpoint file carries pipeline state, so a fresh conversation can resume at each gate.
4. **A human at every decision.** The designer picks the direction, decides what matters and approves the result. Agents must present findings and wait for explicit confirmation before a step advances.
5. **Say what wasn't checked.** Every report states what ran and what didn't.

## The pipeline

| Step | Role | What it does |
|---|---|---|
| Triage | n/a | Right-sizes the run (Light / Medium / Full) |
| Gather | Analyst agent | Pulls tickets, PRDs, analytics, compliance flags |
| Synthesize | PM agent | Themes, requirements, constraints |
| **Gate 1** | n/a | Research complete? (PASS / CONCERNS / FAIL) |
| Architecture | Architect agent | API contracts, data model, risks (added after late-surfacing issues) |
| Design spec | UX designer agent | Structured spec with early wireframe |
| Explore | Party Mode + sandbox | Real subagents debate; 3-4 HTML directions |
| **Gate: direction** | n/a | Human chooses; tradeoffs documented |
| Design + evaluate | n/a | Prototype, then harden, audit, heuristic eval, critique |
| **Gate 2: polish** | n/a | READY FOR HANDOFF or NEEDS WORK |
| Ship | Engineer agent | Coded handoff mapped to production components |
| Docs, Measure | Writer / Analyst | Microcopy and release notes; post-launch loop |

What I built on top of stock BMAD: 6 agent personas, 4 new skills (triage, quality gate, design sandbox, orchestrator), 6 modified skills, 3 mandatory gates, 7 steps, 2 design-system profiles and 4 domain templates. All configuration; no compiled code.

## Use case 1: A payroll variance-review page

**The ask:** "create the filter page with all the variances", for a payroll analyst who reviews thousands of pay variances. I ran the design-only path (research and architecture skipped by choice) and the page was built in the company design system.

**What the gates did to the design**

| Check | First result | After my decisions |
|---|---|---|
| Harden (states and edge cases) | NEEDS WORK, 58% | 18 of 19 states covered (95%), one accepted limit |
| Audit (tokens, components, accessibility) | FAIL | No open FAIL; overall CONCERNS, exceptions documented |
| Critique (100-point rubric) | n/a | Grade B, 86/100 (re-scored after my input) |
| Polish gate | 0 FAIL, 3 CONCERNS (limit 2) | READY FOR HANDOFF after I ruled two items were app-level, not feature-level |

**Findings the prototype surfaced that nobody had listed.** Building against realistically sized data exposed situations missing from the brief:
- Ticking one filter group could generate dozens of chips and push results off screen. Fix: two rows, then a "Show more (N)" control.
- Rows ticked and then hidden by a later filter could still receive bulk actions. Fix: un-tick them and say how many.
- Severity was conveyed by colour alone on the cards. Fix: the severity word leads the tag.
- Rows that can't be actioned (for example a missing employee) needed a disabled state with a reason.
- A phone layout, plus loading, error and no-match states, were missing from the first build.

**How I worked:** I compared against the production app's real reports and the component showcase (and read the code behind them) before accepting a pattern, so the prototype copied behavior that already existed and added only what was missing. One decision example: bulk buttons collapse into a menu when space runs out, matching the production toolbar, instead of a split button that would imply a single main action.

**The handoff changed.** I replaced the generic engineering-handoff step with a conversion that rebuilds the finished page on the real component library and packages a story, a design contract, a gap report and a patch that applies cleanly. In a throwaway copy: **86 of 86 unit tests passed**, lint had 0 errors, and the patch applied at the base commit. I listed what was **not** run (real app build, backend tests, security scans, a screen reader).

## Use case 2: Ticket to tested prototype, with receipts

I timed the full skills chain on one real ticket (adding self-service photo upload to the web app):

| Step | Time | Est. cost |
|---|---|---|
| Ticket to PRD | ~2 min | ~$1 |
| Working POC (real components, not a mockup) | 54 min | ~$50 |
| UX pipeline (triage to Party Mode) | 28 min | ~$34 |
| Design iteration and delivery | 1h 13m | ~$71 |
| **Total** | **2h 37m** | **~$156** |

The POC drew reactions ("this feels busy"), so the pipeline built three placement options as working code. A hub tile was chosen and clicking your own avatar was kept as a fast path. A detail review then found 4 critical and 4 important gaps (for example, no keyboard pan and no error path). I bundled only four cheap fixes into the next pass and flagged the rest for decision, to avoid scope creep. A third check, a wallet-widget prototype, scored **A (91/100)** with 18 of 20 edge cases covered on the first pass.

## What I learned

- **Cost is real.** The ticket-to-prototype run cost ~$156, well above my earlier $15-25 estimate for a pipeline run, because it included the POC and iteration. I now present cost as an open lever, not a solved one.
- **Don't trust generated claims.** On the variance page, helper-agent reports were wrong twice; my checks against the real source caught both. Verify before writing claims.
- **Stale specs mislead.** The first design spec described an early build, so the harden check ran against outdated intent. I refreshed it before the later gates.
- **A strict rule can fail correct code.** The audit's token rule rejected valid component code because it expected one token format. I documented a fix before handing work to engineering.
- **Personas are soft control.** They shape tone, but only the gates enforce quality.

## Where it went next

I listed 15 known gaps in four areas (cost and memory, cross-team dependencies, adoption, pipeline maturity) and proposed an 8-week V1: governance, skills, agent foundation and team collaboration, piloted on 3-5 real stories with engineering consuming the handoffs. The pipeline then became part of a wider product-design-engineering process where design co-defined the gates and handoffs.

---

# AUTHOR-ONLY APPENDIX (delete before publishing)

## Confirm before publishing

1. **Your exact role.** The source decks credit two designers building together, and the presenter script says the co-designer revised the agents' design-system context. State what you owned. Credit your co-designer **[CONFIRM: name and whether they're OK being named]**.
2. **Employer naming and approval.** Every source deck is marked PRIVATE AND CONFIDENTIAL. Confirm you may show this at all, and whether to name the company.
3. **Numbers you can't source from a deck.** "2-4 weeks before design" is the team's own estimate, not a measurement. No before/after time-to-handoff data exists yet in what I read.
4. **Variance page figures** (58%, 95%, 86/100) come from your session notes and presentation brief. The brief itself says the harden 95% was updated by hand rather than re-run, and that the audit and polish reports weren't reconciled. Say "self-scored" if you publish them.
5. **Anything marked "Not run"** should stay in the write-up. It's part of why the case is credible.

## Sources used

| Claim | Source |
|---|---|
| 12 days, 6 agents, 4 new / 6 modified skills, 3 gates, 7 steps, problem framing, harness, triage, gaps, V1 ask, wallet 91/100 and 18/20 | `~/Downloads/ux-pipeline-presentation.pptx` (5 May 2026) |
| Photo-upload timing, tokens, cost; placement options; detail-review findings; 49-skill toolkit | `~/Downloads/UXDLC_Overview_and_testing_ds.pptx` |
| Three use cases run in a week; harness explanation | `~/Downloads/Presentation Script — Gaps + V1.pdf` |
| Variance page checks, decisions, handoff, 86/86 tests, lessons | `payvaragent/_bmad-output/session-handoff.md`, `.../presentation/presentation-brief.md`, `variance-review-ready/README.md` |
| Config and token-rule problems | `selfsvc-hcm-implementation/_program/pipeline-playground-integration.md` |
| Downstream triad / Agentic SDLC | `~/Downloads/design-SDLC.pptx`, `Agentic_SDLC_*.pptx`, `UX Monthly All-Hands - June2026.pptx` |

## Not yet read (would strengthen the case)

`~/Projects/kq-design-ops/` (session notes from April to June, `docs/bmad-changes.md`, `docs/bmad-learnings.md`, per-agent pipeline pages, demo videos, and git history to document your authorship), the speaker notes' detail beyond what's quoted, and `The_AI_Delivery_Operating_System.pptx` (image-only).
