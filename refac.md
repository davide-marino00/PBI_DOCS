# Power BI Model & Report Documentation

*Generated on: 2025-04-29 02:13:00*

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

The Power BI data model appears to be designed for a business domain focused on project management and timesheet tracking, likely within a professional services or project-based organization. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a strong emphasis on monitoring employee time allocation and identifying gaps in timesheet submissions. The model includes various lookup tables ('lkp_Project', 'lkp_Unit', 'lkp_Code', and 'lkp_fltr_Employee') that provide contextual information about projects, organizational units, and timesheet codes, facilitating detailed analysis of time spent on different projects and units.

The structure of the model indicates a relational approach where the 'vw_Timesheet' serves as a central fact table, linking to various dimensions such as dates ('DimDate') and project-related attributes. This setup allows for comprehensive reporting and analysis, enabling stakeholders to assess project performance, employee productivity, and overall resource allocation. The inclusion of 'vw_missing_timesheet' further enhances the model's utility by providing insights into compliance and potential issues with timesheet submissions, thereby supporting operational efficiency and accountability within the organization.

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

The 'DimDate' table serves as a comprehensive date dimension, providing essential temporal attributes that enable businesses to analyze and report on data trends over time. By offering detailed breakdowns of dates, including day, week, month, and quarter information, this table supports effective time-based analysis and enhances the accuracy of time-related metrics in reporting and dashboards.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting within the dataset. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient data retrieval and analysis in time-based reporting and analytics. |
| `DayName` | `string` | The 'DayName' column in the 'DimDate' table stores the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | Column Description: The 'DayNumberOfMonth' column represents the numerical day of the month (ranging from 1 to 31) for each date entry in the 'DimDate' table, facilitating date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical designation of each day of the week, with values ranging from 1 (Sunday) to 7 (Saturday), facilitating date-related analyses and reporting. |
| `DayNumberOfYear` | `int64` | Column Description: The 'DayNumberOfYear' column represents the sequential day of the year (ranging from 1 to 365 or 366) for each date entry in the 'DimDate' table, facilitating date-based calculations and analyses. |
| `MonthName` | `string` | Column Description: The 'MonthName' column contains the full names of the months, providing a human-readable representation of the month for date-related analyses in the DimDate table. |
| `MonthNumberOfYear` | `int64` | Column Description: The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month (1 for January through 12 for December), facilitating time-based analysis and reporting. |
| `ShortDate` | `dateTime` | Column Description: The 'ShortDate' column captures the date in a concise format, facilitating efficient date-based analysis and reporting within the DimDate table. |
| `WeekNumberOfYear` | `int64` | Column Description: The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Indicates whether the date corresponds to the current calendar year, with values typically set to 'Yes' or 'No'.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `CurrentYearFlag` in a data model, typically within a tool like Power BI or Excel. Here's a simple explanation of what this code does:

1. **Purpose**: The `CurrentYearFlag` column is designed to indicate whether a date in the `DimDate` table falls within the current year or not.

2. **How It Works**:
   - The function `YEAR(DimDate[ShortDate])` extracts the year from the date in the `ShortDate` column of the `DimDate` table.
   - The function `YEAR(TODAY())` gets the current year based on today's date.
   - The `IF` statement checks if the year extracted from `DimDate[ShortDate]` is less than or equal to the current year.

3. **Output**:
   - If the year from `DimDate[ShortDate]` is less than or equal to the current year, the expression returns `1`. This means that the date is either in the current year or a previous year.
   - If the year is greater than the current year, it returns `0`, indicating that the date is in a future year.

4. **Business Implication**: This calculated column can be used in reports or dashboards to easily filter or categorize data based on whether the dates are in the current year or earlier. For example, it can help in analyzing trends, performance, or any time-sensitive metrics by distinguishing between current and future dates.

In summary, the `CurrentYearFlag` column helps identify dates that are in the current year or earlier, allowing for better data analysis and reporting.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table captures and categorizes various metrics related to hours worked and their corresponding percentage contributions, enabling businesses to analyze workforce productivity and resource allocation effectively. This table serves as a critical tool for performance evaluation and strategic decision-making regarding labor management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column contains string representations of hours worked alongside their corresponding percentage contributions, facilitating the analysis of employee time allocation and productivity metrics. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string values that represent the various metrics related to hours worked and their corresponding percentage contributions, facilitating detailed analysis of workforce productivity. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes and organizes various codes used across the business, enabling efficient data classification and retrieval. Its columns facilitate the identification of code qualifications and their sorting order, supporting streamlined reporting and analysis processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers used to categorize and enrich data within the lkp_Code table, facilitating efficient data retrieval and analysis. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numeric identifier to categorize data for enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table is a string that specifies the order in which records should be arranged for optimal data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics. This table enhances data integrity and usability by standardizing employee information for analysis and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column stores unique identifiers for employees, facilitating data enrichment and integration within the employee lookup table. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, facilitating the enrichment of data by providing identifiable references for each individual within the employee lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, linking project identifiers to their respective qualification and billing metrics, while categorizing them into groups for streamlined reporting and analysis. This table enables businesses to efficiently track project performance and financials, facilitating informed decision-making and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing amount associated with each project, facilitating financial tracking and analysis. |
| `Group` | `string` | The 'Group' column in the 'lkp_Project' table categorizes projects into distinct classifications for enhanced data organization and retrieval. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the organization. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of each project, serving as a numerical identifier for categorizing projects based on their eligibility or compliance criteria. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their characteristics such as billable status and external affiliations, which aids in resource allocation, financial tracking, and performance analysis across different departments.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column (int64) in the 'lkp_Unit' table indicates the department responsible for billable activities associated with each unit, facilitating accurate financial tracking and reporting. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column identifies the organizational unit associated with each entry in the lkp_Unit table, facilitating data enrichment and classification. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores external type identifiers for owned sub-units, facilitating data enrichment and integration processes. |
| `Unit` | `string` | The 'Unit' column in the 'lkp_Unit' table stores string values representing various measurement units used for data enrichment processes. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, status, leadership, and financial aspects. This table enables effective project management and oversight by providing stakeholders with insights into project progress, categorization, and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing essential data for project tracking and performance analysis. |
| `Administration` | `string` | The 'Administration' column in the 'tbl_Project' table captures the administrative details and oversight responsibilities associated with each project, facilitating effective management and governance. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, ensuring accurate tracking of updates within the project management system. |
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each project, facilitating the identification and management of client relationships within the project database. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating efficient data retrieval and management within the tbl_Project table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated, providing a timestamp for tracking project creation within the tbl_Project table. |
| `Currency` | `string` | The 'Currency' column in the 'tbl_Project' table specifies the type of currency used for financial transactions related to each project, facilitating accurate budgeting and reporting. |
| `DateCreated` | `dateTime` | Column Description: The 'DateCreated' column records the exact date and time when each project entry was initiated, providing a timestamp for tracking project creation and historical reference. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the strategic focus area or category of the project within the Devoteam framework, facilitating targeted analysis and reporting. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column in the 'tbl_Project' table indicates the scheduled date and time for the completion of each project, facilitating effective project timeline management and planning. |
| `Project` | `string` | The 'Project' column stores the names of various projects, enabling the organization to categorize and track project-specific data effectively. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed textual overview of each project, providing essential context and objectives to enhance understanding and facilitate decision-making. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the category or classification of a project within the 'tbl_Project' table, facilitating organization and management of related projects. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a detailed textual overview of the project group, enhancing understanding of its objectives and scope within the tbl_Project table. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for data enrichment processes. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the associated project in the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency amount of the sales price for each project, stored as a decimal to ensure precise financial calculations. |
| `Startdate` | `dateTime` | The 'Startdate' column in the 'tbl_Project' table captures the date and time when a project is initiated, serving as a critical reference point for project timelines and scheduling. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, facilitating effective project management and tracking. |
| `Statuscode` | `string` | The 'Statuscode' column in the 'tbl_Project' table indicates the current operational status of each project, facilitating effective tracking and management of project progress. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with each project, stored as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table provides a comprehensive overview of employees who have not submitted their timesheets, enabling managers to identify and address compliance issues efficiently. It includes key details such as employee and manager information, contract dates, and the specific time periods affected, facilitating timely follow-up and ensuring accurate payroll processing.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by employees that have not been recorded in their timesheets, facilitating accurate tracking and reporting of labor hours. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is set to expire, providing critical information for managing workforce availability and compliance within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours agreed upon in the contract for each employee, facilitating the identification of discrepancies in timesheet submissions within the 'vw_missing_timesheet' table. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and ensuring compliance with contractual obligations. |
| `ContractStatusToday` | `string` | Column Description: Indicates the current status of the contract associated with the timesheet, providing essential context for identifying missing entries. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_missing_timesheet' table uniquely identifies each employee whose timesheet data is missing, facilitating targeted data enrichment and analysis. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on outstanding time reporting issues. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours recorded for each employee's timesheet entry, facilitating the identification of discrepancies in time reporting. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data updates. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_missing_timesheet' table identifies the unique identifier of the manager responsible for overseeing the employee associated with the missing timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees who have missing timesheets, facilitating accountability and follow-up actions. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee work hour reporting. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the missing timesheet records, facilitating the tracking and management of time allocation across various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project, facilitating the tracking and management of timesheet entries associated with specific projects in the 'vw_missing_timesheet' table. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true signifying rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet data is complete and ready for reporting purposes. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of tracking and analyzing missing timesheet entries for accurate financial reporting. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double to accommodate precise pricing information within the context of the 'vw_missing_timesheet' table. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date and time when a timesheet entry is missing, facilitating timely follow-up and data enrichment processes. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific organizational subunit associated with each entry in the missing timesheet records, facilitating targeted follow-up and resolution of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains unique identifiers for timesheets, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' view. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the tasks and activities recorded in the timesheet, facilitating better understanding and analysis of employee work hours within the 'vw_missing_timesheet' table. |
| `Week_` | `int64` | The 'Week_' column (int64) in the 'vw_missing_timesheet' table represents the week number of the year associated with each timesheet entry, facilitating the identification of missing submissions by week. |
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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `BillableDep`. Let's break down what it does in simple business terms:

