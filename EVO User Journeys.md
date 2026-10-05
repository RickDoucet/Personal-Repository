# IntelligenEVO User Journeys

## Executive Summary

This document captures end-to-end user journeys across the full IntelligenEVO platform, grouped by functional domain. Each journey is written as a narrative with a persona, a trigger, the steps the user takes, and the outcome they achieve. Together these journeys cover the operator and back-office capabilities exposed through the Intelligen EVO GUI, the player-facing services, and the platform administration layer.

Journeys are organized into seven domains:

1. Device Operations and Monitoring
2. Content and Configuration Management
3. Financials and Accounting
4. Player Services and Responsible Gaming
5. Reporting and Data
6. Jackpots
7. Platform Administration and Access

The domain-to-service mapping is drawn from the Evo Platform service catalog in the Business Glossary. Service acronyms (DMS, GSM, FIN, UMS, and so on) are defined there and are referenced inline so each journey can be traced back to the owning service.

## How to Read These Journeys

- Persona: the primary role performing the journey.
- Trigger: the event or need that starts the journey.
- Steps: the ordered actions the persona takes, with the systems involved.
- Outcome: the state the persona reaches when the journey completes.
- Services touched: the EVO services that participate behind the scenes.

## Domain and Service Map

| Domain | Primary EVO services |
| --- | --- |
| Device Operations and Monitoring | DMS, SIT, PLSIT, CCS, RTDP, ADR |
| Content and Configuration Management | GSM, CCM, DCM, SCCM |
| Financials and Accounting | FIN, ACC, CMS |
| Player Services and Responsible Gaming | placcnt, plsessn, plsmart, plwallet, PlayRewards, PLLOGS |
| Reporting and Data | DLR, SCHS, CDS, Spin Level Data |
| Jackpots | JKPT |
| Platform Administration and Access | UMS, EIS, GHS, Intelligen EVO GUI |

## Primary Personas

| Persona | Description |
| --- | --- |
| Floor Operator | Monitors the gaming floor and responds to real-time device conditions. |
| Field or Floor Technician | Resolves hardware and device issues on the floor. |
| Configuration Administrator | Registers and configures sites, controllers, and EGMs. |
| Content Manager | Builds and publishes game content, packages, and configuration profiles. |
| Finance Analyst | Reviews meters, period data, and financial summaries. |
| Billing and Accounting Clerk | Produces invoices and payment files. |
| Cash and Vault Operator | Handles physical cash movement across the site. |
| Player | Uses player account, session, wallet, and rewards services. |
| Cage or Host Staff | Assists players with accounts, limits, and responsible-gaming settings. |
| Reporting User | Generates and retrieves reports for operations or compliance. |
| Regulator or Auditor | Accesses read-only history, reports, and audit records. |
| Integration Administrator | Configures imports and exports to third-party systems. |
| Jackpot Administrator | Configures jackpot controllers, tiers, and contributions. |
| Security Administrator | Manages users, roles, domains, and permissions. |

---

## Domain 1: Device Operations and Monitoring

Services: DMS (Device Monitoring Service), SIT (Situation Management), PLSIT (Player Situation Service), CCS (Command and Control Service), RTDP (Real-Time Data Push), ADR (Accidental Data Recorder).

### J1.1 Monitor gaming floor health in real time

- Persona: Floor Operator
- Trigger: Start of shift, the operator needs a live picture of every EGM and site controller on the floor.
- Steps:
  1. The operator signs in to the Intelligen EVO GUI and opens the monitoring view.
  2. The GUI subscribes to live updates over WebSocket, served by RTDP from its in-memory topology and status cache.
  3. DMS continuously ingests status reports and event logs from EGMs and VSCs and feeds device health into the platform.
  4. The operator scans the floor map and status tiles, filtering by site, bank, or device state.
  5. The operator drills into a specific EGM to view its recent events and current condition.
- Outcome: The operator has a current, continuously refreshing view of device health and can spot problems before they affect play.
- Services touched: DMS, RTDP, Intelligen EVO GUI.

### J1.2 Respond to a device situation or alert

