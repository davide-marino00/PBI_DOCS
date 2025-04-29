# Power BI Model & Report Documentation

*Generated on: 2025-04-29 12:08:36*

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

The 'DimDate' table indicates that the model supports time-based analysis, allowing users to evaluate trends over specific periods. Relationships between the timesheet views and the lookup tables enable detailed insights into project allocation, resource utilization, and potential discrepancies in timesheet submissions. Overall, this data model is structured to empower stakeholders with the ability to track project progress, analyze employee productivity, and ensure accountability in time reporting, ultimately supporting better decision-making and operational efficiency within the organization.

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

The 'DimDate' table serves as a comprehensive date dimension, providing essential temporal attributes that enable businesses to analyze trends and performance over time. By offering detailed breakdowns of dates into various formats, such as day, week, month, and quarter, this table supports effective time-based reporting and enhances the ability to perform time-series analysis across different business metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) associated with each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting within the dataset. |
| `DateKey` | `int64` | Column Description: The 'DateKey' column (int64) serves as a unique identifier for each date in the 'DimDate' table, facilitating efficient date-based queries and data enrichment processes. |
| `DayName` | `string` | The 'DayName' column in the 'DimDate' table provides the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | Column Description: The 'DayNumberOfMonth' column represents the numerical day of the month (ranging from 1 to 31) for each date entry in the 'DimDate' table, facilitating date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical value of the day of the week, where 1 corresponds to Sunday and 7 corresponds to Saturday, facilitating date-related analyses and reporting. |
| `DayNumberOfYear` | `int64` | Column Description: The 'DayNumberOfYear' column represents the sequential day of the year (ranging from 1 to 365 or 366) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `MonthName` | `string` | The 'MonthName' column in the 'DimDate' table stores the full names of the months, facilitating easy reference and reporting for date-related analyses. |
| `MonthNumberOfYear` | `int64` | Column Description: Represents the numerical value of the month (1 for January through 12 for December) to facilitate date-related analyses and reporting within the DimDate table. |
| `ShortDate` | `dateTime` | Column Description: The 'ShortDate' column captures the date in a concise format, facilitating easy reference and analysis of date-related data within the DimDate table. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Column Description: Indicates whether the date corresponds to the current calendar year, with values typically set to 'Yes' or 'No'.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `CurrentYearFlag` in a data model, typically within a tool like Power BI. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The `CurrentYearFlag` column is designed to indicate whether a given date in the `DimDate` table falls within the current year or not.

2. **How It Works**:
   - The function `YEAR(DimDate[ShortDate])` extracts the year from the date in the `ShortDate` column of the `DimDate` table.
   - The function `YEAR(TODAY())` gets the current year based on today's date.
   - The `IF` statement checks if the year extracted from `DimDate[ShortDate]` is less than or equal to the current year.

3. **Output**:
   - If the year from `DimDate[ShortDate]` is less than or equal to the current year, the expression returns `1`. This means that the date is either in the current year or a previous year.
   - If the year is greater than the current year, it returns `0`, indicating that the date is in a future year.

4. **Result**: 
   - The `CurrentYearFlag` column will have a value of `1` for all dates that are in the current year or any previous year, and a value of `0` for dates that are in the future. 

In summary, this DAX expression helps to easily identify and flag dates that are either in the current year or earlier, which can be useful for analysis, reporting, or filtering data based on time.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table captures and categorizes various metrics related to hours worked and their corresponding percentage contributions, enabling businesses to analyze workforce productivity and resource allocation effectively. This table serves as a critical tool for performance evaluation and strategic planning by providing insights into labor utilization across different projects or departments.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column stores string representations of hours worked alongside their corresponding percentage contributions to total hours, facilitating performance analysis and resource allocation. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and their corresponding percentage contributions, facilitating data enrichment and analysis within the 'Hours_nd_Percentage' table. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, facilitating data consistency and integrity. It includes a unique identifier for each code, a qualification metric to indicate its relevance or applicability, and a sorting parameter to organize the codes for efficient retrieval and reporting.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers used to categorize and enrich data within the lookup table, facilitating efficient data retrieval and integration. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of data entries, serving as a numeric flag to enhance data enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table specifies the order in which records should be arranged, facilitating efficient data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference dataset for employee information, providing essential details such as EmployeeId and EmployeeName to facilitate filtering and reporting across various business analytics and performance metrics in Power BI. This table enhances data integrity and enables efficient data retrieval for employee-related insights.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column (string) uniquely identifies each employee in the 'lkp_fltr_Employee' table, facilitating data enrichment and integration processes. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, facilitating the enrichment of data by providing identifiable references for each individual in the employee lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, linking project identifiers with their respective qualification and billing metrics, while categorizing them into groups. This table is essential for analyzing project performance and financial tracking within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing amount associated with each project, facilitating financial analysis and reporting. |
| `Group` | `string` | The 'Group' column in the 'lkp_Project' table categorizes projects into distinct classifications for enhanced data organization and retrieval. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the organization. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of each project, serving as a numerical identifier for categorizing projects based on their eligibility or compliance criteria. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their characteristics such as billable status and external affiliations. This table is essential for analyzing resource allocation and financial performance across different units, enabling informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | Column Description: The 'BillableDep' column (int64) indicates the department responsible for billable activities associated with each unit, facilitating accurate financial tracking and reporting. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column identifies the specific organizational unit within the company, facilitating data enrichment and analysis related to departmental structures and functions. |
| `OWN-Sub-ExtT` | `string` | Column Description: This column stores external identifiers for ownership sub-units, facilitating data enrichment and integration within the lkp_Unit table. |
| `Unit` | `string` | The 'Unit' column stores the names of measurement units used for data enrichment, facilitating standardized interpretation and analysis across various datasets. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, status, leadership, and financial aspects. This table enables effective project management and oversight by providing stakeholders with insights into project progress, categorization, and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | Column Description: The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and performance analysis. |
| `Administration` | `string` | The 'Administration' column in the 'tbl_Project' table captures the administrative details and oversight responsibilities associated with each project, facilitating effective management and governance. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates and changes in the project lifecycle. |
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each project, facilitating the identification and management of client relationships within the project database. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating efficient data retrieval and management within the tbl_Project table. |
| `Created_on` | `dateTime` | Column Description: The 'Created_on' column records the date and time when each project entry was initiated, providing a timestamp for tracking project creation and timeline management. |
| `Currency` | `string` | The 'Currency' column in the 'tbl_Project' table specifies the type of currency used for financial transactions related to each project, facilitating accurate budgeting and reporting. |
| `DateCreated` | `dateTime` | Column Description: The 'DateCreated' column records the timestamp of when each project entry was created, providing a reference for tracking project initiation and historical data analysis. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or category of the project within the Devoteam framework, facilitating targeted analysis and reporting. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column in the 'tbl_Project' table indicates the scheduled date and time for the completion of each project, facilitating effective project timeline management and planning. |
| `Project` | `string` | The 'Project' column stores the names of various projects, enabling the identification and categorization of data related to specific initiatives within the tbl_Project table. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed narrative of the project's objectives, scope, and key deliverables, providing essential context for stakeholders and team members. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the category or classification of a project within the 'tbl_Project' table, facilitating organization and management of related projects. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a detailed textual overview of the project group, facilitating better understanding and categorization of related projects within the 'tbl_Project' table. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for data enrichment and management processes. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the project's execution and management within the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency amount for the sales price of a project, stored as a decimal to ensure precision in financial calculations. |
| `Startdate` | `dateTime` | The 'Startdate' column in the 'tbl_Project' table captures the date and time when a project is initiated, serving as a critical reference point for project timelines and scheduling. |
| `Status` | `string` | The 'Status' column in the 'tbl_Project' table indicates the current progress or state of each project, helping stakeholders assess project health and make informed decisions. |
| `Statuscode` | `string` | The 'Statuscode' column in the 'tbl_Project' table indicates the current operational status of each project, facilitating effective tracking and management of project progress. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with each project, expressed as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table serves as a critical resource for identifying employees who have not submitted their timesheets within a specified timeframe, enabling managers to address compliance issues and ensure accurate payroll processing. By tracking relevant details such as employee and manager information, contract dates, and hours, this table facilitates timely interventions to enhance workforce accountability and operational efficiency.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by employees that have not been recorded in their timesheets, facilitating accurate tracking and reporting of labor hours. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to conclude, providing essential information for tracking contract durations within the context of missing timesheet records. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours agreed upon in the contract for each employee, facilitating the identification of discrepancies in timesheet submissions. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of contracts for employees who have missing timesheets, providing essential context for identifying potential compliance issues. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on outstanding time reporting issues. |
| `Hours_` | `double` | The 'Hours_' column represents the total number of hours recorded for each employee's timesheet entry, stored as a double to accommodate fractional hours for precise tracking. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the employee's timesheet entries, facilitating accountability and tracking within the 'vw_missing_timesheet' table. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's manager, providing essential context for identifying oversight and accountability in timesheet submissions. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee time tracking. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the missing timesheet records, facilitating the tracking and management of time allocation across various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project, facilitating the tracking and management of timesheet entries associated with specific projects in the 'vw_missing_timesheet' table. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true signifying rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet data is complete and ready for reporting, with a value of true signifying readiness and false indicating that further action is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of tracking and analyzing missing timesheet entries for accurate financial reporting. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double to accommodate precise pricing information within the context of timesheet data analysis. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date and time when a timesheet entry is missing, facilitating timely follow-up and data enrichment processes. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific organizational subunit associated with each entry in the missing timesheet records, facilitating targeted follow-up and resolution of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' table. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a textual summary of the tasks or activities recorded in the timesheet, facilitating better understanding and analysis of employee work hours within the 'vw_missing_timesheet' table. |
| `Week_` | `int64` | The 'Week_' column (int64) in the 'vw_missing_timesheet' table represents the numerical week of the year associated with each timesheet entry, facilitating the identification of missing submissions by week. |
| `Year_` | `int64` | The 'Year_' column (int64) in the 'vw_missing_timesheet' table represents the calendar year associated with each timesheet entry, facilitating the analysis of timesheet data over time. |

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the `BillableDep` column from the `lkp_Unit` table. This is done using the `RELATED` function, which pulls in data from a related table based on the relationships defined in your data model.

