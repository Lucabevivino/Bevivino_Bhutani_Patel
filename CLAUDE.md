# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Project

**RacePulse** — Software Engineering 2 project, Politecnico di Milano, A.Y. 2026-2027.

A distributed platform that collects Formula 1 racing-session data from
[OpenF1](https://openf1.org/) and exposes it to a large, heterogeneous population of
consumers — racing enthusiasts and third-party applications — through both a GUI and an API.
Consumers subscribe to only the subset of information they care about.

- **Team (3 students → full scope):** Luca Bevivino, Liza Bhutani, Jeel Patel
- **Reference professor:** Matteo Camilli (GitHub: `matteocamilli`)

The two assignments and their full rules are in the repo root:
`Assignment RDD AY 2026-2027-1.pdf` (Requirements & Design) and
`Assignment IT AY 2026-2027-1.pdf` (Implementation & Testing).

## Deliverables & deadlines

| Deliverable | What it is | Deadline |
|---|---|---|
| RASD | Requirement Analysis & Specification Document | 23/12/2026 |
| DD | Design Document | 06/01/2027 |
| ITD | Implementation & Test Deliverable (code + doc) | 31/01/2027 |
| ATD | Acceptance Test Deliverable (on another group's prototype) | 07/02/2027 |

All deadlines expire at 23:59 Rome time. Final PDFs go in `DeliveryFolder/` named
`RASDv1.pdf`, `DDv1.pdf`, `ITDv1.pdf`, `ATDv1.pdf` (later fixes → `v2`, `v3`, … plus a
section describing the changes).

## Scope (3-person group → the full problem statement)

For **R&DD**, all of Sections 6.1–6.4 of the RDD assignment apply (including expected scale
and evolution). For **I&T**, the implementation covers **historical OpenF1 data only**
(live data is out of scope), focused on **car telemetry, weather, and race-control data**,
end-to-end: acquisition → persistent storage → API + GUI query/inspection → replay of
historical sessions. Consumers must be able to select information of interest.
Non-functional focus: **response time** and **evolvability**.

## Key domain facts to honor (from the assignment)

OpenF1 is an *external data provider*; telemetry/location arrive at ~3.7 samples/sec/car.
Initial-deployment targets (document and design against these):

- ≥ **10,000** simultaneous human users + up to **100** external software systems, with very
  heterogeneous subscriptions.
- ≥ **95%** of live info delivered within **2 s** of reception; red-flag/safety-car
  race-control events within **1 s**.
- Peaks must not cause uncontrolled data loss — short delays are preferred over silent drops.
- Live distribution service available ≥ **99.9%** of a session; one consumer's failure must
  not affect others.
- Detailed race info retained ≥ **5 years**; historical queries return first results within
  **3 s** (complex telemetry analyses may take longer).
- Evolution: ~**10×** consumers without fundamental redesign; new data sources, analyses, and
  consumer categories; possibly other racing series and extra physical devices.

## Repository layout

- `RASD/` — Requirement Analysis & Specification Document (LaTeX)
  - `chapters/` (one `.tex` per chapter), `alloy/` (`.als` model), `figures/`, `structure/`
- `DD/` — Design Document (LaTeX): `chapters/`, `figures/`, `structure/`
- `IT/` — prototype: `backend/`, `frontend/`
- `ITD/` — Implementation & Test Deliverable (LaTeX): `chapters/`, `figures/`, `structure/`
- `ATD/` — Acceptance Test Deliverable (LaTeX): `chapters/`, `figures/`, `structure/`
- `DeliveryFolder/` — versioned final PDFs
- `PROJECT_NOTES.md` — living scratchpad (not graded; keep it updated between sessions)

## Required document structures (must follow)

**RASD** (IEEE-derived):
1. Introduction (Purpose · Scope · Definitions/Acronyms/Abbreviations · Revision history ·
   Reference Documents · Document Structure)
2. Overall Description (Product perspective incl. scenarios + domain model as class/state
   diagrams · Product functions · User characteristics · Assumptions/dependencies/constraints)
3. Specific Requirements (External interfaces: User/Hardware/Software/Communication ·
   Functional requirements: use-case diagrams, use cases, sequence/activity diagrams, mapping
   to requirements · Performance · Design constraints · System attributes: Reliability,
   Availability, Security, Maintainability, Portability)
4. Formal Analysis Using Alloy (objectives, model, what it proves, worlds/assertion checks)
5. Effort Spent
6. References

**DD**:
1. Introduction (as above)
2. Architectural Design (Overview · Component view · Deployment view · Runtime view ·
   Component interfaces · Selected styles/patterns + rationale · Other design decisions)
3. User Interface Design
4. Requirements Traceability (RASD requirements → design elements)
5. Implementation, Integration and Test Plan
6. Effort Spent
7. References

**ITD** (document): front page (team + link to installable code + link to source) ·
Introduction/scope · implemented requirements (with rationale for exclusions) · adopted
frameworks (languages, middleware, any API not in DD — each with pros/cons) · source-code
structure · testing (esp. system-test cases + outcomes) · installation instructions ·
Effort Spent · References. Code must be installable/runnable on GitHub with a README and an
open-source license.

**ATD**: front page · identification of the other project · installation setup (problems,
incoherences found) · acceptance test cases + outcomes (with motivation) · any additional
points · Effort Spent · References.

## Working conventions

- **Documentation is the most important deliverable** — prioritize it over code.
- Documents are LaTeX, one `.tex` file per chapter, compiled to PDF into `DeliveryFolder/`.
- Alloy models must actually be validated in the tool; report generated worlds / assertion
  checks in the RASD (screenshots or exported worlds).
- Every document needs a **front page** (title, version, names, date) and a **TOC** covering
  the first three heading levels.
- **Generative-AI disclosure is mandatory**: RASD, DD, ITD and ATD must each include a
  dedicated section naming the tools used, the inputs/prompts given, the outputs obtained, and
  how outputs were verified, refined and integrated. You are responsible for and must be able
  to defend all content; never copy unverified LLM output into the graded documents.
- Each member must commit; track **hours per member per document** (Effort Spent sections and
  `PROJECT_NOTES.md`).
- Resolve ambiguity in the problem statement explicitly and record choices + rationale
  (document assumptions clearly).
- Code must be properly commented; prefer stability/quality over feature count.

## How to work in this repo (for Claude Code)

- Read the relevant assignment PDF (`Assignment RDD…` / `Assignment IT…`) before drafting
  content for that document; text extraction works via `pdftotext -layout <pdf> out.txt`.
- Keep the actual deliverable content in `RASD/chapters`, `DD/chapters`, etc.; put
  cross-session working notes in `PROJECT_NOTES.md`, not in the graded documents.
- When a requirement, design decision, or domain assumption is settled, record it in
  `PROJECT_NOTES.md` so it survives context resets.
- Use a mix of natural language, UML (PlantUML or another tool the team standardizes on), and
  Alloy for the RASD/DD; keep diagram sources so they can be regenerated and versioned.
