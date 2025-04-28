# Power BI Model & Report Documentation

*Generated on: 2025-04-29 00:14:29*

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

The Power BI data model appears to be designed for a project management and timesheet tracking system, likely within a corporate or organizational context. It integrates various dimensions and fact tables to facilitate comprehensive reporting and analysis of employee hours, project performance, and resource allocation. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a focus on capturing and monitoring timesheet submissions, while the 'DimDate' table allows for time-based analysis, enabling users to track performance over specific periods.

The structure of the model indicates a strong emphasis on relationships between projects, units, and employee contributions. The lookup tables ('lkp_Project', 'lkp_Unit', 'lkp_Code') serve to standardize and categorize the data, enhancing the ability to generate insights into project costs, employee productivity, and potential gaps in timesheet submissions. Overall, this data model is likely aimed at improving operational efficiency, ensuring accurate project tracking, and facilitating informed decision-making within the organization.

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
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient data retrieval and analysis in time-based reporting and analytics. |
| `DayName` | `string` | The 'DayName' column contains the full name of the day (e.g., Monday, Tuesday) corresponding to each date in the 'DimDate' table, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | The 'DayNumberOfMonth' column (int64) in the 'DimDate' table represents the numerical day of the month, ranging from 1 to 31, providing a granular reference for date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical day of the week, where values range from 1 (Sunday) to 7 (Saturday), facilitating date-related analyses and reporting. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column (int64) in the 'DimDate' table represents the sequential day of the year, ranging from 1 to 365 (or 366 for leap years), facilitating date-related calculations and analyses. |
| `MonthName` | `string` | The 'MonthName' column contains the full names of the months, providing a human-readable representation of the month associated with each date in the 'DimDate' table. |
| `MonthNumberOfYear` | `int64` | The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month, ranging from 1 for January to 12 for December, facilitating time-based analysis and reporting. |
| `ShortDate` | `dateTime` | The 'ShortDate' column represents a simplified date format used to facilitate quick reference and analysis of specific dates within the DimDate table. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Indicates whether the date corresponds to the current calendar year, with values typically set to 'Yes' or 'No'.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CurrentYearFlag' in a data model, specifically within a date dimension table (DimDate). 

Here's what it does in simple terms:

1. **YEAR(DimDate[ShortDate])**: This part extracts the year from the date in the 'ShortDate' column of the DimDate table. For example, if the date is '2023-05-15', it will return '2023'.

2. **YEAR(TODAY())**: This part gets the current year based on today's date. If today is any date in 2023, this will return '2023'.

3. **IF(..., 1, 0)**: The IF function checks a condition. In this case, it checks if the year extracted from 'ShortDate' is less than or equal to the current year. 
   - If the condition is true (meaning the date is in the current year or a previous year), it returns '1'.
   - If the condition is false (meaning the date is in a future year), it returns '0'.

**Overall Purpose**: This calculated column effectively flags each date in the DimDate table. It assigns a '1' to dates that are in the current year or any previous year, and a '0' to dates that are in the future. This can be useful for filtering or analyzing data based on whether dates are current or past versus future.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table is designed to track and analyze the distribution of hours worked alongside their corresponding percentage contributions to overall performance metrics. This table aids in assessing workforce productivity and resource allocation, enabling informed decision-making for operational efficiency.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column contains string representations of hours worked alongside their corresponding percentage contributions, providing insights into employee productivity and time allocation. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and percentage calculations, facilitating data enrichment and analysis within the 'Hours_nd_Percentage' table. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, with associated qualification levels and sorting preferences, enabling efficient data retrieval and analysis for reporting and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers used to categorize and enrich data within the lookup table 'lkp_Code'. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numeric flag to categorize data for enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table contains string values that determine the order in which records are displayed or processed, facilitating organized data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing a mapping of unique Employee IDs to their corresponding names. This table is essential for filtering and aggregating employee-related metrics across various reports and dashboards in Power BI, enabling better insights into workforce performance and management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column stores unique identifiers for employees, represented as strings, to facilitate data enrichment and ensure accurate referencing within the employee lookup table. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, serving as a key identifier for enriching employee-related data within the lkp_fltr_Employee table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, detailing key attributes such as project names, qualification metrics, billing amounts, and associated groups, enabling businesses to analyze project performance and financials effectively.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing amount associated with each project, facilitating financial tracking and analysis. |
| `Group` | `string` | The 'Group' column categorizes projects into specific classifications to facilitate data enrichment and analysis within the lkp_Project table. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the lkp_Project table. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, with integer values representing different qualification levels or criteria met. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their identifiers and billing classifications. It facilitates efficient reporting and analysis by linking units to their respective billable departments and external classifications, ensuring clarity in resource allocation and financial tracking.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column represents the identifier for departments that are eligible for billing, facilitating the tracking of chargeable units within the 'lkp_Unit' table. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column contains the names of organizational units, serving as a key identifier for categorizing and enriching data related to various departments or divisions within the organization. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores string values that represent external type identifiers for owned sub-units, facilitating data enrichment and classification processes. |
| `Unit` | `string` | The 'Unit' column contains string values representing the various measurement units used for data enrichment purposes. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, leadership, and status. This table enables businesses to effectively manage and analyze their projects by providing insights into project categorization, progress, and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and reporting within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column contains string values that represent the administrative details or oversight responsibilities associated with each project listed in the tbl_Project table. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates within the 'tbl_Project' table. |
| `ClientName` | `string` | The 'ClientName' column stores the name of the client associated with each project, facilitating identification and management of client-specific projects within the 'tbl_Project' table. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating the tracking and management of client-related data within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated in the 'tbl_Project' table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project in the tbl_Project table. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the exact date and time when each project entry was created in the 'tbl_Project' table. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or business unit associated with each project in the 'tbl_Project' table. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column indicates the scheduled date and time for the completion of the project, facilitating project timeline management and tracking. |
| `Project` | `string` | The 'Project' column contains the names or identifiers of various projects associated with the entries in the tbl_Project table, facilitating the organization and tracking of project-related data. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed textual overview of each project, outlining its objectives, scope, and key deliverables within the 'tbl_Project' table. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the specific category or classification of a project within the 'tbl_Project' table, facilitating organization and management of related projects. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a textual summary of the project group, detailing its objectives and scope within the context of the overall project management framework. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) serves as a unique identifier for each project in the 'tbl_Project' table, facilitating efficient data retrieval and management. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the project's execution within the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency sales price of a project, stored as a decimal value to facilitate accurate financial calculations and reporting. |
| `Startdate` | `dateTime` | The 'Startdate' column records the date and time when a project is initiated, serving as a key reference point for project timelines and scheduling within the 'tbl_Project' table. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, allowing for effective tracking and management of project timelines and deliverables. |
| `Statuscode` | `string` | The 'Statuscode' column indicates the current operational status of each project, represented as a string to facilitate tracking and reporting on project progress. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, expressed as a decimal to accommodate precise financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table serves as a critical resource for tracking employees who have not submitted their timesheets, enabling managers to identify gaps in time reporting by employee and week. This table facilitates timely follow-ups and ensures compliance with labor regulations by providing insights into contract details and hours worked.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by an employee that have not been recorded in their timesheet. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract is set to expire, providing essential information for managing timesheet compliance and contract renewals within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to assist in identifying discrepancies in timesheet submissions within the 'vw_missing_timesheet' view. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract for each employee, providing insights into their employment status as it relates to timesheet submissions. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on outstanding time reporting. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours recorded for each employee's timesheet entry, facilitating the identification of discrepancies in time reporting. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) identifies the unique identifier of the manager responsible for overseeing the timesheet entries in the 'vw_missing_timesheet' table. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees' timesheet submissions, facilitating accountability and tracking within the 'vw_missing_timesheet' table. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee time tracking. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the missing timesheet records, facilitating the tracking and management of time allocation across various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project associated with the timesheet entries, facilitating the tracking and management of time allocation across various projects. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected (true) or accepted (false). |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness and false indicating it is not yet prepared. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total monetary value of sales transactions associated with the records in the 'vw_missing_timesheet' table, stored as a double for precision in financial calculations. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double data type, to facilitate accurate financial analysis and reporting within the 'vw_missing_timesheet' table. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with missing timesheet entries, formatted to facilitate quick reference and analysis. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific sub-division or department within the organization associated with each entry in the 'vw_missing_timesheet' table, facilitating targeted analysis of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' view. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the timesheet entries, providing context and details about the work performed during the reported period. |
| `Week_` | `int64` | The 'Week_' column (int64) represents the week number of the year associated with each entry in the 'vw_missing_timesheet' table, facilitating the identification and analysis of timesheet data by week. |
| `Year_` | `int64` | The 'Year_' column (int64) in the 'vw_missing_timesheet' table represents the calendar year associated with each timesheet entry, facilitating the analysis of timesheet data over time. |

