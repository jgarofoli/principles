# Principles

Operational specifications for technical communication, epistemic hygiene, and decision-making. 

These documents are designed to be used both as human field guides and as system instructions for language models. They prioritize local completeness, front-loaded conclusions, and explicit separation between measured facts and model inferences.

## Contents

### Cold-Start Communication
Protocols for writing and reviewing documents where the reader (human or model) arrives with no active state in working memory.

* **[Cold-Start Communication (`cold-start-communication.md`)](cold-start-communication.md)** — Writer specification (v1.1). Covers mode-scaled completeness, BLUF hierarchy, dense evidence (Tufte), referential strictness (ASD-STE100), 6-state epistemic labeling, authorship tracking, and decision falsifiability.
* **[Cold-Start Reader Triage (`cold-start-reader-triage.md`)](cold-start-reader-triage.md)** — Reviewer specification (v1.0). Covers stakes-based calibration, the two-pass reading reflex (30-second structural gate + claim audit), diplomatic markup comments for human authors, and LLM triage prompts.

## Usage

* **For humans:** Use the mode tables and checklists to write self-contained technical artifacts or to audit incoming RFCs, post-mortems, and decision memos without paying unnecessary cognitive tax.
* **For language models:** Load `cold-start-communication.md` into your system prompt, `CLAUDE.md`, `GEMINI.md`, or persistent agent skill to enforce calibrated, non-sycophantic, high-density outputs across interrupted sessions.
