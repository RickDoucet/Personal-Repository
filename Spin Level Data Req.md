# Spin Level Data — Requirements

## Overview
Provide detailed functional and non-functional requirements to build Spin Level Data (SLD) functionality in IntelligenEVO.

This feature enables Lottery employees and Gaming Operators to access daily Spin Level Data of gameplay, cash in, and cash out events in the IntelligenEVOData Lake.

## Scope
Include:
- Automatic daily SLD export generation for all EGMs and delivery to the IntelligenEVO Data Lake.
- Storage in the IntelligenEVO Data Lake with a configurable retention period (default 7 rolling calendar days).
- Host-side purge of SLD after successful export to the Data Lake.
- Deployment-level configuration for enablement and retention settings (configured at deployment; changes require a formal change request).

Exclude:
- Recovery or regeneration of data deleted after the configured retention period.
- User generated or requested files or report functionality.

## Functional Requirements

### 1. Daily Export Generation
1.1 The system must automatically compile and export daily Spin Level Data for all EGMs at the end of each calendar day and deliver the export to the IntelligenEVO Data Lake.
1.2 The export must contain gameplay, cash in, and cash out events for the calendar day.
1.3 The export must be available from the Data Lake by the next business morning.

### 2. Storage and Retention
2.1 Generated SLD data must be stored in the IntelligenEVO Data Lake as the authoritative storage location.
2.2 The system must retain generated SLD data in the Data Lake for the configured retention period; the retention period is configured at deployment and defaults to 7 rolling calendar days, inclusive of the creation date.
2.3 After the configured retention period expires, the Data Lake must automatically delete expired data.
2.4 Data deleted by retention policy must not be recoverable or regenerable through standard system functionality.
2.5 The IntelligenEVO host must purge local/host copies of SLD immediately after successful export to the Data Lake; host-side SLD must not be retained beyond the export/purge cycle.
2.6 The system shall ensure that all generated Spin Level Data (SLD) is stored within the Data Lake and made available for consumption by the Spin Level Data Vertical within the IntelligenEVO Data Lake.
2.7 Generated SLD data will remain independent and separate from existing customer reports developed on the Data Lake.

### 3. Configuration
3.1 The system must provide a deployment-level configuration flag to enable or disable Spin Level Data exports; the default deployment setting is Enabled.
3.2 When SLD exports are disabled, the export generation and delivery features must be inactive and hidden from users.


## Data Requirements
### 4 Spin Level Data 
4.1 The Spin Level Data must present all date time fields in the following format: ‘YYYY-MM-DD-HH.MM.SS.mmmmmm’

4.2 The Spin Level Data  must present all date time fields in the same time zone as EVO System time.

4.3 The Spin Level Data must support the following data types:

  - Game Play

    Generated for each game cycle on a EGM.  Depending on the outcome of the game cycle, record may be immediately followed by additional records of g2sWin_Level Item or g2sGamePlay SourceRef.

 - g2sWin_Level Item

    Additional game cycle outcome information associated with Game Play record.

 - g2sGamePlay SourceRef

    Additional game cycle outcome information associated with Game Play record.

 - Cash In
 - Cash Out

4.4 Game Play records must include the following information:

1) VLT ID
    
2) Creation Date Time

    The date and time on the EVO System when the event arrives and is logged.

3) Log Sequence
4) Device ID
5) Transaction ID
    - The transaction ID of the event
6) Game Time
    - The date and time on the EGM when the game cycle completes.
7) Play State
8) Play Result
    - Gameplay event: G2S_gameWon or G2S_gameLost
9) Denom
    - The base denomination of the game.  (Currency, 2 dp)
10) Initial Wager
11) Final Wager
12) Initial Win
13) Secondary Played
14) Secondary Wager
15) Secondary Win
16) Final Win
17) Paytable ID
    - The unique identifier of the Game (Theme & Payout Percentage)
18) Theme ID
19) Initial Start Time
    - The date and time on the EGM when the game cycle begins.
