# Power BI Model & Report Documentation

*Generated on: 2025-04-29 00:34:11*

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

The Power BI data model appears to be designed for a project management and timesheet tracking system, likely within a corporate or organizational context. It integrates various dimensions and fact tables to facilitate comprehensive reporting and analysis of employee hours, project performance, and resource allocation. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a focus on capturing and monitoring timesheet submissions, while the 'lkp_' tables (lookup tables) provide essential metadata for projects, units, and codes, enhancing the model's ability to categorize and filter data effectively.

The structure of the model indicates a star schema, where 'vw_Timesheet' serves as the central fact table linked to multiple dimension tables, including 'DimDate' for time-based analysis, and various lookup tables for project and unit classifications. This setup allows users to analyze hours worked and percentages against specific projects and units, while also identifying gaps in timesheet submissions through the 'vw_missing_timesheet' table. Overall, the model is likely aimed at improving operational efficiency, ensuring accurate project tracking, and facilitating data-driven decision-making within the organization.

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

---

### <a name="tables"></a>Tables

#### <a name="table-dimdate"></a>Table: `DimDate`

The 'DimDate' table serves as a comprehensive date dimension, providing essential temporal attributes that enable businesses to analyze and report on data across various timeframes. By offering detailed breakdowns of dates, including day, week, month, and quarter information, this table supports effective time-based analysis and enhances reporting capabilities in Power BI.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient date-based queries and data enrichment processes. |
| `DayName` | `string` | The 'DayName' column contains the full name of the day of the week corresponding to each date in the 'DimDate' table, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | The 'DayNumberOfMonth' column (int64) in the 'DimDate' table represents the numerical day of the month, ranging from 1 to 31, providing a granular reference for date-related analyses. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical value of the day of the week, with values ranging from 1 (Sunday) to 7 (Saturday), facilitating date-related analyses and reporting. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column (int64) in the 'DimDate' table represents the sequential day of the year, ranging from 1 to 365 (or 366 for leap years), facilitating date-related calculations and analyses. |
| `MonthName` | `string` | The 'MonthName' column contains the full names of the months, providing a human-readable representation of the month for each date entry in the DimDate table. |
| `MonthNumberOfYear` | `int64` | The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month (1 for January through 12 for December), facilitating time-based analysis and reporting. |
| `ShortDate` | `dateTime` | The 'ShortDate' column in the 'DimDate' table represents a simplified date format, capturing the specific date without time details to facilitate easier date-based analysis and reporting. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Indicates whether the date corresponds to the current calendar year, with values typically set to 'Yes' or 'No'.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CurrentYearFlag' in a data model, specifically within a date dimension table (DimDate). Here's what it does in simple business terms:

1. **Extract Year from Date**: The expression takes a date from the column `DimDate[ShortDate]` and extracts the year from it using the `YEAR` function.

2. **Compare with Current Year**: It then compares this extracted year to the current year, which is obtained using the `YEAR(TODAY())` function. `TODAY()` gives the current date, and `YEAR(TODAY())` gives the year of that date.

3. **Flagging**: If the year from the date in `DimDate[ShortDate]` is less than or equal to the current year, the expression returns a value of `1`. If the year is greater than the current year, it returns `0`.

### What It Achieves:
- The result is a flag (1 or 0) that indicates whether the date in `DimDate[ShortDate]` is from the current year or any previous year. 
- A value of `1` means the date is from the current year or earlier, while a value of `0` indicates the date is from a future year.

This flag can be useful for filtering or analyzing data based on whether dates fall within the current year or not, helping businesses focus on relevant time periods for reporting or decision-making.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table is designed to track and analyze the distribution of work hours and their corresponding percentages across various projects or tasks, enabling businesses to assess resource allocation and productivity effectively. This table facilitates insights into time management and operational efficiency by categorizing hours worked and their impact on overall performance metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column contains string representations of hours worked alongside their corresponding percentage contributions, providing a detailed view of employee time allocation and performance metrics. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and percentage calculations, facilitating enriched data analysis within the 'Hours_nd_Percentage' table. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, with associated qualification levels and sorting criteria to facilitate efficient data retrieval and reporting. This table enhances data integrity and consistency by providing a centralized source for code definitions and their hierarchical organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers used to categorize and enrich data within the lookup table 'lkp_Code'. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numeric flag to categorize data for enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table contains string values that determine the order in which records are displayed or processed, facilitating organized data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential details such as EmployeeId and EmployeeName to facilitate filtering and reporting across various business analytics and performance metrics. This table enhances data integrity and consistency by standardizing employee information used in other analytical processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column stores unique string identifiers for each employee, facilitating data enrichment and integration within the employee lookup table. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, serving as a key identifier for enriching employee-related data within the lkp_fltr_Employee table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, capturing essential attributes such as project names, qualification scores, billing amounts, and associated groups. This table enables businesses to analyze project performance and financial metrics, facilitating informed decision-making and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing amount associated with each project, facilitating financial tracking and analysis. |
| `Group` | `string` | The 'Group' column categorizes projects into distinct classifications, facilitating organized data management and retrieval within the lkp_Project table. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the lkp_Project table. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, with numerical values representing different levels of qualification criteria met. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing attributes such as billable department status and external classification. This table is essential for analyzing resource allocation and financial performance across different units, facilitating informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column represents the identifier for departments that are eligible for billing, facilitating the tracking of billable units within the organization. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column identifies the specific organizational unit within the company, facilitating data enrichment and analysis related to departmental structures. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores string values that represent external type identifiers for owned sub-units, facilitating data enrichment and classification. |
| `Unit` | `string` | The 'Unit' column contains string values representing the various measurement units used for quantifying data in the lkp_Unit table. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related data, capturing essential details such as project identifiers, descriptions, statuses, and leadership assignments. This table enables businesses to effectively manage and analyze their projects, facilitating better decision-making and resource allocation across various project groups.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key reference for project timelines and performance analysis within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column contains string values that represent the administrative details or oversight responsibilities associated with each project listed in the tbl_Project table. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates within the 'tbl_Project' table. |
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each project, facilitating the identification and management of client relationships within the project data. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating the tracking and management of client-related data within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated in the 'tbl_Project' table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project, represented as a string. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the exact date and time when each project entry was created in the 'tbl_Project' table. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or business unit associated with each project in the 'tbl_Project' table. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column represents the scheduled date and time for the completion of a project, facilitating project timeline management and tracking within the 'tbl_Project' table. |
| `Project` | `string` | The 'Project' column contains the names of various projects associated with the entries in the tbl_Project table, serving as a key identifier for tracking and managing project-related data. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed textual overview of each project, outlining its objectives, scope, and key deliverables within the 'tbl_Project' table. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the specific category or classification of a project within the 'tbl_Project' table, facilitating organization and management of related projects. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a textual summary of the project group, detailing its objectives and scope within the context of the overall project management framework. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for project-related data management and analysis. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the project's execution within the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency sales price of a project, stored as a decimal value to facilitate accurate financial calculations and reporting. |
| `Startdate` | `dateTime` | The 'Startdate' column records the date and time when a project is initiated, serving as a key reference point for project timelines and scheduling within the 'tbl_Project' table. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, allowing for effective tracking and management of project workflows. |
| `Statuscode` | `string` | The 'Statuscode' column in the 'tbl_Project' table indicates the current operational status of each project, represented as a string value to facilitate tracking and reporting. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, expressed as a decimal to accommodate precise financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table is designed to identify and track employees who have not submitted their timesheets for specific weeks, providing insights into potential compliance issues and resource management. It includes key details such as employee and manager information, contract dates, and hours, enabling managers to address missing submissions and ensure accurate payroll processing.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column represents the total number of hours worked by an employee, recorded as a double data type, within the context of identifying missing timesheet entries. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to terminate, providing essential information for tracking contract durations within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to assist in identifying discrepancies in timesheet submissions within the 'vw_missing_timesheet' view. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of employee contracts as of today, providing essential context for identifying missing timesheet entries in the 'vw_missing_timesheet' table. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet submissions. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on absent submissions. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by employees that have not been recorded in their timesheets. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_missing_timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and tracking of missing submissions. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's manager, providing context for accountability and oversight related to missing timesheet entries. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee time tracking. |
| `Project` | `string` | The 'Project' column contains the names of projects associated with missing timesheet entries, facilitating the identification and tracking of unrecorded work hours. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project associated with the timesheet entries, facilitating the tracking and management of project-related hours. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected (true) or not (false) in the context of tracking missing timesheets. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for processing, with a value of true signifying readiness and false indicating it is not yet prepared. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total monetary value of sales transactions associated with the records in the 'vw_missing_timesheet' table, stored as a double for precision in financial calculations. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the context of timesheet data analysis. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with missing timesheet entries, formatted for concise reporting and analysis. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific sub-division or department associated with each entry in the missing timesheet records, facilitating targeted analysis and reporting. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' view. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the timesheet entries, providing context and details about the work performed during the recorded time period. |
| `Week_` | `int64` | The 'Week_' column (int64) represents the week number of the year associated with each entry in the 'vw_missing_timesheet' table, facilitating the identification and analysis of timesheet data by week. |
| `Year_` | `int64` | The 'Year_' column (int64) in the 'vw_missing_timesheet' table represents the calendar year associated with each entry, facilitating the analysis of timesheet data over time. |

