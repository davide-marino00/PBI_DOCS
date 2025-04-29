# Power BI Model & Report Documentation

*Generated on: 2025-04-29 12:17:12*

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

The Power BI data model appears to be designed for a project management or resource allocation domain, focusing on tracking and analyzing timesheet data related to various projects and units within an organization. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a strong emphasis on monitoring employee hours and identifying gaps in timesheet submissions, which is crucial for accurate project costing and resource management. The inclusion of lookup tables like 'lkp_Project', 'lkp_Unit', and 'lkp_Code' indicates that the model is structured to provide detailed insights into project performance, employee contributions, and the organizational units involved.

The relationships established between these tables facilitate comprehensive reporting and analysis. For instance, linking 'vw_Timesheet' with 'DimDate' allows for time-based analysis of hours worked, while connections to project and unit lookup tables enable users to drill down into specific projects or organizational units. The model likely supports various metrics, such as total hours worked, percentage of missing timesheets, and project-specific performance indicators, thereby equipping stakeholders with the necessary insights to optimize project execution and resource utilization.

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

The 'DimDate' table serves as a comprehensive date dimension for analytical reporting, enabling businesses to perform time-based analysis by providing essential date attributes such as day, week, month, and quarter. This table facilitates trend analysis, seasonality insights, and time-based comparisons across various business metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting within the dataset. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient data retrieval and analysis in time-based reporting and analytics. |
| `DayName` | `string` | The 'DayName' column in the 'DimDate' table stores the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | Column Description: The 'DayNumberOfMonth' column represents the numerical day of the month (ranging from 1 to 31) for each date entry in the 'DimDate' table, facilitating date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | Column Description: Represents the day of the week as an integer, where values range from 1 (Sunday) to 7 (Saturday), facilitating date-related analyses and reporting in the DimDate table. |
| `DayNumberOfYear` | `int64` | Column Description: The 'DayNumberOfYear' column (int64) represents the sequential day of the year, ranging from 1 to 365 (or 366 for leap years), facilitating date-related calculations and analyses within the DimDate table. |
| `MonthName` | `string` | The 'MonthName' column in the 'DimDate' table stores the full names of the months, facilitating easy reference and reporting for date-related analyses. |
| `MonthNumberOfYear` | `int64` | Column Description: The 'MonthNumberOfYear' column (int64) represents the numerical value of the month (1 for January through 12 for December) to facilitate time-based analysis and reporting within the DimDate table. |
| `ShortDate` | `dateTime` | Column Description: The 'ShortDate' column captures the date in a concise format, facilitating efficient date-based analysis and reporting within the DimDate table. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Indicates whether the date corresponds to the current calendar year, with values of 'Yes' or 'No' to facilitate time-based analysis and reporting.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `CurrentYearFlag`. Here's a simple explanation of what it does:

1. **Purpose**: This calculated column is designed to identify whether a date in the `DimDate` table falls within the current year or not.

2. **How It Works**:
   - The function `YEAR(DimDate[ShortDate])` extracts the year from the date in the `ShortDate` column of the `DimDate` table.
   - The function `YEAR(TODAY())` gets the current year based on today's date.
   - The `IF` statement then compares the year extracted from `DimDate[ShortDate]` with the current year:
     - If the year from `DimDate[ShortDate]` is less than or equal to the current year, it returns `1`.
     - If the year is greater than the current year, it returns `0`.

3. **Outcome**: 
   - The result is a column where each row will have a value of `1` if the date is in the current year or any previous year, and `0` if the date is in a future year. 

In summary, this calculated column helps in flagging dates that are either in the current year or earlier, making it easier to filter or analyze data based on time periods.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table captures and categorizes various metrics related to hours worked and their corresponding percentage contributions, enabling businesses to analyze workforce productivity and resource allocation effectively. This table serves as a critical tool for performance tracking and decision-making in operational management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column contains string representations of hours worked and their corresponding percentage of total hours, providing insights into employee time allocation and productivity metrics. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and their corresponding percentage contributions, facilitating data enrichment and analysis within the Hours_nd_Percentage table. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column categorizes the sequence of hours and percentage values, facilitating the analysis of time allocation and performance metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, facilitating data consistency and integrity. It includes a unique identifier for each code, a qualification metric to indicate its relevance or applicability, and a sorting parameter to organize the codes for efficient retrieval and reporting.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column in the 'lkp_Code' table stores unique string identifiers that facilitate the enrichment of data by linking it to corresponding descriptive attributes. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numerical flag to categorize data for enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table contains string values that determine the order in which records are displayed or processed, facilitating organized data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics. This table enhances data integrity and enables efficient analysis of employee-related metrics within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column stores unique identifiers for employees, facilitating data enrichment and integration within the employee lookup table. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, facilitating the enrichment of data by providing identifiable references for each individual within the employee lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, linking project identifiers with their respective qualification and billing metrics, while categorizing them into groups. This table is essential for analyzing project performance and financial outcomes, enabling informed decision-making and strategic planning within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing code associated with each project, facilitating financial tracking and reporting. |
| `Group` | `string` | The 'Group' column in the 'lkp_Project' table categorizes projects into distinct classifications to facilitate data enrichment and analysis. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the organization. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of each project, serving as a numerical identifier for categorizing projects based on their eligibility or compliance criteria. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their characteristics such as billable status and external affiliations. This table is essential for analyzing resource allocation and financial performance across different units, facilitating informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column (int64) in the 'lkp_Unit' table indicates the department responsible for billable activities, facilitating accurate tracking and reporting of revenue-generating units. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column identifies the specific organizational unit within the company, facilitating data enrichment and analysis related to departmental structures and functions. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores external type identifiers for owned sub-units, facilitating data enrichment and integration processes. |
| `Unit` | `string` | The 'Unit' column stores the names of measurement units used for quantifying various data attributes, facilitating consistent data interpretation and analysis across the dataset. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, leadership, and status. This table enables effective project management and oversight by providing insights into project categorization, progress, and resource allocation within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and performance analysis within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column in the 'tbl_Project' table captures the administrative details and oversight responsibilities associated with each project, facilitating effective management and governance. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates and changes in the project lifecycle. |
| `ClientName` | `string` | The 'ClientName' column stores the names of clients associated with each project, facilitating easy identification and management of client relationships within the project data. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating efficient data retrieval and management within the tbl_Project table. |
| `Created_on` | `dateTime` | Column Description: The 'Created_on' column records the date and time when each project entry was initiated, providing a timestamp for tracking project creation within the tbl_Project table. |
| `Currency` | `string` | The 'Currency' column specifies the type of currency used for financial transactions within the project, facilitating accurate budget management and reporting. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the exact date and time when each project entry was created, facilitating tracking and auditing of project timelines. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or competency associated with each project, facilitating targeted resource allocation and performance evaluation within the tbl_Project table. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column indicates the scheduled date and time for the completion of the project, facilitating effective project timeline management and tracking. |
| `Project` | `string` | The 'Project' column contains the name or identifier of a specific project, serving as a key reference for associating related data within the tbl_Project table. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed narrative of the project's objectives, scope, and key features, providing essential context for stakeholders and team members. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the specific category or classification of a project within the 'tbl_Project' table, facilitating organized management and reporting of related projects. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a textual summary of the project group, detailing its objectives and scope to enhance understanding and facilitate collaboration among stakeholders. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for linking project-related data across the database. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the project's execution and management within the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency amount for the sales price of a project, stored as a decimal to ensure precision in financial calculations. |
| `Startdate` | `dateTime` | The 'Startdate' column in the 'tbl_Project' table captures the date and time when a project is initiated, serving as a key reference point for project timelines and scheduling. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, helping stakeholders assess project health and prioritize resources effectively. |
| `Statuscode` | `string` | The 'Statuscode' column in the 'tbl_Project' table indicates the current operational status of each project, facilitating effective tracking and management of project progress. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with each project, expressed as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table serves as a critical resource for identifying employees who have not submitted their timesheets within a specified timeframe, enabling managers to address compliance issues and ensure accurate payroll processing. By tracking relevant details such as employee and manager information, contract dates, and hours, this table facilitates timely interventions to enhance workforce accountability and operational efficiency.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by employees that have not been recorded in their timesheets, facilitating accurate tracking of labor and resource allocation. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to conclude, providing critical information for managing timesheet compliance and workforce planning within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to facilitate the identification of discrepancies in timesheet submissions within the 'vw_missing_timesheet' table. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of employee contracts, providing essential context for identifying missing timesheet entries in the 'vw_missing_timesheet' table. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_missing_timesheet' table uniquely identifies each employee associated with missing timesheet entries, facilitating targeted data analysis and reporting. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up of individuals for timely data completion. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours recorded for each employee's timesheet entry, facilitating the identification of discrepancies in time reporting. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data updates. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the employee's timesheet entries, facilitating accountability and tracking within the 'vw_missing_timesheet' table. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees' timesheet submissions, facilitating accountability and tracking within the 'vw_missing_timesheet' view. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing critical insights for identifying gaps in employee time tracking. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the missing timesheet records, facilitating the tracking and management of time allocation across various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores unique identifiers for projects associated with missing timesheets, facilitating the tracking and management of time allocation across various initiatives. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true signifying rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet data is complete and ready for reporting, with a value of true signifying readiness. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double to accommodate precise financial calculations within the 'vw_missing_timesheet' table. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double to accommodate precise pricing information within the context of timesheet data analysis. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with missing timesheet entries, facilitating timely follow-up and resolution of discrepancies. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific organizational subunit associated with each entry in the missing timesheet records, facilitating targeted analysis and resolution of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' view. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the work performed during the recorded timesheet period, facilitating better understanding and analysis of employee activities. |
| `Week_` | `int64` | The 'Week_' column (int64) in the 'vw_missing_timesheet' table represents the numerical week of the year associated with each timesheet entry, facilitating the identification of missing submissions by week. |
| `Year_` | `int64` | The 'Year_' column (int64) in the 'vw_missing_timesheet' table represents the calendar year associated with each timesheet entry, facilitating the analysis of timesheet data over time. |

