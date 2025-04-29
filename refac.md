# Power BI Model & Report Documentation

*Generated on: 2025-04-29 12:25:35*

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

The Power BI data model appears to be designed for a project management and time tracking system, likely within a corporate or organizational context. It integrates various dimensions and fact tables to facilitate comprehensive reporting and analysis of employee hours, project performance, and timesheet compliance. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a focus on capturing and monitoring timesheet submissions, while the 'lkp_' tables (lookup tables) provide essential metadata for projects, units, and codes, enhancing the model's ability to categorize and analyze data effectively.

The structure of the model indicates a star schema, where 'vw_Timesheet' serves as a central fact table linked to multiple dimension tables, including 'DimDate' for time-related analysis, 'lkp_Project' for project-specific insights, and 'lkp_Unit' for organizational unit tracking. The inclusion of 'tbl_Project' further supports project-level analysis, allowing users to drill down into specific projects and their associated timesheet data. Overall, this data model is likely aimed at improving operational efficiency, ensuring accurate time reporting, and providing insights into resource allocation and project management within the organization.

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

The 'DimDate' table serves as a comprehensive date dimension for analytical reporting, enabling businesses to perform time-based analysis by providing essential date attributes such as day, week, month, and quarter. This table facilitates trend analysis, seasonality assessments, and time-series comparisons across various business metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting within the dataset. |
| `DateKey` | `int64` | Column Description: The 'DateKey' column (int64) serves as a unique identifier for each date in the 'DimDate' table, facilitating efficient date-based queries and data enrichment processes. |
| `DayName` | `string` | The 'DayName' column in the 'DimDate' table stores the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | Column Description: The 'DayNumberOfMonth' column represents the numerical day of the month (ranging from 1 to 31) for each date entry in the 'DimDate' table, facilitating date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | Column Description: The 'DayNumberOfWeek' column represents the numerical value of the day in the week (1 for Sunday through 7 for Saturday), facilitating date-related analyses and reporting within the DimDate table. |
| `DayNumberOfYear` | `int64` | Column Description: The 'DayNumberOfYear' column represents the sequential day of the year (ranging from 1 to 365 or 366) for each date entry in the DimDate table, facilitating time-based analyses and calculations. |
| `MonthName` | `string` | The 'MonthName' column in the 'DimDate' table provides the full name of each month, facilitating time-based analysis and reporting by enhancing date-related data with clear, human-readable month identifiers. |
| `MonthNumberOfYear` | `int64` | The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month (1 for January through 12 for December), facilitating time-based analysis and reporting. |
| `ShortDate` | `dateTime` | The 'ShortDate' column in the 'DimDate' table represents a simplified date format, capturing the specific date without time details to facilitate efficient date-based analysis and reporting. |
| `WeekNumberOfYear` | `int64` | Column Description: Represents the week number of the year (1-52) for each date in the DimDate table, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** The 'CurrentYearFlag' column indicates whether a date falls within the current calendar year, using a string value to denote 'Yes' or 'No' for easy identification in date-related analyses.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `CurrentYearFlag` in a data model, typically within a tool like Power BI. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The `CurrentYearFlag` column is designed to indicate whether a date in the `DimDate` table falls within the current year or not.

2. **How it Works**:
   - **YEAR(DimDate[ShortDate])**: This part extracts the year from the date in the `ShortDate` column of the `DimDate` table.
   - **YEAR(TODAY())**: This part gets the current year based on today's date.
   - **IF Statement**: The `IF` function checks if the year extracted from `DimDate[ShortDate]` is less than or equal to the current year. 
     - If it is (meaning the date is in the current year or earlier), it returns `1`.
     - If it is not (meaning the date is in a future year), it returns `0`.

3. **Outcome**: 
   - The result is a column where each row will have a value of `1` if the date is in the current year or earlier, and `0` if the date is in a future year. 
   - This flag can be useful for filtering or analyzing data based on whether dates are relevant to the current year.

In summary, the `CurrentYearFlag` column helps identify which dates are in the current year or earlier, allowing for easier analysis of time-sensitive data.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table captures and categorizes various metrics related to hours worked and their corresponding percentage contributions, enabling businesses to analyze workforce productivity and resource allocation effectively. This table serves as a critical tool for performance assessment and strategic decision-making regarding labor management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column stores string representations of hours worked alongside their corresponding percentage of total hours, facilitating the analysis of employee time allocation and productivity metrics. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and their corresponding percentage calculations, facilitating data enrichment and analysis within the 'Hours_nd_Percentage' table. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data entries, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, facilitating data consistency and integrity. It includes attributes for qualification criteria and sorting preferences, enabling efficient data retrieval and analysis in reporting and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column in the 'lkp_Code' table stores unique string identifiers that facilitate the enrichment of data by linking it to corresponding descriptive attributes or categories. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numerical identifier to categorize data for enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table contains string values that determine the order in which records are displayed or processed, facilitating organized data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference dataset for employee information, enabling efficient filtering and lookup of employee details such as their unique identifiers and names. This table supports data analysis and reporting by providing a structured way to access employee-related data across various business processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column stores unique identifiers for employees, facilitating data enrichment and integration within the employee lookup table. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, facilitating the enrichment of data by providing identifiable references for each individual in the employee lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, providing essential details such as project names, qualification metrics, billing amounts, and associated groups, enabling effective project management and financial analysis within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing amount associated with each project, facilitating financial tracking and reporting. |
| `Group` | `string` | The 'Group' column in the 'lkp_Project' table categorizes projects into distinct classifications to facilitate data enrichment and analysis. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the organization. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of each project, serving as a numerical identifier for categorizing projects based on their eligibility criteria. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their characteristics such as billable department status and external affiliations. This table is essential for analyzing resource allocation and financial performance across different units, facilitating informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column (int64) in the 'lkp_Unit' table indicates the department responsible for billable activities, facilitating accurate tracking and reporting of revenue-generating units. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column identifies the organizational unit associated with each entry in the lkp_Unit table, facilitating data enrichment and classification. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores external type identifiers for owned sub-units, facilitating data enrichment and integration processes. |
| `Unit` | `string` | The 'Unit' column in the 'lkp_Unit' table stores the names of measurement units used for data enrichment, facilitating consistent reference across various datasets. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, status, leadership, and financial aspects. This table enables effective project management and oversight by providing stakeholders with insights into project progress, categorization, and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and performance analysis within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column in the 'tbl_Project' table captures the administrative details and oversight responsibilities associated with each project, facilitating effective management and governance. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates and changes in the project lifecycle. |
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each project, facilitating easy identification and management of client relationships within the project data. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating efficient tracking and management of client-related data within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated, providing a timestamp for tracking project creation within the tbl_Project table. |
| `Currency` | `string` | The 'Currency' column specifies the type of currency used for financial transactions related to each project in the tbl_Project table. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the timestamp of when each project entry was initially created, providing a reference for tracking project timelines and historical data. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or business unit associated with each project in the 'tbl_Project' table, facilitating targeted analysis and reporting. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column indicates the scheduled date and time for the completion of the project, facilitating effective project timeline management and tracking. |
| `Project` | `string` | The 'Project' column stores the names of various projects, enabling the identification and categorization of data related to specific initiatives within the tbl_Project table. |
| `ProjectDescription` | `string` | Column Description: A detailed narrative outlining the objectives, scope, and key elements of the project, facilitating a clear understanding of its purpose and goals. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the category or classification of a project within the 'tbl_Project' table, facilitating organization and management of related projects. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a detailed textual overview of the project group, facilitating better understanding and categorization of related projects within the 'tbl_Project' table. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for linking related data and ensuring data integrity across the project management system. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the project's execution and management within the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency sales price of a project, stored as a decimal to facilitate precise financial calculations and reporting. |
| `Startdate` | `dateTime` | The 'Startdate' column in the 'tbl_Project' table captures the date and time when a project is initiated, serving as a key reference point for project timelines and scheduling. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, helping stakeholders assess project health and prioritize resources effectively. |
| `Statuscode` | `string` | The 'Statuscode' column in the 'tbl_Project' table indicates the current operational status of each project, facilitating effective tracking and management of project progress. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with each project, expressed as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table serves to identify and analyze instances of missing timesheet submissions within the organization, providing key insights into employee attendance and compliance. It includes essential details such as the time period, employee and manager information, and contract specifics, enabling management to address gaps in timesheet reporting and ensure accurate payroll processing.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by employees that have not been recorded in their timesheets, facilitating accurate payroll and resource management. |
| `Approved` | `boolean` | Indicates whether the timesheet has been approved (true) or not (false) in the context of tracking missing timesheet submissions. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column indicates the date and time when an employee's contract is set to expire, providing essential information for managing timesheet submissions and contract renewals. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to facilitate the identification of discrepancies in timesheet submissions within the 'vw_missing_timesheet' table. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of employee contracts, providing essential context for identifying missing timesheet entries in the 'vw_missing_timesheet' table. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on absent time entries. |
| `Hours_` | `double` | The 'Hours_' column represents the total number of hours recorded for each employee's timesheet entry, stored as a double to accommodate fractional hours for precise time tracking. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | Column Description: The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating the tracking and accountability of employee time reporting. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees' timesheet submissions, facilitating accountability and tracking within the 'vw_missing_timesheet' view. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee work hour reporting. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the missing timesheet records, facilitating the tracking and management of time allocation across various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project, facilitating the tracking and management of timesheet entries associated with specific projects in the 'vw_missing_timesheet' table. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true signifying rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet data is complete and ready for reporting, with a value of true signifying readiness and false indicating further action is needed. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double to accommodate precise financial calculations within the 'vw_missing_timesheet' table. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double to accommodate precise pricing information within the context of timesheet data analysis. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date and time associated with missing timesheet entries, facilitating the identification and resolution of discrepancies in employee time reporting. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific organizational subunit associated with each entry in the missing timesheet records, facilitating targeted analysis and resolution of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains unique identifiers for each timesheet entry, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' view. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a textual summary of the tasks or activities recorded in the timesheet, facilitating better understanding and analysis of employee work hours in the 'vw_missing_timesheet' table. |
| `Week_` | `int64` | The 'Week_' column (int64) in the 'vw_missing_timesheet' table represents the week number of the year associated with each timesheet entry, facilitating the identification and analysis of missing timesheet submissions by week. |
| `Year_` | `int64` | Column Description: The 'Year_' column (int64) represents the calendar year associated with each entry in the 'vw_missing_timesheet' table, facilitating the analysis of timesheet data over time. |

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `BillableDep`. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the `lkp_Unit` table for the column `BillableDep`. This is done using the `RELATED` function, which pulls in data from a related table based on existing relationships in the data model.