##### Calculated Columns

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column indicates the department associated with billable hours for employees, facilitating the tracking and management of time entries in the context of missing timesheets.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'BillableDep'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'BillableDep' column from the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on existing relationships in the data model.

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of specific billable data, the default assumption is that it is billable.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that the item is considered billable.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that the item is not billable.

In summary, this DAX expression effectively categorizes items as billable or not based on the presence and value of related data. If there is no related data or if the related data indicates a non-zero billable amount, it marks the item as billable (1). If the related data indicates a zero billable amount, it marks the item as not billable (0).

**`CC_ActiveEmployees`** (`string`)

- **Description:** The 'CC_ActiveEmployees' column contains a string representation of the active employees in the cost center, used to identify personnel who may be missing timesheet submissions in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
IF(vw_missing_timesheet[ShortDate] >= vw_missing_timesheet[ContractStartDate] && 
			      (vw_missing_timesheet[ShortDate] <= vw_missing_timesheet[ContractEndDate] || ISBLANK(vw_missing_timesheet[ContractEndDate]))
			        , 1
			        , 0
			    )
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CC_ActiveEmployees' in a data model, specifically within a table named 'vw_missing_timesheet'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether an employee is considered "active" based on their contract dates and a specific date (referred to as 'ShortDate').

2. **Conditions Checked**:
   - **Contract Start Date**: It first checks if the 'ShortDate' (which likely represents a specific date, such as today or a reporting date) is on or after the employee's 'ContractStartDate'. This means the employee's contract must have started before or on this date for them to be considered active.
   - **Contract End Date**: Next, it checks if the 'ShortDate' is on or before the 'ContractEndDate'. This means the employee's contract must still be valid on this date. However, if the 'ContractEndDate' is blank (which could indicate that the employee's contract is ongoing or has no specified end), the employee is still considered active.

3. **Output**:
   - If both conditions are met (the 'ShortDate' is within the range of the contract dates), the expression returns a value of **1**, indicating that the employee is active.
   - If either condition is not met, it returns a value of **0**, indicating that the employee is not active.

In summary, this DAX expression effectively flags employees as active (1) or inactive (0) based on whether the current date falls within their contract period, allowing for better tracking of employee status in reports or dashboards.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to assist in identifying and addressing missing or unsubmitted timesheet data.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IncompleteFlag' in a data table named `vw_missing_timesheet`. 

Here's what it does in simple terms:

- It checks the value in the column `MissingHours` for each row in the `vw_missing_timesheet` table.
- If the value of `MissingHours` is less than 0, it means that there is an issue (specifically, it indicates that there are negative missing hours, which typically shouldn't happen).
- In this case, the expression will return a value of 1, which can be interpreted as a flag indicating that the entry is incomplete or problematic.
- If the value of `MissingHours` is 0 or greater, the expression will return 0, indicating that there is no issue with that entry.

In summary, this expression effectively flags rows where there is a problem with the recorded missing hours, helping to identify incomplete or erroneous data in the dataset.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit associated with each entry in the missing timesheet records, facilitating targeted analysis and reporting.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'Unit' column of the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on existing relationships in the data model.

2. **Handle Missing Values**: If the related 'Unit' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression will return the text "Unknown". This helps to identify cases where the unit information is missing.

3. **Return the Unit Name**: If there is a valid related 'Unit' value (i.e., it is not blank), the expression will return that value. This means that the calculated column will show the actual unit name associated with the record.

In summary, this DAX expression effectively populates the 'MAIN_UNIT' column with either the name of the unit from the related table or "Unknown" if no unit information is available. This ensures that users can easily identify records with missing unit data while also displaying the correct unit names where applicable.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column represents the year and week number in a string format, used to identify the specific week of the year for tracking timesheet submissions in the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX expression creates a new calculated column called 'YearWeek' that combines the year and the week number of a date from the 'ContractEndDate' field.

Here's a breakdown of what it does:

1. **YEAR([ContractEndDate])**: This part extracts the year from the 'ContractEndDate'. For example, if the date is December 15, 2023, this will return "2023".

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the 'ContractEndDate'. For instance, if the date is December 15, 2023, it might return "50" because it is the 50th week of the year.

3. **FORMAT(..., "00")**: This ensures that the week number is always displayed as two digits. So, if the week number is 5, it will be formatted as "05".

4. **Concatenation (&)**: The expression combines the year and the formatted week number into a single string, separated by a hyphen. So, if the year is "2023" and the week number is "50", the final result will be "2023-50".

In summary, this DAX expression generates a string that represents the year and week number of the 'ContractEndDate', formatted as "YYYY-WW". This can be useful for reporting or analysis purposes, allowing users to easily identify and group data by year and week.

##### Measures

**`Count_Negative_Consultant`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have recorded negative hours in a timesheet. Here's a breakdown of what it does in simple business terms:

1. **CALCULATE Function**: This function is used to modify the context in which data is evaluated. In this case, it is being used to apply a specific filter to the data.

2. **DISTINCTCOUNT**: This part of the expression counts the number of unique entries in the 'EmployeeName' column from the 'vw_missing_timesheet' table. Essentially, it tells us how many different employees are involved.

3. **Filter Condition**: The expression includes a filter that only considers rows where the 'MissingHours' value is less than 0. This means it is specifically looking for instances where employees have negative hours recorded, which could indicate an error or a need for correction in their timesheet.

In summary, this measure calculates the total number of unique employees who have negative hours logged in their timesheets. This can help a business identify potential issues with timesheet entries and ensure accurate reporting of employee hours.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have recorded negative missing hours in a dataset called 'vw_missing_timesheet'.

Here's a breakdown of what it does:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it counts the unique Employee IDs from the 'vw_missing_timesheet' table.

3. **Filter Condition**: The expression includes a filter that only considers rows where the 'MissingHours' value is less than 0. This means it is looking for instances where employees have reported negative hours, which could indicate an error or a specific situation that needs attention.

In summary, this measure calculates how many different employees have reported negative missing hours, helping the business identify potential issues with timesheet entries or attendance records.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of active employees who have missing timesheet hours for a specific period. Here's a breakdown of what it does in simple business terms:

1. **Data Source**: The expression uses a data source called `vw_missing_timesheet`, which likely contains records of employees' hours worked and their contracted hours.

2. **Summarization**: It first summarizes the data by grouping it based on several criteria:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a more specific division within the unit)

