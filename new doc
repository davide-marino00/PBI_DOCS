# Power BI Model & Report Documentation

*Generated on: 2025-07-02 15:20:15*

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
    - [vw_missing_timesheet](#table-vw_missing_timesheet)
    - [vw_Timesheet](#table-vw_timesheet)
- [Report Structure](#report-structure)
  - [Report Level Filters](#report-level-filters)
  - [Report Pages](#report-pages)
    - [Page Navigation](#page-page-navigation)
    - [Timesheet Completeness (Overview)](#page-timesheet-completeness-overview)
    - [Timesheet (Missing)](#page-timesheet-missing)
    - [Timesheet (Not Approved)](#page-timesheet-not-approved)
    - [Timesheet (Not Ready)](#page-timesheet-not-ready)
    - [Timesheet (Rejected)](#page-timesheet-rejected)
    - [Out of Service](#page-out-of-service)
    - [Utilisation/Week](#page-utilisationweek)
    - [Utilisation/Month](#page-utilisationmonth)
    - [Timesheet Overview/Category (Weekly)](#page-timesheet-overviewcategory-weekly)
    - [Timesheet Overview/Category (Monthly)](#page-timesheet-overviewcategory-monthly)
    - [Timesheet Overview/Project](#page-timesheet-overviewproject)
    - [Timesheet Overview/Consultant (Weekly)](#page-timesheet-overviewconsultant-weekly)
    - [Timesheet Overview/Consultant (Monthly)](#page-timesheet-overviewconsultant-monthly)
    - [Timesheet (Details)](#page-timesheet-details)
    - [Drill](#page-drill)

---

## <a name="overview"></a>Overview

The Power BI data model appears to be designed for a business environment focused on project management and time tracking, likely within a professional services or consulting firm. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests that the model is aimed at monitoring employee time allocation to various projects, ensuring accurate reporting and analysis of time spent on tasks. The inclusion of lookup tables like 'lkp_Project', 'lkp_Unit', and 'lkp_Code' indicates a structured approach to categorizing and managing data related to projects, organizational units, and timesheet codes, which are essential for detailed reporting and performance evaluation.

The relationships established between these tables facilitate comprehensive analysis of time-related data, allowing stakeholders to identify trends, track project progress, and address issues such as missing timesheets. The 'DimDate' table further enhances the model by enabling time-based analysis, which is crucial for understanding productivity over different periods. Overall, this data model serves as a robust framework for analyzing employee time management, project performance, and operational efficiency within the organization.

---

## <a name="data-model"></a>Data Model

### <a name="relationships"></a>Relationships

The following relationships link the tables:

* `vw_missing_timesheet`.[`Project`] -> `lkp_Project`.[`Project`]
* `vw_missing_timesheet`.[`ShortDate`] -> `DimDate`.[`ShortDate`]
* `vw_missing_timesheet`.[`SubUnit`] -> `lkp_Unit`.[`Organisatorische eenheid`]
* `vw_missing_timesheet`.[`TimesheetCode`] -> `lkp_Code`.[`Code`]
* `vw_Timesheet`.[`Project`] -> `lkp_Project`.[`Project`]
* `vw_Timesheet`.[`TimesheetCode`] -> `lkp_Code`.[`Code`]
* `vw_Timesheet`.[`TimesheetDate`] -> `DimDate`.[`ShortDate`]
* `vw_Timesheet`.[`Unit`] -> `lkp_Unit`.[`Organisatorische eenheid`]
* `vw_Timesheet`.[`ContractEndDate`] -> `DimDate`.[`ShortDate`] (**Inactive**)
* `vw_Timesheet`.[`ContractStartDate`] -> `DimDate`.[`ShortDate`] (**Inactive**)

---

### <a name="tables"></a>Tables

#### <a name="table-dimdate"></a>Table: `DimDate`

The 'DimDate' table serves as a comprehensive date dimension, enabling businesses to analyze and report on time-based data effectively. It provides essential date attributes, such as day, week, month, and quarter, facilitating detailed time series analysis and enhancing the ability to track performance trends over various periods.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | Column Description: Represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry, facilitating time-based analysis and reporting in the DimDate table. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of the date, facilitating time-based analysis and reporting for scheduling and planning activities. |
| `DateKey` | `int64` | Column Description: The 'DateKey' column (int64) serves as a unique identifier for each date in the 'DimDate' table, facilitating efficient date-based queries and enabling time-based analysis across various business metrics. |
| `DayName` | `string` | The 'DayName' column in the 'DimDate' table stores the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | Column Description: The 'DayNumberOfMonth' column (int64) in the 'DimDate' table represents the numerical day of the month, ranging from 1 to 31, facilitating date-related analyses and scheduling tasks. |
| `DayNumberOfWeek` | `int64` | Column Description: The 'DayNumberOfWeek' column (int64) represents the numerical value of the day in the week, where 1 corresponds to Sunday and 7 corresponds to Saturday, facilitating date-related analyses and scheduling tasks within the DimDate table. |
| `DayNumberOfYear` | `int64` | Column Description: The 'DayNumberOfYear' column represents the sequential day of the year (ranging from 1 to 365 or 366) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `MonthName` | `string` | Column Description: The 'MonthName' column stores the full name of each month, facilitating easy reference and reporting for date-related analyses within the DimDate table. |
| `MonthNumberOfYear` | `int64` | Column Description: Represents the numerical value of the month (1 for January through 12 for December) to facilitate date-related analyses and reporting within the DimDate table. |
| `ShortDate` | `dateTime` | The 'ShortDate' column in the 'DimDate' table represents a simplified date format, capturing the specific date without time details, to facilitate efficient date-based querying and reporting. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Column Description: Indicates whether the date corresponds to the current calendar year, with values typically set to 'Yes' or 'No' for efficient time-based analysis in the DimDate table.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `CurrentYearFlag` in a data model, typically within a tool like Power BI or Excel. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The `CurrentYearFlag` column is designed to indicate whether a date in the `DimDate` table falls within the current year or not.

2. **How It Works**:
   - **YEAR(DimDate[ShortDate])**: This part extracts the year from the date in the `ShortDate` column of the `DimDate` table.
   - **YEAR(TODAY())**: This part gets the current year based on today's date.
   - **IF Statement**: The `IF` function checks if the year extracted from `DimDate[ShortDate]` is less than or equal to the current year. 
     - If it is (meaning the date is in the current year or a previous year), it returns `1`.
     - If it is not (meaning the date is in a future year), it returns `0`.

3. **Outcome**: 
   - The result is a column where each row will have a value of `1` if the date is in the current year or any previous year, and `0` if the date is in a future year. 
   - This flag can be useful for filtering or analyzing data based on whether dates are current or future, helping businesses focus on relevant time periods for reporting or decision-making.

In summary, the `CurrentYearFlag` column helps identify which dates are current or past, allowing for better analysis of time-sensitive data.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table captures and categorizes various metrics related to hours worked and their corresponding percentage contributions, enabling businesses to analyze workforce productivity and resource allocation effectively. This table serves as a critical tool for performance assessment and strategic decision-making regarding labor management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column captures the allocation of hours and their corresponding percentage contributions to specific tasks within the scheduling framework, facilitating effective resource management and task prioritization. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various time and percentage metrics related to scheduling and task enrichment activities. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column stores string values that represent the sequence or priority of hours and percentage allocations for scheduling enrichment tasks. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes and organizes various codes used across the business, facilitating data consistency and integrity. It includes attributes for code identification, qualification criteria, and sorting order, enabling efficient data retrieval and analysis in reporting and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column in the 'lkp_Code' table stores unique string identifiers that categorize and define various entities for efficient data retrieval and reference. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of codes, serving as a numerical identifier for categorizing and filtering data based on specific criteria. |
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

The 'lkp_Project' table serves as a reference for project-related data, linking project identifiers with their respective qualification and billing metrics, while categorizing them into groups for streamlined reporting and analysis. This table enables businesses to efficiently track project performance and financials, facilitating informed decision-making and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the unique identifier for billing records associated with each project, facilitating financial tracking and reporting. |
| `Group` | `string` | The 'Group' column in the 'lkp_Project' table categorizes projects into distinct classifications for streamlined management and reporting. |
| `Project` | `string` | The 'Project' column in the 'lkp_Project' table identifies the name of each project, serving as a key reference for categorizing and managing related scheduling tasks. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, with numerical values representing different levels of qualification criteria met. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their attributes such as billable status and external classifications. This table is essential for analyzing resource allocation and financial performance across different units, facilitating informed decision-making and strategic planning.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | Column Description: The 'BillableDep' column (int64) in the 'lkp_Unit' table indicates the department responsible for billable activities, facilitating accurate tracking and reporting of revenue-generating units. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column in the 'lkp_Unit' table identifies the organizational unit associated with each entry, facilitating structured data management and reporting within the organization. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores external type identifiers for owned sub-units, facilitating the classification and enrichment of scheduling tasks. |
| `Unit` | `string` | The 'Unit' column in the 'lkp_Unit' table stores the names of measurement units used for categorizing and quantifying various items in the scheduling process. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table serves as a critical resource for identifying employees who have not submitted their timesheets within a specified timeframe, enabling managers to address compliance issues and ensure accurate payroll processing. By tracking missing submissions alongside employee and contract details, this table aids in optimizing workforce management and enhancing accountability.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column (double) in the 'vw_missing_timesheet' table represents the total number of hours worked by employees that have not been recorded in their timesheets, facilitating accurate tracking and reporting of labor hours. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when a contract is scheduled to conclude, providing essential information for tracking contract durations and ensuring timely timesheet submissions. |
| `ContractHours` | `int64` | The 'ContractHours' column (int64) in the 'vw_missing_timesheet' table represents the total number of hours contracted for an employee, facilitating the identification of discrepancies in timesheet submissions. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, serving as a critical reference point for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of contracts for employees who have missing timesheets, providing insight into potential compliance or payroll issues. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_missing_timesheet' table uniquely identifies each employee associated with missing timesheet entries, facilitating efficient tracking and resolution of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees whose timesheets are missing, facilitating the identification and follow-up on outstanding time reporting issues. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours that employees have not reported on their timesheets, facilitating the identification of missing time entries for accurate payroll and project tracking. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data processing activities. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and tracking within the 'vw_missing_timesheet' table. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees who have missing timesheets, facilitating accountability and follow-up actions. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in employee timesheets, facilitating the identification of discrepancies in time tracking. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the missing timesheet records, facilitating tracking and resolution of unreported work hours. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with missing timesheets, facilitating the tracking and management of time allocation across various projects. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing a rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for processing, with a value of true signifying readiness and false indicating that it is not yet prepared. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total monetary value of sales transactions associated with missing timesheet entries, recorded as a double for precision in financial calculations. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the context of timesheet data analysis. |
| `ShortDate` | `dateTime` | Column Description: The 'ShortDate' column captures the specific date and time when a timesheet entry is missing, facilitating the tracking and management of timesheet compliance within the 'vw_missing_timesheet' table. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific organizational subunit associated with each entry in the missing timesheet records, facilitating targeted follow-up and resolution of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for timesheets, facilitating the tracking and management of missing timesheet entries within the 'vw_missing_timesheet' table. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a textual summary of the tasks or activities recorded in the timesheet, facilitating the identification and analysis of missing entries in the 'vw_missing_timesheet' table. |
| `Week_` | `int64` | The 'Week_' column (int64) in the 'vw_missing_timesheet' table represents the week number of the year associated with each timesheet entry, facilitating the identification of missing submissions by week. |
| `Year_` | `int64` | Column Description: The 'Year_' column (int64) represents the calendar year associated with each entry in the 'vw_missing_timesheet' table, facilitating the identification and analysis of timesheet data by year. |

##### Calculated Columns

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column indicates the department associated with billable hours for employees whose timesheets are missing, facilitating the identification of revenue-generating activities.
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
   - The code first checks if the `BillableDep` value from the `lkp_Unit` table is blank (i.e., there is no value associated with it).
   - If it is blank, the calculated column will return a value of **1**. This could indicate that if there is no specific billable dependency defined, the unit is treated as billable by default.
   - If the `BillableDep` value is not blank, the code then checks if this value is not equal to **0**.
     - If the `BillableDep` is not zero, it returns **1**, indicating that the unit is billable.
     - If the `BillableDep` is zero, it returns **0**, indicating that the unit is not billable.

3. **Outcome**: 
   - The final result of this calculated column will be either **1** (billable) or **0** (not billable) based on the conditions checked. 
   - This helps in identifying which units can be billed for services or resources, allowing for better financial tracking and reporting.

In summary, this DAX expression effectively categorizes units as billable or not based on their relationship with the `lkp_Unit` table, providing a straightforward way to manage billing information.

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

1. **Purpose**: The calculated column determines whether an employee is considered "active" based on their contract dates and a specific date (referred to as `ShortDate`).

2. **Conditions Checked**:
   - **Contract Start Date**: The code checks if the `ShortDate` (which likely represents a specific date, such as the date of a timesheet entry) is on or after the employee's `ContractStartDate`. This means the employee should have started working on or before this date.
   - **Contract End Date**: It also checks if the `ShortDate` is on or before the `ContractEndDate`. If the `ContractEndDate` is blank (meaning the employee's contract is still active or ongoing), this condition is satisfied regardless of the `ShortDate`.

3. **Output**:
   - If both conditions are met (the `ShortDate` is within the range of the contract dates), the calculated column will return a value of **1**, indicating that the employee is active.
   - If either condition is not met, it will return a value of **0**, indicating that the employee is not active.

### Summary:
In summary, this DAX expression effectively flags employees as "active" (1) or "not active" (0) based on whether the date of interest falls within their contract period. This can help in analyzing employee activity and managing timesheet data more effectively.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, represented as a string value, to facilitate the identification and management of missing or unsubmitted timesheets in the 'vw_missing_timesheet' view.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `IncompleteFlag` in a data model, specifically within a table named `vw_missing_timesheet`. Here's a breakdown of what this expression does in simple business terms:

### Explanation of the DAX Expression:

- **Condition Check**: The expression checks the value of the column `MissingHours` in the `vw_missing_timesheet` table.
- **Logic**: 
  - If the value of `MissingHours` is **less than 0**, it means that there is an issue or a discrepancy with the recorded hours (for example, it could indicate that hours are missing or incorrectly logged).
  - In this case, the expression returns a value of **1**, which can be interpreted as a flag indicating that the entry is incomplete or problematic.
  - If the value of `MissingHours` is **0 or greater**, the expression returns **0**, indicating that there are no issues with the hours recorded.

### What It Achieves:

- **Flagging Incomplete Entries**: This calculated column effectively flags records where there are missing or negative hours, allowing users to easily identify which entries need attention or correction.
- **Data Quality Improvement**: By highlighting these incomplete records, the organization can take action to ensure that all timesheets are accurate and complete, which is crucial for payroll, project tracking, and resource management.

In summary, the `IncompleteFlag` column helps in identifying and managing entries with missing or negative hours, thereby supporting better data integrity and operational efficiency.

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

1. **Check for Related Data**: The code uses the `RELATED` function to look up a value from another table called `lkp_Unit`. Specifically, it is trying to find the `Unit` associated with the current row in the main table.

2. **Handle Missing Data**: The `ISBLANK` function checks if the value retrieved from the `lkp_Unit` table is blank (i.e., there is no corresponding unit found). 

3. **Return Values Based on the Check**:
   - If the `Unit` value is blank (meaning there is no related unit), the code returns the string "Unknown". This indicates that the system could not find a valid unit for that particular entry.
   - If the `Unit` value is not blank (meaning a valid unit was found), it returns the actual unit value from the `lkp_Unit` table.

### Summary:
In essence, this DAX expression ensures that for each row in the main table, there is a clear indication of the unit associated with it. If a unit is found, it displays that unit; if not, it labels it as "Unknown". This helps maintain clarity and consistency in reporting, especially when dealing with incomplete data.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column (string) in the 'vw_missing_timesheet' table represents the specific year and week number, facilitating the identification of timesheet entries that are missing for a given week in the reporting period.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'YearWeek' in a data model, specifically for a table that includes a date field named `ContractEndDate`. Here's a breakdown of what this expression does in simple business terms:

1. **Extracting the Year**: The function `YEAR([ContractEndDate])` takes the date from the `ContractEndDate` column and extracts the year from it. For example, if the `ContractEndDate` is December 15, 2023, this part of the expression will return `2023`.

2. **Calculating the Week Number**: The function `WEEKNUM([ContractEndDate])` calculates the week number of the year for the given `ContractEndDate`. Continuing with the previous example, if the `ContractEndDate` is in the 50th week of the year, this function will return `50`.

3. **Formatting the Week Number**: The `FORMAT(..., "00")` function ensures that the week number is always displayed as a two-digit number. So, if the week number is `5`, it will be formatted as `05`.

4. **Combining Year and Week Number**: The `&` operator is used to concatenate (join together) the year and the formatted week number with a hyphen in between. So, if the year is `2023` and the week number is `50`, the final result will be `2023-50`.

### Summary:
The overall purpose of this DAX expression is to create a unique identifier for each week of the year based on the `ContractEndDate`. The resulting 'YearWeek' column will look like `YYYY-WW`, where `YYYY` is the year and `WW` is the two-digit week number. This can be useful for reporting and analysis, allowing users to easily group or filter data by week and year.

##### Measures

**`Count_Negative_Consultant`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to create a measure called `Count_Negative_Consultant`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure counts the number of unique consultants (employees) who have recorded negative hours in their timesheets. Negative hours might indicate that these consultants have made adjustments or corrections to their reported hours, such as deductions or errors.

2. **Components**:
   - **`CALCULATE` Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data being counted.
   - **`DISTINCTCOUNT` Function**: This counts the number of unique entries in a specified column. Here, it counts unique employee names from the `vw_missing_timesheet` table.
   - **Filter Condition**: The condition `'vw_missing_timesheet'[MissingHours] < 0` filters the data to include only those records where the `MissingHours` (the hours reported by the consultants) are less than zero.

3. **Outcome**: The measure ultimately returns the total number of distinct consultants who have negative hours recorded in their timesheets. This can help management identify potential issues with time reporting or highlight consultants who may need further review or support.

In summary, `Count_Negative_Consultant` provides insight into how many unique consultants have reported negative hours, which can be crucial for understanding time management and ensuring accurate reporting within the organization.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to create a measure called **CountNegativeMissingHours**. Here’s a breakdown of what it does in simple business terms:

1. **Purpose**: This measure counts the number of unique employees who have recorded negative missing hours in the 'vw_missing_timesheet' table.

2. **Components**:
   - **CALCULATE**: This function changes the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.
   - **DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID])**: This part counts the number of unique Employee IDs in the 'vw_missing_timesheet' table. It ensures that each employee is only counted once, even if they have multiple entries.
   - **'vw_missing_timesheet'[MissingHours] < 0**: This filter condition specifies that only records where the MissingHours are less than zero should be considered. In other words, it focuses on cases where employees have negative missing hours, which might indicate an error or a specific situation that needs attention.

3. **Outcome**: The measure ultimately provides a count of how many distinct employees have negative missing hours. This information can be useful for identifying potential issues in timesheet reporting or for understanding employee attendance patterns.

In summary, **CountNegativeMissingHours** helps the business track and analyze instances where employees have reported negative hours, allowing for better management of timesheet accuracy and employee attendance.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the number of employees who are currently active but have missing timesheet hours for a specific period. Here's a breakdown of what it does in simple business terms:

1. **Data Source**: The code uses a data source called `vw_missing_timesheet`, which likely contains records of employee hours worked, contract hours, and their active status.

2. **Summarization**: The code first summarizes the data by grouping it based on several key attributes:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a more specific division within the unit)

   For each group, it calculates two important values:
   - **MissingHours**: This is calculated by taking the total hours worked by the employee in that week and subtracting their contracted hours. If this value is negative, it indicates that the employee has not logged enough hours compared to what they are contracted for.
   - **MinActiveEmp**: This value captures the minimum number of active employees for that group, which helps identify if there is at least one active employee in that grouping.

3. **Filtering**: After summarizing the data, the code filters the results to focus only on those groups where:
   - There is at least one active employee (`MinActiveEmp = 1`).
   - The calculated missing hours are negative (`MissingHours < 0`), meaning the employee has not logged enough hours.

4. **Counting**: Finally, the code counts the number of rows that meet these criteria. Each row represents an active employee who has missing timesheet hours.

### Summary:
In essence, this DAX measure calculates how many active employees have not logged enough hours in their timesheets for a given week and year. This information can be crucial for management to identify potential issues with timesheet compliance and ensure that all employees are accurately reporting their hours worked.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is designed to calculate a measure called `Dax_MissingHours`, which focuses on identifying and summing up the missing hours for active employees in a specific context (like a week or year). Here’s a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code first creates a variable called `ActiveEmployees`. This variable filters a summarized table (`vw_missing_timesheet`) to focus on employees who are currently active. It does this by checking two conditions:
   - The employee must be marked as active (`MinActiveEmp = 1`).
   - The total hours they worked must be less than their contracted hours (`MissingHours < 0`), indicating they have not met their expected working hours.

2. **Summarize Data**: The `SUMMARIZE` function is used to create a new table that groups data by employee name, year, week, unit, and subunit. For each group, it calculates:
   - **MissingHours**: This is calculated as the total hours worked by the employee minus their contracted hours. If this value is negative, it indicates that the employee has not worked enough hours.
   - **MinActiveEmp**: This captures the minimum value of a field that indicates whether the employee is active (1 for active, 0 for inactive).

3. **Filter for Relevant Employees**: The `FILTER` function then narrows down this summarized data to only include those employees who are active and have missing hours (i.e., they worked fewer hours than expected).

4. **Calculate Total Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered list of active employees and sums up their `MissingHours`. This gives the total number of hours that active employees are missing based on the criteria defined.

### Summary:
In essence, this DAX measure calculates the total number of hours that active employees have not worked compared to their contracted hours, focusing only on those who are currently active and have a deficit in their hours. This can help a business identify gaps in employee attendance or performance, allowing for better resource management and planning.

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

1. **Data Source**: The calculation is based on a data table called `vw_missing_timesheet`, which likely contains records of timesheets for employees, including their contract hours and the week associated with each entry.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key pieces of information:
   - `Week_`: This represents the week for which we are calculating the expected contract hours.
   - `EmployeeID`: This identifies each employee.

3. **Calculating Maximum Contract Hours**: For each unique combination of week and employee, the code calculates the maximum contract hours using `MAX(vw_missing_timesheet[ContractHours])`. This means that if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

4. **Summing Up Contract Hours**: The `SUMX` function then takes the results from the `SUMMARIZE` step and sums up all the maximum contract hours calculated for each employee across all weeks. Essentially, it adds up the highest contract hours for each employee for each week.

### What It Achieves:
The overall goal of this DAX measure is to provide a total of the expected contract hours for all employees, ensuring that if there are multiple entries for any employee in a given week, only the highest contract hours are considered. This helps in understanding the total expected workload for employees based on their contractual agreements, while avoiding double counting in cases where there are multiple records for the same week.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the difference between two values related to hours worked and contracted hours for employees or contractors. Here’s a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum value of contracted hours from the `vw_missing_timesheet` table. Contracted hours refer to the total number of hours that an employee or contractor is expected to work as per their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum value of actual hours worked (or logged) from the same table. This represents the total number of hours that the employee or contractor has actually worked.

3. **Calculation**: The expression then subtracts the maximum actual hours worked from the maximum contracted hours. 

### What It Achieves:
The result of this calculation gives you the maximum number of hours that are missing or unaccounted for. In other words, it shows how many hours an employee or contractor has not worked compared to what they were supposed to work according to their contract. 

### Business Implication:
This measure is useful for identifying gaps in hours worked, which can help in managing workforce efficiency, ensuring compliance with contracts, and addressing any issues related to underperformance or missed work hours. It can also assist in payroll and resource planning by highlighting discrepancies between expected and actual work hours.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the "Percentage Completeness" of timesheet submissions in a business context. Here’s a breakdown of what it does in simple terms:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part of the code adds up all the hours that are missing from timesheets. Essentially, it gives you the total number of hours that employees have not reported.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that employees are expected to work in a week according to their contracts. It serves as a benchmark for comparison.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the code calculates the difference between the total missing hours and the expected hours. If the result is positive, it indicates that the missing hours exceed what was expected, suggesting a shortfall in timesheet submissions.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: This function divides the result from the previous step (the difference between missing hours and expected hours) by the expected contract hours. The `DIVIDE` function is used instead of a simple division to handle cases where the denominator (expected hours) might be zero. If it is zero, the function will return 0 instead of causing an error.

### What It Achieves:
The final result of this DAX expression is a percentage that indicates how far off the actual timesheet submissions are from what is expected. 

- If the percentage is positive, it means that there are more missing hours than expected, indicating a problem with timesheet compliance.
- If the percentage is zero or negative, it suggests that the timesheet submissions are meeting or exceeding expectations.

In summary, this measure helps the business understand the completeness of timesheet submissions relative to what is expected, allowing for better management of employee hours and compliance tracking.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is designed to calculate a measure called `UniqueEmployeesPerUnit`. Let's break it down into simpler terms to understand what it achieves:

1. **Data Source**: The measure uses a table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including information about whether their submissions are complete or incomplete.

2. **Grouping Data**: The `SUMMARIZE` function is used to create a new table that groups the data by three columns:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This is the name of the employee.
   - `MAIN_UNIT`: This indicates the main unit or department the employee belongs to.

   For each unique combination of these three columns, the code also calculates a new column called `IncompleteFlag`, which takes the maximum value of the `IncompleteFlag` for that group. This flag likely indicates whether any of the timesheet submissions for that employee in that week are incomplete (e.g., 1 for incomplete, 0 for complete).

3. **Calculating the Sum**: After summarizing the data, the `SUMX` function iterates over the summarized table. It sums up the values in the `IncompleteFlag` column for each group. This means it counts how many unique employees have at least one incomplete timesheet submission for each unit in each week.

### What It Achieves:
In summary, this DAX measure calculates the total number of unique employees who have incomplete timesheet submissions, grouped by week and by their main unit. This information can be valuable for management to identify which units or weeks have higher instances of incomplete timesheets, allowing them to take action to improve compliance and ensure that all employees are submitting their timesheets on time.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and personnel identifiers. This data enables management to analyze labor allocation, project costs, and employee productivity, facilitating informed decision-making and resource optimization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column in the 'vw_Timesheet' table identifies the specific client associated with each timesheet entry, facilitating accurate billing and project tracking. |
| `Code2` | `string` | The 'Code2' column in the 'vw_Timesheet' table represents a secondary identifier or classification code used to categorize or differentiate specific time entries for enhanced reporting and analysis. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column in the 'vw_Timesheet' table represents the date and time when an employee's contract is scheduled to conclude, facilitating accurate tracking of contract durations for payroll and resource planning. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when a contract becomes effective, serving as a reference point for tracking timesheet entries related to that contract within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | Column Description: Indicates the current status of the contract associated with the timesheet entry, represented as a string value. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of contracts as of today, providing essential insights for performance tracking and decision-making within the timesheet reporting framework. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential timing information for tracking contract changes within the timesheet management process. |
| `CostAmount` | `double` | The 'CostAmount' column represents the total monetary cost associated with the recorded time entries in the 'vw_Timesheet' table, expressed as a double-precision floating-point number. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor or resources for each entry in the timesheet, expressed as a double to allow for precise financial calculations. |
| `Currency` | `string` | The 'Currency' column in the 'vw_Timesheet' table specifies the type of currency used for financial entries, facilitating accurate cost tracking and reporting across different currencies. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the individual or entity responsible for payment, facilitating the tracking and management of financial obligations within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_Timesheet' table uniquely identifies each employee, facilitating accurate tracking and reporting of their timesheet entries. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column in the 'vw_Timesheet' table uniquely identifies each employee by their name, facilitating easy reference and reporting on timesheet entries. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) in the 'vw_Timesheet' table uniquely identifies each employee associated with their respective timesheet entries, facilitating accurate tracking and reporting of work hours. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating easy identification and reporting of labor hours. |
| `Employer` | `string` | The 'Employer' column in the 'vw_Timesheet' table identifies the organization or company that employs the individual associated with the recorded timesheet entries. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating accurate payroll and reporting processes. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to each timesheet entry, providing insights that may not be captured by standard fields. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with each timesheet entry, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked on various tasks, facilitating accurate tracking and reporting of employee time allocation. |
| `HoursperWeek` | `int64` | Column Description: The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee each week, facilitating effective time management and resource allocation. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee work patterns. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, enabling efficient tracking and reporting of different work hour classifications. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects within the vw_Timesheet table. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) serves as a unique identifier for project managers within the 'vw_Timesheet' table, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing the timesheet entries in the vw_Timesheet table. |
| `IPMIDName` | `string` | The 'IPMIDName' column in the 'vw_Timesheet' table stores the names of individuals associated with specific IPMIDs, facilitating the tracking and management of timesheet entries related to those identifiers. |
| `JobTitle` | `string` | The 'JobTitle' column in the 'vw_Timesheet' table captures the specific role or position of an employee, providing context for the hours worked and tasks performed during the reporting period. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking timesheet records to related datasets, facilitating efficient data integration and analysis. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_Timesheet' table identifies the unique identifier of the manager responsible for overseeing the associated timesheet entries. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column in the 'vw_Timesheet' table stores the names of managers associated with each timesheet entry, facilitating easy identification of managerial oversight for time tracking and reporting purposes. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table uniquely identifies the manager associated with each timesheet entry, facilitating the tracking and management of employee work hours and approvals. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the manager responsible for overseeing the timesheet entries, facilitating accountability and tracking of employee work hours. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string representations of scheduled work bookings for operational hours, facilitating the tracking and management of time allocation within the timesheet framework. |
| `Project` | `string` | The 'Project' column in the 'vw_Timesheet' table identifies the specific project associated with each recorded time entry, facilitating accurate tracking and reporting of time spent on various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column in the 'vw_Timesheet' table uniquely identifies the associated project for each timesheet entry, facilitating accurate tracking and reporting of time spent on specific projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column in the 'vw_Timesheet' table captures the specific project details associated with each timesheet entry, enabling effective tracking and analysis of time spent on various projects. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the project profile associated with each timesheet entry, facilitating accurate tracking and reporting of project-related labor costs. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, enabling better analysis and reporting of resource allocation across different project types. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing a rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet is ready for reporting, with a value of true signifying readiness and false indicating that further action is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double precision value, within the context of timesheet data analysis for financial reporting and performance evaluation. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale recorded in the timesheet, expressed as a double-precision floating-point number for precise financial calculations. |
| `Status` | `string` | The 'Status' column indicates the current state of each timesheet entry, reflecting whether it is pending, approved, or rejected. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation, with a value of true representing taxed hours and false representing non-taxed hours. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, helping to streamline task management and resource allocation. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for each timesheet entry, facilitating the tracking and management of employee work hours and project allocations within the vw_Timesheet table. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking of work hours and project timelines. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the tasks and activities recorded in the timesheet, facilitating better understanding and analysis of employee work efforts. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table represents the specific measurement unit associated with the time entries, facilitating accurate tracking and reporting of hours worked across various tasks. |
| `UnitCode` | `string` | The 'UnitCode' column in the 'vw_Timesheet' table represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating accurate tracking and reporting of labor costs by department. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and coordination related to timesheet submissions and approvals. |
| `YearWeek` | `string` | The 'YearWeek' column (string) in the 'vw_Timesheet' table represents the year and week number of the timesheet entries, facilitating time-based reporting and analysis of work hours. |

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

1. **Purpose**: The calculated column determines whether a timesheet is considered "approved" based on specific criteria.

2. **Conditions Checked**:
   - The first condition checks if the `Approved` column is marked as `TRUE`. This means that if the timesheet has been officially approved, it meets the criteria.
   - The second condition checks if the `TimesheetCode` for that entry is either "V" or "Z". These codes likely represent specific types of timesheets that are automatically considered approved regardless of the `Approved` status.

3. **Output**:
   - If either of the conditions is met (the timesheet is approved or it has a code of "V" or "Z"), the calculated column will return a value of `1`. This indicates that the timesheet is approved.
   - If neither condition is met, it will return a value of `0`, indicating that the timesheet is not approved.

4. **Summary**: In essence, this DAX expression helps to flag timesheets as approved (1) or not approved (0) based on whether they are officially approved or if they fall under specific codes (V or Z). This can be useful for reporting and analysis, allowing users to quickly identify which timesheets are considered approved based on these criteria.

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column in the 'vw_Timesheet' table identifies the department associated with billable hours, facilitating accurate tracking and reporting of chargeable time for client projects.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column `BillableDep` is designed to determine whether a certain unit is billable based on its related data.

2. **Logic**:
   - The code first checks if the related value from the `lkp_Unit` table for `BillableDep` is blank (i.e., there is no value).
   - If it is blank, the calculated column will return a value of **1**. This indicates that if there is no information about whether the unit is billable, it defaults to being considered billable.
   - If the related `BillableDep` value is not blank, the code then checks if this value is not equal to **0**.
     - If the value is not **0**, it returns **1**, meaning the unit is considered billable.
     - If the value is **0**, it returns **0**, indicating that the unit is not billable.

3. **Outcome**: The final result of this calculation is a column that will have a value of **1** (billable) or **0** (not billable) based on the conditions checked. This helps in identifying which units can be billed for services or products, based on their related data.

In summary, this DAX expression effectively categorizes units as billable or not based on the presence and value of a related field, ensuring that any unit without specific information is treated as billable by default.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillablePrj` in a data model, specifically in a table named `vw_Timesheet`. Here's a breakdown of what this code does in simple business terms:

1. **Purpose**: The `BillablePrj` column is designed to determine whether a particular timesheet entry is considered "billable" or not. A billable entry is one that can be charged to a client or customer.

2. **Conditions for Billability**: The code checks three specific conditions to decide if an entry is billable:
   - **Sales Price Check**: It first checks if the `SalesPrice` for the entry is greater than 0. If there is a positive sales price, it indicates that the work done can be billed to a client.
   - **Project Profile Code Check**: It then checks if the `ProjectProfileCode` is equal to 81. This specific code likely represents a type of project that is always considered billable.
   - **Project Name Check**: Finally, it looks for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it suggests that the project is related to customer success services, which are typically billable.

3. **Output**: 
   - If any of the three conditions are met (i.e., if the sales price is greater than 0, the project profile code is 81, or the project name includes "Customer Success Services"), the calculated column will return a value of **1**. This indicates that the entry is billable.
   - If none of the conditions are met, it returns a value of **0**, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project name. This helps in identifying which work can be charged to clients, aiding in accurate billing and financial reporting.

**`cc_Employer`** (`string`)

- **Description:** The 'cc_Employer' column in the 'vw_Timesheet' table stores the name of the employer associated with each timesheet entry, facilitating the tracking and management of employee work hours by employer.

**`Employee_WeeklyHours`** (`string`)

- **Description:** The 'Employee_WeeklyHours' column captures the total number of hours worked by each employee during the week, represented as a string for flexible formatting and reporting within the timesheet data.
- **DAX Expression:**
```dax
CONCATENATE(
			    ('vw_Timesheet'[EmployeeName]),
			    " - " & ('vw_Timesheet'[HoursperWeek])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet creates a new calculated column called `Employee_WeeklyHours` in a data model, specifically from a table named `vw_Timesheet`. 

Here’s a breakdown of what it does in simple business terms:

1. **Combining Information**: The code takes two pieces of information from the `vw_Timesheet` table:
   - The name of the employee (`EmployeeName`).
   - The number of hours that employee works per week (`HoursperWeek`).

2. **Formatting the Output**: It combines these two pieces of information into a single string. The format of the output will be:
   - The employee's name followed by a dash (" - ") and then the number of hours they work per week.

3. **Example Output**: If an employee named "John Doe" works 40 hours per week, the resulting value in the `Employee_WeeklyHours` column would be:
   - "John Doe - 40"

4. **Purpose**: This calculated column is useful for quickly viewing and understanding how many hours each employee works per week, all in one easy-to-read format. It can help in reporting, analysis, and decision-making regarding workforce management.

In summary, this DAX expression effectively creates a clear and concise representation of each employee's name alongside their weekly working hours, making it easier to analyze employee workloads at a glance.

**`GroupCat`** (`string`)

- **Description:** The 'GroupCat' column categorizes timesheet entries into specific groups, facilitating the organization and analysis of time allocation across various projects or tasks.
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

2. **Lookup for Group**: 
   - The code first checks if there is a corresponding group for a project in the `lkp_Project` table. It does this by looking up the project name from the `vw_Timesheet` table in the `lkp_Project` table.
   - If a match is found (meaning the project has a defined group), it retrieves the group name.

3. **Handling Blank Values**:
   - If the lookup returns a blank (meaning there is no group associated with the project), the code moves to the next step.

4. **Billable Check**:
   - If there is no group found, it checks if the project is billable by looking at the `BillablePrj` column in the `vw_Timesheet` table.
   - If `BillablePrj` equals 1 (indicating the project is billable), it assigns the category "Billable".
   - If `BillablePrj` does not equal 1, it assigns the category "Other Unbillable".

5. **Final Output**:
   - The final result of this calculation is either the group name (if found) or one of two categories: "Billable" or "Other Unbillable", depending on the project's billable status.

In summary, this DAX expression helps categorize projects into groups or identifies them as billable or unbillable, providing clarity on project status for reporting and analysis.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called **IsContractActive** in a data model, specifically within a table named **vw_Timesheet**. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column determines whether a contract is currently active or not.

2. **Conditions Checked**:
   - **ISBLANK('vw_Timesheet'[ContractEndDate])**: This part checks if the **ContractEndDate** field is empty (or blank). If it is blank, it implies that there is no end date set for the contract, which typically means the contract is ongoing or active.
   - **'vw_Timesheet'[ContractEndDate] > TODAY()**: This part checks if the **ContractEndDate** is greater than today's date. If the end date is in the future, it means the contract is still active.

3. **Output**:
   - If either of the above conditions is true (meaning the contract has no end date or the end date is still in the future), the calculated column will return **"Active"**.
   - If neither condition is true (meaning there is an end date that has already passed), it will return **"Not Active"**.

### Summary:
In summary, this DAX expression effectively labels each contract in the **vw_Timesheet** table as either "Active" or "Not Active" based on whether the contract has an end date that is either blank or still in the future. This helps users quickly identify which contracts are currently valid and ongoing.

**`MAIN_UNIT`** (`string`)

- **Description:** The 'MAIN_UNIT' column in the 'vw_Timesheet' table identifies the primary organizational unit responsible for the recorded time entries, facilitating effective resource allocation and project management.
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
   - If there is no related value (meaning the 'Unit' field is blank or missing), the code will return the text "Unknown". This is a way to handle situations where the expected data is not available, ensuring that the output is clear and informative.
   - If there is a related value (meaning the 'Unit' field has data), it will return that value.

3. **Purpose**: The overall purpose of this calculated column is to provide a clear and user-friendly representation of the 'Unit' associated with each row. If the unit is known, it shows that unit; if not, it indicates that the unit is "Unknown". This helps in maintaining data integrity and clarity in reporting or analysis.

In summary, this DAX expression ensures that every row in the 'MAIN_UNIT' column either displays the corresponding unit name or indicates that the unit is unknown, making it easier for users to understand the data at a glance.

**`MonthNumber`** (`string`)

- **Description:** Column Description: The 'MonthNumber' column stores the numerical representation of the month (e.g., "01" for January, "02" for February) as a string, facilitating time-based analysis and reporting within the timesheet data.
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

### Business Purpose:
- **Simplifies Analysis**: By converting dates into month numbers, it makes it easier to analyze data on a monthly basis. For example, you can quickly group or filter data by month.
- **Facilitates Reporting**: This calculated column can be used in reports to show trends, performance, or other metrics aggregated by month, helping stakeholders understand patterns over time.

In summary, this DAX expression helps in transforming date information into a more usable format for analysis and reporting, focusing specifically on the month aspect of the date.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' in the 'vw_Timesheet' table represents the unique identifier for employees, facilitating the tracking and management of timesheet entries associated with each individual.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

### What it Achieves:
- **Count of Unique Employees**: This expression counts how many different employees are present in the timesheet data. For example, if the timesheet includes entries for the same employee multiple times, this function will only count that employee once.
- **Data Analysis**: This is useful for understanding workforce participation, tracking how many distinct employees have logged hours, or analyzing employee engagement over a specific period.

### In Simple Terms:
Imagine you have a list of employees who worked on various projects, and some employees worked on multiple projects. This DAX expression helps you find out how many different employees contributed to the projects, regardless of how many times they appear in the list.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table captures the classification of time entries as either owned, subcontracted, or external, facilitating accurate tracking and reporting of resource allocation and project costs.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used in a calculated column to retrieve a specific value from a related table. Here’s a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a data model where there are two tables that have a relationship. For example, you might have a main table (let's call it "Sales") and a lookup table (let's call it "lkp_Unit").

2. **Purpose**: The purpose of this expression is to pull in a value from the "lkp_Unit" table into the "Sales" table. Specifically, it retrieves the value from the column named "OWN-Sub-ExtT" in the "lkp_Unit" table.

3. **How it Works**: 
   - The `RELATED` function looks for a matching row in the "lkp_Unit" table based on the relationship defined in the data model.
   - It then fetches the value from the "OWN-Sub-ExtT" column of that matching row.

4. **Outcome**: By using this expression, each row in the "Sales" table will now have an additional column that contains the corresponding "OWN-Sub-ExtT" value from the "lkp_Unit" table. This allows for enhanced analysis and reporting, as you can now use this additional information in your calculations or visualizations.

In summary, this DAX expression enriches the data in the main table by linking it to relevant information from a related table, enabling better insights and decision-making.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named `QualifyPrj`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column is designed to determine a qualification status for projects based on related data from another table called `lkp_Project`.

2. **Logic**:
   - The code checks if the `Qualify` field from the `lkp_Project` table is blank (i.e., it has no value).
   - If the `Qualify` field is blank, the calculated column will return a value of **1**. This could indicate a default or a specific status, such as "not qualified" or "unknown."
   - If the `Qualify` field is not blank (meaning it has a value), the calculated column will return the actual value from the `Qualify` field in the `lkp_Project` table.

3. **Outcome**: The result of this calculation is that for each row in the table where this calculated column is being created, you will either get a default value of **1** (if there is no qualification information available) or the specific qualification value from the related project (if it exists). 

In summary, this DAX expression helps to ensure that every project has a qualification status, either by providing a default value when no information is available or by using the existing qualification data when it is present.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the specific organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor allocation within the 'vw_Timesheet' table.

**`RReady_With_V_Z`** (`string`)

- **Description:** Column Description: Indicates the readiness status of a resource for a specific task, represented as a string, within the context of timesheet management and scheduling.
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
   - The first condition checks if the value in the column `[ReportReady]` is `TRUE`. This means that if the report is ready, the condition is satisfied.
   - The second condition checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means that if the timesheet code is one of these two specific codes, the condition is also satisfied.

3. **Output**:
   - If either of the conditions is true (i.e., the report is ready or the timesheet code is "V" or "Z"), the calculated column will return a value of 1.
   - If neither condition is met, it will return a value of 0.

### Summary:
In summary, this DAX expression is used to flag rows in the dataset where either the report is ready or the timesheet code is "V" or "Z". A value of 1 indicates that one of these conditions is met, while a value of 0 indicates that neither condition is satisfied. This can be useful for filtering or analyzing data based on readiness or specific timesheet codes.

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

1. **Condition Check**: The measure first checks the value of another measure called `[ContractStatusMeasure]`. It specifically looks to see if this value is equal to "Active".

2. **Return Value Based on Condition**:
   - If the condition is true (meaning the contract status is "Active"), the measure returns the value of another measure called `[HoursDifference]`. This likely represents the difference in hours related to the contract, such as the number of hours worked or remaining.
   - If the condition is false (meaning the contract status is not "Active"), the measure returns the text "Not Active".

### Summary:
In essence, `ContractStatus2` helps to determine the status of a contract. If the contract is currently active, it provides a numerical value (the hours difference). If the contract is not active, it simply indicates that by returning the text "Not Active". This measure is useful for reporting or analysis where understanding the status of contracts and their associated hours is important.

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

1. **Check for End Date**: The measure first looks at the maximum value of the `ContractEndDate` from the `vw_Timesheet` table. This is essentially checking the latest end date of any contract associated with the timesheet data.

2. **Evaluate Conditions**:
   - **Is Blank**: It checks if this maximum end date is blank (meaning there is no end date specified for the contract).
   - **Is Future Date**: It also checks if the maximum end date is greater than today’s date (meaning the contract is still active because it hasn’t ended yet).

3. **Determine Status**:
   - If either of the above conditions is true (the end date is blank or it is a future date), the measure returns "Active". This indicates that the contract is currently valid and ongoing.
   - If neither condition is true (meaning there is a specific end date that is in the past), it returns "Not Active". This indicates that the contract has ended.

### Summary:
In summary, this measure helps businesses quickly identify whether contracts are currently active or not based on their end dates. If a contract has no end date or is set to end in the future, it is considered "Active"; otherwise, it is "Not Active". This can be useful for tracking contract statuses and managing resources effectively.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 15, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated based on the first week of the year, which can vary depending on the system used (e.g., whether the week starts on Sunday or Monday).

So, when you combine these two functions, `WEEKNUM(TODAY())` effectively tells you which week of the year it is right now. 

### In Business Terms:
This measure is useful for reporting and analysis purposes. For instance, if you want to track sales, performance, or any other metrics on a weekly basis, this measure helps you identify the current week. This can be particularly important for businesses that operate on a weekly cycle, allowing them to make timely decisions based on the most recent data.

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
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries in the timesheet.
   - **`vw_Timesheet[Approved] = FALSE()`**: This condition filters the data to include only those timesheet entries where the `Approved` status is set to `FALSE`. In other words, it focuses on timesheets that have not been approved.

3. **Outcome**: The measure ultimately provides a count of distinct employees who have submitted timesheets that are still pending approval. This information can be useful for tracking outstanding approvals and managing workflow within the organization.

In summary, `Dax_EmpCount_Approved` helps businesses understand how many employees are waiting for their timesheets to be approved, allowing for better management of timesheet processing and approvals.

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
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows us to apply filters to our calculations.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it tells us how many different employees have submitted timesheets.
   - **`vw_Timesheet[ReportReady] = FALSE()`**: This condition filters the data to include only those timesheets where the `ReportReady` status is set to FALSE. In other words, it focuses on timesheets that are not ready for reporting.

3. **Outcome**: The measure ultimately provides the total count of distinct employees who have timesheets that are still pending or not ready for reporting. This can help a business understand how many employees still need to finalize their timesheets before they can be processed or reported.

In summary, `Dax_EmpCount_RReady` helps track the number of employees with incomplete timesheets, which is useful for ensuring timely reporting and identifying any potential delays in the timesheet submission process.

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

When you use this measure, it provides a total count of different employees who have logged their hours in the timesheet. For example, if three employees submitted timesheets, the result of this measure would be 3, regardless of how many times each employee submitted their timesheet. 

In summary, this measure helps businesses understand how many distinct employees are actively participating in timesheet reporting, which can be useful for tracking workforce engagement, project involvement, or resource allocation.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`Msr_ActiveContacts`**

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the percentage of hours worked by a specific group, project, or employee relative to the total hours worked across all selected categories in a report or dashboard. Here’s a breakdown of what it does in simple business terms:

1. **TotalValue Calculation**:
   - The first part of the code defines a variable called `TotalValue`. This variable calculates the total number of hours worked by summing up the `Hours` column from the `vw_Timesheet` table.
   - The `CALCULATE` function is used here to modify the context of the calculation. It considers all selected filters for `GroupCat`, `Project`, and `EmployeeName`, meaning it will sum the hours for all entries that are currently selected in the report, regardless of any other filters that might be applied.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours worked by the current context (which could be a specific group, project, or employee) by dividing the sum of hours for that context by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safe way to perform division in DAX. If the `TotalValue` is zero (which would mean no hours were recorded), it will return 0 instead of causing an error.

### Summary:
In summary, this measure calculates the percentage of hours worked by a specific group, project, or employee compared to the total hours worked across all selected categories. This helps in understanding how much of the total effort is contributed by a particular segment, which can be useful for performance analysis, resource allocation, and reporting.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called "Static Total Employees." Let's break it down into simpler terms to understand what it achieves:

1. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part of the code counts the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table. Essentially, it tells us how many different employees are present in the dataset.

2. **CALCULATE(...)**: The `CALCULATE` function modifies the context in which the data is evaluated. In this case, it is used to apply specific filters to the counting operation.

3. **ALL(vw_Timesheet)**: This function removes any existing filters that might be applied to the `vw_Timesheet` table. By doing this, it ensures that the count of employees is not affected by any other filters that might be in place in the report or dashboard. It allows the measure to consider all records in the `vw_Timesheet` table.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This condition filters the data to only include employees whose contract status is marked as "Valid." This means that only employees who are currently active or valid according to the contract status will be counted.

### Summary:
In summary, this DAX measure calculates the total number of unique employees who have a valid contract status, ignoring any other filters that might be applied in the report. It provides a clear view of how many employees are currently considered valid, which is useful for understanding workforce status and planning.

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

In simple terms, this measure totals all the hours worked by employees as recorded in the timesheet. It provides a single number that represents the overall actual hours worked, which can be useful for reporting, payroll, or project management purposes.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to calculate a measure called 'TotalContractedHours'. Here’s a breakdown of what it does in simple business terms:

1. **Context of the Calculation**: The measure is focused on a table called `vw_Timesheet`, which likely contains records of timesheets for employees, including their names and the hours they are contracted to work each week.

2. **VALUES Function**: The `VALUES(vw_Timesheet[EmployeeName])` part retrieves a unique list of employee names from the `vw_Timesheet` table. This means that if there are multiple entries for the same employee, it will only consider each employee once.

3. **SUMX Function**: The `SUMX` function is an iterator that goes through each unique employee name obtained from the `VALUES` function. For each employee, it performs a calculation and then sums up the results.

4. **MAX Function**: Inside the `SUMX`, the `MAX(vw_Timesheet[HoursPerWeek])` function is used. This function finds the maximum value of `HoursPerWeek` for each employee. Essentially, it looks at all the hours recorded for each employee and picks the highest number of hours they are contracted to work in a week.

5. **Final Calculation**: The overall effect of this measure is to calculate the total contracted hours for all employees by summing up the maximum weekly hours for each unique employee. 

In summary, the 'TotalContractedHours' measure calculates the total number of hours that all employees are contracted to work per week, based on the highest recorded hours for each employee in the timesheet data. This can help management understand the total workforce capacity in terms of contracted hours.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression you've provided calculates the difference between two measures: **TotalActualHours** and **TotalContractedHours**. 

Here's a breakdown of what this means in simple business terms:

1. **TotalActualHours**: This measure represents the total number of hours that have actually been worked or logged by employees on a project or task.

2. **TotalContractedHours**: This measure indicates the total number of hours that were agreed upon in a contract for the project or task. This is the expected or planned amount of work.

3. **TotalHoursDeltaCompleteNr**: The result of the expression `[TotalActualHours] - [TotalContractedHours]` is essentially the difference between the actual hours worked and the hours that were contracted. 

### What It Achieves:
- **Positive Result**: If the result is positive, it means that more hours were worked than were originally contracted. This could indicate that the project is taking longer than expected or that additional work was required.
  
- **Negative Result**: If the result is negative, it means that fewer hours were worked than contracted. This could suggest that the project is ahead of schedule or that less work was needed than anticipated.

- **Zero Result**: If the result is zero, it indicates that the actual hours worked match the contracted hours, suggesting that the project is on track according to the original agreement.

In summary, this DAX measure helps businesses understand how actual work hours compare to what was planned, allowing for better project management and resource allocation decisions.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `TotalHoursTracked`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The measure calculates the total number of hours tracked in a timesheet.

2. **Checking for Blank Values**: The code first checks if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

3. **Returning Values**:
   - If the sum of hours is blank (meaning no hours have been recorded), the measure returns **0**. This is important because it ensures that when there are no entries, the measure does not show a blank value, which could be misleading.
   - If there are hours recorded (the sum is not blank), it simply returns the total sum of hours tracked.

4. **Final Outcome**: The result of this measure will either be the total hours tracked (if there are any) or 0 (if there are no hours tracked). This helps users easily understand how many hours have been logged without encountering any blank values in reports or dashboards.

In summary, `TotalHoursTracked` provides a clear and user-friendly way to see the total hours worked, ensuring that users always receive a numeric value (either the total hours or zero) for better reporting and analysis.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you've provided is designed to create a measure called `TotalHoursTrackedMissing`. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The measure first checks if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return "Empty" if No Hours**: If the sum of hours is indeed blank (meaning no hours have been tracked), the measure returns the text "Empty". This indicates that there is no data available for hours tracked.

3. **Return Total Hours if Present**: If there are hours recorded (the sum is not blank), the measure simply returns the total sum of hours tracked from the `vw_Timesheet` table.

### Summary:
In essence, this measure helps to identify whether any hours have been logged in the timesheet. If no hours are logged, it clearly indicates this by returning "Empty". If hours are logged, it provides the total number of hours tracked. This is useful for reporting and analysis, as it allows users to quickly understand if there is data available or if they need to investigate further.

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

Here’s a breakdown of what each part does in simple business terms:

1. **[TotalHoursTracked]**: This part represents the total number of hours that have been logged or tracked. It could be the total hours worked by an employee or the total hours for a specific project.

2. **CALCULATE(...)**: This function modifies the context in which the data is evaluated. It allows us to perform calculations based on specific filters or conditions.

3. **MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek])**: This part finds the maximum number of hours tracked in a week from the timesheet data. 
   - **DISTINCT(vw_Timesheet[HoursPerWeek])**: This ensures that we are only looking at unique weekly hour values.
   - **MAXX(...)**: This function then takes those unique values and finds the highest one.

4. **ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])**: This part removes all filters from the `vw_Timesheet` table except for the filter on `EmployeeID`. This means that the calculation of maximum hours per week will consider all records for the specific employee, regardless of any other filters that might be applied.

### What It Achieves:
The overall measure `trackedDiff` calculates how many hours an employee has tracked beyond their maximum tracked hours in any single week. 

- If the result is positive, it indicates that the employee has tracked more hours than their highest weekly total, suggesting they are working extra hours.
- If the result is zero or negative, it indicates that the employee has not exceeded their maximum weekly hours tracked.

In summary, `trackedDiff` helps in understanding whether an employee is working more than their usual maximum hours in a week, which can be useful for workload management and ensuring employee well-being.

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
- **DAX Explanation (Generated):** The DAX code snippet you've provided is a measure named `trackedDiff2`. Let's break it down step by step to understand what it calculates in simple business terms.

### Components of the Measure:

1. **[TotalHoursTracked]**: This part of the measure represents the total number of hours that have been tracked for a specific context, such as a particular employee or time period.

2. **CALCULATE Function**: This function modifies the context in which data is evaluated. It allows us to change filters and perform calculations based on those changes.

3. **MAXX Function**: This function is used to find the maximum value from a set of values. In this case, it is applied to a distinct list of hours worked per week.

4. **DISTINCT(vw_Timesheet[HoursPerWeek])**: This part creates a unique list of hours worked per week from the `vw_Timesheet` table. It ensures that we are only considering different values of hours worked.

5. **REMOVEFILTERS(vw_Timesheet[HoursPerWeek])**: This removes any filters that might be applied to the `HoursPerWeek` column, allowing the calculation to consider all possible values of hours worked per week.

6. **ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])**: This keeps the filter context for `EmployeeID` while removing other filters from the `vw_Timesheet` table. This means the calculation will focus on the specific employee's data while ignoring other filters.

### What It Achieves:

Putting it all together, the measure `trackedDiff2` calculates the difference between:

- **Total Hours Tracked**: The total hours recorded for a specific employee or context.
- **Maximum Hours Per Week**: The maximum number of hours that have been recorded for that employee, regardless of any specific week or filter applied to the hours worked.

### In Business Terms:

This measure helps to identify how much the total hours tracked for an employee exceed the maximum hours they have worked in any given week. 

- If the result is positive, it indicates that the employee has tracked more hours than their maximum weekly workload, which could suggest overtime or additional work.
- If the result is zero or negative, it indicates that the tracked hours are within or below their maximum weekly capacity.

In summary, `trackedDiff2` provides insights into employee workload management by comparing total tracked hours against their maximum weekly hours, helping businesses monitor and manage employee productivity and workload effectively

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate the total billable hours from a timesheet data source, specifically focusing on certain criteria. Here’s a breakdown of what it does in simple business terms:

1. **Variable Definition**: The code starts by defining a variable called `FilteredHours`. This variable will hold a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**: The `Filter` function is used to create this filtered dataset. It looks at each entry in the `vw_Timesheet` and applies two main conditions:
   - **Condition 1**: The project qualifies as billable if `QualifyPrj` equals 1 (indicating it is a qualifying project) **and** `BillablePrj` equals 1 (indicating it is a billable project).
   - **Condition 2**: Alternatively, if the project name contains the phrase "Customer Success Services", it will also be included in the filtered dataset. This is checked using the `SEARCH` function, which looks for that specific text within the `Project` field.

3. **Calculating Total Hours**: After filtering the timesheet data based on the above criteria, the `RETURN` statement uses the `SUMX` function. This function takes the filtered dataset (`FilteredHours`) and sums up the `Hours` for each entry that meets the filtering conditions.

### Summary:
In summary, this DAX measure calculates the total number of billable hours from a timesheet by including only those hours that are associated with qualifying and billable projects or projects related to "Customer Success Services". This helps the business understand how many hours can be billed to clients based on specific project criteria.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `UTI_TOTALHOURS`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the total number of hours worked on specific projects that meet a certain qualification criteria.

2. **Components**:
   - **SUM(vw_Timesheet[Hours])**: This part of the code adds up all the values in the `Hours` column from the `vw_Timesheet` table. Essentially, it totals the hours recorded in timesheets.
   - **CALCULATE**: This function modifies the context in which the data is evaluated. In this case, it is used to filter the data before summing the hours.
   - **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to include only those records where the `QualifyPrj` column equals 1. This means it only considers projects that are marked as qualified.

3. **Outcome**: The measure `UTI_TOTALHOURS` will return the total hours worked on projects that are qualified (where `QualifyPrj` is 1). This is useful for reporting and analysis, as it helps the business understand how many hours are being spent on projects that meet specific criteria, allowing for better resource allocation and project management.

In summary, this DAX measure helps businesses track and analyze the total hours worked on qualified projects, providing insights into productivity and project performance.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'UTILIZATION_New', which focuses on determining the utilization ratio of active employees in a business context. Here's a breakdown of what it does in simple terms:

1. **Variables Defined**:
   - **_TotalBillableHours**: This variable captures the total number of hours that employees have billed to clients. It represents the productive time that can be charged for services rendered.
   - **_TotalHours**: This variable captures the total hours worked by employees, which includes both billable and non-billable hours.

2. **Return Logic**:
   - The measure first checks if there are any active contacts (employees) using `[Msr_ActiveContacts]`. If there are no active employees, the measure will not proceed further.
   - Next, it checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, it means there is no data to calculate the utilization ratio.
   - If both checks pass, it calculates the utilization ratio by dividing the total billable hours by the total hours worked. This ratio indicates what percentage of the employees' total working hours are billable.

3. **Handling Division**:
   - The `DIVIDE` function is used to perform the division safely. If the result of the division is blank (which can happen if `_TotalHours` is zero), it returns 0 instead of an error. This ensures that the measure always returns a valid number.

### Summary:
In summary, this DAX measure calculates the billable hour ratio for active employees, providing insights into how effectively employees are utilizing their time for billable work compared to their total working hours. A higher ratio indicates better utilization, which is often a key performance indicator in service-oriented businesses.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

The following filters are applied to the entire report:

- Filter on `DimDate`.`CurrentYearFlag` (Type: Advanced, Explanation: In simple business terms, the filter definition JSON you provided is specifying a condition that focuses on a particular piece of data related to dates.

Here's what it means:

- **`CurrentYearFlag`**: This is a field (or column) in the `DimDate` table that indicates whether a date falls within the current year. 
- **`= 1`**: This part of the filter means that we are only interested in records where the `CurrentYearFlag` is equal to 1.

So, when this filter is applied, it will **include only the data for dates that are in the current year**. Any data from previous years or future years will be **excluded**. 

In summary, this filter helps you focus on the data that is relevant to the current year, allowing for analysis or reporting that is timely and specific to the present timeframe.)

### <a name="report-pages"></a>Report Pages

#### <a name="page-drill"></a>Page: Drill

*Internal Name: `b37e6790682a16090ad7`, Ordinal: 15*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is specifying which values of the `TimesheetCode` field should be included in the data being analyzed. 

In simple business terms, the filter is saying:

- **Include only the following Timesheet Codes**: 
  - 'int'
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'V'
  - 'Z'

This means that when this filter is applied, only records (or entries) that have a `TimesheetCode` matching one of these specified values will be shown in your report or analysis. Any records with a `TimesheetCode` that is not one of these six values will be excluded from the results.

In summary, this filter narrows down the data to focus specifically on these six types of timesheet codes, allowing for a more targeted analysis of relevant entries.)
- Filter on `DimDate`.`CalendarYear` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **Target Field**: The filter is applied to the field `DimDate.CalendarYear`. This field typically contains the years in which your data is recorded, such as 2020, 2021, 2022, etc.

2. **Filter Meaning**: The phrase "(All values)" indicates that **no specific filtering is being applied** to the `CalendarYear` field. This means that all years available in the data will be included in the report or visualization.

3. **Data Inclusion**: Since the filter is set to include "All values," every year present in the `DimDate.CalendarYear` field will be shown. For example, if your dataset includes the years 2019, 2020, 2021, and 2022, all these years will be part of the analysis.

4. **No Exclusions**: There are no exclusions or restrictions on the data. This means that you will not miss any data from any year; everything is considered.

### Summary:
In summary, this filter allows you to see data from every year in your dataset without any limitations. It ensures that all years are included in your analysis, providing a complete view of the data over time.)
- Filter on `DimDate`.`MonthNumberOfYear` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **Target Field**: The filter is applied to the field `DimDate.MonthNumberOfYear`. This field represents the month of the year, typically ranging from 1 (January) to 12 (December).

2. **Filter Action**: The phrase "(All values)" indicates that **no specific filtering is being applied** to the `MonthNumberOfYear` field. This means that all months from January to December are included in the data being analyzed.

3. **Data Inclusion**: Since the filter is set to include "All values," every month will be considered in any calculations, visualizations, or reports. There are no exclusions; therefore, you will see data for every month of the year.

### Summary:
In summary, this filter allows you to see data for all months of the year without any restrictions. It ensures that your analysis includes every month, providing a complete view of the data across the entire year.)
- Filter on `DimDate`.`WeekNumberOfYear` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **Target Field**: The filter is applied to the field `DimDate`.`WeekNumberOfYear`. This field typically represents the week number of the year (e.g., Week 1, Week 2, etc.).

2. **Filter Action**: The phrase "(All values)" indicates that **no specific filtering is being applied** to the `WeekNumberOfYear` field. This means that all week numbers from the dataset will be included in the analysis.

3. **Data Inclusion**: Since the filter is set to include "(All values)", every week number available in the `DimDate` table will be considered. For example, if your dataset includes weeks 1 through 52, all of these weeks will be shown in your reports.

4. **No Exclusions**: There are no exclusions or restrictions on the data. This means that you will not miss any weeks, and the analysis will reflect the complete range of week numbers.

### Summary:
In summary, this filter allows you to see data for every week of the year without any limitations. It ensures that all week numbers are included in your reports, providing a comprehensive view of the data across all weeks.)
- Filter on `lkp_Unit`.`Unit` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** from the specified field, which in this case is `lkp_Unit`.`Unit`. This means that no specific criteria are being applied to limit the data.

2. **Data Included**: When this filter is applied, every single unit from the `Unit` field in the `lkp_Unit` table will be included in the report. There are no exclusions, so all units will be visible.

3. **Data Excluded**: Since the filter is set to include all values, there are no exclusions. Every unit is considered, and none are filtered out.

### Summary:
In simple terms, this filter allows you to see all units available in the dataset without any restrictions. It ensures that every piece of data related to units is included in your analysis, providing a complete view of the information.)
- Filter on `vw_Timesheet`.`Approved` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **Target Field**: The filter is applied to the field `vw_Timesheet`.`Approved`. This field likely indicates whether timesheets have been approved or not.

2. **Filter Meaning**: The expression `"(All values)"` means that **no filtering is being applied** to the `Approved` field. In other words, all records, regardless of whether they are approved or not, will be included in the analysis.

3. **Data Inclusion**: Since the filter includes "All values," it means:
   - Timesheets that are marked as **Approved** will be included.
   - Timesheets that are marked as **Not Approved** will also be included.
   - Any other statuses (if applicable) will also be included.

### Summary:
This filter allows you to see the complete picture of all timesheet records, without excluding any based on their approval status. It ensures that your analysis considers every entry in the `vw_Timesheet` dataset, providing a comprehensive view of the data.)
- Filter on `vw_Timesheet`.`BillableDep` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the field `BillableDep` from the `vw_Timesheet` view. This means that no specific criteria are applied to limit the data.

2. **Data Included**: When this filter is applied, every possible entry in the `BillableDep` field will be shown. For example, if `BillableDep` includes departments like "Sales," "Marketing," and "Development," all these departments will be included in the report.

3. **Data Excluded**: Since the filter is set to include all values, there are no exclusions. No data related to `BillableDep` will be left out; everything is considered.

### Summary:
In simple terms, this filter ensures that all departments related to billable timesheets are visible in your report, allowing you to see the complete picture without any restrictions.)
- Filter on `vw_Timesheet`.`BillablePrj` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** from the `BillablePrj` field in the `vw_Timesheet` view. This means that no specific criteria are applied to limit the data.

2. **Data Included**: When this filter is applied, every single entry related to `BillablePrj` will be shown in your report. This includes all projects that are billable, regardless of any other conditions or attributes.

3. **Data Excluded**: Since the filter is set to include all values, there are no exclusions. Every project that falls under the `BillablePrj` category will be visible.

### Summary:
In simple terms, this filter allows you to see everything related to billable projects without any restrictions. You will not miss any data, as it captures all entries in that category.)
- Filter on `vw_Timesheet`.`EmployeeName` (Type: Categorical, Explanation: The provided Power BI filter definition JSON specifies a condition that focuses on the `EmployeeName` field from the `vw_Timesheet` data view. Here’s a breakdown of what this filter does in simple business terms:

- **Target Field**: The filter is applied to the `EmployeeName` column in the `vw_Timesheet` view, which contains the names of employees.

- **Inclusion Criteria**: The filter states that only records where the `EmployeeName` is exactly 'Sujeev Bhagyanath' should be included in the analysis or report.

- **Exclusion Criteria**: All other employee names will be excluded from the data being analyzed. This means that if an employee's name is anything other than 'Sujeev Bhagyanath', their records will not appear in the results.

In summary, this filter is used to focus exclusively on the data related to the employee named 'Sujeev Bhagyanath', ignoring all other employees in the dataset.)
- Filter on `vw_Timesheet`.`Hours` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the target field, which in this case is `vw_Timesheet`.`Hours`. This means that no specific criteria are applied to limit the data being shown.

2. **Data Included**: When this filter is applied, every single entry in the `Hours` column from the `vw_Timesheet` view will be included in the analysis. There are no restrictions, so all hours logged by employees will be considered.

3. **Data Excluded**: Since the filter includes all values, there are no exclusions. Every record related to hours worked is available for reporting and analysis.

### Summary:
In simple terms, this filter allows you to see **everything** related to hours worked without omitting any data. It’s like saying, “Show me all the hours recorded, without any filters or limits.”)
- Filter on `vw_Timesheet`.`HoursperWeek` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the field `vw_Timesheet`.`HoursperWeek`. This means that when you apply this filter, you are not excluding any data related to the hours worked per week.

2. **Data Included**: Every record in the `vw_Timesheet` table that has a value for `HoursperWeek` will be included in your analysis. This could range from employees who worked 0 hours to those who worked the maximum hours recorded.

3. **Data Excluded**: Since the filter is set to include all values, there are no exclusions. No specific hours per week are filtered out, so you will see a complete picture of all the hours worked by employees.

### Summary:
In summary, applying this filter means you will see all the data related to `HoursperWeek` without any restrictions. This is useful when you want to analyze the total hours worked by all employees without omitting any specific groups or values.)
- Filter on `vw_Timesheet`.`ManagerName` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the field `vw_Timesheet`.`ManagerName`. This means that no specific manager is being filtered in or out.

2. **Data Inclusion**: When this filter is applied, every manager's data from the `vw_Timesheet` view will be included in the report. There are no restrictions, so you will see timesheet entries for all managers.

3. **Data Exclusion**: Since the filter is set to "(All values)", there are no exclusions. No manager's data is being left out; every manager's information is available for analysis.

### Summary:
In simple terms, this filter allows you to see the complete picture of timesheet data for every manager without any limitations. You can analyze the performance, hours worked, or any other metrics related to all managers in the dataset.)
- Filter on `vw_Timesheet`.`QualifyPrj` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and dashboards. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** from the specified field, which in this case is `vw_Timesheet`.`QualifyPrj`. 

2. **Data Inclusion**: This means that when you apply this filter, you are not excluding any projects or entries from the `QualifyPrj` field. Every single project that exists in the `vw_Timesheet` view will be considered in your analysis.

3. **No Restrictions**: Since the filter is set to "(All values)", there are no restrictions or conditions applied. You will see all the data related to projects, regardless of their characteristics or statuses.

### Summary:
In simple terms, this filter allows you to see everything related to projects in your timesheet data without omitting any information. It’s like saying, “Show me all the projects, no matter what.”)
- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all possible values** for the field `vw_Timesheet`.`TimesheetCode`. This means that no specific criteria are applied to limit the data shown based on the `TimesheetCode`.

2. **Data Inclusion**: Since the filter is set to "(All values)", every entry in the `TimesheetCode` field will be included in the report or visualization. This means that all timesheet codes, regardless of their specific values, will be displayed.

3. **No Exclusions**: There are no exclusions or restrictions applied. This filter does not filter out any timesheet codes, so you will see every code that exists in the dataset.

### Summary:
When this filter is applied, you will see all timesheet codes in your report, allowing for a comprehensive view of all the data related to timesheets without any limitations.)
- Filter on `vw_Timesheet`.`Unit` (Type: Categorical, Explanation: The provided Power BI filter definition is specifying a condition for the data that will be included in a report or visualization. Let's break it down in simple business terms:

- **Target Data**: The filter is applied to a specific field called `Unit` within a data source named `vw_Timesheet`. This field likely represents different departments, teams, or business units within an organization.

- **Filter Condition**: The filter condition is defined as "`Unit` IN ('Empl M-Cloud - Data & Analytics')". This means that only the records where the `Unit` is exactly "Empl M-Cloud - Data & Analytics" will be included in the analysis.

- **Inclusion and Exclusion**:
  - **Included**: Any data entries (or rows) in the `vw_Timesheet` where the `Unit` is "Empl M-Cloud - Data & Analytics" will be shown in the report.
  - **Excluded**: All other entries where the `Unit` is anything other than "Empl M-Cloud - Data & Analytics" will be filtered out and not displayed.

In summary, this filter is used to focus the analysis specifically on the "Empl M-Cloud - Data & Analytics" unit, allowing users to see only the relevant data for that particular team or department while ignoring all other units.)
- Filter on `vw_Timesheet`.`QualifyPrj` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** from the specified field, which in this case is `vw_Timesheet`.`QualifyPrj`. 