2. **Handle Blank Values**: 
   - If the `BillableDep` value from the related table is blank (meaning there is no corresponding value), the expression returns `1`. This could indicate that the absence of a value is treated as a positive condition for whatever analysis is being performed.

3. **Check for Non-Zero Values**: 
   - If there is a value present (i.e., it is not blank), the code then checks if this value is not equal to zero. 
   - If the value is not zero, it again returns `1`, suggesting that this condition is also considered favorable or billable.
   - If the value is zero, it returns `0`, indicating that this condition is not billable.

### Summary:
In summary, this DAX expression effectively categorizes entries based on the `BillableDep` value from a related table. It returns `1` if:
- There is no related `BillableDep` value (blank), or
- The related `BillableDep` value is a non-zero number.

It returns `0` only when the related `BillableDep` value is exactly zero. This logic helps in identifying which entries are considered "billable" based on the presence and value of the `BillableDep` field.

**`CC_ActiveEmployees`** (`string`)

- **Description:** The 'CC_ActiveEmployees' column contains a string representation of the active employees in the cost center, providing essential data for identifying personnel associated with missing timesheet entries.
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
   - **End Date Check**: Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. This means the employee's contract must still be valid on this date. However, if the `ContractEndDate` is blank (meaning the employee has no specified end date), the employee is still considered active.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the end date is blank), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns a value of `0`, indicating that the employee is not active.

### Summary:
In summary, this DAX expression effectively flags employees as "active" (1) or "not active" (0) based on whether the date being evaluated falls within their contract period. This is useful for reporting and analysis, allowing businesses to easily identify which employees are currently active based on their contractual agreements.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to facilitate the identification of missing or unsubmitted timesheet data in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called 'IncompleteFlag' in a data model, specifically in a table named `vw_missing_timesheet`. Here's a simple explanation of what this code does:

### Explanation of the DAX Expression:

- **Condition Check**: The expression checks the value in the column `MissingHours` for each row in the `vw_missing_timesheet` table.
- **Logic**: 
  - If the value of `MissingHours` is **less than 0**, it means that there is an issue with the recorded hours (perhaps indicating that hours are missing or incorrectly logged).
  - In this case, the expression returns **1**, which can be interpreted as a flag indicating that the entry is incomplete or problematic.
  - If the value of `MissingHours` is **0 or greater**, it returns **0**, indicating that there are no issues with the hours recorded.

### What It Achieves:

- **Flagging Incomplete Entries**: This calculated column effectively flags any entries where the `MissingHours` is negative, allowing users to easily identify and address incomplete or incorrect timesheet submissions.
- **Data Quality Improvement**: By highlighting these entries, the organization can take action to ensure that all timesheets are complete and accurate, which is crucial for payroll and project management.

In summary, the 'IncompleteFlag' column helps in identifying problematic timesheet entries by marking them with a '1' when there are negative missing hours, thus facilitating better oversight and management of timesheet data.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit associated with each entry in the missing timesheet records, facilitating targeted analysis and reporting on timesheet compliance.
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
   - If there is no related value (meaning the 'Unit' field is blank or missing), the code will return the text "Unknown". This is a way to handle situations where the expected data is not available, ensuring that the report or analysis does not show empty or confusing values.
   - If there is a related value present, it will return that value from the 'Unit' field in the 'lkp_Unit' table.

3. **Purpose**: The overall purpose of this calculated column is to provide a clear and understandable representation of the 'Unit' associated with each row of data. If the unit is not available, it clearly indicates that with the term "Unknown", which helps users quickly identify missing information.

In summary, this DAX expression ensures that every row in the 'MAIN_UNIT' column either shows the corresponding unit name or indicates that the unit is unknown, improving data clarity and usability.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column (string) in the 'vw_missing_timesheet' table represents the specific year and week number for tracking timesheet submissions, facilitating the identification of periods with missing entries.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'YearWeek' in a data model, and it combines two pieces of information: the year and the week number of a specific date, which is represented by the column `[ContractEndDate]`.

Here's a breakdown of what it does:

1. **YEAR([ContractEndDate])**: This part extracts the year from the date in the `[ContractEndDate]` column. For example, if the date is December 15, 2023, this function will return `2023`.

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the date in `[ContractEndDate]`. For instance, if the date is December 15, 2023, it might return `50`, indicating that this date falls in the 50th week of the year.

3. **FORMAT(WEEKNUM([ContractEndDate]), "00")**: This part formats the week number to ensure it always has two digits. So, if the week number is `5`, it will be displayed as `05`. This is important for consistency, especially when sorting or displaying the week numbers.

4. **Concatenation**: The `&` operator is used to concatenate (join together) the year and the formatted week number with a hyphen in between. So, if the year is `2023` and the week number is `50`, the final result will be `2023-50`.

### In Summary:
The DAX expression creates a new column that shows the year and week number of the contract's end date in the format "YYYY-WW". For example, if the contract ends on December 15, 2023, the calculated column 'YearWeek' will display `2023-50`. This format is useful for reporting and analysis, allowing users to easily group or filter data by year and week.

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

