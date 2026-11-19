---
type: user-story-jira-breakdown
source: requirements-package.md (A-07)
feature_set: game-selection
product: charitable-gaming
initiative: EVOCR-8009
---

# User Story to Jira Breakdown — Game Selection (EVOCR-8009)

## Executive Summary

This breakdown converts the Game Selection Requirements Package (A-07) into INVEST-compliant, Definition-of-Ready Jira stories small enough to be built and tested inside a single two-week sprint. Only the Requirements Package was used as source and authoritative reference. The User Story document (A-04) and UX Design Requirements (A-08) were intentionally excluded at the user's request, so UX references are marked N/A and user-intent narrative is derived from the requirement text and the package's own User Story traceability table.

- Slicing strategy: primary axis is **user role / persona** for access and oversight behaviors (Charity, Regulator, IGT Support, Distributor, Retailer), and **workflow behavior** for the Charity-centric selection pipeline. Technical concern types (UI, rule engine, audit/telemetry, distribution jobs) are split into separate stories per the rules.
- Epics: 11 (one per capability area).
- Stories: 58 committed across three recommended delivery phases.
- Deferred items: 3 (see Deferred Register).
- Story counts by phase: Phase 1 (core behavior) 26, Phase 2 (UI/operational) 25, Phase 3 (access control, audit, hardening) 7.

Delivery phases below indicate a recommended build order only. Sprint assignment is out of scope and belongs to the project manager after estimation.

## Epic Structure

| Epic | Capability area | Source stories | Primary FRs |
| --- | --- | --- | --- |
| CG-CTX | Access, Charity and Site Context | US-001 | FR-001, FR-002, FR-003, FR-003.1, FR-003.2, FR-003.3 |
| CG-CAT | Visual Game Catalog | US-002 | FR-004, FR-005, FR-005.1, FR-006, FR-006.1, FR-006.2, FR-007, FR-058 |
| CG-SEL | Valid Selection Rules | US-003 | FR-008, FR-009, FR-010, FR-011 |
| CG-DRFT | Draft Management | US-004, US-005, US-006 | FR-012, FR-013, FR-014, FR-015, FR-015.1, FR-016, FR-017, FR-017.1 |
| CG-DASH | Task-Oriented Dashboard | US-007 | FR-021, FR-022.x, FR-053, FR-054, FR-055, FR-056, FR-057 |
| CG-ACT | Validation, Acknowledgement and Activation | US-008, US-009, US-010 | FR-023, FR-024, FR-025, FR-026, FR-027, FR-028, FR-029, FR-029.1, FR-030, FR-031, FR-038 |
| CG-INQ | In-Quarter Changes and Equipment | US-015, US-016, US-017 | FR-031.1, FR-047.2 |
| CG-OVS | Oversight, Support and Audit | US-011, US-012 | FR-032, FR-033, FR-034, FR-035 |
| CG-META | Game Metadata and Management | US-013 | FR-036, FR-037, FR-039, FR-040, FR-041 |
| CG-SW | Game Switching and Distribution | US-014 | FR-042, FR-043, FR-044, FR-045, FR-046, FR-047, FR-047.1 |
| CG-USR | User and Role Management | US-011, US-013 | FR-048, FR-049, FR-050, FR-051, FR-052 |

## Deferred Register

| ID | Deferred item | Source story | Recommended Jira type | Reason |
| --- | --- | --- | --- | --- |
| D-001 | Quarterly Workflow activation boundaries: regulatory grace-period lengths and the rule preventing starting and closing games within the same pre-quarter period (FR-025) are not numerically defined in the package. | US-010 | Spike | Concrete boundary values and the Quarterly Workflow state machine must be defined before `CG-ACT-ELIGIBILITY-005` can be committed with explicit test data. |
| D-002 | FR-023 requirement text is corrupted: it splices an acknowledgement/revalidation statement with a separate sentence about a site-level Game Switch summary view. The intended requirement and whether a site-level Game Switch summary view is in scope are unclear. | US-008 | Deferred Requirement | Needs authoring clarification. The revalidate-before-activation behavior is carried by `CG-ACT-REVALIDATE-001` from the unambiguous portion plus FR-024; the summary-view fragment is held here pending confirmation. |
| D-003 | Draft-version history, restore, and superseding are explicitly out of scope for Phase 1 per the package Assumptions. | US-006 | Deferred Requirement | Reintroduce only if the out-of-scope assumption is reversed. |

## Story Backlog (grouped by epic)

### CG-CTX — Access, Charity and Site Context

#### CG-CTX-CHARITY-001 Establish Charity and site context (Charity) (Charity user)

- Epic: CG-CTX
- Phase: Phase 1 (core behavior)
- Source story: US-001
- Requirement refs: FR-001, FR-002, FR-051
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, charity-user

As a Charity user
I want to establish and confirm my authorized Charity and licensed site before starting Game Selection
So that I only work within the scope I am permitted to manage

Scope

- In scope: presenting only authorized charities and sites after authentication; requiring explicit Charity and site selection when the user is associated with more than one; carrying the chosen context into the Game Selection workflow.
- Out of scope: role creation (CG-USR-CHARITYROLE-004); Regulator and support scopes.

Acceptance Criteria

- Given a Charity user associated with exactly one Charity and one site, When they authenticate, Then that Charity and site are established automatically and shown as the active context.
- Given a Charity user associated with two or more charities or sites, When they enter Game Selection, Then the system blocks work until both a Charity and a site are explicitly selected.
- Given an established context, When the user begins a draft, Then only games, drafts, and data for the selected Charity and site are available (tenant isolation).
- Given a Charity or site the user is not authorized for, When the context list renders, Then that Charity or site is not presented.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified (CG-USR-CHARITYROLE-004); implementable and testable within a single two-week sprint.

#### CG-CTX-REGULATOR-002 Regulator statewide read-only scope (Regulator)

- Epic: CG-CTX
- Phase: Phase 3 (access control, audit, hardening)
- Source story: US-001, US-011
- Requirement refs: FR-003, FR-052, NFR-001, NFR-008
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, regulator

As a Regulator user
I want to select any Charity and site within my statewide oversight scope in read-only mode
So that I can review selections without being able to change them

Scope

- In scope: statewide selection of any Charity and site within authorized oversight; presenting all Game Selection surfaces as read-only.
- Out of scope: audit history retrieval (CG-OVS-REGVIEW-001); operational edits.

Acceptance Criteria

- Given a Regulator user, When they open Game Selection, Then any Charity and site within their authorized statewide scope is selectable.
- Given a Regulator has selected a Charity and site, When any Game Selection view renders, Then all editing controls (add, remove, save, acknowledge, activate) are absent or disabled.
- Given a Regulator user, When they attempt a write action via a direct request, Then the action is rejected and no data changes.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Regulator); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated (read-only, statewide); dependencies identified (CG-USR-COMPLIANCEROLE-005); implementable and testable within a single two-week sprint.

#### CG-CTX-SUPPORT-003 Delegated access for IGT Support and Distributor (IGT Support / Distributor)

- Epic: CG-CTX
- Phase: Phase 3 (access control, audit, hardening)
- Source story: US-001, US-011
- Requirement refs: FR-003.1
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, igt-support, distributor

As an IGT Support or Distributor user
I want to access and perform Charity Game Selection workflows when my IntelligenEVO permission allows acting on a Charity's behalf
So that I can assist a Charity within authorized limits

Scope

- In scope: entering the Charity Game Selection workflow when the IntelligenEVO User permission model authorizes acting for the Charity.
- Out of scope: the audit recording of delegated actions (CG-OVS-DELEGATE-002).

Acceptance Criteria

- Given an IGT Support or Distributor user whose IntelligenEVO permission authorizes a Charity, When they select that Charity and site, Then they can perform the Charity Game Selection workflow.
- Given the same user and a Charity not covered by their permission, When they attempt to select it, Then it is not available.
- Given a delegated session, When any workflow action is performed, Then the action proceeds under the Charity context while preserving the actual user identity for downstream audit.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Support/Distributor delegated); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-USR-OPSROLE-003, CG-OVS-DELEGATE-002); implementable and testable within a single two-week sprint.

#### CG-CTX-RETAILER-004 Retailer restricted experience hides Game Selection (Retailer)

- Epic: CG-CTX
- Phase: Phase 3 (access control, audit, hardening)
- Source story: US-001
- Requirement refs: FR-003.2
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, retailer

As a Retailer user
I want the standard restricted EVO Retailer experience
So that Game Selection and related audit information are not exposed to me

Scope

- In scope: presenting the standard restricted Retailer experience; hiding Game Selection and related audit information.
- Out of scope: Retailer-specific EVO functionality unrelated to Game Selection.

Acceptance Criteria

- Given a Retailer user, When they sign in, Then no Game Selection module, menu entry, or link is visible.
- Given a Retailer user, When they request a Game Selection or related audit URL directly, Then access is denied.
- Given a Retailer user, When navigation renders, Then only the standard restricted Retailer experience is shown.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Retailer); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (none); implementable and testable within a single two-week sprint.

#### CG-CTX-ALLROLES-005 Hide unauthorized modules and actions (All roles)

- Epic: CG-CTX
- Phase: Phase 1 (core behavior)
- Source story: US-001
- Requirement refs: FR-003.3, NFR-001
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, access-control

As any authenticated user
I want unauthorized modules and actions hidden rather than shown and failing on use
So that I am not offered controls I cannot use

Scope

- In scope: a shared access-guard pattern that hides unauthorized modules and actions at render time.
- Out of scope: per-role scope definitions (covered by the role stories).

Acceptance Criteria

- Given a user lacking permission for a module, When the UI renders, Then the module and its entry points are not displayed.
- Given a user lacking permission for an action, When a screen renders, Then the action control is hidden rather than shown and failing after the user attempts it.
- Given a hidden action is invoked via a direct request, When the server processes it, Then it is rejected with an authorization error.

Definition of Ready

- No TBD text; exactly one primary-axis variant (cross-role guard pattern); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (none); implementable and testable within a single two-week sprint.

### CG-CAT — Visual Game Catalog

#### CG-CAT-DISPLAY-001 Display the visual catalog for next-quarter selection (Charity user)

- Epic: CG-CAT
- Phase: Phase 1 (core behavior)
- Source story: US-002
- Requirement refs: FR-004, FR-005, FR-005.1, NFR-005, NFR-006
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, catalog

As a Charity user
I want a visual catalog of games eligible for next-quarter selection
So that I can see what I am permitted to choose for my site

Scope

- In scope: rendering the visual catalog driven by statewide State Game Approval and the selected site's applicable IGT cabinet compatibility; using authoritative catalog metadata as the source of truth; browser rendering on laptop and tablet.
- Out of scope: in-quarter additions view (CG-INQ-ADD-001); entry detail attributes (CG-CAT-ENTRY-002).

Acceptance Criteria

- Given a State-approved game compatible with the selected site's IGT cabinet, When the catalog loads, Then the game appears regardless of whether it is already installed at the site.
- Given a State-approved game not compatible with the site's cabinet, When the catalog loads, Then the game is not offered for that site.
- Given a game not State-approved for North Dakota, When the catalog loads, Then it is absent for every charity and site.
- Given a Charity user on a tablet, When the catalog loads, Then it renders usably without requiring technical platform knowledge.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user, next-quarter catalog); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated (statewide approval); dependencies identified (CG-META-PROPAGATE-002); implementable and testable within a single two-week sprint.

#### CG-CAT-ENTRY-002 Show brochure-style catalog entry detail (Charity user)

- Epic: CG-CAT
- Phase: Phase 1 (core behavior)
- Source story: US-002
- Requirement refs: FR-006
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, catalog

As a Charity user
I want each catalog entry to show the approved marketing and configuration details
So that I can compare games before selecting

Scope

