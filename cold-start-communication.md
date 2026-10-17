# Cold-Start Communication

v1.1 · Oct 17, 2026 · @Justin

## 1. Scope and modes

This protocol governs any written exchange where the reader arrives with no active memory of the thread: a report, a resumed chat, a recurring brief, a handoff between people, or any turn in a conversation with a language model. It applies in both directions. A model writing to a person follows it so the person can resume cold. A person writing to a model follows it so that everything the model needs is inside its context window, since the model cannot see past that boundary or tell what is missing.

Apply the protocol at a strength matched to the exchange. The modes below are the only calibration the protocol needs; everything else is the same in each.

| Mode | Example | Required | Skipped |
| --- | --- | --- | --- |
| Interactive turn | A question in chat, a quick check, a debugging exchange | Conclusion first. One term per thing. Calibrated uncertainty. | Re-anchor, epistemic tags, tables, the appendix. Expand only on request. |
| Resumed thread | Picking up a topic after days or weeks | Everything in Interactive turn, plus a re-anchor of at most three lines when the reader is a person, or when the thread is a new session for the model. | Epistemic tags unless the turn makes a substantive claim. |
| Document or report | A write-up, a root-cause analysis, a plan, a review, a summary someone will file | Every principle. Local completeness: a reader who has opened none of the sources can resolve every reference. | Nothing. Completeness beats brevity here. |
| Recurring brief | A daily or weekly dispatch on the same topic | Everything in Document or report, plus a one-line invariant header for stable constraints and the delta since the last brief stated as a number or a named change. | Restating full background prose every time; compress stable constraints into the one-line header and state only what changed. |

Out of scope: creative writing, casual conversation, and brainstorming, where premature structure and labels cost more than they return. In those, keep only the habits that cost nothing: say the answer first and do not mislabel a guess as a fact.

## 2. The constraint: cold memory on both sides

A document fails if the reader must recall prior sessions, decode ambiguous references, or hunt for the conclusion. Treat every document as a checkpoint: the reader must be able to read the current state, the change since the last checkpoint, and the decision required, without consulting outside history.

The two parties are cold in different ways. The person returns with no active state and, under time pressure, will not rebuild it even when the record is short. The model holds the whole current conversation and any loaded documents and re-reads them every turn, so within a session it needs no re-anchor; what it lacks is everything outside the window: earlier sessions, documents never loaded, decisions made elsewhere, and anything that has changed in the world since its training. Write to the person for a reader who won't look back. Write to the model for a reader who can't: put what it needs inside the window, state premises rather than implying them, and say when you are relying on something it cannot see.

## Principle I. Conclusion first, then re-anchor

State the conclusion, the key change, or the decision required in the first sentence. For a decision, say who makes it, by when, and what delay costs. Then, in a resumed thread or a document, give the cold reader a re-anchor of at most three lines:

1. Scope and source: what this covers and where it comes from (a title, a file, a prior date).
2. Baseline: the last verified state, marking which items were decided and which were only proposed, and by whom.
3. Headline change since that baseline.

State only the facts the current reply depends on. If the prior record is unclear, say so instead of inventing a baseline. After the re-anchor, use the names it set up; never point back with a bare reference such as "as above" or "the earlier analysis".

A chronological account is not the lead, but it is not forbidden. An incident timeline, a change log, or a reproduction sequence belongs after the conclusion, or in an appendix, where a reader who needs the order can find it.

In an interactive turn the whole principle collapses to one rule: answer, then explain.

## Principle II. Group by argument, not by investigation

Do not narrate how you investigated. Group the supporting material into arguments beneath the conclusion, so that each level summarizes what sits below it and items in one group belong to the same kind. Sibling groups must not overlap. Do not force the groups to be exhaustive when the material does not split cleanly; say which items sit outside the groups instead.

Recommend one option, with the reason in the same sentence. List alternatives only when they are close enough that the reader might reasonably choose differently.

Nest at most two levels. A critical caveat goes at the top level, never inside a sub-point.

## Principle III. Dense evidence, outliers kept

Replace verbal summaries of quantities with the quantities. Report every measurement with its baseline, units, and direction of change, for example `45 ms (−12 ms vs. v1.2; target < 50 ms)`. If no baseline exists, say so. If a comparison exists only in words, say which direction and roughly how much.