1. **Purpose**: This measure counts the number of unique employees (consultants) who have recorded negative hours in a timesheet.

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows you to apply filters to your calculations.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the number of unique employee names in the `vw_missing_timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This filter condition specifies that only the records where the `MissingHours` are less than zero should be considered. In other words, it focuses on cases where employees have negative hours recorded.

3. **Outcome**: The measure ultimately provides a count of how many different employees have negative hours logged in their timesheets. This could indicate issues such as underreporting of hours worked or other discrepancies that need to be addressed.

In summary, `Count_Negative_Consultant` helps the business identify how many unique consultants have reported negative hours, which can be crucial for understanding timesheet accuracy and ensuring proper resource management.

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
   - **CALCULATE**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID])**: This part counts the number of unique Employee IDs in the 'vw_missing_timesheet' table. Essentially, it tells us how many different employees are involved.
   - **'vw_missing_timesheet'[MissingHours] < 0**: This is a filter condition that specifies we are only interested in records where the 'MissingHours' value is less than zero. This means we are looking for instances where employees have negative missing hours, which could indicate an error or an unusual situation.

3. **Outcome**: The measure ultimately provides a count of how many distinct employees have negative missing hours recorded. This can help a business identify potential issues with timesheet entries, such as errors in reporting hours worked or discrepancies that need to be addressed.

In summary, **CountNegativeMissingHours** helps the organization track and manage timesheet accuracy by identifying employees with negative hour entries, which could signal a need for review or correction.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the number of active employees who are missing timesheet hours for a specific period. Let's break it down into simpler terms:

1. **Data Source**: The code uses a table called `vw_missing_timesheet`, which likely contains records of employees, their hours worked, and their contract hours.

2. **Summarization**: The `SUMMARIZE` function creates a new table that groups the data by employee name, year, week, unit, and subunit. For each group, it calculates two key pieces of information:
   - **MissingHours**: This is calculated by taking the total hours worked by the employee in that week and subtracting their contracted hours. If this value is negative, it indicates that the employee has not logged enough hours.
   - **MinActiveEmp**: This captures the minimum value of a field called `CC_ActiveEmployees`, which likely indicates whether the employee is active (1) or inactive (0). 

3. **Filtering**: The `FILTER` function then narrows down this summarized data to only include:
   - Employees who are active (`MinActiveEmp` = 1).
   - Employees who have negative missing hours (`MissingHours` < 0), meaning they have not logged enough hours compared to their contract.

4. **Counting**: Finally, the `COUNTROWS` function counts how many employees meet these criteria, resulting in the total number of active employees who are missing timesheet hours.

### Summary:
In essence, this DAX measure calculates how many active employees have not logged enough hours in their timesheets for a given week, which can help the business identify potential issues with timesheet compliance or employee workload.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate a measure called `Dax_MissingHours`, which focuses on identifying and summing up the missing hours for active employees in a specific context (like a week or year). Here’s a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. This variable filters a summarized table (`vw_missing_timesheet`) to focus on employees who are currently active. It does this by checking a specific condition: it looks for employees who have a minimum count of active status (`MinActiveEmp`) equal to 1.

2. **Calculate Missing Hours**: For each employee, the code calculates the "MissingHours" by taking the total hours they have logged (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contract hours they are supposed to work (`MAX(vw_missing_timesheet[ContractHours])`). This calculation helps to determine how many hours an employee is missing based on their expected workload.

3. **Filter for Negative Missing Hours**: The filtering condition also ensures that only those employees who have negative missing hours (meaning they have logged fewer hours than expected) are included in the final calculation. This is done by checking if `MissingHours` is less than 0.

4. **Sum Up Missing Hours**: Finally, the `SUMX` function is used to sum up the "MissingHours" for all the active employees who meet the criteria. This gives a total of how many hours are missing across all relevant active employees.

### In Summary:
The `Dax_MissingHours` measure calculates the total number of hours that active employees are missing based on their logged hours versus their expected contract hours. It focuses only on those employees who are currently active and have logged fewer hours than they should have, providing valuable insights into potential staffing or workload issues.

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
- **DAX Explanation (Generated):** Certainly! Let's break down the DAX code snippet step by step to understand what it calculates and achieves in business terms.

### Overview of the Code
The DAX expression is designed to calculate the total expected contract hours for employees on a weekly basis. It does this by summarizing data from a table called `vw_missing_timesheet`.

### Breakdown of the Components

1. **SUMMARIZE Function**:
   - This function is used to create a summary table from the `vw_missing_timesheet` data.
   - It groups the data by two key columns: `Week_` (which represents the week of the year) and `EmployeeID` (which identifies each employee).
   - For each unique combination of week and employee, it calculates the maximum value of `ContractHours`. This means that if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

2. **MAX(vw_missing_timesheet[ContractHours])**:
   - This part of the code ensures that for each employee in a given week, only the highest contract hours are considered. This is useful if there are discrepancies or multiple entries for the same week.

3. **SUMX Function**:
   - After creating the summarized table, the `SUMX` function iterates over this table and sums up the calculated "ContractHours" for each employee across all weeks.
   - Essentially, it adds up the maximum contract hours for each employee for each week, resulting in a total expected contract hours figure.

### What It Achieves
In simple terms, this DAX measure calculates the total expected contract hours for all employees, ensuring that for any given week, if an employee has multiple records, only the highest contract hours are counted. This helps in understanding the total expected workload for employees on a weekly basis, which can be crucial for resource planning, payroll calculations, and ensuring that employees are meeting their contractual obligations.

### Summary
The measure `ExpectedContractHoursWeekly` provides a clear view of how many hours employees are expected to work each week, taking into account the highest recorded hours for any discrepancies in the data. This is valuable for management to ensure proper staffing and workload distribution.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the difference between two values related to hours worked and contracted hours for a specific context, likely within a reporting or analytical framework.

Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum value of "ContractHours" from the `vw_missing_timesheet` table. "ContractHours" represents the total number of hours that an employee is contracted to work.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum value of "Hours_" from the same table. "Hours_" represents the actual hours that the employee has worked or reported.

3. **Calculation**: The expression then subtracts the maximum actual hours worked (Hours_) from the maximum contracted hours (ContractHours). 

### What It Achieves:
- The result of this calculation gives you the number of hours that are missing or unaccounted for. In other words, it tells you how many hours an employee was supposed to work (based on their contract) but did not report or log as worked.

### Business Implication:
- This measure can help management identify discrepancies between expected and actual work hours. It can be useful for tracking employee performance, ensuring compliance with work contracts, and identifying potential issues with time reporting. If the result is a positive number, it indicates that there are missing hours that need to be addressed.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates a measure called `PercentageCompleteness`. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part of the code adds up all the missing hours recorded in the `vw_missing_timesheet` table. Essentially, it tells us how many hours were not logged or completed.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that are expected to be worked in a week according to the contract. It serves as a benchmark for what is considered "complete."

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the code calculates the difference between the total missing hours and the expected contract hours. If the result is positive, it indicates that the missing hours exceed what was expected, suggesting a shortfall in work completed.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The `DIVIDE` function takes the result from the previous step (the difference between missing hours and expected hours) and divides it by the expected contract hours. This gives us a ratio or percentage that indicates how much of the expected work is missing. The third argument, `0`, ensures that if the expected hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

### Summary:
In summary, this DAX measure calculates the percentage of expected work hours that are missing. A positive result indicates a shortfall in hours worked compared to what was expected, while a negative result would suggest that the hours worked exceeded expectations. This measure helps in assessing the completeness of timesheet submissions and understanding workforce productivity.

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

1. **Data Source**: The measure is based on a table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including information about whether their submissions are complete or incomplete.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by three key columns:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This is the name of the employee.
   - `MAIN_UNIT`: This likely refers to the department or unit the employee belongs to.

   For each unique combination of these three columns, the code creates a new table that includes a calculated column called `"IncompleteFlag"`. This flag indicates whether there is any incomplete timesheet for that specific employee in that week and unit. The `MAX` function is used here to determine the highest value of the `IncompleteFlag` for each group, which effectively checks if there is at least one incomplete timesheet (assuming `IncompleteFlag` is a binary indicator).

3. **Calculating the Total**: After summarizing the data, the `SUMX` function iterates over the summarized table. It sums up the values of the `IncompleteFlag` for each group. This means it counts how many unique employee-week-unit combinations have an incomplete timesheet.

### What It Achieves:
In summary, this DAX measure calculates the total number of unique employees who have at least one incomplete timesheet for each unit during a specific week. This information can be crucial for management to identify areas where employees may need support in submitting their timesheets on time, ensuring better compliance and operational efficiency.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and management identifiers. This data enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column in the 'vw_Timesheet' table identifies the specific client associated with each timesheet entry, facilitating accurate billing and project tracking. |
| `Code2` | `string` | The 'Code2' column in the 'vw_Timesheet' table represents a secondary identifier or classification code associated with each timesheet entry, facilitating enhanced data categorization and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is set to expire, providing essential information for managing workforce availability and planning. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and project billing periods. |
| `ContractStatusToday` | `string` | Column Description: Indicates the current status of the contract associated with the timesheet entry, represented as a string value. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, providing essential insights for performance tracking and decision-making within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the total monetary cost associated with the recorded time entries in the 'vw_Timesheet' table, expressed as a double-precision floating-point number. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, expressed as a double to allow for precise financial calculations. |
| `Currency` | `string` | The 'Currency' column indicates the type of currency used for financial entries in the timesheet, facilitating accurate cost tracking and reporting. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the individual or entity responsible for payment, facilitating the tracking and management of outstanding invoices within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_Timesheet' table uniquely identifies each employee, facilitating the tracking and management of their respective timesheet entries. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column stores the unique identifier and name of each employee, facilitating the association of timesheet entries with the corresponding personnel in the vw_Timesheet table. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating the identification and tracking of individual work hours and contributions. |
| `Employer` | `string` | The 'Employer' column in the 'vw_Timesheet' table captures the name of the organization for which the employee is working, facilitating accurate tracking and reporting of labor costs associated with each employer. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating accurate payroll processing and reporting. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to each timesheet entry, providing insights that enhance the understanding of the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with each timesheet entry, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, facilitating accurate tracking of labor input for payroll and project management. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating analysis of labor allocation and productivity. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or vacation, to facilitate accurate reporting and analysis of employee time allocation. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee time allocation. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry processes. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries, facilitating accountability and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the names of individuals associated with specific IPM IDs, facilitating the tracking and management of timesheet entries within the vw_Timesheet table. |
| `JobTitle` | `string` | The 'JobTitle' column in the 'vw_Timesheet' table captures the specific role or position of an employee, providing essential context for analyzing timesheet data and resource allocation. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking timesheet records to related datasets, facilitating efficient data integration and analysis. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and reporting within the organization. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column stores the names of managers associated with each timesheet entry, facilitating easy identification of managerial oversight for employee hours logged. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key of the manager responsible for overseeing the associated timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's manager, providing essential context for timesheet approvals and project oversight within the vw_Timesheet table. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries, detailing the hours allocated to specific tasks within the timesheet records. |
| `Project` | `string` | The 'Project' column in the 'vw_Timesheet' table identifies the specific project associated with each recorded time entry, facilitating accurate tracking and reporting of resource allocation and project costs. |
| `ProjectCode` | `string` | The 'ProjectCode' column in the 'vw_Timesheet' table uniquely identifies the specific project associated with each timesheet entry, facilitating accurate tracking and reporting of time spent on various projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that identifies the specific project associated with each timesheet entry, enabling detailed tracking and analysis of time allocation across various projects. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the associated project profile for each timesheet entry, facilitating accurate project tracking and reporting. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, enabling better analysis of resource allocation and project performance. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet is ready for reporting, with a value of true signifying readiness and false indicating further action is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double data type to accommodate precise financial calculations within the 'vw_Timesheet' table. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions recorded in the timesheet, stored as a double to accommodate precise financial calculations. |
| `Status` | `string` | The 'Status' column in the 'vw_Timesheet' table indicates the current state of each timesheet entry, reflecting whether it is submitted, approved, or pending review. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, helping to differentiate between various levels of project urgency or resource allocation. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for each timesheet entry, facilitating the tracking and management of employee work hours and project allocations. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the work performed during the recorded timesheet entry, enhancing clarity and context for project tracking and billing purposes. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table represents the measurement unit associated with the recorded time entries, facilitating accurate tracking and reporting of work hours across various projects. |
| `UnitCode` | `string` | The 'UnitCode' column in the 'vw_Timesheet' table represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating accurate tracking and reporting of labor costs by department. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and data enrichment within the timesheet management system. |
| `YearWeek` | `string` | The 'YearWeek' column (string) in the 'vw_Timesheet' table represents the year and week number of the timesheet entries, facilitating time-based analysis and reporting of work hours. |

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
   - The first condition checks if the column `[Approved]` is set to `TRUE()`. This means that if the timesheet is marked as approved, it meets the criteria.
   - The second condition checks if the `TimesheetCode` from the `vw_Timesheet` table is either "V" or "Z". This means that if the timesheet has a code of "V" or "Z", it also meets the criteria for being considered approved.

3. **Output**:
   - If either of the conditions is met (the timesheet is approved or has a code of "V" or "Z"), the calculated column will return a value of `1`. This indicates that the timesheet is approved.
   - If neither condition is met, it will return a value of `0`, indicating that the timesheet is not approved.

In summary, this DAX expression effectively flags timesheets as approved (1) if they are explicitly marked as approved or if they have specific codes ("V" or "Z"). If neither condition is true, it flags them as not approved (0). This can help in reporting and analysis by easily identifying which timesheets are considered approved based on these criteria.

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

1. **Check for Related Data**: The code first checks if there is a related value in the `BillableDep` column from the `lkp_Unit` table. This is done using the `RELATED` function, which pulls in data from a related table based on the relationships defined in your data model.

2. **Handle Blank Values**: If the `BillableDep` value from the related table is blank (meaning there is no corresponding entry), the formula returns a value of `1`. This could indicate that, in the absence of specific information, the default assumption is that the item is billable.

3. **Check for Non-Zero Values**: If the `BillableDep` value is not blank, the formula then checks if this value is not equal to zero. If it is not zero, it again returns `1`, suggesting that the item is billable.

4. **Return Zero for Zero Values**: If the `BillableDep` value is zero, the formula returns `0`, indicating that the item is not billable.

### Summary:
In summary, this DAX expression effectively categorizes items as billable or not based on the `BillableDep` value from a related table. It assigns a value of `1` for billable items (either when there is no related data or when the related value is non-zero) and a value of `0` for non-billable items (when the related value is zero). This helps in identifying which items can be billed to clients based on their associated data.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `BillablePrj` in a data model, specifically for a table called `vw_Timesheet`. Here's a breakdown of what this expression does in simple business terms:

1. **Purpose**: The `BillablePrj` column is designed to determine whether a particular timesheet entry is considered "billable" or not. A billable entry is one that can be charged to a client or customer.

2. **Conditions Checked**:
   - **Sales Price**: The first condition checks if the `SalesPrice` for the entry is greater than 0. If there is a positive sales price, it indicates that the work done can be billed to a client.
   - **Project Profile Code**: The second condition checks if the `ProjectProfileCode` is equal to 81. This specific code likely represents a type of project that is always billable, regardless of other factors.
   - **Project Name**: The third condition uses the `SEARCH` function to look for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it suggests that the project is related to customer success services, which are typically billable.

3. **Output**: 
   - If any of the three conditions are met (i.e., if the sales price is positive, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of `1`, indicating that the entry is billable.
   - If none of these conditions are met, it returns `0`, indicating that the entry is not billable.

In summary, this DAX expression effectively categorizes timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project name. This helps businesses track which work can be charged to clients, aiding in revenue generation and project management.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column in the 'vw_Timesheet' table captures the name of the employer associated with each timesheet entry, facilitating accurate tracking and reporting of employee work hours by organization.

**`Employee_WeeklyHours`** (`string`)

- **Description:** The 'Employee_WeeklyHours' column captures the total number of hours worked by each employee during the week, represented as a string for flexible formatting and potential inclusion of additional annotations.
- **DAX Expression:**
```dax
CONCATENATE(
			    ('vw_Timesheet'[EmployeeName]),
			    " - " & ('vw_Timesheet'[HoursperWeek])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `Employee_WeeklyHours` in a data model, specifically from a table named `vw_Timesheet`. 

Here's a breakdown of what this code does in simple business terms:

1. **Combining Information**: The code takes two pieces of information from the `vw_Timesheet` table:
   - The name of the employee (`EmployeeName`).
   - The number of hours that employee works per week (`HoursperWeek`).

2. **Formatting the Output**: It combines these two pieces of information into a single string. The format of the output will be:
   - The employee's name followed by a hyphen and then the number of hours they work each week. For example, if the employee's name is "John Doe" and he works 40 hours a week, the result would be "John Doe - 40".

3. **Purpose**: This calculated column is useful for creating a clear and concise representation of each employee's weekly working hours alongside their name. It can help in reporting, analysis, or visualization by providing a quick reference to both the employee's identity and their work commitment in a single field.

In summary, this DAX expression effectively creates a new column that combines employee names with their weekly hours in a readable format, making it easier to understand and analyze employee work hours at a glance.

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

1. **Purpose**: The 'GroupCat' column categorizes projects based on their group affiliation and whether they are billable or not.

2. **Lookup for Group**: The first part of the code checks if there is a corresponding group for a project in the 'lkp_Project' table. It does this by looking up the project name from the 'vw_Timesheet' table in the 'lkp_Project' table. If it finds a match (i.e., the group is not blank), it retrieves the group name.

3. **Condition Handling**:
   - **If Group Exists**: If a group is found (the result is not blank), the 'GroupCat' column will take the value of that group.
   - **If Group Does Not Exist**: If no group is found (the result is blank), it checks another condition:
     - If the project is marked as billable (indicated by 'BillablePrj' being equal to 1), it assigns the category "Billable".
     - If the project is not billable, it assigns the category "Other Unbillable".

4. **Outcome**: The final result is that each project in the 'vw_Timesheet' table will be categorized as either:
   - The specific group it belongs to (if applicable),
   - "Billable" (if it is a billable project without a specific group),
   - "Other Unbillable" (if it is not a billable project and also does not belong to any group).

In summary, this DAX expression helps to classify projects into meaningful categories based on their group affiliation and billability status, which can be useful for reporting and analysis in a business context.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called **IsContractActive** in a data model, specifically for a table named **vw_Timesheet**. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The column determines whether a contract is currently active or not.

2. **Conditions Checked**:
   - **ISBLANK('vw_Timesheet'[ContractEndDate])**: This part checks if the **ContractEndDate** field is empty (i.e., there is no end date specified). If there is no end date, it implies that the contract is still ongoing.
   - **'vw_Timesheet'[ContractEndDate] > TODAY()**: This part checks if the **ContractEndDate** is later than today's date. If the end date is in the future, it means the contract is still active.

3. **Outcome**:
   - If either of the above conditions is true (meaning the contract has no end date or the end date is still in the future), the calculated column will show **"Active"**.
   - If neither condition is true (meaning there is an end date that has already passed), it will show **"Not Active"**.

In summary, this DAX expression effectively labels each contract in the **vw_Timesheet** table as either "Active" or "Not Active" based on whether the contract is still valid according to its end date. This helps users quickly identify which contracts are currently in effect.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the 'lkp_Unit' table for the current row. Specifically, it looks for the 'Unit' field in that related table.

2. **Handle Missing Data**: 
   - If there is no related value (meaning the 'Unit' field is blank), the code will return the text "Unknown". This is a way to handle situations where the expected data is missing, ensuring that the output is clear and informative.
   - If there is a related value (meaning the 'Unit' field has data), it will return that value from the 'lkp_Unit' table.

3. **Purpose**: The overall purpose of this calculated column is to provide a clear and user-friendly representation of the 'Unit' associated with each row. If the unit is not available, it explicitly states "Unknown" instead of leaving it blank, which can help users understand the data better.

In summary, this DAX expression ensures that every row in the 'MAIN_UNIT' column either shows the corresponding unit name or indicates that the unit is unknown, improving data clarity and usability.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) associated with each timesheet entry, facilitating time-based analysis and reporting.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to create a calculated column called 'MonthNumber' in a data model, typically within a tool like Power BI or Excel.

