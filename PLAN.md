# Ewing Registry Standards — PLAN.md

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) ·
> Lane: donated · Risk tier: **medium** (project) with **high**-tier patient/family-facing
> content and consent/ethics surfaces. Slug: `ewing-registry-standards`.

> ### ⛔ BINDING GUARDRAILS — read before any work on this project
> This project produces an **open data *standard* (schema, common data elements, governance,
> interoperability profiles) — NOT a data store.** Hee-Lee Oss **never holds, ingests, hosts, or
> processes any patient record** under this project. The deliverables describe how a *separate,
> appropriately-governed registry operator* could collect Ewing Sarcoma natural-history data
> lawfully and ethically.
>
> 1. **Open-access / aggregate / de-identified only.** Controlled-access resources (dbGaP, EGA,
>    individual-level biobanks) and **any identifiable patient data** are **OUT OF SCOPE** —
>    they require authorized access + IRB and are never touched here.
> 2. **No real patient data, ever** — not in examples, tests, or fixtures. All conformance
>    examples are **fully synthetic** and labelled as such.
> 3. **Per-source license verification is mandatory.** Terminologies (SNOMED CT, LOINC,
>    ICD-O-3, etc.) are **bound by reference, never redistributed.** COSMIC/OncoKB are
>    non-commercial; TCGA/GEO are open — verify and record each before citing.
> 4. **No medical advice.** Patient/family-facing content is **education only**, every assertion
>    **sourced**, carries **"This is not medical advice,"** and is **oncologist + patient-advocate
>    reviewed** (`riskTier: high`) before it ships.
> 5. **Provenance on every assertion** — definition, code-system binding, citation, access date,
>    and license tag for each data element.
> When in doubt, **stop and surface the concern** (CLAUDE.md refusal guardrails).

---

## 1. Executive summary

Ewing Sarcoma is a rare, aggressive cancer of bone and soft tissue that strikes mostly children,
adolescents, and young adults. Because it is rare (~1 case per million people per year; on the
order of 200 new US diagnoses annually), **no single hospital sees enough patients to answer the
questions families most need answered** — how the disease actually behaves over time, which
treatment sequences correlate with better outcomes, what late effects survivors live with, and
why relapsed/metastatic disease remains so deadly. The answers live in **pooled, longitudinal,
natural-history data** — but today that data is fragmented across institutions in incompatible
formats, gated by inconsistent consent, and rarely interoperable or reusable.

This project does **not** build a database and does **not** collect data. It builds the **open,
consent-first *standard*** that lets any legitimate registry collect Ewing natural-history data in
a **harmonized, interoperable, ethically-governed, and reusable** way: a published set of Common
Data Elements (CDEs), terminology bindings, a consent and data-governance framework, and
interoperability profiles (HL7 FHIR / mCODE, OHDSI OMOP CDM, CDISC, REDCap). The public good is
**the standard itself** — released openly so families' data, when they choose to contribute it
elsewhere, is never trapped, never duplicated needlessly, and never wasted.

The hard part is not the schema; it is **doing right by families.** Consent-first, de-identified,
provenance-tracked, and reviewed by clinicians and patient advocates is the entire point. Where we
have not yet secured a partner registry, an ethicist, or a pediatric-oncology reviewer, this plan
says so plainly (`verifiedNeed: false`, `requestor: TO BE SECURED`) rather than pretending
otherwise. **Definition of done is "delivered and adopted," not "merged."**

## 2. Problem & beneficiaries

**Who is helped (in order of directness):**

- **Families and patients facing Ewing Sarcoma** — ultimately, by enabling research that can only
  happen when rare-disease data is pooled and comparable. A harmonized standard shortens the path
  from "data collected" to "question answered," and a consent-first design means contributing data
  is something families *control*, not something done *to* them.
- **AYA (adolescent & young adult) and pediatric survivors** — late-effects and survivorship CDEs
  make long-term outcomes visible, which is where Ewing survivorship is chronically under-measured.
- **Rare-disease registries, patient-advocacy foundations, and academic consortia** (e.g.,
  pediatric-sarcoma foundations, Euro Ewing-type consortia, Children's Oncology Group-aligned
  efforts) — they get a free, vetted, ready-to-adopt standard instead of reinventing one, lowering
  the cost of starting or harmonizing a registry.
- **Researchers and method developers** — interoperable, FAIR-aligned data is reusable across
  studies; the standard reduces the data-wrangling tax that dominates rare-cancer research.

**The verified need:** **TO BE SECURED.** The *general* need (fragmented, non-interoperable rare-
cancer registry data) is well documented in the literature and by NIH/NCATS rare-disease programs.
However, **no specific partner registry, advocacy organization, or clinical reviewer has yet been
secured** for *this* project. Until a named partner confirms they will adopt or steward the
standard, every delivery-dependent task carries `verifiedNeed: false` and `requestor: TO BE
SECURED`. We will not ship patient/family-facing content or claim "adoption" without a real
partner and real reviewers. Securing one partner registry + one oncologist + one patient advocate
is **M0's top non-engineering exit criterion.**

