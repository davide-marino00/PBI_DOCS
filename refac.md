# Power BI Model & Report Documentation

*Generated on: 2025-04-28 23:32:00*

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

The Power BI data model appears to be designed for a business environment focused on project management and timesheet tracking, likely within a professional services or consulting firm. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a strong emphasis on monitoring employee hours worked against various projects, as well as identifying any discrepancies in timesheet submissions. The inclusion of lookup tables like 'lkp_Project', 'lkp_Unit', and 'lkp_Code' indicates a structured approach to categorizing and analyzing data related to projects, organizational units, and timesheet codes, which can facilitate detailed reporting and performance analysis.

The model's structure, featuring a central 'vw_Timesheet' table linked to various dimensions, allows for comprehensive insights into employee productivity, project allocation, and resource management. The 'DimDate' table enhances the model by enabling time-based analysis, while the 'tbl_Project' table likely provides additional project-specific details. Overall, this data model is likely aimed at enhancing operational efficiency, ensuring accurate timesheet reporting, and supporting strategic decision-making through data-driven insights in a project-centric business domain.

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

The 'DimDate' table serves as a comprehensive date dimension that enables businesses to analyze and report on time-based data effectively. By providing detailed attributes such as day, week, month, and quarter, this table supports various time intelligence calculations and enhances the ability to perform trend analysis and seasonal reporting across different business metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) as a string, facilitating time-based analysis and reporting within the DimDate table. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting. |
| `DateKey` | `int64` | The 'DateKey' column (int64) in the 'DimDate' table serves as a unique identifier for each date, facilitating efficient querying and data enrichment processes. |
| `DayName` | `string` | The 'DayName' column contains the full name of the day of the week corresponding to each date in the 'DimDate' table, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | The 'DayNumberOfMonth' column (int64) in the 'DimDate' table represents the numerical day of the month, ranging from 1 to 31, providing a granular reference for date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical day of the week, with values ranging from 1 (Sunday) to 7 (Saturday), facilitating time-based analysis and reporting. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column (int64) in the 'DimDate' table represents the sequential day of the year, ranging from 1 to 365 (or 366 for leap years), facilitating date-related calculations and analyses. |
| `MonthName` | `string` | The 'MonthName' column contains the full names of the months, providing a human-readable representation of the month for each date entry in the DimDate table. |
| `MonthNumberOfYear` | `int64` | The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month, ranging from 1 for January to 12 for December, facilitating time-based analysis and reporting. |
| `ShortDate` | `dateTime` | The 'ShortDate' column in the 'DimDate' table represents a simplified date format, capturing the specific date without time details for streamlined data analysis and reporting. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number within the calendar year, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Indicates whether the date corresponds to the current calendar year, represented as a string value ('Yes' or 'No').
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CurrentYearFlag' in a data model, specifically in a table that contains date information (referred to as 'DimDate').

Here's what it does in simple terms:

1. **Extract Year from Date**: The expression takes a date from the 'ShortDate' column in the 'DimDate' table and extracts the year from it using the `YEAR` function.

2. **Compare with Current Year**: It then compares this extracted year to the current year, which is obtained using the `YEAR(TODAY())` function. `TODAY()` gives the current date, and `YEAR()` extracts the year from that date.

3. **Flagging**: If the year from the 'ShortDate' is less than or equal to the current year, the expression returns a value of 1. This means that the date is either in the current year or a previous year. If the year from 'ShortDate' is greater than the current year, it returns 0, indicating that the date is in a future year.

**In summary**: This calculated column flags each date in the 'DimDate' table with a 1 if it is from the current year or any previous year, and with a 0 if it is from a future year. This can be useful for filtering or analyzing data based on whether dates are in the past or present.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table is designed to track and analyze the distribution of work hours alongside their corresponding percentage contributions to overall productivity or project completion. This table enables businesses to assess resource allocation and optimize workforce efficiency by providing insights into how hours worked correlate with performance metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column contains string representations of hours worked alongside their corresponding percentage contributions, providing insights into employee productivity and time allocation. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and percentage calculations, facilitating the enrichment of data for analytical purposes. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data entries within the 'Hours_nd_Percentage' table, facilitating the organization and analysis of time-related metrics. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, with associated qualification metrics and sorting parameters to facilitate efficient data retrieval and reporting. This table enhances data integrity and consistency by providing a centralized source for code definitions and their respective attributes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers used to categorize and enrich data within the lookup table, facilitating efficient data retrieval and analysis. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numerical flag to categorize data for enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table contains string values that determine the order in which records are displayed or processed, facilitating organized data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics. This table enhances data integrity and enables efficient data retrieval related to employee performance and organizational analysis.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column serves as a unique identifier for each employee in the 'lkp_fltr_Employee' table, facilitating data enrichment and retrieval processes. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, serving as a key identifier for enriching employee-related data within the lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, detailing key attributes such as project names, qualification scores, billing amounts, and associated groups, enabling businesses to analyze project performance and financial metrics effectively.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing code associated with each project, facilitating financial tracking and reporting. |
| `Group` | `string` | The 'Group' column categorizes projects into specific classifications or teams within the 'lkp_Project' table, facilitating organized data management and retrieval. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the lkp_Project table. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, with numerical values representing different levels of qualification criteria met. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing attributes such as billable department identifiers and unit classifications. This table is essential for analyzing resource allocation and financial performance across different organizational segments.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column represents the identifier for departments that are eligible for billing, facilitating the tracking of billable units within the organization. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column contains the names of organizational units, providing essential categorization for data enrichment processes within the lkp_Unit table. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores string values representing the external type classification of owned sub-units, facilitating data enrichment processes. |
| `Unit` | `string` | The 'Unit' column stores the names of measurement units used for data enrichment, facilitating standardized reference across various datasets. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, status, leadership, and financial aspects. This table enables businesses to effectively manage and analyze their projects, facilitating better decision-making and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing a key timestamp for project tracking and performance analysis within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column contains string values that represent the administrative details or oversight responsibilities associated with each project listed in the tbl_Project table. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a timestamp for tracking updates within the 'tbl_Project' table. |
| `ClientName` | `string` | The 'ClientName' column stores the name of the client associated with each project, facilitating identification and management of client relationships within the project data. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, facilitating the tracking and management of client-related data within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project entry was initiated in the 'tbl_Project' table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project in the tbl_Project table. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the exact date and time when each project entry was created in the 'tbl_Project' table. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or business unit associated with each project in the 'tbl_Project' table. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column indicates the scheduled date and time for the completion of the project, facilitating project timeline management and tracking. |
| `Project` | `string` | The 'Project' column contains the names or identifiers of various projects associated with the data entries in the 'tbl_Project' table, facilitating the organization and retrieval of project-specific information. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed narrative of the project's objectives, scope, and key features, providing essential context for stakeholders and team members within the 'tbl_Project' table. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the category or classification of the project, allowing for organized grouping and management of related projects within the 'tbl_Project' table. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a textual summary of the project group, detailing its objectives and scope within the context of the overall project management framework. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) serves as a unique identifier for each project in the 'tbl_Project' table, facilitating efficient data retrieval and management. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the associated project within the 'tbl_Project' table. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency sales price of a project, stored as a decimal value to facilitate accurate financial calculations and reporting. |
| `Startdate` | `dateTime` | The 'Startdate' column records the date and time when a project is initiated, serving as a key reference point for project timelines and scheduling within the 'tbl_Project' table. |
| `Status` | `string` | The 'Status' column indicates the current progress or state of each project within the tbl_Project table, allowing for effective tracking and management of project workflows. |
| `Statuscode` | `string` | The 'Statuscode' column indicates the current operational status of each project, represented as a string, to facilitate tracking and management of project progress within the 'tbl_Project' table. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, stored as a decimal to ensure precision in financial calculations. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table serves as a critical resource for identifying employees who have not submitted their timesheets within a specified timeframe, enabling managers to monitor compliance and ensure accurate payroll processing. It includes key details such as employee and manager information, contract dates, and hours, facilitating targeted follow-ups and improving overall workforce management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column represents the total number of hours worked by an employee, recorded as a double data type, within the context of identifying missing timesheet entries in the 'vw_missing_timesheet' table. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract is set to expire, providing essential information for managing timesheet compliance and contract renewals within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to assist in identifying discrepancies in timesheet submissions within the 'vw_missing_timesheet' view. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract for each employee, providing insights into their engagement and availability for timesheet submissions. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet submissions. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and follow-up on absent submissions. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours recorded for each employee's timesheet entry, facilitating the identification of discrepancies in time reporting. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_missing_timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries that are missing, facilitating accountability and follow-up actions. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the manager responsible for overseeing the employees associated with missing timesheets, facilitating accountability and follow-up actions. |
| `MissingHours` | `double` | The 'MissingHours' column represents the total number of hours not recorded in timesheets, expressed as a double, to facilitate the identification of discrepancies in employee work hour reporting. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the missing timesheet records, facilitating tracking and management of time allocation across various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project associated with the timesheet entries, facilitating the tracking and management of time allocation across various projects. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected (true) or not (false) in the context of tracking missing timesheet submissions. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, with a value of true signifying readiness and false indicating it is not yet prepared. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of tracking and analyzing missing timesheet entries. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double data type, within the 'vw_missing_timesheet' table, which is designed to enhance data analysis related to timesheet discrepancies. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with missing timesheet entries, formatted for concise reporting and analysis. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific sub-division or department within the organization associated with the missing timesheet entries. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet entry, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' view. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the work activities recorded in the timesheet, providing context and details for each entry in the 'vw_missing_timesheet' table. |
| `Week_` | `int64` | The 'Week_' column (int64) represents the numerical week of the year associated with each entry in the 'vw_missing_timesheet' table, facilitating the identification and analysis of timesheet data by week. |
| `Year_` | `int64` | The 'Year_' column (int64) in the 'vw_missing_timesheet' table represents the calendar year associated with each entry, facilitating the analysis of timesheet data over time. |

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'BillableDep'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'BillableDep' column from the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on the current row's context.

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of a specific billable dependency, the default assumption is that it is billable.

