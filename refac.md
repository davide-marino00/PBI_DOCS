# Power BI Model & Report Documentation

*Generated on: 2025-04-29 00:53:33*

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

The Power BI data model appears to be designed for a project management and time tracking system, likely within a corporate or organizational context. It integrates various dimensions and fact tables to provide insights into project performance, employee time allocation, and overall resource management. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a focus on capturing and analyzing timesheet data, which is critical for understanding labor costs, project progress, and identifying gaps in time reporting.

The structure of the model indicates a star schema, where 'vw_Timesheet' serves as a central fact table linked to multiple dimension tables, including 'lkp_Project', 'lkp_Unit', and 'lkp_Code'. This allows for detailed analysis of time entries by project, organizational unit, and timesheet codes. Additionally, the 'DimDate' table facilitates time-based analysis, enabling users to filter and aggregate data across different time periods. The inclusion of 'vw_missing_timesheet' highlights a proactive approach to monitoring compliance with timesheet submissions, ensuring that all project hours are accounted for. Overall, this data model is likely aimed at enhancing decision-making and operational efficiency within project-driven environments.

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

The 'DimDate' table serves as a comprehensive date dimension, providing essential temporal attributes that enable businesses to analyze trends and performance over time. By offering detailed breakdowns of dates into various formats such as day, week, month, and quarter, this table supports effective reporting and enhances time-based analytics across multiple business processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of the date, facilitating time-based analysis and reporting. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient date-based queries and data enrichment processes. |
| `DayName` | `string` | The 'DayName' column contains the full name of the day of the week corresponding to each date in the 'DimDate' table, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | The 'DayNumberOfMonth' column (int64) in the 'DimDate' table represents the numerical day of the month, ranging from 1 to 31, providing a granular reference for date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical day of the week, where values range from 1 (Sunday) to 7 (Saturday), facilitating date-related analyses and reporting. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column represents the sequential day of the year as an integer, ranging from 1 to 365 (or 366 for leap years), providing a numeric reference for each date in the 'DimDate' table. |
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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CurrentYearFlag' in a data model, specifically in a date dimension table (DimDate). 

Here's what it does in simple terms:

1. **YEAR(DimDate[ShortDate])**: This part extracts the year from the date in the 'ShortDate' column of the DimDate table. For example, if the date is '2023-05-15', it will return '2023'.

2. **YEAR(TODAY())**: This part gets the current year based on today's date. If today is any date in 2023, this will return '2023'.

3. **IF(..., 1, 0)**: The IF function checks a condition. In this case, it checks if the year extracted from the 'ShortDate' is less than or equal to the current year. If this condition is true, it returns '1'; if false, it returns '0'.

### What it achieves:
- The 'CurrentYearFlag' column will have a value of '1' for all dates that are in the current year or any previous year. 
- It will have a value of '0' for any future dates (dates in the next year or beyond).

In summary, this calculated column helps identify whether a date is in the current year or earlier, which can be useful for filtering or analyzing data based on time periods.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table is designed to track and analyze the distribution of hours worked alongside their corresponding percentage contributions to overall performance metrics. This table aids in identifying resource allocation efficiency and optimizing workforce management strategies within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column stores string representations of hours worked alongside their corresponding percentage contributions to total hours, facilitating analysis of employee workload distribution. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and percentage calculations, facilitating the enrichment of data for analytical purposes. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, with associated qualification levels and sorting criteria to facilitate efficient data retrieval and reporting. This table enhances data integrity and consistency by providing a centralized source for code definitions and their hierarchical organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers used to categorize and enrich data within the lookup table 'lkp_Code'. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of each entry, serving as a numerical identifier for categorizing data enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table specifies the order in which records should be arranged, facilitating efficient data retrieval and organization. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics. This table enhances data integrity and enables efficient employee-related insights within Power BI dashboards.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column stores unique identifiers for employees, facilitating data enrichment and integration within the employee lookup table. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, facilitating the enrichment of data by providing identifiable references for each individual within the employee lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, capturing essential attributes such as project names, qualification scores, billing amounts, and associated groups, enabling businesses to analyze project performance and financial metrics effectively.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing amount associated with each project, facilitating financial tracking and analysis. |
| `Group` | `string` | The 'Group' column categorizes projects into specific classifications to facilitate data organization and retrieval within the lkp_Project table. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the lkp_Project table. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, with numerical values representing different levels of qualification criteria met. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their identifiers and billing classifications. This table is essential for ensuring accurate reporting and analysis of financial performance by linking units to their respective billable departments and external classifications.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column represents the identifier for departments that are eligible for billing, facilitating the tracking of billable units within the organization. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column contains the names of organizational units, serving to categorize and enrich data related to various departments or divisions within the organization. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column contains string values that represent external type identifiers associated with ownership subclasses in the 'lkp_Unit' table, facilitating data enrichment processes. |
| `Unit` | `string` | The 'Unit' column contains string values representing the various measurement units used for data enrichment purposes. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, leadership, and status. This table enables businesses to effectively manage and analyze their projects by providing insights into project categorization, progress, and financial considerations.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and performance analysis within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column contains string values that represent the administrative details or oversight responsibilities associated with each project in the tbl_Project table. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates within the 'tbl_Project' table. |
| `ClientName` | `string` | The 'ClientName' column stores the name of the client associated with each project, facilitating identification and management of client-specific projects within the tbl_Project table. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating the tracking and management of client-related data within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated in the 'tbl_Project' table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project in the tbl_Project table. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the exact date and time when each project entry was created in the 'tbl_Project' table. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or business unit associated with each project in the 'tbl_Project' table. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column indicates the scheduled date and time for the completion of the project, facilitating effective project timeline management and tracking. |
| `Project` | `string` | The 'Project' column contains the names or identifiers of various projects, serving as a key reference for associating enriched data with specific initiatives within the 'tbl_Project' table. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed textual overview of each project, providing essential context and objectives to enhance understanding and management of project-related data. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the specific category or classification of a project within the 'tbl_Project' table, facilitating organization and management of related projects. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a textual summary of the project group, detailing its objectives and scope within the context of the overall project management framework. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary key for data integrity and relational mapping. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the associated project within the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency sales price of the project, stored as a decimal value for precise financial calculations. |
| `Startdate` | `dateTime` | The 'Startdate' column records the date and time when a project is initiated, serving as a key reference point for project timelines and scheduling within the 'tbl_Project' table. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, allowing for effective tracking and management of project timelines and deliverables. |
| `Statuscode` | `string` | The 'Statuscode' column indicates the current operational status of each project, represented as a string, to facilitate tracking and management of project progress within the 'tbl_Project' table. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, stored as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table is designed to identify and track employees who have not submitted their timesheets for specific weeks, providing insights into potential compliance issues and resource management. It includes key details such as employee and manager information, contract dates, and hours, enabling managers to address gaps in timesheet submissions effectively.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by an employee that have not been recorded in their timesheet. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract is set to expire, providing critical information for managing timesheet compliance and workforce planning within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to assist in identifying discrepancies in timesheet submissions within the 'vw_missing_timesheet' view. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract associated with each entry in the 'vw_missing_timesheet' table, providing essential context for understanding the contractual obligations of employees with missing timesheets. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet submissions. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on outstanding time reporting issues. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours recorded for each employee's timesheet entry, facilitating the identification of discrepancies in reported work hours. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_missing_timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and tracking of missing submissions. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's manager, providing context for accountability and oversight related to missing timesheet entries. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential gaps in employee time tracking. |
| `Project` | `string` | The 'Project' column contains the names of projects associated with missing timesheet entries, enabling identification and tracking of unreported work activities. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project associated with the timesheet entries, facilitating the tracking and management of project-related hours. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness and false indicating it is not yet prepared. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total monetary value of sales transactions associated with missing timesheet entries, recorded as a double for precision in financial calculations. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double data type, to facilitate accurate financial reporting and analysis within the 'vw_missing_timesheet' table. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with missing timesheet entries, formatted for concise reporting and analysis. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific sub-department or team within the organization associated with each entry in the 'vw_missing_timesheet' table, facilitating targeted analysis of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet entry, facilitating the tracking and management of missing timesheet records. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the timesheet entries, providing context and details about the work performed during the reported period. |
| `Week_` | `int64` | The 'Week_' column (int64) represents the numerical week of the year associated with each entry in the 'vw_missing_timesheet' table, facilitating the identification and analysis of timesheet data by week. |
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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'BillableDep'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'BillableDep' column from the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on the relationships defined in your data model.

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of specific information, the default assumption is that the item is billable.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that the item is considered billable.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that the item is not billable.