2. **Data Inclusion**: This means that when you apply this filter, you are not excluding any projects or entries from the `QualifyPrj` field. Every project that exists in the `vw_Timesheet` view will be included in your analysis or report.

3. **No Restrictions**: Since the filter is set to "(All values)", there are no restrictions or conditions applied. You will see all the data related to projects, regardless of their characteristics or statuses.

### Summary:
In simple terms, this filter allows you to see everything related to projects in your timesheet data without omitting any information. It ensures a complete view of all projects, making it useful when you want to analyze or report on the entire dataset without any limitations.)
- Filter on `vw_Timesheet`.`Rejected` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **Target Data**: The filter is applied to a specific dataset or table, which in this case is `vw_Timesheet`.`Rejected`. This likely refers to a view or table that contains records of timesheets that have been rejected.

2. **Filter Meaning**: The phrase "(All values)" indicates that no specific filtering criteria are being applied. This means that all records from the `vw_Timesheet`.`Rejected` dataset will be included in the report or visualization.

3. **Inclusion of Data**: Since the filter is set to "(All values)", every rejected timesheet record will be shown. There are no exclusions or limitations on the data being displayed.

### Summary:
When this filter is applied, you will see all the rejected timesheet records without any restrictions. This is useful when you want to analyze or review every instance of rejected timesheets without missing any data.)
- Filter on `vw_Timesheet`.`ReportReady` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** from the data source. This means that no specific criteria are applied to limit the data being shown.

