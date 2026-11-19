# Jira Epic and Story Breakdown: Game Editor and Game Switch Editor Changes

**Source:** [requirements-package.md](requirements-package.md) (A-07)
**UX reference:** none available for this feature set
**User-story reference:** none available as a standalone document; US-012/US-013/US-014 identifiers below are carried over from the requirements package's traceability table only

## Executive Summary

This breakdown converts the Game Editor and Game Switch Editor requirements package into INVEST-compliant, Definition-of-Ready Jira stories. No separate user-story narrative exists for this feature set, so intent was derived from the requirements package's Executive Summary and functional/non-functional requirements.

The primary slicing axis is **editor/screen context**, because acceptance behavior genuinely differs between the read-only display screens (Game Editor, Game Set Editor) and the interactive selection-and-resolution screen (Game Switch Editor). Cross-cutting access control, audit, and compatibility concerns are split into their own epic per the concern-type separation rule.

- **Epics:** 4
- **Stories:** 14
- **Delivery phases:** Phase 1 (core engine) – 3 stories; Phase 2 (editor UI) – 8 stories; Phase 3 (access control, audit, hardening) – 3 stories
- **Deferred items:** none (the requirements package records no open questions or TBD decision paths)

## Epic Structure

| Epic | Variant | Summary | Requirement coverage |
| --- | --- | --- | --- |
| A. Game Editor Enhancements | GE | Theme Version display, Top Prize display, optional Position on the Details and Paytables tabs | FR-001 series, FR-002 series, FR-003 series |
| B. Game Set Editor Enhancements | GSE | Optional Position and paytable summary display on the Games tab | FR-013 series, FR-014–FR-017 |
| C. Game Switch Editor Enhancements | GSW | Position-from-Chooser behavior, Cost Per Play/paytable selection and resolution on the Switch On/Off tab | FR-004 series, FR-005 series, FR-006–FR-009 series |
| D. Access Control, Audit & Compatibility | SYS | Audit logging, permission enforcement, non-regression of existing Position data, visual standard conformance | FR-010, FR-011, FR-012 series, NFR-001 |

## Deferred Register

None. The requirements package's Open Questions section records no outstanding items, and no TBD or undefined decision paths were found in the functional or non-functional requirements.

## Story Backlog (grouped by epic)

### Epic A — Game Editor Enhancements

#### CG-GAMECFG-GE-001 Display Theme Version on Game Editor Details Tab (GE)

- Epic: Game Editor Enhancements
- Phase: 2 — editor UI
- Source story: US-013 (requirements-package traceability table; narrative not available)
- Requirement refs: FR-001, FR-001.1, FR-001.2
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, ge

As End User (General System User)
I want to see the Theme Version on the Game Editor Details tab
So that I can confirm which theme version a G2S game is running without leaving the editor

Scope
- In scope: read-only Theme Version field on the Details tab, sourced from the imported game configuration's theme metadata.
- Out of scope: editing Theme Version; displaying theme version history.
- Dependencies: none.

Acceptance Criteria
- Given a G2S game open in the Game Editor Details tab, when the tab loads, then a Theme Version field displays the value from the imported game configuration (e.g., theme metadata "3.2.1" displays as "3.2.1").
- Given the Theme Version field is displayed, when the user attempts to edit it, then the field remains read-only.
- Given a game configuration with no theme version attribute, when the tab loads, then the field displays blank rather than raising an error.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

#### CG-GAMECFG-GE-002 Display Top Prize in Game Editor Paytables Bets Table (GE)

- Epic: Game Editor Enhancements
- Phase: 2 — editor UI
- Source story: US-013
- Requirement refs: FR-002, FR-002.1, FR-002.2, FR-002.3
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, ge

As End User (General System User)
I want to see the Top Prize for each bet row in the Game Editor Paytables tab
So that I can evaluate paytable payout potential without cross-referencing other sources

