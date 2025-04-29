# Power BI Model & Report Documentation

*Generated on: 2025-04-29 12:33:54*

## Table of Contents

- [Overview](#overview)
- [Data Model](#data-model)
  - [Relationships](#relationships)
  - [Tables](#tables)
    - [DimDate](#table-dimdate)
    - [Hours_nd_Percentage](#table-hours_nd_percentage)
    - [lkp_Code](#table-lkp_code)
    - [lkp_fltr_Employee](#table-lkp_fltr_employee)
    - [lkp_Project](#table-lkp_project)
    - [lkp_Unit](#table-lkp_unit)
    - [tbl_Project](#table-tbl_project)
    - [vw_missing_timesheet](#table-vw_missing_timesheet)
    - [vw_Timesheet](#table-vw_timesheet)
- [Report Structure](#report-structure)
  - [Report Level Filters](#report-level-filters)
  - [Report Pages](#report-pages)
    - [Customer Analysis](#page-customer-analysis)
    - [Project Details](#page-project-details)

---

## <a name="overview"></a>Overview

The Power BI data model appears to be designed for a business domain focused on project management and time tracking, likely within a professional services or project-based organization. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a strong emphasis on capturing and analyzing employee time allocation to various projects, as well as identifying gaps in timesheet submissions. The inclusion of lookup tables like 'lkp_Project', 'lkp_Unit', and 'lkp_Code' indicates a structured approach to categorizing and standardizing project-related data, units of work, and timesheet codes, which are essential for accurate reporting and analysis.

The relationships established between these tables facilitate comprehensive reporting capabilities, allowing stakeholders to analyze time spent on projects, monitor employee productivity, and identify any discrepancies in timesheet submissions. The 'DimDate' table further enhances the model by enabling time-based analysis, such as tracking performance over specific periods. Overall, this data model is likely aimed at improving operational efficiency, ensuring accountability in project execution, and providing insights that drive strategic decision-making within the organization.

---

## <a name="data-model"></a>Data Model

### <a name="relationships"></a>Relationships

The following relationships link the tables:

* `vw_missing_timesheet`.[`Project`] -> `lkp_Project`.[`Project`]
* `vw_missing_timesheet`.[`ShortDate`] -> `DimDate`.[`ShortDate`]
* `vw_missing_timesheet`.[`SubUnit`] -> `lkp_Unit`.[`Organisatorische eenheid`]
* `vw_missing_timesheet`.[`TimesheetCode`] -> `lkp_Code`.[`Code`]
* `vw_Timesheet`.[`Project`] -> `lkp_Project`.[`Project`]
* `vw_Timesheet`.[`ProjectCode`] -> `tbl_Project`.[`Project`] (Filter: bothDirections)
* `vw_Timesheet`.[`TimesheetCode`] -> `lkp_Code`.[`Code`]
* `vw_Timesheet`.[`TimesheetDate`] -> `DimDate`.[`ShortDate`]
* `vw_Timesheet`.[`Unit`] -> `lkp_Unit`.[`Organisatorische eenheid`]
* `vw_Timesheet`.[`ContractEndDate`] -> `DimDate`.[`ShortDate`] (**Inactive**)
* `vw_Timesheet`.[`ContractStartDate`] -> `DimDate`.[`ShortDate`] (**Inactive**)

### <a name="tables"></a>Tables

#### <a name="table-dimdate"></a>Table: `DimDate`

The 'DimDate' table serves as a comprehensive date dimension, providing essential temporal attributes that enable businesses to analyze and report on data trends over time. By offering detailed breakdowns of dates, including day, week, month, and quarter information, this table supports effective time-based analysis and enhances the accuracy of time-related metrics in reporting and dashboards.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting within the dataset. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient data retrieval and analysis in time-based reporting and analytics. |
| `DayName` | `string` | The 'DayName' column in the 'DimDate' table stores the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | Column Description: The 'DayNumberOfMonth' column represents the numerical day of the month (ranging from 1 to 31) for each date entry in the 'DimDate' table, facilitating date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | Column Description: Represents the day of the week as an integer, where values range from 1 (Sunday) to 7 (Saturday), facilitating date-related analyses and reporting in the DimDate table. |
| `DayNumberOfYear` | `int64` | Column Description: The 'DayNumberOfYear' column represents the sequential day of the year (ranging from 1 to 365 or 366) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `MonthName` | `string` | The 'MonthName' column in the 'DimDate' table stores the full names of the months, facilitating easy reference and reporting for date-related analyses. |
| `MonthNumberOfYear` | `int64` | Column Description: Represents the numerical value of the month (1-12) corresponding to each date in the DimDate table, facilitating time-based analysis and reporting. |
| `ShortDate` | `dateTime` | The 'ShortDate' column in the 'DimDate' table represents a simplified date format, capturing the specific date without time details to facilitate efficient date-based analysis and reporting. |
| `WeekNumberOfYear` | `int64` | Column Description: The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Column Description: Indicates whether the date corresponds to the current calendar year, with values typically set to 'Yes' or 'No' for easy filtering and analysis in time-based reporting.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `CurrentYearFlag` in a data model, typically within a tool like Power BI. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The `CurrentYearFlag` column is designed to indicate whether a date in the `DimDate` table falls within the current year or not.

2. **How it Works**:
   - The expression uses the `YEAR` function to extract the year from the date in the `ShortDate` column of the `DimDate` table.
   - It then compares this year to the current year, which is obtained using the `YEAR(TODAY())` function. `TODAY()` gives the current date, and `YEAR()` extracts the year from that date.

3. **Output**:
   - If the year from the `ShortDate` is less than or equal to the current year, the expression returns `1`. This means that the date is either in the current year or a previous year.
   - If the year from the `ShortDate` is greater than the current year, it returns `0`, indicating that the date is in a future year.

4. **Result**: 
   - The `CurrentYearFlag` column will have a value of `1` for all dates that are in the current year or any previous year, and a value of `0` for dates that are in future years. 

In summary, this calculated column helps in identifying which dates are relevant for analysis based on whether they are in the current year or not, allowing businesses to filter or segment their data accordingly.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table captures and categorizes various metrics related to employee hours worked and their corresponding percentage contributions to overall productivity. This table aids in analyzing workforce efficiency and resource allocation, enabling informed decision-making for operational improvements.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column contains string representations of hours worked alongside their corresponding percentage contributions, facilitating the analysis of time allocation and performance metrics within the dataset. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and their corresponding percentage contributions, facilitating data enrichment and analysis within the 'Hours_nd_Percentage' table. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column categorizes the sequence of hours and percentage values, facilitating the organization and analysis of time allocation and performance metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes and organizes various codes used across the business, facilitating data consistency and integrity. It includes attributes for code identification, qualification criteria, and sorting order, enabling efficient data retrieval and analysis in reporting and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers used to categorize and enrich data within the lookup table, facilitating efficient data retrieval and integration. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numerical identifier to categorize data for enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table specifies the order in which codes should be displayed or processed, facilitating organized data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential details such as EmployeeId and EmployeeName to facilitate filtering and reporting across various business analytics and performance metrics in Power BI. This table enhances data integrity and consistency by standardizing employee information for use in dashboards and reports.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column stores unique identifiers for employees, facilitating data enrichment and integration within the employee lookup table. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, facilitating the enrichment of data by providing identifiable references for each individual in the employee lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, providing essential details such as project identifiers, qualification metrics, billing amounts, and associated groups, enabling effective project management and financial analysis within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the total billing amount associated with each project, facilitating financial analysis and reporting. |
| `Group` | `string` | The 'Group' column in the 'lkp_Project' table categorizes projects into distinct classifications for enhanced data organization and retrieval. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the lkp_Project table. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, serving as a numerical identifier to categorize projects based on their eligibility or compliance criteria. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their characteristics such as billable status and external affiliations. This table is essential for analyzing resource allocation and financial performance across different units, facilitating informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column (int64) in the 'lkp_Unit' table indicates the department responsible for billable activities, facilitating accurate tracking and reporting of revenue-generating units. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column identifies the specific organizational unit within the company, facilitating data enrichment and analysis related to departmental structures and functions. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores external type identifiers for owned sub-units, facilitating data enrichment and integration processes. |
| `Unit` | `string` | The 'Unit' column contains string values representing the various measurement units used for quantifying data in the lkp_Unit table, facilitating consistent data enrichment and analysis. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, status, leadership, and financial aspects. This table enables effective project management and oversight by providing stakeholders with a clear view of project statuses and organizational structure, facilitating informed decision-making and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and performance analysis within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column in the 'tbl_Project' table captures the administrative details and oversight responsibilities associated with each project, facilitating effective management and governance. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates and changes within the 'tbl_Project' table. |
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each project, facilitating the identification and management of client relationships within the project database. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating efficient tracking and management of client-related data within the tbl_Project table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated in the 'tbl_Project' table, providing a timestamp for tracking project creation. |
| `Currency` | `string` | The 'Currency' column in the 'tbl_Project' table specifies the type of currency used for financial transactions related to each project, facilitating accurate budgeting and financial reporting. |
| `DateCreated` | `dateTime` | Column Description: The 'DateCreated' column records the exact date and time when each project entry was created, facilitating tracking and management of project timelines. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or business unit associated with each project in the 'tbl_Project' table. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column in the 'tbl_Project' table indicates the scheduled date and time for the completion of each project, facilitating effective project timeline management and tracking. |
| `Project` | `string` | The 'Project' column contains the name or identifier of each project, serving as a key reference for associating related data and activities within the tbl_Project table. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed narrative of the project's objectives, scope, and key deliverables, providing essential context for stakeholders and team members. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column categorizes projects into specific groups for better organization and management within the 'tbl_Project' table. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a detailed textual overview of the project group, facilitating better understanding and categorization of related projects within the 'tbl_Project' table. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for data enrichment and management processes. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the associated project in the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency amount for the sales price of a project, stored as a decimal to ensure precision in financial calculations. |
| `Startdate` | `dateTime` | The 'Startdate' column in the 'tbl_Project' table captures the date and time when a project is initiated, serving as a critical reference point for project timelines and scheduling. |
| `Status` | `string` | The 'Status' column in the 'tbl_Project' table indicates the current progress or state of each project, helping stakeholders assess project health and make informed decisions. |
| `Statuscode` | `string` | The 'Statuscode' column in the 'tbl_Project' table indicates the current operational status of each project, facilitating effective tracking and management of project progress. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with each project, expressed as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table provides a comprehensive overview of employees who have not submitted their timesheets, detailing the relevant time period, employee and manager information, and contract specifics. This data is essential for management to identify compliance issues, ensure accurate payroll processing, and enhance workforce accountability.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by employees that have not been recorded in their timesheets, facilitating accurate tracking and reporting of labor hours. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to conclude, providing essential information for tracking contract durations within the context of missing timesheet records. |
| `ContractHours` | `int64` | The 'ContractHours' column (int64) in the 'vw_missing_timesheet' table represents the total number of hours contracted for each employee, facilitating the identification of discrepancies in reported timesheets. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of contracts for employees who have missing timesheets, providing essential context for identifying compliance and payroll issues. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on outstanding time reporting issues. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours recorded for each employee's timesheet entry, facilitating the identification of discrepancies in time reporting. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data updates. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the employee's timesheet entries in the 'vw_missing_timesheet' table. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees' timesheet submissions, facilitating accountability and communication regarding missing entries. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee work hour reporting. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the missing timesheet records, facilitating the tracking and management of time allocation across various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores unique identifiers for projects associated with missing timesheets, facilitating the tracking and management of time allocation across various initiatives. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true signifying rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet data is complete and ready for reporting, with a value of true signifying readiness and false indicating further action is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double to accommodate precise monetary values within the 'vw_missing_timesheet' table. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double to accommodate precise pricing information within the context of the 'vw_missing_timesheet' table. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with missing timesheet entries, facilitating the identification and resolution of discrepancies in employee time reporting. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific organizational subunit associated with each entry in the missing timesheet records, facilitating targeted analysis and resolution of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' table. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a textual summary of the tasks or activities recorded in the timesheet, aiding in the identification and analysis of missing entries. |
| `Week_` | `int64` | The 'Week_' column (int64) in the 'vw_missing_timesheet' table represents the numerical week of the year associated with each timesheet entry, facilitating the identification of missing submissions by week. |
| `Year_` | `int64` | The 'Year_' column (int64) in the 'vw_missing_timesheet' table represents the calendar year associated with each timesheet entry, facilitating the analysis of timesheet data over time. |

##### Calculated Columns

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column identifies the department associated with billable hours for employees, facilitating accurate tracking and reporting of time spent on chargeable projects within the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression starts by checking if there is a related value in the `lkp_Unit` table for the column `BillableDep`. This is done using the `RELATED` function, which pulls in data from a related table based on existing relationships in your data model.

2. **Handle Blank Values**: The first part of the `IF` statement uses `ISBLANK` to see if the related `BillableDep` value is blank (i.e., there is no corresponding value in the `lkp_Unit` table). If it is blank, the expression returns `1`. This means that if there is no related billable dependency, it defaults to a value of `1`.

3. **Check for Non-Zero Values**: If the related `BillableDep` value is not blank, the expression then checks if this value is not equal to `0`. If it finds that the value is indeed not zero, it again returns `1`. This indicates that there is a valid billable dependency.

4. **Return Zero for Zero Values**: If the related `BillableDep` value is found and it is equal to `0`, the expression returns `0`. This means that there is a billable dependency, but it has a value of zero, which may imply that it is not billable.

### Summary:
In summary, this DAX expression effectively categorizes the `BillableDep` status into two outcomes:
- It returns `1` if there is either no related billable dependency or if there is a valid billable dependency (non-zero).
- It returns `0` only if there is a related billable dependency that is exactly zero.

This calculated column helps in identifying whether a unit is billable based on its dependency status, which can be useful for reporting and analysis in a business context.

**`CC_ActiveEmployees`** (`string`)

- **Description:** The 'CC_ActiveEmployees' column contains a string representation of the active employees in the company, serving as a reference for identifying personnel who may be missing timesheet entries in the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
IF(vw_missing_timesheet[ShortDate] >= vw_missing_timesheet[ContractStartDate] && 
			      (vw_missing_timesheet[ShortDate] <= vw_missing_timesheet[ContractEndDate] || ISBLANK(vw_missing_timesheet[ContractEndDate]))
			        , 1
			        , 0
			    )
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `CC_ActiveEmployees` in a data model, likely related to employee management or timesheet tracking. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column determines whether an employee is considered "active" on a specific date (referred to as `ShortDate`).

2. **Conditions**:
   - The first condition checks if the `ShortDate` (the date being evaluated) is on or after the employee's `ContractStartDate`. This means the employee's contract must have started before or on this date for them to be considered active.
   - The second condition checks if the `ShortDate` is on or before the `ContractEndDate` (the date when the employee's contract ends). However, if the `ContractEndDate` is blank (meaning the employee does not have a defined end date, possibly indicating they are still employed), this condition is also satisfied.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the contract has no end date), the expression returns a value of `1`, indicating that the employee is active on that date.
   - If either condition is not met, it returns a value of `0`, indicating that the employee is not active on that date.

### Summary:
In summary, this DAX expression effectively flags employees as "active" (1) or "not active" (0) based on whether the date being evaluated falls within their contract period or if they have an ongoing contract without an end date. This helps in tracking employee status over time, which is crucial for payroll, attendance, and resource management.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to assist in identifying and addressing missing or insufficient data in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called 'IncompleteFlag' in a data model, specifically in a table named `vw_missing_timesheet`. Here's a simple explanation of what this code does:

### Explanation of the DAX Expression:

- **Condition Check**: The expression checks the value in the column `MissingHours` for each row in the `vw_missing_timesheet` table.
- **Logic**: 
  - If the value of `MissingHours` is **less than 0**, it means that there is an issue (for example, it could indicate that there are negative hours reported, which doesn't make sense in this context).
  - In this case, the expression returns **1**, which can be interpreted as a flag indicating that the entry is "incomplete" or problematic.
  - If the value of `MissingHours` is **0 or greater**, the expression returns **0**, indicating that there is no issue with that entry.

### Business Interpretation:

In business terms, this calculated column helps identify records that have a problem with the reported hours. By flagging entries with a value of 1, it allows users to quickly see which records need attention or correction, ensuring that all timesheet entries are accurate and complete. This can be particularly useful for payroll processing, project management, or any scenario where accurate time tracking is critical.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit associated with each entry in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet discrepancies by unit.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the 'lkp_Unit' table for the current row. Specifically, it looks for the 'Unit' field in that related table.

2. **Handle Missing Data**: 
   - If there is no related value (meaning the 'Unit' field is blank or missing), the code will return the text "Unknown". This is a way to handle situations where the expected data is not available, ensuring that the result is clear and understandable.
   - If there is a related value (meaning the 'Unit' field has data), it will return that value from the 'lkp_Unit' table.

3. **Purpose**: The overall purpose of this calculated column is to provide a clear and meaningful representation of the 'Unit' for each row in the main table. If the unit is not found, it avoids confusion by labeling it as "Unknown".

In summary, this DAX expression ensures that every row in the 'MAIN_UNIT' column either shows the corresponding unit name or indicates that the unit is unknown, thereby improving data clarity and usability.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column represents the year and week number in a string format, facilitating the identification and analysis of timesheet data for specific weeks within a given year.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'YearWeek' in a data model, and it combines two pieces of information: the year and the week number of a specific date, which is represented by the column `[ContractEndDate]`.

Here’s a breakdown of what this code does in simple business terms:

1. **Extracting the Year**: 
   - The function `YEAR([ContractEndDate])` takes the date from the `[ContractEndDate]` column and extracts the year from it. For example, if the date is December 15, 2023, this part would return `2023`.

2. **Calculating the Week Number**:
   - The function `WEEKNUM([ContractEndDate])` calculates which week of the year the date falls into. For instance, if the date is December 15, 2023, it might return `50`, indicating that it is the 50th week of the year.

3. **Formatting the Week Number**:
   - The `FORMAT(..., "00")` function ensures that the week number is always displayed as a two-digit number. So, if the week number is `5`, it will be formatted as `05`.

4. **Combining Year and Week Number**:
   - The `& "-" &` part of the code concatenates (joins) the year and the formatted week number with a hyphen in between. So, if the year is `2023` and the week number is `50`, the final result will be `2023-50`.

### Summary:
In summary, this DAX expression creates a new column that shows the year and week number of the contract's end date in the format "YYYY-WW". For example, if a contract ends on December 15, 2023, the calculated column 'YearWeek' would display `2023-50`. This can be useful for reporting and analyzing data on a weekly basis, allowing businesses to track contracts and activities over time.

##### Measures

**`Count_Negative_Consultant`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to create a measure called `Count_Negative_Consultant`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure counts the number of unique employees (consultants) who have recorded negative hours in a timesheet.

2. **Components**:
   - **`CALCULATE`**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the number of unique employee names in the `vw_missing_timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This is a filter condition applied within the `CALCULATE` function. It specifies that only those records where the `MissingHours` value is less than zero should be considered. In other words, it focuses on cases where employees have negative hours logged.

3. **Outcome**: The measure ultimately provides a count of how many different employees have negative hours recorded in their timesheets. This could be useful for identifying issues such as underreporting of hours worked or discrepancies in time tracking.

In summary, `Count_Negative_Consultant` helps the business understand how many unique consultants have reported negative hours, which can be an indicator of potential problems in time management or reporting accuracy.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `CountNegativeMissingHours`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure counts the number of unique employees who have recorded negative missing hours in the `vw_missing_timesheet` table.

2. **Components**:
   - **CALCULATE**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **DISTINCTCOUNT**: This function counts the number of unique values in a specified column. Here, it counts unique `EmployeeID`s.
   - **'vw_missing_timesheet'[MissingHours] < 0**: This is the filter condition applied by the `CALCULATE` function. It specifies that only records where the `MissingHours` value is less than zero should be considered.

3. **What It Achieves**: The measure effectively identifies and counts how many different employees have reported negative hours in their timesheets. This could indicate issues such as errors in reporting, adjustments needed, or other anomalies that require attention.

In summary, `CountNegativeMissingHours` helps a business track and understand how many employees have discrepancies in their reported hours, specifically focusing on those with negative values, which could signal potential problems in timekeeping or reporting processes.

**`Dax_EmpCount`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`Dax_EmpCount_MissingTS`**

- **DAX Expression:**
```dax
VAR ActiveEmployees = 
			    FILTER(
			        SUMMARIZE(
			            vw_missing_timesheet,
			            vw_missing_timesheet[EmployeeName],
			            vw_missing_timesheet[Year_],
			            vw_missing_timesheet[Week_],
			            lkp_Unit[Unit],
			            vw_missing_timesheet[SubUnit],
			            "MissingHours", SUM(vw_missing_timesheet[Hours_]) - MAX(vw_missing_timesheet[ContractHours]),
			            "MinActiveEmp", MIN(vw_missing_timesheet[CC_ActiveEmployees])
			        ),
			        [MinActiveEmp] = 1 && [MissingHours] < 0
			    )
			RETURN COUNTROWS(ActiveEmployees)
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to create a measure called `Dax_EmpCount_MissingTS`, which calculates the number of active employees who are missing timesheet entries for a specific period. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The measure uses a data source called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including hours worked and contract hours.

2. **Summarizing Data**: The code first summarizes the data by grouping it based on several key attributes:
   - **Employee Name**: Identifies each employee.
   - **Year and Week**: Specifies the time period for the analysis.
   - **Unit and SubUnit**: Represents the organizational structure or department.
   - **Missing Hours Calculation**: It calculates the total hours reported by each employee and subtracts their contracted hours. This gives a value called "MissingHours," which indicates how many hours an employee is short of their expected hours.
   - **Minimum Active Employees**: It also finds the minimum number of active employees for each grouping, labeled as "MinActiveEmp."

3. **Filtering Criteria**: After summarizing the data, the code filters this summarized data to find:
   - Employees who are currently active (where `MinActiveEmp` equals 1).
   - Employees who have a negative value for "MissingHours," meaning they have not logged enough hours compared to what they are contracted for.

4. **Counting Active Employees**: Finally, the measure counts the number of rows in the filtered data, which corresponds to the number of active employees who are missing timesheet entries for the specified period.

### Summary:
In essence, this DAX measure calculates how many active employees have not submitted enough hours in their timesheets for a given year and week, helping the business identify potential issues with timesheet compliance and employee workload management.

**`Dax_MissingHours`**

- **DAX Expression:**
```dax
VAR ActiveEmployees = 
			    FILTER(
			        SUMMARIZE(
			            vw_missing_timesheet,
			            vw_missing_timesheet[EmployeeName],
			            vw_missing_timesheet[Year_],
			            vw_missing_timesheet[Week_],
			            lkp_Unit[Unit],
			            vw_missing_timesheet[SubUnit],
			            "MissingHours", SUM(vw_missing_timesheet[Hours_]) - MAX(vw_missing_timesheet[ContractHours]),
			            "MinActiveEmp", MIN(vw_missing_timesheet[CC_ActiveEmployees])
			        ),
			        [MinActiveEmp] = 1 && [MissingHours] < 0
			    )
			RETURN 
			    SUMX(ActiveEmployees, [MissingHours])
```
- **DAX Explanation (Generated):** The DAX code snippet you've provided is designed to calculate a measure called `Dax_MissingHours`, which focuses on identifying and summing up the negative missing hours for active employees in a specific context (like a week or year). Here’s a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code first creates a variable called `ActiveEmployees`. This variable filters a summarized table (`vw_missing_timesheet`) to focus on employees who are currently active. An employee is considered active if the minimum value of `CC_ActiveEmployees` for that employee is 1.

2. **Calculate Missing Hours**: For each employee, the code calculates "MissingHours" by taking the total hours recorded (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contract hours they are supposed to work (`MAX(vw_missing_timesheet[ContractHours])`). If this calculation results in a negative number, it indicates that the employee has worked fewer hours than expected.

3. **Filter for Relevant Data**: The `FILTER` function then narrows down the summarized data to only include those employees who are active and have negative missing hours (i.e., they have not met their expected working hours).

4. **Sum the Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered list of active employees and sums up their "MissingHours". This gives the total amount of hours that active employees are missing, which is a crucial metric for understanding workforce productivity and attendance.

In summary, this DAX measure calculates the total number of hours that active employees are missing based on their expected working hours, helping the business identify potential issues with attendance or workload management.

**`ExpectedContractHoursWeekly`**

- **DAX Expression:**
```dax
SUMX(
			    SUMMARIZE(
			        vw_missing_timesheet,
			        vw_missing_timesheet[Week_],
			        vw_missing_timesheet[EmployeeID],
			        "ContractHours", MAX(vw_missing_timesheet[ContractHours])
			    ),
			    [ContractHours]
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you've provided calculates the total expected contract hours for employees on a weekly basis. Here's a breakdown of what it does in simple business terms:

1. **Data Source**: The calculation is based on a table called `vw_missing_timesheet`, which likely contains records of timesheets for employees, including their contract hours and the week associated with each entry.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key dimensions:
   - **Week**: This represents the specific week for which we are calculating the expected contract hours.
   - **EmployeeID**: This identifies each employee.

3. **Calculating Maximum Contract Hours**: For each combination of week and employee, the code calculates the maximum contract hours from the `ContractHours` column. This means that if an employee has multiple entries for the same week, it will only take the highest value of contract hours for that week.

4. **Summing Up Contract Hours**: After summarizing the data and determining the maximum contract hours for each employee in each week, the `SUMX` function then adds up all these maximum contract hours across all employees and weeks.

### What It Achieves:
The overall result of this DAX measure is the total expected contract hours for all employees, aggregated by week. This helps in understanding how many hours are expected to be worked based on the highest contract hours recorded for each employee in each week. It can be useful for tracking employee workload, ensuring compliance with contract terms, and identifying any discrepancies in timesheet reporting.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the difference between two values related to hours worked and contract hours for a specific context, likely in a reporting or analytical scenario.

Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum value of "ContractHours" from the `vw_missing_timesheet` table. "ContractHours" typically represents the total number of hours that an employee is contracted to work.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum value of "Hours_" from the same table. "Hours_" likely represents the actual hours that the employee has worked or reported.

3. **Calculation**: The expression then subtracts the maximum actual hours worked (Hours_) from the maximum contracted hours (ContractHours). 

### What It Achieves:
- The result of this calculation gives you the number of hours that are "missing" or unaccounted for. In other words, it shows how many hours an employee is expected to work (based on their contract) but has not reported or logged as worked.

### Business Implication:
- This measure can help management identify discrepancies between expected and actual work hours. If the result is a positive number, it indicates that the employee has not logged all their contracted hours, which could be a concern for productivity or compliance. If the result is zero or negative, it suggests that the employee has met or exceeded their contracted hours. 

Overall, this measure is useful for tracking employee attendance and ensuring that work hours align with contractual obligations.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the "Percentage Completeness" of timesheets based on the hours that are missing compared to the expected contract hours for a week. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part of the code adds up all the missing hours from the timesheet data. It gives a total of how many hours employees have not reported.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that employees are expected to work in a week according to their contracts.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: This calculation finds the difference between the total missing hours and the expected hours. If the result is positive, it indicates that the missing hours exceed what was expected; if negative, it means that the reported hours are more than expected.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The DIVIDE function takes the result from the previous step (the difference between missing hours and expected hours) and divides it by the expected contract hours. This gives a ratio or percentage of how complete the timesheet reporting is. If the expected hours are zero, it will return 0 instead of an error.

### In Summary:
The "Measure: PercentageCompleteness" calculates how well employees are reporting their hours compared to what is expected. A positive percentage indicates that there are more missing hours than expected, suggesting a completeness issue in timesheet reporting. A negative percentage would indicate that employees are reporting more hours than expected, which might not be a problem but could require further investigation. The result helps management understand the level of compliance with timesheet submissions.

**`UniqueEmployeesPerUnit`**

- **DAX Expression:**
```dax
SUMX(
			    SUMMARIZE(
			        vw_missing_timesheet,
			        vw_missing_timesheet[Week_],
			        vw_missing_timesheet[EmployeeName],
			        vw_missing_timesheet[MAIN_UNIT],
			        "IncompleteFlag", MAX(vw_missing_timesheet[IncompleteFlag])
			    ),
			    [IncompleteFlag]
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate a measure called `UniqueEmployeesPerUnit`. Let's break it down step by step in simple business terms:

1. **Data Source**: The measure operates on a data table called `vw_missing_timesheet`. This table likely contains records of employees, their work weeks, the units they belong to, and whether their timesheets are complete or incomplete.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by three key columns:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This is the name of the employee.
   - `MAIN_UNIT`: This indicates the main unit or department the employee belongs to.

   While grouping, it also creates a new column called `IncompleteFlag`, which captures the maximum value of the `IncompleteFlag` for each combination of week, employee, and unit. The `IncompleteFlag` likely indicates whether an employee has submitted their timesheet (with 0 for complete and 1 for incomplete).

3. **Calculating the Sum**: After summarizing the data, the `SUMX` function iterates over each unique combination of week, employee, and unit created by the `SUMMARIZE` function. For each of these combinations, it sums up the values in the `IncompleteFlag` column.

4. **Final Outcome**: The result of this measure is the total count of incomplete timesheets across all unique employees and units for each week. In other words, it tells you how many timesheets are missing for employees in different units, helping management identify areas where timesheet compliance may be an issue.

In summary, `UniqueEmployeesPerUnit` calculates the total number of incomplete timesheets for employees, grouped by week and unit, allowing the organization to monitor and address timesheet submission issues effectively.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This table enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column in the 'vw_Timesheet' table identifies the specific client associated with each timesheet entry, facilitating accurate billing and project tracking. |
| `Code2` | `string` | The 'Code2' column in the 'vw_Timesheet' table stores a secondary identifier code used to categorize or classify timesheet entries for enhanced data analysis and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract is scheduled to conclude, providing essential information for managing workforce planning and contract renewals within the timesheet data. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and payroll calculations within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | Column Description: Indicates the current status of the contract associated with the timesheet entry, represented as a string value. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, providing essential insights for performance analysis and decision-making within the timesheet data context. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential temporal context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the total monetary cost associated with the recorded time entries in the 'vw_Timesheet' table, expressed as a double-precision floating-point number. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, expressed as a double to allow for precise financial calculations. |
| `Currency` | `string` | The 'Currency' column specifies the type of currency used for financial entries in the timesheet, facilitating accurate monetary calculations and reporting. |
| `DebtorName` | `string` | The 'DebtorName' column contains the name of the individual or entity responsible for payment, providing essential identification for billing and financial tracking within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_Timesheet' table uniquely identifies each employee, facilitating the tracking and management of their respective timesheet entries. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column stores the unique identifier and name of each employee, facilitating the association of timesheet entries with the corresponding personnel in the vw_Timesheet table. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with specific personnel for accurate tracking and reporting. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, facilitating the identification and tracking of individual contributions to timesheet entries. |
| `Employer` | `string` | The 'Employer' column in the 'vw_Timesheet' table captures the name of the organization for which the employee is working, facilitating accurate tracking and reporting of labor costs associated with each employer. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating accurate payroll and reporting processes. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to each timesheet entry, providing further insights into the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with each timesheet entry, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, facilitating accurate tracking of labor input for payroll and project management. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating analysis of labor allocation and productivity. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or vacation, to facilitate accurate reporting and analysis of employee time allocation. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee time allocation. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and management. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) in the 'vw_Timesheet' table uniquely identifies the internal project manager associated with each timesheet entry, facilitating efficient tracking and management of project resources. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries, facilitating accountability and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the names of individual project management identifiers associated with each timesheet entry, facilitating the tracking and management of project-related hours. |
| `JobTitle` | `string` | The 'JobTitle' column in the 'vw_Timesheet' table captures the specific role or position of an employee, providing essential context for analyzing timesheet data and resource allocation. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking timesheet records with related datasets, facilitating seamless data integration and analysis. |
| `ManagerID` | `string` | Column Description: The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and reporting within the organization. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column in the 'vw_Timesheet' table stores the names of managers associated with each timesheet entry, facilitating easy identification of managerial oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key associated with the manager responsible for overseeing the timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct supervisor, facilitating the identification of managerial oversight in timesheet entries. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries, providing detailed information about hours worked on specific tasks within the timesheet records. |
| `Project` | `string` | The 'Project' column in the 'vw_Timesheet' table identifies the specific project associated with each recorded time entry, facilitating accurate tracking and reporting of time spent on various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project, enabling efficient tracking and reporting of time spent on various projects within the timesheet data. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that identifies the specific project associated with each timesheet entry, enabling detailed tracking and analysis of time allocation across various projects. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the specific project profile associated with each timesheet entry, facilitating accurate project tracking and reporting. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, enabling better analysis of resource allocation and project performance. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet is ready for reporting, with a value of true signifying readiness and false indicating that further action is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double to accommodate precise financial calculations within the context of timesheet data analysis. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current state of each timesheet entry, reflecting whether it is submitted, approved, or pending review. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, enabling better resource allocation and management. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for each timesheet entry, facilitating the tracking and management of employee work hours and project allocations. |
| `TimesheetDate` | `dateTime` | Column Description: The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the tasks and activities recorded in the timesheet, providing context and details for each entry. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table represents the measurement unit associated with the recorded time entries, facilitating accurate tracking and reporting of work hours across various projects. |
| `UnitCode` | `string` | The 'UnitCode' column stores a unique identifier for each unit of measurement associated with the timesheet entries, facilitating accurate tracking and reporting of time allocations across different units. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and data enrichment within the timesheet management system. |
| `YearWeek` | `string` | The 'YearWeek' column (string) in the 'vw_Timesheet' table represents the specific year and week number, facilitating time-based analysis and reporting of timesheet data. |

##### Calculated Columns

**`Approved_With_V_Z`** (`string`)

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheets, specifying whether they have been approved with a particular validation or verification process denoted by 'V_Z'.
- **DAX Expression:**
```dax
IF(
			    [Approved] = TRUE() || 
			    vw_Timesheet[TimesheetCode] IN {"V", "Z"}, 
			    1, 
			    0
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'Approved_With_V_Z'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column determines whether a timesheet is considered "approved" based on specific criteria.

2. **Conditions Checked**:
   - The first condition checks if the column `[Approved]` is set to `TRUE()`. This means that if the timesheet has been officially approved, it meets the criteria.
   - The second condition checks if the `TimesheetCode` from the `vw_Timesheet` table is either "V" or "Z". This means that if the timesheet has a code of "V" or "Z", it also meets the criteria for being considered approved.

3. **Output**:
   - If either of the conditions is met (the timesheet is approved or has a code of "V" or "Z"), the calculated column will return a value of `1`. This indicates that the timesheet is approved.
   - If neither condition is met, it will return a value of `0`, indicating that the timesheet is not approved.

In summary, this DAX expression effectively flags timesheets as approved (1) if they are either officially approved or have specific codes ("V" or "Z"). If neither condition is true, it flags them as not approved (0). This can help in reporting and analysis by easily identifying which timesheets are considered approved based on these criteria.

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column identifies the department associated with billable hours recorded in the timesheet, facilitating accurate project cost allocation and departmental budgeting.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `BillableDep`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column `BillableDep` is designed to determine whether a certain unit is billable based on its related data.

2. **Logic**:
   - The code first checks if the related value from the `lkp_Unit` table (specifically the `BillableDep` column) is blank (i.e., there is no value).
   - If it is blank, the calculated column will return a value of **1**. This indicates that if there is no information about whether the unit is billable, it defaults to being considered billable.
   - If the related `BillableDep` value is not blank, the code then checks if this value is not equal to **0**.
     - If the value is not **0**, it returns **1**, meaning the unit is considered billable.
     - If the value is **0**, it returns **0**, indicating that the unit is not billable.

3. **Outcome**: 
   - The final result of this calculated column will be either **1** (indicating the unit is billable) or **0** (indicating it is not billable). 
   - This helps in categorizing units based on their billability status, which can be useful for financial reporting, invoicing, or resource allocation.

In summary, this DAX expression effectively assesses whether a unit can be billed based on the related `BillableDep` information, defaulting to billable if no information is available.

**`BillablePrj`** (`string`)

- **Description:** The 'BillablePrj' column identifies the project associated with billable hours recorded in the timesheet, facilitating accurate client invoicing and project cost tracking.
- **DAX Expression:**
```dax
IF(
			    vw_Timesheet[SalesPrice] > 0 || 
			    vw_Timesheet[ProjectProfileCode] = 81 || 
			    SEARCH("Customer Success Services", vw_Timesheet[Project],, 0) > 0, 
			    1, 
			    0
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `BillablePrj` in a data model, specifically within a table called `vw_Timesheet`. Here's a breakdown of what this code does in simple business terms:

1. **Purpose**: The `BillablePrj` column is designed to identify whether a particular timesheet entry is considered "billable" or not. A billable entry is one that can be charged to a client or customer.

2. **Conditions for Billability**: The code checks three specific conditions to determine if an entry is billable:
   - **Sales Price Greater than Zero**: If the `SalesPrice` for the entry is greater than 0, it indicates that there is a charge associated with this entry, making it billable.
   - **Specific Project Profile Code**: If the `ProjectProfileCode` is equal to 81, this suggests that this particular project is predefined as billable, regardless of other factors.
   - **Contains "Customer Success Services"**: If the `Project` field contains the phrase "Customer Success Services", it indicates that this project is also considered billable.

3. **Output**: 
   - If any of the above conditions are met, the expression returns a value of `1`, indicating that the entry is billable.
   - If none of the conditions are met, it returns a value of `0`, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project description. This helps in tracking which work can be charged to clients, aiding in revenue generation and project management.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column in the 'vw_Timesheet' table captures the name of the employer associated with each timesheet entry, facilitating accurate tracking and reporting of employee work hours by organization.

**`Employee_WeeklyHours`** (`string`)

- **Description:** The 'Employee_WeeklyHours' column captures the total number of hours worked by each employee during the week, represented as a string for flexibility in formatting.
- **DAX Expression:**
```dax
CONCATENATE(
			    ('vw_Timesheet'[EmployeeName]),
			    " - " & ('vw_Timesheet'[HoursperWeek])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `Employee_WeeklyHours` in a data model, specifically within a table named `vw_Timesheet`. 

Here’s a breakdown of what this code does in simple business terms:

1. **Combining Information**: The code takes two pieces of information from the `vw_Timesheet` table:
   - The name of the employee (`EmployeeName`).
   - The number of hours that employee works per week (`HoursperWeek`).

2. **Formatting the Output**: It combines these two pieces of information into a single string. The format of the output will be:
   - The employee's name followed by a hyphen and then the number of hours they work each week. For example, if the employee's name is "John Doe" and he works 40 hours a week, the result would be "John Doe - 40".

3. **Purpose**: This calculated column is useful for creating a clear and concise representation of each employee's weekly working hours alongside their name. It can be helpful for reporting, analysis, or visualization purposes, making it easier to see at a glance how many hours each employee is scheduled to work.

In summary, this DAX expression creates a new column that displays each employee's name along with their weekly hours in a readable format, enhancing the clarity of the data in the `vw_Timesheet` table.

**`GroupCat`** (`string`)

- **Description:** The 'GroupCat' column categorizes timesheet entries into specific groups, facilitating streamlined reporting and analysis of labor distribution across various projects or departments.
- **DAX Expression:**
```dax
IF(
			    NOT(ISBLANK(
			        LOOKUPVALUE(lkp_Project[Group], lkp_Project[PROJECT], vw_Timesheet[Project])
			    )),
			    LOOKUPVALUE(lkp_Project[Group], lkp_Project[PROJECT], vw_Timesheet[Project]),
			    IF(
			        vw_Timesheet[BillablePrj] = 1, 
			        "Billable", 
			        "Other Unbillable"
			    )
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'GroupCat' in a data model, likely related to project management or timesheet tracking. Here's a breakdown of what it does in simple business terms:

1. **Lookup for Group**: The code first checks if there is a corresponding 'Group' value for a project in the 'lkp_Project' table based on the project ID found in the 'vw_Timesheet' table. It does this using the `LOOKUPVALUE` function, which searches for the 'Group' associated with the specific project.

2. **Check for Blank Value**: The `NOT(ISBLANK(...))` part checks if the result of the lookup is not blank. This means it verifies whether a valid 'Group' was found for the project.

3. **Return Group or Classify as Billable/Unbillable**:
   - If a valid 'Group' is found (i.e., the lookup is not blank), it returns that 'Group' value.
   - If no valid 'Group' is found, it then checks if the project is billable by looking at the 'BillablePrj' column in the 'vw_Timesheet' table:
     - If 'BillablePrj' equals 1 (indicating the project is billable), it assigns the label "Billable".
     - If 'BillablePrj' does not equal 1 (indicating the project is not billable), it assigns the label "Other Unbillable".

### Summary:
In summary, this DAX expression categorizes each project in the 'vw_Timesheet' table into one of three categories:
- It assigns the project to its corresponding 'Group' if available.
- If no group is found, it checks if the project is billable and labels it as "Billable" or "Other Unbillable" accordingly. 

This helps in organizing and analyzing projects based on their billing status and group affiliation, which can be crucial for financial reporting and project management.

**`IsContractActive`** (`string`)

- **Description:** Indicates whether the associated contract is currently active, represented as a string value within the timesheet data.
- **DAX Expression:**
```dax
IF(
			    ISBLANK('vw_Timesheet'[ContractEndDate]) || 'vw_Timesheet'[ContractEndDate] > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called **IsContractActive** in a data model, specifically for a table named **vw_Timesheet**. Here's a simple breakdown of what it does:

1. **Checks Contract End Date**: The code first looks at the **ContractEndDate** field in the **vw_Timesheet** table. This field indicates when a contract is supposed to end.

2. **Conditions Evaluated**:
   - **ISBLANK**: It checks if the **ContractEndDate** is blank (meaning there is no end date specified). If there is no end date, it implies that the contract is still active.
   - **Comparison with Today**: If there is a date present, it checks whether that date is greater than today's date. If the end date is in the future, it means the contract is still active.

3. **Outputs**:
   - If either of the above conditions is true (the end date is blank or it is in the future), the calculated column will return **"Active"**.
   - If neither condition is true (meaning there is a specific end date that is in the past), it will return **"Not Active"**.

### Summary:
In business terms, this calculated column helps to quickly identify whether a contract is currently active or not. It provides a clear label of **"Active"** for contracts that are ongoing (either because they have no specified end date or their end date is still in the future) and **"Not Active"** for those that have already expired. This can be useful for reporting, analysis, and decision-making regarding contract management.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column in the 'vw_Timesheet' table identifies the primary organizational unit responsible for the recorded timesheet entries, facilitating effective resource allocation and reporting.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the 'lkp_Unit' table for the current row. Specifically, it looks for the 'Unit' field in that related table.

2. **Handle Missing Data**: 
   - If the related 'Unit' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the code will return the text "Unknown". This is a way to handle situations where the expected data is missing, ensuring that the output is clear and informative.
   - If there is a related 'Unit' value (meaning the data exists), it will return that value.

3. **Purpose**: The overall purpose of this calculated column is to ensure that every row in the main table has a meaningful value for 'MAIN_UNIT'. If the related unit is not available, it clearly indicates that by using "Unknown". This helps maintain data integrity and provides clarity in reporting or analysis.

In summary, this DAX expression is designed to populate the 'MAIN_UNIT' column with either the related unit name or "Unknown" if no related unit exists, ensuring that users can easily identify missing data.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) to facilitate time-based reporting and analysis of timesheet data.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to create a calculated column called 'MonthNumber' in a data model, typically within tools like Power BI or Excel.

### What it Achieves:

1. **Extracting the Month**: This expression takes a date value from the column `[TimesheetDate]` and extracts the month part of that date. 

2. **Numeric Representation**: The result is a number that represents the month. For example:
   - If `[TimesheetDate]` is January 15, 2023, the expression will return `1`.
   - If `[TimesheetDate]` is July 22, 2023, it will return `7`.

3. **Usefulness**: By creating a 'MonthNumber' column, you can easily analyze and visualize data based on months. This can help in reporting, such as tracking performance or trends over different months of the year.

In summary, this DAX expression simplifies the process of identifying which month a specific date falls into, allowing for better data analysis and reporting based on monthly intervals.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the unique employee identification numbers as strings, facilitating the association of timesheet entries with specific employees in the vw_Timesheet view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

### What it Achieves:

1. **Counts Unique Employees**: This expression counts how many different employees are present in the `EmployeeName` column. For example, if there are 10 entries for "John Doe" and 5 entries for "Jane Smith," the result will be 2, because there are two distinct names.

2. **Useful for Reporting**: This calculation is particularly useful in reports or dashboards where you want to know how many individual employees have logged time or worked on tasks, regardless of how many times they appear in the timesheet.

3. **Data Analysis**: It helps in analyzing workforce participation, understanding employee engagement, and making decisions based on the number of unique contributors to projects or tasks.

In summary, this DAX expression provides a straightforward way to quantify the diversity of employees recorded in the timesheet, which can be valuable for various business insights and operational assessments.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table captures the classification of time entries as either owned, subcontracted, or external, facilitating detailed analysis of resource allocation and project costs.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used in a calculated column to retrieve a specific value from a related table. Here's a breakdown of what it does in simple business terms:

1. **Context of Use**: This expression is typically used in a data model where there are two tables that have a relationship defined between them. For example, you might have a main table (let's say "Sales") and a lookup table (let's say "lkp_Unit").

2. **Purpose of the Expression**: The `RELATED` function is designed to pull in data from a related table based on the existing relationship. In this case, it is fetching the value from the column `OWN-Sub-ExtT` in the `lkp_Unit` table.

3. **What It Achieves**: By using this expression in a calculated column, you are effectively enriching your main table (like "Sales") with additional information from the `lkp_Unit` table. Specifically, for each row in the main table, it will look up the corresponding value of `OWN-Sub-ExtT` from the `lkp_Unit` table based on the relationship defined between the two tables.

4. **Example Scenario**: Imagine you have a sales record that includes a unit ID. The `lkp_Unit` table contains additional details about each unit, including a column called `OWN-Sub-ExtT` that might indicate whether the unit is owned, subleased, or externally sourced. By using this DAX expression, you can automatically populate the main sales table with this relevant information for each sale, making it easier to analyze and report on the data.

In summary, this DAX expression helps to enhance your data model by linking related information from different tables, allowing for more comprehensive analysis and reporting.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column contains string values that categorize or qualify the projects associated with each timesheet entry, providing essential context for project-related time tracking and analysis.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'QualifyPrj'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column is designed to determine a qualification status for projects based on related data from another table (referred to as `lkp_Project`).

2. **Logic**:
   - The code checks if the 'Qualify' field from the related `lkp_Project` table is blank (i.e., it has no value).
   - If the 'Qualify' field is blank, the calculated column will return a value of **1**. This could indicate a default or a specific status (like "not qualified").
   - If the 'Qualify' field is not blank (meaning it has a value), the calculated column will return the actual value from the 'Qualify' field in the `lkp_Project` table.

3. **Outcome**: 
   - The result of this calculation is that for each row in the table where this calculated column is being created, you will either get a default value of 1 (if there is no qualification status available) or the actual qualification status from the related project data.
   - This helps in ensuring that every project has a qualification status, either by assigning a default value or by pulling in the existing qualification data.

In summary, this DAX expression ensures that every project has a qualification status, either by providing a default value when none exists or by using the existing qualification value from a related table.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor allocation.

**`RReady_With_V_Z`** (`string`)

- **Description:** Column Description: Indicates the readiness status of resources with respect to the specified variable 'Z', facilitating efficient project time tracking and resource allocation in the timesheet data.
- **DAX Expression:**
```dax
IF(
			    [ReportReady] = TRUE() || 
			    vw_Timesheet[TimesheetCode] IN {"V", "Z"}, 
			    1, 
			    0
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `RReady_With_V_Z`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column determines whether a certain condition is met for each row in the data. It outputs either a 1 (true) or a 0 (false) based on specific criteria.

2. **Conditions Checked**:
   - The first condition checks if the value in the column `[ReportReady]` is `TRUE`. This means that if the report is ready, the condition is satisfied.
   - The second condition checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means that if the timesheet code is one of these two specific codes, the condition is also satisfied.

3. **Output**:
   - If either of the conditions is true (i.e., the report is ready or the timesheet code is "V" or "Z"), the calculated column will return a value of **1**.
   - If neither condition is met, it will return a value of **0**.

### Summary:
In summary, this DAX expression is used to flag rows in the dataset. It indicates whether a report is ready or if the timesheet code is either "V" or "Z". A value of 1 means that one of these conditions is met, while a value of 0 means that neither condition is met. This can be useful for filtering or analyzing data based on readiness or specific timesheet codes.

##### Measures

**`ContractStatus2`**

- **DAX Expression:**
```dax
IF(
			    [ContractStatusMeasure] = "Active",
			    [HoursDifference],
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is a measure called `ContractStatus2`. Here's a breakdown of what it does in simple business terms:

1. **Condition Check**: The measure first checks the value of another measure called `[ContractStatusMeasure]`. It specifically looks to see if this value is equal to "Active".

2. **Return Value Based on Condition**:
   - If the condition is true (meaning the contract status is "Active"), the measure returns the value of another measure called `[HoursDifference]`. This likely represents the difference in hours related to the contract, such as the number of hours worked or remaining.
   - If the condition is false (meaning the contract status is not "Active"), the measure returns the text "Not Active".

### Summary:
In essence, `ContractStatus2` is designed to provide information about the status of a contract. If the contract is currently active, it shows the relevant hours associated with it. If the contract is not active, it simply indicates that by returning "Not Active". This helps users quickly understand the status of contracts and their associated hours.

**`ContractStatusMeasure`**

- **DAX Expression:**
```dax
IF(
			    ISBLANK(MAX('vw_Timesheet'[ContractEndDate])) || MAX('vw_Timesheet'[ContractEndDate]) > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `ContractStatusMeasure`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure is designed to determine the status of a contract based on its end date.

2. **Logic**:
   - The measure first checks the maximum value of the `ContractEndDate` from the `vw_Timesheet` table. This means it looks for the latest contract end date available in the data.
   - It then evaluates two conditions:
     - **Is Blank**: It checks if this maximum end date is blank (i.e., there is no end date recorded for the contract).
     - **Greater than Today**: It checks if the maximum end date is greater than today's date (meaning the contract is still ongoing).
   
3. **Outcomes**:
   - If either of these conditions is true (the end date is blank or it is still in the future), the measure returns "Active". This indicates that the contract is currently active and has not yet ended.
   - If neither condition is true (the end date is not blank and it is in the past), the measure returns "Not Active". This indicates that the contract has ended.

4. **Summary**: In summary, `ContractStatusMeasure` helps businesses quickly identify whether contracts are currently active or have ended, based on their end dates. This can be useful for tracking contract statuses and managing resources effectively.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 10, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated starting from January 1st. 

So, when you combine these two functions, `WEEKNUM(TODAY())` effectively tells you which week of the year it is right now. 

For example, if today is October 10, 2023, and it falls in the 41st week of the year, the expression will return the number 41. 

In business terms, this measure can be useful for reporting and analysis purposes, such as tracking weekly performance, sales, or other metrics that are evaluated on a weekly basis. It helps organizations understand where they stand in the current week relative to their goals or previous performance.

**`Dax_EmpCount_Approved`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[Approved] = FALSE()
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `Dax_EmpCount_Approved`. Let's break it down in simple business terms:

1. **Purpose of the Measure**: This measure is designed to count the number of unique employees whose timesheets have not been approved.

2. **Key Components**:
   - **`CALCULATE` Function**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **`DISTINCTCOUNT` Function**: This function counts the number of unique values in a specified column. Here, it counts unique employee names from the `vw_Timesheet` table.
   - **`vw_Timesheet[Approved] = FALSE()`**: This part of the code sets a filter condition. It specifies that only timesheets that are marked as "not approved" (i.e., where the `Approved` column is FALSE) should be considered in the count.

3. **What It Achieves**: 
   - The measure calculates how many distinct employees have submitted timesheets that have not been approved. This information can be useful for management to identify employees who may need follow-up regarding their timesheet approvals or to assess the overall approval process.

In summary, `Dax_EmpCount_Approved` provides a count of unique employees with unapproved timesheets, helping the business monitor and manage timesheet approvals effectively.

**`Dax_EmpCount_RReady`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[ReportReady] = FALSE()
			)
```
- **DAX Explanation (Generated):** The DAX expression you provided is used to create a measure called `Dax_EmpCount_RReady`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the number of unique employees whose timesheets are not marked as "Report Ready."

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows us to apply filters to our calculations.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it tells us how many different employees are present in the data.
   - **`vw_Timesheet[ReportReady] = FALSE()`**: This condition filters the data to include only those records where the `ReportReady` field is set to FALSE. In other words, it focuses on timesheets that are not ready for reporting.

3. **What It Achieves**: The overall result of this measure is to provide a count of distinct employees who have timesheets that are still pending or not ready for reporting. This can be useful for management to understand how many employees still need to submit or finalize their timesheets before reports can be generated.

In summary, `Dax_EmpCount_RReady` helps track the number of employees with incomplete timesheets, aiding in ensuring timely reporting and accountability.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheet entries in a dataset.

### Breakdown of the Expression:

- **DISTINCTCOUNT**: This function counts the number of distinct (unique) values in a specified column. In this case, it is counting unique entries.
  
- **vw_Timesheet[EmployeeName]**: This refers to a specific column in a table called `vw_Timesheet`, which contains the names of employees. 

### What It Achieves:

When you use this measure, it provides a total count of different employees who have logged their hours in the timesheet. For example, if three employees submitted timesheets, the result would be 3, even if some employees submitted multiple timesheets. This measure helps businesses understand how many individual employees are actively recording their work hours, which can be useful for tracking workforce engagement, resource allocation, and overall productivity.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the average hourly rate for sales based on data from a timesheet. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the `vw_Timesheet` table. Essentially, it totals the revenue generated from sales during a specific period.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked that are recorded in the same `vw_Timesheet` table. It represents the total number of hours that employees or team members have worked during that period.

3. **Division**: The entire expression divides the total sales amount by the total hours worked. This calculation gives you the average hourly rate, which tells you how much revenue is generated for each hour worked.

### In Summary:
The measure `MSR_AVG_HOURLY_RATE` calculates the average revenue earned per hour worked by dividing total sales by total hours. This metric helps businesses understand their efficiency and productivity by showing how effectively they are generating sales relative to the time invested.

**`Msr_Budget`**

- **DAX Expression:**
```dax
VAR CurrentWeek = SELECTEDVALUE(DimDate[WeekNumberOfYear])
			VAR PrevBudget = 
			    CALCULATE(
			        [Msr_SalesPriceBaseCurrency],  -- Use the measure directly
			        FILTER(
			            ALLSELECTED(DimDate),  
			            DimDate[WeekNumberOfYear] < CurrentWeek
			        )
			    )
			RETURN
			    IF(
			        NOT(ISBLANK([Msr_SalesPriceBaseCurrency])),
			        [Msr_SalesPriceBaseCurrency],  -- Use the measure directly
			        PrevBudget
			    )
```
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called `Msr_Budget`, which is designed to calculate a budget figure based on sales data, specifically focusing on the sales price in a base currency. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: 
   - The measure starts by determining the current week number of the year using `SELECTEDVALUE(DimDate[WeekNumberOfYear])`. This means it looks at the context of the data being analyzed (like a report or dashboard) to find out which week is currently selected or being viewed.

2. **Calculate Previous Budget**:
   - Next, it calculates a value called `PrevBudget`. This is done using the `CALCULATE` function, which allows for modifying the context in which a measure is evaluated. 
   - Inside `CALCULATE`, it uses the measure `[Msr_SalesPriceBaseCurrency]`, which presumably represents the sales price in a specific currency.
   - The `FILTER` function is applied to the entire date table (`ALLSELECTED(DimDate)`) to include all relevant dates while filtering out only those weeks that are less than the current week. This means it sums up the sales prices for all previous weeks leading up to the current week.

3. **Return the Appropriate Value**:
   - Finally, the measure uses an `IF` statement to decide what value to return:
     - If the current sales price measure (`[Msr_SalesPriceBaseCurrency]`) is not blank (meaning there is actual sales data for the current week), it returns that current sales price.
     - If the current sales price is blank (indicating no sales data for the current week), it returns the previously calculated budget (`PrevBudget`).

### Summary:
In summary, this measure calculates the budget for the current week based on sales data. If there are sales figures available for the current week, it uses those figures. If not, it falls back to the total sales price from all previous weeks, effectively providing a way to estimate the budget when current data is missing. This is useful for businesses to understand their performance against budget expectations, especially in scenarios where current sales data may not be available yet.

**`MSR_Cumulative Cost`**

- **DAX Expression:**
```dax
VAR SelectedMonth = MAX('DimDate'[MonthNumberOfYear])  -- Get the selected month in the visual
			RETURN 
			CALCULATE(
			    SUM(vw_Timesheet[CostAmount]),
			    FILTER(
			        ALLSELECTED('DimDate'),  
			        'DimDate'[MonthNumberOfYear] <= SelectedMonth  -- Include all months up to the current one
			    )
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the cumulative cost up to a selected month in a report or dashboard. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: 
   - The code starts by determining which month is currently selected in the visual (like a chart or table). It uses `MAX('DimDate'[MonthNumberOfYear])` to find the highest month number that has been selected. For example, if the user has selected data up to March, `SelectedMonth` would be 3.

2. **Calculate Cumulative Cost**:
   - The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to the selected month. This is done using the `CALCULATE` function, which allows for modifying the context of the calculation.

3. **Sum the Costs**:
   - Inside the `CALCULATE` function, it sums up the `CostAmount` from the `vw_Timesheet` table. This is the total cost that needs to be aggregated.

4. **Filter for Relevant Months**:
   - The `FILTER` function is used to create a condition that includes all months from the start of the year up to and including the selected month. The `ALLSELECTED('DimDate')` part ensures that the filter respects any other selections made in the report while still allowing for the cumulative calculation.

5. **Final Result**:
   - The final result of this measure is the total cost from the beginning of the year up to the month that the user has selected. This helps users understand how costs have accumulated over time and provides insights into spending trends.

In summary, this DAX measure helps businesses track and visualize their cumulative costs over time, allowing for better financial analysis and decision-making.

**`MSR_Cumulative Sales`**

- **DAX Expression:**
```dax
VAR SelectedMonth = MAX('DimDate'[MonthNumberOfYear])  -- Get the selected month in the visual
			RETURN 
			CALCULATE(
			    SUM(vw_Timesheet[SalesAmount]),
			    FILTER(
			        ALLSELECTED('DimDate'),  
			        'DimDate'[MonthNumberOfYear] <= SelectedMonth  -- Include all months up to the current one
			    )
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the cumulative sales amount up to a selected month in a report or dashboard. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: 
   - The code starts by determining which month is currently selected in the visual (like a chart or table). It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed.

2. **Calculate Cumulative Sales**:
   - The `CALCULATE` function is then used to compute the total sales amount from the 'vw_Timesheet' table. This is where the actual sales data is stored.

3. **Filter for Relevant Months**:
   - Inside the `CALCULATE` function, a `FILTER` is applied to the 'DimDate' table. The `ALLSELECTED` function ensures that any filters applied to the date dimension are respected, but it allows for the inclusion of all months up to and including the selected month.
   - Specifically, it checks that the month number in the 'DimDate' table is less than or equal to the selected month. This means that if you are looking at March, it will include sales from January, February, and March.

4. **Final Result**:
   - The result of this measure is the total sales amount for all months leading up to and including the selected month. This is useful for understanding how sales have accumulated over time, allowing businesses to track performance and trends effectively.

In summary, this DAX measure helps businesses visualize cumulative sales data up to a specific month, providing insights into sales growth and performance over time.

**`MSR_Cumulative_Revenue`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`Msr_HoursPercentage`**

- **DAX Expression:**
```dax
VAR TotalValue = 
			    CALCULATE(SUM(vw_Timesheet[Hours]), 
			        ALLSELECTED(vw_Timesheet[GroupCat], vw_Timesheet[Project], vw_Timesheet[EmployeeName])
			        )
			RETURN 
			    DIVIDE(SUM(vw_Timesheet[Hours]), TotalValue, 0)
```
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called `Msr_HoursPercentage`, which calculates the percentage of hours worked by a specific group, project, or employee relative to the total hours worked across all selected categories.

Here's a breakdown of what the code does in simple business terms:

1. **TotalValue Calculation**:
   - The first part of the code creates a variable called `TotalValue`. This variable calculates the total number of hours worked by summing up the `Hours` column from the `vw_Timesheet` table.
   - The `CALCULATE` function is used here to modify the context of the calculation. It considers all selected filters for `GroupCat`, `Project`, and `EmployeeName`, meaning it will sum the hours for all entries that are currently selected in those categories, regardless of any other filters that might be applied.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours worked for the current context (which could be a specific group, project, or employee) by dividing the sum of hours for that context by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safer way to handle division in DAX because it can return a specified value (in this case, 0) if the denominator (`TotalValue`) is zero. This prevents errors that could occur from dividing by zero.

### Summary:
In summary, this measure calculates what percentage of the total hours worked (across all selected categories) is represented by the hours worked in the current context (like a specific group, project, or employee). This helps businesses understand how much of their total effort is coming from different segments, allowing for better resource allocation and performance analysis.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the profit margin for a project by subtracting total costs from total sales. Here’s a breakdown of what it does in simple business terms:

1. **Total Sales Calculation**: The expression starts with `sum(vw_Timesheet[SalesAmount])`. This part adds up all the sales amounts recorded in the `vw_Timesheet` table. Essentially, it gives you the total revenue generated from the project.

2. **Total Costs Calculation**: Next, it looks at `sum(vw_Timesheet[CostAmount])`. This part sums up all the costs associated with the project, which includes expenses like labor, materials, and any other costs incurred.

3. **Profit Margin Calculation**: Finally, the expression subtracts the total costs from the total sales: 
   - `sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`. 
   This calculation results in the project margin, which represents the profit made from the project after accounting for all costs.

In summary, this DAX measure calculates the profit margin for a project by determining how much money is left after all costs are deducted from the total sales revenue. A positive result indicates a profitable project, while a negative result suggests a loss.

**`Msr_SalesPriceBaseCurrency`**

- **DAX Expression:**
```dax
SUMX(
			        SUMMARIZE(
			            vw_Timesheet, 
			            vw_Timesheet[ProjectProfile], 
			            vw_Timesheet[Client], 
			            "MeasureValue", SELECTEDVALUE(tbl_Project[SalesPriceBaseCurrency])
			        ), 
			        [MeasureValue]
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate a measure called `Msr_SalesPriceBaseCurrency`. Let's break it down into simpler terms to understand what it does:

1. **SUMX Function**: This function is used to iterate over a table and sum up values. In this case, it will sum up the results of a calculation for each row in a specified table.

2. **SUMMARIZE Function**: This function creates a new table that groups data based on specified columns. Here, it groups the data from `vw_Timesheet` by two columns: `ProjectProfile` and `Client`. This means that for each unique combination of `ProjectProfile` and `Client`, a new row will be created in the summarized table.

3. **SELECTEDVALUE Function**: This function retrieves the value of `SalesPriceBaseCurrency` from the `tbl_Project` table. If there is only one value available for the current context, it returns that value; otherwise, it returns blank. This is important because it ensures that the measure only pulls the relevant sales price for the current project being analyzed.

4. **"MeasureValue"**: This is a new column created in the summarized table that holds the value retrieved by the `SELECTEDVALUE` function. Essentially, for each unique combination of `ProjectProfile` and `Client`, it will store the corresponding sales price in this new column.

5. **Final Calculation**: The `SUMX` function then takes this summarized table and sums up all the values in the "MeasureValue" column. This gives the total sales price in the base currency for all the projects and clients represented in the `vw_Timesheet` table.

### In Summary:
The measure `Msr_SalesPriceBaseCurrency` calculates the total sales price in the base currency for all projects and clients listed in the `vw_Timesheet` table. It does this by grouping the data by project and client, retrieving the relevant sales price for each group, and then summing those prices together. This measure helps businesses understand their total sales revenue in a consistent currency, which is crucial for financial analysis and reporting.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'Static Total Employees'. Let's break it down into simple terms to understand what it does:

1. **CALCULATE Function**: This is a powerful function in DAX that allows you to modify the context in which data is evaluated. In this case, it is used to compute a specific count based on certain conditions.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part of the code counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it tells us how many different employees are present in the dataset.

3. **ALL(vw_Timesheet)**: The `ALL` function removes any filters that might be applied to the `vw_Timesheet` table. This means that the calculation will consider all records in the table, regardless of any other filters that might be in place in the report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This condition filters the data to only include employees whose contract status is marked as "Valid". It ensures that only employees with valid contracts are counted in the total.

### What It Achieves:
Putting it all together, this DAX measure calculates the total number of unique employees who currently have a valid contract, ignoring any other filters that might be applied to the data. This is useful for businesses to understand how many employees are actively engaged under valid contracts at any given time, providing a clear view of the workforce status.

**`TotalActualHours`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression `SUM(vw_Timesheet[Hours])` is used to calculate the total number of hours recorded in a timesheet. 

Here's a breakdown of what it does:

1. **Data Source**: It looks at a table called `vw_Timesheet`. This table likely contains records of time entries made by employees or team members.

2. **Column Reference**: Within this table, it specifically focuses on the column named `Hours`. This column contains numerical values representing the hours worked by individuals.

3. **Summation**: The `SUM` function adds up all the values in the `Hours` column. 

### What It Achieves:
- The result of this measure, `TotalActualHours`, gives you the total number of hours worked across all entries in the `vw_Timesheet` table. 
- This total can be used for various business purposes, such as tracking employee productivity, calculating payroll, or analyzing project time expenditures.

In summary, this DAX expression provides a straightforward way to aggregate and understand the total hours worked, which is essential for effective time management and reporting in a business context.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to calculate a measure called 'TotalContractedHours'. Let's break it down in simple business terms:

1. **SUMX Function**: This function is used to perform a calculation over a table and then sum up the results. In this case, it will iterate through a set of values and calculate a total based on those values.

2. **VALUES Function**: The `VALUES(vw_Timesheet[EmployeeName])` part creates a unique list of employee names from the 'vw_Timesheet' table. This means that for each employee, the calculation will be done separately.

3. **MAX Function**: The `MAX(vw_Timesheet[HoursPerWeek])` part looks at the 'HoursPerWeek' column for each employee and finds the maximum number of hours that are recorded for that employee. This is important because it ensures that if an employee has different entries for hours worked in a week, only the highest value is considered.

4. **Putting It All Together**: The entire expression calculates the total contracted hours by:
   - Going through each unique employee in the 'vw_Timesheet'.
   - For each employee, it finds the maximum hours they are contracted to work in a week.
   - Finally, it sums up these maximum hours across all employees.

### What It Achieves:
In summary, this DAX measure calculates the total maximum contracted hours for all employees based on their weekly hours recorded in the timesheet. This is useful for understanding the total capacity or commitment of all employees in terms of hours they are contracted to work, which can help in resource planning and management.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two measures: **TotalActualHours** and **TotalContractedHours**. 

Here's a breakdown of what this means in simple business terms:

1. **TotalActualHours**: This measure represents the total number of hours that have actually been worked or logged on a project or task. It reflects the real effort put in by employees or resources.

2. **TotalContractedHours**: This measure indicates the total number of hours that were agreed upon in a contract or plan for the project or task. It sets the expectation for how many hours should be worked based on the agreement.

3. **The Calculation**: By subtracting **TotalContractedHours** from **TotalActualHours**, the expression calculates the difference between what was actually worked and what was expected or contracted. 

4. **What It Achieves**: 
   - If the result is positive, it means that more hours were worked than were contracted, indicating potential overwork or additional effort beyond what was planned.
   - If the result is negative, it suggests that fewer hours were worked than expected, which could indicate underperformance or inefficiencies.
   - If the result is zero, it means that the actual hours worked matched the contracted hours, indicating that the project is on track in terms of time management.

In summary, this measure helps businesses understand how actual work hours compare to what was planned, allowing for better project management and resource allocation.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called 'TotalHoursTracked'. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The expression starts by checking if the total sum of hours from the 'vw_Timesheet' table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Zero if Blank**: If the sum of hours is blank (meaning no hours have been tracked), the measure will return a value of **0**. This is important because it ensures that when there are no recorded hours, the measure does not show a blank value, which could be confusing in reports.

3. **Return Total Hours if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the measure will return the actual total sum of hours tracked from the 'vw_Timesheet' table.

### Summary:
In essence, this measure calculates the total hours tracked by summing up the hours from the timesheet. If no hours are recorded, it returns 0 instead of leaving a blank, making it clearer for users to understand that no hours have been logged. This helps in reporting and analysis by providing a consistent output.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you've provided is a measure named `TotalHoursTrackedMissing`. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The measure first checks if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return "Empty" if No Hours**: If the sum of hours is indeed blank (meaning no hours have been tracked), the measure returns the text "Empty". This indicates that there is no data available for hours tracked.

3. **Return Total Hours if Present**: If there are hours recorded (i.e., the sum is not blank), the measure simply returns the total sum of hours tracked.

### Summary:
In essence, this measure helps to identify whether there are any hours recorded in the timesheet. If there are no hours, it clearly indicates that with the word "Empty". If there are hours, it provides the total number of hours tracked. This can be useful for reporting and analysis, allowing users to quickly understand if data is missing or to see the total hours worked.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is a measure named `trackedDiff`. Let's break it down in simple business terms to understand what it calculates or achieves.

### Breakdown of the Code:

1. **[TotalHoursTracked]**: This part of the measure represents the total number of hours that have been tracked for a specific context, such as a project, employee, or time period. It is likely a pre-defined measure that sums up all the hours recorded in a timesheet.

2. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to calculate a specific value based on certain conditions.

3. **MAXX Function**: This function is used to find the maximum value from a set of values. Here, it is applied to the distinct values of `HoursPerWeek` from the `vw_Timesheet` table.

4. **DISTINCT(vw_Timesheet[HoursPerWeek])**: This part retrieves all unique values of hours worked per week from the `vw_Timesheet` table. It ensures that only different hour values are considered in the calculation.

5. **ALLEXCEPT Function**: This function removes all filters from the `vw_Timesheet` table except for the filter on `EmployeeID`. This means that the calculation will consider all records for the same employee, regardless of other filters that might be applied (like project or date).

### What It Achieves:

The measure `trackedDiff` calculates the difference between the total hours tracked for a specific context (like an employee or project) and the maximum hours worked in a week by that same employee across all records in the timesheet.

### In Business Terms:

- **Purpose**: The measure helps to identify how much more or less time an employee has tracked compared to their maximum recorded hours in a week. 

- **Use Case**: For example, if an employee has tracked 50 hours in a given period, but their maximum recorded hours in any week is 40, the `trackedDiff` would show a positive value (10 hours). This indicates that the employee has exceeded their typical workload for that period.

- **Insight**: This can be useful for managers to understand workload trends, identify potential overwork situations, or assess productivity levels. It provides a clear view of whether employees are tracking more hours than they typically do, which can inform decisions about resource allocation and employee well-being.

**`trackedDiff2`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(
			            DISTINCT(vw_Timesheet[HoursPerWeek]), 
			            vw_Timesheet[HoursPerWeek]
			        ),
			        REMOVEFILTERS(vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates a measure called `trackedDiff2`, which is designed to determine the difference between the total hours tracked and a specific maximum value of hours per week for each employee. Here’s a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked by employees over a certain period.

2. **Maximum Hours Per Week**: The next part of the calculation uses the `CALCULATE` function to find the maximum number of hours that any employee has recorded in a week. This is done using `MAXX` and `DISTINCT`, which ensures that only unique weekly hour values are considered.

3. **Removing Filters**: The `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])` part of the code ensures that any existing filters on the `HoursPerWeek` column are ignored. This means that the calculation looks at all possible hours per week, not just those that might be filtered by other report elements.

4. **Keeping Employee Context**: The `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` function keeps the context of the calculation focused on individual employees. It allows the measure to calculate the maximum hours per week for each employee while ignoring other filters that might be applied to the `vw_Timesheet` table.

5. **Final Calculation**: Finally, the measure subtracts the maximum hours per week (calculated in the previous steps) from the total hours tracked. This gives the difference between the total hours an employee has logged and the highest number of hours they have recorded in any single week.

### Summary:
In summary, `trackedDiff2` calculates how many more or fewer hours an employee has tracked compared to their maximum weekly hours. If the result is positive, it indicates that the employee has logged more hours than their maximum in a week; if negative, they have logged fewer. This measure can help managers understand employee workload and time management.

**`UTI_TotalBillableHours`**

- **DAX Expression:**
```dax
Var FilteredHours =
			    Filter(
			        vw_Timesheet,
			        (
			        vw_Timesheet[QualifyPrj] = 1 && 
			        vw_Timesheet[BillablePrj] = 1) ||
			            SEARCH("Customer Success Services",vw_Timesheet[Project],1,0) > 0
			    )
			RETURN 
			    SUMX(FilteredHours, vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the total billable hours from a timesheet data source, specifically focusing on certain criteria that define what counts as "billable."

Here's a breakdown of what this code does in simple business terms:

1. **Variable Definition (FilteredHours)**: The code starts by creating a variable called `FilteredHours`. This variable holds a filtered version of the `vw_Timesheet` data, which is a view of timesheet records.

2. **Filtering Criteria**: The filtering is based on two main conditions:
   - **Qualifying Projects**: It checks if the project qualifies as billable by ensuring that the `QualifyPrj` field equals 1 (indicating it is a qualifying project) and the `BillablePrj` field also equals 1 (indicating it is a billable project).
   - **Specific Project Name**: It also includes any projects that contain the phrase "Customer Success Services" in their name. This is done using the `SEARCH` function, which looks for that specific text within the `Project` field.

3. **Calculating Total Hours**: After filtering the timesheet records based on the above criteria, the code then calculates the total hours worked on these filtered records. This is done using the `SUMX` function, which sums up the `Hours` field for each record that meets the filtering conditions.

4. **Final Output**: The result of this measure, `UTI_TotalBillableHours`, is the total number of hours that are considered billable based on the defined criteria. This measure helps the business understand how many hours can be billed to clients, particularly focusing on qualifying projects and specific customer success services.

In summary, this DAX expression effectively calculates the total billable hours from timesheets, ensuring that only relevant projects are included in the calculation.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `UTI_TOTALHOURS`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the total number of hours worked on specific projects that qualify for a certain criteria.

2. **Components**:
   - **SUM(vw_Timesheet[Hours])**: This part of the code adds up all the values in the `Hours` column from the `vw_Timesheet` table. Essentially, it totals the hours recorded in timesheets.
   - **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to include only those entries where the `QualifyPrj` column equals 1. This means it only considers projects that meet a specific qualification or criteria (indicated by the value 1).

3. **CALCULATE Function**: The `CALCULATE` function modifies the context in which the data is evaluated. In this case, it changes the context to only include rows from the `vw_Timesheet` table where the project qualifies (i.e., `QualifyPrj` is 1).

4. **Outcome**: The overall result of this measure is the total number of hours worked on projects that are marked as qualifying. This can be useful for reporting and analysis, helping businesses understand how much time is being spent on projects that meet certain criteria.

In summary, `UTI_TOTALHOURS` gives you a clear picture of the total hours worked on qualifying projects, helping in performance tracking and resource allocation.

**`UTILIZATION_New`**

- **DAX Expression:**
```dax
VAR _TotalBillableHours = [UTI_TotalBillableHours]
			VAR _TotalHours = [UTI_TOTALHOURS]
			RETURN
			    IF(NOT(ISBLANK([Msr_ActiveContacts])), 
			            IF(NOT(ISBLANK([UTI_TOTALHOURS])), 
			                IF(ISBLANK(DIVIDE(_TotalBillableHours, _TotalHours)),0,DIVIDE(_TotalBillableHours, _TotalHours))
			            )
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate a measure called 'UTILIZATION_New', which is likely aimed at assessing the utilization rate of resources (such as employees or contractors) in a business context. Here's a breakdown of what this code does in simple terms:

1. **Variables Defined**:
   - **_TotalBillableHours**: This variable retrieves the total number of billable hours worked by the resources. Billable hours are those hours that can be charged to clients or projects.
   - **_TotalHours**: This variable retrieves the total hours worked by the resources, which includes both billable and non-billable hours.

2. **Return Logic**:
   - The code uses a series of conditional checks (IF statements) to ensure that certain conditions are met before performing the calculation:
     - **Check for Active Contacts**: The first condition checks if there are any active contacts (likely referring to clients or projects). If there are no active contacts, the measure will not proceed with the calculation.
     - **Check for Total Hours**: The second condition checks if the total hours worked is not blank. If it is blank, the measure will not proceed with the calculation.
     - **Calculate Utilization Rate**: If both previous conditions are satisfied, the measure calculates the utilization rate by dividing the total billable hours by the total hours worked. If this division results in a blank (which can happen if total hours are zero), it returns 0 instead of a blank value.

3. **Final Outcome**:
   - The final result of this measure is the utilization rate expressed as a fraction or percentage. It tells you how effectively the resources are being utilized based on the hours they are working. A higher utilization rate indicates that a larger proportion of their working hours are billable, which is generally a positive indicator for a business.

In summary, this DAX measure calculates the utilization rate of resources by comparing their billable hours to their total hours worked, while ensuring that the calculation only occurs when there are active contacts and total hours available.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

The following filters are applied to the entire report:

- Filter on `DimDate`.`CurrentYearFlag` (Type: Advanced, Definition: `CurrentYearFlag` = `1L`)

### <a name="report-pages"></a>Report Pages

#### <a name="page-customer-analysis"></a>Page: Customer Analysis

*Internal Name: `ecc7667875c00809763b`, Ordinal: 0*

##### Page Level Filters

- Filter on `vw_Timesheet`.`BillablePrj` (Type: Advanced, Definition: `BillablePrj` = `1L`)

##### Visuals on this Page

**image**

- Type: `image`
- Name: `6f358e4969013934154d`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `e2414703060e087d3230`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `a60364499b7915da7e60`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)
  - `Min(DimDate.ShortDate)` (Role: Unknown)
  - `Max(DimDate.ShortDate)` (Role: Unknown)

**Unit**

- Type: `slicer`
- Name: `be723b7e75b4661912d0`
- Fields Used:
  - `Unit` (Query: `vw_Timesheet.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `e4e04919d0548163c0a8`
- Fields Used:
  - `SubUnit` (Query: `vw_Timesheet.Unit`) (Role: Values)

**Client**

- Type: `slicer`
- Name: `8048e854158dc07422ce`
- Fields Used:
  - `ClientName` (Query: `tbl_Project.ClientName`) (Role: Values)
- Visual Level Filters:
  - Filter on `tbl_Project`.`ClientName` (Type: TopN, Definition: `ClientName` IN ())

**Sub-Own-Ext**

- Type: `slicer`
- Name: `800e477135b21a02300b`
- Fields Used:
  - `Own-Sub-ExtT` (Query: `PBI_TIMESHEET.Own-Sub-ExtT`) (Role: Values)

**actionButton**

- Type: `actionButton`
- Name: `88e87e1eddb866e15c8a`
- Fields Used: _(None detected)_

**Project Type**

- Type: `slicer`
- Name: `b37c531703b3e683879e`
- Fields Used:
  - `ProjectProfile` (Query: `vw_Timesheet.ProjectProfile`) (Role: Values)

**Company**

- Type: `slicer`
- Name: `6e846506dec1641cb274`
- Fields Used:
  - `cc_Employer` (Query: `PBI_TIMESHEET.cc_Employer`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`cc_Employer` (Type: Categorical, Definition: `cc_Employer` IN (`'G Cloud NL'`))

**Project Name**

- Type: `slicer`
- Name: `9285b49f5742861eada8`
- Fields Used:
  - `Project` (Query: `vw_Timesheet.Project`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `b5b5047cc00ba23030e5`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**pivotTable**

- Type: `pivotTable`
- Name: `8a02ec339e5cc2bce97e`
- Fields Used:
  - `Year` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `Week` (Query: `DimDate.WeekNumberOfYear`) (Role: Columns)
  - `EmployeeName` (Query: `vw_Timesheet.EmployeeName`) (Role: Rows)
  - `Hours` (Query: `Sum(vw_Timesheet.Hours)`) (Role: Values)
  - `Revenue` (Query: `Sum(vw_Timesheet.SalesAmount)`) (Role: Values)
  - `Avg Hourly Rate` (Query: `vw_Timesheet.MSR_AVG_HOURLY_RATE`) (Role: Values)
  - `Project` (Query: `vw_Timesheet.Project`) (Role: Rows)
  - `Own-Sub-ExtT` (Query: `vw_Timesheet.Own-Sub-ExtT`) (Role: Rows)
  - `ClientName` (Query: `tbl_Project.ClientName`) (Role: Rows)

**Revenue/Week**

- Type: `lineChart`
- Name: `f5560d7670207c1c0572`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `SalesAmount` (Query: `Sum(vw_Timesheet.SalesAmount)`) (Role: Y)
- Visual Level Filters:
  - Filter on `DimDate`.`WeekNumberOfYear` (Type: Advanced, Definition: `WeekNumberOfYear` <> `4L`)
  - Filter on `vw_Timesheet`.`Sum(SalesAmount)` (Type: Advanced, Definition: NOT (`Sum(SalesAmount)` = `50L`))
  - Filter on `vw_missing_timesheet`.`BillableDep` (Type: Categorical, Definition: `BillableDep` IN (`1L`))

**pivotTable**

- Type: `pivotTable`
- Name: `4856952e0903150da5c7`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Rows)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Columns)
  - `Unit` (Query: `vw_Timesheet.MAIN_UNIT`) (Role: Rows)
  - `SubUnit` (Query: `vw_Timesheet.Unit`) (Role: Rows)
  - `EmployeeName` (Query: `vw_Timesheet.EmployeeName`) (Role: Rows)
  - `SalesAmount` (Query: `Sum(vw_Timesheet.SalesAmount)`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`MAIN_UNIT` (Type: Categorical, Definition: `MAIN_UNIT` IN (`'G Cloud'`))
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Definition: (Unparsed condition))

#### <a name="page-project-details"></a>Page: Project Details

*Internal Name: `2f52ad4a004b09107900`, Ordinal: 1*

##### Page Level Filters

- Filter on `vw_Timesheet`.`BillablePrj` (Type: Advanced, Definition: `BillablePrj` = `1L`)

##### Visuals on this Page

**image**

- Type: `image`
- Name: `3e3f0339e8619259ebb2`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `c58305c304407c75000a`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `30ac69436bb17c560e22`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)
  - `Min(DimDate.ShortDate)` (Role: Unknown)
  - `Max(DimDate.ShortDate)` (Role: Unknown)

**Pillar**

- Type: `slicer`
- Name: `1218b3e663e35022d9d5`
- Fields Used:
  - `DevoteamPillar` (Query: `tbl_Project.DevoteamPillar`) (Role: Values)

**Client**

- Type: `slicer`
- Name: `91d269b51621b58cd5b8`
- Fields Used:
  - `ClientName` (Query: `tbl_Project.ClientName`) (Role: Values)

**Company**

- Type: `slicer`
- Name: `bb8c1f8dd224d829d32b`
- Fields Used:
  - `cc_Employer` (Query: `PBI_TIMESHEET.cc_Employer`) (Role: Values)

**Status**

- Type: `slicer`
- Name: `331fc4a3b6d31c038b30`
- Fields Used:
  - `Status` (Query: `tbl_Project.Status`) (Role: Values)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `f4a2a804d812cd7dee49`
- Fields Used:
  - `Own-Sub-ExtT` (Query: `PBI_TIMESHEET.Own-Sub-ExtT`) (Role: Values)

**Project Type**

- Type: `slicer`
- Name: `c275d85b5800b1d47638`
- Fields Used:
  - `ProjectProfile` (Query: `vw_Timesheet.ProjectProfile`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`ProjectProfile` (Type: Categorical, Definition: N/A)

**Project Name**

- Type: `slicer`
- Name: `1221310b511e5b428280`
- Fields Used:
  - `Project` (Query: `vw_Timesheet.Project`) (Role: Values)

**Project Leader**

- Type: `slicer`
- Name: `9402ca390dab8c29e0ca`
- Fields Used:
  - `IPM_ManagerName` (Query: `vw_Timesheet.IPM_ManagerName`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `7e3b4de92424e0cc2bde`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**actionButton**

- Type: `actionButton`
- Name: `f3f386d06e37473759a7`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `0891520feb1b0937cd30`
- Fields Used:
  - `Budget` (Query: `CountNonNull(tbl_Project.SalesPriceBaseCurrency)`) (Role: Values)
  - `Year` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `Week` (Query: `DimDate.WeekNumberOfYear`) (Role: Columns)
  - `EmployeeName` (Query: `vw_Timesheet.EmployeeName`) (Role: Rows)
  - `Hours` (Query: `Sum(vw_Timesheet.Hours)`) (Role: Values)
  - `Revenue` (Query: `Sum(vw_Timesheet.SalesAmount)`) (Role: Values)
  - `AVG Hourly Rate` (Query: `vw_Timesheet.MSR_AVG_HOURLY_RATE`) (Role: Values)
  - `% Realization` (Query: `vw_Timesheet.MSR_PROJECT_%_REALIZATION`) (Role: Values)
  - `Margin` (Query: `vw_Timesheet.MSR_ProjectMargin`) (Role: Values)
  - `Project` (Query: `vw_Timesheet.Project`) (Role: Rows)
  - `Own-Sub-ExtT` (Query: `vw_Timesheet.Own-Sub-ExtT`) (Role: Rows)
  - `Status` (Query: `Min(tbl_Project.Status)`) (Role: Values)
  - `IPManager` (Query: `Min(vw_Timesheet.IPM_ManagerName)`) (Role: Values)
  - `ExpEnddate` (Query: `Min(tbl_Project.PlannedCompletionDate)`) (Role: Values)
  - `Project Type` (Query: `tbl_Project.ProjectGroupDescription`) (Role: Rows)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Definition: NOT (`EmployeeName` = null))

**Actual vs Budget**

- Type: `lineChart`
- Name: `76b4488a85c8253bac24`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Actual` (Query: `vw_Timesheet.MSR_Cumulative_Revenue`) (Role: Y)
  - `Budget` (Query: `vw_Timesheet.Msr_Budget`) (Role: Y)
- Visual Level Filters:
  - Filter on `DimDate`.`WeekNumberOfYear` (Type: Advanced, Definition: NOT (`WeekNumberOfYear` = null))
  - Filter on `vw_Timesheet`.`[MSR_Cumulative_Revenue]` (Type: Advanced, Definition: NOT (`[MSR_Cumulative_Revenue]` = null))

