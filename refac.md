# Power BI Model & Report Documentation

*Generated on: 2025-04-28 23:44:49*

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

The structure of the model indicates a star schema, where the 'vw_Timesheet' serves as a central fact table linked to multiple dimension tables, including 'lkp_Project', 'lkp_Unit', and 'lkp_Code'. This setup allows for detailed insights into how hours are allocated across different projects and units, as well as identifying any discrepancies in timesheet submissions through the 'vw_missing_timesheet' table. Overall, this data model is likely aimed at enhancing operational efficiency, ensuring accurate project tracking, and supporting strategic decision-making through data-driven insights.

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

The 'DimDate' table serves as a comprehensive date dimension, providing essential temporal attributes that enable businesses to analyze trends and performance over time. By offering detailed breakdowns of dates, including day, week, month, and quarter information, this table supports effective reporting and enhances time-based analytics across various business metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year as a string, facilitating time-based analysis and reporting within the DimDate table. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting within the dataset. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient querying and data enrichment processes related to time-based analysis. |
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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CurrentYearFlag' in a data model, specifically in a date dimension table (DimDate). Here's what it does in simple business terms:

1. **YEAR(DimDate[ShortDate])**: This part extracts the year from the date in the 'ShortDate' column of the DimDate table. For example, if the date is '2023-05-15', it will return '2023'.

2. **YEAR(TODAY())**: This part gets the current year based on today's date. If today is any date in 2023, this will return '2023'.

3. **IF(... <= ...)**: The expression checks if the year extracted from the 'ShortDate' is less than or equal to the current year. 

4. **1 or 0**: If the condition is true (meaning the date is in the current year or a previous year), it returns a '1'. If the condition is false (meaning the date is in a future year), it returns a '0'.

### What it achieves:
The 'CurrentYearFlag' column will have a value of '1' for all dates that are in the current year or any previous year, and a value of '0' for any dates that are in the future. This can be useful for filtering or analyzing data based on whether dates are current or past versus future.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table is designed to track and analyze the distribution of work hours and their corresponding percentages across various projects or tasks, enabling businesses to assess resource allocation and productivity effectively. This table facilitates insights into time management and performance metrics, supporting informed decision-making for operational efficiency.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column stores string representations of hours worked alongside their corresponding percentage of total hours, providing insights into employee time allocation and productivity metrics. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and percentage calculations, facilitating the enrichment of data for analytical purposes. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, with associated qualification levels and sorting criteria to facilitate efficient data retrieval and reporting. This table is essential for maintaining data integrity and consistency in code usage throughout the organization's analytics and operational processes.

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
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, serving as a key identifier for enriching employee-related data within the lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, detailing key attributes such as project names, qualification scores, billing amounts, and associated groups, enabling businesses to analyze project performance and financial metrics effectively.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing amount associated with each project, facilitating financial tracking and analysis. |
| `Group` | `string` | The 'Group' column identifies the category or classification of each project within the lookup table, facilitating data enrichment and organization. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the lkp_Project table. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, with numerical values representing different levels of qualification criteria met. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their characteristics such as billable department status and external affiliations. This table is essential for analyzing resource allocation and financial performance across different units, facilitating informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column represents the identifier for departments that are eligible for billing, facilitating the tracking of billable units within the organization. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column identifies the specific organizational unit within the company, facilitating data enrichment and analysis related to departmental structures. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column contains string values that represent the external type classification for owned sub-units, facilitating data enrichment and categorization within the 'lkp_Unit' table. |
| `Unit` | `string` | The 'Unit' column contains string values representing the various measurement units used for data enrichment purposes. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, status, leadership, and financial aspects. This table enables businesses to effectively manage and analyze their projects, facilitating better decision-making and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and performance analysis within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column contains string values that represent the administrative details or oversight responsibilities associated with each project listed in the tbl_Project table. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates within the 'tbl_Project' table. |
| `ClientName` | `string` | The 'ClientName' column stores the name of the client associated with each project, facilitating identification and management of client-specific projects within the 'tbl_Project' table. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating the tracking and management of client-related data within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated in the 'tbl_Project' table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project, represented as a string. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the timestamp of when each project entry was initially created in the 'tbl_Project' table. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or business unit within Devoteam that the project aligns with, facilitating targeted resource allocation and performance tracking. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column indicates the scheduled date and time for the completion of the project, facilitating project timeline management and tracking. |
| `Project` | `string` | The 'Project' column contains the names of various projects associated with the entries in the tbl_Project table, serving as a key identifier for tracking and managing project-related data. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed narrative of the project's objectives, scope, and key features, providing essential context for stakeholders and team members. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the category or classification of the project, facilitating organization and management within the 'tbl_Project' table. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a textual summary of the project group, detailing its objectives and scope within the context of the overall project management framework. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) serves as a unique identifier for each project in the 'tbl_Project' table, facilitating efficient data retrieval and management. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the project's execution within the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency sales price of a project, stored as a decimal value to facilitate accurate financial calculations and reporting. |
| `Startdate` | `dateTime` | The 'Startdate' column records the date and time when a project is initiated, serving as a key reference point for project timelines and scheduling within the 'tbl_Project' table. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, allowing for effective tracking and management of project timelines and deliverables. |
| `Statuscode` | `string` | The 'Statuscode' column indicates the current operational status of each project, represented as a string, to facilitate tracking and management of project progress within the 'tbl_Project' table. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, expressed as a decimal to accommodate precise financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table serves as a critical resource for identifying employees who have not submitted their timesheets within a specified timeframe, enabling managers to address compliance issues and ensure accurate payroll processing. By tracking relevant details such as employee and manager information, contract dates, and hours, this table facilitates timely interventions to enhance workforce accountability and operational efficiency.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column represents the total number of hours worked by an employee, recorded as a double data type, within the context of identifying missing timesheet entries in the 'vw_missing_timesheet' table. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to terminate, providing essential information for tracking contract durations within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to assist in identifying discrepancies in timesheet submissions within the 'vw_missing_timesheet' table. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract for each employee, providing essential information for tracking compliance and contract management within the missing timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on outstanding submissions. |
| `Hours_` | `double` | The 'Hours_' column represents the total number of hours recorded for each entry in the 'vw_missing_timesheet' table, stored as a double to accommodate fractional hours. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_missing_timesheet' table identifies the unique identifier of the manager responsible for overseeing the employee associated with the missing timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column contains the name of the manager responsible for overseeing the employees whose timesheets are missing, facilitating accountability and follow-up actions. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee work hour reporting. |
| `Project` | `string` | The 'Project' column contains the names of projects associated with missing timesheets, enabling identification and tracking of unreported work activities. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project associated with the timesheet entries, facilitating the tracking and management of time allocation across various projects. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected (true) or not (false) in the context of tracking missing timesheet submissions. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of tracking and analyzing missing timesheet entries. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double to accommodate precise pricing information within the 'vw_missing_timesheet' table. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with missing timesheet entries, formatted to facilitate quick reference and analysis. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific sub-division or department within the organization associated with each entry in the 'vw_missing_timesheet' table, facilitating targeted analysis of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' view. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the tasks or activities recorded in the timesheet, providing context for the hours logged by employees in the 'vw_missing_timesheet' table. |
| `Week_` | `int64` | The 'Week_' column (int64) represents the numerical week of the year associated with each entry in the 'vw_missing_timesheet' table, facilitating the tracking and analysis of timesheet submissions by week. |
| `Year_` | `int64` | The 'Year_' column (int64) in the 'vw_missing_timesheet' table represents the calendar year associated with each timesheet entry, facilitating the analysis of timesheet data over time. |

