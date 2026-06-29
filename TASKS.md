# Ewing Registry Standards — TASKS.md

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) ·
> Lane: donated · Slug: `ewing-registry-standards`. See `PLAN.md` for full context.

Backlog for **Ewing Registry Standards** — an open, consent-first **natural-history registry
*standard*** (CDEs, governance, interoperability profiles) for Ewing Sarcoma. **This project
produces a standard, not a data store; Elyos never holds patient data.**

**Binding guardrails (every task inherits these):** open-access / aggregate / de-identified only;
**no real patient data anywhere** (fixtures are synthetic); per-source license verification
(bind terminologies by reference, never redistribute); **no medical advice** (education is
sourced + "not medical advice" + oncologist & advocate reviewed); **provenance on every
assertion**. See `PLAN.md` §7.

**Cold-start status:** **no partner registry or expert reviewers are yet secured.** Therefore
**every task carries `verifiedNeed: false`** and `requestor: TO BE SECURED`. HIGH-tier tasks
(consent/governance, family education) are **blocked** until the named reviewers are secured.

## How these tasks map to Elyos

Each task becomes an Elyos **Task JSON** validated against `packages/schema/src/schemas.ts`.
Field mapping:

- **id** — stable slug `ewing-registry-standards-<area>-NNN`.
- **title** — the title in the milestone table.
- **project** — `ewing-registry-standards`.
- **type** — one of `code | research | writing | data | design-spec | maintenance`.
- **lane** — `donated` (default; no funded tasks. Any `funded` task MUST add `fundedBudgetUsd`
  with a hard cap — none planned).
- **priority** — `high | medium | low`.
- **domain** — array, e.g. `["cancer","rare-disease","data-standards","health-informatics"]`.
- **riskTier** — `low | medium | high`. Consent/de-identification and patient/family-facing
  education are `high`; CDEs/interop/molecular are `medium`; pure tooling/infra is `low`.
- **urgent** — boolean (no urgent tasks at cold-start).
- **deliverable** — `pr | dataset | document | translation`.
- **tokenEstimate** — `small | medium | large` (the Size column).
- **status** — `open | in-progress | review | delivered | done` (all start `open`).
- **context / objective / acceptanceCriteria[] / resources[] / output** — per task.
- **requestor** — `TO BE SECURED` (no partner yet).
- **verifiedNeed** — `false` everywhere (no partner/need confirmed).
- **outputLicense** — code/tooling: **MIT**; standard docs/CDEs/governance/education: **CC-BY-4.0**;
  FHIR IG: **CC0-1.0** (governance decision pending — see PLAN §16 Q3).

Size legend: small ≈ `small`, med ≈ `medium`, large ≈ `large`.
Reviewer key: **Maintainer**; **Domain (clinical)** = pediatric/AYA-oncology or sarcoma-literate
reviewer; **Genomics** = clinical-genomics-literate reviewer; **Ethics** = ethics/IRB-literate
reviewer; **Advocate** = patient advocate; **Oncologist** = credentialed oncologist; **License** =
license reviewer. HIGH-tier reviewers are **TO BE SECURED**.

---

## Milestone M0 — Foundation, compliance scaffolding & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-registry-standards-repo-001 | Monorepo + pnpm/TS/ESM + CI (build/test/lint) skeleton | code | small | low | pr | — | Maintainer |
| ewing-registry-standards-compliance-002 | Data/licensing & compliance guardrail policy + license-register skeleton + CI license-audit + provenance-lint | design-spec | medium | medium | document | 001 | Maintainer + License |
| ewing-registry-standards-landscape-003 | Landscape scan of existing CDEs/standards (GRDR, caDSR, mCODE, OMOP, ICHOM, ERN) + gap analysis | research | medium | medium | document | — | Domain (clinical) |
| ewing-registry-standards-partner-004 | Partner & expert-reviewer recruitment plan + scored candidate list (gates HIGH milestones + O1) | research | small | medium | document | — | Maintainer |
| ewing-registry-standards-glossary-005 | Project glossary + provenance/citation conventions | writing | small | low | document | 001 | Maintainer |

**Acceptance criteria — key tasks**