- In scope: displaying per entry the approved graphic, title, supported cabinets, available Cost Per Play values, payout percentage, top-prize options, State Game Approval date, availability indicators, a brochure-style detail view, a new or previously returning indicator, and current-quarter share of play when data exists.
- Out of scope: selecting values (CG-CAT-CPP-003); hiding technical identifiers (CG-CAT-HIDE-004).

Acceptance Criteria

- Given a catalog entry with complete metadata, When it renders, Then the approved graphic, title, supported cabinets, Cost Per Play values, payout percentage, top-prize options, and State Game Approval date are shown.
- Given an entry, When the user opens its detail, Then a brochure-style detail view and the new or previously returning indicator are shown.
- Given an entry with share-of-play data for the current quarter, When it renders, Then current-quarter share of play is shown; given no such data, Then the share-of-play field is omitted rather than shown as zero.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-CAT-DISPLAY-001); implementable and testable within a single two-week sprint.

#### CG-CAT-CPP-003 Select Cost Per Play, payout and top-prize per entry (Charity user)

- Epic: CG-CAT
- Phase: Phase 1 (core behavior)
- Source story: US-002
- Requirement refs: FR-006.1, FR-006.2, FR-010
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, catalog

As a Charity user
I want to choose Cost Per Play values and their configuration for a catalog entry
So that I can configure each selected title

Scope

- In scope: selecting all or one of the available Cost Per Play values for an entry; for each selected Cost Per Play, selecting one payout percentage and one top-prize option from predefined approved values.
- Out of scope: title and Cost Per Play count limits (CG-SEL-LIMITS-002).

Acceptance Criteria

- Given an entry with multiple available Cost Per Play values, When the user configures it, Then they can select all values or a single value.
- Given a selected Cost Per Play, When the user configures it, Then exactly one payout percentage and one top-prize option can be chosen from the predefined approved set.
- Given payout and top-prize values, When shown, Then they are non-editable and selectable only from approved variants.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-CAT-ENTRY-002); implementable and testable within a single two-week sprint.

#### CG-CAT-HIDE-004 Hide technical identifiers from Charity users (Charity user)

- Epic: CG-CAT
- Phase: Phase 1 (core behavior)
- Source story: US-002
- Requirement refs: FR-007
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, catalog

As a Charity user
I want technical identifiers kept hidden
So that I see only business-relevant game information

Scope

- In scope: suppressing package, math-definition, paytable, routing, deal-inventory, active-pool, and similar technical identifiers from all Charity-facing catalog and selection views.
- Out of scope: internal job records retention (CG-ACT-JOBAUDIT-008).

Acceptance Criteria

- Given a Charity user viewing the catalog, When any entry or detail renders, Then no package, math-definition, paytable, routing, deal-inventory, or active-pool identifier is displayed.
- Given a Charity user inspecting page data, When the response is examined, Then technical identifiers are not present in Charity-facing payloads.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-CAT-ENTRY-002); implementable and testable within a single two-week sprint.

#### CG-CAT-FILTER-005 Filter catalog by all, new or returning games (Charity user)

- Epic: CG-CAT
- Phase: Phase 2 (UI/operational)
- Source story: US-002, US-003
- Requirement refs: FR-058
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, catalog

As a Charity user
I want to filter the catalog by all, new, or previously returning games
So that I can find relevant titles faster

Scope

- In scope: a catalog filter with the options all games, new games, and previously returning games.
- Out of scope: the new or returning indicator itself (CG-CAT-ENTRY-002).

Acceptance Criteria

- Given the catalog, When the user selects the new-games filter, Then only games flagged as new are listed.
- Given the catalog, When the user selects the returning-games filter, Then only previously returning games are listed.
- Given any filter, When the user selects all games, Then the unfiltered catalog is restored.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-CAT-ENTRY-002); implementable and testable within a single two-week sprint.

### CG-SEL — Valid Selection Rules

#### CG-SEL-VALIDCOMBO-001 Present only valid predefined combinations (Charity user)

- Epic: CG-SEL
- Phase: Phase 1 (core behavior)
- Source story: US-003
- Requirement refs: FR-008, FR-010
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, rules

As a Charity user
I want only valid predefined game and configuration combinations offered
So that I cannot build an unsupported selection

Scope

- In scope: presenting only valid predefined game and configuration combinations for the target quarter and compatible IGT cabinet; allowing selection only from predefined approved configurations where multiple approved variants exist.
- Out of scope: count limits (CG-SEL-LIMITS-002); contextual validation and blocking (CG-SEL-VALIDATION-003).

Acceptance Criteria

- Given a target quarter and compatible cabinet, When configuration options render, Then only valid predefined combinations are selectable.
- Given a game with multiple approved payout or top-prize variants, When the user configures it, Then only approved variants are offered and none are free-text editable.
- Given a combination invalid for the target quarter or cabinet, When options render, Then it is not presented.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-CAT-CPP-003); implementable and testable within a single two-week sprint.

#### CG-SEL-LIMITS-002 Enforce title and Cost Per Play limits with live count (Charity user)

- Epic: CG-SEL
- Phase: Phase 1 (core behavior)
- Source story: US-003
- Requirement refs: FR-009
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, rules

As a Charity user
I want the selection limits enforced with a visible count
So that I stay within the allowed number of titles and Cost Per Play values

Scope

- In scope: enforcing a maximum of twelve game titles per site per quarter and up to five approved Cost Per Play values per selected title; Cost Per Play choices not increasing the title count; displaying the current title count during selection.
- Out of scope: in-quarter cumulative-limit additions (CG-INQ-ADD-001).

Acceptance Criteria

- Given eleven titles already selected, When the user adds a twelfth title, Then it is accepted and the count displays 12 of 12.
- Given twelve titles selected, When the user attempts a thirteenth, Then the action is blocked with a maximum-of-twelve-titles message.
- Given a selected title with five Cost Per Play values, When the user adds a sixth, Then it is blocked while the title count remains unchanged.
- Given any selected title, When Cost Per Play values are added, Then the title count does not increase.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit (12, 13, 5, 6); regulatory/role constraints stated; dependencies identified (CG-SEL-VALIDCOMBO-001); implementable and testable within a single two-week sprint.

#### CG-SEL-VALIDATION-003 Block unsupported combinations with contextual validation (Charity user)

- Epic: CG-SEL
- Phase: Phase 1 (core behavior)
- Source story: US-003, US-008
- Requirement refs: FR-011, NFR-004
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, rules

As a Charity user
I want unsupported combinations prevented with contextual validation
So that I can still save an incomplete draft but cannot finalize an invalid one

Scope

- In scope: preventing unsupported combinations from being selected; showing contextual validation; allowing an incomplete draft to be saved while blocking acknowledgement and activation when critical errors remain.
- Out of scope: pre-activation revalidation (CG-ACT-REVALIDATE-001).

Acceptance Criteria

- Given an unsupported combination, When the user attempts to select it, Then selection is prevented and contextual validation explains why.
- Given a draft with non-critical gaps, When the user saves, Then the save succeeds and the draft is retained as incomplete.
- Given a draft with critical errors, When the user attempts acknowledgement or activation, Then both are blocked until the errors are resolved.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-SEL-VALIDCOMBO-001); implementable and testable within a single two-week sprint.

### CG-DRFT — Draft Management

#### CG-DRFT-ADDREMOVE-001 Add and remove titles in the next-quarter draft (Charity user)

- Epic: CG-DRFT
- Phase: Phase 1 (core behavior)
- Source story: US-004
- Requirement refs: FR-012, FR-013, FR-014
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, draft

As a Charity user
I want to add and remove eligible titles in my next-quarter draft
So that I can shape the selection without affecting the active quarter

Scope

- In scope: adding eligible titles up to the twelve-title maximum; removing a title and its associated Cost Per Play and configuration choices from an unacknowledged draft; eligibility based on the current North Dakota-approved catalog and applicable cabinet compatibility.
- Out of scope: copying between sites (CG-DRFT-COPY-002).

Acceptance Criteria

- Given an eligible title and fewer than twelve selected, When the user adds it, Then it joins the draft.
- Given a title in an unacknowledged draft, When the user removes it, Then its Cost Per Play and configuration choices are removed with it.
- Given a title removed from the draft, When the active quarter is inspected, Then the active quarter is unchanged.
- Given eligibility, When determined, Then it uses the current North Dakota-approved catalog and cabinet compatibility, not Charity- or site-specific approval.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-SEL-LIMITS-002); implementable and testable within a single two-week sprint.

#### CG-DRFT-COPY-002 Start a draft from a baseline or copy between sites (Charity user)

- Epic: CG-DRFT
- Phase: Phase 2 (UI/operational)
- Source story: US-005
- Requirement refs: FR-015, FR-016
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, draft

As a Charity user
I want to start a draft from a prior configuration or copy from another authorized site
So that I do not rebuild a similar selection from scratch

Scope

- In scope: starting a draft from the same site's most recently activated configuration or from a saved or activated selection at another authorized site; copying selection choices only.
- Out of scope: per-site cabinet validation on multi-site copy (CG-DRFT-MULTISITE-003).

Acceptance Criteria

- Given the same site's most recently activated configuration, When the user starts a draft from it, Then the selection choices are pre-populated.
- Given a saved or activated selection at another authorized site, When the user copies it, Then the selection choices are copied.
- Given any copy, When it completes, Then site identifiers, deployment state, deal information, active-pool data, and gaming stamps are not copied.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-DRFT-ADDREMOVE-001); implementable and testable within a single two-week sprint.

#### CG-DRFT-MULTISITE-003 Validate cabinet compatibility per target site on copy (Charity user)

- Epic: CG-DRFT
- Phase: Phase 2 (UI/operational)
- Source story: US-005
- Requirement refs: FR-015.1
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, draft

As a Charity user
I want each target site validated independently when I apply a selection across sites
So that incompatible titles are caught per site

Scope

- In scope: validating each target site's applicable IGT cabinet compatibility independently when a selection is applied across multiple authorized sites.
- Out of scope: the copy mechanism itself (CG-DRFT-COPY-002).

Acceptance Criteria

- Given a selection applied to two authorized sites with different cabinets, When applied, Then each site is validated against its own cabinet compatibility.
- Given a title incompatible with one target site's cabinet, When the selection is applied, Then that title is flagged or excluded for that site while remaining valid for compatible sites.
- Given all target sites compatible, When applied, Then the selection is accepted for every site.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user, multi-site apply); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-DRFT-COPY-002); implementable and testable within a single two-week sprint.

#### CG-DRFT-SAVE-004 Save and resume one editable draft per site and quarter (Charity user)

- Epic: CG-DRFT
- Phase: Phase 1 (core behavior)
- Source story: US-006
- Requirement refs: FR-017, NFR-007
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, draft

As a Charity user
I want to save and resume a single draft per site and quarter
So that I can work incrementally without side effects

Scope

- In scope: maintaining one editable draft per site and target quarter; ensuring saving or copying does not affect the active quarter or initiate downloads, deal preparation, gaming-stamp assignment, or EGM configuration.
- Out of scope: audit of draft actions (CG-DRFT-AUDIT-005).

Acceptance Criteria

- Given a site and target quarter, When the user saves a draft, Then exactly one editable draft exists for that site and quarter.
- Given a saved draft, When the user returns later, Then the draft resumes with its prior choices intact.
- Given a save or copy, When it completes, Then no download, deal preparation, gaming-stamp assignment, or EGM configuration is initiated and the active quarter is unchanged.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-DRFT-ADDREMOVE-001); implementable and testable within a single two-week sprint.

#### CG-DRFT-AUDIT-005 Audit material draft actions without mandatory reason (Charity user)

- Epic: CG-DRFT
- Phase: Phase 3 (access control, audit, hardening)
- Source story: US-006
- Requirement refs: FR-017.1
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, audit

As a compliance stakeholder
I want material draft actions recorded in the standard audit log
So that routine saves are traceable without friction

Scope

- In scope: recording material draft actions in the standard audit log without requiring a mandatory change reason for routine saves.
- Out of scope: immutability and tamper-evidence of audit records (CG-OVS-AUDITLOG-003).