##### Calculated Columns

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column indicates the department associated with billable hours for employees, facilitating the tracking and management of time spent on chargeable projects within the 'vw_missing_timesheet' table.
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

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This could indicate that, in the absence of specific information, the default assumption is that the item is billable.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, suggesting that the item is billable.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that the item is not billable.

In summary, this DAX expression effectively categorizes items as billable or not based on the related 'BillableDep' values. It assigns a value of **1** for billable items (including those with no specific data) and **0** for non-billable items (those explicitly marked with a zero).

**`CC_ActiveEmployees`** (`string`)

- **Description:** The 'CC_ActiveEmployees' column contains a string representation of the active employees in the cost center, used to identify those who may be missing timesheet submissions in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
IF(vw_missing_timesheet[ShortDate] >= vw_missing_timesheet[ContractStartDate] && 
			      (vw_missing_timesheet[ShortDate] <= vw_missing_timesheet[ContractEndDate] || ISBLANK(vw_missing_timesheet[ContractEndDate]))
			        , 1
			        , 0
			    )
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named `CC_ActiveEmployees` in a data model, specifically within a table called `vw_missing_timesheet`. Here's what it does in simple business terms:

1. **Purpose**: The expression checks whether an employee is considered "active" based on their contract dates and a specific date (referred to as `ShortDate`).

