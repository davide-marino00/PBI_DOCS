# Power BI Model & Report Documentation

*Generated on: 2025-04-28 17:55:51*

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

The Power BI data model appears to be designed for a project management and time tracking system, likely within a corporate or organizational context. It integrates various dimensions and fact tables to facilitate comprehensive reporting and analysis of employee hours, project performance, and timesheet compliance. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a focus on capturing and monitoring time entries, while the lookup tables ('lkp_Project', 'lkp_Unit', 'lkp_Code', and 'lkp_fltr_Employee') provide essential contextual information about projects, organizational units, and timesheet codes.

The structure of the model indicates a star schema, where 'vw_Timesheet' serves as a central fact table linked to multiple dimension tables, allowing for detailed analysis of time allocation across different projects and units. The inclusion of 'DimDate' enables time-based analysis, facilitating insights into trends over specific periods. Additionally, the 'vw_missing_timesheet' table highlights a mechanism for identifying gaps in timesheet submissions, which is crucial for ensuring accurate project accounting and resource management. Overall, this data model supports decision-making processes related to project management, resource allocation, and operational efficiency.

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

The 'DimDate' table serves as a comprehensive date dimension, providing essential temporal attributes that enable businesses to analyze and report on data across various timeframes. By offering detailed breakdowns of dates, including day, week, month, and quarter information, this table supports effective time-based analytics and enhances the ability to track trends and performance over time.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting within the dataset. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient date-based queries and data enrichment processes. |
| `DayName` | `string` | The 'DayName' column contains the full name of the day (e.g., Monday, Tuesday) corresponding to each date in the 'DimDate' table, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | The 'DayNumberOfMonth' column (int64) in the 'DimDate' table represents the numerical day of the month, ranging from 1 to 31, providing a granular reference for date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical designation of each day of the week, where values range from 1 (Sunday) to 7 (Saturday), facilitating date-related analyses and reporting. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column represents the sequential day of the year as an integer, ranging from 1 to 365 (or 366 in leap years), providing a numerical reference for each date in the 'DimDate' table. |
| `MonthName` | `string` | The 'MonthName' column contains the full names of the months, providing a human-readable representation of the month associated with each date in the 'DimDate' table. |
| `MonthNumberOfYear` | `int64` | The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month, ranging from 1 for January to 12 for December, facilitating time-based analysis and reporting. |
| `ShortDate` | `dateTime` | The 'ShortDate' column in the 'DimDate' table represents a simplified date format, capturing the specific date without time details for streamlined data analysis and reporting. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** The 'CurrentYearFlag' column indicates whether a given date falls within the current calendar year, using a string value to represent this status.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CurrentYearFlag' in a data model, specifically within a date dimension table (DimDate). 

Here's what it does in simple terms:

1. **YEAR(DimDate[ShortDate])**: This part extracts the year from the date in the 'ShortDate' column of the DimDate table. For example, if the date is '2023-05-15', it will return '2023'.

2. **YEAR(TODAY())**: This part gets the current year based on today's date. If today is any date in 2023, this will return '2023'.

3. **IF(..., 1, 0)**: The IF function checks if the year extracted from the 'ShortDate' is less than or equal to the current year. If it is, the expression returns '1'; if not, it returns '0'.

### What it achieves:
- The 'CurrentYearFlag' column will have a value of '1' for all dates in the DimDate table that are from the current year or any previous year. 
- It will have a value of '0' for any future dates (dates that are in the next year or beyond).

### In summary:
This calculated column helps identify whether a date is in the current year or earlier, marking it with a '1', while marking future dates with a '0'. This can be useful for filtering or analyzing data based on whether it pertains to the current or past years.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table is designed to track and analyze the distribution of hours worked alongside their corresponding percentage contributions to overall performance metrics. This table aids in understanding workforce productivity and resource allocation, enabling informed decision-making for operational efficiency.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column contains string representations of hours worked alongside their corresponding percentage contributions, providing insights into employee productivity and time allocation. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and percentage calculations, facilitating enriched data analysis within the 'Hours_nd_Percentage' table. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, with associated qualification levels and sorting criteria to facilitate efficient data analysis and reporting. This table enhances data integrity and consistency by providing a centralized source for code definitions and their hierarchical organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers used to categorize and enrich data within the lookup table 'lkp_Code'. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numerical identifier for categorizing data enrichment criteria. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table specifies the order in which records should be arranged, facilitating efficient data retrieval and organization. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics. This table enhances data integrity and usability by standardizing employee information for analysis and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column serves as a unique identifier for each employee in the 'lkp_fltr_Employee' table, facilitating data enrichment and retrieval processes. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, serving as a key identifier for enriching employee-related data within the lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, capturing essential attributes such as project names, qualification scores, billing amounts, and associated groups. This table enables businesses to analyze project performance and financial metrics, facilitating informed decision-making and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing amount associated with each project, facilitating financial tracking and analysis. |
| `Group` | `string` | The 'Group' column identifies the category or classification of each project within the lookup table, facilitating data enrichment and organization. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the lkp_Project table. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, with integer values representing different qualification levels or criteria met. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their identifiers and billing classifications. It facilitates efficient reporting and analysis by linking units to their respective attributes, ensuring accurate tracking of billable departments and external affiliations.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column represents the identifier for departments that are eligible for billing, facilitating the tracking of billable units within the organization. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column contains the names of organizational units, serving as a key identifier for categorizing and managing data related to different segments within the organization. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores external type identifiers for owned sub-units, facilitating data enrichment and integration processes. |
| `Unit` | `string` | The 'Unit' column contains string values representing the various measurement units used for data enrichment purposes. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, status, leadership, and financial aspects. This table enables businesses to effectively manage and analyze their projects, facilitating better decision-making and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and reporting within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column contains string values that represent the administrative details or oversight responsibilities associated with each project listed in the 'tbl_Project' table. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates within the 'tbl_Project' table. |
| `ClientName` | `string` | The 'ClientName' column stores the name of the client associated with each project, facilitating identification and management of client-specific projects within the 'tbl_Project' table. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating the tracking and management of client-related data within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated in the 'tbl_Project' table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project, represented as a string. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the exact date and time when each project entry was created in the 'tbl_Project' table, providing a timestamp for tracking project initiation. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or business unit associated with each project in the 'tbl_Project' table. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column indicates the scheduled date and time for the completion of the project, facilitating project timeline management and tracking. |
| `Project` | `string` | The 'Project' column contains the names or identifiers of various projects associated with the data entries in the 'tbl_Project' table, facilitating the organization and retrieval of project-specific information. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed textual overview of each project, outlining its objectives, scope, and key features to provide stakeholders with a comprehensive understanding of the project's purpose and goals. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the specific category or classification of a project within the 'tbl_Project' table, facilitating organization and management of related projects. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a textual summary of the project group, detailing its objectives and scope within the context of the overall project management framework. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary key for data integrity and relational mapping. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the project's execution within the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency sales price of a project, stored as a decimal value to facilitate accurate financial calculations and reporting. |
| `Startdate` | `dateTime` | The 'Startdate' column records the date and time when a project is initiated, serving as a key reference point for project timelines and scheduling within the 'tbl_Project' table. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, allowing for effective tracking and management of project workflows. |
| `Statuscode` | `string` | The 'Statuscode' column indicates the current operational status of each project, represented as a string, to facilitate tracking and management of project progress within the 'tbl_Project' table. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, expressed as a decimal to accommodate precise financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table is designed to identify and track employees who have not submitted their timesheets for specific weeks, providing essential insights for managers to ensure compliance with reporting requirements. It includes key details such as employee and manager information, contract dates, and hours, enabling effective follow-up and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column represents the total number of hours worked by an employee, recorded as a double data type, within the context of identifying missing timesheet entries in the 'vw_missing_timesheet' table. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract is set to expire, providing essential information for managing timesheet compliance and contract renewals within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer value, to assist in identifying discrepancies in timesheet submissions. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract for each employee, providing insights into their employment status as it relates to timesheet submissions. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on absent submissions. |
| `Hours_` | `double` | The 'Hours_' column represents the total number of hours recorded for each entry in the 'vw_missing_timesheet' table, stored as a double to accommodate fractional hours. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_missing_timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and tracking of missing submissions. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees' timesheet submissions, facilitating accountability and tracking within the 'vw_missing_timesheet' table. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee work hour reporting. |
| `Project` | `string` | The 'Project' column contains the names of projects associated with missing timesheet entries, facilitating the identification and tracking of unrecorded work hours. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project associated with the timesheet entries, facilitating the tracking and management of time allocation across various projects. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected (true) or not (false) in the context of tracking missing timesheets. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of tracking missing timesheet entries for accurate financial reporting. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double data type, within the 'vw_missing_timesheet' table, which is designed to enhance data analysis related to timesheet discrepancies. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with missing timesheet entries, formatted to facilitate quick reference and analysis. |
| `SubUnit` | `string` | The 'SubUnit' column contains string values representing the specific sub-department or team within the organization associated with each entry in the 'vw_missing_timesheet' table, aiding in the identification of missing timesheet submissions by organizational segment. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for timesheets, facilitating the tracking and management of missing timesheet entries within the 'vw_missing_timesheet' view. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the timesheet entries, providing context and details about the work performed during the reported period. |
| `Week_` | `int64` | The 'Week_' column (int64) represents the numerical week of the year associated with each entry in the 'vw_missing_timesheet' table, facilitating the identification of timesheet data gaps on a weekly basis. |
| `Year_` | `int64` | The 'Year_' column (int64) in the 'vw_missing_timesheet' table represents the calendar year associated with each timesheet entry, facilitating the analysis of missing timesheet data over time. |

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