**Why a standard (not a database):** building another siloed database would *add* fragmentation
and pull Hee-Lee Oss into holding sensitive data — squarely against our guardrails. A standard is the
high-leverage, low-risk intervention: it benefits every registry without Hee-Lee Oss ever touching a
patient record.

## 3. Goals and non-goals

**Goals**
- Publish an **open, versioned Ewing Sarcoma natural-history CDE set** with definitions,
  permissible values, and code-system bindings — every element sourced and provenance-tracked.
- Publish a **consent-first governance framework**: dynamic/granular consent, pediatric
  assent + parental permission, withdrawal, de-identification standard, and tiered data-use
  governance (GA4GH Data Use Ontology-aligned).
- Publish **interoperability profiles** so the same data maps cleanly to FHIR/mCODE, OMOP CDM,
  CDISC, and a REDCap data dictionary — adopt-anywhere, lock-in-nowhere.
- Ship **validation tooling + synthetic conformance fixtures** so an adopter can check their
  implementation without writing it from scratch (and without any real data).
- Ship **plain-language family-facing education** (what a registry collects, why, and a patient's
  consent rights) — sourced, "not medical advice," oncologist + advocate reviewed.
- Be **FAIR** (Findable, Accessible, Interoperable, Reusable) and openly licensed.

**Non-goals (explicit)**
- **Not** operating, hosting, or storing a registry or any patient data.
- **Not** collecting, ingesting, or transmitting identifiable or individual-level patient data.
- **Not** using or redistributing controlled-access data (dbGaP/EGA/biobanks).
- **Not** redistributing licensed terminology *content* (SNOMED CT, LOINC tables, etc.) — we bind
  to codes by reference only.
- **Not** giving medical advice or treatment recommendations to anyone.
- **Not** mandating a single technology — we provide profiles for multiple platforms.
- **Not** a clinical trial protocol, an eligibility engine, or a trial finder (those are sibling
  Hee-Lee Oss projects: `ewing-trial-finder`, `ewing-eligibility-structuring`).

## 4. Success metrics (outcomes)

Outcome-based, beneficiary-centric. Vanity metrics (stars, downloads) are explicitly *not*
success.

| # | Outcome metric | Baseline | Target | How measured |
|---|---|---|---|---|
| O1 | A real registry/consortium **adopts or pilots** the standard | 0 | ≥1 named partner pilots within 12 months of v1.0 | Signed adoption note / public commitment from partner |
| O2 | CDEs **mapped to existing standards** (GRDR, caDSR, mCODE) with no orphan elements | 0% | 100% of core CDEs carry ≥1 crosswalk + provenance | Crosswalk file completeness check in CI |
| O3 | Interoperability profiles **validate** against official validators | n/a | FHIR IG passes HL7 validator; OMOP mapping passes OHDSI checks; REDCap template imports clean | CI validation jobs green |
| O4 | Family-facing education **reviewed & comprehensible** | 0 | 100% reviewed by oncologist + advocate; readability ≤ grade 8 | Sign-off log + readability score |
| O5 | **Consent framework** independently reviewed | none | Reviewed by an ethics/IRB-literate reviewer + patient advocate; all findings resolved | Review sign-off log |
| O6 | **Provenance completeness** | n/a | 100% of assertions carry source + access date + license tag | Automated provenance lint |
| O7 | **License safety** | n/a | 0 instances of redistributed restricted terminology content; 100% sources license-verified | License register audit in CI |

A metric we will *report honestly even if it stays at zero*: **O1 (real adoption).** A standard no
one adopts has helped no family, regardless of how clean the schema is.

## 5. Scope

**In scope**
- Conceptual data model of the Ewing natural-history journey (diagnosis → molecular → staging →
  treatment → response → relapse → survivorship/late effects → PROs → mortality).
- Core + extended CDEs with definitions, value sets, units, and code-system bindings.
- Molecular/genomic CDEs for Ewing biology (EWSR1 fusions and variants) expressed via open
  standards (HGVS, HGNC, Sequence Ontology, GA4GH VRS), **schema only — no sequence data**.
- Patient-reported outcomes (PRO) and late-effects element sets via established instruments
  (referenced, license-checked).
- Consent & governance framework; de-identification standard; data-use ontology bindings.
- Interoperability profiles: FHIR (mCODE-aligned) Implementation Guide, OMOP CDM mapping spec,
  CDISC mapping notes, REDCap data dictionary template.
- JSON Schema for the CDE set, a validator CLI, **synthetic** example records, conformance suite.
- Plain-language family-facing education + a registry adoption kit + versioning/maintenance policy.

