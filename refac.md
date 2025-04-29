# Power BI Model & Report Documentation

*Generated on: 2025-04-29 12:53:58*

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

The Power BI data model appears to be designed for a project management and time tracking system, likely within a corporate or organizational context. It integrates various dimensions and fact tables to facilitate the analysis of employee hours, project performance, and timesheet compliance. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a focus on capturing and monitoring the time employees allocate to different projects, while the lookup tables ('lkp_Project', 'lkp_Unit', 'lkp_Code', and 'lkp_fltr_Employee') provide essential metadata that enhances the granularity and context of the data.

The structure of the model indicates a star schema, where 'vw_Timesheet' serves as the central fact table, linking to multiple dimension tables that categorize and filter the data. The 'DimDate' table allows for time-based analysis, enabling users to track hours worked over specific periods. Meanwhile, the 'tbl_Project' table likely contains detailed information about each project, further enriching the analysis of resource allocation and project status. Overall, this data model is geared towards improving operational efficiency, ensuring accountability in time reporting, and providing insights into project management effectiveness.

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

The 'DimDate' table serves as a comprehensive date dimension, providing essential temporal attributes that enable businesses to analyze trends and performance over time. By offering detailed breakdowns of dates into various formats, such as day, week, month, and quarter, this table supports effective time-based reporting and enhances the ability to conduct time-series analysis across different business metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting across various business metrics. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient querying and analysis of date-related data across various business dimensions. |
| `DayName` | `string` | The 'DayName' column in the 'DimDate' table stores the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | Column Description: The 'DayNumberOfMonth' column represents the numerical day of the month (ranging from 1 to 31) for each date entry in the 'DimDate' table, facilitating date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical value of the day of the week, where 1 corresponds to Sunday and 7 corresponds to Saturday, facilitating date-related analyses and reporting. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column (int64) in the 'DimDate' table represents the sequential day of the year, ranging from 1 to 365 (or 366 for leap years), facilitating date-related calculations and analyses. |
| `MonthName` | `string` | The 'MonthName' column in the 'DimDate' table provides the full name of each month, facilitating time-based analysis and reporting by enhancing date-related data with clear, human-readable month identifiers. |
| `MonthNumberOfYear` | `int64` | Column Description: Represents the numerical value of the month (1 for January through 12 for December) to facilitate time-based analysis and reporting in the DimDate table. |
| `ShortDate` | `dateTime` | Column Description: The 'ShortDate' column captures the date in a concise format, facilitating efficient date-based analysis and reporting within the DimDate table. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Indicates whether the date corresponds to the current calendar year, with values of 'Yes' or 'No' to facilitate time-based analysis and reporting.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `CurrentYearFlag` in a data model, typically in tools like Power BI or Excel. Here's a breakdown of what this expression does in simple business terms:

1. **Purpose**: The `CurrentYearFlag` column is designed to indicate whether a date in the `DimDate` table falls within the current year or not.

2. **Components**:
   - `DimDate[ShortDate]`: This refers to a date field in the `DimDate` table. It represents individual dates that you want to evaluate.
   - `YEAR(DimDate[ShortDate])`: This part extracts the year from each date in the `ShortDate` column.
   - `YEAR(TODAY())`: This retrieves the current year based on today's date.

3. **Logic**:
   - The `IF` function checks if the year extracted from the `ShortDate` is less than or equal to the current year.
   - If the condition is true (meaning the date is in the current year or a previous year), it returns `1`.
   - If the condition is false (meaning the date is in a future year), it returns `0`.

4. **Outcome**: 
   - The result is a column where each row will have a value of `1` if the date is in the current year or any previous year, and a value of `0` if the date is in a future year.

In summary, this calculated column helps to flag dates that are either in the current year or earlier, making it easier for users to filter or analyze data based on time periods relevant to the current year.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table captures and categorizes various metrics related to hours worked and their corresponding percentage contributions, enabling businesses to analyze workforce productivity and resource allocation effectively. This table serves as a critical tool for performance evaluation and strategic planning by providing insights into labor utilization across different projects or departments.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column stores string representations of hours worked alongside their corresponding percentage contributions to total hours, facilitating analysis of employee workload distribution. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string values that represent the specific hours and percentage metrics associated with data enrichment processes, facilitating detailed analysis and reporting. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data entries, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes and organizes various codes used across the business, facilitating data consistency and integrity. It includes attributes for code identification, qualification criteria, and sorting order, enabling efficient data retrieval and analysis in reporting and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column in the 'lkp_Code' table stores unique string identifiers that facilitate the enrichment of data by linking various datasets through standardized codes. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numerical flag to categorize data for enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table specifies the order in which records should be arranged, facilitating efficient data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference dataset for employee information, providing essential details such as EmployeeId and EmployeeName to facilitate filtering and reporting across various business analytics and performance metrics in Power BI. This table enhances data integrity and consistency by standardizing employee identifiers for use in related datasets.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column serves as a unique identifier for each employee in the 'lkp_fltr_Employee' table, facilitating efficient data enrichment and retrieval processes. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, facilitating the enrichment of data by providing identifiable references for each individual in the employee lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, linking project identifiers with their respective qualification and billing metrics, while categorizing them into groups for streamlined reporting and analysis. This table enables businesses to efficiently track project performance and financials, facilitating informed decision-making and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the total billing amount associated with each project, facilitating financial analysis and reporting. |
| `Group` | `string` | The 'Group' column in the 'lkp_Project' table categorizes projects into distinct classifications to facilitate data enrichment and analysis. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the organization. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, serving as a numerical identifier to categorize projects based on their eligibility or compliance criteria. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their characteristics such as billable status and external affiliations. This table is essential for analyzing resource allocation and financial performance across different units, enabling informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column (int64) in the 'lkp_Unit' table indicates the department responsible for billable activities, facilitating accurate tracking and reporting of revenue-generating units. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column identifies the specific organizational unit within the company, facilitating data enrichment and analysis related to departmental structures and functions. |
| `OWN-Sub-ExtT` | `string` | Column 'OWN-Sub-ExtT' in the 'lkp_Unit' table represents the external type designation for owned sub-units, facilitating data enrichment and classification within the dataset. |
| `Unit` | `string` | The 'Unit' column stores the names of measurement units used for data categorization and analysis within the lkp_Unit reference table. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, status, leadership, and financial aspects. This table enables effective project management and reporting by providing insights into project performance and resource allocation within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and performance analysis within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column in the 'tbl_Project' table captures the administrative details or oversight responsibilities associated with each project, facilitating effective management and governance. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates and changes in the project lifecycle. |
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each project, facilitating the identification and management of client relationships within the project database. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating efficient data retrieval and management within the tbl_Project table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated, providing a timestamp for tracking project creation within the tbl_Project table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project, facilitating accurate budget management and reporting. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the timestamp of when each project entry was created, providing a chronological reference for project management and tracking. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or category of the project within the Devoteam framework, facilitating targeted analysis and reporting. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column in the 'tbl_Project' table indicates the scheduled date and time for the completion of each project, facilitating effective project timeline management and tracking. |
| `Project` | `string` | The 'Project' column contains the name or identifier of each project, serving as a key reference for associating related data and activities within the tbl_Project table. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed narrative of the project's objectives and scope, providing essential context for stakeholders and team members involved in 'tbl_Project'. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column categorizes projects into specific groups for better organization and management within the project database. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a detailed textual overview of the project group, facilitating better understanding and categorization of related projects within the 'tbl_Project' table. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for data enrichment and management processes. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the associated project in the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency amount of the sales price for each project, stored as a decimal to ensure precision in financial calculations. |
| `Startdate` | `dateTime` | The 'Startdate' column in the 'tbl_Project' table captures the date and time when a project is initiated, serving as a critical reference point for project timelines and scheduling. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, helping stakeholders assess project health and prioritize resources effectively. |
| `Statuscode` | `string` | The 'Statuscode' column in the 'tbl_Project' table indicates the current operational status of each project, facilitating effective tracking and management of project progress. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with each project, stored as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table serves as a critical resource for identifying employees who have not submitted their timesheets within a specified timeframe, enabling managers to monitor compliance and ensure accurate payroll processing. By providing insights into missing submissions across various dimensions such as employee details and contract information, this table aids in enhancing operational efficiency and accountability within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by employees that have not been recorded in their timesheets, facilitating accurate tracking and reporting of labor hours. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to terminate, providing essential information for tracking contract durations within the context of missing timesheet records. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to facilitate the identification of discrepancies in timesheet submissions. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | Column Description: Indicates the current status of the contract associated with the timesheet, providing insights into whether it is active, expired, or under review. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up of individuals for timely data completion. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours recorded for each employee's timesheet entry, facilitating the identification of discrepancies in time reporting. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_missing_timesheet' table identifies the unique identifier of the manager responsible for overseeing the employee associated with the missing timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees' timesheet submissions, facilitating accountability and tracking within the 'vw_missing_timesheet' view. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee work hour reporting. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the missing timesheet records, facilitating the tracking and management of time allocation across various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project, facilitating the tracking and management of timesheet entries associated with specific projects in the 'vw_missing_timesheet' table. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true signifying rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet data is complete and ready for reporting purposes. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of tracking and analyzing missing timesheet entries for accurate financial reporting. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double to accommodate precise pricing information within the context of timesheet data analysis. |
| `ShortDate` | `dateTime` | Column Description: The 'ShortDate' column captures the specific date and time when a timesheet entry is missing, facilitating timely follow-up and resolution of discrepancies. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific organizational subunit associated with each entry in the 'vw_missing_timesheet' table, facilitating targeted analysis of timesheet discrepancies by department. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' table. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the work performed during the timesheet period, enhancing the understanding of employee activities for reporting and analysis. |
| `Week_` | `int64` | The 'Week_' column (int64) in the 'vw_missing_timesheet' table represents the week number of the year associated with each timesheet entry, facilitating the identification and analysis of missing timesheet submissions on a weekly basis. |
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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `BillableDep`. Let's break down what it does in simple business terms:

