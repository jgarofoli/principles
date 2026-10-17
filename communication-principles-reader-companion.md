# Cold-Start Reader Triage

v1.0 · Oct 17, 2026 · @Justin

## 1. Scope and purpose

This companion to *Cold-Start Communication (v1.1)* governs how to read, audit, and mark up incoming documents that fail cold-start standards.

Do not interrogate a flawed document sentence by sentence. Line-by-line editing of an unanchored draft pays the exact cognitive tax the protocol is designed to prevent. Use this guide to shed cognitive load: calibrate your threshold to the stakes and lifespan of the document, run a 30-second structural gate before reading the body, and translate protocol violations into diplomatic margin comments that force the author to supply the missing rigor.

Unlike the protocol, this companion is not domain-neutral. Its examples assume an autonomous-vehicle simulation and evaluation context (closed-loop simulation, on-vehicle telemetry, planner and policy modules) to make it something concrete and consistent. The mechanisms transfer; the examples do not. If you adapt this guide for a different domain, replace the examples in Section 4 and keep the structure.

## 2. Mechanism I: Calibration

Before marking up a document, set your enforcement threshold along two axes. **Stakes** (how costly or hard to reverse the decision is) sets which principles you enforce. **Lifespan** (how long and how widely the artifact will be read cold) raises the bar on one principle regardless of stakes.

### Axis 1: Stakes

| Stakes | Examples | Enforce strictly | Let slide |
| --- | --- | --- | --- |
| **Low** | Sprint status, PR description for an isolated fix, reversible config change, quick chat update | **Principle I** (What is the ask/status?) and **Principle VI** (Did you run this, or are you guessing?). | Narrative chronology (II), prose vs. tables (III), throat-clearing (V), formal decision frames (VIII). |
| **Medium** | Weekly team brief, component design draft, debugging handoff, experiment summary | **Principles I, III, VI, VII.** Demand explicit baselines/deltas, provenance of claims, and clear attribution of what is decided vs. open. | Minor prose clutter (V) and exhaustive option/error-cost framing (VIII), unless a major trade-off is hidden. |
| **High** | Architecture RFC, root-cause post-mortem, launch gate, vendor lock-in, safety/eval spec | **All Principles (I–VIII).** Zero tolerance for buried leads, hidden outliers, merged sim/real provenance, or unstated reversal criteria. | Nothing. If the document fails the 30-second gate below, stop reading and return it for structural revision. |

### Axis 2: Lifespan

If the artifact will be read cold for more than about a month, or by people outside the thread that produced it (a runbook, an onboarding doc, an eval spec, a design doc that becomes the reference), enforce **Principle IV** (one term per thing, every reference resolvable) strictly, whatever the stakes. Lifespan multiplies the cost of every unresolvable reference: a pronoun that costs one reader ten seconds today costs fifty readers ten seconds each over the year. A low-stakes but long-lived document therefore enforces I, IV, and VI.

The rule of thumb: stakes drive VI and VIII; lifespan drives IV.

## 3. Mechanism II: The two-pass reading reflex

Never read a technical or decision document linearly from sentence one. Execute two distinct passes. Run Pass 1 yourself every time; it is cheap. Delegate Pass 2 to a model when the document is long (Section 5).

### Pass 1: The 30-second gate (structural scan)

Read only the opening paragraph, the section headings, and the tables. Do not read the body prose yet. Ask four questions:

1. **The Ask (Principle I):** Do I know what changed or what decision is required of me, by when, and what delay costs?
2. **The Baseline (Principle I & III):** Is the prior verified state named, and are numbers paired with baselines and units?
3. **The Shape (Principle II):** Are the section headings grouped by logical argument, or are they a diary of how the author spent their week?
4. **The Lever (Principle VIII):** In a decision document, can I see what assumptions the recommendation rests on and what evidence would overturn it?

**Gate rule, by stakes:**

