# Power BI Model & Report Documentation

*Generated on: 2025-04-29 00:28:28*

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

The Power BI data model appears to be designed for a project management and timesheet tracking system, likely within a corporate or organizational context. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a focus on capturing and analyzing employee work hours and project-related activities, while the various lookup tables (e.g., 'lkp_Project', 'lkp_Unit', 'lkp_Code') provide essential metadata that enriches the analysis by categorizing and contextualizing the data. The 'DimDate' table indicates that time-based analysis is a key component, allowing users to filter and aggregate data over different time periods.

The relationships established between these tables facilitate a comprehensive view of project performance, resource allocation, and timesheet compliance. For instance, linking timesheet entries to specific projects and units enables stakeholders to assess project progress and employee productivity effectively. Additionally, the inclusion of a view for missing timesheets highlights an emphasis on accountability and data completeness, which is crucial for accurate reporting and decision-making. Overall, this data model serves as a robust framework for monitoring project execution and optimizing workforce management within the organization.

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

The 'DimDate' table serves as a comprehensive date dimension, providing essential temporal attributes that enable businesses to analyze trends and performance over time. By offering detailed breakdowns of dates into various formats, such as day, week, month, and quarter, this table supports effective reporting and enhances time-based data analysis across multiple business metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) associated with each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient date-based queries and data enrichment processes. |
| `DayName` | `string` | The 'DayName' column contains the full name of the day (e.g., Monday, Tuesday) corresponding to each date in the 'DimDate' table, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | The 'DayNumberOfMonth' column (int64) in the 'DimDate' table represents the numerical day of the month, ranging from 1 to 31, providing a granular reference for date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical value of the day of the week, where 1 corresponds to Sunday and 7 corresponds to Saturday, facilitating date-related analyses and reporting. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column represents the sequential day of the year (ranging from 1 to 365 or 366) for each date in the 'DimDate' table, facilitating date-related calculations and analyses. |
| `MonthName` | `string` | The 'MonthName' column contains the full names of the months, providing a human-readable representation of the month associated with each date in the 'DimDate' table. |
| `MonthNumberOfYear` | `int64` | The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month, ranging from 1 for January to 12 for December, facilitating time-based analysis and reporting. |
| `ShortDate` | `dateTime` | The 'ShortDate' column in the 'DimDate' table represents a simplified date format, capturing the specific date without time details for streamlined data analysis and reporting. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number within the calendar year, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Indicates whether the date corresponds to the current calendar year, with values typically set to 'Yes' or 'No'.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CurrentYearFlag' in a data model, specifically in a date dimension table (DimDate). 

Here's what it does in simple terms:

1. **YEAR(DimDate[ShortDate])**: This part extracts the year from the date in the 'ShortDate' column of the DimDate table. For example, if the date is '2023-05-15', it will return '2023'.

2. **YEAR(TODAY())**: This part gets the current year based on today's date. If today is any date in 2023, this will return '2023'.

3. **IF(..., 1, 0)**: The IF function checks if the year extracted from the 'ShortDate' is less than or equal to the current year. If it is, the expression returns '1'; if it is not, it returns '0'.

### What it achieves:
- The 'CurrentYearFlag' column will have a value of '1' for all dates that are in the current year or any previous year. 
- It will have a value of '0' for any future dates (dates that are in the next year or beyond).

In summary, this calculated column helps identify whether a date is in the current year or earlier, marking it with a '1', while future dates are marked with a '0'. This can be useful for filtering or analyzing data based on whether it pertains to the current or past years.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table likely serves to track and analyze the distribution of work hours and their corresponding percentages across various projects or tasks, enabling businesses to assess resource allocation and productivity effectively. This table can help management make informed decisions regarding workforce optimization and project planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column contains string representations of hours worked alongside their corresponding percentage of total hours, providing insights into employee time allocation and productivity metrics. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and their corresponding percentage contributions, facilitating data enrichment and analysis within the 'Hours_nd_Percentage' table. |
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

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics. This table enhances data integrity and usability by standardizing employee information for analysis and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column (string) uniquely identifies each employee in the 'lkp_fltr_Employee' table, facilitating data enrichment and integration processes. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, serving as a key identifier for enriching employee-related data within the lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, capturing essential attributes such as project names, qualification scores, billing amounts, and associated groups. This table is crucial for analyzing project performance and financial metrics, enabling informed decision-making and strategic planning within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing amount associated with each project, facilitating financial tracking and reporting. |
| `Group` | `string` | The 'Group' column categorizes projects into distinct classifications, facilitating organized data management and retrieval within the lkp_Project table. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the lkp_Project table. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, with numerical values representing different qualification levels or criteria met. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their characteristics such as billable status and external affiliations. This table is essential for analyzing resource allocation and financial performance across different units, facilitating informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column represents the identifier for departments that are eligible for billing, facilitating the tracking of billable units within the organization. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column contains the names of organizational units, serving to categorize and enrich data related to the structure of the organization. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores external type identifiers for owned sub-units, facilitating data enrichment and classification processes. |
| `Unit` | `string` | The 'Unit' column contains string values representing the various measurement units used for data enrichment purposes. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, status, leadership, and financial aspects. This table enables businesses to effectively manage and analyze their projects, facilitating better decision-making and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and performance analysis within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column contains string values that represent the administrative details or oversight responsibilities associated with each project listed in the 'tbl_Project' table. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates within the 'tbl_Project' table. |
| `ClientName` | `string` | The 'ClientName' column stores the name of the client associated with each project, facilitating identification and management of client-specific projects within the 'tbl_Project' table. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating the tracking and management of client-related data within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated in the tbl_Project table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project, represented as a string. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the exact date and time when each project entry was created in the 'tbl_Project' table, facilitating tracking and auditing of project timelines. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or business unit within Devoteam associated with each project, facilitating targeted analysis and reporting. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column indicates the scheduled date and time for the completion of the project, facilitating project timeline management and tracking. |
| `Project` | `string` | The 'Project' column contains the names or identifiers of specific projects associated with the data entries in the 'tbl_Project' table, facilitating the organization and retrieval of project-related information. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed textual overview of each project, outlining its objectives, scope, and key deliverables within the 'tbl_Project' table. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the specific category or classification of a project within the 'tbl_Project' table, facilitating organization and management of related projects. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a textual summary of the project group, detailing its objectives and scope within the 'tbl_Project' table. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary key for data integrity and relational mapping. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the associated project in the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency sales price of a project, stored as a decimal value to facilitate accurate financial calculations and reporting. |
| `Startdate` | `dateTime` | The 'Startdate' column records the date and time when a project is initiated, serving as a key reference point for project timelines and scheduling within the 'tbl_Project' table. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, allowing for effective tracking and management of project timelines and deliverables. |
| `Statuscode` | `string` | The 'Statuscode' column indicates the current operational status of each project, represented as a string, to facilitate tracking and management of project progress within the 'tbl_Project' table. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, stored as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table is designed to identify and track employees who have not submitted their timesheets for specific weeks within a given year, providing essential insights for managers to ensure compliance with time reporting and optimize workforce management. By linking employee and manager details with contract information, this table facilitates timely follow-ups and enhances accountability in labor tracking.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column represents the total number of hours worked by an employee, recorded as a double data type, within the context of identifying and analyzing missing timesheet entries. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to terminate, providing critical information for tracking contract durations within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to assist in identifying discrepancies in timesheet submissions within the 'vw_missing_timesheet' table. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract for each employee, providing essential context for identifying missing timesheet entries. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet submissions. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees who have missing timesheets, facilitating the identification and follow-up of individuals needing to complete their time reporting. |
| `Hours_` | `double` | The 'Hours_' column represents the total number of hours recorded for each employee's timesheet entry, stored as a double to accommodate fractional hours. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_missing_timesheet' table identifies the unique identifier of the manager responsible for overseeing the employee associated with the missing timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column contains the name of the employee's manager, providing context for accountability and oversight in relation to missing timesheet submissions. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee work hour reporting. |
| `Project` | `string` | The 'Project' column contains the names of projects associated with missing timesheet entries, enabling identification and tracking of unrecorded work hours. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project associated with the timesheet entries, facilitating the tracking and management of time allocation across various projects. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected (true) or not (false) in the context of tracking missing timesheet submissions. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total monetary value of sales transactions associated with the records in the 'vw_missing_timesheet' table, stored as a double for precision in financial calculations. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double data type, to facilitate accurate financial analysis and reporting within the 'vw_missing_timesheet' table. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with missing timesheet entries, formatted to facilitate quick reference and analysis. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific sub-division or department within the organization associated with each entry in the 'vw_missing_timesheet' table, facilitating targeted analysis of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains unique identifiers for timesheets, facilitating the tracking and management of employee time entries within the 'vw_missing_timesheet' view. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the timesheet entries, providing context and details about the hours worked for each employee. |
| `Week_` | `int64` | The 'Week_' column (int64) represents the numerical week of the year associated with each entry in the 'vw_missing_timesheet' table, facilitating the identification and analysis of timesheet data by week. |
| `Year_` | `int64` | The 'Year_' column (int64) in the 'vw_missing_timesheet' table represents the calendar year associated with each timesheet entry, facilitating the analysis of timesheet data over time. |

