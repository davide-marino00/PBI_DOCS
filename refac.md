# Power BI Model & Report Documentation

*Generated on: 2025-04-29 01:45:50*

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

The Power BI data model appears to be designed for a project management and timesheet tracking system, likely within a corporate or organizational context. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a focus on capturing and analyzing employee work hours and project-related activities, while the various lookup tables ('lkp_Project', 'lkp_Unit', 'lkp_Code', and 'lkp_fltr_Employee') indicate a structured approach to categorizing and filtering data by projects, organizational units, and timesheet codes. The 'DimDate' table further implies that time-based analysis is a key component, allowing users to examine trends and performance over specific periods.

The relationships established between these tables indicate a comprehensive framework for reporting and analysis. For instance, the connections between 'vw_Timesheet' and the lookup tables enable detailed insights into how hours are allocated across different projects and units, while the 'vw_missing_timesheet' table highlights potential gaps in timesheet submissions, which is critical for accurate project tracking and resource management. Overall, this data model is likely aimed at enhancing operational efficiency, ensuring accountability in time reporting, and facilitating informed decision-making regarding project management and resource allocation.

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

The 'DimDate' table serves as a comprehensive date dimension that enables businesses to analyze and report on time-based metrics, facilitating insights into trends and performance across various time periods. By providing detailed attributes such as day, week, month, and quarter, this table supports effective time-based filtering and aggregation in reporting and analytics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting within the dataset. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient data retrieval and analysis in time-based reporting and analytics. |
| `DayName` | `string` | The 'DayName' column in the 'DimDate' table stores the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | Column Description: The 'DayNumberOfMonth' column represents the numerical day of the month (ranging from 1 to 31) for each date entry in the 'DimDate' table, facilitating date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical designation of each day of the week, where values range from 1 (Sunday) to 7 (Saturday), facilitating date-related analyses and reporting. |
| `DayNumberOfYear` | `int64` | Column Description: The 'DayNumberOfYear' column represents the sequential day of the year (ranging from 1 to 365 or 366) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `MonthName` | `string` | The 'MonthName' column in the 'DimDate' table stores the full names of the months, providing a human-readable reference for date-related analyses and reporting. |
| `MonthNumberOfYear` | `int64` | The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month (1 for January through 12 for December), facilitating time-based analysis and reporting. |
| `ShortDate` | `dateTime` | The 'ShortDate' column in the 'DimDate' table represents a simplified date format, capturing the specific date without time components to facilitate easier date-based analysis and reporting. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Column Description: Indicates whether the date corresponds to the current calendar year, with values of 'Yes' or 'No' to facilitate time-based analysis and reporting.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `CurrentYearFlag` in a data model, typically within a tool like Power BI or Excel. Here's a breakdown of what this expression does in simple business terms:

1. **Purpose**: The `CurrentYearFlag` column is designed to indicate whether a given date falls within the current year or not.

2. **Components**:
   - `DimDate[ShortDate]`: This refers to a date field in a table named `DimDate`. It represents individual dates that we want to evaluate.
   - `YEAR(DimDate[ShortDate])`: This part extracts the year from each date in the `ShortDate` column.
   - `YEAR(TODAY())`: This retrieves the current year based on today's date.

3. **Logic**:
   - The `IF` function checks if the year extracted from the `ShortDate` is less than or equal to the current year.
   - If the condition is true (meaning the date is in the current year or earlier), it returns `1`.
   - If the condition is false (meaning the date is in a future year), it returns `0`.

4. **Outcome**: 
   - The result is a column where each row will have a value of `1` if the date is in the current year or earlier, and `0` if the date is in a future year.
   - This flag can be useful for filtering or analyzing data based on whether dates are relevant to the current year, helping businesses focus on timely information.

In summary, the `CurrentYearFlag` column helps identify which dates are in the current year or earlier, allowing for better analysis and reporting of time-sensitive data.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table captures and categorizes various metrics related to employee hours worked and their corresponding percentage contributions to overall productivity. This table aids in analyzing workforce efficiency and resource allocation, enabling informed decision-making for operational improvements.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column contains string representations of hours worked alongside their corresponding percentage contributions, facilitating the analysis of time allocation and performance metrics within the dataset. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and their corresponding percentage contributions, facilitating data enrichment and analysis within the 'Hours_nd_Percentage' table. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes and organizes various codes used across the business, facilitating data consistency and integrity. It includes attributes for code identification, qualification criteria, and sorting order, enabling efficient data retrieval and analysis in reporting and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column in the 'lkp_Code' table stores unique string identifiers that facilitate the enrichment of data by linking it to corresponding descriptive attributes or categories. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numerical identifier to categorize data for enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table contains string values that determine the order in which records are displayed or processed, facilitating organized data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics. This table enhances data integrity and usability by standardizing employee information for analysis and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column serves as a unique identifier for each employee in the 'lkp_fltr_Employee' table, facilitating efficient data enrichment and retrieval processes. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, facilitating the enrichment of data by providing identifiable references for each individual within the employee lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, providing essential details such as project identifiers, qualification metrics, billing amounts, and associated groups, enabling effective project management and financial analysis within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing amount associated with each project, facilitating financial tracking and analysis. |
| `Group` | `string` | The 'Group' column in the 'lkp_Project' table categorizes projects into distinct classifications to facilitate data enrichment and analysis. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the organization. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, serving as a numerical identifier to categorize projects based on their eligibility or compliance criteria. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their characteristics such as billable status and external affiliations. This table is essential for analyzing resource allocation and financial performance across different units, facilitating informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column (int64) in the 'lkp_Unit' table indicates the department responsible for billable activities, facilitating accurate tracking and reporting of revenue-generating units. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column identifies the specific organizational unit within the company, facilitating data enrichment and analysis related to departmental structures and functions. |
| `OWN-Sub-ExtT` | `string` | Column Description: The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores external type identifiers for owned sub-units, facilitating data enrichment and integration processes. |
| `Unit` | `string` | The 'Unit' column in the 'lkp_Unit' table stores the names of measurement units used for data enrichment, facilitating consistent and accurate data interpretation across various applications. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related data, capturing essential details such as project identifiers, descriptions, statuses, and leadership assignments. This table enables effective project management and oversight by providing insights into project categorization, progress tracking, and resource allocation across various project groups within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing essential data for project tracking and performance analysis. |
| `Administration` | `string` | The 'Administration' column in the 'tbl_Project' table captures the administrative details and oversight responsibilities associated with each project, facilitating effective management and governance. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates and changes within the 'tbl_Project' table. |
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each project, facilitating easy identification and management of client relationships within the project database. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating efficient data retrieval and management within the tbl_Project table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated, providing a timestamp for tracking project creation within the tbl_Project table. |
| `Currency` | `string` | The 'Currency' column in the 'tbl_Project' table specifies the type of currency used for financial transactions related to each project, facilitating accurate budgeting and reporting. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the timestamp of when each project entry was initially created, facilitating tracking and management of project timelines. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or category associated with each project in the 'tbl_Project' table, facilitating targeted analysis and reporting. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column in the 'tbl_Project' table indicates the scheduled date and time for the completion of each project, facilitating effective project timeline management and planning. |
| `Project` | `string` | The 'Project' column contains the names of individual projects, serving as a key identifier for tracking and managing project-related data within the tbl_Project table. |
| `ProjectDescription` | `string` | Column Description: A detailed narrative outlining the objectives, scope, and key elements of the project, facilitating a comprehensive understanding of its purpose and goals. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the category or classification of a project, facilitating organization and reporting within the 'tbl_Project' table. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a detailed textual overview of the project group, facilitating better understanding and categorization of related projects within the 'tbl_Project' table. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for linking related data across the database. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the project's execution and management within the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency amount of the sales price for each project, stored as a decimal to ensure precision in financial calculations. |
| `Startdate` | `dateTime` | The 'Startdate' column in the 'tbl_Project' table captures the date and time when a project is initiated, serving as a critical reference point for project timelines and scheduling. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, allowing for effective tracking and management of project workflows. |
| `Statuscode` | `string` | The 'Statuscode' column in the 'tbl_Project' table indicates the current operational status of each project, facilitating effective tracking and management of project progress. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with each project, stored as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table provides a comprehensive overview of employees who have not submitted their timesheets, detailing the relevant time period, employee and manager information, and contract specifics. This data is essential for management to identify compliance issues, ensure accurate payroll processing, and enhance workforce accountability.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by employees that have not been recorded in their timesheets, facilitating accurate tracking and reporting of labor hours. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is set to expire, providing critical information for managing timesheet compliance and workforce planning within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to facilitate the identification of discrepancies in timesheet submissions within the 'vw_missing_timesheet' table. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | Column Description: Indicates the current status of contracts associated with employees who have missing timesheets, represented as a string value. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on outstanding time reporting issues. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours recorded for each employee's timesheet entry, facilitating the identification of discrepancies in time reporting. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_missing_timesheet' table identifies the unique identifier of the manager responsible for overseeing the employee associated with the missing timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees' timesheet submissions, facilitating accountability and communication regarding missing entries. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee time tracking. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the missing timesheet records, facilitating the tracking and management of time allocation across various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project, facilitating the tracking and management of timesheet entries associated with specific projects in the 'vw_missing_timesheet' table. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true signifying rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet data is complete and ready for reporting, with a value of true signifying readiness and false indicating that further action is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of tracking and analyzing missing timesheet entries for enhanced data insights. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the context of timesheet data analysis. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date and time when a timesheet entry is missing, facilitating timely follow-up and data reconciliation. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific organizational subunit associated with each entry in the missing timesheet records, facilitating targeted follow-up and resolution of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for timesheets, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' view. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the work performed during the timesheet period, aiding in the identification and resolution of missing timesheet entries. |
| `Week_` | `int64` | The 'Week_' column (int64) in the 'vw_missing_timesheet' table represents the numerical week of the year associated with each timesheet entry, facilitating the identification and analysis of missing timesheet submissions by week. |
| `Year_` | `int64` | The 'Year_' column (int64) in the 'vw_missing_timesheet' table represents the calendar year associated with each timesheet entry, facilitating the analysis of timesheet data over time. |