### What it does:

1. **Extracts the Month**: This expression takes a date value from the column `[TimesheetDate]` and extracts the month part of that date.

2. **Returns a Number**: The result is a number that represents the month. For example:
   - If `[TimesheetDate]` is January 15, 2023, the expression will return `1`.
   - If `[TimesheetDate]` is July 10, 2023, it will return `7`.
   - If `[TimesheetDate]` is December 25, 2023, it will return `12`.

### Purpose:

- **Organizing Data**: By creating a 'MonthNumber' column, you can easily categorize or filter your data based on the month. This is useful for reporting and analysis, allowing you to see trends or patterns over different months.

- **Facilitating Analysis**: It helps in performing time-based analysis, such as comparing performance across different months or aggregating data monthly.

In summary, this DAX expression simplifies the process of analyzing data by breaking down dates into their respective month numbers, making it easier to work with time-related insights.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the employee identification numbers as strings, facilitating the association of timesheet entries with specific employees in the vw_Timesheet table.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

### What It Achieves:
1. **Count of Unique Employees**: This expression counts how many different employees are present in the `vw_Timesheet` data. For example, if the `EmployeeName` column has entries like "John", "Jane", "John", and "Doe", the `DISTINCTCOUNT` function will return 3 because there are three unique names.

2. **Data Analysis**: This calculation is useful for understanding workforce metrics, such as how many distinct employees worked during a specific period or project. It helps in reporting and analyzing employee participation without counting duplicates.