20) Initial Player Cashable Amount
21) Initial Player Non Cashable Amount
22) Initial Player Promo Amount
23) Player Cashable Amount
24) Player Non Cashable Amount
25) Player Promo Amount
26) Player Session ID
27) Player ID
28) EGM Description

      
4.5 g2sWin Level Item records must include the following information:
1) Win Level Index
2) Win Level Combo
3) Progressive Allowed

4.6 g2sGamePlay SourceRef records must include the following information:
1) Device Class
2) Device ID
3) Transaction ID
    - The transaction ID of the event.
4) Log Sequence
5) Cashable Amount
6) Promo Amount
7) Non Cashable Amount

4.7 Cash In records must include the following information:
1) VLT ID
2) Creation Date Time
    - The date and time on the EVO System when the event arrives and is logged.
3) Device ID
4) Transaction ID 
    - The transaction ID of the event
5) Currency ID
6) Denom ID
7) Base Cashable Amount
8) Note Date Time
    - The date and time on the EGM when the acceptor device accepts the bank note.
9) EGM Description

4.8 Cash Out records must include the following information:
1) VLT ID
2) Creation Date Time
    - The date and time on the EVO System when the event arrives and is logged.
3) Device ID
4) Transaction ID
    - The transaction ID of the event
5) Validation ID
6) Voucher Sequence
7) Voucher Amount
8) Transfer Date Time
    - The date and time on the EGM when the cashout ticket is printed.
9) EGM Description

### 5 IntelligenEVO Spin Level Data Vertical
5.1 The IntelligenEVO Spin Level Data Vertical currently supports the following information:

| **DV Column Name (technical)** | **EVO Mapping** | **Description** |
|----|----|----|
| egm_id | EGM_EVENT_TRANSACTION_LOG.EGM_ID | EGM Source Identifier |
| site_id | EGM_EVENT_TRANSACTION_LOG.SITE_ID | Venue Source Identifier |
| game_id | EGM_EVENT_TRANSACTION_LOG.GAME_ID | Game Source Identifier |
| event_generation_datetime | EGM_EVENT_TRANSACTION_LOG.EVENT_GENERATION_DATETIME | Date and Time the Event was generated |
| egm_occupied | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(210) | EGM Occupied (0,1)\* |
| number_of_primary_games_started | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(305) | Primary Game Started (0,1)\* |
| wager_changed | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(306) | Bet Changed (0,1)\* |
| primary_game_ended | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(307) | Primary Game Ended (0,1)\* |
| secondary_game_started | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(311) | Secondary Game Started (0,1)\* |
| secondary_game_ended | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(312) | Secondary Game Ended (0,1)\* |
| game_result | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(313) | Game Result (0,1)\* |
| game_ended | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(314) | Game Ended (0,1)\* |
| game_idle | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(315) | Game Idle (0,1)\* |
| coins_accepted | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(570) | Coin Accepted (0,1)\* |
| note_stacked | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(625) | Bill Accepted (0,1)\* |
| bonus_award_paid | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(883) | Bonus Award Paid (0,1)\* |
| partial_bonus_award | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(904) | Partial Bonus Award (0,1)\* |
| session_started | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(921) | Session Started (0,1)\* |
| session_ended | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(924) | Session Ended (0,1)\* |
| voucher_issued | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(987) | Voucher Issued (0,1)\* |
| voucher_redeemed | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(992) | Voucher Redeemend (0,1)\* |
| wat_device_transfer_completed | EGM_EVENT_TYPE.EGM_EVENT_TYPE_ID(1026) | Wide Area Transfer Completed (0,1)\* |
| site_descr | SITE.DESCR | Venue Name |
| site_version_number | SITE.VERSION_NUMBER | Venue Version Number |
| site_audit_datetime | SITE.AUDIT_DATETIME | Venue Audit Datetime |
| site_audit_user_id | SITE.AUDIT_USER_ID | Venue Audit User Source Identifier |
| site_deleted_flag | SITE.DELETED_FLAG | Venue Deleted Flag |
| site_entity_status | SITE.ENTITY_STATUS_TYPE_ID | Venue Status |
| site_type_key_code | SITE.SITE_TYPE_ID | Venue Type Key Code |
| site_time_zone_type_key_code | SITE.TIME_ZONE_TYPE_ID | Venue Time Zone |
| site_ref_number | SITE.REF_NUMBER | Venue Number |
| site_cross_valid_chain_flag | SITE.CROSS_VALID_CHAIN_FLAG | Venue Cross Validation Enabled In Ownership Structure Flag |
| site_cross_valid_all_flag | SITE.CROSS_VALID_ALL_FLAG | Venue Cross Validation Enabled Flag |
| site_revision_number | SITE.REVISION_NUMBER | Venue Revision Number |
| site_domain_id | SITE.DOMAIN_ID | Venue Domain Source Identifier |
| site_tito_enabled_flag | SITE.TITO_ENABLED_FLAG | Venue TITO Enabled Flag |
| site_invoice_enabled_flag | SITE.INVOICE_ENABLED_FLAG | Venue Invoice Enabled Flag |
| egm_descr | EGM.DESCR | EGM Name |
| egm_position_number | EGM.POSITION_NUMBER | EGM Position Number |
| egm_ip_addr | EGM.IP_ADDR | EGM IP Address |
| egm_comm_idn | EGM.COMM_IDN | EGM Terminal ID |
| egm_serial_number | EGM.SERIAL_NUMBER | EGM Serial Number |
| egm_asset_ref_number | EGM.ASSET_REF_NUMBER | EGM Asset ID |
| egm_install_datetime | EGM.INSTALL_DATE | EGM Install Date |
| egm_deinstall_datetime | EGM.DEINSTALL_DATE | EGM Deinstall Date |
| egm_purchase_datetime | EGM.PURCHASE_DATE | EGM Purchase Date |
| egm_purchase_amount | EGM.PURCHASE_AMOUNT | EGM Purchase Amount |
| egm_revision_number | EGM.REVISION_NUMBER | EGM Revision Number |
| egm_gaming_hours_profile_id | EGM.EGM_GAMING_HOURS_PROFILE_ID | Operating Hours Profile Source Identifier |
| egm_invoice_enabled_flag | EGM.INVOICE_ENABLED_FLAG | EGM Invoice Enabled Flag |
| game_ccm_theme_name | GAME.CCM_THEME_NAME | Game Name |
| game_ccm_paytable_name | GAME.CCM_PAYTABLE_NAME | Game Paytable Game |
| game_ccm_game_set_id | GAME.CCM_GAME_SET_ID | Game Set Source Identifier |
| game_position | GAME.POSITION | Game Position |
| game_platform_type_key_code | GAME.PLATFORM_TYPE_KEY_CODE | Game Platform Type Key Code |
| game_version | GAME.VERSION | Game Version |
| game_version_number | GAME.VERSION_NUMBER | Game Version Number |
| game_audit_datetime | GAME.AUDIT_DATETIME | Game Audit Datetime |
| game_audit_user_id | GAME.AUDIT_USER_ID | Game Audit User Source Identifier |



## Non-functional Requirements

### 6. Performance
6.1 Export generation and retention must not negatively impact host system performance.

### 7. Reliability
7.1 Spin Level Data must be generated and sent to the Data Lake consistently at the end of calendar day.

7.2 Retention cleanup must run reliably and delete expired data on schedule.



## Business Rules

- The configured retention period begins on the creation date of each generated file; default is 7 rolling calendar days.
- Deleted exports cannot be recovered or regenerated after the retention period expires.
- Export notifications must only be sent when the daily automatic export completes successfully and is delivered to the Data Lake.


## Acceptance Criteria

- A scheduled end-of-day job generates SLD exports for all EGMs and stores them in a shared location.
- The system retains generated files in the Data Lake for the configured retention period (default 7 rolling calendar days) and deletes expired files automatically.
- The SLD data functionality  feature can be toggled on or off through configuration.

Source: SLD Product Brief.md
