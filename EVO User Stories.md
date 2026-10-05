# IntelligenEVO — User Stories by Service

## Executive Summary

This document defines user stories for IntelligenEVO functionality, organized by platform service. Each service section acts as an epic and groups INVEST-style stories derived from the end-to-end flows in [EVO User Journeys.md](EVO%20User%20Journeys.md). Stories follow the house format: role, capability, benefit, and testable acceptance criteria.

Service acronyms (DMS, GSM, FIN, UMS, and so on) are the canonical Evo Platform service names. Story IDs are prefixed with the service acronym to keep traceability clean across the seven functional domains.

## Service Index

| Domain | Services |
| --- | --- |
| Device Operations and Monitoring | DMS, SIT, PLSIT, CCS, RTDP, ADR |
| Content and Configuration Management | GSM, CCM, DCM, SCCM |
| Financials and Accounting | FIN, ACC, CMS |
| Player Services and Responsible Gaming | placcnt, plsessn, plsmart, plwallet, PlayRewards, PLLOGS |
| Reporting and Data | DLR, SCHS, CDS, Spin Level Data |
| Jackpots | JKPT |
| Platform Administration and Access | UMS, EIS, GHS, Intelligen EVO GUI |

---

## Domain 1: Device Operations and Monitoring

### DMS — Device Monitoring Service

- **DMS-US-001 — Ingest EGM and VSC status in real time:**
  - As a Floor Operator, I want DMS to continuously ingest status reports and event logs from every EGM and VSC so device health reflects the live state of the floor.
  - Acceptance Criteria:
    - DMS ingests status reports and event logs from all connected EGMs and VSCs.
    - Device health is updated as new reports arrive, without manual refresh.
    - Status changes are forwarded to downstream services (SIT, RTDP) for alerting and display.

- **DMS-US-002 — Surface device health to the monitoring view:**
  - As a Floor Operator, I want DMS to feed current device condition into the platform so I can see each device's state on the floor map.
  - Acceptance Criteria:
    - Each device exposes a current state (for example online, offline, fault) sourced from DMS.
    - A device's recent events are retrievable for drill-down.
    - Stale or disconnected devices are distinguishable from healthy ones.

### SIT — Situation Management

- **SIT-US-001 — Raise a situation from a device fault:**
  - As a Field Technician, I want SIT to raise a situation when a device reports a fault such as door open, tilt, or communication loss so I am alerted to conditions that need action.
  - Acceptance Criteria:
    - A device status change forwarded by DMS creates a situation with severity and location.
    - The situation is published through RTDP to subscribed GUI clients.
    - The situation appears in the active situations list with its device and timestamp.

- **SIT-US-002 — Acknowledge and close situations:**
  - As a Field Technician, I want to acknowledge a situation and have it close when the condition clears so the active list reflects only open problems.
  - Acceptance Criteria:
    - A technician can acknowledge a situation, recording who acknowledged and when.
    - When the device reports a cleared status, SIT closes the situation automatically.
    - Closed situations leave an auditable record of acknowledgement and resolution.

### PLSIT — Player Situation Service

- **PLSIT-US-001 — Scope player-specific situations to a device:**
  - As a Floor Operator or Host, I want PLSIT to handle player-specific conditions at a device so they surface alongside standard device situations.
  - Acceptance Criteria:
    - Player-specific conditions are scoped to the relevant device.
    - Player situations appear in the GUI next to device situations in one workflow.
    - The operator can view the player context tied to the situation.

### CCS — Command and Control Service

- **CCS-US-001 — Remotely command an EGM or VSC:**
  - As a Floor Operator, I want to send commands such as enable, disable, or set log verbosity to one or more devices so I can act without walking the floor.
  - Acceptance Criteria:
    - The operator can select one or more EGMs or VSCs and issue a supported command.
    - CCS dispatches the command to the targeted devices over the messaging layer.
    - Per-device acknowledgements are returned and reflected in command status.

- **CCS-US-002 — Apply a bulk command template:**
  - As a Floor Operator, I want to apply a command template across many devices at once so I can perform floor-wide actions efficiently.
  - Acceptance Criteria:
    - A command template can be applied to a selected group of devices.
    - CCS tracks each device's command independently.
    - The operator sees status update per device until each confirms the new state.

### RTDP — Real-Time Data Push