- **High stakes:** If the document fails Pass 1, **stop reading at paragraph two.** Leave a single top-level margin comment requesting the missing structural frame (see Section 4, Row 1) and return the draft. Auditing the sentences of a structurally broken document wastes your time and theirs, because the body will change once the conclusion and assumptions are forced into the open.
- **Medium stakes:** Leave the same structural margin comment, then continue to Pass 2. The body is worth auditing because the document will likely be acted on before the author revises it.
- **Low stakes:** Leave the structural margin comment only if the ask itself is unclear. Otherwise read on.

### Pass 2: The claim audit (line-by-line scan)

Once a document passes the structural gate, read the supporting arguments while scanning for four specific text smells:

1. **De-quantified modifiers ("significant", "fast", "minimal", "mostly"):** Check whether an adjective is hiding a missing measurement, an absent baseline, or an unstated boundary failure.
2. **Merged provenance:** Check whether simulation outputs, heuristic estimates, secondhand reports, recalled figures, and direct physical/production measurements are written in the same declarative voice.
3. **Unanchored references ("this", "it", "the previous approach", rotating terms):** Check whether a cold reader could misidentify which component, version, or metric is under discussion.
4. **Blurred state ("we decided", "the team prefers", "aligned on"):** Check whether a proposal or warm reaction is being silently promoted into a locked decision.

## 4. Mechanism III: The diplomatic markup lexicon

Sending a human colleague a link to a writing protocol reads as pedantic. Asking a targeted question about baselines, failure boundaries, or test environments reads as senior technical leadership. Use the table below to translate protocol violations into actionable margin comments.

| Text smell in the draft | Principle violated | Question to ask yourself | Margin comment to leave the author (choose one) |
| --- | --- | --- | --- |
| **Buried lead / diary structure:** Opens with 3 paragraphs of project history and chronological investigation steps. | **I, II** (Conclusion first; group by argument) | *Do I know what I am being asked to decide or take away yet?* | • "To help cold readers get oriented quickly, could we add a 2-sentence 'Decision Required' (or 'Bottom Line') summary at the very top before the background?"<br>• "Could we move the investigation timeline to an appendix and group the main body by the core arguments supporting your recommendation?" |
| **Missing re-anchor:** Resumes a multi-week thread with "Following up on the earlier analysis..." | **I, IV** (Re-anchor; resolvable references) | *Can a reader who wasn't in the last meeting resolve this without hunting through old docs?* | • "For readers jumping into this doc cold, could we add a 3-line header stating the current baseline, what was already decided vs. proposed, and what changed since the last review?" |
| **Vague quantity:** "Latency improved significantly in the latest run." | **III, IV** (Dense evidence; one meaning per term) | *Compared to what baseline, in what units, and against what target?* | • "What is the exact ms delta here against the `v1.2` baseline, and what is our target threshold?"<br>• "Let's swap 'significantly' for the specific number and baseline so the magnitude is clear at a glance." |
| **Hidden outlier:** "Average error rate across the 12 scenarios was 1.2% (within the 2.0% spec)." | **III** (Outliers kept) | *Did all 12 pass, or did 11 score 0.5% and 1 fail catastrophically?* | • "What was the worst-case scenario across the 12 runs? Did any individual run exceed the 2.0% boundary?"<br>• "Let's surface the max error and any boundary failures alongside the mean so we don't mask tail risk." |
| **Wall of comparative prose:** Three paragraphs comparing 4 models across 3 benchmarks in prose. | **III** (Tables over prose for multi-axis data) | *Am I forced to build a mental spreadsheet to compare these items?* | • "Since we're comparing 4 models across 3 metrics, could we convert this section into a compact table with deltas against the baseline?" |
| **Synonym drift / ambiguous pronoun:** Rotates between "controller", "planner", and "policy", or starts sentences with "This causes..." | **IV** (One term per thing; resolvable references) | *Are these three separate subsystems or one? What exact noun does "This" point to?* | • "Are 'planner' and 'policy' referring to the same module here? Let's pick one term throughout so readers don't assume they are separate components."<br>• "To avoid ambiguity, could we specify whether 'This' refers to the sensor drop or the timeout?" |
| **Reflexive hedging:** "It seems that the memory leak might possibly be related to the cache." | **V, VI** (Cut clutter, keep calibration) | *Is this a 10% hunch or an 80% diagnosis, and what would prove it?* | • "How confident are we in the cache hypothesis (e.g., ~70% vs. ~20%), and what test would confirm or rule it out?" |
| **Unlabeled provenance:** "The new policy resolves the cut-in regression." | **VI** (Label how you know it) | *Was this measured in closed-loop sim, open-loop log replay, or hardware telemetry? Or is it recalled from an earlier run?* | • "Was this verified in closed-loop simulation or on-vehicle telemetry? Let's note the test environment inline so readers know what is proven vs. inferred."<br>• "Is this from a live run this week or recalled from an earlier benchmark? Let's cite the run ID." |
| **Unverified premise:** "Because the downstream API is rate-limited at 100 req/s, we must batch." | **VI, VIII** (Stated vs. Observed; Assumptions) | *Did the author measure this limit, or did someone just tell them that in Slack?* | • "Has the 100 req/s limit been verified against the current production config, or is that an assumption from the vendor docs? If that limit changes, does our batching design change?" |
| **False consensus / blurred state:** "We decided to use the two-column layout." *(when it was only proposed)* | **VII** (Keep authorship and decision state separate) | *Who actually decided this, and when? Or is this still an open proposal?* | • "My understanding was that the two-column layout was proposed last week but still open against the single-column draft. Has a final call been made, or are we deciding that in this review?" |
| **Fused recommendation:** "We should migrate to Vendor B because their throughput is 30% higher." | **VIII** (Assumptions, gaps, error costs, reversal criteria) | *What does this rest on, what wasn't tested, what does it cost if we're wrong, and what would change our mind?* | • "Before we lock this in, could we add a short 'Decision Frame' covering: (1) the key assumptions this rests on, (2) any testing gaps on our own workload, (3) the cost if we're wrong, and (4) what early metric during a pilot would cause us to abort?" |