In summary, this DAX expression provides a straightforward way to quantify the number of unique employees in a dataset, which can be valuable for various business insights and reporting purposes.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table categorizes the type of time entry as either owned, subcontracted, or external, facilitating accurate tracking and reporting of resource allocation.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used in a calculated column to retrieve a specific value from a related table. Here’s a breakdown of what it does in simple business terms:

1. **Context of Use**: This expression is typically used in a data model where there are two tables that have a relationship. For example, you might have a main table (let's call it "Sales") and a lookup table (let's call it "lkp_Unit").

2. **Purpose of the Expression**: The `RELATED` function is designed to pull in data from a related table based on the existing relationship between the two tables. In this case, it is fetching the value from the column `OWN-Sub-ExtT` in the `lkp_Unit` table.

3. **What It Achieves**: By using this expression in a calculated column, you are essentially enriching your main table (e.g., "Sales") with additional information from the `lkp_Unit` table. Specifically, for each row in the main table, it will look up the corresponding value of `OWN-Sub-ExtT` from the `lkp_Unit` table based on the relationship defined in the data model.

4. **Example Scenario**: Imagine you have a sales record that includes a unit identifier. The `lkp_Unit` table contains detailed information about each unit, including a column called `OWN-Sub-ExtT` that might represent a specific classification or category of the unit. By using this DAX expression, each sales record can automatically include this classification, making it easier to analyze sales data by unit category.

In summary, this DAX expression enhances your data by linking related information from another table, allowing for more comprehensive analysis and reporting.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column in the 'vw_Timesheet' table captures the project qualification status as a string, indicating whether the associated time entries meet specific criteria for project classification.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'QualifyPrj'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column 'QualifyPrj' is designed to determine a qualification status for projects based on related data from another table called 'lkp_Project'.

2. **Logic**:
   - The expression first checks if the 'Qualify' field from the related 'lkp_Project' table is blank (i.e., it has no value).
   - If the 'Qualify' field is blank, the expression returns a value of **1**. This could indicate a default or a specific status (like "not qualified" or "unknown").
   - If the 'Qualify' field is not blank (meaning it has a value), the expression returns the actual value from the 'Qualify' field in the 'lkp_Project' table.

3. **Outcome**: 
   - The result of this calculation will be either **1** (if there is no qualification information available) or the actual qualification value from the 'lkp_Project' table (if it exists). 
   - This allows users to easily see whether a project has a qualification status or if it is missing that information.

In summary, this DAX expression helps in assessing the qualification status of projects by providing a default value when no information is available, ensuring that every project has a clear status in the dataset.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor allocation.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of timesheet entries, specifically reflecting whether they have been processed with version Z enhancements.
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

1. **Purpose**: The calculated column is designed to determine whether a certain condition is met for each row in the data. Specifically, it checks if a report is ready or if a specific type of timesheet code is used.

2. **Conditions Checked**:
   - The first condition checks if the column `[ReportReady]` is marked as `TRUE()`. This means that the report is ready for that particular entry.
   - The second condition checks if the `TimesheetCode` in the `vw_Timesheet` table is either "V" or "Z". These are specific codes that might represent certain types of timesheets.

3. **Output**:
   - If either of the conditions is true (meaning the report is ready or the timesheet code is "V" or "Z"), the calculated column will return a value of `1`.
   - If neither condition is met, it will return a value of `0`.

4. **Business Implication**: This calculated column can be used to easily identify which entries are either ready for reporting or belong to specific categories (V or Z). This can help in filtering or analyzing data based on readiness or specific timesheet types, making it easier for decision-makers to focus on relevant entries.