- **RTDP-US-001 — Stream live topology and status to the GUI:**
  - As a Floor Operator, I want RTDP to push live updates over WebSocket so the monitoring view refreshes continuously without reloading.
  - Acceptance Criteria:
    - The GUI subscribes to live updates served by RTDP from its in-memory topology and status cache.
    - Device status and situation changes propagate to subscribed clients in near real time.
    - New topology from GSM (added sites or devices) appears in the live view.

### ADR — Accidental Data Recorder

- **ADR-US-001 — Retrieve raw EGM logs on demand:**
  - As a Field Technician, I want ADR to fetch raw logs and data files from a specific EGM so I can diagnose intermittent behavior without being at the machine.
  - Acceptance Criteria:
    - The technician can request on-demand log retrieval for a named EGM.
    - ADR fetches the raw logs and data files from the device.
    - Retrieved files are made available to the technician for review.

---

## Domain 2: Content and Configuration Management

### GSM — Gaming Site Management

- **GSM-US-001 — Register a site, controller, and EGMs:**
  - As a Configuration Administrator, I want to create a site, register its VSCs, and associate EGMs, SRUs, vendors, models, and traits so new machines are known to the platform.
  - Acceptance Criteria:
    - The administrator can create a site record and register its VSCs.
    - EGMs can be associated with vendors, models, traits, and SRUs.
    - Licenses and configuration profiles can be assigned to the registered devices.

- **GSM-US-002 — Distribute configuration and publish topology changes:**
  - As a Configuration Administrator, I want GSM to distribute configuration to the VSCs and publish entity change events so downstream services pick up the new topology.
  - Acceptance Criteria:
    - Configuration is distributed to the physical VSCs over the messaging layer.
    - Entity change events are published for consumers such as RTDP and DMS.
    - The new site and devices become visible across the platform after distribution.

### CCM — Content and Configuration Management

- **CCM-US-001 — Build and manage game content:**
  - As a Content Manager, I want to create or update themes, paytables, packages, modules, and config profiles so the catalog reflects available content.
  - Acceptance Criteria:
    - The manager can create or update content entities in the catalog.
    - Package CRC, diff, and checksum operations run asynchronously without blocking the manager.
    - Computed package details are available for review and validation.

- **CCM-US-002 — Publish content for distribution:**
  - As a Content Manager, I want to publish validated content so it becomes available to distribute to devices.
  - Acceptance Criteria:
    - Only validated content sets can be published.
    - Published content is registered in the catalog and marked ready for distribution.
    - Published content is discoverable by DCM for installation.

### DCM — Distributed Content Management

- **DCM-US-001 — Distribute and install content to EGMs:**
  - As a Configuration Administrator or Operator, I want DCM to orchestrate pushing packages to target EGMs through the VSCs so published content is installed where intended.
  - Acceptance Criteria:
    - The operator can select target EGMs and the content to distribute.
    - DCM runs its per-EGM job engine to install, uninstall, or switch games as planned.
    - Progress is visible per device through each distribution step.

- **DCM-US-002 — Report and retry failed installs:**
  - As a Configuration Administrator or Operator, I want DCM to flag any device that failed so I can retry it.
  - Acceptance Criteria:
    - Completion is reported per device.
    - Failed devices are flagged distinctly from successful ones.
    - A failed device can be retried without reprocessing the whole batch.

### SCCM — Site Controller Content Manager

- **SCCM-US-001 — Provision site controller content:**
  - As a Configuration Administrator, I want SCCM to prepare packages and modules for the physical VSCs so controllers are provisioned to serve the floor.
  - Acceptance Criteria:
    - The administrator can select site controller content to provision.
    - SCCM prepares the packages and modules for the physical VSCs.
    - The administrator can confirm controllers received and applied the provisioned content.

---

## Domain 3: Financials and Accounting

### FIN — Financial Service

- **FIN-US-001 — Ingest meters and calculate period meters:**
  - As a Finance Analyst, I want FIN to ingest meter reports from the floor and calculate period meters so period data is captured for review.
  - Acceptance Criteria:
    - FIN ingests meter reports from the floor for the period.
    - Period meters are calculated from the ingested reports.
    - Meter issue detection flags anomalies for analyst review.

- **FIN-US-002 — Validate cashouts and generate the financial summary:**
  - As a Finance Analyst, I want FIN to create and validate cashout records and produce the period financial summary so the period can be closed for billing.
  - Acceptance Criteria:
    - Cashout records are created and validated for the period.
    - Flagged meter issues can be resolved or annotated before close.
    - A financial summary is generated and made available as source data for invoicing.