## 5. Using a model for Pass 2 on long documents

Pass 1 takes 30 seconds; run it yourself. Pass 2 on a long, poorly structured document from a human author, or on a sprawling multi-page report from an external source, is where the cognitive tax lands. Delegate it to a language model using the prompt below, with both the document and *Cold-Start Communication (v1.1)* attached.

Two rules govern the model's output:

1. **The triage block is `[Inferred]`.** The model is reading what the text says the author did; it cannot see what the author actually ran. Its labels are provisional and its gap list is a hypothesis.
2. **Verify before forwarding.** Check any claim in the triage block against the source document before you turn it into a margin comment. A margin comment is your statement to a colleague, attributed to you. Forwarding the model's guess as your own understanding is the Principle VII failure the protocol warns about.

> **Prompt:** "Read the attached document using the attached *Cold-Start Communication (v1.1)*. Do not summarize the prose chronologically. Instead, extract a Cold-Start Triage Block:
> 1. **BLUF / Ask:** State the core conclusion or decision required, who makes it, by when, and the cost of delay. If any of these is missing from the text, write `[Gap: Unspecified]`.
> 2. **Baseline & Delta:** State the prior verified baseline and the quantitative change reported. Flag any qualitative adjectives lacking numbers or units.
> 3. **Epistemic Audit:** List the top 3–5 substantive claims and tag each as `[Observed]`, `[Retrieved]`, `[Recalled]`, `[Stated]`, `[Inferred]`, or `[Projected]`, based on what the text says the author did. Where the text does not say, tag the claim `[Stated]` and flag it. Flag where simulation output, a recalled figure, or an inference is presented as physical fact.
> 4. **Decision Frame Gaps:** Identify unstated assumptions, unmeasured boundary/outlier risks, and what evidence would overturn the author's recommendation.
> 5. **Margin Comments:** Draft 3 diplomatic, high-leverage margin comments I can send to the author to close the largest structural or epistemic gaps.
>
> Your whole output is your inference from the text, not a verified account of the author's process. Say so in one line at the top."