1. **Purpose**: The goal of this calculation is to determine whether a certain unit is considered "billable" based on a related table called `lkp_Unit`.

2. **Logic**:
   - The code first checks if the `BillableDep` value from the `lkp_Unit` table is blank (i.e., there is no value present).
   - If it is blank, the calculated column will return a value of **1**. This indicates that the unit is considered billable when there is no specific billable dependency defined.
   - If the `BillableDep` value is not blank, the code then checks if this value is not equal to **0**.
     - If the `BillableDep` is not zero, it returns **1**, meaning the unit is billable.
     - If the `BillableDep` is zero, it returns **0**, indicating that the unit is not billable.

3. **Outcome**: 
   - The final result of this calculation will be either **1** or **0** for each row in the table where this calculated column is applied. A value of **1** signifies that the unit is billable, while a value of **0** indicates it is not.

In summary, this DAX expression effectively categorizes units as billable or not based on the presence and value of the `BillableDep` field from the related `lkp_Unit` table.

**`CC_ActiveEmployees`** (`string`)

- **Description:** Column Description: This column indicates the status of active employees, represented as a string, to assist in identifying those who may be missing timesheet submissions within the 'vw_missing_timesheet' table.
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

2. **Conditions**:
   - The first condition checks if the `ShortDate` (the date being evaluated) is on or after the employee's `ContractStartDate`. This means the employee has started working.
   - The second condition checks if the `ShortDate` is on or before the `ContractEndDate` (the date when the employee's contract ends). However, if the `ContractEndDate` is blank (meaning the employee's contract is still ongoing or indefinite), this condition is also satisfied.