2. **Data Inclusion**: When this filter is applied to the target `vw_Timesheet`.`ReportReady`, it means that every single entry in the `ReportReady` dataset will be included in the report. There are no restrictions or exclusions based on any conditions.

3. **Practical Impact**: As a result, users will see a complete view of all timesheet records available in the `vw_Timesheet` view. This is useful when you want to analyze the entire dataset without filtering out any specific records.

### Summary:
In summary, the filter "(All values)" ensures that all data from the `vw_Timesheet`.`ReportReady` is included in the report, allowing for a comprehensive analysis without any exclusions.)
- Filter on `vw_Timesheet`.`Project` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and dashboards. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** from the `vw_Timesheet`.`Project` data. This means that no specific projects are being excluded or included; rather, every project in the dataset will be shown.

2. **Data Inclusion**: When this filter is applied, it ensures that all projects from the `vw_Timesheet` view are available for analysis. You will see data related to every project without any restrictions.

3. **No Exclusions**: Since the filter is set to "(All values)", there are no projects being filtered out. This is useful when you want a comprehensive view of all projects, rather than focusing on a specific subset.

### Summary:
In summary, this filter allows you to see all projects in the `vw_Timesheet` view without any limitations, providing a complete picture of the data related to all projects.)

##### Visuals on this Page