Never hide an outlier or a boundary failure inside an average or a category label. Name it, with its value, where the reader will see it before the summary. A summary that reports the mean of a set with one failing member has misreported the set.

Use a table when comparing three or more items across two or more attributes and the cells stay short. Use prose when the comparison is one-dimensional or the cells need a sentence each. Use a chart, not a table, when the point is the shape of a series.

Deep bullet hierarchies flatten the relationships between claims and separate a number from the sentence that gives it meaning. Keep a number next to the claim it supports.

## Principle IV. One term per thing, every reference resolvable

Use one term for one referent throughout a document. Do not rotate synonyms for variety; a reader who sees two names assumes two things. Do not start a sentence with a pronoun whose referent is ambiguous; repeat the noun. A pronoun with one obvious referent is fine.

The first time you use an identifier, say what it is: a commit hash, an issue number, a ticket, a file name, a person's role, an acronym, a model name. A reader who has opened none of the sources must be able to resolve every reference from the document alone. Restate a premise rather than pointing at it.

Keep sentences short in instructions and procedures (about 20 words). Let analysis and argument keep their natural rhythm.

Watch for words that change meaning across a document: "significant", "large", "recent", "fast". Either attach a number or pick one meaning and hold it.

## Principle V. Cut clutter, keep calibration

Every word that does not carry information taxes the reader. Strip throat-clearing ("It is worth noting that", "In order to"), reflexive hedges ("it seems that", "perhaps"), restatements of the question, closing recaps, and meta-commentary about the response itself. Prefer active verbs and concrete nouns; passive voice hides who or what caused a change.

Keep any qualifier that encodes probability or scope. "About 70% likely" carries information; "it seems that" does not. "On the three files I checked" carries information; "in general" does not. The test is whether removing the word changes what the reader would bet on.

Do not pad agreement. When you agree, say so in one clause. Do not pad disagreement either; name the ground for it and stop.

## Principle VI. Label how you know it

A cold reader cannot tell what was measured, what was recalled, what was told to the writer, and what was inferred. Mix them and guesses harden into axioms. In findings, root-cause analyses, reports, and any substantive claim in a resumed thread, tag the claim with one of six labels. The first four say where a fact came from; the last two say the claim is not a fact.

| Label | Meaning | Typical case |
| --- | --- | --- |
| `[Observed]` | The writer ran it, measured it, or inspected the primary source in this session. | Test output, a file read, a query result, telemetry. |
| `[Retrieved]` | Taken from an external source the writer opened and can cite. | A fetched page, a paper, a document on disk. Name the source. |
| `[Recalled]` | From the writer's memory or training, not checked in this session. | A model stating a fact it has not looked up; a person citing from memory. |
| `[Stated]` | Supplied by another party in the exchange and not verified. | The user's description of their setup; a colleague's report; a comment on a thread. |
| `[Inferred]` | A deduction, heuristic, or model output about the real thing. | A diagnosis from symptoms; a simulation result read as a claim about the physical system. |
| `[Projected]` | A forecast, roadmap item, or unconfirmed schedule. | "Ships next quarter." |

Rules that follow from the labels:

- Output from a tool or model is `[Observed]` as a fact about the tool and `[Inferred]` as a claim about the thing the tool represents. Say which one you mean.
- `[Recalled]` is the default for anything a language model asserts without a tool call. A fluent, specific, confident claim is still `[Recalled]` until checked. Treat it as having a shelf life; for anything that could have changed, check before asserting.
- `[Stated]` material keeps its author. "Per the user, the service was restarted at 09:10" preserves the chain; "the service was restarted at 09:10" breaks it.
- Before asserting a claim that can be checked by running it, reading the file, or querying the source, check it, and report what was run and what it returned. A test that passes both before and after a change is not evidence for the change.
- When a claim cannot be checked, give odds or alternatives instead of a confident sentence. "About 60% likely; the alternative is X" is a complete answer. "Probably X" is not.