In summary, the `RReady_With_V_Z` column helps flag entries that are either ready for reporting or belong to specific timesheet categories, providing a simple way to categorize and analyze data.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is a measure named `ContractStatus2`. Here's a breakdown of what it does in simple business terms:

1. **Condition Check**: The measure first checks the value of another measure called `[ContractStatusMeasure]`. It specifically looks to see if this measure equals "Active".

2. **Return Value Based on Condition**:
   - If the condition is true (meaning the contract status is "Active"), the measure returns the value of another measure called `[HoursDifference]`. This likely represents the difference in hours related to the contract, such as the number of hours worked or remaining hours on a contract.
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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a measure called `ContractStatusMeasure`, which determines the status of a contract based on its end date. Here’s a breakdown of what it does in simple business terms:

1. **Check for Contract End Date**: The measure first looks at the maximum value of the `ContractEndDate` from the `vw_Timesheet` table. This means it is checking the latest end date of contracts that are relevant to the current context (like a specific employee or project).

2. **Determine if the Date is Blank or in the Future**:
   - **ISBLANK**: It checks if this maximum end date is blank (meaning there is no end date specified for the contract).
   - **Comparison with TODAY()**: It also checks if this maximum end date is greater than today’s date. This means it is looking to see if the contract is still valid and has not yet expired.

3. **Return Contract Status**:
   - If either of the above conditions is true (the end date is blank or it is a future date), the measure returns "Active". This indicates that the contract is currently valid and ongoing.
   - If neither condition is true (meaning there is a specified end date that is in the past), it returns "Not Active". This indicates that the contract has expired.

In summary, this measure effectively categorizes contracts as either "Active" or "Not Active" based on whether they are still valid or have ended, helping businesses quickly assess the status of their contracts.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 10, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as an input and returns the week number of that date within the year. The week number is typically calculated starting from January 1st. 

So, when you combine these two functions, `WEEKNUM(TODAY())` effectively tells you which week of the year it is right now. 

### Business Explanation:
In practical terms, this measure can be used in reports or dashboards to identify the current week in the context of your business operations. For example, if you are tracking weekly sales, performance metrics, or any time-sensitive data, knowing the current week number helps you compare this week’s performance against previous weeks or set targets for the upcoming weeks.

**`Dax_EmpCount_Approved`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[Approved] = FALSE()
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `Dax_EmpCount_Approved`. Here’s a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the number of unique employees whose timesheets have not been approved.

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows us to apply filters to the data before performing calculations.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it tells us how many different employees are represented in the data.
   - **`vw_Timesheet[Approved] = FALSE()`**: This filter condition specifies that we only want to consider timesheets that have not been approved. In other words, it filters the data to include only those entries where the `Approved` status is marked as false.

3. **Outcome**: The overall result of this measure is the total count of distinct employees who have submitted timesheets that are still pending approval. This information can be useful for management to understand how many employees are waiting for their timesheet approvals, which can help in tracking workflow and ensuring timely processing of timesheets.

In summary, `Dax_EmpCount_Approved` provides insights into the number of employees with unapproved timesheets, helping organizations manage their approval processes more effectively.

**`Dax_EmpCount_RReady`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[ReportReady] = FALSE()
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `Dax_EmpCount_RReady`. Let's break it down in simple business terms:

1. **Purpose of the Measure**: This measure is designed to count the number of unique employees whose timesheets are not marked as "Report Ready."

2. **Key Components**:
   - **`CALCULATE` Function**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data being analyzed.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries in the timesheet.
   - **`vw_Timesheet[ReportReady] = FALSE()`**: This filter condition specifies that we only want to consider timesheet entries where the `ReportReady` status is set to FALSE. In other words, it filters out any employees whose timesheets are ready for reporting.

3. **What It Achieves**: The overall result of this measure is the total number of distinct employees who have timesheets that are not ready for reporting. This can be useful for management to identify how many employees still need to complete or finalize their timesheets before they can be reported.

In summary, `Dax_EmpCount_RReady` helps track the number of employees with incomplete timesheets, allowing for better management of reporting processes.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheet entries in a dataset.

### Breakdown of the Expression:

- **DISTINCTCOUNT**: This function counts the number of unique values in a specified column. In this case, it is counting unique entries.
  
- **vw_Timesheet[EmployeeName]**: This refers to the column named `EmployeeName` in the `vw_Timesheet` table. This column contains the names of employees who have submitted timesheets.

### What It Achieves:

When this measure is used in a report or dashboard, it provides the total count of different employees who have logged their hours. For example, if 10 different employees have submitted timesheets during a specific period, the result of this measure will be 10. 

This is useful for understanding workforce participation, tracking employee engagement, or analyzing resource utilization within a business context. It helps management see how many distinct employees are actively contributing to projects or tasks, which can inform staffing decisions and project planning.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the average hourly rate for sales based on data from a timesheet. Here's a breakdown of what it does in simple business terms:

1. **Data Source**: The calculation uses a table called `vw_Timesheet`, which likely contains records of sales and the hours worked by employees.

2. **Sales Amount**: The expression `SUM(vw_Timesheet[SalesAmount])` adds up all the sales amounts recorded in the `SalesAmount` column of the `vw_Timesheet` table. This gives the total sales generated over a specific period.

3. **Hours Worked**: The expression `SUM(vw_Timesheet[Hours])` adds up all the hours worked recorded in the `Hours` column of the same table. This provides the total number of hours worked over the same period.

4. **Average Hourly Rate Calculation**: By dividing the total sales amount by the total hours worked (`SUM(vw_Timesheet[SalesAmount]) / SUM(vw_Timesheet[Hours])`), the measure calculates the average hourly rate. This means it shows how much revenue is generated per hour of work.

### Summary:
In summary, this DAX measure calculates the average hourly rate of sales by taking the total sales revenue and dividing it by the total hours worked. This helps businesses understand how effectively their workforce is generating sales relative to the time they spend working.

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
   - The measure starts by determining the current week number of the year using `SELECTEDVALUE(DimDate[WeekNumberOfYear])`. This means it looks at the context of the report or dashboard to find out which week is currently being analyzed.

2. **Calculate Previous Budget**:
   - The measure then calculates a value called `PrevBudget`. This is done using the `CALCULATE` function, which allows for modifying the context in which a measure is evaluated. 
   - Inside `CALCULATE`, it uses the measure `[Msr_SalesPriceBaseCurrency]`, which represents the sales price in the base currency.
   - The `FILTER` function is applied to `ALLSELECTED(DimDate)`, which means it considers all selected dates but filters them to include only those weeks that are less than the current week. Essentially, it sums up the sales price for all previous weeks.

3. **Return the Appropriate Value**:
   - Finally, the measure uses an `IF` statement to decide what to return:
     - If the current sales price measure `[Msr_SalesPriceBaseCurrency]` is not blank (meaning there is actual sales data for the current week), it returns that current value.
     - If the current sales price is blank (indicating no sales data for the current week), it returns the previously calculated budget (`PrevBudget`).

### Summary:
In summary, this measure calculates the budget for the current week based on sales data. If there are sales figures available for the current week, it uses those figures. If not, it falls back to the total sales price from all previous weeks, effectively providing a way to estimate the budget based on past performance when current data is unavailable. This helps businesses understand their sales trends and make informed decisions based on both current and historical data.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a cumulative cost measure, which helps in understanding the total costs incurred up to a selected month in a given year. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: 
   - The code starts by determining which month is currently selected in the visual (like a report or dashboard). It does this by finding the maximum value of the `MonthNumberOfYear` from the `DimDate` table. This means if you are looking at data for March, the `SelectedMonth` will be 3.

2. **Calculate Cumulative Cost**:
   - The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to and including the selected month.
   - It uses the `CALCULATE` function to modify the context in which the data is evaluated.

3. **Sum the Costs**:
   - Inside the `CALCULATE` function, it sums up the `CostAmount` from the `vw_Timesheet` table. This is the actual cost data that we want to aggregate.

4. **Filter the Data**:
   - The `FILTER` function is used to create a condition that includes all months from the beginning of the year up to the selected month. 
   - The `ALLSELECTED('DimDate')` function ensures that any filters applied to the `DimDate` table (like year or other dimensions) are respected, but it allows the measure to consider all months up to the selected month.