3. **Calculating Missing Hours**: For each group, it calculates two key metrics:
   - **MissingHours**: This is determined by taking the total hours worked (from `vw_missing_timesheet[Hours_]`) and subtracting the maximum contracted hours (`vw_missing_timesheet[ContractHours]`). If this value is negative, it indicates that the employee has not logged enough hours compared to what they are contracted for.
   - **MinActiveEmp**: This checks if the employee is active by finding the minimum value of `vw_missing_timesheet[CC_ActiveEmployees]`. If this value is 1, it means the employee is considered active.

4. **Filtering**: After summarizing, the expression filters the results to include only those employees who are:
   - Active (where `MinActiveEmp` equals 1)
   - Have negative missing hours (indicating they have not logged enough hours).

5. **Counting Active Employees**: Finally, it counts the number of rows that meet these criteria, which gives the total number of active employees who have missing timesheet hours.

In summary, this DAX measure calculates how many active employees have not recorded enough hours in their timesheets for a given period, helping the business identify potential issues with timesheet compliance.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of "missing hours" for active employees who have not met their contracted hours in a given week. Here's a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. This variable filters a summarized view of a dataset called `vw_missing_timesheet`, which contains information about employees, their hours worked, and their contracted hours.

2. **Summarizing Data**: Within the `SUMMARIZE` function, the code groups the data by key attributes such as employee name, year, week, unit, and subunit. It calculates two important metrics for each group:
   - **MissingHours**: This is calculated by taking the total hours worked by the employee in that week and subtracting the maximum contracted hours for that employee. If this value is negative, it indicates that the employee has not worked enough hours compared to what they are contracted for.
   - **MinActiveEmp**: This checks if the employee is active (represented by a value of 1 in the `CC_ActiveEmployees` column).

3. **Filtering for Relevant Employees**: The `FILTER` function then narrows down the summarized data to only include those employees who are active (where `MinActiveEmp` equals 1) and have negative missing hours (meaning they have not worked enough hours).

4. **Calculating Total Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered list of active employees and sums up their `MissingHours`. This gives the total number of hours that active employees are missing compared to their contracted hours.

In summary, this DAX measure calculates the total hours that active employees are short of their contracted hours for a specific week, helping the business identify potential issues with employee attendance or workload management.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total expected contract hours for employees on a weekly basis. Here's a breakdown of what it does in simple business terms:

1. **Data Source**: It uses a table called `vw_missing_timesheet`, which likely contains records of timesheets for employees, including their contract hours and the week associated with each entry.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key pieces of information:
   - The week (`vw_missing_timesheet[Week_]`)
   - The employee ID (`vw_missing_timesheet[EmployeeID]`)

   For each unique combination of week and employee, it calculates the maximum contract hours (`MAX(vw_missing_timesheet[ContractHours])`). This means if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

3. **Calculating Total**: After summarizing the data, the `SUMX` function iterates over the summarized table and sums up the calculated maximum contract hours for each employee per week.

4. **Final Result**: The final output of this measure is the total expected contract hours for all employees across all weeks, ensuring that if an employee has multiple records in a week, only the highest contract hours are counted.

In summary, this DAX measure helps in understanding the total expected hours that employees are contracted to work each week, while ensuring that duplicate entries for the same week do not inflate the total.

**`MISSING%`**