2. **Handle Blank Values**: 
   - If the `BillableDep` value from the related table is blank (meaning there is no corresponding value), the formula returns `1`. This could indicate that if there is no specific billable dependency, it defaults to a value of `1`.

3. **Check for Non-Zero Values**: 
   - If the `BillableDep` value is not blank, the formula then checks if this value is not equal to zero. 
   - If it is not zero, it again returns `1`, suggesting that there is a valid billable dependency present.
   - If the value is zero, it returns `0`, indicating that there is no billable dependency.

### Summary:
In summary, this DAX expression is designed to categorize the `BillableDep` status into two states:
- It returns `1` if there is either no related `BillableDep` value or if the related value is non-zero (indicating a valid billable dependency).
- It returns `0` only when the related `BillableDep` value is specifically zero.

This calculated column helps in identifying whether a unit has a billable dependency or not, which can be useful for reporting and analysis in a business context.

**`CC_ActiveEmployees`** (`string`)

- **Description:** Column Description: This column indicates the status of active employees, represented as a string, within the context of identifying missing timesheet entries in the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
IF(vw_missing_timesheet[ShortDate] >= vw_missing_timesheet[ContractStartDate] && 
			      (vw_missing_timesheet[ShortDate] <= vw_missing_timesheet[ContractEndDate] || ISBLANK(vw_missing_timesheet[ContractEndDate]))
			        , 1
			        , 0
			    )
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `CC_ActiveEmployees` in a data model, specifically within a table called `vw_missing_timesheet`. Here's a breakdown of what this code does in simple business terms:

1. **Purpose**: The calculated column is designed to determine whether an employee is currently active based on their contract dates.

2. **Conditions Checked**:
   - **Start Date Check**: The code first checks if the date in the column `ShortDate` (which likely represents a specific date being evaluated) is on or after the employee's `ContractStartDate`. This means the employee's contract must have started before or on this date for them to be considered active.
   - **End Date Check**: Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. This means the employee's contract must still be valid on this date. However, if the `ContractEndDate` is blank (meaning the employee has no defined end date), the employee is still considered active.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the end date is blank), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns a value of `0`, indicating that the employee is not active.

### Summary:
In summary, this DAX expression effectively flags employees as "active" (1) or "not active" (0) based on whether the date being evaluated falls within their contract period. This is useful for tracking employee status and ensuring that only currently active employees are considered in analyses or reports.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to assist in identifying and addressing missing or insufficient data in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `IncompleteFlag`. Here's a simple explanation of what it does:

- **Purpose**: This calculated column is designed to flag records based on the value of `MissingHours` in the `vw_missing_timesheet` table.

- **Logic**: The expression checks if the value in the `MissingHours` column is less than 0. 

  - If `MissingHours` is less than 0, it returns a value of **1**. This indicates that there is an issue or a specific condition met (in this case, it could mean that there are negative hours, which might signify an incomplete or erroneous entry).
  
  - If `MissingHours` is 0 or greater, it returns a value of **0**. This indicates that there is no issue with the hours recorded.

- **Outcome**: The result is a new column (`IncompleteFlag`) that will have a value of **1** for any record where `MissingHours` is negative, and **0** for all other records. This flag can be useful for quickly identifying and filtering records that may need further attention or correction.

In summary, this DAX expression helps to identify incomplete or problematic entries in the timesheet data by marking them with a flag.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit associated with each entry in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet submissions across different departments.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the 'lkp_Unit' table for the current row. Specifically, it looks for the 'Unit' column in that related table.

2. **Handle Missing Data**: 
   - If there is no related value (meaning the 'Unit' is blank or missing), the code will return the text "Unknown". This is useful for identifying records where the unit information is not available.
   - If there is a related value (meaning the 'Unit' is present), it will return that value from the 'lkp_Unit[Unit]' column.

3. **Purpose**: The overall purpose of this calculated column is to ensure that every record has a clear indication of its unit. If the unit is missing, it provides a default label ("Unknown") instead of leaving it blank. This helps in maintaining data integrity and makes it easier to analyze or report on the data without encountering gaps.

In summary, this DAX expression ensures that every entry in the 'MAIN_UNIT' column either shows the actual unit name or indicates that the unit is unknown, thereby improving clarity and usability of the data.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column (string) in the 'vw_missing_timesheet' table represents the specific year and week number of the timesheet entries, facilitating the identification and analysis of missing timesheet data by week.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'YearWeek' that combines the year and the week number of a date, specifically the date found in the column `[ContractEndDate]`. Here’s a breakdown of what it does in simple business terms:

1. **Extracting the Year**: The function `YEAR([ContractEndDate])` takes the date from the `[ContractEndDate]` column and extracts the year from it. For example, if the date is December 15, 2023, this part would return `2023`.

2. **Calculating the Week Number**: The function `WEEKNUM([ContractEndDate])` calculates which week of the year the date falls into. Continuing with our example, if December 15, 2023, is in the 50th week of the year, this function would return `50`.

3. **Formatting the Week Number**: The `FORMAT(..., "00")` part ensures that the week number is always displayed as two digits. So, if the week number is `5`, it will be formatted as `05`. This is important for consistency, especially when the week number is less than 10.

4. **Combining Year and Week Number**: The `& "-" &` part combines the year and the formatted week number into a single string, separated by a hyphen. So, if the year is `2023` and the week number is `50`, the final result would be `2023-50`.

### Summary
In summary, this DAX expression creates a new column that shows the year and week number of the contract's end date in the format "YYYY-WW". This can be useful for reporting and analysis, allowing users to easily identify and group data by year and week. For example, it helps in tracking contracts that end in the same week across different years.

##### Measures