##### Calculated Columns

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column identifies the department associated with billable hours for employees, facilitating accurate tracking and reporting of time spent on chargeable projects within the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Let's break down what it does in simple business terms:

1. **Purpose**: The goal of this calculated column is to determine whether a certain unit is billable based on a related table called `lkp_Unit`.

2. **Logic**:
   - The expression first checks if the `BillableDep` value from the `lkp_Unit` table is blank (i.e., there is no value).
   - If it is blank, the calculated column will return a value of **1**. This indicates that the unit is considered billable in this case.
   - If the `BillableDep` value is not blank, the expression then checks if this value is not equal to **0**.
     - If `BillableDep` is not equal to **0**, it again returns **1**, indicating that the unit is billable.
     - If `BillableDep` is equal to **0**, it returns **0**, indicating that the unit is not billable.

3. **Summary**: 
   - The calculated column `BillableDep` will return **1** if:
     - The related `BillableDep` value is blank (no value).
     - The related `BillableDep` value is any number other than **0**.
   - It will return **0** only if the related `BillableDep` value is exactly **0**.

In essence, this DAX expression helps categorize units as billable or not based on the presence and value of the `BillableDep` field in the related `lkp_Unit` table.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `CC_ActiveEmployees` in a data model, specifically within a table called `vw_missing_timesheet`. Here's a breakdown of what this code does in simple business terms:

1. **Purpose**: The calculated column is designed to identify whether an employee is currently active based on their contract dates.

2. **Conditions Checked**:
   - **Start Date Check**: The code first checks if the date in the column `ShortDate` (which likely represents a specific date being evaluated) is on or after the employee's `ContractStartDate`. This means the employee's contract must have started before or on this date for them to be considered active.
   - **End Date Check**: Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. This means the employee's contract must still be valid on this date. However, if the `ContractEndDate` is blank (meaning the employee has an open-ended contract), the employee is still considered active.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the contract has no end date), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns `0`, indicating that the employee is not active.

### Summary:
In summary, this DAX expression effectively flags employees as "active" (1) or "not active" (0) based on whether the date being evaluated falls within their contract period. This helps in analyzing employee status and managing workforce data effectively.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is missing required information, represented as a string value for easy identification of incomplete records in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `IncompleteFlag` in a data model, specifically in a table named `vw_missing_timesheet`. Here's a breakdown of what this expression does in simple business terms:

### Explanation of the DAX Expression:

- **Condition Check**: The expression checks the value of the column `MissingHours` for each row in the `vw_missing_timesheet` table.
- **Logic**: 
  - If the value of `MissingHours` is **less than 0** (which might indicate that there are negative hours recorded, possibly due to an error or an incomplete timesheet), the expression returns **1**.
  - If the value of `MissingHours` is **0 or greater**, it returns **0**.

### What It Achieves:

- **Flagging Incomplete Entries**: The `IncompleteFlag` column will effectively mark rows where there is an issue with the recorded hours. A value of **1** indicates that there is a problem (incomplete or erroneous data), while a value of **0** indicates that the data is acceptable or complete.
- **Data Quality Control**: This calculated column helps in identifying and flagging entries that may require further review or correction, thus supporting better data quality and management in the reporting process.

In summary, this DAX expression is a simple way to create a flag that helps identify potentially problematic entries in the timesheet data, allowing for easier monitoring and management of timesheet accuracy.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the 'lkp_Unit' table for the current row. Specifically, it looks for the 'Unit' field in that related table.

2. **Handle Missing Data**: 
   - If there is no related value (meaning the 'Unit' field is blank), the code will return the text "Unknown". This is a way to handle situations where the expected data is missing, ensuring that the output is clear and informative.
   - If there is a related value (meaning the 'Unit' field has data), it will return that value.

3. **Purpose**: The overall purpose of this calculated column is to provide a clear and user-friendly representation of the 'Unit' associated with each row. If the unit is not available, it explicitly states "Unknown", which helps in identifying gaps in the data.

In summary, this DAX expression ensures that every row in the 'MAIN_UNIT' column either shows the corresponding unit name or indicates that the unit is unknown, thereby improving data clarity and usability.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column (string) in the 'vw_missing_timesheet' table represents the specific year and week number for each entry, facilitating the tracking and analysis of timesheet submissions over time.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'YearWeek' in a data model, specifically for a table that contains contract information. Here's a breakdown of what it does in simple business terms:

1. **Extracting the Year**: The function `YEAR([ContractEndDate])` takes the date from the column `[ContractEndDate]` (which represents when a contract ends) and extracts the year from that date. For example, if the contract ends on March 15, 2023, this part would return `2023`.

2. **Calculating the Week Number**: The function `WEEKNUM([ContractEndDate])` calculates the week number of the year for the same `[ContractEndDate]`. Continuing with our example, if the contract ends on March 15, 2023, this function would return `11`, since March 15 falls in the 11th week of the year.

3. **Formatting the Week Number**: The `FORMAT(..., "00")` function ensures that the week number is always displayed as a two-digit number. So, if the week number is `5`, it will be formatted as `05`. This is important for consistency, especially when the week number is less than 10.