- **repo-001:** pnpm workspace builds; `pnpm build && pnpm test && pnpm lint` green in CI; MIT
  LICENSE for code; `CONTRIBUTING.md`, `CITATION.cff`, `NOTICE` present; no secrets committed;
  directory layout matches PLAN §6.1 (`model/ cde/ terminology/ governance/ profiles/ schema/
  tooling/ examples/ education/`).
- **compliance-002:** policy states the binding guardrails verbatim; license-register template has
  a row schema (source/use/license/handling-rule/boundByReference); CI `license-audit` and
  `provenance-lint` jobs exist and fail on a seeded violation; "no real data / synthetic-only"
  rule documented and a CI synthetic-check stub added.
- **partner-004:** ≥3 candidate partners and ≥1 candidate per HIGH-tier reviewer role
  (oncologist, advocate, ethics) identified with contact approach; explicit statement that
  `verifiedNeed` stays `false` until a partner confirms; recruitment is named the gate for M3/M6.
- **landscape-003:** every existing standard reviewed with its license posture; gap analysis
  identifies what a Ewing-specific standard must add vs reuse; recommends mCODE-first + GRDR/caDSR
  crosswalk; cited throughout.

**M0 Definition of Done:** repo + CI green; compliance policy + license register skeleton merged
with working audit/lint gates; landscape report + recruitment plan published; ≥3 partner/reviewer
candidates contacted; glossary merged. No real data anywhere.

---

## Milestone M1 — Conceptual model & module scope

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-registry-standards-model-010 | Ewing natural-history conceptual data model (diagnosis→molecular→staging→treatment→response→relapse→survivorship→PRO→mortality) | design-spec | medium | medium | document | 003 | Domain (clinical) |
| ewing-registry-standards-modules-011 | Core-vs-extended module map; each module traced to ≥1 existing standard | design-spec | small | medium | document | 010 | Domain (clinical) |

**Acceptance criteria — key tasks**

- **model-010:** model covers the full patient journey; each domain area defined and scoped;
  pediatric/AYA/adult coverage decision recorded (or flagged to PLAN §16 Q2); reviewed by a domain
  reviewer; every assertion sourced.
- **modules-011:** clear core (minimal viable) vs extended split; each module crosswalks to GRDR/
  caDSR/mCODE/ICHOM where one exists; gaps explicitly listed.

**M1 Definition of Done:** model + module map reviewed and merged; full-journey coverage; every
module traces to an existing standard or is justified as net-new.

---

## Milestone M2 — Common Data Elements & terminology bindings

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-registry-standards-cde-020 | Machine-readable CDE dictionary v0.1 (definitions, value sets, units, provenance) | data | large | medium | dataset | 011 | Domain (clinical) + License |
| ewing-registry-standards-terminology-021 | Terminology bindings + per-system license register (bind by reference) | data | medium | medium | dataset | 020 | License + Domain (clinical) |
| ewing-registry-standards-molecular-022 | Molecular/genomic CDEs for EWSR1 fusions/variants (HGVS/HGNC/SO/GA4GH VRS; schema only, no sequence data) | data | medium | medium | dataset | 020 | Genomics + Domain (clinical) |
| ewing-registry-standards-crosswalk-023 | Crosswalk of all core CDEs to GRDR/caDSR/mCODE/ICHOM | data | medium | medium | dataset | 020,021 | Domain (clinical) + License |

**Acceptance criteria — key tasks**

- **cde-020:** 100% of **core** CDEs have id/label/definition/dataType/value-set + provenance
  block (source/url/accessedDate/version/license); provenance-lint green; no terminology *content*
  embedded (bindings only).
- **terminology-021:** every binding records system + version + code (by reference); license
  register complete for SNOMED CT (affiliate), LOINC (notice), ICD-O-3/WHO, AJCC (reference-only),
  NCIt, HGNC; license-audit green; SNOMED/PRO-instrument adopter obligations documented.
- **molecular-022:** EWSR1-FLI1/EWSR1-ERG and variant representation via open standards; **no
  sequence/biospecimen data**; reviewed by a genomics-literate reviewer; sourced.
- **crosswalk-023:** O2 met — 100% of core CDEs carry ≥1 crosswalk + provenance; orphans = 0 or
  justified; CI crosswalk-completeness check green.