**Out of scope (explicit — this project will NOT do these)**
- Any data store, hosting, ingestion, or transmission of patient data.
- Any identifiable, individual-level, or controlled-access data use.
- Redistribution of licensed terminology content.
- Medical advice, diagnosis, prognosis tools, or treatment recommendations.
- Trial matching / eligibility evaluation (sibling projects).
- Real-world deployment/operation of a registry instance.
- Regulatory submission packages (we *align* with FDA/EMA registry guidance; we do not file).

## 6. Solution approach & architecture

This is a **data-standards / governance** deliverable, so the "architecture" is a pipeline that
produces and validates *artifacts*, plus the artifact model itself. There is **no runtime that
touches data.**

**6.1 Artifact model (what we ship)**
- `model/` — conceptual model + module map (human-readable spec, Markdown).
- `cde/` — the CDE dictionary as **machine-readable source of truth** (YAML/JSON), one record per
  element: `id`, `label`, `definition`, `dataType`, `unit`, `permissibleValues` (or value-set
  binding), `codeSystemBindings[]` (system + version + code + display, **by reference**),
  `cardinality`, `module`, `coreOrExtended`, `crosswalks[]` (GRDR/caDSR/mCODE/ICHOM), and a
  required `provenance` block (`source`, `url`, `accessedDate`, `license`).
- `terminology/` — value-set *definitions* (which code systems + which codes), **never** the
  terminology tables themselves; plus a per-system **license register**.
- `governance/` — consent framework, de-identification standard, data-use governance, DUO bindings.
- `profiles/` — FHIR IG (mCODE-aligned StructureDefinitions/ValueSets), OMOP CDM mapping,
  CDISC mapping, REDCap data-dictionary template.
- `schema/` — generated **JSON Schema** for the CDE set + the provenance schema.
- `tooling/` — `validator` CLI + `provenance-lint` + `license-audit` + crosswalk-completeness check.
- `examples/` — **fully synthetic** conformance fixtures (clearly labelled `SYNTHETIC — NOT REAL`).
- `education/` — family-facing plain-language materials (reviewed, "not medical advice").

**6.2 Pipeline**
1. Author CDEs in the machine-readable source (`cde/`), each with provenance.
2. **Generate** JSON Schema, the FHIR ValueSets, and tabular views from that single source (no
   hand-maintained duplicates → no drift).
3. **Validate** in CI: JSON Schema lint, provenance-lint (every assertion sourced), license-audit
   (no restricted content redistributed), crosswalk-completeness, FHIR IG publisher/validator,
   OMOP mapping checks, REDCap import test, synthetic-fixture conformance.
4. **Review gates** (human): expert/clinical, ethics/consent, advocate, license.
5. **Release** versioned artifacts (SemVer) with a changelog and migration notes.

**6.3 Tech stack**
- TypeScript + ESM, pnpm workspaces (per CLAUDE.md conventions); Node CLI tooling.
- Authoring source in YAML/JSON; **JSON Schema (draft 2020-12)** as the canonical validation layer.
- FHIR R4 + **mCODE** profiles, built/validated with the **HL7 FHIR IG Publisher**.
- **OHDSI OMOP CDM** mapping (with the OMOP Oncology extension) validated against OHDSI conventions.
- **CDISC** (CDASH/SDTM, Rare Disease therapeutic-area concepts) mapping notes.
- **REDCap** data-dictionary CSV template (the dominant registry capture tool).
- CI: build + test + lint + all validators must pass (matches Hee-Lee Oss `pnpm build && test && lint`).

**6.4 Key decisions**
- **Single machine-readable source of truth** for CDEs; everything else generated. (Avoids the #1
  failure mode of data standards: spec/implementation drift.)
- **Bind, don't bundle** terminologies — license-safe and always current.
- **mCODE-first** for clinical interoperability (it is the emerging US oncology FHIR baseline) and
  **GRDR/caDSR alignment** for rare-disease comparability.
- **Consent and de-identification are first-class artifacts**, specified before any element is
  "blessed" as collectable — privacy is designed in, not bolted on.
- **Profiles, not a platform** — adopt-anywhere, lock-in-nowhere.

## 7. Data, licensing & compliance

**THIS SECTION IS THE PROJECT'S CORE CONSTRAINT — it leads with the binding cancer guardrails
(see the box at the top) and governs everything else.**

**7.1 What data this project handles.** **None that is patient-level.** We handle *standards
artifacts and aggregate/open references only.* We never collect, store, ingest, or transmit
patient data. The standard *describes* fields a registry may collect; Hee-Lee Oss never holds them.

**7.2 Source classes and their handling**