- Persona: Field or Floor Technician
- Trigger: An EGM reports a fault condition such as a door open, tilt, or communication loss.
- Steps:
  1. DMS detects the device status change and forwards it to SIT.
  2. SIT raises a situation and publishes the alert through RTDP to subscribed GUI clients.
  3. The technician sees the new alert surface in the situations list with severity and location.
  4. The technician acknowledges the alert, walks to the machine, and clears the physical condition.
  5. The device reports a cleared status, DMS forwards the change, and SIT closes the situation.
- Outcome: The alert is resolved and cleared from the active situations list, with a record of who acknowledged and when.
- Services touched: DMS, SIT, RTDP.

### J1.3 Track a player-specific situation

- Persona: Floor Operator or Host
- Trigger: A condition tied to a specific player at a device needs attention alongside device alerts.
- Steps:
  1. PLSIT scopes situation handling to player-specific conditions and surfaces them for the relevant device.
  2. The alert appears in the GUI next to standard device situations so the operator sees both in one place.
  3. The operator reviews the player context and takes the appropriate floor action.
- Outcome: Player-specific conditions are visible and actionable in the same workflow as device situations.
- Services touched: PLSIT, SIT, RTDP.

### J1.4 Remotely command an EGM or site controller

- Persona: Floor Operator
- Trigger: The operator needs to enable, disable, or reconfigure a device without walking the floor.
- Steps:
  1. The operator selects one or more EGMs or VSCs in the GUI and chooses a command such as enable, disable, or set log verbosity.
  2. CCS receives the request and dispatches the command to the targeted devices over the messaging layer.
  3. CCS tracks each command and returns acknowledgements as devices respond.
  4. For bulk actions, the operator applies a command template across many devices at once.
  5. The operator watches command status update until each device confirms the new state.
- Outcome: The targeted devices reach the commanded state, with per-device command results recorded.
- Services touched: CCS, DMS, RTDP, Intelligen EVO GUI.

### J1.5 Retrieve raw EGM logs for troubleshooting

- Persona: Field or Floor Technician
- Trigger: A device shows intermittent behavior and support needs the raw logs to diagnose it.
- Steps:
  1. The technician requests on-demand log retrieval for a specific EGM.
  2. ADR fetches the raw logs and data files from the device.
  3. The technician reviews the retrieved files to identify the root cause.
- Outcome: The technician has the raw device data needed to diagnose the issue without being physically at the machine.
- Services touched: ADR.

---

## Domain 2: Content and Configuration Management

Services: GSM (Gaming Site Management), CCM (Content and Configuration Management), DCM (Distributed Content Management), SCCM (Site Controller Content Manager).

### J2.1 Register a new site, controller, and EGM

- Persona: Configuration Administrator
- Trigger: A new site is coming online, or new machines are being added to an existing site.
- Steps:
  1. The administrator opens Gaming Site Management and creates the site record.
  2. The administrator registers the VSCs for the site and associates the EGMs, SRUs, vendors, models, and traits.
  3. The administrator assigns licenses and selects the configuration profiles that apply.
  4. GSM distributes the configuration to the physical VSCs over the messaging layer.
  5. GSM publishes entity change events so downstream consumers such as RTDP and DMS pick up the new topology.
- Outcome: The site and its devices are registered, configured, and visible across the platform.
- Services touched: GSM, RTDP, DMS.

### J2.2 Build and publish game content

- Persona: Content Manager
- Trigger: A new theme, paytable, or configuration profile needs to be added to the catalog.
- Steps:
  1. The content manager opens the content catalog and creates or updates the theme, paytable, package, module, or config profile.
  2. CCM offloads package CRC, diff, and checksum operations to its asynchronous worker so the manager is not blocked.
  3. The content manager reviews the computed package details and validates the content set.
  4. The content manager publishes the content so it becomes available for distribution.
- Outcome: Approved content is registered in the catalog and ready to be distributed to devices.
- Services touched: CCM.

### J2.3 Distribute and install content to EGMs

