---
type: requirements-package
artifact_id: A-07
phase: "2 - Specification"
zone: 2
owner: Product Mgt
ai: Full
---

# Requirements Package: EVOCR-9419 Package Management Updates

## Source and Scope

| Item | Value |
| --- | --- |
| Change request | EVOCR-9419 Package Management Updates |
| Authoritative source | EVOCR-9419 description (6 items), supplied in chat on 2026-11-19 |
| Clarifications | Product Management responses captured in [Decisions from Clarification](#decisions-from-clarification) |
| Reference / Example documents | None |
| Primary persona | Content Manager |
| Product Vision / User Story artifacts | None supplied. User stories below are derived from the description. |

In scope:

- Package Management Editor: split the package table into an Expected Packages tab and an Additional Discovered Packages tab.
- Package Management Editor: restyle the Active checkbox so it reads as editable.
- Package Management Editor: add an Out-of-Sync Date per Software Profile.
- Content Workspace Tile View (Desktop - Download tile): grey and red EGM status indicator.
- Content Workspace Table View EGM nodes: the same indicator (Could priority).
- Content Workspace Table View Power Search: new option for EGMs with Additional Discovered Packages.

Out of scope:

- How the host discovers packages on an EGM and adds them to a Software Profile. This change consumes that existing behavior.
- Changes to who is allowed to edit a Software Profile.

## Terminology

These terms are confirmed for this package. They are not in `docs/glossary.md`. Glossary updates were declined for now.

| Term | Definition |
| --- | --- |
| Package Management Editor | The UI where a user views a Software Profile and sets which packages are Active. |
| Software Profile | A list of packages shared by one or more assigned EGMs. Each package in the profile has an Active flag. |
| Expected Package | A package in a Software Profile that is checked Active. |
| Additional Discovered Package | A package that the host added to a Software Profile and that is not checked Active. |
| Host | The EVO system. It automatically adds packages that are reported or discovered on an EGM to the EGM's Software Profile. It is not a user. |
| Content Workspace | The GUI workspace for EGM content. It has a Tile View (Desktop - Download tile) and a Table View with EGM nodes. |
| Out-of-Sync Date | An optional user-entered date, one per Software Profile. After this date, assigned EGMs still missing Expected Packages show a red indicator. |
| Package Status Indicator | The grey or red dot shown for an EGM in the Content Workspace. Named here for traceability. Final UI label is TBD. |

## Derived User Stories

| ID | Description item | User Story | Priority |
| --- | --- | --- | --- |
| US-001 | 1 | As a Content Manager, I want the Package Management Editor to separate Expected Packages from Additional Discovered Packages, so that I can tell what should be on my EGMs apart from what the system found. | Must |
| US-002 | 2 | As a Content Manager, I want the Active checkboxes to look clearly editable, so that I do not mistake them for read-only fields. | Must |
| US-003 | 3, 6 | As a Content Manager, I want a status dot on the Download tile for EGMs with Additional Discovered Packages or overdue Expected Packages, so that I notice these EGMs without opening each one. | Must |
| US-004 | 4, 6 | As a Content Manager, I want the same status dot on EGM nodes in the Table View, so that I see the same alert in either view. | Could |
| US-005 | 5 | As a Content Manager, I want a Power Search option for EGMs with Additional Discovered Packages, so that I get a discrete list of them. | Must |
| US-006 | 6 | As a Content Manager, I want to set an Out-of-Sync Date on a Software Profile, so that EGMs still missing Expected Packages after that date are escalated to a red dot. | Must |

## Functional Requirements

Priority uses MoSCoW. Requirements tagged with an assumption ID (A-xx) depend on that assumption.

### Package Management Editor: Tabs

| ID | Requirement | Priority | Basis |
| --- | --- | --- | --- |
| FR-001 | The Package Management Editor shall display the packages of the selected Software Profile in two tabs labeled "Expected Packages" and "Additional Discovered Packages". | Must | Item 1 |
| FR-001.1 | The Expected Packages tab shall list every package in the Software Profile that is checked Active. | Must | Item 1.1 |
| FR-001.2 | The Expected Packages tab shall not list any package that is not checked Active. | Must | Item 1.1 |
| FR-001.3 | The Additional Discovered Packages tab shall list every package in the Software Profile that is not checked Active. | Must | Item 1.2, A-01 |
| FR-001.4 | The Additional Discovered Packages tab shall not list any package that is checked Active. | Must | Item 1.2 |
| FR-002 | The Package Management Editor shall open on the Expected Packages tab. | Should | A-02 |
| FR-003 | The Expected Packages tab shall keep the columns and row actions of the current package table. | Must | Item 1, A-03 |
| FR-004 | The Additional Discovered Packages tab shall display the same columns as the Expected Packages tab. | Should | A-03 |
| FR-005 | When a user checks Active on a package in the Additional Discovered Packages tab, the system shall move that package to the Expected Packages tab. | Must | Clarification D-04 |
| FR-006 | When a user unchecks Active on a package in the Expected Packages tab, the system shall move that package to the Additional Discovered Packages tab. | Must | Clarification D-04, A-01 |
| FR-007 | The system shall show a package in its new tab without requiring the user to close or reload the Package Management Editor. | Should | A-04 |
| FR-008 | Each tab shall display an empty-state message when it contains no packages. | Should | A-05 |

### Package Management Editor: Active Checkbox

| ID | Requirement | Priority | Basis |
| --- | --- | --- | --- |
| FR-009 | The Active checkbox outline shall be white. | Must | Item 2, D-05 |
| FR-010 | The Active checkbox check mark shall be white. | Must | Item 2, D-05 |
| FR-011 | The Active checkbox shall be rendered at twice its current width and twice its current height. | Must | Item 2 |
| FR-012 | FR-009 through FR-011 shall apply to the Active checkbox in both the Expected Packages tab and the Additional Discovered Packages tab. | Must | Item 2, FR-001 |
| FR-013 | The Active checkbox shall remain editable for every user who can edit the Software Profile today. | Must | Item 2.1, A-06 |

### Package Management Editor: Out-of-Sync Date

| ID | Requirement | Priority | Basis |
| --- | --- | --- | --- |
| FR-014 | The Package Management Editor shall provide an Out-of-Sync Date field for the selected Software Profile. | Must | Item 6, D-07 |
| FR-014.1 | The system shall store at most one Out-of-Sync Date per Software Profile. | Must | D-07 |
| FR-014.2 | The system shall allow a Software Profile to be saved without an Out-of-Sync Date. | Must | D-08 |
| FR-014.3 | The system shall allow a user to clear an existing Out-of-Sync Date. | Should | D-08 |
| FR-014.4 | A saved Out-of-Sync Date shall apply to every EGM assigned to the Software Profile. | Must | D-03, D-07 |

### Package Status Indicator: Evaluation Rules

| ID | Requirement | Priority | Basis |
| --- | --- | --- | --- |
| FR-015 | The system shall treat an EGM as having Additional Discovered Packages when the EGM reports at least one installed package that is an Additional Discovered Package in its assigned Software Profile. | Must | Items 3, 5, A-07 |
| FR-016 | The system shall treat an EGM as missing Expected Packages when at least one Expected Package in its assigned Software Profile is not reported as installed on the EGM. | Must | Item 6, D-09, A-07 |
| FR-017 | The system shall set an EGM's Package Status Indicator to red when the assigned Software Profile has an Out-of-Sync Date, the current date is after that date, and the EGM is missing Expected Packages. | Must | Item 6, D-09, A-08 |
| FR-018 | The system shall set an EGM's Package Status Indicator to grey when the FR-017 red conditions are not met and the EGM has Additional Discovered Packages. | Must | Item 3, D-10 |
| FR-019 | The system shall show no Package Status Indicator for an EGM that meets neither the red conditions in FR-017 nor the grey conditions in FR-018. | Must | Items 3, 6 |
| FR-020 | The system shall not set a red Package Status Indicator for EGMs whose assigned Software Profile has no Out-of-Sync Date. | Must | D-08 |
| FR-021 | The system shall re-evaluate an EGM's Package Status Indicator when any of these events occurs: an Active flag changes in the assigned Software Profile, the Out-of-Sync Date is set, changed, or cleared, the Out-of-Sync Date passes, or the EGM's reported package inventory changes. | Must | Items 3, 6, NFR-002 |

### Content Workspace: Tile View

| ID | Requirement | Priority | Basis |
| --- | --- | --- | --- |
| FR-022 | The Content Workspace Tile View (Desktop - Download tile) shall display each EGM's Package Status Indicator as defined in FR-017 through FR-019. | Must | Items 3, 6, A-09 |

### Content Workspace: Table View

| ID | Requirement | Priority | Basis |
| --- | --- | --- | --- |
| FR-023 | The Content Workspace Table View shall display each EGM node's Package Status Indicator as defined in FR-017 through FR-019. | Could | Items 4, 6, D-06 |

### Content Workspace: Power Search

| ID | Requirement | Priority | Basis |
| --- | --- | --- | --- |
| FR-024 | EGM View Power Search in the Content Workspace Table View shall include a selectable option for EGMs with Additional Discovered Packages. | Must | Item 5, D-11 |
| FR-024.1 | When the option is selected, the results shall include every EGM that has Additional Discovered Packages as defined in FR-015. | Must | Item 5 |
| FR-024.2 | When the option is selected, the results shall exclude every EGM that does not have Additional Discovered Packages. | Must | Item 5 |
| FR-024.3 | The option shall be combinable with other Power Search criteria. The results shall contain only EGMs that match all selected criteria. | Should | A-10 |

## Non-Functional Requirements

| ID | Requirement | Priority | Basis |
| --- | --- | --- | --- |
| NFR-001 | An EGM shall show the same Package Status Indicator state in the Tile View and the Table View at the same point in time. | Must (if FR-023 is delivered) | Items 3, 4 |
| NFR-002 | A change that triggers re-evaluation under FR-021 shall be reflected in the Package Status Indicator within TBD seconds. | Should | OQ-05 |
| NFR-003 | Adding the Package Status Indicator shall not increase Tile View load time by more than TBD for a site with TBD EGMs. | Should | OQ-06 |
| NFR-004 | A Power Search using the FR-024 option shall return results within TBD seconds for a site with TBD EGMs. | Should | OQ-06 |
| NFR-005 | The white Active checkbox outline and check mark shall meet a non-text contrast ratio of at least 3:1 against the background (WCAG 2.1 AA, SC 1.4.11). Proposed target, pending confirmation. | Should | Item 2.1, OQ-07 |

## Assumptions

| ID | Assumption | Basis | Impact if wrong |
| --- | --- | --- | --- |
| A-01 | Assumed: the Additional Discovered Packages tab shows every non-Active package in the Software Profile, whether the host or a user added it. | The description limits tab 2 to host-added packages, but D-04 confirms that unchecking Active in tab 1 moves a package to tab 2. | FR-001.3 and FR-006 change. User-added packages that are unchecked would need another location (see OQ-02). |
| A-02 | Assumed: the editor opens on the Expected Packages tab. | The description calls it "the first tab". | FR-002 changes. |
| A-03 | Assumed: the existing package table columns stay as they are, and tab 2 reuses them. | The description says the current table becomes tab 1 and gives no column changes. | FR-003 and FR-004 change if tab 2 needs extra columns, such as the discovering EGM or the discovery date. |
| A-04 | Assumed: moving a package between tabs follows the editor's existing save behavior. It is not saved immediately on toggle. | No save behavior is described. | FR-005 through FR-007 need a save or confirmation requirement. |
| A-05 | Assumed: an empty tab shows a message instead of a blank table. | Standard UX practice. Not in the source. | FR-008 is removed. |
| A-06 | Assumed: the existing edit permissions for the Software Profile and the Active flag do not change. | Item 2 is about appearance only. | FR-013 changes, and permission requirements must be added. |
| A-07 | Assumed: whether an EGM "has" a package comes from the package inventory that the EGM already reports to EVO. | D-01 confirms that the host discovers packages reported on the EGM. | FR-015 and FR-016 need a different data source (see OQ-10). |
| A-08 | Assumed: "after the Out-of-Sync Date" means the current date is later than the stored date. Time zone and day boundary are TBD. | The description gives no boundary rule. | FR-017 test cases change (see OQ-01). |
| A-09 | Assumed: the Desktop - Download tile view shows one tile per EGM. | The description refers to "any EGM" in that view. | FR-022 changes if a tile represents a group of EGMs. In that case a roll-up rule is needed (see OQ-09). |
| A-10 | Assumed: the new option combines with other Power Search criteria using the existing intersection behavior. | The glossary says Power Search criteria are executed and their results intersected. | FR-024.3 changes. |

## Decisions from Clarification

| ID | Decision |
| --- | --- |
| D-01 | "Host" means the EVO system auto-adding packages discovered on an EGM. It is not a user. |
| D-02 | Tab 2 label is "Additional Discovered Packages". |
| D-03 | A Software Profile is shared by many EGMs. |
| D-04 | Users can toggle Active in both tabs. Checking Active moves a package to Expected Packages. Unchecking moves it to Additional Discovered Packages. |
| D-05 | Checkbox style: white outline and white check mark on the existing dark background, twice the current size. |
| D-06 | The Table View indicator (item 4) is Could priority. |
| D-07 | The date is called the Out-of-Sync Date. It is entered in the Package Management Editor, one per Software Profile. |
| D-08 | The Out-of-Sync Date is optional. Without a date, no red indicator is shown. |
| D-09 | Red rule: red when the Out-of-Sync Date has passed and the EGM is missing at least one Expected Package, whether or not it has Additional Discovered Packages. This replaces the description's "extra packages beyond that date" wording. |
| D-10 | Grey rule: grey whenever the EGM has at least one Additional Discovered Package and red does not apply. This replaces the description's condition that all Expected Packages must be present. |
| D-11 | Power Search gets one option: EGMs with any Additional Discovered Packages. |
| D-12 | The indicator has no tooltip. |

## Conflicts Detected

- **Red dot rule.** The description says red appears when an EGM "has extra packages beyond that date". Product Management redefined it as Expected Packages still missing after the Out-of-Sync Date (D-09). The requirements follow D-09.
- **Grey dot rule.** The description shows grey only when all Expected Packages are present. Product Management chose to show grey whenever Additional Discovered Packages exist (D-10). The requirements follow D-10.
- **Tab 2 contents.** The description limits tab 2 to host-added packages. D-04 lets users move any package there by unchecking Active. The requirements follow A-01, which is still open as OQ-02.

## Open Questions

- [ ] OQ-01: Which time zone and day boundary decide when the Out-of-Sync Date has passed? Options include site local time or server time, and start or end of day. (FR-017, A-08)
- [ ] OQ-02: If a user unchecks Active on a package that the host did not add, should it appear in Additional Discovered Packages? (FR-001.3, FR-006, A-01)
- [ ] OQ-03: Can a user enter an Out-of-Sync Date in the past? If so, should red indicators appear immediately on save? (FR-014)
- [ ] OQ-04: What is the final Power Search option label? Proposed: "Has Additional Discovered Packages". (FR-024)
- [ ] OQ-05: How fast must the indicator reflect a change (NFR-002)? Is it real-time, on the next poll, or on page refresh?
- [ ] OQ-06: What are the load-time and Power Search performance targets? What EGM count should they be measured at? (NFR-003, NFR-004)
- [ ] OQ-07: Accept WCAG 2.1 AA 3:1 non-text contrast for the checkbox (NFR-005)? The red and grey dots differ only by color and have no tooltip (D-12). Is a legend or other non-color cue needed for color-blind users?
- [ ] OQ-08: Should changes to Active flags and the Out-of-Sync Date be recorded in an audit log, including user, timestamp, and old and new values?
- [ ] OQ-09: Does the Desktop - Download tile represent one EGM or a group of EGMs? If it is a group, what is the roll-up rule for the dot? (FR-022, A-09)
- [ ] OQ-10: What is the source of truth for the packages installed on an EGM? Is it the reported inventory, GAT verification results, or something else? (FR-015, FR-016, A-07)
- [ ] OQ-11: An EGM missing Expected Packages before the Out-of-Sync Date, with no Additional Discovered Packages, shows no dot. Is that intended, or does an existing indicator already cover it? (FR-019)
- [ ] OQ-12: Is there a light theme where white checkboxes would not be visible? (FR-009, FR-010)

## Traceability to User Stories

| User Story | Mapped Requirements |
| --- | --- |
| US-001 Separate Expected and Additional Discovered Packages | FR-001 through FR-008 (including FR-001.1 through FR-001.4) |
| US-002 Editable-looking Active checkbox | FR-009 through FR-013 |
| US-003 Indicator on Tile View | FR-015, FR-018, FR-019, FR-021, FR-022 |
| US-004 Indicator on Table View EGM nodes | FR-015, FR-018, FR-019, FR-021, FR-023 |
| US-005 Power Search option | FR-015, FR-024 (including FR-024.1 through FR-024.3) |
| US-006 Out-of-Sync Date and red escalation | FR-014 (including FR-014.1 through FR-014.4), FR-016, FR-017, FR-018, FR-020, FR-021 |

Unmapped Requirements: None.

Unmapped User Stories: None.