Acceptance Criteria

- Given a material draft action (save, add, remove), When it completes, Then a standard audit entry is recorded with actor, site, quarter, and timestamp.
- Given a routine save, When recorded, Then no change reason is required from the user.
- Given a sequence of draft actions, When the audit log is reviewed, Then each material action appears in order.

Definition of Ready

- No TBD text; exactly one primary-axis variant (draft audit); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-OVS-AUDITLOG-003); implementable and testable within a single two-week sprint.

### CG-DASH — Task-Oriented Dashboard

#### CG-DASH-OVERVIEW-001 Show status and next required action (Charity user)

- Epic: CG-DASH
- Phase: Phase 2 (UI/operational)
- Source story: US-007
- Requirement refs: FR-021
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, dashboard

As a Charity user
I want a task-oriented dashboard showing status, blockers, and the next action
So that I know what to do next for each site and quarter

Scope

- In scope: a dashboard showing site and quarter status, blockers, and the next required action.
- Out of scope: the selection list (CG-DASH-LIST-002); status model internals (CG-DASH-STATUSMODEL-005).

Acceptance Criteria

- Given a site and quarter with an incomplete draft, When the dashboard loads, Then its status and the next required action are shown.
- Given a site with a blocking error, When the dashboard loads, Then the blocker is surfaced with a clear next step.
- Given a site ready to activate, When the dashboard loads, Then the next action indicates activation.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-DASH-STATUSMODEL-005); implementable and testable within a single two-week sprint.

#### CG-DASH-LIST-002 List current and past selections with identifiers (Charity user)

- Epic: CG-DASH
- Phase: Phase 2 (UI/operational)
- Source story: US-007
- Requirement refs: FR-053, FR-054, FR-055
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, dashboard

As a Charity user
I want the dashboard to list current and past selections with site, charity, and quarter
So that I can review history at a glance

Scope

- In scope: listing current and past Game Selections; showing Site, Charity, and Quarter for each; defaulting the view to the current quarter's selections.
- Out of scope: filtering (CG-DASH-FILTER-003); opening a selection (CG-DASH-OPEN-004).

Acceptance Criteria

- Given existing current and past selections, When the dashboard loads, Then both are listed.
- Given any listed selection, When it renders, Then its Site, Charity, and Quarter are shown.
- Given no explicit filter, When the dashboard loads, Then the current quarter's selections are displayed by default.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-DASH-OVERVIEW-001); implementable and testable within a single two-week sprint.

#### CG-DASH-FILTER-003 Filter the dashboard by site and charity (Charity user)

- Epic: CG-DASH
- Phase: Phase 2 (UI/operational)
- Source story: US-007
- Requirement refs: FR-056
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, dashboard

As a Charity user
I want to filter the dashboard by gaming site, by charity, or by both
So that I can focus on a specific scope

Scope

- In scope: filtering the dashboard by Gaming Site, by Charity, or by a Gaming Site and Charity combination.
- Out of scope: the underlying list (CG-DASH-LIST-002).

Acceptance Criteria

- Given multiple sites, When the user filters by one Gaming Site, Then only that site's selections are shown.
- Given multiple charities, When the user filters by one Charity, Then only that Charity's selections are shown.
- Given a Site and Charity combination filter, When applied, Then only selections matching both are shown.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-DASH-LIST-002); implementable and testable within a single two-week sprint.

#### CG-DASH-OPEN-004 Open a selection from the dashboard (Charity user)

- Epic: CG-DASH
- Phase: Phase 2 (UI/operational)
- Source story: US-007
- Requirement refs: FR-057
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, dashboard

As a Charity user
I want to open a Game Selection from the dashboard
So that I can view it or make authorized in-quarter changes

Scope

- In scope: opening a Game Selection from the dashboard to view it and to make supported authorized in-quarter changes.
- Out of scope: the in-quarter add and close behaviors themselves (CG-INQ-ADD-001, CG-INQ-CLOSE-002).

Acceptance Criteria

- Given a listed selection, When the user opens it, Then its Game Selection detail is displayed.
- Given an authorized user and an active-quarter selection, When opened, Then supported in-quarter change actions are available.
- Given a read-only user, When they open a selection, Then it displays without editing actions.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-DASH-LIST-002, CG-INQ-ADD-001); implementable and testable within a single two-week sprint.

#### CG-DASH-STATUSMODEL-005 Separate status types and hide Superseded (Charity user)

- Epic: CG-DASH
- Phase: Phase 2 (UI/operational)
- Source story: US-007
- Requirement refs: FR-022, FR-022.4, FR-022.5
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, dashboard

As a Charity user
I want workflow, activation, and technical statuses shown separately with a defined set of values
So that I can read status without confusion

Scope

- In scope: separating Charity-facing workflow status, activation status, and internal technical processing status; not presenting a Superseded status on the dashboard; providing Game Selection status values (Not Started, In Progress, Completed, Blocked) and Activation status values (Not Started, Pending User Confirmation, Pending Quarterly Close, Activated).
- Out of scope: per-game download and switch status (CG-DASH-DOWNLOAD-006, CG-DASH-SWITCH-007).

Acceptance Criteria

- Given a selection, When the dashboard renders status, Then workflow, activation, and technical statuses are presented as distinct values.
- Given Game Selection status, When shown, Then it is one of Not Started, In Progress, Completed, or Blocked.
- Given Activation status, When shown, Then it is one of Not Started, Pending User Confirmation, Pending Quarterly Close, or Activated.
- Given any selection, When status renders, Then no Superseded status is displayed on the dashboard.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (none); implementable and testable within a single two-week sprint.

#### CG-DASH-DOWNLOAD-006 Show per-game download and installation status (Charity user)

- Epic: CG-DASH
- Phase: Phase 2 (UI/operational)
- Source story: US-007
- Requirement refs: FR-022.1
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, dashboard

As a Charity user
I want each selected game's download and installation status shown
So that I can see technical processing progress

Scope

- In scope: displaying the game download and installation status for each selected game under internal technical processing status.
- Out of scope: the Game Switch on/off status (CG-DASH-SWITCH-007).

Acceptance Criteria

- Given a selected game being distributed, When the dashboard renders, Then its download and installation status is shown.
- Given multiple selected games, When they render, Then each game's download and installation status is shown independently.
- Given an installation failure, When it occurs, Then the failed status is reflected for that game.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-DASH-STATUSMODEL-005); implementable and testable within a single two-week sprint.

#### CG-DASH-SWITCH-007 Show per-game Game Switch on and off status (Charity user)

- Epic: CG-DASH
- Phase: Phase 2 (UI/operational)
- Source story: US-007
- Requirement refs: FR-022.2
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, dashboard

As a Charity user
I want each selected game's Game Switch on or off status shown
So that I can see which games are switched on

Scope

- In scope: displaying the Game Switch on or off status for each selected game.
- Out of scope: real-time monitoring tools (CG-DASH-MONITOR-008).

Acceptance Criteria

- Given a selected game switched on, When the dashboard renders, Then its Game Switch status shows on.
- Given a selected game switched off, When the dashboard renders, Then its Game Switch status shows off.
- Given multiple selected games, When they render, Then each shows its own switch status.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-DASH-STATUSMODEL-005); implementable and testable within a single two-week sprint.

#### CG-DASH-MONITOR-008 Real-time monitoring via Game Switch or Job Monitor (IGT Support)

- Epic: CG-DASH
- Phase: Phase 2 (UI/operational)
- Source story: US-007
- Requirement refs: FR-022.3, NFR-009
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, monitoring

As an IGT Support user
I want monitoring access through the Game Switch Monitor or Job Monitor
So that I can track each selected game's status in real time

Scope

- In scope: providing monitoring access through the existing Game Switch Monitor or Job Monitor Intelligence within EVO so each selected game's status can be tracked in real time.
- Out of scope: the dashboard status fields (CG-DASH-SWITCH-007).

Acceptance Criteria

- Given a selected game with an active job, When the user opens the Game Switch Monitor or Job Monitor, Then the game's current status is shown in real time.
- Given a status change, When it occurs, Then the monitor reflects the new status without a manual reload beyond the tool's normal refresh.
- Given a user without monitoring permission, When they attempt access, Then access is denied.

Definition of Ready

- No TBD text; exactly one primary-axis variant (IGT Support); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-SW-AUTOJOB-004); implementable and testable within a single two-week sprint.

### CG-ACT — Validation, Acknowledgement and Activation

#### CG-ACT-REVALIDATE-001 Revalidate and show errors before acknowledgement (Charity user)

- Epic: CG-ACT
- Phase: Phase 1 (core behavior)
- Source story: US-008
- Requirement refs: FR-024, NFR-004 (FR-023 partial; see D-002)
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, validation

As a Charity user
I want the full selection revalidated with errors shown near the affected game
So that I resolve problems before finalizing

Scope

- In scope: revalidating the complete selection immediately before activation; showing validation errors near the affected game or field; blocking acknowledgement while critical errors remain.
- Out of scope: the corrupted site-level Game Switch summary fragment of FR-023 (see Deferred Register D-002).

Acceptance Criteria

- Given a saved incomplete draft, When the user initiates activation, Then the complete selection is revalidated immediately beforehand.
- Given a validation error, When shown, Then it appears near the affected game or field.
- Given a critical error remains, When the user attempts acknowledgement, Then acknowledgement is blocked.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-SEL-VALIDATION-003); implementable and testable within a single two-week sprint.

#### CG-ACT-MINCONFIG-002 Require at least one valid game and Cost Per Play (Charity user)

- Epic: CG-ACT
- Phase: Phase 1 (core behavior)
- Source story: US-009
- Requirement refs: FR-026
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, activation

As a Charity user
I want activation to require at least one valid game and Cost Per Play
So that an empty quarter cannot be activated

Scope

- In scope: requiring at least one valid game and Cost Per Play configuration before a quarter can be activated.
- Out of scope: Quarterly Workflow timing eligibility (CG-ACT-ELIGIBILITY-005).

Acceptance Criteria

- Given a selection with zero valid game and Cost Per Play configurations, When the user attempts activation, Then it is blocked.
- Given a selection with at least one valid game and Cost Per Play, When the user attempts activation, Then this requirement is satisfied.
- Given a selection whose only game becomes invalid, When activation is attempted, Then it is blocked until a valid configuration exists.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-ACT-REVALIDATE-001); implementable and testable within a single two-week sprint.

#### CG-ACT-RECORD-003 Record acknowledgement attributes (Charity user)

- Epic: CG-ACT
- Phase: Phase 1 (core behavior)
- Source story: US-009
- Requirement refs: FR-027
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, activation

As a compliance stakeholder
I want the full acknowledgement context recorded
So that each acknowledgement is attributable and complete

Scope

- In scope: recording the actual user, role, Charity, site, target quarter, timestamp, and the complete configuration shown at acknowledgement.
- Out of scope: immutability guarantees (CG-OVS-AUDITLOG-003).

Acceptance Criteria

- Given an acknowledgement, When it completes, Then the actual user, role, Charity, site, target quarter, and timestamp are recorded.
- Given a delegated session, When acknowledgement is recorded, Then the actual acting user is captured, not only the Charity context.
- Given an acknowledgement, When recorded, Then the complete configuration shown at that moment is stored.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-ACT-MINCONFIG-002); implementable and testable within a single two-week sprint.

#### CG-ACT-LOCK-004 Lock the configuration on acknowledgement (Charity user)

- Epic: CG-ACT
- Phase: Phase 1 (core behavior)
- Source story: US-009
- Requirement refs: FR-028, NFR-007
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, activation

As a Charity user
I want the acknowledged configuration locked for the quarter
So that it cannot be altered after commitment

Scope

- In scope: locking selected game and Cost Per Play configurations once acknowledged for activation; keeping active configurations locked for the remainder of the quarter.
- Out of scope: authorized in-quarter additions and closures (CG-INQ-ADD-001, CG-INQ-CLOSE-002).