In summary, this DAX expression effectively categorizes items as billable or not based on the related 'BillableDep' value. If there is no related data, it defaults to billable (1). If there is related data and it is non-zero, it also counts as billable (1). If the related data is zero, it is marked as not billable (0).

**`CC_ActiveEmployees`** (`string`)

- **Description:** The 'CC_ActiveEmployees' column contains a string representation of the active employees in the cost center, used to identify personnel who may be missing timesheet submissions in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
IF(vw_missing_timesheet[ShortDate] >= vw_missing_timesheet[ContractStartDate] && 
			      (vw_missing_timesheet[ShortDate] <= vw_missing_timesheet[ContractEndDate] || ISBLANK(vw_missing_timesheet[ContractEndDate]))
			        , 1
			        , 0
			    )
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'CC_ActiveEmployees' in a data table called 'vw_missing_timesheet'. Here's what it does in simple business terms:

1. **Purpose**: The expression checks whether an employee is considered "active" based on their contract dates.

2. **Conditions**:
   - It first checks if the date in the column `ShortDate` is on or after the employee's `ContractStartDate`. This means the employee's contract has started.
   - Then, it checks if the `ShortDate` is on or before the `ContractEndDate`, which means the employee's contract is still valid. Alternatively, if the `ContractEndDate` is blank (meaning the contract does not have an end date), the employee is also considered active.

3. **Output**:
   - If both conditions are met (the employee's contract is active), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns a value of `0`, indicating that the employee is not active.

In summary, this DAX expression effectively identifies whether an employee is currently active based on their contract start and end dates, providing a simple binary output (1 for active, 0 for not active).

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to assist in identifying records that require further attention in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IncompleteFlag' in a data table. Here's what it does in simple business terms:

- The expression checks the value in the 'MissingHours' column of the 'vw_missing_timesheet' table.
- If the value of 'MissingHours' is less than 0, it means that there is an issue (for example, it could indicate that there are negative hours reported, which is not valid).
- In this case, the expression will return a value of 1, which can be interpreted as a flag indicating that there is an incompleteness or error.
- If the value of 'MissingHours' is 0 or greater, the expression will return a value of 0, indicating that there is no issue.

In summary, this calculated column flags records with negative 'MissingHours' as problematic (1 for incomplete) and those with zero or positive hours as fine (0 for complete). This helps in identifying and addressing any discrepancies in the timesheet data.

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

1. **Check for Related Data**: The expression first checks if there is a related value in the 'Unit' column of the 'lkp_Unit' table. This is done using the `RELATED` function, which retrieves a value from a related table based on the relationship defined in the data model.

2. **Handle Missing Data**: If the related 'Unit' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression will return the text "Unknown". This is useful for identifying records where the unit information is missing.

3. **Return Valid Data**: If there is a valid related 'Unit' value (i.e., it is not blank), the expression will return that value instead.

In summary, this DAX expression effectively ensures that every record in the 'MAIN_UNIT' column either shows the corresponding unit name from the 'lkp_Unit' table or indicates "Unknown" if no unit information is available. This helps maintain clarity and completeness in the data by addressing potential gaps in unit information.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column represents the year and week number in a string format, facilitating the identification and analysis of timesheet data by specific weekly periods.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX expression is used to create a new calculated column called 'YearWeek' based on the 'ContractEndDate' field. Here's a breakdown of what it does:

1. **YEAR([ContractEndDate])**: This part extracts the year from the 'ContractEndDate'. For example, if the date is December 15, 2023, this will return "2023".

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the 'ContractEndDate'. For instance, if the date is in the 15th week of the year, this will return "15".

3. **FORMAT(..., "00")**: This ensures that the week number is always displayed as a two-digit number. So, if the week number is 5, it will be formatted as "05".

4. **Concatenation (&)**: The expression combines the year and the formatted week number into a single string, separated by a hyphen. So, if the year is 2023 and the week number is 5, the final result will be "2023-05".

In summary, this DAX expression creates a 'YearWeek' column that represents the year and the week number of the 'ContractEndDate' in a "YYYY-WW" format. This is useful for reporting and analyzing data on a weekly basis within a specific year.

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

1. **Purpose**: The measure counts the number of unique employees (consultants) who have recorded negative hours in their timesheets. Negative hours typically indicate that there was an error or a correction in the reported hours worked.

2. **Components**:
   - **`CALCULATE`**: This function modifies the context in which the data is evaluated. It allows us to apply specific filters to our calculations.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the unique names of employees from the 'vw_missing_timesheet' table. It ensures that each employee is only counted once, even if they have multiple entries with negative hours.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This filter condition specifies that we are only interested in records where the 'MissingHours' value is less than zero. This means we are focusing solely on instances where employees have reported negative hours.

3. **Outcome**: The final result of this measure is the total number of distinct employees who have negative hours recorded in their timesheets. This information can be useful for management to identify potential issues with timesheet reporting or to investigate why certain consultants have negative hours.

In summary, this DAX expression helps organizations track and analyze the number of consultants with negative timesheet entries, which can be critical for maintaining accurate records and addressing any discrepancies in reported work hours.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have recorded negative missing hours in a dataset. Here's a breakdown of what it does in simple business terms:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This counts the number of unique values in a specified column. Here, it counts unique Employee IDs.

3. **'vw_missing_timesheet'[EmployeeID]**: This refers to the column in the dataset that contains the IDs of employees.

4. **Filter Condition**: The expression includes a filter that only considers records where the 'MissingHours' value is less than 0. This means it focuses on instances where employees have negative missing hours.

In summary, this measure calculates how many different employees have negative missing hours recorded in the 'vw_missing_timesheet' data. This could be useful for identifying employees who may have discrepancies in their reported hours or for analyzing patterns in missing timesheets.

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

1. **Data Source**: The expression starts by looking at a data table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including hours worked and contracted hours.

2. **Summarization**: It summarizes the data by grouping it based on several key attributes:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a more specific division within the unit)

   For each group, it calculates two important values:
   - **MissingHours**: This is calculated by taking the total hours recorded in the timesheet for that group and subtracting the maximum contracted hours for that employee. If this value is negative, it indicates that the employee has not logged enough hours compared to what they are contracted for.
   - **MinActiveEmp**: This checks if the employee is currently active (with a value of 1 indicating active status).