**`Count_Negative_Consultant`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `Count_Negative_Consultant`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure is designed to count the number of unique employees (consultants) who have recorded negative hours in a timesheet. Negative hours might indicate that an employee has made an error in reporting their hours or that there are adjustments to their recorded time.

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data being counted.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the number of unique employee names in the `vw_missing_timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries with negative hours.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This filter condition specifies that only the records where the `MissingHours` are less than zero should be considered in the count. Essentially, it filters the data to include only those instances where employees have negative hours.

3. **Outcome**: The result of this measure is a single number that tells you how many different employees have reported negative hours in their timesheets. This information can be useful for identifying potential issues in time reporting, such as errors or discrepancies that need to be addressed.

In summary, `Count_Negative_Consultant` provides a count of unique consultants who have negative hours recorded, helping the business monitor and manage timesheet accuracy.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to create a measure called **CountNegativeMissingHours**. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure counts the number of unique employees who have recorded negative missing hours in the 'vw_missing_timesheet' table.

2. **Components**:
   - **CALCULATE**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data being counted.
   - **DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID])**: This part counts the number of unique Employee IDs in the 'vw_missing_timesheet' table. Essentially, it tells us how many different employees are involved.
   - **'vw_missing_timesheet'[MissingHours] < 0**: This filter condition specifies that we are only interested in records where the 'MissingHours' value is less than zero. In other words, it focuses on cases where employees have negative missing hours, which might indicate an error or a specific situation that needs attention.

3. **Outcome**: The measure ultimately provides a count of how many distinct employees have negative missing hours. This information can be useful for identifying potential issues in timesheet reporting or for understanding employee attendance patterns.

In summary, **CountNegativeMissingHours** helps a business track and analyze the number of employees with negative missing hours, which can be critical for ensuring accurate timekeeping and addressing any discrepancies in employee records.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the number of active employees who are missing timesheet hours in a specific context. Let's break it down into simpler business terms:

1. **Data Source**: The code is working with a data table called `vw_missing_timesheet`, which likely contains records of employee hours worked, including details about their contracts and whether they have submitted their timesheets.

2. **Key Variables**:
   - **EmployeeName**: The name of the employee.
   - **Year_**: The year of the record.
   - **Week_**: The week of the record.
   - **Unit**: The organizational unit the employee belongs to.
   - **SubUnit**: A more specific division within the unit.
   - **Hours_**: The actual hours the employee has worked.
   - **ContractHours**: The hours the employee is contracted to work.

3. **Calculating Missing Hours**: 
   - The code calculates "MissingHours" for each employee by taking the total hours they worked (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum hours they are contracted to work (`MAX(vw_missing_timesheet[ContractHours])`). If this results in a negative number, it indicates that the employee has not submitted enough hours.

4. **Identifying Active Employees**:
   - The code also checks for "MinActiveEmp", which is the minimum number of active employees recorded in the data for that specific context (like a week or a unit). It looks for cases where this value is equal to 1, meaning there is at least one active employee.

5. **Filtering Criteria**:
   - The `FILTER` function is used to narrow down the summarized data to only those records where:
     - There is exactly one active employee (`[MinActiveEmp] = 1`).
     - The employee has missing hours (`[MissingHours] < 0`).

6. **Final Count**:
   - Finally, the `COUNTROWS` function counts how many records meet these criteria, effectively giving the total number of active employees who have not submitted their timesheets and have missing hours.

### Summary:
In summary, this DAX measure calculates the number of active employees who have not submitted their timesheets and have a deficit in hours worked compared to their contracted hours. This information can be crucial for management to identify compliance issues with timesheet submissions and to ensure that all employees are accurately reporting their work hours.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate a measure called `Dax_MissingHours`, which focuses on identifying and summing up the negative missing hours for active employees in a specific context (like a week or year). Here’s a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. This variable filters a summarized table (`vw_missing_timesheet`) to focus on employees who are currently active. It does this by checking a specific condition: it only includes employees where the minimum count of active employees (`MinActiveEmp`) is equal to 1. This means that only those employees who are actively working are considered.

2. **Calculate Missing Hours**: For each employee, the code calculates "MissingHours" by taking the total hours they have recorded (`SUM(vw_missing_timesheet[Hours_])`) and subtracting their contracted hours (`MAX(vw_missing_timesheet[ContractHours])`). If this calculation results in a negative number, it indicates that the employee has not logged enough hours compared to what they are contracted to work.

3. **Filter for Negative Missing Hours**: The filter applied in the `FILTER` function ensures that only those employees who have negative missing hours (i.e., they have logged fewer hours than their contract requires) are included in the final calculation. 

4. **Sum Up Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered list of active employees and sums up their "MissingHours". This gives a total of all the negative hours across all active employees.

### Summary:
In summary, this DAX measure calculates the total number of hours that active employees are missing (i.e., not working enough hours compared to their contracts) for a given period. It helps the business identify potential issues with employee hours, ensuring that they can address any discrepancies in work hours logged versus expected hours.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the total expected contract hours for employees on a weekly basis. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The calculation uses a data table called `vw_missing_timesheet`, which likely contains information about employees, their contract hours, and the weeks they worked.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key pieces of information:
   - **Week**: This represents the specific week for which we are calculating contract hours.
   - **EmployeeID**: This identifies each employee.

3. **Calculating Maximum Contract Hours**: For each unique combination of week and employee, the code calculates the maximum contract hours from the `ContractHours` column. This means that if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

4. **Summing Up Contract Hours**: After summarizing the data and calculating the maximum contract hours for each employee in each week, the `SUMX` function then adds up all these maximum contract hours across all employees and weeks.

### What It Achieves:
The overall result of this DAX measure is the total expected contract hours for all employees, aggregated by week. This helps in understanding how many hours are expected to be worked based on the highest recorded contract hours for each employee in each week, providing valuable insights for workforce planning and management.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the difference between two values related to hours worked and contracted hours for a specific context, likely within a reporting or analytical framework.

Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum value of contracted hours from the `vw_missing_timesheet` table. Contracted hours refer to the total number of hours that an employee is expected to work according to their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum value of actual hours worked (or recorded) from the same table. This represents the total number of hours that the employee has actually logged or reported.

3. **Calculation**: The expression then subtracts the maximum actual hours worked from the maximum contracted hours. 

### What It Achieves:
- The result of this calculation gives you the number of hours that are "missing" or unaccounted for. In other words, it tells you how many hours an employee has not worked compared to what they were contracted to work.
- If the result is positive, it indicates that the employee has not fulfilled their contracted hours. If it is zero or negative, it suggests that the employee has either met or exceeded their contracted hours.

In summary, this measure helps organizations track discrepancies between expected and actual work hours, which can be crucial for payroll, performance management, and ensuring compliance with labor agreements.

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

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the code calculates the difference between the total missing hours and the expected contract hours. If the result is positive, it indicates that the missing hours exceed what was expected; if negative, it means that the reported hours are more than expected.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: This function divides the result from the previous step (the difference between missing hours and expected hours) by the expected contract hours. The third argument, `0`, ensures that if the expected contract hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

### What It Achieves:
The overall measure calculates the percentage of completeness of timesheets by showing how much the missing hours deviate from what is expected. A positive percentage indicates that there are more missing hours than expected, suggesting a potential issue with timesheet reporting. Conversely, a negative percentage indicates that employees are reporting more hours than expected, which could be a sign of over-reporting or mismanagement of hours.

In summary, this measure helps the business understand how well employees are completing their timesheets relative to what is expected, allowing for better management of time reporting and resource allocation.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is designed to calculate a measure called `UniqueEmployeesPerUnit`. Let's break it down step by step in simple business terms:

1. **Data Source**: The measure uses a data table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including information about whether their submissions are complete or incomplete.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by three key columns:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This is the name of the employee.
   - `MAIN_UNIT`: This likely refers to the department or unit within the organization that the employee belongs to.

   While grouping, it also creates a new column called `"IncompleteFlag"` that captures the maximum value of the `IncompleteFlag` for each combination of week, employee, and unit. The `IncompleteFlag` is presumably a binary indicator (e.g., 0 for complete and 1 for incomplete), so using `MAX` here helps to determine if there was at least one instance of incompleteness for that employee in that week.

3. **Calculating the Sum**: After summarizing the data, the `SUMX` function iterates over the summarized table. For each row in this summarized table, it sums up the values of the `IncompleteFlag`. This means it counts how many unique employee-week-unit combinations have an incomplete timesheet submission.

### What It Achieves:
In essence, this DAX measure calculates the total number of unique employees who have not submitted their timesheets completely for each unit, across different weeks. This information can be crucial for management to identify units with compliance issues regarding timesheet submissions and to take necessary actions to improve submission rates. 

In summary, `UniqueEmployeesPerUnit` provides insights into how many employees in each unit are missing complete timesheet submissions, helping the organization monitor and address potential issues in timesheet compliance.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee work hours and project allocations, enabling management to analyze labor costs, track project progress, and assess employee productivity across different financial years. By linking timesheet data with managerial and project identifiers, this table supports informed decision-making and resource planning within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column in the 'vw_Timesheet' table identifies the specific client associated with each timesheet entry, facilitating accurate billing and project tracking. |
| `Code2` | `string` | The 'Code2' column in the 'vw_Timesheet' table stores a secondary identifier code used to categorize or classify timesheet entries for enhanced data analysis and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column in the 'vw_Timesheet' table represents the date and time when an employee's contract is set to expire, providing essential information for managing workforce planning and contract renewals. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and project billing. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of contracts associated with timesheet entries, providing real-time insights into contract conditions for effective project management. |
| `ContractStatusTodayPBI` | `string` | Column Description: This column indicates the current status of contracts as of today, providing a real-time snapshot for analysis within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential temporal context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the total monetary cost associated with the recorded time entries in the 'vw_Timesheet' table, expressed as a double-precision floating-point number. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, expressed as a double to accommodate precise financial calculations. |
| `Currency` | `string` | The 'Currency' column in the 'vw_Timesheet' table specifies the type of currency used for financial transactions recorded in the timesheet, facilitating accurate monetary reporting and analysis. |
| `DebtorName` | `string` | The 'DebtorName' column contains the name of the individual or entity responsible for payment, providing essential identification for financial tracking within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_Timesheet' table uniquely identifies each employee, facilitating the tracking and management of their timesheet entries. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column in the 'vw_Timesheet' table stores the unique identifier and name of each employee, facilitating the association of timesheet entries with specific personnel for accurate tracking and reporting. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating the identification and tracking of individual work hours and contributions. |
| `Employer` | `string` | The 'Employer' column in the 'vw_Timesheet' table captures the name of the organization for which the employee is working, facilitating accurate tracking and reporting of labor costs associated with each employer. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating accurate payroll and reporting processes. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to each timesheet entry, providing insights that enhance the understanding of the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with each timesheet entry, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified period, facilitating accurate tracking of labor input for payroll and project management. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating analysis of labor allocation and productivity. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or vacation, to facilitate accurate reporting and analysis of employee time allocation. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee time entries. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries, facilitating accountability and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column in the 'vw_Timesheet' table stores the names of individuals associated with specific IPMIDs, facilitating the tracking and management of timesheet entries linked to these identifiers. |
| `JobTitle` | `string` | The 'JobTitle' column in the 'vw_Timesheet' table captures the specific role or position of an employee, providing context for their time entries and facilitating analysis of labor distribution across various job functions. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier that facilitates the linking of timesheet records to related datasets, ensuring accurate data integration and analysis. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and reporting within the organization. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, facilitating the identification of supervisory oversight for reported hours. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key associated with the manager responsible for overseeing the timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the names of managers responsible for overseeing employee timesheets, facilitating accountability and performance tracking within the organization. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries, detailing the hours allocated to specific tasks within the timesheet records. |
| `Project` | `string` | The 'Project' column in the 'vw_Timesheet' table identifies the specific project associated with each recorded time entry, enabling effective tracking and analysis of resource allocation and project performance. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with the timesheet entries, facilitating accurate tracking and reporting of time spent on various projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that identifies the specific project associated with each timesheet entry, facilitating detailed tracking and analysis of time spent on various projects. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the specific project profile associated with each timesheet entry, facilitating accurate project tracking and reporting. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into the types of work being performed. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true signifying rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet is ready for reporting, with a value of true signifying readiness and false indicating that further action is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double-precision floating-point number, within the context of timesheet data analysis. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current approval state of each timesheet entry, reflecting whether it is pending, approved, or rejected. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, enabling better resource allocation and management. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for each timesheet entry, facilitating the tracking and management of employee work hours and associated billing. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a detailed narrative of the tasks and activities recorded in the timesheet, providing context and clarity for time entries. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table represents the measurement unit associated with the recorded time entries, facilitating accurate tracking and reporting of work hours across various projects. |
| `UnitCode` | `string` | The 'UnitCode' column in the 'vw_Timesheet' table represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating accurate tracking and reporting of labor costs by department. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and data enrichment related to timesheet entries. |
| `YearWeek` | `string` | The 'YearWeek' column (string) in the 'vw_Timesheet' table represents the specific year and week number, facilitating time-based analysis and reporting of timesheet entries. |

##### Calculated Columns

**`Approved_With_V_Z`** (`string`)

- **Description:** Column Description: This column indicates the method or criteria used to approve timesheets, providing insight into the approval process within the timesheet management system.
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

2. **Conditions**: The code checks two main conditions:
   - **First Condition**: It checks if the column `[Approved]` is marked as `TRUE()`. This means that if the timesheet has been officially approved, it meets the criteria.
   - **Second Condition**: It checks if the `TimesheetCode` from the `vw_Timesheet` table is either "V" or "Z". This means that even if the timesheet is not marked as approved, it will still be considered approved if it has one of these specific codes.

3. **Output**: 
   - If either of the conditions is met (the timesheet is approved or has a code of "V" or "Z"), the calculated column will return a value of `1`. This indicates that the timesheet is approved.
   - If neither condition is met, it will return a value of `0`, indicating that the timesheet is not approved.

In summary, this DAX expression effectively flags timesheets as approved if they are either officially approved or have a specific code ("V" or "Z"). This helps in easily identifying which timesheets can be considered approved for further processing or reporting.

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column identifies the department associated with billable hours recorded in the timesheet, facilitating accurate billing and departmental cost tracking.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the `BillableDep` column from the `lkp_Unit` table. This is done using the `RELATED` function, which retrieves a value from a related table based on the current row context.

2. **Handle Blank Values**: 
   - If the `BillableDep` value from the related table is blank (meaning there is no corresponding entry), the formula returns `1`. This indicates that, in the absence of a specific billable dependency, the default assumption is that it is billable.

3. **Check for Non-Zero Values**: 
   - If the `BillableDep` value is not blank, the formula then checks if this value is not equal to zero. 
   - If it is not zero, the formula again returns `1`, indicating that there is a billable dependency present.
   - If the `BillableDep` value is zero, the formula returns `0`, indicating that there is no billable dependency.

### Summary:
In summary, this DAX expression effectively categorizes each row based on the presence and value of a related billable dependency. It assigns a value of `1` if there is either no related billable dependency or if there is a non-zero billable dependency, and it assigns a value of `0` if there is a related billable dependency that is zero. This helps in identifying which entries are considered billable based on their dependencies.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `BillablePrj` in a data model, likely related to timesheet entries or project tracking. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The `BillablePrj` column is designed to determine whether a particular timesheet entry is considered "billable" or not. A billable entry is one that can be charged to a client or project.

2. **Conditions for Billability**: The code checks three specific conditions to decide if an entry is billable:
   - **Condition 1**: If the `SalesPrice` (the amount charged for the service or work done) is greater than 0. This means that if there is a positive sales price, the entry is billable.
   - **Condition 2**: If the `ProjectProfileCode` equals 81. This suggests that there is a specific project type or category (identified by the code 81) that is always considered billable, regardless of other factors.
   - **Condition 3**: If the `Project` field contains the phrase "Customer Success Services". This indicates that any project related to customer success services is also considered billable.

3. **Output**: 
   - If any of the above conditions are met, the formula returns a value of `1`, indicating that the entry is billable.
   - If none of the conditions are met, it returns `0`, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project type, and project description. This helps in tracking which entries can be invoiced to clients, ensuring accurate billing and project management.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column stores the name of the employer associated with each timesheet entry, facilitating the identification of the organization responsible for employee hours worked.

**`Employee_WeeklyHours`** (`string`)

- **Description:** The 'Employee_WeeklyHours' column captures the total number of hours worked by each employee during the week, represented as a string for flexibility in formatting.
- **DAX Expression:**
```dax
CONCATENATE(
			    ('vw_Timesheet'[EmployeeName]),
			    " - " & ('vw_Timesheet'[HoursperWeek])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `Employee_WeeklyHours` in a data model, specifically from a table named `vw_Timesheet`. 

Here's a breakdown of what it does:

1. **Combining Information**: The code takes two pieces of information from the `vw_Timesheet` table:
   - The name of the employee (`EmployeeName`).
   - The number of hours that employee works per week (`HoursperWeek`).

2. **Formatting the Output**: It combines these two pieces of information into a single string. The format of the output will be:
   - The employee's name followed by " - " and then the number of hours they work per week.

3. **Example Output**: If an employee's name is "John Doe" and he works 40 hours per week, the resulting value in the `Employee_WeeklyHours` column would be:
   - "John Doe - 40"

### Purpose:
The purpose of this calculated column is to create a clear and concise representation of each employee's name along with their weekly working hours. This can be useful for reporting, analysis, or displaying information in dashboards, making it easier to understand how many hours each employee is scheduled to work at a glance.

**`GroupCat`** (`string`)

- **Description:** The 'GroupCat' column categorizes timesheet entries into distinct groups, facilitating streamlined reporting and analysis of time allocation across various projects or departments.
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

1. **Lookup for Group**: The code first checks if there is a corresponding 'Group' value in a lookup table (`lkp_Project`) for a specific project listed in the `vw_Timesheet` table. It does this using the `LOOKUPVALUE` function, which searches for the 'Group' based on the 'PROJECT' field.

2. **Check for Blank Values**: The `NOT(ISBLANK(...))` part checks if the result of the lookup is not blank. This means it verifies whether a valid 'Group' was found for the project.

3. **Return Group if Found**: If a valid 'Group' is found (i.e., the lookup is not blank), the code returns that 'Group' value.

4. **Determine Billable Status**: If no 'Group' is found (the lookup is blank), the code then checks if the project is billable by looking at the `BillablePrj` field in the `vw_Timesheet` table. If `BillablePrj` equals 1, it indicates that the project is billable.

5. **Return Billable or Unbillable**: If the project is billable, the code returns the string "Billable". If it is not billable (i.e., `BillablePrj` is not 1), it returns "Other Unbillable".

### Summary:
In summary, this DAX expression categorizes each project in the `vw_Timesheet` table into one of three categories:
- It assigns the project to a specific 'Group' if that information is available.
- If no 'Group' is found, it checks if the project is billable and labels it as "Billable" or "Other Unbillable" accordingly. 

This helps in organizing and analyzing projects based on their group affiliation and billing status, which is valuable for reporting and decision-making.

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

1. **Purpose**: The calculated column determines whether a contract is currently active or not.

2. **Logic**:
   - The expression checks two conditions regarding the **ContractEndDate**:
     - **ISBLANK('vw_Timesheet'[ContractEndDate])**: This checks if the **ContractEndDate** is blank (meaning there is no end date specified). If there is no end date, it implies that the contract is still ongoing.
     - **'vw_Timesheet'[ContractEndDate] > TODAY()**: This checks if the **ContractEndDate** is greater than today's date. If the end date is in the future, it means the contract is still active.

3. **Output**:
   - If either of the above conditions is true (the contract has no end date or the end date is in the future), the column will return **"Active"**.
   - If neither condition is true (meaning there is an end date that has already passed), it will return **"Not Active"**.

**In summary**, this calculated column helps users quickly identify whether a contract is currently active or has ended, based on the presence and value of the **ContractEndDate**.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column in the 'vw_Timesheet' table identifies the primary organizational unit responsible for the recorded time entries, facilitating effective resource allocation and reporting.
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

2. **Handle Missing Data**: If the related 'Unit' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the code will return the text "Unknown". This is a way to handle situations where the expected data is missing, ensuring that the result is clear and understandable.

3. **Return the Unit Name**: If there is a valid related 'Unit' value (i.e., it is not blank), the code will return that value. This means that the calculated column will show the actual unit name associated with the current row.

### Summary:
In summary, this DAX expression is designed to populate the 'MAIN_UNIT' column with either the name of the unit from the related table or "Unknown" if no unit is found. This helps maintain clarity in the data by ensuring that every row has a meaningful value, even when some data might be missing.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) associated with each timesheet entry, facilitating time-based analysis and reporting.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to create a calculated column called 'MonthNumber' in a data model, typically within tools like Power BI or Excel.