Acceptance Criteria

- Given an acknowledged configuration, When the user tries to edit a selected game or Cost Per Play, Then the edit is prevented.
- Given an active quarter, When the user views the configuration, Then it remains locked for the remainder of the quarter.
- Given a lock is in place, When an unauthorized edit is attempted via a direct request, Then it is rejected.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-ACT-RECORD-003); implementable and testable within a single two-week sprint.

#### CG-ACT-ELIGIBILITY-005 Govern activation eligibility via Quarterly Workflow (Charity user)

- Epic: CG-ACT
- Phase: Phase 3 (access control, audit, hardening)
- Source story: US-010
- Requirement refs: FR-025
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, activation

As a Charity user
I want activation eligibility governed by the approved Quarterly Workflow
So that I can only activate within allowed quarter boundaries

Scope

- In scope: governing activation eligibility using the approved Quarterly Workflow, including applicable quarter boundaries, regulatory grace periods, closure state, and restrictions on starting and closing games within the same pre-quarter period.
- Out of scope: the numeric grace-period and pre-quarter boundary values (see Deferred Register D-001, spike required before commit).

Acceptance Criteria

- Given a quarter within its allowed activation window, When the user activates, Then activation is permitted.
- Given a quarter outside its allowed window or in a closed state, When the user attempts activation, Then it is blocked with the applicable boundary reason.
- Given the pre-quarter restriction, When a user attempts to start and close a game within the same pre-quarter period, Then the action is prevented.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit (pending D-001 boundary values); regulatory/role constraints stated; dependencies identified (D-001 spike, CG-ACT-LOCK-004); implementable and testable within a single two-week sprint once D-001 is resolved.

#### CG-ACT-DISTINIT-006 Initiate distribution only after acknowledgement (System)

- Epic: CG-ACT
- Phase: Phase 1 (core behavior)
- Source story: US-010
- Requirement refs: FR-029
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, activation

As a Charity user
I want distribution and deal activities to start only after acknowledgement
So that nothing is deployed prematurely

Scope

- In scope: initiating game-distribution, installation, deal, and activation activities only after acknowledgement; not sending deals or assigning State Gaming Stamps beforehand.
- Out of scope: per-game activation sequencing (CG-ACT-PERGAME-007).

Acceptance Criteria

- Given a draft or validated-but-unacknowledged selection, When inspected, Then no distribution, installation, deal, or gaming-stamp activity has started.
- Given an acknowledgement, When it completes, Then distribution, installation, and deal activities are initiated.
- Given the pre-acknowledgement state, When checked, Then no State Gaming Stamp has been assigned.

Definition of Ready

- No TBD text; exactly one primary-axis variant (post-acknowledgement trigger); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-ACT-LOCK-004); implementable and testable within a single two-week sprint.

#### CG-ACT-PERGAME-007 Activate each game independently on deal commingle (System)

- Epic: CG-ACT
- Phase: Phase 2 (UI/operational)
- Source story: US-010
- Requirement refs: FR-029.1, NFR-010
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, activation

As a Charity user
I want each game to activate independently once ready
So that a delayed game does not block the rest

Scope

- In scope: allowing each selected game to become active once its configuration is ready and the site controller has received and commingled the required two identical deals; ensuring a delayed or failed game does not block other ready games.
- Out of scope: distribution initiation (CG-ACT-DISTINIT-006).

Acceptance Criteria

- Given a game whose configuration is ready and whose two identical deals are received and commingled, When processed, Then the game becomes active independently.
- Given one game delayed or failed, When other games are ready, Then the ready games activate without being blocked.
- Given a failed game, When reviewed, Then its state is distinguishable from active games.

Definition of Ready

- No TBD text; exactly one primary-axis variant (per-game activation); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-ACT-DISTINIT-006, CG-SW-AUTOJOB-004); implementable and testable within a single two-week sprint.

#### CG-ACT-JOBAUDIT-008 Retain internal job records without exposing them (System)

- Epic: CG-ACT
- Phase: Phase 2 (UI/operational)
- Source story: US-010
- Requirement refs: FR-030
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, activation

As a support stakeholder
I want internal job records retained for support and audit
So that failures and recovery are traceable without exposing technical identifiers to charities

Scope

- In scope: retaining internal job identifiers, timestamps, per-game results, failures, retries, and recovery actions for support and audit.
- Out of scope: Charity-facing outcome display (CG-ACT-OUTCOME-009).

Acceptance Criteria

- Given an activation run, When it completes, Then internal job identifiers, timestamps, per-game results, failures, retries, and recovery actions are retained.
- Given a Charity user, When they view activation information, Then no internal technical identifiers are exposed.
- Given a support user, When they review a run, Then the retained records are available.

Definition of Ready

- No TBD text; exactly one primary-axis variant (internal job retention); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-ACT-PERGAME-007); implementable and testable within a single two-week sprint.

#### CG-ACT-OUTCOME-009 Show activation outcomes and recovery guidance (Charity user)

- Epic: CG-ACT
- Phase: Phase 2 (UI/operational)
- Source story: US-010
- Requirement refs: FR-031
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, activation

As a Charity user
I want activation outcomes and role-appropriate recovery guidance shown in EVO
So that I understand results without expecting external notifications

Scope

- In scope: showing activation outcomes and role-appropriate recovery guidance within EVO.
- Out of scope: any new Phase 1 email or external-notification capability (explicitly excluded).

Acceptance Criteria

- Given a completed activation, When the user views results, Then outcomes are shown in EVO.
- Given a failure, When shown, Then role-appropriate recovery guidance accompanies it.
- Given the outcome view, When rendered, Then no email or external-notification capability is implied or offered.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-ACT-JOBAUDIT-008); implementable and testable within a single two-week sprint.

#### CG-ACT-QUARTERSTAMP-010 Preserve regulatory quarter and activation timestamp (System)

- Epic: CG-ACT
- Phase: Phase 1 (core behavior)
- Source story: US-010
- Requirement refs: FR-038
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, activation

As a compliance stakeholder
I want both the regulatory quarter and the actual activation timestamp preserved
So that each activation event is accurately dated

Scope

- In scope: preserving both the regulatory quarter and the actual activation timestamp for each activation event.
- Out of scope: audit retention period rules (CG-OVS-RETENTION-004).

Acceptance Criteria

- Given an activation event, When recorded, Then the regulatory quarter is stored.
- Given the same event, When recorded, Then the actual activation timestamp is stored separately from the regulatory quarter.
- Given an activation occurring during a grace period, When recorded, Then the regulatory quarter and actual timestamp can differ and both are retained.

Definition of Ready

- No TBD text; exactly one primary-axis variant (activation event record); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-ACT-DISTINIT-006); implementable and testable within a single two-week sprint.

### CG-INQ — In-Quarter Changes and Equipment

#### CG-INQ-ADD-001 Add titles and Cost Per Play in-quarter within cumulative limits (Charity user)

- Epic: CG-INQ
- Phase: Phase 3 (access control, audit, hardening)
- Source story: US-015
- Requirement refs: FR-031.1 (add), FR-009, FR-004
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, in-quarter

As a Charity user
I want to add game titles and Cost Per Play configurations during the active quarter within limits
So that I can expand an active selection when authorized

Scope

- In scope: authorized in-quarter additions of game titles and Cost Per Play configurations within the applicable twelve-title and five Cost Per Play limits, using the visual catalog for authorized in-quarter additions; recording additions in the standard audit trail.
- Out of scope: closing a game (CG-INQ-CLOSE-002).

Acceptance Criteria

- Given an active quarter with fewer than twelve titles, When the user adds an eligible title in-quarter, Then it is accepted and counts toward the cumulative twelve-title limit.
- Given a selected title with fewer than five Cost Per Play values, When the user adds one in-quarter, Then it is accepted within the five-value limit.
- Given the cumulative twelve-title limit is reached, When the user attempts another in-quarter title, Then it is blocked.
- Given any in-quarter addition, When it completes, Then it is recorded in the standard audit trail.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user, in-quarter add); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-ACT-LOCK-004, CG-SEL-LIMITS-002); implementable and testable within a single two-week sprint.

#### CG-INQ-CLOSE-002 Close an active game or Cost Per Play after Attorney General approval (Charity user)

- Epic: CG-INQ
- Phase: Phase 3 (access control, audit, hardening)
- Source story: US-016
- Requirement refs: FR-031.1 (close)
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, in-quarter

As a Charity user
I want to close an active game or Cost Per Play after Attorney General approval
So that I can retire it without disturbing other active games

Scope

- In scope: closing an active game or Cost Per Play after Attorney General approval; ending the associated active deals; recording the closure in the standard audit trail.
- Out of scope: adding titles (CG-INQ-ADD-001).

Acceptance Criteria

- Given Attorney General approval, When the user closes an active game or Cost Per Play, Then its associated active deals are ended.
- Given a closure, When it completes, Then other active games and Cost Per Play configurations are unaffected and the acknowledged configuration is not reopened or modified.
- Given a closure, When attempted again or reversed in the same quarter, Then it cannot be reversed until the next quarter, and the closure is recorded in the standard audit trail.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity user, close); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated (AG approval); dependencies identified (CG-ACT-LOCK-004); implementable and testable within a single two-week sprint.

#### CG-INQ-EQUIP-003 Add or replace EGM or logic box with separate meter history (IGT Support)

- Epic: CG-INQ
- Phase: Phase 3 (access control, audit, hardening)
- Source story: US-017
- Requirement refs: FR-047.2, NFR-011
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, equipment

As an IGT Support user
I want to add or replace EGMs or logic boxes during an active quarter
So that equipment changes keep accurate, separate meter history

Scope

- In scope: supporting active-quarter EGM or logic-box additions and replacements; maintaining separate meter history for added or replaced units.
- Out of scope: initial Game Switch distribution (CG-SW-INSTALL-001).

Acceptance Criteria

- Given an active quarter, When an EGM or logic box is added, Then it is supported and begins its own meter history.
- Given a replacement unit, When it is installed, Then its meter history is maintained separately from the replaced unit.
- Given a replaced unit, When reviewed, Then its historical meter data is preserved and not commingled with the replacement.

Definition of Ready

- No TBD text; exactly one primary-axis variant (IGT Support, equipment change); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (none); implementable and testable within a single two-week sprint.

### CG-OVS — Oversight, Support and Audit

#### CG-OVS-REGVIEW-001 Regulator read-only configuration and audit history (Regulator)

- Epic: CG-OVS
- Phase: Phase 3 (access control, audit, hardening)
- Source story: US-011
- Requirement refs: FR-032, NFR-008
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, regulator

As a Regulator user
I want read-only access to configuration and audit history within my oversight scope
So that I can review without changing anything

Scope

- In scope: providing Regulator users read-only access to the relevant configuration and audit history for charities and sites within their authorized statewide oversight scope.
- Out of scope: Regulator context selection (CG-CTX-REGULATOR-002).

Acceptance Criteria

- Given a Regulator user, When they open a Charity and site in scope, Then configuration and audit history are viewable read-only.
- Given a Charity or site outside their scope, When accessed, Then it is not available.
- Given a Regulator viewing history, When present, Then no edit, acknowledge, or activate controls are offered.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Regulator); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-CTX-REGULATOR-002, CG-OVS-AUDITLOG-003); implementable and testable within a single two-week sprint.

#### CG-OVS-DELEGATE-002 Audited delegated Charity workflows for support (IGT Support / Distributor)

- Epic: CG-OVS
- Phase: Phase 3 (access control, audit, hardening)
- Source story: US-011
- Requirement refs: FR-033
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, delegation, audit

As a compliance stakeholder
I want delegated Charity workflows by support users fully audited
So that assisted actions remain accountable

Scope

- In scope: permitting fully audited delegated Charity workflows for IGT Support and Distributor users when authorized; capturing the actual user plus the Charity context for each delegated action.
- Out of scope: the delegated access entry (CG-CTX-SUPPORT-003).