Apply labels at the sentence or table-row level rather than every clause. In dense tables where space is constrained, `[Observed]` and `[Retrieved]` may collapse into `[Verified]` with an inline source or command. Skip the labels in casual chat and in interactive turns that make no substantive claim. An equivalent two-axis notation (`[Source, Claim type]`) carries the same information but is heavier to write and does not separate Retrieved, Recalled, and Stated, the distinction that matters most when one party is a language model.

## Principle VII. Keep authorship and decision state separate

A proposal is not a decision, and a reader's proposal is not the writer's. Over a long thread these blur: a suggestion the model made is remembered as something the person chose, or a question the person raised is recorded as settled. Prevent it with three habits.

1. Attribute every proposal to its author. "I proposed A; you chose B" is correct. "We went with B" loses who decided.
2. Mark state explicitly: decided, proposed, open. A list of options the reader reacted to warmly is still a list of options until the reader says which one.
3. When citing an earlier statement, quote it exactly. If the exact text is outside the active context or the retrieved record, say the quote is unavailable rather than reconstructing it. A reconstruction presented as a quote is a `[Recalled]` claim wearing an `[Observed]` label.

This principle is the one that most often fails in exchanges with a language model, because the model's own prior output sits in the same transcript as the person's words and reads with the same confidence.

## Principle VIII. State what the conclusion rests on and what would overturn it

A document can be resumable and honest about provenance and still fail the reader who has to act on it. The failure is silent: the conclusion rests on a premise nobody wrote down, or the evidence covers a narrower case than the decision, or the recommendation and the evidence are so fused that the reader cannot reject one and keep the other. Give the decision-maker four things:

1. **Assumptions.** The premises the conclusion depends on, each labeled by source (Principle VI). "Assumes the test environment matches production `[Stated]`" is a sentence the reader can check or dispute.
2. **Gaps.** What was not checked, what could not be checked, and what was looked for and not found. A known gap named is a risk the reader can price; an unnamed one is a surprise.
3. **Options and error costs.** The options actually considered, and what happens if the recommendation is wrong in each direction. A 70% bet reads differently when the downside is a day's rework than when it is a recall.
4. **What would change the answer.** The evidence or event that would reverse the recommendation. This gives the reader a lever other than trust.

Two habits support these. Give ranges, not point values, wherever the width of the range matters to the decision; a point estimate reads as certainty to a human and as a fact to a model. Keep the recommendation in its own sentence, separate from the evidence, so the reader can reject it while keeping the evidence.

The reader's limits differ by kind. A human anchors on the first number, reads a point estimate as certain, and adopts the recommendation without reading the evidence. A model adopts the framing it was given, treats `[Stated]` premises as true, and cannot tell what is absent. Items 1 through 4 address both: assumptions and gaps are the material a model cannot infer, and options and error costs are the context a human skips.

**Placement and budget.** To prevent front-matter from burying the evidence, place this block before the body only in executive decision memos; in analytical reports and recurring briefs, place it immediately after the main evidence table. Treat items 1 and 4 (assumptions, what would change the answer) as mandatory, and items 2 and 3 (gaps, options and error costs) as mandatory only when the decision is costly or hard to reverse.

## 3. Failure vs. compliant, across domains

Each row is a different kind of exchange, to show the principles are not tied to one field.

