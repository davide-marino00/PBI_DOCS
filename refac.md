# Power BI Model & Report Documentation

*Generated on: 2025-04-29 00:01:16*

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

The Power BI data model appears to be designed for a project management and timesheet tracking system, likely within a corporate or organizational context. It integrates various dimensions and fact tables to facilitate comprehensive reporting and analysis of employee hours, project performance, and resource allocation. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a focus on capturing and analyzing time entries, while the lookup tables ('lkp_Project', 'lkp_Unit', 'lkp_Code', and 'lkp_fltr_Employee') provide essential contextual information about projects, organizational units, and timesheet codes.

The 'DimDate' table indicates that the model supports time-based analysis, allowing users to track performance over different periods. Relationships between the timesheet views and the lookup tables enable detailed insights into project-specific hours worked, missing timesheet entries, and overall employee productivity. This structure likely supports decision-making processes related to project management, resource planning, and operational efficiency, making it a valuable tool for stakeholders aiming to optimize project execution and workforce management.

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

The 'DimDate' table serves as a comprehensive date dimension for analytical reporting, enabling businesses to perform time-based analysis by providing essential date attributes such as day, week, month, and quarter. This table facilitates the aggregation and filtering of data across various time periods, enhancing insights into trends and performance metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient date-based queries and data enrichment processes. |
| `DayName` | `string` | The 'DayName' column contains the full name of the day of the week corresponding to each date in the 'DimDate' table, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | The 'DayNumberOfMonth' column (int64) in the 'DimDate' table represents the numerical day of the month, ranging from 1 to 31, providing a granular reference for date-related analyses. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical value of the day of the week, where 1 corresponds to Sunday and 7 corresponds to Saturday, facilitating date-related analyses and reporting. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column (int64) in the 'DimDate' table represents the sequential day of the year, ranging from 1 to 365 (or 366 for leap years), facilitating date-related calculations and analyses. |
| `MonthName` | `string` | The 'MonthName' column contains the full names of the months, providing a human-readable representation of the month associated with each date in the 'DimDate' table. |
| `MonthNumberOfYear` | `int64` | The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month, ranging from 1 for January to 12 for December, facilitating time-based analysis and reporting. |
| `ShortDate` | `dateTime` | The 'ShortDate' column represents a simplified date format, capturing the specific date without time details, to facilitate easy date-based analysis within the DimDate dimension table. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Indicates whether the date corresponds to the current calendar year, with values typically set to 'Yes' or 'No'.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CurrentYearFlag' in a data model, specifically in a date dimension table (DimDate). Here's a breakdown of what it does in simple business terms:

1. **YEAR(DimDate[ShortDate])**: This part extracts the year from the date in the 'ShortDate' column of the DimDate table. For example, if the date is '2023-05-15', it will return '2023'.

2. **YEAR(TODAY())**: This part gets the current year based on today's date. If today is any date in 2023, this will return '2023'.

3. **IF(..., 1, 0)**: The IF function checks a condition. In this case, it checks if the year extracted from the 'ShortDate' is less than or equal to the current year. If this condition is true (meaning the date is in the current year or any previous year), it returns '1'. If the condition is false (meaning the date is in a future year), it returns '0'.

### What it achieves:
The 'CurrentYearFlag' column will have a value of '1' for all dates that are in the current year or any previous year, and '0' for any dates that are in the future. This allows users to easily identify which dates are relevant for the current year and prior years, helping in analyses that focus on historical data versus future projections.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table is designed to track and analyze the distribution of hours worked alongside their corresponding percentage contributions to overall performance metrics. This table aids in understanding workforce productivity and resource allocation, enabling informed decision-making for operational efficiency.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column contains string representations of hours worked alongside their corresponding percentage contributions, providing a detailed view of employee time allocation and performance metrics. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and percentage calculations, facilitating data enrichment and analysis within the 'Hours_nd_Percentage' table. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, with associated qualification levels and sorting criteria to facilitate data analysis and reporting. This table enhances data integrity and consistency by providing a standardized framework for code classification.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers used to categorize and enrich data within the lookup table 'lkp_Code'. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numerical flag to categorize data for enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table contains string values that determine the order in which records are displayed or processed, facilitating organized data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics. This table enhances data integrity and enables efficient data retrieval related to employee-specific insights and performance metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column stores unique identifiers for employees, represented as strings, to facilitate data enrichment and ensure accurate mapping within the employee lookup table. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, serving as a key identifier for enriching employee-related data within the lkp_fltr_Employee table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, detailing key attributes such as project names, qualification scores, billing amounts, and associated groups, enabling businesses to analyze project performance and financial metrics effectively.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing code or identifier associated with each project, facilitating financial tracking and reporting. |
| `Group` | `string` | The 'Group' column categorizes projects into specific classifications or teams within the 'lkp_Project' table, facilitating organized data management and retrieval. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the lkp_Project table. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, with integer values representing different qualification levels or criteria met. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their characteristics such as billable status and external affiliations. This table is essential for analyzing resource allocation and financial performance across different units, facilitating informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column represents the department identifier associated with billable units, facilitating the tracking and management of revenue-generating activities within the organization. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column contains the names of organizational units, serving to categorize and enrich data related to the structure of the organization. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores string values that represent external type identifiers for owned sub-units, facilitating data enrichment and classification processes. |
| `Unit` | `string` | The 'Unit' column stores the names of measurement units used for data enrichment, facilitating standardized reference across various datasets. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, leadership, and status. This table enables businesses to effectively manage and analyze their projects, facilitating better decision-making and resource allocation across various project groups.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and performance analysis within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column contains string values that represent the administrative details or oversight responsibilities associated with each project listed in the 'tbl_Project' table. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates within the 'tbl_Project' table. |
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each project, facilitating the identification and management of client relationships within the project data. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating the tracking and management of client-related data within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated in the 'tbl_Project' table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project, represented as a string. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the exact date and time when each project entry was created in the 'tbl_Project' table. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or business unit associated with each project in the 'tbl_Project' table. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column indicates the scheduled date and time for the completion of the project, facilitating project timeline management and tracking. |
| `Project` | `string` | The 'Project' column contains the names or identifiers of various projects, serving as a key reference for associating enriched data with specific initiatives within the 'tbl_Project' table. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed textual overview of each project, outlining its objectives, scope, and key deliverables within the 'tbl_Project' table. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the specific category or classification of a project within the 'tbl_Project' table, facilitating organization and management of related projects. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a detailed textual description of the project group associated with each entry in the 'tbl_Project' table, facilitating better understanding and categorization of projects. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) serves as a unique identifier for each project within the 'tbl_Project' table, facilitating efficient data retrieval and management. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the project, facilitating accountability and management within the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency sales price of a project, stored as a decimal value to facilitate accurate financial calculations and reporting. |
| `Startdate` | `dateTime` | The 'Startdate' column records the date and time when a project is initiated, serving as a key reference point for project timelines and scheduling within the 'tbl_Project' table. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the 'tbl_Project' table, allowing for effective tracking and management of project workflows. |
| `Statuscode` | `string` | The 'Statuscode' column in the 'tbl_Project' table indicates the current status of each project, represented as a string to facilitate tracking and reporting on project progress. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, stored as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table provides insights into employees who have not submitted their timesheets, detailing the specific weeks and dates affected. This information is crucial for managers to identify compliance issues, track employee attendance, and ensure accurate payroll processing.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column represents the total number of hours worked by an employee, recorded as a double data type, within the context of identifying missing timesheet entries in the 'vw_missing_timesheet' table. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract is set to expire, providing essential information for managing timesheet compliance and workforce planning within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to assist in identifying discrepancies in timesheet submissions within the 'vw_missing_timesheet' table. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries in the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract for each employee, providing essential context for identifying missing timesheet entries. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on absent submissions. |
| `Hours_` | `double` | The 'Hours_' column represents the total number of hours recorded for each entry in the 'vw_missing_timesheet' table, stored as a double to accommodate fractional hours. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the employee's timesheet entries, facilitating the tracking and management of timesheet submissions. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees' timesheet submissions, facilitating accountability and tracking within the 'vw_missing_timesheet' table. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee work hour reporting. |
| `Project` | `string` | The 'Project' column contains the names of projects associated with missing timesheet entries, facilitating the identification and resolution of unrecorded work hours. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project associated with the timesheet entries, facilitating the tracking and management of project-related hours. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness and false indicating it is not yet prepared. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total monetary value of sales transactions associated with the records in the 'vw_missing_timesheet' table, stored as a double for precision in financial calculations. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double data type, within the 'vw_missing_timesheet' table, which is designed to enhance data accuracy and completeness for financial reporting. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with missing timesheet entries, formatted to facilitate quick reference and analysis of attendance records. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific sub-division or department within the organization associated with each entry in the 'vw_missing_timesheet' table, facilitating targeted analysis of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for timesheets, facilitating the tracking and management of employee time entries within the 'vw_missing_timesheet' view. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the timesheet entries, providing context and details about the work performed during the reported period. |
| `Week_` | `int64` | The 'Week_' column (int64) represents the week number of the year associated with each entry in the 'vw_missing_timesheet' table, facilitating the identification and analysis of timesheet data by week. |
| `Year_` | `int64` | The 'Year_' column (int64) in the 'vw_missing_timesheet' table represents the calendar year associated with each timesheet entry, facilitating the analysis of timesheet data over time. |