### ACC — Accounting Service

- **ACC-US-001 — Compute invoices and commissions:**
  - As a Billing and Accounting Clerk, I want ACC to compute invoice charges and commissions from FIN data using configured charge formulas so billing reflects the company hierarchy and site configurations.
  - Acceptance Criteria:
    - ACC consumes meter and cashout data from FIN for the billing period.
    - Charges and commissions are computed using configured formulas, including caps and tiered limits.
    - Generated invoices are available for clerk review and approval.

- **ACC-US-002 — Generate and upload EFT payment files:**
  - As a Billing and Accounting Clerk, I want ACC to generate EFT payment files in the required format so payments can be processed for the billing period.
  - Acceptance Criteria:
    - EFT payment files are generated in the required format after invoice approval.
    - Sites marked EFT-excluded are omitted from the payment files.
    - Payment files are uploaded for processing.

### CMS — Cash Management Service

- **CMS-US-001 — Record physical cash movements:**
  - As a Cash and Vault Operator, I want CMS to post cash movements using a double-entry model so EGM collections, vault deposits, drawer fills, sweeps, and room transfers are tracked.
  - Acceptance Criteria:
    - EGM collections post from the EGM to the vault using the double-entry model.
    - Vault deposits, cash drawer fills, money machine sweeps, and room transfers can be recorded.
    - Each movement is recorded with its source, destination, and amount.

- **CMS-US-002 — Reconcile daily cash:**
  - As a Cash and Vault Operator, I want to reconcile the day's physical cash movements against expected balances so cash is balanced across EGMs, vault, drawers, and rooms.
  - Acceptance Criteria:
    - The operator can reconcile recorded movements against expected balances.
    - Discrepancies are identifiable by location.
    - Reconciled totals balance across EGMs, vault, drawers, and rooms.

---

## Domain 4: Player Services and Responsible Gaming

### placcnt — Play Accounts

- **placcnt-US-001 — Register a player account:**
  - As a Player assisted by Cage or Host Staff, I want placcnt to create a named or anonymous account with credentials so I can use player services.
  - Acceptance Criteria:
    - A named or anonymous account can be created from player details.
    - Credentials and the authentication method are set up, including the progressive token model where used.
    - Account activity events are emitted to PLLOGS for the audit trail.

- **placcnt-US-002 — Enroll in player services:**
  - As a Player, I want to enroll in the services I choose, such as rewards or cashless wagering, so my account is set up for the features I want.
  - Acceptance Criteria:
    - The player can enroll in available services at or after account creation.
    - Enrollment state is retrievable by dependent services (plsessn, plwallet, PlayRewards).
    - Enrollment changes are logged through PLLOGS.

### plsessn — Play Sessions

- **plsessn-US-001 — Open a session with responsible-gaming checks:**
  - As a Player, I want plsessn to run responsible-gaming checks before play so a session opens only within the rules.
  - Acceptance Criteria:
    - plsessn requests player context from placcnt when a session starts.
    - plsmart runs limit, self-exclusion, survey, and tutorial checks before play is allowed.
    - If a limit or exclusion blocks play, the player is informed and the session does not open.

- **plsessn-US-002 — Close a session and update tracking:**
  - As a Player, I want plsessn to record the session close and update spending and time tracking so my play is accounted for.
  - Acceptance Criteria:
    - When the player stops, plsessn records the session close state.
    - plsmart updates spending and time tracking on close.
    - Session open and close events are available for audit through PLLOGS.

### plsmart — Play Smart

- **plsmart-US-001 — Set spending or time limits:**
  - As a Player assisted by Cage or Host Staff, I want plsmart to record and apply spending or time limits so limits are enforced on future play.
  - Acceptance Criteria:
    - A limit request is recorded and applied to future session checks.
    - The change is logged through PLLOGS.
    - On the next session open, plsessn and plsmart enforce the new limit.

- **plsmart-US-002 — Request self-exclusion:**
  - As a Player assisted by Cage or Host Staff, I want plsmart to record a self-exclusion so play is blocked for the exclusion period.
  - Acceptance Criteria:
    - A self-exclusion is recorded and applied to future session checks.
    - The exclusion is logged through PLLOGS.
    - Session checks block play while the exclusion is active.

### plwallet — Play Wallets