5. **Final Output**:
   - The result of this measure is the total cost incurred from January up to the month that is currently selected in the visual. For example, if March is selected, it will sum the costs for January, February, and March.

In summary, this DAX measure helps businesses track how much they have spent cumulatively over time, allowing for better financial analysis and decision-making based on the costs incurred up to a specific point in the year.

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

1. **Identify the Selected Month**: The code starts by determining which month is currently selected in the visual (like a chart or table). It does this by finding the maximum value of the `MonthNumberOfYear` from the `DimDate` table. This means if you are looking at data for March, the `SelectedMonth` would be 3.

2. **Calculate Cumulative Sales**: The main goal of this measure is to calculate the total sales amount from the beginning of the year up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to change the context in which the data is evaluated. Here, it sums up the `SalesAmount` from the `vw_Timesheet` table.

4. **Filter for Relevant Months**: The `FILTER` function is applied to ensure that only the months that are less than or equal to the `SelectedMonth` are included in the calculation. The `ALLSELECTED` function allows the measure to respect any filters that might be applied to the date dimension, ensuring that it only considers the months that are relevant to the current selection.

5. **Final Result**: The final result of this measure is the total sales amount for all months from January up to the month selected in the visual. For example, if March is selected, it will sum the sales from January, February, and March.

In summary, this DAX measure helps users understand how sales have accumulated over time, providing insights into performance up to a specific point in the year.

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

Here's a breakdown of what it does in simple business terms:

1. **TotalValue Calculation**:
   - The measure first calculates the total hours worked (`SUM(vw_Timesheet[Hours])`) across all selected filters for the categories of Group, Project, and Employee. 
   - The `ALLSELECTED` function ensures that it considers only the filters that are currently applied in the report or dashboard, allowing for a dynamic calculation based on user selections.

2. **Percentage Calculation**:
   - After calculating the total hours (stored in the variable `TotalValue`), the measure then calculates the percentage of hours for the current context (the specific group, project, or employee being analyzed).
   - It does this by dividing the sum of hours for the current context (`SUM(vw_Timesheet[Hours])`) by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safer way to handle division in DAX. If the `TotalValue` is zero (meaning no hours were recorded), it will return 0 instead of causing an error.

### In Summary:
The `Msr_HoursPercentage` measure calculates how much of the total hours worked (based on current selections) is attributed to a specific group, project, or employee. This helps in understanding the contribution of different entities to the overall workload, allowing for better resource management and performance analysis.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the project margin for a specific project by subtracting total costs from total sales. Here’s a breakdown of what it does in simple business terms:

1. **Sales Amount**: The first part of the expression, `sum(vw_Timesheet[SalesAmount])`, adds up all the sales amounts recorded in the `vw_Timesheet` table. This represents the total revenue generated from the project.

2. **Cost Amount**: The second part, `sum(vw_Timesheet[CostAmount])`, sums up all the costs associated with the project, as recorded in the same table. This includes all expenses incurred to deliver the project.

3. **Calculating Project Margin**: The entire expression, `sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`, calculates the project margin by taking the total sales (revenue) and subtracting the total costs (expenses). 

In summary, this measure provides a clear view of the profitability of a project by showing how much money is left after covering the costs. A positive result indicates a profitable project, while a negative result suggests a loss.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called `Msr_SalesPriceBaseCurrency`, which essentially sums up the sales prices in a base currency for different projects and clients. Let’s break it down step by step in simple business terms:

1. **SUMMARIZE Function**: 
   - The `SUMMARIZE` function is used to create a summary table from the `vw_Timesheet` data. This summary table groups the data by two columns: `ProjectProfile` and `Client`. 
   - For each unique combination of `ProjectProfile` and `Client`, it creates a new column called `MeasureValue`.

2. **SELECTEDVALUE Function**:
   - The `SELECTEDVALUE` function retrieves the sales price in the base currency from the `tbl_Project` table. This means that for each project and client combination, it looks up the corresponding sales price in the base currency.

3. **SUMX Function**:
   - The `SUMX` function then takes the summary table created by `SUMMARIZE` and iterates over each row of this table.
   - For each row, it sums up the values in the `MeasureValue` column, which contains the sales prices in the base currency.

### What It Achieves:
- The overall goal of this measure is to calculate the total sales price in the base currency for all projects and clients listed in the `vw_Timesheet`. 
- This allows businesses to understand the total revenue or sales value in a consistent currency, which is crucial for financial reporting and analysis.

In summary, `Msr_SalesPriceBaseCurrency` provides a clear view of total sales prices across different projects and clients, ensuring that all values are expressed in the same currency for accurate financial insights.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called "Static Total Employees." Let's break it down into simpler terms to understand what it does:

1. **CALCULATE Function**: This is a powerful function in DAX that allows you to modify the context in which data is evaluated. In this case, it is used to compute a specific count of employees based on certain conditions.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part of the code counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it helps to ensure that each employee is only counted once, even if they appear multiple times in the data.

3. **ALL(vw_Timesheet)**: This function removes any filters that might be applied to the `vw_Timesheet` table. By using `ALL`, the measure looks at all the data in the table without any restrictions from other filters that might be in place in the report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This condition specifies that the measure should only count employees whose contract status is marked as "Valid." This means that only employees who are currently active or valid according to the contract status will be included in the count.

### Summary:
In summary, this DAX measure calculates the total number of unique employees who have a valid contract status, ignoring any other filters that might be applied to the data. It provides a clear view of how many employees are currently active and valid, which is useful for HR reporting and workforce analysis.

**`TotalActualHours`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression `SUM(vw_Timesheet[Hours])` is used to calculate the total number of hours recorded in a timesheet. 

Here's a breakdown of what it does:

- **SUM**: This function adds up all the values in a specified column.
- **vw_Timesheet**: This refers to a table (or view) that contains timesheet data. It likely includes various details about employee hours worked, projects, and dates.
- **[Hours]**: This specifies the column within the `vw_Timesheet` table that contains the actual hours worked by employees.

In simple terms, this measure calculates the total actual hours worked by all employees by summing up the values in the "Hours" column of the timesheet data. This total can be used for reporting, analysis, or decision-making regarding workforce management, project tracking, or payroll.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you've provided calculates the total contracted hours for employees based on their weekly hours. Here's a breakdown of what it does in simple business terms:

1. **SUMX Function**: This function is used to perform a calculation over a table and then sum the results. In this case, it will iterate over a list of employees.

2. **VALUES Function**: This function retrieves a unique list of employee names from the `vw_Timesheet` table. Essentially, it creates a distinct list of all employees who have timesheet entries.

3. **MAX Function**: For each employee in the list, the `MAX` function looks at the `HoursPerWeek` column in the `vw_Timesheet` table. It finds the maximum number of hours that each employee is contracted to work in a week.

4. **Calculation Process**: The `SUMX` function then takes the maximum hours per week for each employee and adds them together. 

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
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two measures: **TotalActualHours** and **TotalContractedHours**. 

Here's a breakdown of what this means in simple business terms:

1. **TotalActualHours**: This measure represents the total number of hours that have actually been worked or logged by employees or resources on a project or task.

2. **TotalContractedHours**: This measure indicates the total number of hours that were agreed upon in a contract for the project or task. This is the expected or planned amount of work.

3. **The Calculation**: The expression `[TotalActualHours] - [TotalContractedHours]` subtracts the total contracted hours from the total actual hours. 

4. **What It Achieves**: 
   - If the result is positive, it means that more hours were worked than what was contracted, indicating potential overwork or additional effort beyond what was planned.
   - If the result is negative, it means that fewer hours were worked than contracted, suggesting that the project may be under budget in terms of hours or that work is lagging behind the expected schedule.
   - If the result is zero, it indicates that the actual hours worked match the contracted hours, showing that the project is on track according to the initial agreement.

In summary, this measure helps assess whether a project is meeting its expected workload in terms of hours worked, which can be crucial for project management, budgeting, and resource allocation.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `TotalHoursTracked`. Here’s a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The measure first checks if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Zero if Blank**: If the sum of hours is blank (meaning no hours have been tracked), the measure will return a value of **0**. This is important because it ensures that when there are no recorded hours, the measure does not show a blank value, which could be misleading in reports.

3. **Return the Sum of Hours if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the measure will return the actual total sum of hours tracked from the `vw_Timesheet` table.