1. **Purpose**: The calculated column is designed to determine whether a certain unit (likely a project, task, or resource) is considered "billable" based on a related table called `lkp_Unit`.

2. **Logic**:
   - The expression first checks if the `BillableDep` value from the `lkp_Unit` table is blank (i.e., there is no value associated with that unit).
   - If it is blank, the expression returns a value of **1**. This indicates that if there is no specific billable dependency defined, the unit is treated as billable by default.
   - If the `BillableDep` value is not blank, the expression then checks if this value is not equal to **0**.
     - If `BillableDep` is not zero, it returns **1**, meaning the unit is billable.
     - If `BillableDep` is zero, it returns **0**, indicating that the unit is not billable.

3. **Outcome**: The final result of this calculation is a column that assigns a value of **1** (billable) or **0** (not billable) to each unit based on the conditions outlined. This helps in identifying which units can be billed for services or resources, facilitating better financial tracking and reporting.

In summary, this DAX expression helps determine the billable status of units by checking their related `BillableDep` values, ensuring that units are correctly classified for billing purposes.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `CC_ActiveEmployees` in a data table called `vw_missing_timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column is designed to determine whether an employee is considered "active" based on their contract dates.

2. **Conditions Checked**:
   - **Contract Start Date**: The code checks if the date in the `ShortDate` column (which likely represents a specific date, such as when a timesheet was submitted) is on or after the employee's `ContractStartDate`. This means the employee's contract must have started before or on this date for them to be considered active.
   - **Contract End Date**: It also checks if the `ShortDate` is on or before the `ContractEndDate`. If the `ContractEndDate` is blank (meaning the employee's contract does not have a specified end date), the employee is still considered active regardless of the `ShortDate`.

3. **Output**:
   - If both conditions are met (the `ShortDate` is within the range of the contract dates or the contract has no end date), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns a value of `0`, indicating that the employee is not active.

### Summary:
In summary, this DAX expression effectively flags employees as "active" (1) or "not active" (0) based on whether the date of the timesheet falls within their contract period. This helps in identifying which employees are currently under contract and should be considered for payroll or other HR-related processes.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to facilitate the identification of records requiring further attention in the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `IncompleteFlag`. Here's a simple explanation of what it does:

1. **Context**: The code is looking at a column named `MissingHours` from a table called `vw_missing_timesheet`.

2. **Condition Check**: It checks whether the value in the `MissingHours` column is less than 0. 

3. **Output**: 
   - If the value is less than 0, it returns `1`. This indicates that there is an issue, such as missing or incomplete data related to hours.
   - If the value is 0 or greater, it returns `0`. This indicates that there are no issues with the hours reported.

### Summary:
The `IncompleteFlag` column essentially flags records where the `MissingHours` is negative, marking them as problematic (with a `1`), while marking all other records as fine (with a `0`). This can help in identifying and addressing incomplete timesheet entries in a dataset.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named **MAIN_UNIT** in a data model, and it serves a specific purpose in handling unit information.

Here's a breakdown of what this code does in simple business terms:

1. **Check for Related Data**: The expression uses the `RELATED` function to look up a value from a related table called `lkp_Unit`. Specifically, it tries to find the value in the `Unit` column of that table.

2. **Handle Missing Values**: The `ISBLANK` function checks if the value retrieved from the `lkp_Unit[Unit]` column is blank (i.e., there is no corresponding unit found). 

3. **Return Values Based on the Check**:
   - If the value is blank (meaning there is no related unit), the expression returns the string **"Unknown"**. This indicates that the system could not find a unit for that particular record.
   - If the value is not blank (meaning a unit was found), it returns the actual unit value from the `lkp_Unit[Unit]` column.

### Summary:
In summary, this DAX expression is designed to ensure that every record in the calculated column **MAIN_UNIT** has a meaningful value. If a unit is found, it displays that unit; if not, it clearly indicates that the unit is "Unknown." This helps maintain data integrity and clarity in reporting by avoiding blank values.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column (string) in the 'vw_missing_timesheet' table represents the specific year and week number for each entry, facilitating the tracking and analysis of timesheet submissions over time.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX code snippet creates a calculated column called 'YearWeek' that combines the year and the week number of a date, specifically the date from the column `[ContractEndDate]`. Here’s a breakdown of what it does in simple business terms:

1. **Extracting the Year**: The function `YEAR([ContractEndDate])` takes the date from the `[ContractEndDate]` column and extracts the year from it. For example, if the date is December 15, 2023, this part will return `2023`.

2. **Calculating the Week Number**: The function `WEEKNUM([ContractEndDate])` calculates which week of the year the `[ContractEndDate]` falls into. For instance, if the date is in the second week of December 2023, this function will return `50` (assuming the week starts on Sunday).

3. **Formatting the Week Number**: The `FORMAT(..., "00")` part ensures that the week number is always displayed as two digits. So, if the week number is `5`, it will be formatted as `05`. This is important for consistency in how the week numbers are presented.

4. **Combining Year and Week Number**: The `& "-" &` part combines the year and the formatted week number into a single string, separated by a hyphen. So, if the year is `2023` and the week number is `50`, the final result will be `2023-50`.

### Summary:
In summary, this DAX expression creates a new column that shows the year and the week number of the contract's end date in the format "YYYY-WW". This can be useful for reporting and analysis, allowing businesses to easily group or filter data by year and week. For example, it helps in tracking contracts that end in the same week across different years.

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

1. **Purpose**: This measure counts the number of unique employees (consultants) who have recorded negative hours in a timesheet. Negative hours might indicate an error, a correction, or a specific situation where hours worked are being adjusted downwards.

2. **Components**:
   - **`CALCULATE`**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data being counted.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the unique names of employees from the `vw_missing_timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries with negative hours.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This condition filters the data to only include records where the `MissingHours` field has a negative value. This means that only those employees who have negative hours will be considered in the count.

3. **Outcome**: The measure ultimately provides a count of how many different employees have negative hours recorded in the timesheet. This information can be useful for identifying potential issues with time reporting, understanding workload adjustments, or managing consultant performance.

In summary, `Count_Negative_Consultant` helps businesses track and analyze the number of consultants with negative hours, which can be critical for operational insights and decision-making.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called **CountNegativeMissingHours**. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure counts the number of unique employees who have recorded negative missing hours in the 'vw_missing_timesheet' table.

2. **Components**:
   - **CALCULATE**: This function changes the context in which the data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID])**: This part counts the number of unique Employee IDs in the 'vw_missing_timesheet' table. Essentially, it tells us how many different employees are involved.
   - **'vw_missing_timesheet'[MissingHours] < 0**: This is a filter condition that specifies we are only interested in records where the 'MissingHours' value is less than zero. This means we are looking for instances where employees have negative missing hours, which could indicate an error or a specific situation that needs attention.

3. **Outcome**: The measure ultimately provides a count of distinct employees who have negative values for missing hours. This can help a business identify potential issues with timesheet entries or highlight employees who may need further review regarding their reported hours.

In summary, **CountNegativeMissingHours** helps the organization track and manage discrepancies in employee timesheets by identifying how many employees have reported negative missing hours.

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

1. **Data Source**: The measure uses a data source called `vw_missing_timesheet`, which likely contains information about employees, their hours worked, and their contractual hours.

2. **Summarization**: The code first summarizes the data by grouping it based on several key attributes:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a more specific division within the unit)

   For each group, it calculates two important values:
   - **MissingHours**: This is calculated by taking the total hours worked by the employee in that week and subtracting their contracted hours. If this value is negative, it indicates that the employee has not logged enough hours compared to what they are contracted to work.
   - **MinActiveEmp**: This value checks if the employee is active during that period. It takes the minimum value of a field called `CC_ActiveEmployees`, which likely indicates whether the employee is active (1) or inactive (0).

3. **Filtering**: After summarizing the data, the code filters the results to only include:
   - Employees who are active (`MinActiveEmp = 1`)
   - Employees who have missing hours (`MissingHours < 0`)

4. **Counting**: Finally, the measure counts the number of rows that meet these criteria, which effectively gives the total number of active employees who have not logged enough hours in their timesheets for the specified week and year.

### Summary:
In summary, this DAX measure calculates how many active employees have not recorded enough hours in their timesheets for a given week and year, helping the business identify potential issues with timesheet compliance.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is designed to calculate a measure called `Dax_MissingHours`, which focuses on identifying and quantifying the hours that certain employees are missing based on their contracted hours versus the hours they have actually logged.

Here's a breakdown of what this code does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. This variable filters a summarized table (`vw_missing_timesheet`) to focus on specific employees who are currently active. An employee is considered active if their minimum count of active employees (`MinActiveEmp`) is equal to 1.

2. **Summarize Data**: The `SUMMARIZE` function is used to group the data by several key attributes:
   - Employee Name
   - Year
   - Week
   - Unit (from a lookup table)
   - SubUnit
   This grouping allows the measure to analyze the data at a detailed level.

3. **Calculate Missing Hours**: For each group of employees, the measure calculates:
   - **MissingHours**: This is determined by taking the total hours logged by the employee (`SUM(vw_missing_timesheet[Hours_])`) and subtracting their maximum contracted hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has logged fewer hours than they were contracted for.

4. **Filter for Specific Conditions**: The `FILTER` function then narrows down the summarized data to only include those employees who meet two conditions:
   - They are active (`MinActiveEmp = 1`).
   - They have missing hours (i.e., `MissingHours < 0`).