1. **Check for Related Data**: The expression first checks if there is a related value in the 'BillableDep' column from the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on the relationships defined in your data model.

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of specific information, the default assumption is that the item is billable.

3. **Check for Non-Zero Values**: If there is a related 'BillableDep' value and it is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that the item is considered billable.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that the item is not billable.

In summary, this DAX expression effectively categorizes items as billable or not based on the related 'BillableDep' value. It defaults to billable (1) if there is no related data or if the related value is non-zero, and it marks the item as not billable (0) if the related value is zero.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named `CC_ActiveEmployees` in a data model, specifically within a table called `vw_missing_timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether an employee is considered "active" based on their contract dates.

2. **Conditions Checked**:
   - **Start Date Check**: It first checks if the date in the column `ShortDate` is on or after the employee's `ContractStartDate`. This means the employee's contract must have started before or on this date for them to be considered active.
   - **End Date Check**: Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. If the `ContractEndDate` is blank (meaning the employee's contract does not have an end date), the employee is still considered active.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the contract has no end date), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns a value of `0`, indicating that the employee is not active.

In summary, this DAX expression effectively flags employees as active (1) or inactive (0) based on whether the current date falls within their contract period. This is useful for reporting and analysis purposes, helping businesses understand which employees are currently active based on their contractual agreements.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to assist in identifying records that require further attention in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IncompleteFlag' in a data model. Here's what it does in simple business terms:

- The expression checks the value in the 'MissingHours' column of the 'vw_missing_timesheet' table.
- If the value of 'MissingHours' is less than 0, it means that there is an issue (like negative hours, which typically shouldn't happen in a normal scenario).
- In this case, the expression returns a value of 1, indicating that there is an "incomplete" or problematic entry.
- If the value of 'MissingHours' is 0 or greater, the expression returns 0, indicating that there are no issues with that entry.

In summary, this calculated column flags entries with negative missing hours as problematic (1) and those with zero or positive hours as normal (0). This helps in identifying and addressing any discrepancies in timesheet data.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'MAIN_UNIT'. Here's a simple breakdown of what it does:

1. **Check for Blank Values**: The expression first checks if the related value from the 'lkp_Unit' table (specifically the 'Unit' column) is blank (i.e., missing or not available).

2. **Return "Unknown" if Blank**: If the 'Unit' value is blank, the expression returns the text "Unknown". This is a way to handle situations where there is no corresponding unit available, ensuring that the result is clear and informative.

3. **Return the Unit if Not Blank**: If the 'Unit' value is not blank, the expression returns the actual value from the 'lkp_Unit[Unit]' column. This means that if there is a valid unit associated with the current record, it will be displayed.

In summary, this DAX expression effectively ensures that every record in the 'MAIN_UNIT' column will either show the corresponding unit name or "Unknown" if no unit is available. This helps maintain data clarity and integrity by avoiding blank entries.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column represents the combination of the year and week number, formatted as a string, to facilitate the tracking and reporting of timesheet data on a weekly basis.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX expression creates a new calculated column called 'YearWeek' that combines the year and the week number of a date from the 'ContractEndDate' field.

Here's a breakdown of what it does:

1. **YEAR([ContractEndDate])**: This part extracts the year from the 'ContractEndDate'. For example, if the date is December 15, 2023, this will return "2023".

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the 'ContractEndDate'. For instance, if the date is in the 3rd week of December, it might return "50".

3. **FORMAT(..., "00")**: This ensures that the week number is always displayed as two digits. So, if the week number is 5, it will be formatted as "05".

4. **Concatenation (&)**: The expression combines the year and the formatted week number into a single string, separated by a hyphen. So, if the year is "2023" and the week number is "50", the final result will be "2023-50".

In summary, this DAX expression generates a string that represents the year and the week number of the 'ContractEndDate', formatted as "YYYY-WW". This can be useful for reporting or analysis purposes, allowing you to easily identify and group data by year and week.

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
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the unique names of employees from the 'vw_missing_timesheet' table. It ensures that each employee is only counted once, even if they have multiple entries.
   
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This condition filters the data to only include entries where the 'MissingHours' value is less than zero. In practical terms, this means it looks for cases where employees have reported negative hours, which could indicate an error or a need for adjustment in their timesheet.

3. **`CALCULATE` Function**: The `CALCULATE` function modifies the context in which the data is evaluated. In this case, it applies the filter for negative hours before counting the distinct employee names.

**Overall Outcome**: The measure ultimately provides a count of how many different employees have reported negative hours in their timesheets. This information can be useful for identifying potential issues with timesheet entries or understanding the extent of discrepancies in reported hours.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have recorded negative missing hours in a timesheet report. Here's a breakdown of what it does in simple business terms:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This part counts the number of unique Employee IDs. It ensures that each employee is only counted once, even if they have multiple entries.

3. **Filter Condition**: The expression includes a filter that only considers records where the 'MissingHours' value is less than 0. This means it focuses on cases where employees have reported negative hours, which could indicate an error or a specific situation that needs attention.

In summary, this measure counts how many different employees have reported negative missing hours in their timesheets. This can help the business identify potential issues with timesheet entries or highlight employees who may need further review regarding their reported hours.

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

1. **Data Source**: The expression starts by looking at a data source called `vw_missing_timesheet`, which likely contains records of employees' timesheets, including hours worked and contract hours.

2. **Summarization**: It summarizes the data by grouping it based on several criteria:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a smaller division within the unit)

   For each group, it calculates two key metrics:
   - **MissingHours**: This is calculated by taking the total hours recorded (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contract hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has not logged enough hours compared to what they are contracted for.
   - **MinActiveEmp**: This checks if the employee is active by taking the minimum value of `vw_missing_timesheet[CC_ActiveEmployees]`. If this value is 1, it means the employee is currently active.

3. **Filtering**: After summarizing the data, the expression filters the results to include only those groups where:
   - The employee is active (`[MinActiveEmp] = 1`)
   - The employee has missing hours (`[MissingHours] < 0`)

4. **Counting**: Finally, it counts the number of rows that meet these criteria, which gives the total number of active employees who have not logged enough hours in their timesheets.

In summary, this DAX measure calculates how many active employees have missing timesheet hours, helping the business identify potential issues with time tracking and ensuring that all employees are accurately reporting their work hours.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of missing hours for active employees who have not met their contracted hours in a given time period. Here's a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. It filters a summarized view of the `vw_missing_timesheet` data, which contains information about employees, their hours worked, and their contracted hours.

2. **Summarization**: The `SUMMARIZE` function groups the data by employee name, year, week, unit, and subunit. For each group, it calculates two key pieces of information:
   - **MissingHours**: This is calculated by taking the total hours worked by the employee (using `SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contracted hours for that employee (using `MAX(vw_missing_timesheet[ContractHours])`). If the result is negative, it indicates that the employee has not worked enough hours compared to what they were contracted for.
   - **MinActiveEmp**: This checks if the employee is active by finding the minimum value of `CC_ActiveEmployees`. If this value is 1, it means the employee is currently active.

