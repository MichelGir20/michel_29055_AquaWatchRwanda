# AquaWatch Rwanda: Rural Water Point Reliability and Maintenance Prioritization System

**Student:** Michel Girinshuti | **Student ID:** 29055
**Course:** INSY 8311 – Database Development with PL/SQL | **Instructor:** Eric Maniraguha
**Academic year:** 2026–2027, Semester I

## Problem statement
Rural water points in Rwanda can become partly or fully non-functional, leaving communities without reliable water. Nationally, 87% of rural households use an improved drinking-water source, yet non-functioning sources are the most common reason households give for not using their nearest source (NISR, 2025). Weak operation and maintenance is cited as a main cause of low operational rates in rural piped systems (JICA, 2025). Existing national tools collect and map water-system data, but public documentation does not describe a transparent, rule-based way to rank water points for maintenance with an audit trail. AquaWatch fills that gap at prototype level.

## Objectives
**Main objective:** Design and implement an Oracle database with PL/SQL that organizes water-point data and supports maintenance prioritization for the sampled districts in the Water For People Rwanda 2021 dataset.

**Specific objectives**
1. Normalize the WPDx survey data into 3NF tables and load it with documented cleaning rules.
2. Record inspections and maintenance work through PL/SQL procedures with exception handling.
3. Calculate functionality rates and maintenance-priority classes with functions and a package.
4. Enforce business rules and audit changes with triggers.
5. Deliver a Streamlit dashboard connected live to the Oracle database.

## Data source
Water Point Data Exchange (2021). *Water For People Rwanda 2021*: 2,057 valid records, sampled Water For People districts, reports from January 2015 to June 2021.
Link: https://catalog.waterpointdata.org/datasets/wfp-rwanda-621f8339/index.html
Licence: WPDx states that data providers license data under CC BY 4.0 (https://www.waterpointdata.org/?p=1507); attribution is given here. To be re-confirmed when the file is downloaded.
**Limitations:** the data is a snapshot of sampled districts, not all Rwandan water points, and it is not current national data. Records with missing functionality status are stored as "Unknown", not guessed.

## Innovation
**Option:** User interface. **Tool:** Python Streamlit with python-oracledb. The app shows district reliability, maps and priority lists, and records inspections by calling PL/SQL procedures. All rules stay in PL/SQL. Documentation will be in `innovation/`.

## Planned tables (preliminary)
DISTRICTS, SECTORS, WATER_POINT_TYPES, WATER_POINTS, FUNCTIONALITY_STATUSES, STATUS_OBSERVATIONS (historical survey data), INSPECTIONS, MAINTENANCE_RECORDS, TECHNICIANS, APP_USERS, AUDIT_LOG. The final design follows data inspection and 3NF normalization in Phase II.

## Planned PL/SQL features
| Feature | Where it is used |
|---|---|
| Procedure `record_inspection` | Saves an inspection and updates the water point's current status |
| Procedure `open_maintenance` / `close_maintenance` | Creates and completes repair work orders |
| Function `fn_priority_score` | Scores a water point from status, days since inspection, repeat failures and technology |
| Function `fn_functionality_rate` | Percentage of functional water points per district |
| Explicit cursor | Loops through overdue water points to build the maintenance alert list |
| Package `PKG_AQUAWATCH` | Groups the procedures and functions above |
| Trigger (business rule) | Blocks maintenance work on a decommissioned water point |
| Trigger (integrity) | Stops a work order closing before its opening date |
| Audit triggers | Write every insert, update and delete on key tables to AUDIT_LOG |
| Restriction trigger | Blocks changes to master data in restricted periods and logs the denied attempt |
| Custom exceptions | `e_decommissioned_point`, `e_invalid_status`, with clear error messages |

## Repository structure
`docs/research/` research log · `docs/proposal/` proposal · `docs/presentation/` slides · `data/` dataset · `innovation/` dashboard

## References
Japan International Cooperation Agency. (2025). *JICA project brief note: The project for rural water supply services and infrastructure management development in Rwanda*. https://www.jica.go.jp/oda/project/1904443/__icsFiles/afieldfile/2025/12/02/briefnote_02_en_1.pdf

Murray, A. L., Stone, G., Yang, A. R., Lawrence, N. F., Matthews, H., & Kayser, G. L. (2024). Rural water point functionality estimates and associations: Evidence from nine countries in sub-Saharan Africa and South Asia. *Water Resources Research, 60*(2), Article e2023WR034679. https://doi.org/10.1029/2023WR034679

National Institute of Statistics of Rwanda. (2025). *EICV7 thematic report: Utilities and amenities 2023/24*. https://www.statistics.gov.rw/integrated-household-living-conditions-survey-7-eicv-7/eicv7-thematic-report-utilities-and-amenities-202324

Oracle. (2024). *Oracle Database PL/SQL language reference* (21c). https://docs.oracle.com/en/database/oracle/oracle-database/21/lnpls/

Rural Water and Sanitation Services Department, Water and Sanitation Corporation. (n.d.). *Data collection by QGIS/QField for O&M work of rural water supply systems in Rwanda*. QField documentation. https://qfield.org/docs/case-studies/rwanda-rural-water.html

Streamlit. (n.d.). *Streamlit documentation*. https://docs.streamlit.io/

Water Point Data Exchange. (2021). *Water For People Rwanda 2021* [Data set]. https://catalog.waterpointdata.org/datasets/wfp-rwanda-621f8339/index.html

Whaley, L., & Cleaver, F. (2017). Can 'functionality' save the community management model of rural water supply? *Water Resources and Rural Development, 9*, 56–66. https://doi.org/10.1016/j.wrr.2017.04.001