- Persona: Configuration Administrator or Operator
- Trigger: Published content needs to be installed on a group of EGMs, or a game switch is scheduled.
- Steps:
  1. The operator selects the target EGMs and the content to distribute.
  2. DCM orchestrates the multi-step distribution, pushing packages to the EGMs through the VSCs.
  3. DCM runs its per-EGM job engine to install, uninstall, or switch games as planned.
  4. The operator monitors progress as each EGM moves through the distribution steps.
  5. DCM reports completion and flags any device that failed so it can be retried.
- Outcome: The targeted EGMs run the intended content, with per-device install results recorded.
- Services touched: DCM, CCM, GSM.

### J2.4 Provision site controller content

- Persona: Configuration Administrator
- Trigger: Physical VSCs need the correct packages and modules provisioned.
- Steps:
  1. The administrator selects the site controller content to provision.
  2. SCCM prepares the packages and modules for the physical VSCs.
  3. The administrator confirms the controllers have received and applied the provisioned content.
- Outcome: Site controllers are provisioned with the correct content and ready to serve the floor.
- Services touched: SCCM, GSM.

---

## Domain 3: Financials and Accounting

Services: FIN (Financial Service), ACC (Accounting Service), CMS (Cash Management Service).

### J3.1 Ingest meters and close a financial period

- Persona: Finance Analyst
- Trigger: A reporting period is ending and meter data must be captured and validated.
- Steps:
  1. FIN ingests meter reports from the floor and calculates period meters.
  2. FIN runs meter issue detection and flags anomalies for review.
  3. The analyst reviews flagged issues and resolves or annotates them.
  4. FIN creates and validates cashout records and generates the financial summary for the period.
  5. The analyst confirms the period figures, which become the source data for invoicing.
- Outcome: The period is closed with validated meters, cashouts, and a financial summary ready for billing.
- Services touched: FIN.

### J3.2 Generate invoices and payment files

- Persona: Billing and Accounting Clerk
- Trigger: A billing cycle is due and invoices must be produced for sites and companies.
- Steps:
  1. ACC consumes meter and cashout data from FIN for the billing period.
  2. The clerk reviews the company hierarchy and site accounting configurations that drive the charges.
  3. ACC computes invoice charges and commissions using the configured charge formulas, including caps and tiered limits.
  4. The clerk reviews the generated invoices and approves them.
  5. ACC generates EFT payment files in the required format and uploads them for processing, excluding any sites marked EFT-excluded.
- Outcome: Invoices are issued and payment files are produced for the billing period.
- Services touched: ACC, FIN.

### J3.3 Manage physical cash operations

- Persona: Cash and Vault Operator
- Trigger: Cash must be collected from EGMs and moved through the vault and drawers.
- Steps:
  1. The operator records an EGM collection in the cash management workflow.
  2. CMS posts the movement using its double-entry model, from the EGM to the vault.
  3. The operator records vault deposits, cash drawer fills, money machine sweeps, and room transfers as they occur.
  4. The operator reconciles the day's physical cash movements against expected balances.
- Outcome: Physical cash movements are recorded and balanced across EGMs, vault, drawers, and rooms.
- Services touched: CMS.

---

## Domain 4: Player Services and Responsible Gaming

Services: placcnt (Play Accounts), plsessn (Play Sessions), plsmart (Play Smart), plwallet (Play Wallets), PlayRewards, PLLOGS (Play Logs).

### J4.1 Register a player account and enroll in services

- Persona: Player, assisted by Cage or Host Staff
- Trigger: A new player wants to join, or an existing guest wants to create a named account.
- Steps:
  1. The player provides their details and placcnt creates a named or anonymous account.
  2. placcnt sets up credentials and the authentication method, including the progressive token model where used.
  3. The player enrolls in the services they want, such as rewards or cashless wagering.
  4. placcnt emits account activity events to PLLOGS for the audit trail.
- Outcome: The player has an active account enrolled in the chosen services, with activity logged.
- Services touched: placcnt, PLLOGS.

### J4.2 Open and close a play session with responsible-gaming checks