**actionButton**

- Type: `actionButton`
- Name: `27e6b6fd18af776e6acc`
- Fields Used: _(None detected)_

**tableEx**

- Type: `tableEx`
- Name: `a2c6ac67ed4bc6ae0822`
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

#### <a name="page-out-of-service"></a>Page: Out of Service

*Internal Name: `bcfb1ca820fd416f559b`, Ordinal: 6*

##### Page Level Filters

- Filter on `vw_Timesheet`.`ContractStatusTodayPBI` (Type: Categorical, Explanation: In simple business terms, this Power BI filter is designed to focus on a specific subset of data from the `vw_Timesheet` table, specifically looking at the `ContractStatusTodayPBI` field.

Here's what the filter does:

- **Target Field**: The filter is applied to the `ContractStatusTodayPBI` column in the `vw_Timesheet` table.
- **Included Data**: The filter specifies that only records where the `ContractStatusTodayPBI` value is **exactly** 'Not Valid' should be included in the analysis or report.
- **Excluded Data**: Any records where the `ContractStatusTodayPBI` value is anything other than 'Not Valid' will be excluded from the results.

In summary, when this filter is applied, you will only see timesheet records that have a contract status of 'Not Valid', allowing you to focus on this specific condition in your analysis.)
- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Advanced, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition you've provided is a bit abstract, but let's break it down in simple terms.

### Understanding the Filter Definition

The filter definition is:
```json
"NOT ((Unparsed condition))"
```

1. **NOT**: This indicates that we are excluding certain data based on a condition. In other words, we want to see everything **except** what meets the criteria defined in the condition.

2. **(Unparsed condition)**: This part is not specified, but it typically represents a logical condition that would determine which records to exclude. For example, it could be something like "TimesheetCode is equal to 'ABC'" or "TimesheetCode is in a specific list."

### What This Means for `vw_Timesheet`.`TimesheetCode`

When this filter is applied to the `vw_Timesheet`.`TimesheetCode`, it means:

- **Inclusion of Data**: All records from the `vw_Timesheet` table will be included **except** those that meet the criteria defined in the "Unparsed condition."
  
- **Exclusion of Data**: Any records where the `TimesheetCode` matches the condition specified (once it is defined) will be excluded from the report or visualization.

### Example Scenario

If the "Unparsed condition" were defined as "TimesheetCode is equal to 'ABC'", then applying this filter would result in:

- **Included**: All timesheet records with codes other than 'ABC'.
- **Excluded**: All timesheet records with the code 'ABC'.

### Summary

In summary, this filter is designed to show all timesheet records except those that meet a specific condition related to the `TimesheetCode`. The exact records excluded depend on the details of the "Unparsed condition," which would need to be defined to understand the full impact of the filter.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `8591963a5bac18e76937`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `24fcf76f7c95dc711120`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `3e8e556164590008022c`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `1db16de7c4039b717334`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Unit**

- Type: `slicer`
- Name: `3b3c37c93822b3c82515`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `c2b4045697ab07c830e5`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `eb4c2eb02eca05bda3d6`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**image**

- Type: `image`
- Name: `3d57a50b88b63486e770`
- Fields Used: _(None detected)_

**Current Week**

- Type: `card`
- Name: `25d601e290764a0b3868`
- Fields Used:
  - `CurrentWeekCard` (Query: `PBI_TIMESHEET.CurrentWeekCard`) (Role: Values)

**pivotTable**

- Type: `pivotTable`
- Name: `c4cfe004b815c750b069`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `YearWeek` (Query: `PBI_TIMESHEET.YearWeek`) (Role: Rows)
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Columns)
  - `Sum of Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is saying that we want to include all records where the `EmployeeName` field is not empty or missing. 

Here's a breakdown of what this means:

- **`EmployeeName` = null**: This part checks if the `EmployeeName` is empty or has no value (null).
- **NOT**: This means we want to exclude those records where `EmployeeName` is null.

So, when this filter is applied to the target `vw_Timesheet`.`EmployeeName`, it will only show data for employees who have a name listed. Any records where the employee's name is missing will be filtered out and not displayed in the report. Essentially, you will only see timesheet entries for employees who have a valid name.)

**barChart**

- Type: `barChart`
- Name: `f45ec945d5670592a757`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Category)
  - `Count of EmployeeName` (Query: `Count(PBI_TIMESHEET.EmployeeName)`) (Role: Y)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `625a3d330a77441002d6`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is designed to exclude any records where the field `OWN-Sub-ExtT` is empty or has no value (i.e., is null).

Here's a breakdown of what this means:

- **Target Field**: The filter is applied to the field `OWN-Sub-ExtT` in the `lkp_Unit` table.
- **Condition**: The filter specifies "NOT (`OWN-Sub-ExtT` = null)", which means it is looking for records where `OWN-Sub-ExtT` does **not** equal null.
- **Inclusion/Exclusion**: As a result, this filter will include only those records where `OWN-Sub-ExtT` has a valid value (i.e., it is not empty). Any records where `OWN-Sub-ExtT` is null will be excluded from the data being analyzed or displayed.

In summary, this filter ensures that only records with meaningful data in the `OWN-Sub-ExtT` field are considered, helping to focus the analysis on relevant information.)

**Billable Dep**

- Type: `slicer`
- Name: `ae83fdd3a580586b6888`
- Fields Used:
  - `BillableDep` (Query: `PBI_TIMESHEET.BillableDep`) (Role: Values)

**clusteredColumnChart**

- Type: `clusteredColumnChart`
- Name: `43b27f3f4d2018a91440`
- Fields Used:
  - `JobTitle` (Query: `PBI_TIMESHEET.JobTitle`) (Role: Category)
  - `Count of EmployeeName` (Query: `Count(PBI_TIMESHEET.EmployeeName)`) (Role: Y)

**actionButton**

- Type: `actionButton`
- Name: `bbccc79f835e3860e689`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `b324607b040115510097`
- Fields Used: _(None detected)_

**35817d0444429b50cdd0**

- Type: `None`
- Name: `35817d0444429b50cdd0`
- Fields Used: _(None detected)_

#### <a name="page-page-navigation"></a>Page: Page Navigation

*Internal Name: `88b699ece9b230678625`, Ordinal: 0*

##### Visuals on this Page

**image**

- Type: `image`
- Name: `de9b9511c3cf57754405`
- Fields Used: _(None detected)_

**actionButton**

- Type: `actionButton`
- Name: `a44026d22203dd4b5140`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `b504171bb603a1f77bd5`
- Fields Used: _(None detected)_

**pageNavigator**

- Type: `pageNavigator`
- Name: `01e97403abec6221fac1`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `27c6082c1f9cb2b089e3`
- Fields Used: _(None detected)_

**pageNavigator**

- Type: `pageNavigator`
- Name: `df5f5aecf331beb7c015`
- Fields Used: _(None detected)_

**actionButton**

- Type: `actionButton`
- Name: `364c0777f0bcf196c2c4`
- Fields Used: _(None detected)_

**actionButton**

- Type: `actionButton`
- Name: `d02efed79230a598bb07`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `d3cee65d8dbd72357272`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `289940676b3d4f8baf19`
- Fields Used: _(None detected)_

#### <a name="page-timesheet-details"></a>Page: Timesheet (Details)

*Internal Name: `2823d445ab5cb970c02c`, Ordinal: 14*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is specifying which values of the `TimesheetCode` field should be included in the data being analyzed. 

In simple business terms, the filter is saying:

- **Include only the records** where the `TimesheetCode` is one of the following: 
  - 'int'
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'V'
  - 'Z'

This means that any records with a `TimesheetCode` that is **not** one of these specified values will be **excluded** from the analysis. 

So, if you apply this filter, you will only see data related to those specific `TimesheetCode` values, helping you focus on a particular subset of timesheet entries that are relevant for your analysis or reporting.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `6df0ad0de0ba662015a3`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `af5b290f10977ded7047`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `15b6f6d19a20881eed70`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `34b4ecf0586c18b98d59`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Unit**

- Type: `slicer`
- Name: `3aa8565ed740c256818a`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `4e510e709725a9396c23`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `94a6fb8f30daab5fda00`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**Project**

- Type: `slicer`
- Name: `59c925de11b45f268a85`
- Fields Used:
  - `Project` (Query: `PBI_TIMESHEET.Project`) (Role: Values)

**image**

- Type: `image`
- Name: `dddb22859aa1d99bc30a`
- Fields Used: _(None detected)_

**tableEx**

- Type: `tableEx`
- Name: `5fc36ece4b29bb927879`
- Fields Used:
  - `TimesheetDate` (Query: `PBI_TIMESHEET.TimesheetDate`) (Role: Values)
  - `SubUnit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)
  - `Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Values)
  - `HoursperWeek` (Query: `Sum(PBI_TIMESHEET.HoursperWeek)`) (Role: Values)
  - `Approved` (Query: `PBI_TIMESHEET.Approved`) (Role: Values)
  - `ReportReady` (Query: `PBI_TIMESHEET.ReportReady`) (Role: Values)
  - `Rejected` (Query: `PBI_TIMESHEET.Rejected`) (Role: Values)
  - `TimesheetCode` (Query: `PBI_TIMESHEET.TimesheetCode`) (Role: Values)
  - `Project` (Query: `PBI_TIMESHEET.Project`) (Role: Values)
  - `Week` (Query: `DimDate.WeekNumberOfYear`) (Role: Values)
  - `Year` (Query: `DimDate.CalendarYear`) (Role: Values)
  - `Month` (Query: `DimDate.MonthNumberOfYear`) (Role: Values)
  - `BillableDep` (Query: `Sum(PBI_TIMESHEET.BillableDep)`) (Role: Values)
  - `BillablePrj` (Query: `Sum(PBI_TIMESHEET.BillablePrj)`) (Role: Values)
  - `QualifyPrj` (Query: `Sum(PBI_TIMESHEET.QualifyPrj)`) (Role: Values)
  - `Index` (Query: `Sum(PBI_TIMESHEET.Index)`) (Role: Values)
  - `Unit` (Query: `lkp_Unit.Unit`) (Role: Values)
  - `ManagerName` (Query: `vw_Timesheet.ManagerName`) (Role: Values)
  - `ExtraDetails` (Query: `vw_Timesheet.ExtraDetails`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is designed to exclude any records where the `EmployeeName` field is empty or not provided. 

Here's a breakdown of what this means:

- **Target Field**: The filter is applied to the `EmployeeName` field in the `vw_Timesheet` data view.
- **Condition**: The filter states "NOT (`EmployeeName` = null)", which means it is looking for records where `EmployeeName` is **not** null (or empty).
- **Effect**: When this filter is applied, only those records where there is a valid employee name will be included in the analysis. Any records that do not have an employee name (i.e., where `EmployeeName` is null) will be excluded.

In summary, this filter ensures that only timesheet entries with actual employee names are considered, helping to maintain the quality and relevance of the data being analyzed.)

**Hours**

- Type: `card`
- Name: `a06b439d7365397be637`
- Fields Used:
  - `Sum of Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Values)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `2483a4659832c6555006`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition for the data that will be included in the analysis of the target field `lkp_Unit`.`OWN-Sub-ExtT`.

Here's what it means:

- **`NOT (`OWN-Sub-ExtT` = null)`**: This part of the filter is saying that we want to **exclude any records** where the value of `OWN-Sub-ExtT` is **null** (which means it has no value or is empty).

So, when this filter is applied, it will only include records where `OWN-Sub-ExtT` has a valid value (i.e., it is not empty). In other words, you will only see data entries that have meaningful information in the `OWN-Sub-ExtT` field, and any entries that do not have a value in that field will be left out of the analysis. 

This is useful for ensuring that your analysis focuses on complete and relevant data, avoiding any gaps that could skew results or insights.)

**Billable Employee**

- Type: `slicer`
- Name: `c30cd8806ddc212580c6`
- Fields Used:
  - `BillableDep` (Query: `PBI_TIMESHEET.BillableDep`) (Role: Values)