Acceptance Criteria

- Given an authorized support or distributor user acting for a Charity, When they perform a workflow action, Then it is permitted and fully audited.
- Given a delegated action, When audited, Then both the actual acting user and the Charity context are recorded.
- Given an unauthorized delegation attempt, When made, Then it is denied and the attempt is not performed.

Definition of Ready

- No TBD text; exactly one primary-axis variant (support/distributor delegation); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-CTX-SUPPORT-003, CG-OVS-AUDITLOG-003); implementable and testable within a single two-week sprint.

#### CG-OVS-AUDITLOG-003 Immutable audit entries for all users (System)

- Epic: CG-OVS
- Phase: Phase 2 (UI/operational)
- Source story: US-012
- Requirement refs: FR-034, NFR-002
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, audit

As a compliance stakeholder
I want immutable audit entries for all material actions by all users
So that the record is tamper-evident

Scope

- In scope: recording immutable, tamper-evident audit entries for all users covering saves, edits, copies, acknowledgements, activation actions, download or configuration events, exceptions, recovery events, and delegated or elevated interventions, with the required attributes.
- Out of scope: retrieval and retention period (CG-OVS-RETENTION-004).

Acceptance Criteria

- Given any material action listed, When it occurs, Then an audit entry is recorded with the required attributes (actor, role, Charity, site, quarter, timestamp, result).
- Given a recorded audit entry, When modification is attempted, Then it cannot be altered and tampering is evident.
- Given a delegated or elevated intervention, When performed, Then it is captured as an audit entry.

Definition of Ready

- No TBD text; exactly one primary-axis variant (audit write); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (none); implementable and testable within a single two-week sprint.

#### CG-OVS-RETENTION-004 Retrieve and retain audit records (System)

- Epic: CG-OVS
- Phase: Phase 3 (access control, audit, hardening)
- Source story: US-012
- Requirement refs: FR-035, NFR-003
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, audit, retention

As a compliance stakeholder
I want audit, selection, acknowledgement, and activation records retrievable for the retention period
So that records are available when needed

Scope

- In scope: making audit records retrievable through authorized UI or API workflows; retaining selection, acknowledgement, activation, and audit records for at least three years in a recoverable form.
- Out of scope: the audit write path (CG-OVS-AUDITLOG-003).

Acceptance Criteria

- Given an authorized user, When they query audit records via UI or API, Then matching records are returned.
- Given a record within the retention period, When requested, Then it is retrievable in a recoverable form.
- Given the three-year retention requirement, When verified, Then selection, acknowledgement, activation, and audit records are retained for at least three years.

Definition of Ready

- No TBD text; exactly one primary-axis variant (retrieval and retention); one actor and one primary flow; test data and expected result explicit (three years); regulatory/role constraints stated; dependencies identified (CG-OVS-AUDITLOG-003); implementable and testable within a single two-week sprint.

### CG-META — Game Metadata and Management

#### CG-META-EXPOSE-001 Expose charitable-gaming game details in product views (System)

- Epic: CG-META
- Phase: Phase 2 (UI/operational)
- Source story: US-013
- Requirement refs: FR-036
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, metadata

As an authorized user
I want charitable-gaming game details exposed in the relevant product views
So that Cost Per Play, payout, and top prize are visible where needed

Scope

- In scope: exposing charitable-gaming game details, including Cost Per Play, payout percentage, and top prize, in the relevant product views.
- Out of scope: propagation to Game Switching (CG-META-PROPAGATE-002).

Acceptance Criteria

- Given a charitable-gaming game, When its product view renders, Then Cost Per Play, payout percentage, and top prize are shown.
- Given multiple approved Cost Per Play values, When displayed, Then each is shown with its payout and top prize.
- Given a non-charitable-gaming game, When viewed, Then charitable-gaming-specific fields are not forced into the view.

Definition of Ready

- No TBD text; exactly one primary-axis variant (metadata exposure); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-META-EDITOR-005); implementable and testable within a single two-week sprint.

#### CG-META-PROPAGATE-002 Propagate authoritative configuration to Switching and Selection (System)

- Epic: CG-META
- Phase: Phase 1 (core behavior)
- Source story: US-013, US-014
- Requirement refs: FR-037, NFR-006
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, metadata

As an authorized user
I want authoritative game configuration propagated to Game Switching and Game Selection
So that the same approved options are available everywhere

Scope

- In scope: propagating authoritative charitable-gaming game configuration values to the Game Switching and Game Selection experience so the same approved options are available for selection and installation planning.
- Out of scope: product-view exposure (CG-META-EXPOSE-001).

Acceptance Criteria

- Given approved game configuration values, When updated in the authoritative source, Then the same values appear in Game Selection and Game Switching.
- Given a selectable option in Game Selection, When compared to Game Switching, Then the approved options match.
- Given a value not approved in the authoritative source, When checked, Then it is not offered in either experience.

Definition of Ready

- No TBD text; exactly one primary-axis variant (configuration propagation); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-META-EDITOR-005); implementable and testable within a single two-week sprint.

#### CG-META-UPLOAD-003 Upload game packages via EVO Game Package Administration (IGT Support)

- Epic: CG-META
- Phase: Phase 1 (core behavior)
- Source story: US-013
- Requirement refs: FR-039
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, metadata

As an authorized IGT Support user
I want to upload game packages and metadata using the existing EVO workflow
So that games are available for management and selection

Scope

- In scope: uploading game packages and metadata using the existing EVO Game Package Administration workflow.
- Out of scope: game add/edit/delete (CG-META-MANAGE-004).

Acceptance Criteria

- Given an authorized IGT Support user, When they upload a game package and metadata, Then it is accepted via the existing Game Package Administration workflow.
- Given a user without the permission, When they attempt an upload, Then it is denied.
- Given an uploaded package, When reviewed, Then its metadata is available for game management.

Definition of Ready

- No TBD text; exactly one primary-axis variant (IGT Support upload); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-USR-OPSROLE-003); implementable and testable within a single two-week sprint.

#### CG-META-MANAGE-004 Add, edit, or delete games via EVO Game Management (IGT Support)

- Epic: CG-META
- Phase: Phase 1 (core behavior)
- Source story: US-013
- Requirement refs: FR-040
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, metadata

As an authorized IGT Support user
I want to add, edit, or delete games using the existing EVO Game Management workflow
So that the game catalog stays accurate

Scope

- In scope: adding, editing, or deleting games using the existing EVO Game Management workflow.
- Out of scope: Cost Per Play and payout configuration (CG-META-EDITOR-005).

Acceptance Criteria

- Given an authorized IGT Support user, When they add a game, Then it is created via the existing Game Management workflow.
- Given an existing game, When edited or deleted by an authorized user, Then the change is applied.
- Given a user without permission, When they attempt add, edit, or delete, Then it is denied.

Definition of Ready

- No TBD text; exactly one primary-axis variant (IGT Support game management); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-META-UPLOAD-003); implementable and testable within a single two-week sprint.

#### CG-META-EDITOR-005 Configure Cost Per Play, payout and top prize in the Game Editor (IGT Support)

- Epic: CG-META
- Phase: Phase 1 (core behavior)
- Source story: US-013
- Requirement refs: FR-041
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, metadata

As an authorized IGT Support user
I want the Game Editor to configure Cost Per Play, payout, and top prize
So that charitable-gaming games carry the correct approved values

Scope

- In scope: configuring Cost Per Play, payout percentage, and top-prize values for charitable-gaming-related games in the Game Editor.
- Out of scope: Game Switch editor configuration (CG-SW-CONFIG-003).

Acceptance Criteria

- Given a charitable-gaming game in the Game Editor, When an authorized user sets Cost Per Play, payout percentage, and top prize, Then the values are saved.
- Given saved values, When the game is used downstream, Then those approved values are available.
- Given a user without permission, When they attempt to configure these values, Then it is denied.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Game Editor configuration); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-META-MANAGE-004); implementable and testable within a single two-week sprint.

### CG-SW — Game Switching and Distribution

#### CG-SW-INSTALL-001 Download and install games onto EGMs (IGT Support)

- Epic: CG-SW
- Phase: Phase 2 (UI/operational)
- Source story: US-014
- Requirement refs: FR-042
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, game-switching

As an authorized IGT Support user
I want to download and install games onto EGMs at licensed sites
So that selected games reach the floor

Scope

- In scope: downloading and installing games onto EGMs at licensed sites using the existing EVO Game Switching workflow.
- Out of scope: uninstalling games (CG-SW-UNINSTALL-002).

Acceptance Criteria

- Given an authorized IGT Support user, When they install a game onto an EGM at a licensed site, Then it is downloaded and installed via Game Switching.
- Given a user without permission, When they attempt installation, Then it is denied.
- Given an installation, When it completes, Then the game is present on the target EGM.

Definition of Ready

- No TBD text; exactly one primary-axis variant (IGT Support install); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-USR-OPSROLE-003); implementable and testable within a single two-week sprint.

#### CG-SW-UNINSTALL-002 Uninstall games from EGMs (IGT Support)

- Epic: CG-SW
- Phase: Phase 2 (UI/operational)
- Source story: US-014
- Requirement refs: FR-043
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, game-switching

As an authorized IGT Support user
I want to uninstall games from EGMs at licensed sites
So that retired games are removed from the floor

Scope

- In scope: uninstalling games from EGMs at licensed sites using the existing EVO Game Switching workflow.
- Out of scope: installing games (CG-SW-INSTALL-001).

Acceptance Criteria

- Given an authorized IGT Support user, When they uninstall a game from an EGM, Then it is removed via Game Switching.
- Given a user without permission, When they attempt uninstallation, Then it is denied.
- Given an uninstall, When it completes, Then the game is no longer present on the target EGM.

Definition of Ready

- No TBD text; exactly one primary-axis variant (IGT Support uninstall); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-SW-INSTALL-001); implementable and testable within a single two-week sprint.

#### CG-SW-CONFIG-003 Configure Cost Per Play, payout and top prize on installed games (IGT Support)

- Epic: CG-SW
- Phase: Phase 2 (UI/operational)
- Source story: US-014
- Requirement refs: FR-044, FR-045
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, game-switching

As an authorized IGT Support user
I want to configure Cost Per Play, payout, and top prize on installed games via the Game Switch editor
So that EGMs run the approved configuration

Scope

- In scope: configuring Cost Per Play, payout percentage, and top-prize values for charitable-gaming games distributed to and installed on EGMs using the existing EVO Game Switching workflow and Game Switch editor.
- Out of scope: automatic job creation from acknowledgement (CG-SW-AUTOJOB-004).

Acceptance Criteria

- Given an installed charitable-gaming game, When an authorized user sets Cost Per Play, payout, and top prize in the Game Switch editor, Then the values are applied.
- Given applied values, When the EGM runs, Then it reflects the configured values.
- Given a user without permission, When they attempt configuration, Then it is denied.

Definition of Ready

- No TBD text; exactly one primary-axis variant (IGT Support switch configuration); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-SW-INSTALL-001); implementable and testable within a single two-week sprint.

#### CG-SW-AUTOJOB-004 Auto-create Game Switch jobs from the acknowledged configuration (System)

- Epic: CG-SW
- Phase: Phase 2 (UI/operational)
- Source story: US-014
- Requirement refs: FR-046, FR-047
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, game-switching

As a Charity user
I want Game Switch jobs created automatically from the acknowledged configuration
So that activation does not require terminal-by-terminal assignment

Scope

- In scope: automatically creating and executing Game Switch jobs to activate the acknowledged configuration on EGMs, consuming the authoritative approved configuration; applying one validated IGT game configuration to the applicable EGMs at the selected site.
- Out of scope: manual Game Switch editing (CG-SW-CONFIG-003).

Acceptance Criteria