| Failure (high cognitive tax) | Compliant (cold-start ready) | Principle |
| --- | --- | --- |
| "After going through the logs from Tuesday and trying a few things, it seems like there might be a drift problem." *(the writer measured the drift)* | `[Observed]` Sensor `SA-04` drifted +1.4% over 72 h (baseline: last calibration; tolerance 1.0%). | I, VI |
| The same sentence. *(the writer read the logs but did not measure)* | `[Observed]` Tuesday's `SA-04` logs show the drift signature. `[Inferred]` Drift is about 60% likely; confirm by rerunning the `SA-04` calibration test. | V, VI |
| "I think the tax treatment changed recently, so you should probably refile." | `[Recalled]` The treatment changed in 2024; not checked this session. Confirm against the current guidance before refiling. | VI |
| "You mentioned the API is rate-limited at 100/s." *(the user said this; the writer never checked)* | `[Stated]` Per you, the rate limit is 100 requests/s. If that is wrong, the batching plan below changes. | VI, VII |
| "We decided on the two-column layout." *(the model proposed it; the user said "nice")* | I proposed the two-column layout; you have not chosen between it and the single-column draft. Open: which one? | VII |
| "This caused problems downstream that we should look at before release." | `[Inferred]` The +1.4% drift likely lowers recall in the consumer module; recall has not been measured. Recalibrate `SA-04` before the `v2.4` release. | IV, VI |
| "Results were mostly within spec." *(one of 12 runs failed)* | 11 of 12 runs within spec. Run 7 failed at 2.3× tolerance; cause unknown. | III |
| Three paragraphs comparing four benchmarks on three platforms. | A table: one row per benchmark, one column per platform, each cell the delta vs. baseline. | III |
| A reply to "continue where we left off" that opens with the full history of the project. | One-line answer; three-line re-anchor (scope, baseline with decided vs. proposed, headline change); then the body. | I |
| "We should migrate to the new vendor; their benchmarks are 30% faster and the team is on board." | Recommend migrating; decision is yours by Friday, after which the current contract auto-renews for a year. Assumes vendor benchmarks transfer to our workload `[Stated]`; not checked. Gap: no load test on our data. Options: migrate now, pilot for a month, stay. If wrong: migrating costs about 3 weeks of rework; staying costs about 15% throughput over the year. Would reverse if a pilot shows less than 10% gain. | I, VIII |

## Appendix. Sources and lineage

The principles stand without this appendix; it records where they came from. Entries were cross-checked against historical publication records on 2026-10-17.

| Principle | Source | Claim | Status |
| --- | --- | --- | --- |
| I | Army Regulation 25-50, *Preparing and Managing Correspondence* | Origin of BLUF (Bottom Line Up Front); requires the main point first and active voice. | [Verified](https://armypubs.army.mil/epubs/DR_pubs/DR_a/ARN42124-AR_25-50-007-WEB-13.pdf) |
| I | Churchill, "Brevity" memorandum to the War Cabinet, 9 August 1940 | Short crisp paragraphs, detail to an appendix, no padding phrases. | [Verified](https://www.futilitycloset.com/2026/09/26/summary-advice/) |
| I | FM 6-0, *Commander and Staff Organization and Operations* (May 2022) / ADP 6-0 | Five-paragraph order (Situation, Mission, Execution, Sustainment, Command and Signal) opens with Situation; warfighting function retitled Command and Control in doctrine updates. Used here as the model for re-anchoring, not for front-loading. | Verified |
| II | Barbara Minto, *The Pyramid Principle* (course manuals 1973–1982; first commercial edition 1985; US edition 1987; revised 1996) | Vertical summarizing, horizontal grouping, SCQA as a re-anchoring device. | Verified |
| III | Edward Tufte, *Visual Explanations* (1997), *The Cognitive Style of PowerPoint* (2003) | Challenger: none of 13 pre-launch charts related temperature to O-ring damage. Columbia: the Boeing slide "Review Of Test Data Indicates Conservatism for Tile Penetration" placed "Volume of ramp is 1920cu in vs 3 cu in for test" low in a 6-level, 11-sentence hierarchy; the debris was 640× the test projectile. | Verified |
| III | Robison et al., "Representation and Misrepresentation: Tufte and the Morton Thiokol Engineers on the Challenger" (2002) | Contests Tufte's reading of the Challenger engineers. | Recalled |
| IV | ASD-STE100 Simplified Technical English | AECMA readability project begun 1979 for aircraft maintenance documentation; first release 1986 (*AECMA Simplified English Guide*); renamed ASD-STE100 in 2004. One meaning per approved word; 20-word limit in procedures, 25 in descriptive text. | Verified |
| IV | Tufte on the Columbia slide | "significant" or "significantly" used five times with different meanings. | Recalled |
| V | William Zinsser, *On Writing Well* (1976) | Clutter as a cognitive tax; active verbs enforce accountability. | Recalled |
| VI | NTSB investigation practice | The public docket holds factual information only; analysis, findings, and probable cause appear only in the final report. | Recalled |
| VII | No external source | Drawn from observed failure patterns in long model-assisted threads. | Inferred |
| VIII | No external source | Assumptions, gaps, error costs, and overturning conditions as the minimum a decision-maker needs. Closest analogues are pre-mortem practice and structured decision memos; no single lineage claimed. | Inferred |