**Billable Project**

- Type: `slicer`
- Name: `39e6b5f0eb8b6b01a769`
- Fields Used:
  - `BillablePrj` (Query: `PBI_TIMESHEET.BillablePrj`) (Role: Values)

**actionButton**

- Type: `actionButton`
- Name: `3154553f0a639aa0ecd0`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `9af21176ce50e795a076`
- Fields Used: _(None detected)_

**a744be00de6dda219c3c**

- Type: `None`
- Name: `a744be00de6dda219c3c`
- Fields Used: _(None detected)_

#### <a name="page-timesheet-missing"></a>Page: Timesheet (Missing)

*Internal Name: `c7a70300497c7d819345`, Ordinal: 2*

##### Page Level Filters

- Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: In simple business terms, this Power BI filter is designed to focus on a specific subset of data related to the `ContractStatusToday` field in the `vw_missing_timesheet` view.

### What the Filter Does:
- **Includes Only Valid Contracts**: The filter specifies that only records where the `ContractStatusToday` is marked as 'Valid' should be included in the analysis or report. 

### What the Filter Excludes:
- **Excludes All Other Statuses**: Any records where the `ContractStatusToday` is not 'Valid' (such as 'Expired', 'Pending', or any other status) will be excluded from the data being analyzed.

### Summary:
When this filter is applied, you will only see data related to contracts that are currently considered valid, allowing you to focus on the most relevant and actionable information regarding contract statuses.)
- Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition is designed to refine the data being analyzed by applying specific conditions to the field `lkp_Unit`.`OWN-Sub-ExtT`. Let's break it down in simple business terms:

1. **Exclusion of Null Values**: The first part of the filter, `NOT (`OWN-Sub-ExtT` = null)`, means that any records where the `OWN-Sub-ExtT` field has no value (i.e., is null) will be excluded from the analysis. In other words, we only want to include records that have a valid entry in this field.

2. **Exclusion of 'SUB' Values**: The second part of the filter, `NOT (`OWN-Sub-ExtT` = 'SUB')`, indicates that any records where the `OWN-Sub-ExtT` field is equal to 'SUB' will also be excluded. This means we are filtering out any records that specifically have the value 'SUB' in this field.

### Summary:
When this filter is applied to `lkp_Unit`.`OWN-Sub-ExtT`, it will include only those records that:
- Have a valid value in the `OWN-Sub-ExtT` field (not null).
- Do not have the value 'SUB'.

In practical terms, this filter helps focus the analysis on relevant data by removing any entries that are either missing important information or are categorized as 'SUB'.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `01a1248764780a5b0d37`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `812ae7c1b3e52c1be9d1`
- Fields Used: _(None detected)_

**TimeSpan**

- Type: `slicer`
- Name: `20cf868db1316e429562`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Unit**

- Type: `slicer`
- Name: `62412e98bb69b50e0e6a`
- Fields Used:
  - `MAIN_UNIT` (Query: `vw_missing_timesheet.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `60e8af4a4251ebba64d2`
- Fields Used:
  - `SubUnit` (Query: `vw_missing_timesheet.SubUnit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `9907229c588d83306d65`
- Fields Used:
  - `EmployeeName` (Query: `vw_missing_timesheet.EmployeeName`) (Role: Values)

**Billable Dep**

- Type: `slicer`
- Name: `722b38dd0b2eee0eba6a`
- Fields Used:
  - `BillableDep` (Query: `vw_missing_timesheet.BillableDep`) (Role: Values)

**Year**

- Type: `slicer`
- Name: `c3793ae032d0384ac931`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**image**

- Type: `image`
- Name: `8e19f212bed9ab924ec7`
- Fields Used: _(None detected)_

**Current Week**

- Type: `card`
- Name: `bda1276c6560ed368bb6`
- Fields Used:
  - `CurrentWeekCard` (Query: `PBI_TIMESHEET.CurrentWeekCard`) (Role: Values)

**pivotTable**

- Type: `pivotTable`
- Name: `002e4384b75677a2a39e`
- Fields Used:
  - `Employee Name` (Query: `vw_missing_timesheet.EmployeeName`) (Role: Rows)
  - `WeekNr` (Query: `DimDate.WeekNumberOfYear`) (Role: Columns)
  - `Year` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `SubUnit` (Query: `vw_missing_timesheet.SubUnit`) (Role: Rows)
  - `Unit` (Query: `vw_missing_timesheet.MAIN_UNIT`) (Role: Rows)
  - `Dax_MissingHours` (Query: `vw_missing_timesheet.Dax_MissingHours`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_missing_timesheet`.`EmployeeName` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is designed to exclude any records where the `EmployeeName` field is empty or not provided. 

Here's a breakdown of what this means:

- **Target Field**: The filter is applied to the `EmployeeName` field in the `vw_missing_timesheet` view.
- **Condition**: The filter specifies `NOT (`EmployeeName` = null)`. This means that the filter is looking for records where `EmployeeName` is **not** null (or empty).
- **Result**: When this filter is applied, only those records where there is a valid employee name will be included in the data. Any records that do not have an employee name (i.e., where `EmployeeName` is null) will be excluded from the results.

In summary, this filter ensures that the analysis only considers employees who have a name listed, thereby filtering out any entries that lack this information.)

**funnel**

- Type: `funnel`
- Name: `e8345b5bcec9a09b42a4`
- Fields Used:
  - `Year_` (Query: `vw_missing_timesheet.Year_`) (Role: Category)
  - `Week_` (Query: `vw_missing_timesheet.Week_`) (Role: Category)
  - `Count` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Y)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `6f6c0b37150048d19029`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: The provided Power BI filter definition is a set of conditions that determine which data will be included or excluded when analyzing the `lkp_Unit`.`OWN-Sub-ExtT` field. Let's break it down in simple business terms:

1. **`OWN-Sub-ExtT` Field**: This is the specific data column we are focusing on. It likely contains values that categorize or describe units in some way.

2. **Conditions Explained**:
   - **`NOT (`OWN-Sub-ExtT` = null)`**: This part of the filter means that we want to include only those records where the `OWN-Sub-ExtT` field has a value. In other words, we are excluding any records where this field is empty or has no value (null).
   - **`AND`**: This indicates that both conditions must be true for a record to be included in the results.
   - **`NOT (`OWN-Sub-ExtT` = 'SUB')`**: This condition specifies that we want to exclude any records where the `OWN-Sub-ExtT` field has the value 'SUB'. So, if a record has 'SUB' in this field, it will not be included in the results.

3. **Overall Effect**: When this filter is applied, it will include only those records from the `lkp_Unit`.`OWN-Sub-ExtT` field that:
   - Have a value (not null).
   - Do not have the value 'SUB'.

In summary, this filter is designed to focus on valid entries in the `OWN-Sub-ExtT` field while specifically excluding any entries that are either empty or labeled as 'SUB'. This helps in analyzing only the relevant data that meets these criteria.)

**Completeness %**

- Type: `card`
- Name: `8ae0b81d1d01397c4b9e`
- Fields Used:
  - `MISSING%` (Query: `vw_missing_timesheet.MISSING%`) (Role: Values)

**lineStackedColumnComboChart**

- Type: `lineStackedColumnComboChart`
- Name: `1c92d295067cb8567006`
- Fields Used:
  - `Dax_EmpCount_MissingTS` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Y)
  - `Unit` (Query: `ONEDRIVE_UNIT.Unit`) (Role: Series)
  - `Year_` (Query: `vw_missing_timesheet.Year_`) (Role: Category)
  - `Week_` (Query: `vw_missing_timesheet.Week_`) (Role: Category)

**actionButton**

- Type: `actionButton`
- Name: `999eb49e91c740b112ac`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `bd83a561a366a48aa414`
- Fields Used: _(None detected)_

**84cc12bb941a76de0d48**

- Type: `None`
- Name: `84cc12bb941a76de0d48`
- Fields Used: _(None detected)_

#### <a name="page-timesheet-not-approved"></a>Page: Timesheet (Not Approved)

*Internal Name: `09e41f413ff67a0916b1`, Ordinal: 3*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: The provided Power BI filter definition is specifying which values of the `TimesheetCode` field should be included in the data being analyzed. 

In simple business terms, this filter is saying:

- **Include Only Specific Codes**: The filter will only allow data entries where the `TimesheetCode` matches one of the following values: 'int', 'ROEG', 'ROIG', 'RSEG', 'V', or 'Z'.
  
- **Exclude Everything Else**: Any entries with a `TimesheetCode` that is not one of these specified values will be excluded from the analysis.

So, when this filter is applied, you will only see data related to those specific timesheet codes, helping you focus on particular categories of timesheets that are relevant for your analysis or reporting.)
- Filter on `vw_Timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: In simple business terms, this Power BI filter is designed to include only those records from the `vw_Timesheet` data view where the `ContractStatusToday` field has a value of 'Valid'. 

Here's a breakdown of what this means:

- **Target Data**: The filter is applied to the `ContractStatusToday` column in the `vw_Timesheet` view.
- **Included Data**: Only the entries (or rows) where `ContractStatusToday` is exactly 'Valid' will be shown in your report or analysis.
- **Excluded Data**: Any entries where `ContractStatusToday` has a different value (like 'Expired', 'Pending', or any other status) will be excluded from the results.

In summary, this filter helps you focus on only the contracts that are currently considered valid, allowing for more targeted analysis or reporting on those specific records.)
- Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Categorical, Explanation: In simple business terms, this Power BI filter is designed to focus on a specific subset of data from the `vw_Timesheet` table, particularly the `Approved_With_V_Z` column.

Here's what the filter does:

- **Target Column**: The filter is applied to the `Approved_With_V_Z` column in the `vw_Timesheet` view.
- **Included Data**: The filter specifies that only records where the `Approved_With_V_Z` value is exactly '0L' should be included in the analysis or report.
- **Excluded Data**: Any records where the `Approved_With_V_Z` value is anything other than '0L' will be excluded from the results.

In summary, this filter narrows down the data to only those timesheets that have an `Approved_With_V_Z` status of '0L', allowing users to focus on this specific category of timesheets while ignoring all others.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `0d6cb6903c109d087102`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `bfde1ca3f53aa53921a7`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `9f2f81ffba6e22020597`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `b87ace2e075869c45dc4`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Unit**

- Type: `slicer`
- Name: `73f0f40a51626f178e66`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `19a0a7fcab8d1a777925`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `f53075c5541ba7c24ab2`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**Billable Dep**

- Type: `slicer`
- Name: `ccfda715779c4b3910dd`
- Fields Used:
  - `BillableDep` (Query: `PBI_TIMESHEET.BillableDep`) (Role: Values)

**image**

- Type: `image`
- Name: `55201ded5da57e006c19`
- Fields Used: _(None detected)_

**Current Week**

- Type: `card`
- Name: `bcef45f5131d7221a95a`
- Fields Used:
  - `CurrentWeekCard` (Query: `PBI_TIMESHEET.CurrentWeekCard`) (Role: Values)

**pivotTable**

- Type: `pivotTable`
- Name: `10a11519360ea108f0fe`
- Fields Used:
  - `Employee Name` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Rows)
  - `Year` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `Week` (Query: `DimDate.WeekNumberOfYear`) (Role: Columns)
  - `Sum of Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Values)

**funnel**

- Type: `funnel`
- Name: `7b29e231048c44140c85`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Dax_EmpCount_Approved` (Query: `PBI_TIMESHEET.Dax_EmpCount_Approved`) (Role: Y)
  - `Dax_EmpCount_MissingTS` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Unknown)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `d4a3360423c7a99c26ad`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: The provided Power BI filter definition JSON is used to control which data is included or excluded when analyzing the `OWN-Sub-ExtT` field from the `lkp_Unit` table. Let's break it down in simple business terms:

1. **Field in Focus**: The filter is applied to the `OWN-Sub-ExtT` column, which likely contains different values representing various categories or types of units.

2. **Conditions Explained**:
   - **`NOT (`OWN-Sub-ExtT` = null)`**: This part of the filter means that any records where the `OWN-Sub-ExtT` value is empty or missing (null) will be excluded from the analysis. In other words, we only want to include records that have a defined value in this field.
   
   - **`AND NOT (`OWN-Sub-ExtT` = 'SUB')`**: This condition specifies that any records where the `OWN-Sub-ExtT` value is equal to 'SUB' will also be excluded. So, if a record has 'SUB' as its value, it will not be included in the results.

3. **Overall Effect**: When this filter is applied, the data included will be all records from the `lkp_Unit` table where:
   - The `OWN-Sub-ExtT` field has a value (not null).
   - The value is not 'SUB'.

In summary, this filter ensures that only records with valid, non-'SUB' values in the `OWN-Sub-ExtT` field are considered in the analysis, effectively filtering out any records that are either missing this information or specifically categorized as 'SUB'.)

**lineStackedColumnComboChart**

- Type: `lineStackedColumnComboChart`
- Name: `6795818fad24cbc3bed1`
- Fields Used:
  - `Unit` (Query: `ONEDRIVE_UNIT.Unit`) (Role: Series)
  - `Dax_EmpCount_Approved` (Query: `PBI_TIMESHEET.Dax_EmpCount_Approved`) (Role: Y)
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Dax_EmpCount_MissingTS` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Unknown)

**actionButton**

- Type: `actionButton`
- Name: `ac05cd7e4e2e3404a317`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `8fa3ff6ecb414e6277e3`
- Fields Used: _(None detected)_

**d9a46154ba9a97ce4cec**

- Type: `None`
- Name: `d9a46154ba9a97ce4cec`
- Fields Used: _(None detected)_

#### <a name="page-timesheet-not-ready"></a>Page: Timesheet (Not Ready)

*Internal Name: `9baf1be1d83a6d5866ac`, Ordinal: 4*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: In simple business terms, this Power BI filter is used to narrow down the data displayed in a report or visualization based on specific criteria related to timesheet codes.

Here's what the filter does:

- **Target Field**: The filter is applied to the field called `TimesheetCode` in the `vw_Timesheet` data view.
  
- **Included Values**: The filter specifies that only timesheet codes that match certain values will be included in the report. The specific codes that are allowed are:
  - 'int'
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'V'
  - 'Z'

- **Excluded Values**: Any timesheet codes that do not match one of these specified values will be excluded from the report. This means that if a timesheet code is anything other than 'int', 'ROEG', 'ROIG', 'RSEG', 'V', or 'Z', it will not be shown in the results.

In summary, this filter ensures that only timesheet entries with the specified codes are displayed, allowing users to focus on a specific subset of data relevant to their analysis or reporting needs.)
- Filter on `vw_Timesheet`.`RReady_With_V_Z` (Type: Categorical, Explanation: In simple business terms, this Power BI filter is specifying that we only want to include data from the `vw_Timesheet` table where the column `RReady_With_V_Z` has a value of '0L'. 

Here's a breakdown of what this means:

- **Target Table**: The filter is applied to the `vw_Timesheet` table, which likely contains timesheet data.
- **Column of Interest**: The filter focuses on the `RReady_With_V_Z` column within that table.
- **Included Value**: The filter specifies that only rows where `RReady_With_V_Z` equals '0L' should be included in the analysis or report.

**What is excluded**: Any rows where `RReady_With_V_Z` has a value other than '0L' will be excluded from the results. 

In summary, this filter narrows down the data to only those entries that meet the specific condition of having '0L' in the `RReady_With_V_Z` column, allowing for a more focused analysis on that particular subset of data.)
- Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the `vw_Timesheet` view, specifically focusing on the `Approved_With_V_Z` column.

Here's what it means:

- **Target Data**: The filter is applied to the `vw_Timesheet` view, which likely contains records of timesheets submitted by employees.
- **Column of Interest**: The filter is looking at the `Approved_With_V_Z` column. This column likely indicates whether a timesheet has been approved or not, with `1` representing an approved status.
- **Filter Condition**: The condition "`Approved_With_V_Z` = 1" means that only the records where the `Approved_With_V_Z` column has a value of `1` will be included in the results. In other words, it filters out any timesheets that have not been approved (those with a value other than `1`).

### Summary:
When this filter is applied, you will only see timesheet records that have been approved. Any timesheets that are not approved will be excluded from your analysis or report.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `2460b63edcbdb71aeac4`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `703071d1067bcaab414d`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `653af24e2654017a6ac7`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `c421e4c6d70d0cad22d9`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Unit**

- Type: `slicer`
- Name: `a8e8dff918c0a492ae11`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `cf6972a4818aa7d1dad9`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `8807d345008fdb860829`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**Billable Dep**

- Type: `slicer`
- Name: `2a6d9db23d0bb72249bc`
- Fields Used:
  - `BillableDep` (Query: `PBI_TIMESHEET.BillableDep`) (Role: Values)

**image**

- Type: `image`
- Name: `b99cc97bb3e702c20a4d`
- Fields Used: _(None detected)_

**funnel**

- Type: `funnel`
- Name: `b52f1861992222ae04ab`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Dax_EmpCount_RReady` (Query: `PBI_TIMESHEET.Dax_EmpCount_RReady`) (Role: Y)
  - `Dax_EmpCount_MissingTS` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Unknown)

**Current Week**

- Type: `card`
- Name: `7a4eb1cec898099b8293`
- Fields Used:
  - `CurrentWeekCard` (Query: `PBI_TIMESHEET.CurrentWeekCard`) (Role: Values)

**pivotTable**

- Type: `pivotTable`
- Name: `8caf6990839ca261d6c7`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Columns)
  - `Sum of Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Values)
  - `ManagerName` (Query: `PBI_TIMESHEET.ManagerName`) (Role: Rows)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `a79c02a0e817751e2611`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is designed to exclude any records from the data set where the field `OWN-Sub-ExtT` is empty or has no value (i.e., is null).

Here's a breakdown of what this means:

- **Target Field**: The filter is applied to the field `OWN-Sub-ExtT` in the `lkp_Unit` table.
- **Condition**: The filter specifies "NOT (`OWN-Sub-ExtT` = null)", which means it is looking for records where `OWN-Sub-ExtT` is **not** null.
- **Inclusion/Exclusion**: As a result, any records that have a blank or missing value in the `OWN-Sub-ExtT` field will be excluded from the analysis or report. Only records that have a valid, non-empty value in this field will be included.

In summary, this filter ensures that only relevant data with actual values in the `OWN-Sub-ExtT` field is considered, helping to provide clearer insights by removing any incomplete or irrelevant entries.)

**lineStackedColumnComboChart**

- Type: `lineStackedColumnComboChart`
- Name: `2100d0aee38d214109d2`
- Fields Used:
  - `Unit` (Query: `ONEDRIVE_UNIT.Unit`) (Role: Series)
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Dax_EmpCount_RReady` (Query: `PBI_TIMESHEET.Dax_EmpCount_RReady`) (Role: Y)
  - `Dax_EmpCount_MissingTS` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Unknown)

**actionButton**

- Type: `actionButton`
- Name: `1460a61db45e74381952`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `5b38412323b278611d77`
- Fields Used: _(None detected)_

**c0b07da99a9c7c3cc262**

- Type: `None`
- Name: `c0b07da99a9c7c3cc262`
- Fields Used: _(None detected)_

#### <a name="page-timesheet-rejected"></a>Page: Timesheet (Rejected)

*Internal Name: `38960dec36aa7a18131d`, Ordinal: 5*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is specifying which values of the `TimesheetCode` field should be included in the data being analyzed. 

In simple business terms, this filter is saying:

- **Include Only Specific Codes**: The filter will only allow data entries where the `TimesheetCode` matches one of the following values: 'int', 'ROEG', 'ROIG', 'RSEG', 'V', or 'Z'.
  
- **Exclude Everything Else**: Any entries with a `TimesheetCode` that is not one of these specified values will be excluded from the analysis.

So, when this filter is applied, you will only see data related to timesheets that have the codes 'int', 'ROEG', 'ROIG', 'RSEG', 'V', or 'Z', and all other timesheet codes will be ignored. This helps focus the analysis on specific types of timesheets that are relevant to your needs.)
- Filter on `vw_Timesheet`.`Rejected` (Type: Categorical, Explanation: In simple business terms, the provided Power BI filter definition is specifying a condition for the data that will be included in a report or visualization. 

Here's what it means:

- **Target Data**: The filter is applied to a specific field called `Rejected` in a data view named `vw_Timesheet`.
- **Condition**: The filter states that we only want to include records where the `Rejected` field has a value of 'true'.

### What This Means for Your Data:
- **Included Data**: Only the timesheet entries that have been marked as rejected will be shown in the report. This means you will see all the records where the `Rejected` status is confirmed as 'true'.
- **Excluded Data**: Any timesheet entries that are not rejected (i.e., those where `Rejected` is 'false' or possibly null) will not be included in the report.

In summary, this filter is used to focus on and analyze only the timesheet entries that have been rejected, allowing for a clearer understanding of issues or trends related to rejected timesheets.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `33d9f34a3c810bc130e2`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `9e49a1ccf545668dbed1`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `2e8a5f5214a0677aabae`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `e67259b5ca00707a749a`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Unit**

- Type: `slicer`
- Name: `d69f930404ceba21373e`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `fe2227af645fbae33222`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `2c3a64e445b431787c42`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**image**

- Type: `image`
- Name: `226a3da90b98a781bde1`
- Fields Used: _(None detected)_

**funnel**

- Type: `funnel`
- Name: `bd647d7918a68cd057b7`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Dax_EmpCount_RReady` (Query: `PBI_TIMESHEET.Dax_EmpCount_RReady`) (Role: Y)
  - `Dax_EmpCount_MissingTS` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Unknown)

**Current Week**

- Type: `card`
- Name: `bc8247c10670836c53c3`
- Fields Used:
  - `CurrentWeekCard` (Query: `PBI_TIMESHEET.CurrentWeekCard`) (Role: Values)

**pivotTable**

- Type: `pivotTable`
- Name: `0494a83bd32c0c5986c1`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Rows)
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Columns)
  - `Sum of Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Values)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `58ea40517da51a540785`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is designed to include only those records where the field `OWN-Sub-ExtT` has a value. 