5. **Calculate Total Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered list of active employees and sums up their `MissingHours`. This gives the total number of hours that active employees are missing compared to their contracted hours.

In summary, this DAX measure calculates the total number of hours that active employees have not worked compared to what they are contracted to work, focusing only on those who have logged fewer hours than expected. This can help management identify potential issues with employee attendance or time reporting.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided calculates the total expected contract hours for employees on a weekly basis. Let's break it down step by step in simple business terms:

1. **Data Source**: The calculation is based on a data table called `vw_missing_timesheet`. This table likely contains records of timesheets, including information about employees, the weeks they worked, and their contract hours.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key pieces of information:
   - `Week_`: This represents the week of the year.
   - `EmployeeID`: This identifies each employee.

   For each unique combination of week and employee, the code calculates the maximum contract hours that the employee is expected to work during that week. This is done using the `MAX` function, which ensures that if there are multiple entries for an employee in a week, only the highest contract hours are considered.

3. **Creating a New Column**: Within the `SUMMARIZE` function, a new column called `"ContractHours"` is created. This column holds the maximum contract hours for each employee for each week.

4. **Calculating Total Contract Hours**: After summarizing the data, the outer `SUMX` function takes the summarized table and sums up all the values in the `"ContractHours"` column. This gives the total expected contract hours for all employees across all weeks.

### Summary
In summary, this DAX measure calculates the total expected contract hours for all employees, ensuring that for each employee in each week, only the highest contract hours are counted. This helps in understanding the overall expected workload for the organization on a weekly basis, which can be useful for planning and resource allocation.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the difference between two values related to hours worked and contracted hours for a specific context, likely in a reporting or analytical scenario.

Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum value of "ContractHours" from the `vw_missing_timesheet` table. "ContractHours" represents the total number of hours that an employee is contracted to work.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum value of "Hours_" from the same table. "Hours_" represents the actual hours that the employee has worked or reported.

3. **Calculation**: The expression then subtracts the maximum actual hours worked (Hours_) from the maximum contracted hours (ContractHours). 

### What It Achieves:
- The result of this calculation gives you the number of hours that are "missing" or unaccounted for. In other words, it tells you how many hours an employee was supposed to work (according to their contract) but did not report or log as worked.

### Summary:
In summary, this DAX measure helps identify discrepancies between contracted hours and reported hours, allowing businesses to see if employees are underreporting their hours or if there are potential issues with timesheet submissions. This can be crucial for payroll accuracy, resource planning, and ensuring compliance with labor agreements.

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

1. **SUM(vw_missing_timesheet[MissingHours])**: This part of the code adds up all the missing hours from the timesheet data. Essentially, it tells us how many hours employees did not report or submit.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that employees are expected to work in a week according to their contracts. 

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the code calculates the difference between the total missing hours and the expected hours. If the result is positive, it indicates that the missing hours exceed what was expected, suggesting a shortfall in reported hours.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The DIVIDE function takes the result from the previous step (the difference between missing hours and expected hours) and divides it by the expected contract hours. This gives us a ratio that represents how much of the expected hours are missing. If there are no expected hours (to avoid division by zero), it returns 0.

### What It Achieves:
- The final result of this measure is a percentage that indicates how much of the expected working hours are unaccounted for due to missing timesheets. 
- A positive percentage means that there are more missing hours than expected, highlighting a potential issue with timesheet submissions.
- A negative percentage would indicate that the reported hours exceed the expected hours, which could suggest over-reporting or extra hours worked.
- This measure helps management understand compliance with timesheet reporting and can be used to identify areas where employees may need support or reminders to submit their hours accurately.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate a measure called `UniqueEmployeesPerUnit`. Let's break it down into simpler terms to understand what it achieves:

1. **Data Source**: The measure uses a table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including information about whether their submissions are complete or incomplete.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by three key columns:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This is the name of the employee.
   - `MAIN_UNIT`: This likely refers to the department or unit the employee belongs to.

   For each unique combination of these three columns, the code also calculates a new column called `IncompleteFlag`, which takes the maximum value of the `IncompleteFlag` column for that group. This flag indicates whether there are any incomplete timesheet submissions for that employee in that week and unit.

3. **Calculating the Sum**: After summarizing the data, the `SUMX` function iterates over each of these unique combinations (each row in the summarized table) and sums up the values of the `IncompleteFlag`. 

   - If `IncompleteFlag` is 1 (indicating incomplete submissions), it contributes to the sum. If it’s 0 (indicating complete submissions), it does not.

4. **Final Result**: The final result of this measure is the total count of unique employee-week-unit combinations where at least one timesheet submission is incomplete. Essentially, it tells you how many employees, across different units, have missing timesheet submissions for the weeks considered.

In summary, `UniqueEmployeesPerUnit` calculates the number of unique employees who have not fully submitted their timesheets, grouped by week and unit. This measure helps in understanding compliance with timesheet submissions within different units of the organization.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This data enables organizations to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column in the 'vw_Timesheet' table identifies the specific client associated with each timesheet entry, facilitating accurate billing and project tracking. |
| `Code2` | `string` | The 'Code2' column in the 'vw_Timesheet' table stores a secondary identifier code used to categorize or classify timesheet entries for enhanced data analysis and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract is set to expire, providing essential information for resource planning and contract management within the timesheet data. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and payroll calculations within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract associated with each timesheet entry, providing essential context for evaluating project progress and resource allocation. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, providing real-time insights for performance analysis and decision-making within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential context for timesheet entries related to contract changes. |
| `CostAmount` | `double` | The 'CostAmount' column represents the total monetary cost associated with the recorded time entries in the 'vw_Timesheet' table, expressed as a double-precision floating-point number. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, expressed as a double to accommodate precise financial calculations. |
| `Currency` | `string` | The 'Currency' column specifies the type of currency used for financial entries in the timesheet, facilitating accurate monetary reporting and analysis. |
| `DebtorName` | `string` | The 'DebtorName' column contains the name of the individual or entity responsible for payment, providing essential context for financial transactions recorded in the 'vw_Timesheet' table. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_Timesheet' table uniquely identifies each employee, facilitating the tracking and management of their respective timesheet entries. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column stores the unique identifier and name of each employee, facilitating the association of timesheet entries with the respective personnel in the vw_Timesheet table. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with specific employees for accurate tracking and reporting. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating the identification and tracking of individual work hours and contributions. |
| `Employer` | `string` | The 'Employer' column identifies the organization or company that employs the individual associated with the timesheet entry, facilitating the tracking of labor costs and resource allocation. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating accurate payroll processing and reporting. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to each timesheet entry, providing insights that enhance the understanding of the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with each timesheet entry, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified period, facilitating accurate tracking of labor input for payroll and project management. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating analysis of labor distribution and productivity. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee time allocation. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee time allocation. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing project-related timesheet entries, facilitating accountability and resource management. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the names of individuals associated with specific IPM IDs, facilitating the identification and tracking of personnel in timesheet records. |
| `JobTitle` | `string` | The 'JobTitle' column in the 'vw_Timesheet' table captures the specific role or position of an employee, providing essential context for analyzing timesheet data and resource allocation. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking timesheet records to related datasets, facilitating seamless data integration and analysis. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and reporting within the organization. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column in the 'vw_Timesheet' table stores the names of managers associated with each timesheet entry, facilitating easy identification of managerial oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key associated with the manager responsible for overseeing the timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct supervisor, facilitating the identification of managerial oversight for timesheet entries. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries, detailing the hours allocated to various tasks within the timesheet records. |
| `Project` | `string` | The 'Project' column in the 'vw_Timesheet' table identifies the specific project associated with each recorded time entry, facilitating accurate tracking and reporting of resource allocation and project costs. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with the timesheet entries, facilitating accurate tracking and reporting of time spent on various projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that identifies the specific project associated with each timesheet entry, facilitating detailed tracking and analysis of time spent on various projects. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the associated project profile for each timesheet entry, facilitating accurate project tracking and reporting. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, enabling better analysis and reporting of resource allocation across different project types. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing a rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet is ready for reporting, with a value of true signifying readiness and false indicating further action is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double to accommodate precise financial calculations within the 'vw_Timesheet' table. |
| `SalesPrice` | `double` | The 'SalesPrice' column (double) in the 'vw_Timesheet' table represents the monetary value assigned to each service or product sold, facilitating financial analysis and reporting. |
| `Status` | `string` | The 'Status' column indicates the current approval state of each timesheet entry, reflecting whether it is pending, approved, or rejected. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, enabling better resource allocation and management. |
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
   - The first condition checks if the column `[Approved]` is marked as `TRUE`. This means that if the timesheet has been officially approved, it meets the criteria.
   - The second condition checks if the `TimesheetCode` from the `vw_Timesheet` table is either "V" or "Z". This means that if the timesheet has a code of "V" or "Z", it also meets the criteria for approval.

3. **Output**:
   - If either of the conditions is met (the timesheet is approved or has a code of "V" or "Z"), the calculated column will return a value of `1`. This indicates that the timesheet is considered approved.
   - If neither condition is met, it returns a value of `0`, indicating that the timesheet is not approved.

In summary, this DAX expression effectively flags timesheets as approved (1) if they are either explicitly approved or have specific codes ("V" or "Z"). If they do not meet these criteria, they are flagged as not approved (0). This helps in easily identifying which timesheets can be considered approved based on the defined rules.

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column indicates the department associated with billable hours recorded in the timesheet, facilitating accurate tracking of labor costs by department.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Let's break down what it does in simple business terms:

1. **Purpose**: The goal of this calculation is to determine whether a certain unit (likely a project, task, or resource) is considered "billable" based on a related table called `lkp_Unit`.