3. **Check for Non-Zero Values**: If there is a related 'BillableDep' value and it is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that there is a valid billable dependency.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**. This indicates that there is no billable dependency in this case.

In summary, this DAX expression effectively categorizes each row based on whether it has a billable dependency or not. It returns **1** if there is a billable dependency (either because it's not blank or it's a default case) and **0** if the dependency is explicitly zero. This helps in identifying which entries can be considered billable based on their related data.

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
   - **End Date Check**: Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. If the `ContractEndDate` is blank (meaning the employee's contract is still ongoing or has no specified end), the employee is also considered active.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the contract has no end date), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns a value of `0`, indicating that the employee is not active.

In summary, this DAX expression effectively flags employees as active (1) or inactive (0) based on whether the current date falls within their contract period. This helps in analyzing employee status and managing workforce data efficiently.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to assist in identifying and addressing missing or insufficient data in the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IncompleteFlag' in a data table named `vw_missing_timesheet`. 

Here's what it does in simple terms:

- It checks the value in the column `MissingHours` for each row in the `vw_missing_timesheet` table.
- If the value of `MissingHours` is less than 0, it means that there are negative hours recorded, which could indicate an issue or an incomplete timesheet.
- In this case, the expression returns a value of 1, signaling that the timesheet is incomplete or has a problem.
- If the value of `MissingHours` is 0 or greater, it returns a value of 0, indicating that the timesheet is complete or does not have any issues.

In summary, this expression flags rows with incomplete timesheets by assigning a value of 1 for issues and 0 for complete records.

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

1. **Check for Related Data**: The expression first checks if there is a related value in the 'lkp_Unit' table for the 'Unit' column. This is done using the `RELATED` function, which retrieves data from a related table based on existing relationships in the data model.

2. **Handle Missing Data**: If the related 'Unit' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression will return the text "Unknown". This helps to identify cases where the unit information is missing.

3. **Return Valid Unit**: If there is a valid related 'Unit' value (i.e., it is not blank), the expression will return that value instead.

In summary, this DAX expression effectively ensures that every entry in the 'MAIN_UNIT' column either shows the corresponding unit name from the 'lkp_Unit' table or indicates "Unknown" if no unit information is available. This is useful for maintaining clarity in reports and analyses by clearly marking missing data.

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

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the 'ContractEndDate'. For instance, if the date is in the 3rd week of December, it might return "50".

3. **FORMAT(..., "00")**: This ensures that the week number is always displayed as two digits. So, if the week number is 5, it will be formatted as "05".

4. **Concatenation (&)**: The expression combines the year and the formatted week number into a single string, separated by a hyphen. So, if the year is "2023" and the week number is "50", the final result will be "2023-50".

In summary, this DAX expression generates a string that represents the year and the week number of the 'ContractEndDate', formatted as "YYYY-WW". This can be useful for reporting or analysis purposes, allowing users to easily identify and group data by year and week.

##### Measures

**`Count_Negative_Consultant`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have recorded negative hours in a timesheet. Here's a breakdown of what it does:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This function counts the number of unique entries in a specified column. Here, it counts the unique names of employees from the 'EmployeeName' column in the 'vw_missing_timesheet' table.

3. **Filter Condition**: The expression includes a filter that specifies only to consider records where the 'MissingHours' column has a value less than 0. This means it focuses on instances where employees have reported negative hours, which could indicate an error or a need for correction.

In summary, this DAX measure calculates the total number of distinct employees who have negative hours recorded in their timesheets. This information can be useful for identifying potential issues with timesheet entries or for managing employee workload and reporting accuracy.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have recorded negative missing hours in a timesheet report. Here’s a breakdown of what it does:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This counts the number of unique values in a specified column. Here, it counts the unique `EmployeeID`s from the `vw_missing_timesheet` table.

3. **Filter Condition**: The expression includes a filter that only considers records where the `MissingHours` value is less than 0. This means it focuses on instances where employees have reported negative hours, which could indicate an error or a specific situation that needs attention.

In summary, this measure calculates how many different employees have reported negative missing hours in their timesheets. This information can be useful for identifying potential issues with time reporting or for auditing purposes.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of active employees who have missing timesheet hours for a specific period, specifically those who have reported fewer hours than their contracted hours.

Here's a breakdown of what it does:

1. **Data Source**: The expression uses a table called `vw_missing_timesheet`, which likely contains records of employee hours worked, their contracted hours, and other relevant details.

2. **Summarization**: It first summarizes the data by grouping it based on:
   - Employee Name
   - Year
   - Week
   - Unit (from another table called `lkp_Unit`)
   - SubUnit
   This means it organizes the data to look at each employee's hours worked during specific weeks of specific years.

3. **Calculating Missing Hours**: For each group (each employee for each week), it calculates:
   - **MissingHours**: This is the difference between the total hours worked (`SUM(vw_missing_timesheet[Hours_])`) and the maximum contracted hours (`MAX(vw_missing_timesheet[ContractHours])`). If an employee worked fewer hours than they were contracted for, this value will be negative.

4. **Identifying Active Employees**: It also calculates:
   - **MinActiveEmp**: This checks if the employee is considered active during that time period by taking the minimum value of `vw_missing_timesheet[CC_ActiveEmployees]`. If this value is 1, it indicates that the employee is active.

5. **Filtering**: The `FILTER` function then narrows down the summarized data to only include:
   - Employees who are active (`[MinActiveEmp] = 1`)
   - Employees who have missing hours (i.e., those with `[MissingHours] < 0`)

6. **Counting Rows**: Finally, the expression counts the number of rows in the filtered result, which represents the number of active employees who have not logged enough hours compared to their contracted hours.

In summary, this DAX measure calculates how many active employees have reported working fewer hours than they are contracted for, highlighting potential issues with timesheet reporting.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of "missing hours" for active employees who have not met their contracted hours in a specific time frame (defined by year and week).

Here's a breakdown of what it does:

1. **Define Active Employees**: The expression starts by creating a variable called `ActiveEmployees`. This variable filters a summarized table of data from `vw_missing_timesheet`, which contains information about employees and their hours worked.

2. **Summarization**: The `SUMMARIZE` function groups the data by employee name, year, week, unit, and subunit. For each group, it calculates two key pieces of information:
   - **MissingHours**: This is calculated by taking the total hours worked (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contracted hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has not worked enough hours compared to what they were contracted for.
   - **MinActiveEmp**: This captures the minimum value of active employees (`MIN(vw_missing_timesheet[CC_ActiveEmployees])`) for that group. If this value is 1, it means that there is at least one active employee in that group.

3. **Filtering**: The `FILTER` function then narrows down the summarized data to only include groups where:
   - There is at least one active employee (`[MinActiveEmp] = 1`).
   - The calculated missing hours are negative (`[MissingHours] < 0`), indicating that the employee has not fulfilled their contracted hours.

4. **Calculate Total Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered `ActiveEmployees` table and sums up the `MissingHours` for each of those groups.

In summary, this DAX measure calculates the total number of hours that active employees are missing based on their contracted hours, focusing only on those who have not met their required hours. This helps the business identify gaps in employee work hours and manage staffing or workload issues effectively.

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

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key pieces of information: the week (`vw_missing_timesheet[Week_]`) and the employee ID (`vw_missing_timesheet[EmployeeID]`). This means that for each employee in each week, we are creating a summary.

3. **Calculating Maximum Contract Hours**: Within each group (each unique combination of week and employee), the expression calculates the maximum contract hours (`MAX(vw_missing_timesheet[ContractHours])`). This is important because an employee might have multiple entries for the same week, and we want to consider only the highest contract hours recorded for that week.

4. **Summing Up**: After summarizing the data and calculating the maximum contract hours for each employee per week, the `SUMX` function then takes these summarized results and adds them up. Essentially, it totals all the maximum contract hours across all employees and weeks.

In summary, this DAX measure provides the total expected contract hours for all employees, ensuring that if there are multiple entries for any employee in a given week, only the highest value is counted. This helps in understanding the overall expected workload for employees on a weekly basis.

**`MISSING%`**

- **DAX Expression:**
```dax
DIVIDE([Dax_EmpCount]-[Dax_EmpCount_MissingTS],[Dax_EmpCount]) * 100
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of employees that are not missing a timestamp (TS) in a dataset. Here's a breakdown of what it does in simple business terms:

1. **[Dax_EmpCount]**: This represents the total number of employees in the dataset.

2. **[Dax_EmpCount_MissingTS]**: This indicates the number of employees who are missing a timestamp. A missing timestamp could mean that there is incomplete data for those employees.

3. **[Dax_EmpCount] - [Dax_EmpCount_MissingTS]**: This part of the expression calculates the number of employees who have a valid timestamp. It does this by subtracting the count of employees with missing timestamps from the total employee count.

4. **DIVIDE(..., [Dax_EmpCount])**: The `DIVIDE` function is used to safely perform division. Here, it divides the number of employees with valid timestamps by the total number of employees. This gives us a fraction representing the proportion of employees with valid timestamps.

5. **... * 100**: Finally, the result is multiplied by 100 to convert the fraction into a percentage.

In summary, this measure calculates the percentage of employees who have a valid timestamp compared to the total number of employees. A higher percentage indicates better data completeness regarding timestamps for employees.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked and contracted hours for employees or contractors. Here's a breakdown of what it achieves:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum number of contracted hours from the `vw_missing_timesheet` table. Contracted hours refer to the total hours that an employee or contractor is expected to work as per their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum number of hours actually recorded or worked by the employee or contractor from the same table.

3. **Subtraction**: The expression then subtracts the maximum recorded hours from the maximum contracted hours. 

In simple terms, this DAX measure calculates the maximum number of hours that are missing or unaccounted for by comparing what was supposed to be worked (contracted hours) against what was actually worked (recorded hours). 

The result tells you how many hours are missing for the employee or contractor, which can help in identifying discrepancies in timesheets or understanding underperformance in terms of hours worked.

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

1. **SUM(vw_missing_timesheet[MissingHours])**: This part adds up all the missing hours from the timesheets. It looks at a table called `vw_missing_timesheet` and sums up the values in the `MissingHours` column. This gives us the total number of hours that employees did not report.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the expected number of hours that employees should be working in a week according to their contracts. 

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the expression calculates the difference between the total missing hours and the expected contract hours. If the result is positive, it indicates that the missing hours exceed what was expected, suggesting a shortfall in timesheet submissions.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The `DIVIDE` function takes the result from the previous step (the difference) and divides it by the expected contract hours. This gives us a ratio that represents how much of the expected hours are missing. The third argument, `0`, ensures that if the expected contract hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

In summary, this DAX measure calculates the percentage of expected working hours that are missing from timesheet submissions. A higher percentage indicates a greater shortfall in reported hours, which could signal issues with compliance or employee engagement in reporting their work hours.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total number of unique employees who have missing timesheets, grouped by their respective units and weeks. Here's a breakdown of what it does in simple business terms:

1. **Data Source**: The expression uses a table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions.

2. **Grouping**: The `SUMMARIZE` function is used to create a summary table. This table groups the data by three columns:
   - `Week_`: The week of the year.
   - `EmployeeName`: The name of the employee.
   - `MAIN_UNIT`: The main unit or department the employee belongs to.

3. **IncompleteFlag**: For each group created by the `SUMMARIZE` function, it calculates a new column called "IncompleteFlag". This column uses the `MAX` function to determine if there is any indication of incompleteness in the timesheet for that employee in that week. If at least one record indicates that the timesheet is incomplete, the flag will be set to 1 (true); otherwise, it will be 0 (false).

4. **Summation**: The outer `SUMX` function then takes this summarized table and sums up the values in the "IncompleteFlag" column. This effectively counts how many unique employees have missing timesheets across the specified weeks and units.

In summary, this DAX measure helps to identify and quantify the number of unique employees who have not submitted their timesheets, organized by week and unit, which can be crucial for tracking compliance and ensuring timely submissions in a business context.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet entries, capturing essential details such as financial year, dates, and associated project and management identifiers. This data is crucial for tracking labor costs, project allocation, and performance management within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column identifies the name of the client associated with each timesheet entry, facilitating the tracking of billable hours and project management. |
| `Code2` | `string` | The 'Code2' column stores a secondary identifier or classification code associated with each timesheet entry, facilitating enhanced data categorization and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to conclude, providing essential information for timesheet management and resource planning. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when a contract begins, providing essential context for timesheet entries related to specific contractual agreements. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract associated with the timesheet entry, providing a snapshot of its active, inactive, or pending state. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, represented as a string, to facilitate real-time reporting and analysis within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the specific date and time when a contract was transferred, providing essential temporal context for timesheet entries related to contract management. |
| `CostAmount` | `double` | The 'CostAmount' column represents the monetary value associated with the hours worked, expressed as a double, within the timesheet data for accurate financial reporting and analysis. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, expressed as a double to allow for precise financial calculations. |
| `Currency` | `string` | The 'Currency' column (string) in the 'vw_Timesheet' table specifies the type of currency used for financial entries related to timesheet records. |
| `DebtorName` | `string` | The 'DebtorName' column contains the name of the individual or entity responsible for payment, providing essential identification for financial tracking within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee associated with the timesheet entries in the 'vw_Timesheet' table, facilitating accurate tracking and reporting of work hours. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column stores the unique identifier and name of each employee, facilitating the association of timesheet entries with the corresponding personnel in the 'vw_Timesheet' table. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with the corresponding employee records. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating the identification of personnel for time tracking and reporting purposes. |
| `Employer` | `string` | The 'Employer' column contains the name of the organization or company that employs the individual associated with the timesheet entries. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating the tracking and management of employee work hours by employer. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to each timesheet entry, providing further insights into the recorded hours or tasks. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with the recorded timesheet entries, facilitating financial analysis and reporting. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, facilitating accurate tracking of labor input for payroll and project management. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating analysis of work patterns and resource allocation. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee work patterns. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee work hours. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries, facilitating accountability and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the names of the IPM IDs associated with each timesheet entry, facilitating the identification and tracking of project management metrics. |
| `JobTitle` | `string` | The 'JobTitle' column contains the titles of positions held by employees, providing context for their roles and responsibilities within the organization as recorded in the timesheet data. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking records within the 'vw_Timesheet' table, facilitating data integration and enrichment processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_Timesheet' table identifies the unique identifier of the manager responsible for overseeing the timesheet entries. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, facilitating the identification of managerial oversight for employee hours worked. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key associated with the manager responsible for overseeing the timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct supervisor, facilitating the identification of managerial oversight for each timesheet entry. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of work booking entries associated with employee timesheets, detailing the specific tasks or projects worked on during a given period. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each timesheet entry, allowing for detailed tracking and analysis of time spent on various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with the timesheet entries, facilitating the tracking and reporting of time spent on specific projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that identifies the specific project associated with each timesheet entry, providing context for the recorded hours and activities. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the project profile associated with each timesheet entry, facilitating the tracking and analysis of time spent on specific projects. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into the types of work being performed. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected (true) or accepted (false) during the review process. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for processing or review, with a value of true signifying readiness. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double data type, within the context of timesheet data analysis. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current state of each timesheet entry, reflecting whether it is submitted, approved, or pending review. |
| `Taxed` | `boolean` | Indicates whether the recorded timesheet entry is subject to taxation, with a value of true representing taxable entries and false for non-taxable ones. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, providing insights into resource allocation and project management. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores a unique identifier for each timesheet entry, facilitating the tracking and categorization of employee work hours within the 'vw_Timesheet' view. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains a textual summary of the tasks or activities recorded in the timesheet, providing context and details for each entry. |
| `Unit` | `string` | The 'Unit' column represents the measurement unit associated with the time entries recorded in the 'vw_Timesheet' table, facilitating clarity in reporting and analysis of hours worked. |
| `UnitCode` | `string` | The 'UnitCode' column represents a unique identifier for the unit associated with each timesheet entry, facilitating the categorization and tracking of labor hours by specific organizational units. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and record-keeping within the timesheet management system. |
| `YearWeek` | `string` | The 'YearWeek' column (string) in the 'vw_Timesheet' table represents the year and week number of the timesheet entries, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`Approved_With_V_Z`** (`string`)

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheets, specifying whether they have been approved with a particular validation or criteria denoted by 'V_Z'.
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
   - It first checks if the value in the 'Approved' column is TRUE (meaning the item is approved).
   - It also checks if the 'TimesheetCode' from the 'vw_Timesheet' table is either "V" or "Z".

2. **Logical OR**: The two conditions are connected by an OR operator (`||`). This means that if either condition is true, the overall result will be true.

3. **Output Values**: 
   - If either the 'Approved' status is TRUE or the 'TimesheetCode' is "V" or "Z", the expression returns a value of 1.
   - If neither condition is met (meaning the item is not approved and the 'TimesheetCode' is neither "V" nor "Z"), it returns a value of 0.

**What it achieves**: This calculated column effectively flags records as "approved" (1) if they are either explicitly approved or have a specific timesheet code ("V" or "Z"). If neither of these conditions is met, it flags them as "not approved" (0). This can be useful for filtering or analyzing data based on approval status and specific timesheet codes.

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

1. **Check for Related Data**: The expression first checks if there is a related value in the 'BillableDep' column from the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on the relationships defined in your data model.

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of specific billable data, the default assumption is that the item is billable.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that the item is considered billable.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that the item is not billable.

In summary, this DAX expression effectively categorizes items as billable or not based on the presence and value of related data. If there is no data or if the data indicates a non-zero value, the item is marked as billable (1). If the related data is zero, it is marked as not billable (0).

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

1. **Purpose**: The expression determines whether a particular timesheet entry is considered "billable" based on certain criteria.

2. **Criteria for Billability**:
   - **Sales Price Check**: It first checks if the `SalesPrice` for the entry is greater than 0. If there is a positive sales price, it indicates that the work done is billable.
   - **Project Profile Code Check**: It also checks if the `ProjectProfileCode` is equal to 81. This specific code likely represents a type of project that is always billable, regardless of other factors.
   - **Project Name Check**: Lastly, it searches for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it suggests that the project is related to customer success services, which are typically billable.

3. **Output**: 
   - If any of the above conditions are met (i.e., if the sales price is positive, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of **1**. This indicates that the timesheet entry is billable.
   - If none of these conditions are met, it returns a value of **0**, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific financial and project-related criteria, helping the business track which work can be charged to clients.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column contains the name of the employer associated with the timesheet entry, providing essential context for employee work hours and payroll processing.

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

1. **Employee Name**: It takes the name of the employee from the 'vw_Timesheet' table, which is represented by `('vw_Timesheet'[EmployeeName])`.

2. **Hours per Week**: It also takes the number of hours that the employee works per week, represented by `('vw_Timesheet'[HoursperWeek])`.

3. **Combining Information**: The expression then combines these two pieces of information into one string. It adds a separator " - " between the employee's name and their weekly hours. 

So, for example, if an employee named "John Doe" works 40 hours per week, the resulting value in the 'Employee_WeeklyHours' column would be:
**"John Doe - 40"**

In summary, this DAX expression effectively creates a clear and concise representation of each employee's name along with their weekly working hours, making it easier to read and understand the data at a glance.

**`GroupCat`** (`string`)

- **Description:** The 'GroupCat' column categorizes timesheet entries into distinct groups for better organization and analysis of time allocation across various projects or tasks.
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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'GroupCat' in a data model, likely related to project management or timesheet tracking. Here's a breakdown of what it achieves in simple business terms:

1. **Lookup for Group**: The expression first checks if there is a corresponding 'Group' value in the 'lkp_Project' table for the project listed in the 'vw_Timesheet' table. It does this using the `LOOKUPVALUE` function, which searches for the 'Group' based on the 'PROJECT' identifier.

2. **Check for Blank Values**: The `NOT(ISBLANK(...))` part checks if the result of the lookup is not blank. If there is a valid 'Group' found for the project, it means that the project is associated with a specific group.

3. **Return Group Value**: If a valid 'Group' is found, the expression returns that 'Group' value. This means that the project is categorized under a specific group, which could be important for reporting or analysis.

4. **Handle Unmatched Projects**: If no 'Group' is found (i.e., the lookup result is blank), the expression then checks if the project is billable by looking at the 'BillablePrj' column in the 'vw_Timesheet' table. 

5. **Categorize as Billable or Unbillable**: If the project is billable (indicated by a value of 1), it categorizes it as "Billable". If it is not billable, it categorizes it as "Other Unbillable".

In summary, this DAX expression categorizes each project in the timesheet into one of three categories: 
- A specific 'Group' if it exists,
- "Billable" if the project is billable but not associated with a group,
- "Other Unbillable" if the project is not billable and also not associated with a group. 

This categorization helps in understanding the nature of projects and their billing status, which is crucial for financial analysis and project management.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IsContractActive' in a data model, specifically for a table named 'vw_Timesheet'. Here's what it does in simple business terms:

1. **Checks Contract End Date**: The expression first checks if the 'ContractEndDate' for each record (or row) in the 'vw_Timesheet' table is either blank (meaning there is no end date specified) or if the end date is greater than today's date.

2. **Determines Contract Status**:
   - If either of the above conditions is true (the contract has no end date or the end date is in the future), it labels the contract as "Active".
   - If neither condition is true (meaning there is an end date that has already passed), it labels the contract as "Not Active".

In summary, this calculated column helps to easily identify whether a contract is currently active or not based on its end date. This can be useful for tracking ongoing contracts and managing resources effectively.

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

2. **Handle Missing Data**: If there is no related value (meaning the 'Unit' field is blank), the expression will return the text "Unknown". This is a way to handle situations where the expected data is missing, ensuring that the output is clear rather than leaving it blank.

3. **Return the Unit Name**: If there is a related value (meaning the 'Unit' field is not blank), the expression will return that value. This means that the calculated column will show the actual unit name associated with the current row.

In summary, this DAX expression effectively populates the 'MAIN_UNIT' column with either the name of the unit from the related table or "Unknown" if no unit is found. This helps maintain clarity in the data by ensuring that every row has a meaningful value, even when some data might be missing.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) associated with each timesheet entry, facilitating time-based analysis and reporting.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to extract the month number from a date value found in the column named `TimesheetDate`. 

In simple terms, this means that for each date in the `TimesheetDate` column, the expression will return a number that represents the month of that date. For example:

- If the date is January 15, 2023, the expression will return `1`.
- If the date is February 10, 2023, it will return `2`.
- If the date is December 5, 2023, it will return `12`.

This calculated column, named 'MonthNumber', helps in organizing or analyzing data by month, making it easier to perform further calculations or create reports based on monthly trends or summaries.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the employee identification number as a string, facilitating the association of timesheet entries with specific employees in the 'vw_Timesheet' view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

In simple business terms, this expression achieves the following:

- It counts how many different employees have recorded timesheets, ensuring that each employee is only counted once, regardless of how many times they appear in the table.
- This is useful for understanding the total number of distinct employees working on a project or within a specific time period, helping management assess workforce engagement or resource allocation.

Overall, it provides a clear view of employee participation without duplication.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table captures the classification of time entries as either owned, subcontracted, or external, facilitating the analysis of resource allocation and project costs.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used to retrieve a specific value from a related table in a data model. Here’s a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a calculated column within a table that has a relationship with another table called `lkp_Unit`.

2. **Purpose**: The expression fetches the value from the `OWN-Sub-ExtT` column in the `lkp_Unit` table. 

3. **How it Works**: 
   - The `RELATED` function looks for a matching row in the `lkp_Unit` table based on the existing relationship between the two tables.
   - It then retrieves the value from the `OWN-Sub-ExtT` column for that matching row.

4. **Outcome**: By using this expression, you can enrich your current table with additional information from the `lkp_Unit` table, specifically the `OWN-Sub-ExtT` value. This can help in analysis or reporting by providing more context or details related to each row in your current table.

In summary, this DAX expression is a way to pull in related data from another table, enhancing the information available for analysis.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column contains string values that indicate the qualification status of projects associated with timesheet entries, helping to categorize and assess project eligibility for resource allocation and reporting.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'QualifyPrj'. Here's a simple breakdown of what it does:

1. **Check for Blank Value**: The expression first checks if the 'Qualify' field from a related table called 'lkp_Project' is blank (i.e., has no value).

2. **Return Value Based on Check**:
   - If the 'Qualify' field is blank, the expression returns a value of **1**. This could indicate a default or fallback status when there is no specific qualification available.
   - If the 'Qualify' field is not blank, it returns the actual value from the 'Qualify' field in the 'lkp_Project' table.

### Summary:
In essence, this DAX expression is designed to ensure that every row in the 'QualifyPrj' column has a value. If there is no qualification available for a project, it assigns a default value of 1. If there is a qualification, it uses that value instead. This helps maintain data integrity and provides a clear indication of project qualification status.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor allocation.

**`RReady_With_V_Z`** (`string`)

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of timesheet entries, represented as a string, to facilitate data processing and reporting within the 'vw_Timesheet' view.
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
   - It first checks if the value of the column `[ReportReady]` is `TRUE`. This likely indicates whether a report is ready for some process or review.
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These codes probably represent specific types of timesheets or statuses.

2. **Output Values**: 
   - If either of the conditions is met (meaning the report is ready or the timesheet code is "V" or "Z"), the expression returns a value of `1`.
   - If neither condition is met, it returns a value of `0`.

3. **Purpose**: The overall purpose of this calculated column is to create a simple binary indicator (1 or 0) that signifies whether a particular record (like a timesheet) is considered "ready" based on the specified criteria. This can be useful for filtering, reporting, or further analysis in your data model.

In summary, this DAX expression helps identify records that are either ready for reporting or belong to specific categories (V or Z), making it easier to manage and analyze timesheet data.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'ContractStatus2' that evaluates the status of a contract and returns different values based on that status.

Here's a breakdown of what it does:

1. **Condition Check**: The expression first checks if the measure `[ContractStatusMeasure]` equals "Active". This measure likely indicates whether a contract is currently active or not.

2. **Return Value if Active**: If the contract is indeed "Active", the expression returns the value of another measure called `[HoursDifference]`. This measure probably represents the difference in hours related to the contract, such as the time remaining until the contract expires or the total hours worked under the contract.

3. **Return Value if Not Active**: If the contract is not "Active" (meaning it could be "Inactive" or any other status), the expression returns the text "Not Active".

In summary, this DAX expression helps to determine the status of a contract. If the contract is active, it provides a numerical value (the hours difference), and if it is not active, it simply indicates that by returning the text "Not Active". This can be useful for reporting or analysis purposes, allowing users to quickly understand the status of contracts and their associated hours.

**`ContractStatusMeasure`**

- **DAX Expression:**
```dax
IF(
			    ISBLANK(MAX('vw_Timesheet'[ContractEndDate])) || MAX('vw_Timesheet'[ContractEndDate]) > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to determine the status of a contract based on its end date. Here’s a breakdown of what it does in simple business terms:

1. **Check for Contract End Date**: The expression first looks at the maximum value of the 'ContractEndDate' from the 'vw_Timesheet' table. This means it is checking the latest end date of contracts that are recorded.

2. **Evaluate Conditions**:
   - **ISBLANK**: It checks if this maximum end date is blank (meaning there is no end date recorded).
   - **Comparison with Today**: It also checks if this maximum end date is greater than today’s date (meaning the contract is still ongoing).

3. **Determine Status**:
   - If either of the above conditions is true (the end date is blank or it is in the future), the measure returns "Active". This indicates that the contract is currently active and has not ended.
   - If neither condition is true (the end date is not blank and it is in the past), it returns "Not Active". This means the contract has ended.

In summary, this measure helps in identifying whether a contract is currently active or not based on its end date, providing valuable information for contract management and decision-making.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 15, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated starting from the first week of the year.

So, when you combine these two functions, `WEEKNUM(TODAY())` effectively tells you which week of the year it is right now. 

For instance, if today is October 15, 2023, this expression would return the week number that corresponds to that date, which helps businesses understand how far along they are in the year and can be useful for reporting, planning, or analysis purposes.

**`Dax_EmpCount_Approved`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[Approved] = FALSE()
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees whose timesheets have not been approved. Here's a breakdown of what each part does:

1. **CALCULATE**: This function changes the context in which data is evaluated. It allows you to apply filters to your calculations.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it ensures that each employee is only counted once, even if they have multiple timesheet entries.

3. **vw_Timesheet[Approved] = FALSE()**: This is a filter condition applied within the `CALCULATE` function. It specifies that only the timesheet entries that are marked as "not approved" (i.e., where the `Approved` field is FALSE) should be considered in the count.

In summary, this DAX measure calculates the total number of distinct employees who have submitted timesheets that have not been approved. This information can be useful for tracking pending approvals and managing workforce productivity.

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

1. **CALCULATE Function**: This function changes the context in which data is evaluated. In this case, it is used to modify the filter context for the calculation that follows.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it is counting the unique names of employees from the `vw_Timesheet` table.

3. **Filter Condition**: The expression includes a filter condition that specifies `vw_Timesheet[ReportReady] = FALSE()`. This means that the calculation will only consider those records where the `ReportReady` field is set to FALSE, indicating that the timesheet is not ready for reporting.

In summary, this measure calculates the total number of unique employees who have timesheets that are not yet ready for reporting. This can help a business understand how many employees still need to finalize their timesheets before they can be processed or reported.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees recorded in the `vw_Timesheet` table.

Here's a breakdown of what it does:

- **DISTINCTCOUNT**: This function counts the number of distinct (unique) values in a specified column.
- **vw_Timesheet[EmployeeName]**: This specifies the column from which the unique values are being counted. In this case, it refers to the names of employees in the `vw_Timesheet` table.

In simple business terms, this measure tells you how many different employees have logged their time in the timesheet. It helps in understanding workforce participation or engagement by showing the total number of unique employees who have submitted timesheets, regardless of how many times each employee has submitted their hours.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_%_Budget_Used`**

- **DAX Expression:**
```dax
DIVIDE(SUM(vw_Timesheet[SalesAmount]), SUM('tbl_Project'[SalesPriceBaseCurrency]), 0)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of the budget that has been used based on sales data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the 'vw_Timesheet' table. Essentially, it totals the revenue generated from sales.

2. **SUM('tbl_Project'[SalesPriceBaseCurrency])**: This part sums up the total budget or sales price allocated for the projects from the 'tbl_Project' table. It represents the total amount that was planned or budgeted for sales.

3. **DIVIDE(..., ..., 0)**: The DIVIDE function takes the total sales amount (from the first part) and divides it by the total budget (from the second part). If the budget is zero (to avoid division by zero errors), it returns 0 instead of an error.

In simple terms, this measure calculates how much of the budget has been used by comparing actual sales to the planned budget. The result is a ratio or percentage that helps businesses understand their performance against their budgeted sales targets. If the result is greater than 100%, it indicates that sales have exceeded the budget. If it's less than 100%, it shows that not all of the budget has been utilized.

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the timesheet. Essentially, it gives you the total revenue generated from sales over a specific period.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the timesheet. It represents the total number of hours that employees or team members have worked during the same period.

3. **Division**: By dividing the total sales amount by the total hours worked, the expression calculates the average hourly rate. This means it tells you how much revenue, on average, is generated for each hour worked.

In summary, this DAX measure provides insight into the efficiency and productivity of the workforce by showing how much revenue is earned for every hour of work. It helps businesses understand their performance and make informed decisions regarding staffing and resource allocation.

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

1. **Identify Current Week**: The measure starts by determining the current week number of the year using the `SELECTEDVALUE` function. This means it looks at the data context (like filters applied in a report) to find out which week is currently being analyzed.

2. **Calculate Previous Budget**: Next, it calculates the budget value for the most recent week prior to the current week. It does this by using the `CALCULATE` function along with a `FILTER`. The `FILTER` function looks at all the available weeks (ignoring any filters that might limit the date range) and selects only those weeks that are less than the current week. It then retrieves the budget value for the most recent week using the measure `[Msr_SalesPriceBaseCurrency]`.

3. **Return Value**: Finally, the measure checks if there is a budget value available for the current week. If there is (i.e., it is not blank), it returns that current budget value. If there isn’t a current budget value available, it falls back to the previously calculated budget value from the most recent week.

In summary, this measure dynamically provides the budget for the current week if available; if not, it defaults to the budget from the last available week. This approach ensures that users always have a relevant budget figure to work with, even if the current week’s data is incomplete.

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

3. **Using CALCULATE and FILTER**: 
   - The `CALCULATE` function is used to change the context in which the data is evaluated. 
   - Inside `CALCULATE`, the `SUM` function adds up the `CostAmount` from the `vw_Timesheet` table.
   - The `FILTER` function is applied to the `ALLSELECTED('DimDate')` table, which means it considers all the dates that are currently selected in the visual, ignoring any filters that might limit the date range. 
   - The filter condition `'DimDate'[MonthNumberOfYear] <= SelectedMonth` ensures that only the months up to and including the selected month are included in the calculation.

4. **Final Outcome**: The result of this measure is the total cost accumulated from January through the selected month. For example, if March is selected, it will sum the costs for January, February, and March, providing a cumulative total that helps in understanding spending trends over time.

In summary, this DAX measure helps businesses track their cumulative costs over time, allowing for better financial analysis and decision-making based on the selected month in their reports.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the cumulative sales amount up to a selected month in a report or visual. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It does this by finding the maximum value of the 'MonthNumberOfYear' from the 'DimDate' table. This means if you are looking at data for March, the `SelectedMonth` variable will hold the value for March (which is 3).

2. **Calculate Cumulative Sales**: The main goal of this measure is to calculate the total sales amount from the 'vw_Timesheet' table for all months up to and including the selected month. 

3. **Using CALCULATE and FILTER**: 
   - The `CALCULATE` function is used to change the context in which the data is evaluated. 
   - Inside `CALCULATE`, the `SUM` function adds up the 'SalesAmount' from the 'vw_Timesheet' table.
   - The `FILTER` function is applied to the 'DimDate' table to include all months that are less than or equal to the `SelectedMonth`. The `ALLSELECTED` function ensures that any filters applied to the 'DimDate' table are respected, but it still allows for the cumulative calculation across the selected months.

4. **Final Outcome**: The result of this measure is the total sales amount for all months leading up to and including the month that is currently selected in the visual. This allows users to see how sales have accumulated over time, providing valuable insights into sales performance trends.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the percentage of hours worked by a specific group, project, or employee relative to the total hours worked across all selected categories in a timesheet data model. Here’s a breakdown of what it does in simple business terms:

1. **TotalValue Calculation**: 
   - The first part of the code creates a variable called `TotalValue`. This variable calculates the total number of hours worked by summing up the `Hours` column from the `vw_Timesheet` table.
   - The `ALLSELECTED` function is used here to ensure that the calculation considers all currently selected filters for `GroupCat`, `Project`, and `EmployeeName`. This means that if you have filters applied in your report (like selecting a specific project or employee), it will still calculate the total hours for all those selections.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours worked by the current context (which could be a specific group, project, or employee) by dividing the sum of hours for that context by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safe way to perform division in DAX. If the `TotalValue` is zero (meaning no hours were recorded), it will return 0 instead of causing an error.

In summary, this measure calculates what percentage of the total hours worked (based on the current selections) is represented by the hours worked in the current context (like a specific employee or project). This helps in understanding how much contribution a particular entity has made compared to the overall effort recorded in the timesheet.

**`MSR_PROJECT_%_REALIZATION`**

- **DAX Expression:**
```dax
IF(sum(tbl_Project[SalesPriceBaseCurrency]) <> 0, sum(vw_Timesheet[SalesAmount])/sum(tbl_Project[SalesPriceBaseCurrency]))
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called 'MSR_PROJECT_%_REALIZATION', which essentially assesses the percentage of sales realized from a project compared to its total sales price.

Here's a breakdown of what it does:

1. **Condition Check**: The expression starts with an `IF` statement that checks if the total sales price (from the `tbl_Project` table) is not equal to zero. This is important because if the total sales price is zero, dividing by zero would lead to an error.

2. **Calculating Sales Amount**: If the total sales price is not zero, the expression proceeds to calculate the percentage of sales realized. It does this by taking the total sales amount from the `vw_Timesheet` table.

3. **Division**: The sales amount is then divided by the total sales price. This gives a ratio that represents how much of the expected sales price has actually been realized through sales.

4. **Result**: The final result of this measure is a percentage that indicates the effectiveness of the project in generating sales relative to its total sales price. A higher percentage means that a larger portion of the expected sales has been achieved.

In summary, this DAX measure helps businesses understand how well a project is performing in terms of sales realization compared to its projected sales price.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the profit margin for a project by subtracting total costs from total sales. Here’s a breakdown of what it does:

1. **Total Sales Calculation**: The expression starts with `sum(vw_Timesheet[SalesAmount])`, which adds up all the sales amounts recorded in the `vw_Timesheet` table. This gives you the total revenue generated from the project.

2. **Total Costs Calculation**: Next, it calculates the total costs with `sum(vw_Timesheet[CostAmount])`. This sums up all the costs associated with the project, which could include expenses like labor, materials, and overhead.

3. **Profit Margin Calculation**: Finally, the expression subtracts the total costs from the total sales: `sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`. The result of this calculation is the project margin, which represents the profit made from the project after accounting for all costs.

In simple terms, this DAX measure tells you how much money a project has made after covering its expenses. A positive result indicates a profit, while a negative result indicates a loss.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called `Msr_SalesPriceBaseCurrency`, which essentially sums up the sales price in the base currency for different projects and clients from a timesheet view. Here’s a breakdown of what it does in simple business terms:

1. **SUMMARIZE Function**: The expression starts by creating a summary table from the `vw_Timesheet` data. This summary groups the data by two key dimensions: `ProjectProfile` and `Client`. This means it organizes the data so that each unique combination of project and client is represented.

2. **SELECTEDVALUE Function**: For each of these unique combinations, it retrieves the sales price in the base currency from the `tbl_Project` table. The `SELECTEDVALUE` function is used here to get the specific sales price associated with the current context of the project being evaluated. If there’s only one value available for that project, it will return that value; if there are multiple values, it will return blank.

3. **MeasureValue**: The result of the `SELECTEDVALUE` function is stored in a new column called `MeasureValue` within the summarized table. This column now holds the sales price in the base currency for each project-client combination.

4. **SUMX Function**: Finally, the `SUMX` function iterates over the summarized table and sums up all the values in the `MeasureValue` column. This gives the total sales price in the base currency across all the projects and clients represented in the `vw_Timesheet`.

In summary, this DAX expression calculates the total sales price in the base currency for all projects and clients listed in the timesheet, providing a consolidated view of sales prices that can be used for reporting or analysis.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of unique employees who have a "Valid" contract status, regardless of any filters that might be applied to the data.

Here's a breakdown of what each part does:

1. **CALCULATE**: This function changes the context in which the data is evaluated. It allows us to apply specific filters to our calculation.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it tells us how many different employees are present.

3. **ALL(vw_Timesheet)**: This function removes any existing filters on the `vw_Timesheet` table. By using this, we ensure that the count of employees is not affected by any other filters that might be applied in the report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This is a filter condition that specifies we only want to count employees whose contract status is "Valid". 

Putting it all together, this DAX expression calculates the total number of unique employees who currently have a valid contract, ignoring any other filters that might be in place. This is useful for getting a clear view of the workforce that is actively employed under valid contracts, without being influenced by other data selections.

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

In simple terms, this measure totals all the hours logged by employees in the timesheet, providing a single number that represents the overall hours worked. This can be useful for tracking labor costs, project hours, or overall productivity within a business.

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

2. **MAX(vw_Timesheet[HoursPerWeek])**: For each employee in the unique list, this part finds the maximum number of hours they are contracted to work in a week. If an employee has multiple entries in the timesheet, it will take the highest value of hours per week recorded for that employee.

3. **SUMX(...)**: The `SUMX` function then takes the unique employee list and the maximum hours per week for each employee and sums these maximum values together. This means it adds up the highest contracted hours for each employee to get a total.

In summary, this DAX measure calculates the total of the maximum contracted hours per week for all employees, ensuring that if an employee has multiple records, only their highest contracted hours are counted. This gives a clear picture of the total contracted hours across the workforce.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression `[TotalActualHours] - [TotalContractedHours]` calculates the difference between two measures: **Total Actual Hours** and **Total Contracted Hours**.

In simple business terms, here's what it achieves:

- **Total Actual Hours** represents the actual number of hours worked or spent on a project or task.
- **Total Contracted Hours** refers to the number of hours that were agreed upon in a contract for that project or task.

By subtracting the Total Contracted Hours from the Total Actual Hours, this measure provides insight into how many hours were either over or under the contracted amount. 

- If the result is positive, it indicates that more hours were worked than what was contracted, suggesting potential overages or additional work done.
- If the result is negative, it means that fewer hours were worked than contracted, which could imply efficiency or that the project was completed in less time than expected.

Overall, this measure helps businesses assess their performance against contractual agreements regarding time and resources.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total hours tracked from a timesheet, specifically from the `vw_Timesheet` table. Here’s a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The expression first checks if the total sum of hours (`SUM(vw_Timesheet[Hours])`) is blank. A blank value typically means that there are no hours recorded or that the data is missing.

2. **Return Zero if Blank**: If the sum of hours is indeed blank (meaning no hours have been tracked), the expression will return a value of **0**. This is important because it ensures that when there are no recorded hours, the measure does not show a blank value, which could be confusing in reports.

3. **Return Total Hours if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the expression will return the actual total of those hours. 

In summary, this DAX measure effectively ensures that when calculating total hours tracked, it will either show the total hours if they exist or a zero if there are no hours recorded. This makes the reporting clearer and more user-friendly by avoiding blanks in the results.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total hours tracked from a timesheet, but it also includes a check to see if there are any hours recorded. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the `Hours` column of the `vw_Timesheet` table. Essentially, it calculates the total hours worked.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (i.e., if there are no hours recorded). If there are no hours, it means that either no timesheets were submitted or all entries are zero.

3. **IF(...)**: The IF function evaluates the condition provided by ISBLANK. If the total hours are blank (meaning there are no hours tracked), it returns the text "Empty". If there are hours tracked, it returns the total number of hours calculated.

In summary, this DAX expression helps to determine whether any hours have been recorded in the timesheet. If no hours are recorded, it indicates this by returning "Empty". If hours are present, it provides the total number of hours tracked. This is useful for reporting and understanding whether there is any activity logged in the timesheet.

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

1. **TotalHoursTracked**: This part of the expression represents the total number of hours that have been tracked for a specific employee or a group of employees. It is likely a measure that sums up all the hours recorded in a given context (like a specific time period or project).

2. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to find the maximum hours worked per week for each employee.

3. **MAXX and DISTINCT**: The `MAXX` function is used to find the maximum value from a set of values. Here, it is looking at the distinct values of `HoursPerWeek` from the `vw_Timesheet` table. This means it is checking how many hours each employee has worked in a week and identifying the highest number of hours recorded.

4. **ALLEXCEPT**: This part of the expression keeps the context for each employee (using `EmployeeID`) while ignoring any other filters that might be applied. This ensures that the calculation of maximum hours per week is done for each employee individually, without interference from other filters.

5. **Final Calculation**: The entire expression subtracts the maximum hours worked in a week (for each employee) from the total hours tracked. 

### In Summary:
The measure `trackedDiff` calculates how many hours an employee has tracked beyond their maximum recorded hours in any single week. If the result is positive, it indicates that the employee has tracked more hours than their peak weekly performance; if negative, it suggests they have tracked fewer hours than their best week. This can help managers understand employee workload and performance trends over time.

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

2. **Calculating Maximum Hours Per Week**: The expression then calculates the maximum number of hours worked per week from the `vw_Timesheet` table. It does this by:
   - Using `MAXX` to find the highest value of `HoursPerWeek`.
   - `DISTINCT` ensures that only unique values of `HoursPerWeek` are considered, so if an employee has reported the same hours multiple times, it only counts once.

3. **Removing Filters**: The `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])` part ensures that any existing filters on the `HoursPerWeek` column are ignored. This means the calculation looks at all possible values of hours worked per week, not just those that might be currently filtered in the report.

4. **Keeping Employee Context**: The `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` function keeps the context of the specific employee (or employees) being analyzed while ignoring other filters. This means the calculation is focused on the individual employee's data.

5. **Final Calculation**: Finally, the expression subtracts the maximum hours per week (calculated in the previous steps) from the total hours tracked. 

**What It Achieves**: The overall goal of this measure is to determine how many hours an employee has tracked beyond their maximum reported hours per week. If the result is positive, it indicates that the employee has tracked more hours than their maximum weekly limit; if it's negative or zero, it means they have tracked fewer or exactly their maximum hours. This can help in assessing workload, overtime, or compliance with work hour policies.

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

1. **Variable Definition (FilteredHours)**: The expression starts by creating a variable called `FilteredHours`. This variable holds a filtered version of the `vw_Timesheet` table.

2. **Filtering Criteria**: The filtering is based on two main conditions:
   - The project qualifies as billable if `vw_Timesheet[QualifyPrj]` equals 1 (indicating it's a qualifying project) **and** `vw_Timesheet[BillablePrj]` equals 1 (indicating it's a billable project).
   - Alternatively, it includes any projects that contain the phrase "Customer Success Services" in their name. This is checked using the `SEARCH` function, which looks for that specific text within the `vw_Timesheet[Project]` field.

3. **Summing Up Hours**: After filtering the timesheet data based on the above criteria, the expression uses `SUMX` to iterate over the filtered results and sum up the `vw_Timesheet[Hours]` for all the entries that meet the conditions.

4. **Return Value**: Finally, the total sum of hours that meet the specified criteria is returned as the result of the measure.

In simple terms, this DAX measure calculates the total number of hours worked on projects that are either specifically marked as billable or fall under the "Customer Success Services" category. This helps businesses track and report on billable work effectively.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** This DAX expression is used to calculate a measure called 'UTI_TOTALHOURS'. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the values in the 'Hours' column from the 'vw_Timesheet' table. Essentially, it totals the number of hours recorded in timesheets.

2. **CALCULATE(...)**: The CALCULATE function modifies the context in which the data is evaluated. In this case, it is used to apply a specific condition to the sum of hours.

3. **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to only include rows where the 'QualifyPrj' column equals 1. This means that only the hours associated with projects that meet a certain qualification (indicated by the value 1) will be included in the total.

In summary, the DAX expression calculates the total number of hours worked on projects that qualify (where 'QualifyPrj' is 1). This measure helps businesses understand how many hours are being spent on specific qualified projects, which can be important for budgeting, resource allocation, and performance tracking.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'UTILIZATION_New', which represents the ratio of billable hours to total hours worked for active employees. Here's a breakdown of what it does in simple business terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable captures the total number of hours that employees have billed to clients. It is sourced from another measure called `[UTI_TotalBillableHours]`.
   - `_TotalHours`: This variable captures the total number of hours worked by employees, sourced from another measure called `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The code checks if there are any active employees by evaluating `[Msr_ActiveContacts]`. If there are active employees (i.e., the measure is not blank), it proceeds to the next check.
   - It then checks if the total hours worked (`_TotalHours`) is not blank. If it is not blank, it moves to calculate the ratio.
   - The ratio is calculated by dividing the total billable hours (`_TotalBillableHours`) by the total hours worked (`_TotalHours`). 
   - If the result of this division is blank (which can happen if there are no hours worked), it returns 0 instead of a blank value.

3. **Final Outcome**:
   - The final output of this measure is the billable hour ratio for active employees. This ratio helps the business understand how effectively employees are utilizing their time—specifically, how much of their total working hours are being spent on billable tasks. A higher ratio indicates better utilization of employee time towards billable work.

In summary, this DAX expression calculates the utilization rate of active employees by determining the proportion of their billable hours to their total hours worked, providing valuable insights into productivity and efficiency.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

The following filters are applied to the entire report:

- Filter on (Name: `912435ffa0798d130252`) (Type: N/A, Definition: `N/A`)

### <a name="report-pages"></a>Report Pages

#### <a name="page-customer-analysis"></a>Page: Customer Analysis

*Internal Name: `ecc7667875c00809763b`, Ordinal: 0*

##### Visuals on this Page

**image**

- Type: `image`
- Name: `6f358e4969013934154d`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `e2414703060e087d3230`
- Fields Used: _(None detected)_

**TimeSpan**

- Type: `slicer`
- Name: `a60364499b7915da7e60`
- Fields Used: _(None detected)_

**Unit**

- Type: `slicer`
- Name: `be723b7e75b4661912d0`
- Fields Used: _(None detected)_

**SubUnit**

- Type: `slicer`
- Name: `e4e04919d0548163c0a8`
- Fields Used: _(None detected)_

**Client**

- Type: `slicer`
- Name: `8048e854158dc07422ce`
- Fields Used: _(None detected)_

**Sub-Own-Ext**

- Type: `slicer`
- Name: `800e477135b21a02300b`
- Fields Used: _(None detected)_

**actionButton**

- Type: `actionButton`
- Name: `88e87e1eddb866e15c8a`
- Fields Used: _(None detected)_

**Project Type**

- Type: `slicer`
- Name: `b37c531703b3e683879e`
- Fields Used: _(None detected)_

**Company**

- Type: `slicer`
- Name: `6e846506dec1641cb274`
- Fields Used: _(None detected)_

**Project Name**

- Type: `slicer`
- Name: `9285b49f5742861eada8`
- Fields Used: _(None detected)_

**Employee**

- Type: `slicer`
- Name: `b5b5047cc00ba23030e5`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `8a02ec339e5cc2bce97e`
- Fields Used: _(None detected)_

**Revenue/Week**

- Type: `lineChart`
- Name: `f5560d7670207c1c0572`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `4856952e0903150da5c7`
- Fields Used: _(None detected)_


---

#### <a name="page-project-details"></a>Page: Project Details

*Internal Name: `2f52ad4a004b09107900`, Ordinal: 1*

##### Visuals on this Page

**image**

- Type: `image`
- Name: `3e3f0339e8619259ebb2`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `c58305c304407c75000a`
- Fields Used: _(None detected)_

**TimeSpan**

- Type: `slicer`
- Name: `30ac69436bb17c560e22`
- Fields Used: _(None detected)_

**Pillar**

- Type: `slicer`
- Name: `1218b3e663e35022d9d5`
- Fields Used: _(None detected)_

**Client**

- Type: `slicer`
- Name: `91d269b51621b58cd5b8`
- Fields Used: _(None detected)_

**Company**

- Type: `slicer`
- Name: `bb8c1f8dd224d829d32b`
- Fields Used: _(None detected)_

**Status**

- Type: `slicer`
- Name: `331fc4a3b6d31c038b30`
- Fields Used: _(None detected)_

**Sub-Own-Ext**

- Type: `slicer`
- Name: `f4a2a804d812cd7dee49`
- Fields Used: _(None detected)_

**Project Type**

- Type: `slicer`
- Name: `c275d85b5800b1d47638`
- Fields Used: _(None detected)_

**Project Name**

- Type: `slicer`
- Name: `1221310b511e5b428280`
- Fields Used: _(None detected)_

**Project Leader**

- Type: `slicer`
- Name: `9402ca390dab8c29e0ca`
- Fields Used: _(None detected)_

**Employee**

- Type: `slicer`
- Name: `7e3b4de92424e0cc2bde`
- Fields Used: _(None detected)_

**actionButton**

- Type: `actionButton`
- Name: `f3f386d06e37473759a7`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `0891520feb1b0937cd30`
- Fields Used: _(None detected)_

**Actual vs Budget**

- Type: `lineChart`
- Name: `76b4488a85c8253bac24`
- Fields Used: _(None detected)_


---