- **plwallet-US-001 — Deposit funds for cashless wagering:**
  - As a Player, I want plwallet to record a deposit against my wallet so I have funds available for cashless play.
  - Acceptance Criteria:
    - A deposit is recorded against the player's wallet.
    - plwallet pulls player context from placcnt and runs responsible-gaming checks through plsmart on the transaction.
    - The wallet balance updates and is available for cashless wagering.

- **plwallet-US-002 — Withdraw a wallet balance:**
  - As a Player, I want plwallet to process a withdrawal so I can cash out my balance.
  - Acceptance Criteria:
    - A withdrawal is processed against the available wallet balance.
    - Responsible-gaming checks apply to the withdrawal where configured.
    - The wallet balance updates to reflect the withdrawal.

### PlayRewards — Loyalty Rewards

- **PlayRewards-US-001 — Accrue loyalty from carded play:**
  - As a Player, I want PlayRewards to accrue loyalty based on my play so I earn rewards while I play carded.
  - Acceptance Criteria:
    - Loyalty accrues based on carded play tied to the player account.
    - The player can view their rewards balance and available offers.
    - Accrual is associated with the correct player via placcnt.

- **PlayRewards-US-002 — Redeem rewards:**
  - As a Player, I want to redeem earned rewards for an offered benefit so my loyalty has value.
  - Acceptance Criteria:
    - The player can redeem rewards for an available offer.
    - PlayRewards updates the balance after redemption.
    - The redemption is recorded.

### PLLOGS — Play Logs

- **PLLOGS-US-001 — Maintain a player activity audit trail:**
  - As a Regulator or Auditor, I want PLLOGS to capture player service activity so account, session, limit, and exclusion changes have an audit trail.
  - Acceptance Criteria:
    - Account activity, session open/close, limit, and exclusion events are recorded.
    - Each log entry carries the actor, action, and timestamp.
    - Logged activity is retrievable for audit and compliance review.

---

## Domain 5: Reporting and Data

### DLR — Data Lake Reporting

- **DLR-US-001 — Generate a jurisdiction or custom report:**
  - As a Reporting User or Regulator, I want DLR to run parameterized queries and format results into a file so I can meet a compliance or operational need.
  - Acceptance Criteria:
    - The user can select a jurisdiction report or build a custom report in the data workspace.
    - DLR runs parameterized queries against the data lake through Athena.
    - Results are formatted into a CSV or spreadsheet file and stored.

- **DLR-US-002 — Schedule and deliver reports:**
  - As a Reporting User, I want DLR to deliver a download link and run reports on a schedule so I can retrieve reports on demand or repeatedly.
  - Acceptance Criteria:
    - DLR delivers a download link to the user for generated reports.
    - A report can be scheduled for repeat delivery.
    - Scheduled runs produce the same formatted output as on-demand runs.

### SCHS — Site Controller Host Service

- **SCHS-US-001 — Run localized site controller reports:**
  - As a Reporting User serving a VSC, I want SCHS to generate reports such as cash flow, validations, EGM events, and commissions in English or French so I get operational reports in my language.
  - Acceptance Criteria:
    - The operator can select a site controller report and parameters.
    - SCHS generates the report and renders it using localized templates in English or French.
    - The rendered output matches the selected parameters.

### CDS — Custom Data Service

- **CDS-US-001 — Import data from external systems:**
  - As an Integration Administrator, I want CDS to process import files and load the data so external retailer, financial, or regulatory data enters the platform.
  - Acceptance Criteria:
    - An import can be configured to ingest data from a file.
    - CDS processes the import file and loads the data.
    - The administrator can verify the import ran and the data loaded as expected.

- **CDS-US-002 — Export data on a schedule:**
  - As an Integration Administrator, I want CDS to run scheduled exports so data flows out to external systems reliably.
  - Acceptance Criteria:
    - A scheduled export can be configured with its destination and timetable.
    - CDS runs the export on its schedule.
    - The administrator can verify the export ran and the data moved as expected.

### Spin Level Data

- **SLD-US-001 — Compile and export daily spin level data:**
  - As an IntelligenEVO user (Lottery employee or Gaming Operator), I want the platform to compile and export spin level data for all EGMs at the end of each calendar day so detailed gameplay data is available without manual intervention.
  - Acceptance Criteria:
    - Spin level data for all EGMs is compiled and exported at the end of each calendar day.
    - The export is delivered to the Data Lake.
    - The export includes gameplay, cash-in, and cash-out events for the day.