Scope
- In scope: read-only Top Prize column in the Bets table, currency-formatted to two decimal places in the deployment display currency.
- Out of scope: editing Top Prize; multi-currency conversion or display-currency switching.
- Dependencies: none.

Acceptance Criteria
- Given a bet row with paytable data in the Game Editor Paytables tab, when the Bets table renders, then a Top Prize column displays the value derived from that row's paytable data (e.g., "$5,000.00").
- Given the Top Prize value, when displayed, then it shows two decimal places in the deployment display currency.
- Given the Top Prize field, when the user attempts to edit it, then the field remains read-only.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

#### CG-GAMECFG-GE-003 Make Position Field Optional in Game Editor Paytables Bets Table (GE)

- Epic: Game Editor Enhancements
- Phase: 2 — editor UI
- Source story: US-013
- Requirement refs: FR-003, FR-003.1
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, ge

As End User (General System User)
I want to leave the Position field blank in the Paytables Bets table
So that I can save a bet row without assigning a position when one is not yet known

Scope
- In scope: removing required validation on Position for Bets table rows.
- Out of scope: Position behavior on the Game Set Editor Games tab (CG-GAMECFG-GSE-001) or Switch On/Off tab (CG-GAMECFG-GSW-002).
- Dependencies: none directly; non-regression is verified together with CG-GAMECFG-SYS-003.

Acceptance Criteria
- Given a bet row with Position left blank, when the user saves the row, then the save succeeds without a validation error.
- Given a bet row with an existing Position value, when the user edits other fields and saves without changing Position, then the existing Position value is preserved unchanged.
- Given a blank Position value, when the row is saved, then no default or placeholder value is auto-populated into Position.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

### Epic B — Game Set Editor Enhancements

#### CG-GAMECFG-GSE-001 Make Position Field Optional on Game Set Editor Games Tab (GSE)

- Epic: Game Set Editor Enhancements
- Phase: 2 — editor UI
- Source story: US-013
- Requirement refs: FR-013, FR-013.1
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, gse

As End User (General System User)
I want to leave Position blank on the Game Set Editor Games tab
So that I can configure a game set without assigning a position when one is not yet determined

Scope
- In scope: removing required validation on Position for Games tab rows.
- Out of scope: Position behavior on the Game Editor Paytables tab or Switch On/Off tab.
- Dependencies: none directly; non-regression is verified together with CG-GAMECFG-SYS-003.

Acceptance Criteria
- Given a game row on the Games tab with Position left blank, when the user saves, then the save succeeds without a validation error.
- Given a blank Position, when saved, then no error message is shown to the user.
- Given an existing game row with a Position value, when the user saves without changing Position, then the stored Position value is unchanged.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

#### CG-GAMECFG-GSE-002 Display Cost Per Play, Payout Percentage, and Top Prize on Game Set Editor Games Tab (GSE)

- Epic: Game Set Editor Enhancements
- Phase: 2 — editor UI
- Source story: US-013
- Requirement refs: FR-014, FR-015, FR-016, FR-017
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, gse

As End User (General System User)
I want to see each Cost Per Play with its Payout Percentage and Top Prize for a game
So that I can review the paytable options available for that game within the game set

Scope
- In scope: read-only display of Cost Per Play, Payout Percentage, and Top Prize derived from the selected theme's paytables.
- Out of scope: editing any of the three values; paytable selection (handled in Epic C for the Switch On/Off tab).
- Dependencies: none.

Acceptance Criteria
- Given a game with multiple paytables on the Games tab, when the tab renders, then each Cost Per Play (Denomination) defined for the game's theme is displayed.
- Given a displayed Cost Per Play, when shown, then its Payout Percentage is displayed read-only (e.g., Cost Per Play "$0.25" shows Payout Percentage "92.50%").
- Given a displayed Cost Per Play, when shown, then its Top Prize is displayed read-only (e.g., Top Prize "$2,500.00").
- Given the Cost Per Play, Payout Percentage, and Top Prize fields, when the user attempts to edit any of them, then all three remain read-only.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