Here's a breakdown of what this means:

- **`OWN-Sub-ExtT`**: This is a specific field or column in your data that likely contains some kind of identifier or value related to units or entities.

- **`= null`**: This part of the filter checks if the value in the `OWN-Sub-ExtT` field is null, which means it is empty or does not have any data.

- **`NOT`**: This indicates that we want to exclude the records where the condition is true. In this case, we want to exclude records where `OWN-Sub-ExtT` is null.

So, when this filter is applied, it effectively means:

- **Include**: Only the records where `OWN-Sub-ExtT` has a value (i.e., it is not empty).
- **Exclude**: Any records where `OWN-Sub-ExtT` is null (i.e., it has no value).

In summary, this filter ensures that your analysis only considers records that have meaningful data in the `OWN-Sub-ExtT` field, filtering out any records that do not provide useful information.)

**lineStackedColumnComboChart**

- Type: `lineStackedColumnComboChart`
- Name: `96645752c8d2f73b9541`
- Fields Used:
  - `Unit` (Query: `ONEDRIVE_UNIT.Unit`) (Role: Series)
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Category)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Dax_EmpCount_RReady` (Query: `PBI_TIMESHEET.Dax_EmpCount_RReady`) (Role: Y)
  - `Dax_EmpCount_MissingTS` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Unknown)

**actionButton**

- Type: `actionButton`
- Name: `c804810919911d4856d6`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `ce86c62357d56e44a05c`
- Fields Used: _(None detected)_

**e724c3df43091206d900**

- Type: `None`
- Name: `e724c3df43091206d900`
- Fields Used: _(None detected)_

#### <a name="page-timesheet-completeness-overview"></a>Page: Timesheet Completeness (Overview)

*Internal Name: `19c0c6c0604da63e00a5`, Ordinal: 1*

##### Page Level Filters

- Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: The provided Power BI filter definition JSON is designed to refine the data displayed in a report or visualization by applying specific criteria to the field `lkp_Unit`.`OWN-Sub-ExtT`. Here’s a breakdown of what this filter does in simple business terms:

1. **Excludes Null Values**: The filter starts with `NOT (`OWN-Sub-ExtT` = null)`. This means that any records where the `OWN-Sub-ExtT` field has no value (i.e., is null) will be excluded from the data being analyzed. In other words, we only want to see records that have a valid entry in this field.

2. **Excludes Specific Value**: The second part of the filter is `NOT (`OWN-Sub-ExtT` = 'SUB')`. This means that any records where the `OWN-Sub-ExtT` field has the value 'SUB' will also be excluded. So, if a record has 'SUB' in this field, it will not be included in the results.

### Summary:
When this filter is applied to `lkp_Unit`.`OWN-Sub-ExtT`, it will include only those records that have a valid value in the `OWN-Sub-ExtT` field, specifically excluding any records that are either null or have the value 'SUB'. Essentially, you are focusing on records that contain meaningful data in this field, while filtering out any irrelevant or unwanted entries.)
- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: In simple business terms, this Power BI filter is used to narrow down the data that is being analyzed in the report or dashboard. Specifically, it focuses on the `TimesheetCode` field from the `vw_Timesheet` view.

Here's what the filter does:

- **Includes Specific Codes**: The filter specifies that only certain `TimesheetCode` values should be included in the analysis. These values are:
  - 'int'
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'V'
  - 'Z'

- **Excludes Other Codes**: Any `TimesheetCode` that is **not** one of these specified values will be excluded from the analysis. This means that if a timesheet has a code like 'ABC' or 'XYZ', it will not be considered in the report.

In summary, this filter ensures that only timesheets with the specified codes are included in the data being analyzed, allowing for a focused view on those particular entries.)
- Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: The provided filter definition JSON is specifying a condition for the data that will be included when analyzing the `ContractStatusToday` field from the `vw_missing_timesheet` view in Power BI.

### Explanation in Business Terms:

- **Field Being Filtered**: The filter is applied to the `ContractStatusToday` field, which likely indicates the current status of contracts in the context of missing timesheets.

- **Filter Condition**: The condition states that we only want to include records where the `ContractStatusToday` is 'Valid'.

### What This Means:

- **Included Data**: Only contracts that have a status of 'Valid' will be included in the analysis. This means that any contracts that are not valid (for example, those that might be expired, canceled, or in any other non-valid state) will be excluded from the results.

### Summary:

In summary, this filter ensures that the analysis focuses solely on contracts that are currently valid, allowing for a clearer understanding of the situation regarding missing timesheets for those contracts. Any data related to contracts that are not valid will not be considered in the analysis.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `812a45379ee7e0731409`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `d9964187c34b0cc071a4`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `464d5d984441e7d9bc9e`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `502563c9e06e44a0cdca`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `b07789f00d9e6274199e`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: The provided Power BI filter definition JSON is a logical expression that determines which data will be included or excluded when applied to the target field `lkp_Unit`.`OWN-Sub-ExtT`. Let's break it down in simple business terms:

1. **`OWN-Sub-ExtT` Field**: This is the specific data column we are focusing on. It likely contains values that categorize or describe units in some way.

2. **Filter Logic**:
   - **`NOT (`OWN-Sub-ExtT` = null)`**: This part of the filter means that we want to include only those records where the `OWN-Sub-ExtT` field has a value. In other words, we are excluding any records where this field is empty or has no data.
   - **`AND`**: This logical operator means that both conditions must be true for a record to be included.
   - **`NOT (`OWN-Sub-ExtT` = 'SUB')`**: This part specifies that we want to exclude any records where the `OWN-Sub-ExtT` field has the value 'SUB'. 

3. **Overall Effect**: When this filter is applied, it will include only those records where:
   - The `OWN-Sub-ExtT` field has a value (i.e., it is not empty).
   - The value of `OWN-Sub-ExtT` is not 'SUB'.

### Summary:
In summary, this filter is designed to include all records that have a valid value in the `OWN-Sub-ExtT` field, while specifically excluding any records that have the value 'SUB'. This helps in focusing the analysis on relevant data while filtering out unwanted entries.)

**shape**

- Type: `shape`
- Name: `c28eadc1779b8cd617ae`
- Fields Used: _(None detected)_

**Not Approved**

- Type: `card`
- Name: `9dc4e3b28032a7e040cc`
- Fields Used:
  - `Dax_EmpCount_Approved` (Query: `PBI_TIMESHEET.Dax_EmpCount_Approved`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the `vw_Timesheet` view, specifically focusing on the `Approved_With_V_Z` field.

Here's what the filter means:

- **Field Being Filtered**: The filter is looking at the `Approved_With_V_Z` column in the `vw_Timesheet` data.
- **Condition**: The condition states that the value of `Approved_With_V_Z` must be equal to `0`.

### What This Means for the Data:

- **Included Data**: Only the records (or rows) from the `vw_Timesheet` view where the `Approved_With_V_Z` value is exactly `0` will be included in the analysis or report.
- **Excluded Data**: Any records where `Approved_With_V_Z` has a value other than `0` (for example, `1`, `2`, or any other number) will be excluded from the analysis.

### Summary:

In summary, this filter is used to focus on timesheet entries that have not been approved (assuming that `0` indicates "not approved"). Therefore, when this filter is applied, you will only see the timesheets that are in this specific state, helping you analyze or report on unapproved timesheet entries.)
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: In simple business terms, this Power BI filter is specifying which values of the `TimesheetCode` field should be included in the data being analyzed. 

Here's what the filter is doing:

- It is looking at the `TimesheetCode` column in the `vw_Timesheet` data.
- The filter includes only those records where the `TimesheetCode` is one of the following three values: 'int', 'ROEG', or 'RSEG'.

This means that any records with a `TimesheetCode` that is **not** 'int', 'ROEG', or 'RSEG' will be excluded from the analysis. Essentially, the filter narrows down the data to focus only on these specific codes, allowing for a more targeted analysis of timesheets associated with these codes.)
  - Filter on `vw_Timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the field `ContractStatusToday` from the `vw_Timesheet` data source. This means that no specific criteria are being applied to limit the data.

2. **Data Included**: When this filter is applied, every possible status that exists in the `ContractStatusToday` field will be shown. For example, if the statuses include "Active," "Inactive," "Pending," etc., all of these statuses will be included in the report.

3. **Data Excluded**: Since the filter is set to include all values, there are no exclusions. No data is being filtered out; everything related to `ContractStatusToday` will be visible.

### Summary:
In simple terms, this filter allows you to see every single entry in the `ContractStatusToday` field without any restrictions. It ensures that all contract statuses are represented in your analysis, providing a complete view of the data.)

**Billable Dep**

- Type: `slicer`
- Name: `28e95ab20b7ea6211d27`
- Fields Used:
  - `BillableDep` (Query: `PBI_TIMESHEET.BillableDep`) (Role: Values)

**Not Ready**

- Type: `card`
- Name: `451b6887aca2d9da5e89`
- Fields Used:
  - `Dax_EmpCount_RReady` (Query: `PBI_TIMESHEET.Dax_EmpCount_RReady`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the `vw_Timesheet` view, specifically focusing on the `Approved_With_V_Z` column.

Here's what it means:

- **Target Data**: The filter is applied to the `vw_Timesheet` view, which likely contains records of timesheets submitted by employees.
- **Column of Interest**: The filter is looking at the `Approved_With_V_Z` column. This column likely indicates whether a timesheet has been approved based on certain criteria (the "V_Z" part might refer to specific approval conditions or categories).
- **Filter Condition**: The condition "`Approved_With_V_Z` = 1" means that only those records where the `Approved_With_V_Z` column has a value of 1 will be included in the analysis or report. 

### Summary:
When this filter is applied, it will **include only the timesheet records that have been approved** (where `Approved_With_V_Z` equals 1) and **exclude all other records** that have not been approved (where `Approved_With_V_Z` is not equal to 1). This allows users to focus solely on the approved timesheets for their analysis or reporting needs.)
  - Filter on `vw_Timesheet`.`RReady_With_V_Z` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the target table `vw_Timesheet`. 

Here's what it means:

- **Target Table**: The filter is applied to a specific dataset called `vw_Timesheet`.
- **Field Being Filtered**: The filter is focusing on a field (or column) named `RReady_With_V_Z`.
- **Condition**: The condition specified is that the value of `RReady_With_V_Z` must be equal to `0`.

### What This Filter Does:
- **Includes Data**: The filter will include only those records (or rows) from the `vw_Timesheet` where the `RReady_With_V_Z` field has a value of `0`.
- **Excludes Data**: Any records where `RReady_With_V_Z` has a value other than `0` (such as `1`, `2`, or any other number) will be excluded from the results.

### Summary:
In summary, this filter is used to narrow down the dataset to only those entries where the `RReady_With_V_Z` value is `0`, effectively filtering out all other entries. This can be useful for analyzing a specific subset of data that meets this condition.)
  - Filter on `vw_Timesheet`.`[Dax_EmpCount_RReady]` (Type: Advanced, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the specified field. In this case, it applies to the target `vw_Timesheet`.`[Dax_EmpCount_RReady]`.

2. **Data Included**: When this filter is applied, it means that every single record or entry in the `Dax_EmpCount_RReady` field will be considered. There are no restrictions or exclusions based on any criteria.

3. **Data Excluded**: Since the filter includes all values, there are no data points being excluded. Every employee count that is available in the dataset will be shown in the report.

### Summary:
In simple terms, this filter allows you to see the complete picture of employee counts without leaving anything out. It ensures that all relevant data is available for analysis, making it useful when you want to analyze the entire dataset without any limitations.)
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: The provided Power BI filter definition JSON specifies a condition for the `TimesheetCode` field in the `vw_Timesheet` view. Here's a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Inclusion of Specific Codes**: The filter is set to include only those records where the `TimesheetCode` matches one of the specified values. In this case, the valid `TimesheetCode` values are:
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'V'
  - 'Z'
  - 'int'

### What Data is Included:
- When this filter is applied, only the timesheet entries that have a `TimesheetCode` of 'ROEG', 'ROIG', 'RSEG', 'V', 'Z', or 'int' will be included in the analysis or report. 

### What Data is Excluded:
- Any timesheet entries with a `TimesheetCode` that is **not** one of the specified values will be excluded from the analysis. This means that if a timesheet has a code like 'ABC', 'XYZ', or any other code not listed, it will not appear in the results.

### Summary:
In summary, this filter is used to focus on a specific set of timesheet codes that are likely relevant for a particular analysis or report, ensuring that only those entries are considered while filtering out all others.)

**Active Employee**

- Type: `card`
- Name: `756ca04cf0b3a64ce88f`
- Fields Used:
  - `Msr_ActiveContacts` (Query: `PBI_TIMESHEET.Msr_ActiveContacts`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_missing_timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is specifying which values of the `TimesheetCode` field should be included in the data being analyzed. 

In simple terms, the filter is saying:

- **Include only the following Timesheet Codes**: 
  - 'int'
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'Z'
  - 'V'

This means that when this filter is applied, only records (or entries) that have a `TimesheetCode` matching one of these specified values will be shown in your report or analysis. 

**What is excluded?**
- Any `TimesheetCode` that is **not** one of these values will be excluded from the data. For example, codes like 'ABC', 'XYZ', or any other code that is not listed will not appear in the results.

In summary, this filter narrows down the dataset to only include specific timesheet codes, allowing you to focus on those particular entries in your analysis.)

**Current Week**

- Type: `card`
- Name: `d4162e8875d6de692b7e`
- Fields Used:
  - `CurrentWeekCard` (Query: `PBI_TIMESHEET.CurrentWeekCard`) (Role: Values)

**image**

- Type: `image`
- Name: `4697c47e0c77c5a01da0`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `9ff7bc58d51013170884`
- Fields Used: _(None detected)_

**Missing**

- Type: `card`
- Name: `0beffc802ac3d38cd733`
- Fields Used:
  - `Dax_EmpCount_MissingTS` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: The provided filter definition JSON is specifying a condition that will be applied to the data in the `ContractStatusToday` column of the `vw_Timesheet` view in Power BI.

### Explanation in Business Terms:

- **Target Data**: The filter is applied to the `ContractStatusToday` field, which likely indicates the current status of contracts in the timesheet data.

- **Filter Condition**: The filter condition is "`ContractStatusToday` IN ('Valid')". This means that only the records where the `ContractStatusToday` is marked as "Valid" will be included in the analysis or report.

### What This Means for Your Data:

- **Included Data**: Any record (or row) in the `vw_Timesheet` view that has a `ContractStatusToday` value of "Valid" will be included in the results. This could represent contracts that are currently active, approved, or otherwise in good standing.

- **Excluded Data**: Any record where the `ContractStatusToday` is not "Valid" (for example, statuses like "Expired", "Pending", or "Invalid") will be excluded from the results. This means you will not see any information related to contracts that are not currently valid.

### Summary:

In summary, this filter ensures that your analysis focuses solely on contracts that are currently valid, allowing for more accurate insights and reporting on active contracts within the timesheet data.)

**Billable Dep**

- Type: `slicer`
- Name: `d0fd6724b72e9e9f0db1`
- Fields Used:
  - `BillableDep` (Query: `vw_missing_timesheet.BillableDep`) (Role: Values)

**Completeness %**

- Type: `card`
- Name: `82be32b9c5d5079089a9`
- Fields Used:
  - `MISSING%` (Query: `vw_missing_timesheet.MISSING%`) (Role: Values)

**shape**

- Type: `shape`
- Name: `183fd6702db411a0c215`
- Fields Used: _(None detected)_

**Manager / Employee**

- Type: `pivotTable`
- Name: `65ffad047035e8381c06`
- Fields Used:
  - ` ` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `  ` (Query: `PBI_TIMESHEET.IPM_ManagerName`) (Role: Rows)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the `vw_Timesheet` view, specifically focusing on the `Approved_With_V_Z` field.

Here's what the filter means:

- **Field**: `Approved_With_V_Z` - This is a column in the `vw_Timesheet` data that likely indicates whether a timesheet has been approved or not, with a specific value representing a certain status.

- **Condition**: `= 0` - This condition means that we are interested in only those records where the value of `Approved_With_V_Z` is equal to 0.

### What Data is Included or Excluded:

- **Included Data**: The filter will include only those timesheet records where `Approved_With_V_Z` is 0. This typically means that these records are not approved or are in a state that is considered "not approved."

- **Excluded Data**: Any records where `Approved_With_V_Z` is not equal to 0 (for example, records where it is 1 or any other value) will be excluded from the results. This means that any approved timesheets or those in a different status will not be shown.

### Summary:
In summary, this filter is used to focus on timesheet records that have not been approved, allowing users to analyze or report on only those specific entries.)

**funnel**

- Type: `funnel`
- Name: `e119b8a99ea024715e22`
- Fields Used:
  - `Dax_EmpCount_Approved` (Query: `PBI_TIMESHEET.Dax_EmpCount_Approved`) (Role: Y)
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Category)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: The provided filter definition JSON specifies a condition for the `TimesheetCode` field in the `vw_Timesheet` view. Here’s a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Includes Specific Codes**: The filter is set to include only those records where the `TimesheetCode` matches one of the specified values. In this case, the valid codes are:
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'V'
  - 'Z'
  - 'int'

### What Data is Included:
- **Records with Matching Codes**: When this filter is applied, only the timesheet entries that have a `TimesheetCode` of 'ROEG', 'ROIG', 'RSEG', 'V', 'Z', or 'int' will be included in the analysis or report. 

### What Data is Excluded:
- **All Other Codes**: Any timesheet entries that have a `TimesheetCode` not listed above will be excluded from the results. This means that if a timesheet has a code like 'ABC', 'XYZ', or any other code not specified in the filter, it will not appear in the filtered data.

### Summary:
In summary, this filter narrows down the data to only those timesheet entries that have specific codes, allowing for focused analysis on those particular entries while ignoring all others.)
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the `vw_Timesheet` view, specifically focusing on the `Approved_With_V_Z` field.

Here's what it means:

- The filter is looking for records where the value of `Approved_With_V_Z` is equal to **0**.
- When this filter is applied, it will **include** only those records from the `vw_Timesheet` where `Approved_With_V_Z` is **0**.
- Conversely, it will **exclude** any records where `Approved_With_V_Z` is **not 0** (i.e., any records where the value is 1 or any other number).

In summary, this filter is used to focus on timesheet entries that have not been approved (assuming that `0` indicates "not approved").)

**funnel**

- Type: `funnel`
- Name: `4c5e0b8242a93e4d5107`
- Fields Used:
  - `Dax_EmpCount_MissingTS` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Y)
  - `MAIN_UNIT` (Query: `vw_missing_timesheet.MAIN_UNIT`) (Role: Category)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: The provided Power BI filter definition is specifying which values of the `TimesheetCode` field should be included in the data being analyzed. 

In simple business terms, this filter is saying:

- **Include Only Specific Codes**: The filter will only allow data entries where the `TimesheetCode` matches one of the following values: 'int', 'ROEG', 'ROIG', 'RSEG', 'V', or 'Z'.
  
- **Exclude Everything Else**: Any entries with a `TimesheetCode` that is not one of these specified values will be excluded from the analysis.

So, when this filter is applied, you will only see timesheet records that have one of the listed codes, helping you focus on specific types of timesheets relevant to your analysis or reporting needs.)
  - Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and dashboards. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the field `ContractStatusToday` from the `vw_missing_timesheet` view. This means that no specific criteria are being applied to limit the data.

2. **Data Included**: When this filter is applied, every possible value for `ContractStatusToday` will be shown. For example, if `ContractStatusToday` includes statuses like "Active," "Inactive," "Pending," etc., all of these statuses will be included in the report.

3. **Data Excluded**: Since the filter is set to include all values, there are no exclusions. No data related to `ContractStatusToday` will be left out; everything is considered.

### Summary:
In summary, this filter allows you to see all records related to `ContractStatusToday` without any restrictions. It ensures that every status is represented in your analysis, providing a complete view of the data without filtering anything out.)
  - Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: The provided filter definition JSON is specifying a condition for the data that will be included when analyzing the `ContractStatusToday` field from the `vw_missing_timesheet` view in Power BI.

### Explanation in Business Terms:

- **Target Field**: The filter is applied to the field called `ContractStatusToday`. This field likely indicates the current status of contracts in the context of missing timesheets.

- **Filter Condition**: The filter condition is `IN ('Valid')`. This means that only records where the `ContractStatusToday` is marked as "Valid" will be included in the analysis.

### What This Means for Your Data:

- **Included Data**: Only contracts that are currently in a "Valid" status will be considered. This means you will see data related to contracts that are active and presumably in good standing.

- **Excluded Data**: Any contracts that are not marked as "Valid" (for example, those that might be "Expired," "Pending," or "Invalid") will be excluded from the analysis. This helps focus on contracts that are currently relevant and actionable.

### Summary:

In summary, this filter ensures that your analysis only looks at contracts that are currently valid, allowing you to concentrate on the most pertinent data regarding missing timesheets without the noise of other contract statuses.)

**Manager / Employee**

- Type: `pivotTable`
- Name: `8aaaf26692b347396169`
- Fields Used:
  - `  ` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `  ` (Query: `PBI_TIMESHEET.IPM_ManagerName`) (Role: Rows)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the `vw_Timesheet` view, specifically focusing on the `Approved_With_V_Z` column.

Here's what it means:

- The filter is looking for records (or rows) in the `vw_Timesheet` data where the value in the `Approved_With_V_Z` column is equal to **1**.
- This means that only those records that have been marked as "approved" (indicated by the value of 1) will be included in the analysis or report.
- Any records where `Approved_With_V_Z` is **not equal to 1** (for example, records with a value of 0 or any other value) will be excluded from the results.

In summary, this filter ensures that only approved timesheet entries are considered, allowing users to focus on the relevant data that meets the approval criteria.)
  - Filter on `vw_Timesheet`.`RReady_With_V_Z` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the target table `vw_Timesheet`. 

Here's what it means:

- The filter is looking at a specific column named `RReady_With_V_Z`.
- The condition "`RReady_With_V_Z` = 0" means that only the records (or rows) where the value in the `RReady_With_V_Z` column is equal to 0 will be included in the analysis or report.
- Any records where `RReady_With_V_Z` has a value other than 0 (such as 1, 2, or any other number) will be excluded from the results.

In summary, this filter is used to focus on a specific subset of data where the `RReady_With_V_Z` value is 0, allowing analysts to analyze or visualize only those records that meet this criterion.)
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: The provided filter definition in JSON format specifies a condition that will be applied to the `TimesheetCode` field in the `vw_Timesheet` view. Here’s a breakdown of what this means in simple business terms:

### What the Filter Does:
- **Inclusion of Specific Codes**: The filter is designed to include only those records from the `vw_Timesheet` view where the `TimesheetCode` matches one of the specified values.
- **Specified Values**: The values included by this filter are:
  - `int`
  - `ROEG`
  - `ROIG`
  - `RSEG`

### What Data is Included:
- When this filter is applied, only the timesheets that have a `TimesheetCode` of `int`, `ROEG`, `ROIG`, or `RSEG` will be shown in the report or analysis.
- Any timesheet with a `TimesheetCode` that is **not** one of these four values will be excluded from the results.

### Summary:
In summary, this filter is used to focus on a specific subset of timesheet data, allowing users to analyze or report on only those timesheets that are relevant to the specified codes. This can help in scenarios where only certain types of timesheets are of interest, such as specific projects or categories of work.)

**funnel**

- Type: `funnel`
- Name: `d6ba9f253d604522d7e9`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Category)
  - `Dax_EmpCount_RReady` (Query: `PBI_TIMESHEET.Dax_EmpCount_RReady`) (Role: Y)
  - `Count of MAIN_UNIT` (Query: `CountNonNull(vw_Timesheet.MAIN_UNIT)`) (Role: Unknown)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: The provided filter definition JSON specifies a condition for the `TimesheetCode` field in the `vw_Timesheet` view. Here’s a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Includes Specific Codes**: The filter is set to include only those records where the `TimesheetCode` matches one of the specified values. In this case, the valid codes are:
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'V'
  - 'Z'
  - 'int'

### What Data is Included:
- **Only the Listed Codes**: When this filter is applied, only timesheet entries that have a `TimesheetCode` of 'ROEG', 'ROIG', 'RSEG', 'V', 'Z', or 'int' will be included in the results. Any timesheet entries with codes outside of this list will be excluded.

### Summary:
In summary, this filter is used to focus on specific timesheet entries that are relevant for analysis or reporting, ensuring that only the selected codes are considered, while all other codes are ignored. This helps in narrowing down the data to only what is necessary for the business context.)
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the `vw_Timesheet` view, specifically focusing on the `Approved_With_V_Z` column.

Here's what it means:

- **Target Data**: The filter is applied to the `vw_Timesheet` view, which likely contains records of timesheets submitted by employees.
- **Column of Interest**: The filter is looking at the `Approved_With_V_Z` column. This column likely indicates whether a timesheet has been approved or not, with `1` representing "approved."
- **Filter Condition**: The condition "`Approved_With_V_Z` = 1" means that only those records where the `Approved_With_V_Z` column has a value of `1` will be included in the results. In other words, the filter is including only the timesheets that have been approved.

### Summary:
When this filter is applied, you will see only the approved timesheets in your report or analysis. Any timesheets that are not approved (where `Approved_With_V_Z` is not equal to `1`) will be excluded from the results.)
  - Filter on `vw_Timesheet`.`RReady_With_V_Z` (Type: Advanced, Explanation: In simple business terms, the provided filter definition JSON is specifying a condition that will be applied to the data in the target table called `vw_Timesheet`. 

Here's what it means:

- The filter is looking at a specific column named `RReady_With_V_Z`.
- The condition "`RReady_With_V_Z` = 0" means that the filter will **only include** rows (or records) from the `vw_Timesheet` table where the value in the `RReady_With_V_Z` column is exactly **0**.
- Any rows where `RReady_With_V_Z` has a value other than 0 (such as 1, 2, or any negative number) will be **excluded** from the results.

In summary, this filter is used to focus on a specific subset of data where the `RReady_With_V_Z` column indicates a value of 0, effectively filtering out all other values.)

**shape**

- Type: `shape`
- Name: `fec88edf8d1d5608b714`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `b0970b8fc01d258940aa`
- Fields Used: _(None detected)_

**3b871b9179460305b949**

- Type: `None`
- Name: `3b871b9179460305b949`
- Fields Used: _(None detected)_

#### <a name="page-timesheet-overviewcategory-monthly"></a>Page: Timesheet Overview/Category (Monthly)

*Internal Name: `4d6dc730d5c5f33b2ea4`, Ordinal: 10*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: In simple business terms, this Power BI filter is used to narrow down the data displayed in a report or dashboard based on specific criteria related to timesheet codes.

### What the Filter Does:
- The filter is applied to a field called `TimesheetCode` from a data source named `vw_Timesheet`.
- It specifies that only certain timesheet codes should be included in the analysis.

### Included Data:
The filter includes the following timesheet codes:
- **int**
- **ROEG**
- **ROIG**
- **RSEG**
- **V**
- **Z**

### Excluded Data:
Any timesheet codes that are **not** in the list above will be excluded from the report. This means that if a timesheet code is anything other than `int`, `ROEG`, `ROIG`, `RSEG`, `V`, or `Z`, it will not appear in the results.

### Summary:
In summary, this filter ensures that only the specified timesheet codes are considered in the analysis, allowing users to focus on specific types of timesheets while ignoring all others.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `d28f5e22f193f29a0d1f`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `e9aa188e49cdcac4f753`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `4f88dfbb16402e866ea9`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `3d911960df1ed1ac62e2`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)
  - `Min(DimDate.ShortDate)` (Role: Unknown)
  - `Max(DimDate.ShortDate)` (Role: Unknown)

**Unit**

- Type: `slicer`
- Name: `53cd937113b3f942e4e1`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `66ba428b970f98a3db68`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `b9e9f50150ab771e007a`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**Project**

- Type: `slicer`
- Name: `4c377af52bbb53790ca1`
- Fields Used:
  - `Project` (Query: `vw_Timesheet.Project`) (Role: Values)

**image**

- Type: `image`
- Name: `75a17bf5e9db26093a76`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `b3f6f1b3b472cfc2a392`
- Fields Used:
  - `Project` (Query: `PBI_TIMESHEET.Project`) (Role: Rows)
  - `Category` (Query: `PBI_TIMESHEET.GroupCat`) (Role: Rows)
  - `Year` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `Employee Name` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `MonthNumberOfYear` (Query: `DimDate.MonthNumberOfYear`) (Role: Columns)
  - `Hours` (Query: `vw_Timesheet.TotalActualHours`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`Project` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the field `vw_Timesheet`.`Project`. This means that no specific projects are being excluded or included; rather, every project in the dataset will be considered.

2. **Data Inclusion**: When this filter is applied, it allows all projects from the `vw_Timesheet` view to be shown in your report. There are no restrictions on which projects can be displayed.

3. **Practical Impact**: If you are looking at a report that summarizes timesheet data by project, using this filter means you will see data for every project that exists in the `vw_Timesheet` view. This is useful when you want a comprehensive overview without limiting the analysis to specific projects.

### Summary:
In summary, the filter `(All values)` means that all projects will be included in your analysis, providing a complete view of the data related to projects in the timesheet. There are no exclusions, so every project will be represented in the report.)
  - Filter on `vw_Timesheet`.`GroupCat` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and dashboards. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **Target Field**: The filter is applied to the field `vw_Timesheet`.`GroupCat`. This field likely represents categories or groups related to timesheet entries, such as different departments, projects, or types of work.

2. **Filter Meaning**: The phrase "(All values)" indicates that **no specific filtering is being applied** to the `GroupCat` field. This means that all categories or groups present in the data will be included in the report or visualization.

3. **Data Inclusion**: Since the filter is set to include all values, every entry in the `GroupCat` field will be shown. For example, if there are categories like "Development," "Marketing," and "Sales," all of these will be visible in the report.

4. **No Exclusions**: There are no exclusions or restrictions. This means that if there are any new categories added in the future, they will also automatically be included in the report without needing to adjust the filter.

### Summary:
In summary, this filter allows you to see all data related to the `GroupCat` field in your timesheet report, ensuring that no categories are left out. It provides a comprehensive view of all the groups involved in the timesheet data.)
  - Filter on (Name: `c7b94678aab7e1d8abc3`) (Type: Advanced, Explanation: The JSON you provided indicates that there is no specific filter definition available. In simple terms, this means that there are no criteria set to include or exclude any data when this filter is applied. 

In a business context, think of it like having a report or dashboard that shows all available data without any restrictions. Since there are no filters defined, every piece of data remains visible, and nothing is being filtered out. 

In summary, this filter does not limit or change the data in any way; it simply allows all data to be displayed as is.)
  - Filter on `vw_Timesheet`.`FinancialYear` (Type: Advanced, Explanation: In Power BI, filters are used to control which data is displayed in your reports and dashboards. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the field `vw_Timesheet.FinancialYear`. This means that when you apply this filter, you are not excluding any specific financial years from your data view.

2. **Data Inclusion**: Since the filter is set to "(All values)", every financial year available in the `vw_Timesheet` dataset will be included in the analysis. For example, if your dataset contains financial years from 2020 to 2023, all these years will be shown in your reports.

3. **No Exclusions**: There are no restrictions or exclusions applied. This is useful when you want to see a complete picture of the data across all financial years without limiting the view to a specific year or range.

### Summary:
In summary, this filter allows you to see all financial years in your timesheet data, ensuring that no data is left out. This is particularly helpful for comprehensive analysis or reporting where you want to understand trends or performance over multiple years.)

**Hours Distribution per Category**

- Type: `barChart`
- Name: `393cbe2916d2bebb49d9`
- Fields Used:
  - `Unit` (Query: `ONEDRIVE_UNIT.Unit`) (Role: Category)
  - `Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Y)
  - `GroupCat` (Query: `PBI_TIMESHEET.GroupCat`) (Role: Series)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `cf0e8815f4ac63cc1e44`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is designed to include only those records from the data source where the field `OWN-Sub-ExtT` is not empty or missing.

Here's a breakdown of what this means:

- **`OWN-Sub-ExtT`**: This is a specific field or column in your data that likely contains some important information related to units or entities you are analyzing.

- **`= null`**: This part of the filter checks if the value in the `OWN-Sub-ExtT` field is null, which means it is either empty or does not have any data.

- **`NOT`**: This negates the condition, meaning that instead of including records where `OWN-Sub-ExtT` is null, it will exclude them.

So, when this filter is applied, it effectively ensures that only records with a valid (non-empty) value in the `OWN-Sub-ExtT` field are included in your analysis. Any records where `OWN-Sub-ExtT` is null will be left out, allowing you to focus on data that has meaningful information.)

**Value/Percentage**

- Type: `slicer`
- Name: `45a66b7e3287915d6d3d`
- Fields Used:
  - `Hours_nd_Percentage` (Query: `Hours_nd_Percentage.Hours_nd_Percentage`) (Role: Values)

**Billable Dep**

- Type: `slicer`
- Name: `235465cf13497be45127`
- Fields Used:
  - `BillableDep` (Query: `PBI_TIMESHEET.BillableDep`) (Role: Values)

**Billable Prj**

- Type: `slicer`
- Name: `b8561014583422667ea6`
- Fields Used:
  - `BillablePrj` (Query: `PBI_TIMESHEET.BillablePrj`) (Role: Values)

**Category**

- Type: `slicer`
- Name: `006b2685c274c849b3b0`
- Fields Used:
  - `GroupCat` (Query: `PBI_TIMESHEET.GroupCat`) (Role: Values)

**actionButton**

- Type: `actionButton`
- Name: `d45227eb5a8a05388086`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `906e492dc2c895680cb7`
- Fields Used: _(None detected)_

**0c1ff44a49313745b76a**

- Type: `None`
- Name: `0c1ff44a49313745b76a`
- Fields Used: _(None detected)_

#### <a name="page-timesheet-overviewcategory-weekly"></a>Page: Timesheet Overview/Category (Weekly)

*Internal Name: `865887498f4acf763b3d`, Ordinal: 9*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is specifying which values of the `TimesheetCode` field should be included in the data being analyzed. 

In simple terms, the filter is saying:

- **Include only the following Timesheet Codes**: 
  - 'int'
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'V'
  - 'Z'

This means that when you apply this filter to the data, only records (or entries) that have a `TimesheetCode` matching one of these specified values will be shown. Any records with a `TimesheetCode` that is not in this list will be excluded from the analysis.

So, if you were looking at a report or dashboard, you would only see data related to those specific Timesheet Codes, helping you focus on particular types of timesheets relevant to your analysis or reporting needs.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `35236d830185c05005a6`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `297dddf35514668c5f0a`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `26c93150208589bbb57d`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `582550f98a80b1b3086c`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)
  - `Min(DimDate.ShortDate)` (Role: Unknown)
  - `Max(DimDate.ShortDate)` (Role: Unknown)

**Unit**

- Type: `slicer`
- Name: `62babe310ac19ddf8035`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `19869fbad60007442024`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `230fd268acdbfcf0e6d7`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**Project**

- Type: `slicer`
- Name: `680a097e0989e3e02bff`
- Fields Used:
  - `Project` (Query: `vw_Timesheet.Project`) (Role: Values)

**image**

- Type: `image`
- Name: `b91696d49a34bd1ec780`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `c1c393d2cc21e4000db9`
- Fields Used:
  - `Project` (Query: `PBI_TIMESHEET.Project`) (Role: Rows)
  - `Category` (Query: `PBI_TIMESHEET.GroupCat`) (Role: Rows)
  - `Week` (Query: `DimDate.WeekNumberOfYear`) (Role: Columns)
  - `Year` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `Employee Name` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `Hours` (Query: `vw_Timesheet.TotalActualHours`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`Project` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** from the `vw_Timesheet`.`Project` data. This means that no specific projects are being excluded or included; instead, every project in the dataset will be considered.

2. **Data Inclusion**: When this filter is applied, it allows you to see data related to every project available in the `vw_Timesheet` view. You will not miss any project data, regardless of its status, type, or any other characteristics.

3. **No Restrictions**: Since the filter is set to "(All values)", there are no restrictions or conditions applied. This is useful when you want a comprehensive view of all projects without narrowing down to specific ones.

### Summary:
In summary, this filter ensures that all projects from the `vw_Timesheet` view are included in your analysis, providing a complete picture of project-related data without any exclusions.)
  - Filter on `vw_Timesheet`.`GroupCat` (Type: Categorical, Explanation: In Power BI, filters are used to control which data is displayed in your reports and visualizations. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **Target Field**: The filter is applied to the field `vw_Timesheet`.`GroupCat`. This field likely represents different categories or groups related to timesheets, such as project categories, employee groups, or task types.

2. **Filter Meaning**: The phrase "(All values)" indicates that **no specific filtering is being applied** to the `GroupCat` field. This means that all categories or groups within the `GroupCat` field will be included in the data being analyzed or displayed.

3. **Data Inclusion**: Since the filter is set to include "All values," every possible category in the `GroupCat` field will be shown in the report. For example, if there are categories like "Development," "Design," and "Testing," all of these will be included in the analysis.

4. **Data Exclusion**: There are no exclusions with this filter. No categories are being left out; everything is considered.

### Summary:
In summary, this filter allows you to see all the data related to the `GroupCat` field without any restrictions. It ensures that every category is represented in your reports, providing a complete view of the timesheet data across all groups.)
  - Filter on (Name: `c7b94678aab7e1d8abc3`) (Type: Advanced, Explanation: The JSON you provided, `"(No definition found)"`, indicates that there is no specific filter definition applied. In simple business terms, this means that there are no criteria set to include or exclude any data when this filter is used.

### What This Means:
- **No Data Restrictions**: Since there is no filter defined, all data will be included in any analysis or report. There are no limitations on what data can be viewed or analyzed.
- **Full Visibility**: Users will see the complete dataset without any segments or subsets being filtered out.

### Summary:
In essence, this filter does not change or limit the data in any way; it allows for a comprehensive view of all available information.)
  - Filter on `vw_Timesheet`.`FinancialYear` (Type: Advanced, Explanation: In Power BI, filters are used to control which data is displayed in your reports and dashboards. The filter definition JSON you provided is:

```json
"(All values)"
```

### Explanation in Business Terms:

1. **What It Means**: The filter is set to include **all values** for the `FinancialYear` field from the `vw_Timesheet` data. This means that when you apply this filter, you are not excluding any specific years from your analysis.

2. **Data Included**: Every financial year available in the `vw_Timesheet` dataset will be included in your report. For example, if your dataset contains financial years from 2018 to 2023, all these years will be shown in your visuals.

3. **Data Excluded**: Since the filter is set to "(All values)", there are no exclusions. No financial years are left out of the analysis.

### Summary:
This filter allows you to see a complete view of all financial years in your timesheet data, ensuring that you have a comprehensive understanding of trends and performance across all years without any limitations.)

**Hours Distribution per Category**

- Type: `barChart`
- Name: `eed6aff9c10e0791d1c6`
- Fields Used:
  - `Unit` (Query: `ONEDRIVE_UNIT.Unit`) (Role: Category)
  - `Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Y)
  - `GroupCat` (Query: `PBI_TIMESHEET.GroupCat`) (Role: Series)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `521d9213056264e973d4`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is designed to exclude any records where the field `OWN-Sub-ExtT` is empty or has no value (i.e., is null).

Here's a breakdown of what this means:

- **Target Field**: The filter is applied to the field `lkp_Unit`.`OWN-Sub-ExtT`, which likely represents a specific attribute or category related to units in your data model.
  
- **Filter Logic**: The filter uses the condition `NOT (`OWN-Sub-ExtT` = null)`. This means that it will only include records where `OWN-Sub-ExtT` has a value. In other words, any record where `OWN-Sub-ExtT` is empty or not filled in will be excluded from the analysis.

### Summary:
When this filter is applied, you will only see data entries that have a defined value in the `OWN-Sub-ExtT` field. Any entries without a value in this field will be ignored, ensuring that your analysis focuses only on complete and relevant data.)

**Value/Percentage**

- Type: `slicer`
- Name: `58d66ea51d3a5e519c94`
- Fields Used:
  - `Hours_nd_Percentage` (Query: `Hours_nd_Percentage.Hours_nd_Percentage`) (Role: Values)

**Billable Dep**

- Type: `slicer`
- Name: `97efc82bb9100ae69a40`
- Fields Used:
  - `BillableDep` (Query: `PBI_TIMESHEET.BillableDep`) (Role: Values)

**Billable Prj**

- Type: `slicer`
- Name: `886540a31bbe2c6cccb1`
- Fields Used:
  - `BillablePrj` (Query: `PBI_TIMESHEET.BillablePrj`) (Role: Values)

**Category**

- Type: `slicer`
- Name: `501bb15dc37e34533838`
- Fields Used:
  - `GroupCat` (Query: `PBI_TIMESHEET.GroupCat`) (Role: Values)

**actionButton**

- Type: `actionButton`
- Name: `fef819910cab65cc1d5e`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `eb2bb2d2e060386ca40c`
- Fields Used: _(None detected)_

**920bcde87609e60dc0d7**

- Type: `None`
- Name: `920bcde87609e60dc0d7`
- Fields Used: _(None detected)_

#### <a name="page-timesheet-overviewconsultant-monthly"></a>Page: Timesheet Overview/Consultant (Monthly)

*Internal Name: `3215a5c48eb52948c14e`, Ordinal: 13*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: The provided Power BI filter definition is specifying a condition for the `TimesheetCode` field in the `vw_Timesheet` view. Here's a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Includes Specific Codes**: The filter is set to include only certain values for the `TimesheetCode`. Specifically, it allows the following codes:
  - `int`
  - `ROEG`
  - `ROIG`
  - `RSEG`
  - `V`
  - `Z`
  - `RSIG`

### What This Means:
- **Data Inclusion**: When this filter is applied, only records from the `vw_Timesheet` view that have a `TimesheetCode` matching one of the specified values will be included in the analysis or report. 
- **Data Exclusion**: Any records with a `TimesheetCode` that is **not** one of these specified values will be excluded from the results. This means that if a timesheet has a code like `ABC` or `XYZ`, it will not show up in the filtered data.

### Summary:
In summary, this filter is used to focus on a specific set of timesheet codes that are likely relevant for a particular analysis or report, ensuring that only those entries are considered while filtering out all others.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `888a7ce6dcd9e79a8571`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `e24ea6b952b0e52f8bc3`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `d52f867b0050a9900aab`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `66934532619b0b009073`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Unit**

- Type: `slicer`
- Name: `9029f0dd89303697364d`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `646e8ef1016d8cbb5042`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `e410cc9b9e813ccb6a09`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**Own-Sub-Ext**

- Type: `slicer`
- Name: `4982e2f59b589712a24d`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is designed to exclude any records where the field `OWN-Sub-ExtT` is empty or has no value (i.e., is null).