- Persona: Player
- Trigger: The player starts play at a device and ends it when finished.
- Steps:
  1. The player starts a session and plsessn requests player context from placcnt.
  2. plsessn calls plsmart to run limit, self-exclusion, survey, and tutorial checks before play is allowed.
  3. If a limit or exclusion blocks play, the player is informed and the session does not open.
  4. If checks pass, the session opens and the player plays.
  5. When the player stops, plsessn records the session close state and plsmart updates spending and time tracking.
- Outcome: Play occurs only within responsible-gaming rules, and the session open and close are recorded.
- Services touched: plsessn, placcnt, plsmart, PLLOGS.

### J4.3 Set spending limits or self-exclude

- Persona: Player, assisted by Cage or Host Staff
- Trigger: The player wants to set a spending or time limit, or request self-exclusion.
- Steps:
  1. The player requests a limit or self-exclusion through the responsible-gaming workflow.
  2. plsmart records the limit or exclusion and applies it to future session checks.
  3. plsmart logs the change through PLLOGS for the audit trail.
  4. On the next session open, plsessn and plsmart enforce the new setting.
- Outcome: The player's limit or exclusion is active and enforced on subsequent play.
- Services touched: plsmart, plsessn, PLLOGS.

### J4.4 Deposit and withdraw funds for cashless wagering

- Persona: Player
- Trigger: The player wants to add funds to a digital wallet or cash out a balance.
- Steps:
  1. The player initiates a deposit and plwallet records the transaction against the player's wallet.
  2. plwallet pulls player context from placcnt and runs responsible-gaming checks through plsmart on the transaction.
  3. The player's wallet balance updates and is available for cashless wagering.
  4. When the player withdraws, plwallet processes the withdrawal and updates the balance.
- Outcome: The player can move funds in and out of a digital wallet for cashless play, within responsible-gaming rules.
- Services touched: plwallet, placcnt, plsmart.

### J4.5 Earn and redeem loyalty rewards

- Persona: Player
- Trigger: The player plays carded and later wants to redeem earned rewards.
- Steps:
  1. As the player plays, PlayRewards accrues loyalty based on play.
  2. The player views their rewards balance and available offers.
  3. The player redeems rewards for the offered benefit.
  4. PlayRewards updates the balance and records the redemption.
- Outcome: The player earns loyalty from play and can redeem it for benefits.
- Services touched: PlayRewards, placcnt.

---

## Domain 5: Reporting and Data

Services: DLR (Data Lake Reporting), SCHS (Site Controller Host Service), CDS (Custom Data Service), Spin Level Data.

### J5.1 Generate and deliver a jurisdiction or custom report

- Persona: Reporting User or Regulator
- Trigger: A compliance deadline or operational need requires a report.
- Steps:
  1. The user selects a jurisdiction report or builds a custom report in the data workspace.
  2. DLR runs parameterized queries against the data lake through Athena.
  3. DLR formats the results into a CSV or spreadsheet file and stores it.
  4. DLR delivers a download link to the user, and can run the report on a schedule for repeat delivery.
  5. The user downloads the file and uses it for operations or compliance submission.
- Outcome: The requested report is generated and delivered, on demand or on a schedule.
- Services touched: DLR.

### J5.2 Run site controller reports

- Persona: Reporting User serving a VSC
- Trigger: A site controller operator needs reports such as cash flow, validations, EGM events, or commissions.
- Steps:
  1. The operator selects the site controller report and parameters.
  2. SCHS generates the report and renders it using localized templates in English or French.
  3. The operator reviews the rendered output.
- Outcome: The site controller operator has the operational report they requested in their language.
- Services touched: SCHS.

### J5.3 Import and export data to third-party systems

- Persona: Integration Administrator
- Trigger: Data must flow to or from an external retailer, financial, or regulatory system.
- Steps:
  1. The administrator configures an import to ingest data from a file, or a scheduled export to send data out.
  2. CDS processes the import file and loads the data, or runs the scheduled export on its timetable.
  3. The administrator verifies the integration ran and the data moved as expected.
- Outcome: Data is reliably exchanged with external systems through configured imports and exports.
- Services touched: CDS.

### J5.4 Access daily Spin Level Data from the Data Lake