| Source | Use | License/access posture | Handling rule |
|---|---|---|---|
| **GRDR / NCATS rare-disease CDEs** | Crosswalk + reuse of element definitions | Public, US-Gov | Cite + verify reuse terms; record provenance |
| **NCI caDSR / NCIt** | CDE + concept alignment | Open (NCIt is open) | Bind by code; cite version |
| **HL7 FHIR / mCODE** | Interop profiles | HL7 (open, mCODE under HL7 terms) | Reuse per HL7 IP policy; cite |
| **OHDSI OMOP CDM (+Oncology)** | Mapping | CC-BY-4.0 | Attribute; cite version |
| **CDISC standards** | Mapping notes | CDISC license terms (verify) | **Bind/reference only; do not copy controlled content**; cite |
| **SNOMED CT** | Code bindings | **Affiliate license required; member-country free** | **Reference codes only — never redistribute tables.** Document the adopter's licensing obligation |
| **LOINC** | Lab/PRO codes | Free **with** LOINC license + attribution | Reference codes; include required notice; no table redistribution |
| **ICD-O-3 / WHO classifications** | Morphology/topography | WHO terms (verify) | Reference codes; cite; no redistribution of content |
| **ICCC-3 / AJCC staging** | Classification/staging | Verify (AJCC is copyrighted) | **Reference structure only; do not reproduce AJCC tables** |
| **HGNC, HGVS, Sequence Ontology, GA4GH VRS/Phenopackets/DUO** | Molecular + consent encoding | Open | Reuse; cite versions |
| **PRO instruments** (PROMIS, PRO-CTCAE, PedsQL, EQ-5D-Y) | PRO element references | **Mixed — PROMIS/PRO-CTCAE generally free; PedsQL & EQ-5D require licensing** | **Reference + flag licensing obligation; do NOT embed instrument item text unless license confirmed** |
| **COSMIC / OncoKB** | Optional biology cross-reference | **Non-commercial** | Cite only if used; respect NC terms; prefer open alternatives |
| **TCGA / GEO (open)** | Optional aggregate context | Open | Aggregate/open only; cite |
| **dbGaP / EGA / biobanks** | — | Controlled-access | **OUT OF SCOPE — never accessed** |

**7.3 Provenance model (mandatory).** Every CDE, value set, crosswalk, and education assertion
carries a structured provenance record: `source` (citation), `url`, `accessedDate`, `version`,
`license` (SPDX-style tag or explicit terms), and `boundByReference: true|false`. A CI
`provenance-lint` fails the build on any unsourced assertion (O6). Education claims additionally
require a citation to an authoritative source (NCI, COG/SIOPE, peer-reviewed).

**7.4 Privacy / PII stance.** Because we hold no data, the privacy work is in **what we tell
adopters to do**: the de-identification standard mandates HIPAA Safe Harbor *or* Expert
Determination, small-cell suppression / k-anonymity guidance for aggregate release, and explicit
handling for the re-identification risk that is acute in *rare* diseases (a rare diagnosis + age +
geography can be identifying). Synthetic fixtures only; a CI check rejects any fixture not flagged
synthetic.

**7.5 Attribution.** All reused open sources attributed in `NOTICE`/`CITATION.cff` and inline
provenance. Required notices (e.g., LOINC) reproduced verbatim where mandated.

**7.6 Output licensing (our deliverables).**
- **Code/tooling:** **MIT** (Apache-2.0 considered for explicit patent grant — governance
  decision, tracked as open question).
- **Standard docs, CDE dictionary, governance specs, education:** **CC-BY-4.0**.
- **FHIR Implementation Guide:** **CC0-1.0** (HL7 IG convention) *or* CC-BY-4.0 — governance
  decision.
- We license **only our own authored content**; bound terminologies retain their own licenses and
  are the adopter's responsibility (documented in the license register).

## 8. Quality, review & risk gates

**Risk tier:** project-level **medium**; **high** for (a) patient/family-facing education and
(b) the consent/ethics & de-identification framework. Per the Good Deed Definition, **high-tier
artifacts require credentialed expert sign-off before they ship.**

**Required reviews before an artifact is "done":**

| Artifact class | Required reviewer(s) | Tier |
|---|---|---|
| Repo/tooling/CI, JSON Schema | Maintainer | low |
| CDEs, terminology bindings, crosswalks, interop profiles | Maintainer + **clinical/domain reviewer** (pediatric/AYA oncology or sarcoma-literate) | medium |
| Molecular/genomic CDEs | Domain reviewer + **clinical genomics-literate** reviewer | medium |
| Consent / governance / de-identification framework | **Ethics/IRB-literate reviewer + patient advocate** (+ data-protection-literate review) | high |
| Family-facing education | **Oncologist + patient advocate**, "not medical advice," sourced | high |
| Anything citing a source | License reviewer (license register check) | gate |

**Definition of Shipped (this project):** acceptance criteria met **and** CI green (all
validators + provenance-lint + license-audit) **and** required human review signed off **and**
(for the standard as a whole) **a real partner has the artifacts in hand and at least one has
committed to pilot/adopt** (O1). Until O1, the project is "delivered to readiness," **not** "done."

## 9. Roadmap & milestones

Phased; each milestone has measurable **exit criteria**. M0 is a thin foundation + cold-start
(governance, landscape, partner recruitment). Patient-facing and consent artifacts are gated
behind securing reviewers.