- **DAX Expression:**
```dax
DIVIDE([Dax_EmpCount]-[Dax_EmpCount_MissingTS],[Dax_EmpCount]) * 100
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of employees that are not missing time stamps (TS) compared to the total number of employees. Here's a breakdown of what it does:

1. **[Dax_EmpCount]**: This represents the total count of employees.

2. **[Dax_EmpCount_MissingTS]**: This represents the count of employees who are missing time stamps.

3. **[Dax_EmpCount] - [Dax_EmpCount_MissingTS]**: This part calculates the number of employees who have valid time stamps by subtracting the count of missing time stamps from the total employee count.

4. **DIVIDE(..., [Dax_EmpCount])**: The DIVIDE function takes the number of employees with valid time stamps and divides it by the total number of employees. This gives us a fraction that represents the proportion of employees with valid time stamps.

5. **... * 100**: Finally, multiplying by 100 converts this fraction into a percentage.

In summary, this measure calculates the percentage of employees who have valid time stamps, helping the business understand how many employees are accounted for in terms of time tracking. A higher percentage indicates better compliance with time tracking requirements.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked and contracted hours for employees or contractors. Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression finds the maximum number of contracted hours from the `vw_missing_timesheet` table. Contracted hours refer to the total hours that an employee or contractor is expected to work according to their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part finds the maximum number of hours actually recorded (or worked) by the employees or contractors from the same table.

3. **Subtraction**: The expression then subtracts the maximum recorded hours from the maximum contracted hours. 

**What it achieves**: The result of this calculation gives you the maximum number of hours that are missing or unaccounted for. In other words, it shows how many hours an employee or contractor has not worked compared to what they were supposed to work according to their contract. This measure helps in identifying gaps in timesheet reporting and can be useful for managing workforce productivity and ensuring compliance with contractual obligations.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of completeness regarding timesheet submissions for employees. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part adds up all the missing hours from a table that tracks timesheets. Essentially, it tells us how many hours employees have not reported.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that employees are expected to work in a week according to their contracts.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the expression calculates the difference between the total missing hours and the expected hours. If the result is positive, it indicates that the missing hours exceed what was expected, suggesting a shortfall in timesheet submissions.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The DIVIDE function takes the result from the previous step (the difference) and divides it by the expected contract hours. This gives us a ratio or percentage. The third argument (0) ensures that if the expected hours are zero, the result will be zero instead of causing an error.

In summary, this DAX measure calculates how much of the expected working hours are not being reported in timesheets, expressed as a percentage. A higher percentage indicates a greater issue with timesheet completeness, while a lower percentage suggests better compliance with reporting hours worked.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total number of unique employees who have missing timesheets, grouped by their respective units and weeks. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The expression uses a table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including whether their submissions are complete or incomplete.

2. **Grouping**: The `SUMMARIZE` function is used to create a summary table that groups the data by three key columns:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This is the name of the employee.
   - `MAIN_UNIT`: This indicates the main unit or department the employee belongs to.

3. **IncompleteFlag**: For each combination of week, employee, and unit, the expression calculates a new column called "IncompleteFlag". This column uses the `MAX` function to determine if there is any instance of an incomplete timesheet for that employee in that week. If there is at least one incomplete timesheet, the flag will be set to 1 (true); otherwise, it will be 0 (false).

4. **Summation**: Finally, the `SUMX` function iterates over the summarized table and sums up the values in the "IncompleteFlag" column. This effectively counts how many unique employees had at least one missing timesheet during the specified weeks, across the different units.

In summary, this DAX measure provides a count of unique employees who have not submitted their timesheets correctly, organized by week and unit, helping management identify areas where compliance may be lacking.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This table enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column identifies the specific client associated with each timesheet entry, facilitating the tracking of billable hours and project management. |
| `Code2` | `string` | The 'Code2' column stores a secondary identifier or classification code related to timesheet entries, facilitating enhanced data categorization and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to terminate, providing essential information for timesheet management and workforce planning. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and payroll calculations within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract associated with the timesheet entry, represented as a string value. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, represented as a string, within the context of timesheet data analysis. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential temporal context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the monetary value associated with the hours worked, expressed as a double, within the timesheet data for accurate financial reporting and analysis. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, expressed as a double to allow for precise financial calculations. |
| `Currency` | `string` | The 'Currency' column (string) in the 'vw_Timesheet' table specifies the type of currency used for financial transactions recorded in the timesheet. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the individual or entity responsible for payment, providing essential identification for billing and financial tracking within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee within the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column contains the concatenated identifier and name of employees, facilitating easy identification and tracking of timesheet entries within the 'vw_Timesheet' view. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating the identification of personnel for time tracking and reporting purposes. |
| `Employer` | `string` | The 'Employer' column contains the name of the organization or company that employs the individual associated with the timesheet entries. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating the tracking and management of employee work hours by organization. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to the timesheet entries, providing further insights into the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with the recorded timesheet entries, facilitating financial analysis and reporting. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, allowing for precise tracking of labor input. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, providing insights into workload and resource allocation. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee time worked. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee work hours. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each record, facilitating efficient data retrieval and organization within the timesheet records. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries, facilitating accountability and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the name associated with the IPM ID, providing a reference for identifying specific project management instances within the timesheet data. |
| `JobTitle` | `string` | The 'JobTitle' column contains the designation or role of the employee, providing context for their responsibilities and tasks recorded in the timesheet. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking records within the 'vw_Timesheet' table, facilitating data integration and enrichment processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_Timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, facilitating the identification of managerial oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table uniquely identifies the manager associated with each timesheet entry, facilitating the tracking and management of employee work hours and approvals. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct manager, facilitating the identification of supervisory relationships within the timesheet data. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries, detailing the hours allocated to various tasks within the timesheet records. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each timesheet entry, allowing for detailed tracking of time spent on various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with the timesheet entries, facilitating the tracking and reporting of time spent on specific projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that represents the specific profile or characteristics of a project associated with the timesheet entries, providing context for resource allocation and project management. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the specific project profile associated with each timesheet entry, facilitating project tracking and reporting. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into the types of work being performed. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of timesheet entries for accurate financial analysis and reporting. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current state of the timesheet entry, reflecting whether it is submitted, approved, or pending review. |
| `Taxed` | `boolean` | Indicates whether the reported timesheet entry is subject to taxation, with a value of true representing taxable entries and false for non-taxable entries. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, providing insights into resource allocation and project management. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours and related activities. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the tasks or activities recorded in the timesheet, providing context and details for each entry. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table specifies the measurement unit associated with the recorded time entries, facilitating accurate tracking and reporting of work hours. |
| `UnitCode` | `string` | The 'UnitCode' column represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating the categorization and analysis of labor data by department or team. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and record-keeping within the timesheet management system. |
| `YearWeek` | `string` | The 'YearWeek' column represents the year and week number in a string format, facilitating the organization and analysis of timesheet data by specific weekly periods. |

##### Calculated Columns

**`Approved_With_V_Z`** (`string`)

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheets, specifically denoting whether they have been approved with a specific validation or criteria labeled 'V_Z'.
- **DAX Expression:**
```dax
IF(
			    [Approved] = TRUE() || 
			    vw_Timesheet[TimesheetCode] IN {"V", "Z"}, 
			    1, 
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'Approved_With_V_Z'. Here's a breakdown of what it does in simple business terms:

1. **Condition Check**: The expression checks two conditions:
   - First, it looks at the column `[Approved]`. If this column has a value of `TRUE`, it means that the item (like a timesheet or request) has been approved.
   - Second, it checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These codes likely represent specific categories or types of timesheets.

2. **Output Values**: 
   - If either of the conditions is met (meaning the item is approved or it has a TimesheetCode of "V" or "Z"), the expression returns a value of `1`. This indicates a positive outcome, meaning the item is considered approved in this context.
   - If neither condition is met, it returns a value of `0`, indicating that the item is not approved.

3. **Purpose**: The overall purpose of this calculated column is to flag items as approved based on either their approval status or their specific TimesheetCode. This can help in reporting or filtering data to quickly identify which items are approved according to these criteria.

In summary, this DAX expression effectively creates a simple binary indicator (1 or 0) to show whether an item is approved based on specific conditions.

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column indicates the department associated with billable hours recorded in the timesheet, facilitating accurate tracking of revenue-generating activities.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'BillableDep'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'BillableDep' column from the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on the relationships defined in your data model.

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that in the absence of a specific billable dependency, it defaults to a value of 1.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it finds that the value is indeed not zero, it again returns **1**. This suggests that there is a valid billable dependency present.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**. This indicates that there is no billable dependency in this case.

### Summary:
In summary, this DAX expression effectively categorizes each row based on the presence and value of a related 'BillableDep'. It assigns a value of **1** if there is either no related entry or if the related entry has a non-zero value, and it assigns a value of **0** if the related entry is zero. This can be useful for determining whether a particular unit is considered billable based on its dependencies.

**`BillablePrj`** (`string`)

- **Description:** The 'BillablePrj' column identifies the project associated with billable hours recorded in the timesheet, facilitating accurate billing and project cost tracking.
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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'BillablePrj' in a data table called 'vw_Timesheet'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether a particular entry in the timesheet is considered "billable" or not. A billable entry is one that can be charged to a client or project.

2. **Conditions Checked**:
   - **Sales Price**: It first checks if the 'SalesPrice' for the entry is greater than 0. If there is a positive sales price, it indicates that the work done can be billed.
   - **Project Profile Code**: Next, it checks if the 'ProjectProfileCode' is equal to 81. This specific code likely represents a type of project that is always billable.
   - **Project Name**: Finally, it looks for the phrase "Customer Success Services" within the 'Project' field. If this phrase is found, it suggests that the project is related to customer success services, which are typically billable.

3. **Output**: 
   - If any of these conditions are true (i.e., if the sales price is positive, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of **1**. This indicates that the entry is billable.
   - If none of these conditions are met, it returns a value of **0**, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project name.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column contains the name of the employer associated with the timesheet entry, providing essential context for payroll and labor tracking.

**`Employee_WeeklyHours`** (`string`)

- **Description:** The 'Employee_WeeklyHours' column captures the total number of hours worked by each employee during the week, represented as a string for flexibility in formatting.
- **DAX Expression:**
```dax
CONCATENATE(
			    ('vw_Timesheet'[EmployeeName]),
			    " - " & ('vw_Timesheet'[HoursperWeek])
			)
```
- **DAX Explanation (Generated):** This DAX expression creates a new calculated column called 'Employee_WeeklyHours' that combines two pieces of information into a single text string for each employee.

Here's what it does step-by-step:

1. **Employee Name**: It takes the name of the employee from the column `EmployeeName` in the `vw_Timesheet` table.

2. **Hours per Week**: It retrieves the number of hours that employee works per week from the `HoursperWeek` column in the same table.

3. **Concatenation**: It combines (or concatenates) these two pieces of information into one string. The format of the resulting string is: "Employee Name - Hours per Week". For example, if the employee's name is "John Doe" and he works 40 hours per week, the resulting string would be "John Doe - 40".

In summary, this DAX expression effectively creates a clear and readable summary of each employee's name along with their weekly working hours, making it easier to view and understand employee work hours at a glance.

**`GroupCat`** (`string`)

- **Description:** The 'GroupCat' column categorizes timesheet entries into specific groups for enhanced reporting and analysis of time allocation across different projects or activities.
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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'GroupCat' in a data model, likely related to project management or timesheet tracking. Here's a breakdown of what it does in simple business terms:

1. **Lookup for Group**: The expression first checks if there is a corresponding 'Group' value for a project in the 'lkp_Project' table based on the project ID found in the 'vw_Timesheet' table. It does this using the `LOOKUPVALUE` function, which retrieves the 'Group' associated with the project.

2. **Check for Blank Value**: The `ISBLANK` function is used to determine if the result of the lookup is empty (i.e., there is no matching group for the project). The `NOT` function negates this check, meaning it will proceed if a group is found.

3. **Return Group or Category**: 
   - If a group is found (the lookup is not blank), the expression returns that group value.
   - If no group is found (the lookup is blank), it then checks if the project is billable by looking at the 'BillablePrj' column in the 'vw_Timesheet' table. If 'BillablePrj' equals 1, it means the project is billable, and the expression returns "Billable".
   - If 'BillablePrj' does not equal 1, it returns "Other Unbillable", indicating that the project is not billable and does not belong to any group.

In summary, this DAX expression categorizes each project in the 'vw_Timesheet' table into one of three categories:
- The specific group from the 'lkp_Project' table if it exists,
- "Billable" if the project is billable and has no associated group,
- "Other Unbillable" if the project is not billable and has no associated group. 

This helps in organizing and analyzing projects based on their billing status and group affiliation.

**`IsContractActive`** (`string`)

- **Description:** Indicates whether the contract associated with the timesheet entry is currently active, represented as a string value.
- **DAX Expression:**
```dax
IF(
			    ISBLANK('vw_Timesheet'[ContractEndDate]) || 'vw_Timesheet'[ContractEndDate] > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IsContractActive' in a data model, specifically for a table named 'vw_Timesheet'. Here's what it does in simple business terms:

1. **Checks Contract End Date**: The expression first checks if the 'ContractEndDate' field for each record in the 'vw_Timesheet' table is either blank (meaning there is no end date specified) or if the end date is in the future (greater than today's date).

2. **Determines Contract Status**:
   - If either of the above conditions is true (the contract has no end date or the end date is still ahead of today), it labels the contract as "Active".
   - If neither condition is true (meaning there is a specified end date that is in the past), it labels the contract as "Not Active".

3. **Outcome**: The result is a clear indication of whether each contract is currently active or not, which can be useful for reporting, analysis, and decision-making regarding contracts in the organization. 

In summary, this expression helps identify which contracts are still valid and ongoing based on their end dates.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit associated with each timesheet entry, facilitating the categorization and analysis of labor hours by department or team.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'MAIN_UNIT'. Here's a simple breakdown of what it does:

1. **Check for Blank Values**: The expression starts by checking if there is a related value in the 'lkp_Unit' table for the 'Unit' column. It uses the `ISBLANK` function to determine if this related value is empty or not.

2. **Return "Unknown" if Blank**: If the related 'Unit' value is blank (meaning there is no corresponding unit found), the expression will return the text "Unknown". This helps to clearly indicate that there is no valid unit associated with the current record.

3. **Return the Related Unit if Not Blank**: If there is a valid related 'Unit' value (i.e., it is not blank), the expression will return that specific unit value from the 'lkp_Unit' table.

In summary, this DAX expression effectively populates the 'MAIN_UNIT' column with either the corresponding unit name from the 'lkp_Unit' table or the word "Unknown" if no unit is found. This ensures that users can easily identify records that lack a valid unit, improving data clarity and usability.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) to facilitate time-based reporting and analysis of timesheet data.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to extract the month number from a date value found in the column named `TimesheetDate`. 

In simple terms, this means that for each entry in the `TimesheetDate` column, the expression will return a number that represents the month of that date. For example:

- If the date is January 15, 2023, the expression will return `1`.
- If the date is July 4, 2023, it will return `7`.
- If the date is December 25, 2023, it will return `12`.

This calculated column, named 'MonthNumber', helps in organizing or analyzing data based on the month, making it easier to perform further calculations or create reports that summarize information by month.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the unique employee identification numbers as strings, facilitating the association of timesheet entries with specific employees in the 'vw_Timesheet' view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to count the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

In simple business terms, this calculation achieves the following:

- It identifies how many different employees have recorded timesheets, ensuring that each employee is only counted once, regardless of how many times they appear in the data.
- This is useful for understanding workforce participation, tracking employee engagement, or analyzing the number of distinct contributors to a project or task over a specific period.

Overall, this expression helps businesses get a clear picture of their employee involvement by providing a count of unique employees.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column captures the classification of time entries as either owned, subcontracted, or external, providing clarity on the nature of the work recorded in the 'vw_Timesheet' table.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used to retrieve a value from a related table in a data model. Here's a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a calculated column within a table that has a relationship with another table. In this case, the current table is related to the `lkp_Unit` table.

2. **Purpose**: The expression fetches the value from the `OWN-Sub-ExtT` column of the `lkp_Unit` table. This means that for each row in the current table, it looks up the corresponding value in the `lkp_Unit` table based on the established relationship.

3. **Outcome**: The result is that the calculated column will contain the values from the `OWN-Sub-ExtT` column for each row, allowing you to enrich your current table with additional information from the `lkp_Unit` table.

In summary, this DAX expression helps to pull in specific data from a related table, enhancing the current dataset with relevant information that can be used for analysis or reporting.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column contains string values that categorize or qualify the projects associated with each timesheet entry, providing context for the work performed.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'QualifyPrj'. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Value**: The expression first checks if the 'Qualify' field from a related table called 'lkp_Project' is blank (i.e., has no value). This is done using the `ISBLANK` function.

2. **Return Values Based on the Check**:
   - If the 'Qualify' field is blank, the expression returns a value of **1**. This could indicate a default or fallback status, suggesting that if there is no qualification information available, it defaults to a value of 1.
   - If the 'Qualify' field is not blank, it retrieves and returns the actual value from the 'Qualify' field in the 'lkp_Project' table.

In summary, this DAX expression effectively ensures that every row in the 'QualifyPrj' column has a value. If there is no qualification data available for a project, it assigns a default value of 1; otherwise, it uses the existing qualification value. This helps maintain data integrity and provides a clear indication of project qualification status.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor allocation.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of resources in relation to the V-Z phase, represented as a string, within the context of timesheet data for project management and resource allocation.
- **DAX Expression:**
```dax
IF(
			    [ReportReady] = TRUE() || 
			    vw_Timesheet[TimesheetCode] IN {"V", "Z"}, 
			    1, 
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'RReady_With_V_Z'. Here's a simple breakdown of what it does:

1. **Condition Check**: The expression checks two conditions:
   - It first checks if the value of the column `[ReportReady]` is `TRUE`. This means that the report is ready for some action or processing.
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These are specific codes that likely represent certain types of timesheets.

2. **Output Values**: 
   - If either of the conditions is met (i.e., if the report is ready or if the timesheet code is "V" or "Z"), the expression returns a value of `1`. This indicates a positive condition where the item is considered "ready" in some context.
   - If neither condition is met, it returns a value of `0`, indicating that the item is not ready.

**In summary**, this DAX expression effectively flags records as "ready" (1) if the report is ready or if the timesheet code is either "V" or "Z". If neither condition is true, it flags them as "not ready" (0). This can be useful for filtering or categorizing data based on readiness for reporting or processing.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'ContractStatus2'. Here's a breakdown of what it does in simple business terms:

1. **Condition Check**: The expression first checks the value of another measure called `[ContractStatusMeasure]`. It specifically looks to see if this measure equals "Active".

2. **Return Value if Active**: If the condition is true (meaning the contract status is "Active"), the expression returns the value of another measure called `[HoursDifference]`. This could represent the number of hours related to the active contract, such as hours worked or hours remaining.

3. **Return Value if Not Active**: If the condition is false (meaning the contract status is not "Active"), the expression returns the text "Not Active".

In summary, this measure helps to determine the status of a contract. If the contract is active, it provides the relevant hours associated with it. If the contract is not active, it simply indicates that by returning "Not Active". This can be useful for reporting or analysis purposes, allowing users to quickly understand the status of contracts and their associated hours.

**`ContractStatusMeasure`**

- **DAX Expression:**
```dax
IF(
			    ISBLANK(MAX('vw_Timesheet'[ContractEndDate])) || MAX('vw_Timesheet'[ContractEndDate]) > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to determine the status of a contract based on its end date. Here’s a breakdown of what it does in simple business terms:

1. **Check for Contract End Date**: The expression first looks at the maximum value of the 'ContractEndDate' from the 'vw_Timesheet' table. This means it is checking the latest end date of any contract recorded in that table.

2. **Evaluate Conditions**:
   - **ISBLANK**: It checks if this maximum end date is blank (meaning there is no end date recorded).
   - **Comparison with Today**: It also checks if this maximum end date is greater than today’s date (meaning the contract is still ongoing).

3. **Determine Status**:
   - If either of the above conditions is true (the end date is blank or it is in the future), the measure returns "Active". This indicates that the contract is currently active and has not ended.
   - If neither condition is true (the end date is not blank and it is in the past), it returns "Not Active". This means the contract has ended.

In summary, this measure effectively categorizes contracts as either "Active" or "Not Active" based on their end dates, helping businesses quickly assess the status of their contracts.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 15, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated starting from the first week of the year.

So, when you combine these two functions, `WEEKNUM(TODAY())` will give you the week number for the current date. 

For example, if today is October 15, 2023, and it falls in the 42nd week of the year, the expression will return the number 42. 

In business terms, this measure can be useful for reporting and analysis purposes, allowing you to understand which week of the year you are currently in, which can help in tracking performance, planning, and making timely decisions.

**`Dax_EmpCount_Approved`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[Approved] = FALSE()
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees whose timesheets have not been approved. Here's a breakdown of what it does:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to change the filter context to focus on specific criteria.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it is counting the unique names of employees from the `vw_Timesheet` table.

3. **Filter Condition**: The expression includes a filter condition that specifies `vw_Timesheet[Approved] = FALSE()`. This means that the calculation will only consider timesheets that have not been approved (i.e., where the `Approved` field is marked as FALSE).

In summary, this DAX measure calculates the total number of distinct employees who have submitted timesheets that are still pending approval. This can be useful for tracking how many employees are waiting for their timesheet approvals, helping management understand the workload or efficiency of the approval process.

**`Dax_EmpCount_RReady`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[ReportReady] = FALSE()
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees whose timesheets are not marked as "Report Ready." 

Here's a breakdown of what it does:

1. **CALCULATE Function**: This function changes the context in which data is evaluated. In this case, it is used to modify the filter context for the calculation that follows.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it counts the unique names of employees from the `vw_Timesheet` table.

3. **Filter Condition**: The expression includes a filter condition that specifies only to consider those records where the `ReportReady` column is set to FALSE. This means it will only count employees whose timesheets are not ready for reporting.

In summary, this measure calculates the total number of distinct employees who have timesheets that are still pending or not ready for submission. This can be useful for tracking which employees still need to finalize their timesheets before a reporting deadline.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheet entries in the `vw_Timesheet` table.

Here's a breakdown of what it achieves:

- **DISTINCTCOUNT**: This function counts only the unique values in a specified column. In this case, it ensures that each employee is counted only once, regardless of how many times they appear in the timesheet records.

- **vw_Timesheet[EmployeeName]**: This specifies the column from which the unique employee names are being counted. It refers to the names of employees who have submitted timesheets.

In simple terms, this measure tells you how many different employees have logged their hours in the timesheet system. It helps businesses understand employee participation and engagement in timesheet reporting.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_%_Budget_Used`**

- **DAX Expression:**
```dax
DIVIDE(SUM(vw_Timesheet[SalesAmount]), SUM('tbl_Project'[SalesPriceBaseCurrency]), 0)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of the budget that has been used based on sales data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part adds up all the sales amounts recorded in the 'vw_Timesheet' table. Essentially, it totals the revenue generated from sales.

2. **SUM('tbl_Project'[SalesPriceBaseCurrency])**: This part sums up the total budget or sales price from the 'tbl_Project' table. This represents the total amount that was budgeted or expected to be earned from the projects.

3. **DIVIDE(..., ..., 0)**: The DIVIDE function takes the total sales amount (from the first part) and divides it by the total budget (from the second part). If the budget is zero (to avoid division by zero errors), it returns 0 instead of an error.

In simple terms, this measure calculates how much of the budget has been used by comparing actual sales to the budgeted sales. The result is a percentage that indicates the proportion of the budget that has been utilized based on actual sales performance. If the result is 100%, it means the entire budget has been used; if it's less than 100%, it indicates that there is still budget remaining.

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the timesheet. Essentially, it gives you the total revenue generated from sales over a specific period.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the timesheet. It represents the total number of hours that employees or team members have worked during the same period.

3. **Division**: The expression then divides the total sales amount by the total hours worked. This calculation gives you the average hourly rate, which tells you how much revenue is generated for each hour worked.

In summary, this DAX measure calculates the average revenue earned per hour worked, helping businesses understand their efficiency and productivity in generating sales relative to the time invested.

**`Msr_Budget`**

- **DAX Expression:**
```dax
VAR CurrentWeek = SELECTEDVALUE(DimDate[WeekNumberOfYear])
			-- Find the most recent available budget value dynamically
			VAR PrevBudget = 
			    CALCULATE(
			        [Msr_SalesPriceBaseCurrency],  -- Use the measure directly
			        FILTER(
			            ALLSELECTED(DimDate),  
			            DimDate[WeekNumberOfYear] < CurrentWeek
			        )
			    )
			-- Return current measure if available; otherwise, use the previous budget value
			RETURN
			    IF(
			        NOT(ISBLANK([Msr_SalesPriceBaseCurrency])),
			        [Msr_SalesPriceBaseCurrency],  -- Use the measure directly
			        PrevBudget
			    )
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'Msr_Budget', which dynamically determines the budget value based on the current week in the year. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: The code starts by capturing the current week number from a date dimension table (DimDate). This is done using the `SELECTEDVALUE` function, which retrieves the week number that is currently being analyzed.

2. **Calculate Previous Budget**: Next, it calculates the budget for the previous weeks. It does this by using the `CALCULATE` function to sum up the measure `[Msr_SalesPriceBaseCurrency]` (which represents the sales price in base currency) for all weeks that are earlier than the current week. The `FILTER` function is used to ensure that only weeks before the current week are considered.

3. **Return the Appropriate Value**: Finally, the code checks if the current measure `[Msr_SalesPriceBaseCurrency]` has a value (i.e., it is not blank). If it does have a value, it returns that value as the budget for the current week. If it does not have a value (meaning there is no budget set for the current week), it falls back to the previously calculated budget value from earlier weeks.

In summary, this DAX measure effectively provides the budget for the current week if available; if not, it defaults to the most recent budget from previous weeks. This approach ensures that the analysis always has a relevant budget figure to work with, enhancing decision-making based on the most current data available.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the cumulative cost up to a selected month in a reporting visual. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It uses the `MAX` function on the `MonthNumberOfYear` from the `DimDate` table to find the highest month number that has been selected. This means if you are looking at data for March, the `SelectedMonth` would be 3.

2. **Calculate Cumulative Cost**: The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to and including the selected month. 

3. **Using CALCULATE**: The `CALCULATE` function is used to modify the context in which the data is evaluated. It sums up the `CostAmount` from the `vw_Timesheet` table.

4. **Filtering the Data**: The `FILTER` function is applied to the `DimDate` table. It uses `ALLSELECTED` to ensure that any filters applied to the date table (like year or other date-related filters) are respected. The filter condition checks that the month number is less than or equal to the `SelectedMonth`. This means it will include all months from January up to the selected month.

5. **Final Result**: The result of this measure is the total cost accumulated from the start of the year through the selected month. For example, if the selected month is March, it will sum the costs for January, February, and March.

In summary, this DAX measure helps users understand how much cost has been incurred cumulatively up to a specific month, providing valuable insights into spending trends over time.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the cumulative sales amount up to a selected month in a report or visual. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It does this by finding the maximum value of the 'MonthNumberOfYear' from the 'DimDate' table. This means if you are looking at data for March, the `SelectedMonth` variable will hold the value for March (which is 3).

2. **Calculate Cumulative Sales**: The main goal of this measure is to calculate the total sales amount from the 'vw_Timesheet' table for all months up to and including the selected month. 

3. **Use of CALCULATE and FILTER**: 
   - The `CALCULATE` function changes the context in which the data is evaluated. 
   - The `FILTER` function is used to create a new context that includes all months from the 'DimDate' table that are less than or equal to the `SelectedMonth`. This means if March is selected, it will include sales from January, February, and March.

4. **Summing Sales Amount**: Finally, the expression sums up the 'SalesAmount' from the 'vw_Timesheet' table for the filtered months. 

In summary, this DAX measure calculates the total sales from the beginning of the year up to the month that is currently selected in the report, allowing users to see cumulative sales performance over time.

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
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of hours worked by a specific group, project, or employee compared to the total hours worked across all selected categories in a report or dashboard.

Here's a breakdown of what it does:

1. **TotalValue Calculation**: 
   - The variable `TotalValue` is created to hold the total sum of hours from the `vw_Timesheet` table. 
   - The `CALCULATE` function is used to modify the context of the calculation. It sums up the `Hours` column while ignoring any filters applied to the `GroupCat`, `Project`, and `EmployeeName` columns. This means it calculates the total hours across all groups, projects, and employees that are currently selected in the report.

2. **Percentage Calculation**:
   - The `RETURN` statement uses the `DIVIDE` function to calculate the percentage of hours for the current context (the specific group, project, or employee being analyzed).
   - It takes the sum of hours for the current context (using `SUM(vw_Timesheet[Hours])`) and divides it by the `TotalValue` calculated earlier.
   - If `TotalValue` is zero (which would mean there are no hours to compare against), the `DIVIDE` function will return 0 instead of causing an error.

In summary, this measure provides insight into how much of the total hours worked is attributed to a specific selection, allowing users to understand the contribution of different groups, projects, or employees in relation to the overall workload.

**`MSR_PROJECT_%_REALIZATION`**

- **DAX Expression:**
```dax
IF(sum(tbl_Project[SalesPriceBaseCurrency]) <> 0, sum(vw_Timesheet[SalesAmount])/sum(tbl_Project[SalesPriceBaseCurrency]))
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called 'MSR_PROJECT_%_REALIZATION', which essentially assesses the percentage of sales realization for a project. Here’s a breakdown of what it does in simple business terms:

1. **Check for Sales Price**: The expression starts with an `IF` statement that checks if the total sales price (from the `tbl_Project` table) is not equal to zero. This is important because if the sales price is zero, it would not make sense to calculate a percentage, as you cannot divide by zero.

2. **Calculate Sales Amount**: If the sales price is greater than zero, the expression proceeds to calculate the percentage of realization. It does this by taking the total sales amount (from the `vw_Timesheet` table) and dividing it by the total sales price.

3. **Result**: The result of this calculation gives you the percentage of the sales amount realized compared to the total sales price for the project. This percentage helps in understanding how much of the expected revenue (sales price) has actually been achieved (sales amount).

In summary, this DAX measure helps project managers or business analysts evaluate the effectiveness of a project in terms of revenue realization, providing insights into how well the project is performing financially.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the profit margin for a project by subtracting total costs from total sales. Here's a breakdown of what it does:

1. **Total Sales Calculation**: The expression starts with `sum(vw_Timesheet[SalesAmount])`, which adds up all the sales amounts recorded in the `vw_Timesheet` table. This represents the total revenue generated from the project.

2. **Total Costs Calculation**: Next, it calculates the total costs with `sum(vw_Timesheet[CostAmount])`. This sums up all the costs associated with the project, which could include expenses like labor, materials, and other overheads.

3. **Profit Margin Calculation**: Finally, the expression subtracts the total costs from the total sales: `sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`. The result of this calculation is the project margin, which indicates how much profit the project has generated after covering its costs.

In simple terms, this DAX measure helps businesses understand how profitable a project is by showing the difference between what they earned (sales) and what they spent (costs). A positive result indicates a profit, while a negative result indicates a loss.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total sales price in the base currency for different projects and clients from a timesheet view. Here’s a breakdown of what it does in simple business terms:

1. **SUMMARIZE Function**: The expression starts by creating a summary table from the `vw_Timesheet` data. This summary groups the data by two key dimensions: `ProjectProfile` and `Client`. Essentially, it organizes the data so that we can look at sales prices for each unique combination of project and client.

2. **SELECTEDVALUE Function**: For each group created in the summary, it retrieves the sales price in the base currency from the `tbl_Project` table. The `SELECTEDVALUE` function is used here to get the specific sales price for the current context of the project being evaluated. If there is more than one sales price available, it will return a blank.

3. **MeasureValue**: The result of the `SELECTEDVALUE` function is stored as a new column called "MeasureValue" in the summarized table. This column represents the sales price in the base currency for each project-client combination.

4. **SUMX Function**: Finally, the `SUMX` function iterates over the summarized table and adds up all the values in the "MeasureValue" column. This gives the total sales price in the base currency across all the projects and clients included in the timesheet.

In summary, this DAX measure calculates the total sales price in the base currency for all projects and clients listed in the timesheet, providing a clear view of revenue based on the selected projects.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of unique employees who have a "Valid" contract status on a specific day. Here’s a breakdown of what each part does:

1. **CALCULATE**: This function changes the context in which data is evaluated. It allows us to apply filters and perform calculations based on those filters.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it ensures that each employee is only counted once, even if they appear multiple times in the data.

3. **ALL(vw_Timesheet)**: This function removes any existing filters on the `vw_Timesheet` table. By doing this, it ensures that the count of employees is not affected by any other filters that might be applied in the report or dashboard. It gives a complete view of the data.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This is a filter condition that specifies we only want to consider employees whose contract status is "Valid" for the calculation. 

In summary, this DAX expression calculates the total number of unique employees who currently have a valid contract, ignoring any other filters that might be applied to the data. This is useful for understanding how many employees are actively valid under their contracts at a given time.

**`TotalActualHours`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression `SUM(vw_Timesheet[Hours])` is used to calculate the total number of hours recorded in a timesheet. 

Here's a breakdown of what it does:

- **SUM**: This function adds up all the values in a specified column.
- **vw_Timesheet**: This refers to a table (or view) that contains timesheet data.
- **[Hours]**: This is the specific column within the `vw_Timesheet` table that holds the number of hours worked.

In simple terms, this measure totals all the hours from the `Hours` column in the `vw_Timesheet` table. The result gives you the overall count of hours worked, which can be useful for tracking employee time, project hours, or overall labor costs.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the total contracted hours for all employees listed in the `vw_Timesheet` table. Here's a breakdown of what it does in simple business terms:

1. **VALUES(vw_Timesheet[EmployeeName])**: This part of the expression creates a unique list of employee names from the `vw_Timesheet` table. Essentially, it identifies each employee without duplicates.

2. **MAX(vw_Timesheet[HoursPerWeek])**: For each employee in the unique list, this function finds the maximum number of hours they are contracted to work in a week. This means if an employee has multiple entries for different weeks, it will take the highest number of hours they are scheduled to work.

3. **SUMX(...)**: The `SUMX` function then takes the unique list of employees and the maximum hours per week for each employee, and sums these maximum values together. 

In summary, this DAX measure calculates the total maximum contracted hours for all employees by summing up the highest weekly hours each employee is scheduled to work. This gives a clear picture of the total contracted hours across the organization, ensuring that if an employee has varying hours, only their highest contracted hours are counted.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression `[TotalActualHours] - [TotalContractedHours]` calculates the difference between two values: the total actual hours worked and the total contracted hours agreed upon.

In simple business terms, this measure helps to determine how many hours have been worked beyond or below what was originally contracted. 

- **TotalActualHours**: This represents the actual hours that employees or resources have worked on a project or task.
- **TotalContractedHours**: This is the number of hours that were initially agreed upon in the contract for the project or task.

By subtracting the total contracted hours from the total actual hours, the measure provides insight into whether the project is over budget in terms of hours (if the result is positive) or under budget (if the result is negative). This information is crucial for project management, as it helps in assessing performance, managing resources, and making informed decisions about future work or contracts.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'TotalHoursTracked' that calculates the total number of hours tracked in a timesheet. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the 'Hours' column of the 'vw_Timesheet' table. Essentially, it calculates the total hours worked.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (i.e., if there are no hours recorded). 

3. **IF(...)**: The IF function evaluates the result of the ISBLANK check. 
   - If the total hours are blank (meaning there are no hours tracked), it returns **0**. This ensures that instead of showing a blank value, it clearly indicates that no hours have been tracked.
   - If there are hours tracked (the total is not blank), it simply returns the total hours calculated.

In summary, this DAX measure ensures that when you look at the total hours tracked, you either see the actual total number of hours or a zero if no hours have been recorded. This helps in reporting and analysis by providing a clear and consistent output.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total hours tracked in a timesheet, but it includes a check to see if there are any hours recorded. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the `Hours` column of the `vw_Timesheet` table. Essentially, it calculates the total hours worked.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (meaning there are no hours recorded). 

3. **IF(...)**: The IF function evaluates the condition provided by ISBLANK. If the total hours are blank (i.e., there are no hours tracked), it returns the text "Empty". If there are hours tracked, it returns the total number of hours calculated.

In summary, this DAX expression is used to determine if any hours have been recorded in the timesheet. If no hours are recorded, it indicates this by returning "Empty". If hours are present, it provides the total number of hours tracked. This is useful for reporting and ensuring that data is available for analysis.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** This DAX expression calculates the difference between two values related to hours tracked for employees. Here's a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The expression starts with `[TotalHoursTracked]`, which represents the total number of hours that have been recorded or tracked for a specific employee or group of employees.

2. **Maximum Hours Per Week**: The second part of the expression uses the `CALCULATE` function to find the maximum number of hours worked in a week by an employee. It does this by looking at the `vw_Timesheet` table, specifically the `HoursPerWeek` column. The `MAXX` function is used here to ensure that it retrieves the highest value of hours worked in any week.

3. **Distinct Values**: The `DISTINCT` function is applied to ensure that only unique values of `HoursPerWeek` are considered when calculating the maximum. This means if an employee worked the same number of hours in multiple weeks, it will only count that value once.

4. **Filtering by Employee**: The `ALLEXCEPT` function is used to maintain the context of the calculation for each employee. It ensures that the maximum hours are calculated only for the specific employee identified by `EmployeeID`, ignoring any other filters that might be applied to the data.

5. **Final Calculation**: Finally, the expression subtracts the maximum hours worked in a week (calculated in the previous step) from the total hours tracked. This gives the difference between the total hours an employee has tracked and their highest weekly hours.

**In summary**, this DAX measure calculates how many hours an employee has tracked beyond their maximum hours worked in any single week. This can help in understanding if an employee is consistently working more hours than their peak weekly performance, which could indicate workload issues or overtime.

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
- **DAX Explanation (Generated):** The DAX expression you've provided calculates a measure called `trackedDiff2`, which essentially determines the difference between the total hours tracked and the maximum hours recorded per week for each employee, while ignoring any filters applied to the hours per week.

Here's a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts by referencing another measure called `[TotalHoursTracked]`. This represents the total number of hours that have been logged or tracked for a specific context, such as a project or a time period.

2. **Maximum Hours Per Week**: The next part of the expression calculates the maximum number of hours that have been recorded per week for each employee. This is done using the `MAXX` function, which looks at a distinct list of hours per week from the `vw_Timesheet` table.

3. **Removing Filters**: The `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])` part ensures that any filters applied to the hours per week are ignored. This means that the calculation will consider all possible hours per week, not just those that are currently filtered in the report or dashboard.

4. **Employee Context**: The `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` function keeps the context of the specific employee while removing other filters. This ensures that the maximum hours per week is calculated for each individual employee, regardless of any other filters that might be applied to the data.

5. **Final Calculation**: Finally, the measure subtracts the maximum hours per week (calculated in the previous steps) from the total hours tracked. This gives you the difference between what has been logged and the highest number of hours that could have been logged in a week for that employee.

In summary, `trackedDiff2` calculates how many hours an employee has tracked beyond their maximum recorded hours per week, providing insights into whether they are working more than their typical maximum workload.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total billable hours from a timesheet data source, specifically focusing on certain criteria that define what counts as "billable."

Here's a breakdown of what the code does:

1. **Variable Definition (FilteredHours)**: The code starts by creating a variable called `FilteredHours`. This variable holds a filtered version of the `vw_Timesheet` table.

2. **Filtering Criteria**: The filtering is based on two main conditions:
   - The project qualifies as billable if `vw_Timesheet[QualifyPrj]` equals 1 (indicating it is a qualifying project) **and** `vw_Timesheet[BillablePrj]` equals 1 (indicating it is a billable project).
   - Alternatively, it includes any project that contains the phrase "Customer Success Services" in its name. This is checked using the `SEARCH` function, which looks for that specific text within the `vw_Timesheet[Project]` field.

3. **Calculating Total Hours**: After filtering the timesheet data based on the above criteria, the `SUMX` function is used to sum up the `Hours` from the filtered results. `SUMX` iterates over each row in the `FilteredHours` variable and adds up the values in the `vw_Timesheet[Hours]` column.

4. **Return Value**: Finally, the expression returns the total of the billable hours that meet the specified conditions.

In summary, this DAX measure calculates the total number of hours worked on projects that are either explicitly marked as billable or are related to "Customer Success Services." This helps businesses understand how many hours are being billed to clients based on specific project criteria.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** This DAX expression is used to calculate the total number of hours worked on specific projects that meet a certain qualification criterion. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the values in the "Hours" column from the "vw_Timesheet" table. Essentially, it calculates the total hours recorded.

2. **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to only include rows where the "QualifyPrj" column equals 1. This means it focuses on projects that are marked as qualified.

3. **CALCULATE(...)**: The CALCULATE function modifies the context in which the data is evaluated. In this case, it applies the filter from the second part to the sum of hours. 

Putting it all together, this DAX expression calculates the total hours worked on projects that are considered qualified (where "QualifyPrj" equals 1). This is useful for reporting or analyzing the amount of time spent on projects that meet specific criteria, helping businesses understand resource allocation and project performance.

**`UTILIZATION_New`**

- **DAX Expression:**
```dax
-- Calculate the Billable Hour Ratio for Active Employees
			VAR _TotalBillableHours = [UTI_TotalBillableHours]
			VAR _TotalHours = [UTI_TOTALHOURS]
			RETURN
			    IF(NOT(ISBLANK([Msr_ActiveContacts])), 
			            IF(NOT(ISBLANK([UTI_TOTALHOURS])), 
			                IF(ISBLANK(DIVIDE(_TotalBillableHours, _TotalHours)),0,DIVIDE(_TotalBillableHours, _TotalHours))
			            )
			    )
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the "Billable Hour Ratio" for active employees within a business context. Here's a breakdown of what it does in simple terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable retrieves the total number of billable hours worked by employees, which is represented by the measure `[UTI_TotalBillableHours]`.
   - `_TotalHours`: This variable retrieves the total hours worked by employees, represented by the measure `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The code checks if there are any active contacts (employees) using the measure `[Msr_ActiveContacts]`. If there are no active contacts, the calculation does not proceed.
   - If there are active contacts, it then checks if the total hours worked (`_TotalHours`) is not blank (meaning there are hours recorded).
   - If total hours are available, it calculates the ratio of billable hours to total hours. This is done using the `DIVIDE` function, which safely divides `_TotalBillableHours` by `_TotalHours`. If the result of this division is blank (which could happen if `_TotalHours` is zero), it returns 0 instead of an error.

3. **Outcome**:
   - The final result of this measure is the Billable Hour Ratio, which indicates the proportion of hours that were billable compared to the total hours worked by active employees. This ratio is crucial for understanding how effectively employee time is being utilized in generating billable work, which can impact profitability and resource management.

In summary, this DAX expression helps businesses assess the efficiency of their active employees by calculating how much of their working time is spent on billable tasks.

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


---

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

**Actual vs Budget**

- Type: `lineChart`
- Name: `76b4488a85c8253bac24`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Actual` (Query: `vw_Timesheet.MSR_Cumulative_Revenue`) (Role: Y)
  - `Budget` (Query: `vw_Timesheet.Msr_Budget`) (Role: Y)


---