3. **Filtering**: After summarizing the data, the expression filters the results to only include those groups where:
   - The employee is active (`MinActiveEmp` equals 1).
   - The `MissingHours` is less than 0, meaning they have not logged enough hours.

4. **Counting**: Finally, the expression counts the number of rows that meet these criteria, which represents the total number of active employees who have missing timesheet hours.

In summary, this DAX measure calculates how many active employees have not logged enough hours in their timesheets, helping the business identify potential issues with timesheet compliance or employee workload.

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

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. It filters a summarized view of a dataset called `vw_missing_timesheet`, which contains information about employees, their working hours, and their contracts.

2. **Summarization**: Within this summarized view, it groups the data by:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a more specific division within the unit)

   For each group, it calculates two key pieces of information:
   - **MissingHours**: This is calculated by taking the total hours worked (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contracted hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has not worked enough hours compared to what they were contracted for.
   - **MinActiveEmp**: This checks if the employee is active by finding the minimum value of a field that indicates whether the employee is currently active (`vw_missing_timesheet[CC_ActiveEmployees]`). A value of 1 means the employee is active.

3. **Filtering**: The `FILTER` function then narrows down the summarized data to only include:
   - Employees who are active (`[MinActiveEmp] = 1`)
   - Employees who have missing hours (i.e., those with a negative `MissingHours` value).

4. **Final Calculation**: Finally, the `RETURN` statement uses `SUMX` to sum up the `MissingHours` for all the filtered active employees. This gives the total number of hours that active employees are missing compared to their contracted hours.

In summary, this DAX measure calculates the total hours that active employees have not worked compared to what they were supposed to work, helping the business identify potential gaps in employee attendance or productivity.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total expected contract hours for employees on a weekly basis. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: It uses a data table called `vw_missing_timesheet`, which likely contains records of timesheets for employees, including their contract hours and the week associated with each entry.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key pieces of information:
   - The week (`vw_missing_timesheet[Week_]`)
   - The employee ID (`vw_missing_timesheet[EmployeeID]`)

   For each unique combination of week and employee, it calculates the maximum contract hours (`MAX(vw_missing_timesheet[ContractHours])`). This means if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

3. **Calculating Total**: After summarizing the data, the `SUMX` function iterates over this summarized table and adds up all the maximum contract hours calculated for each employee in each week. 

4. **Final Result**: The final result of this measure is the total expected contract hours for all employees across all weeks, considering only the highest contract hours recorded for each employee in each week.

In summary, this DAX measure helps to understand how many contract hours are expected from employees on a weekly basis, ensuring that if there are multiple records for an employee in a week, only the highest value is considered.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked and contracted hours for employees or contractors. Here’s a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum number of hours that are contracted for a specific employee or contractor from the 'vw_missing_timesheet' table. Contracted hours refer to the total hours that an employee is expected to work as per their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum number of hours that the employee or contractor has actually recorded or worked, also from the same table.

3. **Subtraction**: The expression then subtracts the maximum recorded hours (actual hours worked) from the maximum contracted hours. 

**What it achieves**: The result of this calculation gives you the number of hours that are missing or unaccounted for. In other words, it shows how many hours the employee or contractor has not worked compared to what they were supposed to work according to their contract. 

This measure is useful for identifying gaps in hours worked, which can help in managing workforce efficiency, ensuring compliance with contracts, and addressing any potential issues with timesheet submissions.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of completeness regarding missing hours in relation to expected contract hours for a given period, typically a week. Here’s a breakdown of what it does:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part sums up all the missing hours recorded in the `vw_missing_timesheet` table. Essentially, it gives the total number of hours that were not logged as timesheets.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the expected number of hours an employee should work in a week according to their contract. 

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: This calculation finds the difference between the total missing hours and the expected contract hours. If the result is positive, it indicates that the missing hours exceed what was expected.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The `DIVIDE` function takes the result from the previous step (the difference) and divides it by the expected contract hours. This gives a ratio of how much the missing hours exceed the expected hours. The third argument, `0`, ensures that if the expected contract hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

In summary, this measure calculates how much the missing hours deviate from the expected hours as a percentage. A positive result indicates a shortfall in hours worked compared to what was expected, while a negative result would suggest that the expected hours were met or exceeded. This measure helps in assessing employee time management and compliance with work hour expectations.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total number of unique employees who have missing timesheets, grouped by their respective units and weeks. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The expression uses a table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including whether their submissions are complete or incomplete.

2. **Grouping**: The `SUMMARIZE` function is used to create a summary table. This summary groups the data by three key columns:
   - `Week_`: The week of the year.
   - `EmployeeName`: The name of the employee.
   - `MAIN_UNIT`: The main unit or department the employee belongs to.

3. **IncompleteFlag**: For each group created by the `SUMMARIZE` function, it calculates a new column called "IncompleteFlag". This column uses the `MAX` function to determine if there is any record in that group indicating that the employee has an incomplete timesheet. If at least one record shows an incomplete timesheet, the flag will be set to 1 (true); otherwise, it will be 0 (false).

4. **Summation**: Finally, the `SUMX` function iterates over the summarized table and sums up the values in the "IncompleteFlag" column. This effectively counts how many unique employees have at least one instance of an incomplete timesheet for each week and unit.

In summary, this measure helps the business track how many unique employees are not submitting their timesheets correctly, broken down by week and unit. This information can be crucial for identifying compliance issues and ensuring that all employees are meeting their timesheet obligations.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This table enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column identifies the name of the client associated with each timesheet entry, facilitating the tracking and reporting of billable hours by client. |
| `Code2` | `string` | The 'Code2' column stores a secondary identifier or classification code related to timesheet entries, facilitating enhanced data categorization and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract is set to expire, providing essential information for managing workforce planning and contract renewals within the timesheet data. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when a contract begins, providing essential context for timesheet entries related to specific contractual agreements. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract associated with the timesheet entry, represented as a string value. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, represented as a string, to facilitate real-time reporting and analysis within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential temporal context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the monetary value associated with the hours worked, stored as a double to accommodate precise financial calculations within the timesheet data. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, stored as a double to allow for precise financial calculations. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions recorded in the timesheet, represented as a string. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the individual or entity responsible for payment, providing essential identification for financial tracking within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee associated with the timesheet entries in the 'vw_Timesheet' table, facilitating accurate tracking and reporting of work hours. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column contains the unique identifier and name of each employee, facilitating the association of timesheet entries with specific personnel in the vw_Timesheet table. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating identification and reporting of labor hours. |
| `Employer` | `string` | The 'Employer' column contains the name of the organization or company that employs the individual associated with the timesheet entries. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating the tracking and management of employee work hours by employer. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to the timesheet entries, providing further insights into the recorded hours or activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with the recorded timesheet entries, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, facilitating accurate tracking of labor input for payroll and project management. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating analysis of labor allocation and productivity. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee work hours. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee work hours. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and management. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries, facilitating accountability and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the names of the IPM IDs associated with each timesheet entry, facilitating the identification and tracking of project management metrics. |
| `JobTitle` | `string` | The 'JobTitle' column contains the titles of positions held by employees, providing context for their roles in relation to the timesheet data. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking records within the 'vw_Timesheet' table to other related datasets, facilitating data integration and analysis. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier for the manager associated with each timesheet entry, facilitating the tracking and management of employee work hours and approvals. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, facilitating the identification of supervisory oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key associated with the manager responsible for overseeing the timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct manager, facilitating the identification of supervisory relationships within the timesheet data. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries, detailing the hours and tasks allocated to employees within the timesheet records. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each timesheet entry, allowing for detailed tracking and reporting of time spent on various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with the timesheet entries, facilitating the tracking and reporting of time spent on specific projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that represents the specific characteristics or details of the project associated with each timesheet entry, facilitating better project management and resource allocation. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the specific project profile associated with each timesheet entry, facilitating project tracking and reporting. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into the types of work being performed. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected (true) or accepted (false) during the review process. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of timesheet entries for accurate financial analysis and reporting. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current state of the timesheet entry, reflecting whether it is submitted, approved, or pending review. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation, with a value of true representing taxed hours and false representing non-taxed hours. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, providing insights into resource allocation and project management. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours and associated billing. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the tasks or activities recorded in the timesheet, providing context and details for each entry. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table specifies the measurement unit associated with the recorded time entries, facilitating accurate tracking and reporting of work hours. |
| `UnitCode` | `string` | The 'UnitCode' column represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating the categorization and analysis of labor data by specific departments or teams. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and record-keeping within the timesheet data. |
| `YearWeek` | `string` | The 'YearWeek' column represents the year and week number in a string format, facilitating time-based analysis and reporting within the timesheet data. |

##### Calculated Columns

**`Approved_With_V_Z`** (`string`)

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheets, specifically denoting whether the approval was granted with a specific condition or criteria labeled as 'V_Z'.
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

2. **Result**: 
   - If either of the conditions is met (meaning the item is approved or it has a timesheet code of "V" or "Z"), the expression returns a value of `1`. This indicates a positive outcome, suggesting that the item is considered approved under the defined criteria.
   - If neither condition is met, it returns a value of `0`, indicating that the item does not meet the approval criteria.

In summary, this DAX expression effectively flags items as approved (with a value of `1`) if they are either explicitly approved or fall under specific timesheet codes ("V" or "Z"). If not, it marks them as not approved (with a value of `0`). This can help in reporting or filtering data based on approval status.

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

1. **Check for Blank Values**: The expression first checks if the value in the 'BillableDep' column from a related table called 'lkp_Unit' is blank (i.e., there is no value). 

2. **Return 1 for Blank Values**: If the 'BillableDep' value is blank, the expression returns a value of 1. This indicates that when there is no information available about whether the unit is billable, it defaults to being considered billable.

3. **Check for Non-Zero Values**: If the 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. 

4. **Return 1 for Non-Zero Values**: If the 'BillableDep' value is indeed not zero, it returns 1, indicating that the unit is billable.

5. **Return 0 for Zero Values**: If the 'BillableDep' value is zero, the expression returns 0, indicating that the unit is not billable.

In summary, this DAX expression effectively categorizes units as billable (returns 1) or not billable (returns 0) based on the presence and value of the 'BillableDep' field from the related 'lkp_Unit' table. If there's no information (blank), it assumes the unit is billable. If there is information, it checks if it's a non-zero value to determine if it's billable.

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

1. **Purpose**: The expression determines whether a particular timesheet entry is considered "billable" or not. A billable entry is one that can be charged to a client or project.

2. **Conditions Checked**:
   - **Sales Price**: It first checks if the `SalesPrice` for the entry is greater than 0. If there is a positive sales price, it indicates that the entry can potentially generate revenue.
   - **Project Profile Code**: Next, it checks if the `ProjectProfileCode` is equal to 81. This specific code likely represents a type of project that is always billable.
   - **Project Name**: Finally, it looks for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it suggests that the project is related to customer success, which is also considered billable.

3. **Output**: 
   - If any of the above conditions are true (i.e., if the sales price is positive, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of **1**. This indicates that the timesheet entry is billable.
   - If none of these conditions are met, it returns **0**, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project name, helping the business identify which entries can be charged to clients.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column stores the name of the employer associated with the timesheet entry, facilitating the identification of the organization responsible for the recorded work hours.

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

3. **Combining Information**: The expression then combines these two pieces of information into one string. It formats it so that the employee's name is followed by a dash (" - ") and then the number of hours they work each week.

For example, if an employee named "John Doe" works 40 hours per week, the resulting string in the 'Employee_WeeklyHours' column would be "John Doe - 40".

In summary, this DAX expression effectively creates a clear and concise representation of each employee's name along with their weekly working hours, making it easier to read and understand their work schedule at a glance.

**`GroupCat`** (`string`)

- **Description:** The 'GroupCat' column categorizes timesheet entries into specific groups, facilitating organized reporting and analysis of time allocation across various projects or activities.
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

1. **Lookup for Group**: The expression first tries to find a corresponding 'Group' value from a lookup table called `lkp_Project`. It does this by checking if there is a project in the `vw_Timesheet` table that matches a project in the `lkp_Project` table.

2. **Check for Existence**: The `NOT(ISBLANK(...))` part checks if the lookup returned a valid 'Group' value. If it finds a match (meaning the project exists in the lookup table), it will use that 'Group' value for the 'GroupCat' column.

3. **Fallback Logic**: If the lookup does not return a valid 'Group' (meaning the project does not exist in the `lkp_Project` table), the expression then checks if the project is billable. This is done by looking at the `BillablePrj` column in the `vw_Timesheet` table:
   - If `BillablePrj` equals 1 (indicating that the project is billable), it assigns the value "Billable" to the 'GroupCat' column.
   - If `BillablePrj` does not equal 1 (indicating that the project is not billable), it assigns the value "Other Unbillable".

**In summary**, this DAX expression categorizes each project in the `vw_Timesheet` table into a 'GroupCat' based on whether it has a corresponding group in the `lkp_Project` table. If it does, it uses that group; if not, it checks if the project is billable and categorizes it as either "Billable" or "Other Unbillable". This helps in organizing and analyzing projects based on their billing status and group affiliation.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IsContractActive' in a data table named 'vw_Timesheet'. Here's what it does in simple business terms:

1. **Check Contract End Date**: The expression first checks if the 'ContractEndDate' for a given record is blank (meaning there is no end date specified) or if the end date is in the future (greater than today's date).

2. **Determine Contract Status**:
   - If either of those conditions is true (the contract has no end date or it ends after today), the expression will return "Active". This indicates that the contract is currently valid and ongoing.
   - If neither condition is true (meaning there is an end date and it is in the past), the expression will return "Not Active". This indicates that the contract has expired.

In summary, this DAX code helps to identify whether a contract is currently active or not based on its end date, providing valuable information for managing contracts effectively.

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

1. **Check for Related Data**: The expression first checks if there is a related value in the 'lkp_Unit' table for the current row. Specifically, it looks for the 'Unit' field in that related table.

2. **Handle Missing Data**: If there is no related 'Unit' value (meaning it's blank or missing), the expression will return the text "Unknown". This is a way to handle situations where the data might not be complete.

3. **Return the Unit Value**: If there is a related 'Unit' value available, the expression will return that value instead.

In summary, this DAX code is designed to ensure that every row in the 'MAIN_UNIT' column has a meaningful value. If the related unit information is missing, it labels it as "Unknown"; otherwise, it shows the actual unit value. This helps maintain clarity and completeness in the data analysis.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column stores the numerical representation of the month (e.g., "01" for January, "02" for February) as a string, facilitating time-based analysis and reporting within the 'vw_Timesheet' table.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to extract the month number from a date value found in the column named `TimesheetDate`. 

In simple terms, this means that for each entry in the `TimesheetDate` column, the expression will return a number that represents the month of that date. For example, if the date is January 15, 2023, the expression will return `1`, since January is the first month of the year. If the date is July 10, 2023, it will return `7`, as July is the seventh month.

This calculated column, therefore, helps in organizing or analyzing data by month, making it easier to summarize or report on timesheet entries based on the month they occurred.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the employee identification number as a string, facilitating the association of timesheet entries with specific employees in the 'vw_Timesheet' view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

In simple business terms, this means that the expression counts how many different employees have recorded their timesheets, without counting any employee more than once. For example, if three employees submitted timesheets, but one of them submitted multiple times, this expression would still return a count of three, reflecting the unique individuals who contributed to the timesheet data. 

This is useful for understanding workforce participation or engagement, as it provides a clear picture of how many distinct employees are involved in the timesheet submissions.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table categorizes the type of time entry as either owned, subcontracted, or external, facilitating the analysis of resource allocation and project costs.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used to retrieve a specific value from a related table in a data model. Here's a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a calculated column within a table that has a relationship with another table. In this case, the current table is likely related to the `lkp_Unit` table.

2. **Purpose**: The expression is designed to fetch the value from the `OWN-Sub-ExtT` column of the `lkp_Unit` table. This means that for each row in the current table, it looks up the corresponding value in the `lkp_Unit` table based on the established relationship.

3. **Outcome**: By using this expression, you can enrich your current table with additional information from the `lkp_Unit` table. Specifically, it allows you to pull in the `OWN-Sub-ExtT` data, which could represent a specific attribute or classification related to the units in your dataset.

In summary, this DAX expression helps to enhance your data analysis by linking and integrating relevant information from different tables, making it easier to derive insights and make informed business decisions.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'QualifyPrj'. Here's a simple breakdown of what it does:

1. **Check for Blank Value**: The expression first checks if the 'Qualify' field from a related table called 'lkp_Project' is blank (i.e., it has no value).

2. **Return Value Based on Check**:
   - If the 'Qualify' value is blank, the expression returns **1**. This could indicate a default or fallback value, suggesting that if there is no qualification information available, it defaults to a value of 1.
   - If the 'Qualify' value is not blank, it returns the actual value from the 'Qualify' field in the 'lkp_Project' table.

**In summary**, this DAX expression is designed to ensure that every row in the 'QualifyPrj' column has a value. If there is no qualification data available for a project, it assigns a default value of 1; otherwise, it uses the existing qualification value from the related project data. This helps maintain consistency and ensures that there are no missing values in the 'QualifyPrj' column.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor allocation.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of resources in relation to the project, represented as a string value within the timesheet data.
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
   - It first checks if the value of the column `[ReportReady]` is `TRUE`. This means that the report is ready for some action or processing.
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These codes likely represent specific types of timesheets or statuses.

2. **Output Values**: 
   - If either of the conditions is met (i.e., if the report is ready or if the timesheet code is "V" or "Z"), the expression returns a value of `1`. This indicates a positive condition where the report is ready or the timesheet is of a specific type.
   - If neither condition is met, it returns a value of `0`. This indicates that the report is not ready and the timesheet code is not "V" or "Z".

3. **Purpose**: The overall purpose of this calculated column is to flag records based on their readiness status or specific timesheet codes. A value of `1` signifies that the record is either ready for reporting or falls under the specified timesheet categories, while a `0` indicates otherwise. This can be useful for filtering or analyzing data based on readiness and specific timesheet types in reports or dashboards.

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

1. **Condition Check**: The expression first checks the value of another measure called `[ContractStatusMeasure]`. It specifically looks to see if this measure equals "Active".

2. **Return Value Based on Condition**:
   - If the condition is true (meaning the contract is indeed "Active"), it returns the value of another measure called `[HoursDifference]`. This likely represents the difference in hours related to the contract, such as hours worked or hours remaining.
   - If the condition is false (meaning the contract is "Not Active"), it returns the text "Not Active".

**Overall Purpose**: This measure essentially provides a way to display either the hours difference for active contracts or a simple message indicating that the contract is not active. It helps users quickly understand the status of contracts and their associated hours.

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

1. **Check for Contract End Date**: The expression first looks at the maximum value of the 'ContractEndDate' from the 'vw_Timesheet' table. This means it is checking the latest end date of contracts recorded.

2. **Evaluate Conditions**:
   - **ISBLANK**: It checks if this maximum end date is blank (meaning there is no end date recorded for the contract).
   - **Comparison with Today**: It also checks if this maximum end date is greater than today's date. This means it is looking to see if the contract is still valid and has not yet ended.

3. **Determine Status**:
   - If either of the above conditions is true (the end date is blank or the end date is in the future), the expression returns "Active". This indicates that the contract is currently active and valid.
   - If neither condition is true (the end date is not blank and it is in the past), it returns "Not Active". This means the contract has ended.

In summary, this measure effectively categorizes contracts as either "Active" or "Not Active" based on their end dates, helping users quickly understand the status of contracts in the system.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 10, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. Week numbers typically start from 1 for the first week of the year.

So, when you combine these two functions, `WEEKNUM(TODAY())` calculates which week of the year it currently is. For instance, if today is October 10, 2023, and it falls in the 41st week of the year, the expression will return the number 41.

In summary, this DAX expression helps you identify the current week number in the context of the calendar year, which can be useful for reporting and analysis purposes, such as tracking weekly performance or trends.

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

3. **Filter Condition**: The expression includes a filter condition that specifies `vw_Timesheet[Approved] = FALSE()`. This means that the calculation will only consider timesheets that have not been approved (i.e., where the `Approved` field is marked as FALSE).

In summary, this DAX measure calculates the total number of unique employees who have submitted timesheets that are still pending approval. This information can be useful for tracking outstanding approvals and managing workflow within the organization.

**`Dax_EmpCount_RReady`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[ReportReady] = FALSE()
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees whose timesheet reports are not ready. Here's a breakdown of what it does:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it is counting unique employee names from the `vw_Timesheet` table.

3. **Filter Condition**: The expression includes a filter condition that specifies `vw_Timesheet[ReportReady] = FALSE()`. This means that the calculation will only consider those records where the `ReportReady` status is marked as FALSE, indicating that the timesheet reports are not ready.

In summary, this DAX measure calculates the total number of unique employees who have timesheet reports that are still pending or not ready. This information can be useful for tracking employee compliance with timesheet submissions and identifying those who may need follow-up.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheet entries in the `vw_Timesheet` table.

In simple business terms, this measure counts how many different employees are represented in the timesheet data. If an employee has submitted multiple timesheets, they will only be counted once. This helps organizations understand the total number of distinct employees contributing to work, which can be useful for reporting, resource management, and analyzing workforce engagement.

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

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the timesheet. Essentially, it totals the revenue generated from sales during a specific period.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the timesheet. It gives the total number of hours that employees or team members have worked during the same period.

3. **Division**: The expression then divides the total sales amount by the total hours worked. This calculation gives you the average hourly rate, which tells you how much revenue is generated for each hour worked.

In summary, this DAX measure calculates the average revenue earned per hour worked, helping businesses understand their efficiency and productivity in generating sales relative to the time invested.

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
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called 'Msr_Budget' that calculates a budget value based on the current week in the context of a report or dashboard. Here's a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: The measure starts by determining which week of the year is currently selected in the report. This is done using the `SELECTEDVALUE` function on the `DimDate[WeekNumberOfYear]` column. The result is stored in the variable `CurrentWeek`.

2. **Calculate Previous Budget**: Next, the measure calculates the budget for all previous weeks (i.e., weeks before the current week). This is done using the `CALCULATE` function, which modifies the context of the calculation. It uses the measure `[Msr_SalesPriceBaseCurrency]` to get the sales price in the base currency for those previous weeks. The `FILTER` function is applied to the `DimDate` table to include only those weeks where the week number is less than `CurrentWeek`. The `ALLSELECTED` function ensures that any filters applied to the date dimension are respected, but it still allows for the calculation of previous weeks.

3. **Return the Appropriate Value**: Finally, the measure checks if the current week's sales price (i.e., `[Msr_SalesPriceBaseCurrency]`) is not blank. If it has a value, it returns that value. If it is blank (meaning there are no sales for the current week), it returns the calculated previous budget value instead.

In summary, this measure effectively provides the sales price for the current week if available; otherwise, it falls back to the budget from previous weeks. This is useful for analyzing performance against budgeted figures, ensuring that users have relevant data to make informed decisions.

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

1. **Identify the Selected Month**: The code starts by determining which month is currently selected in the visual. It uses the `MAX` function on the `MonthNumberOfYear` column from the `DimDate` table to find the highest month number that has been selected. This means if you are looking at data for March, the `SelectedMonth` would be 3.

2. **Calculate Cumulative Cost**: The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function changes the context in which the data is evaluated. It sums up the `CostAmount` from the `vw_Timesheet` table, but it does so under specific conditions defined in the next step.

4. **Filtering the Date Context**: The `FILTER` function is used to create a new context for the calculation. It looks at all the dates in the `DimDate` table (ignoring any filters that might be applied to it) and includes only those months where the `MonthNumberOfYear` is less than or equal to the `SelectedMonth`. This means if March is selected, it will consider January, February, and March for the cumulative total.

5. **Final Result**: The result of this measure is the total cost from the `vw_Timesheet` table for all months up to and including the month that is currently selected in the visual. This allows users to see how costs accumulate over time, providing valuable insights into spending trends.

In summary, this DAX measure helps businesses track and visualize their cumulative costs over a specified period, making it easier to analyze financial performance month by month.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the cumulative sales amount up to a selected month in a report or dashboard. Here's a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The code starts by determining which month is currently selected in the visual (like a chart or table). It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed.

2. **Calculate Cumulative Sales**: The main goal of this measure is to calculate the total sales amount from the 'vw_Timesheet' table for all months up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to modify the context in which the sales data is being evaluated. It allows us to sum up the sales amounts while applying specific filters.

4. **Filtering the Date Context**: The `FILTER` function is applied to the 'DimDate' table, using `ALLSELECTED` to ensure that it considers all the months that are currently selected in the report. The filter condition checks that the month number is less than or equal to the selected month. This means it includes all sales from the beginning of the year up to the selected month.

5. **Final Result**: The result of this measure is the total sales amount for all months leading up to and including the month that the user has selected in the visual. This allows users to see how sales have accumulated over time, providing insights into sales performance and trends.

In summary, this DAX measure helps businesses track cumulative sales performance over time, making it easier to analyze trends and make informed decisions based on sales data.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the percentage of hours worked by a specific group, project, or employee in relation to the total hours worked across all selected categories. Here’s a breakdown of what it does:

1. **TotalValue Calculation**: 
   - The first part of the code creates a variable called `TotalValue`. This variable calculates the total number of hours from the `vw_Timesheet` table.
   - The `CALCULATE` function is used to sum up the `Hours` column, but it does so while ignoring any filters that might be applied to the `GroupCat`, `Project`, or `EmployeeName` columns. This means it considers all hours worked across these categories, regardless of any selections made in a report or dashboard.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours worked by the current context (which could be a specific group, project, or employee).
   - It does this by taking the sum of hours for the current context (using `SUM(vw_Timesheet[Hours])`) and dividing it by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safe way to handle division in DAX. If the `TotalValue` is zero (meaning there are no hours to divide by), it will return 0 instead of causing an error.

In summary, this measure calculates what percentage of the total hours worked (across all selected categories) is represented by the hours worked in the current context (like a specific employee or project). This helps in understanding how much of the total effort is contributed by a particular segment of the data.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the profit margin for a project by subtracting the total costs from the total sales. Here’s a breakdown of what it does:

1. **Total Sales Calculation**: The expression starts with `sum(vw_Timesheet[SalesAmount])`, which adds up all the sales amounts recorded in the `vw_Timesheet` table. This represents the total revenue generated from the project.

2. **Total Costs Calculation**: Next, it calculates the total costs with `sum(vw_Timesheet[CostAmount])`. This sums up all the costs associated with the project, which could include expenses like labor, materials, and other overheads.

3. **Profit Margin Calculation**: Finally, the expression subtracts the total costs from the total sales: `sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`. The result of this calculation gives you the project margin, which indicates how much profit the project has generated after covering its costs.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called `Msr_SalesPriceBaseCurrency`, which essentially sums up the sales price in the base currency for different projects and clients from a timesheet view.

Here's a breakdown of what it does in simple business terms:

1. **SUMMARIZE Function**: The expression starts by creating a summary table from the `vw_Timesheet` data. This summary groups the data by two key dimensions: `ProjectProfile` and `Client`. This means it organizes the data so that each unique combination of project and client is represented.

2. **SELECTEDVALUE Function**: For each unique combination of project and client, it retrieves the corresponding sales price in the base currency from the `tbl_Project` table. The `SELECTEDVALUE` function is used here to get the sales price for the current context (i.e., the specific project and client being evaluated).

3. **MeasureValue**: The result of the `SELECTEDVALUE` function is stored in a new column called "MeasureValue" within the summarized table. This column holds the sales price for each project-client combination.

4. **SUMX Function**: Finally, the `SUMX` function iterates over the summarized table and sums up all the values in the "MeasureValue" column. This gives the total sales price in the base currency across all the unique project-client combinations.

In summary, this DAX expression calculates the total sales price in the base currency for all projects and clients listed in the timesheet, providing a clear view of revenue or sales performance based on the specified criteria.

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

1. **CALCULATE**: This function changes the context in which the data is evaluated. It allows us to apply filters to the data before performing calculations.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it ensures that each employee is only counted once, even if they appear multiple times in the data.

3. **ALL(vw_Timesheet)**: This function removes any existing filters on the `vw_Timesheet` table. By doing this, it ensures that the calculation considers all records in the table, regardless of any other filters that might be applied in the report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This is a filter condition that specifies we only want to include employees whose contract status is "Valid". It narrows down the data to only those employees who are currently active under valid contracts.

In summary, this DAX expression calculates the total number of unique employees who have a valid contract status, ignoring any other filters that might be applied to the data. This is useful for understanding the current workforce that is actively employed under valid contracts.

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

In simple terms, this measure takes all the hours logged in the `Hours` column of the `vw_Timesheet` table and adds them together to give a single total. This total represents the cumulative hours worked, which can be useful for reporting, payroll calculations, or project management purposes.

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

2. **MAX(vw_Timesheet[HoursPerWeek])**: For each employee in the unique list, this part finds the maximum number of hours they have recorded per week. This means if an employee has different entries for hours worked in a week, it will take the highest value.

3. **SUMX(...)**: The SUMX function then takes the unique list of employees and the maximum hours per week for each employee and sums them up. This gives the total contracted hours across all employees.

In summary, this measure calculates the total maximum contracted hours for all employees by looking at their highest recorded hours per week in the timesheet. It helps in understanding the total capacity of hours that employees are contracted to work, based on their highest weekly entries.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression `[TotalActualHours] - [TotalContractedHours]` calculates the difference between two measures: **Total Actual Hours** and **Total Contracted Hours**.

In simple business terms, here's what it achieves:

- **Total Actual Hours**: This represents the total number of hours that have actually been worked or logged by employees or resources on a project or task.
  
- **Total Contracted Hours**: This indicates the total number of hours that were agreed upon in a contract for the same project or task.

By subtracting **Total Contracted Hours** from **Total Actual Hours**, the expression provides a measure of how many hours have been exceeded or are underutilized compared to what was originally contracted. 

- If the result is positive, it means that more hours were worked than were contracted, indicating potential overwork or additional effort.
- If the result is negative, it suggests that fewer hours were worked than were contracted, which could indicate underperformance or efficiency.

Overall, this measure helps in assessing whether the actual work aligns with the contractual expectations, which is crucial for project management and resource allocation.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'TotalHoursTracked' that calculates the total number of hours recorded in a timesheet. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The expression first checks if the total sum of hours from the `vw_Timesheet[Hours]` column is blank (i.e., there are no hours recorded).

2. **Return Zero if Blank**: If the sum is blank (meaning no hours have been tracked), the measure will return a value of 0. This is important because it ensures that when there are no hours logged, the measure does not show a blank value, which could be confusing in reports.

3. **Return Total Hours if Not Blank**: If there are hours recorded (the sum is not blank), the measure will return the actual total sum of hours tracked.

In summary, this DAX expression ensures that the 'TotalHoursTracked' measure provides a clear and meaningful output: it will either show the total hours worked or a zero if no hours have been logged, making it easier for users to understand the data at a glance.

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

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the `Hours` column of the `vw_Timesheet` table. Essentially, it calculates the total hours that have been logged.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (meaning there are no hours recorded). If there are no hours, it indicates that the timesheet is empty.

3. **IF(...)**: The IF function evaluates the result of the ISBLANK check. 
   - If the total hours are blank (i.e., no hours have been logged), it returns the text "Empty".
   - If there are hours recorded, it returns the total sum of those hours.

In summary, this DAX expression helps to determine whether there are any hours tracked in the timesheet. If there are no hours, it clearly indicates that the timesheet is empty; if there are hours, it provides the total number of hours tracked. This is useful for reporting and understanding timesheet activity.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** The DAX expression you've provided calculates the difference between two values related to hours tracked for employees. Here's a breakdown of what it does in simple business terms:

1. **[TotalHoursTracked]**: This part represents the total number of hours that have been tracked for a specific employee or a group of employees. It is likely a measure that sums up all the hours recorded in a timesheet.

2. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to find a specific value related to hours worked.

3. **MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])**: This part calculates the maximum number of hours worked in a week from the distinct values of hours recorded in the timesheet. Essentially, it looks at all the unique weekly hour entries and finds the highest one.

4. **ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])**: This function removes all filters from the `vw_Timesheet` table except for the filter on `EmployeeID`. This means that the calculation of maximum hours per week will consider all records for the specific employee, ignoring any other filters that might be applied.