3. **Output**:
   - If both conditions are true (the employee's contract is active on the `ShortDate`), the expression returns `1`, indicating that the employee is active on that date.
   - If either condition is false (the employee's contract has not started or has ended), it returns `0`, indicating that the employee is not active on that date.

### Summary:
In summary, this DAX expression effectively flags whether an employee is active on a given date based on their contract start and end dates. A value of `1` means the employee is active, while a value of `0` means they are not. This can be useful for reporting and analysis related to employee availability and timesheet submissions.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to help identify and address missing or insufficient data in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `IncompleteFlag` in a data model, specifically in a table named `vw_missing_timesheet`. 

Here's a breakdown of what this expression does in simple business terms:

- **Purpose**: The `IncompleteFlag` column is designed to indicate whether there are any missing hours for a timesheet entry.

- **Logic**: The expression checks the value in the `MissingHours` column for each row in the `vw_missing_timesheet` table.
  
- **Condition**: 
  - If the value in `MissingHours` is **less than 0** (which implies that there is an error or an unexpected situation, as missing hours should not be negative), the expression returns **1**. This indicates that the timesheet entry is considered "incomplete" or problematic.
  - If the value in `MissingHours` is **0 or greater**, the expression returns **0**, indicating that the timesheet entry is complete or valid.

- **Outcome**: The result is a new column (`IncompleteFlag`) that flags each timesheet entry as either incomplete (1) or complete (0) based on the condition checked. This can help in identifying which entries need further attention or correction.

In summary, this DAX expression helps to quickly identify and flag timesheet entries that have an issue with missing hours, allowing for better management and oversight of timesheet accuracy.

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

1. **Check for Related Data**: The code first checks if there is a related value in the `lkp_Unit` table for the current row. Specifically, it looks for the `Unit` field in that related table.

2. **Handle Missing Data**: 
   - If the related `Unit` value is **blank** (meaning there is no corresponding entry in the `lkp_Unit` table), the code will return the text **"Unknown"**. This is a way to handle situations where the expected data is missing, ensuring that the output is clear and informative.
   - If there is a valid related `Unit` value (i.e., it is not blank), the code will return that value.

3. **Purpose**: The overall purpose of this calculated column is to provide a clear and user-friendly representation of the unit associated with each record. If the unit is not available, it explicitly states "Unknown" instead of leaving it blank, which helps in data analysis and reporting by making it clear when data is missing.

In summary, this DAX expression ensures that every row in the `MAIN_UNIT` column either shows the related unit name or indicates that the unit is unknown, improving the clarity and usability of the data.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column (string) in the 'vw_missing_timesheet' table represents the specific year and week number for each entry, facilitating the identification and tracking of timesheet submissions by week.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'YearWeek' in a data model, and it combines two pieces of information: the year and the week number of a specific date, which is represented by the column `[ContractEndDate]`.

Here's a breakdown of what the code does:

1. **YEAR([ContractEndDate])**: This part extracts the year from the date in the `[ContractEndDate]` column. For example, if the date is December 15, 2023, this function will return `2023`.

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the date in `[ContractEndDate]`. For instance, if the date is December 15, 2023, it might return `50`, indicating that this date falls in the 50th week of the year.

3. **FORMAT(..., "00")**: This part ensures that the week number is always displayed as a two-digit number. So, if the week number is `5`, it will be formatted as `05`.

4. **Concatenation**: The `&` operator is used to combine the year and the formatted week number into a single string. For example, if the year is `2023` and the week number is `50`, the final result will be `2023-50`.

### What It Achieves:
The overall result of this DAX expression is a string that represents the year and the week number of the contract's end date in a clear and structured format (like `2023-50`). This can be useful for reporting and analysis, allowing users to easily group or filter data by year and week, making it simpler to track trends or performance over time.

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

1. **Purpose**: This measure counts the number of unique employees (consultants) who have recorded negative hours in their timesheets.

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows you to apply filters to your calculations.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the number of unique employee names in the `vw_missing_timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This filter condition specifies that only the records where the `MissingHours` are less than zero should be considered. In other words, it focuses on cases where employees have negative hours logged, which might indicate issues like over-reporting or errors in timesheet submissions.

3. **Outcome**: The measure ultimately provides a count of how many different employees have negative hours recorded in their timesheets. This information can be useful for identifying potential problems in time reporting or for addressing discrepancies in employee work hours.

In summary, `Count_Negative_Consultant` helps businesses track and manage timesheet accuracy by identifying how many unique consultants have logged negative hours, which could signal a need for further investigation or correction.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called **CountNegativeMissingHours**. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure counts the number of unique employees who have recorded negative missing hours in a timesheet.

2. **Components**:
   - **CALCULATE**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **DISTINCTCOUNT**: This function counts the number of unique values in a specified column. Here, it counts unique Employee IDs.
   - **'vw_missing_timesheet'[EmployeeID]**: This refers to the column in the data that contains the IDs of employees.
   - **'vw_missing_timesheet'[MissingHours] < 0**: This is a filter condition that only includes records where the MissingHours value is less than zero, meaning it looks for instances where employees have negative hours recorded.

3. **What it Achieves**: The measure calculates how many different employees have a record of negative missing hours. This could indicate issues such as data entry errors, where hours that should be accounted for are incorrectly recorded as negative, or it could highlight employees who are consistently missing hours that need to be addressed.

In summary, **CountNegativeMissingHours** helps the business identify and quantify the number of employees with problematic timesheet entries related to missing hours, allowing for better management and resolution of these discrepancies.

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

2. **Summarization**: The code first creates a summarized table that groups data by employee name, year, week, unit, and subunit. It also calculates two key pieces of information for each group:
   - **MissingHours**: This is calculated by taking the total hours worked (from `vw_missing_timesheet[Hours_]`) and subtracting the maximum contractual hours (from `vw_missing_timesheet[ContractHours]`). If this value is negative, it indicates that the employee has not logged enough hours compared to what they are contracted to work.
   - **MinActiveEmp**: This captures the minimum value of active employees (from `vw_missing_timesheet[CC_ActiveEmployees]`) for the grouped data. A value of 1 indicates that at least one employee in that group is active.

3. **Filtering**: After summarizing the data, the code filters this summarized table to find only those groups where:
   - There is at least one active employee (`MinActiveEmp = 1`).
   - The calculated missing hours are negative (`MissingHours < 0`), meaning the employee has not logged enough hours.

4. **Counting**: Finally, the measure counts the number of rows in the filtered table, which corresponds to the number of active employees who have missing timesheet hours.

### Summary:
In essence, this DAX measure calculates how many active employees have not logged enough hours in their timesheets for a given year and week, helping the business identify potential issues with timesheet compliance.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is designed to calculate a measure called `Dax_MissingHours`, which focuses on identifying and summing up the negative missing hours for active employees in a specific context (like a week or year). Here’s a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. This variable filters a summarized table (`vw_missing_timesheet`) to focus on employees who are currently active. It does this by checking a specific condition: it only includes employees where the minimum count of active employees (`MinActiveEmp`) is equal to 1. This means that only those employees who are considered active are included in the calculation.

2. **Calculate Missing Hours**: For each employee, the code calculates "MissingHours" by taking the total hours recorded (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contract hours they are supposed to work (`MAX(vw_missing_timesheet[ContractHours])`). If this calculation results in a negative number, it indicates that the employee has worked fewer hours than expected.

3. **Filter for Negative Missing Hours**: The filtering condition also ensures that only those employees with negative missing hours (i.e., they have worked less than their contracted hours) are included in the final calculation.

4. **Sum Up Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered list of active employees and sums up their "MissingHours." This gives a total of all the negative hours across all active employees, effectively showing how many hours are missing or unaccounted for in total.

In summary, this DAX measure calculates the total number of hours that active employees are missing based on their contracted hours, focusing specifically on those who have worked less than expected. This can help a business identify potential issues with employee attendance or workload management.

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

1. **Data Source**: The calculation is based on a data table called `vw_missing_timesheet`, which likely contains information about employees, their contract hours, and the weeks they worked.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key pieces of information:
   - **Week**: This represents the week of the year.
   - **EmployeeID**: This identifies each employee.

3. **Calculating Maximum Contract Hours**: For each unique combination of week and employee, the code calculates the maximum contract hours from the `ContractHours` column. This means that if an employee has multiple entries for the same week, it will only take the highest number of contract hours recorded for that week.

4. **Summing Up Contract Hours**: The outer `SUMX` function then takes the results from the `SUMMARIZE` step and sums up all the maximum contract hours calculated for each employee across all weeks. 

### What It Achieves:
- The final result of this measure is the total expected contract hours for all employees, ensuring that if an employee has multiple records for a week, only the highest contract hours are counted. This helps in understanding the total expected workload for employees on a weekly basis, which can be useful for planning, resource allocation, and performance tracking.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the difference between two values related to hours worked and contracted hours for employees or contractors. Here’s a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum number of hours that are contracted for a specific employee or contractor from the `vw_missing_timesheet` table. Essentially, it tells us the total hours that the employee is supposed to work according to their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum number of hours that the employee or contractor has actually recorded or worked, as noted in the same table.

3. **Calculation**: The expression then subtracts the maximum recorded hours (actual hours worked) from the maximum contracted hours. 

### What It Achieves:
The result of this calculation gives you the number of hours that are "missing" or unaccounted for. In other words, it shows how many hours the employee or contractor has not worked compared to what they are contracted to work. 

### Business Implication:
This measure can help management identify gaps in hours worked versus hours expected, which can be crucial for understanding productivity, ensuring compliance with contracts, and managing workforce resources effectively. If the result is a positive number, it indicates that the employee has not worked all their contracted hours, which may require follow-up or adjustments.

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

1. **SUM(vw_missing_timesheet[MissingHours])**: This part of the code adds up all the missing hours from a table called `vw_missing_timesheet`. Essentially, it tells us how many hours were not accounted for in timesheets.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the expected number of hours that should be worked in a week according to the contract. It serves as a benchmark for comparison.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the code calculates the difference between the total missing hours and the expected contract hours. If the result is positive, it indicates that the missing hours exceed what was expected; if negative, it means that the hours worked are more than expected.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The `DIVIDE` function takes the result from the previous step (the difference between missing hours and expected hours) and divides it by the expected contract hours. This gives us a ratio that represents how much the actual hours deviate from what was expected. The third argument, `0`, ensures that if the expected hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

### What It Achieves:
The overall measure, `PercentageCompleteness`, calculates how the actual hours worked (considering missing hours) compare to the expected hours. 

- If the result is positive, it indicates a percentage of how much the actual hours fall short of expectations.
- If the result is negative, it shows how much the actual hours exceed expectations.

In summary, this measure helps the business understand the completeness of timesheet submissions relative to what is expected, allowing for better management of workforce hours and identifying potential issues with timesheet compliance.

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
   - `EmployeeName`: This identifies each employee.
   - `MAIN_UNIT`: This likely refers to the department or unit within the organization.

   For each unique combination of these three columns, the code creates a new summary table.

3. **Creating a Flag**: Within this summary table, a new column called `"IncompleteFlag"` is created. This column uses the `MAX` function to determine the highest value of `IncompleteFlag` for each group. The `IncompleteFlag` is probably a binary indicator (0 or 1) that shows whether an employee has submitted their timesheet completely (0) or not (1). By using `MAX`, the measure effectively checks if there is at least one instance of an incomplete timesheet for that employee in that week and unit.

4. **Calculating the Total**: The outer `SUMX` function then takes this summarized data and sums up the values in the `IncompleteFlag` column. This means it adds up all the flags for each unique employee in each unit for the specified weeks.

### What It Achieves:
In summary, this DAX measure calculates the total number of unique employees who have incomplete timesheets for each unit during the specified weeks. It helps the organization understand how many employees are not meeting their timesheet submission requirements, broken down by unit and week. This information can be crucial for identifying areas where additional support or reminders may be needed to ensure compliance with timesheet submissions.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This table enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column in the 'vw_Timesheet' table identifies the specific client associated with each timesheet entry, facilitating accurate billing and project tracking. |
| `Code2` | `string` | The 'Code2' column in the 'vw_Timesheet' table stores a secondary identifier code that categorizes timesheet entries for enhanced data analysis and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when a contract associated with a timesheet concludes, providing essential information for contract management and resource planning. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when a contract becomes effective, providing essential context for timesheet entries related to specific contractual agreements. |
| `ContractStatusToday` | `string` | Column Description: Indicates the current status of the contract associated with the timesheet entry, represented as a string value. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, providing essential insights for performance tracking and decision-making within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the total monetary cost associated with the recorded time entries in the 'vw_Timesheet' table, expressed as a double-precision floating-point number for precise financial calculations. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, expressed as a double to accommodate precise financial calculations. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions recorded in the timesheet, facilitating accurate cost analysis and reporting. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the individual or entity responsible for payment, facilitating the tracking and management of outstanding invoices within the timesheet records. |
| `EmployeeID` | `string` | Column Description: A unique identifier for each employee, represented as a string, used to associate timesheet entries with the corresponding employee in the vw_Timesheet table. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column stores the unique identifier and name of each employee, facilitating the association of timesheet entries with the corresponding personnel in the vw_Timesheet table. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating the identification and tracking of individual work hours and contributions. |
| `Employer` | `string` | The 'Employer' column in the 'vw_Timesheet' table captures the name of the organization for which the employee is working, facilitating the tracking and reporting of labor costs associated with each employer. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating accurate payroll processing and reporting. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to each timesheet entry, providing insights that enhance the understanding of the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with each timesheet entry, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, facilitating accurate tracking of labor input for payroll and project management. |
| `HoursperWeek` | `int64` | Column Description: The 'HoursperWeek' column represents the total number of hours worked by an employee in a given week, stored as an integer value, to facilitate time tracking and resource allocation within the organization. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee time allocation. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee work hours. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry timelines. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing project-related timesheet entries, facilitating accountability and resource management. |
| `IPMIDName` | `string` | The 'IPMIDName' column in the 'vw_Timesheet' table stores the names of individual IPMIDs (Integrated Project Management Identifiers) associated with each timesheet entry, facilitating project tracking and resource allocation. |
| `JobTitle` | `string` | The 'JobTitle' column in the 'vw_Timesheet' table captures the specific role or position of an employee, providing essential context for analyzing timesheet data and resource allocation. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking timesheet records with related datasets, facilitating efficient data integration and analysis. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and reporting within the organization. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, facilitating easy identification of managerial oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key associated with the manager responsible for overseeing the timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct supervisor, facilitating the identification of managerial oversight for timesheet entries. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries, detailing the hours allocated to specific tasks within the timesheet records. |
| `Project` | `string` | The 'Project' column in the 'vw_Timesheet' table identifies the specific project associated with each recorded time entry, facilitating accurate tracking and reporting of resource allocation and productivity. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project, enabling efficient tracking and reporting of time spent on various projects within the timesheet records. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that identifies the specific project associated with each timesheet entry, providing essential context for resource allocation and project tracking. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the specific project profile associated with each timesheet entry, facilitating accurate project tracking and reporting. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, enabling better analysis of resource allocation and project performance. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true signifying rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet is ready for reporting, with a value of true signifying readiness and false indicating further action is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double to accommodate precise financial calculations within the context of timesheet data analysis. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column in the 'vw_Timesheet' table indicates the current state of each timesheet entry, reflecting whether it is submitted, approved, or pending review. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, enabling better resource allocation and management within the project. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for each timesheet entry, facilitating the tracking and management of employee work hours and related activities. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the tasks and activities recorded in the timesheet, enhancing clarity and context for time tracking and project management. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table specifies the measurement unit associated with the recorded time entries, facilitating accurate time tracking and reporting. |
| `UnitCode` | `string` | The 'UnitCode' column in the 'vw_Timesheet' table represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating accurate tracking and reporting of labor costs by department. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and data enrichment within the timesheet management system. |
| `YearWeek` | `string` | The 'YearWeek' column (string) in the 'vw_Timesheet' table represents the specific year and week number, facilitating time-based analysis and reporting of timesheet data. |

##### Calculated Columns

**`Approved_With_V_Z`** (`string`)

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheets, specifying whether they have been approved with a particular validation or verification process.
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

2. **Conditions**:
   - The first condition checks if the `Approved` column is marked as `TRUE`. This means that if the timesheet has been officially approved, it meets the criteria.
   - The second condition checks if the `TimesheetCode` for that entry is either "V" or "Z". These codes likely represent specific types of timesheets that are automatically considered approved regardless of the `Approved` status.

3. **Output**:
   - If either of the conditions is met (i.e., the timesheet is approved or has a code of "V" or "Z"), the formula returns a value of `1`. This indicates that the timesheet is approved.
   - If neither condition is met, it returns a value of `0`, indicating that the timesheet is not approved.

In summary, this DAX expression effectively flags timesheet entries as approved (1) or not approved (0) based on whether they are explicitly approved or fall under specific codes (V or Z). This can help in reporting and analysis by easily identifying which timesheets are considered approved.

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column identifies the department associated with billable hours recorded in the timesheet, facilitating accurate project costing and resource allocation.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Let's break down what it does in simple business terms:

1. **Purpose**: The calculated column `BillableDep` is designed to determine whether a certain unit (likely a project, task, or resource) is considered "billable" based on a related table called `lkp_Unit`.

2. **Logic**:
   - The expression first checks if the `BillableDep` value from the `lkp_Unit` table is blank (i.e., there is no associated value).
   - If it is blank, the calculated column will return a value of **1**. This indicates that if there is no specific billable dependency defined, the unit is treated as billable by default.
   - If the `BillableDep` value is not blank, the expression then checks if this value is not equal to **0**.
     - If the `BillableDep` is not zero, it returns **1**, indicating that the unit is billable.
     - If the `BillableDep` is zero, it returns **0**, indicating that the unit is not billable.

3. **Outcome**: 
   - The final result of this calculated column will be either **1** (billable) or **0** (not billable) based on the conditions checked. 
   - Essentially, this column helps in identifying which units can be billed for services or work, based on their relationship to the `lkp_Unit` table.

In summary, the `BillableDep` column helps to classify units as billable or not, based on whether they have a defined billable dependency and whether that dependency is non-zero.

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

1. **Purpose**: The `BillablePrj` column is designed to identify whether a particular timesheet entry is considered "billable" or not. A billable entry is one that can be charged to a client or customer.

2. **Conditions for Billability**: The expression checks three specific conditions to determine if an entry is billable:
   - **Sales Price Greater Than Zero**: If the `SalesPrice` for the entry is greater than 0, it indicates that there is a charge associated with this entry, making it billable.
   - **Specific Project Profile Code**: If the `ProjectProfileCode` is equal to 81, this suggests that this particular project is predefined as billable, regardless of other factors.
   - **Project Name Contains "Customer Success Services"**: If the `Project` name includes the phrase "Customer Success Services," it indicates that this project is also considered billable.

3. **Output**: 
   - If any of the above conditions are met, the expression returns a value of `1`, indicating that the entry is billable.
   - If none of the conditions are met, it returns `0`, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project name. This helps in tracking which work can be charged to clients, aiding in financial reporting and project management.

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
- **DAX Explanation (Generated):** This DAX code snippet creates a new calculated column called `Employee_WeeklyHours` in a data model, specifically from a table named `vw_Timesheet`. 

Here's a breakdown of what it does in simple business terms:

1. **Combining Information**: The code takes two pieces of information from the `vw_Timesheet` table:
   - The name of the employee (`EmployeeName`).
   - The number of hours that employee works in a week (`HoursperWeek`).

2. **Formatting the Output**: It combines these two pieces of information into a single string. The format of the output will be:
   - The employee's name followed by a hyphen and then the number of hours they work per week. For example, if the employee's name is "John Doe" and he works 40 hours a week, the output will be "John Doe - 40".

3. **Purpose**: This calculated column is useful for creating a clear and concise representation of each employee's weekly working hours alongside their name. It can be helpful for reporting, analysis, or displaying information in dashboards where you want to quickly see both the employee's name and their weekly hours in one place.

In summary, this DAX expression effectively creates a user-friendly label that combines employee names with their corresponding weekly hours, making it easier to read and understand the data at a glance.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'GroupCat' in a data model, likely for a timesheet or project management context. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The 'GroupCat' column categorizes projects based on whether they are linked to a specific group or if they are billable or unbillable.

2. **Lookup for Group**: 
   - The code first checks if there is a corresponding group for the project listed in the `vw_Timesheet` table by looking it up in the `lkp_Project` table.
   - It uses the `LOOKUPVALUE` function to find the group associated with the project. If a group is found (i.e., it is not blank), it retrieves that group name.

3. **Condition Check**:
   - If a group is found (the result of the lookup is not blank), the 'GroupCat' column will take the value of that group.
   - If no group is found (the result is blank), it moves to the next condition.

4. **Billable vs. Unbillable**:
   - If no group is found, the code checks if the project is billable by looking at the `BillablePrj` column in the `vw_Timesheet` table.
   - If `BillablePrj` equals 1 (indicating that the project is billable), it assigns the value "Billable" to the 'GroupCat' column.
   - If `BillablePrj` does not equal 1 (indicating that the project is unbillable), it assigns the value "Other Unbillable" to the 'GroupCat' column.

### Summary:
In summary, this DAX expression categorizes each project in the timesheet as follows:
- If the project is linked to a specific group, it assigns that group name.
- If not linked to a group, it checks if the project is billable and assigns "Billable" if it is, or "Other Unbillable" if it isn't. 

This helps in organizing and analyzing projects based on their billing status and group association, which can be crucial for financial reporting and project management.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called **IsContractActive** in a data model, specifically for a table named **vw_Timesheet**. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column determines whether a contract is currently active or not.

2. **Conditions Checked**:
   - **ISBLANK('vw_Timesheet'[ContractEndDate])**: This part checks if the **ContractEndDate** field is empty (i.e., there is no end date specified for the contract). If there is no end date, it implies that the contract is ongoing or has not been finalized.
   - **'vw_Timesheet'[ContractEndDate] > TODAY()**: This part checks if the **ContractEndDate** is later than today's date. If the end date is in the future, it means the contract is still active.

3. **Results**:
   - If either of the above conditions is true (meaning the contract has no end date or the end date is still in the future), the calculated column will return **"Active"**.
   - If neither condition is true (meaning there is an end date that has already passed), it will return **"Not Active"**.

### Summary:
In summary, this DAX expression effectively labels each contract in the **vw_Timesheet** table as either **"Active"** or **"Not Active"** based on whether the contract has an end date that is either missing or still in the future. This helps users quickly identify which contracts are currently valid and ongoing.

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

1. **Check for Related Data**: The code first checks if there is a related value in the 'Unit' column of the 'lkp_Unit' table. This is done using the `RELATED` function, which retrieves a value from a related table based on the current row's context.

2. **Handle Missing Data**: The `ISBLANK` function checks if the value retrieved from the 'lkp_Unit[Unit]' column is blank (i.e., there is no related unit information available).

3. **Return Values Based on the Check**:
   - If the related 'Unit' value is blank (meaning there is no corresponding unit found), the code returns the string "Unknown". This helps to clearly indicate that the unit information is missing.
   - If there is a valid related 'Unit' value, it simply returns that value.

### Summary:
In summary, this DAX expression is designed to populate the 'MAIN_UNIT' column with either the actual unit name from the related table or "Unknown" if no unit information is available. This ensures that users can easily identify when unit data is missing, improving data clarity and usability.

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
  - and so on, up to
  - 12, which represents December.

### Business Purpose:
- **Simplifies Analysis**: By converting dates into month numbers, it allows for easier grouping and analysis of data by month. For example, you can quickly summarize timesheet data to see how many hours were logged in each month.
- **Facilitates Reporting**: This calculated column can be used in reports and dashboards to visualize trends over time, such as monthly performance or workload.

In summary, this DAX expression helps businesses analyze and report on their data by breaking down dates into their respective month numbers, making it easier to track and compare performance across different months.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the unique employee identification numbers as strings, facilitating the association of timesheet entries with specific employees in the vw_Timesheet view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

### What it Achieves:

1. **Counting Unique Employees**: The primary purpose of this expression is to count how many different employees are present in the timesheet data. For example, if the timesheet includes entries for the same employee multiple times, this function will only count that employee once.

2. **Data Analysis**: This calculation is useful for understanding workforce metrics, such as how many distinct employees worked during a specific period or on a specific project. It helps in analyzing employee participation and resource allocation.

3. **Avoiding Duplicates**: By using `DISTINCTCOUNT`, the expression ensures that duplicate entries (e.g., an employee appearing multiple times in the timesheet) do not inflate the count. This provides a more accurate representation of the workforce.

In summary, this DAX expression helps businesses track and analyze the number of unique employees involved in activities recorded in the timesheet, which can be critical for reporting, resource management, and operational insights.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table captures the type of ownership or classification for time entries, indicating whether the hours logged are attributed to internal staff, subcontractors, or external resources.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used in a calculated column to retrieve a specific value from a related table in a data model.

### Explanation in Business Terms:

1. **Context of Use**: This expression is typically used in a situation where you have two tables that are connected by a relationship. For example, you might have a main table that contains sales data and a related table that contains information about different units or departments.

2. **What It Does**: The `RELATED` function looks up a value from a related table. In this case, it is fetching the value from the column `OWN-Sub-ExtT` in the `lkp_Unit` table.

3. **Purpose**: By using this expression, you can enrich your main table with additional information from the `lkp_Unit` table. For instance, if `OWN-Sub-ExtT` contains details about ownership or categorization of units, this expression allows you to pull that information into your main table for analysis.

4. **Outcome**: The result is that each row in your main table will now include the corresponding value from the `OWN-Sub-ExtT` column based on the relationship defined between the two tables. This can help in reporting, filtering, or further calculations that depend on this additional context.

In summary, this DAX expression enhances your data by linking related information, making it easier to analyze and derive insights from your dataset.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column contains string values that categorize or qualify the projects associated with each timesheet entry, facilitating better project management and reporting.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `QualifyPrj`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column is designed to determine a qualification status for projects based on related data from another table called `lkp_Project`.

2. **Logic**:
   - The expression checks if the `Qualify` field from the `lkp_Project` table (which is related to the current table) is blank (i.e., has no value).
   - If the `Qualify` field is blank, the calculated column will return a value of `1`. This could signify a default or a specific status indicating that the project does not have a qualification assigned.
   - If the `Qualify` field is not blank (meaning it has a value), the calculated column will return the actual value from the `Qualify` field in the `lkp_Project` table.

3. **Outcome**: The result of this calculation provides a way to ensure that every project has a qualification status. If no qualification is specified, it defaults to `1`, allowing for consistent data handling and analysis.

In summary, this DAX expression helps to fill in gaps in project qualification data by providing a default value when no qualification is available, ensuring that every project can be evaluated or categorized effectively.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor allocation.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of timesheet entries, specifically denoting whether they have been validated and are ready for further processing or reporting.
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

1. **Purpose**: The calculated column is designed to determine whether a certain condition is met for each row in the dataset. Specifically, it checks if a report is ready or if a specific timesheet code is used.

2. **Conditions Checked**:
   - The first condition checks if the column `[ReportReady]` is marked as `TRUE()`. This means that the report is ready for that particular entry.
   - The second condition checks if the value in the `vw_Timesheet[TimesheetCode]` column is either "V" or "Z". These are specific codes that might represent certain types of timesheets.

3. **Output**:
   - If either of the conditions is met (i.e., the report is ready or the timesheet code is "V" or "Z"), the calculated column will return a value of `1`.
   - If neither condition is met, it will return a value of `0`.

4. **Business Implication**: This calculated column can be used to easily identify which entries are either ready for reporting or fall under specific categories (V or Z). This can help in filtering, analyzing, or summarizing data based on readiness or specific timesheet codes, making it easier for decision-makers to focus on relevant entries.

In summary, `RReady_With_V_Z` is a flag that indicates whether a report is ready or if a timesheet falls under certain codes, helping to streamline data analysis and reporting processes.

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

1. **Condition Check**: The measure first checks the value of another measure called `ContractStatusMeasure`. It specifically looks to see if this measure equals "Active".

2. **Return Value Based on Condition**:
   - If `ContractStatusMeasure` is "Active", the measure will return the value of another measure called `HoursDifference`. This likely represents the number of hours related to an active contract.
   - If `ContractStatusMeasure` is not "Active", the measure will return the text "Not Active".

### Summary:
In essence, `ContractStatus2` is used to determine the status of a contract. If the contract is active, it provides the number of hours associated with that contract. If the contract is not active, it simply indicates that by returning "Not Active". This measure helps users quickly understand the status of contracts and their associated hours in a report or dashboard.

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

1. **Check for Contract End Date**: The measure first looks at the maximum value of the `ContractEndDate` from the `vw_Timesheet` table. This means it is checking the latest end date of all contracts associated with the timesheet records.

2. **Evaluate Conditions**:
   - **Is the End Date Blank?**: The measure checks if this maximum end date is blank (i.e., there is no end date recorded for the contract).
   - **Is the End Date in the Future?**: It also checks if the maximum end date is greater than today’s date (meaning the contract is still valid and has not yet ended).

3. **Determine Status**:
   - If either of the above conditions is true (the end date is blank or it is in the future), the measure returns "Active". This indicates that the contract is currently valid and ongoing.
   - If neither condition is true (the end date is not blank and it is in the past), the measure returns "Not Active". This indicates that the contract has ended.

### Summary:
In summary, the `ContractStatusMeasure` calculates whether a contract is currently active or not based on its end date. If there is no end date or if the end date is still in the future, it labels the contract as "Active". If the end date has passed, it labels it as "Not Active". This measure helps businesses quickly assess the status of contracts in relation to their timelines.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 15, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated based on the first week of the year, which can vary depending on the system used (e.g., whether the week starts on Sunday or Monday).

So, when you combine these two functions, `WEEKNUM(TODAY())` calculates which week of the year it currently is. 

### In Business Terms:
This measure helps businesses understand which week of the year they are currently in. This can be useful for reporting, planning, and analyzing performance on a weekly basis. For example, if a company wants to track sales or activities week by week, knowing the current week number allows them to compare performance against previous weeks or set targets for the current week.

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
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. It ensures that each employee is only counted once, even if they have multiple timesheet entries.
   - **`vw_Timesheet[Approved] = FALSE()`**: This filter condition specifies that we only want to consider timesheets that are marked as not approved (i.e., where the `Approved` field is set to FALSE).

3. **Outcome**: The measure ultimately provides a count of how many distinct employees have submitted timesheets that have not yet been approved. This information can be useful for tracking pending approvals and managing workflow within the organization.

In summary, `Dax_EmpCount_Approved` helps businesses understand how many employees are waiting for their timesheets to be approved, which can aid in resource management and operational efficiency.

**`Dax_EmpCount_RReady`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[ReportReady] = FALSE()
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `Dax_EmpCount_RReady`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the number of unique employees whose timesheet reports are not ready.

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows us to apply filters to the data before performing calculations.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it tells us how many different employees are represented in the data.
   - **`vw_Timesheet[ReportReady] = FALSE()`**: This filter condition specifies that we only want to consider those records where the `ReportReady` field is marked as FALSE. In other words, it filters the data to include only those timesheets that are not ready.

3. **Outcome**: The measure ultimately provides a count of distinct employees who have timesheets that are still pending or not finalized. This can be useful for management to understand how many employees still need to submit their timesheets or how many reports are outstanding.

In summary, `Dax_EmpCount_RReady` helps track the number of employees with incomplete timesheet reports, enabling better oversight and management of reporting processes.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheet entries in a dataset.

### Breakdown of the Expression:

1. **DISTINCTCOUNT**: This function counts the number of distinct (unique) values in a specified column. In this case, it is counting unique entries.

2. **vw_Timesheet[EmployeeName]**: This part refers to the column named `EmployeeName` in the `vw_Timesheet` table. This column contains the names of employees who have submitted timesheets.

### What It Achieves:

- **Unique Employee Count**: The measure calculates how many different employees have logged their hours in the timesheet. For example, if three employees submitted timesheets, the result would be 3, regardless of how many times each employee submitted their timesheet.

### Business Implications:

- **Workforce Analysis**: This measure helps businesses understand how many individual employees are actively participating in timesheet reporting, which can be useful for workforce management and resource allocation.
- **Engagement Tracking**: It can also indicate employee engagement with the timesheet process, helping identify if there are any issues with participation or compliance.

In summary, this DAX measure provides a clear count of unique employees contributing to timesheet data, which is valuable for operational insights and decision-making.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the `vw_Timesheet` table. Essentially, it totals the revenue generated from sales during the specified period.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the same `vw_Timesheet` table. It represents the total number of hours that employees or resources have worked during that period.

3. **Division**: The expression then divides the total sales amount by the total hours worked. This calculation gives you the average hourly rate, which tells you how much revenue is generated for each hour worked.

### In Summary:
The measure `MSR_AVG_HOURLY_RATE` calculates the average revenue earned per hour worked by dividing total sales by total hours. This metric helps businesses understand their efficiency and productivity in generating sales relative to the time invested.

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
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called `Msr_Budget`, which is designed to calculate a budget figure based on sales data, specifically focusing on the sales price in a base currency. Here’s a breakdown of what this measure does in simple business terms:

1. **Identify the Current Week**: 
   - The measure starts by determining the current week of the year using the `SELECTEDVALUE` function on the `DimDate[WeekNumberOfYear]` column. This means it looks at the context of the report or dashboard to find out which week is currently being analyzed.

2. **Calculate Previous Budget**:
   - Next, it calculates a value called `PrevBudget`. This is done using the `CALCULATE` function, which allows for modifying the context in which a measure is evaluated. 
   - Inside `CALCULATE`, it uses the measure `[Msr_SalesPriceBaseCurrency]`, which presumably represents the sales price in the base currency.
   - The `FILTER` function is applied to the `DimDate` table to include only those weeks that are less than the current week. This means it sums up or aggregates the sales price for all previous weeks.

3. **Return the Appropriate Value**:
   - Finally, the measure uses an `IF` statement to decide what to return:
     - If the current sales price measure (`[Msr_SalesPriceBaseCurrency]`) is not blank (meaning there is data for the current week), it returns that current sales price.
     - If the current sales price is blank (indicating no data for the current week), it returns the previously calculated budget (`PrevBudget`).

### Summary:
In summary, this measure calculates the sales price for the current week if available. If there is no sales data for the current week, it falls back to the total sales price from all previous weeks. This approach ensures that the measure provides a meaningful budget figure, either reflecting current performance or leveraging historical data when current data is missing.

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
   - The code starts by determining which month is currently selected in the visual (like a chart or table). It uses the `MAX` function to find the highest month number from the 'DimDate' table. This means if you are looking at data for March, the `SelectedMonth` will be 3.

2. **Calculate Cumulative Cost**:
   - The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to and including the selected month. 

3. **Using CALCULATE and FILTER**:
   - The `CALCULATE` function is used to change the context in which the data is evaluated. It sums up the `CostAmount` from the `vw_Timesheet` table.
   - The `FILTER` function is applied to the 'DimDate' table to include all months that are less than or equal to the `SelectedMonth`. This means if March is selected, it will consider January, February, and March for the calculation.

4. **ALLSELECTED Function**:
   - The `ALLSELECTED` function ensures that any filters applied to the 'DimDate' table (like year or other date filters) are respected while still allowing the measure to consider all months up to the selected month.

### Summary:
In summary, this DAX measure calculates the total costs incurred from the start of the year up to the month that the user has selected in the report. This is useful for understanding how costs accumulate over time and helps in tracking expenses against budgets or forecasts.

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
   - It uses the `CALCULATE` function to modify the context of the calculation. This means it will sum up the sales amounts while applying specific filters.

3. **Filter for Relevant Months**:
   - The `FILTER` function is used to create a list of months that should be included in the calculation. It looks at all the months available in the 'DimDate' table (using `ALLSELECTED` to respect any other filters that might be applied) and includes only those months where the month number is less than or equal to the selected month.

4. **Summing Up Sales**:
   - Finally, it sums the sales amounts from the 'vw_Timesheet' table for all the months that meet the filter criteria (i.e., all months up to the selected month).

### In Summary:
This DAX measure calculates the total sales from the start of the year up to the month that the user has selected in the report. It helps users understand how sales are accumulating over time, allowing for better analysis of sales performance within a specific timeframe.

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
   - The measure first calculates the total hours worked (`SUM(vw_Timesheet[Hours])`) across all selected filters for `GroupCat`, `Project`, and `EmployeeName`. 
   - The `ALLSELECTED` function ensures that it considers only the filters that are currently applied in the report or dashboard, allowing for dynamic calculations based on user selections.

2. **Percentage Calculation**:
   - After calculating the total hours (stored in the variable `TotalValue`), the measure then calculates the percentage of hours worked for the current context (which could be a specific group, project, or employee).
   - It does this by dividing the sum of hours for the current context (`SUM(vw_Timesheet[Hours])`) by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safer way to handle division in DAX. If the `TotalValue` is zero (meaning no hours were recorded), it will return 0 instead of causing an error.

### Summary:
In summary, `Msr_HoursPercentage` calculates what portion of the total hours worked is attributed to a specific group, project, or employee, expressed as a percentage. This helps in understanding how much of the total effort is coming from different parts of the organization or different projects, facilitating better resource allocation and performance analysis.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the project margin for a specific project by subtracting total costs from total sales. Here’s a breakdown of what it does in simple business terms:

1. **Sales Amount**: The first part of the expression, `sum(vw_Timesheet[SalesAmount])`, adds up all the sales amounts recorded in the `vw_Timesheet` table. This represents the total revenue generated from the project.

2. **Cost Amount**: The second part, `sum(vw_Timesheet[CostAmount])`, sums up all the costs associated with the project, as recorded in the same table. This includes all expenses incurred to deliver the project.

3. **Calculating Project Margin**: The entire expression subtracts the total costs from the total sales: 
   - **Project Margin = Total Sales - Total Costs**. 

In essence, this measure calculates the profit (or margin) from the project by showing how much money is left after covering all the costs. A positive result indicates that the project is profitable, while a negative result suggests a loss. 

Overall, this measure helps businesses assess the financial performance of their projects, enabling better decision-making regarding resource allocation and project management.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate a measure called `Msr_SalesPriceBaseCurrency`. Let's break it down into simpler terms to understand what it does:

1. **Context**: The measure operates on a data table called `vw_Timesheet`, which likely contains information about projects, clients, and possibly other related data.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data from `vw_Timesheet` by two columns: `ProjectProfile` and `Client`. This means that the measure will look at each unique combination of project profiles and clients in the timesheet data.

3. **Calculating Measure Value**: For each unique combination of `ProjectProfile` and `Client`, the measure retrieves a value from another table called `tbl_Project`. Specifically, it uses the `SELECTEDVALUE` function to get the `SalesPriceBaseCurrency` for the current project. This function returns the value of `SalesPriceBaseCurrency` if there is a single value selected; otherwise, it returns blank.

4. **Summing Up Values**: After summarizing the data and calculating the `MeasureValue` for each group, the `SUMX` function iterates over this summarized table. It sums up all the `MeasureValue` entries, which represent the sales prices in the base currency for each project and client combination.

### In Summary:
The `Msr_SalesPriceBaseCurrency` measure calculates the total sales price in the base currency for all projects and clients listed in the `vw_Timesheet`. It does this by grouping the data by project and client, retrieving the relevant sales price for each group, and then summing those prices together. This measure helps businesses understand their total sales revenue in a consistent currency, which is crucial for financial reporting and analysis.

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

1. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part of the code counts the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table. Essentially, it tells us how many different employees are present in the data.

2. **CALCULATE**: This function modifies the context in which the data is evaluated. It allows us to apply filters or change the way the data is aggregated.

3. **ALL(vw_Timesheet)**: This function removes any filters that might be applied to the `vw_Timesheet` table. By using `ALL`, the measure ensures that it considers all records in the table, regardless of any other filters that might be in place in the report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This part adds a filter condition. It specifies that we only want to count employees whose contract status is marked as "Valid." This means that only employees with a valid contract will be included in the count.

### Summary:
In summary, this DAX measure calculates the total number of unique employees who have a valid contract, ignoring any other filters that might be applied to the data. It provides a clear view of how many employees are currently active and eligible based on their contract status.

**`TotalActualHours`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression `SUM(vw_Timesheet[Hours])` is used to calculate the total number of hours recorded in a timesheet. 

Here's a breakdown of what it does:

- **SUM**: This function adds up all the values in a specified column.
- **vw_Timesheet**: This refers to a table (or view) that contains timesheet data. It includes various columns related to employee hours worked, projects, dates, etc.
- **[Hours]**: This is the specific column within the `vw_Timesheet` table that contains the number of hours each employee has logged.

In simple terms, this measure totals all the hours from the `Hours` column in the `vw_Timesheet` table. The result gives you the overall number of hours worked by all employees over a specified period or under certain conditions, depending on how the measure is used in reports or dashboards. This is useful for tracking labor costs, project hours, or overall productivity.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the total contracted hours for employees based on their weekly hours. Here’s a breakdown of what it does in simple business terms:

1. **SUMX Function**: This function is used to perform a calculation over a table and then sum the results. In this case, it will iterate through a list of employees.

2. **VALUES Function**: This function retrieves a unique list of employee names from the `vw_Timesheet` table. Essentially, it creates a list of all the employees who have timesheet entries.

3. **MAX Function**: For each employee in the unique list, the `MAX` function looks at the `HoursPerWeek` column in the `vw_Timesheet` table. It finds the maximum number of hours that each employee is contracted to work in a week.

4. **Putting it Together**: The `SUMX` function then takes the maximum hours per week for each employee and adds them up. 

### What It Achieves:
The overall result of this DAX measure is the total number of contracted hours for all employees, based on the highest weekly hours recorded for each employee. This means if an employee has different entries for hours worked in various weeks, it will only count the maximum hours they are contracted for, ensuring that the measure reflects the total contracted hours rather than the total hours worked. 

In summary, this measure helps businesses understand the total contracted hours across all employees, which can be useful for resource planning, budgeting, and workforce management.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two measures: **TotalActualHours** and **TotalContractedHours**. Here's a breakdown of what this means in simple business terms:

1. **TotalActualHours**: This measure represents the total number of hours that have actually been worked or logged on a project or task. It reflects the real effort put in by employees or resources.

2. **TotalContractedHours**: This measure indicates the total number of hours that were agreed upon in a contract or plan for the project or task. It represents the expected or planned effort.

3. **The Calculation**: The expression `[TotalActualHours] - [TotalContractedHours]` subtracts the total contracted hours from the total actual hours. 

4. **What It Achieves**: The result of this calculation shows the difference between what was actually worked and what was planned. 
   - If the result is positive, it means that more hours were worked than were contracted, indicating potential overwork or additional effort beyond what was planned.
   - If the result is negative, it means that fewer hours were worked than were contracted, suggesting that the project may be behind schedule or that resources were underutilized.
   - If the result is zero, it indicates that the actual hours worked perfectly matched the contracted hours.

In summary, this measure helps businesses understand how actual work compares to planned work, which is crucial for project management, resource allocation, and financial forecasting.

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

1. **Check for Blank Values**: The measure first checks if the total sum of hours from the `vw_Timesheet` table is blank (meaning there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Zero if Blank**: If the sum of hours is blank (i.e., there are no hours tracked), the measure will return a value of **0**. This is important because it ensures that when there are no recorded hours, the measure does not show a blank value, which could be confusing in reports.

3. **Return the Sum of Hours if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the measure will return the actual total sum of hours tracked from the `vw_Timesheet` table.

### Summary:
In essence, this measure calculates the total hours tracked by summing up the hours from the timesheet. If there are no hours recorded, it returns 0 instead of a blank value. This helps in providing clear and consistent reporting, ensuring that users can easily understand the total hours tracked without encountering empty values.

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
   - If the total sum of hours is blank (meaning no hours have been tracked), the measure returns the text "Empty". This indicates that there are no hours recorded in the timesheet.
   - If there are hours recorded (the sum is not blank), it returns the actual total number of hours tracked.

### Summary:
In essence, this measure helps to identify whether any hours have been logged in the timesheet. If no hours are logged, it clearly indicates that by returning "Empty". If hours are logged, it provides the total number of hours tracked. This can be useful for reporting and ensuring that time tracking is being properly maintained.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you've provided calculates a measure called `trackedDiff`, which essentially determines the difference between the total hours tracked for a specific context (like an employee or a project) and the maximum hours tracked in a week for that same context.

Here's a breakdown of what each part does in simple business terms:

1. **[TotalHoursTracked]**: This part represents the total number of hours that have been logged or tracked. It could be the total hours worked by an employee or the total hours for a specific project.

2. **CALCULATE(...)**: This function modifies the context in which the data is evaluated. It allows us to compute a value based on specific filters or conditions.

3. **MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])**: 
   - **DISTINCT(vw_Timesheet[HoursPerWeek])**: This part creates a list of unique weekly hours logged from the timesheet data.
   - **MAXX(...)**: This function then looks at that list of unique weekly hours and finds the maximum value. In other words, it identifies the highest number of hours that were tracked in any single week for the context being evaluated (like a specific employee).

4. **ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])**: This function removes all filters from the `vw_Timesheet` table except for the filter on `EmployeeID`. This means that while calculating the maximum hours per week, it only considers the data for the specific employee, ignoring any other filters that might be applied.

### What It Achieves:
The overall calculation of `trackedDiff` gives you the difference between:
- The total hours tracked for a specific employee (or context).
- The maximum hours that employee has tracked in any single week.

In practical terms, this measure helps to identify how much the total hours tracked deviate from the highest weekly hours recorded. This can be useful for understanding workload distribution, identifying trends in hours worked, or assessing whether an employee is consistently working more or less than their peak performance in a given week.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is a measure called `trackedDiff2`. Let's break it down into simpler terms to understand what it calculates or achieves.

### Components of the Measure:

1. **[TotalHoursTracked]**: This part represents the total number of hours tracked for a specific context, likely for an employee or a project. It is the starting point of our calculation.

2. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to compute a specific value based on certain filters.

3. **MAXX Function**: This function is used to find the maximum value from a set of values. Here, it is applied to a distinct list of hours worked per week (`vw_Timesheet[HoursPerWeek]`).

4. **DISTINCT Function**: This function creates a unique list of hours worked per week from the `vw_Timesheet` table. It ensures that each hour value is only considered once.

5. **REMOVEFILTERS**: This part removes any filters that might be applied to the `HoursPerWeek` column. This means that the calculation will consider all possible hours worked per week, regardless of any existing filters.

6. **ALLEXCEPT Function**: This function keeps the filter context for `EmployeeID` while removing other filters. This means that the calculation will focus on the specific employee's data while ignoring other filters that might be applied to the `vw_Timesheet` table.

### What It Achieves:

Putting it all together, the measure `trackedDiff2` calculates the difference between:

- **Total Hours Tracked**: The total hours recorded for a specific employee or project.
- **Maximum Hours Worked Per Week**: The highest number of hours that have been recorded for that employee across all weeks, ignoring any specific week filters but keeping the context of the employee.

### In Business Terms:

This measure helps to identify how much the total hours tracked for an employee differ from their maximum recorded hours in any week. If the result is positive, it indicates that the employee has tracked more hours than their maximum weekly average, which could suggest increased workload or overtime. If it's negative, it suggests that the employee has tracked fewer hours than their maximum weekly average, which might indicate underutilization or time off. This insight can be valuable for workforce management and productivity analysis.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the total billable hours for specific projects from a timesheet data source. Here's a breakdown of what it does in simple business terms:

1. **Variable Definition**: The code starts by defining a variable called `FilteredHours`. This variable will hold a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**: The `Filter` function is used to create a subset of the `vw_Timesheet` data based on certain conditions:
   - It includes only those entries where:
     - The project qualifies as a billable project (`vw_Timesheet[QualifyPrj] = 1` and `vw_Timesheet[BillablePrj] = 1`).
     - OR the project name contains the phrase "Customer Success Services" (this is checked using the `SEARCH` function).
   - Essentially, this means that the measure is looking for hours worked on projects that are either explicitly marked as billable or are related to customer success services.

3. **Calculating Total Hours**: After filtering the data, the `RETURN` statement uses the `SUMX` function to calculate the total hours from the filtered dataset. `SUMX` goes through each entry in `FilteredHours` and sums up the `Hours` column.

### Summary:
In summary, this DAX measure calculates the total number of hours worked on projects that are either classified as billable or are specifically related to customer success services. This helps the business understand how much time is being spent on valuable projects that can generate revenue.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `UTI_TOTALHOURS`. Let's break it down in simple business terms:

1. **Purpose of the Measure**: This measure calculates the total number of hours worked on specific projects that meet a certain qualification criterion.

2. **Components of the Code**:
   - **`SUM(vw_Timesheet[Hours])`**: This part of the code adds up all the values in the `Hours` column from the `vw_Timesheet` table. Essentially, it totals the hours worked.
   - **`CALCULATE(...)`**: This function modifies the context in which the data is evaluated. It allows us to apply filters to the data before performing the calculation.
   - **`vw_Timesheet[QualifyPrj] = 1`**: This is a filter condition. It specifies that we only want to include hours from projects where the `QualifyPrj` column has a value of 1. In a business context, this likely means that only certain qualified projects are considered in the total hours calculation.

3. **What It Achieves**: The overall effect of this measure is to provide a total count of hours worked on projects that are deemed "qualified" (where `QualifyPrj` equals 1). This can help management understand how much time is being spent on important projects, aiding in resource allocation, project evaluation, and performance tracking.

In summary, `UTI_TOTALHOURS` gives a clear picture of the total hours worked on qualified projects, helping the business make informed decisions based on this data.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is a measure called `UTILIZATION_New`, which calculates the utilization rate of resources (like employees or contractors) based on their billable hours compared to their total hours worked. Here’s a breakdown of what it does in simple business terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable captures the total number of hours that are billable to clients. It represents the productive time that can be charged for services rendered.
   - `_TotalHours`: This variable captures the total number of hours worked by the resources, which includes both billable and non-billable hours.

2. **Return Logic**:
   - The measure first checks if there are any active contacts (represented by `[Msr_ActiveContacts]`). If there are no active contacts, the measure will not perform any calculations and will return a blank value.
   - If there are active contacts, it then checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the measure will not perform any calculations and will return a blank value.
   - If both conditions are met (active contacts exist and total hours are available), it calculates the utilization rate by dividing the total billable hours by the total hours worked. 
   - If the result of this division is blank (which can happen if total hours are zero), it returns 0 instead of a blank value. This ensures that the measure always provides a numeric output.

3. **Outcome**:
   - The final output of this measure is the utilization rate, which is expressed as a fraction or percentage. This rate indicates how effectively the resources are being utilized in terms of billable work compared to their total available working hours. A higher utilization rate suggests that more of the available time is being spent on billable work, which is generally a positive indicator for business performance.

In summary, `UTILIZATION_New` helps businesses understand how efficiently their resources are being used by comparing billable hours to total hours worked, while also ensuring that the measure handles cases where data might be missing.

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