### Epic C — Game Switch Editor Enhancements

#### CG-GAMECFG-GSW-001 Determine Game Position from EGM Chooser Ordering File (GSW)

- Epic: Game Switch Editor Enhancements
- Phase: 1 — core engine
- Source story: US-014
- Requirement refs: FR-005, FR-005.1
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, gsw

As End User (General System User)
I want the system to position a switched-on game in the EGM Game Chooser based on the EGM Chooser Ordering file
So that the game's displayed order on the EGM reflects the CCM-managed Chooser configuration rather than a manual field

Scope
- In scope: deriving a switched-on game's Chooser position from the EGM Chooser Ordering file, including default placement for unordered games.
- Out of scope: the Position field's UI behavior on the Switch On/Off tab (CG-GAMECFG-GSW-002).
- Dependencies: none; CG-GAMECFG-GSW-002 depends on this story.

Acceptance Criteria
- Given a game referenced in the EGM Chooser Ordering file, when the game is switched on, then its position in the EGM Game Chooser matches the order defined in that file.
- Given a switched-on game not referenced in the EGM Chooser Ordering file, when the Chooser is rendered, then the game is placed after all ordered games, in default order by theme name (e.g., an unordered game sorts alphabetically among other unordered games after the ordered set).
- Given the EGM Chooser Ordering file is the position source, when the Switch On/Off tab's Position field holds any value, then it has no effect on the game's actual Chooser position.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

#### CG-GAMECFG-GSW-002 Make Position Field Optional and Informational-Only on Switch On/Off Tab (GSW)

- Epic: Game Switch Editor Enhancements
- Phase: 2 — editor UI
- Source story: US-014
- Requirement refs: FR-004, FR-004.1, FR-004.2
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, gsw

As End User (General System User)
I want to leave Position blank on the Switch On/Off tab without it affecting game order
So that I am not forced to maintain a value that the EGM Chooser Ordering file already controls

Scope
- In scope: removing required validation on Position for the Switch On/Off tab and confirming it does not drive Chooser position.
- Out of scope: computing actual Chooser position (CG-GAMECFG-GSW-001).
- Dependencies: CG-GAMECFG-GSW-001 (position-from-Chooser behavior must exist to verify Position has no effect).

Acceptance Criteria
- Given the Switch On/Off tab, when a user leaves Position blank and saves, then the save succeeds without a validation error.
- Given a Position value entered on the Switch On/Off tab, when the game is switched on, then the system does not use that value to compute or store the game's actual Chooser position.
- Given an existing Position value on a previously switched-on game, when the user saves the tab without changing Position, then the stored value is preserved unchanged.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

#### CG-GAMECFG-GSW-003 Select One or More Cost Per Play Values to Switch On a Game (GSW)

- Epic: Game Switch Editor Enhancements
- Phase: 2 — editor UI
- Source story: US-014
- Requirement refs: FR-006, FR-006.1
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, gsw

As End User (General System User)
I want to select one or more Cost Per Play values for a game on the Switch On/Off tab
So that I can define which denominations the game offers once switched on

Scope
- In scope: multi-select Cost Per Play control and the minimum-one-selection rule for switch-on.
- Out of scope: Payout Percentage/Top Prize selection per Cost Per Play (CG-GAMECFG-GSW-004); paytable resolution (CG-GAMECFG-GSW-005).
- Dependencies: none; CG-GAMECFG-GSW-004 depends on this story.

Acceptance Criteria
- Given the Switch On/Off tab for a game, when the user selects one Cost Per Play value and attempts to switch the game on, then the Cost Per Play selection is accepted pending paytable configuration.
- Given the Switch On/Off tab, when the user selects multiple Cost Per Play values, then all selected values are retained for the game's switch-on configuration.
- Given the Switch On/Off tab, when the user attempts to switch a game on with zero Cost Per Play values selected, then the system rejects the switch-on request with a validation message.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

#### CG-GAMECFG-GSW-004 Select Payout Percentage and Top Prize per Selected Cost Per Play (GSW)