**M0 — Foundation, compliance scaffolding & cold-start.**
Goal: a clean repo, the compliance/guardrail policy, a landscape scan, a glossary, and a partner/
expert recruitment plan. *Exit:* repo + CI green; compliance policy + license register skeleton
merged; landscape report covering GRDR/caDSR/mCODE/OMOP/ICHOM/ERN; partner & expert recruitment
plan published; **≥3 partner/reviewer candidates contacted.**

**M1 — Conceptual model & module scope.**
Goal: the Ewing natural-history conceptual model and the core-vs-extended module map. *Exit:*
model doc + module map reviewed by a domain reviewer; modules cover the full journey; every module
traces to ≥1 existing standard.

**M2 — Common Data Elements & terminology bindings.**
Goal: machine-readable CDE dictionary v0.1 with full provenance, code bindings, and crosswalks.
*Exit:* 100% of **core** CDEs have definition + value set + ≥1 code binding (by reference) + ≥1
crosswalk + provenance; molecular CDEs reviewed; license register complete; provenance-lint &
license-audit green.

**M3 — Consent, de-identification & data-use governance (HIGH).**
Goal: the consent-first framework, de-identification standard, and tiered data-use governance.
*Exit:* all three reviewed and signed off by ethics-literate reviewer + patient advocate; DUO
bindings validate; rare-disease re-identification risk explicitly addressed. **Blocked until an
ethics reviewer + advocate are secured.**

**M4 — Interoperability profiles.**
Goal: FHIR/mCODE IG, OMOP CDM mapping, CDISC mapping notes, REDCap template. *Exit:* FHIR IG
passes HL7 validator; OMOP mapping passes OHDSI checks; REDCap template imports clean; ≥90% of core
CDEs represented in each profile (gaps documented).

**M5 — Reference implementation & validation tooling.**
Goal: JSON Schema, validator CLI, synthetic fixtures, conformance suite in CI. *Exit:* validator
runs on synthetic fixtures with documented pass/fail cases; conformance suite green in CI; zero
real data anywhere (synthetic-check enforced).

**M6 — Family education, adoption kit & v1.0 (HIGH for education).**
Goal: plain-language family-facing education, registry adoption kit, versioning/maintenance policy,
v1.0 release. *Exit:* education reviewed by oncologist + advocate, readability ≤ grade 8, "not
medical advice" present, every claim sourced; adoption kit complete; v1.0 tagged with changelog;
**≥1 partner has artifacts and a stated pilot intent (O1).**

Dependencies: M2 depends on M1; M3 is parallelizable but gated on securing ethics/advocate
reviewers; M4/M5 depend on M2; M6 depends on M2–M5 + securing clinical/advocate reviewers.

## 10. Work breakdown

The itemized, schema-mapped backlog lives in **`TASKS.md`** — ~20 tasks across M0–M6, each
expressible as a Hee-Lee Oss Task JSON (validated against `packages/schema/src/schemas.ts`), with size,
risk tier, deliverable, dependencies, reviewer, and acceptance criteria. `TASKS.md` includes a
complete, schema-valid example Task JSON for the first M0 task and per-milestone Definitions of
Done. All tasks currently carry `verifiedNeed: false` (no partner yet secured).

## 11. Governance, roles & stakeholders

- **Maintainer (Owner):** TBD — accountable for the repo, CI, releases, and the review process.
- **Reviewers (rotation):** domain/clinical reviewer (pediatric/AYA oncology or sarcoma-literate);
  license reviewer; engineering reviewer. Rotation tracked in `GOVERNANCE.md`.
- **Steward (last-mile owner):** the **partner registry / advocacy organization** that adopts the
  standard — owns real-world adoption (O1). **TO BE SECURED.**
- **Partner / requestor:** a Ewing/pediatric-sarcoma foundation, rare-disease registry program, or
  academic consortium. **TO BE SECURED** (`verifiedNeed: false` until then).
- **Expert reviewers (HIGH tier):** credentialed **oncologist** (pediatric/AYA sarcoma) +
  **patient advocate** for education; **ethics/IRB-literate reviewer** (+ data-protection-literate)
  for consent/governance. **TO BE SECURED** — these gate the high-tier milestones.
- **Community/board:** edge cases, license disputes, and definition changes go through Hee-Lee Oss
  governance per `good-deed-definition.md`.

No artifact in a high tier ships without its named expert sign-off recorded in the review log.

## 12. Dependencies & integrations

- **Standards bodies / sources:** NCATS GRDR, NCI caDSR/NCIt, HL7 FHIR + mCODE, OHDSI OMOP CDM,
  CDISC, REDCap, GA4GH (Phenopackets, DUO, VRS, Beacon), ICHOM, ERN/EURACAN, ICD-O-3/WHO, AJCC,
  ICCC-3, HGNC, HGVS, Sequence Ontology, LOINC, SNOMED CT.
- **Tooling:** HL7 FHIR IG Publisher; OHDSI/OMOP validators; JSON Schema validator; Node/pnpm.
- **Hee-Lee Oss pieces:** Task schema (`packages/schema`), CLI workspace conventions, CI/governance
  workflows, the registry entry in `ROADMAP.md` (Track 8a).