5. **Final Calculation**: The entire expression subtracts the maximum weekly hours (calculated in the previous step) from the total hours tracked. 

### What It Achieves:
In summary, this DAX measure calculates how many hours an employee has tracked beyond their maximum recorded hours in any single week. If the result is positive, it indicates that the employee has worked more hours than their highest weekly average; if it's negative or zero, it means they have not exceeded their maximum weekly hours. This can help in assessing workload and productivity for employees.

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

2. **Maximum Hours Per Week**: The next part of the expression calculates the maximum number of hours that have been recorded in a week for each employee. This is done using the `MAXX` function, which looks at the distinct values of `HoursPerWeek` from the `vw_Timesheet` table. Essentially, it finds the highest number of hours that any employee has logged in a single week.

3. **Removing Filters**: The `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])` part ensures that any filters applied to the `HoursPerWeek` column are ignored. This means that the calculation of maximum hours per week is done across all weeks, not just the weeks currently being filtered in the report.

4. **Keeping Employee Context**: The `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` function keeps the context of the specific employee while removing other filters. This means that the maximum hours per week is calculated for each employee individually, regardless of any other filters that might be applied to the data.

5. **Final Calculation**: Finally, the measure subtracts the maximum hours per week from the total hours tracked. This gives you the difference, which can indicate how many hours an employee has tracked beyond their maximum weekly hours or how much they have underperformed compared to their maximum.