##### Calculated Columns

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column indicates the department associated with billable hours for employees, helping to identify revenue-generating activities in the context of missing timesheet entries.
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

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of specific information, the default assumption is that the item is billable.

3. **Check for Non-Zero Values**: If there is a related 'BillableDep' value and it is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that the item is considered billable.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that the item is not billable.

In summary, this DAX expression effectively categorizes items as billable or not based on the related 'BillableDep' values. If there is no related data, it assumes the item is billable. If there is related data and it is non-zero, it also considers the item billable. If the related data is zero, it marks the item as not billable.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CC_ActiveEmployees' in a data table named 'vw_missing_timesheet'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether an employee is considered "active" based on their contract dates.

2. **Conditions Checked**:
   - **Start Date Check**: It first checks if the date in the column 'ShortDate' is on or after the 'ContractStartDate'. This means the employee's contract has started.
   - **End Date Check**: Next, it checks if the 'ShortDate' is on or before the 'ContractEndDate'. This means the employee's contract is still valid. However, if the 'ContractEndDate' is blank (meaning the employee has no end date specified), the employee is still considered active.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the end date is blank), the expression returns a value of **1**, indicating that the employee is active.
   - If either condition is not met, it returns **0**, indicating that the employee is not active.

In summary, this DAX expression effectively flags employees as active (1) or inactive (0) based on whether the current date falls within their contract period or if they have an ongoing contract without an end date. This helps in identifying which employees are currently active based on their contractual agreements.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to assist in identifying records that require further attention in the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IncompleteFlag' in a data table named `vw_missing_timesheet`. 

Here's what it does in simple terms:

- It checks the value in the column `MissingHours` for each row in the `vw_missing_timesheet` table.
- If the value of `MissingHours` is less than 0, it means that there are negative missing hours, which could indicate an issue or an incomplete record.
- In this case, the expression will return a value of 1, signaling that the record is flagged as incomplete.
- If the value of `MissingHours` is 0 or greater, the expression will return a value of 0, indicating that the record is complete or does not have any missing hours.

In summary, this expression effectively flags records as incomplete (1) or complete (0) based on whether the `MissingHours` value is negative.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression starts by checking if there is any related data in the 'lkp_Unit' table for the current row. Specifically, it looks at the 'Unit' column in that table.

2. **Handle Missing Data**: 
   - If there is no related data (meaning the 'Unit' value is blank), the expression will return the text "Unknown". This is a way to handle situations where the expected information is missing, ensuring that the report or analysis does not show an empty value.
   - If there is related data (meaning the 'Unit' value is present), it will return the actual value from the 'Unit' column in the 'lkp_Unit' table.

3. **Purpose**: The overall purpose of this expression is to ensure that every row in the 'MAIN_UNIT' column has a meaningful value. If the related unit information is available, it shows that value; if not, it clearly indicates that the unit is "Unknown". This helps maintain data integrity and clarity in reporting.

In summary, this DAX expression effectively provides a fallback option for missing unit information, enhancing the usability of the data by ensuring that users can easily identify when unit information is not available.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column represents the year and week number in a string format, facilitating the identification of specific time periods for tracking missing timesheet entries.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX expression creates a new calculated column called 'YearWeek' that combines the year and the week number of a date from the 'ContractEndDate' column.

Here's a breakdown of what it does:

1. **YEAR([ContractEndDate])**: This part extracts the year from the 'ContractEndDate'. For example, if the date is December 15, 2023, this will return "2023".

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the 'ContractEndDate'. For instance, if the date is December 15, 2023, it might return "50" because it's the 50th week of the year.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a specific measure called 'Count_Negative_Consultant'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The measure counts the number of unique employees (consultants) who have recorded negative hours in a timesheet.

2. **Components**:
   - **`CALCULATE`**: This function modifies the context in which data is evaluated. In this case, it is used to apply a filter to the data.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the number of unique employee names from the 'vw_missing_timesheet' table. It ensures that each employee is only counted once, even if they have multiple entries.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This condition filters the data to only include entries where the 'MissingHours' value is less than zero. This means it focuses on cases where employees have negative hours recorded, which could indicate issues such as over-reporting or errors in timesheet submissions.

3. **Outcome**: The final result of this measure is the total number of distinct employees who have negative hours logged in their timesheets. This information can be useful for management to identify potential problems with timesheet accuracy or to address issues with employee reporting.

In summary, this DAX expression helps organizations track and manage timesheet discrepancies by identifying how many unique consultants have reported negative hours.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX expression you've provided is designed to calculate a specific measure called 'CountNegativeMissingHours'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure counts the number of unique employees who have recorded negative missing hours in the 'vw_missing_timesheet' table.

2. **Components**:
   - **CALCULATE**: This function changes the context in which data is evaluated. In this case, it is used to apply a filter to the data.
   - **DISTINCTCOUNT**: This function counts the number of unique values in a specified column. Here, it counts unique 'EmployeeID' values.
   - **'vw_missing_timesheet'[MissingHours] < 0**: This is the filter condition applied by the CALCULATE function. It specifies that only records where the 'MissingHours' are less than zero should be considered.