- Given an acknowledged configuration, When activation proceeds, Then Game Switch jobs are created and executed automatically from the authoritative approved configuration.
- Given applicable IGT EGMs at the selected site, When jobs run, Then one validated configuration is applied across them without terminal-by-terminal Charity assignment.
- Given a non-applicable terminal, When jobs run, Then it is excluded from the configuration.

Definition of Ready

- No TBD text; exactly one primary-axis variant (auto job creation); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-ACT-LOCK-004); implementable and testable within a single two-week sprint.

#### CG-SW-HISTORY-005 Preserve Game Switching history and observability (System)

- Epic: CG-SW
- Phase: Phase 2 (UI/operational)
- Source story: US-014
- Requirement refs: FR-047.1, NFR-009
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, game-switching

As a support stakeholder
I want Game Switching history and processing evidence preserved
So that activity is observable and auditable

Scope

- In scope: preserving historical records and retaining standard EVO processing evidence and observability for Game Switching activity, including per-game job status, failures, retries, and recovery actions.
- Out of scope: Charity-facing outcome display (CG-ACT-OUTCOME-009).

Acceptance Criteria

- Given Game Switching activity, When it runs, Then historical records and standard processing evidence are retained.
- Given a job, When reviewed, Then its status, failures, retries, and recovery actions are observable.
- Given completed activity, When queried later, Then the historical record remains available.

Definition of Ready

- No TBD text; exactly one primary-axis variant (switching history/observability); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-SW-AUTOJOB-004); implementable and testable within a single two-week sprint.

### CG-USR — User and Role Management

#### CG-USR-MANAGE-001 Create, edit and delete users and assign roles (IGT Support / Distributor)

- Epic: CG-USR
- Phase: Phase 1 (core behavior)
- Source story: US-011
- Requirement refs: FR-048
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, user-management

As an authorized IGT Support or Distributor user
I want to create, edit, and delete users and assign roles using the existing EVO workflow
So that access is managed while preserving audit identity

Scope

- In scope: creating, editing, and deleting users and assigning roles and permissions using the existing EVO User Management workflow while preserving user audit identity.
- Out of scope: the specific role definitions (CG-USR-EGMROLE-002 through CG-USR-COMPLIANCEROLE-005).

Acceptance Criteria

- Given an authorized user, When they create, edit, or delete a user and assign roles, Then the change is applied via the existing User Management workflow.
- Given a user is edited or deleted, When audit is reviewed, Then the user's audit identity is preserved.
- Given an unauthorized user, When they attempt user management, Then it is denied.

Definition of Ready

- No TBD text; exactly one primary-axis variant (IGT Support/Distributor user admin); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (none); implementable and testable within a single two-week sprint.

#### CG-USR-EGMROLE-002 IGT Support or Distributor EGM performance role (System)

- Epic: CG-USR
- Phase: Phase 1 (core behavior)
- Source story: US-011
- Requirement refs: FR-049
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, user-management

As an administrator
I want an IGT Support or Distributor role granting EGM performance management
So that performance can be managed via existing EVO functionality

Scope

- In scope: supporting an IGT Support or Distributor role that provides permissions to manage EGM performance using the existing EVO EGM Management functionality.
- Out of scope: the operations role (CG-USR-OPSROLE-003).

Acceptance Criteria

- Given the EGM performance role, When assigned, Then the user can manage EGM performance via EVO EGM Management.
- Given a user without the role, When they attempt EGM performance management, Then it is denied.
- Given the role, When reviewed, Then its permissions are scoped to EGM performance management.

Definition of Ready

- No TBD text; exactly one primary-axis variant (EGM performance role); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-USR-MANAGE-001); implementable and testable within a single two-week sprint.

#### CG-USR-OPSROLE-003 IGT Support or Operations role for packages, games and switching (System)

- Epic: CG-USR
- Phase: Phase 1 (core behavior)
- Source story: US-013
- Requirement refs: FR-050
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, user-management

As an administrator
I want an IGT Support or Operations role for package, game, software, and switching management
So that operational tasks are permitted via existing EVO workflows

Scope

- In scope: supporting an IGT Support or IGT Operations role that provides permissions to upload packages, manage games, manage EGM software, and manage Game Switching jobs using the existing EVO Game Package Administration, Game Management, and Game Switching workflows.
- Out of scope: the Charity User role (CG-USR-CHARITYROLE-004).

Acceptance Criteria

- Given the operations role, When assigned, Then the user can upload packages, manage games, manage EGM software, and manage Game Switching jobs.
- Given a user without the role, When they attempt these tasks, Then they are denied.
- Given the role, When reviewed, Then its permissions cover the four listed areas.

Definition of Ready

- No TBD text; exactly one primary-axis variant (operations role); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-USR-MANAGE-001); implementable and testable within a single two-week sprint.

#### CG-USR-CHARITYROLE-004 Charity User role for Game Selection and Quarterly Workflow (System)

- Epic: CG-USR
- Phase: Phase 1 (core behavior)
- Source story: US-001
- Requirement refs: FR-051
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, user-management

As an administrator
I want a Charity User role permitting the Game Selection and Quarterly Workflow
So that charity users can execute their workflow

Scope

- In scope: supporting a Charity User role that provides permission to execute the Game Selection workflow and the Quarterly Workflow.
- Out of scope: context establishment behavior (CG-CTX-CHARITY-001).

Acceptance Criteria

- Given the Charity User role, When assigned, Then the user can execute the Game Selection and Quarterly Workflow.
- Given a user without the role, When they attempt these workflows, Then they are denied.
- Given the role, When reviewed, Then its permissions are limited to Game Selection and the Quarterly Workflow.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Charity User role); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-USR-MANAGE-001); implementable and testable within a single two-week sprint.

#### CG-USR-COMPLIANCEROLE-005 Single Compliance and Regulator read-only role (System)

- Epic: CG-USR
- Phase: Phase 1 (core behavior)
- Source story: US-011, US-012
- Requirement refs: FR-052
- UX refs: N/A (A-08 excluded)
- Labels: charitable-gaming, game-selection, user-management

As an administrator
I want a single Compliance and Regulator read-only role
So that oversight users can review without making changes

Scope

- In scope: supporting a single Compliance and Regulator role that allows read-only review of approved game selection history, validation status, activation history, and audit records with no operational changes permitted.
- Out of scope: Regulator oversight views (CG-OVS-REGVIEW-001).

Acceptance Criteria

- Given the Compliance and Regulator role, When assigned, Then the user can review selection history, validation status, activation history, and audit records read-only.
- Given the role, When any operational action is attempted, Then it is not permitted.
- Given the role, When reviewed, Then no edit, acknowledge, or activate permission is present.

Definition of Ready

- No TBD text; exactly one primary-axis variant (Compliance/Regulator role); one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated; dependencies identified (CG-USR-MANAGE-001); implementable and testable within a single two-week sprint.

## Suggested Starting Point

A coherent starter set that delivers a visible browse-to-save slice for the Charity role, single site context, minimal branching:

- CG-USR-CHARITYROLE-004 (Charity User role)
- CG-CTX-CHARITY-001 (single-site context)
- CG-META-PROPAGATE-002 (authoritative config feeding the catalog)
- CG-CAT-DISPLAY-001 and CG-CAT-ENTRY-002 (catalog browse)
- CG-CAT-CPP-003 (configure a title)
- CG-SEL-LIMITS-002 (enforce the twelve-title and five Cost Per Play limits)
- CG-DRFT-ADDREMOVE-001 and CG-DRFT-SAVE-004 (build and save a draft)

This is a non-binding recommendation to establish an end-to-end spine. Actual sprint scope is decided by the project manager after estimation.

## Jira Import CSV