2. **Logic**:
   - The expression first checks if the `BillableDep` value from the `lkp_Unit` table is blank (i.e., there is no value associated with that unit).
   - If it is blank, the calculated column will return a value of **1**. This indicates that the unit is considered billable when there is no specific billable dependency defined.
   - If the `BillableDep` value is not blank, the expression then checks if this value is not equal to **0**.
     - If `BillableDep` is not zero, it again returns **1**, indicating that the unit is billable.
     - If `BillableDep` is zero, it returns **0**, indicating that the unit is not billable.

3. **Outcome**: 
   - The final result of this calculation is either **1** (billable) or **0** (not billable). 
   - This helps in identifying which units can be billed for services or work done, based on the criteria defined in the `lkp_Unit` table.

In summary, this DAX expression effectively categorizes units as billable or not based on their associated `BillableDep` values, helping businesses track and manage billable work efficiently.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `BillablePrj` in a data model, specifically in a table called `vw_Timesheet`. Here's a breakdown of what this expression does in simple business terms:

1. **Purpose**: The `BillablePrj` column is designed to identify whether a particular timesheet entry is billable or not. A billable entry is one that can be charged to a client or customer.

2. **Conditions**: The expression checks three specific conditions to determine if an entry is billable:
   - **Sales Price Check**: It first checks if the `SalesPrice` for the entry is greater than 0. If there is a positive sales price, it indicates that the work done can be billed.
   - **Project Profile Code Check**: Next, it checks if the `ProjectProfileCode` is equal to 81. This specific code likely represents a type of project that is always considered billable.
   - **Project Name Check**: Finally, it searches for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it suggests that the project is related to customer success services, which are typically billable.

3. **Output**: 
   - If any of these conditions are met (i.e., if the sales price is greater than 0, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of `1`, indicating that the entry is billable.
   - If none of these conditions are met, it returns a value of `0`, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project name, helping the business track which work can be charged to clients.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column contains the name of the employer associated with each timesheet entry, facilitating the identification of the organization responsible for employee hours worked.

**`Employee_WeeklyHours`** (`string`)

- **Description:** The 'Employee_WeeklyHours' column captures the total number of hours worked by each employee during the week, represented as a string for flexible formatting and potential inclusion of additional context.
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
   - The employee's name followed by a hyphen and a space, and then the number of hours they work per week.

3. **Example Output**: If an employee's name is "John Doe" and he works 40 hours per week, the resulting value in the `Employee_WeeklyHours` column would be:
   - "John Doe - 40"

4. **Purpose**: This calculated column is useful for quickly viewing and understanding how many hours each employee works in a clear and concise format. It can help in reporting, analysis, or simply providing a summary of employee work hours in a more readable way.

In summary, this DAX expression creates a new column that combines employee names with their weekly working hours, making it easier to read and analyze employee work data.

**`GroupCat`** (`string`)

- **Description:** The 'GroupCat' column categorizes timesheet entries into distinct groups for streamlined reporting and analysis of labor distribution across various projects or departments.
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

1. **Check for Project Group**: The code first checks if there is a corresponding project group for the project listed in the 'vw_Timesheet' table. It does this by looking up the project in the 'lkp_Project' table using the `LOOKUPVALUE` function. If it finds a group associated with that project, it retrieves that group.

2. **Return Project Group or Classify**: 
   - If a project group is found (meaning the result of the lookup is not blank), it returns that project group as the value for 'GroupCat'.
   - If no project group is found (the lookup result is blank), it then checks if the project is billable by looking at the 'BillablePrj' column in the 'vw_Timesheet' table. 
     - If 'BillablePrj' equals 1 (indicating that the project is billable), it assigns the value "Billable" to 'GroupCat'.
     - If 'BillablePrj' does not equal 1 (indicating that the project is not billable), it assigns the value "Other Unbillable" to 'GroupCat'.

### Summary:
In summary, this DAX expression categorizes each project in the 'vw_Timesheet' table into one of three categories:
- The specific project group from the 'lkp_Project' table (if it exists),
- "Billable" (if the project is billable but has no specific group),
- "Other Unbillable" (if the project is not billable and has no specific group).

This helps in organizing and analyzing projects based on their billing status and group classification, which can be useful for reporting and decision-making.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called **IsContractActive** in a data model, specifically for a table named **vw_Timesheet**. Here's a simple breakdown of what it does:

1. **Purpose**: The calculated column determines whether a contract is currently active or not.

2. **Logic**:
   - The expression checks two conditions regarding the **ContractEndDate**:
     - **ISBLANK('vw_Timesheet'[ContractEndDate])**: This checks if the **ContractEndDate** is blank (i.e., there is no end date specified). If there is no end date, it implies that the contract is still ongoing.
     - **'vw_Timesheet'[ContractEndDate] > TODAY()**: This checks if the **ContractEndDate** is greater than today's date. If the end date is in the future, it means the contract is still active.

3. **Outcome**:
   - If either of the above conditions is true (meaning the contract has no end date or the end date is still in the future), the calculated column will return **"Active"**.
   - If both conditions are false (meaning there is an end date and it is in the past), it will return **"Not Active"**.

In summary, this DAX expression effectively labels each contract in the **vw_Timesheet** table as either "Active" or "Not Active" based on whether the contract has an end date that is still valid or is missing altogether. This helps users quickly identify which contracts are currently in effect.

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

1. **Check for Related Data**: The code first checks if there is a related value in another table called `lkp_Unit`. Specifically, it looks for the `Unit` field in that table.

2. **Handle Missing Data**: 
   - If there is no related value (meaning the `Unit` field is blank or missing), the code will return the text "Unknown". This is a way to handle situations where the expected data is not available, ensuring that the result is clear and informative.
   - If there is a related value (meaning the `Unit` field has data), it will return that value from the `lkp_Unit` table.

3. **Purpose**: The overall purpose of this calculated column is to provide a clear and user-friendly representation of the unit associated with a record. If the unit is known, it shows that unit; if not, it indicates that the unit is unknown.

In summary, this DAX expression ensures that every record has a meaningful value for 'MAIN_UNIT', either displaying the actual unit or indicating that it is unknown when no data is available.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) to facilitate time-based reporting and analysis of timesheet data.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to create a calculated column called 'MonthNumber' in a data model, typically within a tool like Power BI or Excel.

### What It Does:
- **Extracts the Month**: This expression takes a date value from the column `[TimesheetDate]` and extracts the month part of that date.
- **Returns a Number**: The result is a number between 1 and 12, where:
  - 1 represents January,
  - 2 represents February,
  - 3 represents March,
  - and so on, up to
  - 12, which represents December.

### Business Context:
- **Simplifies Analysis**: By creating a 'MonthNumber' column, it allows users to easily analyze data based on the month. For example, you can quickly group or filter timesheet data by month to see trends in hours worked or project activity.
- **Facilitates Reporting**: This numeric representation of months can be used in reports and dashboards to visualize data over time, making it easier to compare performance across different months.

In summary, this DAX expression helps in breaking down dates into a more manageable format (the month number), which is useful for various analytical and reporting purposes.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the unique employee identification numbers as strings, facilitating the association of timesheet entries with specific employees in the vw_Timesheet view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

### In Simple Business Terms:

- **What it does**: This expression counts how many different employees have entries in the timesheet. 
- **Why it's useful**: By knowing the number of unique employees, a business can understand workforce participation, track employee engagement, or analyze resource allocation over a specific period.
- **Outcome**: If you apply this calculation in a report or dashboard, it will show you a total count of distinct employees who have submitted timesheets, helping management make informed decisions about staffing and project assignments. 

In summary, this DAX code helps businesses keep track of how many individual employees are actively logging their work hours, which is crucial for resource management and operational insights.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table categorizes the type of time entry as either owned, subcontracted, or external, facilitating accurate tracking and reporting of resource allocation.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used in a calculated column to retrieve a specific value from a related table. Here’s a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a table that has a relationship with another table called `lkp_Unit`. The relationship is established based on a common key or identifier.

2. **Purpose**: The expression aims to pull in data from the `lkp_Unit` table, specifically the column named `OWN-Sub-ExtT`. 

3. **Functionality**: 
   - **RELATED Function**: The `RELATED` function is designed to access a value from a related table. In this case, it looks for the value in the `OWN-Sub-ExtT` column of the `lkp_Unit` table that corresponds to the current row in the table where the calculated column is being created.
   - **Value Retrieval**: When this expression is evaluated for each row, it finds the related record in the `lkp_Unit` table and retrieves the value from the `OWN-Sub-ExtT` column.

4. **Outcome**: The result is that each row in the current table will have a new column (the calculated column) that contains the corresponding `OWN-Sub-ExtT` value from the `lkp_Unit` table. This allows for enriched data analysis, as it combines information from two related tables.

In summary, this DAX expression enhances the current table by adding a column that reflects specific information from a related table, enabling better insights and analysis based on that additional data.

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

1. **Purpose**: The calculated column is designed to determine a qualifying value for a project based on related data from another table (in this case, the 'lkp_Project' table).

2. **Logic**:
   - The expression first checks if the 'Qualify' field from the related 'lkp_Project' table is blank (i.e., it has no value).
   - If the 'Qualify' field is blank, the calculated column will return a value of **1**. This could indicate a default or fallback qualification status for projects that do not have a specified qualification.
   - If the 'Qualify' field is not blank (meaning it has a value), the calculated column will return the actual value from the 'Qualify' field in the 'lkp_Project' table.

3. **Outcome**: The result is that every project will have a qualification value. If a project has a defined qualification in the related table, it will use that value. If not, it will default to **1**. This approach ensures that there are no blank values in the 'QualifyPrj' column, making it easier to analyze and report on project qualifications.

