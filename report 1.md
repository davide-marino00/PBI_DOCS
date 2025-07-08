# Power BI Model & Report Documentation

*Generated on: 2025-07-02 16:05:48*

## Table of Contents

- [Overview](#overview)
- [Data Model](#data-model)
  - [Relationships](#relationships)
  - [Tables](#tables)
    - [brdg_Client](#table-brdg_client)
    - [DimDate](#table-dimdate)
    - [Hours_nd_Percentage](#table-hours_nd_percentage)
    - [lkp_Code](#table-lkp_code)
    - [lkp_fltr_Employee](#table-lkp_fltr_employee)
    - [lkp_Project](#table-lkp_project)
    - [lkp_SalesPerson](#table-lkp_salesperson)
    - [lkp_Unit](#table-lkp_unit)
    - [tbl_Project](#table-tbl_project)
    - [vw_missing_timesheet](#table-vw_missing_timesheet)
    - [vw_Timesheet](#table-vw_timesheet)
- [Report Structure](#report-structure)
  - [Report Level Filters](#report-level-filters)
  - [Report Pages](#report-pages)
    - [Customer Analysis](#page-customer-analysis)
    - [Project Details](#page-project-details)
    - [Drill](#page-drill)

---

## <a name="overview"></a>Overview

The Power BI data model appears to be designed for a business domain focused on project management and resource allocation, likely within a consulting or service-oriented organization. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a strong emphasis on tracking employee hours and project-related activities, enabling the organization to monitor time spent on various projects and identify any discrepancies in timesheet submissions. The inclusion of lookup tables like 'lkp_Project', 'lkp_SalesPerson', and 'lkp_Unit' indicates a structured approach to categorizing and analyzing data related to projects, sales personnel, and organizational units, facilitating detailed reporting and insights into operational efficiency.

The relationships established between these tables highlight a comprehensive framework for analyzing time allocation against projects, clients, and sales performance over time. For instance, linking 'vw_Timesheet' to 'DimDate' allows for time-based analysis, while connections to 'lkp_Code' and 'lkp_Unit' provide additional layers of detail regarding the nature of the work performed and the organizational context. Overall, this data model is likely aimed at enhancing decision-making capabilities by providing stakeholders with actionable insights into project performance, resource utilization, and client engagement.

---

## <a name="data-model"></a>Data Model

### <a name="relationships"></a>Relationships

The following relationships link the tables:

* `lkp_SalesPerson`.[`ClientName`] -> `brdg_Client`.[`ClientName`]
* `lkp_SalesPerson`.[`YearMonth`] -> `DimDate`.[`ShortDate`]
* `vw_missing_timesheet`.[`Project`] -> `lkp_Project`.[`Project`]
* `vw_missing_timesheet`.[`ShortDate`] -> `DimDate`.[`ShortDate`]
* `vw_missing_timesheet`.[`SubUnit`] -> `lkp_Unit`.[`Organisatorische eenheid`]
* `vw_missing_timesheet`.[`TimesheetCode`] -> `lkp_Code`.[`Code`]
* `vw_Timesheet`.[`Project`] -> `lkp_Project`.[`Project`]
* `vw_Timesheet`.[`ProjectCode`] -> `tbl_Project`.[`Project`] (Filter: bothDirections)
* `vw_Timesheet`.[`TimesheetCode`] -> `lkp_Code`.[`Code`]
* `vw_Timesheet`.[`TimesheetDate`] -> `DimDate`.[`ShortDate`]
* `vw_Timesheet`.[`Unit`] -> `lkp_Unit`.[`Organisatorische eenheid`]
* `vw_Timesheet`.[`Client`] -> `brdg_Client`.[`ClientName`] (**Inactive**)
* `vw_Timesheet`.[`ContractEndDate`] -> `DimDate`.[`ShortDate`] (**Inactive**)
* `vw_Timesheet`.[`ContractStartDate`] -> `DimDate`.[`ShortDate`] (**Inactive**)

---

### <a name="tables"></a>Tables

#### <a name="table-brdg_client"></a>Table: `brdg_Client`

The 'brdg_Client' table serves as a central repository for client information, capturing essential details such as ClientName to facilitate effective customer relationship management and enhance targeted marketing strategies. This table supports business analytics by enabling insights into client demographics and engagement patterns.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each record in the 'brdg_Client' table, facilitating easy identification and management of client relationships. |

---

#### <a name="table-dimdate"></a>Table: `DimDate`

The 'DimDate' table serves as a comprehensive date dimension, providing essential temporal attributes that enable businesses to analyze trends and performance over time. By offering detailed breakdowns of dates, including day, week, month, and quarter information, this table supports effective time-based reporting and enhances the ability to perform time-series analysis across various business metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | Column Description: The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of the date, facilitating time-based analysis and reporting across various business metrics. |
| `DateKey` | `int64` | Column Description: The 'DateKey' column (int64) serves as a unique identifier for each date in the 'DimDate' table, facilitating efficient date-based queries and analysis within the scheduling and reporting processes. |
| `DayName` | `string` | The 'DayName' column in the 'DimDate' table stores the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | Column Description: The 'DayNumberOfMonth' column (int64) represents the numerical day of the month, ranging from 1 to 31, facilitating date-related analyses and scheduling tasks within the DimDate table. |
| `DayNumberOfWeek` | `int64` | Column Description: The 'DayNumberOfWeek' column represents the day of the week as an integer, where values range from 1 (Sunday) to 7 (Saturday), facilitating date-related analyses and scheduling tasks within the DimDate table. |
| `DayNumberOfYear` | `int64` | Column Description: The 'DayNumberOfYear' column (int64) represents the sequential day of the year, ranging from 1 to 365 (or 366 for leap years), facilitating date-based analysis and reporting within the DimDate table. |
| `MonthName` | `string` | Column Description: The 'MonthName' column contains the full names of the months, facilitating easy identification and reporting of dates within the DimDate table. |
| `MonthNumberOfYear` | `int64` | Column Description: Represents the numerical value of the month (1 for January through 12 for December) to facilitate date-related analyses and reporting within the DimDate table. |
| `ShortDate` | `dateTime` | Column Description: The 'ShortDate' column captures the date in a concise format, facilitating efficient date-based queries and reporting within the DimDate table. |
| `WeekNumberOfYear` | `int64` | Column Description: Represents the week number of the year, ranging from 1 to 52, to facilitate date-based analysis and reporting within the DimDate table. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Column Description: Indicates whether the date corresponds to the current calendar year, with values typically set to 'Yes' or 'No' for easy filtering and reporting.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `CurrentYearFlag` in a data model, typically within a tool like Power BI. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The `CurrentYearFlag` column is designed to indicate whether a date in the `DimDate` table falls within the current year or not.

2. **How It Works**:
   - The expression `YEAR(DimDate[ShortDate])` extracts the year from the date in the `ShortDate` column of the `DimDate` table.
   - The expression `YEAR(TODAY())` gets the current year based on today's date.
   - The `IF` function checks if the year extracted from `DimDate[ShortDate]` is less than or equal to the current year.

3. **Output**:
   - If the year from `DimDate[ShortDate]` is less than or equal to the current year, the expression returns `1`. This means that the date is either in the current year or a previous year.
   - If the year is greater than the current year, it returns `0`, indicating that the date is in a future year.

4. **Result**: 
   - The `CurrentYearFlag` column will have a value of `1` for all dates that are in the current year or any previous year, and a value of `0` for dates that are in the future. 

In summary, this DAX expression helps to easily identify and flag dates that are not in the future, which can be useful for reporting, analysis, or filtering data based on time.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table captures and categorizes various metrics related to hours worked and their corresponding percentage contributions, enabling businesses to analyze workforce productivity and resource allocation effectively. This table supports decision-making by providing insights into labor efficiency and performance trends across different departments or projects.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column captures the allocation of hours and their corresponding percentage contributions to specific tasks, facilitating effective scheduling and resource management within the enrichment tasks framework. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various time and percentage metrics related to scheduled enrichment tasks, facilitating the analysis of resource allocation and task performance. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column captures the sequence of hours and their corresponding percentage allocations for scheduling enrichment tasks, facilitating efficient resource management and prioritization. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes and organizes various codes used across the business, facilitating data consistency and integrity. It includes attributes for qualification criteria and sorting preferences, enabling efficient data retrieval and analysis in reporting and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column in the 'lkp_Code' table stores unique string identifiers that categorize and define various entities for efficient data retrieval and enrichment tasks. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of codes, serving as a numerical identifier for categorizing and filtering data entries based on their compliance or eligibility criteria. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table specifies the order in which codes should be displayed or processed, facilitating organized data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference dataset for employee information, providing essential details such as EmployeeId and EmployeeName to facilitate filtering and reporting across various business analytics and performance metrics in Power BI. This table enhances data integrity and enables efficient data retrieval for employee-related insights.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column (string) in the 'lkp_fltr_Employee' table uniquely identifies each employee, facilitating efficient data retrieval and management within employee-related processes. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, facilitating easy identification and reference within the employee lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, linking project identifiers with their respective qualification and billing metrics, as well as categorizing them into groups. This table is essential for analyzing project performance and financial outcomes, enabling informed decision-making and strategic planning within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing identifier associated with each project, facilitating financial tracking and management. |
| `Group` | `string` | The 'Group' column in the 'lkp_Project' table categorizes projects into distinct classifications for streamlined management and reporting. |
| `Project` | `string` | The 'Project' column in the 'lkp_Project' table identifies the name of each project, serving as a key reference for categorizing and managing related scheduling tasks. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, with numeric values representing different levels of qualification criteria met. |

---

#### <a name="table-lkp_salesperson"></a>Table: `lkp_SalesPerson`

The 'lkp_SalesPerson' table serves as a reference for sales personnel associated with client transactions, linking each salesperson to specific clients and their corresponding sales activities over time. This table enables businesses to analyze sales performance, track client relationships, and optimize sales strategies based on historical data.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each salesperson, facilitating the identification and management of client relationships within the sales team. |
| `SalesPerson` | `string` | The 'SalesPerson' column contains the names of individuals responsible for generating sales, serving as a key reference for identifying sales performance and accountability within the organization. |
| `YearMonth` | `dateTime` | Column Description: The 'YearMonth' column captures the specific year and month associated with each sales person's performance data, facilitating time-based analysis and reporting within the lkp_SalesPerson table. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their characteristics such as billable status and external affiliations. This table is essential for analyzing resource allocation and financial performance across different units, enabling informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | Column Description: The 'BillableDep' column (int64) in the 'lkp_Unit' table indicates the department responsible for billable activities, facilitating accurate tracking and reporting of revenue-generating units. |
| `Organisatorische eenheid` | `string` | Column Description: This column represents the organizational unit, providing a textual identifier for categorizing and managing various departments or divisions within the organization. |
| `OWN-Sub-ExtT` | `string` | Column Description: The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores external type identifiers for owned sub-units, facilitating the classification and management of unit relationships within the scheduling framework. |
| `Unit` | `string` | The 'Unit' column in the 'lkp_Unit' table stores the names of measurement units used for categorizing and quantifying various items in the scheduling process. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, statuses, and leadership assignments. This table enables effective project management and oversight by providing insights into project categorization, progress tracking, and resource allocation across various project groups within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key reference for project timeline analysis and performance evaluation. |
| `Administration` | `string` | The 'Administration' column in the 'tbl_Project' table captures the administrative details or oversight responsibilities associated with each project, facilitating effective management and coordination. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates and changes within the 'tbl_Project' table. |
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each project, facilitating easy identification and management of client relationships within the project database. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating efficient tracking and management of client-related activities within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | Column Description: The 'Created_on' column records the date and time when the project was initiated, providing a timestamp for tracking project creation within the tbl_Project table. |
| `Currency` | `string` | The 'Currency' column in the 'tbl_Project' table specifies the type of currency used for financial transactions related to each project, ensuring clarity in budgeting and cost management. |
| `DateCreated` | `dateTime` | Column Description: The 'DateCreated' column records the exact date and time when each project entry was created, facilitating tracking and auditing of project timelines. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or business unit associated with each project in the 'tbl_Project' table. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column in the 'tbl_Project' table indicates the scheduled date and time by which a project is expected to be completed, facilitating effective project timeline management. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the tbl_Project table, facilitating the organization and tracking of related enrichment tasks. |
| `ProjectDescription` | `string` | Column Description: A detailed narrative outlining the objectives, scope, and key activities of the project, facilitating clear understanding and communication among stakeholders. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the specific category or classification of projects within the 'tbl_Project' table, facilitating organized management and reporting of related project activities. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a detailed textual description of the project group, facilitating better understanding and categorization of related projects within the 'tbl_Project' table. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for project-related data and operations. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the associated project in the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency amount of the sales price for each project, stored as a decimal to ensure precision in financial calculations. |
| `Startdate` | `dateTime` | The 'Startdate' column in the 'tbl_Project' table captures the scheduled initiation date and time for each project, facilitating effective timeline management and resource allocation. |
| `Status` | `string` | The 'Status' column in the 'tbl_Project' table indicates the current progress or state of each project, helping stakeholders quickly assess project health and manage resources effectively. |
| `Statuscode` | `string` | The 'Statuscode' column in the 'tbl_Project' table indicates the current operational status of each project, facilitating effective tracking and management of project progress. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, expressed as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table serves as a critical resource for identifying employees who have not submitted their timesheets within a specified timeframe, enabling managers to address compliance issues and ensure accurate payroll processing. By tracking relevant details such as employee and manager information, contract dates, and hours, this table facilitates timely interventions to enhance workforce accountability and operational efficiency.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by an employee that have not yet been recorded in their timesheet. |
| `Approved` | `boolean` | Indicates whether the timesheet has been approved (true) or not (false) within the context of tracking missing timesheets. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to conclude, aiding in the identification of missing timesheet entries related to contract expiration. |
| `ContractHours` | `int64` | The 'ContractHours' column (int64) in the 'vw_missing_timesheet' table represents the total number of hours contracted for an employee, facilitating the identification of discrepancies in reported timesheets. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, serving as a key reference point for tracking timesheet submissions and ensuring compliance with contractual obligations. |
| `ContractStatusToday` | `string` | Column Description: Indicates the current status of the contract associated with the timesheet, providing insights into whether it is active, expired, or under review. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_missing_timesheet' table uniquely identifies each employee associated with missing timesheet entries, facilitating tracking and resolution of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on outstanding time reporting issues. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours recorded for tasks that have not been submitted in timesheets, facilitating the identification of unaccounted work hours. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data processing activities. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_missing_timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and tracking of missing submissions. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees who have missing timesheets, facilitating accountability and follow-up actions. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, enabling effective tracking and management of employee time discrepancies. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet submissions related to various projects. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores the unique identifier for each project associated with missing timesheet entries, facilitating the tracking and management of project-related time discrepancies. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing a rejection and false indicating acceptance or no rejection. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for processing, with a value of true signifying readiness and false indicating that it is not yet prepared. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double data type to accommodate precise financial calculations within the 'vw_missing_timesheet' table. |
| `SalesPrice` | `double` | The 'SalesPrice' column (double) in the 'vw_missing_timesheet' table represents the monetary value assigned to each sale, facilitating financial analysis and reporting for timesheet-related transactions. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date and time associated with missing timesheet entries, facilitating timely follow-up and resolution of scheduling discrepancies. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific organizational subunit associated with each entry in the 'vw_missing_timesheet' table, facilitating targeted analysis of timesheet discrepancies by department. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for timesheets, facilitating the tracking and management of missing timesheet entries within the 'vw_missing_timesheet' table. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a textual summary of the tasks or activities recorded in the timesheet, facilitating the identification and analysis of missing entries in the 'vw_missing_timesheet' table. |
| `Week_` | `int64` | The 'Week_' column (int64) in the 'vw_missing_timesheet' table represents the week number of the year associated with each timesheet entry, facilitating the identification and analysis of missing timesheet submissions on a weekly basis. |
| `Year_` | `int64` | Column Description: The 'Year_' column (int64) represents the calendar year associated with each entry in the 'vw_missing_timesheet' table, facilitating the tracking and analysis of timesheet data over time. |

##### Calculated Columns

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column identifies the department associated with billable hours for employees missing timesheet entries, facilitating accurate tracking and reporting of labor costs.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression starts by checking if there is a related value in the `lkp_Unit` table for the column `BillableDep`. This is done using the `RELATED` function, which pulls in data from a related table based on relationships defined in the data model.

2. **Handling Blank Values**: The first part of the `IF` statement checks if the related `BillableDep` value is blank (i.e., there is no corresponding entry in the `lkp_Unit` table). If it is blank, the expression returns a value of `1`. This indicates that if there is no related data, the result is considered "billable" by default.

3. **Checking for Non-Zero Values**: If the related `BillableDep` value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns `1`, indicating that the item is billable.

4. **Final Outcome**: If the related `BillableDep` value is blank or non-zero, the calculated column will return `1`, meaning the item is considered billable. If the related value is zero, it returns `0`, indicating that the item is not billable.

### Summary:
In summary, this DAX expression is used to determine whether an item is billable based on related data. It returns `1` (billable) if there is no related data or if the related `BillableDep` value is non-zero. It returns `0` (not billable) only if the related value is zero. This helps in categorizing items effectively for billing purposes.

**`CC_ActiveEmployees`** (`string`)

- **Description:** Column Description: This column indicates the status of active employees in relation to their timesheet submissions, represented as a string value within the context of identifying missing timesheet records.
- **DAX Expression:**
```dax
IF(vw_missing_timesheet[ShortDate] >= vw_missing_timesheet[ContractStartDate] && 
			      (vw_missing_timesheet[ShortDate] <= vw_missing_timesheet[ContractEndDate] || ISBLANK(vw_missing_timesheet[ContractEndDate]))
			        , 1
			        , 0
			    )
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `CC_ActiveEmployees` in a data model, specifically within a table called `vw_missing_timesheet`. Here's a breakdown of what this code does in simple business terms:

1. **Purpose**: The calculated column is designed to determine whether an employee is considered "active" based on their contract dates.

2. **Conditions Checked**:
   - The code first checks if the date in the column `ShortDate` (which likely represents a specific date, such as when a timesheet is submitted) is on or after the employee's `ContractStartDate`. This means the employee's contract must have started before or on this date for them to be considered active.
   - Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. This means the employee's contract must still be valid on this date. However, if the `ContractEndDate` is blank (which could indicate that the employee is still employed indefinitely), the employee is also considered active.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the contract has no end date), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns a value of `0`, indicating that the employee is not active.

**Summary**: This DAX expression effectively flags employees as "active" (1) or "not active" (0) based on whether the date in `ShortDate` falls within their contract period, allowing for better tracking of employee status in relation to their contracts.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to facilitate the identification and management of missing or unsubmitted timesheet data.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `IncompleteFlag` in a data model, specifically in a table named `vw_missing_timesheet`. 

Here's a breakdown of what this expression does in simple business terms:

- **Purpose**: The `IncompleteFlag` column is designed to indicate whether there are missing hours recorded for a particular entry in the `vw_missing_timesheet` table.

- **Logic**: The expression checks the value in the `MissingHours` column for each row:
  - If the value in `MissingHours` is **less than 0** (which implies that there is a discrepancy or an issue with the recorded hours), the expression returns **1**. This means that the entry is flagged as "incomplete" or problematic.
  - If the value in `MissingHours` is **0 or greater**, the expression returns **0**, indicating that there are no issues with the recorded hours.

- **Outcome**: The result is a new column (`IncompleteFlag`) that will have a value of **1** for entries that have missing or negative hours, and **0** for those that do not. This flag can be useful for quickly identifying which entries need attention or further investigation regarding their time reporting.

In summary, this DAX expression helps to easily identify and flag entries with missing or negative hours, allowing for better tracking and management of timesheet data.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `MAIN_UNIT`. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression starts by checking if there is a related value in the `lkp_Unit` table for the current row. Specifically, it looks for the `Unit` field in that related table.

2. **Handle Missing Data**: 
   - If the related `Unit` value is **blank** (meaning there is no corresponding entry in the `lkp_Unit` table), the expression will return the text **"Unknown"**. This is a way to handle situations where the expected data is missing, ensuring that the output is clear and informative rather than leaving it empty.
   - If there is a valid related `Unit` value, it will return that value instead.

3. **Purpose**: The overall purpose of this calculated column is to provide a clear and user-friendly representation of the unit associated with each row in the main table. If the unit is not found, it explicitly indicates that the unit is unknown, which can help in data analysis and reporting by highlighting gaps in the data.

In summary, this DAX expression ensures that every row in the `MAIN_UNIT` column either shows the related unit name or indicates "Unknown" if no unit is available, making the data more understandable and actionable.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column (string) in the 'vw_missing_timesheet' table represents the specific year and week number for tracking and identifying timesheet entries that are missing or incomplete.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'YearWeek' in a data model, typically in tools like Power BI or Excel. Here's a breakdown of what it does in simple business terms:

1. **Extracting the Year**: The function `YEAR([ContractEndDate])` takes the date from the column `[ContractEndDate]` and extracts the year from it. For example, if the contract ends on March 15, 2023, this part will return `2023`.

2. **Calculating the Week Number**: The function `WEEKNUM([ContractEndDate])` calculates the week number of the year for the same date. Continuing with our example, if March 15, 2023, falls in the 11th week of the year, this function will return `11`.

3. **Formatting the Week Number**: The `FORMAT(..., "00")` part ensures that the week number is always displayed as a two-digit number. So, if the week number is `5`, it will be formatted as `05`.

4. **Combining Year and Week**: The `& "-" &` part combines the year and the formatted week number into a single string, separated by a hyphen. So, for our example of March 15, 2023, the final result would be `2023-11`.

### Summary:
The overall purpose of this DAX expression is to create a unique identifier for each contract's end date that combines the year and the week number in a format like "YYYY-WW". This makes it easy to analyze or group data by year and week, which can be useful for reporting or tracking trends over time.

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

1. **Purpose**: This measure counts the number of unique employees (consultants) who have recorded negative hours in a timesheet. Negative hours might indicate errors, adjustments, or other specific situations where hours worked are reported as less than zero.

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data being counted.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the number of unique employee names in the `vw_missing_timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries with negative hours.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This filter condition specifies that only the records where the `MissingHours` are less than zero should be considered in the count. 

3. **Outcome**: The measure ultimately provides a count of how many different consultants have negative hours recorded in the timesheet. This information can be useful for identifying potential issues with timesheet entries, such as errors that need to be corrected or patterns that may require further investigation.

In summary, `Count_Negative_Consultant` helps businesses track and manage timesheet accuracy by identifying how many unique consultants have reported negative hours, which could indicate problems that need addressing.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is a measure named `CountNegativeMissingHours`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure is designed to count the number of unique employees who have recorded negative missing hours in a timesheet.

2. **Components**:
   - **`CALCULATE`**: This function is used to change the context in which data is evaluated. In this case, it is modifying the context to focus on a specific condition.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID])`**: This part counts the number of unique Employee IDs from the `vw_missing_timesheet` table. Essentially, it tells us how many different employees are involved.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This condition filters the data to only include records where the `MissingHours` value is less than zero. In other words, it looks for instances where employees have negative missing hours, which might indicate an error or a situation where hours were incorrectly recorded.

3. **Outcome**: The overall result of this measure is the total number of distinct employees who have negative values for missing hours in their timesheets. This could be useful for identifying potential issues in time reporting or for auditing purposes.

In summary, `CountNegativeMissingHours` helps a business track how many employees have reported negative missing hours, which can signal problems in timekeeping or data entry that need to be addressed.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the number of active employees who are missing timesheet hours for a specific period. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The code works with a data table called `vw_missing_timesheet`, which likely contains records of employees, their hours worked, and their contractual hours.

2. **Summarization**: The code first creates a summarized view of the data using the `SUMMARIZE` function. It groups the data by:
   - Employee Name
   - Year
   - Week
   - Unit (from another table called `lkp_Unit`)
   - SubUnit

   For each group, it calculates two new values:
   - **MissingHours**: This is calculated by taking the total hours worked (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contractual hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has not logged enough hours compared to what they are contracted for.
   - **MinActiveEmp**: This captures the minimum value of active employees (`MIN(vw_missing_timesheet[CC_ActiveEmployees])`) for that group, which helps identify if there is at least one active employee in that grouping.

3. **Filtering**: After summarizing, the code filters this summarized data to find only those groups where:
   - There is at least one active employee (`[MinActiveEmp] = 1`).
   - The calculated missing hours are negative (`[MissingHours] < 0`), meaning the employee has not logged enough hours.

4. **Counting**: Finally, the code counts the number of rows that meet these criteria using `COUNTROWS(ActiveEmployees)`. This gives the total number of active employees who are missing timesheet hours for the specified year and week.

### Summary:
In essence, this DAX measure calculates how many active employees have not logged enough hours in their timesheets for a given week and year, helping the business identify potential issues with timesheet compliance.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is designed to calculate a measure called `Dax_MissingHours`, which focuses on identifying and summing up the missing hours for active employees who have not met their contracted hours in a given time frame. Here’s a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. This variable filters a summarized table (`vw_missing_timesheet`) to focus on employees who are currently active (indicated by `CC_ActiveEmployees` being equal to 1).

2. **Summarize Data**: Within this summarized table, the code groups data by several key attributes:
   - Employee Name
   - Year
   - Week
   - Unit (from a lookup table)
   - SubUnit

   For each group, it calculates two important values:
   - **MissingHours**: This is calculated by taking the total hours worked (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contracted hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has not worked enough hours compared to what was expected.
   - **MinActiveEmp**: This simply captures the minimum value of active employees in that group, which helps in filtering out only those groups where there is at least one active employee.

3. **Filter for Relevant Data**: The `FILTER` function then narrows down the summarized data to only include groups where:
   - There is at least one active employee (`MinActiveEmp = 1`)
   - The calculated `MissingHours` is less than zero (indicating that the employee has not met their required hours).

4. **Calculate Total Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered `ActiveEmployees` table and sums up the `MissingHours` for those employees who meet the criteria.

### Summary:
In essence, this DAX measure calculates the total number of hours that active employees are missing compared to their contracted hours, specifically focusing on those who have not fulfilled their expected work hours. This can help management identify gaps in employee attendance or performance, allowing for better resource planning and management.

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

1. **Data Source**: The calculation is based on a data table called `vw_missing_timesheet`, which likely contains records of employee timesheets, including information about the week, employee IDs, and their respective contract hours.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key pieces of information:
   - **Week**: This represents the specific week for which we are calculating contract hours.
   - **EmployeeID**: This identifies each employee.

3. **Calculating Maximum Contract Hours**: For each unique combination of week and employee, the code calculates the maximum contract hours using `MAX(vw_missing_timesheet[ContractHours])`. This means that if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

4. **Summing Up Contract Hours**: The outer `SUMX` function then takes the summarized data (which now contains the maximum contract hours for each employee per week) and sums these values together. Essentially, it adds up all the maximum contract hours for all employees across all weeks.

### What It Achieves:
The overall result of this DAX measure is the total expected contract hours for all employees, aggregated by week. This can be useful for understanding how many hours are expected to be worked by employees in a given week, helping management to plan resources, monitor workloads, and ensure that staffing levels meet contractual obligations.

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
- The result of this calculation gives you the number of hours that are missing or unaccounted for. In other words, it shows how many hours an employee is expected to work (based on their contract) but has not reported or logged as worked.

### Example:
- If an employee is contracted to work 40 hours (ContractHours) but has only logged 30 hours (Hours_), the calculation would yield:
  - 40 (ContractHours) - 30 (Hours_) = 10
- This means there are 10 hours that the employee has not accounted for in their timesheet.

In summary, this DAX measure helps identify discrepancies between contracted hours and actual hours worked, which can be crucial for payroll, performance tracking, and ensuring compliance with work agreements.

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

1. **SUM(vw_missing_timesheet[MissingHours])**: This part of the code adds up all the missing hours from a table called `vw_missing_timesheet`. Essentially, it gives you the total number of hours that were not recorded or submitted as timesheets.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the expected number of hours that should have been worked in a week according to the contract. It serves as a benchmark for comparison.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the code calculates the difference between the total missing hours and the expected contract hours. If the total missing hours exceed the expected hours, this result will be negative, indicating a shortfall in hours worked.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The `DIVIDE` function takes the result from the previous step (the difference between missing hours and expected hours) and divides it by the expected contract hours. This gives you a ratio that represents how much of the expected hours were not completed. The third argument, `0`, ensures that if the expected hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

### What It Achieves:
The overall measure calculates the percentage of expected working hours that were not completed due to missing timesheets. A positive result indicates a shortfall in hours worked compared to what was expected, while a negative result would suggest that the hours worked exceeded expectations. This measure helps businesses understand how well their employees are meeting their expected working hours and can highlight areas where timesheet compliance may need improvement.

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

1. **Data Source**: The measure uses a data table called `vw_missing_timesheet`. This table likely contains records of employees and their timesheet submissions, including information about whether their submissions are complete or incomplete.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by three key columns:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This is the name of the employee.
   - `MAIN_UNIT`: This likely refers to the department or unit within the organization that the employee belongs to.

   While grouping, the code also creates a new column called `IncompleteFlag`, which captures the maximum value of the `IncompleteFlag` for each combination of week, employee, and unit. The `IncompleteFlag` indicates whether the employee has submitted their timesheet completely or not (e.g., 0 for complete, 1 for incomplete).

3. **Calculating the Measure**: After summarizing the data, the `SUMX` function iterates over each group created by `SUMMARIZE`. For each group, it sums up the values of the `IncompleteFlag`. 

4. **Final Outcome**: The end result of this measure is the total count of incomplete timesheet submissions across all employees, grouped by their respective units and weeks. This helps the organization understand how many employees in each unit have not submitted their timesheets completely for each week.

In summary, the `UniqueEmployeesPerUnit` measure calculates the total number of incomplete timesheet submissions by employees, organized by week and unit. This information is valuable for tracking compliance with timesheet submissions and identifying areas where follow-up may be needed.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This table enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved for processing, with a value of true representing approval and false indicating pending or rejected status. |
| `Client` | `string` | The 'Client' column in the 'vw_Timesheet' table identifies the specific client associated with each timesheet entry, facilitating accurate billing and project tracking. |
| `Code2` | `string` | The 'Code2' column in the 'vw_Timesheet' table represents a secondary identifier or classification code used to categorize or differentiate time entries for enhanced reporting and analysis. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column in the 'vw_Timesheet' table represents the date and time when an employee's contract is set to expire, facilitating effective resource planning and management. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when a contract becomes effective, serving as a reference point for tracking associated timesheet entries within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | Column Description: Indicates the current status of the contract associated with the timesheet entry, represented as a string value. |
| `ContractStatusTodayPBI` | `string` | Column Description: This column indicates the current status of contracts as of today, providing real-time insights for timesheet management and reporting. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential timing information for tracking contract changes within the timesheet management process. |
| `CostAmount` | `double` | The 'CostAmount' column represents the total monetary cost associated with the recorded time entries in the 'vw_Timesheet' table, expressed as a double-precision floating-point number for precise financial calculations. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor or resources allocated to tasks recorded in the timesheet, expressed as a double for precision in financial calculations. |
| `Currency` | `string` | The 'Currency' column (string) in the 'vw_Timesheet' table specifies the type of currency used for financial transactions recorded in the timesheet entries. |
| `DebtorName` | `string` | Column Description: The 'DebtorName' column stores the name of the debtor associated with each timesheet entry, facilitating the identification of clients for billing and account management purposes. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_Timesheet' table uniquely identifies each employee, facilitating the tracking and management of their recorded work hours and related timesheet data. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column in the 'vw_Timesheet' table stores the unique identifier and name of each employee, facilitating the tracking and management of timesheet entries associated with specific personnel. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating easy identification and reporting of labor hours. |
| `Employer` | `string` | The 'Employer' column in the 'vw_Timesheet' table identifies the organization or company for which the employee is recording their work hours. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating accurate payroll and reporting processes. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column in the 'vw_Timesheet' table captures additional contextual information or notes related to time entries, enhancing the clarity and comprehensiveness of timesheet records. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with each timesheet entry, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked on various tasks, enabling accurate tracking and reporting of employee time allocation. |
| `HoursperWeek` | `int64` | Column Description: The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating effective time management and resource allocation. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee work patterns. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, enabling efficient tracking and reporting of different work hour classifications. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when timesheet data was ingested into the system, facilitating accurate tracking and auditing of data processing activities. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects within the vw_Timesheet table. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating efficient tracking and management of timesheet entries associated with specific projects. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries within the vw_Timesheet table. |
| `IPMIDName` | `string` | The 'IPMIDName' column in the 'vw_Timesheet' table stores the names of individuals associated with specific IPM IDs, facilitating the tracking and management of timesheet entries linked to those identifiers. |
| `JobTitle` | `string` | The 'JobTitle' column in the 'vw_Timesheet' table captures the specific role or position of an employee, providing context for the hours worked and tasks performed during the reporting period. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking timesheet records to related datasets, facilitating efficient data integration and analysis. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_Timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column in the 'vw_Timesheet' table stores the names of managers associated with each timesheet entry, facilitating easy identification of managerial oversight for time tracking and reporting purposes. |
| `ManagerKey` | `int64` | Column Description: The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key of the manager responsible for overseeing the associated timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct supervisor, facilitating the identification of managerial oversight for timesheet entries in the 'vw_Timesheet' table. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of scheduled work bookings for operational hours, facilitating the tracking and management of time allocation within the timesheet framework. |
| `Project` | `string` | The 'Project' column in the 'vw_Timesheet' table identifies the specific project associated with each recorded time entry, facilitating accurate tracking and reporting of time spent on various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column in the 'vw_Timesheet' table uniquely identifies the specific project associated with each timesheet entry, facilitating accurate tracking and reporting of time spent on various projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column in the 'vw_Timesheet' table captures the specific project-related details associated with each timesheet entry, facilitating effective tracking and management of project resources and activities. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the project profile associated with each timesheet entry, facilitating accurate tracking and reporting of project-related labor hours. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, enabling better analysis and reporting of resource allocation across different project types. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet is ready for reporting, with a value of true signifying readiness and false indicating that further action is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double-precision floating-point number, within the context of timesheet data for accurate financial analysis and reporting. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet context. |
| `Status` | `string` | The 'Status' column in the 'vw_Timesheet' table indicates the current state of each timesheet entry, reflecting whether it is pending, approved, or rejected. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority assigned to each timesheet entry, facilitating the management and allocation of resources based on urgency and importance. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for each timesheet entry, facilitating the tracking and management of employee work hours within the vw_Timesheet table. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking and reporting of employee work hours. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a detailed narrative of the tasks and activities recorded in the timesheet, providing context and clarity for time entries. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table represents the measurement unit associated with the recorded time entries, facilitating accurate tracking and reporting of work hours across various tasks. |
| `UnitCode` | `string` | The 'UnitCode' column in the 'vw_Timesheet' table represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating accurate tracking and reporting of labor costs by department. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and coordination related to timesheet submissions and approvals. |
| `YearWeek` | `string` | The 'YearWeek' column (string) in the 'vw_Timesheet' table represents the specific year and week number, facilitating the organization and analysis of timesheet data by weekly intervals. |

##### Calculated Columns

**`Approved_With_V_Z`** (`string`)

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheets, specifically identifying those that have been approved with a versioning or validation process denoted by 'V_Z'.
- **DAX Expression:**
```dax
IF(
			    [Approved] = TRUE() || 
			    vw_Timesheet[TimesheetCode] IN {"V", "Z"}, 
			    1, 
			    0
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `Approved_With_V_Z`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column determines whether a timesheet entry is considered "approved" based on specific criteria.

2. **Conditions**: The code checks two main conditions:
   - **First Condition**: It checks if the `Approved` column is marked as `TRUE`. This means that if the timesheet has been officially approved, it meets the criteria.
   - **Second Condition**: It checks if the `TimesheetCode` for that entry is either "V" or "Z". These codes likely represent specific types of timesheets that are automatically considered approved regardless of the `Approved` status.

3. **Output**: 
   - If either of the conditions is met (the timesheet is approved or the code is "V" or "Z"), the calculated column will return a value of `1`. This indicates that the timesheet is approved.
   - If neither condition is met, it returns a value of `0`, indicating that the timesheet is not approved.

In summary, this DAX expression effectively flags timesheet entries as approved (1) or not approved (0) based on whether they are officially approved or have specific codes ("V" or "Z"). This can help in reporting and analysis by easily identifying which timesheets are considered approved.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Let's break it down step by step in simple business terms:

1. **Purpose**: The calculated column `BillableDep` is designed to determine whether a certain condition related to billing is met for each row in the data.

2. **Checking for Related Data**: The function `RELATED(lkp_Unit[BillableDep])` is used to look up a value from a related table called `lkp_Unit`. Specifically, it checks the `BillableDep` column in that table.

3. **First Condition - Is Blank**: The first part of the `IF` statement checks if the value retrieved from `lkp_Unit[BillableDep]` is blank (i.e., there is no related value). If it is blank, the formula returns `1`. This means that if there is no related billing information, the result is considered "billable" by default.

4. **Second Condition - Not Zero**: If the value is not blank, the formula then checks if the value is not equal to zero. If the value is not zero, it again returns `1`, indicating that there is a valid billing amount, and thus it is billable.

5. **Final Outcome - Zero Case**: If the value is neither blank nor non-zero (meaning it is zero), the formula returns `0`, indicating that the item is not billable.

### Summary:
In summary, this DAX expression calculates a value for the `BillableDep` column that indicates whether an item is billable based on the related `BillableDep` value from another table. It returns `1` if there is no related value or if the related value is greater than zero, and it returns `0` if the related value is exactly zero. This helps in identifying which items can be billed based on their associated billing data.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillablePrj` in a data model, specifically in a table named `vw_Timesheet`. Here's a breakdown of what this code does in simple business terms:

1. **Purpose**: The `BillablePrj` column is designed to determine whether a particular entry in the timesheet is considered "billable" or not. A billable entry is one that can be charged to a client or project.

2. **Conditions for Billability**: The code checks three specific conditions to decide if an entry is billable:
   - **Condition 1**: If the `SalesPrice` (the amount charged for the service) is greater than 0. This means that if there is a price associated with the entry, it is billable.
   - **Condition 2**: If the `ProjectProfileCode` is equal to 81. This suggests that there is a specific project type or category (identified by the code 81) that is always considered billable.
   - **Condition 3**: If the `Project` name contains the phrase "Customer Success Services". This indicates that any project related to customer success services is also considered billable.

3. **Output**: 
   - If any of the above conditions are met (i.e., if at least one of them is true), the calculated column will return a value of `1`, indicating that the entry is billable.
   - If none of the conditions are met, it will return a value of `0`, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project name. This helps in identifying which services can be charged to clients, facilitating better billing and revenue tracking.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column in the 'vw_Timesheet' table captures the name of the employer associated with each timesheet entry, facilitating accurate tracking and reporting of employee work hours by employer.

**`cc_SalesPerson`** (`string`)

- **Description:** Column Description: The 'cc_SalesPerson' column identifies the sales representative associated with each timesheet entry, facilitating tracking and accountability for sales-related activities.

**`Employee_WeeklyHours`** (`string`)

- **Description:** The 'Employee_WeeklyHours' column captures the total number of hours worked by each employee during the week, represented as a string for flexible formatting and reporting within the timesheet data.
- **DAX Expression:**
```dax
CONCATENATE(
			    ('vw_Timesheet'[EmployeeName]),
			    " - " & ('vw_Timesheet'[HoursperWeek])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `Employee_WeeklyHours` in a data model, specifically from a table named `vw_Timesheet`. 

Here's a breakdown of what it does in simple business terms:

1. **Combining Information**: The code takes two pieces of information from the `vw_Timesheet` table:
   - The name of the employee (`EmployeeName`).
   - The number of hours that employee works per week (`HoursperWeek`).

2. **Formatting the Output**: It combines these two pieces of information into a single string. The format of the output will be:
   - The employee's name followed by a hyphen and then the number of hours they work per week. 
   - For example, if the employee's name is "John Doe" and he works 40 hours per week, the resulting string would be "John Doe - 40".

3. **Purpose**: This calculated column is useful for creating a clear and concise representation of each employee's weekly working hours alongside their name. It can be helpful for reporting, analysis, or visualization purposes, making it easier to understand how many hours each employee is scheduled to work at a glance.

In summary, this DAX expression creates a new column that neatly combines employee names with their corresponding weekly hours, enhancing the readability and usability of the data.

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

1. **Lookup for Group**: The code first checks if there is a corresponding 'Group' value for a specific project in the 'lkp_Project' table. It does this by looking up the 'Group' based on the 'PROJECT' field in the 'lkp_Project' table and the 'Project' field in the 'vw_Timesheet' table.

2. **Check for Blank Values**: The `NOT(ISBLANK(...))` part checks if the lookup returned a valid 'Group' value. If it did (meaning the project exists in the 'lkp_Project' table), it will use that 'Group' value for the 'GroupCat' column.

3. **Handling Missing Groups**: If the lookup does not return a valid 'Group' (meaning the project is not found in the 'lkp_Project' table), the code then checks if the project is billable. This is done by checking if the 'BillablePrj' field in the 'vw_Timesheet' table equals 1.

4. **Assigning Categories**: 
   - If the project is billable (i.e., 'BillablePrj' equals 1), the 'GroupCat' will be set to "Billable".
   - If the project is not billable (i.e., 'BillablePrj' does not equal 1), the 'GroupCat' will be set to "Other Unbillable".

### Summary:
In summary, this DAX expression categorizes each project in the 'vw_Timesheet' table into one of three categories:
- It assigns the project’s group from the 'lkp_Project' table if available.
- If the project is not found in the 'lkp_Project' table, it checks if the project is billable and assigns "Billable" or "Other Unbillable" accordingly. 

This helps in organizing and analyzing projects based on their billing status and group affiliation, which is useful for financial reporting and project management.

**`IsContractActive`** (`string`)

- **Description:** Indicates whether the associated contract is currently active, represented as a string value in the vw_Timesheet table.
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
   - **ISBLANK('vw_Timesheet'[ContractEndDate])**: This part checks if the **ContractEndDate** field is empty (i.e., there is no end date specified for the contract). If there is no end date, it implies that the contract is ongoing or has not been finalized.
   - **'vw_Timesheet'[ContractEndDate] > TODAY()**: This part checks if the **ContractEndDate** is later than today’s date. If the end date is in the future, it means the contract is still active.

3. **Output**:
   - If either of the above conditions is true (meaning the contract has no end date or the end date is still in the future), the calculated column will return **"Active"**.
   - If neither condition is true (meaning there is an end date that has already passed), it will return **"Not Active"**.

### Summary:
In summary, this DAX expression helps businesses quickly identify which contracts are currently active by labeling them as "Active" or "Not Active" based on their end dates. This can be useful for tracking contract statuses, managing resources, or making informed decisions about ongoing agreements.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column in the 'vw_Timesheet' table identifies the primary organizational unit responsible for the time entries recorded, facilitating effective resource allocation and project management.
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

2. **Handle Missing Data**: If the related `Unit` value is blank (meaning there is no corresponding entry in the `lkp_Unit` table), the code will return the text "Unknown". This is a way to handle situations where the expected data is missing, ensuring that the result is clear and informative.

3. **Return the Unit Value**: If there is a valid related `Unit` value (i.e., it is not blank), the code will return that value. This means that the calculated column will show the actual unit from the `lkp_Unit` table.

### Summary:
In summary, this DAX expression is designed to populate the 'MAIN_UNIT' column with either the corresponding unit name from the `lkp_Unit` table or "Unknown" if no unit is found. This helps maintain data integrity and clarity in reporting by ensuring that users can easily identify when a unit is missing.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) to facilitate time-based reporting and analysis of timesheet data.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to create a calculated column called 'MonthNumber' in a data model, typically within a tool like Power BI or Excel.

### What It Does:
- **Extracts the Month**: This expression takes a date value from the column `[TimesheetDate]` and extracts the month part of that date.
- **Returns a Number**: The result is a number that represents the month, ranging from 1 to 12. For example, if the date in `[TimesheetDate]` is January 15, 2023, the expression will return `1`. If the date is July 10, 2023, it will return `7`.

### Business Purpose:
- **Simplifies Analysis**: By converting dates into month numbers, it makes it easier to analyze data by month. For instance, you can quickly group or filter data to see trends or totals for each month.
- **Facilitates Reporting**: This calculated column can be used in reports to show monthly performance, track progress over time, or compare results across different months.

In summary, this DAX expression helps businesses organize and analyze their data by month, making it more accessible and actionable for decision-making.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' in the 'vw_Timesheet' table represents the unique identifier for employees, facilitating the tracking and management of individual timesheet entries.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

### What it Achieves:

1. **Counts Unique Employees**: This expression counts how many different employees are present in the `vw_Timesheet` table. For example, if the table has entries for "John Doe," "Jane Smith," and "John Doe" again, the result would be 2 because "John Doe" is counted only once.

2. **Useful for Reporting**: This calculation is particularly useful in reports or dashboards where you want to know how many distinct employees have submitted timesheets, worked on projects, or been involved in any activity tracked in the timesheet.

3. **Data Analysis**: It helps in analyzing workforce participation, understanding employee engagement, and making decisions based on the number of unique contributors to a project or task.

In summary, this DAX expression provides a straightforward way to quantify the diversity of employees involved in the timesheet data, which can be critical for various business insights and operational decisions.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table captures the classification of time entries as either owned, subcontracted, or external, facilitating accurate tracking and reporting of resource allocation.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used in a calculated column to retrieve a specific value from a related table. Here’s a breakdown of what it does in simple business terms:

1. **Context of Use**: This expression is typically used in a data model where there are two tables that have a relationship defined between them. For example, you might have a main table (let's call it "Sales") and a lookup table (let's call it "lkp_Unit").

2. **Purpose of the Expression**: The expression is designed to pull in a value from the "lkp_Unit" table into the "Sales" table. Specifically, it retrieves the value from the column named "OWN-Sub-ExtT" in the "lkp_Unit" table.

3. **How It Works**: 
   - The `RELATED` function looks for a matching row in the "lkp_Unit" table based on the relationship defined in the data model.
   - It then fetches the value from the "OWN-Sub-ExtT" column for that matching row.

4. **Outcome**: By using this expression, each row in the "Sales" table will now have an additional column (the calculated column) that contains the corresponding "OWN-Sub-ExtT" value from the "lkp_Unit" table. This allows for enhanced analysis and reporting, as you can now see how each sale relates to the specific "OWN-Sub-ExtT" value.

In summary, this DAX expression enriches the main table with relevant information from a related table, facilitating better insights and decision-making based on that additional data.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column in the 'vw_Timesheet' table captures the project qualification status as a string, indicating whether the associated time entries meet specific project criteria for reporting and analysis.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'QualifyPrj'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column is designed to determine a qualification status for projects based on related data from another table (referred to as `lkp_Project`).

2. **Logic**:
   - The code checks if the 'Qualify' field from the related `lkp_Project` table is blank (i.e., it has no value).
   - If the 'Qualify' field is blank, the calculated column will return a value of **1**. This could indicate a default or a specific status (like "not qualified").
   - If the 'Qualify' field has a value (meaning it is not blank), the calculated column will return that value instead. This means it will reflect whatever qualification status is present in the `lkp_Project` table.

3. **Outcome**: The result is a column that either assigns a default value of 1 for projects without a qualification or pulls in the existing qualification value from the related table for those that do have one. This helps in ensuring that every project has a qualification status, either by default or by reference to existing data.

In summary, this DAX expression ensures that every project has a qualification status, either by assigning a default value or by using the value from a related table.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the specific organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor allocation within the 'vw_Timesheet' table.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of resources for enrichment tasks, represented as a string, within the context of timesheet management.
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

1. **Purpose**: The calculated column determines whether a certain condition is met for each row in the data. It outputs either a 1 (true) or a 0 (false).

2. **Conditions Checked**:
   - The first condition checks if the value in the column `[ReportReady]` is `TRUE`. This means that if the report is ready, the condition is satisfied.
   - The second condition checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means that if the timesheet code is one of these two specific codes, the condition is also satisfied.

3. **Output**:
   - If either of the conditions is true (i.e., the report is ready or the timesheet code is "V" or "Z"), the calculated column will return a value of **1**.
   - If neither condition is met, it will return a value of **0**.

### Summary:
In summary, this DAX expression is used to flag rows in the dataset where either the report is ready or the timesheet code is "V" or "Z". A value of 1 indicates that one of these conditions is met, while a value of 0 indicates that neither condition is met. This can be useful for filtering or analyzing data based on readiness or specific timesheet codes.

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
   - If the condition is true (meaning the contract status is "Active"), the measure returns the value of another measure called `[HoursDifference]`. This likely represents the difference in hours related to the contract, such as hours worked or hours remaining.
   - If the condition is false (meaning the contract status is not "Active"), the measure returns the text "Not Active".

### Summary:
In essence, `ContractStatus2` is used to determine the status of a contract. If the contract is currently active, it provides the relevant hours difference; if not, it simply indicates that the contract is not active. This measure helps users quickly understand the status of contracts and their associated hours in a straightforward manner.

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

1. **Check for Contract End Date**: The measure first looks at the maximum value of the `ContractEndDate` from the `vw_Timesheet` table. This means it is checking the latest contract end date available in the data.

2. **Determine if the End Date is Blank or in the Future**:
   - **ISBLANK**: It checks if this maximum end date is blank (meaning there is no end date recorded).
   - **Comparison with TODAY()**: It also checks if the maximum end date is greater than today’s date. This means it is looking to see if the contract is still valid and has not yet expired.

3. **Return Contract Status**:
   - If either of the above conditions is true (the end date is blank or it is a future date), the measure returns "Active". This indicates that the contract is currently active and valid.
   - If neither condition is true (meaning the end date is in the past), it returns "Not Active". This indicates that the contract has expired.

In summary, this measure effectively categorizes contracts as either "Active" or "Not Active" based on whether they have a valid end date that is either missing or still in the future. This helps businesses quickly assess the status of their contracts.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 15, 2023, the `TODAY()` function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. Week numbers typically start from 1 for the first week of the year.

So, when you combine these two functions, `WEEKNUM(TODAY())` calculates which week of the year it is right now. 

### In Business Terms:
This measure helps businesses understand which week they are currently in during the year. For example, if the result is 42, it indicates that we are in the 42nd week of the year. This can be useful for reporting, planning, and analyzing data on a weekly basis, such as sales performance, project timelines, or operational metrics.

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
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows us to apply specific filters to our calculations.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it tells us how many different employees are represented in the data.
   - **`vw_Timesheet[Approved] = FALSE()`**: This filter condition specifies that we only want to consider timesheets that have not been approved. In other words, it filters the data to include only those records where the `Approved` status is marked as false.

3. **Outcome**: The overall result of this measure is the total count of distinct employees who have submitted timesheets that are still pending approval. This can be useful for management to understand how many employees are waiting for their timesheets to be approved, which can help in tracking workflow and ensuring timely approvals.

In summary, `Dax_EmpCount_Approved` provides insights into the number of employees with unapproved timesheets, helping organizations manage their approval processes more effectively.

**`Dax_EmpCount_RReady`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[ReportReady] = FALSE()
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `Dax_EmpCount_RReady`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the number of unique employees whose timesheets are not marked as "Report Ready."

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. In this case, it is used to modify the filter applied to the data.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries in the timesheet.
   - **`vw_Timesheet[ReportReady] = FALSE()`**: This condition filters the data to include only those entries where the `ReportReady` field is set to FALSE. This means it focuses on timesheets that are not ready for reporting.

3. **Outcome**: The measure ultimately provides a count of how many distinct employees have timesheets that are still pending or not ready for reporting. This can be useful for management to understand how many employees still need to submit or finalize their timesheets before reports can be generated.

In summary, `Dax_EmpCount_RReady` helps track the number of employees with incomplete timesheets, aiding in ensuring timely reporting and accountability.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheet entries in the `vw_Timesheet` table.

### Breakdown of the Expression:

- **DISTINCTCOUNT**: This function counts the number of unique values in a specified column. In this case, it is counting unique entries.
  
- **vw_Timesheet[EmployeeName]**: This specifies the column from which the unique values are being counted. Here, it refers to the names of employees recorded in the `vw_Timesheet` table.

### What It Achieves:

In simple terms, this measure provides a count of how many different employees have logged their work hours. This is useful for understanding workforce participation, tracking employee engagement, or analyzing resource utilization over a specific period. For example, if you want to know how many distinct employees worked on a project or during a specific timeframe, this measure will give you that number.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the average hourly rate for sales based on data from a timesheet. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the `vw_Timesheet` table. Essentially, it totals the revenue generated from sales over a specific period or for a specific group of employees.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the same `vw_Timesheet` table. It represents the total number of hours that employees have worked during that same period.

3. **Division**: The entire expression divides the total sales amount by the total hours worked. This calculation gives you the average hourly rate, which tells you how much revenue is generated for each hour worked.

### In Summary:
The measure `MSR_AVG_HOURLY_RATE` calculates the average revenue earned per hour worked by employees. It helps businesses understand how effectively their workforce is generating sales relative to the time they spend working. This metric can be useful for evaluating productivity and making informed decisions about staffing and resource allocation.

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
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called `Msr_Budget`, which is designed to dynamically calculate a budget value based on the current week of the year. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: The measure starts by determining the current week number of the year using the `SELECTEDVALUE` function. This means it looks at the context in which the measure is being evaluated (like a report or dashboard) to find out which week is currently selected.

2. **Find the Previous Budget Value**: The measure then calculates the most recent available budget value before the current week. It does this by using the `CALCULATE` function along with a `FILTER`. The `FILTER` function looks at all the dates in the `DimDate` table and selects only those where the week number is less than the current week. This effectively filters out any weeks that are not prior to the current week.

3. **Return the Appropriate Value**: Finally, the measure checks if there is a current budget value available (using `[Msr_SalesPriceBaseCurrency]`). If this current value is not blank (meaning it exists), it returns that value. If the current value is blank (meaning there is no budget for the current week), it falls back to the previously calculated budget value from the last available week.

### Summary:
In essence, this measure calculates the budget for the current week if it exists; if not, it retrieves the most recent budget from the previous weeks. This approach ensures that users always have a relevant budget figure to work with, even if the current week’s budget data is not yet available.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'MSR_Cumulative Cost', which provides the total cost accumulated up to a selected month in a reporting visual. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The code starts by determining which month is currently selected in the visual (like a chart or table). It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed.

2. **Calculate Cumulative Cost**: The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to and including the selected month. 

3. **Summing Costs**: The `CALCULATE` function is used to change the context in which the data is evaluated. It sums up the 'CostAmount' from the 'vw_Timesheet' table.

4. **Filtering the Data**: The `FILTER` function is applied to ensure that only the months that are less than or equal to the selected month are included in the calculation. The `ALLSELECTED` function allows the measure to respect any filters applied to the 'DimDate' table, ensuring that it only considers the relevant months based on user selections.

In summary, this DAX measure calculates the total costs incurred from the start of the year up to the month that the user has selected in the report. This is useful for understanding cumulative expenses over time, helping businesses track their costs effectively.

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
   - The `CALCULATE` function is then used to compute the total sales amount from the 'vw_Timesheet' table. This is where the actual sales data is stored.

3. **Filter for Relevant Months**:
   - Inside the `CALCULATE` function, a `FILTER` is applied. This filter looks at all the months in the 'DimDate' table (using `ALLSELECTED` to respect any other filters that might be applied) and includes only those months that are less than or equal to the selected month. This means if you are looking at March, it will include sales from January, February, and March.

4. **Final Result**:
   - The result of this measure is the total sales amount for all months up to and including the selected month. This allows users to see how sales have accumulated over time, providing insights into trends and performance.

In summary, this DAX measure helps businesses track cumulative sales performance up to a specific month, enabling better analysis of sales trends over time.

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

Here's a breakdown of what it does:

1. **TotalValue Calculation**:
   - The measure first calculates the total hours worked (`SUM(vw_Timesheet[Hours])`) across all selected filters for `GroupCat`, `Project`, and `EmployeeName`. 
   - The `ALLSELECTED` function is used here, which means it respects any filters that are currently applied in the report but ignores any filters specifically on the `GroupCat`, `Project`, and `EmployeeName` columns. This allows the measure to consider the total hours worked across all relevant data while still respecting other filters that might be applied.

2. **Percentage Calculation**:
   - After calculating the total hours (stored in the variable `TotalValue`), the measure then calculates the percentage of hours worked for the current context (which could be a specific group, project, or employee).
   - It does this by dividing the sum of hours for the current context (`SUM(vw_Timesheet[Hours])`) by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safe way to perform division in DAX. If the `TotalValue` is zero (which would lead to a division error), it returns 0 instead of an error.

### In Summary:
The `Msr_HoursPercentage` measure calculates what portion of the total hours worked (across all selected categories) is represented by the hours worked in the current context (like a specific project or employee). This helps in understanding how much of the total effort is contributed by a particular segment, allowing for better analysis of resource allocation and performance.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the project margin for a specific measure called 'MSR_ProjectMargin'. Here's a breakdown of what it does in simple business terms:

1. **Sales Amount**: The expression starts by summing up all the values in the `SalesAmount` column from the `vw_Timesheet` table. This represents the total revenue generated from projects.

2. **Cost Amount**: Next, it sums up all the values in the `CostAmount` column from the same table. This represents the total costs incurred for those projects.

3. **Calculating Project Margin**: The measure then subtracts the total costs (CostAmount) from the total sales (SalesAmount). 

In essence, this DAX expression calculates the **project margin**, which is the profit made from projects after accounting for the costs. A positive project margin indicates that the projects are generating more revenue than they cost, while a negative margin would suggest that costs exceed revenue. 

Overall, this measure helps businesses assess the financial performance of their projects by providing a clear view of profitability.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate a measure called `Msr_SalesPriceBaseCurrency`. Let's break it down into simpler terms to understand what it achieves:

1. **Context of the Calculation**: The measure operates on a data table called `vw_Timesheet`, which likely contains information about projects, clients, and possibly timesheet entries related to those projects.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data from `vw_Timesheet` by two columns: `ProjectProfile` and `Client`. This means that the calculation will consider each unique combination of project profiles and clients.

3. **Selecting a Value**: Within each group created by `SUMMARIZE`, the code uses `SELECTEDVALUE(tbl_Project[SalesPriceBaseCurrency])`. This function retrieves the sales price in the base currency for the current project profile being evaluated. If there is a single value for the sales price, it will return that; if there are multiple values, it will return blank.

4. **Calculating the Total**: The `SUMX` function then takes the summarized table (which now includes the project profile, client, and the corresponding sales price) and sums up the "MeasureValue" for each group. Essentially, it adds up all the sales prices in the base currency for each unique combination of project profile and client.

### Summary of What It Achieves:
In simple terms, this DAX measure calculates the total sales price in the base currency for all projects and clients listed in the `vw_Timesheet`. It groups the data by project and client, retrieves the relevant sales price for each group, and then sums these values to provide a comprehensive total. This measure is useful for understanding the overall sales performance in a consistent currency across different projects and clients.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called "Static Total Employees." Let's break it down into simple terms to understand what it does:

1. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part of the code counts the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table. Essentially, it tells us how many different employees are present in the dataset.

2. **CALCULATE(...)**: The `CALCULATE` function modifies the context in which the data is evaluated. In this case, it is used to apply specific filters to the counting operation.

3. **ALL(vw_Timesheet)**: This function removes any existing filters that might be applied to the `vw_Timesheet` table. By doing this, it ensures that the count of employees is based on the entire dataset, rather than just a subset that might be filtered by other criteria in a report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This condition filters the data to only include employees whose contract status is marked as "Valid." This means that the measure will only count employees who are currently active or valid according to the specified criteria.

### Summary:
In summary, this DAX measure calculates the total number of unique employees who have a "Valid" contract status, ignoring any other filters that might be applied to the data. This is useful for understanding how many employees are currently active and eligible under the specified conditions, providing a clear view of the workforce status at a given point in time.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to calculate a measure called 'TotalContractedHours'. Let's break it down in simple business terms:

1. **SUMX Function**: This function is used to perform a calculation over a table and then sum the results. In this case, it will iterate through a set of values and calculate a total based on those values.

2. **VALUES Function**: The `VALUES(vw_Timesheet[EmployeeName])` part creates a unique list of employee names from the 'vw_Timesheet' table. This means that for each employee, the calculation will be performed separately.

3. **MAX Function**: The `MAX(vw_Timesheet[HoursPerWeek])` part looks at the 'HoursPerWeek' column in the 'vw_Timesheet' table and finds the maximum number of hours that each employee is contracted to work in a week.

4. **Putting It All Together**: The entire expression calculates the total contracted hours for all employees by:
   - Going through each unique employee name.
   - For each employee, it finds the maximum hours they are contracted to work per week.
   - Finally, it sums up these maximum hours across all employees.

### What It Achieves:
In summary, this DAX measure calculates the total number of contracted hours for all employees by summing up the highest weekly hours each employee is contracted to work. This is useful for understanding the overall capacity or workload that the organization has based on employee contracts.

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

2. **TotalContractedHours**: This measure indicates the total number of hours that were agreed upon or contracted for the project or task. This is the expected or planned amount of work.

3. **The Calculation**: The expression `[TotalActualHours] - [TotalContractedHours]` subtracts the total contracted hours from the total actual hours. 

4. **What It Achieves**: 
   - If the result is positive, it means that more hours were worked than were originally contracted, indicating potential overwork or additional effort beyond what was planned.
   - If the result is negative, it suggests that fewer hours were worked than contracted, which could indicate underperformance or that the project is ahead of schedule.
   - If the result is zero, it means that the actual hours worked perfectly match the contracted hours, indicating that the project is on track as per the agreement.

In summary, this measure helps assess whether a project is meeting its expected workload in terms of hours worked, providing insights into resource utilization and project performance.

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

1. **Check for Blank Values**: The measure first checks if the total sum of hours from the 'vw_Timesheet' table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Zero if Blank**: If the sum of hours is blank (meaning no hours have been tracked), the measure will return a value of 0. This is important because it ensures that when there are no recorded hours, the measure does not show a blank value, which could be confusing in reports.

3. **Return the Sum of Hours if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the measure will return the actual total sum of hours tracked from the 'vw_Timesheet' table.

### Summary:
In essence, this measure calculates the total hours tracked by summing up the hours from the timesheet. If no hours are recorded, it returns 0 instead of leaving it blank. This makes it easier for users to understand the data in reports, as they will see a clear "0" instead of an empty space when no hours have been logged.

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

3. **Return Total Hours if Available**: If there are hours recorded (i.e., the sum is not blank), the measure returns the actual total number of hours tracked. This is calculated using the `SUM` function on the `Hours` column of the `vw_Timesheet` table.

### Summary:
In essence, this measure helps to identify whether any hours have been tracked in the timesheet. If no hours are recorded, it clearly indicates this by returning "Empty". If hours are present, it provides the total number of hours tracked. This can be useful for reporting and ensuring that time tracking is being properly maintained.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you provided defines a measure called `trackedDiff`. Let's break it down step by step to understand what it calculates and achieves in simple business terms.

### Components of the DAX Expression:

1. **[TotalHoursTracked]**: This part represents the total number of hours that have been tracked for a specific context, such as a project, employee, or time period. It is likely a measure that sums up all the hours recorded in a timesheet.

2. **CALCULATE Function**: This function modifies the context in which data is evaluated. It allows us to perform calculations based on specific filters or conditions.

3. **MAXX Function**: This function is used to evaluate an expression for each row in a table and return the maximum value. In this case, it is being used to find the maximum number of hours worked per week from a distinct list of hours.

4. **DISTINCT(vw_Timesheet[HoursPerWeek])**: This part creates a unique list of hours worked per week from the `vw_Timesheet` table. It ensures that only unique values are considered when calculating the maximum.

5. **ALLEXCEPT Function**: This function removes all filters from the `vw_Timesheet` table except for the filter on `EmployeeID`. This means that the calculation will consider all records for a specific employee, regardless of other filters that might be applied.

### What It Achieves:

Putting it all together, the `trackedDiff` measure calculates the difference between the total hours tracked for a specific context (like an employee or project) and the maximum hours worked in a week by that same employee, considering all their records.

### In Business Terms:

- **Purpose**: The measure helps to identify how much the total hours tracked deviate from the maximum hours that an employee has worked in a single week. 

- **Use Case**: This could be useful for managers to understand if an employee is consistently working more or less than their peak performance week. It can highlight trends in workload, help in resource allocation, and identify potential issues with employee workload management.

In summary, `trackedDiff` provides insights into an employee's current tracked hours compared to their maximum weekly hours, helping businesses monitor performance and workload effectively.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided calculates a measure called `trackedDiff2`, which is designed to find the difference between two values related to hours tracked in a timesheet system. Here’s a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked by an employee over a specific period.

2. **Maximum Hours Per Week**: The next part of the calculation uses the `CALCULATE` function to determine the maximum number of hours that have been recorded per week for that employee. This is done using:
   - `MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])`: This part looks at the unique values of hours worked per week and finds the highest value among them.

3. **Removing Filters**: The `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])` function is used to ignore any filters that might be applied to the `HoursPerWeek` column. This ensures that the calculation considers all weeks, not just those currently filtered in the report.

4. **Keeping Employee Context**: The `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` function keeps the context of the specific employee while removing other filters. This means that the calculation will only look at the data for the current employee, regardless of any other filters that might be applied to the report.

5. **Final Calculation**: The final result of the measure `trackedDiff2` is the difference between the total hours tracked by the employee and the maximum hours they have worked in any single week. 

### Summary:
In summary, `trackedDiff2` calculates how many more hours an employee has tracked in total compared to their highest recorded hours in a single week. This can help identify if an employee is consistently working more hours than their peak week, which might indicate workload issues or overtime.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate the total billable hours for specific projects from a timesheet data source. Here's a breakdown of what it does in simple business terms:

1. **Variable Definition**: The code starts by defining a variable called `FilteredHours`. This variable will hold a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**: The `Filter` function is used to create a subset of the `vw_Timesheet` data based on certain conditions:
   - It includes only those entries where:
     - The project qualifies as a billable project (`vw_Timesheet[QualifyPrj] = 1` and `vw_Timesheet[BillablePrj] = 1`).
     - OR the project name contains the phrase "Customer Success Services" (this is checked using the `SEARCH` function).

3. **Calculating Total Hours**: After filtering the data, the `SUMX` function is used to calculate the total hours from the filtered entries. It sums up the `Hours` column for all the rows that meet the filtering criteria.

4. **Final Result**: The final result of this measure, `UTI_TotalBillableHours`, is the total number of hours worked on projects that are either specifically marked as billable or are related to "Customer Success Services".

In summary, this DAX measure helps a business understand how many hours have been worked on projects that are either billable or related to customer success, allowing for better tracking of billable work and resource allocation.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `UTI_TOTALHOURS`. Here’s a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the total number of hours worked, but only for specific projects that meet a certain condition.

2. **Components**:
   - **SUM(vw_Timesheet[Hours])**: This part of the code adds up all the values in the `Hours` column from the `vw_Timesheet` table. Essentially, it totals the hours worked.
   - **CALCULATE**: This function modifies the context in which the data is evaluated. It allows us to apply filters to the data before performing the calculation.
   - **vw_Timesheet[QualifyPrj] = 1**: This is a filter condition. It specifies that we only want to include hours from projects where the `QualifyPrj` column has a value of 1. This means we are focusing on a specific subset of projects that qualify for whatever criteria is defined by the value of 1.

3. **Outcome**: The measure `UTI_TOTALHOURS` will return the total hours worked on projects that are marked as qualifying (where `QualifyPrj` equals 1). This is useful for reporting and analysis, as it helps stakeholders understand how many hours are being spent on projects that meet certain criteria, allowing for better resource allocation and project management.

In summary, this DAX measure helps businesses track and analyze the total hours worked on qualifying projects, providing insights into productivity and project performance.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the "Billable Hour Ratio" for active employees within a business context. Let's break it down step by step in simple terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable captures the total number of hours that employees have billed to clients. It represents the productive time that can be charged for services rendered.
   - `_TotalHours`: This variable captures the total number of hours worked by employees, which includes both billable and non-billable hours.

2. **Return Statement**:
   - The `RETURN` section of the code is where the actual calculation happens. It checks a few conditions before calculating the ratio:
     - **Check for Active Contacts**: The first `IF` statement checks if there are any active employees (referred to as `[Msr_ActiveContacts]`). If there are no active employees, the calculation does not proceed.
     - **Check for Total Hours**: The second `IF` statement checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the calculation does not proceed.
     - **Calculate the Ratio**: If both previous checks pass, the code attempts to calculate the ratio of billable hours to total hours. This is done using the `DIVIDE` function, which safely divides `_TotalBillableHours` by `_TotalHours`. If the result of this division is blank (which could happen if `_TotalHours` is zero), it returns 0 instead of an error.

3. **Outcome**:
   - The final output of this measure is the Billable Hour Ratio, which indicates the proportion of hours that employees are billing to clients compared to the total hours they have worked. A higher ratio suggests better utilization of employee time for billable work, which is typically a key performance indicator in service-oriented businesses.

In summary, this DAX measure helps businesses understand how effectively their active employees are utilizing their time for billable work, which is crucial for assessing productivity and profitability.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

The following filters are applied to the entire report:

- Filter on `DimDate`.`CurrentYearFlag` (Type: Advanced, Explanation: In simple business terms, the provided Power BI filter definition JSON is specifying a condition that focuses on a particular aspect of your data related to dates.

Here's what it means:

- **Target Field**: The filter is applied to the field called `CurrentYearFlag` in the `DimDate` table. This field is likely a flag that indicates whether a date falls within the current year.

- **Filter Condition**: The condition "`CurrentYearFlag` = 1" means that the filter will only include data where the `CurrentYearFlag` is set to 1.

- **Data Included**: By applying this filter, you are including only those records (or rows) from your data that represent dates in the current year. 

- **Data Excluded**: Any records where the `CurrentYearFlag` is not equal to 1 (which would typically represent dates from previous years or future years) will be excluded from your analysis.

In summary, this filter is used to focus your analysis on data that is relevant to the current year, ensuring that you are only looking at the most recent and pertinent information for your reporting or decision-making.)

### <a name="report-pages"></a>Report Pages

#### <a name="page-customer-analysis"></a>Page: Customer Analysis

*Internal Name: `ecc7667875c00809763b`, Ordinal: 0*

##### Page Level Filters

- Filter on `vw_Timesheet`.`BillablePrj` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the `vw_Timesheet` view, specifically focusing on the `BillablePrj` field.

Here's what it means:

- The filter is looking for records where the `BillablePrj` value is equal to **1**.
- This means that only the entries in the `vw_Timesheet` that have a `BillablePrj` status of **1** will be included in the analysis or report.
- Any records where `BillablePrj` is **not equal to 1** (for example, 0, 2, or any other value) will be excluded from the results.

In summary, this filter is used to focus on projects that are billable, as indicated by the value **1** in the `BillablePrj` field.)

##### Visuals on this Page

**image**

- Type: `image`
- Name: `49543b4ac15a8ce539d3`
- Fields Used: _(None detected)_

**slicer**

- Type: `slicer`
- Name: `e2414703060e087d3230`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `4543cd36183039c320d3`
- Fields Used:
  - `cc_SalesPerson` (Query: `vw_Timesheet.cc_SalesPerson`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `a60364499b7915da7e60`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)
  - `Min(DimDate.ShortDate)` (Role: Unknown)
  - `Max(DimDate.ShortDate)` (Role: Unknown)

**slicer**

- Type: `slicer`
- Name: `be723b7e75b4661912d0`
- Fields Used:
  - `Unit` (Query: `vw_Timesheet.MAIN_UNIT`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `e4e04919d0548163c0a8`
- Fields Used:
  - `SubUnit` (Query: `vw_Timesheet.Unit`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `8048e854158dc07422ce`
- Fields Used:
  - `ClientName` (Query: `tbl_Project.ClientName`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `800e477135b21a02300b`
- Fields Used:
  - `Own-Sub-ExtT` (Query: `PBI_TIMESHEET.Own-Sub-ExtT`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `b5b5047cc00ba23030e5`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `6e846506dec1641cb274`
- Fields Used:
  - `cc_Employer` (Query: `PBI_TIMESHEET.cc_Employer`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `b37c531703b3e683879e`
- Fields Used:
  - `ProjectProfile` (Query: `vw_Timesheet.ProjectProfile`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `9285b49f5742861eada8`
- Fields Used:
  - `Project` (Query: `vw_Timesheet.Project`) (Role: Values)

**actionButton**

- Type: `actionButton`
- Name: `88e87e1eddb866e15c8a`
- Fields Used: _(None detected)_

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

#### <a name="page-drill"></a>Page: Drill

*Internal Name: `15ce80bb1dcc6c28b46a`, Ordinal: 2*

##### Page Level Filters

- Filter on `vw_Timesheet`.`EmployeeName` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the field `vw_Timesheet.EmployeeName`. This means that no specific employee names are being excluded or included; instead, every employee's name from the timesheet data will be considered.

2. **Data Inclusion**: When this filter is applied, it ensures that every employee's timesheet data is visible in the report. There are no restrictions on which employees' data can be seen.

3. **No Exclusions**: Since the filter is set to "(All values)", there are no exclusions. This means that if there were any employees who might have been filtered out (for example, if you were only interested in a specific department or role), they will still be included in the report.

### Summary:
In simple terms, this filter allows you to see the timesheet data for **all employees** without any limitations. It provides a complete view of the data related to employee names in the timesheet, ensuring that no one is left out.)
- Filter on `vw_Timesheet`.`Unit` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the target field, which in this case is `vw_Timesheet`.`Unit`. This means that no specific criteria are applied to limit the data.

2. **Data Included**: When this filter is applied, every single entry in the `Unit` column of the `vw_Timesheet` dataset will be included in the report. There are no exclusions or restrictions.

3. **Data Excluded**: Since the filter is set to include all values, there are no data points being excluded. Every unit recorded in the timesheet will be visible.

### Summary:
In simple terms, this filter allows you to see all the data related to `Unit` in the timesheet without any limitations. It ensures that every unit is represented in your analysis, providing a complete view of the data.)
- Filter on `vw_Timesheet`.`EmployeeName` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and dashboards. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the field `vw_Timesheet.EmployeeName`. This means that when this filter is applied, it does not exclude any employees from the data being analyzed or displayed.

2. **Data Included**: Every employee listed in the `vw_Timesheet` view will be included in the report. There are no restrictions or conditions applied to limit which employees are shown.

3. **Data Excluded**: Since the filter is set to include all values, no employee is excluded. This means that all timesheet entries for every employee will be visible in the report.

### Summary:
In simple terms, this filter allows you to see data for every employee without any limitations. You will have a complete view of all timesheet entries related to all employees in the dataset.)
- Filter on `vw_Timesheet`.`Hours` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the target field, which in this case is `vw_Timesheet`.`Hours`. This means that no specific criteria are applied to limit the data.

2. **Data Included**: When this filter is applied, every single entry in the `Hours` column of the `vw_Timesheet` view will be included in the analysis. There are no restrictions or exclusions based on any conditions.

3. **Data Excluded**: Since the filter is set to include all values, there are no exclusions. All records related to hours worked, regardless of any specific attributes (like date, employee, project, etc.), will be shown.

### Summary:
In simple terms, this filter allows you to see **everything** related to hours worked in the timesheet data without filtering anything out. It’s like saying, "Show me all the hours logged, without any limits.")
- Filter on `vw_Timesheet`.`HoursperWeek` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the field `vw_Timesheet`.`HoursperWeek`. This means that when this filter is applied, it does not exclude any data related to the hours worked per week.

2. **Data Included**: Every record in the `vw_Timesheet` dataset will be considered, regardless of how many hours are logged per week. For example, if some employees worked 20 hours, some worked 40 hours, and others worked 60 hours, all of these records will be included in any analysis or visualizations.

3. **Data Excluded**: Since the filter is set to "(All values)", there are no exclusions. No data is left out based on the `HoursperWeek` field.

### Summary:
In summary, this filter allows you to see the complete picture of hours worked per week without any restrictions. It ensures that all employees' hours are accounted for in your analysis, providing a comprehensive view of the data.)
- Filter on `vw_Timesheet`.`Approved` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and dashboards. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **Target Field**: The filter is applied to the field `vw_Timesheet`.`Approved`. This field likely indicates whether a timesheet has been approved or not.

2. **Filter Meaning**: The filter `(All values)` means that **all records** from the `Approved` field will be included in the analysis. There are no restrictions or exclusions based on the approval status of the timesheets.

3. **Data Impact**: Since the filter includes all values, it does not limit the data to only approved or only unapproved timesheets. Instead, it allows you to see the complete picture, including both approved and unapproved timesheets.

### Summary:
When this filter is applied, you will see all timesheet records, regardless of their approval status. This is useful when you want to analyze the entire dataset without filtering out any specific approvals.)
- Filter on `vw_Timesheet`.`ReportReady` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** from the data source. This means that when you apply this filter to the target `vw_Timesheet`.`ReportReady`, you are not excluding any records. Every entry in the `ReportReady` view will be considered.

2. **Data Included**: Since the filter is set to "(All values)", all timesheet records will be included in your analysis. This could encompass all employees, all time entries, and all relevant data points without any restrictions.

3. **Data Excluded**: There are no exclusions with this filter. No data is being filtered out, so you will see the complete dataset as it exists in the `vw_Timesheet`.`ReportReady`.

### Summary:
In summary, applying this filter means you are looking at the entire dataset without any limitations or exclusions. This is useful when you want a comprehensive view of all timesheet data for analysis or reporting purposes.)
- Filter on `vw_Timesheet`.`Rejected` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and dashboards. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **Target Data**: The filter is applied to a specific dataset or table, which in this case is `vw_Timesheet`.`Rejected`. This likely refers to a view or table that contains records of timesheets that have been rejected.

2. **Filter Meaning**: The phrase "(All values)" indicates that no specific filtering criteria are being applied. This means that all records from the `vw_Timesheet`.`Rejected` dataset will be included in the report or visualization.

3. **Inclusion of Data**: Since the filter is set to include "(All values)", every single rejected timesheet record will be shown. There are no exclusions or limitations based on any conditions.

### Summary:
When this filter is applied, you will see all rejected timesheet entries without any restrictions. This is useful when you want to analyze or report on the complete set of rejected timesheets without missing any data.)
- Filter on `vw_Timesheet`.`Project` (Type: Categorical, Explanation: This Power BI filter definition is specifying a condition that focuses on a particular project within the dataset. Let's break it down in simple business terms:

1. **Target Data**: The filter is applied to the `Project` field in the `vw_Timesheet` view. This means we are looking at the projects listed in the timesheet data.

2. **Filter Condition**: The filter condition is saying that we only want to include data for a specific project. The project in question is named **"VLAAMSE MAATSCH. WATERV. - Changes >= 4 hours"**.

3. **Inclusion Criteria**: By using the `IN` clause, the filter is explicitly including only the records that match this project name. This means that any timesheet entries or data related to this specific project will be included in the analysis.

4. **Exclusion of Other Data**: All other projects that do not match this name will be excluded from the analysis. This means that if there are timesheet entries for other projects, they will not be considered when this filter is applied.

In summary, this filter is set up to focus solely on the project "VLAAMSE MAATSCH. WATERV. - Changes >= 4 hours" in the timesheet data, including only the relevant entries for that project and excluding everything else.)
- Filter on `tbl_Project`.`ClientName` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the field `tbl_Project.ClientName`. This means that when this filter is applied, it does not restrict or limit the data based on any specific client names.

2. **Data Included**: Every client name from the `tbl_Project` table will be included in the report or visualization. There are no exclusions, so you will see data for all clients.

3. **No Exclusions**: Since the filter is set to "All values," there are no client names being excluded. This ensures that you have a complete view of all projects associated with every client.

### Summary:
In simple terms, this filter allows you to see data for every client in your project table without any restrictions. You will not miss any information related to any client name when this filter is applied.)
- Filter on `tbl_Project`.`ProjectGroupDescription` (Type: Categorical, Explanation: This Power BI filter definition is specifying a condition that focuses on the `ProjectGroupDescription` field from the `tbl_Project` table. 

In simple business terms, this filter is saying:

- **Include Only**: The filter will only include records (or rows) from the `tbl_Project` table where the `ProjectGroupDescription` is exactly "Time material".
- **Exclude Everything Else**: Any records where the `ProjectGroupDescription` is anything other than "Time material" will be excluded from the analysis or report.

So, when this filter is applied, you will only see projects that fall under the "Time material" category, and all other project descriptions will be ignored.)

##### Visuals on this Page

**actionButton**

- Type: `actionButton`
- Name: `652ecd292e367e2adcdd`
- Fields Used: _(None detected)_

**tableEx**

- Type: `tableEx`
- Name: `7f11fbe3e496fafc0c07`
- Fields Used:
  - `TimesheetDate` (Query: `PBI_TIMESHEET.TimesheetDate`) (Role: Values)
  - `SubUnit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)
  - `Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Values)
  - `HoursperWeek` (Query: `Sum(PBI_TIMESHEET.HoursperWeek)`) (Role: Values)
  - `Approved` (Query: `PBI_TIMESHEET.Approved`) (Role: Values)
  - `ReportReady` (Query: `PBI_TIMESHEET.ReportReady`) (Role: Values)
  - `Rejected` (Query: `PBI_TIMESHEET.Rejected`) (Role: Values)
  - `Project` (Query: `PBI_TIMESHEET.Project`) (Role: Values)
  - `ManagerName` (Query: `PBI_TIMESHEET.ManagerName`) (Role: Values)
  - `Week` (Query: `DimDate.WeekNumberOfYear`) (Role: Values)
  - `Index` (Query: `Sum(PBI_TIMESHEET.Index)`) (Role: Values)
  - `Year` (Query: `Sum(DimDate.CalendarYear)`) (Role: Values)
  - `Unit` (Query: `lkp_Unit.Unit`) (Role: Values)
  - `QualifyPrj` (Query: `Sum(vw_Timesheet.QualifyPrj)`) (Role: Values)
  - `BillablePrj` (Query: `Sum(vw_Timesheet.BillablePrj)`) (Role: Values)
  - `ExtraDetails` (Query: `vw_Timesheet.ExtraDetails`) (Role: Values)
  - `Month` (Query: `Sum(DimDate.MonthNumberOfYear)`) (Role: Values)
  - `TimesheetCode` (Query: `vw_Timesheet.TimesheetCode`) (Role: Values)
  - `BillableDep` (Query: `Sum(vw_Timesheet.BillableDep)`) (Role: Values)

#### <a name="page-project-details"></a>Page: Project Details

*Internal Name: `2f52ad4a004b09107900`, Ordinal: 1*

##### Page Level Filters

- Filter on `vw_Timesheet`.`BillablePrj` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the `BillablePrj` column of the `vw_Timesheet` view.

Here's what it means:

- The filter is looking for records where the value in the `BillablePrj` column is equal to **1**.
- This means that only those entries in the `vw_Timesheet` that have a `BillablePrj` value of **1** will be included in the analysis or report.
- Any entries where the `BillablePrj` value is **not** equal to **1** (such as 0, 2, or any other number) will be excluded from the results.

In summary, this filter is used to focus on a specific subset of data that represents billable projects, allowing users to analyze only those timesheets that are associated with billable work.)

##### Visuals on this Page

**image**

- Type: `image`
- Name: `23882348c029454140c3`
- Fields Used: _(None detected)_

**slicer**

- Type: `slicer`
- Name: `5cc474d19a9be62f7c6f`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `30ac69436bb17c560e22`
- Fields Used:
  - `TimeSpan` (Query: `DimDate.ShortDate`) (Role: Values)
  - `Min(DimDate.ShortDate)` (Role: Unknown)
  - `Max(DimDate.ShortDate)` (Role: Unknown)

**slicer**

- Type: `slicer`
- Name: `1218b3e663e35022d9d5`
- Fields Used:
  - `DevoteamPillar` (Query: `tbl_Project.DevoteamPillar`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `91d269b51621b58cd5b8`
- Fields Used:
  - `ClientName` (Query: `tbl_Project.ClientName`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `3371e7216d27c7716636`
- Fields Used:
  - `cc_SalesPerson` (Query: `vw_Timesheet.cc_SalesPerson`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `bb8c1f8dd224d829d32b`
- Fields Used:
  - `cc_Employer` (Query: `PBI_TIMESHEET.cc_Employer`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `331fc4a3b6d31c038b30`
- Fields Used:
  - `Status` (Query: `tbl_Project.Status`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `c275d85b5800b1d47638`
- Fields Used:
  - `ProjectProfile` (Query: `vw_Timesheet.ProjectProfile`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `1221310b511e5b428280`
- Fields Used:
  - `Project` (Query: `vw_Timesheet.Project`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `9402ca390dab8c29e0ca`
- Fields Used:
  - `IPM_ManagerName` (Query: `vw_Timesheet.IPM_ManagerName`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `7e3b4de92424e0cc2bde`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**slicer**

- Type: `slicer`
- Name: `f4a2a804d812cd7dee49`
- Fields Used:
  - `Own-Sub-ExtT` (Query: `PBI_TIMESHEET.Own-Sub-ExtT`) (Role: Values)

**actionButton**

- Type: `actionButton`
- Name: `dab076d39b091c758018`
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