### What It Does:
- **Extracts the Month**: This expression takes a date value from the column `[TimesheetDate]` and extracts the month part of that date.
- **Returns a Number**: The result is a number between 1 and 12, where:
  - 1 represents January,
  - 2 represents February,
  - 3 represents March,
  - and so on, up to 12 for December.

### Business Context:
- **Simplifies Analysis**: By converting dates into month numbers, it allows for easier grouping and analysis of data by month. For example, you can quickly summarize timesheet data to see how many hours were logged in each month.
- **Facilitates Reporting**: This calculated column can be used in reports and dashboards to visualize trends over time, such as monthly performance or workload.

In summary, this DAX expression helps businesses analyze and report on their data by breaking down dates into a more manageable format—specifically, the month number.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the employee identification numbers as strings, facilitating the association of timesheet entries with specific employees in the 'vw_Timesheet' table.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

### What It Achieves:
- **Count of Unique Employees**: This expression counts how many different employees have entries in the timesheet. For example, if there are multiple entries for the same employee, they will only be counted once.
- **Data Analysis**: This is useful for understanding workforce participation, tracking how many distinct employees are working during a specific period, or analyzing employee engagement in projects.

### In Simple Terms:
Imagine you have a list of employees who worked on various projects, and some employees worked on multiple projects. This DAX expression helps you find out how many individual employees contributed, regardless of how many times they appear in the list. It gives you a clear picture of the workforce involved.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table categorizes the type of time entry as either owned, subcontracted, or external, facilitating accurate tracking and reporting of resource allocation.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used in a calculated column to retrieve a specific value from a related table. Here’s a breakdown of what it does in simple business terms:

1. **Context of Use**: This expression is typically used in a data model where there are two tables that have a relationship. For example, you might have a main table (let's call it "Sales") and a lookup table (let's call it "lkp_Unit").

2. **Purpose of the Expression**: The `RELATED` function is designed to fetch a value from a related table based on the current row context of the main table. In this case, it is looking for the value in the column `OWN-Sub-ExtT` from the `lkp_Unit` table.

3. **What It Achieves**: By using this expression, the calculated column will populate each row in the main table (e.g., "Sales") with the corresponding value from the `OWN-Sub-ExtT` column in the `lkp_Unit` table. This is useful for enriching the main table with additional information that is stored in the related table.

4. **Example Scenario**: Imagine you have a sales record that includes a unit identifier. The `lkp_Unit` table contains detailed information about each unit, including a column called `OWN-Sub-ExtT` that categorizes the unit in a specific way (like ownership type or external classification). By using this DAX expression, each sales record can automatically include this categorization, making it easier to analyze sales data based on unit characteristics.

In summary, this DAX expression enhances the main table by pulling in relevant data from a related table, allowing for more comprehensive analysis and reporting.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column contains string values that categorize or specify the qualification status of projects associated with each timesheet entry, aiding in project management and resource allocation.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'QualifyPrj'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column 'QualifyPrj' is designed to determine a qualification status for a project based on related data from another table called 'lkp_Project'.

2. **Logic**:
   - The expression first checks if the 'Qualify' field from the related 'lkp_Project' table is blank (i.e., it has no value).
   - If the 'Qualify' field is blank, the calculated column will return a value of **1**. This could signify a default or a specific status indicating that the project does not have a qualification assigned.
   - If the 'Qualify' field is not blank (meaning it has a value), the calculated column will return the actual value from the 'Qualify' field in the 'lkp_Project' table.

3. **Outcome**: 
   - The result of this calculation is that for each row in the table where this calculated column is being created, you will either get a **1** (if there is no qualification) or the specific qualification value from the related project (if it exists).
   - This helps in easily identifying projects that are either unqualified or have a specific qualification status, which can be useful for reporting or analysis purposes.

In summary, the 'QualifyPrj' column provides a straightforward way to assess the qualification status of projects by either assigning a default value or pulling in existing qualification data from a related table.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for submitting the timesheet entries, facilitating accurate tracking and reporting of labor hours.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of resources, formatted as a string, within the context of timesheet data for project management and resource allocation.
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

1. **Purpose**: The calculated column is designed to determine whether a certain condition is met for each row in the data. It outputs either a 1 or a 0 based on this condition.

2. **Conditions Checked**:
   - The first condition checks if the value of the column `[ReportReady]` is `TRUE`. This means that if the report is ready, the condition is satisfied.
   - The second condition checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means that if the timesheet code is one of these two specific codes, the condition is also satisfied.

3. **Output**:
   - If either of the conditions is true (i.e., the report is ready or the timesheet code is "V" or "Z"), the calculated column will return a value of `1`.
   - If neither condition is met, it will return a value of `0`.