- **Sibling projects** (consume/feed): `ewing-open-data-catalog`, `ewsr1-fli1-knowledge-graph`,
  `ewing-outcomes-harmonization`, `rare-cancer-registry-templates` (shares governance patterns),
  `ewing-survivorship-late-effects` (late-effects CDEs), `ewing-family-guide` (education review
  pool).
- **External access required for HIGH milestones:** named ethics reviewer, oncologist, advocate,
  and partner registry — all **TO BE SECURED**.

## 13. Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| No partner/steward secured → standard nobody adopts (O1 stays 0) | High | High | M0 recruitment as a gating task; design for easy adoption (REDCap template, multiple profiles); publish even if pilot delayed; track O1 honestly | Maintainer |
| Inadvertent use of identifiable/controlled-access data | Low | Critical | Hard out-of-scope guardrail; synthetic-only fixtures; CI synthetic-check; code review for any data reference | Maintainer + License reviewer |
| Redistributing restricted terminology content (SNOMED/LOINC/AJCC/PRO instruments) | Medium | High | "Bind, don't bundle" rule; license register; CI license-audit; legal-aware review of value sets | License reviewer |
| Consent/de-id framework wrong or unsafe (esp. rare-disease re-identification) | Medium | Critical | HIGH-tier ethics + advocate sign-off; explicit rare-disease re-id section; small-cell suppression guidance | Ethics reviewer |
| Family-facing content reads as medical advice | Medium | High | "Not medical advice" + sourcing + oncologist & advocate review; readability gate; no prognosis/treatment guidance | Oncologist + Advocate |
| Spec/implementation drift | Medium | Medium | Single machine-readable source of truth; generate all derived artifacts; CI drift check | Maintainer |
| Standard fragmentation (yet another incompatible CDE set) | Medium | High | mCODE-first + GRDR/caDSR crosswalks mandatory; align before invent; O2 = 100% crosswalk | Domain reviewer |
| Reviewer scarcity (rare disease, small expert pool) delays HIGH milestones | High | Medium | Early recruitment; partner with advocacy orgs; sequence HIGH work after reviewers secured; never bypass the gate | Maintainer |
| Standard drifts out of date as source standards evolve | Medium | Medium | Versioned releases; binding-by-reference; maintenance policy + periodic re-validation | Steward |
| Scope creep into operating a registry | Low | High | Non-goals are explicit and binding; "no data store" is a hard line | Maintainer |

## 14. Security & privacy

- **Threat surface is unusually small by design:** no data store, no patient data, no runtime
  handling PII. The primary surface is the *repository* and the *correctness/safety of guidance*.
- **No secrets in the repo:** no tokens/keys/credentials in code, logs, receipts, or fixtures
  (CLAUDE.md). CI runs with least privilege; no production data anywhere.
- **PII:** none held. The de-identification standard we publish is the privacy deliverable; it
  addresses HIPAA Safe Harbor/Expert Determination and **rare-disease re-identification risk**
  (rare diagnosis + quasi-identifiers can re-identify) with suppression/k-anonymity guidance.
- **Synthetic-only fixtures**, machine-checked; any fixture not labelled synthetic fails CI.
- **Abuse/misuse prevention:** the standard could be misread as endorsing data collection without
  consent — so consent-first is structural (you cannot conform without the consent module), and the
  education/adoption materials state the legal/ethical obligations plainly. Refuse and flag any
  request to weaken consent, include identifiable fields by default, or repurpose the schema for
  surveillance or non-public/for-profit-primary benefit (CLAUDE.md refusal guardrails).
- **Provenance integrity:** signed releases + checksums; CITATION.cff; immutable changelog.

## 15. Sustainability & maintenance

- **After delivery,** the **steward/partner** owns real-world adoption and feedback; the
  **maintainer** owns versioned upkeep (re-validating bindings as source standards release new
  versions, ≥ annually or on major upstream change).
- **Versioning:** SemVer with published migration notes; deprecation policy for changed CDEs.
- **Outcome tracking:** O1 (adoption) and O3 (validation) tracked publicly; an outcomes log
  records each partner pilot and any data-quality feedback (aggregate only).
- **Bus-factor:** single machine-readable source + generated artifacts + CI keep the project
  maintainable by one rotating maintainer; governance doc records decisions and reviewer roster.
- **Funding:** donated lane; no funded tasks planned. If aggregate-data validation work later
  warrants metered runs, it would be a separate `funded` task with a hard `fundedBudgetUsd` cap.

## 16. Open questions

1. **Partner & reviewers:** which registry/advocacy org will steward this, and who are the named
   oncologist, advocate, and ethics reviewers? (Gates O1 and all HIGH milestones.) **TO BE SECURED.**
2. **Pediatric vs AYA scope:** single standard covering pediatric→AY→adult Ewing, or a pediatric
   core with an AYA/adult extension module? (Affects consent/assent and late-effects CDEs.)