4. **Combining Year and Week Number**: The `&` operator is used to concatenate (join together) the year and the formatted week number with a hyphen in between. So, if the year is `2023` and the week number is `11`, the final result would be `2023-11`.

### Summary:
In summary, this DAX expression creates a new column that combines the year and the week number of the contract's end date into a single string formatted as "YYYY-WW". This allows for easy identification and grouping of contracts based on the year and week they end, which can be useful for reporting and analysis purposes. For example, a contract ending on March 15, 2023, would be represented as `2023-11`.

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

1. **Purpose**: This measure counts the number of unique consultants (employees) who have recorded negative hours in their timesheets.

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the number of unique employee names in the `vw_missing_timesheet` table. It ensures that each consultant is only counted once, even if they have multiple entries with negative hours.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This is the filter condition applied by the `CALCULATE` function. It specifies that only those records where the `MissingHours` value is less than zero should be considered in the count.

3. **Outcome**: The measure ultimately provides a count of how many different consultants have reported negative hours. This could indicate issues such as underreporting of hours worked, errors in timesheet submissions, or other discrepancies that need to be addressed.

In summary, `Count_Negative_Consultant` helps the business identify and track the number of consultants with negative hours, which can be critical for ensuring accurate timekeeping and resource management.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to create a measure called **CountNegativeMissingHours**. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure counts the number of unique employees who have recorded negative missing hours in a timesheet.

2. **Components**:
   - **CALCULATE**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **DISTINCTCOUNT**: This function counts the number of unique values in a specified column. Here, it counts unique Employee IDs.
   - **'vw_missing_timesheet'[EmployeeID]**: This refers to the column in the data that contains the IDs of employees.
   - **'vw_missing_timesheet'[MissingHours] < 0**: This is a filter condition that specifies we are only interested in records where the MissingHours value is less than zero (indicating negative hours).

3. **What it Achieves**: The measure calculates how many different employees have a record of negative missing hours. This could be useful for identifying issues in timesheet reporting or understanding discrepancies in hours worked versus hours reported.

In summary, **CountNegativeMissingHours** helps a business track how many employees have reported negative hours, which could indicate potential errors or issues in their timesheet submissions.

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

