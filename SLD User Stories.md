# Spin Level Data — User Stories

## Summary
User stories describing how Lottery employees and Gaming Operators will access daily Spin Level Data (SLD) exports from IntelligenEVO and how administrators will configure retention and deployment support.

---

**Epic: Daily Generation & Delivery**

- **US-001 — Automatic daily SLD export creation:**
  - As an `IntelligenEVO` user (Lottery employee or Gaming Operator), I want the system to automatically compile and export spin level data for all EGMs at the end of each calendar day so the day's gameplay events are available in the Data Lake without manual intervention.
  - Acceptance Criteria:
    - System exports all EGM spin level data automatically at the end of each calendar day.
    - Export is delivered to the IntelligenEVO Data Lake.
    - The export includes gameplay, cash-in, and cash-out events for the calendar day.

- **US-002 — Access spin level data through the Data Lake:**
  - As an `IntelligenEVO` user, I want access to the spin level data through the IntelligenEVO Data Lake so I can load it into an external data warehouse for analysis.
  - Acceptance Criteria:
    - Exported SLD is accessible from the configured IntelligenEVO Data Lake location.
    - Access is available to authorized Lottery employees and Gaming Operators.
    - The Data Lake location contains the daily SLD exports for the relevant calendar day.

**Epic: Storage & Retention Management**

- **US-004 — SLD stored only in the Data Lake:**
  - As a Product Manager, I want spin level data to be stored only in the Data Lake and not retained on host services so host storage and performance issues are minimized.
  - Acceptance Criteria:
    - Spin Level Data is exported to the Data Lake.
    - The host does not retain the daily SLD after export.
    - Host-side storage usage is released once SLD is shared to the Data Lake.

- **US-005 — Purge SLD from host after export:**
  - As a Product Manager, I want IntelligenEVO to purge spin level data from the host once it has been shared to the Data Lake so the host remains performant and storage-efficient.
  - Acceptance Criteria:
    - Host-side SLD is purged immediately after successful export to the Data Lake.
    - There is no residual host storage of exported SLD.
    - Purge activity is logged for audit purposes.

- **US-006 — Configurable retention period for Data Lake storage:**
  - As an administrator, I want the SLD Data Lake retention period to be configurable at deployment with a default of 7 days so retention matches deployment and customer agreement requirements.
  - Acceptance Criteria:
    - Retention period is configurable at deployment time.
    - Default retention period is 7 rolling calendar days, inclusive of creation date.
    - Retention changes require a formal change request after deployment.

- **US-007 — Enforce retention purge after configured period:**
  - As an administrator, I want spin level data to be purged from the Data Lake after the configured retention period so expired data does not consume storage.
  - Acceptance Criteria:
    - Data older than the configured retention period is deleted automatically.
    - The retention period includes the creation date as the first day.
    - Once purged, the data cannot be recovered or regenerated.

**Epic: Deployment Configuration & Enablement**

- **US-008 — Enable/disable Spin Level Data feature:**
  - As an administrator, I want a deployment-level flag to enable or disable Spin Level Data exports so I can control whether the deployment supports SLD exports to the IntelligenEVO Data Lake.
  - Acceptance Criteria:
    - A deployment configuration flag exists for enabling/disabling SLD exports.
    - Default deployment setting is enabled.
    - When disabled, SLD export generation and Data Lake delivery are inactive.

- **US-009 — Retention applies only to non-audit data:**
  - As a product owner, I want the configured 7-day retention period to apply only to spin level data, not required for other audit or EGM event functionality so audit-critical data can be preserved separately.
  - Acceptance Criteria:
    - Retention policy applies to regular SLD exported to the Data Lake.
    - Data needed for other functionality (such as EGM event audits) is exempt from purge rules when required.
    - The system documents which SLD data is subject to the 7-day retention period.

---

## Notes / Implementation Considerations

- Daily SLD export is produced at the end of each calendar day for all EGMs.
- The export is delivered to the IntelligenEVO Data Lake and not retained on the host.
- Data Lake retention is configurable at deployment, with a default of 7 rolling calendar days.
- The host must purge SLD once it has been shared to the Data Lake.
- Retention changes after deployment require a formal change request.

File created from: `SLD Product Brief` (Spin Level Data)