In summary, this DAX expression helps to standardize project qualification values by providing a default when no specific qualification is available, ensuring that every project has a qualification status.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for submitting the timesheet data, facilitating accurate tracking and reporting of labor hours.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of timesheet entries, specifically reflecting whether they have been processed with the 'V_Z' validation criteria.
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
   - The first condition checks if the column `[ReportReady]` is marked as `TRUE()`. This likely indicates that a report is ready for processing or review.
   - The second condition checks if the value in the `vw_Timesheet[TimesheetCode]` column is either "V" or "Z". These codes probably represent specific types of timesheets or statuses that are relevant to the business.

3. **Output**:
   - If either of the conditions is met (i.e., the report is ready or the timesheet code is "V" or "Z"), the calculated column will return a value of `1`.
   - If neither condition is met, it will return a value of `0`.

4. **Business Implication**: This calculated column can be used to easily identify rows that are either ready for reporting or belong to specific categories (V or Z). This can help in filtering, analyzing, or aggregating data based on readiness or specific timesheet types, making it easier for decision-makers to focus on relevant entries.

In summary, `RReady_With_V_Z` serves as a flag to indicate whether a report is ready or if a timesheet falls into specific categories, facilitating better data management and analysis.

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

1. **Condition Check**: The measure first checks the value of another measure called `[ContractStatusMeasure]`. This measure likely indicates the status of a contract, and in this case, it specifically checks if the status is "Active".

2. **Calculating Hours**: If the contract status is "Active", the measure returns the value of another measure called `[HoursDifference]`. This measure probably calculates the difference in hours related to the contract, such as the time remaining until the contract expires or the hours worked under the contract.

3. **Alternative Outcome**: If the contract status is not "Active" (meaning it could be "Inactive", "Expired", or any other status), the measure returns the text "Not Active".

### Summary:
In summary, `ContractStatus2` provides a way to see how many hours are associated with an active contract. If the contract is active, it shows the hours; if not, it simply states that the contract is "Not Active". This measure helps users quickly understand the status of contracts and their associated hours, aiding in decision-making and resource management.

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

2. **Evaluate Conditions**:
   - **Is the End Date Blank?**: It checks if this maximum end date is blank (meaning there is no end date recorded). If there is no end date, it implies that the contract is still ongoing.
   - **Is the End Date in the Future?**: It also checks if the maximum end date is greater than today’s date. If the end date is in the future, it means the contract is still active.

3. **Determine Status**:
   - If either of the above conditions is true (the end date is blank or it is in the future), the measure returns "Active". This indicates that the contract is currently valid and ongoing.
   - If neither condition is true (meaning the end date is not blank and is in the past), it returns "Not Active". This indicates that the contract has ended.

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

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated starting from the first week of the year. 

So, when you combine these two functions, `WEEKNUM(TODAY())` effectively tells you which week of the year it is right now. For instance, if today is October 10, 2023, and it falls in the 41st week of the year, the expression will return the number 41.

In business terms, this measure can be useful for reporting and analysis purposes, such as tracking weekly performance, sales, or other metrics that are evaluated on a weekly basis. It helps businesses understand where they stand in the current week of the year.

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
   - **`CALCULATE`**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. It ensures that each employee is only counted once, even if they have multiple timesheet entries.
   - **`vw_Timesheet[Approved] = FALSE()`**: This condition filters the data to include only those timesheet entries where the `Approved` status is set to `FALSE`. In other words, it focuses on timesheets that have not been approved.

3. **Outcome**: The measure ultimately provides a count of distinct employees who have submitted timesheets that are still pending approval. This can be useful for tracking how many employees are waiting for their timesheets to be reviewed and approved, helping management understand the workload and approval process.

In summary, `Dax_EmpCount_Approved` gives you a clear view of how many unique employees have timesheets that are not yet approved, allowing for better management of timesheet approvals.

**`Dax_EmpCount_RReady`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[ReportReady] = FALSE()
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `Dax_EmpCount_RReady`. Here's a breakdown of what this measure does in simple business terms:

1. **Purpose**: The measure calculates the number of unique employees whose timesheets are not marked as "Report Ready."

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows us to apply filters to the data before performing calculations.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries in the timesheet.
   - **`vw_Timesheet[ReportReady] = FALSE()`**: This filter condition specifies that we only want to consider timesheet entries where the `ReportReady` status is set to FALSE. In other words, it filters out any entries that are marked as ready for reporting.

3. **Outcome**: The final result of this measure is the total count of distinct employees who have timesheet entries that are not ready for reporting. This can be useful for management to identify how many employees still need to finalize their timesheets before they can be processed or reported.

In summary, `Dax_EmpCount_RReady` helps track the number of employees who have not yet completed their timesheet reporting, allowing for better management of reporting deadlines and ensuring that all necessary timesheets are ready for review.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees recorded in the `vw_Timesheet` table.

### Breakdown of the Expression:

- **DISTINCTCOUNT**: This function counts the number of distinct (unique) values in a specified column. In this case, it is counting unique entries.
  
- **vw_Timesheet[EmployeeName]**: This specifies the column from which the unique values are being counted. Here, it refers to the `EmployeeName` column in the `vw_Timesheet` table.

### What It Achieves:

- **Unique Employee Count**: The measure calculates how many different employees have entries in the timesheet. For example, if three employees submitted timesheets, the result would be 3, even if some employees submitted multiple timesheets.

### Business Implication:

This measure is useful for understanding workforce participation or engagement. It helps businesses track how many distinct employees are actively logging their hours, which can be important for resource planning, payroll, and analyzing employee workload.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the average hourly rate for sales based on data from a table called `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the `SalesAmount` column of the `vw_Timesheet` table. Essentially, it totals the revenue generated from sales.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked, as recorded in the `Hours` column of the same table. It gives the total number of hours that have been logged.

3. **Division**: The entire expression divides the total sales amount by the total hours worked. This calculation gives you the average hourly rate, which represents how much revenue is generated for each hour worked.

### In Summary:
The measure `MSR_AVG_HOURLY_RATE` calculates the average revenue earned per hour worked by taking the total sales and dividing it by the total hours logged. This metric is useful for understanding productivity and profitability on an hourly basis.

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
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called `Msr_Budget`, which is designed to calculate a budget figure based on sales data, specifically focusing on the current week and the previous weeks' performance. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: 
   - The measure starts by determining the current week number of the year using `SELECTEDVALUE(DimDate[WeekNumberOfYear])`. This means it looks at the date context in which the measure is being evaluated and identifies which week we are currently in.

2. **Calculate Previous Budget**:
   - Next, it calculates the budget for all previous weeks (i.e., weeks before the current week). This is done using the `CALCULATE` function, which modifies the context of the calculation.
   - Inside `CALCULATE`, it uses the measure `[Msr_SalesPriceBaseCurrency]`, which presumably represents the sales price in the base currency. 
   - The `FILTER` function is applied to the `DimDate` table to include only those weeks where the week number is less than the current week. The `ALLSELECTED` function ensures that any filters applied to the date context are respected, but it still allows for the calculation of previous weeks.

3. **Return the Appropriate Value**:
   - Finally, the measure uses an `IF` statement to decide what value to return:
     - If the current week's sales price (represented by `[Msr_SalesPriceBaseCurrency]`) is not blank (meaning there is actual sales data for the current week), it returns that value.
     - If the current week's sales price is blank (indicating no sales data), it returns the calculated budget from previous weeks (`PrevBudget`).

### Summary:
In summary, this measure calculates the budget for the current week based on actual sales data if available. If there are no sales for the current week, it falls back to the budget calculated from previous weeks. This approach helps businesses understand their current performance in relation to past performance, ensuring they have a budget figure to work with even when current data is lacking.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'MSR_Cumulative Cost', which represents the total cost accumulated up to a selected month in a report or dashboard.

Here's a breakdown of what the code does in simple business terms:

1. **Identify the Selected Month**: 
   - The code starts by determining which month is currently selected in the visual (like a chart or table). It uses the `MAX` function to find the highest month number from the 'DimDate' table. This means if you are looking at data for March, the `SelectedMonth` variable will hold the value for March (which is 3).

2. **Calculate Cumulative Cost**:
   - The `CALCULATE` function is then used to compute the total cost. Specifically, it sums up the `CostAmount` from the `vw_Timesheet` table.

3. **Filter for Relevant Months**:
   - The `FILTER` function is applied to ensure that only the months up to and including the selected month are considered in the calculation. The `ALLSELECTED` function allows the measure to respect any filters that might be applied in the report, but it removes any filters specifically on the 'DimDate' table. This means it looks at all months available in the data but only sums costs for those months that are less than or equal to the selected month.

In summary, this DAX measure calculates the total cost incurred from the beginning of the year up to the month that the user has selected in the report. This is useful for understanding cumulative expenses over time, helping businesses track their costs effectively.

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
   - The main goal of this measure is to calculate the total sales amount from the beginning of the year up to and including the selected month.
   - It uses the `CALCULATE` function to change the context of the calculation. This means it will sum up the sales amounts while applying specific filters.

3. **Filter for Relevant Months**:
   - The `FILTER` function is used to create a condition that includes all months from the start of the year up to the selected month. 
   - The `ALLSELECTED` function ensures that any other filters applied in the report (like year or product filters) are still respected, but it removes any filters specifically on the 'DimDate' table. This allows the measure to consider all months up to the selected month.

4. **Final Calculation**:
   - Finally, the `SUM` function adds up the sales amounts from the 'vw_Timesheet' table for all the months that meet the filtering criteria (i.e., all months up to the selected month).

### Summary:
In summary, this DAX measure calculates the total sales from the beginning of the year up to the month that is currently selected in the report. This allows users to see how sales have accumulated over time, providing valuable insights into sales performance for the selected period.

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

1. **Total Hours Calculation**: 
   - The variable `TotalValue` is created to calculate the total number of hours worked. It uses the `CALCULATE` function to sum up the hours from the `vw_Timesheet` table.
   - The `ALLSELECTED` function is used to ensure that this total includes all selected filters for `GroupCat`, `Project`, and `EmployeeName`. This means that if you have filters applied in your report (like viewing a specific project or employee), it will still consider all relevant data for those categories when calculating the total.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours worked by the current context (like a specific employee or project) by dividing the sum of hours for that context (`SUM(vw_Timesheet[Hours])`) by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safe way to handle division in DAX. If the `TotalValue` is zero (meaning there are no hours to compare against), it will return 0 instead of causing an error.

In summary, this measure calculates what portion of the total hours worked (across all selected filters) is represented by the hours worked in the current context. This is useful for understanding how much of the total effort is contributed by a specific group, project, or employee.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the project margin for a specific measure called 'MSR_ProjectMargin'. Here's a breakdown of what it does in simple business terms:

1. **Sales Amount**: The first part of the expression, `sum(vw_Timesheet[SalesAmount])`, adds up all the sales amounts recorded in the `vw_Timesheet` table. This represents the total revenue generated from projects.

2. **Cost Amount**: The second part, `sum(vw_Timesheet[CostAmount])`, sums up all the costs associated with those projects. This includes all expenses incurred to deliver the projects.

3. **Calculating Margin**: The expression then subtracts the total cost from the total sales amount: 
   - `sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`. 
   This calculation gives you the project margin, which is essentially the profit made from the projects after accounting for the costs.

### In Summary:
The measure 'MSR_ProjectMargin' calculates the profit from projects by taking the total sales revenue and subtracting the total costs. A positive result indicates that the projects are profitable, while a negative result would suggest a loss. This measure helps businesses assess the financial performance of their projects.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called `Msr_SalesPriceBaseCurrency`, which essentially sums up the sales price in a base currency for different projects and clients. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The measure uses a table called `vw_Timesheet`, which likely contains records of timesheets related to various projects and clients.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key fields: `ProjectProfile` and `Client`. This means that the calculation will be done for each unique combination of project and client.

3. **Calculating Measure Value**: For each group (each unique project-client combination), it retrieves a value called `SalesPriceBaseCurrency` from another table called `tbl_Project`. The `SELECTEDVALUE` function is used here to get the sales price for the current context (i.e., the specific project and client being evaluated).

4. **Summing Up Values**: After creating this summarized table with the sales price for each project-client combination, the `SUMX` function iterates over this summarized table and sums up the `MeasureValue` (the sales price) for all the groups.

### In Summary:
The measure `Msr_SalesPriceBaseCurrency` calculates the total sales price in a base currency for all projects and clients listed in the `vw_Timesheet`. It does this by grouping the data by project and client, retrieving the relevant sales price for each group, and then summing these prices together. This helps businesses understand the total sales revenue in a consistent currency across different projects and clients.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called "Static Total Employees." Let's break it down into simple terms to understand what it achieves:

1. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part of the code counts the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table. Essentially, it tells us how many different employees are present in the data.

2. **CALCULATE**: This function modifies the context in which the data is evaluated. It allows us to apply specific filters to the data before performing the count.

3. **ALL(vw_Timesheet)**: This part removes any existing filters that might be applied to the `vw_Timesheet` table. By using `ALL`, the measure ensures that it considers all records in the table, regardless of any other filters that might be in place in the report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This condition filters the data to only include employees whose contract status is marked as "Valid." It means that we are only interested in counting employees who are currently active or valid in terms of their employment status.

### Summary:
In summary, this DAX measure calculates the total number of unique employees who have a valid contract status, ignoring any other filters that might be applied to the data. This is useful for getting a clear picture of how many employees are actively employed under valid contracts, regardless of any other criteria that might be affecting the data view.

**`TotalActualHours`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression `SUM(vw_Timesheet[Hours])` is used to calculate the total number of hours recorded in a timesheet. 

Here's a breakdown of what it does:

- **SUM**: This function adds up all the values in a specified column.
- **vw_Timesheet**: This refers to a table (or view) that contains timesheet data. It likely includes various columns related to employee hours worked, projects, dates, etc.
- **[Hours]**: This specifies the column within the `vw_Timesheet` table that contains the actual hours worked by employees.

In simple business terms, this measure calculates the total actual hours worked by all employees as recorded in the timesheet. It provides a single number that represents the sum of all hours logged, which can be useful for reporting, payroll, project management, or performance analysis.

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

4. **Putting It All Together**: The `SUMX` function iterates over each unique employee name, calculates the maximum contracted hours per week for that employee, and then sums all those maximum values together.

### What It Achieves:
The measure `TotalContractedHours` calculates the total number of contracted hours for all employees by taking the highest weekly hours each employee is contracted for and adding them up. This gives a clear picture of the total contracted hours across the organization, which can be useful for resource planning, budgeting, and understanding workforce capacity.

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

2. **TotalContractedHours**: This measure indicates the total number of hours that were agreed upon in a contract or plan for the project or task. It represents the expected or planned effort.

3. **The Calculation**: The expression `[TotalActualHours] - [TotalContractedHours]` subtracts the total contracted hours from the total actual hours. 

4. **What It Achieves**: 
   - If the result is positive, it means that more hours were worked than were originally planned (i.e., there is an overage in hours).
   - If the result is negative, it indicates that fewer hours were worked than expected (i.e., there is an underage in hours).
   - If the result is zero, it means that the actual hours worked matched the contracted hours exactly.

In summary, this measure helps businesses understand whether they are exceeding, meeting, or falling short of their planned work hours, which is crucial for project management, budgeting, and resource allocation.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called **TotalHoursTracked**. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The expression starts by checking if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Zero if Blank**: If the sum of hours is blank (meaning no hours have been tracked), the measure will return **0**. This is important because it ensures that when there are no entries, the measure does not show a blank value, which could be confusing in reports.

3. **Return the Sum of Hours if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the measure will return the actual total sum of hours tracked from the `vw_Timesheet` table.

### Summary:
In essence, this measure calculates the total hours tracked from a timesheet. If no hours are recorded, it returns 0; otherwise, it returns the total hours. This helps provide clear and actionable insights into time tracking without leaving gaps in the data.

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

2. **Return "Empty" if No Hours**: If the sum of hours is blank (meaning no hours have been tracked), the measure returns the text "Empty". This indicates that there is no data available for hours tracked.

3. **Return Total Hours if Available**: If there are hours recorded (i.e., the sum is not blank), the measure returns the actual total sum of hours tracked.

### Summary:
In essence, this measure helps to identify whether there are any hours recorded in the timesheet. If there are no hours, it clearly indicates that with the word "Empty". If there are hours, it provides the total number of hours tracked. This is useful for reporting and analysis, as it allows users to quickly understand if there is data available or if they need to investigate further.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is a measure named `trackedDiff`. Let's break it down into simpler terms to understand what it calculates or achieves.

### Components of the Measure:

1. **[TotalHoursTracked]**: This part represents the total number of hours that have been tracked for a specific context, such as a project, employee, or time period. It is likely a measure that sums up all the hours recorded in a timesheet.

2. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to calculate a specific value while ignoring certain filters.

3. **MAXX Function**: This function evaluates an expression for each row in a table and returns the maximum value. Here, it is used to find the maximum number of hours worked per week from the distinct values in the `vw_Timesheet[HoursPerWeek]` column.

4. **DISTINCT Function**: This function returns a unique list of values from the specified column. In this case, it ensures that only unique weekly hour values are considered when calculating the maximum.

5. **ALLEXCEPT Function**: This function removes all filters from the `vw_Timesheet` table except for the filter on `EmployeeID`. This means that the calculation of the maximum hours per week will only consider the specific employee, ignoring any other filters that might be applied.

### What It Achieves:

Putting it all together, the measure `trackedDiff` calculates the difference between the total hours tracked for a specific employee and the maximum hours they have worked in any single week. 

- **In Business Terms**: This measure helps to identify how much the total hours tracked for an employee exceeds their maximum weekly hours. If the result is positive, it indicates that the employee has tracked more hours than their highest recorded week, which could suggest overwork or an anomaly in tracking. If it’s negative or zero, it indicates that the tracked hours are within or below their maximum weekly hours, suggesting normal tracking behavior.

### Summary:

In summary, `trackedDiff` provides insights into an employee's workload by comparing their total tracked hours against their maximum weekly hours, helping management understand workload distribution and identify potential issues with overworking employees.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates a measure called `trackedDiff2`, which is designed to find the difference between two values related to hours tracked in a timesheet.

Here's a breakdown of what this measure does in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked by an employee over a specific period.

2. **Maximum Hours Per Week**: The next part of the calculation uses the `CALCULATE` function to determine the maximum number of hours that have been recorded per week for the employee. This is done using:
   - `MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])`: This part looks at the unique values of hours per week from the `vw_Timesheet` table and finds the maximum value among them.
   - `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])`: This ensures that any filters applied to the `HoursPerWeek` column are ignored, allowing the calculation to consider all weeks for the employee.
   - `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])`: This keeps the context of the specific employee while ignoring other filters, ensuring that the maximum hours per week are calculated only for that employee.

3. **Calculating the Difference**: Finally, the measure subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. 

### What It Achieves:
In summary, `trackedDiff2` calculates how many more hours an employee has tracked compared to their maximum recorded hours in any single week. This can help identify if an employee is consistently working more hours than their typical maximum or if there are any discrepancies in their time tracking. It provides insights into workload management and can assist in identifying potential issues with time reporting or employee workload.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the total billable hours for specific projects from a timesheet data source. Let's break it down into simpler terms:

1. **Variable Definition (FilteredHours)**: 
   - The code starts by creating a variable called `FilteredHours`. This variable is used to store a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**:
   - The filter applied to `vw_Timesheet` checks for two main conditions:
     - **Condition 1**: The project qualifies as billable if `vw_Timesheet[QualifyPrj]` equals 1 (indicating it's a qualifying project) **and** `vw_Timesheet[BillablePrj]` equals 1 (indicating it's a billable project).
     - **Condition 2**: Alternatively, it checks if the project name contains the phrase "Customer Success Services". This is done using the `SEARCH` function, which looks for that specific text within the `vw_Timesheet[Project]` field.

3. **Calculating Total Hours**:
   - After filtering the timesheet data based on the above criteria, the code uses the `SUMX` function to calculate the total hours worked on the filtered projects. `SUMX` iterates over each row in the `FilteredHours` variable and sums up the `vw_Timesheet[Hours]` for those rows.

4. **Final Outcome**:
   - The final result of this measure, `UTI_TotalBillableHours`, is the total number of hours that are billable, either from qualifying projects or from projects related to "Customer Success Services".

In summary, this DAX measure effectively calculates the total billable hours from a timesheet, focusing on specific qualifying projects and any projects that fall under "Customer Success Services". This helps the business understand how many hours can be billed to clients based on these criteria.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `UTI_TOTALHOURS`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the total number of hours worked on specific projects that meet a certain qualification criteria.

2. **Components**:
   - **SUM(vw_Timesheet[Hours])**: This part of the code adds up all the values in the `Hours` column from the `vw_Timesheet` table. Essentially, it totals the hours recorded in timesheets.
   - **CALCULATE**: This function modifies the context in which the data is evaluated. In this case, it is used to filter the data before performing the sum.
   - **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to include only those entries where the `QualifyPrj` column equals 1. This means it only considers projects that are marked as qualified.

3. **Outcome**: The measure `UTI_TOTALHOURS` will return the total hours worked on projects that are qualified (where `QualifyPrj` is 1). This is useful for reporting and analysis, as it helps businesses understand how many hours are being spent on projects that meet specific criteria, which can inform resource allocation, project management, and performance evaluation.

In summary, this DAX measure helps organizations track and analyze the total hours worked on qualified projects, providing valuable insights into project performance and resource utilization.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is a measure named 'UTILIZATION_New'. Let's break it down into simple business terms to understand what it calculates or achieves.

### Key Components of the Measure:

1. **Variables Defined**:
   - **_TotalBillableHours**: This variable captures the total number of hours that can be billed to clients. It is represented by another measure called `[UTI_TotalBillableHours]`.
   - **_TotalHours**: This variable captures the total number of hours worked, represented by the measure `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The measure first checks if there are any active contacts using `[Msr_ActiveContacts]`. If there are no active contacts (i.e., the measure is blank), the calculation will not proceed further.
   - If there are active contacts, it then checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the calculation will not proceed.
   - If both conditions are met (active contacts exist and total hours are available), it calculates the utilization rate by dividing the total billable hours by the total hours worked.