3. **Filtering Criteria**: The `FILTER` function then narrows down the summarized data to only include those employees who are active (`[MinActiveEmp] = 1`) and have negative missing hours (`[MissingHours] < 0`). This means we are only looking at active employees who have not fulfilled their contracted hours.

4. **Calculate Total Missing Hours**: Finally, the `SUMX` function takes the filtered list of active employees and sums up their `MissingHours`. This gives the total number of hours that active employees are missing compared to their contracted hours.

In summary, this DAX measure calculates the total hours that active employees are short of their contracted hours, helping the business identify potential issues with employee attendance or workload management.

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

1. **Data Source**: It uses a table called `vw_missing_timesheet`, which likely contains records of timesheets that are missing for various employees.

2. **Grouping Data**: The `SUMMARIZE` function groups the data by two key columns: `Week_` (which represents the week of the year) and `EmployeeID` (which identifies each employee). This means that for each unique combination of week and employee, it will create a summary row.

3. **Calculating Maximum Contract Hours**: For each group (each employee in each week), it calculates the maximum value of `ContractHours`. This means if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

4. **Summing Up**: The `SUMX` function then takes the summarized data and adds up all the maximum contract hours calculated for each employee across all weeks. 

In summary, this DAX expression effectively totals the highest expected contract hours for each employee for each week, providing a clear view of the total expected hours across the organization for the specified time period.

**`MISSING%`**