- Epic: Game Switch Editor Enhancements
- Phase: 2 — editor UI
- Source story: US-014
- Requirement refs: FR-007, FR-008, FR-008.1
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, gsw

As End User (General System User)
I want to choose exactly one Payout Percentage and one Top Prize for each selected Cost Per Play
So that I can specify which paytable variant a game uses at that denomination

Scope
- In scope: per-Cost-Per-Play selection of exactly one Payout Percentage and exactly one Top Prize from read-only, system-presented values.
- Out of scope: resolving the selections to a single paytable or rejecting invalid combinations (CG-GAMECFG-GSW-005).
- Dependencies: CG-GAMECFG-GSW-003 (Cost Per Play selection must exist before per-Cost-Per-Play paytable selection).

Acceptance Criteria
- Given a selected Cost Per Play, when the user configures it, then the system requires selection of exactly one Payout Percentage from the paytables defined for that game theme and Cost Per Play.
- Given the same selected Cost Per Play, when the user configures it, then the system requires selection of exactly one Top Prize from the paytables defined for that game theme and Cost Per Play.
- Given the Payout Percentage and Top Prize options, when presented, then both are read-only values the user selects among (e.g., user picks "92.50%" and "$2,500.00" from presented lists) rather than typed entries.
- Given multiple selected Cost Per Play values, when each is configured, then the Payout Percentage/Top Prize selection is independent per Cost Per Play.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

#### CG-GAMECFG-GSW-005 Resolve Selections to Exactly One Paytable and Reject Invalid Combinations (GSW)

- Epic: Game Switch Editor Enhancements
- Phase: 1 — core engine
- Source story: US-014
- Requirement refs: FR-009, FR-009.1
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, gsw

As End User (General System User)
I want the system to resolve my Cost Per Play, Payout Percentage, and Top Prize selections to exactly one valid paytable
So that I cannot switch on a game with an inconsistent or non-existent paytable combination

Scope
- In scope: resolution logic that maps a (Cost Per Play, Payout Percentage, Top Prize) tuple to exactly one paytable, and rejection of switch-on requests that fail to resolve.
- Out of scope: the selection UI itself (CG-GAMECFG-GSW-003, CG-GAMECFG-GSW-004).
- Dependencies: CG-GAMECFG-GSW-003, CG-GAMECFG-GSW-004 (selections must exist to be resolved).

Acceptance Criteria
- Given a selected Cost Per Play, Payout Percentage, and Top Prize, when they resolve to exactly one paytable defined for the game at that Cost Per Play, then the switch-on request for that Cost Per Play is accepted.
- Given a selected combination that does not resolve to any valid paytable (e.g., a Top Prize not defined for the chosen Payout Percentage at that Cost Per Play), when the user submits the switch-on request, then the system rejects it with a validation message identifying the invalid Cost Per Play.
- Given a selected combination that would resolve to more than one paytable, when the user submits the switch-on request, then the system rejects it rather than arbitrarily selecting one paytable.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

### Epic D — Access Control, Audit & Compatibility

#### CG-GAMECFG-SYS-001 Record Audit Entry for Switch-On/Off and Configuration Changes (SYS)

- Epic: Access Control, Audit & Compatibility
- Phase: 1 — core engine
- Source story: US-012
- Requirement refs: FR-012, FR-012.1
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, sys

As Regulator / Compliance Authority
I want an audit entry recorded whenever a game is switched on or off, or its switch-on configuration changes
So that I have a non-repudiable record of who changed what and when for compliance review

Scope
- In scope: audit entry creation for switch-on, switch-off, and switch-on configuration changes (Cost Per Play, Payout Percentage, Top Prize).
- Out of scope: audit entry viewing/reporting UI (not specified in the requirements package).
- Dependencies: none.