3. **Handling Division**:
   - The measure uses the `DIVIDE` function to perform the division. This function is preferred because it safely handles cases where the denominator (total hours) might be zero or blank. If the division results in a blank (which would happen if total hours are zero), it returns 0 instead.

### What It Achieves:

In summary, this measure calculates the utilization rate of resources (like employees or contractors) by determining the proportion of billable hours to total hours worked. 

- **Utilization Rate**: This is a key performance indicator (KPI) in many businesses, especially in service industries, as it helps assess how effectively resources are being used. A higher utilization rate indicates that more of the available working hours are being spent on billable work, which is generally a positive sign for profitability.

- **Business Decision Support**: By ensuring that the measure only calculates when there are active contacts and valid total hours, it provides a reliable metric that can be used for reporting and decision-making regarding resource allocation and efficiency.

In essence, 'UTILIZATION_New' helps businesses understand how well they are utilizing their workforce in terms of billable work, which is crucial for financial performance and operational efficiency.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

The following filters are applied to the entire report:

- Filter on `DimDate`.`CurrentYearFlag` (Type: Advanced, Explanation: This Power BI filter definition JSON is designed to filter data based on the `CurrentYearFlag` column from the `DimDate` table. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter specifically targets the `CurrentYearFlag` field in the `DimDate` table. This field typically indicates whether a date belongs to the current year.

2. **Filter Condition**: The filter is set to include only those records where the `CurrentYearFlag` is equal to `1`. In this context, a value of `1` usually means "true" or "yes," indicating that the date is indeed part of the current year.

3. **Inclusion of Data**: When this filter is applied, it will include all records from the `DimDate` table that have a `CurrentYearFlag` of `1`. This means only dates from the current year will be considered in any analysis or visualizations.

4. **Exclusion of Data**: Conversely, any records where the `CurrentYearFlag` is not `1` (for example, dates from previous years or future years) will be excluded from the analysis. 

In summary, this filter ensures that only the dates from the current year are included in the data set, allowing users to focus their analysis on this specific time frame.)

### <a name="report-pages"></a>Report Pages

#### <a name="page-customer-analysis"></a>Page: Customer Analysis

*Internal Name: `ecc7667875c00809763b`, Ordinal: 0*

##### Page Level Filters

- Filter on `vw_Timesheet`.`BillablePrj` (Type: Advanced, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data from the `vw_Timesheet` view, particularly the `BillablePrj` column. Let's break it down in simple business terms:

1. **Source of Data**: The filter is applied to a data source called `vw_Timesheet`, which likely contains records related to timesheets, including information about billable projects.

2. **Target Column**: The filter specifically targets the `BillablePrj` column within this data source. This column likely indicates whether a project is billable or not.

3. **Filter Condition**: The filter condition is set to include only those records where the `BillablePrj` value is equal to `1L`. In many business contexts, a value of `1` typically signifies that a project is billable, while other values (like `0`) might indicate non-billable projects.

4. **Inclusion of Data**: As a result, when this filter is applied, it will **include only the records** from the `vw_Timesheet` where the `BillablePrj` is marked as `1L`. This means you will only see data related to projects that can be billed to clients.

5. **Exclusion of Data**: Conversely, any records where `BillablePrj` is not equal to `1L` (such as `0` or any other value) will be **excluded** from the analysis. This helps in focusing on the financial aspects of the projects that generate revenue.

In summary, this filter is used to isolate and analyze only the billable projects from the timesheet data, ensuring that any insights or reports generated will reflect only those projects that can contribute to the company's income.)

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
  - Filter on `tbl_Project`.`ClientName` (Type: TopN, Explanation: This Power BI filter definition JSON is designed to filter the data in the `tbl_Project` table based on the `ClientName` field. Here’s a breakdown of what this filter does in simple business terms:

1. **Purpose of the Filter**: The filter aims to include only those projects from the `tbl_Project` table where the `ClientName` matches certain criteria derived from another data source.

2. **Source of Criteria**: The criteria for filtering are determined by a subquery that pulls data from two sources:
   - `tbl_Project` (referred to as `t`)
   - `vw_Timesheet` (referred to as `v`)

3. **Selecting Client Names**: The subquery specifically selects the `ClientName` from the `tbl_Project` table. However, it does this by first determining which clients have the highest total hours recorded in the `vw_Timesheet` view.

4. **Top Clients**: The subquery is set to return the top 5 clients based on the total hours worked (from the `Hours` column in `vw_Timesheet`). This means that only the clients who have the most hours logged will be considered.

5. **Filtering Logic**: The main filter then checks if the `ClientName` in `tbl_Project` is among those top 5 clients identified in the subquery. If a project’s `ClientName` is in this list, it will be included in the results; if not, it will be excluded.

### Summary:
In summary, this filter will include projects from `tbl_Project` only if their `ClientName` is one of the top 5 clients based on the total hours recorded in the `vw_Timesheet`. This allows the analysis to focus on the most significant clients in terms of hours worked, potentially highlighting key projects that are more relevant for business insights.)

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
  - Filter on `vw_Timesheet`.`cc_Employer` (Type: Categorical, Explanation: This Power BI filter definition is designed to focus on a specific subset of data from the `vw_Timesheet` view, particularly concerning the `cc_Employer` field. Here’s a breakdown of what this filter does in simple business terms:

1. **Data Source**: The filter is applied to a data source called `vw_Timesheet`, which likely contains timesheet records related to various employers.

2. **Target Field**: The filter specifically targets the `cc_Employer` column within this data source. This column presumably contains the names or identifiers of employers associated with the timesheet entries.

3. **Filter Condition**: The filter condition specifies that we are only interested in records where the `cc_Employer` is equal to `'G Cloud NL'`. 

4. **Inclusion of Data**: As a result, when this filter is applied, only the timesheet entries that belong to the employer named `'G Cloud NL'` will be included in the analysis or report. 

5. **Exclusion of Data**: Conversely, any timesheet entries associated with other employers will be excluded from the results. This means that if an entry has a different employer name, it will not appear in the filtered dataset.

In summary, this filter is used to narrow down the data to only those timesheet records that are linked to the employer `'G Cloud NL'`, effectively excluding all other employers from the analysis.)

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
  - Filter on `DimDate`.`WeekNumberOfYear` (Type: Advanced, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data related to the `DimDate` table, particularly the `WeekNumberOfYear` column. Let's break it down in simple business terms:

1. **Target Data**: The filter is applied to the `WeekNumberOfYear` field in the `DimDate` table. This field represents the week number of the year (for example, Week 1, Week 2, etc.).

2. **Filter Condition**: The filter specifies a condition that includes only those records where the `WeekNumberOfYear` is equal to **4**. 

3. **Inclusion Criteria**: 
   - Only data from the 4th week of the year will be included in any analysis or visualizations that use this filter. 
   - This means that if you are looking at sales data, for example, only the sales that occurred during the 4th week of the year will be considered.

4. **Exclusion Criteria**: 
   - All records from other weeks (Week 1, Week 2, Week 3, Week 5, etc.) will be excluded from the analysis. 
   - This helps in narrowing down the focus to just the data relevant to the 4th week.

In summary, when this filter is applied, it ensures that only the data corresponding to the 4th week of the year is included in your reports or dashboards, while all other weeks are ignored. This can be particularly useful for analyzing trends, performance, or specific events that occurred during that week.)
  - Filter on `vw_Timesheet`.`Sum(SalesAmount)` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which data is included when calculating the total sales amount from the `vw_Timesheet` view, specifically for the `Sum(SalesAmount)` measure.

### Breakdown of the Filter:

1. **Source of Data**: The filter is applied to a data source called `vw_Timesheet`, which likely contains records of timesheets and associated sales amounts.

2. **Condition Applied**: The filter specifies a condition that excludes certain records based on their sales amount. 

3. **Sales Amount Comparison**:
   - The filter checks the `SalesAmount` for each record in the `vw_Timesheet`.
   - It compares the `SalesAmount` to a value of **50** (the "50L" indicates a long integer, but for our purposes, we can think of it simply as the number 50).

4. **Exclusion Logic**: 
   - The filter uses a "Not" condition, meaning it will **exclude** records where the `SalesAmount` is **equal to or greater than 50**.
   - In simpler terms, if a record has a sales amount of 50 or more, it will not be included in the total calculation.

### Summary:
When this filter is applied to the `Sum(SalesAmount)` calculation, it will only include records from the `vw_Timesheet` where the `SalesAmount` is **less than 50**. Any record with a sales amount of 50 or higher will be excluded from the total. This allows for a focused analysis on lower sales amounts, potentially to understand performance in that specific range.)
  - Filter on `vw_missing_timesheet`.`BillableDep` (Type: Categorical, Explanation: This Power BI filter definition JSON is designed to filter data from a specific view called `vw_missing_timesheet`, focusing on a column named `BillableDep`. Here’s a breakdown of what this filter does in simple business terms:

1. **Source of Data**: The filter is applied to a data view called `vw_missing_timesheet`. This view likely contains records related to timesheets, possibly indicating which employees have or have not submitted their timesheets.

2. **Target Column**: The filter specifically targets the `BillableDep` column within this view. This column likely indicates whether a particular entry is associated with a billable department.

3. **Filter Condition**: The filter condition specifies that we are interested in entries where the `BillableDep` value is equal to "1L". This means:
   - Only records where the `BillableDep` is "1L" will be included in the results.
   - Any records with a different value in the `BillableDep` column will be excluded from the results.

### Summary:
In summary, when this filter is applied, it will only show records from the `vw_missing_timesheet` view where the `BillableDep` is "1L". All other records with different `BillableDep` values will be filtered out, allowing users to focus solely on the relevant data associated with that specific department.)

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
  - Filter on `vw_Timesheet`.`MAIN_UNIT` (Type: Categorical, Explanation: This Power BI filter definition is designed to focus on specific data from a table called `vw_Timesheet`, particularly the column named `MAIN_UNIT`. Here's a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Target Data**: The filter is applied to the `MAIN_UNIT` column in the `vw_Timesheet` view.
- **Included Values**: The filter specifies that only records where the `MAIN_UNIT` is one of the following three values will be included in the analysis:
  - **'G Cloud'**
  - **'M-Cloud'**
  - **'Public'**

### What the Filter Excludes:
- **Excluded Values**: Any records in the `vw_Timesheet` where the `MAIN_UNIT` is not one of the three specified values will be excluded from the analysis. This means that if a record has a `MAIN_UNIT` value like 'Private', 'Other', or anything else that is not 'G Cloud', 'M-Cloud', or 'Public', it will not be considered in the results.

### Summary:
In summary, this filter is used to narrow down the data to only those entries in the `vw_Timesheet` that belong to the specified units ('G Cloud', 'M-Cloud', 'Public'), effectively excluding all other units from the analysis. This helps in focusing on specific segments of data that are relevant for reporting or analysis purposes.)
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which employee names are included in a report or visualization based on the `EmployeeName` field from the `vw_Timesheet` data source.