- Persona: Reporting User (Lottery employee or Gaming Operator)
- Trigger: The user needs detailed spin, cash-in, and cash-out events for a calendar day.
- Steps:
  1. At the end of each calendar day, the platform compiles Spin Level Data for all EGMs and exports it to the Data Lake.
  2. The host purges its copy after a successful export, keeping storage lean.
  3. The user accesses the day's Spin Level Data in the Data Lake within the configured retention window.
  4. The user works with the gameplay, cash-in, and cash-out events for analysis.
- Outcome: The user has access to detailed daily gameplay data in the Data Lake for the retained period.
- Services touched: Spin Level Data, data lake.

---

## Domain 6: Jackpots

Service: JKPT (Jackpot Service).

### J6.1 Configure a jackpot controller and tiers

- Persona: Jackpot Administrator
- Trigger: A new jackpot program is being set up, or an existing one needs tuning.
- Steps:
  1. The administrator creates a jackpot controller in the jackpot workflow.
  2. The administrator defines the tiers and the contribution configuration for each tier.
  3. The administrator associates the eligible devices with the controller.
  4. The administrator activates the configuration.
- Outcome: The jackpot program is configured and contributing on the eligible devices.
- Services touched: JKPT, GSM.

### J6.2 Handle a jackpot award

- Persona: Floor Operator or Attendant
- Trigger: A jackpot hits and an award must be processed.
- Steps:
  1. JKPT registers the award when the jackpot is triggered.
  2. The attendant reviews the award details in the GUI.
  3. The attendant processes the award according to site procedure.
  4. JKPT records the award and resets or reseeds the tier per its configuration.
- Outcome: The jackpot award is processed and recorded, and the tier returns to its configured state.
- Services touched: JKPT.

---

## Domain 7: Platform Administration and Access

Services: UMS (User Management Service), EIS (EGM Information Service), GHS (General Hosting Service), Intelligen EVO GUI.

### J7.1 Sign in with single sign-on and get role-based access

- Persona: Any platform user
- Trigger: A user opens the Intelligen EVO GUI at the start of their work.
- Steps:
  1. The user authenticates, using SAML single sign-on where configured.
  2. UMS validates the identity and issues a JWT carrying the user's roles, domains, and permissions.
  3. The GUI uses the token to show only the features and data the user is authorized for.
  4. Behind the scenes, EIS aggregates data from downstream services into the shapes the GUI needs, enforcing authorization and caching.
- Outcome: The user is signed in and sees a role-appropriate view of the platform.
- Services touched: UMS, EIS, Intelligen EVO GUI.

### J7.2 Administer users, roles, and permissions

- Persona: Security Administrator
- Trigger: A new employee needs access, or an existing user's access must change.
- Steps:
  1. The administrator opens user management and creates or edits the user.
  2. The administrator assigns roles, domains, and permissions appropriate to the user's job.
  3. Where an approval workflow applies, the change is routed for approval before it takes effect.
  4. UMS applies the change so the user's next sign-in reflects the new access.
- Outcome: The user has the correct, approved access across all platform services.
- Services touched: UMS.

### J7.3 Personalize and persist the GUI workspace

- Persona: Floor Operator or any power user
- Trigger: The user arranges their monitoring and data views to suit their work and wants them preserved.
- Steps:
  1. The user arranges panels, tabs, and layouts in the GUI.
  2. The GUI stores the editor layout state through GHS document and collection storage.
  3. On the next sign-in, the GUI restores the saved layout from GHS.
- Outcome: The user's personalized workspace persists across sessions.
- Services touched: GHS, Intelligen EVO GUI.

---

## Traceability

- Service definitions and acronyms: Business Glossary, Evo Platform service catalog.
- Spin Level Data journey: Spin Level Data product brief and requirements.
- Cross-service real-time flow (DMS to SIT to RTDP, GSM entity events): Evo Platform protocol and integration patterns in the Business Glossary.

## Open Questions and Next Steps

- Confirm whether any domains should be split further into service-level journeys for the next pass.
- Validate persona names against the team's standard role taxonomy.
- Confirm localization and jurisdiction scope for the reporting journeys.
- Decide whether to add Mermaid flowcharts alongside the narrative journeys in a later revision.