**M2 Definition of Done:** CDE dictionary v0.1 + bindings + molecular CDEs + crosswalk merged;
provenance-lint, license-audit, crosswalk-completeness all green; domain + license sign-off
recorded.

---

## Milestone M3 — Consent, de-identification & data-use governance (HIGH — blocked on reviewers)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-registry-standards-consent-030 | Consent-first framework (dynamic/granular consent, pediatric assent + parental permission, withdrawal) | design-spec | large | high | document | 011 | Ethics + Advocate |
| ewing-registry-standards-deid-031 | De-identification standard (HIPAA Safe Harbor/Expert Determination + rare-disease re-identification + small-cell suppression) | design-spec | medium | high | document | 030 | Ethics + Advocate |
| ewing-registry-standards-governance-032 | Tiered data-use governance + GA4GH DUO bindings + access model | design-spec | medium | high | document | 030 | Ethics + Domain (clinical) |

**Acceptance criteria — key tasks**

- **consent-030:** consent is structural (cannot conform without it); covers granular/dynamic
  consent, **pediatric assent + parental permission**, and withdrawal/revocation; signed off by
  ethics-literate reviewer + patient advocate; sourced.
- **deid-031:** mandates Safe Harbor *or* Expert Determination; explicitly addresses
  **rare-disease re-identification** (rare diagnosis + age + geography); gives small-cell
  suppression / k-anonymity guidance for aggregate release; HIGH sign-off recorded.
- **governance-032:** access tiers defined; DUO codes bound and validate; data-use governance
  aligns with consent options.

**M3 Definition of Done:** all three artifacts reviewed and **signed off by ethics reviewer +
advocate**; DUO bindings validate; rare-disease re-id risk addressed. **Do not start until the
ethics reviewer + advocate are secured (partner-004).**

---

## Milestone M4 — Interoperability profiles

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-registry-standards-fhir-040 | FHIR R4 Implementation Guide (mCODE-aligned profiles + value sets) | code | large | medium | pr | 020,021 | Domain (clinical) + Maintainer |
| ewing-registry-standards-omop-041 | OMOP CDM (+Oncology) mapping specification | data | medium | medium | document | 020,021 | Domain (clinical) |
| ewing-registry-standards-redcap-042 | REDCap data-dictionary template (importable) | data | medium | medium | dataset | 020,021 | Domain (clinical) + Maintainer |

**Acceptance criteria — key tasks**

- **fhir-040:** IG builds and **passes the HL7 FHIR IG Publisher/validator** in CI; ≥90% of core
  CDEs represented (gaps documented); profiles mCODE-aligned; license = CC0/CC-BY (per §16 Q3).
- **omop-041:** mapping passes OHDSI convention checks; uses the OMOP Oncology extension where
  relevant; ≥90% core-CDE coverage documented.
- **redcap-042:** template imports into REDCap cleanly (validated); field types/value sets match
  the CDE dictionary (generated from the single source of truth).

**M4 Definition of Done:** FHIR IG, OMOP mapping, REDCap template merged; each passes its official
validator in CI; coverage gaps documented; O3 met.

---

## Milestone M5 — Reference implementation & validation tooling

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-registry-standards-jsonschema-050 | JSON Schema (draft 2020-12) for CDE set + provenance, generated from source of truth | code | medium | low | pr | 020 | Maintainer |
| ewing-registry-standards-validator-051 | Validator CLI + provenance-lint + license-audit + crosswalk-completeness checks | code | medium | low | pr | 050 | Maintainer |
| ewing-registry-standards-fixtures-052 | Synthetic conformance fixtures (labelled SYNTHETIC) + conformance test suite in CI | data | medium | low | dataset | 050,051 | Maintainer + Domain (clinical) |

**Acceptance criteria — key tasks**

- **jsonschema-050:** schema generated (not hand-maintained) from the CDE source; validates the
  synthetic fixtures; CI drift-check ensures schema matches source.
- **validator-051:** CLI validates a record against the schema and reports pass/fail with reasons;
  provenance-lint, license-audit, and crosswalk-completeness run as commands and in CI.
- **fixtures-052:** all fixtures **fully synthetic and labelled** `SYNTHETIC — NOT REAL`; CI
  synthetic-check rejects any unlabelled fixture; suite includes intentional pass and fail cases.