3. **Outcome**: The result of this measure will give you the total number of distinct employees who have negative values for missing hours. This could indicate issues such as over-reporting of hours or errors in timesheet submissions, which can be important for payroll accuracy and employee management.

In summary, 'CountNegativeMissingHours' helps organizations identify how many employees have discrepancies in their reported hours, specifically those that are negative, allowing for further investigation or corrective action.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of active employees who are missing timesheet hours for a specific period, specifically when their reported hours are less than what is expected based on their contract hours.

Here's a breakdown of what it does:

1. **Data Source**: It uses a table called `vw_missing_timesheet`, which likely contains records of employees' hours worked, their contract hours, and their active status.

2. **Summarization**: The `SUMMARIZE` function creates a new table that groups the data by employee name, year, week, unit, and subunit. For each group, it calculates:
   - **MissingHours**: This is the difference between the total hours reported by the employee and their maximum contract hours. If this value is negative, it indicates that the employee has not logged enough hours.
   - **MinActiveEmp**: This captures the minimum value of a field that indicates whether the employee is active (with a value of 1 meaning they are active).

3. **Filtering**: The `FILTER` function then narrows down this summarized data to only include:
   - Employees who are active (`MinActiveEmp` = 1).
   - Employees who have missing hours (i.e., `MissingHours` < 0).

4. **Counting**: Finally, the `COUNTROWS` function counts how many employees meet these criteria.

In summary, this DAX measure calculates the number of active employees who have not logged enough hours according to their contract, helping the business identify potential issues with timesheet submissions.

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

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. It filters a summarized dataset (`vw_missing_timesheet`) to focus on employees who are currently active (indicated by `CC_ActiveEmployees` being equal to 1).

