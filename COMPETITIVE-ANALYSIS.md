# Competitive + Improvement Analysis — `ewing-registry-standards`

**Project under review:** An open, consent-first **natural-history registry STANDARD** (Common Data
Elements + terminology bindings + consent/governance framework + interoperability profiles) for
Ewing Sarcoma. **Not** a data store; Elyos holds no patient data. Deliverables tell a separately
governed registry operator *how* to collect Ewing natural-history data lawfully, interoperably, and
ethically.

**Method:** Reviewed `PLAN.md` (17 sections + Appendix A + sign-off) and `TASKS.md` (M0–M6 + backlog
+ schema-valid example). Grounded the competitive claims with web research (cited inline). Guardrails
respected throughout: schema/governance only, synthetic fixtures only, bind-don't-bundle terminology,
no medical advice, provenance on every assertion.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually strong: it correctly frames itself as **profiles/CDEs, not a database**, leads
with binding cancer guardrails, makes `verifiedNeed: false` honest, mandates provenance + license
audits in CI, and adopts "single machine-readable source of truth → generate everything" (the
canonical fix for spec/impl drift). Sections are complete and in spec order. The findings below are
gaps, errors, and weak spots — not wholesale problems.

**A. Factual/standards errors and stale targets**

1. **GRDR is retired (2017) — yet the plan makes it a mandatory crosswalk target.** O2 ("CDEs mapped
   to existing standards (GRDR, caDSR, mCODE)") and tasks crosswalk-023 / modules-011 treat **GRDR**
   as a live anchor standard. NCATS retired GRDR in 2017 and succeeded it with **RaDaR (Registry and
   Data Repository)** and the broader rare-disease data-standards effort. Crosswalking to a retired
   program as a *required* gate is an error; the live anchors should be **RaDaR/NCATS**, the **EU
   JRC Set of Common Data Elements for RD Registration** (the de facto rare-disease MDS, 16 core
   elements), and **ERDRI-CDS**. (Sources: NCATS RaDaR https://registries.ncats.nih.gov/about-radar/ ;
   GRDR retirement https://avillach-lab.hms.harvard.edu/nihncats-global-rare-diseases-patient-registry-data-repository-grdr%C2%AE ;
   EU CDE set https://eu-rd-platform.jrc.ec.europa.eu/set-of-common-data-elements_en )

2. **The single most important rare-disease MDS is essentially absent from the core plan.** The
   **EU JRC "Set of Common Data Elements for Rare Disease Registration"** (16 mandatory elements;
   the instrument every European RD registry and ERN is expected to implement) and its domain-specific
   extension (DCDEs) and **ERDRI-CDS** are the closest thing to an authoritative rare-disease MDS. They
   appear only glancingly (ERN/EURACAN in passing, "EU RD common data elements" in the prompt, not the
   plan body). For a project whose entire thesis is "align, don't reinvent," **ERDRI-CDS / EU RD CDE
   set must be a first-class crosswalk anchor alongside mCODE and caDSR.** (Sources:
   https://eu-rd-platform.jrc.ec.europa.eu/set-of-common-data-elements_en ;
   https://pmc.ncbi.nlm.nih.gov/articles/PMC9166638/ )

3. **Phenopackets is under-weighted as an interoperability target.** The plan uses GA4GH VRS/DUO/Beacon
   and mentions Phenopackets in §12 references, but the **profiles deliverable (M4) is FHIR/OMOP/CDISC/
   REDCap only — no Phenopacket profile.** GA4GH **Phenopackets v2** is an ISO-track, cancer-and-rare-
   disease-capable exchange format and is the natural bridge for the molecular CDEs (EWSR1 fusions).
   There is even prior art (npj Genomic Medicine "RareLink", Scientific Data 2025 RD-CDM) showing
   REDCap↔FHIR↔Phenopackets is a solved harmonization path — the plan should profile to it, not just
   cite it. (Sources: https://www.ga4gh.org/news_item/phenopackets-v2-expands-utility-to-provide-a-more-complete-medical-picture/ ;
   https://www.nature.com/articles/s41525-025-00534-z )

4. **ICHOM is cited as if a sarcoma set exists; it does not (publicly).** O2 and modules-011 list
   **ICHOM** crosswalks "(sarcoma/AYA where applicable)." Research found **no published ICHOM Standard
   Set for bone/soft-tissue sarcoma.** The plan should either drop ICHOM-sarcoma as an assumed anchor
   or explicitly flag it as "no set exists → use validated sarcoma PRO/function instruments (TESS,
   MSTS, SAM, PROMIS) instead." As written, a crosswalk-completeness CI gate could chase a non-existent
   target. (Source: https://pmc.ncbi.nlm.nih.gov/articles/PMC10968476/ — SAM; TESS/MSTS systematic
   review https://pmc.ncbi.nlm.nih.gov/articles/PMC6863783/ )

5. **Epidemiology slightly off / unsourced in-plan.** The exec summary says "~1 case per million per
   year; ~200 new US diagnoses annually." Published incidence is ~**0.15/100,000** overall and ~**1.5
   per million in AYA** (Euro-Ewing / incidence literature); US annual count is commonly cited closer
   to ~200–250 across all ages, but the "1 per million" and "200" figures are presented without the
   per-guardrail citation the plan itself mandates (every assertion sourced). Minor, but it is a
   self-consistency violation in the showcase section. (Source:
   https://pmc.ncbi.nlm.nih.gov/articles/PMC8282698/ )

**B. Completeness gaps**

6. **No explicit alignment to the OMOP Oncology Extension's real anchors.** §6.3 names "OMOP +
   Oncology extension" but omits the two things that make OMOP-onc usable for a registry: the
   **NAACCR** data-dictionary integration (US tumor-registry data) and the **HemOnc** ontology for
   regimen derivation. A serious OMOP mapping spec must say how Ewing regimens map to HemOnc and how
   staging/episodes use the episode model. (Source: https://pmc.ncbi.nlm.nih.gov/articles/PMC8140810/ )

7. **FAIR is asserted, not operationalized.** O-metrics cover adoption/validation/provenance/license
   but there is **no FAIR/maturity metric** (e.g., persistent identifiers for each CDE, machine-
   readable metadata, a resolvable PID scheme, FAIR self-assessment). Given the rare-disease FAIR
   stewardship literature, add a FAIR conformance check. (Source:
   https://datascience.codata.org/articles/10.5334/dsj-2023-012 )

8. **"Core CDE" set size and selection method are undefined.** M2 requires "100% of *core* CDEs"
   crosswalked, but *core* is never numerically scoped or given a selection rubric (Delphi? align to
   EU 16 + mCODE core?). Without a definition, the headline O2 metric ("0 orphan elements") is
   unfalsifiable — you can always shrink "core."

9. **Consent framework lacks named anchor models.** M3 says "dynamic/granular consent, GA4GH DUO,"
   which is good, but does not anchor to recognized instruments: **GA4GH Consent codes / DUO**,
   **MONDO/ORDO** disease coding, **OHDSI/All-of-Us broad-consent** templates, or EU **GDPR Art. 9 /
   EHDS** secondary-use provisions. For a HIGH-tier artifact this should name its sources.

10. **No internationalization of *terminology axis* decision.** Open Q4 (SNOMED-first vs maximize
    openly-bindable) is left open, but it materially changes adoptability (SNOMED affiliate licensing
    is a real adopter cost). This should be a near-term decision, not deferred indefinitely, because
    M2 can't finish bindings without it.

**C. Weak metrics / risk gaps**

11. **O1 (adoption) is the only true outcome and is entirely exogenous.** Every other metric is a
    process/CI gate. The plan honestly admits O1 may stay 0, but offers **no leading indicator** of
    adoption (e.g., letters of intent, # registries given a gap-analysis, REDCap-template downloads by
    a named partner). Add a leading proxy so the project can show traction before a signed pilot.

12. **"Reviewer scarcity" risk is rated Medium impact but is arguably the critical-path bottleneck.**
    M3 and M6 (the highest-value, highest-risk artifacts) are *both* hard-blocked on a 3-person
    expert pool (oncologist + advocate + ethicist) that is "TO BE SECURED." If recruitment fails, the
    project ships only M0–M2/M4–M5 (schema + profiles, no consent, no education) — i.e., the *easy*
    half. Impact should be High and there should be a fallback (e.g., partner with an existing ERN/
    advocacy ethics board rather than recruiting individuals).

13. **No competitive-overlap / "why not just adopt mCODE+EU-CDE" rebuttal.** The plan never states
    the falsifiable hypothesis that a *Ewing-specific* layer is needed at all. The strongest reviewer
    objection ("just use mCODE + the EU RD CDE set") is unaddressed in the body (it is implicitly the
    landscape-003 task, but the plan should pre-answer it — see §3/§4 below).

---

## 2. Competitive landscape

These are the standards/programs an adopter would weigh instead of (or alongside) a Ewing-specific
standard. None is Ewing-specific; that gap is the project's opening (see §3).

| Competitor / adjacent | What it does | Strengths | Weaknesses / gaps for Ewing | Source |
|---|---|---|---|---|
| **NCI caDSR / CDE Browser + NCIt** | ISO/IEC 11179 metadata registry of cancer CDEs; searchable, downloadable | Authoritative US cancer CDE repository; open NCIt; huge existing element pool to reuse | Element-level, not a packaged disease registry standard; no consent/governance, no FHIR IG; not rare-sarcoma curated | https://wiki.nci.nih.gov/display/caDSR/caDSR+CDE+Browser |
| **HL7 FHIR mCODE (STU4, R4)** | Minimal core oncology data elements as FHIR profiles (diagnosis, genomics, treatment, outcomes) | The emerging US oncology interoperability baseline; balloted HL7 standard; genomics + treatment domains | "Minimal" + adult-cancer-centric; thin on pediatric/AYA, late-effects/survivorship, and natural-history longitudinal modeling; no consent framework | https://hl7.org/fhir/us/mcode/ |
| **OHDSI OMOP CDM + Oncology Extension (v5.4)** | Standardized observational data model; oncology episodes, NAACCR + HemOnc vocab | Strong for retrospective analytics across institutions; episode model; mature tooling | A *storage/analytics* model, not a consent/collection standard; mapping is heavy; rare-disease/pediatric specifics absent | https://pmc.ncbi.nlm.nih.gov/articles/PMC8140810/ |
| **GA4GH Phenopackets v2 (+ VRS, DUO, Beacon)** | Computable phenotype+genotype exchange schema; rare-disease origin, now cancer-capable | ISO-track; ideal for molecular (fusion) + phenotype exchange; rare-disease pedigree | An exchange envelope, not a registry CDE set or governance framework; needs disease-specific value sets layered on | https://www.ga4gh.org/news_item/phenopackets-v2-expands-utility-to-provide-a-more-complete-medical-picture/ |
| **EU JRC Set of Common Data Elements for RD Registration + ERDRI-CDS / DCDEs** | The de facto European rare-disease MDS (16 mandatory elements) + domain-specific extensions; ERN-mandated | Authoritative rare-disease MDS; semantic-interoperability focus; ERN adoption; FAIR stewardship guidance | Generic across all rare diseases; not oncology/sarcoma-specific; light on molecular oncology + survivorship; EU-governance framed | https://eu-rd-platform.jrc.ec.europa.eu/set-of-common-data-elements_en ; https://pmc.ncbi.nlm.nih.gov/articles/PMC9166638/ |
| **RD-Connect / ERN PARTNER registries / EJP-RD** | Infrastructure linking RD registries, biobanks, genomics; ERN registry tooling | Real adoption across 24 ERNs; genome-phenome platform; governance + data-sharing precedent | Platform/infrastructure, not a reusable open Ewing CDE standard; controlled-access oriented | https://cordis.europa.eu/project/id/305444/reporting |
| **RD-CDM / RareLink (npj Genomic Med 2025; Sci Data 2025)** | Ontology-based RD common data model (78 elements) harmonizing ERDRI-CDS ↔ FHIR ↔ Phenopackets, REDCap-deployable | **Direct prior art** for the exact harmonization the plan proposes; open, FHIR+Phenopacket mappings, REDCap framework | Generic rare disease, not Ewing; uses real patient data in its pilots (different scope) — but a model to reuse, not reinvent | https://www.nature.com/articles/s41525-025-00534-z ; https://www.nature.com/articles/s41597-025-04558-z |
| **CDISC CDASH/SDTM + Rare Disease & Oncology TAUGs** | Regulatory clinical-trial data collection/submission standards | FDA/EMA submission lingua franca; rare-disease + oncology TAUGs exist | Trial-submission oriented, license-restricted content, heavy for a community natural-history registry; "how to structure, not what to collect" | https://www.cdisc.org/standards/therapeutic-areas/rare-diseases |
| **Childhood-cancer registries: SEER, ICCC-3 classification, EU pediatric registries** | Population incidence/survival registries + classification taxonomy | Authoritative population-level epidemiology; ICCC-3 standard tumor taxonomy | Population surveillance, coarse-grained; not longitudinal natural-history CDEs; no molecular/PRO depth | (ICCC-3 / SEER — classification anchor, cite in landscape-003) |
| **Euro-Ewing Consortium registry (EEC NEWTS, EURO-E.W.I.N.G.-99)** | Disease-specific trial + non-trial international registry for Ewing | The actual domain authority; real natural-history data; obvious *partner* candidate | A data-collection effort, not an openly published reusable CDE standard/governance toolkit | https://ascopubs.org/doi/10.1200/PO-25-00377 ; https://pmc.ncbi.nlm.nih.gov/articles/PMC9740851/ |

**Key competitive read:** there is **no open, Ewing-specific, consent-first registry standard** today.
The space is "generic rare-disease MDS (EU CDE/ERDRI/RD-CDM)" + "generic oncology interoperability
(mCODE/OMOP-onc)" + "disease-specific *registries* that don't publish a reusable standard"
(Euro-Ewing). The winning move is a **thin Ewing profile that composes these**, not a new island.

---

## 3. Gaps we can fill (the Ewing/rare-sarcoma, open, consent-first angle)

1. **A Ewing-specific *profile* of existing standards — "align, don't reinvent" made literal.** No one
   has published `mCODE + EU-RD-CDE + Phenopackets` composed into a Ewing natural-history package with
   value sets bound to EWSR1 biology. That composition is the whole product.
2. **Pediatric/AYA + survivorship depth that mCODE lacks.** mCODE is minimal and adult-leaning; Ewing
   is a pediatric/AYA disease where **late-effects and survivorship CDEs** are the under-measured gap.
   This is a defensible net-new contribution (built on PROMIS/PRO-CTCAE + sarcoma function measures).
3. **Molecular natural-history for fusion-driven sarcoma.** EWSR1::FLI1/ERG fusion + variant
   representation via HGVS/HGNC/SO/VRS/Phenopackets, as schema-only CDEs — a concrete, citable,
   open contribution generic standards don't carry.
4. **Consent-first as a *structural conformance requirement*** (you cannot conform without the consent
   module) — neither mCODE nor OMOP-onc ships an opinionated consent/governance layer; the EU side has
   governance but it's GDPR-framed and generic.
5. **Adopt-anywhere, license-clean packaging for low-resource adopters.** A REDCap data dictionary +
   FHIR IG + OMOP map generated from one source, with a per-source license register, lowers the
   barrier for small Ewing foundations/registries that can't afford SNOMED-heavy stacks.
6. **An explicit US (HIPAA) ⇄ EU (GDPR/EHDS) crosswalk** for a single disease — most standards pick one
   jurisdiction; a rare cancer is inherently international (too few patients per country).

---

## 4. Differentiators to win

1. **Composition over creation.** Ship as a *profile/crosswalk layer* over mCODE + EU-RD-CDE +
   Phenopackets + OMOP-onc, with 100% crosswalk coverage as a CI gate. The pitch: "adopt this and you
   are *automatically* mCODE- and ERDRI-conformant." That is the single strongest differentiator.
2. **Consent-first by construction**, expert-reviewed, with rare-disease re-identification explicitly
   handled (rare dx + age + geography) — a safety story competitors don't tell.
3. **Single source of truth → all artifacts generated** (FHIR/OMOP/REDCap/JSON Schema) — eliminates the
   drift that kills most CDE projects; verifiable in CI.
4. **Radical provenance + license hygiene** (every assertion sourced; license-audit gate; bind-don't-
   bundle) — turns a compliance burden into a trust signal for cautious clinical adopters.
5. **Beneficiary-honest definition of done** ("delivered & adopted," O1 reported even at zero) — credible
   to advocacy partners in a way vanity-metric projects are not.
6. **Open + free + low-stack** vs CDISC (heavy/regulated) and SNOMED-mandatory designs.

---

## 5. Claude API leverage

**Where Claude clearly helps (draft → human-verify):**

1. **CDE crosswalking / mapping at scale.** Claude drafts candidate mappings of each Ewing CDE to
   mCODE, EU-RD-CDE/ERDRI-CDS, caDSR/NCIt, OMOP-onc, and Phenopackets fields, with rationale and
   confidence — exactly the O2 crosswalk work, accelerated. Every mapping flagged "DRAFT — verify."
2. **Generating the derived artifacts from the single source.** Claude scaffolds the FHIR
   StructureDefinitions/ValueSets (FSH/Sushi), the OMOP mapping spec (incl. HemOnc/NAACCR notes), the
   REDCap data dictionary CSV, and JSON Schema — then humans/validators confirm. Big leverage on M4/M5.
3. **Drafting governance + family-facing prose.** Claude drafts the consent framework, de-identification
   standard, adoption kit, and grade-≤8 family education with inline citations and "not medical advice"
   — as *drafts for the ethicist/oncologist/advocate to ratify.*
4. **Provenance + license register population.** Claude proposes provenance blocks (source/url/version/
   license/boundByReference) and license-register rows, and runs a first-pass "is this redistribution?"
   triage — humans confirm license terms.
5. **Landscape monitoring / drift detection.** Claude periodically diffs upstream standard releases
   (mCODE STU bumps, EU-CDE/ERDRI revisions, OMOP-onc) and drafts migration notes.
6. **An MCP server** exposing the CDE dictionary + crosswalks as queryable tools (see §7).

**Where Claude must NOT decide (human gate is mandatory):**

- **Consent/ethics/governance validity** — clinical-ethics + patient-advocate sign-off required;
  Claude drafts, never approves (HIGH tier).
- **Clinical-data-element correctness** (is this CDE clinically meaningful/safe for Ewing?) — pediatric/
  AYA oncology + genomics reviewer required.
- **Any clinical assertion in family education** — oncologist + advocate verify every sourced claim; no
  prognosis/treatment guidance, ever.
- **License/standard terms** — never let Claude assert a license is permissive; SNOMED/LOINC/AJCC/PRO
  terms are human-verified before any binding ships. **No fabricated standards or CDE IDs** — every
  external code/identifier must resolve to a real, cited source or the build fails.
- **"Verified need" / adoption claims** — only a named partner makes those true.

---

## 6. Ten concrete optimizations

1. **Replace GRDR with live anchors** (RaDaR/NCATS + EU JRC RD-CDE + ERDRI-CDS) in O2, modules-011,
   crosswalk-023; treat GRDR's 75 CDEs as historical reference only.
2. **Add Phenopackets v2 as a first-class M4 profile** (it bridges molecular CDEs and rare-disease
   exchange) and add a Phenopacket conformance fixture.
3. **Reuse RD-CDM / RareLink as prior art** (npj Genomic Med / Sci Data 2025): adopt its
   REDCap↔FHIR↔Phenopackets mapping patterns instead of re-deriving them; cite as the methodological
   baseline in landscape-003.
4. **Define "core CDE" numerically with a selection rubric** (e.g., EU-16 ∪ mCODE-core ∪ Ewing-specific
   molecular/survivorship, Delphi-confirmed) so O2's "0 orphans" is falsifiable.
5. **Make the terminology-axis decision now** (Open Q4): recommend *openly-bindable-first* (NCIt/LOINC/
   ICD-O/HGNC) with SNOMED as an optional adopter-licensed overlay — lowers adoption cost and de-risks
   M2.
6. **Drop or re-scope ICHOM-sarcoma**: replace with validated sarcoma instruments (TESS, MSTS, SAM,
   PROMIS, PedsQL) and flag their mixed licensing in the register.
7. **Add a leading adoption indicator** to §4 (letters of intent / gap-analyses delivered to named
   registries / partner REDCap-template imports) so traction is visible before O1 signs.
8. **Operationalize FAIR** with a metric: persistent identifiers per CDE, machine-readable metadata, and
   a FAIR self-assessment in CI (cite CODATA RD-FAIR stewardship guidance).
9. **Specify the OMOP-onc mapping concretely**: HemOnc regimens for Ewing (VDC/IE chemotherapy), NAACCR
   fields, and the episode model — not just "OMOP + oncology extension."
10. **Upgrade the reviewer-scarcity mitigation**: partner with an existing ERN/advocacy ethics board for
    sign-off rather than recruiting 3 individuals; raise the risk to High impact and gate M3/M6 on the
    *board*, not on three named people.

---

## 7. Parallel & perpendicular spin-offs

1. **`rare-cancer-registry-templates` core extraction (parallel).** Most of this (provenance model,
   license register, consent module, CI gates, FHIR/OMOP/REDCap generators) is disease-agnostic. Extract
   a **generic rare-cancer registry-standard toolkit**; Ewing becomes the first *profile*. Directly
   answers Open Q8 and de-risks bus-factor.
2. **`ewing-outcomes-harmonization` (perpendicular).** A crosswalk/harmonization layer mapping existing
   Ewing trial/registry outcome definitions (EFS/OS/response, late effects) onto these CDEs — feeds O2
   and gives the standard immediate retrospective utility.
3. **`ewing-open-data-catalog` (parallel).** A FAIR catalog of *open/aggregate* Ewing datasets annotated
   with these CDEs — reinforces the no-data-store stance while demonstrating reuse value.
4. **mCODE / Phenopackets profile contribution (perpendicular).** Upstream the Ewing value sets as an
   mCODE/Phenopackets *extension or example*, gaining distribution and credibility from established
   communities rather than competing with them.
5. **MCP server (`ewing-cde-mcp`).** Expose the CDE dictionary, crosswalks, value sets, and license
   register as MCP tools so any agent/EHR-integration can query "what's the mCODE/OMOP/Phenopacket
   mapping for Ewing tumor stage?" — a low-cost, high-visibility adoption accelerant.
6. **Generalized "registry-standard linter" (parallel).** The provenance-lint + license-audit +
   crosswalk-completeness + synthetic-check tooling is reusable across the whole Elyos cancer track —
   ship it as a standalone package.
7. **`ewing-survivorship-late-effects` CDE module (perpendicular).** The genuinely net-new clinical
   contribution; could stand alone and be adopted even by registries not taking the full standard.

---

## 8. Open questions for the maintainer

1. **Anchor-standard correction:** Do you accept replacing GRDR with RaDaR/NCATS + EU JRC RD-CDE +
   ERDRI-CDS as the mandatory crosswalk anchors? (Affects O2, M1, M2.)
2. **Phenopackets:** add it as a required M4 profile and a conformance fixture, or keep it a "future"
   (Beacon-074) backlog item?
3. **Reuse RD-CDM/RareLink:** adopt the published REDCap↔FHIR↔Phenopackets RD-CDM as the methodological
   baseline (and possibly the generic core), rather than deriving mappings independently?
4. **"Core" definition:** what is the numeric size and selection method for the core CDE set, and who
   confirms it (Delphi panel? the secured clinical reviewer)?
5. **Terminology axis (Q4 from plan):** can we commit now to openly-bindable-first with SNOMED optional,
   to unblock M2 and lower adoption cost?
6. **ICHOM-sarcoma:** confirmed there is no published set — switch to TESS/MSTS/SAM/PROMIS/PedsQL?
7. **Generic-core extraction:** is Ewing the pilot profile of a shared `rare-cancer-registry-templates`
   core, and should the toolkit be extracted upstream before M2 hardens?
8. **Partner/reviewer path:** pursue an ERN/EURACAN or Euro-Ewing Consortium *institutional* partner
   (which brings an ethics board + clinical reviewers + a real adoption channel) rather than recruiting
   three individuals separately?
9. **Jurisdiction:** commit to dual US(HIPAA)+EU(GDPR/EHDS) from v1, or US-first with an EU annex?
   (A rare cancer is inherently international — leaning dual seems strategically necessary.)
