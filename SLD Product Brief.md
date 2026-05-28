# Spin Level Data  
## Background and Product Vision
Existing customers have requested the ability to export or access Spin Level data (gameplay, cash in, cash out events) from IGT systems so it may be loaded into an external data warehouse or used for analysis. The customers will use this data to analyze Game and Electronic Gaming Machine (EGM) performance.

## Product Objectives
The objective of the Spin Level Data export is to:
- Provide the IntelligenEVO User with a specific set of data from every EGM related to spin data that will be processed by IntelligenEVO and delivered to the Data Lake. 
- Provide spin level data daily for all EGMs for a single calendar day.
- Provide the spin level data at the end of each calendar day in the IntelligenEVO Data Lake.
- Provide access to the spin level data to the IntelligenEVO Users through the Spin Level Data Vertical within the IntelligenEVO Data Lake.

## Product Functionality
This Spin Level Data will be accessed by Lottery employees or Gaming Operators; the product must support the following functionality:
1) IntelligenEVO must compile the Spin Level Data and exported the Spin Level Data to the data lake.
     - Spin Level data must only be stored on the IntelligenEVO Data Lake to alleviate storage and performance issues on the host services.
2) IntelligenEVO will automatically send Spin Level Data for all EGMs to the Data Lake at the end of the calendar day. 
3) Spin Level Data must only be stored in the Data Lake for the configured Retention period before it is purged.
4) The IntelligenEVO host must purge the Spin Level Data once it is shared to the Data Lake.
5) The Spin level rentention period configuration is a configurable period of a number of rolling calendars days from the date of creation before it is purged.
   - Default value is 7 days
   - The 7-day configured retention period only applies to Spin Level data that is not needed for other functionality available to the User (such as EGM event audits).
    - The 7-day period will include the creation date as the first day of the period.
     - After the 7-day period the Spin Level data must be erased from storage to maintain system performance and storage capacity. 
        - Users will not be able to request recovery or regeneration of the data that was erased due to the 7 day period.
        - Any future product needs that may use the Spin Level Data in the Data Lake will be subject to and limited by the configured data retention period. 
6) IntelligenEVO must support the following Spin Level Data configurations:
    - Spin Level Data Flag (Enable/Disabled)
        - Spin Level Data Flag controls whether the deployment will support the creation and access to Spin Level Data.
            - Default: Enabled
    - Retention Period ("x" number of days) 
        - Retention Period defines the number of rolling calendar days after the Spin Level data creation date that the Spin Level Data will be deleted from the Data Lake.
            - Default value will be 7 days.
                - Retention Period for product will be 7 days.
                    - IGT and a Customer may agree to additional storage fees if the customer wishes to make the period greater than 7 days. 
            - Retention Period must only be available on the host and will require a formal change request from the customer for IGT to adjust the period.
            - PRetention Period must be configured at the time of deployment.
    