3. **Code license:** MIT vs Apache-2.0 (patent grant)? FHIR IG: CC0 vs CC-BY-4.0?
4. **Terminology primary axis:** SNOMED CT-first (licensing burden on adopters) vs maximize
   openly-bindable systems (NCIt/LOINC/ICD-O) to lower the adoption barrier?
5. **PRO instruments:** which to reference given mixed licensing (PROMIS/PRO-CTCAE free vs
   PedsQL/EQ-5D licensed)? Reference-only vs negotiate inclusion?
6. **Dynamic consent depth:** how granular (per-use, per-recipient, time-bound) without making
   adoption impractical?
7. **International scope:** US (HIPAA) + EU (GDPR) both, or US-first with an EU governance annex?
8. **Relationship to `rare-cancer-registry-templates`:** is Ewing a specialization of a shared
   generic rare-cancer core, and should the core be extracted upstream?

## 17. References

- Hee-Lee Oss: `CLAUDE.md` (work rules, lanes, guardrails); `docs/good-deed-definition.md` (criteria +
  risk tiers); `packages/schema/src/schemas.ts` (Task schema); `planning/ROADMAP.md` (Track 8a,
  cancer guardrails).
- NIH/NCATS **GRDR** Common Data Elements; **GARD** rare-disease resources.
- NCI **caDSR** / **NCI Thesaurus (NCIt)**.
- HL7 **FHIR R4**; **mCODE** (minimal Common Oncology Data Elements) IG.
- OHDSI **OMOP CDM** + Oncology extension.
- **CDISC** CDASH/SDTM; Rare Disease therapeutic-area concepts.
- **REDCap** data dictionary format.
- **GA4GH**: Phenopackets, Data Use Ontology (DUO), Variation Representation Spec (VRS), Beacon.
- **ICHOM** standard sets (sarcoma/AYA where applicable).
- **ERN / EURACAN** rare adult solid cancer common data elements.
- Classifications/terminologies: **ICD-O-3**, WHO classification of tumours; **AJCC** staging;
  **ICCC-3**; **HGNC**; **HGVS**; **Sequence Ontology**; **LOINC**; **SNOMED CT**.
- PRO instruments: **PROMIS**, **PRO-CTCAE**, **PedsQL**, **EQ-5D-Y** (license terms vary).
- **FAIR** data principles (Wilkinson et al., 2016).
- FDA/EMA guidance on the use of **patient registries** for evidence generation.
- Clinical context for Ewing Sarcoma: NCI PDQ (Ewing Sarcoma treatment), **Children's Oncology
  Group (COG)** / **SIOPE** resources, **Euro Ewing** consortium publications (clinical claims to
  be sourced + expert-reviewed per the guardrails; cited, not reproduced).

---

## Appendix A — Improvements applied

The following 25 specific improvements were identified during planning and **have been applied**
to the plan above (and to `TASKS.md`). Each is concrete and verifiable in the documents.

1. **Front-loaded a binding-guardrails box** at the very top (open/aggregate/de-identified only;
   no real data; license verification; not-medical-advice; provenance) so the cancer constraints
   are unmissable — and §7 leads with them as required.
2. **Reframed "schema, not data store" as a structural guarantee** in the executive summary,
   scope, and non-goals — Hee-Lee Oss never holds patient data, shrinking the threat surface (§14).
3. **Made "verified need" honest:** `verifiedNeed: false` and `requestor: TO BE SECURED`
   everywhere, with securing a partner as M0's top non-engineering exit criterion (§2, §9, §11).
4. **Added "bind, don't bundle" as a first-class rule** for terminologies, with a CI license-audit
   gate, preventing redistribution of SNOMED/LOINC/AJCC/PRO content (§6, §7, §13).
5. **Built a per-source license register table** (§7.2) covering each terminology/standard's
   posture, including the SNOMED affiliate-license and PRO-instrument licensing nuances.
6. **Specified a mandatory structured provenance record** (source/url/accessedDate/version/
   license/boundByReference) with a `provenance-lint` CI gate (O6, §7.3).
7. **Added rare-disease re-identification risk** explicitly to the de-identification standard and
   security section (rare diagnosis + quasi-identifiers) — a failure mode generic plans miss.
8. **Single machine-readable source of truth + generated artifacts** to eliminate spec/impl drift
   (§6.4, §13) — the classic data-standard failure mode.
9. **mCODE-first + mandatory GRDR/caDSR crosswalks** to avoid creating yet another incompatible
   CDE set; O2 requires 100% crosswalk coverage of core CDEs.
10. **Outcome metrics are beneficiary-centric** (adoption, validation, review, provenance, license
    safety) with baselines/targets — and we commit to reporting O1 (adoption) even at zero (§4).
11. **Separated project-level (medium) from artifact-level (high) risk tiers** with a per-artifact
    reviewer matrix (§8), so HIGH content can't ship without named expert sign-off.