**M5 Definition of Done:** validator + schema + synthetic fixtures + conformance suite green in
CI; **zero real data anywhere** (synthetic-check enforced); drift-check green.

---

## Milestone M6 — Family education, adoption kit & v1.0 (HIGH for education — blocked on reviewers)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-registry-standards-education-060 | Plain-language family-facing education ("what a registry collects & why; your consent rights") | writing | medium | high | document | 030,031 | Oncologist + Advocate |
| ewing-registry-standards-adoption-061 | Registry adoption kit (how to implement the standard via FHIR/OMOP/REDCap) | writing | medium | medium | document | 040,041,042,051 | Domain (clinical) + Maintainer |
| ewing-registry-standards-versioning-062 | Versioning, deprecation & maintenance policy + v1.0 release | maintenance | small | low | document | all M2–M5 | Maintainer |

**Acceptance criteria — key tasks**

- **education-060:** every claim sourced to an authoritative source (NCI/COG/SIOPE/peer-reviewed);
  contains **"This is not medical advice"**; readability ≤ grade 8; **reviewed and signed off by an
  oncologist + a patient advocate**; no prognosis/treatment guidance. **Blocked until reviewers
  secured.**
- **adoption-061:** step-by-step adoption via each profile; references the consent/de-id
  requirements; includes the REDCap template; aimed at lowering the barrier for a partner pilot (O1).
- **versioning-062:** SemVer policy + migration/deprecation notes + ≥annual re-validation
  commitment; v1.0 tagged with changelog.

**M6 Definition of Done:** education reviewed + published (oncologist + advocate sign-off,
readability gate, "not medical advice", sourced); adoption kit complete; v1.0 tagged; **≥1 partner
has the artifacts and a stated pilot intent (O1)** — otherwise the project is "delivered to
readiness," not "done."

---

## Backlog / future (sized, unscheduled)

| ID | Title | Type | Size | Risk | Deliverable | Notes |
|---|---|---|---|---|---|---|
| ewing-registry-standards-cdisc-070 | CDISC (CDASH/SDTM + Rare Disease TA) mapping notes | data | medium | medium | document | Reference-only; verify CDISC license before use |
| ewing-registry-standards-pro-071 | PRO/late-effects element set (PROMIS/PRO-CTCAE/PedsQL/EQ-5D-Y) | data | medium | medium | dataset | Mixed licensing — reference + flag adopter obligations; no item text without license |
| ewing-registry-standards-i18n-072 | Internationalization governance annex (GDPR + non-English education) | design-spec | medium | high | document | Depends on §16 Q7 decision; education translations are `high` |
| ewing-registry-standards-rare-core-073 | Extract a shared rare-cancer core with `rare-cancer-registry-templates` | design-spec | large | medium | document | Depends on §16 Q8; coordinate upstream |
| ewing-registry-standards-beacon-074 | GA4GH Beacon / Phenopackets discovery profile (aggregate, no record exposure) | data | medium | medium | document | Aggregate-only; reinforces no-data-store stance |

---

## Generated task index

Every milestone-table row (M0–M6) and every Backlog/future row now has an executable
`tasks/<id>.json` validated against `packages/schema/src/schemas.ts` (28 files total). The Elyos
CLI executes these JSON files, not this Markdown. Acceptance criteria mirror the "Acceptance
criteria — key tasks" bullets above; rows that lacked explicit criteria (glossary-005 and the
Backlog/future rows) received criteria derived from their title, deliverable, and the binding
guardrails. All tasks carry `status: open`, `lane: donated`, `verifiedNeed: false`, and
`requestor: TO BE SECURED` per the cold-start policy; HIGH-tier tasks restate their
blocked-until-reviewers-secured framing in `context`/`acceptanceCriteria`.

**Fan-out:** none — `TASKS.md` already enumerates each task as a concrete row, and no row defines
a bounded fan-out dimension (no named language/dataset/document set), so each row maps 1:1 to a
single task JSON. No languages, datasets, documents, or beneficiaries were fabricated.
Internationalization/translation work (i18n-072) remains a single governance-annex task; concrete
education translations expand only on the PLAN §16 Q7 decision and once HIGH-tier reviewers are
secured.