- **SLD-US-002 — Purge host copy after export:**
  - As a Product Manager, I want the host to purge its spin level data copy after a successful export so host storage stays lean.
  - Acceptance Criteria:
    - Host-side spin level data is purged after a successful Data Lake export.
    - No residual host storage of exported SLD remains.
    - Purge activity is logged for audit.

- **SLD-US-003 — Access daily spin level data in the Data Lake:**
  - As a Reporting User, I want to access the day's spin level data in the Data Lake within the retention window so I can analyze gameplay, cash-in, and cash-out events.
  - Acceptance Criteria:
    - The day's SLD is accessible from the configured Data Lake location.
    - Access is limited to authorized Lottery employees and Gaming Operators.
    - Data is available for the configured retention period.

---

## Domain 6: Jackpots

### JKPT — Jackpot Service

- **JKPT-US-001 — Configure a jackpot controller and tiers:**
  - As a Jackpot Administrator, I want to create a jackpot controller, define tiers and contributions, and associate eligible devices so a jackpot program contributes on the floor.
  - Acceptance Criteria:
    - The administrator can create a jackpot controller and define its tiers.
    - Contribution configuration can be set per tier.
    - Eligible devices can be associated and the configuration activated.

- **JKPT-US-002 — Process a jackpot award:**
  - As a Floor Operator or Attendant, I want JKPT to register and record a jackpot award and reseed the tier so awards are handled per site procedure.
  - Acceptance Criteria:
    - JKPT registers the award when the jackpot is triggered.
    - The attendant can review award details and process the award.
    - JKPT records the award and resets or reseeds the tier per its configuration.

---

## Domain 7: Platform Administration and Access

### UMS — User Management Service

- **UMS-US-001 — Sign in with single sign-on and role-based access:**
  - As any platform user, I want UMS to authenticate me and issue a token carrying my roles, domains, and permissions so I see only what I am authorized for.
  - Acceptance Criteria:
    - The user can authenticate using SAML single sign-on where configured.
    - UMS validates the identity and issues a JWT carrying roles, domains, and permissions.
    - The GUI uses the token to show only authorized features and data.

- **UMS-US-002 — Administer users, roles, and permissions:**
  - As a Security Administrator, I want to create or edit users and assign roles, domains, and permissions so access matches each user's job.
  - Acceptance Criteria:
    - The administrator can create or edit a user and assign roles, domains, and permissions.
    - Where an approval workflow applies, the change is routed for approval before taking effect.
    - UMS applies the change so the user's next sign-in reflects the new access.

### EIS — EGM Information Service

- **EIS-US-001 — Aggregate data for the GUI with enforced authorization:**
  - As any platform user, I want EIS to aggregate downstream data into the shapes the GUI needs so views load efficiently and only show authorized data.
  - Acceptance Criteria:
    - EIS aggregates data from downstream services into GUI-ready shapes.
    - Authorization is enforced so users only receive data they are permitted to see.
    - Aggregated results are cached to serve the GUI efficiently.

### GHS — General Hosting Service

- **GHS-US-001 — Host shared platform services:**
  - As a Platform Administrator, I want GHS to host shared platform capabilities so dependent services run reliably.
  - Acceptance Criteria:
    - Shared hosted capabilities are available to dependent services.
    - Hosted services report their availability for monitoring.
    - Failures are surfaced so administrators can respond.

### Intelligen EVO GUI

- **GUI-US-001 — Present a role-appropriate operator workspace:**
  - As any platform user, I want the Intelligen EVO GUI to present a role-appropriate view so I can perform my work across monitoring, content, financials, players, reporting, jackpots, and administration.
  - Acceptance Criteria:
    - The GUI renders features and data based on the signed-in user's token from UMS.
    - Live monitoring views subscribe to RTDP updates over WebSocket.
    - Unauthorized features and data are not shown to the user.

---

## Notes / Implementation Considerations

- Service-to-domain mapping and story content are derived from [EVO User Journeys.md](EVO%20User%20Journeys.md); treat that document as the source of truth for flow details.
- Story IDs are service-prefixed (for example DMS-US-001) to preserve per-service traceability. Renumber if these stories are imported into a tracker with its own scheme.
- Acceptance criteria describe observable outcomes. Non-functional detail (latency targets, retention periods, throughput) should be confirmed against the relevant product spec before build.
- Responsible-gaming stories (plsmart, plsessn, plwallet) carry regulatory weight. Route them through the Regulatory Risk Reviewer before finalizing acceptance criteria.

File created from: `EVO User Journeys.md`