4. **Business Implication**: This calculated column can be used to quickly identify rows where either the report is ready or the timesheet code is relevant (specifically "V" or "Z"). This can help in filtering or analyzing data based on readiness or specific timesheet codes, making it easier for decision-makers to focus on important entries.

In summary, `RReady_With_V_Z` flags rows with a `1` if the report is ready or if the timesheet code is "V" or "Z", and a `0` otherwise.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is a measure called `ContractStatus2`. Here's a simple explanation of what it does:

1. **Condition Check**: The measure first checks the value of another measure called `[ContractStatusMeasure]`. It looks to see if this value is equal to "Active".

2. **Return Value Based on Condition**:
   - If the condition is true (meaning the contract status is "Active"), the measure returns the value of another measure called `[HoursDifference]`. This likely represents the difference in hours related to the contract, such as time worked or time remaining.
   - If the condition is false (meaning the contract status is not "Active"), the measure returns the text "Not Active".

### Summary:
In summary, `ContractStatus2` is designed to provide information about the status of a contract. If the contract is active, it shows the hours difference; if not, it simply indicates that the contract is not active. This helps users quickly understand the status of contracts and the associated hours.

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
   - The measure first checks if the maximum value of the `ContractEndDate` from the `vw_Timesheet` table is blank (meaning there is no end date recorded) or if that date is greater than today’s date.
   - If either of these conditions is true (i.e., there is no end date or the end date is still in the future), the measure will return the status "Active".
   - If neither condition is true (meaning the end date is in the past), it will return the status "Not Active".

3. **Outcome**: 
   - This measure helps users quickly identify whether a contract is currently active or has ended. 
   - It provides a simple "Active" or "Not Active" label based on the contract's end date, which can be useful for reporting and decision-making regarding contract management.

In summary, `ContractStatusMeasure` effectively categorizes contracts as either "Active" or "Not Active" based on their end dates, helping businesses keep track of their contractual obligations.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 10, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated starting from the first week of the year. 

So, when you combine these two functions, `WEEKNUM(TODAY())` effectively tells you which week of the year it is right now. 

For example, if today is October 10, 2023, and it falls in the 41st week of the year, this expression will return the number 41. 

In business terms, this measure can be useful for reporting and analysis purposes, such as tracking weekly performance, sales, or any other metrics that are evaluated on a weekly basis. It helps organizations understand where they stand in the current week of the year.

**`Dax_EmpCount_Approved`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[Approved] = FALSE()
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `Dax_EmpCount_Approved`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the number of unique employees whose timesheets have not been approved.

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries.
   - **`vw_Timesheet[Approved] = FALSE()`**: This condition filters the data to include only those timesheet entries where the `Approved` status is set to `FALSE`. In other words, it focuses on timesheets that have not been approved.

3. **Outcome**: The measure ultimately provides a count of distinct employees who have submitted timesheets that are still pending approval. This information can be useful for management to understand how many employees are waiting for their timesheets to be reviewed and approved.

In summary, `Dax_EmpCount_Approved` helps track the number of unique employees with unapproved timesheets, aiding in monitoring and managing the approval process.

**`Dax_EmpCount_RReady`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[ReportReady] = FALSE()
			)