##### Calculated Columns

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column indicates the department associated with billable hours for employees, helping to identify revenue-generating activities within the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression starts by checking if there is a related value in the `lkp_Unit` table for the column `BillableDep`. This is done using the `RELATED` function, which pulls in data from a related table based on the relationships defined in your data model.

2. **Handle Blank Values**: The first part of the `IF` statement checks if the related `BillableDep` value is blank (i.e., there is no corresponding value in the `lkp_Unit` table). If it is blank, the expression returns `1`. This could indicate that if there is no specific billable dependency, it defaults to a value of `1`, possibly meaning that the item is considered billable by default.

3. **Check for Non-Zero Values**: If the related `BillableDep` value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns `1`, suggesting that there is a valid billable dependency present.

4. **Return Zero for Zero Values**: If the related `BillableDep` value is zero, the expression returns `0`. This indicates that if there is a billable dependency but it is zero, then the item is not considered billable.

### Summary:
In summary, this DAX expression is designed to determine whether an item is billable based on its relationship to the `lkp_Unit` table. It assigns a value of `1` if:
- There is no related `BillableDep` value (considered billable by default).
- There is a related `BillableDep` value that is not zero.

It assigns a value of `0` if there is a related `BillableDep` value that is zero. This helps in categorizing items as billable or not based on their dependencies.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `CC_ActiveEmployees` in a data model, likely related to employee management or timesheet tracking. Here's a breakdown of what this code does in simple business terms:

1. **Purpose**: The calculated column determines whether an employee is considered "active" on a specific date (referred to as `ShortDate`).

2. **Conditions Checked**:
   - **Start Date Check**: It first checks if the `ShortDate` (the date being evaluated) is on or after the employee's `ContractStartDate`. This means the employee's contract must have started before or on this date for them to be considered active.
   - **End Date Check**: Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. This means the employee's contract must still be valid on this date. However, if the `ContractEndDate` is blank (meaning the employee's contract does not have an end date), the employee is still considered active.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the contract has no end date), the expression returns a value of `1`, indicating that the employee is active on that date.
   - If either condition is not met, it returns a value of `0`, indicating that the employee is not active on that date.

### Summary:
In summary, this DAX expression effectively flags employees as "active" (1) or "not active" (0) based on whether the date in question falls within their contract period. This can help businesses track which employees are currently active and eligible for work or timesheet submissions.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to facilitate the identification of records requiring further attention in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `IncompleteFlag` in a data model, specifically in a table named `vw_missing_timesheet`. Here's a breakdown of what this expression does in simple business terms:

### Explanation of the DAX Expression:

- **Condition Check**: The expression checks the value of the column `MissingHours` in the `vw_missing_timesheet` table.
- **Logic**: 
  - If the value of `MissingHours` is **less than 0**, it means that there is an issue with the recorded hours (perhaps indicating that the hours are negative, which doesn't make sense in this context).
  - In this case, the expression returns **1**, which can be interpreted as a flag indicating that there is an incompleteness or error related to the timesheet.
  - If the value of `MissingHours` is **0 or greater**, the expression returns **0**, indicating that there is no issue or incompleteness.

### What It Achieves:

- **Flagging Incomplete Records**: This calculated column effectively flags records where the `MissingHours` value is problematic (i.e., negative). 
- **Data Quality Control**: By identifying these records, the business can take action to investigate and correct any discrepancies in the timesheet data.
- **Simplified Reporting**: The `IncompleteFlag` can be used in reports or dashboards to quickly filter or highlight entries that need attention, ensuring better management of timesheet submissions and accuracy.

In summary, this DAX expression helps in identifying and flagging incomplete or erroneous timesheet entries based on the `MissingHours` value, facilitating better oversight and data integrity in timesheet management.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression starts by checking if there is a related value in the 'lkp_Unit' table for the 'Unit' column. This is done using the `RELATED` function, which pulls in data from a related table based on existing relationships in your data model.

2. **Handle Missing Data**: The `ISBLANK` function checks if the value retrieved from the 'lkp_Unit[Unit]' column is blank (i.e., there is no corresponding unit found). 

3. **Return Values Based on the Check**:
   - If the value is blank (meaning there is no related unit), the expression returns the string "Unknown". This indicates that the system could not find a unit associated with the current record.
   - If there is a related unit (i.e., the value is not blank), it returns the actual unit name from the 'lkp_Unit[Unit]' column.

### Summary:
In summary, this DAX expression is designed to populate the 'MAIN_UNIT' column with either the name of the unit associated with each record or "Unknown" if no unit is found. This helps ensure that users can easily identify records with missing unit information, improving data clarity and usability.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column (string) in the 'vw_missing_timesheet' table represents the specific year and week number for each entry, facilitating the tracking and analysis of timesheet submissions over time.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'YearWeek' in a data model, typically within tools like Power BI or Excel. Here's a breakdown of what it does in simple business terms:

1. **Extracting the Year**: The function `YEAR([ContractEndDate])` takes a date from the column `[ContractEndDate]` and extracts the year from that date. For example, if the contract ends on December 15, 2023, this part would return `2023`.

2. **Calculating the Week Number**: The function `WEEKNUM([ContractEndDate])` calculates the week number of the year for the same date. Continuing with our example, if December 15, 2023, falls in the 50th week of the year, this function would return `50`.

3. **Formatting the Week Number**: The `FORMAT(..., "00")` part ensures that the week number is always displayed as a two-digit number. So, if the week number is `5`, it will be formatted as `05`.

4. **Combining Year and Week Number**: The `& "-" &` part combines the year and the formatted week number into a single string, separated by a hyphen. So, for our example of December 15, 2023, the final result would be `2023-50`.

### Summary:
In summary, this DAX expression creates a new column that represents the year and the week number of the contract's end date in a format like "YYYY-WW". This is useful for reporting and analysis, allowing users to easily group or filter data by year and week.

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

1. **Purpose**: This measure counts the number of unique employees (consultants) who have recorded negative hours in their timesheets. Negative hours typically indicate that there was a correction or an adjustment made, such as a reduction in hours worked.

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows you to apply filters to your calculations.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the number of unique employee names in the `vw_missing_timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries with negative hours.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This filter condition specifies that only the records where the `MissingHours` are less than zero should be considered in the count. In other words, it focuses on those instances where employees have negative hours.

3. **Outcome**: The measure ultimately provides a count of how many different consultants have negative hours recorded in their timesheets. This information can be useful for management to identify issues with timesheet reporting, track corrections, or understand employee workload adjustments.

In summary, `Count_Negative_Consultant` helps businesses monitor and analyze the frequency of negative hour entries among their consultants, which can be critical for accurate payroll and project management.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `CountNegativeMissingHours`. Here’s a breakdown of what it does in simple business terms:

1. **Purpose**: This measure counts the number of unique employees who have recorded negative missing hours in a timesheet.

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID])`**: This part counts the number of unique Employee IDs from the `vw_missing_timesheet` table. Essentially, it tells us how many different employees are involved.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This condition filters the data to only include records where the `MissingHours` value is less than zero. This means we are only interested in cases where employees have negative missing hours, which could indicate an error or an adjustment in their reported hours.

3. **Outcome**: The measure ultimately provides a count of how many distinct employees have negative values for missing hours. This can help a business identify potential issues with timesheet reporting or highlight employees who may need further review regarding their reported hours.

In summary, `CountNegativeMissingHours` helps organizations track and manage discrepancies in employee timesheets by counting how many employees have reported negative missing hours.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called `Dax_EmpCount_MissingTS`, which counts the number of active employees who have missing timesheet hours for a specific period. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The measure uses a data source called `vw_missing_timesheet`, which likely contains information about employees, their working hours, and whether they have submitted their timesheets.

2. **Summarization**: The code first creates a summarized table that groups data by employee name, year, week, unit, and subunit. This means it organizes the data to look at each employee's hours worked during specific time frames.

3. **Calculating Missing Hours**: For each group (each employee for each week), it calculates:
   - **MissingHours**: This is determined by taking the total hours worked (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contracted hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has not logged enough hours compared to what they were contracted to work.
   - **MinActiveEmp**: This checks if the employee is active during that period by finding the minimum value of `CC_ActiveEmployees`. If this value is 1, it means the employee is considered active.

4. **Filtering**: The `FILTER` function then narrows down the summarized data to only include those records where:
   - The employee is active (`MinActiveEmp = 1`).
   - The calculated missing hours are less than zero (`MissingHours < 0`), indicating that they have not logged enough hours.

5. **Counting Active Employees**: Finally, the measure counts the number of rows in the filtered table, which corresponds to the number of active employees who have missing timesheet hours.

### Summary:
In summary, this DAX measure counts how many active employees have not logged enough hours in their timesheets for a given year and week. This information can be crucial for management to identify potential issues with timesheet submissions and ensure that all employees are accurately reporting their work hours.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is designed to calculate a measure called `Dax_MissingHours`, which focuses on identifying and quantifying the missing hours for active employees in a specific context, such as a timesheet or payroll system. Here's a breakdown of what this code does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. This variable filters a summarized view of the data from a table called `vw_missing_timesheet`. The summary includes key details such as the employee's name, the year and week of the data, the unit they belong to, and their subunit.

2. **Calculate Missing Hours**: For each employee in the summarized data, the code calculates "MissingHours." This is done by taking the total hours recorded (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contract hours they are supposed to work (`MAX(vw_missing_timesheet[ContractHours])`). If the result is negative, it indicates that the employee has worked fewer hours than expected.

3. **Filter for Active Employees**: The code then filters this summarized data to only include employees who are currently active (`[MinActiveEmp] = 1`) and have negative missing hours (`[MissingHours] < 0`). This means it focuses on employees who are actively working but have not met their expected hours.

4. **Calculate Total Missing Hours**: Finally, the `RETURN` statement uses the `SUMX` function to sum up the "MissingHours" for all the active employees identified in the previous step. This gives a total count of hours that active employees are missing based on their expected hours.

In summary, this DAX measure calculates the total number of hours that active employees are missing from their expected work hours, helping the business identify potential issues with employee attendance or timesheet reporting. This information can be crucial for payroll accuracy, resource planning, and overall workforce management.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate the total expected contract hours for employees on a weekly basis. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The calculation is based on a data table called `vw_missing_timesheet`, which likely contains records of employee timesheets, including information about the week, employee IDs, and their contract hours.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key dimensions:
   - **Week**: This represents the specific week for which we are calculating contract hours.
   - **EmployeeID**: This identifies each employee.

3. **Calculating Maximum Contract Hours**: For each unique combination of week and employee, the code calculates the maximum contract hours using `MAX(vw_missing_timesheet[ContractHours])`. This means that if an employee has multiple entries for the same week, it will take the highest value of contract hours recorded for that week.

4. **Summing Up Contract Hours**: After summarizing the data to get the maximum contract hours for each employee per week, the `SUMX` function then adds up all these maximum contract hours across all employees and weeks. 

5. **Final Result**: The final result of this measure, `ExpectedContractHoursWeekly`, is the total of the maximum contract hours for all employees across all weeks. This gives a clear picture of the total expected hours that employees are contracted to work each week, considering the highest recorded hours for each employee.

In summary, this DAX measure helps businesses understand the total expected working hours for their employees on a weekly basis, ensuring they can plan resources and manage workloads effectively.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the difference between two values related to hours worked and hours contracted for a specific context, likely in a reporting or analytical scenario.

Here's a breakdown of what it does:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum value of the "ContractHours" column from the "vw_missing_timesheet" table. "ContractHours" represents the total number of hours that an employee is contracted to work.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum value of the "Hours_" column from the same table. "Hours_" likely represents the actual hours that the employee has worked or reported.

3. **Subtraction**: The expression then subtracts the maximum actual hours worked (from "Hours_") from the maximum contracted hours (from "ContractHours"). 

### What It Achieves:
The overall result of this DAX measure, `MissingHoursOut`, calculates the number of hours that are missing or unaccounted for. In simple terms, it tells you how many hours an employee has not worked compared to what they were supposed to work according to their contract. 

If the result is a positive number, it indicates that the employee has not worked enough hours. If it is zero, it means they have worked exactly the contracted hours. If it were negative (though unlikely in this context), it would suggest that the employee has worked more hours than contracted, which might indicate overtime or additional work hours.

In summary, this measure helps in assessing whether employees are meeting their contracted work hours and can be useful for payroll, performance tracking, or resource management.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates a measure called `PercentageCompleteness`. Let's break it down in simple business terms:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part of the code adds up all the missing hours from a table called `vw_missing_timesheet`. Essentially, it tells us how many hours were not logged or completed.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that are expected to be worked in a week according to a contract. It serves as a benchmark for what should ideally be achieved.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the code calculates the difference between the total missing hours and the expected contract hours. If the result is positive, it indicates that the actual hours worked are less than what was expected. If it's negative, it means that the hours worked exceeded the expected hours.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: This function divides the difference calculated in the previous step by the expected contract hours. The `DIVIDE` function is used instead of a simple division operator to handle cases where the denominator (expected hours) might be zero. If it is zero, the result will be 0 instead of causing an error.

### What It Achieves:
The overall measure calculates the percentage of completeness regarding the expected working hours. Specifically, it shows how much the actual hours worked (after accounting for missing hours) deviate from the expected hours. 

- A positive percentage indicates that there are more missing hours than expected, suggesting a shortfall in work completion.
- A negative percentage indicates that the actual hours worked exceeded the expected hours, which could imply overachievement.

In summary, `PercentageCompleteness` helps assess how well the actual work aligns with contractual expectations, providing insights into productivity and potential areas for improvement.

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

1. **Data Source**: The measure is based on a data table called `vw_missing_timesheet`. This table likely contains records of employees and their timesheet submissions, including information about whether their submissions are complete or incomplete.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by three key columns:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This is the name of the employee.
   - `MAIN_UNIT`: This indicates the main unit or department the employee belongs to.

   By grouping the data in this way, the measure is preparing to analyze the unique combinations of employees and their respective units for each week.

3. **Creating a New Column**: Within the `SUMMARIZE` function, a new column called `"IncompleteFlag"` is created. This column uses the `MAX` function to determine the highest value of `IncompleteFlag` for each group. The `IncompleteFlag` likely indicates whether an employee's timesheet is incomplete (e.g., 1 for incomplete, 0 for complete). By using `MAX`, the measure ensures that if any employee in that group has an incomplete timesheet, the flag will reflect that.

4. **Calculating the Total**: The outer `SUMX` function then takes the summarized data and sums up the values in the `[IncompleteFlag]` column. This means it adds up all the flags for the unique combinations of employees and their units across the weeks.

### What It Achieves:
In summary, this DAX measure calculates the total number of unique employees who have incomplete timesheets, grouped by their respective units and weeks. It helps the business understand how many employees in each unit are missing timesheet submissions, which can be crucial for tracking compliance and ensuring that all employees are submitting their timesheets on time. This information can be used for reporting, identifying trends, and taking corrective actions if necessary.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This table enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column in the 'vw_Timesheet' table identifies the specific client associated with each timesheet entry, facilitating accurate billing and project tracking. |
| `Code2` | `string` | The 'Code2' column in the 'vw_Timesheet' table stores a secondary identifier code used to categorize or classify timesheet entries for enhanced data analysis and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract is set to expire, providing essential information for managing workforce planning and contract renewals within the timesheet data. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when a contract becomes effective, providing essential context for timesheet entries related to specific contractual agreements. |
| `ContractStatusToday` | `string` | Column Description: Indicates the current status of the contract associated with the timesheet entry, represented as a string value. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, providing essential insights for performance tracking and decision-making within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the total monetary cost associated with the recorded time entries in the 'vw_Timesheet' table, expressed as a double-precision floating-point number. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, expressed as a double to allow for precise financial calculations. |
| `Currency` | `string` | The 'Currency' column specifies the type of currency used for financial entries in the timesheet, facilitating accurate reporting and analysis of costs across different currencies. |
| `DebtorName` | `string` | The 'DebtorName' column contains the name of the individual or entity responsible for payment, providing essential context for financial tracking and accountability within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_Timesheet' table uniquely identifies each employee, facilitating the association of timesheet entries with the corresponding personnel for accurate tracking and reporting of work hours. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column in the 'vw_Timesheet' table stores the unique identifier and name of each employee, facilitating the association of timesheet entries with the corresponding personnel. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating the identification and tracking of individual work hours and contributions. |
| `Employer` | `string` | The 'Employer' column identifies the organization or company for which the employee is working during the recorded timesheet entry. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating accurate payroll processing and reporting. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to each timesheet entry, enhancing the understanding of the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with each timesheet entry, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, facilitating accurate tracking and reporting of labor efforts. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, providing essential data for analyzing labor allocation and productivity. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or vacation, to facilitate accurate reporting and analysis of employee time allocation. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee time allocation. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing project timelines and resource allocation, facilitating effective project tracking and accountability within the timesheet records. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the names of individual project management identifiers associated with each timesheet entry, facilitating the tracking and reporting of project-related hours. |
| `JobTitle` | `string` | The 'JobTitle' column in the 'vw_Timesheet' table captures the specific role or position of an employee, providing essential context for analyzing timesheet data and resource allocation. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking timesheet records with related datasets, facilitating data integration and analysis. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_Timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and reporting within the organization. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column stores the names of managers associated with each timesheet entry, facilitating easy identification of managerial oversight for employee hours logged. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key of the manager responsible for overseeing the timesheet entries, facilitating effective management and reporting of employee work hours. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's manager, facilitating the identification of supervisory relationships within the timesheet data. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries, detailing the hours and tasks allocated to employees for accurate timesheet reporting and analysis. |
| `Project` | `string` | The 'Project' column in the 'vw_Timesheet' table identifies the specific project associated with each recorded time entry, facilitating accurate tracking and reporting of resource allocation and project costs. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project, facilitating the tracking and reporting of time spent on various projects within the timesheet data. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that identifies the specific project associated with each timesheet entry, enabling detailed tracking and analysis of time spent on various projects. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the specific project profile associated with each timesheet entry, facilitating accurate project tracking and reporting. |
| `ProjectType` | `string` | Column Description: The 'ProjectType' column categorizes the nature of the projects recorded in the timesheet, enabling better analysis of resource allocation and project performance. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true signifying rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet is ready for reporting, with a value of true signifying readiness and false indicating further action is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double to accommodate precise financial calculations within the context of timesheet data analysis. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current approval or processing state of each timesheet entry, facilitating tracking and management of employee work hours. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, enabling better resource allocation and management. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for each timesheet entry, facilitating the tracking and management of employee work hours and project allocations. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a textual summary of the tasks and activities recorded in the timesheet, facilitating better understanding and analysis of employee work efforts. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table represents the measurement unit associated with the recorded time entries, facilitating accurate tracking and reporting of work hours across various projects. |
| `UnitCode` | `string` | The 'UnitCode' column in the 'vw_Timesheet' table represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating accurate tracking and reporting of labor costs by department. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and data enrichment within the timesheet management system. |
| `YearWeek` | `string` | The 'YearWeek' column represents the year and week number in a string format, facilitating time-based analysis and reporting of timesheet entries within the vw_Timesheet table. |

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
- **DAX Explanation (Generated):** This DAX code snippet creates a calculated column named `Approved_With_V_Z`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The column is designed to determine whether a timesheet is considered "approved" based on specific criteria.

2. **Conditions**: The code checks two main conditions:
   - **First Condition**: It checks if the `Approved` column is marked as `TRUE`. This means that if the timesheet has been officially approved, it meets the criteria.
   - **Second Condition**: It checks if the `TimesheetCode` for that entry is either "V" or "Z". These codes likely represent specific types of timesheets that are automatically considered approved regardless of the `Approved` status.

3. **Output**: 
   - If either of the conditions is met (the timesheet is approved or it has a code of "V" or "Z"), the calculated column will return a value of `1`. This indicates that the timesheet is approved.
   - If neither condition is met, it will return a value of `0`, indicating that the timesheet is not approved.

In summary, the `Approved_With_V_Z` column effectively flags timesheets as approved (1) or not approved (0) based on whether they are officially approved or have specific codes ("V" or "Z"). This helps in easily identifying which timesheets can be considered approved for further processing or reporting.

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column identifies the department associated with billable hours recorded in the timesheet, facilitating accurate billing and resource allocation.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `BillableDep`. Let's break it down step by step in simple business terms:

1. **Purpose**: The calculated column `BillableDep` is designed to determine whether a certain unit is billable based on a related table called `lkp_Unit`.

2. **Logic**:
   - The code first checks if the `BillableDep` value from the `lkp_Unit` table is blank (i.e., there is no value).
   - If it is blank, the calculated column will return a value of **1**. This indicates that the unit is considered billable when there is no specific billable dependency defined.
   - If the `BillableDep` value is not blank, the code then checks if this value is not equal to **0**.
     - If the value is not **0**, it returns **1**, meaning the unit is billable.
     - If the value is **0**, it returns **0**, indicating that the unit is not billable.

3. **Outcome**: 
   - The final result of this calculation will be either **1** (indicating the unit is billable) or **0** (indicating the unit is not billable). 
   - This helps in identifying which units can be billed for services or products based on their relationship with the `lkp_Unit` table.

In summary, this DAX expression effectively categorizes units as billable or not based on the presence and value of a related field, helping businesses manage their billing processes more efficiently.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `BillablePrj` in a data model, specifically in a table called `vw_Timesheet`. Here's a breakdown of what this code does in simple business terms:

1. **Purpose**: The `BillablePrj` column is designed to identify whether a particular timesheet entry is considered "billable" or not. A billable entry is one that can be charged to a client or customer.

2. **Conditions for Billability**: The code checks three specific conditions to determine if an entry is billable:
   - **Condition 1**: `vw_Timesheet[SalesPrice] > 0` - This checks if the sales price for the entry is greater than zero. If there is a sales price, it indicates that the work done can be billed to a client.
   - **Condition 2**: `vw_Timesheet[ProjectProfileCode] = 81` - This checks if the project profile code is equal to 81. If it is, this specific project is predefined as billable, regardless of other factors.
   - **Condition 3**: `SEARCH("Customer Success Services", vw_Timesheet[Project],, 0) > 0` - This checks if the project name contains the phrase "Customer Success Services". If it does, this indicates that the project is related to customer success, which is also considered billable.

3. **Output**: 
   - If any of the three conditions are met (meaning the entry is billable), the expression returns `1`.
   - If none of the conditions are met, it returns `0`.

In summary, this DAX expression effectively flags timesheet entries as billable (1) or non-billable (0) based on specific criteria related to sales price, project profile, and project name. This helps businesses track which work can be invoiced to clients, ensuring accurate billing and revenue recognition.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column in the 'vw_Timesheet' table captures the name of the employer associated with each timesheet entry, facilitating the identification of the organization responsible for employee hours worked.

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

Here's a breakdown of what this code does in simple business terms:

1. **Combining Information**: The code takes two pieces of information from the `vw_Timesheet` table:
   - The name of the employee (`EmployeeName`).
   - The number of hours that employee works per week (`HoursperWeek`).

2. **Formatting the Output**: It combines these two pieces of information into a single string. The format of the output will be:
   - The employee's name followed by a hyphen and then the number of hours they work per week. For example, if the employee's name is "John Doe" and he works 40 hours per week, the result would be "John Doe - 40".

3. **Purpose**: This calculated column is useful for creating a clear and concise representation of each employee's weekly working hours alongside their name. It can help in reporting, analysis, or visualization, making it easier to see how many hours each employee is scheduled to work at a glance.

In summary, this DAX expression creates a new column that neatly combines employee names with their corresponding weekly hours, enhancing the readability and usability of the data.

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

1. **Check for Project Group**: The first part of the code checks if there is a corresponding project group for the project listed in the 'vw_Timesheet' table. It does this by looking up the 'Group' from the 'lkp_Project' table based on the project name.

2. **Return Project Group if Available**: If a project group is found (meaning the lookup is not blank), it returns that project group name. This means that if the project is associated with a specific group, it will categorize it accordingly.

3. **Handle Missing Project Group**: If no project group is found (the lookup is blank), the code then checks if the project is billable. This is determined by looking at the 'BillablePrj' column in the 'vw_Timesheet' table.

4. **Categorize as Billable or Unbillable**: 
   - If 'BillablePrj' equals 1 (indicating that the project is billable), it assigns the category "Billable".
   - If 'BillablePrj' does not equal 1, it assigns the category "Other Unbillable".

### Summary:
In summary, this DAX expression categorizes each project in the 'vw_Timesheet' table into one of three groups:
- The specific project group from the 'lkp_Project' table if it exists.
- "Billable" if the project is billable but has no associated group.
- "Other Unbillable" if the project is not billable and has no associated group.

This categorization helps in understanding the nature of projects in terms of their billing status and group association, which can be useful for reporting and analysis.

**`IsContractActive`** (`string`)

- **Description:** Column Description: Indicates whether the contract associated with the timesheet entry is currently active, represented as a string value.
- **DAX Expression:**
```dax
IF(
			    ISBLANK('vw_Timesheet'[ContractEndDate]) || 'vw_Timesheet'[ContractEndDate] > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called **IsContractActive** in a data model, specifically for a table named **vw_Timesheet**. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column determines whether a contract is currently active or not.

2. **Conditions Checked**:
   - **ISBLANK('vw_Timesheet'[ContractEndDate])**: This part checks if the **ContractEndDate** is blank (i.e., there is no end date specified). If there is no end date, it implies that the contract is ongoing and should be considered active.
   - **'vw_Timesheet'[ContractEndDate] > TODAY()**: This part checks if the **ContractEndDate** is greater than today's date. If the end date is in the future, it means the contract is still active.

3. **Output**:
   - If either of the above conditions is true (meaning the contract has no end date or the end date is still in the future), the column will display **"Active"**.
   - If neither condition is true (meaning there is an end date that has already passed), the column will display **"Not Active"**.

### Summary:
In summary, this DAX expression helps businesses quickly identify which contracts are currently active by checking if they have an end date that is either not set or is still in the future. If a contract has already ended, it will be marked as "Not Active." This information can be crucial for managing contracts, resources, and planning.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the 'Unit' column of the 'lkp_Unit' table. This is done using the `RELATED` function, which retrieves a value from a related table based on the relationship defined in the data model.

2. **Handle Missing Data**: The `ISBLANK` function checks if the value retrieved from the 'lkp_Unit[Unit]' column is blank (i.e., there is no corresponding unit found). 

3. **Return Values Based on the Check**:
   - If the related 'Unit' value is blank (meaning there is no matching unit), the expression returns the string "Unknown". This indicates that the unit information is not available.
   - If there is a valid related 'Unit' value, it simply returns that value.

### Summary:
In summary, this DAX expression is designed to populate the 'MAIN_UNIT' column with either the corresponding unit name from the 'lkp_Unit' table or "Unknown" if no unit is found. This helps ensure that every record has a clear indication of its unit status, making it easier to analyze and understand the data.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) to facilitate time-based reporting and analysis of timesheet data.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to create a calculated column named 'MonthNumber' in a data model, typically within a tool like Power BI or Excel.

### What It Does:
- **Extracts the Month**: This expression takes a date value from the column `[TimesheetDate]` and extracts the month part of that date.
- **Returns a Number**: The result is a number between 1 and 12, where:
  - 1 represents January,
  - 2 represents February,
  - 3 represents March,
  - and so on, up to
  - 12, which represents December.

### Business Purpose:
- **Simplifies Analysis**: By converting dates into month numbers, it allows for easier grouping and analysis of data by month. For example, you can quickly summarize timesheet data to see how many hours were logged in each month.
- **Facilitates Reporting**: This calculated column can be used in reports and dashboards to visualize trends over time, such as monthly performance or workload.

In summary, this DAX expression helps businesses analyze and report on their timesheet data by breaking it down into monthly segments, making it easier to understand patterns and trends.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' column in the 'vw_Timesheet' table stores the unique employee identification numbers as strings, facilitating the association of timesheet entries with specific employees for accurate tracking and reporting.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

### In Simple Business Terms:

- **Purpose**: This expression counts how many different employees have entries in the timesheet.
- **What it Achieves**: It helps in understanding the total number of distinct employees who have worked or logged time, which can be useful for reporting, analysis, or resource management. For example, if you want to know how many unique employees contributed to a project or worked during a specific period, this calculation provides that insight.

### Summary:
In essence, this DAX code helps businesses track employee participation by counting each employee only once, regardless of how many times they appear in the timesheet.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table categorizes the type of time entry as either owned, subcontracted, or external, facilitating accurate tracking and reporting of resource allocation.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used in a calculated column to retrieve a specific value from a related table. Here's a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a table that has a relationship with another table called `lkp_Unit`. The relationship is established based on a common key or identifier.

2. **Purpose**: The expression is designed to pull in a value from the `lkp_Unit` table, specifically from the column named `OWN-Sub-ExtT`.

3. **Functionality**: 
   - The `RELATED` function looks at the current row in the table where the calculated column is being created.
   - It finds the corresponding row in the `lkp_Unit` table based on the established relationship.
   - It then retrieves the value from the `OWN-Sub-ExtT` column of that related row.

4. **Outcome**: As a result, for each row in the original table, this calculated column will display the value from the `OWN-Sub-ExtT` column of the related `lkp_Unit` table. This allows users to enrich their data with additional information from another table without duplicating data.

In summary, this DAX expression helps to enhance the data in the current table by adding relevant information from a related table, making it easier to analyze and understand the data in a more comprehensive way.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `QualifyPrj`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column is designed to determine a qualification status for a project based on related data from another table called `lkp_Project`.

2. **Logic**:
   - The expression checks if the `Qualify` field from the `lkp_Project` table is blank (i.e., has no value) for the current row being evaluated.
   - If the `Qualify` field is blank, the expression returns a value of `1`. This could signify a default or a specific status indicating that the project does not have a qualification assigned.
   - If the `Qualify` field is not blank (meaning it has a value), the expression returns the actual value from the `Qualify` field in the `lkp_Project` table.

3. **Outcome**: The result of this calculation will be a column where each row will either show a `1` (if there is no qualification) or the actual qualification value from the related project. This helps in easily identifying projects that are either qualified or unqualified based on the related data.

In summary, this DAX expression effectively assigns a qualification status to each project, ensuring that if no qualification is found, a default value of `1` is used, making it easier to analyze project qualifications in your data model.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for submitting the timesheet data, facilitating accurate tracking and reporting of labor hours.

**`RReady_With_V_Z`** (`string`)

- **Description:** Column Description: Indicates the readiness status of a resource with respect to a specific project or task, represented as a string value in the context of timesheet data.
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
   - The first condition checks if the value of the column `[ReportReady]` is `TRUE`. This means that if the report is ready, the condition is satisfied.
   - The second condition checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means that if the timesheet code is one of these two specific codes, the condition is also satisfied.

3. **Output**:
   - If either of the conditions is true (i.e., the report is ready or the timesheet code is "V" or "Z"), the calculated column will return a value of `1`.
   - If neither condition is met, it will return a value of `0`.

### Summary:
In summary, this DAX expression is used to flag rows in the dataset where either the report is ready or the timesheet code is "V" or "Z". A value of `1` indicates that one of these conditions is met, while a value of `0` indicates that neither condition is met. This can be useful for filtering or analyzing data based on readiness or specific timesheet codes.

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

1. **Condition Check**: The measure first checks the value of another measure called `[ContractStatusMeasure]`. It specifically looks to see if this measure equals "Active".

2. **Outcome Based on Condition**:
   - If the condition is true (meaning the contract status is "Active"), the measure returns the value of another measure called `[HoursDifference]`. This likely represents the difference in hours related to the contract, such as hours worked or hours remaining.
   - If the condition is false (meaning the contract status is not "Active"), the measure returns the text "Not Active".

### Summary:
In essence, `ContractStatus2` is used to determine the status of a contract. If the contract is currently active, it provides the relevant hours difference; if not, it simply indicates that the contract is not active. This measure helps users quickly understand the current state of contracts and their associated hours.

**`ContractStatusMeasure`**

- **DAX Expression:**
```dax
IF(
			    ISBLANK(MAX('vw_Timesheet'[ContractEndDate])) || MAX('vw_Timesheet'[ContractEndDate]) > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `ContractStatusMeasure`, which determines the status of a contract based on its end date. Here’s a breakdown of what it does in simple business terms:

1. **Check for Contract End Date**: The measure first looks at the maximum value of the `ContractEndDate` from the `vw_Timesheet` table. This means it is checking the latest end date of contracts that are relevant to the current context (like a specific employee or project).

2. **Determine if the End Date is Blank or in the Future**:
   - **ISBLANK**: It checks if this maximum end date is blank (meaning there is no end date specified for the contract).
   - **Comparison with TODAY()**: It also checks if this maximum end date is greater than today’s date. This means it is looking to see if the contract is still valid and has not yet expired.

3. **Return Contract Status**:
   - If either of the above conditions is true (the end date is blank or it is a future date), the measure returns "Active". This indicates that the contract is currently valid and ongoing.
   - If neither condition is true (meaning the contract has an end date that is in the past), it returns "Not Active". This indicates that the contract has expired or is no longer valid.

### Summary:
In summary, the `ContractStatusMeasure` calculates whether a contract is currently active or not based on its end date. If there is no end date or if the end date is still in the future, it labels the contract as "Active". If the end date has passed, it labels it as "Not Active". This measure helps businesses quickly assess the status of contracts in their records.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 10, 2023, this function will return that specific date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. Week numbers typically start from 1 for the first week of the year.

So, when you combine these two functions, `WEEKNUM(TODAY())` calculates which week of the year it is right now. 

### Business Implications:
- **Tracking Performance**: This measure can be useful for businesses to track weekly performance metrics, such as sales or customer engagement, by knowing which week they are currently in.
- **Reporting**: It helps in generating reports that are organized by week, making it easier to analyze trends over time.
- **Planning**: Understanding the current week number can assist in planning activities, promotions, or resource allocation based on the time of year.

In summary, this DAX expression provides a simple yet powerful way to identify the current week of the year, which can be crucial for various business analyses and reporting.

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
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. It ensures that each employee is only counted once, even if they have multiple timesheet entries.
   - **`vw_Timesheet[Approved] = FALSE()`**: This condition filters the data to include only those timesheets that have not been approved (i.e., where the `Approved` field is marked as FALSE).

3. **Outcome**: The measure ultimately provides a count of how many distinct employees have submitted timesheets that are still pending approval. This information can be useful for management to understand how many employees are waiting for their timesheet approvals and to identify any potential bottlenecks in the approval process.

In summary, `Dax_EmpCount_Approved` helps track the number of employees with unapproved timesheets, aiding in better management and oversight of the approval workflow.

**`Dax_EmpCount_RReady`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[ReportReady] = FALSE()
			)
```
- **DAX Explanation (Generated):** The DAX expression you've provided is used to create a measure called `Dax_EmpCount_RReady`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure counts the number of unique employees whose timesheet reports are not ready.

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows us to apply filters to our calculations.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it tells us how many different employees are present in the data.
   - **`vw_Timesheet[ReportReady] = FALSE()`**: This is a filter condition. It specifies that we only want to consider those records where the `ReportReady` field is marked as FALSE, meaning the reports are not ready.

3. **What it achieves**: The overall measure calculates the total number of distinct employees who have timesheet reports that are still pending or not completed. This can be useful for management to understand how many employees still need to finalize their timesheets, helping to ensure timely reporting and processing.

In summary, `Dax_EmpCount_RReady` provides insight into the number of employees with incomplete timesheet reports, allowing for better tracking and management of reporting processes.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheet entries in a dataset.

### Breakdown of the Expression:

1. **DISTINCTCOUNT**: This function counts the number of distinct (unique) values in a specified column. In this case, it is counting unique entries.

2. **vw_Timesheet[EmployeeName]**: This part specifies the column from which the unique values are being counted. Here, it refers to the `EmployeeName` column in the `vw_Timesheet` table, which contains the names of employees.

### What It Achieves:

- **Unique Employee Count**: The measure calculates how many different employees have logged their hours in the timesheet. For example, if three employees (Alice, Bob, and Charlie) have submitted timesheets, the result of this measure would be 3, even if some employees submitted multiple timesheets.

### Business Implication:

This measure is useful for understanding workforce participation and engagement. By knowing how many unique employees are contributing to timesheets, a business can assess employee involvement, project staffing levels, and resource allocation. It can also help in identifying trends over time, such as whether more employees are becoming involved in projects or if there are fluctuations in workforce engagement.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the average hourly rate for a specific dataset, which in this case is represented by `vw_Timesheet`. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the values in the `SalesAmount` column of the `vw_Timesheet` table. Essentially, it calculates the total revenue or sales generated during the period being analyzed.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the values in the `Hours` column of the same table. This represents the total number of hours worked or logged during that same period.

3. **Division**: The expression then divides the total sales amount by the total hours worked. This gives you the average amount of money earned per hour worked.

### What It Achieves:
The overall result of this DAX expression is the **average hourly rate**. This metric is useful for understanding how much revenue is generated for each hour of work. It can help businesses assess productivity, set pricing strategies, and evaluate employee performance or project profitability. In summary, it provides insights into the efficiency and effectiveness of labor in generating sales.

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
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called `Msr_Budget`, which is used to calculate a budget figure based on sales data, specifically focusing on the sales price in a base currency. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: 
   - The measure starts by determining the current week of the year using the `SELECTEDVALUE` function on the `DimDate[WeekNumberOfYear]` column. This means it looks at the context in which the measure is being evaluated (like a report or dashboard) to find out which week is currently selected.

2. **Calculate Previous Budget**:
   - Next, it calculates a value called `PrevBudget`. This is done using the `CALCULATE` function, which allows for modifying the context of the calculation. 
   - Inside `CALCULATE`, it uses the measure `[Msr_SalesPriceBaseCurrency]`, which presumably represents the sales price in a base currency.
   - The `FILTER` function is applied to the `DimDate` table to include only those weeks that are less than the current week. This means it sums up the sales price for all previous weeks, effectively giving you the total sales price up to the week before the current one.

3. **Return the Appropriate Value**:
   - Finally, the measure uses an `IF` statement to decide what to return:
     - If the current sales price measure (`[Msr_SalesPriceBaseCurrency]`) is not blank (meaning there is actual data for the current week), it returns that current sales price.
     - If the current sales price is blank (indicating no sales data for the current week), it returns the previously calculated budget (`PrevBudget`).

### Summary:
In summary, this measure calculates the sales price for the current week if there is data available. If there is no data for the current week, it falls back to the total sales price from all previous weeks. This allows businesses to have a budget figure that reflects either current performance or historical performance when current data is unavailable.

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
   - The code starts by determining which month is currently selected in the visual (like a chart or table). It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month that the user is focusing on.

2. **Calculate Cumulative Cost**:
   - The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to and including the selected month. 

3. **Using CALCULATE and FILTER**:
   - The `CALCULATE` function is used to change the context in which the data is evaluated. It sums up the `CostAmount` from the `vw_Timesheet` table.
   - The `FILTER` function is applied to the 'DimDate' table to include all months that are less than or equal to the selected month. This means it will consider all costs from January up to the month that the user has selected.

4. **ALLSELECTED**:
   - The `ALLSELECTED` function ensures that any filters applied to the 'DimDate' table (like year or other date filters) are respected while still allowing the measure to calculate the cumulative total for all months up to the selected month.

### Summary:
In summary, this DAX measure calculates the total cost incurred from the start of the year up to the month that the user has selected in the visual. It helps users understand how costs accumulate over time, providing valuable insights into spending trends within a specified timeframe.

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
   - The code starts by determining which month is currently selected in the visual (like a chart or table). It does this by finding the maximum value of the `MonthNumberOfYear` from the `DimDate` table. This means if you are looking at data for March, the `SelectedMonth` would be 3.

2. **Calculate Cumulative Sales**:
   - The `CALCULATE` function is then used to compute the total sales amount (`SUM(vw_Timesheet[SalesAmount])`). This is the core calculation that sums up all sales.

3. **Filter for Relevant Months**:
   - The `FILTER` function is applied to ensure that only the months up to and including the selected month are considered. The `ALLSELECTED('DimDate')` part means that it looks at all the months that are currently selected in the report context, but the filter condition (`'DimDate'[MonthNumberOfYear] <= SelectedMonth`) restricts it to only those months that are less than or equal to the selected month.

4. **Final Result**:
   - The final result of this measure is the total sales amount for all months from the beginning of the year up to and including the selected month. For example, if March is selected, it will sum the sales for January, February, and March.

In summary, this DAX measure helps users understand how sales have accumulated over time, allowing them to see the total sales up to any given month they choose in the visual. This is particularly useful for tracking performance and making informed business decisions based on cumulative sales trends.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the percentage of hours worked by a specific group, project, or employee in relation to the total hours worked across all selected categories. Here's a breakdown of what it does in simple business terms:

1. **TotalValue Calculation**:
   - The first part of the code defines a variable called `TotalValue`. This variable calculates the total number of hours worked by summing up the `Hours` column from the `vw_Timesheet` table.
   - The `CALCULATE` function is used here to modify the context of the calculation. It considers all selected filters for `GroupCat`, `Project`, and `EmployeeName`, meaning it looks at the data based on whatever selections the user has made in their report or dashboard.
   - The `ALLSELECTED` function ensures that the calculation respects the current selections made by the user but ignores any other filters that might be applied to the data.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours worked by the current context (which could be a specific employee, project, or group) by dividing the sum of hours for that context by the `TotalValue` calculated earlier.
   - The `SUM(vw_Timesheet[Hours])` part sums the hours for the current context (the specific group, project, or employee).
   - The `DIVIDE` function is used to perform the division. It takes three arguments: the numerator (the sum of hours for the current context), the denominator (`TotalValue`), and a third argument (0) that specifies what to return if the denominator is zero (to avoid division errors).

### Summary:
In summary, this DAX measure calculates the percentage of hours worked by a specific group, project, or employee compared to the total hours worked across all selected categories. This helps in understanding how much of the total effort is contributed by a particular segment, which can be useful for performance analysis, resource allocation, and reporting.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the project margin for a specific project by subtracting total costs from total sales. Here’s a breakdown of what it does in simple business terms:

1. **Total Sales Calculation**: The expression `sum(vw_Timesheet[SalesAmount])` adds up all the sales amounts recorded in the `vw_Timesheet` table. This represents the total revenue generated from the project.

2. **Total Costs Calculation**: The expression `sum(vw_Timesheet[CostAmount])` adds up all the costs associated with the project, as recorded in the same `vw_Timesheet` table. This represents the total expenses incurred for the project.

3. **Project Margin Calculation**: By subtracting the total costs from the total sales (`sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`), the measure calculates the project margin. This margin indicates how much profit the project has generated after covering its costs.

In summary, this DAX measure provides a clear view of the profitability of a project by showing the difference between what the project earned (sales) and what it spent (costs). A positive result indicates a profitable project, while a negative result suggests a loss.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called `Msr_SalesPriceBaseCurrency`, which essentially sums up the sales prices in a base currency for different projects and clients. Let's break it down step by step in simple business terms:

1. **Data Source**: The calculation is based on a table called `vw_Timesheet`, which likely contains records of timesheets related to various projects and clients.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key dimensions: `ProjectProfile` and `Client`. This means that the calculation will consider each unique combination of project profiles and clients.

3. **Measure Value Calculation**: For each group created by `SUMMARIZE`, the code retrieves a specific value called `SalesPriceBaseCurrency` from another table called `tbl_Project`. The `SELECTEDVALUE` function is used here to get the sales price for the current context (i.e., the specific project and client being evaluated).

4. **Summing Up**: The `SUMX` function then takes the summarized table (which now contains the project-client combinations and their corresponding sales prices) and sums up the `MeasureValue` for all these combinations. 

In summary, this DAX measure calculates the total sales price in the base currency for all projects and clients listed in the `vw_Timesheet`. It effectively aggregates the sales prices, allowing businesses to see the total revenue or cost associated with their projects in a consistent currency.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'Static Total Employees'. Let's break it down in simple business terms:

1. **Purpose**: The measure aims to count the number of unique employees who have a valid contract status as of today.

2. **Components**:
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it ensures that each employee is only counted once, even if they appear multiple times in the data.
   
   - **`ALL(vw_Timesheet)`**: This function removes any filters that might be applied to the `vw_Timesheet` table. By doing this, the calculation considers all records in the table, regardless of any other filters that might be in place (like date filters or department filters). This is important to ensure that the count reflects the total number of employees without any restrictions.

   - **`vw_Timesheet[ContractStatusToday] = "Valid"`**: This condition filters the data to only include employees whose contract status is marked as "Valid" as of today. This means that only employees who are currently active and have a valid contract will be counted.

3. **Overall Calculation**: When you put it all together, this DAX expression calculates the total number of unique employees who have a valid contract status, ignoring any other filters that might limit the data. This is useful for understanding how many employees are currently eligible or active based on their contract status.

In summary, this measure provides a clear count of active employees with valid contracts, helping businesses track their workforce effectively.

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

In simple business terms, this measure calculates the total actual hours worked by employees as recorded in the timesheet. It aggregates all the hours from the `Hours` column, providing a single total that can be used for reporting, analysis, or decision-making regarding labor costs, productivity, or resource allocation.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate the total contracted hours for employees based on their weekly hours recorded in a timesheet. Here’s a breakdown of what it does in simple business terms:

1. **SUMX Function**: This function is used to iterate over a table and perform a calculation for each row, then sum up the results. In this case, it will calculate something for each employee and then add those results together.

2. **VALUES Function**: This function retrieves a unique list of employee names from the `vw_Timesheet` table. Essentially, it creates a list of all the employees who have recorded hours in the timesheet.

3. **MAX Function**: For each employee in the unique list, the `MAX` function looks at the `HoursPerWeek` column in the `vw_Timesheet` table and finds the maximum number of hours recorded for that employee. This means if an employee has different entries for different weeks, it will take the highest number of hours they are contracted to work in a week.

4. **Putting It All Together**: The `SUMX` function then takes the maximum hours per week for each employee (as calculated by the `MAX` function) and adds them all together. 

### What It Achieves:
The overall result of this DAX measure is the total of the maximum contracted hours per week for all employees. This means it gives you a single number that represents the total maximum hours that all employees are contracted to work in a week, based on the highest recorded hours for each individual employee. 

In summary, this measure helps businesses understand their total contracted workforce hours, which can be useful for planning, budgeting, and resource allocation.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two measures: **TotalActualHours** and **TotalContractedHours**. 

Here's a breakdown of what this means in simple business terms:

1. **TotalActualHours**: This measure represents the total number of hours that have actually been worked or logged by employees or resources on a project or task.

2. **TotalContractedHours**: This measure indicates the total number of hours that were agreed upon in a contract for the project or task. It reflects the expected or planned hours that should be worked based on the contract terms.

3. **The Calculation**: The expression `[TotalActualHours] - [TotalContractedHours]` subtracts the total contracted hours from the total actual hours. 

4. **What It Achieves**: 
   - If the result is positive, it means that more hours were worked than what was contracted, indicating potential overwork or additional effort beyond what was planned.
   - If the result is negative, it means that fewer hours were worked than what was contracted, suggesting that the project or task is under budget in terms of hours.
   - If the result is zero, it indicates that the actual hours worked match the contracted hours perfectly.

In summary, this measure helps businesses understand how actual work hours compare to what was expected or agreed upon, which can be crucial for project management, budgeting, and resource allocation.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `TotalHoursTracked`. Here's a simple explanation of what it does:

1. **Check for Blank Values**: The measure first checks if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Zero if Blank**: If the sum of hours is blank (meaning no hours have been tracked), the measure returns a value of `0`. This ensures that when there are no recorded hours, the measure does not show a blank value, which could be confusing in reports.

3. **Return the Sum if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the measure simply returns the total sum of hours tracked from the `vw_Timesheet` table.

### Summary:
In business terms, this measure calculates the total hours tracked by employees. If no hours have been logged, it returns zero instead of leaving the value blank. This makes it easier to understand and interpret the data in reports, as it provides a clear indication of hours worked or tracked, even when there are none.

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

1. **Check for Blank Values**: The measure first checks if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Value Based on Check**:
   - If the sum of hours is blank (meaning no hours have been tracked), the measure returns the text "Empty". This indicates that there are no recorded hours for the selected context (like a specific project, employee, or time period).
   - If there are hours recorded (the sum is not blank), it simply returns the total number of hours tracked.

### Summary:
In essence, this measure helps to identify whether any hours have been logged in the timesheet. If no hours are logged, it clearly indicates that with the word "Empty". If hours are logged, it provides the total number of hours tracked. This is useful for reporting and analysis, as it helps users quickly understand if there is missing data in their timesheet records.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates a measure called `trackedDiff`, which essentially determines the difference between two values related to hours tracked for employees. Here’s a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked by an employee over a specific period.

2. **Maximum Hours Per Week**: The next part of the calculation uses the `CALCULATE` function to find the maximum number of hours that an employee has recorded in a single week. This is done using:
   - `MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])`: This part looks at the distinct values of hours recorded per week and finds the maximum value among them.
   - `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])`: This part ensures that the calculation only considers the data for the specific employee (identified by `EmployeeID`), ignoring any other filters that might be applied to the data.

3. **Calculating the Difference**: Finally, the measure subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. 

### What It Achieves:
- The result of this measure, `trackedDiff`, tells you how many hours an employee has tracked beyond their maximum weekly hours. 
- If the result is positive, it indicates that the employee has logged more hours than their highest recorded week, suggesting they may be working extra hours. If it’s zero or negative, it means they have not exceeded their maximum weekly hours.

In summary, `trackedDiff` helps in assessing whether employees are working more hours than their typical maximum, which can be useful for workload management and ensuring employee well-being.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided calculates a measure called `trackedDiff2`. Let's break it down into simpler terms to understand what it achieves.

### What It Calculates:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been tracked for a specific context (like a specific employee or time period).

2. **Maximum Hours Per Week**: The next part of the calculation uses the `CALCULATE` function to find the maximum number of hours tracked per week from the `vw_Timesheet` table. This is done using:
   - `MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])`: This part looks at all the unique values of `HoursPerWeek` and finds the highest value among them.

3. **Context Adjustments**:
   - `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])`: This removes any filters that might be applied to the `HoursPerWeek` column, ensuring that the calculation considers all possible values of hours per week, regardless of any specific filtering.
   - `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])`: This keeps the context for each employee while ignoring other filters. It means that the maximum hours per week will be calculated for each employee individually.

### Final Calculation:

The final result of the measure `trackedDiff2` is the difference between the total hours tracked and the maximum hours tracked per week for each employee. 

### Business Implications:

- **Performance Measurement**: This measure helps in assessing how much an employee's tracked hours deviate from their maximum potential hours per week. 
- **Identifying Gaps**: If the result is positive, it indicates that the employee has tracked fewer hours than their maximum capacity, which could highlight underutilization or potential areas for increased productivity.
- **Resource Management**: Managers can use this information to make informed decisions about workload distribution, project assignments, or identifying employees who may need additional support or motivation.

In summary, `trackedDiff2` provides insights into employee productivity by comparing actual tracked hours against the maximum hours they could potentially work in a week.

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

1. **Variable Creation (FilteredHours)**: The code starts by creating a variable called `FilteredHours`. This variable is used to store a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**: The filtering process looks for specific conditions in the timesheet records:
   - It includes records where the project qualifies for billing (`vw_Timesheet[QualifyPrj] = 1`) and is marked as billable (`vw_Timesheet[BillablePrj] = 1`).
   - Additionally, it includes any records where the project name contains the phrase "Customer Success Services". This is done using the `SEARCH` function, which checks if that phrase appears in the project name.

3. **Calculating Total Hours**: After filtering the timesheet data based on the above criteria, the code then calculates the total hours worked on these filtered records. This is done using the `SUMX` function, which sums up the `Hours` column from the filtered dataset.

4. **Final Output**: The final result of this measure, `UTI_TotalBillableHours`, is the total number of hours that are billable based on the specified conditions. This measure helps the business understand how many hours can be billed to clients, focusing on qualifying projects and specific services.

In summary, this DAX measure effectively calculates the total billable hours from timesheet entries that either qualify for billing or are related to customer success services, providing valuable insights into billable work for the organization.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `UTI_TOTALHOURS`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the total number of hours worked on specific projects that meet a certain qualification criteria.

2. **Components**:
   - **SUM(vw_Timesheet[Hours])**: This part of the code adds up all the values in the `Hours` column from the `vw_Timesheet` table. Essentially, it totals the hours recorded in timesheets.
   - **CALCULATE**: This function modifies the context in which the data is evaluated. It allows you to apply filters to the data before performing the calculation.
   - **vw_Timesheet[QualifyPrj] = 1**: This is a filter condition. It specifies that only the rows in the `vw_Timesheet` table where the `QualifyPrj` column equals 1 should be included in the total. In other words, it focuses on projects that are marked as qualified.

3. **Outcome**: The measure `UTI_TOTALHOURS` will return the total hours worked on projects that are considered qualified (where `QualifyPrj` is 1). This is useful for reporting and analysis, as it helps stakeholders understand how many hours are being dedicated to projects that meet specific criteria.

In summary, this DAX measure helps businesses track and analyze the total hours spent on qualified projects, enabling better resource management and decision-making.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is a measure named 'UTILIZATION_New'. Let's break it down into simple business terms to understand what it calculates or achieves.

### Key Components of the Measure:

1. **Variables Defined**:
   - **_TotalBillableHours**: This variable captures the total number of hours that are billable to clients. It is represented by another measure called `[UTI_TotalBillableHours]`.
   - **_TotalHours**: This variable captures the total number of hours worked, represented by the measure `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The measure uses a series of conditional checks (using the `IF` function) to determine what value to return.
   - **Check for Active Contacts**: The first condition checks if there are any active contacts (using `[Msr_ActiveContacts]`). If there are no active contacts, the measure will not proceed further.
   - **Check for Total Hours**: If there are active contacts, it then checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the measure will not proceed further.
   - **Calculate Utilization**: If both previous checks pass, it calculates the utilization rate by dividing the total billable hours by the total hours worked. 
     - If this division results in a blank value (which can happen if total hours are zero), it returns 0 instead of a blank.

### What It Achieves:

In summary, this measure calculates the **utilization rate** of resources (like employees or contractors) by determining the proportion of hours that are billable compared to the total hours worked. 

- **Utilization Rate**: This is a key performance indicator in many businesses, especially in service industries, as it helps assess how effectively time is being used. A higher utilization rate indicates that more of the available working hours are being spent on billable work, which is generally a positive sign for profitability.

### Final Output:

- If there are active contacts and total hours are available, it provides a percentage (or ratio) of billable hours to total hours.
- If there are no active contacts or total hours are blank, it returns 0, indicating that utilization cannot be calculated under those circumstances.

This measure is crucial for understanding and improving operational efficiency and profitability in a business context.

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