### Breakdown of the Filter:

1. **Target Data Source**: The filter is applied to the `vw_Timesheet` view, specifically focusing on the `EmployeeName` column.

2. **Filter Logic**: The filter uses a condition that specifies which employee names should be excluded from the results:
   - **Contains Condition**: The filter checks if the `EmployeeName` contains the substring "van".
   - **Not Condition**: The filter is set to exclude any employee names that contain "van".

### What This Means in Business Terms:

- **Included Data**: All employee names that do **not** have "van" anywhere in their name will be included in the report. For example, names like "John Smith", "Alice Johnson", or "Mary Brown" would be included.
  
- **Excluded Data**: Any employee names that do contain "van" will be excluded from the report. This means names like "Vanessa", "Ivan", or "Savannah" will not appear in the results.

### Summary:

In summary, this filter is used to ensure that the report only shows employee names that do not include the substring "van". This could be useful in scenarios where you want to focus on a specific group of employees while filtering out those whose names contain that particular string.)

#### <a name="page-project-details"></a>Page: Project Details

*Internal Name: `2f52ad4a004b09107900`, Ordinal: 1*

##### Page Level Filters

- Filter on `vw_Timesheet`.`BillablePrj` (Type: Advanced, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data from the `vw_Timesheet` view, particularly the `BillablePrj` column. Here’s a breakdown of what this filter does in simple business terms:

1. **Data Source**: The filter is applied to a data source called `vw_Timesheet`, which likely contains records related to timesheets, including information about billable projects.

2. **Target Column**: The filter specifically targets the `BillablePrj` column within this data source. This column probably indicates whether a project is billable or not.

3. **Filter Condition**: The filter condition is set to include only those records where the `BillablePrj` value is equal to `1L`. In many systems, a value of `1` typically indicates "true" or "yes," suggesting that the project is billable.

4. **Inclusion Criteria**: Therefore, when this filter is applied, it will **include** only the records from the `vw_Timesheet` where the `BillablePrj` is marked as billable (i.e., where `BillablePrj` equals `1L`).

5. **Exclusion Criteria**: Conversely, any records where `BillablePrj` is not equal to `1L` (which could represent non-billable projects) will be **excluded** from the results.

In summary, this filter is used to isolate and analyze only the timesheet entries that are associated with billable projects, allowing users to focus on revenue-generating activities.)

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
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which employee names are included in the data from the `vw_Timesheet` view. Let's break it down in simple business terms:

1. **Source of Data**: The filter is applied to a data source called `vw_Timesheet`, which likely contains timesheet records for employees.

2. **Target Field**: The specific field being filtered is `EmployeeName`, which represents the names of employees in the timesheet data.

3. **Filter Condition**: The filter is set up to **exclude** any records where the `EmployeeName` is `null`. In other words, it is looking for employee names that are present and not empty.

4. **What This Means for Data**:
   - **Included Data**: Only records where `EmployeeName` has a valid name (i.e., it is not empty or missing) will be included in the results.
   - **Excluded Data**: Any records where `EmployeeName` is `null` (meaning there is no name provided for that employee) will be excluded from the results.

In summary, this filter ensures that the analysis only considers employees who have a name listed in the timesheet, thereby improving the quality and relevance of the data being analyzed.)

**Actual vs Budget**

- Type: `lineChart`
- Name: `76b4488a85c8253bac24`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Actual` (Query: `vw_Timesheet.MSR_Cumulative_Revenue`) (Role: Y)
  - `Budget` (Query: `vw_Timesheet.Msr_Budget`) (Role: Y)
- Visual Level Filters:
  - Filter on `DimDate`.`WeekNumberOfYear` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which data is included or excluded based on the `WeekNumberOfYear` field from the `DimDate` table.

### Breakdown of the Filter:

1. **Target Data**: The filter specifically targets the `WeekNumberOfYear` column in the `DimDate` table. This column typically represents the week number of the year (e.g., Week 1, Week 2, etc.).

2. **Filter Condition**: The filter is set up to exclude any records where the `WeekNumberOfYear` is null. In simpler terms, it means:
   - **Included Data**: Only records that have a valid week number (i.e., not null) will be included in the analysis.
   - **Excluded Data**: Any records where the week number is missing (null) will be excluded from the results.

### Business Implications:

- **Data Integrity**: By applying this filter, you ensure that your analysis only considers complete data where the week number is known. This is important for accurate reporting and insights.
- **Focus on Relevant Time Periods**: This filter helps in focusing on weeks that have actual data, which can be crucial for time-based analyses, such as sales performance or operational metrics over specific weeks.

In summary, this filter ensures that only records with a defined week number are included in your Power BI reports, thereby enhancing the quality and relevance of the data being analyzed.)
  - Filter on `vw_Timesheet`.`[MSR_Cumulative_Revenue]` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which data is included when analyzing the `MSR_Cumulative_Revenue` measure from the `vw_Timesheet` view. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter is applied to the `MSR_Cumulative_Revenue` measure, which likely represents the total cumulative revenue recorded in timesheets.

2. **Filter Condition**: The key part of this filter is the condition specified in the "Where" section. It states that we want to **exclude** any records where the `MSR_Cumulative_Revenue` is **null**.

3. **What is Included**: As a result of this filter, only those records where `MSR_Cumulative_Revenue` has a valid (non-null) value will be included in the analysis. This means that any timesheet entries that do not have a revenue figure will be ignored.

4. **Business Impact**: By applying this filter, the analysis will focus solely on timesheet entries that contribute to revenue, ensuring that any calculations, visualizations, or reports generated will reflect only meaningful data. This helps in making informed business decisions based on accurate revenue figures.

In summary, this filter ensures that only valid revenue data is considered, filtering out any entries that do not have a revenue value, thus providing a clearer picture of the organization's financial performance.)