2. **Conditions Checked**:
   - **Contract Start Date**: It first checks if the `ShortDate` (the date being evaluated) is on or after the employee's `ContractStartDate`. This means the employee's contract must have started before or on this date for them to be considered active.
   - **Contract End Date**: Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. If the `ContractEndDate` is blank (meaning the employee's contract does not have an end date), the employee is still considered active.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the contract has no end date), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns `0`, indicating that the employee is not active.

In summary, this DAX expression effectively identifies whether an employee is currently active based on their contract start and end dates, providing a simple way to flag active employees in the dataset.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to assist in identifying records that require further attention in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IncompleteFlag' in a data model. Here's what it does in simple business terms:

- The expression checks the value in the 'MissingHours' column of the 'vw_missing_timesheet' table.
- If the value of 'MissingHours' is less than 0, it means that there is an issue (specifically, it indicates that there are negative hours, which typically shouldn't happen).
- In this case, the expression returns a value of 1, which can be interpreted as a flag indicating that the record is incomplete or problematic.
- If the value of 'MissingHours' is 0 or greater, the expression returns a value of 0, indicating that there is no issue with that record.

In summary, this DAX expression effectively flags records as incomplete (1) when there are negative missing hours and marks them as complete (0) when there are no negative hours. This helps in identifying and addressing potential data issues in the timesheet records.

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

1. **Check for Related Data**: The expression starts by checking if there is a related value in the 'Unit' column of the 'lkp_Unit' table. This is done using the `RELATED` function, which retrieves a value from a related table based on the relationship defined in the data model.

2. **Handle Missing Values**: The `ISBLANK` function checks if the value retrieved from the 'lkp_Unit[Unit]' column is blank (i.e., there is no corresponding unit found). 

3. **Return Results**: 
   - If the value is blank (meaning there is no related unit), the expression returns the string "Unknown". This indicates that the system could not find a matching unit for the current record.
   - If there is a related unit (the value is not blank), it returns the actual unit name from the 'lkp_Unit[Unit]' column.

In summary, this DAX expression effectively populates the 'MAIN_UNIT' column with either the name of the unit associated with the current record or "Unknown" if no unit is found. This helps ensure that users can easily identify records with missing unit information.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column represents the year and week number in a string format, facilitating the identification of specific time periods for analyzing missing timesheet entries.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX expression creates a new calculated column called 'YearWeek' that combines the year and the week number of a date from the 'ContractEndDate' column.

Here's a breakdown of what it does:

1. **YEAR([ContractEndDate])**: This part extracts the year from the 'ContractEndDate'. For example, if the date is December 15, 2023, this will return "2023".

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the 'ContractEndDate'. For instance, if the date is in the 3rd week of December, it will return "50" (assuming the week starts on Sunday).

3. **FORMAT(..., "00")**: This ensures that the week number is always displayed as two digits. So, if the week number is 5, it will be formatted as "05".

4. **Concatenation (&)**: The expression combines the year and the formatted week number into a single string, separated by a hyphen. For example, if the year is 2023 and the week number is 50, the final result will be "2023-50".

In summary, this DAX expression effectively creates a label that represents the year and week number of the contract's end date in a clear and standardized format, such as "2023-50". This can be useful for reporting or analyzing data by week and year.

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

1. **CALCULATE Function**: The outer function, `CALCULATE`, is used to modify the context in which data is evaluated. In this case, it is changing the context to focus on a specific condition.

2. **DISTINCTCOUNT Function**: Inside the `CALCULATE`, the `DISTINCTCOUNT` function counts the number of unique entries in the 'EmployeeName' column from the 'vw_missing_timesheet' table. This means it will count how many different employees are listed.

3. **Condition**: The second part of the `CALCULATE` function specifies a condition: it only considers rows where the 'MissingHours' value is less than 0. This means it is looking for employees who have negative hours recorded, which could indicate an issue such as over-reporting or a data entry error.

In summary, this DAX expression calculates the number of unique employees who have recorded negative hours in the 'vw_missing_timesheet' table. This measure helps identify potential problems with timesheet entries, allowing the business to address any discrepancies or errors in reporting.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have recorded negative missing hours in a timesheet report. 

Here's a breakdown of what it does:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This counts the number of unique values in a specified column. Here, it counts the unique `EmployeeID`s from the `vw_missing_timesheet` table.

3. **Filter Condition**: The expression includes a filter that only considers rows where the `MissingHours` value is less than 0. This means it focuses on cases where employees have reported negative hours, which could indicate an error or a specific situation that needs attention.

In summary, the measure `CountNegativeMissingHours` calculates how many different employees have negative missing hours recorded in the timesheet. This information can help the business identify potential issues with time reporting or attendance.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of active employees who have missing timesheet hours for a specific period. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The expression starts by looking at a data table called `vw_missing_timesheet`, which likely contains information about employees, their hours worked, and their contractual hours.

2. **Summarization**: It summarizes the data by grouping it based on several key attributes:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a more specific division within the unit)

3. **Calculating Missing Hours**: For each group, it calculates two important metrics:
   - **MissingHours**: This is calculated by taking the total hours worked (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contractual hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has not logged enough hours compared to what they are contracted for.
   - **MinActiveEmp**: This metric captures the minimum value of active employees (`MIN(vw_missing_timesheet[CC_ActiveEmployees])`) for that group. If this value is 1, it means there is at least one active employee in that group.

4. **Filtering**: The expression then filters the summarized data to find only those groups where:
   - There is at least one active employee (`[MinActiveEmp] = 1`)
   - The calculated missing hours are less than zero (`[MissingHours] < 0`), indicating that the employee has not logged enough hours.

5. **Counting Active Employees**: Finally, the expression counts the number of rows that meet the filtering criteria, which corresponds to the number of active employees who have missing timesheet hours.

In summary, this DAX measure calculates how many active employees have not logged enough hours in their timesheets for a given year and week, helping the business identify potential issues with timesheet compliance.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of "missing hours" for active employees who have not met their contracted hours in a specific time frame. Here's a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code first creates a variable called `ActiveEmployees`. This variable filters a summarized view of the data from a table called `vw_missing_timesheet`. It focuses on specific columns such as employee names, year, week, unit, subunit, and calculates two key metrics:
   - **MissingHours**: This is calculated by taking the total hours worked by an employee (from `vw_missing_timesheet[Hours_]`) and subtracting the maximum contracted hours they should have worked (from `vw_missing_timesheet[ContractHours]`). If this value is negative, it indicates that the employee has not worked enough hours compared to their contract.
   - **MinActiveEmp**: This checks if the employee is active by finding the minimum value of a column that indicates whether the employee is active (`vw_missing_timesheet[CC_ActiveEmployees]`). If this value is 1, it means the employee is currently active.

2. **Filter for Relevant Employees**: The `FILTER` function then narrows down the summarized data to only include those employees who are active (`MinActiveEmp = 1`) and have negative missing hours (`MissingHours < 0`). This means we are only looking at active employees who have not fulfilled their contracted hours.

3. **Calculate Total Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered list of active employees and sums up their missing hours. This gives the total number of hours that active employees are missing compared to what they were contracted to work.

In summary, this DAX measure calculates the total hours that active employees have not worked compared to their contractual obligations, focusing only on those who are currently active and have a deficit in hours worked.

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

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key dimensions: `Week_` (which represents the week of the year) and `EmployeeID` (which identifies each employee). This means that for each unique combination of week and employee, we will summarize the data.

3. **Calculating Maximum Contract Hours**: For each group (each employee in each week), the expression calculates the maximum contract hours using `MAX(vw_missing_timesheet[ContractHours])`. This means if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

4. **Summing Up**: Finally, the `SUMX` function takes the summarized data and adds up all the maximum contract hours calculated for each employee across all weeks. 

In summary, this DAX measure provides the total expected contract hours for all employees, ensuring that if there are multiple records for an employee in a week, only the highest contract hours are considered. This helps in understanding the overall expected workload for employees on a weekly basis.

**`MISSING%`**

- **DAX Expression:**
```dax
DIVIDE([Dax_EmpCount]-[Dax_EmpCount_MissingTS],[Dax_EmpCount]) * 100
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of employees that are not missing time stamps (TS) compared to the total number of employees. Here’s a breakdown of what it does:

1. **[Dax_EmpCount]**: This represents the total number of employees.

2. **[Dax_EmpCount_MissingTS]**: This represents the number of employees who are missing time stamps.

3. **[Dax_EmpCount] - [Dax_EmpCount_MissingTS]**: This part calculates the number of employees who have valid time stamps by subtracting the number of employees missing time stamps from the total employee count.

4. **DIVIDE(..., [Dax_EmpCount])**: The DIVIDE function takes the number of employees with valid time stamps and divides it by the total number of employees. This gives us a fraction that represents the proportion of employees with valid time stamps.

5. **... * 100**: Finally, multiplying by 100 converts this fraction into a percentage.

In summary, this measure calculates the percentage of employees who have valid time stamps, helping the business understand how many employees are properly accounted for in terms of time tracking. A higher percentage indicates better compliance with time tracking requirements.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked or contracted hours. Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression finds the maximum number of hours that are contracted for a particular project or employee. Essentially, it looks at the highest value in the 'ContractHours' column from the 'vw_missing_timesheet' table, which represents the total hours that should ideally be worked according to the contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part finds the maximum number of hours that have actually been recorded or worked, as indicated in the 'Hours_' column of the same table. It shows the highest value of hours that have been logged by the employee or for the project.

3. **Subtraction**: The expression then subtracts the maximum recorded hours (actual hours worked) from the maximum contracted hours (expected hours). 

### What it Achieves:
The result of this calculation gives you the number of hours that are missing or unaccounted for. In other words, it tells you how many hours should have been worked (according to the contract) but were not recorded or logged. This measure is useful for identifying gaps in timesheet reporting and ensuring that employees are meeting their contracted hours.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of completeness regarding missing hours in relation to expected contract hours on a weekly basis. Here's a breakdown of what it does:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part adds up all the missing hours recorded in the `vw_missing_timesheet` table. Essentially, it gives the total number of hours that were not accounted for in timesheets.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that are expected to be worked in a week according to the contract.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the total missing hours are subtracted from the expected contract hours. This calculation shows how many hours are missing compared to what should have been worked.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The `DIVIDE` function takes the result from the previous step (the difference between missing hours and expected hours) and divides it by the expected contract hours. This gives a ratio or percentage of how much of the expected hours are missing. The third argument (0) ensures that if the expected hours are zero, the result will be zero instead of causing an error.

In summary, this measure calculates the percentage of expected working hours that are missing due to unreported timesheets. A higher percentage indicates a greater discrepancy between expected and actual hours worked, which could signal issues with timesheet compliance or employee attendance.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total number of unique employees who have missing timesheets, grouped by their respective units and weeks. Here's a breakdown of what it does in simple business terms:

1. **Data Source**: The expression uses a table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including whether their submissions are complete or incomplete.

2. **Grouping**: The `SUMMARIZE` function is used to create a summary table. This table groups the data by three columns:
   - `Week_`: The week of the year.
   - `EmployeeName`: The name of the employee.
   - `MAIN_UNIT`: The main unit or department the employee belongs to.

3. **IncompleteFlag**: For each group created by the `SUMMARIZE` function, it calculates a new column called "IncompleteFlag". This column uses the `MAX` function to determine if there is any record in that group indicating that the employee has an incomplete timesheet. If at least one record shows an incomplete timesheet, the flag will be set to 1 (true); otherwise, it will be 0 (false).

4. **Summation**: Finally, the `SUMX` function iterates over the summarized table and sums up the values in the "IncompleteFlag" column. This effectively counts how many unique employees have at least one incomplete timesheet for each week and unit.

In summary, this DAX measure provides a count of unique employees who have not submitted their timesheets completely, organized by week and unit. This information can help management identify areas where timesheet compliance may need improvement.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as the financial year, submission dates, and associated project and management identifiers. This table enables businesses to analyze labor allocation, project performance, and resource management effectively.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column identifies the name of the client associated with each timesheet entry, facilitating the tracking of billable hours and project allocation. |
| `Code2` | `string` | The 'Code2' column contains a string value that serves as a secondary identifier for categorizing or referencing specific entries within the timesheet data. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract is set to expire, providing essential information for managing workforce planning and contract renewals within the timesheet data. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and payroll calculations within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract associated with the timesheet entry, represented as a string value. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, providing a real-time snapshot for reporting and analysis within the timesheet data context. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing a timestamp for tracking contract changes within the timesheet data. |
| `CostAmount` | `double` | The 'CostAmount' column represents the monetary value associated with the hours worked, expressed as a double, within the timesheet data for accurate financial reporting and analysis. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor or resources for each entry in the timesheet, stored as a double for precision in financial calculations. |
| `Currency` | `string` | The 'Currency' column (string) in the 'vw_Timesheet' table specifies the type of currency used for financial transactions recorded in the timesheet entries. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the individual or entity responsible for payment, providing essential identification for financial tracking within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee associated with the timesheet entries in the 'vw_Timesheet' table, facilitating accurate tracking and reporting of employee work hours. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column contains the concatenated identifier and name of employees, facilitating easy identification and tracking of timesheet entries within the 'vw_Timesheet' table. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating the identification of personnel for time tracking and reporting purposes. |
| `Employer` | `string` | The 'Employer' column contains the name of the organization or company that employs the individual associated with the timesheet entries. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating the tracking and management of employee work hours by employer. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to the timesheet entries, providing further insights into the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with the recorded timesheet entries, facilitating financial analysis and reporting. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, allowing for precise tracking of labor input for payroll and project management purposes. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating analysis of work patterns and resource allocation. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, providing clarity on the nature of the work performed. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, facilitating accurate reporting and analysis of employee work patterns. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries, facilitating accountability and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the names of individual IPMIDs associated with each timesheet entry, facilitating the identification and tracking of project management identifiers. |
| `JobTitle` | `string` | The 'JobTitle' column contains the titles of positions held by employees, providing context for their roles in relation to the timesheet data. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking records within the 'vw_Timesheet' table, facilitating data integration and enrichment processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_Timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, facilitating the identification of managerial oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key associated with the manager responsible for overseeing the timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct manager, facilitating the identification of supervisory relationships within the timesheet data. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries, detailing the hours allocated to various tasks within the timesheet records. |
| `Project` | `string` | The 'Project' column contains the name of the project associated with each timesheet entry, allowing for the tracking and analysis of time spent on specific projects. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with the timesheet entries, facilitating the tracking and reporting of time spent on specific projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that represents the specific characteristics and details of the project associated with each timesheet entry, facilitating better project management and resource allocation. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the project profile associated with each timesheet entry, facilitating the tracking and analysis of time spent on specific projects. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into the types of work being performed. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected (true) or accepted (false). |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness and false indicating it is not yet ready. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of timesheet data analysis. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current approval or processing state of each timesheet entry, reflecting whether it is pending, approved, or rejected. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation, with a value of true representing taxed hours and false representing non-taxed hours. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, providing insights into resource allocation and project management. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores a unique identifier for each timesheet entry, facilitating the tracking and categorization of employee work hours within the vw_Timesheet table. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the tasks or activities recorded in the timesheet, providing context for the hours logged by employees. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table specifies the measurement unit associated with the recorded time entries, facilitating accurate tracking and reporting of work hours. |
| `UnitCode` | `string` | The 'UnitCode' column represents a unique identifier for the unit associated with each timesheet entry, facilitating the categorization and tracking of work hours by specific organizational units. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and data enrichment within the timesheet management system. |
| `YearWeek` | `string` | The 'YearWeek' column represents the year and week number in a string format, facilitating the tracking and reporting of timesheet entries by specific weeks within a given year. |

##### Calculated Columns

**`Approved_With_V_Z`** (`string`)

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheets, specifically denoting whether they have been approved with a specific validation or criteria represented by 'V_Z'.
- **DAX Expression:**
```dax
IF(
			    [Approved] = TRUE() || 
			    vw_Timesheet[TimesheetCode] IN {"V", "Z"}, 
			    1, 
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'Approved_With_V_Z'. Here's a simple breakdown of what it does:

1. **Condition Check**: The expression checks two conditions:
   - It first checks if the value in the column `[Approved]` is `TRUE`. This means it is verifying if an item (like a timesheet or request) has been approved.
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means it is looking for specific codes that might represent certain types of entries.

2. **Output Values**: 
   - If either of the conditions is met (meaning the item is approved or it has a TimesheetCode of "V" or "Z"), the expression returns a value of `1`.
   - If neither condition is met, it returns a value of `0`.

**What It Achieves**: 
This calculated column effectively flags entries as "approved" (with a value of `1`) if they are either explicitly approved or fall under specific categories represented by the codes "V" or "Z". If neither condition is satisfied, it marks them as "not approved" (with a value of `0`). This can be useful for filtering or analyzing data based on approval status and specific codes.

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

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of a specific billable dependency, the default assumption is that it is billable.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that there is a billable dependency present.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that there is no billable dependency.

In summary, this DAX expression effectively categorizes each entry based on whether it has a billable dependency or not. It assigns a value of **1** if there is a billable dependency (either because it is not blank or not zero) and a value of **0** if there is a zero value indicating no dependency. This helps in identifying which entries are billable based on their relationship to the 'lkp_Unit' table.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'BillablePrj' in a data model, specifically for a table named `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether a particular entry in the timesheet is considered "billable" or not. A billable entry is one that can be charged to a client or project.

2. **Conditions Checked**:
   - **Sales Price**: It first checks if the `SalesPrice` for the entry is greater than 0. If there is a positive sales price, it indicates that the work done can be billed.
   - **Project Profile Code**: Next, it checks if the `ProjectProfileCode` is equal to 81. This specific code likely represents a type of project that is always billable.
   - **Project Name**: Finally, it looks for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it suggests that the project is related to customer success services, which are typically billable.

3. **Output**: 
   - If any of the above conditions are true (meaning the entry is billable), the expression returns a value of **1**.
   - If none of the conditions are met (meaning the entry is not billable), it returns a value of **0**.

In summary, this DAX expression effectively flags timesheet entries as billable (1) or not billable (0) based on specific criteria related to sales price, project profile, and project name. This helps in identifying which work can be invoiced to clients.

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

2. **Hours per Week**: It also takes the number of hours that employee works per week, represented by `('vw_Timesheet'[HoursperWeek])`.

3. **Combining Information**: The expression then combines these two pieces of information into one string. It uses " - " as a separator between the employee's name and their weekly hours. 

So, for example, if an employee named "John Doe" works 40 hours per week, the resulting string in the 'Employee_WeeklyHours' column would be "John Doe - 40".

In summary, this DAX expression helps to create a clear and concise representation of each employee's name along with their weekly working hours, making it easier to read and understand the data at a glance.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'GroupCat' in a data model, specifically for a table called `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Lookup for Group**: The expression first tries to find a corresponding 'Group' value from a lookup table called `lkp_Project`. It does this by looking up the 'PROJECT' field in the `lkp_Project` table that matches the 'Project' field in the `vw_Timesheet` table.

2. **Check for Existence**: The `NOT(ISBLANK(...))` part checks if the lookup returned a valid 'Group' value. If it did (meaning there is a match), it uses that 'Group' value as the result for the 'GroupCat' column.

3. **Fallback Logic**: If the lookup does not return a valid 'Group' (meaning there is no match), the expression then checks if the 'BillablePrj' field in the `vw_Timesheet` table equals 1. 
   - If 'BillablePrj' is 1, it categorizes the entry as "Billable".
   - If 'BillablePrj' is not 1, it categorizes it as "Other Unbillable".

### Summary:
In summary, this DAX expression categorizes each entry in the `vw_Timesheet` table into one of three categories:
- It assigns the 'Group' from the `lkp_Project` table if a match is found.
- If no match is found, it checks if the project is billable and assigns "Billable" or "Other Unbillable" accordingly. 

This helps in organizing and analyzing timesheet data based on project groupings and billing status.

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

1. **Checks Contract End Date**: The expression first checks if the 'ContractEndDate' for a given record (or row) in the 'vw_Timesheet' table is either blank (meaning there is no end date specified) or if the end date is in the future (greater than today's date).

2. **Determines Contract Status**:
   - If either of the above conditions is true (the end date is blank or it's a future date), the expression labels the contract as "Active".
   - If neither condition is true (meaning there is a specific end date that is in the past), it labels the contract as "Not Active".

3. **Outcome**: The result of this expression is a simple status indicator for each contract, helping users quickly understand whether a contract is currently active or not based on its end date.

In summary, this DAX code helps businesses easily identify which contracts are still valid and ongoing, which is crucial for managing resources, planning, and compliance.

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

1. **Check for Blank Values**: The expression starts by checking if the related value from the 'lkp_Unit' table (specifically the 'Unit' column) is blank (i.e., missing or not available).

2. **Return "Unknown" if Blank**: If the 'Unit' value is indeed blank, the expression will return the text "Unknown". This is a way to handle situations where there is no corresponding unit available, ensuring that the result is clear and informative.

3. **Return the Unit if Not Blank**: If the 'Unit' value is not blank, the expression will return the actual value from the 'Unit' column in the 'lkp_Unit' table. This means that if there is a valid unit associated with the data, it will be displayed.

In summary, this DAX expression effectively ensures that for each row in the main table, there is either a valid unit name displayed or "Unknown" if no unit is available. This helps maintain clarity in the data by avoiding blank entries.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) to facilitate time-based reporting and analysis.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to extract the month number from a date value found in the column named `TimesheetDate`. 

In simple terms, this means that for each entry in the `TimesheetDate` column, the expression will return a number that represents the month of that date. For example:

- If the date is January 15, 2023, the expression will return `1`.
- If the date is February 10, 2023, it will return `2`.
- If the date is December 5, 2023, it will return `12`.

This calculated column, named 'MonthNumber', helps in organizing and analyzing data by month, making it easier to perform month-based aggregations or comparisons in reports and dashboards.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the employee identification number as a string, facilitating the association of timesheet entries with specific employees in the 'vw_Timesheet' view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

In simple business terms, this means that the expression counts how many different employees have recorded their time in the timesheet, without counting any employee more than once. For example, if three employees submitted timesheets, but one of them submitted multiple entries, this expression would still return a count of three, reflecting the unique individuals rather than the total number of timesheets submitted. 

This is useful for understanding workforce participation or engagement, as it helps to identify how many distinct employees are involved in the timesheet data being analyzed.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table categorizes the type of time entry as either owned, subcontracted, or external, facilitating the analysis of resource allocation and project costs.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used to retrieve a value from a related table in a data model. Here's a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a calculated column within a table that has a relationship with another table. In this case, the current table is related to the `lkp_Unit` table.

2. **Purpose**: The expression fetches the value from the `OWN-Sub-ExtT` column in the `lkp_Unit` table. This means that for each row in the current table, it looks up the corresponding value in the `lkp_Unit` table based on the established relationship.

3. **Outcome**: The result is that each row in the current table will have a new column that contains the value from the `OWN-Sub-ExtT` column of the `lkp_Unit` table. This is useful for enriching the data in the current table with additional information that is stored in the related table.

In summary, this DAX expression helps to pull in specific information from another table, allowing for more comprehensive data analysis and reporting.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column contains string values that categorize or qualify the projects associated with each timesheet entry, providing context for the recorded hours.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'QualifyPrj'. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Value**: The expression first checks if the 'Qualify' field from a related table called 'lkp_Project' is blank (i.e., it has no value).

2. **Return Value Based on Check**:
   - If the 'Qualify' field is blank, the expression returns a value of **1**. This could indicate a default or placeholder value, suggesting that there is no qualification information available for that project.
   - If the 'Qualify' field is not blank (meaning it has a value), the expression returns the actual value from the 'Qualify' field in the 'lkp_Project' table.

**Overall Purpose**: This calculated column effectively ensures that every entry has a value. If there is no qualification information available for a project, it assigns a default value of 1. If there is qualification information, it uses that value instead. This helps maintain data integrity and ensures that analyses can proceed without missing values.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for submitting the timesheet data, facilitating accurate tracking and reporting of labor hours.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of timesheet entries, represented as a string, for processing or review within the 'vw_Timesheet' table.
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
   - It first checks if the value of the column `[ReportReady]` is `TRUE`. This means it is verifying if a report is ready.
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means it is looking for specific codes that might indicate a certain type of timesheet.

2. **Output Values**: 
   - If either of the conditions is met (i.e., if the report is ready or if the timesheet code is "V" or "Z"), the expression returns a value of `1`. This indicates a positive outcome, meaning that the item is considered "ready" in some context.
   - If neither condition is met, it returns a value of `0`. This indicates a negative outcome, meaning that the item is not considered "ready".

In summary, this DAX expression effectively flags records as "ready" (1) if either the report is ready or if the timesheet code is one of the specified values ("V" or "Z"). If neither condition is true, it flags them as "not ready" (0). This can help in filtering or analyzing data based on readiness criteria.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'ContractStatus2'. Here's what it does in simple business terms:

1. **Condition Check**: The expression first checks the value of another measure called `[ContractStatusMeasure]`. It looks to see if this measure equals "Active".

2. **Return Value Based on Condition**:
   - If the condition is true (meaning the contract status is "Active"), it returns the value of another measure called `[HoursDifference]`. This likely represents the difference in hours related to the contract, such as hours worked or hours remaining.
   - If the condition is false (meaning the contract status is not "Active"), it returns the text "Not Active".

**In summary**, this measure helps to determine the status of a contract. If the contract is active, it provides a numerical value representing hours; if not, it simply indicates that the contract is not active. This can be useful for reporting or analysis purposes, allowing users to quickly understand the status of contracts and their associated hours.

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
   - **ISBLANK**: It checks if this maximum end date is blank (i.e., there is no end date recorded). If there is no end date, it implies that the contract is still ongoing.
   - **Comparison with Today**: If there is an end date, it then checks if this date is greater than today’s date. If the end date is in the future, it means the contract is still active.

3. **Return Status**:
   - If either of the above conditions is true (no end date or the end date is in the future), the measure returns "Active".
   - If neither condition is true (meaning there is an end date that is in the past), it returns "Not Active".

In summary, this measure effectively categorizes contracts as "Active" or "Not Active" based on whether they have an end date that is either missing or still in the future. This helps businesses quickly assess which contracts are currently valid and ongoing.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 15, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated starting from the first week of the year.

So, when you combine these two functions, `WEEKNUM(TODAY())` will give you the week number for the current date. For instance, if today is in the 42nd week of the year, this expression will return the number 42.

In business terms, this measure helps you understand which week of the year it currently is, which can be useful for reporting, planning, or analyzing data on a weekly basis.

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

3. **Filter Condition**: The expression includes a filter condition that specifies `vw_Timesheet[Approved] = FALSE()`. This means that the calculation will only consider timesheets that have not been approved (i.e., where the `Approved` column is marked as FALSE).

In summary, the measure `Dax_EmpCount_Approved` calculates the total number of distinct employees who have submitted timesheets that are still pending approval. This can be useful for tracking how many employees are waiting for their timesheet approvals, helping management understand the workload and efficiency of the approval process.

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

3. **Filter Condition**: The expression includes a filter that specifies `vw_Timesheet[ReportReady] = FALSE()`. This means that only the timesheets that are not marked as "Report Ready" will be considered in the count.

In summary, this measure calculates how many different employees have timesheets that are still pending or not ready for reporting. This can be useful for tracking which employees still need to submit their timesheets or for identifying potential delays in the reporting process.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheets in the `vw_Timesheet` table.

Here's a breakdown of what it achieves:

- **DISTINCTCOUNT**: This function counts only the unique values in a specified column. In this case, it ensures that each employee is only counted once, regardless of how many times they appear in the data.
  
- **vw_Timesheet[EmployeeName]**: This specifies the column from which the unique employee names are being counted. It refers to the names of employees who have submitted timesheets.

In simple terms, this measure provides the total number of different employees who have logged their work hours, helping businesses understand workforce participation and engagement.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_%_Budget_Used`**

- **DAX Expression:**
```dax
DIVIDE(SUM(vw_Timesheet[SalesAmount]), SUM('tbl_Project'[SalesPriceBaseCurrency]), 0)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of the budget that has been used based on sales data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the `vw_Timesheet` table. Essentially, it totals the revenue generated from sales.

2. **SUM('tbl_Project'[SalesPriceBaseCurrency])**: This part sums up the total budget or sales price available for the projects from the `tbl_Project` table. It represents the total amount that was budgeted or expected to be earned.

3. **DIVIDE(..., ..., 0)**: The `DIVIDE` function takes the total sales amount (from the first part) and divides it by the total budget (from the second part). The third argument, `0`, is a safeguard that specifies what to return if the denominator (total budget) is zero. In this case, it will return 0 instead of causing an error.

In simple terms, this measure calculates how much of the budget has been used by comparing actual sales to the budgeted sales. The result is a ratio that shows the proportion of the budget that has been utilized, which can help in assessing financial performance and project management.

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** This DAX expression calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the `vw_Timesheet` table. Essentially, it totals the revenue generated from sales.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the same `vw_Timesheet` table. It totals the time spent on tasks or projects.

3. **Division**: The expression then divides the total sales amount by the total hours worked. 

**What it achieves**: The result of this calculation gives you the average hourly rate of sales. In simple terms, it tells you how much revenue is generated for each hour worked. This metric can help businesses understand their efficiency and profitability in relation to the time invested in sales activities.

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
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called 'Msr_Budget' that calculates a budget value based on the current week in the year. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: The measure starts by determining the current week number of the year using the `SELECTEDVALUE` function. This means it looks at the data context (like filters applied in a report) to find out which week is currently being analyzed.

2. **Calculate Previous Budget**: Next, it calculates the budget value for the most recent week prior to the current week. It does this by:
   - Using the `CALCULATE` function to evaluate the measure `[Msr_SalesPriceBaseCurrency]`, which presumably represents the sales price in the base currency.
   - The `FILTER` function is applied to the entire date table (`ALLSELECTED(DimDate)`) to only consider weeks that are less than the current week. This ensures that it looks back at previous weeks to find the last available budget value.

3. **Return the Appropriate Value**: Finally, the measure checks if the current budget value (`[Msr_SalesPriceBaseCurrency]`) is available (not blank). 
   - If it is available, it returns this current budget value.
   - If it is not available (meaning there is no budget for the current week), it falls back to the previously calculated budget value from the last available week.

In summary, this measure dynamically retrieves the budget for the current week if it exists; if not, it provides the budget from the most recent prior week. This approach ensures that users always have a relevant budget figure to work with, even if the current week's data is missing.

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

1. **Identify the Selected Month**: The code starts by determining which month is currently selected in the visual. It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month that the user has chosen to focus on.

2. **Calculate Cumulative Cost**: The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to change the context in which the data is evaluated. In this case, it sums up the `CostAmount` from the `vw_Timesheet` table.

4. **Filtering the Data**: The `FILTER` function is applied to the entire date table ('DimDate') using `ALLSELECTED`, which means it considers all the months that are currently selected in the visual context. It then filters this data to include only those months where the month number is less than or equal to the selected month. This ensures that only the costs from the selected month and all previous months are included in the total.

5. **Final Output**: The result of this measure is the cumulative cost from the start of the year up to the month that the user has selected. This is useful for understanding how costs accumulate over time and can help in budgeting and financial analysis.

In summary, this DAX measure helps businesses track and analyze their cumulative costs over time, providing insights into spending trends up to a specific point in the year.

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

1. **Identify the Selected Month**: The code starts by determining which month is currently selected in the visual (like a chart or table). It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed.

2. **Calculate Cumulative Sales**: The main goal of this measure is to calculate the total sales amount for all months up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to modify the context in which the sales amount is summed. It allows for more complex calculations by changing the filters applied to the data.

4. **Filtering the Date Context**: Inside the `CALCULATE`, there is a `FILTER` function that adjusts the context to include all months that are less than or equal to the selected month. The `ALLSELECTED` function ensures that any other filters applied to the 'DimDate' table (like year or other date filters) are respected, but it removes any filters specifically on the month.

5. **Summing Sales Amount**: Finally, the `SUM` function adds up the `SalesAmount` from the `vw_Timesheet` table for all the months that meet the filter criteria.

In summary, this DAX measure calculates the total sales from the beginning of the year up to the month that is currently selected in the report. This allows users to see how sales have accumulated over time, providing valuable insights into sales performance.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the percentage of hours worked by a specific group, project, or employee relative to the total hours worked across all selected categories in a timesheet report. Here’s a breakdown of what it does:

1. **TotalValue Calculation**: 
   - The variable `TotalValue` is created to hold the total sum of hours from the `vw_Timesheet` table.
   - The `CALCULATE` function is used to sum the `Hours` column, but it does so while ignoring any filters applied to the `GroupCat`, `Project`, and `EmployeeName` columns. This means it calculates the total hours across all selected data, regardless of the specific group, project, or employee currently being viewed.

2. **Return Statement**:
   - The `RETURN` statement then calculates the percentage of hours for the current context (the specific group, project, or employee being analyzed).
   - It uses the `SUM` function to get the total hours for the current context and divides that by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safe way to handle division in DAX. If the `TotalValue` is zero (which would mean no hours were recorded), it returns 0 instead of causing an error.

In summary, this measure calculates what percentage of the total hours worked (across all selected categories) is represented by the hours worked in the current context (specific group, project, or employee). This helps in understanding how much a particular segment contributes to the overall workload.

**`MSR_PROJECT_%_REALIZATION`**

- **DAX Expression:**
```dax
IF(sum(tbl_Project[SalesPriceBaseCurrency]) <> 0, sum(vw_Timesheet[SalesAmount])/sum(tbl_Project[SalesPriceBaseCurrency]))
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called 'MSR_PROJECT_%_REALIZATION', which essentially assesses the percentage of sales realized from a project compared to its total sales price.

Here's a breakdown of what it does:

1. **Condition Check**: The expression starts with an `IF` statement that checks if the total sales price (from the `tbl_Project` table) is not equal to zero. This is important because if the total sales price is zero, dividing by it would lead to an error.

2. **Calculating Sales Amount**: If the total sales price is not zero, the expression proceeds to calculate the percentage of sales realized. It does this by taking the total sales amount from the `vw_Timesheet` table.

3. **Calculating Total Sales Price**: The total sales price is summed up from the `tbl_Project` table.

4. **Percentage Calculation**: The final step is to divide the total sales amount by the total sales price. This gives the percentage of the sales that have been realized compared to what was expected or planned.

In summary, this DAX measure calculates the percentage of actual sales achieved for a project relative to its total sales price, but only if the total sales price is greater than zero. If the total sales price is zero, it avoids any calculation to prevent errors. This measure helps in understanding how well a project is performing in terms of sales realization.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the profit margin for a project by subtracting the total costs from the total sales revenue. Here’s a breakdown of what it does:

1. **Total Sales Amount**: The expression first sums up all the values in the `SalesAmount` column from the `vw_Timesheet` table. This represents the total revenue generated from sales related to the project.

2. **Total Cost Amount**: Next, it sums up all the values in the `CostAmount` column from the same table. This represents the total costs incurred for the project.

3. **Calculating Project Margin**: Finally, the expression subtracts the total costs from the total sales. The result is the project margin, which indicates how much profit the project has made after covering its costs.

In simple terms, this measure helps a business understand how profitable a project is by showing the difference between what they earned (sales) and what they spent (costs). A positive result indicates a profit, while a negative result indicates a loss.

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

1. **SUMMARIZE Function**: The expression starts by creating a summary table from the `vw_Timesheet` data. This summary groups the data by two key dimensions: `ProjectProfile` and `Client`. Essentially, it organizes the timesheet entries based on which project and client they are associated with.

2. **SELECTEDVALUE Function**: For each unique combination of `ProjectProfile` and `Client`, it retrieves the corresponding sales price in the base currency from the `tbl_Project` table. The `SELECTEDVALUE` function is used here to get the sales price for the current context of the project being evaluated.

3. **MeasureValue**: The result of the `SELECTEDVALUE` function is stored in a new column called "MeasureValue" within the summarized table. This column represents the sales price for each project-client combination.

4. **SUMX Function**: Finally, the `SUMX` function iterates over the summarized table and adds up all the values in the "MeasureValue" column. This gives the total sales price in the base currency across all the projects and clients included in the timesheet.

In summary, this DAX measure calculates the total sales price in the base currency for all projects and clients listed in the timesheet, providing a clear view of revenue generated from those projects.

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

1. **CALCULATE**: This function modifies the context in which data is evaluated. It allows us to change the filters applied to the data.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it gives us the total number of distinct employees.

3. **ALL(vw_Timesheet)**: This function removes any filters that might be applied to the `vw_Timesheet` table. By using this, we ensure that the count of employees is not affected by any other filters in the report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This condition filters the data to only include employees whose contract status is "Valid". 

Putting it all together, this DAX expression calculates the total number of unique employees who currently have a valid contract, ignoring any other filters that might be in place. This is useful for getting a clear view of the workforce that is actively employed under valid contracts, without being influenced by other data selections.

**`TotalActualHours`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression `SUM(vw_Timesheet[Hours])` is used to calculate the total number of hours recorded in a timesheet. 

Here's a breakdown of what it does:

- **SUM**: This function adds up all the values in a specified column.
- **vw_Timesheet**: This refers to a table (or view) that contains timesheet data, which likely includes information about employee hours worked.
- **[Hours]**: This is the specific column within the `vw_Timesheet` table that contains the number of hours each employee has logged.

In simple terms, this measure totals all the hours worked by employees as recorded in the timesheet, providing a single number that represents the overall hours worked during a specific period or across the entire dataset. This is useful for tracking labor costs, productivity, and resource allocation within a business.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the total contracted hours for all employees based on their maximum hours per week. Here's a breakdown of what it does in simple business terms:

1. **VALUES(vw_Timesheet[EmployeeName])**: This part of the expression creates a unique list of employee names from the 'vw_Timesheet' table. Essentially, it identifies each employee who has recorded hours.

2. **MAX(vw_Timesheet[HoursPerWeek])**: For each employee in the unique list, this function finds the maximum number of hours that employee is contracted to work in a week. If an employee has multiple entries, it will take the highest value.

3. **SUMX(...)**: This function iterates over the unique list of employees and sums up the maximum hours per week for each employee. 

In summary, the entire expression calculates the total maximum contracted hours for all employees by summing up the highest weekly hours each employee is set to work. This gives a clear picture of the total contracted hours across the workforce, which can be useful for planning and resource allocation.

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

- If the result is positive, it indicates that more hours were worked than what was contracted, suggesting potential overwork or additional effort beyond what was planned.
- If the result is negative, it means that fewer hours were worked than contracted, which could indicate efficiency or underutilization of resources.

Overall, this measure helps in assessing project performance and resource management against contractual obligations.

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

3. **Return the Sum if Not Blank**: If there are hours recorded (the sum is not blank), the expression simply returns the total sum of hours tracked.

In summary, this measure ensures that when calculating total hours, if there are no hours logged, it will return 0 instead of a blank value. If there are hours logged, it will return the actual total of those hours. This makes it easier for users to understand the data without encountering blanks in reports or dashboards.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total hours tracked in a timesheet, but it also includes a check to see if there are any hours recorded. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the `Hours` column of the `vw_Timesheet` table. Essentially, it calculates the total hours worked.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (i.e., if there are no hours recorded). If there are no hours, it means that the timesheet is empty.

3. **IF(...)**: This function evaluates the condition provided by ISBLANK. If the total hours are blank (meaning no hours have been tracked), it returns the text "Empty". If there are hours recorded, it returns the total number of hours calculated.

In summary, this DAX expression helps to determine whether any hours have been tracked in the timesheet. If no hours are recorded, it clearly indicates that the timesheet is empty by returning "Empty". If hours are present, it simply shows the total number of hours tracked. This is useful for reporting and ensuring that timesheet data is being captured effectively.

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

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked by an employee over a specific period.

2. **Maximum Hours Per Week**: The expression then calculates the maximum number of hours that an employee has logged in a single week. This is done using the `MAXX` function, which looks at the distinct values of `HoursPerWeek` from the `vw_Timesheet` table. Essentially, it finds the highest number of hours that the employee has recorded in any week.

3. **Context Filtering**: The `CALCULATE` function is used to change the context of the calculation. The `ALLEXCEPT` function ensures that the calculation for the maximum hours per week is done only for the specific employee (identified by `EmployeeID`). This means that it ignores any other filters that might be applied to the data, focusing solely on the hours logged by that particular employee.

4. **Final Calculation**: Finally, the expression subtracts the maximum hours per week from the total hours tracked. This gives you the difference between the total hours an employee has tracked and their highest weekly logged hours.

### Summary:
In summary, this DAX measure calculates how many more hours an employee has tracked in total compared to their maximum logged hours in a single week. This can help identify if an employee is consistently working more hours than their typical weekly maximum or if there are discrepancies in their time tracking.

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
- **DAX Explanation (Generated):** The DAX expression you've provided calculates a measure called `trackedDiff2`, which essentially finds the difference between the total hours tracked and the maximum hours recorded per week for each employee, while ignoring any specific filters on the hours per week.

Here's a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked for a specific context (like a project, employee, or time period).

2. **Maximum Hours Per Week**: The next part of the expression calculates the maximum number of hours that have been recorded in a week for each employee. This is done using the `MAXX` function, which looks at the distinct values of `HoursPerWeek` from the `vw_Timesheet` table.

3. **Removing Filters**: The `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])` part ensures that any filters applied to the `HoursPerWeek` column are ignored. This means that the calculation of maximum hours per week is done across all weeks, not just the ones currently filtered in the report.

4. **Keeping Employee Context**: The `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` function keeps the context of the specific employee while removing other filters. This means that the maximum hours per week is calculated for each employee individually, regardless of any other filters that might be applied to the data.

5. **Final Calculation**: Finally, the measure subtracts the maximum hours per week (calculated in the previous steps) from the total hours tracked. This gives you the difference between what has been tracked and the highest number of hours that have been recorded in a week for that employee.

In summary, `trackedDiff2` measures how much the total hours tracked for an employee exceed the maximum hours they have logged in any single week. This can help identify if an employee is consistently working more hours than their typical maximum, which could be useful for workload management or identifying potential burnout.

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

1. **Variable Definition (FilteredHours)**: The code starts by defining a variable called `FilteredHours`. This variable is created by filtering the `vw_Timesheet` table based on specific conditions.

2. **Filtering Criteria**:
   - The first condition checks if the project qualifies as billable by ensuring that both `QualifyPrj` and `BillablePrj` columns are equal to 1. This means that only projects that are marked as both qualifying and billable will be included.
   - The second condition uses the `SEARCH` function to look for the phrase "Customer Success Services" within the `Project` column. If this phrase is found (indicated by a result greater than 0), those entries will also be included in the filtered results.

3. **Summing Up Hours**: After applying the filter, the `SUMX` function is used to iterate over the filtered rows in `FilteredHours` and sum up the `Hours` column. This means it adds together all the hours that meet the filtering criteria.

4. **Return Value**: Finally, the expression returns the total sum of hours that are either from qualifying and billable projects or from projects related to "Customer Success Services."

In summary, this DAX measure calculates the total number of billable hours from a timesheet, focusing on projects that are either explicitly marked as billable or are related to customer success services. This helps businesses track and report on the hours that can be billed to clients, ensuring accurate financial reporting and resource allocation.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'UTI_TOTALHOURS'. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression calculates the total number of hours recorded in the 'Hours' column of the 'vw_Timesheet' table. Essentially, it adds up all the hours worked.

2. **CALCULATE(...)**: The CALCULATE function modifies the context in which the data is evaluated. In this case, it is used to apply a specific filter to the data being summed.

3. **vw_Timesheet[QualifyPrj] = 1**: This condition acts as a filter. It specifies that only the rows in the 'vw_Timesheet' table where the 'QualifyPrj' column equals 1 should be included in the total hours calculation. This means that the measure is only considering hours worked on projects that qualify (as indicated by the value 1).

In summary, the 'UTI_TOTALHOURS' measure calculates the total hours worked on projects that meet a specific qualification criterion (where 'QualifyPrj' equals 1). This helps businesses understand how many hours are being spent on eligible projects, which can be important for budgeting, resource allocation, and performance tracking.

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
   - The code checks if there are any active contacts (employees) using the measure `[Msr_ActiveContacts]`. If there are no active contacts, the calculation will not proceed.
   - Next, it checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the calculation will not proceed further.
   - If both conditions are met, it calculates the ratio of billable hours to total hours by dividing `_TotalBillableHours` by `_TotalHours`.
   - If the result of this division is blank (which could happen if there are no hours worked), it returns 0 instead of a blank value.

3. **Outcome**:
   - The final result of this measure is the Billable Hour Ratio, which indicates the proportion of hours that were billable compared to the total hours worked by active employees. This ratio is crucial for understanding employee productivity and efficiency in generating revenue.

In summary, this DAX expression effectively calculates how much of the total working time of active employees is spent on billable tasks, providing valuable insights into workforce utilization.

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
- Fields Used: _(None detected)_

**TimeSpan**

- Type: `slicer`
- Name: `a60364499b7915da7e60`
- Fields Used: _(None detected)_

**Unit**

- Type: `slicer`
- Name: `be723b7e75b4661912d0`
- Fields Used: _(None detected)_

**SubUnit**

- Type: `slicer`
- Name: `e4e04919d0548163c0a8`
- Fields Used: _(None detected)_

**Client**

- Type: `slicer`
- Name: `8048e854158dc07422ce`
- Fields Used: _(None detected)_

**Sub-Own-Ext**

- Type: `slicer`
- Name: `800e477135b21a02300b`
- Fields Used: _(None detected)_

**actionButton**

- Type: `actionButton`
- Name: `88e87e1eddb866e15c8a`
- Fields Used: _(None detected)_

**Project Type**

- Type: `slicer`
- Name: `b37c531703b3e683879e`
- Fields Used: _(None detected)_

**Company**

- Type: `slicer`
- Name: `6e846506dec1641cb274`
- Fields Used: _(None detected)_

**Project Name**

- Type: `slicer`
- Name: `9285b49f5742861eada8`
- Fields Used: _(None detected)_

**Employee**

- Type: `slicer`
- Name: `b5b5047cc00ba23030e5`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `8a02ec339e5cc2bce97e`
- Fields Used: _(None detected)_

**Revenue/Week**

- Type: `lineChart`
- Name: `f5560d7670207c1c0572`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `4856952e0903150da5c7`
- Fields Used: _(None detected)_


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
- Fields Used: _(None detected)_

**TimeSpan**

- Type: `slicer`
- Name: `30ac69436bb17c560e22`
- Fields Used: _(None detected)_

**Pillar**

- Type: `slicer`
- Name: `1218b3e663e35022d9d5`
- Fields Used: _(None detected)_

**Client**

- Type: `slicer`
- Name: `91d269b51621b58cd5b8`
- Fields Used: _(None detected)_

**Company**

- Type: `slicer`
- Name: `bb8c1f8dd224d829d32b`
- Fields Used: _(None detected)_

**Status**

- Type: `slicer`
- Name: `331fc4a3b6d31c038b30`
- Fields Used: _(None detected)_

**Sub-Own-Ext**

- Type: `slicer`
- Name: `f4a2a804d812cd7dee49`
- Fields Used: _(None detected)_

**Project Type**

- Type: `slicer`
- Name: `c275d85b5800b1d47638`
- Fields Used: _(None detected)_

**Project Name**

- Type: `slicer`
- Name: `1221310b511e5b428280`
- Fields Used: _(None detected)_

**Project Leader**

- Type: `slicer`
- Name: `9402ca390dab8c29e0ca`
- Fields Used: _(None detected)_

**Employee**

- Type: `slicer`
- Name: `7e3b4de92424e0cc2bde`
- Fields Used: _(None detected)_

**actionButton**

- Type: `actionButton`
- Name: `f3f386d06e37473759a7`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `0891520feb1b0937cd30`
- Fields Used: _(None detected)_

**Actual vs Budget**

- Type: `lineChart`
- Name: `76b4488a85c8253bac24`
- Fields Used: _(None detected)_


---