Acceptance Criteria
- Given a game is switched on, when the switch-on completes, then an audit entry is recorded capturing the acting user, the affected game, the configuration change, and a timestamp.
- Given a game is switched off, when the switch-off completes, then an audit entry is recorded with the same four data points.
- Given a switched-on game's Cost Per Play, Payout Percentage, or Top Prize selections change, when the change is saved, then an audit entry is recorded describing the specific configuration change.
- Given an audit entry has been recorded, when a Regulator / Compliance Authority reviews it, then all four fields (acting user, affected game, configuration change, timestamp) are present and accurate.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

#### CG-GAMECFG-SYS-002 Enforce Existing Permission Context for New Fields and Controls (SYS)

- Epic: Access Control, Audit & Compatibility
- Phase: 3 — access control, audit surfacing, compatibility hardening
- Source story: US-013, US-014
- Requirement refs: FR-010
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, sys

As End User (General System User)
I want the new Theme Version, Top Prize, Position, and Cost Per Play controls to honor my existing domain and permission context
So that I only see or edit what my role and domain already allow

Scope
- In scope: applying existing permission and domain rules to all new fields/controls introduced across Epics A, B, and C.
- Out of scope: defining new permission models or roles (none specified in the requirements package).
- Dependencies: CG-GAMECFG-GE-001, CG-GAMECFG-GE-002, CG-GAMECFG-GE-003, CG-GAMECFG-GSE-001, CG-GAMECFG-GSE-002, CG-GAMECFG-GSW-002, CG-GAMECFG-GSW-003, CG-GAMECFG-GSW-004 (fields/controls must exist to apply permission checks against).

Acceptance Criteria
- Given a user without edit permission for a given editor, when they view the new fields (Theme Version, Top Prize, Cost Per Play/Payout Percentage/Top Prize selections), then the fields render read-only or hidden consistent with existing permission rules for that editor.
- Given a user with edit permission for the Switch On/Off tab, when they access the Cost Per Play selection controls, then the controls are editable consistent with their existing domain scope.
- Given a user restricted to a specific domain, when they view games outside that domain, then the new fields are not exposed, consistent with current domain scoping.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

#### CG-GAMECFG-SYS-003 Preserve Existing Position Values with No Migration Required (SYS)

- Epic: Access Control, Audit & Compatibility
- Phase: 3 — access control, audit surfacing, compatibility hardening
- Source story: US-013, US-014
- Requirement refs: FR-011
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, sys

As End User (General System User)
I want existing records that already contain a Position value to remain unchanged after Position becomes optional
So that making Position optional does not silently alter or require migration of my existing game configurations

Scope
- In scope: non-regression verification that deploying the optional-Position change leaves existing Position data untouched, with no migration script required.
- Out of scope: new Position validation behavior (covered in CG-GAMECFG-GE-003, CG-GAMECFG-GSE-001, CG-GAMECFG-GSW-002).
- Dependencies: CG-GAMECFG-GE-003, CG-GAMECFG-GSE-001, CG-GAMECFG-GSW-002 (verifies non-regression after those changes are implemented).

Acceptance Criteria
- Given an existing bet row, game-set row, or switch-on record with a stored Position value, when the optional-Position change is deployed, then the stored value is displayed and persisted unchanged.
- Given an existing record with a Position value, when no migration script or data transformation is run, then the record's Position value remains identical before and after deployment.
- Given a newly created record with a blank Position, when compared to a pre-existing record with a Position value, then both save successfully without one affecting the other's validation behavior.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

#### CG-GAMECFG-SYS-004 Conform New Fields and Controls to Current Evolution Visual Standard (SYS)

- Epic: Access Control, Audit & Compatibility
- Phase: 3 — access control, audit surfacing, compatibility hardening
- Source story: US-013, US-014
- Requirement refs: NFR-001
- UX refs: none available
- Labels: charitable-gaming, game-gameswitch, sys

As End User (General System User)
I want the Theme Version, Top Prize, optional Position, and Cost Per Play controls to look and behave consistent with the rest of the application
So that the new UI feels native and predictable within the existing editors