```csv
Issue Type,Summary,Description,Story Points,Epic Link,Labels,Priority,Assignee,Acceptance Criteria,Fix Version,Affects Version
Epic,CG-CTX Access Charity and Site Context,Establish authorized Charity and site context and role-appropriate access for Game Selection.,,,charitable-gaming game-selection,Medium,,,,
Epic,CG-CAT Visual Game Catalog,Visual catalog of eligible games with brochure-style detail and configuration selection.,,,charitable-gaming game-selection,Medium,,,,
Epic,CG-SEL Valid Selection Rules,Enforce valid predefined combinations and selection limits.,,,charitable-gaming game-selection,Medium,,,,
Epic,CG-DRFT Draft Management,Add remove copy save and resume next-quarter drafts without affecting the active quarter.,,,charitable-gaming game-selection,Medium,,,,
Epic,CG-DASH Task-Oriented Dashboard,Status blockers next action lists filters and per-game processing status.,,,charitable-gaming game-selection,Medium,,,,
Epic,CG-ACT Validation Acknowledgement and Activation,Revalidate acknowledge lock and activate selections with per-game activation.,,,charitable-gaming game-selection,Medium,,,,
Epic,CG-INQ In-Quarter Changes and Equipment,Authorized in-quarter additions closures and EGM or logic-box changes.,,,charitable-gaming game-selection,Medium,,,,
Epic,CG-OVS Oversight Support and Audit,Regulator read-only access delegated workflows and immutable auditing.,,,charitable-gaming game-selection,Medium,,,,
Epic,CG-META Game Metadata and Management,Upload manage and configure charitable-gaming game metadata.,,,charitable-gaming game-selection,Medium,,,,
Epic,CG-SW Game Switching and Distribution,Install uninstall configure and auto-activate games on EGMs.,,,charitable-gaming game-selection,Medium,,,,
Epic,CG-USR User and Role Management,Manage users and define Game Selection roles.,,,charitable-gaming game-selection,Medium,,,,
Story,CG-CTX-CHARITY-001 Establish Charity and site context,Establish and confirm authorized Charity and licensed site before Game Selection. FR-001 FR-002 FR-051.,,CG-CTX Access Charity and Site Context,charitable-gaming game-selection charity-user,Medium,,Single Charity and site auto-established; multi-association requires explicit selection; only authorized context data available; unauthorized charities and sites not presented.,,
Story,CG-CTX-REGULATOR-002 Regulator statewide read-only scope,Regulator selects any Charity and site in scope read-only. FR-003 FR-052.,,CG-CTX Access Charity and Site Context,charitable-gaming game-selection regulator,Medium,,Any in-scope Charity and site selectable; all editing controls absent or disabled; write attempts rejected.,,
Story,CG-CTX-SUPPORT-003 Delegated access for IGT Support and Distributor,Support and Distributor perform Charity workflows when permission allows. FR-003.1.,,CG-CTX Access Charity and Site Context,charitable-gaming game-selection igt-support distributor,Medium,,Authorized Charity workflow available; non-permitted charities unavailable; actual identity preserved for audit.,,
Story,CG-CTX-RETAILER-004 Retailer restricted experience hides Game Selection,Standard restricted Retailer experience hides Game Selection and audit. FR-003.2.,,CG-CTX Access Charity and Site Context,charitable-gaming game-selection retailer,Medium,,No Game Selection module visible; direct URL access denied; only standard restricted experience shown.,,
Story,CG-CTX-ALLROLES-005 Hide unauthorized modules and actions,Hide unauthorized modules and actions rather than failing after use. FR-003.3.,,CG-CTX Access Charity and Site Context,charitable-gaming game-selection access-control,Medium,,Unauthorized modules not displayed; unauthorized actions hidden; direct invocation rejected server-side.,,
Story,CG-CAT-DISPLAY-001 Display the visual catalog,Visual catalog by statewide approval and site cabinet compatibility. FR-004 FR-005 FR-005.1.,,CG-CAT Visual Game Catalog,charitable-gaming game-selection catalog,Medium,,Approved compatible game appears regardless of install state; incompatible game not offered; non-approved game absent; usable on tablet.,,
Story,CG-CAT-ENTRY-002 Show brochure-style catalog entry detail,Catalog entry shows approved details and indicators. FR-006.,,CG-CAT Visual Game Catalog,charitable-gaming game-selection catalog,Medium,,Graphic title cabinets CPP payout top-prize approval date shown; brochure detail and new/returning indicator; share of play only when data exists.,,
Story,CG-CAT-CPP-003 Select Cost Per Play payout and top prize,Select CPP values and one payout and top-prize each. FR-006.1 FR-006.2 FR-010.,,CG-CAT Visual Game Catalog,charitable-gaming game-selection catalog,Medium,,Select all or one CPP; one payout and top prize per CPP from approved set; payout and top prize non-editable.,,
Story,CG-CAT-HIDE-004 Hide technical identifiers from Charity users,Suppress technical identifiers from Charity views. FR-007.,,CG-CAT Visual Game Catalog,charitable-gaming game-selection catalog,Medium,,No package math paytable routing deal-inventory or active-pool identifiers shown; identifiers absent from Charity payloads.,,
Story,CG-CAT-FILTER-005 Filter catalog by all new or returning games,Filter catalog by all new or previously returning games. FR-058.,,CG-CAT Visual Game Catalog,charitable-gaming game-selection catalog,Medium,,New filter lists only new games; returning filter lists only returning games; all restores unfiltered catalog.,,
Story,CG-SEL-VALIDCOMBO-001 Present only valid predefined combinations,Offer only valid predefined game and configuration combinations. FR-008 FR-010.,,CG-SEL Valid Selection Rules,charitable-gaming game-selection rules,Medium,,Only valid combinations selectable; only approved variants offered non-editable; invalid combinations not presented.,,
Story,CG-SEL-LIMITS-002 Enforce title and Cost Per Play limits with live count,Enforce twelve titles and five CPP with visible count. FR-009.,,CG-SEL Valid Selection Rules,charitable-gaming game-selection rules,Medium,,Twelfth title accepted shows 12 of 12; thirteenth blocked; sixth CPP blocked with title count unchanged; CPP additions do not raise title count.,,
Story,CG-SEL-VALIDATION-003 Block unsupported combinations with contextual validation,Prevent unsupported combinations allow incomplete draft block finalize. FR-011.,,CG-SEL Valid Selection Rules,charitable-gaming game-selection rules,Medium,,Unsupported selection prevented with context; incomplete draft saves; critical errors block acknowledgement and activation.,,
Story,CG-DRFT-ADDREMOVE-001 Add and remove titles in the next-quarter draft,Add and remove eligible titles without affecting active quarter. FR-012 FR-013 FR-014.,,CG-DRFT Draft Management,charitable-gaming game-selection draft,Medium,,Eligible title added under twelve; removal clears its CPP and config; active quarter unchanged; eligibility from approved catalog and cabinet compatibility.,,
Story,CG-DRFT-COPY-002 Start a draft from a baseline or copy between sites,Start from prior config or copy from another authorized site choices only. FR-015 FR-016.,,CG-DRFT Draft Management,charitable-gaming game-selection draft,Medium,,Baseline pre-populates choices; cross-site copy copies choices; identifiers deployment deal pool and stamps not copied.,,
Story,CG-DRFT-MULTISITE-003 Validate cabinet compatibility per target site on copy,Validate each target site cabinet independently on multi-site apply. FR-015.1.,,CG-DRFT Draft Management,charitable-gaming game-selection draft,Medium,,Each site validated against its cabinet; incompatible title flagged or excluded per site; all-compatible accepted everywhere.,,
Story,CG-DRFT-SAVE-004 Save and resume one editable draft per site and quarter,One editable draft per site and quarter no side effects. FR-017.,,CG-DRFT Draft Management,charitable-gaming game-selection draft,Medium,,Exactly one draft per site and quarter; resumes with prior choices; no downloads deals stamps or EGM config and active quarter unchanged.,,
Story,CG-DRFT-AUDIT-005 Audit material draft actions without mandatory reason,Record material draft actions without mandatory change reason. FR-017.1.,,CG-DRFT Draft Management,charitable-gaming game-selection audit,Medium,,Material action recorded with actor site quarter timestamp; routine save needs no reason; actions appear in order.,,
Story,CG-DASH-OVERVIEW-001 Show status and next required action,Dashboard shows site and quarter status blockers and next action. FR-021.,,CG-DASH Task-Oriented Dashboard,charitable-gaming game-selection dashboard,Medium,,Incomplete draft shows status and next action; blocker surfaced with next step; ready site indicates activation.,,
Story,CG-DASH-LIST-002 List current and past selections with identifiers,List current and past selections with site charity quarter default current. FR-053 FR-054 FR-055.,,CG-DASH Task-Oriented Dashboard,charitable-gaming game-selection dashboard,Medium,,Current and past selections listed; each shows Site Charity Quarter; current quarter shown by default.,,
Story,CG-DASH-FILTER-003 Filter the dashboard by site and charity,Filter dashboard by site charity or both. FR-056.,,CG-DASH Task-Oriented Dashboard,charitable-gaming game-selection dashboard,Medium,,Site filter shows one site; Charity filter shows one charity; combination filter matches both.,,
Story,CG-DASH-OPEN-004 Open a selection from the dashboard,Open selection to view or make authorized in-quarter changes. FR-057.,,CG-DASH Task-Oriented Dashboard,charitable-gaming game-selection dashboard,Medium,,Selection detail opens; authorized in-quarter actions available; read-only user sees no edit actions.,,
Story,CG-DASH-STATUSMODEL-005 Separate status types and hide Superseded,Separate workflow activation technical statuses hide Superseded. FR-022 FR-022.4 FR-022.5.,,CG-DASH Task-Oriented Dashboard,charitable-gaming game-selection dashboard,Medium,,Three status types distinct; selection status from defined set; activation status from defined set; no Superseded on dashboard.,,
Story,CG-DASH-DOWNLOAD-006 Show per-game download and installation status,Show download and installation status per selected game. FR-022.1.,,CG-DASH Task-Oriented Dashboard,charitable-gaming game-selection dashboard,Medium,,Download and install status shown per game; shown independently per game; failure reflected.,,
Story,CG-DASH-SWITCH-007 Show per-game Game Switch on and off status,Show Game Switch on or off status per selected game. FR-022.2.,,CG-DASH Task-Oriented Dashboard,charitable-gaming game-selection dashboard,Medium,,On shows on; off shows off; each game shows its own switch status.,,
Story,CG-DASH-MONITOR-008 Real-time monitoring via Game Switch or Job Monitor,Monitoring via Game Switch Monitor or Job Monitor real time. FR-022.3.,,CG-DASH Task-Oriented Dashboard,charitable-gaming game-selection monitoring,Medium,,Current status shown in real time; change reflected on normal refresh; unpermitted access denied.,,
Story,CG-ACT-REVALIDATE-001 Revalidate and show errors before acknowledgement,Revalidate complete selection show errors near game block acknowledgement. FR-024.,,CG-ACT Validation Acknowledgement and Activation,charitable-gaming game-selection validation,Medium,,Complete selection revalidated before activation; error shown near affected game; acknowledgement blocked with critical error.,,
Story,CG-ACT-MINCONFIG-002 Require at least one valid game and Cost Per Play,Require one valid game and CPP before activation. FR-026.,,CG-ACT Validation Acknowledgement and Activation,charitable-gaming game-selection activation,Medium,,Zero valid config blocks activation; at least one valid config satisfies requirement; invalidation re-blocks.,,
Story,CG-ACT-RECORD-003 Record acknowledgement attributes,Record actor role charity site quarter timestamp and config at acknowledgement. FR-027.,,CG-ACT Validation Acknowledgement and Activation,charitable-gaming game-selection activation,Medium,,Actor role charity site quarter timestamp recorded; actual acting user captured in delegation; complete config stored.,,
Story,CG-ACT-LOCK-004 Lock the configuration on acknowledgement,Lock acknowledged config for the quarter. FR-028.,,CG-ACT Validation Acknowledgement and Activation,charitable-gaming game-selection activation,Medium,,Acknowledged config edits prevented; active config locked for the quarter; unauthorized direct edit rejected.,,
Story,CG-ACT-ELIGIBILITY-005 Govern activation eligibility via Quarterly Workflow,Govern activation by quarter boundaries grace periods and closure. FR-025.,,CG-ACT Validation Acknowledgement and Activation,charitable-gaming game-selection activation,Medium,,Activation permitted within window; blocked outside window or closed with reason; start-and-close in same pre-quarter prevented.,,
Story,CG-ACT-DISTINIT-006 Initiate distribution only after acknowledgement,Start distribution deal and activation only after acknowledgement. FR-029.,,CG-ACT Validation Acknowledgement and Activation,charitable-gaming game-selection activation,Medium,,No distribution or stamp before acknowledgement; acknowledgement initiates distribution; no stamp assigned pre-acknowledgement.,,
Story,CG-ACT-PERGAME-007 Activate each game independently on deal commingle,Each game activates when ready and two deals commingled delayed non-blocking. FR-029.1.,,CG-ACT Validation Acknowledgement and Activation,charitable-gaming game-selection activation,Medium,,Ready game with commingled deals activates independently; delayed game does not block ready games; failed state distinguishable.,,
Story,CG-ACT-JOBAUDIT-008 Retain internal job records without exposing them,Retain internal job ids results retries recovery not exposed to charity. FR-030.,,CG-ACT Validation Acknowledgement and Activation,charitable-gaming game-selection activation,Medium,,Job ids timestamps results failures retries recovery retained; no technical ids exposed to charity; support can review.,,
Story,CG-ACT-OUTCOME-009 Show activation outcomes and recovery guidance,Show outcomes and role-appropriate recovery in EVO no external notification. FR-031.,,CG-ACT Validation Acknowledgement and Activation,charitable-gaming game-selection activation,Medium,,Outcomes shown in EVO; recovery guidance on failure; no email or external notification implied.,,
Story,CG-ACT-QUARTERSTAMP-010 Preserve regulatory quarter and activation timestamp,Preserve regulatory quarter and actual activation timestamp per event. FR-038.,,CG-ACT Validation Acknowledgement and Activation,charitable-gaming game-selection activation,Medium,,Regulatory quarter stored; actual timestamp stored separately; grace-period activation keeps both.,,
Story,CG-INQ-ADD-001 Add titles and Cost Per Play in-quarter within cumulative limits,Authorized in-quarter additions within twelve and five limits audited. FR-031.1 FR-009 FR-004.,,CG-INQ In-Quarter Changes and Equipment,charitable-gaming game-selection in-quarter,Medium,,In-quarter title added under cumulative twelve; CPP added under five; limit reached blocks further; additions audited.,,
Story,CG-INQ-CLOSE-002 Close an active game or Cost Per Play after Attorney General approval,Close active game or CPP after AG approval ends deals audited irreversible. FR-031.1.,,CG-INQ In-Quarter Changes and Equipment,charitable-gaming game-selection in-quarter,Medium,,Approved closure ends associated deals; others unaffected and config not reopened; cannot reverse until next quarter and recorded in audit.,,
Story,CG-INQ-EQUIP-003 Add or replace EGM or logic box with separate meter history,Active-quarter EGM or logic-box add replace with separate meter history. FR-047.2.,,CG-INQ In-Quarter Changes and Equipment,charitable-gaming game-selection equipment,Medium,,Added unit starts own meter history; replacement meter history separate; replaced unit history preserved.,,
Story,CG-OVS-REGVIEW-001 Regulator read-only configuration and audit history,Regulator read-only config and audit history in scope. FR-032.,,CG-OVS Oversight Support and Audit,charitable-gaming game-selection regulator,Medium,,In-scope config and audit viewable read-only; out-of-scope unavailable; no edit acknowledge or activate controls.,,
Story,CG-OVS-DELEGATE-002 Audited delegated Charity workflows for support,Fully audited delegated Charity workflows for support and distributor. FR-033.,,CG-OVS Oversight Support and Audit,charitable-gaming game-selection delegation audit,Medium,,Authorized delegated action permitted and audited; actual user and charity context recorded; unauthorized delegation denied.,,
Story,CG-OVS-AUDITLOG-003 Immutable audit entries for all users,Immutable tamper-evident audit for all material actions. FR-034.,,CG-OVS Oversight Support and Audit,charitable-gaming game-selection audit,Medium,,Material action records entry with required attributes; entry cannot be altered and tampering evident; delegated or elevated intervention captured.,,
Story,CG-OVS-RETENTION-004 Retrieve and retain audit records,Retrieve via UI or API and retain at least three years. FR-035 NFR-003.,,CG-OVS Oversight Support and Audit,charitable-gaming game-selection audit retention,Medium,,Authorized query returns records; record retrievable in recoverable form; three-year retention verified.,,
Story,CG-META-EXPOSE-001 Expose charitable-gaming game details in product views,Expose CPP payout and top prize in product views. FR-036.,,CG-META Game Metadata and Management,charitable-gaming game-selection metadata,Medium,,CPP payout top prize shown in product view; each CPP shows payout and top prize; non-charitable game not forced.,,
Story,CG-META-PROPAGATE-002 Propagate authoritative configuration to Switching and Selection,Propagate authoritative config to Switching and Selection. FR-037.,,CG-META Game Metadata and Management,charitable-gaming game-selection metadata,Medium,,Updated authoritative values appear in Selection and Switching; options match across both; non-approved value not offered.,,
Story,CG-META-UPLOAD-003 Upload game packages via EVO Game Package Administration,Upload packages and metadata via existing workflow. FR-039.,,CG-META Game Metadata and Management,charitable-gaming game-selection metadata,Medium,,Authorized upload accepted; unpermitted upload denied; uploaded metadata available for management.,,
Story,CG-META-MANAGE-004 Add edit or delete games via EVO Game Management,Add edit delete games via existing workflow. FR-040.,,CG-META Game Metadata and Management,charitable-gaming game-selection metadata,Medium,,Authorized add creates game; edit or delete applied; unpermitted change denied.,,
Story,CG-META-EDITOR-005 Configure Cost Per Play payout and top prize in the Game Editor,Game Editor configures CPP payout and top prize. FR-041.,,CG-META Game Metadata and Management,charitable-gaming game-selection metadata,Medium,,Authorized values saved; values available downstream; unpermitted configuration denied.,,
Story,CG-SW-INSTALL-001 Download and install games onto EGMs,Install games onto EGMs via Game Switching. FR-042.,,CG-SW Game Switching and Distribution,charitable-gaming game-selection game-switching,Medium,,Authorized install downloads and installs; unpermitted install denied; game present on target EGM.,,
Story,CG-SW-UNINSTALL-002 Uninstall games from EGMs,Uninstall games from EGMs via Game Switching. FR-043.,,CG-SW Game Switching and Distribution,charitable-gaming game-selection game-switching,Medium,,Authorized uninstall removes game; unpermitted uninstall denied; game absent from target EGM.,,
Story,CG-SW-CONFIG-003 Configure Cost Per Play payout and top prize on installed games,Configure CPP payout top prize on installed games via Game Switch editor. FR-044 FR-045.,,CG-SW Game Switching and Distribution,charitable-gaming game-selection game-switching,Medium,,Authorized values applied on installed game; EGM reflects configured values; unpermitted configuration denied.,,
Story,CG-SW-AUTOJOB-004 Auto-create Game Switch jobs from the acknowledged configuration,Auto-create and execute Game Switch jobs from acknowledged config site-wide. FR-046 FR-047.,,CG-SW Game Switching and Distribution,charitable-gaming game-selection game-switching,Medium,,Acknowledged config auto-creates and executes jobs; one validated config applied across applicable EGMs; non-applicable terminal excluded.,,
Story,CG-SW-HISTORY-005 Preserve Game Switching history and observability,Preserve switching history and processing evidence. FR-047.1.,,CG-SW Game Switching and Distribution,charitable-gaming game-selection game-switching,Medium,,History and evidence retained; job status failures retries recovery observable; historical record available later.,,
Story,CG-USR-MANAGE-001 Create edit and delete users and assign roles,Manage users and assign roles via existing workflow preserving audit identity. FR-048.,,CG-USR User and Role Management,charitable-gaming game-selection user-management,Medium,,Authorized user admin applied via existing workflow; audit identity preserved on edit or delete; unauthorized admin denied.,,
Story,CG-USR-EGMROLE-002 IGT Support or Distributor EGM performance role,Role granting EGM performance management via EVO. FR-049.,,CG-USR User and Role Management,charitable-gaming game-selection user-management,Medium,,Role enables EGM performance management; without role denied; permissions scoped to EGM performance.,,
Story,CG-USR-OPSROLE-003 IGT Support or Operations role for packages games and switching,Role for packages games software and switching jobs. FR-050.,,CG-USR User and Role Management,charitable-gaming game-selection user-management,Medium,,Role enables the four operational areas; without role denied; permissions cover the four areas.,,
Story,CG-USR-CHARITYROLE-004 Charity User role for Game Selection and Quarterly Workflow,Role permitting Game Selection and Quarterly Workflow. FR-051.,,CG-USR User and Role Management,charitable-gaming game-selection user-management,Medium,,Role enables Game Selection and Quarterly Workflow; without role denied; permissions limited to those workflows.,,
Story,CG-USR-COMPLIANCEROLE-005 Single Compliance and Regulator read-only role,Read-only role for history validation activation and audit. FR-052.,,CG-USR User and Role Management,charitable-gaming game-selection user-management,Medium,,Role enables read-only review of history validation activation audit; operational actions not permitted; no edit acknowledge or activate permission.,,
```