3. **Calculating Missing Hours**: For each group of employees, it calculates:
   - **MissingHours**: This is determined by taking the total hours worked (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contractual hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has not logged enough hours compared to what was expected.

4. **Identifying Active Employees**: It also calculates:
   - **MinActiveEmp**: This checks the minimum value of a field called `CC_ActiveEmployees`, which likely indicates whether the employee is currently active (1 for active, 0 for inactive). 

5. **Filtering Criteria**: The `FILTER` function then narrows down the summarized data to only include:
   - Employees who are active (`[MinActiveEmp] = 1`)
   - Employees who have missing hours (`[MissingHours] < 0`)

6. **Counting Active Employees**: Finally, the measure counts the number of rows in the filtered result, which represents the number of active employees who have missing timesheet hours.

### Summary:
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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is designed to calculate a measure called `Dax_MissingHours`, which focuses on identifying and quantifying the hours that certain employees are missing based on their contractual obligations. Here’s a breakdown of what this code does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. This variable filters a summarized table of data (`vw_missing_timesheet`) to focus on employees who are currently active (indicated by `CC_ActiveEmployees` being equal to 1).

2. **Summarize Data**: The `SUMMARIZE` function is used to group the data by several key attributes:
   - **Employee Name**: Identifies which employee the data pertains to.
   - **Year and Week**: Specifies the time period for the data.
   - **Unit and SubUnit**: Provides additional organizational context.
   - **Missing Hours Calculation**: For each employee, it calculates the total hours they have logged (`SUM(vw_missing_timesheet[Hours_])`) and subtracts the maximum hours they are contracted to work (`MAX(vw_missing_timesheet[ContractHours])`). This gives the total missing hours for that employee.

3. **Filter for Missing Hours**: The `FILTER` function then narrows down this summarized data to only include those employees who are active and have a negative value for `MissingHours`. This means it focuses on employees who have worked fewer hours than they are contracted for.

4. **Calculate Total Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered list of active employees and sums up their `MissingHours`. This gives the total number of hours that active employees are missing compared to their contractual obligations.

### Summary:
In essence, this DAX measure calculates the total number of hours that active employees are missing from their expected work hours, helping the business understand potential shortfalls in labor hours. This information can be crucial for workforce management, ensuring that staffing levels meet operational needs.

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

1. **Data Source**: The calculation uses a data table called `vw_missing_timesheet`, which likely contains information about employee timesheets, including the week, employee IDs, and their respective contract hours.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key pieces of information:
   - **Week**: This represents the specific week for which we are calculating the expected hours.
   - **EmployeeID**: This identifies each individual employee.

3. **Calculating Maximum Contract Hours**: For each unique combination of week and employee, the code calculates the maximum contract hours using `MAX(vw_missing_timesheet[ContractHours])`. This means that if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

4. **Summing Up Contract Hours**: After summarizing the data and determining the maximum contract hours for each employee in each week, the `SUMX` function then sums up all these maximum contract hours. Essentially, it adds together the highest contract hours for each employee across all weeks.

5. **Final Result**: The final result of this measure, `ExpectedContractHoursWeekly`, gives you the total expected contract hours for all employees, considering only the maximum hours recorded for each employee in each week. This helps in understanding the total expected workload based on the highest contractual obligations recorded, which can be useful for planning and resource allocation.

In summary, this DAX measure helps businesses track and analyze the total expected contract hours for employees on a weekly basis, ensuring that they are aware of the maximum commitments made by each employee.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked and contracted hours for employees. Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum value of "ContractHours" from the `vw_missing_timesheet` table. "ContractHours" represents the total number of hours that an employee is contracted to work.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum value of "Hours_" from the same table. "Hours_" represents the actual hours that the employee has worked or reported.

3. **Subtraction**: The expression then subtracts the maximum actual hours worked (Hours_) from the maximum contracted hours (ContractHours).

### What It Achieves:
The overall calculation gives you the difference between the maximum contracted hours and the maximum hours actually worked. 

- **If the result is positive**: This indicates that there are missing hours that the employee was contracted to work but did not report. It highlights a shortfall in hours worked compared to what was expected.

- **If the result is zero or negative**: This means that the employee has either met or exceeded their contracted hours, indicating no missing hours.

In summary, this measure helps identify any discrepancies between expected and actual work hours, which can be crucial for managing workforce productivity and ensuring compliance with contractual obligations.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates a measure called `PercentageCompleteness`. Let's break it down into simple business terms:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part of the code adds up all the missing hours from a table called `vw_missing_timesheet`. Essentially, it tells us how many hours were not accounted for in timesheets.

2. **[ExpectedContractHoursWeekly]**: This represents the expected number of hours that should have been worked in a week according to the contract. It serves as a benchmark for comparison.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the code calculates the difference between the total missing hours and the expected contract hours. If the result is positive, it means there are more missing hours than expected; if negative, it means the hours worked are exceeding the expected hours.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: This function divides the difference calculated in the previous step by the expected contract hours. The `DIVIDE` function is used instead of a simple division to handle cases where the denominator (expected hours) might be zero. If it is zero, the result will be 0 instead of causing an error.

### What It Achieves:
The overall measure calculates the percentage of completeness regarding timesheet submissions. Specifically, it shows how the actual missing hours compare to the expected hours. 

- If the result is positive, it indicates a shortfall in timesheet submissions relative to what was expected, suggesting that employees are not reporting their hours accurately.
- If the result is negative, it indicates that employees are reporting more hours than expected, which could suggest over-reporting or additional work being done.

In summary, this measure helps the business understand how well employees are completing their timesheets compared to what is expected, allowing for better management of time reporting and resource allocation.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate a measure called `UniqueEmployeesPerUnit`. Let's break it down step by step in simple business terms:

1. **Data Source**: The measure is based on a table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including information about whether their submissions are complete or incomplete.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by three key columns:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This identifies each employee.
   - `MAIN_UNIT`: This indicates the main unit or department the employee belongs to.

   While grouping, the code also creates a new column called `IncompleteFlag`, which captures the maximum value of the `IncompleteFlag` for each combination of week, employee, and unit. The `IncompleteFlag` likely indicates whether an employee has submitted their timesheet completely (e.g., 0 for complete, 1 for incomplete).

3. **Calculating the Sum**: After summarizing the data, the `SUMX` function iterates over the summarized table. For each group (each unique combination of week, employee, and unit), it sums up the values in the `IncompleteFlag` column.

4. **Final Outcome**: The final result of this measure is the total count of incomplete timesheet submissions across all employees and units for each week. This measure helps in understanding how many employees have not submitted their timesheets completely, which can be crucial for payroll processing, compliance, and operational efficiency.

In summary, `UniqueEmployeesPerUnit` calculates the total number of incomplete timesheet submissions by employees, grouped by week and unit, helping the organization track and manage timesheet compliance effectively.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This table enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column in the 'vw_Timesheet' table identifies the specific client associated with each timesheet entry, facilitating accurate billing and project tracking. |
| `Code2` | `string` | The 'Code2' column in the 'vw_Timesheet' table stores a secondary identifier for time entries, facilitating enhanced data categorization and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to conclude, providing essential information for project planning and resource allocation within the timesheet data. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and payroll calculations within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of contracts associated with timesheet entries, providing a real-time overview of contractual obligations and conditions. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, providing essential insights for performance tracking and decision-making within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential temporal context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the total monetary cost associated with the recorded time entries in the 'vw_Timesheet' table, expressed as a double-precision floating-point number. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, expressed as a double to accommodate precise financial calculations. |
| `Currency` | `string` | The 'Currency' column in the 'vw_Timesheet' table specifies the type of currency used for financial transactions recorded in the timesheet, facilitating accurate cost analysis and reporting. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the individual or entity responsible for payment, facilitating the tracking and management of outstanding invoices within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_Timesheet' table uniquely identifies each employee, facilitating the tracking and management of their respective timesheet entries. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column stores the unique identifier and name of each employee, facilitating the association of timesheet entries with the corresponding personnel in the vw_Timesheet table. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating easy identification and reporting of labor hours. |
| `Employer` | `string` | The 'Employer' column identifies the organization or company for which the employee is working during the recorded timesheet entry. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating accurate payroll processing and reporting. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to each timesheet entry, providing insights that enhance the understanding of the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with each timesheet entry, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, facilitating accurate tracking and reporting of labor efforts. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating analysis of labor distribution and productivity. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee work patterns. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee time allocation. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry timelines. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for project managers, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing project timelines and resource allocation, facilitating effective project tracking and accountability within the timesheet records. |
| `IPMIDName` | `string` | The 'IPMIDName' column in the 'vw_Timesheet' table stores the names of individuals associated with specific IPMIDs, facilitating the tracking and management of timesheet entries linked to these identifiers. |
| `JobTitle` | `string` | The 'JobTitle' column in the 'vw_Timesheet' table captures the specific role or position of an employee, providing essential context for analyzing timesheet data and resource allocation. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking timesheet records to related datasets, facilitating data integration and analysis. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and reporting within the organization. |
| `ManagerIDName` | `string` | Column Description: The 'ManagerIDName' column stores the names of managers associated with each timesheet entry, facilitating easy identification of supervisory oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key of the manager responsible for overseeing the associated timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct supervisor, facilitating the identification of managerial oversight for each timesheet entry. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries, providing detailed information about hours worked on specific tasks within the timesheet records. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each timesheet entry, enabling detailed tracking of time allocation and resource management across various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column in the 'vw_Timesheet' table uniquely identifies the specific project associated with each timesheet entry, facilitating accurate tracking and reporting of time spent on various projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that identifies the specific project associated with each timesheet entry, facilitating detailed tracking and analysis of time allocation across various projects. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the associated project profile for each timesheet entry, facilitating accurate project tracking and reporting. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, enabling better analysis of resource allocation and project performance. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet is ready for reporting, with a value of true signifying readiness and false indicating further processing is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double to accommodate precise financial calculations within the 'vw_Timesheet' table. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current approval or processing state of each timesheet entry, facilitating tracking and management of employee work hours. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation, with a value of true representing taxed hours and false representing non-taxed hours. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, enabling better resource allocation and management. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for each timesheet entry, facilitating efficient tracking and management of employee work hours within the vw_Timesheet table. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the tasks and activities recorded in the timesheet, providing context and details for each entry. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table represents the measurement unit associated with the recorded time entries, facilitating accurate tracking and reporting of work hours across different projects or tasks. |
| `UnitCode` | `string` | The 'UnitCode' column in the 'vw_Timesheet' table represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating accurate tracking and reporting of labor costs by department. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and data enrichment within the timesheet management system. |
| `YearWeek` | `string` | The 'YearWeek' column (string) in the 'vw_Timesheet' table represents the specific year and week number, facilitating time-based analysis and reporting of timesheet data. |

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `Approved_With_V_Z`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column determines whether a timesheet is considered "approved" based on specific criteria.

2. **Conditions**: The code checks two main conditions:
   - **Condition 1**: It checks if the `Approved` column is marked as `TRUE`. This means that if the timesheet has been officially approved, it meets the criteria.
   - **Condition 2**: It checks if the `TimesheetCode` is either "V" or "Z". These codes likely represent specific types of timesheets that are automatically considered approved regardless of the `Approved` status.

3. **Output**: 
   - If either of the conditions is met (the timesheet is approved or the code is "V" or "Z"), the calculated column will return a value of `1`. This indicates that the timesheet is approved.
   - If neither condition is met, it returns a value of `0`, indicating that the timesheet is not approved.

In summary, this DAX expression effectively flags timesheets as approved (1) or not approved (0) based on whether they are explicitly approved or fall under specific codes (V or Z). This helps in easily identifying which timesheets can be considered approved for further processing or reporting.

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column identifies the department associated with billable hours recorded in the timesheet, facilitating accurate cost allocation and departmental budgeting.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `BillableDep`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column is designed to determine whether a certain unit is considered "billable" based on a related table called `lkp_Unit`.

2. **Logic**:
   - The expression first checks if the `BillableDep` value from the `lkp_Unit` table is blank (i.e., there is no value).
   - If it is blank, the expression returns **1**. This indicates that if there is no information about whether the unit is billable, it defaults to being considered billable.
   - If the `BillableDep` value is not blank, it then checks if this value is not equal to **0**.
     - If the value is not **0**, it returns **1**, meaning the unit is billable.
     - If the value is **0**, it returns **0**, indicating that the unit is not billable.

3. **Outcome**: The final result of this calculated column will be either **1** (indicating the unit is billable) or **0** (indicating it is not billable). This helps in categorizing units based on their billable status, which can be useful for reporting, analysis, or decision-making processes in a business context.

In summary, this DAX expression helps to classify units as billable or not based on the related `BillableDep` information, providing a clear indicator for further analysis or billing processes.

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

1. **Purpose**: The `BillablePrj` column is designed to identify whether a particular timesheet entry is billable or not. A billable entry is one that can be charged to a client or customer.

2. **Conditions Checked**:
   - **Sales Price**: The first condition checks if the `SalesPrice` for the entry is greater than 0. If there is a positive sales price, it indicates that the work done can be billed to a client.
   - **Project Profile Code**: The second condition checks if the `ProjectProfileCode` is equal to 81. This specific code likely represents a type of project that is always considered billable, regardless of other factors.
   - **Project Name**: The third condition uses the `SEARCH` function to look for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it suggests that the project is related to customer success services, which are typically billable.

3. **Output**: 
   - If any of the three conditions are met (i.e., if the sales price is positive, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of `1`, indicating that the entry is billable.
   - If none of the conditions are met, it returns a value of `0`, indicating that the entry is not billable.

In summary, this DAX expression helps categorize timesheet entries as billable or non-billable based on specific criteria related to sales price, project type, and project name. This categorization is essential for accurate billing and financial reporting.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `Employee_WeeklyHours` in a data model, specifically within a table named `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Combining Information**: The code takes two pieces of information from the `vw_Timesheet` table:
   - The name of the employee (`EmployeeName`).
   - The number of hours that employee works per week (`HoursperWeek`).

2. **Formatting the Output**: It combines these two pieces of information into a single string. The format of the output will be:
   - The employee's name followed by a hyphen and then the number of hours they work each week. For example, if the employee's name is "John Doe" and he works 40 hours a week, the output would be "John Doe - 40".

3. **Purpose**: The purpose of this calculated column is to create a clear and concise representation of each employee's weekly working hours alongside their name. This can be useful for reporting, analysis, or simply for better readability in the data model.

In summary, this DAX expression helps to create a user-friendly summary of each employee's name and their corresponding weekly hours worked, making it easier to understand and analyze employee work patterns.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'GroupCat' in a data model, likely related to project management or timesheet tracking. Here’s a breakdown of what it does in simple business terms:

1. **Purpose**: The 'GroupCat' column categorizes projects based on whether they are linked to a specific group in a lookup table or if they are billable or unbillable.

2. **Lookup for Group**: 
   - The code first checks if there is a corresponding group for the project listed in the `vw_Timesheet` table by looking it up in the `lkp_Project` table. 
   - It uses the `LOOKUPVALUE` function to find the group associated with the project. If a group is found (i.e., it is not blank), it will return that group.

3. **Conditional Logic**:
   - If a group is found (the result of the lookup is not blank), the 'GroupCat' column will take the value of that group.
   - If no group is found (the result of the lookup is blank), it checks if the project is billable by looking at the `BillablePrj` column in the `vw_Timesheet` table.
     - If `BillablePrj` equals 1 (indicating the project is billable), it assigns the category "Billable".
     - If `BillablePrj` does not equal 1 (indicating the project is unbillable), it assigns the category "Other Unbillable".

4. **Outcome**: 
   - The final result is that each project in the `vw_Timesheet` table will be categorized as either a specific group (if applicable), "Billable", or "Other Unbillable". This helps in understanding the nature of each project in terms of its billing status and group association.

In summary, this DAX expression effectively classifies projects into meaningful categories based on their group affiliation and billing status, which can be very useful for reporting and analysis in a business context.

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

1. **Checks Contract End Date**: The code first checks if the **ContractEndDate** for each record (or row) in the **vw_Timesheet** table is blank (meaning there is no end date specified) or if the end date is in the future (greater than today's date).

2. **Determines Contract Status**:
   - If either of the above conditions is true (the contract has no end date or the end date is still ahead of today), it labels the contract as **"Active"**.
   - If neither condition is true (meaning there is a specific end date that has already passed), it labels the contract as **"Not Active"**.

### In Summary:
This calculated column helps to quickly identify whether a contract is currently active or not based on its end date. If a contract is still ongoing (either because it has no end date or it ends in the future), it is marked as "Active"; otherwise, it is marked as "Not Active". This can be useful for tracking contract statuses in a business context.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code uses the `RELATED` function to look up a value from another table called `lkp_Unit`. Specifically, it tries to find the value in the column `Unit` that is related to the current row of the table where this calculated column is being created.

2. **Handle Missing Data**: The `ISBLANK` function checks if the value retrieved from the `lkp_Unit[Unit]` column is blank (i.e., there is no related unit found). 

3. **Return Values Based on the Check**:
   - If the value is blank (meaning there is no related unit), the code returns the string "Unknown". This indicates that the system could not find a corresponding unit for that particular row.
   - If the value is not blank (meaning a related unit was found), it returns the actual unit name from the `lkp_Unit[Unit]` column.

### Summary:
In summary, this DAX expression is designed to populate the 'MAIN_UNIT' column with the name of a unit from a related table. If no unit is found, it will label that entry as "Unknown". This helps ensure that every row has a clear indication of its unit, either by showing the actual unit name or by marking it as unknown when no unit is available.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) to facilitate time-based reporting and analysis of timesheet data.
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
- **Simplifies Analysis**: By converting dates into month numbers, it makes it easier to analyze data by month. For example, you can quickly summarize or visualize timesheet data by month.
- **Facilitates Reporting**: This calculated column can be used in reports to group or filter data based on the month, helping businesses track performance or trends over time.

In summary, this DAX expression helps in breaking down dates into a more manageable format (month numbers), which is useful for reporting and analysis purposes.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the employee identification numbers as strings, facilitating the association of timesheet entries with specific employees in the vw_Timesheet view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

### What it Achieves:

1. **Counts Unique Employees**: This expression counts how many different employees are present in the `EmployeeName` column. For example, if the column has the names "Alice", "Bob", "Alice", and "Charlie", the result would be 3, because there are three unique names.

2. **Useful for Reporting**: This calculation is particularly useful in business reporting and analysis. It helps organizations understand how many distinct employees are contributing to a project, working in a department, or logged hours in a timesheet.

3. **Avoids Duplicates**: By using `DISTINCTCOUNT`, the expression ensures that each employee is only counted once, regardless of how many times their name appears in the timesheet. This is important for accurate reporting and analysis.

In summary, this DAX expression provides a clear count of how many different employees are recorded in the timesheet, which can be valuable for workforce analysis and resource management.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table captures the classification of time entries as either owned, subcontracted, or external, facilitating accurate tracking and reporting of resource allocation.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used in a calculated column to retrieve a specific value from a related table. Here’s a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a table that has a relationship with another table called `lkp_Unit`. The relationship is established based on a common key or identifier.

2. **Purpose**: The expression aims to fetch the value from the `OWN-Sub-ExtT` column in the `lkp_Unit` table for each row in the current table where the expression is being evaluated.

3. **How It Works**: 
   - When you use `RELATED`, DAX looks for the corresponding row in the `lkp_Unit` table that matches the current row based on the established relationship.
   - It then retrieves the value from the `OWN-Sub-ExtT` column of that matched row.

4. **Outcome**: The result is that each row in the current table will have a new column (the calculated column) that contains the value from `lkp_Unit[OWN-Sub-ExtT]`. This allows you to enrich your data with additional information from the related table, which can be useful for analysis and reporting.

In summary, this DAX expression enhances your data model by linking information from the `lkp_Unit` table to the current table, providing valuable insights based on the `OWN-Sub-ExtT` values.

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

1. **Purpose**: The calculated column 'QualifyPrj' is designed to determine a qualification status for projects based on related data from another table called 'lkp_Project'.

2. **Logic**:
   - The code checks if the 'Qualify' field from the 'lkp_Project' table is blank (i.e., it has no value).
   - If the 'Qualify' field is blank, the calculated column will return a value of **1**. This could indicate a default or a specific status, such as "not qualified" or "unknown".
   - If the 'Qualify' field is not blank (meaning it has a value), the calculated column will return the actual value from the 'Qualify' field in the 'lkp_Project' table.

3. **Outcome**: The result is that for each row in the table where this calculated column is created, you will either get a default value of 1 (if there is no qualification information available) or the specific qualification value from the related 'lkp_Project' table. This helps in easily identifying projects that have a qualification status and those that do not.

In summary, this DAX expression helps to ensure that every project has a qualification status, either by providing a default value when no information is available or by pulling in the relevant qualification data when it exists.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for submitting the timesheet data, facilitating accurate tracking and reporting of labor hours.

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

1. **Purpose**: The calculated column determines whether a certain condition is met for each row in the dataset. It outputs either a 1 (true) or a 0 (false) based on specific criteria.

2. **Conditions Checked**:
   - The first condition checks if the column `[ReportReady]` is marked as `TRUE()`. This likely indicates that a report is ready for processing or has been completed.
   - The second condition checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These codes probably represent specific types of timesheets or statuses that are relevant to the business.

3. **Output**:
   - If either of the conditions is true (meaning the report is ready or the timesheet code is "V" or "Z"), the calculated column will return a value of **1**.
   - If neither condition is met, it will return a value of **0**.

4. **Business Implication**: This calculated column can be used to easily identify rows that are either ready for reporting or belong to specific categories (V or Z). This can help in filtering, analyzing, or aggregating data based on readiness or specific timesheet types, making it easier for decision-makers to focus on relevant entries. 

In summary, `RReady_With_V_Z` helps flag important records in the dataset based on readiness and specific timesheet codes, facilitating better data analysis and reporting.

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

1. **Condition Check**: The measure first checks the value of another measure called `ContractStatusMeasure`. It specifically looks to see if this measure equals "Active".

2. **Return Value Based on Condition**:
   - If `ContractStatusMeasure` is "Active", the measure will return the value of another measure called `HoursDifference`. This likely represents the difference in hours related to the contract, such as the number of hours worked or remaining hours on a contract.
   - If `ContractStatusMeasure` is not "Active", the measure will return the text "Not Active".

### Summary:
In essence, `ContractStatus2` is used to determine the status of a contract. If the contract is currently active, it provides a numerical value (the hours difference). If the contract is not active, it simply indicates that by returning the text "Not Active". This measure helps users quickly understand the status of contracts and the associated hours, facilitating better decision-making and reporting.

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
   - The measure first checks if the maximum value of the `ContractEndDate` from the `vw_Timesheet` table is blank (meaning there is no end date recorded) or if that date is greater than today's date.
   - If either of these conditions is true, it means the contract is still active (either it has no end date or it ends in the future).
   - If neither condition is true (meaning the contract has an end date that is in the past), it indicates that the contract is not active.

3. **Output**:
   - If the contract is active, the measure returns the text "Active".
   - If the contract is not active, it returns the text "Not Active".

### Summary
In summary, `ContractStatusMeasure` helps businesses quickly identify whether contracts are currently active or not based on their end dates. This can be useful for managing contracts, ensuring compliance, and making informed decisions about contract renewals or terminations.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to determine the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 10, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and calculates which week of the year that date falls into. The week number is typically calculated starting from January 1st, with the first week of the year being the week that contains January 1st.

So, when you combine these two functions, `WEEKNUM(TODAY())` calculates the week number for the current date. 

### Business Implication:
In practical terms, this measure can be used in reports or dashboards to identify which week of the year it currently is. This can be particularly useful for businesses that track performance, sales, or other metrics on a weekly basis. By knowing the current week number, teams can align their activities, analyze trends, and make timely decisions based on weekly performance data.

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
   - **`vw_Timesheet[Approved] = FALSE()`**: This condition filters the data to include only those timesheet entries where the `Approved` status is set to FALSE. In other words, it focuses on timesheets that have not been approved.

3. **Outcome**: The measure ultimately provides a count of how many distinct employees have submitted timesheets that are still pending approval. This information can be useful for tracking outstanding approvals and managing workflow within the organization.

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

1. **Purpose**: This measure calculates the number of unique employees whose timesheets are not marked as "Report Ready."

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows us to apply filters to the data before performing calculations.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it tells us how many different employees are present in the data.
   - **`vw_Timesheet[ReportReady] = FALSE()`**: This condition filters the data to include only those timesheets where the `ReportReady` field is set to FALSE. In other words, it focuses on timesheets that are not ready for reporting.

3. **What It Achieves**: The overall result of this measure is to provide a count of distinct employees who have timesheets that are still pending or not ready for reporting. This can be useful for management to understand how many employees still need to finalize their timesheets before reports can be generated.

In summary, `Dax_EmpCount_RReady` helps businesses track how many employees have not yet completed their timesheets, which is important for ensuring timely reporting and payroll processing.

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

In simple terms, this measure calculates how many different employees have logged time in the timesheet. For example, if there are entries for "Alice", "Bob", and "Alice" again, the result of this measure would be 2, because there are two unique names (Alice and Bob).

### Business Context:

This measure is useful for understanding workforce participation or engagement. It helps businesses track how many individual employees are contributing to projects or tasks over a specific period, which can be important for resource management, payroll, and performance analysis.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the average hourly rate for a given dataset, specifically from a table called `vw_Timesheet`. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the `SalesAmount` column of the `vw_Timesheet` table. Essentially, it totals the revenue generated from sales.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked, as recorded in the `Hours` column of the same table. It gives the total number of hours that have been logged.

3. **Division**: The expression then divides the total sales amount by the total hours worked. This calculation gives you the average hourly rate, which represents how much revenue is generated for each hour worked.

### In Summary:
The measure `MSR_AVG_HOURLY_RATE` calculates the average revenue earned per hour worked by taking the total sales and dividing it by the total hours. This metric is useful for understanding productivity and profitability on an hourly basis, helping businesses assess how effectively they are generating revenue relative to the time invested.

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
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called `Msr_Budget`, which is used to calculate a budget figure based on sales data and the week of the year. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: 
   - The measure starts by determining the current week number of the year using `SELECTEDVALUE(DimDate[WeekNumberOfYear])`. This means it looks at the date context in which the measure is being evaluated and identifies which week it is.

2. **Calculate Previous Budget**:
   - Next, it calculates a value called `PrevBudget`. This is done using the `CALCULATE` function, which modifies the context in which a measure is evaluated. 
   - Inside `CALCULATE`, it uses the measure `[Msr_SalesPriceBaseCurrency]`, which presumably represents some sales figure in the base currency.
   - The `FILTER` function is applied to the `DimDate` table to include only those weeks that are less than the current week. This means it sums up the sales figures for all previous weeks.

3. **Return the Appropriate Value**:
   - Finally, the measure uses an `IF` statement to decide what to return:
     - If the current sales figure (`[Msr_SalesPriceBaseCurrency]`) is not blank (meaning there is actual sales data for the current week), it returns that current sales figure.
     - If the current sales figure is blank (indicating no sales data for the current week), it returns the previously calculated budget (`PrevBudget`).

### Summary:
In summary, this measure calculates the budget for the current week based on sales data. If there are sales figures available for the current week, it uses those figures. If not, it falls back to the total sales from all previous weeks, effectively providing a way to estimate the budget when current data is missing. This helps businesses understand their performance against budget expectations, even in the absence of current week sales data.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'MSR_Cumulative Cost', which provides the cumulative cost up to a selected month in a report or dashboard. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The code starts by determining which month is currently selected in the visual (like a chart or table). It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed.

2. **Calculate Cumulative Cost**: The main goal of this measure is to calculate the total cost incurred up to and including the selected month. 

3. **Sum the Costs**: The `SUM` function is used to add up all the costs from the 'vw_Timesheet' table, specifically the 'CostAmount' column.

4. **Filter the Data**: The `FILTER` function is applied to ensure that only the months up to and including the selected month are considered in the calculation. The `ALLSELECTED` function is used to maintain any other filters that might be applied to the 'DimDate' table, ensuring that the calculation respects the context of the report.

5. **Final Result**: The result of this measure is the total cost accumulated from the beginning of the year (or the relevant time period) up to the selected month. This allows users to see how costs have built up over time, providing valuable insights for budgeting and financial analysis.

In summary, this DAX measure helps businesses track their cumulative costs over time, allowing for better financial planning and decision-making based on historical data.

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

3. **Use of CALCULATE and FILTER**: 
   - The `CALCULATE` function is used to change the context in which the data is evaluated. In this case, it’s summing up the `SalesAmount` from the `vw_Timesheet` table.
   - The `FILTER` function is applied to the `DimDate` table to include all months that are less than or equal to the `SelectedMonth`. This means if March is selected, it will include sales from January, February, and March.

4. **ALLSELECTED Function**: The `ALLSELECTED` function ensures that any filters applied to the `DimDate` table (like year or other slicers) are respected while still allowing the measure to consider all months up to the selected month.

### Summary:
In summary, this DAX measure calculates the total sales from the start of the year up to the month that the user has selected in the visual. This allows users to see how sales have accumulated over time, providing valuable insights into sales performance for the selected period.

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

1. **Calculate Total Hours**: 
   - The variable `TotalValue` is created to hold the total number of hours worked. This is done using the `CALCULATE` function, which sums up the hours from the `vw_Timesheet` table.
   - The `ALLSELECTED` function is used to ensure that the total hours are calculated based on the current selections in the report, but it ignores any filters applied specifically to the `GroupCat`, `Project`, or `EmployeeName` fields. This means it considers all relevant data while still respecting other filters that might be applied.

2. **Calculate Individual Hours**: 
   - The measure then sums up the hours for the current context (which could be a specific group, project, or employee).

3. **Calculate Percentage**: 
   - Finally, the `DIVIDE` function is used to calculate the percentage of hours worked by the current context (the sum of hours for the selected group, project, or employee) compared to the total hours calculated in `TotalValue`.
   - If the total value is zero (to avoid division by zero), it returns 0 instead of an error.

### Summary:
In summary, `Msr_HoursPercentage` calculates what percentage of the total hours worked (across all selected categories) is attributed to the specific group, project, or employee currently being analyzed. This helps in understanding how much of the total effort is coming from a particular segment, which can be useful for performance analysis and resource allocation.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the project margin for a specific measure called `MSR_ProjectMargin`. Here's a breakdown of what it does in simple business terms:

1. **Sales Amount**: The expression starts by summing up the total sales amount from a table called `vw_Timesheet`. This represents the total revenue generated from projects or services.

2. **Cost Amount**: Next, it sums up the total cost amount from the same `vw_Timesheet` table. This represents the total expenses incurred to deliver those projects or services.

3. **Calculating Project Margin**: The measure then subtracts the total cost amount from the total sales amount. This calculation gives you the project margin, which is essentially the profit made from the projects after accounting for the costs.

In summary, this DAX expression calculates the profit (or margin) from projects by taking the total revenue generated and subtracting the total costs associated with those projects. A positive result indicates a profitable project, while a negative result indicates a loss.

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

1. **Data Source**: The measure uses a table called `vw_Timesheet`, which likely contains records of timesheets related to various projects and clients.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key dimensions: `ProjectProfile` and `Client`. This means that for each unique combination of project and client, the measure will perform calculations.

3. **Calculating Sales Price**: Within each group, the measure retrieves the sales price in the base currency using `SELECTEDVALUE(tbl_Project[SalesPriceBaseCurrency])`. This function gets the sales price for the current project being evaluated. If there are multiple sales prices, it will return the single value if there's only one, or it will return blank if there are multiple values.

4. **Summing Up Values**: The outer `SUMX` function then takes the summarized table (which now contains the project-client combinations and their corresponding sales prices) and sums up the `MeasureValue` for all these combinations. 

In summary, this DAX measure calculates the total sales price in the base currency for all projects and clients listed in the `vw_Timesheet`. It effectively aggregates the sales prices, allowing businesses to see the total revenue or cost associated with their projects and clients in a consistent currency.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a measure called 'Static Total Employees'. Let's break down what it does in simple business terms:

1. **Purpose**: The measure calculates the total number of unique employees who have a valid contract status as of today.

2. **Components**:
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. It ensures that each employee is only counted once, even if they appear multiple times in the data.
   
   - **`ALL(vw_Timesheet)`**: This function removes any filters that might be applied to the `vw_Timesheet` table. By doing this, the measure looks at all the data in the table, rather than just a subset that might be filtered by other criteria (like date or department).
   
   - **`vw_Timesheet[ContractStatusToday] = "Valid"`**: This condition filters the data to only include rows where the employee's contract status is marked as "Valid". This means that only employees who currently have a valid contract will be counted.

3. **Overall Calculation**: When you put it all together, this measure counts how many unique employees have a valid contract, ignoring any other filters that might be in place. This is useful for understanding the current workforce that is actively employed under valid contracts, providing a clear picture of the organization's staffing status.

In summary, the 'Static Total Employees' measure gives you a count of all employees who are currently validly contracted, ensuring that you have an accurate and comprehensive view of your workforce.

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

In simple terms, this measure totals up all the hours from the `Hours` column in the `vw_Timesheet` table. The result gives you the overall number of hours worked, which can be useful for reporting, payroll calculations, or analyzing employee productivity.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to calculate a measure called 'TotalContractedHours'. Let's break it down in simple business terms:

1. **SUMX Function**: This function is used to perform a calculation over a table and then sum up the results. In this case, it will iterate through a set of values and calculate a total based on those values.

2. **VALUES Function**: The `VALUES(vw_Timesheet[EmployeeName])` part creates a unique list of employee names from the 'vw_Timesheet' table. This means that for each employee, the calculation will be done separately.

3. **MAX Function**: The `MAX(vw_Timesheet[HoursPerWeek])` part looks at the 'HoursPerWeek' column in the 'vw_Timesheet' table and finds the maximum number of hours that each employee is contracted to work in a week.

4. **Putting It All Together**: The entire expression calculates the total contracted hours for all employees by:
   - Going through each unique employee name.
   - For each employee, it finds the maximum hours they are contracted to work in a week.
   - Finally, it sums up these maximum hours across all employees.

### What It Achieves:
In summary, this DAX measure calculates the total number of contracted hours for all employees by summing up the highest number of hours each employee is scheduled to work in a week. This is useful for understanding the overall capacity or commitment of your workforce in terms of hours worked.

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
   - If the result is **positive**, it means that more hours were worked than were originally contracted, indicating potential overwork or additional effort beyond what was planned.
   - If the result is **negative**, it suggests that fewer hours were worked than contracted, which could indicate underperformance or that the project is ahead of schedule.
   - If the result is **zero**, it means that the actual hours worked match the contracted hours, indicating that the project is on track as per the agreement.

In summary, this measure helps assess whether the actual work hours align with the contracted expectations, providing insights into project performance and resource management.

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

1. **Check for Blank Values**: The expression starts by checking if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Zero if Blank**: If the sum of hours is blank (meaning no hours have been tracked), the measure will return a value of **0**. This is important because it ensures that when there are no recorded hours, the measure does not show a blank value, which could be confusing in reports.

3. **Return the Sum if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the measure will return the actual total sum of hours tracked from the `vw_Timesheet` table.

### Summary:
In essence, this measure calculates the total hours tracked by summing up the hours from the timesheet. If no hours are recorded, it returns 0 instead of a blank value. This makes it easier for users to understand the data in reports, as they will always see a numerical value (either the total hours or zero) rather than an empty space.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you've provided is a measure named `TotalHoursTrackedMissing`. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The measure first checks if the total sum of hours tracked in the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Value Based on Check**:
   - If the sum of hours is blank (meaning no hours have been logged), the measure returns the text "Empty". This indicates that there are no hours to report.
   - If there are hours logged (the sum is not blank), it returns the actual total number of hours tracked.

### Summary:
In essence, this measure helps to identify whether any hours have been recorded in the timesheet. If no hours are recorded, it clearly indicates that with the word "Empty". If hours are present, it provides the total number of hours tracked. This can be useful for reporting and ensuring that time tracking is being properly maintained.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you've provided calculates a measure called `trackedDiff`, which essentially determines the difference between the total hours tracked for a specific context (like an employee or a project) and the maximum hours tracked per week for that same context.

Here's a breakdown of what each part of the code does in simple business terms:

1. **[TotalHoursTracked]**: This part represents the total number of hours that have been logged or tracked. It could be the total hours worked by an employee or the total hours for a specific project.

2. **CALCULATE(...)**: This function modifies the context in which the data is evaluated. It allows us to perform calculations based on specific filters or conditions.

3. **MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])**: 
   - **DISTINCT(vw_Timesheet[HoursPerWeek])**: This part creates a list of unique values for hours tracked per week from the timesheet data.
   - **MAXX(...)**: This function then looks at that list of unique weekly hours and finds the maximum value. In other words, it identifies the highest number of hours that have been tracked in any single week for the context being evaluated (like a specific employee).

4. **ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])**: This part is used to maintain the filter context for the specific employee while ignoring any other filters that might be applied to the timesheet data. It ensures that the calculation of maximum hours per week is done only for the selected employee.

### What It Achieves:
The overall calculation of `trackedDiff` gives you the difference between the total hours tracked and the maximum hours tracked in any single week for that employee. 

- If the result is positive, it indicates that the total hours tracked are greater than the maximum weekly hours, which might suggest consistent overtime or additional work.
- If the result is zero or negative, it indicates that the total hours tracked are less than or equal to the maximum weekly hours, which could suggest that the employee is working within their typical capacity.

In summary, `trackedDiff` helps in understanding how an employee's total tracked hours compare to their maximum weekly hours, providing insights into workload and productivity.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided calculates a measure called `trackedDiff2`. Let's break it down into simpler terms to understand what it achieves:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been tracked for a specific context (like a specific employee or time period).

2. **Calculating the Maximum Hours Per Week**:
   - The `CALCULATE` function is used to modify the context in which the data is evaluated.
   - Inside `CALCULATE`, the `MAXX` function is applied to find the maximum value of `HoursPerWeek` from a distinct list of hours recorded in the `vw_Timesheet` table. This means it looks at all the unique hours per week that have been logged and finds the highest one.

3. **Removing Filters**:
   - The `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])` part ensures that any filters applied to the `HoursPerWeek` column are ignored. This allows the calculation to consider all possible values of `HoursPerWeek` without any restrictions.

4. **Keeping Employee Context**:
   - The `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` function keeps the context for the specific employee (identified by `EmployeeID`) while removing other filters. This means the calculation will only consider the data related to the current employee, but it will not be limited by any other filters that might be applied to the `vw_Timesheet` table.

5. **Final Calculation**:
   - The final result of the measure `trackedDiff2` is the difference between the total hours tracked for the employee and the maximum hours they have logged in any week. 

### Summary:
In simple business terms, `trackedDiff2` calculates how many more hours an employee has tracked compared to their highest recorded hours in any single week. This measure can help identify if an employee is consistently working more hours than their peak week, which could be useful for workload management or performance evaluation.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is designed to calculate the total billable hours from a timesheet data source, specifically focusing on certain criteria. Here's a breakdown of what it does in simple business terms:

1. **Variable Definition**: The code starts by defining a variable called `FilteredHours`. This variable will hold a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**: The `Filter` function is used to create this filtered dataset. It includes two main conditions:
   - **Condition 1**: It checks if the project qualifies as billable (`vw_Timesheet[QualifyPrj] = 1` and `vw_Timesheet[BillablePrj] = 1`). This means that only projects that are marked as both qualifying and billable will be included.
   - **Condition 2**: It also includes any projects that contain the phrase "Customer Success Services" in their name (`SEARCH("Customer Success Services", vw_Timesheet[Project], 1, 0) > 0`). This allows for additional projects that may not meet the first criteria but are still relevant to the business.

3. **Calculating Total Hours**: After filtering the data based on the above criteria, the `SUMX` function is used to calculate the total hours worked on these filtered projects. `SUMX` iterates over each row in the `FilteredHours` variable and sums up the `vw_Timesheet[Hours]` for those rows.

4. **Final Output**: The result of this measure, `UTI_TotalBillableHours`, is the total number of hours that are billable based on the specified conditions. This measure helps the business understand how many hours can be billed to clients, which is crucial for revenue tracking and project management.

In summary, this DAX measure calculates the total billable hours from a timesheet, focusing on projects that are either specifically marked as billable or fall under a particular service category, helping the business track its billable work effectively.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `UTI_TOTALHOURS`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the total number of hours worked on specific projects that meet a certain qualification criteria.

2. **Components**:
   - **SUM(vw_Timesheet[Hours])**: This part of the code adds up all the values in the `Hours` column from the `vw_Timesheet` table. Essentially, it totals the hours recorded in timesheets.
   - **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to include only those entries where the `QualifyPrj` column equals 1. This means it only considers projects that are marked as qualified.

3. **CALCULATE Function**: The `CALCULATE` function modifies the context in which the data is evaluated. In this case, it ensures that the sum of hours is calculated only for the qualified projects.

### Summary:
In summary, the `UTI_TOTALHOURS` measure calculates the total hours worked on projects that are deemed qualified (where `QualifyPrj` equals 1). This is useful for understanding how much time is being spent on projects that meet specific criteria, helping businesses track performance and resource allocation effectively.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is a measure called `UTILIZATION_New`, which is designed to calculate the utilization rate of resources (like employees or contractors) based on their billable hours compared to their total hours worked. Here's a breakdown of what it does in simple business terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable captures the total number of hours that are billable (i.e., hours that can be charged to clients).
   - `_TotalHours`: This variable captures the total number of hours worked by the resource, which includes both billable and non-billable hours.

2. **Return Logic**:
   - The measure first checks if there are any active contacts (represented by `[Msr_ActiveContacts]`). If there are no active contacts, the measure will not proceed with the calculation.
   - If there are active contacts, it then checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the measure will not proceed further.
   - If `_TotalHours` is available, the measure calculates the utilization rate by dividing `_TotalBillableHours` by `_TotalHours`. This gives a percentage that indicates how much of the total time worked was billable.
   - If the result of the division is blank (which can happen if `_TotalHours` is zero), the measure will return 0 instead of a blank value.

3. **Final Outcome**:
   - The final output of this measure is a percentage that represents the utilization of resources. A higher percentage indicates that a greater portion of the hours worked is billable, which is generally a positive indicator of productivity and efficiency in a business context.

In summary, `UTILIZATION_New` calculates how effectively resources are being utilized by comparing their billable hours to their total hours worked, while ensuring that the calculation only occurs when there are active contacts and valid total hours.

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