In summary, `trackedDiff2` calculates how many hours an employee has tracked compared to their maximum logged hours in a week, providing insights into their performance relative to their peak productivity.

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

1. **Variable Definition (FilteredHours)**: The expression starts by defining a variable called `FilteredHours`. This variable is created by filtering the `vw_Timesheet` table based on specific conditions.

2. **Filtering Criteria**:
   - The first condition checks if the project qualifies as billable by ensuring that both `QualifyPrj` and `BillablePrj` columns are equal to 1. This means that only projects that are marked as both qualifying and billable will be included.
   - The second condition uses the `SEARCH` function to look for the phrase "Customer Success Services" within the `Project` column. If this phrase is found (indicated by a result greater than 0), those records will also be included in the filtered results.

3. **Summation of Hours**: After filtering the timesheet data based on the above criteria, the expression uses the `SUMX` function to sum up the `Hours` column from the filtered results. `SUMX` iterates over each row in the `FilteredHours` variable and adds up the hours for those rows that meet the filtering conditions.

4. **Return Value**: Finally, the expression returns the total of the billable hours that meet the specified criteria.

In summary, this DAX measure calculates the total number of hours worked on projects that are either marked as billable and qualifying or are specifically related to "Customer Success Services." This helps businesses track and report on billable work effectively.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'UTI_TOTALHOURS'. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression calculates the total number of hours recorded in the 'Hours' column of the 'vw_Timesheet' table. Essentially, it adds up all the hours worked.