Scope
- In scope: visual/interaction conformance review and remediation of all new fields and controls against the Evolution Visual standard version in force for this release.
- Out of scope: new standard definition (the standard itself is external to this feature set).
- Dependencies: CG-GAMECFG-GE-001, CG-GAMECFG-GE-002, CG-GAMECFG-GE-003, CG-GAMECFG-GSE-001, CG-GAMECFG-GSE-002, CG-GAMECFG-GSW-002, CG-GAMECFG-GSW-003, CG-GAMECFG-GSW-004 (all new fields/controls must exist for conformance review).

Acceptance Criteria
- Given the Theme Version field on the Game Editor Details tab, when rendered, then its layout, typography, and spacing conform to the Evolution Visual standard version in force for this release.
- Given the Top Prize field and Cost Per Play/Payout Percentage/Top Prize displays across the Game Editor, Game Set Editor, and Switch On/Off tab, when rendered, then they use the standard's read-only field styling conventions.
- Given the Cost Per Play selection controls on the Switch On/Off tab, when rendered, then their interactive states (selected, disabled, error) conform to the applicable standard's component states.

Definition of Ready
- No TBD text; exactly one primary-axis variant; one actor and one primary flow; test data and expected result explicit; regulatory/role constraints stated when relevant; dependencies identified; implementable and testable within a single two-week sprint.

## Suggested Starting Point

Start with **Epic A (Game Editor Enhancements)**: CG-GAMECFG-GE-001, CG-GAMECFG-GE-002, and CG-GAMECFG-GE-003. These are self-contained, read-only display and validation-relaxation stories with no dependencies, and they deliver visible end-to-end behavior in a single screen.

Run Phase 1 core-engine work in parallel: CG-GAMECFG-GSW-001 (position from Chooser), CG-GAMECFG-GSW-005 (paytable resolution), and CG-GAMECFG-SYS-001 (audit logging). The Game Switch Editor UI stories (CG-GAMECFG-GSW-002 through GSW-004) and the Phase 3 hardening stories depend on this engine work being in place first.

This is a non-binding sequencing recommendation; sprint scope remains the project manager's decision after estimation.

## Traceability

| Jira Story Key | Source Story | Requirement Refs |
| --- | --- | --- |
| CG-GAMECFG-GE-001 | US-013 | FR-001, FR-001.1, FR-001.2 |
| CG-GAMECFG-GE-002 | US-013 | FR-002, FR-002.1, FR-002.2, FR-002.3 |
| CG-GAMECFG-GE-003 | US-013 | FR-003, FR-003.1 |
| CG-GAMECFG-GSE-001 | US-013 | FR-013, FR-013.1 |
| CG-GAMECFG-GSE-002 | US-013 | FR-014, FR-015, FR-016, FR-017 |
| CG-GAMECFG-GSW-001 | US-014 | FR-005, FR-005.1 |
| CG-GAMECFG-GSW-002 | US-014 | FR-004, FR-004.1, FR-004.2 |
| CG-GAMECFG-GSW-003 | US-014 | FR-006, FR-006.1 |
| CG-GAMECFG-GSW-004 | US-014 | FR-007, FR-008, FR-008.1 |
| CG-GAMECFG-GSW-005 | US-014 | FR-009, FR-009.1 |
| CG-GAMECFG-SYS-001 | US-012 | FR-012, FR-012.1 |
| CG-GAMECFG-SYS-002 | US-013, US-014 | FR-010 |
| CG-GAMECFG-SYS-003 | US-013, US-014 | FR-011 |
| CG-GAMECFG-SYS-004 | US-013, US-014 | NFR-001 |

### Open Questions

None. The requirements package records no open questions, and no deferred decision paths were identified during decomposition.

### Summary

- Source stories referenced: 3 (US-012, US-013, US-014, identified only via the requirements package's traceability table)
- Jira stories generated: 14
- Epics: 4
- Stories by phase: Phase 1 — 3; Phase 2 — 8; Phase 3 — 3
- Stories deferred: 0