- **DAX Expression:**
```dax
DIVIDE([Dax_EmpCount]-[Dax_EmpCount_MissingTS],[Dax_EmpCount]) * 100
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of employees that are not missing a timestamp (TS) in a dataset. Here's a breakdown of what it does:

1. **[Dax_EmpCount]**: This represents the total count of employees in the dataset.

2. **[Dax_EmpCount_MissingTS]**: This represents the count of employees who are missing a timestamp.

3. **[Dax_EmpCount] - [Dax_EmpCount_MissingTS]**: This part calculates the number of employees who have a timestamp. It does this by subtracting the count of employees missing a timestamp from the total employee count.

4. **DIVIDE(..., [Dax_EmpCount])**: The `DIVIDE` function takes the number of employees with a timestamp (calculated in the previous step) and divides it by the total employee count. This gives the proportion of employees who have a timestamp.

5. **... * 100**: Finally, the result is multiplied by 100 to convert the proportion into a percentage.

In summary, this DAX expression calculates the percentage of employees who have a timestamp recorded, helping to assess data completeness regarding employee records. A higher percentage indicates better data quality, while a lower percentage suggests that many employee records are missing timestamps.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked and contracted hours for employees or contractors. Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum value of contracted hours from the `vw_missing_timesheet` table. Contracted hours refer to the total number of hours that an employee or contractor is expected to work as per their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum value of actual hours worked (or reported hours) from the same table. This represents the total number of hours that the employee or contractor has actually logged or worked.

3. **Subtraction**: The expression then subtracts the maximum actual hours worked from the maximum contracted hours. 

### What it Achieves:
The result of this calculation gives you the maximum number of hours that are missing or unaccounted for, which means it shows how many hours an employee or contractor has not worked compared to what they were supposed to work. 

In summary, this measure helps identify any discrepancies between expected and actual work hours, allowing management to address potential issues such as underreporting of hours or non-compliance with contracted hours.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of completeness regarding missing hours in timesheets compared to expected contract hours. Here’s a breakdown of what it does:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part adds up all the missing hours recorded in the `vw_missing_timesheet` table. It gives the total number of hours that were not logged as expected.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the expected number of hours that should have been worked in a week according to the contract.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: This calculation finds the difference between the total missing hours and the expected contract hours. If the result is positive, it indicates that there are more missing hours than expected; if negative, it means that the logged hours meet or exceed expectations.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The `DIVIDE` function takes the result from the previous step (the difference) and divides it by the expected contract hours. This gives a ratio that represents how much the missing hours exceed the expected hours. If the expected hours are zero, it will return 0 instead of causing an error.

In summary, this measure calculates the proportion of missing hours relative to what was expected, allowing the business to understand how much of the expected work is unaccounted for. A higher percentage indicates a greater issue with missing timesheets, while a lower percentage suggests better compliance with expected hours.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total number of unique employees who have missing timesheets, grouped by week and main unit. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The expression uses a table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions.

2. **Grouping**: The `SUMMARIZE` function is used to create a summary table. This summary groups the data by:
   - **Week**: The specific week in question.
   - **Employee Name**: The name of each employee.
   - **Main Unit**: The main unit or department the employee belongs to.

3. **IncompleteFlag**: For each group (combination of week, employee, and main unit), it calculates the maximum value of the `IncompleteFlag`. This flag indicates whether the employee has missing timesheet entries. If at least one entry is missing for that group, the flag will be set to 1 (or true); otherwise, it will be 0 (or false).

4. **Summation**: The outer `SUMX` function then sums up the values of the `IncompleteFlag` for all the groups created in the previous step. This effectively counts how many unique employees have missing timesheets across the specified weeks and units.

In summary, this measure provides a total count of unique employees who have not submitted their timesheets, helping the business identify areas where compliance may be lacking and where follow-up may be needed.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This data is crucial for tracking labor costs, project resource allocation, and overall workforce productivity within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column identifies the specific client associated with each timesheet entry, facilitating the tracking of billable hours and project management. |
| `Code2` | `string` | The 'Code2' column stores a secondary identifier or classification code related to the timesheet entries, facilitating enhanced data categorization and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to terminate, providing essential information for timesheet management and resource planning. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and payroll calculations within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract associated with the timesheet entry, represented as a string for easy interpretation and reporting. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, represented as a string, to facilitate real-time reporting and analysis within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the specific date and time when a contract was transferred, providing essential temporal context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the monetary value associated with the hours worked, expressed as a double data type, within the context of timesheet entries. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, expressed as a double to allow for precise financial calculations. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions recorded in the timesheet, represented as a string. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the individual or entity responsible for payment, facilitating the tracking and management of financial obligations within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee associated with the timesheet entries in the 'vw_Timesheet' table, facilitating accurate tracking and reporting of work hours. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column contains the concatenated identifier and name of employees, facilitating easy identification and reference within the timesheet records. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with specific employees for accurate tracking and reporting. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees associated with each timesheet entry, facilitating the identification of personnel for time tracking and reporting purposes. |
| `Employer` | `string` | The 'Employer' column contains the name of the organization or company that employs the individual associated with the timesheet entries. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating the tracking and management of employee work hours by organization. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to the timesheet entries, providing further insights into the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with the recorded timesheet entries, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, facilitating accurate tracking of labor input for payroll and project management. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating analysis of work patterns and resource allocation. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee work hours. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee work hours. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and management. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries, facilitating accountability and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the names of the IPM IDs associated with each timesheet entry, facilitating the identification and tracking of project management metrics. |
| `JobTitle` | `string` | The 'JobTitle' column contains the titles of positions held by employees, providing context for their roles in relation to the timesheet data. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking records within the 'vw_Timesheet' table to other related datasets, facilitating data integration and analysis. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and reporting within the organization. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, facilitating easy identification of managerial oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for the manager associated with each timesheet entry, facilitating the tracking and management of employee work hours. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct manager, facilitating the identification of supervisory relationships within the timesheet data. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries associated with employee timesheets, providing detailed insights into the hours worked on specific tasks or projects. |
| `Project` | `string` | The 'Project' column contains the names of the projects associated with each timesheet entry, allowing for the tracking and analysis of time spent on specific initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with the timesheet entries, facilitating the tracking and reporting of time spent on specific projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that represents the specific characteristics and details of the project associated with each timesheet entry, facilitating better project management and resource allocation. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the project profile associated with each timesheet entry, facilitating the tracking and analysis of time spent on various projects. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into the types of work being performed. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected (true) or accepted (false) during the review process. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness and false indicating it is not yet prepared. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double data type for precision in financial calculations within the 'vw_Timesheet' table. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current state of the timesheet entry, reflecting whether it is submitted, approved, or pending review. |
| `Taxed` | `boolean` | Indicates whether the recorded timesheet entry is subject to taxation, with a value of true representing taxable entries and false for non-taxable ones. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, helping to differentiate between various levels of project engagement or resource allocation. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours and related activities. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the tasks or activities recorded in the timesheet, providing context for the hours logged by employees. |
| `Unit` | `string` | The 'Unit' column represents the measurement unit associated with the time entries recorded in the 'vw_Timesheet' table, facilitating the understanding of the duration or quantity of work performed. |
| `UnitCode` | `string` | The 'UnitCode' column represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating the categorization and analysis of labor data by department or team. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and record-keeping within the timesheet management system. |
| `YearWeek` | `string` | The 'YearWeek' column represents the combination of the year and week number, formatted as a string, to facilitate time-based reporting and analysis in the timesheet data. |

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'Approved_With_V_Z'. Here's what it does in simple business terms:

1. **Condition Check**: The expression checks two conditions:
   - It first checks if the value in the 'Approved' column is TRUE (meaning the item is approved).
   - It also checks if the 'TimesheetCode' in the 'vw_Timesheet' table is either "V" or "Z".

2. **Output Values**: 
   - If either of the conditions is met (i.e., the item is approved or the timesheet code is "V" or "Z"), the expression returns a value of **1**.
   - If neither condition is met, it returns a value of **0**.

3. **Purpose**: The purpose of this calculated column is to create a simple binary indicator (1 or 0) that signifies whether a timesheet is either approved or falls under specific codes ("V" or "Z"). This can be useful for filtering, reporting, or further analysis in your data model, allowing users to easily identify which timesheets meet these criteria.

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

1. **Check for Related Data**: The expression first checks if there is a related value in the 'BillableDep' column from the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on the relationships defined in the data model.

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of specific information, the default assumption is that the item is billable.

3. **Check for Non-Zero Values**: If there is a related 'BillableDep' value and it is not blank, the expression then checks if this value is not equal to **0**. If the value is not zero, it again returns **1**, indicating that the item is considered billable.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is **0**, the expression returns **0**, indicating that the item is not billable.

In summary, this DAX expression effectively categorizes items as billable or not based on the related 'BillableDep' value. If there is no related data, it defaults to billable (1). If there is related data and it is non-zero, it is also considered billable (1). If the related data is zero, it is marked as not billable (0).

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'BillablePrj' in a data model, specifically in a table named `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether a particular timesheet entry is considered "billable" based on certain conditions.

2. **Conditions Checked**:
   - **Sales Price**: It first checks if the `SalesPrice` for the entry is greater than 0. This means that if there is a positive sales price, the entry is likely billable.
   - **Project Profile Code**: It also checks if the `ProjectProfileCode` is equal to 81. If it is, this specific project is predefined as billable regardless of other factors.
   - **Project Name**: Lastly, it looks for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it indicates that the project is related to customer success services, which are typically billable.