##### Calculated Columns

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column identifies the department associated with billable hours in the context of missing timesheet entries, facilitating accurate tracking and reporting of labor costs.
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

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of a specific billable dependency, the default assumption is that it is billable.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that there is a billable dependency present.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that there is no billable dependency.

In summary, this DAX expression effectively categorizes entries based on their billable dependency status. It assigns a value of **1** if there is a billable dependency (either because it's not blank or it's a non-zero value) and a value of **0** if the dependency is explicitly zero. This helps in identifying which entries are considered billable based on their related data.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'CC_ActiveEmployees' in a data model, specifically within a table called `vw_missing_timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether an employee is considered "active" based on their contract dates.

2. **Conditions Checked**:
   - **Start Date Check**: It first checks if the date in the column `ShortDate` is on or after the employee's `ContractStartDate`. This means the employee's contract must have started before or on this date for them to be considered active.
   - **End Date Check**: Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. If the `ContractEndDate` is blank (meaning the employee's contract does not have an end date), the employee is still considered active.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the contract has no end date), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns a value of `0`, indicating that the employee is not active.

In summary, this DAX expression effectively flags employees as active (1) or inactive (0) based on whether the current date falls within their contract period or if they have an ongoing contract without an end date. This helps in identifying which employees are currently active based on their contractual agreements.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to assist in identifying records that require further attention in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IncompleteFlag' in a data model. Here's what it does in simple business terms:

- The expression checks the value in the 'MissingHours' column of the 'vw_missing_timesheet' table.
- If the value of 'MissingHours' is less than 0, it means that there is an issue (for example, it could indicate that there are negative hours reported, which is not valid).
- In this case, the expression returns a value of 1, which can be interpreted as a flag indicating that the entry is incomplete or problematic.
- If the value of 'MissingHours' is 0 or greater, the expression returns a value of 0, indicating that there is no issue with that entry.

In summary, this expression helps identify entries in the dataset that have negative missing hours, marking them as incomplete or problematic with a flag of 1, while marking all other entries as complete with a flag of 0.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit associated with each entry in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet submissions by unit.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'lkp_Unit' table for the current row. Specifically, it looks for the 'Unit' field in that related table.

2. **Handle Missing Data**: If the related 'Unit' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression will return the text "Unknown". This helps to identify cases where the unit information is missing.

3. **Return the Unit Name**: If there is a valid related 'Unit' value (i.e., it is not blank), the expression will return that value. This means that the calculated column will show the actual unit name associated with the current row.

In summary, this DAX expression effectively populates the 'MAIN_UNIT' column with either the name of the unit from the related table or "Unknown" if no unit information is available. This ensures that users can easily see which units are associated with each record, while also clearly indicating when that information is missing.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column represents the year and week number in a string format, used to identify and categorize timesheet entries by their corresponding week of the year in the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX expression creates a new calculated column called 'YearWeek' that combines the year and the week number of a specific date, which in this case is the 'ContractEndDate'.

Here's a breakdown of what it does:

1. **YEAR([ContractEndDate])**: This part extracts the year from the 'ContractEndDate'. For example, if the date is December 15, 2023, this would return "2023".

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the 'ContractEndDate'. For instance, if the date is in the 50th week of the year, this would return "50".

3. **FORMAT(..., "00")**: This ensures that the week number is always displayed as two digits. So, if the week number is 5, it will be formatted as "05".

4. **Concatenation (&)**: The expression combines the year and the formatted week number into a single string, separated by a hyphen. So, if the year is 2023 and the week number is 05, the final result would be "2023-05".

In summary, this DAX expression effectively creates a unique identifier for each week of the year based on the 'ContractEndDate', formatted as "YYYY-WW". This can be useful for reporting or analyzing data on a weekly basis.

##### Measures

**`Count_Negative_Consultant`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'Count_Negative_Consultant'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The measure counts the number of unique employees (consultants) who have recorded negative hours in their timesheets. Negative hours typically indicate an error or a situation where hours were deducted or adjusted.

2. **Components**:
   - **CALCULATE**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **DISTINCTCOUNT**: This function counts the number of unique entries in a specified column. Here, it counts unique employee names from the 'vw_missing_timesheet' table.
   - **'vw_missing_timesheet'[EmployeeName]**: This specifies the column from which unique employee names are being counted.
   - **'vw_missing_timesheet'[MissingHours] < 0**: This is the filter condition applied. It restricts the counting to only those records where the 'MissingHours' value is less than zero, meaning it only considers cases where negative hours are recorded.

3. **Outcome**: The result of this measure will give you the total number of different employees who have negative hours logged in their timesheets. This can help management identify potential issues with timesheet entries or track consultants who may need to correct their reported hours. 

In summary, 'Count_Negative_Consultant' helps in monitoring and managing timesheet accuracy by highlighting how many consultants have negative hour entries.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have recorded negative missing hours in a timesheet report. Here's a breakdown of what it does in simple business terms:

1. **CALCULATE Function**: This function is used to modify the context in which data is evaluated. In this case, it is being used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This part of the expression counts the number of unique Employee IDs. It ensures that each employee is only counted once, even if they have multiple entries.

3. **'vw_missing_timesheet'[MissingHours] < 0**: This is the filter applied to the data. It specifies that we are only interested in records where the 'MissingHours' value is less than zero, meaning we are looking for instances where employees have negative missing hours.

In summary, this measure counts how many different employees have negative missing hours recorded in the timesheet data. This could be useful for identifying issues with time reporting or understanding employee attendance patterns.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of active employees who have missing timesheet hours for a specific period, where the total hours they reported are less than their contracted hours.

Here's a breakdown of what it does:

1. **Data Source**: It uses a table called `vw_missing_timesheet`, which likely contains records of employee hours worked, their contracted hours, and other relevant details.

2. **Summarization**: The expression first summarizes the data by grouping it based on several fields:
   - Employee Name
   - Year
   - Week
   - Unit (from another table called `lkp_Unit`)
   - SubUnit
   This grouping helps to organize the data for each employee by specific time periods and organizational units.

3. **Calculating Missing Hours**: For each group, it calculates:
   - **MissingHours**: This is determined by taking the total hours worked (SUM of `Hours_`) and subtracting the maximum contracted hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has reported fewer hours than they are contracted to work.

4. **Identifying Active Employees**: It also calculates:
   - **MinActiveEmp**: This checks the minimum value of `CC_ActiveEmployees` for each group. If this value is 1, it indicates that the employee is considered active during that period.

5. **Filtering**: The `FILTER` function then narrows down the summarized data to only include those records where:
   - The employee is active (`MinActiveEmp = 1`)
   - The employee has missing hours (i.e., `MissingHours < 0`)

6. **Counting Rows**: Finally, the expression counts the number of rows that meet these criteria, which gives the total number of active employees who have reported fewer hours than their contracted amount for the specified time frame.

In summary, this DAX measure calculates how many active employees have missing timesheet hours, specifically those who have reported working less than their contracted hours. This information can be crucial for management to identify potential issues with time reporting and ensure compliance with contracted work hours.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of "missing hours" for active employees who have not met their contracted hours in a given week. Here’s a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code first creates a list of employees from a data source called `vw_missing_timesheet`. It groups this data by employee name, year, week, unit, and subunit. 

2. **Calculate Missing Hours**: For each employee in this list, it calculates two key pieces of information:
   - **Missing Hours**: This is determined by taking the total hours worked (from `vw_missing_timesheet[Hours_]`) and subtracting the maximum contracted hours (from `vw_missing_timesheet[ContractHours]`). If the result is negative, it indicates that the employee has not worked enough hours compared to what they are contracted for.
   - **Minimum Active Employees**: This checks if there is at least one active employee in the data for that grouping.

3. **Filter for Relevant Employees**: The code then filters this list to only include those employees who are active (where `MinActiveEmp` equals 1) and have negative missing hours (meaning they have not fulfilled their contracted hours).

4. **Sum the Missing Hours**: Finally, it sums up the missing hours for all the filtered active employees. This gives a total count of how many hours are missing across all relevant employees.

In summary, this DAX measure calculates the total number of hours that active employees are missing based on their contracted hours, helping the business identify potential staffing issues or areas where employees may need additional support to meet their work commitments.

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

1. **Data Source**: It uses a table called `vw_missing_timesheet`, which likely contains records of timesheets for employees, including the week, employee ID, and their contract hours.

2. **Grouping Data**: The `SUMMARIZE` function groups the data by two key fields: `Week_` (which represents the week of the year) and `EmployeeID` (which identifies each employee). This means it organizes the data so that we can look at each employee's hours for each week separately.

3. **Calculating Maximum Contract Hours**: For each group (each employee in each week), it calculates the maximum contract hours using `MAX(vw_missing_timesheet[ContractHours])`. This is important because if an employee has multiple entries for the same week, we want to consider only the highest contract hours they are expected to work.

4. **Summing Up Contract Hours**: After summarizing the data and determining the maximum contract hours for each employee per week, the `SUMX` function then adds up all these maximum contract hours across all employees and weeks. 

In summary, this DAX expression effectively totals the highest expected contract hours for each employee for each week, giving a clear picture of the total expected work hours across the organization for the specified time period.

**`MISSING%`**

- **DAX Expression:**
```dax
DIVIDE([Dax_EmpCount]-[Dax_EmpCount_MissingTS],[Dax_EmpCount]) * 100
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of employees that are not missing time stamps (TS) compared to the total number of employees. Here’s a breakdown of what it does:

1. **[Dax_EmpCount]**: This represents the total count of employees.

2. **[Dax_EmpCount_MissingTS]**: This represents the count of employees who are missing time stamps.

3. **[Dax_EmpCount] - [Dax_EmpCount_MissingTS]**: This part calculates the number of employees who have valid time stamps by subtracting the count of missing time stamps from the total employee count.

4. **DIVIDE(..., [Dax_EmpCount])**: The `DIVIDE` function takes the number of employees with valid time stamps and divides it by the total number of employees. This gives you a fraction that represents the portion of employees with valid time stamps.

5. **... * 100**: Finally, multiplying by 100 converts that fraction into a percentage.

In summary, this measure calculates the percentage of employees who have valid time stamps, helping the business understand how many employees are properly accounted for in terms of time tracking. A higher percentage indicates better compliance with time tracking requirements.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked on a contract. Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression finds the maximum number of hours that are supposed to be worked according to the contract. Essentially, it retrieves the highest value of hours that should have been logged for a particular contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part finds the maximum number of hours that have actually been recorded or logged by the employees for that same contract. It looks for the highest value of hours that have been submitted in the timesheet.

3. **Subtraction**: The expression then subtracts the maximum logged hours (actual hours worked) from the maximum contract hours (expected hours). 

**What it achieves**: The result of this calculation gives you the number of hours that are missing or unaccounted for in the timesheet compared to what was expected based on the contract. If the result is positive, it indicates that there are hours that should have been logged but weren't. If it's zero or negative, it means that the logged hours meet or exceed the expected contract hours.

In summary, this measure helps identify any discrepancies between expected and actual hours worked, which is crucial for managing contracts and ensuring that all work hours are properly accounted for.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of completeness regarding missing hours in relation to expected contract hours on a weekly basis. Here’s a breakdown of what it does:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part adds up all the missing hours recorded in the `vw_missing_timesheet` table. Essentially, it tells us how many hours are missing from the timesheets.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that are expected to be worked in a week according to the contract.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the expression calculates the difference between the total missing hours and the expected contract hours. If the result is positive, it indicates that the missing hours exceed what was expected.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The `DIVIDE` function takes the result from the previous step (the difference between missing hours and expected hours) and divides it by the expected contract hours. This gives us a ratio that represents how much of the expected hours are not being met due to missing timesheets. The third argument, `0`, ensures that if the expected hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

In summary, this measure calculates the proportion of missing hours compared to the expected hours, providing insight into how complete the timesheet submissions are. A higher percentage indicates a larger gap between expected and actual hours worked, which could signal issues with timesheet compliance or employee attendance.

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

2. **Grouping**: The `SUMMARIZE` function is used to create a summary table. This summary groups the data by three key columns:
   - `Week_`: The week of the year.
   - `EmployeeName`: The name of the employee.
   - `MAIN_UNIT`: The main unit or department the employee belongs to.

3. **IncompleteFlag**: For each group created by the `SUMMARIZE` function, it calculates a new column called "IncompleteFlag". This column uses the `MAX` function to determine if there is any record in that group indicating that the employee has an incomplete timesheet. If at least one record shows an incomplete timesheet, the flag will be set to 1 (true); otherwise, it will be 0 (false).

4. **Summing Up**: Finally, the `SUMX` function iterates over the summarized table and sums up the values in the "IncompleteFlag" column. This effectively counts how many unique employees have at least one incomplete timesheet for each unit and week.