```
- **DAX Explanation (Generated):** The DAX expression you've provided is used to create a measure called `Dax_EmpCount_RReady`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the number of unique employees whose timesheets are not marked as "Report Ready."

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries in the timesheet.
   - **`vw_Timesheet[ReportReady] = FALSE()`**: This filter condition specifies that we only want to consider timesheet entries where the `ReportReady` status is set to FALSE. In other words, it filters out any entries that are marked as ready for reporting.

3. **Outcome**: The final result of this measure is the total number of distinct employees who have timesheet entries that are not ready for reporting. This can help a business identify how many employees still need to complete or finalize their timesheets before they can be reported.

In summary, `Dax_EmpCount_RReady` provides valuable insight into the number of employees who have pending timesheet submissions, helping management track and encourage timely reporting.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees recorded in the `vw_Timesheet` table.

### Breakdown of the Expression:

- **DISTINCTCOUNT**: This function counts the number of unique values in a specified column. In this case, it is counting unique entries.
  
- **vw_Timesheet[EmployeeName]**: This specifies the column from which the unique values are being counted. Here, it refers to the `EmployeeName` column in the `vw_Timesheet` table.

### What It Achieves:

In simple business terms, this measure provides the total number of different employees who have submitted timesheets. It helps organizations understand how many distinct employees are actively logging their work hours, which can be useful for workforce analysis, payroll processing, and resource management. 

For example, if the result of this measure is 50, it means that there are 50 different employees who have filled out timesheets, regardless of how many times each employee has submitted their hours.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does in simple business terms:

1. **Data Source**: The calculation uses a table called `vw_Timesheet`, which likely contains records of sales and the hours worked by employees.

2. **Sales Amount**: The expression `SUM(vw_Timesheet[SalesAmount])` adds up all the sales amounts recorded in the `SalesAmount` column of the `vw_Timesheet` table. This gives the total sales generated over a specific period.

3. **Hours Worked**: The expression `SUM(vw_Timesheet[Hours])` adds up all the hours worked recorded in the `Hours` column of the same table. This provides the total number of hours worked during that same period.

4. **Average Hourly Rate Calculation**: The entire expression divides the total sales amount by the total hours worked. This means it calculates how much revenue was generated for each hour worked, which is the average hourly rate.

### Summary:
In summary, this DAX measure calculates the average hourly rate of sales by taking the total sales generated and dividing it by the total hours worked. This helps businesses understand how effectively their time is being converted into sales revenue.

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
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called `Msr_Budget`, which is used to calculate a budget value based on sales data over time, specifically focusing on weekly performance. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: 
   - The measure starts by determining the current week number of the year using the `SELECTEDVALUE` function. This means it looks at the context in which the measure is being evaluated (like a report or dashboard) to find out which week is currently selected.

2. **Calculate Previous Budget**:
   - Next, it calculates a value called `PrevBudget`. This is done using the `CALCULATE` function, which modifies the context of the calculation. 
   - It uses the measure `[Msr_SalesPriceBaseCurrency]`, which presumably represents some form of sales or revenue in a base currency.
   - The `FILTER` function is applied to the `DimDate` table to include only those weeks that are earlier than the current week. The `ALLSELECTED` function ensures that any filters applied to the date dimension are respected, but it still allows for the calculation of previous weeks.

3. **Return the Appropriate Value**:
   - Finally, the measure uses an `IF` statement to decide what to return:
     - If the current sales measure `[Msr_SalesPriceBaseCurrency]` is not blank (meaning there is actual sales data for the current week), it returns that current sales value.
     - If the current sales measure is blank (indicating no sales data for the current week), it returns the previously calculated budget value (`PrevBudget`).

### Summary:
In summary, this measure calculates the budget for the current week based on sales data. If there are sales figures available for the current week, it uses those figures. If not, it falls back to the budget from previous weeks. This approach helps businesses understand their current performance against historical data, allowing for better decision-making and forecasting.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a cumulative cost measure, specifically named 'MSR_Cumulative Cost'. Here's a breakdown of what it does in simple business terms:

1. **Identify the Current Month**: The code starts by determining the month that is currently selected in the visual (like a chart or table). It does this by finding the maximum value of the 'MonthNumberOfYear' from the 'DimDate' table. This means if you are looking at data for March, the `SelectedMonth` would be 3.

2. **Calculate Cumulative Cost**: The main goal of this measure is to calculate the total cost incurred up to and including the selected month. 

3. **Summing Costs**: The `CALCULATE` function is used to change the context in which the data is evaluated. It sums up the 'CostAmount' from the 'vw_Timesheet' table.

4. **Filtering the Data**: The `FILTER` function is applied to ensure that only the months that are less than or equal to the `SelectedMonth` are included in the calculation. The `ALLSELECTED` function allows the measure to respect any filters that might be applied in the report, ensuring that it only considers the relevant months based on user selections.

5. **Final Outcome**: The result of this measure is the total cost from the beginning of the year up to the selected month. For example, if the selected month is March, it will sum the costs for January, February, and March.

In summary, this DAX measure calculates the total costs accumulated from the start of the year up to the month currently selected in the report, allowing users to see how costs have built up over time.

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
   - The code starts by determining which month is currently selected in the visual (like a chart or table). It uses `MAX('DimDate'[MonthNumberOfYear])` to find the highest month number that has been selected. For example, if the user has selected data up to March, `SelectedMonth` will be 3.

2. **Calculate Cumulative Sales**:
   - The `CALCULATE` function is then used to compute the total sales amount (`SUM(vw_Timesheet[SalesAmount])`). This means it will sum up all the sales figures from the `vw_Timesheet` table.

3. **Filter for Relevant Months**:
   - The `FILTER` function is applied to ensure that only the sales from the months that are less than or equal to the selected month are included in the calculation. The `ALLSELECTED('DimDate')` part ensures that the filter respects any other selections made in the report, such as year or other date filters.

4. **Final Result**:
   - The result of this measure is the total sales amount for all months up to and including the selected month. For instance, if the selected month is March, it will sum the sales from January, February, and March.

In summary, this DAX measure calculates the total sales accumulated from the beginning of the year up to the month that the user has selected, allowing for a clear view of sales performance over time.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the percentage of hours worked by a specific group, project, or employee compared to the total hours worked across all selected categories. Here’s a breakdown of what it does in simple business terms:

1. **TotalValue Calculation**:
   - The first part of the code, defined by the variable `TotalValue`, calculates the total number of hours worked. 
   - It uses the `CALCULATE` function to sum up the hours from the `vw_Timesheet` table.
   - The `ALLSELECTED` function ensures that this total is based on the current selections made in the report for the categories of Group, Project, and Employee. This means it respects any filters applied by the user but ignores any filters specifically on the `GroupCat`, `Project`, and `EmployeeName` fields.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours worked for the current context (which could be a specific group, project, or employee).
   - It does this by taking the sum of hours for the current context (using `SUM(vw_Timesheet[Hours])`) and dividing it by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safe way to handle division in DAX. If the `TotalValue` is zero (meaning no hours were recorded), it will return 0 instead of causing an error.

### Summary:
In summary, this measure calculates what percentage of the total hours worked (based on the user's current selections) is represented by the hours worked in the current context (like a specific employee or project). This helps in understanding how much of the total effort is contributed by a particular segment, making it easier to analyze performance and resource allocation.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the project margin for a specific measure called 'MSR_ProjectMargin'. Here's a breakdown of what it does in simple business terms:

1. **Sales Amount**: The expression starts by summing up all the values in the `SalesAmount` column from the `vw_Timesheet` table. This represents the total revenue generated from projects.

2. **Cost Amount**: Next, it sums up all the values in the `CostAmount` column from the same table. This represents the total costs incurred for those projects.

3. **Calculating Margin**: The expression then subtracts the total costs from the total sales. In other words, it takes the total revenue and deducts the total expenses to find out how much profit (or margin) is left over after covering the costs.

### Summary:
In essence, this DAX measure calculates the **project margin** by determining how much profit a project generates after accounting for its costs. A positive result indicates that the project is profitable, while a negative result would suggest a loss. This measure is crucial for assessing the financial performance of projects within an organization.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called `Msr_SalesPriceBaseCurrency`, which essentially sums up the sales prices in a base currency for different projects and clients. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The calculation is based on a table called `vw_Timesheet`, which likely contains records related to projects and clients.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key fields: `ProjectProfile` and `Client`. This means that the measure will look at each unique combination of project profiles and clients.

3. **Selecting Sales Price**: For each group created in the previous step, the code retrieves the sales price in the base currency from another table called `tbl_Project`. The `SELECTEDVALUE` function is used here to get the sales price for the current context (i.e., the specific project and client being evaluated).

4. **Calculating Total**: After summarizing the data and retrieving the sales prices, the `SUMX` function is used to iterate over each of these groups and sum up the values of the sales prices (referred to as `MeasureValue`).

### What It Achieves:
- **Total Sales Price Calculation**: The measure calculates the total sales price in the base currency for all projects and clients in the `vw_Timesheet` table.
- **Contextual Insight**: By grouping the data by project and client, it provides insights into how much revenue is generated from each project/client combination, which can be useful for financial analysis and reporting.

In summary, this DAX measure helps businesses understand their sales performance in a standardized currency across different projects and clients, enabling better financial decision-making and reporting.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a measure called "Static Total Employees." Let's break down what it does in simple business terms:

1. **Purpose**: The measure calculates the total number of unique employees who have a "Valid" contract status as of today.

2. **Components**:
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it ensures that each employee is only counted once, even if they appear multiple times in the data.
   
   - **`ALL(vw_Timesheet)`**: This function removes any filters that might be applied to the `vw_Timesheet` table. By doing this, the measure looks at all the data in the table, rather than just a subset that might be filtered by other criteria (like date or department).
   
   - **`vw_Timesheet[ContractStatusToday] = "Valid"`**: This condition filters the data to only include employees whose contract status is marked as "Valid" for today. It ensures that only those employees who are currently active and have valid contracts are counted.

3. **Overall Calculation**: When you put it all together, this measure counts how many unique employees have a valid contract today, ignoring any other filters that might be applied to the data. This is useful for understanding the current workforce status in terms of valid employment contracts.

In summary, the "Static Total Employees" measure provides a clear count of active employees with valid contracts, helping businesses track their workforce effectively.

**`TotalActualHours`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression `SUM(vw_Timesheet[Hours])` is used to calculate the total number of hours recorded in a timesheet. 

Here's a breakdown of what it does:

- **SUM**: This function adds up all the values in a specified column.
- **vw_Timesheet**: This refers to a table (or view) that contains timesheet data. It includes various columns related to employee hours worked, among other details.
- **[Hours]**: This is the specific column within the `vw_Timesheet` table that contains the number of hours each employee has logged.

In simple business terms, this measure calculates the total actual hours worked by all employees as recorded in the timesheet. It provides a single number that represents the sum of all hours, which can be useful for reporting, payroll, or analyzing workforce productivity.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate the total contracted hours for employees based on their weekly hours. Here’s a breakdown of what it does in simple business terms:

1. **SUMX Function**: This function is used to perform a calculation over a table and then sum the results. In this case, it will sum up values for each employee.

2. **VALUES Function**: This function retrieves a unique list of employee names from the `vw_Timesheet` table. Essentially, it creates a list of all the employees who have timesheet entries.

3. **MAX Function**: For each employee in the unique list, the `MAX` function looks at the `HoursPerWeek` column in the `vw_Timesheet` table. It finds the maximum number of hours that each employee is contracted to work in a week.

4. **Putting It All Together**: The `SUMX` function then takes the maximum hours per week for each employee (as determined by the `MAX` function) and adds them all together. 

### What It Achieves:
The overall result of this DAX expression is the total number of contracted hours for all employees. It effectively sums up the highest weekly hours that each employee is contracted to work, giving you a comprehensive view of total contracted hours across the organization. 

In summary, this measure helps businesses understand their total workforce capacity in terms of hours, which can be useful for planning, budgeting, and resource allocation.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two measures: **TotalActualHours** and **TotalContractedHours**. Here's a breakdown of what this means in simple business terms:

1. **TotalActualHours**: This measure represents the total number of hours that have actually been worked or logged by employees or resources on a project or task.

2. **TotalContractedHours**: This measure indicates the total number of hours that were agreed upon in a contract for the project or task. This is the expected or planned amount of work.

3. **The Calculation**: The expression `[TotalActualHours] - [TotalContractedHours]` subtracts the total contracted hours from the total actual hours. 

4. **What It Achieves**: 
   - If the result is positive, it means that more hours were worked than what was contracted, indicating potential overwork or additional effort beyond what was planned.
   - If the result is negative, it means that fewer hours were worked than what was contracted, suggesting that the project may be under budget in terms of hours or that tasks are not being completed as expected.
   - If the result is zero, it indicates that the actual hours worked match the contracted hours, showing that the project is on track in terms of time.

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

1. **Check for Blank Values**: The measure first checks if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Zero if Blank**: If the sum of hours is blank (meaning no hours have been tracked), the measure will return a value of **0**. This is important because it ensures that when there are no hours recorded, the measure does not show a blank value, which could be confusing in reports.

3. **Return the Sum of Hours if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the measure will return the actual total sum of hours tracked from the `vw_Timesheet` table.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is a measure named `TotalHoursTrackedMissing`. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The measure first checks if the total hours tracked (from the `vw_Timesheet` table) is blank. This means it is looking to see if there are any recorded hours for the timesheet.

2. **Return Value Based on Check**:
   - If the total hours are blank (meaning no hours have been recorded), the measure returns the text "Empty". This indicates that there are no hours tracked for the selected context (like a specific employee, project, or time period).
   - If there are hours recorded (i.e., the total is not blank), it simply returns the sum of those hours. This gives the actual total hours tracked.

### Summary:
In essence, this measure helps to identify whether there are any hours recorded in the timesheet. If there are no hours, it clearly indicates that with the word "Empty". If there are hours, it provides the total number of hours tracked. This is useful for reporting and analysis, as it helps users quickly understand if data is missing or if there are actual hours to consider.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates a measure called `trackedDiff`, which essentially determines the difference between the total hours tracked for a specific context (like an employee or a project) and the maximum hours tracked per week for that same context.

Here's a breakdown of what each part does in simple business terms:

1. **[TotalHoursTracked]**: This part represents the total number of hours that have been logged or tracked. It could be the total hours an employee has worked on tasks or projects.

2. **CALCULATE(...)**: This function modifies the context in which the data is evaluated. It allows us to perform calculations based on specific filters or conditions.

3. **MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])**: 
   - **DISTINCT(vw_Timesheet[HoursPerWeek])**: This part creates a list of unique values for hours worked per week from the timesheet data.
   - **MAXX(...)**: This function then looks at that list of unique weekly hours and finds the maximum value. Essentially, it identifies the highest number of hours that have been tracked in any single week for the context being evaluated (like a specific employee).

4. **ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])**: This function removes all filters from the `vw_Timesheet` table except for the filter on `EmployeeID`. This means that the calculation of the maximum hours per week will only consider the data for the specific employee, ignoring any other filters that might be applied.

### What It Achieves:
The overall calculation of `trackedDiff` gives you the difference between the total hours tracked for an employee and the maximum hours they have tracked in any single week. 

- If `trackedDiff` is positive, it indicates that the employee has logged more hours overall than their maximum weekly hours, which could suggest consistent overtime or additional work.
- If it is zero or negative, it means the total hours tracked are less than or equal to their maximum weekly hours, indicating that they have not exceeded their typical workload.

In summary, this measure helps in understanding how an employee's total tracked hours compare to their peak weekly performance, which can be useful for workload management and performance analysis.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is a measure named `trackedDiff2`. Let's break down what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts by referencing another measure called `[TotalHoursTracked]`. This measure likely represents the total number of hours that have been logged or tracked by employees over a certain period.

2. **Calculating the Maximum Hours Per Week**: The next part of the code calculates the maximum number of hours worked in a week by an employee. It does this using the `MAXX` function, which looks at a distinct list of hours per week from the `vw_Timesheet` table. Essentially, it finds the highest value of hours that any employee has recorded in a week.

3. **Removing Filters**: The `REMOVEFILTERS` function is used to ignore any filters that might be applied to the `HoursPerWeek` column. This means that the calculation of the maximum hours per week will consider all records, regardless of any specific filtering that might be in place.

4. **Keeping Employee Context**: The `ALLEXCEPT` function is used to maintain the context of the calculation for each individual employee. This means that while it ignores filters on `HoursPerWeek`, it still respects the filter for `EmployeeID`. So, the maximum hours per week calculated will be specific to each employee.

5. **Final Calculation**: Finally, the measure subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. 

### Summary:
In summary, the `trackedDiff2` measure calculates the difference between the total hours an employee has tracked and the maximum number of hours they have worked in any single week. This can help identify how much more or less an employee has worked compared to their peak weekly performance, providing insights into their workload and productivity.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the total billable hours from a timesheet data source, specifically focusing on certain projects that qualify for billing. Here’s a breakdown of what it does in simple business terms:

1. **Variable Definition**: The code starts by defining a variable called `FilteredHours`. This variable will hold a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**: The filtering is based on two main conditions:
   - **Qualifying Projects**: It checks if the project qualifies for billing (`vw_Timesheet[QualifyPrj] = 1`) and if it is a billable project (`vw_Timesheet[BillablePrj] = 1`). This means that only projects that meet both of these criteria will be included.
   - **Specific Project Name**: Additionally, it looks for any projects that contain the phrase "Customer Success Services" in their name. If a project matches this name, it will also be included in the filtered results.

3. **Summing Up Hours**: After applying the filters, the code uses the `SUMX` function to calculate the total hours worked on the filtered projects. `SUMX` iterates over each row in the `FilteredHours` variable and sums up the `Hours` column from the `vw_Timesheet`.

4. **Final Output**: The result of this measure, `UTI_TotalBillableHours`, is the total number of hours that can be billed to clients, based on the specified criteria. This helps the business understand how many hours are being worked on projects that are eligible for billing, which is crucial for revenue tracking and project management.

In summary, this DAX measure effectively calculates the total billable hours for specific projects that either qualify as billable or are related to "Customer Success Services," providing valuable insights into the company's billable work.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `UTI_TOTALHOURS`. Let's break it down in simple business terms:

1. **Purpose**: This measure calculates the total number of hours worked on specific projects that meet a certain qualification criteria.

2. **Components**:
   - **SUM(vw_Timesheet[Hours])**: This part of the code adds up all the values in the `Hours` column from the `vw_Timesheet` table. Essentially, it totals the hours worked.
   - **CALCULATE**: This function modifies the context in which the data is evaluated. In this case, it is used to apply a filter to the data being summed.
   - **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to include only those entries where the `QualifyPrj` column has a value of 1. This means that only projects that are marked as "qualified" (or meet certain criteria) will be included in the total hours calculation.

3. **Outcome**: The measure `UTI_TOTALHOURS` will return the total hours worked on projects that are considered qualified. This is useful for reporting and analysis, as it helps the business understand how much time is being spent on projects that meet specific standards or requirements.

In summary, this DAX measure helps organizations track and analyze the total hours worked on qualified projects, providing insights into resource allocation and project performance.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is a measure called 'UTILIZATION_New'. Let's break it down into simple business terms to understand what it calculates or achieves.

### Key Components of the Measure:

1. **Variables Defined**:
   - **_TotalBillableHours**: This variable captures the total number of hours that can be billed to clients. It is represented by another measure called `[UTI_TotalBillableHours]`.
   - **_TotalHours**: This variable captures the total number of hours worked, represented by another measure called `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The measure uses a series of conditional checks (IF statements) to determine what value to return.
   - **First Check**: It checks if there are any active contacts (`[Msr_ActiveContacts]`). If there are no active contacts, the measure will not proceed further.
   - **Second Check**: If there are active contacts, it checks if the total hours worked (`[UTI_TOTALHOURS]`) is not blank. If it is blank, the measure will not proceed further.
   - **Final Calculation**: If both previous checks are satisfied, it calculates the utilization rate by dividing the total billable hours by the total hours worked. If this division results in a blank value (which can happen if total hours are zero), it returns 0 instead of a blank.