### Summary:
In essence, this measure calculates the total hours tracked by summing up the hours from the timesheet. If no hours are recorded, it safely returns 0 instead of leaving a blank, making it easier to interpret in reports and dashboards. This helps users quickly understand how many hours have been logged without confusion over missing data.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate a measure called `TotalHoursTrackedMissing`. Here's a breakdown of what it does in simple business terms:

1. **Check for Missing Data**: The expression first checks if the total hours tracked (from the `vw_Timesheet` table) is blank or empty. This is done using the `ISBLANK` function combined with `SUM(vw_Timesheet[Hours])`, which sums up the hours recorded in the `Hours` column of the `vw_Timesheet` table.

2. **Return Value Based on Check**:
   - If the total hours are blank (meaning no hours have been recorded), the measure will return the text "Empty". This indicates that there are no hours tracked for the selected context (like a specific employee, project, or time period).
   - If there are hours recorded (i.e., the total is not blank), it simply returns the sum of those hours.

### Summary:
In essence, this measure helps identify whether there are any hours tracked in the timesheet. If there are no hours recorded, it clearly indicates that with the word "Empty". If hours are present, it provides the total number of hours tracked. This can be useful for reporting and analysis, helping users quickly understand if there is missing data in their timesheet records.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you've provided calculates a measure called `trackedDiff`. Let's break it down into simpler terms to understand what it achieves:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been tracked for a specific employee or a group of employees. This is the baseline figure we are working with.

2. **Calculating Maximum Hours Per Week**:
   - The `CALCULATE` function is used to modify the context in which data is evaluated. Inside this function, we have `MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])`.
   - This part is finding the maximum number of hours tracked in a week for each employee. It looks at the `HoursPerWeek` column in the `vw_Timesheet` table, ensuring that it only considers unique values (using `DISTINCT`), and then it calculates the maximum of those values.

3. **Context Filtering**:
   - The `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` function is used to keep the context for each individual employee while ignoring other filters that might be applied to the `vw_Timesheet` table. This means that the calculation of maximum hours per week is done specifically for each employee, regardless of any other filters that might be in place.

4. **Final Calculation**:
   - The final result of the measure `trackedDiff` is the difference between the total hours tracked (`[TotalHoursTracked]`) and the maximum hours tracked in any single week for that employee. 

### In Summary:
The `trackedDiff` measure calculates how many hours an employee has tracked beyond their maximum tracked hours in any single week. If the result is positive, it indicates that the employee has tracked more hours overall than they did in their busiest week. If it's negative, it suggests that their total tracked hours are less than their maximum weekly hours. This measure can help in understanding employee workload and productivity over time.

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

1. **Total Hours Tracked**: The measure starts by referencing another measure called `[TotalHoursTracked]`. This measure likely calculates the total number of hours that have been logged or tracked by employees over a certain period.

2. **Calculating the Maximum Hours Per Week**: The next part of the code uses the `CALCULATE` function to determine the maximum number of hours worked per week from a table called `vw_Timesheet`. It does this by:
   - Using `MAXX` to find the highest value of `HoursPerWeek` from the distinct (unique) values in that column. This means it looks at all the different weekly hour totals and picks the largest one.
   - The `REMOVEFILTERS` function is used to ignore any filters that might be applied to the `HoursPerWeek` column, ensuring that the calculation considers all possible values, not just those filtered by the current context.
   - The `ALLEXCEPT` function keeps the context for `EmployeeID`, meaning it calculates the maximum hours per week specifically for the employee currently being analyzed, while ignoring other filters.

3. **Calculating the Difference**: Finally, the measure subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. 

### What It Achieves:
In summary, `trackedDiff2` calculates the difference between the total hours an employee has tracked and the maximum number of hours they have worked in any given week. This can help identify whether an employee is consistently working more or fewer hours than their maximum weekly average, which can be useful for performance analysis, workload management, or identifying trends in employee productivity.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to calculate a measure called `UTI_TotalBillableHours`. Let's break it down into simple business terms to understand what it achieves:

1. **Purpose**: This measure is designed to calculate the total number of billable hours worked on specific projects from a timesheet data source.

2. **Filtering Criteria**: 
   - The measure first creates a filtered list of timesheet entries (referred to as `FilteredHours`). 
   - It includes only those entries where:
     - The project qualifies as a billable project (`vw_Timesheet[QualifyPrj] = 1` and `vw_Timesheet[BillablePrj] = 1`), meaning these projects are recognized as eligible for billing.
     - Alternatively, it includes entries for any project that contains the phrase "Customer Success Services" in its name. This is done using the `SEARCH` function, which checks if that specific text appears in the project name.

3. **Calculation of Total Hours**: 
   - After filtering the timesheet data based on the criteria above, the measure then sums up the hours worked on these filtered entries using the `SUMX` function. This function iterates over each entry in the filtered list and adds up the `Hours` recorded.

4. **Outcome**: The final result of this measure is the total number of hours that can be billed to clients for the specified projects, which helps in understanding the workload and potential revenue from billable work.

In summary, `UTI_TotalBillableHours` calculates the total hours worked on projects that are either explicitly marked as billable or fall under a specific category (Customer Success Services), providing valuable insights into billable work for the business.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `UTI_TOTALHOURS`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the total number of hours worked on specific projects that meet a certain qualification criteria.

2. **Components**:
   - **SUM(vw_Timesheet[Hours])**: This part of the code adds up all the values in the `Hours` column from the `vw_Timesheet` table. Essentially, it totals the hours logged by employees.
   - **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to include only those entries where the `QualifyPrj` column equals 1. This means it focuses on projects that are marked as qualified or relevant for the calculation.

3. **CALCULATE Function**: The `CALCULATE` function modifies the context in which the data is evaluated. In this case, it ensures that the sum of hours is only calculated for the projects that meet the qualification criteria (i.e., where `QualifyPrj` is 1).

### Summary:
In summary, the `UTI_TOTALHOURS` measure calculates the total hours worked on projects that are considered qualified (where `QualifyPrj` equals 1). This helps businesses understand how much time is being spent on important or relevant projects, allowing for better resource allocation and project management.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is a measure called 'UTILIZATION_New'. Let's break it down step by step to understand what it calculates and achieves in simple business terms.

### Key Components of the DAX Code:

1. **Variables Defined**:
   - **_TotalBillableHours**: This variable captures the total number of hours that are billable to clients.
   - **_TotalHours**: This variable captures the total number of hours worked, which includes both billable and non-billable hours.

2. **Return Statement**:
   - The measure uses a series of conditional checks (IF statements) to determine what value to return based on certain conditions.

### What It Achieves:

- **Active Contacts Check**: 
  - The first condition checks if there are any active contacts (represented by `[Msr_ActiveContacts]`). If there are no active contacts, the measure does not proceed further and returns a blank value.

- **Total Hours Check**: 
  - If there are active contacts, the next condition checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the measure does not proceed further and returns a blank value.

- **Utilization Calculation**:
  - If both previous checks are satisfied (there are active contacts and total hours are available), the measure calculates the utilization rate by dividing the total billable hours (`_TotalBillableHours`) by the total hours worked (`_TotalHours`).
  - If the division results in a blank (which can happen if `_TotalHours` is zero), the measure returns 0 instead of a blank value.

### Summary:

In summary, the 'UTILIZATION_New' measure calculates the utilization rate of hours worked by comparing billable hours to total hours, but only if there are active contacts and total hours are available. If any of these conditions are not met, it returns a blank or zero. This measure helps businesses understand how effectively their resources are being utilized in terms of billable work.

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
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Definition: NOT (`EmployeeName` = `null`))

**Actual vs Budget**

- Type: `lineChart`
- Name: `76b4488a85c8253bac24`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Actual` (Query: `vw_Timesheet.MSR_Cumulative_Revenue`) (Role: Y)
  - `Budget` (Query: `vw_Timesheet.Msr_Budget`) (Role: Y)
- Visual Level Filters:
  - Filter on `DimDate`.`WeekNumberOfYear` (Type: Advanced, Definition: NOT (`WeekNumberOfYear` = `null`))
  - Filter on `vw_Timesheet`.`[MSR_Cumulative_Revenue]` (Type: Advanced, Definition: NOT (`[MSR_Cumulative_Revenue]` = `null`))