2. **Summarize Data**: Within this filtering process, it groups the data by key attributes such as employee name, year, week, unit, and subunit. For each group, it calculates:
   - **Missing Hours**: This is determined by taking the total hours worked (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contracted hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has not worked enough hours compared to what they are contracted for.
   - **Minimum Active Employees**: It also finds the minimum number of active employees in that group, which helps ensure that the calculation only includes groups where there is at least one active employee.

3. **Filter for Relevant Cases**: The `FILTER` function then narrows down the summarized data to only include those groups where:
   - There is at least one active employee (`MinActiveEmp = 1`).
   - The calculated missing hours are negative (`MissingHours < 0`), meaning the employee has not fulfilled their contracted hours.

4. **Calculate Total Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to sum up the missing hours for all the filtered active employees. This gives the total number of hours that active employees are missing compared to their contracted hours.

In summary, this DAX measure calculates the total hours that active employees have not worked compared to their contractual obligations, focusing only on those who are currently active and have a shortfall in hours.

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

1. **Data Source**: It uses a data table called `vw_missing_timesheet`, which likely contains information about employees, their contract hours, and the weeks they worked.

2. **Grouping Data**: The `SUMMARIZE` function groups the data by two key pieces of information:
   - The week (`vw_missing_timesheet[Week_]`)
   - The employee ID (`vw_missing_timesheet[EmployeeID]`)

   For each unique combination of week and employee, it calculates the maximum contract hours (`MAX(vw_missing_timesheet[ContractHours])`). This means if an employee has multiple entries for the same week, it will take the highest number of contract hours they are expected to work.

3. **Summing Up**: After summarizing the data, the `SUMX` function then takes this summarized table and adds up all the maximum contract hours calculated for each employee across all weeks. 

4. **Final Result**: The final output of this measure is the total expected contract hours for all employees, considering the highest contract hours for each employee in each week.

In summary, this DAX measure helps to determine the total expected working hours for employees based on their maximum contract hours recorded for each week, ensuring that if there are multiple entries for an employee in a week, only the highest is counted.

**`MISSING%`**

- **DAX Expression:**
```dax
DIVIDE([Dax_EmpCount]-[Dax_EmpCount_MissingTS],[Dax_EmpCount]) * 100
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of employees that are not missing time stamps (TS) compared to the total number of employees. Here's a breakdown of what each part does:

1. **[Dax_EmpCount]**: This represents the total count of employees.

2. **[Dax_EmpCount_MissingTS]**: This represents the count of employees who are missing time stamps.

3. **[Dax_EmpCount] - [Dax_EmpCount_MissingTS]**: This part calculates the number of employees who have valid time stamps by subtracting the count of employees missing time stamps from the total employee count.

4. **DIVIDE(..., [Dax_EmpCount])**: The `DIVIDE` function takes the number of employees with valid time stamps and divides it by the total number of employees. This gives us a fraction that represents the proportion of employees with valid time stamps.

5. **... * 100**: Finally, multiplying by 100 converts this fraction into a percentage.

In summary, this measure calculates the percentage of employees who have valid time stamps, helping the business understand how many employees are accounted for in terms of time tracking. A higher percentage indicates better compliance with time tracking requirements.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked on a contract. Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression finds the maximum number of contract hours that are expected or agreed upon for a specific contract. Essentially, it tells us the highest number of hours that should have been worked according to the contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part finds the maximum number of hours that have actually been recorded or logged by the employee for that same contract. It shows how many hours were actually worked.

3. **Overall Calculation**: By subtracting the maximum recorded hours (actual hours worked) from the maximum contract hours (expected hours), the expression calculates the number of hours that are missing or unaccounted for. 

In summary, this measure helps identify how many hours are missing from what was expected based on the contract, which can be useful for understanding discrepancies in work hours and ensuring that employees are meeting their contractual obligations.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of completeness regarding timesheet submissions for employees. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part adds up all the hours that are missing from timesheets. Essentially, it tells us how many hours employees did not report.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that employees are expected to work in a week according to their contracts.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the expression calculates the difference between the total missing hours and the expected hours. If the result is positive, it indicates that the missing hours exceed what was expected, suggesting a shortfall in timesheet reporting.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The DIVIDE function takes the result from the previous step (the difference between missing hours and expected hours) and divides it by the expected contract hours. This gives us a ratio or percentage. The third argument (0) ensures that if the expected hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

In summary, this measure calculates how much the reported hours fall short of what is expected, expressed as a percentage of the expected hours. A higher percentage indicates a greater shortfall in timesheet reporting, while a lower percentage suggests better compliance with expected reporting.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of unique employees who have missing timesheets, grouped by their respective units and weeks. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The expression uses a data table called `vw_missing_timesheet`, which presumably contains records of employees and their timesheet submissions.

2. **Grouping**: The `SUMMARIZE` function is used to create a summary table. This summary groups the data by three key columns:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This is the name of the employee.
   - `MAIN_UNIT`: This indicates the main unit or department the employee belongs to.

3. **IncompleteFlag**: For each group created by the `SUMMARIZE` function, it calculates a new column called "IncompleteFlag". This column uses the `MAX` function to determine if there is any indication of incompleteness in the timesheet for that employee in that week. If at least one record indicates that the timesheet is incomplete, the flag will be set accordingly.

4. **Summation**: Finally, the `SUMX` function iterates over the summarized table and sums up the values in the "IncompleteFlag" column. This effectively counts how many unique employees have missing timesheets for each unit and week.

In summary, this DAX measure calculates the total count of unique employees who have not submitted their timesheets, organized by week and unit. This information can be crucial for management to identify compliance issues and take necessary actions to ensure timely timesheet submissions.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and personnel identifiers. This table enables management to analyze labor allocation, project costs, and employee productivity, facilitating informed decision-making and resource planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column identifies the name of the client associated with each timesheet entry, facilitating the tracking of billable hours and project management. |
| `Code2` | `string` | The 'Code2' column stores a secondary identifier or classification code related to the timesheet entries, facilitating enhanced data categorization and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to conclude, providing essential information for timesheet management and workforce planning. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and payroll calculations within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract associated with the timesheet entry, represented as a string value. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, providing a real-time snapshot for analysis within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential temporal context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the monetary value associated with the time recorded in the timesheet, expressed as a double to accommodate precise financial calculations. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, measured as a double to allow for precise financial calculations. |
| `Currency` | `string` | The 'Currency' column specifies the type of currency used for financial entries in the timesheet, represented as a string to accommodate various currency codes. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the individual or entity responsible for payment, providing essential identification for billing and financial tracking within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee associated with the timesheet entries in the 'vw_Timesheet' table, facilitating accurate tracking and reporting of work hours. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column contains the concatenated identifier and name of employees, facilitating easy identification and tracking of timesheet entries within the 'vw_Timesheet' view. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating identification and reporting of labor hours. |
| `Employer` | `string` | The 'Employer' column contains the name of the organization or company that employs the individual associated with the timesheet entries. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating the tracking and management of employee work hours by organization. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to the timesheet entries, providing further insights into the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with the recorded timesheet entries, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, allowing for precise tracking of labor input for payroll and project management purposes. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column represents the total number of hours worked by an employee in a given week, recorded as an integer value within the timesheet data. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee work hours. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee work hours. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for project management instances, facilitating the tracking and association of timesheet entries with specific internal projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries, facilitating accountability and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the name associated with the IPM ID, providing a reference for identifying specific project management instances within the timesheet data. |
| `JobTitle` | `string` | The 'JobTitle' column contains the designation or role of the employee, providing context for the timesheet entries and aiding in the analysis of labor distribution across different positions within the organization. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking records within the 'vw_Timesheet' table, facilitating data integration and analysis across related datasets. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_Timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, facilitating the identification of supervisory oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table uniquely identifies the manager associated with each timesheet entry, facilitating the tracking and management of employee work hours and approvals. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct manager, facilitating the identification of supervisory relationships within the timesheet data. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries, detailing the specific tasks or projects associated with each timesheet record. |
| `Project` | `string` | The 'Project' column contains the names of the projects associated with each timesheet entry, allowing for the tracking and analysis of time spent on specific initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with the timesheet entries, facilitating the tracking and reporting of time spent on specific projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that provides detailed information about the specific project associated with each timesheet entry, facilitating better tracking and analysis of project-related labor efforts. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the specific project profile associated with each timesheet entry, facilitating project tracking and reporting. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into the types of work being performed. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness and false indicating it is not yet prepared. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of timesheet data analysis. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current approval or processing state of each timesheet entry, facilitating tracking and management of employee work hours. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, represented as a string value. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours and associated billing. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the tasks or activities recorded in the timesheet, providing context and details for each entry. |
| `Unit` | `string` | The 'Unit' column represents the measurement unit associated with the time entries recorded in the 'vw_Timesheet' table, facilitating clarity in reporting and analysis of work hours. |
| `UnitCode` | `string` | The 'UnitCode' column stores a unique identifier for each unit of work associated with the timesheet entries, facilitating the categorization and tracking of labor across different departments or projects. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and data management within the timesheet records. |
| `YearWeek` | `string` | The 'YearWeek' column represents the year and week number in a string format, facilitating time-based analysis and reporting within the timesheet data. |

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'Approved_With_V_Z'. Here's a breakdown of what it does in simple business terms:

1. **Condition Check**: The expression checks two conditions:
   - It first checks if the value in the column `[Approved]` is `TRUE`. This means it is verifying if an item (like a timesheet or request) has been approved.
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means it is looking for specific codes that might represent certain types of timesheets or requests.

2. **Output Values**: 
   - If either of the conditions is met (meaning the item is approved or has a code of "V" or "Z"), the expression returns a value of `1`. This indicates a positive outcome, suggesting that the item is considered approved in this context.
   - If neither condition is met, it returns a value of `0`, indicating that the item is not approved.

3. **Purpose**: The overall purpose of this calculated column is to flag items as approved based on either their approval status or specific codes. This can be useful for reporting, filtering, or further analysis, allowing users to easily identify which items are approved based on these criteria.

In summary, this DAX expression helps categorize items as approved if they meet certain conditions, making it easier to manage and analyze approval statuses in the dataset.

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column indicates the department associated with billable hours recorded in the timesheet, facilitating tracking and reporting of chargeable work across different organizational units.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'BillableDep'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'BillableDep' column from the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on the relationships defined in your data model.

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of specific data, the default assumption is that the item is billable.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that the item is considered billable.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that the item is not billable.

In summary, this DAX expression effectively categorizes items as billable or not based on the presence and value of the 'BillableDep' field from the related 'lkp_Unit' table. If there's no related data or if the value is non-zero, it marks the item as billable (1). If the value is zero, it marks it as not billable (0).

**`BillablePrj`** (`string`)

- **Description:** The 'BillablePrj' column identifies the project associated with billable hours recorded in the timesheet, facilitating accurate invoicing and project cost tracking.
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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'BillablePrj' in a data model, specifically for a table called `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether a particular entry in the timesheet is considered "billable" or not. A billable entry is one that can be charged to a client or project.

2. **Conditions Checked**:
   - **Sales Price**: It first checks if the `SalesPrice` for the entry is greater than 0. This means that if there is a positive sales price associated with the timesheet entry, it is considered billable.
   - **Project Profile Code**: Next, it checks if the `ProjectProfileCode` is equal to 81. If it is, this specific project profile is deemed billable regardless of other factors.
   - **Project Name**: Lastly, it looks for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, the entry is also considered billable.

3. **Output**: 
   - If any of the above conditions are met (i.e., if the sales price is positive, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of **1**. This indicates that the entry is billable.
   - If none of these conditions are met, it returns a value of **0**, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project name. This helps in identifying which work can be charged to clients, aiding in accurate billing and financial reporting.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column contains the name of the employer associated with the timesheet entry, facilitating the identification of the organization for which the recorded hours were worked.

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

Here's what it does in simple terms:

1. **Employee Name**: It takes the name of the employee from the 'vw_Timesheet' table, which is represented by `('vw_Timesheet'[EmployeeName])`.

2. **Hours per Week**: It also takes the number of hours that the employee works per week, represented by `('vw_Timesheet'[HoursperWeek])`.

3. **Combining Information**: The expression then combines these two pieces of information into one string. It adds a separator " - " between the employee's name and their weekly hours. 

For example, if an employee's name is "John Doe" and he works 40 hours per week, the result of this expression would be "John Doe - 40".

In summary, this DAX expression helps to create a clear and concise representation of each employee's name along with their weekly working hours, making it easier to read and analyze the data.

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

1. **Lookup for Group**: The expression first checks if there is a corresponding 'Group' value for a project in the 'lkp_Project' table based on the 'Project' field from the 'vw_Timesheet' table. It uses the `LOOKUPVALUE` function to find this 'Group' value.

2. **Check for Blank**: The `NOT(ISBLANK(...))` part checks if the result of the lookup is not blank. This means it verifies whether a valid 'Group' exists for the given project.

3. **Return Group or Category**:
   - If a valid 'Group' is found (i.e., the lookup is not blank), the expression returns that 'Group' value.
   - If no valid 'Group' is found (the lookup is blank), it then checks if the project is billable by looking at the 'BillablePrj' field in the 'vw_Timesheet' table.
     - If 'BillablePrj' equals 1 (indicating that the project is billable), it returns the string "Billable".
     - If 'BillablePrj' does not equal 1 (indicating that the project is not billable), it returns the string "Other Unbillable".

**In summary**, this DAX expression categorizes each project in the 'vw_Timesheet' table into one of three categories:
- The specific 'Group' from the 'lkp_Project' table if it exists,
- "Billable" if the project is billable but has no associated group,
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

1. **Checks Contract End Date**: The expression first checks if the 'ContractEndDate' for each record (or row) in the 'vw_Timesheet' table is blank (meaning there is no end date specified) or if the end date is in the future (greater than today's date).

2. **Determines Contract Status**:
   - If either of those conditions is true (the end date is blank or it is a future date), the expression labels the contract as "Active".
   - If neither condition is true (meaning there is a specific end date that is in the past), it labels the contract as "Not Active".

3. **Outcome**: The result is a clear indication of whether each contract is currently active or not, which can be useful for reporting, analysis, or decision-making regarding contracts in the organization.

In summary, this DAX code helps identify and categorize contracts based on their end dates, allowing users to easily see which contracts are still active.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit associated with each timesheet entry, facilitating the categorization and analysis of labor data by department or team.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'lkp_Unit' table for the current row. Specifically, it looks for the 'Unit' field in that related table.

2. **Handle Missing Data**: If there is no related value found (meaning the 'Unit' is blank), the expression will return the text "Unknown". This helps to identify cases where the unit information is missing.

3. **Return the Unit Name**: If there is a related value (meaning the 'Unit' is not blank), it will return that value. This means that the calculated column will show the actual unit name associated with the current row.

In summary, this DAX expression effectively ensures that every row in the 'MAIN_UNIT' column either displays the corresponding unit name from the related table or indicates "Unknown" if no unit information is available. This is useful for maintaining clarity in data reporting and analysis, especially when dealing with incomplete data.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column stores the numerical representation of the month (as a string) corresponding to each timesheet entry, facilitating time-based analysis and reporting.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to extract the month number from a date value found in the column named `TimesheetDate`. 

In simple terms, this means that for each date listed in the `TimesheetDate` column, the expression will return a number that represents the month of that date. For example:

- If the date is January 15, 2023, the expression will return `1`.
- If the date is February 10, 2023, it will return `2`.
- If the date is December 5, 2023, it will return `12`.

This calculated column, therefore, helps in organizing or analyzing data by month, making it easier to summarize or report on timesheet entries based on the month they occurred.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the employee identification numbers as strings, facilitating the association of timesheet entries with specific employees in the 'vw_Timesheet' view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

In simple business terms, this means that the expression counts how many different employees have recorded their time in the timesheet, without counting any employee more than once. For example, if three employees submitted timesheets and one of them submitted multiple entries, this calculation would still only count that employee once. 

The result of this calculation helps businesses understand the total number of distinct employees who are actively logging their hours, which can be useful for tracking workforce participation, project involvement, or resource allocation.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column contains string values that categorize the type of time entry as either owned, subcontracted, or external, providing clarity on resource allocation in the timesheet data.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used to retrieve a value from a related table in a data model. Here's a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a calculated column within a table that has a relationship with another table called `lkp_Unit`.

2. **Purpose**: The expression fetches the value from the `OWN-Sub-ExtT` column in the `lkp_Unit` table. 

3. **How it Works**: 
   - When you use `RELATED`, it looks for a matching row in the `lkp_Unit` table based on the existing relationship defined in the data model.
   - It then pulls the value from the `OWN-Sub-ExtT` column of that matching row.

4. **Outcome**: The result is that for each row in the current table, you get the corresponding value from the `lkp_Unit` table's `OWN-Sub-ExtT` column. This is useful for enriching your data with additional information that is related to each entry.

In summary, this DAX expression helps to enhance your data by linking it to another table and bringing in relevant information from that table, which can be crucial for analysis and reporting.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column contains string values that indicate the qualification status of projects associated with timesheet entries, helping to categorize and assess project eligibility for resource allocation and reporting.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'QualifyPrj'. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The expression first checks if the 'Qualify' field from a related table called 'lkp_Project' is blank (i.e., has no value). This is done using the `ISBLANK` function.

2. **Return Values Based on the Check**:
   - If the 'Qualify' field is blank, the expression returns a value of **1**. This could indicate a default or fallback status for projects that do not have a qualification value assigned.
   - If the 'Qualify' field is not blank, it returns the actual value from the 'Qualify' field in the 'lkp_Project' table.

**Overall Purpose**: This calculated column effectively ensures that every project has a qualification value. If a project does not have a specified qualification, it defaults to 1, allowing for consistent data handling and analysis. This can be useful for reporting or further calculations where a qualification value is necessary.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor allocation.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of resources, represented as a string, within the context of timesheet data for project management and resource allocation.
- **DAX Expression:**
```dax
IF(
			    [ReportReady] = TRUE() || 
			    vw_Timesheet[TimesheetCode] IN {"V", "Z"}, 
			    1, 
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'RReady_With_V_Z'. Here's a breakdown of what it does in simple business terms:

1. **Condition Check**: The expression checks two conditions:
   - First, it looks at a column called `[ReportReady]`. If this column has a value of `TRUE`, it means that the report is ready.
   - Second, it checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These are specific codes that might indicate certain types of timesheets.

2. **Output Values**: 
   - If either of the conditions is met (i.e., the report is ready or the timesheet code is "V" or "Z"), the expression returns a value of `1`.
   - If neither condition is met, it returns a value of `0`.

3. **Purpose**: The overall purpose of this calculated column is to flag records based on the readiness of the report or specific timesheet codes. A value of `1` indicates that the record is either ready for reporting or falls under the specified timesheet codes, while a value of `0` indicates that it does not meet these criteria.

In summary, this DAX expression helps in identifying which records are ready for reporting or belong to specific categories, making it easier for users to filter or analyze data based on these flags.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'ContractStatus2' that evaluates the status of a contract and provides a specific output based on that status.

Here's a breakdown of what it does:

1. **Condition Check**: The expression first checks if the measure `[ContractStatusMeasure]` equals "Active". This measure likely indicates whether a contract is currently active or not.

2. **Output Based on Condition**:
   - If the contract is "Active", the expression returns the value of another measure called `[HoursDifference]`. This measure probably represents the difference in hours related to the contract, such as the time remaining until expiration or the total hours worked under the contract.
   - If the contract is not "Active", the expression returns the text "Not Active".

**In summary**: This DAX measure helps to determine the status of a contract. If the contract is active, it provides a numerical value (the hours difference), and if it is not active, it simply indicates that by returning the text "Not Active". This can be useful for reporting or analysis purposes, allowing users to quickly understand the status of contracts and their associated metrics.

**`ContractStatusMeasure`**

- **DAX Expression:**
```dax
IF(
			    ISBLANK(MAX('vw_Timesheet'[ContractEndDate])) || MAX('vw_Timesheet'[ContractEndDate]) > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called `ContractStatusMeasure`, which determines the status of a contract based on its end date. Here’s a breakdown of what it does in simple business terms:

1. **Check for End Date**: The expression first looks at the maximum value of the `ContractEndDate` from the `vw_Timesheet` table. This means it is checking the latest end date of all contracts in the data.

2. **Evaluate Conditions**:
   - **ISBLANK**: It checks if this maximum end date is blank (meaning there is no end date recorded for the contract).
   - **Comparison with Today**: It also checks if this maximum end date is greater than today’s date (meaning the contract is still ongoing).

3. **Determine Status**:
   - If either of these conditions is true (the end date is blank or it is in the future), the measure returns "Active". This indicates that the contract is currently active and has not yet ended.
   - If neither condition is true (the end date is not blank and it is in the past), it returns "Not Active", indicating that the contract has ended.

In summary, this measure helps businesses quickly identify whether contracts are currently active or have ended, based on their end dates.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 15, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated based on the first week of the year, which can vary depending on the system used (e.g., whether the week starts on Sunday or Monday).

So, when you combine these two functions, `WEEKNUM(TODAY())` effectively tells you which week of the year it is right now. For instance, if today is October 15, 2023, this expression would return the week number corresponding to that date, helping businesses understand which week they are currently in for reporting or planning purposes.

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

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it counts the unique names of employees from the `vw_Timesheet` table.

3. **Filter Condition**: The expression includes a filter condition that specifies `vw_Timesheet[Approved] = FALSE()`. This means that only those timesheets that are marked as not approved (i.e., where the Approved status is false) will be considered in the count.

In summary, this DAX measure calculates the total number of distinct employees who have submitted timesheets that have not yet been approved. This can be useful for tracking pending approvals and understanding how many employees are waiting for their timesheets to be reviewed.

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

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it counts the unique names of employees from the `vw_Timesheet` table.

3. **Filter Condition**: The expression includes a filter that specifies `vw_Timesheet[ReportReady] = FALSE()`. This means that only those records where the `ReportReady` column is set to FALSE (indicating that the report is not ready) will be considered in the count.

In summary, this measure calculates the total number of distinct employees who have timesheets that are not yet ready for reporting. This can be useful for tracking which employees still need to finalize their timesheets before reports can be generated.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees recorded in the `vw_Timesheet` table. 

In simple business terms, this measure counts how many different employees have submitted timesheets, ensuring that each employee is only counted once, regardless of how many times they appear in the data. 

For example, if three employees submitted timesheets and one of them submitted multiple times, the result of this measure would still be three, reflecting the unique count of employees rather than the total number of submissions. This is useful for understanding workforce participation or tracking employee engagement in timesheet submissions.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_%_Budget_Used`**

- **DAX Expression:**
```dax
DIVIDE(SUM(vw_Timesheet[SalesAmount]), SUM('tbl_Project'[SalesPriceBaseCurrency]), 0)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of the budget that has been used based on sales data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the 'vw_Timesheet' table. Essentially, it totals the revenue generated from sales.

2. **SUM('tbl_Project'[SalesPriceBaseCurrency])**: This part sums up the total budget or sales price allocated for the projects from the 'tbl_Project' table. It represents the total amount that was planned or budgeted for sales.

3. **DIVIDE(..., ..., 0)**: The DIVIDE function takes the total sales amount (from the first part) and divides it by the total budget (from the second part). If the budget is zero, it will return 0 instead of causing an error, which is a safeguard to prevent division by zero.

In simple terms, this measure calculates how much of the budget has been used by comparing actual sales to the planned budget. The result is a ratio that shows the percentage of the budget that has been utilized based on the sales achieved. If the result is 1 (or 100%), it means the entire budget has been used; if it's less than 1, it indicates that there is still budget remaining.

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** This DAX expression calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the timesheet. Essentially, it gives you the total revenue generated from sales during the specified period.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the timesheet. It provides the total number of hours that employees or team members have worked during the same period.

3. **Division**: The expression then divides the total sales amount by the total hours worked. This calculation gives you the average amount of sales generated per hour worked.

In simple terms, this measure helps a business understand how much revenue is being generated for each hour of work, allowing for better assessment of productivity and efficiency.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'Msr_Budget' that dynamically retrieves the budget value based on the current week in the year. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: The code starts by determining the current week of the year using the `SELECTEDVALUE` function. This means it looks at the data context (like filters applied in a report) to find out which week is currently being analyzed.

2. **Calculate Previous Budget**: Next, it calculates the budget value for the most recent week before the current week. This is done using the `CALCULATE` function, which allows for modifying the context in which a measure is evaluated. The `FILTER` function is used to include only those weeks that are less than the current week. The measure `[Msr_SalesPriceBaseCurrency]` is used to get the budget value for those previous weeks.

3. **Return the Appropriate Value**: Finally, the code checks if the current budget value (for the current week) is available (not blank). If it is available, it returns that value. If it is not available, it falls back to the previously calculated budget value from the last available week.

In summary, this DAX measure effectively ensures that for any given week, you either get the current budget value if it exists or the last known budget value from a previous week if it doesn’t. This is useful for reporting and analysis, as it provides continuity in budget data even when some weeks may not have a defined budget.

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

4. **Filtering the Data**: The `FILTER` function is applied to the `ALLSELECTED('DimDate')`, which means it considers all the dates that are currently selected in the report, ignoring any other filters that might be applied to the date dimension. It then filters this selection to include only those months where the `MonthNumberOfYear` is less than or equal to the `SelectedMonth`. 

5. **Final Result**: As a result, the measure returns the total cost for all months from January up to the month that is currently selected in the visual. For example, if March is selected, it will sum the costs for January, February, and March.

In summary, this DAX measure provides a running total of costs for the selected month, allowing users to see how costs accumulate over time up to that point in the year.

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

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed or filtered in the report.

2. **Calculate Cumulative Sales**: The main goal of this measure is to calculate the total sales amount from the beginning of the year up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to modify the context in which the sales data is being evaluated. It allows us to change the filter context to include only the relevant months.

4. **Filtering the Date Table**: Inside the `CALCULATE` function, the `FILTER` function is applied to the 'DimDate' table. The `ALLSELECTED` function ensures that any filters applied to the date table are respected, but it allows us to include all months up to the selected month. Specifically, it filters the dates to include only those where the month number is less than or equal to the selected month.

5. **Summing Sales Amount**: Finally, the `SUM` function is used to total the `SalesAmount` from the `vw_Timesheet` table for all the months that meet the filter criteria (i.e., all months up to and including the selected month).

In summary, this DAX measure calculates the total sales from the start of the year up to the month that the user has selected in the report, allowing for a clear view of cumulative sales performance over time.

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
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of hours worked by a specific group, project, or employee compared to the total hours worked across all selected categories in a report or dashboard. Here’s a breakdown of what it does:

1. **TotalValue Calculation**: 
   - The expression first calculates the total hours worked by summing up the `Hours` column from the `vw_Timesheet` table. 
   - It uses the `CALCULATE` function along with `ALLSELECTED`, which means it considers all the currently selected filters for `GroupCat`, `Project`, and `EmployeeName`. This allows the measure to respect any filters applied in the report while still calculating the total across those selected categories.

2. **Percentage Calculation**:
   - After calculating the total hours (stored in the variable `TotalValue`), the expression then calculates the percentage of hours for the current context (the specific group, project, or employee being analyzed).
   - It does this by dividing the sum of hours for the current context (using `SUM(vw_Timesheet[Hours])`) by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used here to safely perform the division. If `TotalValue` is zero (which would mean no hours were recorded), it returns 0 instead of causing an error.

In summary, this measure provides insights into how much of the total hours worked is attributed to a specific group, project, or employee, expressed as a percentage. This can help in understanding workload distribution and performance across different categories.

**`MSR_PROJECT_%_REALIZATION`**

- **DAX Expression:**
```dax
IF(sum(tbl_Project[SalesPriceBaseCurrency]) <> 0, sum(vw_Timesheet[SalesAmount])/sum(tbl_Project[SalesPriceBaseCurrency]))
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called 'MSR_PROJECT_%_REALIZATION', which essentially assesses the percentage of sales realized from a project compared to the total sales price of that project. Here’s a breakdown of what it does in simple business terms:

1. **Check for Zero Sales Price**: The expression starts with an `IF` statement that checks if the total sales price (in the base currency) from the `tbl_Project` table is not equal to zero. This is important because if the sales price is zero, dividing by it would lead to an error.

2. **Calculate Sales Amount**: If the total sales price is not zero, the expression proceeds to calculate the percentage of sales realized. It does this by taking the total sales amount from the `vw_Timesheet` table.

3. **Calculate the Percentage**: The sales amount is then divided by the total sales price. This gives you a ratio or percentage that indicates how much of the expected sales price has actually been realized through sales.

In summary, this measure helps businesses understand how effectively they are converting their projected sales into actual sales. A higher percentage indicates better performance in realizing sales compared to what was initially expected. If the sales price is zero, the measure avoids any calculation to prevent errors.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** This DAX expression calculates the profit margin for a project by subtracting the total costs from the total sales. Here's a breakdown of what it does:

1. **Total Sales Calculation**: The expression first sums up all the values in the `SalesAmount` column from the `vw_Timesheet` table. This represents the total revenue generated from the project.

2. **Total Costs Calculation**: Next, it sums up all the values in the `CostAmount` column from the same table. This represents the total costs incurred for the project.

3. **Profit Margin Calculation**: Finally, the expression subtracts the total costs from the total sales. The result is the project margin, which indicates how much profit the project has made after covering its costs.

In simple terms, this measure helps a business understand how profitable a project is by showing the difference between what they earned (sales) and what they spent (costs).

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
- **DAX Explanation (Generated):** This DAX expression calculates the total sales price in a base currency for different projects and clients from a timesheet view. Here’s a breakdown of what it does in simple business terms:

1. **SUMMARIZE Function**: The expression starts by creating a summary table from the `vw_Timesheet` data. This summary groups the data by two key dimensions: `ProjectProfile` and `Client`. Essentially, it organizes the timesheet data so that we can look at sales prices for each unique combination of project and client.

2. **SELECTEDVALUE Function**: For each group created in the summary, it retrieves the sales price in the base currency from the `tbl_Project` table. The `SELECTEDVALUE` function is used here to get the specific sales price for the current project being evaluated. If there’s only one sales price for that project, it returns that value; if there are multiple, it returns blank.

3. **SUMX Function**: After creating this summary table with the sales prices, the `SUMX` function then iterates over each row of this summary table. It sums up the "MeasureValue" (which is the sales price in base currency) for all the unique combinations of project profiles and clients.

In summary, this DAX measure calculates the total sales price in the base currency for all projects and clients listed in the timesheet, providing a clear view of revenue or costs associated with those projects.

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

3. **ALL(vw_Timesheet)**: This function removes any existing filters on the `vw_Timesheet` table. By doing this, it ensures that the count of employees is not affected by any other filters that might be applied in the report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This is a filter condition that specifies we only want to consider employees whose contract status is "Valid" for the calculation.

Putting it all together, this DAX expression calculates the total number of unique employees who currently have a valid contract, regardless of any other filters that might be in place in the report. This is useful for understanding the active workforce under valid contracts at a given point in time.

**`TotalActualHours`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression `SUM(vw_Timesheet[Hours])` is used to calculate the total number of hours recorded in a timesheet. 

Here's a breakdown of what it does:

- **SUM**: This function adds up all the values in a specified column.
- **vw_Timesheet**: This refers to a table (or view) that contains timesheet data, which likely includes various details about employee hours worked.
- **[Hours]**: This is the specific column within the `vw_Timesheet` table that contains the number of hours each employee has logged.

In simple terms, this measure totals all the hours worked by employees as recorded in the timesheet, providing a single number that represents the overall hours worked during a specific period or across the entire dataset. This can be useful for reporting, payroll calculations, or analyzing workforce productivity.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the total contracted hours for employees based on their weekly hours recorded in a timesheet. Here's a breakdown of what it does in simple business terms:

1. **VALUES(vw_Timesheet[EmployeeName])**: This part of the expression creates a unique list of employee names from the timesheet data. Essentially, it identifies each employee who has recorded hours.

2. **MAX(vw_Timesheet[HoursPerWeek])**: For each employee in the unique list, this part finds the maximum number of hours they have recorded per week. This is important because it ensures that if an employee has multiple entries for different weeks, only the highest number of hours is considered.

3. **SUMX(...)**: The SUMX function then takes the unique list of employees and the maximum hours per week for each employee and sums these maximum values together. This means it adds up the highest contracted hours for all employees.

In summary, this DAX measure calculates the total of the maximum contracted hours per week for each employee, giving you a clear picture of the total contracted hours across the organization based on the highest weekly hours recorded.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression `[TotalActualHours] - [TotalContractedHours]` calculates the difference between two measures: **Total Actual Hours** and **Total Contracted Hours**.

In simple business terms, here's what it achieves:

- **Total Actual Hours** represents the actual number of hours worked or spent on a project or task.
- **Total Contracted Hours** refers to the number of hours that were agreed upon in a contract for that project or task.

By subtracting the Total Contracted Hours from the Total Actual Hours, this measure provides insight into how many hours were either over or under the contracted amount. 

- If the result is positive, it indicates that more hours were worked than what was contracted, suggesting potential overages or additional work.
- If the result is negative, it means that fewer hours were worked than contracted, which could indicate efficiency or underutilization of resources.

Overall, this measure helps businesses assess their performance against contractual agreements regarding time and resources.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total hours tracked from a timesheet, specifically from a table called `vw_Timesheet`. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the values in the "Hours" column of the `vw_Timesheet` table. Essentially, it calculates the total number of hours recorded.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (i.e., if there are no hours recorded). 

3. **IF(...)**: The IF function evaluates the result of the ISBLANK check:
   - If the total hours calculated is blank (meaning there are no hours tracked), it returns **0**. This ensures that if no hours are recorded, the measure will show a value of zero instead of a blank.
   - If there are hours tracked (the result is not blank), it simply returns the total hours calculated.

In summary, this DAX expression ensures that the measure `TotalHoursTracked` will always return a number: either the total hours tracked or zero if no hours have been recorded. This is useful for reporting and analysis, as it provides a clear and consistent output.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total hours tracked in a timesheet, but it also includes a check to handle situations where no hours have been recorded.

Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the `Hours` column of the `vw_Timesheet` table. 

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (i.e., if there are no hours recorded). 

3. **IF(...)**: The IF function evaluates the result of the ISBLANK check:
   - If the total hours are blank (meaning no hours have been tracked), it returns the text "Empty".
   - If there are hours recorded, it returns the total number of hours calculated by the SUM function.

In simple terms, this measure helps users understand whether any hours have been tracked. If no hours are recorded, it clearly indicates that with the word "Empty". If hours are present, it shows the total number of hours tracked. This is useful for reporting and ensuring that users can quickly see if there is missing data in their timesheets.

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

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked for a specific employee or group of employees.

2. **Maximum Hours Per Week**: The expression then calculates the maximum number of hours that have been recorded per week for each employee. This is done using the `CALCULATE` function combined with `MAXX` and `DISTINCT`. 
   - `DISTINCT(vw_Timesheet[HoursPerWeek])` creates a list of unique weekly hours recorded.
   - `MAXX` then finds the highest value from this list, which gives the maximum hours worked in any single week for that employee.

3. **Filtering by Employee**: The `ALLEXCEPT` function is used to ensure that the calculation of maximum hours is done only for the specific employee identified by `EmployeeID`. This means that the maximum hours are calculated without considering other filters that might be applied to the data, except for the employee filter.

4. **Calculating the Difference**: Finally, the expression subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. This gives the result of how many hours were tracked beyond the maximum weekly hours recorded for that employee.

In summary, this DAX measure calculates how many hours an employee has tracked beyond their maximum recorded hours in any single week. This can help in understanding if an employee is consistently working more hours than their typical workload.

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
- **DAX Explanation (Generated):** The DAX expression you've provided calculates a measure called `trackedDiff2`, which essentially determines the difference between the total hours tracked and the maximum hours tracked per week for each employee, while ignoring any specific filters on the hours per week.

Here's a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked for a specific context (like a project, employee, or time period).

2. **Maximum Hours Per Week**: The next part of the expression calculates the maximum number of hours that have been recorded per week for each employee. This is done using:
   - `MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])`: This part looks at all the unique hours recorded per week and finds the highest value among them.
   - `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])`: This ensures that any filters applied to the hours per week are ignored, allowing the calculation to consider all recorded hours.
   - `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])`: This keeps the context of the specific employee while removing other filters, ensuring that the maximum hours are calculated just for that employee.

3. **Calculating the Difference**: Finally, the expression subtracts the maximum hours tracked per week (calculated in the previous step) from the total hours tracked. This gives you the difference between what has been logged overall and the highest weekly hours recorded for that employee.

**In summary**: The `trackedDiff2` measure calculates how many hours exceed the maximum weekly hours tracked for each employee. This can help identify if an employee is consistently logging more hours than their maximum recorded workload, which could indicate overwork or inefficiencies.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total billable hours from a timesheet data source, specifically focusing on certain criteria. Here’s a breakdown of what it does in simple business terms:

1. **Variable Definition (FilteredHours)**: The expression starts by defining a variable called `FilteredHours`. This variable is created by filtering the `vw_Timesheet` table based on specific conditions.

2. **Filtering Criteria**:
   - The filter checks two main conditions:
     - **Qualifying Projects**: It looks for entries where the `QualifyPrj` column equals 1, indicating that the project qualifies for billing.
     - **Billable Projects**: It also checks if the `BillablePrj` column equals 1, meaning the project is billable.
   - Additionally, it includes any projects that contain the phrase "Customer Success Services" in the `Project` column. This is done using the `SEARCH` function, which looks for that specific text within the project names.

3. **Summation of Hours**: After applying the filter, the expression uses the `SUMX` function to calculate the total hours worked on the filtered projects. `SUMX` iterates over the filtered results and sums up the values in the `Hours` column.

4. **Final Output**: The final result of this measure, `UTI_TotalBillableHours`, is the total number of hours that meet the specified criteria—either qualifying and billable projects or projects related to "Customer Success Services."

In summary, this DAX measure effectively calculates the total billable hours from a timesheet, focusing on projects that are either explicitly marked as billable and qualifying or are related to customer success services. This helps businesses track and report on their billable work accurately.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** The DAX expression you provided is used to calculate a measure called 'UTI_TOTALHOURS'. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the values in the 'Hours' column from the 'vw_Timesheet' table. Essentially, it totals the number of hours recorded in the timesheet.

2. **CALCULATE(...)**: The CALCULATE function modifies the context in which the data is evaluated. In this case, it is used to apply a specific filter to the data being summed.

3. **vw_Timesheet[QualifyPrj] = 1**: This is the filter condition applied by the CALCULATE function. It specifies that only the rows in the 'vw_Timesheet' table where the 'QualifyPrj' column equals 1 should be included in the sum. This means that the measure will only count hours that are associated with projects that qualify under a certain criterion (indicated by the value 1).

In summary, the 'UTI_TOTALHOURS' measure calculates the total number of hours worked on projects that meet a specific qualification (where 'QualifyPrj' equals 1). This helps businesses track and analyze the hours spent on eligible projects, providing insights into resource allocation and project performance.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the "Billable Hour Ratio" for active employees in a business context. Here's a breakdown of what it does in simple terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable captures the total number of hours that employees have billed to clients or projects. It is represented by the measure `[UTI_TotalBillableHours]`.
   - `_TotalHours`: This variable captures the total number of hours worked by employees, represented by the measure `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The expression checks if there are any active contacts (employees) using `[Msr_ActiveContacts]`. If there are no active employees, the calculation does not proceed.
   - If there are active employees, it then checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the calculation does not proceed further.
   - If both conditions are met (active employees exist and total hours are available), it calculates the ratio of billable hours to total hours. This is done using the `DIVIDE` function, which safely divides `_TotalBillableHours` by `_TotalHours`.

3. **Handling Division**:
   - If the division results in a blank (which can happen if `_TotalHours` is zero), the expression returns 0 instead of an error. This is a safeguard to ensure that the measure always returns a valid number.

**In summary**, this DAX measure calculates the ratio of billable hours to total hours worked for active employees, providing insight into how effectively employees are utilizing their time for billable work. If there are no active employees or total hours, it ensures that the measure returns a sensible value (0) rather than an error.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

The following filters are applied to the entire report:

- Filter on (Name: `912435ffa0798d130252`) (Type: N/A, Definition: `N/A`)

### <a name="report-pages"></a>Report Pages

#### <a name="page-customer-analysis"></a>Page: Customer Analysis

*Internal Name: `ecc7667875c00809763b`, Ordinal: 0*

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