### What It Achieves:

In summary, this measure calculates the **utilization rate** of resources (like employees or teams) based on the hours they have worked. Utilization rate is a key performance indicator in many businesses, especially in service industries, as it shows how effectively the available hours are being used for billable work.

- If there are active contacts and total hours worked are available, it provides a percentage that indicates how much of the total hours worked were billable.
- If there are no active contacts or if total hours worked is not available, it ensures that the measure returns a meaningful value (either 0 or a calculated percentage) rather than leaving it blank.

This measure helps businesses understand their efficiency in utilizing their workforce for billable tasks, which can inform decisions about resource allocation, project management, and overall productivity.

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
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`cc_Employer` (Type: Categorical, Definition: (No conditions found in Where clause))

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
  - Filter on `vw_Timesheet`.`Sum(SalesAmount)` (Type: Advanced, Definition: (Complex condition))

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
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Definition: (Complex condition))

**Actual vs Budget**

- Type: `lineChart`
- Name: `76b4488a85c8253bac24`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Actual` (Query: `vw_Timesheet.MSR_Cumulative_Revenue`) (Role: Y)
  - `Budget` (Query: `vw_Timesheet.Msr_Budget`) (Role: Y)
- Visual Level Filters:
  - Filter on `DimDate`.`WeekNumberOfYear` (Type: Advanced, Definition: (Complex condition))
  - Filter on `vw_Timesheet`.`[MSR_Cumulative_Revenue]` (Type: Advanced, Definition: (Complex condition))