12. **Sequenced HIGH milestones (M3 consent, M6 education) behind securing reviewers** and made
    that an explicit dependency/risk rather than an assumption (§9, §13).
13. **Synthetic-only fixtures enforced by CI** (a check rejects any unlabelled fixture) so no real
    patient data can ever enter examples/tests (§6, §14).
14. **Added pediatric assent + parental permission** and AYA scope to the consent framework and
    open questions — Ewing's age distribution demands it (§3, §16 Q2).
15. **Defined "Definition of Shipped" as delivered + adopted**, not merged, per Hee-Lee Oss quality bar,
    including the O1 partner-pilot condition (§8).
16. **Added an adoption kit + REDCap template** to lower the real-world adoption barrier (REDCap is
    the dominant registry tool) — directly serving O1 (§5, §6, M4/M6).
17. **Explicit out-of-scope list** including trial-matching/eligibility (sibling projects), regulatory
    filing, and operating a registry — prevents scope creep (§3, §5, §13).
18. **Molecular CDEs expressed via open standards** (HGVS/HGNC/Sequence Ontology/GA4GH VRS) with
    "schema only, no sequence data" — keeps Ewing biology in scope without controlled-access data.
19. **Interoperability via multiple profiles** (FHIR/OMOP/CDISC/REDCap) for adopt-anywhere,
    lock-in-nowhere, each validated by its official validator in CI (O3, §6).
20. **Added a maintenance/versioning policy** (SemVer + migration notes + ≥annual re-validation as
    upstream standards evolve) with the steward owning adoption (§15).
21. **Risk table assigns an owner to every risk** and includes the "no partner → no adoption" risk
    as High/High (§13).
22. **Education content constraints made concrete:** readability ≤ grade 8, "not medical advice,"
    sourced, oncologist + advocate reviewed, no prognosis/treatment guidance (§4 O4, §8).
23. **Refusal/abuse handling tailored:** refuse requests to weaken consent, default-in identifiable
    fields, or repurpose the schema for surveillance/for-profit-primary benefit (§14).
24. **International governance flagged** (HIPAA + GDPR) as an open question with an EU annex option,
    rather than silently assuming US-only (§16 Q7).
25. **Linked to sibling Hee-Lee Oss projects** (catalog, KG, outcomes-harmonization, survivorship,
    family-guide, rare-cancer-registry-templates) for reuse and a possible shared rare-cancer core
    (§12, §16 Q8) — avoiding duplicated effort across the cancer track.

---

## Review sign-off

**Reviewer:** planning author acting as senior staff engineer + TPM (self-review pass).
**Date:** 2026-06-28. **Scope of review:** completeness against `PLAN_SPEC.md` (17 sections),
correctness against `CLAUDE.md` + `good-deed-definition.md` guardrails, schema-mapping of
`TASKS.md`, and the binding cancer guardrails.

**Completeness check**
- All 17 required H2 sections present and in spec order. ✔
- Metadata header present (Status/Version/Last updated/Owner/Lane). ✔
- §7 leads with the cancer guardrails and is specific + conservative. ✔
- Risks table uses required columns (Risk | Likelihood | Impact | Mitigation | Owner). ✔
- Roadmap M0..M6 each has goal + measurable exit criteria; M0 is the thin cold-start. ✔
- `TASKS.md` maps every task to schema fields, has per-milestone tables + DoD, a schema-valid
  example Task JSON, and a backlog. ✔ (verified against `packages/schema/src/schemas.ts`)
- Appendix A lists 25 specific, applied improvements. ✔

**Correctness fixes applied during review**
- Confirmed the example Task JSON includes **all required fields** from the schema
  (`id, title, project, type, lane, priority, domain, riskTier, urgent, deliverable,
  tokenEstimate, status, context, objective, acceptanceCriteria, output, verifiedNeed`) and uses
  **only enum-valid values**; `acceptanceCriteria` has ≥1 item; `output` is non-empty;
  `verifiedNeed: false`. Corrected to ensure no `additionalProperties` violations.
- Ensured **no task implies holding patient data**; the one data-handling area (synthetic
  fixtures) is explicitly synthetic and CI-checked.
- Verified every HIGH-tier artifact (consent/de-id, family education) carries a named expert
  reviewer requirement and is gated on securing reviewers (no silent assumption of availability).
- Confirmed `verifiedNeed: false` and `requestor: TO BE SECURED` are consistent across PLAN and
  TASKS (no invented partner).

**Outstanding (require a human decision, not fixable in-plan):** Open Questions 1–8 — most
critically securing a partner/steward and the HIGH-tier reviewers (oncologist, advocate, ethics),
and the license decisions (code MIT/Apache-2.0; FHIR IG CC0/CC-BY).

**Verdict:** Plan is internally consistent, guardrail-compliant, and ready to enter M0. It must
**not** advance past M2 into the HIGH-tier consent/education milestones until the named reviewers
and a partner are secured.