You can run the `jira-csv-import` skill to create these issues in Jira. This breakdown does not push to Jira.

## Traceability and Handback

### Story to source and requirement traceability

| Story key | Source story | Requirement refs |
| --- | --- | --- |
| CG-CTX-CHARITY-001 | US-001 | FR-001, FR-002, FR-051 |
| CG-CTX-REGULATOR-002 | US-001, US-011 | FR-003, FR-052 |
| CG-CTX-SUPPORT-003 | US-001, US-011 | FR-003.1 |
| CG-CTX-RETAILER-004 | US-001 | FR-003.2 |
| CG-CTX-ALLROLES-005 | US-001 | FR-003.3, NFR-001 |
| CG-CAT-DISPLAY-001 | US-002 | FR-004, FR-005, FR-005.1, NFR-005, NFR-006 |
| CG-CAT-ENTRY-002 | US-002 | FR-006 |
| CG-CAT-CPP-003 | US-002 | FR-006.1, FR-006.2, FR-010 |
| CG-CAT-HIDE-004 | US-002 | FR-007 |
| CG-CAT-FILTER-005 | US-002, US-003 | FR-058 |
| CG-SEL-VALIDCOMBO-001 | US-003 | FR-008, FR-010 |
| CG-SEL-LIMITS-002 | US-003 | FR-009 |
| CG-SEL-VALIDATION-003 | US-003, US-008 | FR-011, NFR-004 |
| CG-DRFT-ADDREMOVE-001 | US-004 | FR-012, FR-013, FR-014 |
| CG-DRFT-COPY-002 | US-005 | FR-015, FR-016 |
| CG-DRFT-MULTISITE-003 | US-005 | FR-015.1 |
| CG-DRFT-SAVE-004 | US-006 | FR-017, NFR-007 |
| CG-DRFT-AUDIT-005 | US-006 | FR-017.1 |
| CG-DASH-OVERVIEW-001 | US-007 | FR-021 |
| CG-DASH-LIST-002 | US-007 | FR-053, FR-054, FR-055 |
| CG-DASH-FILTER-003 | US-007 | FR-056 |
| CG-DASH-OPEN-004 | US-007 | FR-057 |
| CG-DASH-STATUSMODEL-005 | US-007 | FR-022, FR-022.4, FR-022.5 |
| CG-DASH-DOWNLOAD-006 | US-007 | FR-022.1 |
| CG-DASH-SWITCH-007 | US-007 | FR-022.2 |
| CG-DASH-MONITOR-008 | US-007 | FR-022.3, NFR-009 |
| CG-ACT-REVALIDATE-001 | US-008 | FR-024, NFR-004 (FR-023 partial, see D-002) |
| CG-ACT-MINCONFIG-002 | US-009 | FR-026 |
| CG-ACT-RECORD-003 | US-009 | FR-027 |
| CG-ACT-LOCK-004 | US-009 | FR-028, NFR-007 |
| CG-ACT-ELIGIBILITY-005 | US-010 | FR-025 (see D-001) |
| CG-ACT-DISTINIT-006 | US-010 | FR-029 |
| CG-ACT-PERGAME-007 | US-010 | FR-029.1, NFR-010 |
| CG-ACT-JOBAUDIT-008 | US-010 | FR-030 |
| CG-ACT-OUTCOME-009 | US-010 | FR-031 |
| CG-ACT-QUARTERSTAMP-010 | US-010 | FR-038 |
| CG-INQ-ADD-001 | US-015 | FR-031.1, FR-009, FR-004 |
| CG-INQ-CLOSE-002 | US-016 | FR-031.1 |
| CG-INQ-EQUIP-003 | US-017 | FR-047.2, NFR-011 |
| CG-OVS-REGVIEW-001 | US-011 | FR-032, NFR-008 |
| CG-OVS-DELEGATE-002 | US-011 | FR-033 |
| CG-OVS-AUDITLOG-003 | US-012 | FR-034, NFR-002 |
| CG-OVS-RETENTION-004 | US-012 | FR-035, NFR-003 |
| CG-META-EXPOSE-001 | US-013 | FR-036 |
| CG-META-PROPAGATE-002 | US-013, US-014 | FR-037, NFR-006 |
| CG-META-UPLOAD-003 | US-013 | FR-039 |
| CG-META-MANAGE-004 | US-013 | FR-040 |
| CG-META-EDITOR-005 | US-013 | FR-041 |
| CG-SW-INSTALL-001 | US-014 | FR-042 |
| CG-SW-UNINSTALL-002 | US-014 | FR-043 |
| CG-SW-CONFIG-003 | US-014 | FR-044, FR-045 |
| CG-SW-AUTOJOB-004 | US-014 | FR-046, FR-047 |
| CG-SW-HISTORY-005 | US-014 | FR-047.1, NFR-009 |
| CG-USR-MANAGE-001 | US-011 | FR-048 |
| CG-USR-EGMROLE-002 | US-011 | FR-049 |
| CG-USR-OPSROLE-003 | US-013 | FR-050 |
| CG-USR-CHARITYROLE-004 | US-001 | FR-051 |
| CG-USR-COMPLIANCEROLE-005 | US-011, US-012 | FR-052 |

### Open questions

- OQ-1: FR-023 text is corrupted (a revalidation statement spliced with a site-level Game Switch summary-view sentence). Confirm the intended requirement and whether a site-level Game Switch summary view is in scope. Tracked as D-002.
- OQ-2: The package defines no concrete numeric Cost Per Play, payout, or top-prize values; these come from the authoritative catalog. Acceptance criteria use the explicit limit values (twelve titles, five Cost Per Play) and catalog-derived representative values. Confirm this is acceptable for test design.
- OQ-3: FR-025 does not state the regulatory grace-period lengths or the pre-quarter start-and-close boundary values. Tracked as D-001 (spike) and gating CG-ACT-ELIGIBILITY-005.
- OQ-4: FR-022.3 relies on the existing Game Switch Monitor and Job Monitor Intelligence. Confirm these EVO capabilities are available to this feature set or whether integration work is required.

### Summary

- Source user stories (from the package traceability table): 17.
- Jira stories generated: 58.
- Epics: 11.
- Stories by recommended phase: Phase 1 = 26, Phase 2 = 25, Phase 3 = 7.
- Deferred items: 3 (D-001 spike, D-002 clarification, D-003 out-of-scope for Phase 1).
- All functional requirements FR-001 through FR-058 are mapped to at least one story. Non-functional requirements are cited on the stories whose behavior they constrain.