Here's a breakdown of what this means:

- **Target Field**: The filter is applied to the field `lkp_Unit`.`OWN-Sub-ExtT`.
- **Condition**: The filter states "NOT (`OWN-Sub-ExtT` = null)", which means it is looking for records where `OWN-Sub-ExtT` does **not** equal null.
- **Inclusion/Exclusion**: As a result, this filter will include only those records where `OWN-Sub-ExtT` has a valid value (i.e., it is not empty). Any record where `OWN-Sub-ExtT` is null will be excluded from the results.

In summary, this filter ensures that only records with meaningful data in the `OWN-Sub-ExtT` field are considered, filtering out any records that lack this information.)

**image**

- Type: `image`
- Name: `223afae80d189e808382`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `37aeaba9e2b02eac1abb`
- Fields Used:
  - `Employee Name` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `Project` (Query: `PBI_TIMESHEET.Project`) (Role: Rows)
  - `Sum of Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Values)
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Rows)
  - `Year` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `Month` (Query: `DimDate.MonthNumberOfYear`) (Role: Columns)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is designed to exclude any records where the `EmployeeName` field is empty or not specified. 

Here's a breakdown of what this means:

- **Target Field**: The filter is applied to the `EmployeeName` field in the `vw_Timesheet` data view.
- **Condition**: The filter uses the condition `NOT (`EmployeeName` = null)`. 
- **Interpretation**: 
  - The term `null` refers to cases where there is no value provided for `EmployeeName`. This could mean that the employee's name is missing or not recorded in the data.
  - By using `NOT`, the filter is saying, "I want to include all records where the `EmployeeName` is NOT null." 

**In summary**: When this filter is applied, it will only show records where there is a valid employee name present. Any records that do not have an employee name (i.e., those that are blank or missing) will be excluded from the results.)

**actionButton**

- Type: `actionButton`
- Name: `cfa706d8a390a0c61707`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `0324a9692aca3cdac199`
- Fields Used: _(None detected)_

**629b6a08a617a0c0ecbd**

- Type: `None`
- Name: `629b6a08a617a0c0ecbd`
- Fields Used: _(None detected)_

#### <a name="page-timesheet-overviewconsultant-weekly"></a>Page: Timesheet Overview/Consultant (Weekly)

*Internal Name: `96c31ad35c0ed9f19d1d`, Ordinal: 12*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is specifying which values of the `TimesheetCode` field from the `vw_Timesheet` data view should be included in the analysis or report.

### Breakdown of the Filter:

- **Target Field**: The filter is applied to the `TimesheetCode` field.
- **Included Values**: The filter includes only the following specific values:
  - 'int'
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'V'
  - 'Z'
  - 'RSIG'

### Business Explanation:

When this filter is applied, the report or analysis will only show data related to timesheets that have one of the specified codes. In practical terms, this means:

- **Included**: If a timesheet has a code of 'int', 'ROEG', 'ROIG', 'RSEG', 'V', 'Z', or 'RSIG', it will be included in the results.
- **Excluded**: Any timesheet with a code that is not one of these (for example, codes like 'ABC', 'XYZ', or any other code not listed) will be excluded from the results.

### Summary:

In summary, this filter is used to focus on specific types of timesheets that are relevant for the analysis, ensuring that only the data associated with these particular codes is considered, while all other timesheet codes are ignored.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `5f37b9d9309798827013`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `eefe24b606f9e11195ec`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `b68ed4ff60eaaa69eba3`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `e3a7d72294d8d9e61495`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Unit**

- Type: `slicer`
- Name: `b2b4cc327952867b69e4`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `432e1018d6ab8f57edee`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`Unit` (Type: Advanced, Explanation: The provided Power BI filter definition JSON is specifying a condition that filters the data based on the `Unit` field in the `vw_Timesheet` view.

In simple business terms, this filter is saying:

**"Include all records where the `Unit` field has a value, and exclude any records where the `Unit` field is empty or has no value."**

### Breakdown:
- **`NOT`**: This indicates that we are excluding certain records based on the condition that follows.
- **`(`Unit` = null)`**: This part checks if the `Unit` field is empty (or null). 

### What this means for your data:
- **Included**: Any record in the `vw_Timesheet` where the `Unit` has a valid entry (e.g., it could be a department, team, or any other identifier that is not empty).
- **Excluded**: Any record where the `Unit` is not specified (i.e., it is null or empty).

In summary, this filter ensures that you only see timesheet entries that are associated with a specific unit, helping to focus on relevant data and avoid confusion from entries that lack this important information.)

**Employee**

- Type: `slicer`
- Name: `7b4d0077c29ca2050146`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**Own-Sub-Ext**

- Type: `slicer`
- Name: `e6c6261fce63077266d0`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is saying that we want to include only those records from the data source where the field `OWN-Sub-ExtT` is not empty or missing.

Here's a breakdown of what this means:

- **`OWN-Sub-ExtT`**: This is a specific column or field in your data that likely contains some kind of identifier or value related to units (like a unit number, code, or description).
  
- **`= null`**: This part checks if the value in the `OWN-Sub-ExtT` field is null, which means it has no value or is empty.

- **`NOT`**: This negates the condition, meaning we want to exclude any records where `OWN-Sub-ExtT` is null.

So, when this filter is applied, it will only show records where `OWN-Sub-ExtT` has a valid value (i.e., it is not null). Any records that do not have a value in this field will be excluded from the results. 

In summary, this filter ensures that you are only looking at data that has meaningful entries in the `OWN-Sub-ExtT` field, helping to focus your analysis on relevant information.)

**image**

- Type: `image`
- Name: `8887940f9bb84336a480`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `936465ad5c5d30136110`
- Fields Used:
  - `Employee Name` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `Project` (Query: `PBI_TIMESHEET.Project`) (Role: Rows)
  - `Sum of Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Values)
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Rows)
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Columns)
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Columns)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is saying:

**"Include all records where the Employee Name is not empty."**

Here's a breakdown of what this means:

- **`EmployeeName`**: This refers to the name of the employee in the dataset.
- **`= null`**: This part checks if the Employee Name is empty or missing (which is referred to as "null" in data terminology).
- **`NOT`**: This means we want to exclude those records where the Employee Name is empty.

So, when this filter is applied to the data in the `vw_Timesheet` view, it will only show records for employees who have a name listed. Any records where the Employee Name is missing will be excluded from the results. This helps ensure that the analysis focuses only on valid employee entries.)

**actionButton**

- Type: `actionButton`
- Name: `50e8a7f168a5904d13ed`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `81ffb40a701db7112455`
- Fields Used: _(None detected)_

**fd150d496c7e55b27240**

- Type: `None`
- Name: `fd150d496c7e55b27240`
- Fields Used: _(None detected)_

#### <a name="page-timesheet-overviewproject"></a>Page: Timesheet Overview/Project

*Internal Name: `a2f57de1df4564ebf3b0`, Ordinal: 11*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is specifying which values of the `TimesheetCode` field should be included in the data being analyzed. 

In simple terms, the filter is saying:

- **Include only the following Timesheet Codes**: 
  - 'int'
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'V'
  - 'Z'

This means that when this filter is applied, only records (or entries) from the dataset that have a `TimesheetCode` matching one of these specified values will be shown. Any records with a `TimesheetCode` that is not one of these six values will be excluded from the analysis.

So, if you are looking at a report or dashboard in Power BI that uses this filter, you will only see data related to those specific Timesheet Codes, helping you focus on a particular subset of timesheet entries.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `3101b894b4728043d0de`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `35fbf172c62998f76e07`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `45f2da347ace6c48a106`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `e9bc7580801d40e87870`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Project**

- Type: `slicer`
- Name: `4cdc8d399e4d53955378`
- Fields Used:
  - `Project` (Query: `PBI_TIMESHEET.Project`) (Role: Values)

**Unit**

- Type: `slicer`
- Name: `0571d0c1bd49a0989746`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `d799f882219e350c7985`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `2f5af7aadc23b9bf904d`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**image**

- Type: `image`
- Name: `c4844a1b7c6e3d323193`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `eecbe36df2b3c67b0a37`
- Fields Used:
  - `Category` (Query: `PBI_TIMESHEET.GroupCat`) (Role: Columns)
  - `%RT Sum of Hours` (Query: `Sum(PBI_TIMESHEET.Hours)`) (Role: Values)
  - `Employee Name` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `SubUnit` (Query: `PBI_TIMESHEET.Unit`) (Role: Rows)
  - `Unit` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Rows)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is designed to exclude any records where the `EmployeeName` field is empty or not provided. 

Here's a breakdown of the filter:

- **`EmployeeName` = null**: This part checks if the `EmployeeName` is empty (or null). In a business context, this means it looks for entries where no employee name has been recorded.

- **NOT**: This negates the condition. So instead of including records where the employee name is empty, it excludes them.

When this filter is applied to the target `vw_Timesheet`.`EmployeeName`, it means that only records with a valid employee name will be included in the analysis. Any records where the employee name is missing will be left out. 

In summary, this filter ensures that the data you are analyzing only contains entries for employees who have a name listed, helping to maintain data quality and relevance in your reports.)

**Category**

- Type: `slicer`
- Name: `bc6e0a0fd1579ff6c6a0`
- Fields Used:
  - `GroupCat` (Query: `PBI_TIMESHEET.GroupCat`) (Role: Values)

**Billable Dep**

- Type: `slicer`
- Name: `111406e7a2ebf355d67e`
- Fields Used:
  - `BillableDep` (Query: `PBI_TIMESHEET.BillableDep`) (Role: Values)

**Billable Prj**

- Type: `slicer`
- Name: `3d43058b0d612922473e`
- Fields Used:
  - `BillablePrj` (Query: `PBI_TIMESHEET.BillablePrj`) (Role: Values)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `ce32662685796e9a464a`
- Fields Used:
  - `OWN-Sub-ExtT` (Query: `ONEDRIVE_UNIT.OWN-Sub-ExtT`) (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: In simple business terms, this Power BI filter definition is saying that we want to include only those records where the field `OWN-Sub-ExtT` is not empty or missing. 

Here's a breakdown of what this means:

- **`OWN-Sub-ExtT`**: This is a specific column or field in your data that likely contains some kind of identifier or value related to units.
  
- **`= null`**: This part checks if the value in the `OWN-Sub-ExtT` field is empty or does not exist (which is referred to as "null" in data terminology).

- **`NOT`**: This means we are excluding any records where `OWN-Sub-ExtT` is null.

So, when this filter is applied, it will only show records where `OWN-Sub-ExtT` has a valid value (i.e., it is not null). Any records that have no value in this field will be excluded from the results. 

In summary, this filter ensures that you are only looking at data that has meaningful entries in the `OWN-Sub-ExtT` field, helping to focus your analysis on relevant records.)

**actionButton**

- Type: `actionButton`
- Name: `0ea232c0bb7903644da2`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `30c8b9bb00c2c69e9843`
- Fields Used: _(None detected)_

**3eb6cc504940db2a5407**

- Type: `None`
- Name: `3eb6cc504940db2a5407`
- Fields Used: _(None detected)_

#### <a name="page-utilisationmonth"></a>Page: Utilisation/Month

*Internal Name: `19b33dca06c265585412`, Ordinal: 8*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is specifying which values of the `TimesheetCode` field from the `vw_Timesheet` table should be included in the analysis or report.

### Breakdown of the Filter:

- **Target Field**: `vw_Timesheet`.`TimesheetCode`
  - This is the specific column in the data that we are filtering on. It contains various codes that represent different types of timesheets.

- **Filter Condition**: `IN ('int', 'ROEG', 'ROIG', 'RSEG', 'V', 'Z', 'RSIG')`
  - This part of the filter indicates that we want to include only those records where the `TimesheetCode` matches one of the specified values in the parentheses.

### Included Values:
The filter includes the following `TimesheetCode` values:
- `int`
- `ROEG`
- `ROIG`
- `RSEG`
- `V`
- `Z`
- `RSIG`

### Excluded Values:
Any `TimesheetCode` that is **not** one of the above values will be excluded from the analysis. This means that if a timesheet has a code like `ABC`, `XYZ`, or any other code not listed, it will not be considered in the results.

### Summary:
In simple terms, this filter is set up to focus on specific types of timesheets that are identified by their codes. Only the timesheets with the codes `int`, `ROEG`, `ROIG`, `RSEG`, `V`, `Z`, and `RSIG` will be included in the report or analysis, while all other timesheet codes will be ignored. This helps in narrowing down the data to only the relevant entries for whatever analysis is being performed.)
- Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: The provided filter definition JSON is specifying a condition for the data that will be included when analyzing the `ContractStatusToday` field from the `vw_missing_timesheet` view in Power BI.

### Explanation in Business Terms:

- **Target Field**: The filter is applied to the `ContractStatusToday` field, which likely indicates the current status of contracts in the system.

- **Filter Condition**: The filter states that only records where the `ContractStatusToday` is equal to 'Valid' should be included in the analysis.

### What This Means:

- **Included Data**: Only contracts that are currently marked as 'Valid' will be considered. This means you will see data related to contracts that are active and in good standing.

- **Excluded Data**: Any contracts that have a status other than 'Valid' (for example, 'Expired', 'Pending', or 'Cancelled') will be excluded from the analysis. This helps focus on contracts that are currently operational and relevant for decision-making.

### Summary:

In summary, this filter ensures that your analysis only looks at contracts that are valid, allowing for more accurate insights and reporting on the current state of contracts without the noise of inactive or irrelevant statuses.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `0f0d6f04bcba0936f322`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `26da01e81bf0dbc42270`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `9340a58008d85982feb4`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `5a2b6b4e50f3b2e572a5`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Unit**

- Type: `slicer`
- Name: `8060c1c04a4583ebc3a5`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `2f72a40ecbbbb83f60ec`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `35a72a456ad38b474f7e`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**Company**

- Type: `slicer`
- Name: `ff705e41d0c1c3ba9cf7`
- Fields Used:
  - `cc_Employer` (Query: `PBI_TIMESHEET.cc_Employer`) (Role: Values)

**Billable Dep**

- Type: `slicer`
- Name: `3ffbb8363baba81a1c0c`
- Fields Used:
  - `BillableDep` (Query: `PBI_TIMESHEET.BillableDep`) (Role: Values)

**image**

- Type: `image`
- Name: `525c1a46fa75668dccae`
- Fields Used: _(None detected)_

**lineChart**

- Type: `lineChart`
- Name: `c5a7305437e71cbbbbd5`
- Fields Used:
  - `Utilization` (Query: `vw_Timesheet.UTILIZATION_New`) (Role: Y)
  - `Month` (Query: `DimDate.MonthNumberOfYear`) (Role: Category)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `0d0b7b061d634193ff3e`
- Fields Used:
  - `Own-Sub-ExtT` (Query: `PBI_TIMESHEET.Own-Sub-ExtT`) (Role: Values)

**Utilization/Month**

- Type: `pivotTable`
- Name: `3ea0cc68861d87db061b`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Rows)
  - `Unit` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Rows)
  - `UTILIZATION_New` (Query: `PBI_TIMESHEET.UTILIZATION`) (Role: Values)
  - `Calendar Year` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `MonthNumberOfYear` (Query: `DimDate.MonthNumberOfYear`) (Role: Columns)

**lineChart**

- Type: `lineChart`
- Name: `ebdefe00dcf868433ac4`
- Fields Used:
  - `Unit` (Query: `lkp_Unit.Unit`) (Role: Series)
  - `UTILIZATION_New` (Query: `vw_Timesheet.UTILIZATION_New`) (Role: Y)
  - `MonthNumberOfYear` (Query: `DimDate.MonthNumberOfYear`) (Role: Category)

**lineChart**

- Type: `lineChart`
- Name: `1c026ae0be7337e368e2`
- Fields Used:
  - `UTILIZATION_New` (Query: `vw_Timesheet.UTILIZATION_New`) (Role: Y)
  - `Unit` (Query: `vw_Timesheet.Unit`) (Role: Series)
  - `MonthNumberOfYear` (Query: `DimDate.MonthNumberOfYear`) (Role: Category)

**actionButton**

- Type: `actionButton`
- Name: `a24c382ddef42ab042c1`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `e935d1784d364e9818a4`
- Fields Used: _(None detected)_

**e818101206fbab318728**

- Type: `None`
- Name: `e818101206fbab318728`
- Fields Used: _(None detected)_

#### <a name="page-utilisationweek"></a>Page: Utilisation/Week

*Internal Name: `f0b5cc1c206a68c8c659`, Ordinal: 7*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is specifying which values of the `TimesheetCode` field should be included in the data being analyzed. 

In simple terms, the filter is saying:

- **Include only the following Timesheet Codes**: 
  - 'int'
  - 'ROEG'
  - 'ROIG'
  - 'RSEG'
  - 'V'
  - 'Z'
  - 'RSIG'

### What This Means for Your Data:

- **Included Data**: When this filter is applied, only records (or entries) that have a `TimesheetCode` matching one of the specified values will be included in your analysis. For example, if you have a timesheet entry with the code 'ROEG', it will be included in the results.

- **Excluded Data**: Any records with a `TimesheetCode` that is **not** one of these specified values will be excluded from the analysis. For instance, if there is a timesheet entry with the code 'ABC', it will not appear in the results.

### Summary:
This filter helps narrow down the dataset to focus only on specific timesheet codes that are relevant for your analysis, ensuring that you are looking at the most pertinent information.)
- Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: In simple business terms, this Power BI filter is designed to focus on a specific subset of data related to the `ContractStatusToday` field in the `vw_missing_timesheet` view.

### What the Filter Does:
- **Includes Only Valid Contracts**: The filter specifies that only records where the `ContractStatusToday` is marked as 'Valid' will be included in the analysis or report.
  
### What the Filter Excludes:
- **Excludes All Other Contract Statuses**: Any records where the `ContractStatusToday` is not 'Valid' (such as 'Expired', 'Pending', or any other status) will be excluded from the data being analyzed.

### Summary:
When this filter is applied, you will only see data related to contracts that are currently valid, allowing you to focus on active and relevant contracts without any distractions from those that are not valid.)

##### Visuals on this Page

**textbox**

- Type: `textbox`
- Name: `e598b219764e53b23f86`
- Fields Used: _(None detected)_

**image**

- Type: `image`
- Name: `77f8dd74c4c32798af7c`
- Fields Used: _(None detected)_

**Year**

- Type: `slicer`
- Name: `761ace8646bfedd56cd9`
- Fields Used:
  - `CalendarYear` (Query: `DimDate.CalendarYear`) (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `67b9bf082b13cbe2972e`
- Fields Used:
  - `ShortDate` (Query: `DimDate.ShortDate`) (Role: Values)

**Unit**

- Type: `slicer`
- Name: `b81d500d2f739dc6a5b4`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `6d8573f0c6ac8f381225`
- Fields Used:
  - `Unit` (Query: `PBI_TIMESHEET.Unit`) (Role: Values)

**Employee**

- Type: `slicer`
- Name: `7dc145f704253953df3b`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Values)

**Company**

- Type: `slicer`
- Name: `421d71f99b8ec5132d05`
- Fields Used:
  - `cc_Employer` (Query: `PBI_TIMESHEET.cc_Employer`) (Role: Values)

**Billable Dep**

- Type: `slicer`
- Name: `99aae39106f26adaa88b`
- Fields Used:
  - `BillableDep` (Query: `PBI_TIMESHEET.BillableDep`) (Role: Values)

**image**

- Type: `image`
- Name: `b80b9d885ebcd89e13a4`
- Fields Used: _(None detected)_

**Utilization/Week**

- Type: `pivotTable`
- Name: `4ecbee1043f03ecc8b71`
- Fields Used:
  - `EmployeeName` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `Unit` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Rows)
  - `Utilization` (Query: `PBI_TIMESHEET.UTILIZATION`) (Role: Values)
  - `Week` (Query: `DimDate.WeekNumberOfYear`) (Role: Columns)
  - `Calendar Year` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `SubUnit` (Query: `vw_Timesheet.Unit`) (Role: Rows)

**lineChart**

- Type: `lineChart`
- Name: `7362c199860207beb1bd`
- Fields Used:
  - `Week` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Utilization` (Query: `vw_Timesheet.UTILIZATION_New`) (Role: Y)

**Current Week**

- Type: `card`
- Name: `ce457d20f07d952ccf2a`
- Fields Used:
  - `CurrentWeekCard` (Query: `PBI_TIMESHEET.CurrentWeekCard`) (Role: Values)

**Sub-Own-Ext**

- Type: `slicer`
- Name: `5dc95c6daacd02139db8`
- Fields Used:
  - `Own-Sub-ExtT` (Query: `PBI_TIMESHEET.Own-Sub-ExtT`) (Role: Values)

**lineChart**

- Type: `lineChart`
- Name: `a8972e2511ac6bed470c`
- Fields Used:
  - `Week` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `Utilization` (Query: `vw_Timesheet.UTILIZATION_New`) (Role: Y)
  - `Unit` (Query: `lkp_Unit.Unit`) (Role: Series)

**lineChart**

- Type: `lineChart`
- Name: `533ba0bc1c6ad6622592`
- Fields Used:
  - `WeekNumberOfYear` (Query: `DimDate.WeekNumberOfYear`) (Role: Category)
  - `UTILIZATION_New` (Query: `vw_Timesheet.UTILIZATION_New`) (Role: Y)
  - `Unit` (Query: `vw_Timesheet.Unit`) (Role: Series)

**actionButton**

- Type: `actionButton`
- Name: `1aac21e8140deb43d06f`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `3d4f4c950ffa336d5af8`
- Fields Used: _(None detected)_

**256e456cfd6a08250f63**

- Type: `None`
- Name: `256e456cfd6a08250f63`
- Fields Used: _(None detected)_


---