##### Calculated Columns

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column contains the names of departments associated with billable hours, providing insight into which departments are generating revenue through recorded timesheets.
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

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of specific information, the default assumption is that the item is billable.

3. **Check for Non-Zero Values**: If there is a related 'BillableDep' value and it is not blank, the expression then checks if this value is not equal to zero. If the value is indeed not zero, it again returns **1**, indicating that the item is billable.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that the item is not billable.

In summary, this DAX expression effectively categorizes items as billable or not based on the related 'BillableDep' value. If there is no related data, or if the related value is non-zero, it marks the item as billable (1). If the related value is zero, it marks it as not billable (0).

**`CC_ActiveEmployees`** (`string`)

- **Description:** The 'CC_ActiveEmployees' column contains a string representation of the active employees in the cost center, used to identify personnel who may be missing timesheet entries in the 'vw_missing_timesheet' view.
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
   - **Contract Start Date**: It first checks if the 'ShortDate' (the date being evaluated) is on or after the employee's 'ContractStartDate'. This means the employee's contract must have started before or on this date for them to be considered active.
   - **Contract End Date**: Next, it checks if the 'ShortDate' is on or before the 'ContractEndDate'. If the 'ContractEndDate' is blank (meaning the employee's contract does not have an end date), the employee is still considered active regardless of the 'ShortDate'.

3. **Output**:
   - If both conditions are met (the 'ShortDate' falls within the contract period or the contract has no end date), the expression returns a value of **1**, indicating that the employee is active.
   - If either condition is not met, it returns a value of **0**, indicating that the employee is not active.

In summary, this DAX expression effectively flags employees as active (1) or inactive (0) based on whether the date being evaluated falls within their contract period or if they have an ongoing contract without an end date. This helps in analyzing employee status in relation to their contracts.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to assist in identifying and addressing missing data in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IncompleteFlag' in a data model. Here's what it does in simple business terms:

- The expression checks the value in the 'MissingHours' column of the 'vw_missing_timesheet' table.
- If the value of 'MissingHours' is less than 0, it means that there is an issue (perhaps indicating that there are negative hours recorded, which doesn't make sense in this context).
- In this case, the expression will return a value of 1, which can be interpreted as a flag indicating that there is an incomplete or problematic entry.
- If the value of 'MissingHours' is 0 or greater, the expression will return 0, indicating that there is no issue with that entry.

In summary, this calculated column flags entries with negative missing hours as problematic (1) and those with zero or positive hours as normal (0). This helps in identifying and addressing incomplete or incorrect data in the timesheet records.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column contains the primary unit of measurement or organizational unit associated with the timesheet entries, represented as a string.
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

2. **Handle Missing Data**: If the related 'Unit' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression will return the text "Unknown". This is a way to handle situations where the expected data is missing, ensuring that the output is clear rather than leaving it blank.

3. **Return the Unit Name**: If there is a valid related 'Unit' value, the expression will return that value. This means that for rows where the data is available, it will show the actual unit name from the 'lkp_Unit' table.

In summary, this DAX expression effectively ensures that every row in the 'MAIN_UNIT' column will either display the corresponding unit name from the related table or indicate "Unknown" if no unit is found. This helps maintain clarity in reporting and analysis by avoiding blank entries.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column represents the year and week number in a string format, facilitating the identification of specific time periods for tracking missing timesheet entries.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX expression creates a new calculated column called 'YearWeek' that combines the year and the week number of a date from the 'ContractEndDate' field.

Here's a breakdown of what it does:

1. **YEAR([ContractEndDate])**: This part extracts the year from the 'ContractEndDate'. For example, if the date is December 15, 2023, this will return "2023".

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the 'ContractEndDate'. For instance, if the date is December 15, 2023, it might return "50" because December 15 falls in the 50th week of the year.

3. **FORMAT(..., "00")**: This ensures that the week number is always displayed as two digits. So, if the week number is 5, it will be formatted as "05".

4. **Concatenation (&)**: The expression combines the year and the formatted week number into a single string, separated by a hyphen. For example, if the year is 2023 and the week number is 50, the final result will be "2023-50".

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have negative hours recorded in a timesheet. Here's a breakdown of what it does:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it counts the unique names of employees from the 'EmployeeName' column in the 'vw_missing_timesheet' table.

3. **Filter Condition**: The expression includes a filter that only considers records where the 'MissingHours' column has a value less than 0. This means it focuses on cases where employees have negative hours, which could indicate missing or incorrectly reported time.

In summary, this measure calculates how many different employees have reported negative hours in their timesheets. This could be useful for identifying issues with time reporting or understanding employee workload discrepancies.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have recorded negative missing hours in a timesheet. 

Here's a breakdown of what it does:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT**: This part counts the number of unique Employee IDs from the 'vw_missing_timesheet' table. Essentially, it ensures that each employee is only counted once, even if they have multiple entries.

3. **Filter Condition**: The expression includes a filter that only considers records where the 'MissingHours' value is less than 0. This means it focuses solely on instances where employees have reported negative hours, which could indicate an error or an adjustment in their timesheet.

In summary, this measure counts how many different employees have negative missing hours recorded in their timesheets, helping the business identify potential issues with time reporting or attendance.

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

1. **Data Source**: The expression starts by looking at a data table called `vw_missing_timesheet`, which likely contains records of employees, their hours worked, and their contract hours.

2. **Summarization**: It summarizes the data by grouping it based on several factors:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a more specific division within the unit)

3. **Calculating Missing Hours**: For each group, it calculates:
   - **MissingHours**: This is determined by taking the total hours worked (from `vw_missing_timesheet[Hours_]`) and subtracting the maximum contract hours (from `vw_missing_timesheet[ContractHours]`). If this value is negative, it indicates that the employee has not logged enough hours compared to what they are contracted for.

4. **Identifying Active Employees**: It also calculates:
   - **MinActiveEmp**: This checks if the employee is active by finding the minimum value of `vw_missing_timesheet[CC_ActiveEmployees]`. If this value equals 1, it means the employee is currently active.

5. **Filtering**: The expression then filters the summarized data to include only those records where:
   - The employee is active (`MinActiveEmp` = 1)
   - The calculated missing hours are less than zero (indicating they have not logged enough hours).

6. **Counting Active Employees**: Finally, it counts the number of rows that meet these criteria, which gives the total number of active employees who have missing timesheet hours.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of missing hours for active employees who have not met their contracted hours in a given week. Here's a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. It filters a summarized view of the data from a table called `vw_missing_timesheet`. This summary includes key details such as the employee's name, the year, the week, the unit they belong to, and their subunit.

2. **Calculate Missing Hours**: For each employee in the summary, it calculates the "MissingHours" by taking the total hours they worked (from `vw_missing_timesheet[Hours_]`) and subtracting the maximum hours they were contracted to work (from `vw_missing_timesheet[ContractHours]`). If this calculation results in a negative number, it indicates that the employee has not worked enough hours compared to their contract.

3. **Filter for Active Employees**: The summary is then filtered to include only those employees who are marked as active (where `MinActiveEmp` equals 1) and who have negative missing hours (meaning they have not fulfilled their contracted hours).

4. **Sum Missing Hours**: Finally, the expression sums up the "MissingHours" for all the filtered active employees. This gives a total count of hours that active employees are missing based on their contractual obligations.

In summary, this DAX measure helps the business identify how many hours active employees are falling short of their contracted hours, allowing for better management of workforce productivity and planning.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total expected contract hours for employees on a weekly basis. Here’s a breakdown of what it does in simple terms:

1. **Data Source**: It uses a table called `vw_missing_timesheet`, which likely contains records of timesheets for employees, including their contract hours and the week associated with each entry.

2. **Grouping Data**: The `SUMMARIZE` function groups the data by two key pieces of information:
   - The week (`vw_missing_timesheet[Week_]`)
   - The employee ID (`vw_missing_timesheet[EmployeeID]`)

   For each unique combination of week and employee, it calculates the maximum contract hours (`MAX(vw_missing_timesheet[ContractHours])`). This means if an employee has multiple entries for the same week, it will only take the highest contract hours recorded for that week.

3. **Calculating Total**: The outer `SUMX` function then takes the summarized data (which now has unique week-employee pairs with their maximum contract hours) and sums up the "ContractHours" for all these pairs. 

In summary, this measure provides the total expected contract hours for all employees across different weeks, ensuring that if there are multiple records for an employee in a week, only the highest contract hours are considered. This helps in understanding the overall expected workload for employees on a weekly basis.

**`MISSING%`**

- **DAX Expression:**
```dax
DIVIDE([Dax_EmpCount]-[Dax_EmpCount_MissingTS],[Dax_EmpCount]) * 100
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of employees that are not missing time stamps (TS) compared to the total number of employees. Here’s a breakdown of what each part does:

1. **[Dax_EmpCount]**: This represents the total number of employees in the dataset.

2. **[Dax_EmpCount_MissingTS]**: This represents the number of employees who are missing time stamps.

3. **[Dax_EmpCount] - [Dax_EmpCount_MissingTS]**: This part calculates the number of employees who have valid time stamps by subtracting the number of employees missing time stamps from the total employee count.

4. **DIVIDE(..., [Dax_EmpCount])**: The DIVIDE function is used to safely perform division. It takes the number of employees with valid time stamps (from the previous step) and divides it by the total number of employees. This gives us a decimal value representing the proportion of employees with valid time stamps.

5. **... * 100**: Finally, multiplying by 100 converts the decimal proportion into a percentage.

In summary, this measure calculates the percentage of employees who have valid time stamps, helping the business understand how many employees are accounted for in terms of time tracking. A higher percentage indicates better compliance with time tracking requirements.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked and contracted hours for employees or contractors. Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum number of contracted hours from the `vw_missing_timesheet` table. Contracted hours refer to the total hours that an employee or contractor is expected to work as per their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum number of hours actually recorded or worked by the employee or contractor from the same table.

3. **Subtraction**: The expression then subtracts the maximum recorded hours (actual hours worked) from the maximum contracted hours. 

**What it achieves**: The result of this calculation gives you the maximum number of hours that are missing or unaccounted for, meaning it shows how many hours an employee or contractor has not worked compared to what they were supposed to work according to their contract. 

In summary, this measure helps identify any discrepancies between expected and actual work hours, which can be useful for managing workforce productivity and ensuring compliance with contractual obligations.

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

1. **SUM(vw_missing_timesheet[MissingHours])**: This part adds up all the hours that are missing from timesheets. Essentially, it tells us how many hours employees did not report.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that employees are expected to work in a week according to their contracts.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, we subtract the expected hours from the total missing hours. This calculation shows how many hours are missing beyond what is expected. If the result is positive, it indicates that employees are not meeting their expected hours.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: Finally, this part divides the result from the previous step by the expected contract hours. This gives us a ratio or percentage of how much of the expected hours are missing. The third argument (0) ensures that if the expected hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

In summary, this measure calculates the percentage of expected working hours that are not being reported in timesheets. A higher percentage indicates a greater issue with timesheet compliance, while a lower percentage suggests better adherence to expected reporting.

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

1. **Data Source**: The expression uses a table called `vw_missing_timesheet`, which presumably contains records of employees and their timesheet submissions, including whether their submissions are complete or incomplete.

2. **Grouping Data**: The `SUMMARIZE` function is used to create a summary table. This table groups the data by three key columns:
   - `Week_`: The week of the year.
   - `EmployeeName`: The name of the employee.
   - `MAIN_UNIT`: The main unit or department the employee belongs to.

   Additionally, it creates a new column called "IncompleteFlag" that captures the maximum value of the `IncompleteFlag` for each group. This flag indicates whether the employee has any incomplete timesheet submissions during that week.

3. **Calculating Total**: The `SUMX` function then iterates over this summarized table. For each group (each unique combination of week, employee, and unit), it sums up the values in the "IncompleteFlag" column. Since the "IncompleteFlag" is likely a binary value (0 for complete, 1 for incomplete), this effectively counts how many unique employees have incomplete timesheets.

In summary, this DAX measure calculates the total number of unique employees with missing timesheets, organized by week and unit. This information can help management identify areas where timesheet compliance may be lacking and take appropriate actions to improve it.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet entries, capturing essential details such as financial year, dates, and associated project and managerial information. This data is crucial for tracking labor costs, project resource allocation, and overall workforce productivity within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column identifies the name of the client associated with each timesheet entry, facilitating the tracking of billable hours and project allocation. |
| `Code2` | `string` | The 'Code2' column contains a string value that represents a secondary code associated with the timesheet entry, used for categorization or classification purposes. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to terminate, providing essential information for timesheet management and workforce planning. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and payroll calculations within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract associated with the timesheet entry, providing a snapshot of its active, inactive, or pending state. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, represented as a string, to facilitate real-time reporting and analysis within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing a timestamp for tracking contract changes within the timesheet data. |
| `CostAmount` | `double` | The 'CostAmount' column represents the monetary value associated with the hours worked, expressed as a double, within the timesheet data for accurate financial reporting and analysis. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor or resources utilized in the timesheet entries, stored as a double for precision in financial calculations. |
| `Currency` | `string` | The 'Currency' column specifies the type of currency used for financial entries in the timesheet, represented as a string to accommodate various currency codes. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the individual or entity responsible for payment, facilitating the tracking of financial obligations associated with timesheet entries. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee associated with the timesheet entries in the 'vw_Timesheet' table, facilitating accurate tracking and reporting of work hours. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column contains the concatenated identifier and name of employees, facilitating easy identification and tracking of timesheet entries within the 'vw_Timesheet' view. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees associated with each timesheet entry, facilitating identification and reporting of labor hours. |
| `Employer` | `string` | The 'Employer' column contains the name of the organization or company that employs the individual associated with the timesheet entries. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating the tracking and management of employee work hours by employer. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to the timesheet entries, providing further insights into the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with the recorded timesheet entries, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specific time period, allowing for precise tracking of labor input for payroll and project management purposes. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column represents the total number of hours worked by an employee in a given week, recorded as an integer value within the timesheet data. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee work hours. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, facilitating accurate reporting and analysis of employee work patterns. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries, facilitating accountability and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the names of individuals associated with specific IPM IDs, facilitating the identification of personnel linked to timesheet entries in the 'vw_Timesheet' table. |
| `JobTitle` | `string` | The 'JobTitle' column contains the titles of positions held by employees, providing context for their roles in relation to the timesheet data. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking records within the 'vw_Timesheet' table, facilitating data integration and retrieval across related datasets. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_Timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, facilitating the identification of supervisory oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key associated with the manager responsible for overseeing the timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct manager, facilitating the identification of supervisory relationships within the timesheet data. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries associated with overtime hours worked, providing insights into employee time allocation within the timesheet records. |
| `Project` | `string` | The 'Project' column contains the names of the projects associated with each timesheet entry, allowing for the tracking and analysis of time spent on specific initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with the timesheet entries, facilitating the tracking and reporting of time spent on specific projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that identifies the specific profile or characteristics of the project associated with each timesheet entry, facilitating detailed project tracking and analysis. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the specific project profile associated with each timesheet entry, facilitating accurate project tracking and reporting. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into the types of work being performed. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected (true) or accepted (false) during the review process. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of timesheet data for financial analysis and reporting. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data. |
| `Status` | `string` | The 'Status' column indicates the current state of each timesheet entry, reflecting whether it is submitted, approved, or pending review. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, allowing for differentiated analysis and reporting based on the assigned tier status. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for each timesheet entry, facilitating the tracking and categorization of employee work hours within the vw_Timesheet view. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the tasks and activities recorded in the timesheet, providing context and details for each entry. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table specifies the measurement unit associated with the recorded time entries, facilitating accurate time tracking and reporting. |
| `UnitCode` | `string` | The 'UnitCode' column represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating the categorization and analysis of labor data by department or team. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and record-keeping within the timesheet management system. |
| `YearWeek` | `string` | The 'YearWeek' column represents the year and week number in a string format, facilitating the tracking and reporting of timesheet entries by week within the 'vw_Timesheet' table. |

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
   - First, it looks at the column `[Approved]`. If this column has a value of `TRUE`, it means that the item (like a timesheet or request) has been approved.
   - Second, it checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These codes likely represent specific categories or types of timesheets.

2. **Output Values**: 
   - If either of the conditions is met (meaning the item is approved or it has a TimesheetCode of "V" or "Z"), the expression returns a value of `1`. This indicates a positive outcome, meaning the item is considered approved in this context.
   - If neither condition is met, it returns a value of `0`, indicating that the item is not approved.

3. **Purpose**: The overall purpose of this calculated column is to create a simple binary indicator (1 or 0) that helps in identifying which items are approved based on either their approval status or their specific timesheet codes. This can be useful for reporting, filtering, or further analysis in a business context. 

In summary, this DAX expression helps to flag items as approved based on specific criteria, making it easier to manage and analyze approved items in the dataset.

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

1. **Check for Blank Values**: The expression first checks if the value in the 'BillableDep' column from a related table called 'lkp_Unit' is blank (i.e., it has no value). This is done using the `ISBLANK` function.

2. **Return 1 for Blank Values**: If the 'BillableDep' value is blank, the expression returns a value of 1. This indicates that if there is no information available about whether the unit is billable, it defaults to being considered billable.

3. **Check for Non-Zero Values**: If the 'BillableDep' value is not blank, the expression then checks if it is not equal to zero. This is done using the second `IF` statement.

4. **Return 1 for Non-Zero Values**: If the 'BillableDep' value is not zero, the expression again returns a value of 1. This means that if there is a valid billable amount, it is considered billable.

5. **Return 0 for Zero Values**: If the 'BillableDep' value is zero, the expression returns a value of 0. This indicates that if the unit is explicitly marked as not billable (zero), it will not be considered billable.

In summary, this DAX expression effectively categorizes units as billable or not based on the 'BillableDep' value from the related 'lkp_Unit' table. If there's no information (blank), it assumes billable (1). If there's a valid amount (non-zero), it also assumes billable (1). If the amount is zero, it marks it as not billable (0).

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

1. **Purpose**: The expression determines whether a particular timesheet entry is considered "billable" based on certain criteria.

2. **Criteria for Billability**:
   - **Sales Price Check**: It first checks if the `SalesPrice` for the timesheet entry is greater than 0. If there is a positive sales price, it indicates that the entry can be billed to a client.
   - **Project Profile Code Check**: It also checks if the `ProjectProfileCode` is equal to 81. This specific code likely represents a type of project that is always billable, regardless of other factors.
   - **Project Name Check**: Lastly, it searches for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it suggests that the project is related to customer success services, which are typically billable.

3. **Output**: 
   - If any of the above conditions are met (i.e., if the sales price is positive, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of **1**. This indicates that the timesheet entry is billable.
   - If none of these conditions are met, it returns a value of **0**, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or not based on specific financial and project-related criteria, helping the business identify which work can be charged to clients.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column stores the name of the employer associated with each timesheet entry, facilitating the identification of the organization responsible for employee compensation.

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

1. **Employee Name**: It takes the name of the employee from the 'EmployeeName' column in the 'vw_Timesheet' table.

2. **Hours per Week**: It also takes the number of hours that employee works per week from the 'HoursperWeek' column.

3. **Combining Information**: The expression then combines these two pieces of information into one string. It formats it so that the employee's name is followed by a dash and then the number of hours they work each week. For example, if the employee's name is "John Doe" and he works 40 hours per week, the result would be "John Doe - 40".

In summary, this DAX expression helps to create a clear and concise representation of each employee's name along with their weekly working hours, making it easier to read and understand the data at a glance.

**`GroupCat`** (`string`)

- **Description:** The 'GroupCat' column categorizes timesheet entries into specific groups for better organization and analysis of employee work activities.
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

2. **Check for Blank**: The `ISBLANK` function is used to determine if the result of the lookup is empty (i.e., there is no matching group for that project). The `NOT` function negates this check, meaning it will proceed if a group is found.

3. **Return Group or Category**: 
   - If a group is found (the lookup is not blank), the expression returns that group value.
   - If no group is found (the lookup is blank), it then checks if the project is billable by looking at the 'BillablePrj' field in the 'vw_Timesheet' table. If 'BillablePrj' equals 1, it categorizes the project as "Billable". 
   - If 'BillablePrj' does not equal 1, it categorizes the project as "Other Unbillable".

In summary, this DAX expression categorizes each project in the timesheet as either its corresponding group (if available), "Billable" (if it is a billable project), or "Other Unbillable" (if it is not billable and has no group). This helps in organizing and analyzing projects based on their billing status and group affiliation.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IsContractActive' in a data table, likely related to employee contracts or agreements. Here's a breakdown of what it does in simple business terms:

1. **Check for Contract End Date**: The expression first checks if the 'ContractEndDate' field in the 'vw_Timesheet' table is blank (meaning there is no end date specified) or if the end date is greater than today's date.

2. **Determine Contract Status**:
   - If either of these conditions is true (the contract has no end date or the end date is still in the future), the expression will return "Active". This indicates that the contract is currently valid and ongoing.
   - If neither condition is true (meaning there is an end date and it has already passed), it will return "Not Active". This indicates that the contract is no longer valid.

In summary, this DAX expression effectively categorizes contracts as either "Active" or "Not Active" based on whether they have an end date and whether that date has passed. This helps in easily identifying which contracts are currently in effect.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'Unit' column of the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on existing relationships in the data model.

2. **Handle Missing Values**: If the related 'Unit' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression will return the text "Unknown". This is useful for identifying records where the unit information is missing.

3. **Return Valid Unit**: If there is a valid related 'Unit' value (i.e., it is not blank), the expression will return that value instead.

In summary, this DAX expression effectively ensures that every record in the 'MAIN_UNIT' column either shows the corresponding unit name from the 'lkp_Unit' table or indicates "Unknown" if no unit information is available. This helps maintain clarity and completeness in the data by addressing potential gaps in unit information.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column stores the numerical representation of the month (as a string) corresponding to each timesheet entry, facilitating time-based analysis and reporting.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to extract the month number from a date value found in the column named `TimesheetDate`. 

In simple terms, this means that for each entry in the `TimesheetDate` column, the expression will return a number that represents the month of that date. For example:

- If the date is January 15, 2023, the expression will return `1`.
- If the date is February 10, 2023, it will return `2`.
- If the date is December 5, 2023, it will return `12`.

This calculated column, therefore, helps in organizing or analyzing data based on the month in which the timesheet entry was made, making it easier to summarize or report on activities by month.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the employee identification number as a string, facilitating the association of timesheet entries with specific employees in the 'vw_Timesheet' view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

In simple business terms, this expression counts how many different employees have recorded their time in the timesheet. If an employee appears multiple times in the timesheet (for example, if they submitted timesheets for multiple days), they will only be counted once. 

So, the result of this calculation gives you a clear picture of how many individual employees are contributing to the timesheet data, which can be useful for understanding workforce participation or resource allocation.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table captures the classification of time entries as either owned, subcontracted, or external, facilitating the analysis of resource allocation and project costs.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used to retrieve a value from a related table in a data model. Here's a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a calculated column within a table that has a relationship with another table. In this case, the current table is likely related to the `lkp_Unit` table.

2. **Purpose**: The expression fetches the value from the `OWN-Sub-ExtT` column in the `lkp_Unit` table. This means that for each row in the current table, it looks up the corresponding value from the `lkp_Unit` table based on the established relationship.

3. **Outcome**: The result is that each row in the current table will have a new column (the calculated column) that contains the value from `lkp_Unit[OWN-Sub-ExtT]`. This is useful for bringing in additional information that is related to the data in the current table, allowing for more comprehensive analysis and reporting.

In summary, this DAX expression helps to enrich the data in the current table by pulling in relevant information from a related table, which can be crucial for making informed business decisions.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column contains string values that categorize or qualify the projects associated with each timesheet entry, providing insights into project types or statuses for analysis and reporting.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'QualifyPrj'. Here's what it does in simple business terms:

1. **Check for Blank Value**: The expression first checks if the 'Qualify' value from a related table called 'lkp_Project' is blank (i.e., it has no value).

2. **Return 1 if Blank**: If the 'Qualify' value is blank, the expression returns a value of 1. This could indicate a default or placeholder status for projects that do not have a qualification assigned.

3. **Return Related Value if Not Blank**: If the 'Qualify' value is not blank, it simply returns the actual 'Qualify' value from the 'lkp_Project' table.

In summary, this DAX code is designed to ensure that every project has a qualification value. If a project does not have a qualification, it assigns a default value of 1; otherwise, it uses the existing qualification value from the related table. This helps maintain consistency in the data by ensuring that there are no blanks in the 'QualifyPrj' column.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor allocation.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of timesheet entries, represented as a string, to facilitate data enrichment processes.
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
   - It first checks if the value of the column `[ReportReady]` is `TRUE`. This means it is verifying if a report is ready.
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means it is looking for specific codes that might indicate a certain type of timesheet.

2. **Output Values**: 
   - If either of the conditions is met (i.e., if the report is ready or if the timesheet code is "V" or "Z"), the expression returns a value of `1`. This can be interpreted as a positive indicator that the item is ready or relevant.
   - If neither condition is met, it returns a value of `0`. This indicates that the item is not ready or does not meet the specified criteria.

**In summary**, this DAX expression effectively flags records as "ready" (1) if the report is ready or if the timesheet code is "V" or "Z". If neither condition is true, it flags them as "not ready" (0). This can help in filtering or analyzing data based on readiness or specific timesheet codes.

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

**In summary**: This measure helps to determine the status of a contract. If the contract is active, it provides the relevant hours difference; if not, it simply indicates that the contract is not active. This can be useful for reporting or analysis purposes, allowing users to quickly see the status and associated hours of active contracts.

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

1. **Check for Contract End Date**: The expression first looks at the maximum value of the 'ContractEndDate' from the 'vw_Timesheet' table. This means it is checking the latest end date of any contract recorded in that table.

2. **Evaluate Conditions**:
   - **ISBLANK**: It checks if this maximum end date is blank (meaning there is no end date recorded). If there is no end date, it implies that the contract is still active.
   - **Comparison with Today**: If there is an end date, it checks if this date is greater than today's date. If the end date is in the future, it also means the contract is still active.

3. **Return Status**:
   - If either of the above conditions is true (no end date or the end date is in the future), the expression returns "Active".
   - If neither condition is met (meaning the end date is in the past), it returns "Not Active".

In summary, this measure effectively categorizes contracts as "Active" or "Not Active" based on whether they have a future end date or no end date at all. This helps businesses quickly assess which contracts are currently valid and ongoing.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 15, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. Week numbers typically start from 1 for the first week of the year.

So, when you combine these two functions, `WEEKNUM(TODAY())` will give you the week number for the current date. 

For instance, if today is October 15, 2023, this expression will return the week number that corresponds to that date, which helps in understanding which week of the year we are currently in. This can be useful for reporting, analysis, or tracking performance on a weekly basis.

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

3. **Filter Condition**: The expression includes a filter condition that specifies `vw_Timesheet[Approved] = FALSE()`. This means that the calculation will only consider timesheets that have not been approved (i.e., where the `Approved` column is marked as FALSE).

In summary, this DAX measure calculates the total number of distinct employees who have submitted timesheets that are still pending approval. This can be useful for tracking how many employees are waiting for their timesheets to be reviewed and approved.

**`Dax_EmpCount_RReady`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[ReportReady] = FALSE()
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'Dax_EmpCount_RReady' that calculates the number of unique employees whose timesheets are not marked as "Report Ready."

Here's a breakdown of what it does:

1. **CALCULATE Function**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it counts the unique names of employees from the `vw_Timesheet` table.

3. **Filter Condition**: The expression includes a filter condition that specifies `vw_Timesheet[ReportReady] = FALSE()`. This means that only the timesheets that are not marked as "Report Ready" will be considered in the count.

In summary, this measure counts how many different employees have timesheets that are still pending or not ready for reporting. This can be useful for tracking the status of timesheets and understanding how many employees still need to submit or finalize their timesheets.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheet entries in the `vw_Timesheet` table.

Here's a breakdown of what it achieves:

- **DISTINCTCOUNT**: This function counts only the unique values in a specified column. In this case, it ensures that each employee is only counted once, regardless of how many times they appear in the timesheet data.

- **vw_Timesheet[EmployeeName]**: This specifies the column from which the unique employee names are being counted. It refers to the names of employees who have submitted timesheets.

In simple terms, this measure tells you how many different employees have logged their hours in the timesheet system. This is useful for understanding workforce participation, tracking employee engagement, or analyzing resource utilization in a business context.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_%_Budget_Used`**

- **DAX Expression:**
```dax
DIVIDE(SUM(vw_Timesheet[SalesAmount]), SUM('tbl_Project'[SalesPriceBaseCurrency]), 0)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of the budget that has been used based on sales data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the 'vw_Timesheet' table. Essentially, it totals the revenue generated from sales.

2. **SUM('tbl_Project'[SalesPriceBaseCurrency])**: This part sums up the total budget or sales price defined in the 'tbl_Project' table. It represents the total amount that was budgeted or expected to be earned from the projects.

3. **DIVIDE(..., 0)**: The DIVIDE function takes the total sales amount (from the first part) and divides it by the total budget (from the second part). If the total budget is zero, it will return 0 instead of causing an error (which can happen with regular division).

In simple terms, this measure calculates how much of the budget has been used by comparing actual sales to the budgeted sales. The result is a ratio that shows the proportion of the budget that has been utilized, which can help in assessing financial performance and project management.

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the timesheet. Essentially, it totals the revenue generated from sales during a specific period.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the timesheet. It gives the total number of hours that employees or team members have worked during the same period.

3. **Division**: The expression then divides the total sales amount by the total hours worked. This calculation gives you the average hourly rate, which represents how much revenue is generated for each hour worked.

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
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called 'Msr_Budget' that calculates a budget value based on the current week in the year. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: The measure starts by determining which week of the year is currently selected. This is done using the `SELECTEDVALUE` function on the `DimDate[WeekNumberOfYear]` column. This means that the measure is context-sensitive and will respond to the week that is currently being analyzed.

2. **Calculate Previous Budget**: Next, the measure calculates the budget value from the most recent week prior to the current week. It does this by using the `CALCULATE` function along with a `FILTER`. The `FILTER` function looks at all the available weeks (using `ALLSELECTED` to respect any filters applied in the report) and selects only those weeks where the week number is less than the current week. The measure `[Msr_SalesPriceBaseCurrency]` is then evaluated for these filtered weeks to find the most recent budget value.

3. **Return the Appropriate Value**: Finally, the measure checks if the current budget value (from `[Msr_SalesPriceBaseCurrency]`) is available (not blank). If it is available, it returns this current value. If it is not available (meaning there is no budget set for the current week), it falls back to the previously calculated budget value from the last available week.

In summary, this measure dynamically retrieves the budget for the current week if it exists; if not, it provides the budget from the most recent prior week. This allows for a seamless analysis of budget values over time, ensuring that users always have relevant data to work with.

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

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed or analyzed.

2. **Calculate Cumulative Cost**: The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to modify the context in which the data is evaluated. It sums up the `CostAmount` from the `vw_Timesheet` table.

4. **Filtering the Data**: The `FILTER` function is applied to ensure that only the relevant months are included in the calculation. It uses `ALLSELECTED` to consider all the months that are currently selected in the visual, but it restricts the data to those months that are less than or equal to the selected month. This means if you are looking at March, it will include January, February, and March in the calculation.

5. **Final Result**: The result of this measure is the total cost accumulated from the start of the year up to the month that is currently selected in the visual. This allows users to see how costs have built up over time, providing valuable insights into spending trends.

In summary, this DAX measure helps businesses track and visualize their cumulative costs over time, making it easier to analyze financial performance month by month.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the cumulative sales amount up to a selected month in a reporting visual. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed or filtered.

2. **Calculate Cumulative Sales**: The main goal of this measure is to calculate the total sales amount from the beginning of the year up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to modify the context in which the sales data is being evaluated. It allows us to change the filter context to include only the relevant months.

4. **Filtering the Date Table**: Inside the `CALCULATE`, there is a `FILTER` function that adjusts the context of the 'DimDate' table. It uses `ALLSELECTED` to consider all the months that are currently selected in the visual, but it only includes those months where the month number is less than or equal to the selected month. This means it sums up sales for all months from January up to the selected month.

5. **Summing Sales Amount**: Finally, the `SUM` function is applied to the 'SalesAmount' column from the 'vw_Timesheet' table, which adds up all the sales amounts that meet the criteria set by the filters.

In summary, this DAX measure calculates the total sales from the start of the year up to the month that the user has selected in the report, allowing businesses to see how sales are accumulating over time.

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
   - The `CALCULATE` function is used to modify the context of the calculation. It sums up the `Hours` column while ignoring any filters applied to the `GroupCat`, `Project`, and `EmployeeName` columns. This means it calculates the total hours across all selected groups, projects, and employees, regardless of any specific selections made in the report.

2. **Percentage Calculation**:
   - The `RETURN` statement uses the `DIVIDE` function to calculate the percentage of hours for the current context (the specific group, project, or employee being analyzed).
   - It takes the sum of hours for the current context (using `SUM(vw_Timesheet[Hours])`) and divides it by the `TotalValue` calculated earlier.
   - If `TotalValue` is zero (which would mean no hours were recorded), the `DIVIDE` function will return 0 instead of causing an error.

In summary, this measure provides insight into how much of the total hours worked is attributed to a specific group, project, or employee, expressed as a percentage. This can help in understanding workload distribution and performance across different categories.

**`MSR_PROJECT_%_REALIZATION`**

- **DAX Expression:**
```dax
IF(sum(tbl_Project[SalesPriceBaseCurrency]) <> 0, sum(vw_Timesheet[SalesAmount])/sum(tbl_Project[SalesPriceBaseCurrency]))
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called 'MSR_PROJECT_%_REALIZATION', which essentially assesses how much of the expected sales price has been realized through actual sales.

Here's a breakdown of what it does:

1. **Condition Check**: The expression starts with an `IF` statement that checks if the total sales price (from the `tbl_Project` table) is not equal to zero. This is important because if the total sales price is zero, dividing by it would lead to an error or an undefined result.

2. **Calculating Realization Percentage**: If the total sales price is indeed not zero, the expression proceeds to calculate the percentage of sales realized. It does this by taking the total sales amount (from the `vw_Timesheet` table) and dividing it by the total sales price (from the `tbl_Project` table).

3. **Result**: The result of this calculation gives you a percentage that indicates how much of the projected sales price has been achieved through actual sales. A higher percentage means that a larger portion of the expected revenue has been realized, which is a positive indicator of project performance.

In summary, this DAX measure helps businesses understand their sales performance relative to their expectations, allowing them to gauge how effectively they are converting their projected sales into actual revenue.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the profit margin for a project by subtracting the total costs from the total sales. Here's a breakdown of what it does:

1. **Total Sales Calculation**: The expression starts with `sum(vw_Timesheet[SalesAmount])`, which adds up all the sales amounts recorded in the `vw_Timesheet` table. This gives you the total revenue generated from the project.

2. **Total Costs Calculation**: Next, it calculates the total costs with `sum(vw_Timesheet[CostAmount])`, which sums up all the costs associated with the project as recorded in the same table.

3. **Profit Margin Calculation**: Finally, the expression subtracts the total costs from the total sales: `sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`. The result of this calculation represents the project's profit margin, indicating how much money is left after covering the costs.

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

1. **SUMMARIZE Function**: The expression starts by creating a summary table from the `vw_Timesheet` data. This summary groups the data by two key dimensions: `ProjectProfile` and `Client`. Essentially, it organizes the timesheet data to focus on specific projects and their associated clients.

2. **SELECTEDVALUE Function**: Within this summary, it retrieves the sales price in the base currency for each project from the `tbl_Project` table. The `SELECTEDVALUE` function is used here to get the sales price for the current context of the project being summarized. If there’s only one sales price for that project, it returns that value; if there are multiple, it returns blank.

3. **MeasureValue**: The result of the `SELECTEDVALUE` function is stored in a new column called "MeasureValue" in the summary table. This column represents the sales price in the base currency for each unique combination of project and client.

4. **SUMX Function**: Finally, the `SUMX` function iterates over the summarized table and adds up all the values in the "MeasureValue" column. This gives the total sales price in the base currency across all the projects and clients included in the timesheet.

In summary, this DAX expression calculates the total sales price in the base currency for all projects and clients listed in the timesheet, providing a clear view of revenue based on the selected projects.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of unique employees who have a "Valid" contract status on a specific day, regardless of any filters that might be applied to the data.

Here's a breakdown of what each part does:

1. **CALCULATE**: This function changes the context in which data is evaluated. It allows you to modify filters and perform calculations based on those modified filters.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it gives you the total number of different employees.

3. **ALL(vw_Timesheet)**: This function removes any filters that might be applied to the `vw_Timesheet` table. By using `ALL`, the calculation will consider all records in the table, ignoring any other filters that might limit the data (like date filters or department filters).

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This condition filters the data to only include records where the contract status of the employee is "Valid". 

Putting it all together, this DAX expression calculates the total number of unique employees who currently have a valid contract, without being affected by any other filters that might be in place in the report or dashboard. This is useful for getting a clear view of the workforce that is actively employed under valid contracts at any given time.

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

In simple terms, this measure totals all the hours that have been logged in the timesheet, providing a single number that represents the overall hours worked. This can be useful for tracking employee work hours, calculating payroll, or analyzing productivity.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the total contracted hours for employees based on their weekly hours recorded in a timesheet. Here’s a breakdown of what it does in simple business terms:

1. **VALUES(vw_Timesheet[EmployeeName])**: This part of the expression creates a unique list of employee names from the timesheet data. Essentially, it identifies each employee who has recorded hours.

2. **MAX(vw_Timesheet[HoursPerWeek])**: For each employee in the unique list, this part finds the maximum number of hours they are contracted to work in a week. If an employee has multiple entries, it will take the highest value of hours recorded.

3. **SUMX(...)**: The SUMX function then takes the unique list of employees and iterates over it. For each employee, it calculates the maximum hours per week (as determined in the previous step) and sums these maximum values together.

In summary, this DAX measure totals the highest contracted hours per week for each employee, giving you a single figure that represents the total contracted hours across all employees. This is useful for understanding the overall capacity or commitment of your workforce in terms of hours worked.

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

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (meaning there are no hours recorded). 

3. **IF(...)**: The IF function evaluates the condition provided by ISBLANK. If the total hours calculated is blank (no hours tracked), it returns 0. If there are hours tracked, it returns the total hours calculated.

In summary, this measure ensures that if there are no hours recorded, it will show a total of 0 instead of a blank value. This is useful for reporting and analysis, as it provides a clear numerical output (0) rather than leaving it empty, making it easier to understand and interpret the data.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total hours tracked in a timesheet, but it includes a check to see if there are any hours recorded. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the `Hours` column of the `vw_Timesheet` table.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (meaning there are no hours recorded).

3. **IF(...)**: This function evaluates the condition provided by ISBLANK. If the total hours are blank (i.e., there are no hours tracked), it returns the text "Empty". If there are hours recorded, it returns the total sum of those hours.

In simple terms, this measure checks if any hours have been logged in the timesheet. If no hours are logged, it indicates that by returning "Empty". If hours are logged, it provides the total number of hours tracked. This is useful for quickly understanding whether there is any data available for analysis or reporting.

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

1. **Total Hours Tracked**: The expression starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked for a specific employee or group of employees.

2. **Maximum Hours Per Week**: The second part of the expression uses the `CALCULATE` function to find the maximum number of hours that have been recorded per week for each employee. This is done by:
   - Using `MAXX` to evaluate the maximum value from a distinct list of hours per week (`vw_Timesheet[HoursPerWeek]`).
   - The `ALLEXCEPT` function is used to ensure that this calculation is done for each employee individually, ignoring any other filters that might be applied to the data.

3. **Calculating the Difference**: Finally, the expression subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. 

### What It Achieves:
The overall goal of this measure, `trackedDiff`, is to show how much the total hours tracked for an employee exceed their maximum recorded hours in any single week. If the result is positive, it indicates that the employee has tracked more hours than their maximum weekly limit; if it's negative or zero, it means they have not exceeded their maximum weekly hours. This can help in monitoring employee workload and ensuring that hours are being tracked appropriately against expected limits.

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
- **DAX Explanation (Generated):** The DAX expression you've provided calculates a measure called `trackedDiff2`, which essentially determines the difference between the total hours tracked and the maximum hours recorded per week for each employee, while ignoring any specific filters on the hours per week.

Here's a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked for a specific context (like a project, employee, or time period).

2. **Maximum Hours Per Week**: The next part of the expression calculates the maximum number of hours that have been recorded per week for each employee. This is done using:
   - `MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])`: This part looks at the unique values of hours worked per week and finds the highest value among them.
   - `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])`: This ensures that any filters applied to the hours per week are ignored, allowing the calculation to consider all recorded hours.
   - `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])`: This keeps the context focused on each individual employee while removing other filters, ensuring that the maximum hours per week is calculated specifically for each employee.

3. **Calculating the Difference**: Finally, the measure subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. This gives you the difference between what has been tracked and the highest amount that has been recorded in a week for that employee.

**In summary**, `trackedDiff2` measures how much the total hours tracked for an employee exceeds their maximum recorded hours in any single week. This can help identify if an employee is consistently working more hours than they typically log in a week, which could be useful for workload management or identifying potential burnout.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total billable hours from a timesheet data source, specifically focusing on certain criteria related to projects. Here's a breakdown of what it does in simple business terms:

1. **Variable Definition (FilteredHours)**: The expression starts by defining a variable called `FilteredHours`. This variable is created by filtering the `vw_Timesheet` table based on specific conditions.

2. **Filtering Criteria**:
   - The filter checks for two main conditions:
     - **Qualifying Projects**: It looks for entries where the `QualifyPrj` column is equal to 1, indicating that the project qualifies for billing.
     - **Billable Projects**: It also checks if the `BillablePrj` column is equal to 1, meaning the project is billable.
   - Additionally, it includes any projects that contain the phrase "Customer Success Services" in the `Project` column. This is done using the `SEARCH` function, which looks for that specific text within the project names.

3. **Summation of Hours**: After filtering the timesheet data based on the above criteria, the expression uses the `SUMX` function to calculate the total hours worked (from the `Hours` column) for all the entries that meet the filtering conditions.

4. **Return Value**: Finally, the expression returns the total sum of billable hours that meet the specified criteria.

In summary, this DAX measure calculates the total number of hours that can be billed to clients, focusing on projects that are either specifically marked as qualifying and billable or are related to "Customer Success Services." This helps businesses track and report on their billable work effectively.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** This DAX expression is used to calculate a measure called 'UTI_TOTALHOURS'. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the values in the 'Hours' column from the 'vw_Timesheet' table. Essentially, it totals the number of hours recorded.

2. **CALCULATE(...)**: The CALCULATE function modifies the context in which the data is evaluated. In this case, it is used to apply a specific filter to the data being summed.

3. **vw_Timesheet[QualifyPrj] = 1**: This is the filter condition applied by the CALCULATE function. It specifies that only the rows in the 'vw_Timesheet' table where the 'QualifyPrj' column equals 1 should be considered in the sum. This means that the measure will only count hours that are associated with projects that qualify under this condition.

In summary, the 'UTI_TOTALHOURS' measure calculates the total number of hours worked on projects that meet a specific qualification (where 'QualifyPrj' equals 1). This helps businesses understand how many hours are being spent on eligible projects, which can be important for budgeting, resource allocation, and performance tracking.

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
   - `_TotalBillableHours`: This variable captures the total number of hours that have been billed to clients or customers. It represents the productive time that generates revenue.
   - `_TotalHours`: This variable captures the total number of hours worked by employees, which includes both billable and non-billable hours.

2. **Return Logic**:
   - The code checks if there are any active contacts (employees) using the measure `[Msr_ActiveContacts]`. If there are no active employees, the calculation does not proceed.
   - If there are active employees, it then checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the calculation does not proceed further.
   - If both conditions are met, it calculates the Billable Hour Ratio by dividing the total billable hours by the total hours worked. This ratio indicates what portion of the total hours worked is billable.

3. **Handling Division**:
   - The `DIVIDE` function is used to perform the division safely. If the result of the division is blank (which could happen if `_TotalHours` is zero), it returns 0 instead of an error.

In summary, this DAX measure calculates the proportion of hours worked by active employees that are billable to clients. It helps the business understand how effectively its employees are utilizing their time in terms of generating revenue.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

The following filters are applied to the entire report:

- Filter on `DimDate`.`CurrentYearFlag` (Type: Advanced, Definition: `{"Version": 2, "From": [{"Name": "d", "Entity": "DimDate", "Type": 0}], "Where": [{"Condition": {"Comparison": {"ComparisonKind": 0, "Left": {"Column"...`)

### <a name="report-pages"></a>Report Pages

#### <a name="page-customer-analysis"></a>Page: Customer Analysis

*Internal Name: `ecc7667875c00809763b`, Ordinal: 0*

##### Page Level Filters

- Filter on `vw_Timesheet`.`BillablePrj` (Type: Advanced, Definition: `{"Version": 2, "From": [{"Name": "v", "Entity": "vw_Timesheet", "Type": 0}], "Where": [{"Condition": {"Comparison": {"ComparisonKind": 0, "Left": {"Co...`)

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

- Filter on `vw_Timesheet`.`BillablePrj` (Type: Advanced, Definition: `{"Version": 2, "From": [{"Name": "v", "Entity": "vw_Timesheet", "Type": 0}], "Where": [{"Condition": {"Comparison": {"ComparisonKind": 0, "Left": {"Co...`)

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