2. **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to include only those entries where the 'QualifyPrj' column equals 1. This means that it only considers hours that are associated with projects that meet a specific qualification or criteria (indicated by the value 1).

3. **CALCULATE**: This function takes the sum of hours and applies the filter specified. It modifies the context in which the data is evaluated, ensuring that only the relevant hours (those linked to qualified projects) are included in the total.

In summary, the 'UTI_TOTALHOURS' measure calculates the total number of hours worked on projects that qualify under a certain criterion (where 'QualifyPrj' equals 1). This helps businesses understand how much time is being spent on specific projects that meet their qualification standards.

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
- **DAX Explanation (Generated):** This DAX expression calculates a measure called 'UTILIZATION_New', which is designed to assess the utilization rate of resources, typically in a business context where billable hours are tracked.

Here's a breakdown of what this code does:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable retrieves the total number of hours that are billable to clients, represented by the measure `[UTI_TotalBillableHours]`.
   - `_TotalHours`: This variable retrieves the total number of hours worked, represented by the measure `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The expression uses a series of conditional checks (IF statements) to ensure that certain conditions are met before performing the calculation.
   - **First Check**: It checks if there are any active contacts (`[Msr_ActiveContacts]`). If there are no active contacts (i.e., the measure is blank), the calculation does not proceed.
   - **Second Check**: If there are active contacts, it then checks if the total hours worked (`[UTI_TOTALHOURS]`) is not blank. If it is blank, the calculation does not proceed.
   - **Final Calculation**: If both previous checks are satisfied, it calculates the utilization rate by dividing the total billable hours by the total hours worked. If this division results in a blank (which can happen if total hours worked is zero), it returns 0 instead of a blank value.

**In Summary**: The 'UTILIZATION_New' measure calculates the ratio of billable hours to total hours worked, but only if there are active contacts and total hours worked is available. If any of these conditions are not met, it ensures that the measure returns 0 instead of a blank, providing a clear indication of utilization even in cases where data may be incomplete. This measure is useful for understanding how effectively resources are being utilized in generating billable work.

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


---

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


---