In summary, this DAX measure helps the business track how many unique employees are missing timesheets, allowing management to identify potential issues with timesheet compliance across different units and weeks.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing key details such as financial year, submission dates, and associated project and personnel identifiers. This table enables management to analyze labor allocation, project costs, and employee productivity, facilitating informed decision-making and resource planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column identifies the name of the client associated with each timesheet entry, facilitating the tracking of billable hours and project management. |
| `Code2` | `string` | The 'Code2' column stores a secondary identifier or classification code associated with each timesheet entry, facilitating enhanced data categorization and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to terminate, providing essential information for timesheet management and resource planning. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and payroll calculations within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract associated with the timesheet entry, represented as a string for easy interpretation and reporting. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, represented as a string, to facilitate real-time reporting and analysis within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential temporal context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the monetary value associated with the hours worked, expressed as a double, within the context of timesheet data for accurate financial reporting and analysis. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary cost associated with each entry in the timesheet, stored as a double to allow for precise financial calculations. |
| `Currency` | `string` | The 'Currency' column (string) in the 'vw_Timesheet' table specifies the type of currency used for financial entries related to timesheet records. |
| `DebtorName` | `string` | The 'DebtorName' column contains the name of the individual or entity responsible for payment, providing essential identification for financial tracking within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee associated with the timesheet entries in the 'vw_Timesheet' table, facilitating accurate tracking and reporting of work hours. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column contains the concatenated identifier and name of employees, facilitating easy reference and identification within the timesheet records. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating identification and reporting of labor hours. |
| `Employer` | `string` | The 'Employer' column contains the name of the organization or company that employs the individual associated with the timesheet entries. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating the tracking and management of employee work hours by organization. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to the timesheet entries, providing further insights into the recorded hours or activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with the recorded timesheet entries, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, facilitating accurate tracking of labor for payroll and project management purposes. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating analysis of labor allocation and productivity. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee time worked. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave hours, facilitating accurate reporting and analysis of employee time allocation. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries, facilitating accountability and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the names of the IPM IDs associated with each timesheet entry, facilitating the identification and tracking of project management metrics. |
| `JobTitle` | `string` | The 'JobTitle' column contains the titles of positions held by employees, providing context for their roles in relation to the timesheet data. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking records within the 'vw_Timesheet' table, facilitating data integration and retrieval across related datasets. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating the tracking and management of employee work hours and project allocations. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, facilitating easy identification of managerial oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key associated with the manager responsible for overseeing the timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct manager, facilitating the identification of supervisory relationships within the timesheet data. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries related to operational hours worked, facilitating the tracking and management of employee time allocation within the timesheet records. |
| `Project` | `string` | The 'Project' column contains the name of the project associated with each timesheet entry, enabling tracking and reporting of time spent on specific initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with the timesheet entries, facilitating the tracking and reporting of time spent on specific projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that represents the specific characteristics or details of the project associated with each timesheet entry, facilitating better project management and resource allocation. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the specific project profile associated with each timesheet entry, facilitating accurate project tracking and reporting. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into the types of work being performed. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double data type for precision in financial calculations within the 'vw_Timesheet' table. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current state of each timesheet entry, reflecting whether it is submitted, approved, or pending review. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation, with a value of true representing taxed hours and false representing non-taxed hours. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, providing insights into resource allocation and project management. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores a unique identifier for each timesheet entry, facilitating the tracking and categorization of employee work hours within the vw_Timesheet table. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the tasks or activities recorded in the timesheet, providing context and details for each entry. |
| `Unit` | `string` | The 'Unit' column represents the measurement unit associated with the time entries recorded in the 'vw_Timesheet' table, facilitating the understanding of the duration or quantity of work performed. |
| `UnitCode` | `string` | The 'UnitCode' column represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating the categorization and analysis of labor data by department or team. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and record-keeping within the timesheet management system. |
| `YearWeek` | `string` | The 'YearWeek' column represents the year and week number in a string format, facilitating the organization and analysis of timesheet data by specific weekly periods. |

##### Calculated Columns

**`Approved_With_V_Z`** (`string`)

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheets, specifically denoting whether they have been approved with a specific validation or verification process identified by 'V_Z'.
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
   - It first checks if the value in the 'Approved' column is TRUE (meaning the item is approved).
   - It also checks if the 'TimesheetCode' in the 'vw_Timesheet' table is either "V" or "Z".

2. **Output Values**: 
   - If either of the conditions is met (i.e., the item is approved or the TimesheetCode is "V" or "Z"), the expression returns a value of **1**.
   - If neither condition is met, it returns a value of **0**.

3. **Purpose**: The overall purpose of this calculated column is to flag records as either approved (1) or not approved (0) based on the specified criteria. This can be useful for reporting or analysis, allowing users to easily identify which records are approved or fall under the specific TimesheetCodes of "V" or "Z".

In summary, this DAX expression helps in categorizing records based on approval status and specific timesheet codes, making it easier to filter or analyze data related to approvals.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'BillableDep'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'BillableDep' column from the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on the relationships defined in your data model.

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of a specific billable dependency, it defaults to a value of 1.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**. This suggests that there is a valid billable dependency present.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**. This indicates that there is no billable dependency in this case.

### Summary:
In summary, this DAX expression effectively categorizes the 'BillableDep' status into two outcomes: 
- It assigns a value of **1** if there is either no related billable dependency or if there is a valid (non-zero) billable dependency.
- It assigns a value of **0** only when there is a related billable dependency that is specifically zero.