Generated ids:

- M0: `ewing-registry-standards-repo-001` (seed), `ewing-registry-standards-compliance-002`,
  `ewing-registry-standards-landscape-003`, `ewing-registry-standards-partner-004`,
  `ewing-registry-standards-glossary-005`
- M1: `ewing-registry-standards-model-010`, `ewing-registry-standards-modules-011`
- M2: `ewing-registry-standards-cde-020`, `ewing-registry-standards-terminology-021`,
  `ewing-registry-standards-molecular-022`, `ewing-registry-standards-crosswalk-023`
- M3 (HIGH, blocked on reviewers): `ewing-registry-standards-consent-030`,
  `ewing-registry-standards-deid-031`, `ewing-registry-standards-governance-032`
- M4: `ewing-registry-standards-fhir-040`, `ewing-registry-standards-omop-041`,
  `ewing-registry-standards-redcap-042`
- M5: `ewing-registry-standards-jsonschema-050`, `ewing-registry-standards-validator-051`,
  `ewing-registry-standards-fixtures-052`
- M6: `ewing-registry-standards-education-060` (HIGH, blocked on reviewers),
  `ewing-registry-standards-adoption-061`, `ewing-registry-standards-versioning-062`
- Backlog/future: `ewing-registry-standards-cdisc-070`, `ewing-registry-standards-pro-071`,
  `ewing-registry-standards-i18n-072` (HIGH), `ewing-registry-standards-rare-core-073`,
  `ewing-registry-standards-beacon-074`

---

## Example task JSON

A complete, schema-valid Task JSON for the first M0 task (validated against
`packages/schema/src/schemas.ts`; `verifiedNeed: false` — no partner secured):

```json
{
  "id": "ewing-registry-standards-repo-001",
  "title": "Monorepo + pnpm/TS/ESM + CI (build/test/lint) skeleton",
  "project": "ewing-registry-standards",
  "type": "code",
  "lane": "donated",
  "priority": "high",
  "domain": ["cancer", "rare-disease", "data-standards", "health-informatics", "software"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "pr",
  "tokenEstimate": "small",
  "status": "open",
  "context": "Ewing Registry Standards produces an OPEN, CONSENT-FIRST registry STANDARD (common data elements, governance, interoperability profiles) for Ewing Sarcoma. It is NOT a data store and Elyos never holds patient data. This task establishes the repository skeleton and CI that all later artifacts depend on. Binding guardrails apply to the whole repo: open/aggregate/de-identified only, no real patient data anywhere (fixtures must be synthetic), per-source license verification (bind terminologies by reference, never redistribute), and provenance on every assertion. No partner or expert reviewers are secured yet, so verifiedNeed is false.",
  "objective": "Stand up a pnpm + TypeScript + ESM monorepo with passing CI (build, test, lint) and the directory layout (model/, cde/, terminology/, governance/, profiles/, schema/, tooling/, examples/, education/), plus the licensing/citation scaffolding, so subsequent standards work has a clean, guardrail-aware foundation.",
  "acceptanceCriteria": [
    "pnpm workspace builds and `pnpm build && pnpm test && pnpm lint` pass green in CI",
    "MIT LICENSE present for code; CONTRIBUTING.md, CITATION.cff, and NOTICE files present",
    "Directory layout matches PLAN.md section 6.1 (model/ cde/ terminology/ governance/ profiles/ schema/ tooling/ examples/ education/)",
    "No secrets, tokens, or API keys committed; .gitignore and a secret-scan check are in place",
    "A placeholder synthetic-fixture check and empty provenance-lint/license-audit CI jobs exist (to be implemented in compliance-002)",
    "README states the binding cancer guardrails and the 'standard, not a data store' scope"
  ],
  "resources": [
    "C:/code/elyos/CLAUDE.md",
    "C:/code/elyos/docs/good-deed-definition.md",
    "C:/code/elyos/packages/schema/src/schemas.ts",
    "planning/projects/ewing-registry-standards/PLAN.md"
  ],
  "output": "A pull request adding the monorepo skeleton, CI workflow (build/test/lint + placeholder guardrail jobs), directory layout, and licensing/citation scaffolding for the Ewing Registry Standards project.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "MIT"
}
```