3. **Output**: 
   - If any of these conditions are met (i.e., if the sales price is positive, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of **1**. This signifies that the timesheet entry is billable.
   - If none of these conditions are met, it returns a value of **0**, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project name. This helps in identifying which projects or entries can be invoiced to clients.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column contains the name of the employer associated with the timesheet entry, providing essential context for payroll and labor tracking.

**`Employee_WeeklyHours`** (`string`)

- **Description:** The 'Employee_WeeklyHours' column captures the total number of hours worked by each employee during the week, represented as a string for flexible formatting.
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

3. **Combining Information**: The expression then combines these two pieces of information into one string. It formats it so that the employee's name is followed by a dash and then the number of hours they work each week. For example, if the employee's name is "John Doe" and he works 40 hours a week, the result would be "John Doe - 40".

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'GroupCat' in a data model, likely related to project management or timesheet tracking. Here's a breakdown of what it does in simple business terms:

1. **Check for Project Group**: The expression first checks if there is a corresponding project group for the project listed in the 'vw_Timesheet' table. It does this by using the `LOOKUPVALUE` function to find the 'Group' from the 'lkp_Project' table based on the project name.

2. **Return Project Group if Exists**: If a project group is found (i.e., the result is not blank), it returns that project group name. This means that if the project is associated with a specific group, that group will be assigned to the 'GroupCat' column.

3. **Handle Missing Project Group**: If no project group is found (the result is blank), the expression then checks if the project is billable. It does this by looking at the 'BillablePrj' column in the 'vw_Timesheet' table.

4. **Assign Billable or Unbillable Category**: 
   - If 'BillablePrj' equals 1 (indicating that the project is billable), it assigns the value "Billable" to the 'GroupCat' column.
   - If 'BillablePrj' does not equal 1 (indicating that the project is not billable), it assigns the value "Other Unbillable" to the 'GroupCat' column.

In summary, this DAX expression categorizes each entry in the 'vw_Timesheet' table into a project group if available, or labels it as "Billable" or "Other Unbillable" based on whether the project is billable or not. This helps in organizing and analyzing projects based on their billing status and associated groups.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IsContractActive' in a data model, specifically for a table named 'vw_Timesheet'. 

Here's what it does in simple terms:

1. **Checks Contract End Date**: The expression first checks if the 'ContractEndDate' for each record (or row) in the 'vw_Timesheet' table is blank (meaning there is no end date specified) or if the end date is greater than today's date.

2. **Determines Contract Status**:
   - If either of those conditions is true (the end date is blank or it is in the future), the expression will return "Active". This means that the contract is currently valid and ongoing.
   - If neither condition is true (the end date is specified and it is in the past), it will return "Not Active". This indicates that the contract has ended.

In summary, this calculated column helps to quickly identify whether a contract is currently active or not based on its end date.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'Unit' column of the 'lkp_Unit' table. This is done using the `RELATED` function, which retrieves data from a related table based on existing relationships in the data model.

2. **Handle Missing Data**: If the related 'Unit' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression will return the text "Unknown". This is useful for identifying records where the unit information is not available.

3. **Return Valid Data**: If there is a valid related 'Unit' value (i.e., it is not blank), the expression will return that value instead.

In summary, this DAX expression effectively ensures that every record in the 'MAIN_UNIT' column either shows the corresponding unit name from the 'lkp_Unit' table or indicates "Unknown" if no unit information is available. This helps maintain clarity in reporting by clearly identifying missing data.

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

This calculated column, named 'MonthNumber', helps in organizing or analyzing data by month, making it easier to perform further calculations or create reports based on monthly trends or summaries.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the employee identification numbers as strings, facilitating the association of timesheet entries with specific employees in the 'vw_Timesheet' view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to count the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

In simple business terms, this calculation achieves the following:

- It identifies all the different employees who have entries in the timesheet.
- It counts each employee only once, regardless of how many times they appear in the timesheet.
- The result gives you a clear picture of how many distinct employees have logged their time, which can be useful for understanding workforce participation, tracking employee activity, or analyzing resource utilization.

Overall, this expression helps businesses assess the diversity of their workforce in terms of employee engagement with timesheet submissions.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table captures the classification of time entries as either owned, subcontracted, or external, facilitating detailed analysis of resource allocation and project costs.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used to retrieve a value from a related table in a data model. Here's a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a calculated column within a table that has a relationship with another table. In this case, the current table is related to the `lkp_Unit` table.

2. **Purpose**: The expression fetches the value from the `OWN-Sub-ExtT` column of the `lkp_Unit` table. This means that for each row in the current table, it looks up the corresponding value from the `lkp_Unit` table based on the established relationship.

3. **Outcome**: The result is that each row in the current table will have a new column that contains the value from the `OWN-Sub-ExtT` column of the `lkp_Unit` table. This is useful for enriching the data in the current table with additional information that is stored in the related table.

In summary, this DAX expression helps to pull in specific data from another table, allowing for more comprehensive analysis and reporting by combining related information.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column contains string values that indicate the qualification status of projects associated with timesheet entries, providing insights into project categorization and compliance.
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
   - If the 'Qualify' field is not blank, it retrieves and returns the actual value from the 'Qualify' field in the 'lkp_Project' table.

**Overall Purpose**: This calculated column effectively ensures that every project has a qualification value. If a project does not have a specified qualification, it defaults to **1**. This helps maintain consistency in the data by ensuring that there are no blank values in the 'QualifyPrj' column, which can be important for analysis and reporting.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor data.

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
   - It first checks if the value of the column `[ReportReady]` is `TRUE`. This likely indicates whether a report is ready for some action or review.
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These codes probably represent specific types of timesheets or statuses.

2. **Output Values**: 
   - If either of the conditions is met (i.e., if the report is ready or if the timesheet code is "V" or "Z"), the expression returns a value of `1`. This could signify that the item is considered "ready" or "valid" for further processing.
   - If neither condition is met, it returns a value of `0`, indicating that the item is not ready or does not meet the criteria.

In summary, this DAX expression effectively flags records as either "ready" (1) or "not ready" (0) based on the readiness of the report or specific timesheet codes. This can help in filtering or analyzing data based on readiness for reporting or processing.

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

2. **Return Value if Active**: If the contract status is "Active", the measure will return the value of another measure called `[HoursDifference]`. This likely represents the difference in hours related to the contract, such as hours worked or hours remaining.

3. **Return Value if Not Active**: If the contract status is not "Active" (meaning it could be "Inactive" or any other status), the measure will return the text "Not Active".

In summary, this DAX expression helps to determine the status of a contract. If the contract is active, it provides a numerical value (the hours difference). If the contract is not active, it simply indicates that by returning the text "Not Active". This can be useful for reporting or analysis where understanding the status of contracts and their associated hours is important.

**`ContractStatusMeasure`**

- **DAX Expression:**
```dax
IF(
			    ISBLANK(MAX('vw_Timesheet'[ContractEndDate])) || MAX('vw_Timesheet'[ContractEndDate]) > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called `ContractStatusMeasure`, which determines the status of a contract based on its end date. Here's a breakdown of what it does in simple business terms:

1. **Check for Contract End Date**: The expression first looks at the maximum value of the `ContractEndDate` from the `vw_Timesheet` table. This means it is checking the latest end date of contracts that are being considered.

2. **Evaluate Conditions**:
   - **ISBLANK**: It checks if this maximum end date is blank (meaning there is no end date recorded for the contract).
   - **Comparison with Today**: It also checks if this maximum end date is greater than today's date. This means it is looking to see if the contract is still valid and has not yet expired.

3. **Determine Status**:
   - If either of the above conditions is true (the end date is blank or it is still in the future), the measure returns "Active". This indicates that the contract is currently valid and ongoing.
   - If neither condition is true (meaning there is a specific end date that has already passed), it returns "Not Active". This indicates that the contract has expired.

In summary, this measure effectively categorizes contracts as either "Active" or "Not Active" based on their end dates, helping businesses quickly assess the status of their contracts.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 10, 2023, this function will return that specific date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated starting from January 1st, and it can vary depending on the specific rules used (like whether the week starts on Sunday or Monday).

So, when you combine these two functions, `WEEKNUM(TODAY())` effectively tells you which week of the year it is right now. For instance, if today is October 10, 2023, this expression would return the week number corresponding to that date, which helps in understanding how far along we are in the year in terms of weeks. 

In a business context, this can be useful for reporting, planning, or analyzing data on a weekly basis, allowing teams to track performance or activities that are organized by week.

**`Dax_EmpCount_Approved`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[Approved] = FALSE()
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'Dax_EmpCount_Approved'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The measure calculates the number of unique employees whose timesheets have not been approved.

2. **Components**:
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the 'vw_Timesheet' table. It ensures that each employee is only counted once, even if they have multiple entries in the timesheet.
   
   - **`CALCULATE(...)`**: This function modifies the context in which the data is evaluated. It allows us to apply filters to the data being counted.

   - **`vw_Timesheet[Approved] = FALSE()`**: This filter condition specifies that we only want to consider timesheet entries where the 'Approved' status is set to FALSE. In other words, it filters the data to include only those timesheets that have not been approved.

3. **Outcome**: The final result of this measure is the total number of unique employees who have submitted timesheets that are still pending approval. This can be useful for tracking how many employees are waiting for their timesheet approvals, which can help in managing payroll and ensuring timely processing of work hours.

In summary, 'Dax_EmpCount_Approved' provides insights into the number of employees with unapproved timesheets, helping the business monitor and manage its approval process effectively.

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

3. **Filter Condition**: The expression includes a filter condition that specifies `vw_Timesheet[ReportReady] = FALSE()`. This means that only the records where the "Report Ready" status is false (i.e., the timesheets that are not ready for reporting) will be considered in the count.

In summary, this measure calculates the total number of distinct employees who have timesheets that are not yet ready for reporting. This can be useful for tracking which employees still need to finalize their timesheets before they can be processed or reported.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees recorded in the `vw_Timesheet` table. 

In simple business terms, this measure counts how many different employees have submitted timesheets, ensuring that each employee is only counted once, regardless of how many times they appear in the data. 

For example, if three employees submitted timesheets and one of them submitted multiple times, the result of this measure would still be three, reflecting the distinct count of employees rather than the total number of timesheets submitted. This is useful for understanding workforce participation or engagement in timesheet reporting.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_%_Budget_Used`**

- **DAX Expression:**
```dax
DIVIDE(SUM(vw_Timesheet[SalesAmount]), SUM('tbl_Project'[SalesPriceBaseCurrency]), 0)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of the budget that has been used based on sales data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the `vw_Timesheet` table. Essentially, it totals the revenue generated from sales.

2. **SUM('tbl_Project'[SalesPriceBaseCurrency])**: This part sums up the total budget or sales price available for the projects from the `tbl_Project` table. It represents the total amount that was planned or budgeted for sales.

3. **DIVIDE(..., ..., 0)**: The `DIVIDE` function takes the total sales amount (from the first part) and divides it by the total budget (from the second part). The third argument, `0`, specifies that if the budget is zero (to avoid division by zero), the result should be 0 instead of an error.

In simple terms, this measure calculates how much of the budget has been used by comparing actual sales to the planned budget. The result is a ratio that shows the percentage of the budget that has been utilized, helping businesses understand their performance against their financial goals.

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the timesheet. Essentially, it gives you the total revenue generated from sales during a specific period.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the timesheet. It represents the total number of hours that employees or team members have worked during the same period.

3. **Division**: The expression then divides the total sales amount by the total hours worked. This calculation results in the average hourly rate, which tells you how much revenue is generated for each hour worked.

In summary, this DAX measure calculates the average revenue earned per hour worked, providing insights into productivity and efficiency in generating sales relative to the time invested.

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

1. **Identify the Current Week**: The measure starts by determining the current week number of the year using the `SELECTEDVALUE` function. This means it looks at the data context (like a report filter) to find out which week is currently being analyzed.

2. **Find Previous Budget Value**: Next, it calculates the previous budget value by looking at all available data for weeks that are earlier than the current week. It uses the `CALCULATE` function along with `FILTER` to gather the budget data from weeks before the current one. The measure `[Msr_SalesPriceBaseCurrency]` is used here to get the sales price in the base currency for those previous weeks.

3. **Return the Appropriate Value**: Finally, the measure checks if there is a budget value available for the current week. If there is (meaning it is not blank), it returns that current budget value. If there isn’t a current budget value available, it falls back to the most recent budget value from the previous weeks that it calculated earlier.

In summary, this measure dynamically provides the budget for the current week if it exists; if not, it retrieves the last available budget from previous weeks. This approach ensures that users always have a relevant budget figure to work with, even if the current week’s data is incomplete.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the cumulative cost up to a selected month in a reporting visual. Here's a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed or filtered.

2. **Calculate Cumulative Cost**: The main goal of this measure is to calculate the total cost incurred up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to change the context in which the data is evaluated. In this case, it will sum up the costs from the `vw_Timesheet` table.

4. **Filtering the Data**: The `FILTER` function is applied to the entire 'DimDate' table (using `ALLSELECTED` to respect any other filters that might be applied). It filters the dates to include only those months that are less than or equal to the selected month. This means if you are looking at March, it will include January, February, and March in the calculation.

5. **Final Result**: The result of this measure is the total cost from the `vw_Timesheet` table for all months up to and including the month that is currently selected in the visual. This allows users to see how costs accumulate over time, providing insights into spending trends and helping with budget management.

In summary, this DAX measure helps businesses track their cumulative costs over time, making it easier to analyze financial performance month by month.

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

2. **Calculate Cumulative Sales**: The main goal of this measure is to calculate the total sales amount from the beginning of the year up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to modify the context in which the sales data is being evaluated. It allows us to change the filter context to include only the relevant months.

4. **Filtering the Date Table**: The `FILTER` function is applied to the entire date table (`ALLSELECTED('DimDate')`). This means it considers all the months that are currently selected in the report, but it only includes those months that are less than or equal to the selected month. Essentially, it gathers all the months leading up to the selected month.

5. **Summing Sales Amount**: Finally, the `SUM` function adds up the sales amounts from the 'vw_Timesheet' table for all the months that meet the filtering criteria.

In summary, this DAX measure calculates the total sales from the start of the year up to the month that the user has selected in the visual. This is useful for understanding how sales are accumulating over time and helps in tracking performance against targets or previous periods.

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
   - The variable `TotalValue` is created to store the total sum of hours from the `vw_Timesheet` table. 
   - The `CALCULATE` function is used to change the context of the calculation. It sums up the `Hours` column while ignoring any filters applied to the `GroupCat`, `Project`, and `EmployeeName` fields. This means it calculates the total hours across all groups, projects, and employees that are currently selected in the report.

2. **Percentage Calculation**:
   - The `RETURN` statement uses the `DIVIDE` function to calculate the percentage of hours for the current context (the specific group, project, or employee being analyzed).
   - It takes the sum of hours for the current context (using `SUM(vw_Timesheet[Hours])`) and divides it by the `TotalValue` calculated earlier.
   - If `TotalValue` is zero (which would mean no hours were recorded), the `DIVIDE` function will return 0 instead of causing an error.

In summary, this measure provides insight into how much of the total hours worked is attributed to a specific category (like a project or employee), expressed as a percentage. This can help in understanding workload distribution and performance across different segments of the organization.

**`MSR_PROJECT_%_REALIZATION`**

- **DAX Expression:**
```dax
IF(sum(tbl_Project[SalesPriceBaseCurrency]) <> 0, sum(vw_Timesheet[SalesAmount])/sum(tbl_Project[SalesPriceBaseCurrency]))
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called 'MSR_PROJECT_%_REALIZATION', which essentially assesses how much of the project's potential revenue has been realized based on actual sales.

Here's a breakdown of what it does:

1. **Condition Check**: The expression starts with an `IF` statement that checks if the total sales price (in the base currency) from the `tbl_Project` table is not equal to zero. This is important because if the total sales price is zero, it would not make sense to calculate a percentage, as you cannot divide by zero.

2. **Calculating Realization Percentage**: If the total sales price is not zero, the expression proceeds to calculate the percentage of realization. It does this by taking the total sales amount from the `vw_Timesheet` table and dividing it by the total sales price from the `tbl_Project` table.

   - **Numerator**: `sum(vw_Timesheet[SalesAmount])` represents the actual sales that have been realized from the project.
   - **Denominator**: `sum(tbl_Project[SalesPriceBaseCurrency])` represents the total potential revenue that could be generated from the project.

3. **Result**: The result of this calculation is a percentage that indicates how much of the expected revenue (sales price) has actually been realized (sales amount). If the total sales price is zero, the expression will return a blank (or no value), avoiding any errors from division by zero.

In summary, this DAX measure helps businesses understand the effectiveness of their projects by showing the percentage of expected revenue that has been successfully converted into actual sales.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the profit margin for a project by subtracting the total costs from the total sales. Here’s a breakdown of what it does:

1. **Total Sales Calculation**: The expression starts with `sum(vw_Timesheet[SalesAmount])`, which adds up all the sales amounts recorded in the `vw_Timesheet` table. This gives you the total revenue generated from the project.

2. **Total Costs Calculation**: Next, it calculates the total costs with `sum(vw_Timesheet[CostAmount])`, which sums up all the costs associated with the project as recorded in the same table.

3. **Profit Margin Calculation**: Finally, the expression subtracts the total costs from the total sales: 
   - `sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`. 
   This result represents the project's profit margin, indicating how much money is left after covering the costs.

In simple terms, this DAX measure tells you how profitable a project is by showing the difference between what you earned (sales) and what you spent (costs). A positive result indicates a profit, while a negative result indicates a loss.

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

1. **SUMMARIZE Function**: The expression starts by creating a summary table from the `vw_Timesheet` data. This summary groups the data by two key dimensions: `ProjectProfile` and `Client`. Essentially, it organizes the timesheet data so that we can look at sales prices for each unique combination of project and client.

2. **SELECTEDVALUE Function**: Within this summary, it retrieves the sales price in the base currency for each project from the `tbl_Project` table. The `SELECTEDVALUE` function is used here to get the specific sales price associated with the current context of the project being evaluated. If there’s only one sales price for that project, it takes that value; if there are multiple, it returns blank.

3. **MeasureValue**: The result of the `SELECTEDVALUE` function is named "MeasureValue" in the summary table. This means that for each unique project and client combination, we now have a corresponding sales price in the base currency.

4. **SUMX Function**: Finally, the `SUMX` function iterates over the summarized table created in the previous step. It adds up all the "MeasureValue" entries, which represent the sales prices for each project and client combination.

In summary, this DAX expression calculates the total sales price in the base currency for all projects and clients listed in the timesheet, providing a clear view of sales performance across different projects and clients.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of unique employees who have a "Valid" contract status on a specific day. Here's a breakdown of what each part does:

1. **CALCULATE**: This function changes the context in which data is evaluated. It allows us to apply filters to our calculations.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it ensures that each employee is only counted once, even if they appear multiple times in the data.

3. **ALL(vw_Timesheet)**: This function removes any existing filters on the `vw_Timesheet` table. By doing this, it ensures that the count of employees is not affected by any other filters that might be applied in the report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This is a filter condition that specifies we only want to consider employees whose contract status is "Valid" on the day in question.

In summary, this DAX expression calculates the total number of unique employees with a valid contract status, ignoring any other filters that might be in place. This is useful for understanding how many employees are currently eligible or active based on their contract status.

**`TotalActualHours`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression `SUM(vw_Timesheet[Hours])` is used to calculate the total number of hours recorded in a timesheet. 

Here's a breakdown of what it does:

- **SUM**: This function adds up all the values in a specified column.
- **vw_Timesheet**: This refers to a table (or view) that contains timesheet data, which includes information about hours worked.
- **[Hours]**: This is the specific column within the `vw_Timesheet` table that contains the number of hours worked by employees.

In simple terms, this measure totals all the hours logged in the timesheet, providing a single number that represents the total actual hours worked. This can be useful for reporting, payroll calculations, or analyzing employee productivity.

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

2. **MAX(vw_Timesheet[HoursPerWeek])**: For each employee in the unique list, this function finds the maximum number of hours that are recorded for that employee in the `HoursPerWeek` column. This means if an employee has multiple entries for hours worked in a week, it will take the highest value.

3. **SUMX(...)**: The `SUMX` function then takes the results from the previous steps and sums them up. It goes through each unique employee, retrieves their maximum hours per week, and adds all those maximum values together.

In summary, this DAX measure calculates the total maximum contracted hours per week for all employees, ensuring that if an employee has multiple records, only their highest recorded hours are counted. This gives a clear picture of the total contracted hours across the workforce based on the highest weekly hours each employee is contracted for.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression `[TotalActualHours] - [TotalContractedHours]` calculates the difference between two measures: **Total Actual Hours** and **Total Contracted Hours**.

In simple business terms, here's what it achieves:

- **Total Actual Hours** represents the total number of hours that have actually been worked or logged by employees on a project or task.
- **Total Contracted Hours** refers to the total number of hours that were agreed upon in a contract for the same project or task.

By subtracting the Total Contracted Hours from the Total Actual Hours, this measure provides insight into how many hours have been exceeded or are still available compared to what was originally planned or contracted. 

- If the result is positive, it indicates that more hours have been worked than were contracted, which could suggest potential overages or additional work.
- If the result is negative, it means that fewer hours have been worked than were contracted, indicating that the project is under budget in terms of hours.

Overall, this measure helps in assessing project performance and resource utilization against contractual agreements.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total hours tracked from a timesheet, specifically from a table called `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The expression first checks if the total sum of hours (`SUM(vw_Timesheet[Hours])`) is blank. This could happen if there are no entries in the timesheet or if all the hours recorded are zero.

2. **Return Zero if Blank**: If the sum of hours is indeed blank (meaning there are no hours tracked), the expression returns a value of `0`. This ensures that when there are no hours recorded, the measure will not show a blank value, which could be confusing in reports.

3. **Return Total Hours if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the expression simply returns the total sum of hours tracked.

In summary, this DAX measure ensures that when calculating total hours tracked, it will either show the actual total hours or return zero if no hours have been recorded, providing a clear and consistent output for reporting purposes.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total hours tracked from a timesheet, but it includes a check to handle situations where no hours have been recorded.

Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the `Hours` column of the `vw_Timesheet` table.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (meaning there are no hours recorded).

3. **IF(...)**: This function evaluates the condition provided by ISBLANK. If the total hours are blank (i.e., there are no hours tracked), it returns the text "Empty". If there are hours recorded, it returns the total sum of those hours.

In simple terms, this measure checks if any hours have been logged in the timesheet. If no hours are logged, it indicates that by returning "Empty". If there are hours logged, it provides the total number of hours tracked. This helps users quickly understand whether there is data available or if they need to input hours.

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

2. **Maximum Hours Per Week**: The second part of the expression uses the `CALCULATE` function to find the maximum number of hours worked in a week by an employee. It does this by looking at the `vw_Timesheet` table, which contains records of hours worked. The `MAXX` function is used to evaluate the maximum value of `HoursPerWeek` for distinct entries (to avoid counting the same week multiple times).

3. **Filtering by Employee**: The `ALLEXCEPT` function is used to ensure that the calculation of maximum hours is done only for the specific employee (or group of employees) identified by `EmployeeID`. This means that even if there are multiple employees in the dataset, the calculation will focus only on the hours for the employee in question.

4. **Calculating the Difference**: Finally, the expression subtracts the maximum hours worked in a week (calculated in the previous step) from the total hours tracked. This gives you the difference between the total hours an employee has tracked and the highest number of hours they worked in any single week.

### Summary:
In summary, this DAX measure calculates how many more hours an employee has tracked in total compared to their maximum hours worked in any single week. This can help identify if an employee is consistently working more hours than their peak weekly performance, which could be useful for workload management or performance evaluation.

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
- **DAX Explanation (Generated):** This DAX expression calculates the difference between two values related to hours tracked for employees. Here's a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been recorded or tracked for a specific employee or group of employees.

2. **Calculating Maximum Hours Per Week**: The expression then calculates the maximum number of hours worked in a week by an employee. It does this by:
   - Using `MAXX` to find the highest value of `HoursPerWeek` from a distinct list of hours recorded in the `vw_Timesheet` table. This means it looks at all the unique weekly hour entries and picks the maximum one.
   - The `CALCULATE` function modifies the context in which this maximum is calculated. It removes any filters that might be applied to `HoursPerWeek`, ensuring that the calculation considers all possible weekly hours, regardless of any specific filtering.

3. **Keeping Employee Context**: The `ALLEXCEPT` function is used to maintain the context of the calculation for each individual employee (identified by `EmployeeID`). This means that while it removes filters on `HoursPerWeek`, it still respects the grouping by each employee, ensuring that the maximum hours per week are calculated specifically for each employee.

4. **Final Calculation**: Finally, the expression subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. This gives the result of how many hours tracked exceed the maximum weekly hours recorded for that employee.

In summary, this measure calculates how many hours an employee has tracked beyond their maximum recorded hours in a week, helping to identify any overages or discrepancies in their time tracking.

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

1. **Variable Definition (FilteredHours)**: The expression starts by creating a variable called `FilteredHours`. This variable holds a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**: The filter applied to `vw_Timesheet` includes two main conditions:
   - The project qualifies as billable if `vw_Timesheet[QualifyPrj]` equals 1 (indicating it is a qualifying project) and `vw_Timesheet[BillablePrj]` equals 1 (indicating it is a billable project).
   - Alternatively, it includes any projects that contain the phrase "Customer Success Services" in their name. This is checked using the `SEARCH` function, which looks for that specific text within the `vw_Timesheet[Project]` field.

3. **Calculating Total Hours**: After filtering the data based on the above criteria, the expression uses the `SUMX` function to sum up the `Hours` from the filtered results. `SUMX` iterates over each row in the `FilteredHours` variable and adds up the values in the `vw_Timesheet[Hours]` column.

4. **Return Value**: Finally, the result of the `SUMX` calculation is returned, which represents the total number of billable hours that meet the specified criteria.

In summary, this DAX measure calculates the total hours worked on projects that are either explicitly marked as billable or are related to "Customer Success Services," providing a clear view of billable time for reporting or analysis purposes.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** This DAX expression is used to calculate the total number of hours worked on specific projects that qualify under certain criteria. Here’s a breakdown of what it does:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the values in the "Hours" column from the "vw_Timesheet" table. Essentially, it totals the hours recorded.

2. **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to only include rows where the "QualifyPrj" column has a value of 1. This means that it only considers projects that meet a certain qualification or criteria.

3. **CALCULATE**: This function modifies the context in which the data is evaluated. It takes the sum of hours and applies the filter specified (only including projects where "QualifyPrj" equals 1).

In simple terms, this DAX measure calculates the total hours worked on projects that are marked as qualifying (where "QualifyPrj" is 1). It helps businesses understand how many hours are being spent on projects that meet specific criteria, which can be useful for reporting and decision-making.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'UTILIZATION_New', which represents the Billable Hour Ratio for active employees in a business context. Here's a breakdown of what it does in simple terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable retrieves the total number of billable hours worked by employees, which is stored in another measure called `[UTI_TotalBillableHours]`.
   - `_TotalHours`: This variable retrieves the total hours worked by employees, stored in another measure called `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The code checks if there are any active contacts (employees) using `[Msr_ActiveContacts]`. If there are no active contacts, the calculation will not proceed.
   - If there are active contacts, it then checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the calculation will not proceed further.
   - If both conditions are met (active contacts exist and total hours are available), it calculates the Billable Hour Ratio by dividing the total billable hours by the total hours worked.

3. **Handling Division**:
   - The `DIVIDE` function is used to perform the division. This function is preferred because it safely handles cases where the denominator (total hours) might be zero or blank. If the result of the division is blank (which can happen if total hours are zero), it returns 0 instead.

**In Summary**: This measure calculates the ratio of billable hours to total hours for active employees, providing insight into how effectively employees are utilizing their time for billable work. If there are no active employees or if total hours are not available, it ensures that the measure returns a meaningful result (either a ratio or zero) without causing errors.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

The following filters are applied to the entire report:

- Filter on Unknown Target (Type: N/A, Definition: `N/A`)

### <a name="report-pages"></a>Report Pages

#### <a name="page-customer-analysis"></a>Page: Customer Analysis

*Internal Name: `ecc7667875c00809763b`, Ordinal: 0*

##### Visuals on this Page

**image**

- Type: `image`
- Name: `6f358e4969013934154d`

**Year**

- Type: `slicer`
- Name: `e2414703060e087d3230`

**TimeSpan**

- Type: `slicer`
- Name: `a60364499b7915da7e60`

**Unit**

- Type: `slicer`
- Name: `be723b7e75b4661912d0`

**SubUnit**

- Type: `slicer`
- Name: `e4e04919d0548163c0a8`

**Client**

- Type: `slicer`
- Name: `8048e854158dc07422ce`

**Sub-Own-Ext**

- Type: `slicer`
- Name: `800e477135b21a02300b`

**actionButton**

- Type: `actionButton`
- Name: `88e87e1eddb866e15c8a`

**Project Type**

- Type: `slicer`
- Name: `b37c531703b3e683879e`

**Company**

- Type: `slicer`
- Name: `6e846506dec1641cb274`

**Project Name**

- Type: `slicer`
- Name: `9285b49f5742861eada8`

**Employee**

- Type: `slicer`
- Name: `b5b5047cc00ba23030e5`

**pivotTable**

- Type: `pivotTable`
- Name: `8a02ec339e5cc2bce97e`

**Revenue/Week**

- Type: `lineChart`
- Name: `f5560d7670207c1c0572`

**pivotTable**

- Type: `pivotTable`
- Name: `4856952e0903150da5c7`


---

#### <a name="page-project-details"></a>Page: Project Details

*Internal Name: `2f52ad4a004b09107900`, Ordinal: 1*

##### Visuals on this Page

**image**

- Type: `image`
- Name: `3e3f0339e8619259ebb2`

**Year**

- Type: `slicer`
- Name: `c58305c304407c75000a`

**TimeSpan**

- Type: `slicer`
- Name: `30ac69436bb17c560e22`

**Pillar**

- Type: `slicer`
- Name: `1218b3e663e35022d9d5`

**Client**

- Type: `slicer`
- Name: `91d269b51621b58cd5b8`

**Company**

- Type: `slicer`
- Name: `bb8c1f8dd224d829d32b`

**Status**

- Type: `slicer`
- Name: `331fc4a3b6d31c038b30`

**Sub-Own-Ext**

- Type: `slicer`
- Name: `f4a2a804d812cd7dee49`

**Project Type**

- Type: `slicer`
- Name: `c275d85b5800b1d47638`

**Project Name**

- Type: `slicer`
- Name: `1221310b511e5b428280`

**Project Leader**

- Type: `slicer`
- Name: `9402ca390dab8c29e0ca`

**Employee**

- Type: `slicer`
- Name: `7e3b4de92424e0cc2bde`

**actionButton**

- Type: `actionButton`
- Name: `f3f386d06e37473759a7`

**pivotTable**

- Type: `pivotTable`
- Name: `0891520feb1b0937cd30`

**Actual vs Budget**

- Type: `lineChart`
- Name: `76b4488a85c8253bac24`


---