This calculated column can be useful for reporting or analysis purposes, helping to identify whether a unit is billable based on its dependencies.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'BillablePrj' in a data model, specifically within a table called `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether a particular entry in the timesheet is considered "billable" or not. A billable entry is one that can be charged to a client or project.

2. **Conditions Checked**:
   - **Sales Price**: It first checks if the `SalesPrice` for the entry is greater than 0. If there is a positive sales price, it indicates that the work done can be billed.
   - **Project Profile Code**: Next, it checks if the `ProjectProfileCode` is equal to 81. This specific code likely represents a type of project that is always billable.
   - **Project Name**: Finally, it searches for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it suggests that the project is related to customer success services, which are typically billable.

3. **Output**: 
   - If any of the above conditions are true (i.e., if the sales price is positive, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of **1**. This indicates that the entry is billable.
   - If none of the conditions are met, it returns a value of **0**, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project name. This helps in identifying which work can be invoiced to clients.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column contains the name of the employer associated with the timesheet entry, facilitating the identification of the organization responsible for the recorded work hours.

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

1. **Employee Name**: It takes the name of the employee from the 'EmployeeName' field in the 'vw_Timesheet' table.

2. **Hours per Week**: It also takes the number of hours that the employee works per week from the 'HoursperWeek' field in the same table.

3. **Combining Information**: The expression then combines these two pieces of information into one string. It formats it so that the employee's name is followed by a dash (" - ") and then the number of hours they work each week.

For example, if an employee's name is "John Doe" and he works 40 hours per week, the resulting string in the 'Employee_WeeklyHours' column would be "John Doe - 40".

In summary, this DAX expression helps to create a clear and concise representation of each employee's name along with their weekly working hours, making it easier to read and understand the data at a glance.

**`GroupCat`** (`string`)

- **Description:** The 'GroupCat' column categorizes timesheet entries into specific groups for enhanced reporting and analysis of labor distribution.
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

1. **Lookup for Group**: The expression first attempts to find a corresponding 'Group' value from a lookup table called `lkp_Project`. It does this by checking if there is a project in the `vw_Timesheet` table that matches a project in the `lkp_Project` table.

2. **Check for Blank Values**: The `NOT(ISBLANK(...))` part checks if the lookup returned a valid 'Group' value. If it did (meaning the project exists in the `lkp_Project` table), it will use that 'Group' value for the 'GroupCat' column.

3. **Handling Non-Matching Projects**: If the lookup does not return a valid 'Group' (meaning the project is not found in the `lkp_Project` table), the expression then checks if the project is billable. This is done by checking if the `BillablePrj` column in the `vw_Timesheet` table equals 1.

4. **Assigning Categories**: 
   - If the project is billable (i.e., `BillablePrj` equals 1), the 'GroupCat' will be set to "Billable".
   - If the project is not billable, it will be categorized as "Other Unbillable".

In summary, this DAX expression categorizes each project in the `vw_Timesheet` table into a 'GroupCat' based on whether it has a corresponding group in the `lkp_Project` table or if it is billable. The result is either the group name, "Billable", or "Other Unbillable", helping to classify projects for reporting or analysis purposes.

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

1. **Check Contract End Date**: The expression first checks if the 'ContractEndDate' for each record (or row) in the 'vw_Timesheet' table is blank (meaning there is no end date specified) or if the end date is in the future (greater than today's date).

2. **Determine Contract Status**:
   - If either of these conditions is true (the end date is blank or it is a future date), the expression will return "Active". This means that the contract is currently valid and ongoing.
   - If neither condition is true (meaning there is a specified end date that is in the past), it will return "Not Active". This indicates that the contract has ended.

In summary, this calculated column helps to quickly identify whether a contract is currently active or not based on its end date, providing valuable information for managing contracts effectively.

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

1. **Check for Blank Values**: The expression first checks if the related value from the 'lkp_Unit' table (specifically the 'Unit' column) is blank (i.e., missing or not available).

2. **Return "Unknown" if Blank**: If the 'Unit' value is blank, the expression will return the text "Unknown". This helps to clearly indicate that there is no associated unit available for that particular record.

3. **Return the Unit if Not Blank**: If the 'Unit' value is not blank, the expression will return the actual value from the 'lkp_Unit[Unit]' column. This means that the calculated column will show the relevant unit name for that record.

In summary, this DAX expression effectively ensures that every record in the 'MAIN_UNIT' column either displays the corresponding unit name from the 'lkp_Unit' table or indicates "Unknown" when no unit is available. This is useful for maintaining clarity in data reporting and analysis, as it prevents confusion that could arise from having blank values.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) associated with each timesheet entry, facilitating time-based analysis and reporting.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to extract the month number from a date value found in the column named `TimesheetDate`. 

In simple terms, this means that for each entry in the `TimesheetDate` column, the expression will return a number that represents the month of that date. For example:

- If the date is January 15, 2023, the expression will return `1`.
- If the date is February 10, 2023, it will return `2`.
- If the date is December 5, 2023, it will return `12`.

This calculated column, named 'MonthNumber', helps in organizing or analyzing data by month, making it easier to perform further calculations or visualizations based on the month of the year.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the unique employee identification numbers as strings, facilitating the association of timesheet entries with specific employees in the 'vw_Timesheet' view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

In simple business terms, this expression counts how many different employees have recorded their time in the timesheet. If an employee appears multiple times in the timesheet (for example, if they submitted timesheets for multiple days), they will only be counted once. 

So, the result of this calculation gives you a clear picture of how many individual employees are contributing to the timesheet data, which can be useful for understanding workforce participation, resource allocation, or project involvement.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table captures the classification of time entries as either owned, subcontracted, or external, facilitating the analysis of resource allocation and project costs.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used to retrieve a value from a related table in a data model. Here's a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a calculated column within a table that has a relationship with another table. In this case, the current table is related to the `lkp_Unit` table.

2. **Purpose**: The expression is designed to fetch the value from the `OWN-Sub-ExtT` column in the `lkp_Unit` table. 

3. **How it Works**: When you use `RELATED`, DAX looks for a matching row in the `lkp_Unit` table based on the existing relationship. It then retrieves the value from the specified column (`OWN-Sub-ExtT`) for that matching row.

4. **Outcome**: The result is that for each row in the current table, you get the corresponding value from the `OWN-Sub-ExtT` column of the `lkp_Unit` table. This allows you to enrich your data with additional information from the related table, which can be useful for analysis and reporting.

In summary, this DAX expression helps to pull in specific information from a related table, enhancing the data available for each record in the current table.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'QualifyPrj'. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The expression first checks if the 'Qualify' value from a related table called 'lkp_Project' is blank (i.e., it has no value).

2. **Return Values Based on the Check**:
   - If the 'Qualify' value is blank, the expression returns a value of **1**. This could indicate a default or fallback status for projects that do not have a qualification assigned.
   - If the 'Qualify' value is not blank, it returns the actual value from the 'Qualify' column in the 'lkp_Project' table. This means that if there is a qualification available, it will be used.

**Overall Purpose**: The calculated column 'QualifyPrj' effectively ensures that every project has a qualification value. If a project does not have a qualification, it defaults to 1, allowing for consistent data handling and analysis. This is useful for reporting and decision-making, as it ensures that all projects can be evaluated based on their qualification status.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for submitting the timesheet data, facilitating accurate tracking and reporting of labor hours.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of timesheet entries, represented as a string, to facilitate data processing and analysis within the 'vw_Timesheet' view.
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
   - Whether the value of the column `[ReportReady]` is `TRUE`. This likely indicates that a report is ready for some action or review.
   - Whether the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These codes probably represent specific types of timesheets or statuses that are relevant to the business.

2. **Output Values**: 
   - If either of the conditions is met (i.e., if the report is ready or if the timesheet code is "V" or "Z"), the expression returns a value of `1`. This could signify that the item is considered "ready" or meets certain criteria for further processing.
   - If neither condition is met, it returns a value of `0`, indicating that the item does not meet the criteria.

3. **Purpose**: The overall purpose of this calculated column is to flag records as either "ready" (1) or "not ready" (0) based on the specified conditions. This can help in filtering or analyzing data later on, allowing users to quickly identify which records are ready for action based on the report status or specific timesheet codes. 

In summary, this DAX expression helps in determining if a record is ready for further processing based on specific criteria related to report readiness and timesheet codes.

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

1. **Condition Check**: The expression first checks if the value of another measure called `[ContractStatusMeasure]` is equal to "Active". This measure likely indicates whether a contract is currently active or not.

2. **Output Based on Condition**:
   - If the contract is "Active", the expression returns the value of another measure called `[HoursDifference]`. This measure probably calculates the difference in hours related to the contract, such as the time remaining or the total hours worked.
   - If the contract is not "Active", the expression returns the text "Not Active".

**In summary**: This measure helps to determine the status of a contract. If the contract is active, it provides the relevant hours difference; if not, it simply states that the contract is not active. This can be useful for reporting or analysis purposes, allowing users to quickly understand the status and related metrics of contracts.

**`ContractStatusMeasure`**

- **DAX Expression:**
```dax
IF(
			    ISBLANK(MAX('vw_Timesheet'[ContractEndDate])) || MAX('vw_Timesheet'[ContractEndDate]) > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to determine the status of a contract based on its end date. Here's a breakdown of what it does in simple business terms:

1. **Check for End Date**: The expression first looks at the maximum value of the 'ContractEndDate' from the 'vw_Timesheet' table. This means it is checking the latest contract end date available in the data.

2. **Evaluate Conditions**: 
   - It checks if this maximum end date is blank (meaning there is no end date recorded).
   - It also checks if this maximum end date is greater than today's date (meaning the contract is still ongoing).

3. **Determine Status**:
   - If either of the above conditions is true (either there is no end date or the end date is in the future), the expression returns "Active". This indicates that the contract is currently active and has not yet ended.
   - If neither condition is true (meaning there is an end date that is in the past), it returns "Not Active". This indicates that the contract has ended.

In summary, this measure helps to identify whether a contract is currently active or not based on its end date, providing valuable information for contract management and oversight.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 15, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. Week numbers typically start from 1 for the first week of the year.

So, when you combine these two functions, `WEEKNUM(TODAY())` effectively tells you which week of the year it is right now. For instance, if today is October 15, 2023, this expression would return the week number corresponding to that date, which might be 42, indicating it's the 42nd week of the year.

In summary, this DAX expression helps businesses understand the current week in the context of their annual planning or reporting, allowing them to track performance or activities on a weekly basis.

**`Dax_EmpCount_Approved`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[Approved] = FALSE()
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees whose timesheets have not been approved. Here's a breakdown of what it does:

1. **CALCULATE Function**: This function changes the context in which data is evaluated. In this case, it is used to modify the filter context for the calculation that follows.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it is counting the unique names of employees from the `vw_Timesheet` table.

3. **Filter Condition**: The expression includes a filter condition that specifies `vw_Timesheet[Approved] = FALSE()`. This means that the calculation will only consider timesheets that have not been approved (i.e., where the `Approved` column is marked as FALSE).

Putting it all together, this DAX measure calculates the total number of distinct employees who have submitted timesheets that are still pending approval. This information can be useful for understanding how many employees are waiting for their timesheet approvals, which can help in managing workflow and ensuring timely processing of timesheets.

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

3. **Filter Condition**: The expression includes a filter condition that specifies `vw_Timesheet[ReportReady] = FALSE()`. This means that the calculation will only consider timesheets where the "Report Ready" status is set to FALSE, indicating that these timesheets are not ready for reporting.

In summary, the measure `Dax_EmpCount_RReady` calculates the total number of distinct employees who have timesheets that are not yet ready for reporting. This can help a business identify how many employees still need to finalize their timesheets before they can be processed or reported.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheet entries in the `vw_Timesheet` table.

Here's a breakdown of what it achieves:

- **DISTINCTCOUNT**: This function counts only the unique values in a specified column. In this case, it ensures that each employee is counted only once, regardless of how many times they appear in the timesheet records.

- **vw_Timesheet[EmployeeName]**: This specifies the column from which the unique employee names are being counted. It refers to the names of employees who have submitted timesheets.

In simple terms, this measure provides the total number of different employees who have logged their work hours, helping businesses understand workforce participation and engagement.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_%_Budget_Used`**

- **DAX Expression:**
```dax
DIVIDE(SUM(vw_Timesheet[SalesAmount]), SUM('tbl_Project'[SalesPriceBaseCurrency]), 0)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of the budget that has been used based on sales data. Here’s a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part adds up all the sales amounts recorded in the 'vw_Timesheet' table. Essentially, it totals the revenue generated from sales.

2. **SUM('tbl_Project'[SalesPriceBaseCurrency])**: This part sums up the total budget or sales price available for the projects from the 'tbl_Project' table. It represents the total amount that was planned or budgeted for sales.

3. **DIVIDE(..., ..., 0)**: The DIVIDE function takes the total sales amount (from the first part) and divides it by the total budget (from the second part). If the budget is zero (to avoid division by zero errors), it returns 0 instead of an error.

In simple terms, this measure calculates how much of the budget has been used by comparing actual sales to the planned budget. The result is a ratio or percentage that indicates the proportion of the budget that has been utilized based on sales performance.

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** This DAX expression calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the timesheet. Essentially, it totals the revenue generated from sales.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the timesheet. It totals the time spent on tasks or projects.

3. **Division**: The expression then divides the total sales amount by the total hours worked. This gives you the average amount of sales generated per hour worked.

In simple terms, this measure calculates how much revenue is earned for each hour of work, providing insight into productivity and efficiency in generating sales.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'Msr_Budget' that dynamically determines the budget value based on the current week in the year. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: The code starts by capturing the current week number from a date dimension table (DimDate). This is done using the `SELECTEDVALUE` function, which retrieves the week number that is currently selected in the report or dashboard.

2. **Calculate Previous Budget**: Next, the code calculates the budget value for the most recent week prior to the current week. It does this by:
   - Using the `CALCULATE` function to evaluate another measure called `[Msr_SalesPriceBaseCurrency]`, which presumably represents the sales price in the base currency.
   - The `FILTER` function is applied to the date dimension to include only those weeks that are less than the current week. The `ALLSELECTED` function ensures that any filters applied to the date dimension are respected, but it allows the measure to consider all weeks up to the current one.

3. **Return the Appropriate Value**: Finally, the code checks if the current measure `[Msr_SalesPriceBaseCurrency]` has a value (i.e., it is not blank). 
   - If it does have a value, that value is returned as the budget for the current week.
   - If it does not have a value (meaning there is no budget set for the current week), the measure returns the previously calculated budget value from the most recent week.

In summary, this DAX measure effectively ensures that a budget value is always available for the current week by either using the current week's budget (if it exists) or falling back to the most recent previous week's budget if the current one is not set. This approach helps maintain continuity in budget reporting and analysis.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the cumulative cost up to a selected month in a reporting visual. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The code starts by determining which month is currently selected in the visual. It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed or filtered.

2. **Calculate Cumulative Cost**: The main purpose of this measure is to calculate the total cost incurred up to and including the selected month. 

3. **Summing Costs**: The `SUM` function is used to add up the values in the `CostAmount` column from the `vw_Timesheet` table. This represents the total costs recorded.

4. **Filtering the Data**: The `FILTER` function is applied to ensure that only the months that are less than or equal to the selected month are included in the calculation. The `ALLSELECTED` function is used to consider any filters that might be applied to the 'DimDate' table, allowing for a dynamic response to user selections.

5. **Final Result**: The result of this measure is the total cost accumulated from the beginning of the year up to the selected month. This allows users to see how costs have built up over time, providing valuable insights for budgeting and financial analysis.

In summary, this DAX measure helps businesses track and visualize their cumulative costs over time, making it easier to understand spending patterns and manage budgets effectively.

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

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed or filtered.

2. **Calculate Cumulative Sales**: The main goal of this measure is to calculate the total sales amount from the beginning of the year up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to modify the context in which the sales amount is summed. It allows for more complex calculations by changing the filters applied to the data.

4. **Filtering the Date Context**: Inside the `CALCULATE`, there is a `FILTER` function that adjusts the context to include all months from the start of the year up to the selected month. The `ALLSELECTED` function ensures that any other filters applied to the 'DimDate' table are respected, but it removes any filters that would limit the months to only those currently visible.

5. **Summing Sales Amount**: Finally, the `SUM` function adds up the `SalesAmount` from the `vw_Timesheet` table for all the months that meet the criteria set by the filter.

In summary, this DAX measure calculates the total sales from the beginning of the year up to the month that is currently selected in the visual, allowing users to see cumulative sales performance over time.

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
   - The first part of the code defines a variable called `TotalValue`. This variable calculates the total number of hours from the `vw_Timesheet` table. 
   - The `CALCULATE` function is used to sum the `Hours` column, but it considers all selected filters for `GroupCat`, `Project`, and `EmployeeName`. This means it looks at the total hours worked across all the selected categories, ignoring any other filters that might be applied.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours for the current context (which could be a specific group, project, or employee) by dividing the sum of hours for that context by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safer way to handle division in DAX because it can return a default value (in this case, 0) if the denominator (`TotalValue`) is zero, preventing errors.

In summary, this measure provides insight into how much of the total hours worked is attributed to a specific group, project, or employee, expressed as a percentage. This can help in understanding workload distribution and performance across different categories.

**`MSR_PROJECT_%_REALIZATION`**

- **DAX Expression:**
```dax
IF(sum(tbl_Project[SalesPriceBaseCurrency]) <> 0, sum(vw_Timesheet[SalesAmount])/sum(tbl_Project[SalesPriceBaseCurrency]))
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called 'MSR_PROJECT_%_REALIZATION', which essentially assesses the percentage of sales realization for a project. Here's a breakdown of what it does in simple business terms:

1. **Check for Zero Sales Price**: The expression starts with an `IF` statement that checks if the total sales price (in the base currency) from the `tbl_Project` table is not equal to zero. This is important because if the sales price is zero, dividing by it would lead to an error.

2. **Calculate Sales Amount**: If the total sales price is not zero, the expression proceeds to calculate the percentage of sales realization. It does this by taking the total sales amount from the `vw_Timesheet` table.

3. **Calculate the Percentage**: The sales amount is then divided by the total sales price. This gives the percentage of how much of the potential sales (represented by the sales price) has actually been realized (represented by the sales amount).

In summary, this DAX measure calculates the percentage of sales that have been realized for a project, but only if there is a valid sales price to compare against. If there is no sales price, it avoids any calculation to prevent errors. This measure helps businesses understand how effectively they are converting their project sales potential into actual sales revenue.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the profit margin for a project by subtracting the total costs from the total sales. Here's a breakdown of what it does:

1. **Total Sales Calculation**: The expression starts with `sum(vw_Timesheet[SalesAmount])`, which adds up all the sales amounts recorded in the `vw_Timesheet` table. This gives you the total revenue generated from the project.

2. **Total Costs Calculation**: Next, it calculates the total costs with `sum(vw_Timesheet[CostAmount])`. This sums up all the costs associated with the project, which could include expenses like labor, materials, and other overheads.

3. **Profit Margin Calculation**: Finally, the expression subtracts the total costs from the total sales: `sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`. The result of this calculation represents the project's profit margin, indicating how much money is left after covering all costs.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total sales price in a base currency for different projects and clients from a timesheet view. Here’s a breakdown of what it does in simple business terms:

1. **SUMMARIZE Function**: The expression starts by creating a summary table from the `vw_Timesheet` data. This summary groups the data by two key dimensions: `ProjectProfile` and `Client`. Essentially, it organizes the data so that we can look at sales prices for each unique combination of project and client.

2. **SELECTEDVALUE Function**: Within this summary, it retrieves the sales price in the base currency from another table called `tbl_Project`. The `SELECTEDVALUE` function is used here to get the specific sales price for the current context of the project being summarized. If there’s only one sales price for that project, it returns that value; if there are multiple, it returns blank.

3. **MeasureValue**: The result of the `SELECTEDVALUE` is stored in a new column called "MeasureValue" in the summary table. This column represents the sales price in the base currency for each project-client combination.

4. **SUMX Function**: Finally, the `SUMX` function iterates over the summary table created in the previous step. It sums up all the values in the "MeasureValue" column. This gives us the total sales price in the base currency across all projects and clients listed in the timesheet.

In summary, this DAX expression effectively calculates the total sales price in a base currency for all projects and clients recorded in the timesheet, allowing businesses to understand their sales performance in a standardized currency.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of unique employees who have a "Valid" contract status, regardless of any filters that might be applied to the data.

Here's a breakdown of what each part does:

1. **CALCULATE**: This function changes the context in which the data is evaluated. It allows us to apply specific filters to our calculation.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it tells us how many different employees are present.

3. **ALL(vw_Timesheet)**: This function removes any filters that might be applied to the `vw_Timesheet` table. By using this, we ensure that the count of employees is not affected by any other filters that might limit the data (like date ranges or departments).

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This is a filter condition that specifies we only want to count employees whose contract status is "Valid". 

Putting it all together, this DAX expression calculates the total number of unique employees who currently have a valid contract, ignoring any other filters that might be in place in the report or dashboard. This is useful for understanding the size of the workforce that is actively engaged under valid contracts at any given time.

**`TotalActualHours`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression `SUM(vw_Timesheet[Hours])` is used to calculate the total number of hours recorded in a timesheet. 

Here's a breakdown of what it does:

- **SUM**: This function adds up all the values in a specified column.
- **vw_Timesheet**: This refers to a table (or view) that contains timesheet data, which likely includes records of hours worked by employees.
- **[Hours]**: This is the specific column within the `vw_Timesheet` table that contains the number of hours each employee has logged.

In simple terms, this measure totals all the hours worked as recorded in the timesheet, providing a single number that represents the overall hours worked by all employees during a specific period or under certain conditions. This is useful for tracking labor costs, project hours, or overall productivity.

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

2. **MAX(vw_Timesheet[HoursPerWeek])**: For each employee in the unique list, this part finds the maximum number of hours they are contracted to work in a week. If an employee has multiple entries, it will take the highest value of hours recorded.

3. **SUMX(...)**: This function iterates over the unique list of employees and sums up the maximum hours per week for each employee. In other words, it adds together the highest contracted hours for all employees.

Overall, the measure 'TotalContractedHours' gives you the total of the maximum weekly hours that all employees are contracted to work, ensuring that if an employee has multiple records, only their highest contracted hours are counted. This helps in understanding the total capacity of contracted work hours across the workforce.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression `[TotalActualHours] - [TotalContractedHours]` calculates the difference between two values: the total actual hours worked and the total contracted hours agreed upon.

In simple business terms:

- **TotalActualHours**: This represents the actual number of hours that have been worked by employees or resources on a project or task.
- **TotalContractedHours**: This is the number of hours that were originally agreed upon in a contract for the project or task.

By subtracting the total contracted hours from the total actual hours, this measure provides insight into how many hours have been worked beyond or below what was initially planned. 

- If the result is positive, it indicates that more hours were worked than contracted, which could suggest overwork or additional effort.
- If the result is negative, it means that fewer hours were worked than contracted, which might indicate underperformance or efficiency.

Overall, this measure helps businesses understand their resource utilization and whether they are meeting their contractual obligations in terms of hours worked.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'TotalHoursTracked' that calculates the total number of hours tracked in a timesheet. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The expression first checks if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Zero if Blank**: If the sum of hours is blank (meaning no hours have been logged), the expression returns a value of 0. This ensures that when there are no entries, the measure does not show a blank value, which could be confusing.

3. **Return Total Hours if Not Blank**: If there are hours recorded (the sum is not blank), the expression simply returns the total sum of hours from the `vw_Timesheet` table.

In summary, this measure ensures that when calculating total hours tracked, it will either show the actual total hours if they exist or return 0 if no hours have been logged. This makes it easier for users to understand the data without encountering blanks in their reports.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total hours tracked in a timesheet, but it includes a check to handle cases where no hours have been recorded.

Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the `Hours` column of the `vw_Timesheet` table.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (meaning there are no hours recorded). 

3. **IF(...)**: The IF function evaluates the condition provided by ISBLANK. 
   - If the total hours are blank (i.e., no hours have been tracked), it returns the text "Empty".
   - If there are hours recorded, it returns the total number of hours calculated by the SUM function.

In simple terms, this measure helps to identify whether any hours have been logged in the timesheet. If no hours are logged, it clearly indicates that by returning "Empty". If hours are logged, it provides the total number of hours tracked. This is useful for reporting and ensuring that time tracking is being properly maintained.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** This DAX expression calculates the difference between two values related to hours tracked for employees. Here’s a breakdown of what it does in simple business terms:

1. **[TotalHoursTracked]**: This part represents the total number of hours that have been tracked for a specific employee or a group of employees. It is a measure that sums up all the hours recorded.

2. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to find a specific value related to hours worked.

3. **MAXX and DISTINCT**: Inside the CALCULATE function, the expression uses MAXX to find the maximum value of hours worked per week from a distinct list of hours recorded in the `vw_Timesheet` table. This means it looks at all the unique weekly hours logged and identifies the highest number of hours worked in any week.

4. **ALLEXCEPT**: This part of the expression ensures that the calculation of the maximum weekly hours is done while keeping the context of the specific employee (identified by `EmployeeID`). It removes any other filters that might affect the calculation, focusing solely on the hours for that employee.

5. **Final Calculation**: The overall expression subtracts the maximum weekly hours (calculated in the previous step) from the total hours tracked. 

### What It Achieves:
In summary, this DAX measure calculates how many hours an employee has tracked beyond their maximum recorded hours in any single week. If the result is positive, it indicates that the employee has worked more hours than their highest weekly average. If it’s zero or negative, it means they have not exceeded their maximum weekly hours. This can help in understanding employee workload and tracking overtime or productivity.

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
- **DAX Explanation (Generated):** The DAX expression you've provided calculates the difference between two values related to hours tracked in a timesheet system. Here's a breakdown of what it achieves in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked by an employee over a specific period.

2. **Maximum Hours Per Week**: The expression then calculates the maximum number of hours that have been recorded per week for the same employee. This is done using the `MAXX` function, which looks at the distinct values of `HoursPerWeek` from the `vw_Timesheet` table. Essentially, it finds the highest number of hours that the employee has logged in any single week.

3. **Context Management**: The `CALCULATE` function is used to modify the context in which the maximum hours are calculated:
   - `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])` ensures that any filters applied to the `HoursPerWeek` column are ignored. This means the calculation considers all weeks, not just those filtered by specific criteria.
   - `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` keeps the context focused on the specific employee by removing filters from all other columns in the `vw_Timesheet` table except for the `EmployeeID`. This ensures that the maximum hours are calculated only for the relevant employee.

4. **Final Calculation**: The final result of the measure, `trackedDiff2`, is the difference between the total hours tracked by the employee and the maximum hours they have logged in any single week. 

In summary, this measure helps to identify how much the total hours tracked by an employee exceeds their maximum weekly hours. This could be useful for understanding workload, identifying potential overwork, or ensuring compliance with work hour regulations.

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

Here's a breakdown of what it does:

1. **Variable Definition (FilteredHours)**: The expression starts by defining a variable called `FilteredHours`. This variable holds a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**: The filtering is based on two main conditions:
   - The project qualifies as billable if `vw_Timesheet[QualifyPrj]` equals 1 (indicating it's a qualifying project) **and** `vw_Timesheet[BillablePrj]` equals 1 (indicating it's a billable project).
   - Alternatively, it includes any projects that contain the phrase "Customer Success Services" in their name. This is checked using the `SEARCH` function, which looks for that specific text within the `vw_Timesheet[Project]` field.

3. **Summing Up Hours**: After applying the filters, the expression uses the `SUMX` function to iterate over the filtered results (the `FilteredHours` variable) and sums up the `vw_Timesheet[Hours]` for all the entries that meet the filtering criteria.

**In summary**, this DAX measure calculates the total number of hours worked on projects that are either specifically marked as billable or fall under the "Customer Success Services" category. This helps the business understand how many hours can be billed to clients, ensuring that only relevant projects are included in the total.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** This DAX expression is used to calculate a measure called 'UTI_TOTALHOURS'. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the values in the 'Hours' column from the 'vw_Timesheet' table. Essentially, it totals the number of hours recorded.

2. **CALCULATE(...)**: The CALCULATE function modifies the context in which the data is evaluated. In this case, it is used to apply a specific filter to the data being summed.

3. **vw_Timesheet[QualifyPrj] = 1**: This filter condition specifies that only the rows in the 'vw_Timesheet' table where the 'QualifyPrj' column equals 1 should be included in the total. This means that the measure is only considering hours that are associated with projects that qualify under a certain criterion (indicated by the value 1).

In summary, the 'UTI_TOTALHOURS' measure calculates the total number of hours worked on projects that meet a specific qualification (where 'QualifyPrj' is 1). This helps businesses understand how many hours are being spent on eligible projects, which can be important for budgeting, resource allocation, and performance tracking.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the "Billable Hour Ratio" for active employees within a business context. Here's a breakdown of what it does in simple terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable captures the total number of hours that have been billed to clients. It is derived from another measure called `[UTI_TotalBillableHours]`.
   - `_TotalHours`: This variable captures the total number of hours worked by employees, which comes from the measure `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The expression checks if there are any active contacts (employees) using `[Msr_ActiveContacts]`. If there are no active contacts, the calculation does not proceed.
   - If there are active contacts, it then checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the calculation does not proceed further.
   - If both conditions are met (active contacts exist and total hours are available), it calculates the Billable Hour Ratio by dividing the total billable hours by the total hours worked.

3. **Handling Division**:
   - The `DIVIDE` function is used to perform the division. This function is preferred because it safely handles cases where the denominator (total hours) might be zero or blank. If the result of the division is blank (which could happen if `_TotalHours` is zero), it returns 0 instead of an error.

In summary, this DAX measure calculates the ratio of billable hours to total hours worked for active employees, providing insights into how effectively employee time is being utilized for billable work. If there are no active employees or if total hours are not available, it ensures that the measure returns a meaningful result (either a ratio or zero) without causing errors.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

The following filters are applied to the entire report:

- Filter on (Name: `912435ffa0798d130252`) (Type: N/A, Definition: `N/A`)

### <a name="report-pages"></a>Report Pages

#### <a name="page-customer-analysis"></a>Page: Customer Analysis

*Internal Name: `ecc7667875c00809763b`, Ordinal: 0*

##### Page Level Filters

- Filter on (Name: `f2cd73450105bd670e4c`) (Type: N/A, Definition: `N/A`)

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

- Filter on (Name: `f2cd73450105bd670e4c`) (Type: N/A, Definition: `N/A`)

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

