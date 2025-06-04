# Power BI Model & Report Documentation

*Generated on: 2025-06-04 10:59:56*

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
    - [Timesheet Overview/Category per Week](#page-timesheet-overviewcategory-per-week)
    - [Timesheet Overview/Category per Month](#page-timesheet-overviewcategory-per-month)
    - [Timesheet Overview/Project](#page-timesheet-overviewproject)
    - [Timesheet Overview/Consultant (Weekly)](#page-timesheet-overviewconsultant-weekly)
    - [Timesheet Overview/Consultant (Monthly)](#page-timesheet-overviewconsultant-monthly)
    - [Timesheet (Details)](#page-timesheet-details)

---

## <a name="overview"></a>Overview

The Power BI data model appears to be designed for a business domain focused on time tracking and project management, likely within a professional services or project-based organization. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a strong emphasis on capturing and analyzing employee work hours and the submission of timesheets, which are critical for billing, resource allocation, and performance evaluation. The inclusion of lookup tables like 'lkp_Project', 'lkp_Unit', and 'lkp_Code' indicates a structured approach to categorizing and filtering data related to specific projects, organizational units, and timesheet codes, enhancing the model's analytical capabilities.

The relationships established between these tables facilitate comprehensive reporting and insights into time management practices. For instance, linking 'vw_Timesheet' with 'DimDate' allows for time-based analysis, while connections to project and unit lookup tables enable detailed breakdowns of hours worked by project and organizational structure. Additionally, the 'vw_missing_timesheet' table highlights a focus on identifying gaps in timesheet submissions, which is crucial for ensuring accurate project tracking and accountability. Overall, this data model is likely aimed at improving operational efficiency, enhancing project oversight, and supporting strategic decision-making through data-driven insights.

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

### <a name="tables"></a>Tables

#### <a name="table-dimdate"></a>Table: `DimDate`

The 'DimDate' table serves as a comprehensive date dimension, enabling businesses to analyze and report on time-based data effectively. It provides essential date attributes, such as day, week, month, and quarter, facilitating detailed time-series analysis and enhancing the ability to track performance trends over various periods.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year (e.g., Q1, Q2, Q3, Q4) for each date entry in the DimDate table, facilitating time-based analysis and reporting. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of a date, facilitating time-based analysis and reporting within the dataset. |
| `DateKey` | `int64` | Column Description: The 'DateKey' column (int64) serves as a unique identifier for each date in the 'DimDate' table, facilitating efficient date-based queries and data enrichment processes. |
| `DayName` | `string` | The 'DayName' column in the 'DimDate' table provides the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating time-based analysis and reporting. |
| `DayNumberOfMonth` | `int64` | Column Description: The 'DayNumberOfMonth' column represents the numerical day of the month (ranging from 1 to 31) for each date entry in the 'DimDate' table, facilitating date-related analyses and reporting. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical value of the day of the week, where 1 corresponds to Sunday and 7 corresponds to Saturday, facilitating date-related analyses and reporting. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column represents the sequential day of the year (ranging from 1 to 365 or 366) for each date entry in the 'DimDate' table, facilitating time-based analysis and reporting. |
| `MonthName` | `string` | The 'MonthName' column in the 'DimDate' table stores the full names of the months, providing a human-readable representation of the month for enhanced date-related reporting and analysis. |
| `MonthNumberOfYear` | `int64` | Column Description: Represents the numerical value of the month (1 for January through 12 for December) to facilitate time-based analysis and reporting within the DimDate table. |
| `ShortDate` | `dateTime` | The 'ShortDate' column in the 'DimDate' table represents a simplified date format, capturing the specific date without time details to facilitate efficient date-based analysis and reporting. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number of the year for each date, facilitating time-based analysis and reporting. |

##### Calculated Columns

**`CurrentYearFlag`** (`string`)

- **Description:** Indicates whether the date corresponds to the current calendar year, with values typically set to 'Yes' or 'No'.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `CurrentYearFlag` in a data model, typically within a tool like Power BI or Excel. Here's a breakdown of what this expression does in simple business terms:

1. **Purpose**: The `CurrentYearFlag` column is designed to indicate whether a date falls within the current year or not.

2. **Components**:
   - `DimDate[ShortDate]`: This refers to a date field in a table called `DimDate`. It contains various dates that we want to evaluate.
   - `YEAR(DimDate[ShortDate])`: This part extracts the year from each date in the `ShortDate` column.
   - `YEAR(TODAY())`: This retrieves the current year based on today's date.

3. **Logic**:
   - The `IF` function checks if the year extracted from the `ShortDate` is less than or equal to the current year.
   - If the condition is true (meaning the date is in the current year or earlier), it returns `1`.
   - If the condition is false (meaning the date is in a future year), it returns `0`.

4. **Outcome**: 
   - The result is a column where each row will have a value of `1` if the date is in the current year or earlier, and `0` if the date is in a future year.
   - This flag can be useful for filtering or analyzing data based on whether dates are relevant to the current year.

In summary, this DAX expression helps to easily identify and flag dates that are either in the current year or earlier, allowing for better data analysis and reporting.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table captures and categorizes various metrics related to hours worked and their corresponding percentage contributions, enabling businesses to analyze workforce productivity and resource allocation effectively. This table supports decision-making by providing insights into labor efficiency and performance trends across different departments or projects.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column contains string representations of hours worked alongside their corresponding percentage contributions, facilitating the analysis of employee productivity and workload distribution. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string representations of various metrics related to hours worked and their corresponding percentages, facilitating data enrichment and analysis within the 'Hours_nd_Percentage' table. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column contains string values that represent the sequence or ranking of hours and percentage data, facilitating the organization and analysis of time-related metrics within the 'Hours_nd_Percentage' table. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes and organizes various codes used across the business, facilitating data consistency and integrity. It includes attributes for code identification, qualification criteria, and sorting order, enabling efficient data retrieval and analysis in reporting and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers used to categorize and enrich data within the lookup table, facilitating efficient data retrieval and integration. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Code' table indicates the qualification status of records, serving as a numerical identifier to categorize data for enrichment processes. |
| `Sort` | `string` | The 'Sort' column in the 'lkp_Code' table contains string values that determine the order in which records are displayed or processed, facilitating organized data retrieval and presentation. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics. This table enhances data integrity and usability by standardizing employee information for analysis and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column (string) uniquely identifies each employee in the 'lkp_fltr_Employee' table, facilitating data enrichment and integration processes. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, facilitating the enrichment of data by providing identifiable references for each individual within the employee lookup table. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, providing essential details such as project identifiers, qualification metrics, billing amounts, and associated groups, enabling effective project management and financial analysis within the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the billing amount associated with each project, facilitating financial tracking and reporting. |
| `Group` | `string` | The 'Group' column in the 'lkp_Project' table categorizes projects into distinct classifications for enhanced data organization and retrieval. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for enriching data related to specific initiatives within the organization. |
| `Qualify` | `int64` | The 'Qualify' column (int64) in the 'lkp_Project' table indicates the qualification status of a project, with integer values representing different levels of qualification criteria met. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing their attributes such as billable status and external classifications, which aids in resource allocation and financial reporting. This table is essential for ensuring accurate tracking and management of units across various departments and projects.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column (int64) in the 'lkp_Unit' table indicates the department responsible for billable activities, facilitating accurate tracking and reporting of revenue-generating units. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column identifies the specific organizational unit within the data enrichment process, facilitating the categorization and management of related data. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column in the 'lkp_Unit' table stores string values that represent external types associated with ownership subclasses, facilitating enriched data integration and classification. |
| `Unit` | `string` | The 'Unit' column contains string values representing the various measurement units used to standardize and categorize data within the lkp_Unit reference table. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table provides a comprehensive overview of employees who have not submitted their timesheets, detailing the relevant time period, employee and manager information, and contract specifics. This table is essential for HR and management to identify compliance issues, ensure accurate payroll processing, and enhance workforce accountability.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | The 'ActualHours' column represents the total number of hours worked by an employee, recorded as a double to allow for precise decimal values, within the context of identifying and addressing missing timesheet entries. |
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column represents the date and time when an employee's contract is scheduled to conclude, providing essential information for tracking contract durations within the context of missing timesheet records. |
| `ContractHours` | `int64` | The 'ContractHours' column represents the total number of hours contracted for a specific employee or project, stored as an integer, to facilitate the identification of discrepancies in timesheet submissions. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for tracking timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of contracts for employees who have missing timesheets, providing essential context for identifying compliance and payroll issues. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet discrepancies. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the full names of employees whose timesheets are missing, facilitating the identification and resolution of timesheet discrepancies. |
| `Hours_` | `double` | The 'Hours_' column (double) in the 'vw_missing_timesheet' table represents the total number of hours recorded for each employee's timesheet entry, facilitating the identification of discrepancies in time reporting. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating tracking and auditing of data entry processes. |
| `ManagerID` | `string` | The 'ManagerID' column (string) in the 'vw_missing_timesheet' table identifies the unique identifier of the manager responsible for overseeing the employee associated with the missing timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column contains the names of managers responsible for overseeing employees who have missing timesheets, facilitating accountability and follow-up actions. |
| `MissingHours` | `double` | The 'MissingHours' column (double) in the 'vw_missing_timesheet' table quantifies the total number of hours not recorded in timesheets, providing insights into potential discrepancies in employee work hour reporting. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each entry in the missing timesheet records, facilitating the tracking and management of time allocation across various initiatives. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a unique identifier for each project, facilitating the tracking and management of timesheet entries associated with specific projects in the 'vw_missing_timesheet' table. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true signifying rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet data is complete and ready for reporting purposes. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales, recorded as a double-precision floating-point number, within the context of tracking missing timesheet entries for accurate financial reporting. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value of sales transactions, stored as a double data type, to facilitate accurate financial analysis and reporting within the 'vw_missing_timesheet' table. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with missing timesheet entries, facilitating timely follow-up and resolution of discrepancies. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific organizational subunit associated with each entry in the missing timesheet records, facilitating targeted follow-up and resolution of timesheet discrepancies. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for timesheets, facilitating the tracking and management of employee work hours within the 'vw_missing_timesheet' table. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the work activities recorded in the timesheet, facilitating better understanding and analysis of employee time allocation. |
| `Week_` | `int64` | The 'Week_' column (int64) in the 'vw_missing_timesheet' table represents the numerical week of the year associated with each timesheet entry, facilitating the identification and analysis of missing timesheet submissions by week. |
| `Year_` | `int64` | Column Description: The 'Year_' column (int64) represents the calendar year associated with each entry in the 'vw_missing_timesheet' table, facilitating the analysis of timesheet data over time. |

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the `lkp_Unit` table for the column `BillableDep`. This is done using the `RELATED` function, which pulls in data from a related table based on existing relationships in your data model.

2. **Handle Blank Values**: If the related `BillableDep` value is blank (meaning there is no corresponding entry in the `lkp_Unit` table), the expression returns a value of `1`. This indicates that, in the absence of specific information, the default assumption is that the item is billable.

3. **Check for Non-Zero Values**: If the related `BillableDep` value is not blank, the code then checks if this value is not equal to zero. If it is not zero, it again returns `1`, indicating that the item is considered billable.

4. **Return Zero for Zero Values**: If the related `BillableDep` value is zero, the expression returns `0`, indicating that the item is not billable.

### Summary:
In summary, this DAX expression effectively categorizes items as billable or not based on the `BillableDep` value from the related `lkp_Unit` table. It assigns a value of `1` (billable) if there is no related data or if the related value is non-zero, and it assigns a value of `0` (not billable) if the related value is zero. This helps in identifying which items can be billed based on their associated data.

**`CC_ActiveEmployees`** (`string`)

- **Description:** The 'CC_ActiveEmployees' column contains a string representation of the active employees in the company, used to identify those who may be missing timesheet submissions in the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
IF(vw_missing_timesheet[ShortDate] >= vw_missing_timesheet[ContractStartDate] && 
			      (vw_missing_timesheet[ShortDate] <= vw_missing_timesheet[ContractEndDate] || ISBLANK(vw_missing_timesheet[ContractEndDate]))
			        , 1
			        , 0
			    )
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `CC_ActiveEmployees` in a data model, specifically within a table named `vw_missing_timesheet`. Here's a breakdown of what this code does in simple business terms:

1. **Purpose**: The calculated column is designed to identify whether an employee is currently active based on their contract dates.

2. **Conditions Checked**:
   - **Start Date**: The code first checks if the date in the column `ShortDate` (which likely represents a specific date being evaluated) is on or after the employee's `ContractStartDate`. This means the employee's contract must have started before or on this date for them to be considered active.
   - **End Date**: Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. This means the employee's contract must still be valid on this date. However, if the `ContractEndDate` is blank (meaning the employee has no specified end date), the employee is still considered active.

3. **Output**:
   - If both conditions are met (the date is within the contract period or the end date is blank), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns a value of `0`, indicating that the employee is not active.

### Summary:
In summary, this DAX expression effectively flags employees as "active" (1) or "not active" (0) based on whether the date being evaluated falls within their contract period. This helps in analyzing employee status and managing workforce data efficiently.

**`IncompleteFlag`** (`string`)

- **Description:** The 'IncompleteFlag' column indicates whether a timesheet entry is incomplete, using a string value to signify its status for data enrichment and analysis.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a calculated column called `IncompleteFlag` in a data model, specifically within a table named `vw_missing_timesheet`. Here's a breakdown of what this expression does in simple business terms:

### Explanation of the DAX Expression:

- **Condition Check**: The expression checks the value in the `MissingHours` column of the `vw_missing_timesheet` table.
- **Logic**: 
  - If the value of `MissingHours` is **less than 0**, it means that there is an issue (for example, it could indicate that there are negative hours recorded, which typically shouldn't happen).
  - In this case, the expression returns **1**, which can be interpreted as a flag indicating that there is an incomplete or problematic entry.
  - If the value of `MissingHours` is **0 or greater**, the expression returns **0**, indicating that there is no issue with the hours recorded.

### What It Achieves:

- **Flagging Incomplete Entries**: The `IncompleteFlag` column will help identify records where there is a discrepancy or error in the `MissingHours` data. 
- **Data Quality Control**: By flagging these entries, the business can easily filter or analyze records that may require further investigation or correction.
- **Simplified Reporting**: This flag can be used in reports or dashboards to quickly highlight issues related to timesheet entries, allowing for better management of employee hours and ensuring accurate payroll processing.

In summary, this DAX expression is a simple yet effective way to identify and flag records with negative missing hours, helping maintain data integrity and support operational decision-making.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the 'lkp_Unit' table for the current row. Specifically, it looks for the 'Unit' field in that related table.

2. **Handle Missing Data**: 
   - If there is no related value (meaning the 'Unit' field is blank or missing), the code will return the text "Unknown". This is a way to handle situations where the expected data is not available, ensuring that the column does not end up with a blank value.
   - If there is a related value (meaning the 'Unit' field has data), it will return that value.

3. **Purpose**: The overall purpose of this calculated column is to provide a clear and user-friendly representation of the 'Unit' information. If the unit is known, it shows the actual unit name; if it’s not known, it clearly indicates that with the word "Unknown".

In summary, this DAX expression ensures that every row in the 'MAIN_UNIT' column either displays the corresponding unit name from the related table or indicates that the unit is unknown, thus improving data clarity and usability.

**`YearWeek`** (`string`)

- **Description:** The 'YearWeek' column represents the year and week number in a string format, facilitating the identification and analysis of timesheet data for specific weeks within a given year.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'YearWeek' in a data model, and it combines two pieces of information: the year and the week number of a specific date, which is represented by the column `[ContractEndDate]`.

Here's a breakdown of what this code does in simple business terms:

1. **Extracting the Year**: The function `YEAR([ContractEndDate])` takes the date from the `[ContractEndDate]` column and extracts the year from it. For example, if the date is December 15, 2023, this part would return `2023`.

2. **Calculating the Week Number**: The function `WEEKNUM([ContractEndDate])` calculates which week of the year the date falls into. For instance, if the date is December 15, 2023, it might return `50`, indicating that it is the 50th week of the year.

3. **Formatting the Week Number**: The `FORMAT(..., "00")` function ensures that the week number is always displayed as a two-digit number. So, if the week number is `5`, it will be formatted as `05`.

4. **Combining Year and Week Number**: The `& "-" &` part of the code concatenates (joins) the year and the formatted week number with a hyphen in between. So, if the year is `2023` and the week number is `50`, the final result will be `2023-50`.

### Summary:
The overall result of this DAX expression is a string that represents the year and the week number of the `[ContractEndDate]` in the format "YYYY-WW". For example, if the contract ends on December 15, 2023, the calculated column 'YearWeek' would show `2023-50`. This can be useful for reporting and analysis, allowing users to easily group or filter data by year and week.

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

1. **Purpose**: This measure counts the number of unique consultants (employees) who have recorded negative hours in their timesheets. Negative hours typically indicate that there was an error in reporting or that hours were deducted for some reason.

2. **Components**:
   - **`CALCULATE`**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data being counted.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the unique names of employees from the `vw_missing_timesheet` table. It ensures that each employee is only counted once, even if they have multiple entries with negative hours.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This filter condition specifies that only those records where the `MissingHours` value is less than zero should be considered in the count. This means that only employees with negative hours will be counted.

3. **Outcome**: The result of this measure is a single number that tells you how many different consultants have reported negative hours in their timesheets. This information can be useful for identifying issues in time reporting, understanding workload discrepancies, or addressing potential errors in timesheet submissions.

In summary, `Count_Negative_Consultant` helps businesses track and manage consultants who may have discrepancies in their reported hours, allowing for better oversight and corrective actions if necessary.

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
   - **DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID])**: This part counts the number of unique Employee IDs from the 'vw_missing_timesheet' table. Essentially, it tells us how many different employees are involved.
   - **'vw_missing_timesheet'[MissingHours] < 0**: This is a filter condition applied within the CALCULATE function. It specifies that we only want to consider records where the MissingHours value is less than zero, meaning we are looking for instances where employees have negative missing hours.

3. **Outcome**: The measure ultimately provides a count of how many distinct employees have negative missing hours recorded in the timesheet. This could indicate issues such as data entry errors or discrepancies in time reporting, which might need to be addressed by management or HR.

In summary, **CountNegativeMissingHours** helps organizations identify and quantify employees with negative missing hours, allowing them to investigate potential problems in time tracking or reporting.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the number of active employees who are missing timesheet hours in a specific context. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The code works with a data table called `vw_missing_timesheet`, which likely contains information about employees, their hours worked, and their contractual hours.

2. **Summarization**: The code first creates a summarized view of the data. It groups the information by:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a more specific division within the unit)

   For each group, it calculates two key pieces of information:
   - **MissingHours**: This is calculated by taking the total hours worked (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contractual hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has worked fewer hours than expected.
   - **MinActiveEmp**: This captures the minimum value of active employees (`MIN(vw_missing_timesheet[CC_ActiveEmployees])`) for that group. If this value is 1, it means there is at least one active employee in that group.

3. **Filtering**: After summarizing the data, the code filters this summarized data to find only those groups where:
   - There is at least one active employee (`[MinActiveEmp] = 1`).
   - The calculated missing hours are negative (`[MissingHours] < 0`), indicating that the employee has not logged enough hours.

4. **Counting**: Finally, the code counts the number of rows in the filtered result, which represents the number of active employees who are missing timesheet hours.

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

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. This variable filters a summarized table (`vw_missing_timesheet`) to focus on employees who are currently active. It does this by checking a specific condition: it only includes employees who have a minimum active status (`MinActiveEmp`) of 1.

2. **Calculate Missing Hours**: For each employee, the code calculates "MissingHours" by taking the total hours they have recorded (`SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contract hours they are supposed to work (`MAX(vw_missing_timesheet[ContractHours])`). If this calculation results in a negative number, it indicates that the employee has worked fewer hours than expected.

3. **Filter for Relevant Data**: The `FILTER` function ensures that only those employees who are active and have negative missing hours are included in the final calculation. This means the measure is specifically looking for employees who are underperforming in terms of hours worked.

4. **Sum Up Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered list of active employees and sums up their "MissingHours." This gives a total of all the negative hours across all active employees, highlighting the overall shortfall in hours worked.

In summary, this DAX measure calculates the total number of hours that active employees are missing (i.e., not working enough hours compared to their contract) by summing up the negative differences between their actual hours worked and their expected hours. This can help management identify potential issues with employee attendance or workload management.

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

1. **Data Source**: The calculation is based on a table called `vw_missing_timesheet`, which likely contains records of timesheets for employees, including their contract hours and the week associated with each entry.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key pieces of information:
   - **Week**: This represents the specific week for which we are calculating contract hours.
   - **EmployeeID**: This identifies each employee.

3. **Calculating Maximum Contract Hours**: For each unique combination of week and employee, the code calculates the maximum contract hours from the `ContractHours` column. This means that if an employee has multiple entries for the same week, it will only consider the highest number of contract hours they are supposed to work for that week.

4. **Summing Up Contract Hours**: The `SUMX` function then takes the results from the `SUMMARIZE` step and adds up all the maximum contract hours calculated for each employee across all weeks. 

### What It Achieves:
The overall result of this DAX measure is the total expected contract hours for all employees, aggregated by week. This is useful for understanding how many hours are expected to be worked by employees in a given week, which can help in planning, resource allocation, and ensuring that staffing levels meet contractual obligations.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the difference between two values related to hours worked and contracted hours for employees or contractors. Here’s a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression retrieves the maximum value of "ContractHours" from the `vw_missing_timesheet` table. "ContractHours" represents the total number of hours that an employee or contractor is supposed to work according to their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part retrieves the maximum value of "Hours_" from the same table. "Hours_" represents the actual hours that the employee or contractor has logged or reported working.

3. **Overall Calculation**: The expression subtracts the maximum actual hours worked (Hours_) from the maximum contracted hours (ContractHours). 

### What It Achieves:
- The result of this calculation gives you the maximum number of hours that are missing or unaccounted for. In other words, it tells you how many hours an employee or contractor has not worked compared to what they were contracted to work.

### Business Implication:
- This measure helps in identifying gaps in hours worked versus hours expected, which can be crucial for payroll, project management, and ensuring that resources are being utilized effectively. It can also highlight potential issues with time reporting or resource allocation.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates the percentage of completeness regarding missing hours in relation to expected contract hours. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part of the code adds up all the missing hours recorded in the `vw_missing_timesheet` table. Essentially, it tells us how many hours are missing from the timesheets.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that are expected to be worked in a week according to the contract. It serves as a benchmark for comparison.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the code calculates the difference between the total missing hours and the expected contract hours. If the result is positive, it indicates that the missing hours exceed what was expected; if negative, it means the missing hours are less than expected.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: This function divides the result of the previous calculation (the difference between missing hours and expected hours) by the expected contract hours. The `DIVIDE` function is used instead of a simple division to handle cases where the denominator (expected hours) might be zero, in which case it returns 0 instead of an error.

### What It Achieves:
The overall measure, `PercentageCompleteness`, calculates how much the missing hours deviate from the expected contract hours as a percentage. 

- If the result is positive, it indicates that there are more missing hours than expected, suggesting a shortfall in timesheet submissions.
- If the result is negative, it indicates that the missing hours are less than expected, which could imply better compliance with timesheet submissions.

In summary, this measure helps the business understand the extent of timesheet compliance by comparing actual missing hours against what is expected, expressed as a percentage. This can be useful for identifying areas where improvements are needed in timesheet reporting.

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

1. **Data Source**: The measure uses a data source called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including information about whether their submissions are complete or incomplete.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by three key attributes:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This identifies each employee.
   - `MAIN_UNIT`: This likely refers to the department or unit within the organization that the employee belongs to.

   While grouping, it also creates a new column called `IncompleteFlag`, which captures the maximum value of the `IncompleteFlag` for each combination of week, employee, and unit. The `IncompleteFlag` probably indicates whether an employee has submitted their timesheet (with a value of 0 for complete and 1 for incomplete).

3. **Calculating the Measure**: After summarizing the data, the `SUMX` function iterates over each group created by `SUMMARIZE`. For each group, it sums up the `IncompleteFlag` values. This means it counts how many times an employee has an incomplete timesheet for each week and unit.

4. **Final Outcome**: The final result of this measure is the total count of incomplete timesheet submissions across all employees and units for the specified weeks. This measure helps the business understand how many employees have not submitted their timesheets, which can be crucial for payroll processing and ensuring compliance with reporting requirements.

In summary, `UniqueEmployeesPerUnit` calculates the total number of incomplete timesheet submissions by employees, grouped by week and unit, helping the organization track and manage timesheet compliance effectively.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This table enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet entry has been approved (true) or not (false). |
| `Client` | `string` | The 'Client' column in the 'vw_Timesheet' table identifies the specific client associated with each timesheet entry, facilitating accurate billing and project tracking. |
| `Code2` | `string` | The 'Code2' column in the 'vw_Timesheet' table represents a secondary identifier or classification code associated with each timesheet entry, facilitating enhanced data categorization and reporting. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract is set to expire, providing essential information for managing workforce planning and contract renewals within the timesheet data. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for timesheet entries and payroll calculations. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the contract associated with each timesheet entry, providing essential context for evaluating project progress and resource allocation. |
| `ContractStatusTodayPBI` | `string` | Column Description: This column indicates the current status of contracts as of today, providing a real-time snapshot for analysis within the timesheet data. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the date and time when a contract was transferred, providing essential context for timesheet entries related to contract changes. |
| `CostAmount` | `double` | The 'CostAmount' column represents the total monetary cost associated with the recorded time entries in the 'vw_Timesheet' table, expressed as a double-precision floating-point number for precise financial calculations. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the cost of labor for each entry in the timesheet, expressed as a double to allow for precise financial calculations. |
| `Currency` | `string` | The 'Currency' column in the 'vw_Timesheet' table specifies the type of currency used for financial entries, facilitating accurate monetary calculations and reporting. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the individual or entity responsible for payment, facilitating the tracking and management of outstanding invoices within the timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) in the 'vw_Timesheet' table uniquely identifies each employee, facilitating the association of timesheet entries with the corresponding personnel for accurate tracking and reporting of work hours. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column in the 'vw_Timesheet' table stores the unique identifier and name of each employee, facilitating the association of timesheet entries with specific personnel for accurate reporting and analysis. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee in the 'vw_Timesheet' table, facilitating the association of timesheet entries with specific personnel for accurate tracking and reporting. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees associated with each timesheet entry, facilitating the identification and tracking of individual work hours and contributions. |
| `Employer` | `string` | The 'Employer' column in the 'vw_Timesheet' table captures the name of the organization for which the employee is working, facilitating accurate tracking of labor costs and resource allocation. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the employer associated with each timesheet entry, facilitating accurate payroll and reporting processes. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to each timesheet entry, providing insights that enhance the understanding of the recorded hours and activities. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year associated with each timesheet entry, facilitating financial reporting and analysis. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee during a specified time period, facilitating accurate payroll and project tracking. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) in the 'vw_Timesheet' table represents the total number of hours worked by an employee in a given week, facilitating analysis of labor allocation and productivity. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or holiday hours, to facilitate accurate reporting and analysis of employee time worked. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, facilitating accurate reporting and analysis of employee work hours. |
| `Index` | `int64` | The 'Index' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for each timesheet entry, facilitating efficient data retrieval and organization. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the timesheet data was ingested into the system, facilitating accurate tracking and auditing of data entry. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, facilitating the tracking and association of timesheet entries with specific projects. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) in the 'vw_Timesheet' table serves as a unique identifier for project managers, facilitating the association of timesheet entries with their respective project management records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Management (IPM) manager responsible for overseeing project-related timesheet entries, facilitating accountability and resource management. |
| `IPMIDName` | `string` | The 'IPMIDName' column in the 'vw_Timesheet' table stores the names of individuals associated with specific IPM IDs, facilitating the tracking and management of timesheet entries linked to those identifiers. |
| `JobTitle` | `string` | The 'JobTitle' column in the 'vw_Timesheet' table captures the specific role or position of an employee, providing essential context for analyzing timesheet data and resource allocation. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier for linking timesheet records to related datasets, facilitating data integration and analysis. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier of the manager responsible for overseeing the timesheet entries, facilitating accountability and reporting within the organization. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, facilitating the identification of supervisory oversight for reported hours. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) in the 'vw_Timesheet' table identifies the unique key associated with the manager responsible for overseeing the timesheet entries. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct supervisor, facilitating the identification of managerial oversight for timesheet entries. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column stores string representations of work booking details related to overtime hours worked, facilitating the tracking and analysis of employee time allocation in the vw_Timesheet table. |
| `Project` | `string` | The 'Project' column in the 'vw_Timesheet' table identifies the specific project associated with each recorded time entry, facilitating accurate tracking and reporting of resource allocation and project costs. |
| `ProjectCode` | `string` | The 'ProjectCode' column in the 'vw_Timesheet' table uniquely identifies the specific project associated with each timesheet entry, facilitating accurate tracking and reporting of time spent on various projects. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string that identifies the specific project associated with each timesheet entry, facilitating detailed tracking and reporting of time spent on various projects. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) in the 'vw_Timesheet' table uniquely identifies the associated project profile for each timesheet entry, facilitating efficient tracking and reporting of project-related labor costs. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, enabling better analysis of resource allocation and project management. |
| `Rejected` | `boolean` | Indicates whether a timesheet entry has been rejected, with a value of true representing rejection and false indicating acceptance. |
| `ReportReady` | `boolean` | Indicates whether the timesheet is ready for reporting, with a value of true signifying readiness and false indicating further processing is required. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated from sales transactions, recorded as a double to accommodate precise financial calculations within the context of timesheet data analysis. |
| `SalesPrice` | `double` | The 'SalesPrice' column represents the monetary value assigned to each sale, stored as a double to accommodate precise pricing calculations within the timesheet data context. |
| `Status` | `string` | The 'Status' column indicates the current approval state of each timesheet entry, reflecting whether it is pending, approved, or rejected. |
| `Taxed` | `boolean` | Indicates whether the hours recorded in the timesheet are subject to taxation. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet entry, enabling better resource allocation and management. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column stores unique identifiers for each timesheet entry, facilitating the tracking and management of employee work hours and project allocations. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column captures the specific date and time when the timesheet entry was recorded, facilitating accurate tracking and reporting of employee work hours. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column contains detailed textual descriptions of the tasks and activities recorded in the timesheet, providing context and insights into the work performed during the reporting period. |
| `Unit` | `string` | The 'Unit' column in the 'vw_Timesheet' table represents the measurement unit associated with the recorded time entries, facilitating accurate reporting and analysis of work hours. |
| `UnitCode` | `string` | The 'UnitCode' column in the 'vw_Timesheet' table represents a unique identifier for the organizational unit associated with each timesheet entry, facilitating accurate tracking and reporting of labor costs by department. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and data management within the timesheet records. |
| `YearWeek` | `string` | The 'YearWeek' column represents the year and week number in a string format, facilitating time-based analysis and reporting of timesheet entries within the vw_Timesheet table. |

##### Calculated Columns

**`Approved_With_V_Z`** (`string`)

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheets, specifying whether they have been approved with a specific validation process or criteria denoted by 'V_Z'.
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

1. **Purpose**: The calculated column is designed to determine whether a timesheet is considered "approved" based on specific criteria.

2. **Conditions**: The code checks two main conditions:
   - **First Condition**: It checks if the `Approved` column has a value of `TRUE`. This means that if the timesheet is marked as approved, it meets the criteria.
   - **Second Condition**: It checks if the `TimesheetCode` is either "V" or "Z". This means that if the timesheet has a code of "V" or "Z", it also meets the criteria for being considered approved.

3. **Output**: 
   - If either of the conditions is met (the timesheet is approved or the code is "V" or "Z"), the calculated column will return a value of `1`. This indicates that the timesheet is approved according to the defined rules.
   - If neither condition is met, it will return a value of `0`, indicating that the timesheet is not approved.

In summary, this DAX expression effectively flags timesheets as approved (1) or not approved (0) based on whether they are explicitly marked as approved or have specific codes ("V" or "Z"). This can help in reporting and analysis by easily identifying which timesheets meet the approval criteria.

**`BillableDep`** (`string`)

- **Description:** The 'BillableDep' column identifies the department associated with billable hours recorded in the timesheet, facilitating accurate tracking and reporting of labor costs by department.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillableDep`. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the `BillableDep` column of the `lkp_Unit` table. This is done using the `RELATED` function, which pulls in data from a related table based on existing relationships in your data model.

2. **Handle Blank Values**: 
   - If the `BillableDep` value from the related table is **blank** (meaning there is no associated value), the expression returns **1**. This could indicate that the absence of a value is treated as a positive condition for whatever analysis is being performed.

3. **Check for Non-Zero Values**: 
   - If the `BillableDep` value is **not blank**, the code then checks if this value is **not equal to zero**. 
   - If it is not zero, the expression again returns **1**. This suggests that a non-zero value is also considered a positive condition.

4. **Return Zero for Zero Values**: 
   - If the `BillableDep` value is zero, the expression returns **0**. This indicates that a zero value is treated as a negative condition.

### Summary:
In summary, this DAX expression effectively categorizes the `BillableDep` values into two groups:
- It assigns a value of **1** if there is either no related `BillableDep` value (blank) or if the related value is non-zero.
- It assigns a value of **0** if the related `BillableDep` value is zero.

This calculated column can be useful for flagging records that are either billable or have a certain level of activity, helping in reporting or analysis related to billable hours or costs.

**`BillablePrj`** (`string`)

- **Description:** Column Description: The 'BillablePrj' column identifies the specific project associated with billable hours recorded in the timesheet, facilitating accurate client invoicing and project cost tracking.
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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `BillablePrj` in a data model, specifically for a table named `vw_Timesheet`. Here's a breakdown of what this expression does in simple business terms:

1. **Purpose**: The `BillablePrj` column is designed to identify whether a particular timesheet entry is billable or not. A billable entry is one that can be charged to a client or project.

2. **Conditions Checked**:
   - **Sales Price Check**: The first condition checks if the `SalesPrice` for the timesheet entry is greater than 0. If there is a positive sales price, it indicates that the entry is likely billable.
   - **Project Profile Code Check**: The second condition checks if the `ProjectProfileCode` is equal to 81. This specific code likely represents a type of project that is always considered billable.
   - **Project Name Search**: The third condition uses the `SEARCH` function to look for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it suggests that the project is related to customer success services, which are typically billable.

3. **Output**: 
   - If any of the three conditions are met (i.e., if the sales price is greater than 0, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of `1`, indicating that the timesheet entry is billable.
   - If none of these conditions are met, it returns a value of `0`, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or non-billable based on specific criteria related to sales price, project profile, and project name. This helps in tracking which work can be charged to clients, ensuring accurate billing and financial reporting.

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
- **DAX Explanation (Generated):** This DAX code snippet creates a new calculated column called `Employee_WeeklyHours` in a data model, specifically from a table named `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Combining Information**: The code takes two pieces of information from the `vw_Timesheet` table:
   - The name of the employee (`EmployeeName`).
   - The number of hours that employee works in a week (`HoursperWeek`).

2. **Formatting the Output**: It combines these two pieces of information into a single text string. The format of the output will be:
   - The employee's name followed by a hyphen and then the number of hours they work per week. For example, if the employee's name is "John Doe" and he works 40 hours per week, the result would be "John Doe - 40".

3. **Purpose**: This calculated column is useful for creating a clear and concise representation of each employee's weekly working hours alongside their name. It can be helpful for reporting, analysis, or visualization purposes, making it easier to see at a glance how many hours each employee is scheduled to work.

In summary, this DAX expression effectively creates a user-friendly label that combines employee names with their corresponding weekly hours, enhancing the readability of the data.

**`GroupCat`** (`string`)

- **Description:** The 'GroupCat' column categorizes timesheet entries into specific groups, facilitating streamlined reporting and analysis of time allocation across different projects or departments.
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

1. **Check for Project Group**: The first part of the code checks if there is a corresponding project group for each project listed in the 'vw_Timesheet' table. It does this by looking up the project in the 'lkp_Project' table using the `LOOKUPVALUE` function. If a project group is found (i.e., it is not blank), it retrieves that group.

2. **Assign Group Category**: 
   - If a project group is found, the 'GroupCat' column will be populated with the name of that project group.
   - If no project group is found (meaning the lookup returned blank), the code then checks if the project is billable by looking at the 'BillablePrj' column in the 'vw_Timesheet' table.
     - If 'BillablePrj' equals 1 (indicating that the project is billable), it assigns the value "Billable" to the 'GroupCat' column.
     - If 'BillablePrj' does not equal 1 (indicating that the project is not billable), it assigns the value "Other Unbillable".

### Summary:
In summary, this DAX expression categorizes each project in the 'vw_Timesheet' table into one of three groups:
- The specific project group from the 'lkp_Project' table if it exists.
- "Billable" if the project is billable but has no specific group.
- "Other Unbillable" if the project is not billable and has no specific group.

This categorization helps in analyzing projects based on their billing status and associated groups, which can be useful for reporting and decision-making.

**`IsContractActive`** (`string`)

- **Description:** Indicates whether the associated contract is currently active, with values reflecting the contract's status in relation to timesheet entries.
- **DAX Expression:**
```dax
IF(
			    ISBLANK('vw_Timesheet'[ContractEndDate]) || 'vw_Timesheet'[ContractEndDate] > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named **IsContractActive** in a data model, specifically for a table called **vw_Timesheet**. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The goal of this calculated column is to determine whether a contract is currently active or not.

2. **Conditions Checked**:
   - **ISBLANK('vw_Timesheet'[ContractEndDate])**: This part checks if the **ContractEndDate** field is empty (or blank). If it is blank, it implies that the contract does not have a specified end date, which typically means the contract is ongoing or active.
   - **'vw_Timesheet'[ContractEndDate] > TODAY()**: This part checks if the **ContractEndDate** is greater than today's date. If the end date is in the future, it means the contract is still active.

3. **Result**:
   - If either of the above conditions is true (the contract has no end date or the end date is in the future), the calculated column will return **"Active"**.
   - If neither condition is true (meaning there is a specified end date that is in the past), it will return **"Not Active"**.

### Summary:
In summary, this DAX expression effectively labels each contract in the **vw_Timesheet** table as either **"Active"** or **"Not Active"** based on whether the contract has an end date that is either missing or still in the future. This helps users quickly identify which contracts are currently valid and ongoing.

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
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column called `MAIN_UNIT`. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The code first checks if there is a related value in the `lkp_Unit` table for the current row. Specifically, it looks for the `Unit` field in that related table.

2. **Handle Missing Data**: 
   - If there is no related value (meaning the `Unit` field is blank), the code will return the text "Unknown". This is a way to handle situations where the expected data is missing, ensuring that the output is clear and informative.
   - If there is a related value (meaning the `Unit` field has data), it will return that value from the `lkp_Unit` table.

3. **Purpose**: The overall purpose of this calculated column is to provide a clear and user-friendly representation of the unit associated with each row of data. If the unit is not available, it explicitly states "Unknown" instead of leaving it blank, which helps in understanding and analyzing the data better.

In summary, this DAX expression ensures that every entry in the `MAIN_UNIT` column either shows the corresponding unit name or indicates that the unit is unknown, improving data clarity and usability.

**`MonthNumber`** (`string`)

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month (e.g., "01" for January, "02" for February) to facilitate time-based reporting and analysis of timesheet data.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to create a calculated column called 'MonthNumber' in a data model, typically within a tool like Power BI or Excel.

### What It Does:
- **Extracts the Month**: This expression takes a date value from the column `[TimesheetDate]` and extracts the month part of that date.
- **Returns a Number**: The result is a number that represents the month. For example:
  - If `[TimesheetDate]` is January 15, 2023, the expression will return `1`.
  - If `[TimesheetDate]` is July 22, 2023, it will return `7`.
  
### Purpose:
- **Simplifies Analysis**: By converting dates into month numbers, it makes it easier to analyze data by month. For instance, you can quickly group or filter timesheet data by month to see trends or totals.
- **Facilitates Reporting**: This calculated column can be used in reports to show monthly performance, track hours worked, or compare data across different months.

In summary, this DAX expression helps in transforming date information into a more usable format for analysis and reporting, specifically focusing on the month of each timesheet entry.

**`NR_EMP_COLUMN`** (`string`)

- **Description:** The 'NR_EMP_COLUMN' stores the unique employee identification numbers as strings, facilitating the association of timesheet entries with specific employees in the vw_Timesheet view.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

### In Simple Business Terms:

1. **Purpose**: This expression helps to determine how many different employees have recorded their timesheets. 

2. **What It Does**: 
   - It looks at the `EmployeeName` column in the `vw_Timesheet` table.
   - It counts only the unique names, meaning if the same employee appears multiple times in the timesheet, they are only counted once.

3. **Outcome**: The result of this calculation gives you a clear picture of how many individual employees are contributing to the timesheet data, which can be useful for understanding workforce participation, resource allocation, or project involvement.

In summary, this DAX expression is a straightforward way to quantify the distinct number of employees who have logged their time, providing valuable insights into employee engagement and activity.

**`Own-Sub-ExtT`** (`string`)

- **Description:** The 'Own-Sub-ExtT' column in the 'vw_Timesheet' table captures the classification of time entries as either owned, subcontracted, or external, facilitating accurate tracking and reporting of resource allocation.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used in a calculated column to retrieve a specific value from a related table. Here's a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a table that has a relationship with another table called `lkp_Unit`. The relationship is established based on a common key or identifier.

2. **Purpose**: The expression is designed to pull in data from the `lkp_Unit` table, specifically the column named `OWN-Sub-ExtT`. This means that for each row in the current table where this calculated column is being created, it will look up the corresponding value from the `lkp_Unit` table.

3. **Result**: The result of this expression is that each row in the current table will have a new column that contains the value of `OWN-Sub-ExtT` from the `lkp_Unit` table, based on the relationship defined between the two tables. This allows you to enrich your current data with additional information from the `lkp_Unit` table.

In summary, this DAX expression helps to enhance your dataset by bringing in relevant information from a related table, making it easier to analyze and report on the data with additional context.

**`QualifyPrj`** (`string`)

- **Description:** The 'QualifyPrj' column contains string values that categorize or qualify the projects associated with each timesheet entry, facilitating enhanced project management and reporting.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX code snippet is used to create a calculated column named 'QualifyPrj'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The calculated column 'QualifyPrj' is designed to determine a qualifying value for each row in the current table based on related data from another table called 'lkp_Project'.

2. **Checking for Blank Values**: The expression starts by checking if the 'Qualify' field from the 'lkp_Project' table is blank (i.e., it has no value) for the related project. This is done using the `ISBLANK` function combined with `RELATED`, which fetches the value from the related table.

3. **Conditional Logic**: 
   - If the 'Qualify' value is blank (meaning there is no qualification information available for that project), the expression returns a value of **1**. This could signify a default or placeholder value indicating that the project is considered qualified in the absence of specific information.
   - If the 'Qualify' value is not blank (meaning there is a qualification value available), it returns the actual value from the 'Qualify' field in the 'lkp_Project' table.

4. **Outcome**: The result of this calculation is that each row in the current table will have a 'QualifyPrj' value that either reflects the qualification status from the related project or defaults to 1 if no qualification information is available. This helps in ensuring that every project has a qualifying value, which can be useful for further analysis or reporting.

In summary, this DAX expression ensures that every project has a qualifying value, either by pulling it from a related table or defaulting to 1 when no information is available.

**`ReportingEntity`** (`string`)

- **Description:** The 'ReportingEntity' column identifies the organization or department responsible for the timesheet entries, facilitating accurate reporting and analysis of labor allocation.

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

1. **Purpose**: The calculated column determines whether a certain condition is met for each row in the data. It outputs either a `1` (true) or a `0` (false).

2. **Conditions Checked**:
   - The first condition checks if the column `[ReportReady]` is `TRUE`. This means that if the report is ready, the condition is satisfied.
   - The second condition checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means that if the timesheet code is one of these two specific codes, the condition is also satisfied.

3. **Output**:
   - If either of the conditions is true (the report is ready or the timesheet code is "V" or "Z"), the calculated column will return `1`.
   - If neither condition is met, it will return `0`.

### Summary:
In summary, this calculated column helps identify rows where either the report is ready or the timesheet code is specifically "V" or "Z". It effectively flags these rows with a `1` for further analysis or processing, while all other rows are flagged with a `0`. This can be useful for filtering or aggregating data based on readiness or specific timesheet codes.

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
In essence, `ContractStatus2` is designed to provide a clear output based on the status of a contract. If the contract is active, it shows the relevant hours difference; if not, it simply indicates that the contract is not active. This helps users quickly understand the status of contracts and their associated hours.

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
   - **Is the End Date Blank?**: The measure checks if this maximum end date is blank (i.e., there is no end date specified). If there is no end date, it implies that the contract is ongoing or has not been finalized.
   - **Is the End Date in the Future?**: If there is an end date, it checks whether this date is greater than today’s date. If the end date is in the future, it means the contract is still active.

3. **Return Status**:
   - If either of the above conditions is true (the end date is blank or it is in the future), the measure returns "Active". This indicates that the contract is currently valid and ongoing.
   - If neither condition is met (meaning the end date is not blank and is in the past), it returns "Not Active", indicating that the contract has ended.

### Summary:
In summary, this measure effectively categorizes contracts as "Active" or "Not Active" based on whether they have a future end date or no end date at all. This helps businesses quickly assess the status of contracts and manage their resources accordingly.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 10, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated starting from January 1st. 

So, when you combine these two functions, `WEEKNUM(TODAY())` effectively tells you which week of the year it is right now. 

For example, if today is October 10, 2023, this expression might return "41", indicating that it is the 41st week of the year.

### In Business Terms:
This measure helps businesses understand which week of the year they are currently in. This can be useful for reporting, planning, and analysis purposes, such as tracking weekly sales performance, monitoring project timelines, or aligning marketing campaigns with specific weeks of the year.

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
   - **`vw_Timesheet[Approved] = FALSE()`**: This condition filters the data to include only those timesheet entries where the `Approved` status is set to FALSE. In other words, it focuses on timesheets that have not been approved.

3. **Outcome**: The measure ultimately provides a count of distinct employees who have submitted timesheets that are still pending approval. This information can be useful for tracking outstanding approvals and managing workflow within the organization.

In summary, `Dax_EmpCount_Approved` helps businesses understand how many employees are waiting for their timesheets to be approved, which can aid in identifying bottlenecks in the approval process.

**`Dax_EmpCount_RReady`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[ReportReady] = FALSE()
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `Dax_EmpCount_RReady`. Here's a breakdown of what this measure does in simple business terms:

1. **Purpose**: This measure calculates the number of unique employees whose timesheets are not marked as "Report Ready."

2. **Components**:
   - **`CALCULATE`**: This function changes the context in which data is evaluated. It allows us to apply filters to our calculations.
   - **`DISTINCTCOUNT(vw_Timesheet[EmployeeName])`**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it tells us how many different employees are present in the data.
   - **`vw_Timesheet[ReportReady] = FALSE()`**: This condition filters the data to include only those records where the `ReportReady` field is set to FALSE. In other words, it focuses on timesheets that are not ready for reporting.

3. **What it achieves**: The measure ultimately provides a count of distinct employees who have timesheets that are still pending or not ready for reporting. This can be useful for management to identify how many employees still need to submit or finalize their timesheets before reports can be generated.

In summary, `Dax_EmpCount_RReady` helps businesses track the number of employees with incomplete timesheets, enabling better management of reporting processes.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheet entries in a dataset.

Here's a breakdown of what it does:

- **DISTINCTCOUNT**: This function counts the number of distinct (unique) values in a specified column. In this case, it is counting unique entries.
  
- **vw_Timesheet[EmployeeName]**: This refers to the column named `EmployeeName` in the `vw_Timesheet` table. This column contains the names of employees who have submitted timesheets.

### What It Achieves:
The measure `DISTINCT_COUNT_EMP` will return the total number of different employees who have logged their hours in the timesheet. For example, if three employees (Alice, Bob, and Charlie) have submitted timesheets, the result of this measure will be 3. If Alice submits multiple timesheets, she will still only be counted once.

### Business Implication:
This measure is useful for understanding workforce engagement and activity. It helps businesses track how many individual employees are actively participating in timesheet reporting, which can be important for project management, resource allocation, and overall workforce analysis.

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
- **DAX Explanation (Generated):** This DAX code snippet defines a measure called `Msr_HoursPercentage`, which calculates the percentage of hours worked by a specific group, project, or employee relative to the total hours worked across all selected categories.

Here’s a breakdown of what it does in simple business terms:

1. **Total Hours Calculation**: 
   - The variable `TotalValue` is created to calculate the total number of hours worked. It uses the `CALCULATE` function to sum up the hours from the `vw_Timesheet` table.
   - The `ALLSELECTED` function is used to ensure that the total hours are calculated based on the current selections made in the report for `GroupCat`, `Project`, and `EmployeeName`. This means it respects any filters applied by the user but ignores any other filters that might limit the data.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours worked by the current context (which could be a specific group, project, or employee) by dividing the sum of hours for that context (`SUM(vw_Timesheet[Hours])`) by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safer way to handle division in DAX because it can return a specified value (in this case, 0) if the denominator (`TotalValue`) is zero, preventing errors.

3. **Final Outcome**:
   - The final result of this measure is a percentage that shows how much of the total hours worked (based on the current selections) is attributed to the specific context being evaluated (like a specific employee or project). This helps in understanding the contribution of different parts of the organization to the overall hours worked.

In summary, `Msr_HoursPercentage` provides insights into how individual or grouped efforts contribute to total hours worked, allowing for better analysis of productivity and resource allocation.

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

1. **Purpose**: The measure calculates the total number of unique employees who have a "Valid" contract status as of today.

2. **Components**:
   - **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. It ensures that each employee is only counted once, even if they appear multiple times in the data.
   
   - **ALL(vw_Timesheet)**: This function removes any filters that might be applied to the `vw_Timesheet` table. By doing this, it ensures that the count of employees is not affected by any other filters that might be in place in the report or dashboard. Essentially, it looks at all the data in the `vw_Timesheet` table without any restrictions.

   - **vw_Timesheet[ContractStatusToday] = "Valid"**: This condition filters the data to only include employees whose contract status is marked as "Valid". This means that only employees who currently have a valid contract will be counted.

3. **Overall Calculation**: When you put it all together, this measure counts the number of unique employees who have a valid contract, ignoring any other filters that might be applied to the data. This is useful for getting a clear picture of how many employees are actively valid under their contracts at any given time.

In summary, the 'Static Total Employees' measure provides a straightforward count of all employees with valid contracts, ensuring that the count is accurate and not influenced by other data filters.

**`TotalActualHours`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression `SUM(vw_Timesheet[Hours])` is used to calculate the total number of hours recorded in a timesheet. 

Here's a breakdown of what it does:

- **SUM**: This function adds up all the values in a specified column.
- **vw_Timesheet**: This refers to a table (or view) that contains timesheet data. It likely includes various columns related to employee hours worked, projects, dates, etc.
- **[Hours]**: This is the specific column within the `vw_Timesheet` table that contains the number of hours each employee has logged.

In simple terms, this measure totals all the hours worked by employees as recorded in the timesheet. It helps businesses understand the total labor hours spent on various tasks or projects, which is essential for tracking productivity, managing resources, and calculating costs.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to calculate a measure called 'TotalContractedHours'. Let's break it down in simple business terms:

1. **SUMX Function**: This function is used to perform a calculation over a table and then sum up the results. In this case, it will iterate through a list of employees and calculate a value for each one.

2. **VALUES Function**: The `VALUES(vw_Timesheet[EmployeeName])` part creates a unique list of employee names from the 'vw_Timesheet' table. This means that if there are multiple entries for the same employee, each employee will only be counted once in the calculation.

3. **MAX Function**: For each employee in the unique list, the `MAX(vw_Timesheet[HoursPerWeek])` function finds the maximum number of hours that employee is contracted to work in a week. This is important because it ensures that if an employee has different entries for different weeks, only the highest contracted hours are considered.

4. **Putting It All Together**: The `SUMX` function then takes the maximum contracted hours for each employee (as calculated by the `MAX` function) and adds them all together. 

### What It Achieves:
In summary, this DAX measure calculates the total contracted hours for all employees by summing up the maximum hours each employee is contracted to work per week. This gives a clear picture of the total capacity of contracted work hours across the organization, ensuring that only the highest contracted hours for each employee are counted. This can be useful for workforce planning, budgeting, and understanding overall labor capacity.

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

4. **What It Achieves**: The result of this calculation gives you the **TotalHoursDeltaCompleteNr**, which shows the difference between what was actually worked and what was planned. 

   - If the result is positive, it means that more hours were worked than were contracted, indicating potential overwork or additional effort beyond what was planned.
   - If the result is negative, it means that fewer hours were worked than were contracted, suggesting that the project may be under-resourced or that tasks were completed more efficiently than expected.
   - If the result is zero, it indicates that the actual hours worked matched the contracted hours perfectly.

In summary, this measure helps businesses understand how actual work compares to what was planned, allowing for better resource management and project planning.

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

1. **Check for Blank Values**: The expression starts by checking if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Zero if Blank**: If the sum of hours is blank (meaning no hours have been tracked), the measure will return a value of **0**. This is important because it ensures that when there are no recorded hours, the measure reflects that by showing zero instead of leaving it blank.

3. **Return Total Hours if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the measure will return the actual total sum of hours tracked from the `vw_Timesheet` table.

### Summary:
In summary, the `TotalHoursTracked` measure calculates the total hours recorded in the timesheet. If no hours are recorded, it returns zero; otherwise, it returns the total hours tracked. This helps in reporting and analysis by providing a clear and meaningful value, ensuring that users can easily understand the amount of time tracked without encountering blank values.

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
   - If the total hours are blank (meaning no hours have been recorded), the measure will return the text "Empty". This indicates that there are no hours tracked for the selected context (like a specific employee, project, or time period).
   - If there are hours recorded (i.e., the total is not blank), it will return the actual sum of the hours tracked.

### Summary:
In essence, this measure helps to identify whether any hours have been logged in the timesheet. If no hours are logged, it clearly indicates that with the word "Empty". If hours are logged, it provides the total number of hours tracked. This is useful for reporting and analysis, as it allows users to quickly see if there is missing data in their timesheet records.

**`trackedDiff`**

- **DAX Expression:**
```dax
[TotalHoursTracked] - 
			    CALCULATE(
			        MAXX(DISTINCT(vw_Timesheet[HoursPerWeek]), vw_Timesheet[HoursPerWeek]),
			        ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])
			    )
```
- **DAX Explanation (Generated):** The DAX code snippet you provided calculates a measure called `trackedDiff`, which is designed to find the difference between two values related to hours tracked for employees. Here’s a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked for a specific employee or group of employees.

2. **Maximum Hours Per Week**: The next part of the calculation uses the `CALCULATE` function to determine the maximum number of hours that have been recorded in a week for the same employee. This is done by:
   - Using `MAXX` to find the highest value of `HoursPerWeek` from a distinct list of hours recorded in the `vw_Timesheet` table.
   - The `ALLEXCEPT` function is used to ensure that this calculation is only considering the data for the specific employee (identified by `EmployeeID`), ignoring any other filters that might be applied to the data.

3. **Calculating the Difference**: Finally, the measure subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. 

### What It Achieves:
The `trackedDiff` measure effectively shows how much the total hours tracked for an employee exceed the maximum hours they have recorded in any single week. 

- **Positive Value**: If the result is positive, it indicates that the employee has logged more hours overall than their highest weekly total, suggesting they may be working consistently more than their peak week.
  
- **Negative Value**: If the result is negative, it means that the total hours tracked are less than or equal to their maximum weekly hours, indicating that they have not exceeded their peak performance in terms of hours worked.

In summary, this measure helps in assessing employee workload and performance by comparing their total tracked hours against their maximum weekly hours, providing insights into their work patterns.

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

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been tracked for a specific context (like an employee or a project).

2. **Calculating the Maximum Hours Per Week**:
   - The `CALCULATE` function is used to modify the context in which the data is evaluated.
   - Inside `CALCULATE`, the `MAXX` function is used to find the maximum value of `HoursPerWeek` from a distinct list of hours recorded in the `vw_Timesheet` table. This means it looks at all the unique values of hours worked per week and finds the highest one.

3. **Removing Filters**:
   - The `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])` part ensures that any existing filters on the `HoursPerWeek` column are ignored. This allows the calculation to consider all possible values of hours worked per week, not just those that might be currently filtered in the report.

4. **Keeping Employee Context**:
   - The `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` function keeps the context of the `EmployeeID` while removing other filters. This means that the calculation will still be specific to each employee, but it won't be limited by any other filters that might be applied to the `vw_Timesheet` table.

5. **Final Calculation**:
   - The overall calculation subtracts the maximum hours per week (found in the previous steps) from the total hours tracked. 

### Summary:
In simple business terms, `trackedDiff2` calculates the difference between the total hours an employee has tracked and the maximum hours they have worked in a week, regardless of any specific week filters. This measure helps to identify how much more or less an employee has worked compared to their highest recorded weekly hours, providing insights into their workload and productivity.

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
- **DAX Explanation (Generated):** The DAX code snippet you provided is designed to calculate the total billable hours from a timesheet data source, specifically focusing on certain projects that qualify for billing. Here’s a breakdown of what it does in simple business terms:

1. **Variable Definition**: The code starts by defining a variable called `FilteredHours`. This variable will hold a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**: The `Filter` function is used to create this filtered dataset. It includes two main conditions:
   - **Condition 1**: The project qualifies for billing (`vw_Timesheet[QualifyPrj] = 1`) and is marked as a billable project (`vw_Timesheet[BillablePrj] = 1`).
   - **Condition 2**: Alternatively, it includes any projects that contain the phrase "Customer Success Services" in their name. This is checked using the `SEARCH` function, which looks for that specific text within the `vw_Timesheet[Project]` field.

3. **Calculating Total Hours**: After filtering the timesheet data based on the above criteria, the code uses the `SUMX` function to calculate the total hours worked. `SUMX` iterates over the `FilteredHours` variable and sums up the `Hours` field from the filtered dataset.

4. **Final Output**: The result of this measure, `UTI_TotalBillableHours`, is the total number of hours that meet the specified criteria for billable projects. This measure helps the business understand how many hours can be billed to clients, focusing on qualified projects and specific customer success services.

In summary, this DAX measure effectively calculates the total billable hours from timesheets, ensuring that only relevant projects are included in the calculation.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** The DAX code snippet you provided is used to create a measure called `UTI_TOTALHOURS`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: This measure calculates the total number of hours worked on specific projects that qualify under certain criteria.

2. **Components**:
   - **SUM(vw_Timesheet[Hours])**: This part of the code adds up all the values in the `Hours` column from the `vw_Timesheet` table. Essentially, it totals the hours recorded in timesheets.
   - **CALCULATE**: This function modifies the context in which the data is evaluated. It allows us to apply filters to the data before performing the calculation.
   - **vw_Timesheet[QualifyPrj] = 1**: This condition acts as a filter. It specifies that only the rows where the `QualifyPrj` column equals 1 should be included in the total. In other words, it focuses on projects that meet a certain qualification criterion.

3. **Outcome**: The measure `UTI_TOTALHOURS` will return the total hours worked on projects that are marked as qualifying (where `QualifyPrj` is 1). This is useful for reporting and analysis, as it helps stakeholders understand how many hours are being spent on projects that meet specific standards or requirements.

In summary, this DAX measure helps businesses track and analyze the total hours worked on eligible projects, providing insights into resource allocation and project management.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'UTILIZATION_New', which essentially assesses how effectively time is being utilized in a business context, particularly in relation to billable hours.

Here's a breakdown of what this code does in simple terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable captures the total number of hours that are billable to clients.
   - `_TotalHours`: This variable captures the total number of hours worked by employees.

2. **Return Logic**:
   - The code uses a series of checks to ensure that certain conditions are met before performing the calculation:
     - **Check for Active Contacts**: It first checks if there are any active contacts (represented by `[Msr_ActiveContacts]`). If there are no active contacts, the calculation does not proceed.
     - **Check for Total Hours**: Next, it checks if the total hours worked (`[UTI_TOTALHOURS]`) is not blank. If it is blank, the calculation does not proceed.
   
3. **Calculation of Utilization**:
   - If both checks pass, it calculates the utilization by dividing the total billable hours by the total hours worked. 
   - If the result of this division is blank (which can happen if total hours worked is zero), it returns 0 instead of a blank value. This ensures that the measure always provides a numerical output.

### Summary:
In summary, this DAX measure calculates the utilization rate by determining the proportion of billable hours to total hours worked, but only if there are active contacts and total hours are available. If any of the necessary data is missing, it ensures that the measure returns a meaningful value (0) instead of leaving it blank. This helps businesses understand how effectively their workforce is being utilized in terms of billable work.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

The following filters are applied to the entire report:

- Filter on `DimDate`.`CurrentYearFlag` (Type: Advanced, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data related to dates, specifically the `CurrentYearFlag` from the `DimDate` table.

### Breakdown of the Filter:

1. **Target Data**: The filter is applied to the `CurrentYearFlag` column in the `DimDate` table. This column typically indicates whether a date falls within the current year.

2. **Filter Condition**: The filter is set to include only those records where the `CurrentYearFlag` is equal to `1`. 

3. **Meaning of `CurrentYearFlag`**: 
   - A value of `1` in the `CurrentYearFlag` column signifies that the date is part of the current year.
   - Conversely, any date with a `CurrentYearFlag` of `0` would indicate that it is not in the current year.

### Summary in Business Terms:

When this filter is applied, it will **include only the dates from the `DimDate` table that are in the current year**. Any dates that are from previous years or future years will be **excluded** from the analysis. This allows users to focus solely on data relevant to the current year, which can be particularly useful for year-to-date reporting, current year performance analysis, or any other time-sensitive evaluations.)

### <a name="report-pages"></a>Report Pages

#### <a name="page-out-of-service"></a>Page: Out of Service

*Internal Name: `bcfb1ca820fd416f559b`, Ordinal: 6*

##### Page Level Filters

- Filter on `vw_Timesheet`.`ContractStatusTodayPBI` (Type: Categorical, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data from the `vw_Timesheet` view, particularly the `ContractStatusTodayPBI` column. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter is applied to the `ContractStatusTodayPBI` field within the `vw_Timesheet` dataset. This field likely indicates the current status of contracts related to timesheets.

2. **Inclusion Criteria**: The filter specifies that only records where the `ContractStatusTodayPBI` is equal to `'Not Valid'` should be included in the analysis. This means that any timesheet entries or contracts that have a status of "Not Valid" will be part of the data being analyzed.

3. **Exclusion Criteria**: Conversely, any records where the `ContractStatusTodayPBI` is anything other than `'Not Valid'` will be excluded from the analysis. This means that valid contracts or any other statuses will not be considered in the results.

In summary, when this filter is applied, the analysis will only show data related to contracts that are currently marked as "Not Valid," effectively filtering out all other contract statuses. This can help stakeholders focus on issues or areas that need attention regarding invalid contracts.)
- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `vw_Timesheet` view are included based on the `TimesheetCode` field. Here's a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter is applied to the `TimesheetCode` column in the `vw_Timesheet` view.

2. **Filter Logic**: The filter specifies that we want to **exclude** any records where the `TimesheetCode` starts with the letter "D". 

3. **Inclusion Criteria**: As a result, only records with `TimesheetCode` values that do **not** begin with "D" will be included in the analysis or report. 

### Summary:
- **Included**: Timesheet codes like "A123", "B456", "C789", etc. (anything that does not start with "D").
- **Excluded**: Timesheet codes like "D001", "D234", "D567", etc. (any code that starts with "D").

In essence, this filter helps focus the analysis on timesheets that are not categorized under any codes starting with "D", allowing for a clearer view of other categories.)

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
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which employee names are included in a report or visualization based on the `vw_Timesheet` data source. Let's break it down in simple business terms:

1. **Data Source**: The filter is applied to a view called `vw_Timesheet`, which likely contains timesheet data for employees.

2. **Target Field**: The specific field being filtered is `EmployeeName`, which represents the names of employees in the timesheet data.

3. **Filter Logic**: The filter is set up to **exclude** any records where the `EmployeeName` is `null`. In other words, it is looking for employee names that actually exist and are not empty or missing.

4. **What This Means**: When this filter is applied, only those employees who have a valid name (i.e., their name is not `null`) will be included in the results. Any records where the employee name is missing will be ignored or excluded from the analysis.

In summary, this filter ensures that the report only shows data for employees who have a name listed, thereby improving the quality and relevance of the information presented.)

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
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition JSON is designed to control which records from the `lkp_Unit` table are included in your analysis, specifically focusing on the `OWN-Sub-ExtT` column.

### Breakdown of the Filter:

1. **Source Table**: The filter is applied to the `lkp_Unit` table, referred to as `o` in the JSON.

2. **Filter Condition**: The key part of the filter is the condition specified in the `Where` section. It states that we want to **exclude** any records where the `OWN-Sub-ExtT` column has a value of `null`.

3. **What This Means**:
   - **Included Data**: Only records from the `lkp_Unit` table where the `OWN-Sub-ExtT` column has a value (i.e., it is not null) will be included in the analysis.
   - **Excluded Data**: Any records where `OWN-Sub-ExtT` is null will be filtered out and not considered in any reports or visualizations.

### Summary:
In simple terms, this filter ensures that you are only looking at records that have meaningful data in the `OWN-Sub-ExtT` column, effectively cleaning your dataset by removing any entries that lack this information. This helps in providing more accurate insights and analyses based on complete data.)

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

**pageNavigator**

- Type: `pageNavigator`
- Name: `01e97403abec6221fac1`
- Fields Used: _(None detected)_

**shape**

- Type: `shape`
- Name: `b504171bb603a1f77bd5`
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

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is designed to include specific values from the `TimesheetCode` column in the `vw_Timesheet` view. Here's a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Target Data**: The filter is applied to the `TimesheetCode` field within the `vw_Timesheet` dataset.
- **Included Values**: The filter specifies that only certain `TimesheetCode` values should be included in the analysis. The values that are included are:
  - `'int'`
  - `'ROEG'`
  - `'ROIG'`
  - `'RSEG'`
  - `'V'`
  - `'Z'`

### What This Means for Your Data:
- **Inclusion**: When this filter is applied, only records from the `vw_Timesheet` that have a `TimesheetCode` matching one of the specified values will be considered. This means that any records with a `TimesheetCode` that is not one of these six values will be excluded from the analysis.
- **Purpose**: This type of filtering is useful when you want to focus on specific types of timesheets that are relevant for a particular analysis or report, ensuring that the data you are looking at is relevant and targeted.

### Summary:
In summary, this filter is set up to include only those timesheets that have a `TimesheetCode` of `'int'`, `'ROEG'`, `'ROIG'`, `'RSEG'`, `'V'`, or `'Z'`, while excluding all other timesheet codes from the dataset.)

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
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which employee names are included in the data being analyzed from the `vw_Timesheet` view. Let's break it down in simple business terms:

1. **Source of Data**: The filter is applied to a data source called `vw_Timesheet`, which likely contains timesheet records for employees.

2. **Target Field**: The specific field being filtered is `EmployeeName`, which represents the names of employees in the timesheet data.

3. **Filter Logic**: The filter is set up to **exclude** any records where the `EmployeeName` is `null`. In other words, it is looking for employee names that are present and not empty.

4. **What This Means for Data**:
   - **Included Data**: Only records where `EmployeeName` has a valid name (i.e., it is not `null`) will be included in the analysis.
   - **Excluded Data**: Any records where `EmployeeName` is missing or not specified (i.e., `null`) will be excluded from the results.

In summary, this filter ensures that only employees with valid names are considered in the analysis, helping to maintain the quality and relevance of the data being reported on.)

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
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition JSON is designed to control which records from the `lkp_Unit` table are included in your analysis based on the `OWN-Sub-ExtT` column.

### Breakdown of the Filter:

1. **Target Entity**: The filter is applied to the `lkp_Unit` table, specifically focusing on the `OWN-Sub-ExtT` column.

2. **Filter Logic**: The filter uses a "Not" condition, which means it will exclude certain records based on the criteria defined.

3. **Condition Explained**:
   - The filter checks if the value in the `OWN-Sub-ExtT` column is **not** equal to `null`.
   - In simpler terms, this means that the filter will **include** only those records where the `OWN-Sub-ExtT` column has a value (i.e., it is not empty or missing).

### Summary:
When this filter is applied, it ensures that only records from the `lkp_Unit` table where the `OWN-Sub-ExtT` field contains a valid value (not `null`) are included in your Power BI report or analysis. Any records where `OWN-Sub-ExtT` is `null` will be excluded. This helps in focusing on relevant data that has meaningful entries in that specific column.)

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

- Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data from the `vw_missing_timesheet` view, particularly the `ContractStatusToday` field. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter is applied to the `ContractStatusToday` column within the `vw_missing_timesheet` dataset. This column likely indicates the current status of contracts related to timesheets.

2. **Inclusion Criteria**: The filter specifies that only records where `ContractStatusToday` is equal to `'Valid'` should be included in the analysis. This means that any timesheet records associated with contracts that are currently valid will be considered.

3. **Exclusion Criteria**: Conversely, any records where `ContractStatusToday` is not `'Valid'` (for example, statuses like `'Expired'`, `'Pending'`, or any other status) will be excluded from the analysis. 

In summary, when this filter is applied, the resulting data will only show timesheet records linked to contracts that are currently valid, allowing users to focus on active and relevant contracts while ignoring any that are not valid.)
- Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition JSON is designed to control which records from the `lkp_Unit` table are included based on the values in the `OWN-Sub-ExtT` column. Let's break it down into simpler terms:

### What the Filter Does:

1. **Target Data**: The filter is applied to the `OWN-Sub-ExtT` column within the `lkp_Unit` table.

2. **Inclusion Criteria**:
   - The filter **excludes** any records where the `OWN-Sub-ExtT` value is **null**. This means that if a record does not have a value in this column, it will not be included in the results.
   - The filter also **excludes** any records where the `OWN-Sub-ExtT` value is equal to **'SUB'**. So, if a record has 'SUB' in this column, it will also be left out.

### Summary of Included Data:
- The filter will include records from the `lkp_Unit` table where the `OWN-Sub-ExtT` column has a value that is **not null** and is **not equal to 'SUB'**. 

### Practical Implication:
In practical business terms, this means that when you apply this filter, you will only see records that have meaningful values in the `OWN-Sub-ExtT` column, specifically those that are neither empty (null) nor categorized as 'SUB'. This helps in focusing on relevant data while ignoring potentially unhelpful or irrelevant entries.)

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
  - Filter on `vw_missing_timesheet`.`EmployeeName` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which employee names are included in the data from the `vw_missing_timesheet` view. Let's break it down in simple business terms:

1. **Target Data**: The filter is applied to the `EmployeeName` field within the `vw_missing_timesheet` view. This view likely contains information about employees and their timesheet submissions.

2. **Filter Logic**: The filter specifies a condition that focuses on excluding certain data. Specifically, it states that we want to **exclude** any records where the `EmployeeName` is `null`. 

3. **What This Means**:
   - **Included Data**: The filter will include all employee names that have actual values (i.e., those that are not empty or missing).
   - **Excluded Data**: Any records where the `EmployeeName` is `null` (meaning there is no name provided for that employee) will be filtered out and not shown in the results.

In summary, when this filter is applied, you will only see records of employees who have a valid name listed in the `vw_missing_timesheet` view, effectively ignoring any entries that do not have an associated employee name.)

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
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `lkp_Unit` table are included based on the values in the `OWN-Sub-ExtT` column. Let's break it down in simple business terms:

### What the Filter Does:

1. **Target Data**: The filter is applied to the `OWN-Sub-ExtT` column in the `lkp_Unit` table.

2. **Inclusion Criteria**:
   - The filter **excludes** any records where the `OWN-Sub-ExtT` value is **null**. This means that if there is no value (or the value is missing) in this column, those records will not be included in the results.
   - The filter also **excludes** any records where the `OWN-Sub-ExtT` value is equal to **'SUB'**. This means that any record that specifically has 'SUB' in this column will also be left out.

### Summary of Included Data:
- The filter will include records from the `lkp_Unit` table where the `OWN-Sub-ExtT` column has a value that is **not null** and is **not equal to 'SUB'**. 

### Example:
- If you have records with the following `OWN-Sub-ExtT` values:
  - 'A'
  - 'B'
  - null
  - 'SUB'
  - 'C'

After applying this filter, only the records with 'A', 'B', and 'C' will be included in the results, while the records with null and 'SUB' will be excluded.

In essence, this filter helps to focus on relevant data by filtering out any incomplete or specific unwanted entries.)

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

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON specifies a filter that is applied to the `TimesheetCode` field in the `vw_Timesheet` view. Here’s a breakdown of what this filter does in simple business terms:

### What Data is Included:
The filter is designed to **include** only those records from the `vw_Timesheet` view where the `TimesheetCode` matches one of the following specific values:

1. **'int'**
2. **'ROEG'**
3. **'ROIG'**
4. **'RSEG'**
5. **'V'**
6. **'Z'**

### What Data is Excluded:
Any records in the `vw_Timesheet` view that have a `TimesheetCode` **not** matching one of the above values will be **excluded** from the results. This means that if a timesheet has a code that is different from 'int', 'ROEG', 'ROIG', 'RSEG', 'V', or 'Z', it will not be shown in the report or analysis.

### Summary:
In summary, this filter is used to narrow down the data to only those timesheets that have specific codes, allowing users to focus on a defined subset of timesheet entries that are relevant for their analysis or reporting needs.)
- Filter on `vw_Timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: This Power BI filter definition JSON is designed to filter data from a specific view called `vw_Timesheet`, focusing on a column named `ContractStatusToday`. Here’s a breakdown of what this filter does in simple business terms:

1. **Data Source**: The filter is applied to a data source identified as `vw_Timesheet`, which likely contains records related to timesheets, including various statuses of contracts.

2. **Target Column**: The filter specifically targets the column `ContractStatusToday`. This column presumably indicates the current status of contracts associated with the timesheet entries.

3. **Filter Condition**: The filter includes a condition that specifies which records to include based on the values in the `ContractStatusToday` column.

4. **Included Value**: The filter is set to include only those records where the `ContractStatusToday` is equal to `'Valid'`. This means that any timesheet entry with a contract status that is not `'Valid'` will be excluded from the results.

### Summary:
In summary, when this filter is applied, it will only show timesheet records where the contract status is currently valid. Any records with statuses other than `'Valid'` will be filtered out and not displayed in the report or analysis. This helps users focus on active and valid contracts, ensuring that the data they are analyzing is relevant and up-to-date.)
- Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Categorical, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data from the `vw_Timesheet` view, particularly the `Approved_With_V_Z` column. Let's break it down in simple business terms:

1. **Target Data**: The filter is applied to the `vw_Timesheet` view, specifically looking at the `Approved_With_V_Z` field. This field likely indicates whether a timesheet has been approved or not, with different values representing different statuses.

2. **Filter Condition**: The filter is set to include only those records where the `Approved_With_V_Z` value is equal to "0L". In this context, "0L" typically represents a specific status, which could mean "not approved" or "pending approval". 

3. **Inclusion/Exclusion**: 
   - **Included Data**: Only the records from the `vw_Timesheet` where `Approved_With_V_Z` is "0L" will be included in the analysis. This means you will see only those timesheets that are not approved.
   - **Excluded Data**: Any records where `Approved_With_V_Z` has a value other than "0L" (such as approved timesheets or those with different statuses) will be excluded from the results.

In summary, this filter is used to isolate and analyze only the timesheets that have not yet been approved, allowing users to focus on pending approvals within the dataset.)

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
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `lkp_Unit` table are included in your analysis based on the values in the `OWN-Sub-ExtT` column. Let's break it down into simpler terms:

### What the Filter Does:

1. **Target Data**: The filter is applied to the `OWN-Sub-ExtT` column in the `lkp_Unit` table.

2. **Inclusion Criteria**:
   - The filter **excludes** any records where the `OWN-Sub-ExtT` value is **null**. This means that if there is no value (or the value is missing) in this column, those records will not be included in the results.
   - The filter also **excludes** any records where the `OWN-Sub-ExtT` value is equal to **'SUB'**. So, if the value in this column is specifically 'SUB', those records will also be left out.

### Summary of Included Data:
- The filter will include records from the `lkp_Unit` table where the `OWN-Sub-ExtT` column has a value that is **not null** and is **not equal to 'SUB'**. 

### Example:
- If you have records with the following `OWN-Sub-ExtT` values:
  - `null` (excluded)
  - `'SUB'` (excluded)
  - `'ABC'` (included)
  - `'XYZ'` (included)

Only the records with values like `'ABC'` and `'XYZ'` will be included in your analysis, while those with `null` or `'SUB'` will be filtered out. 

This filter helps ensure that your analysis focuses on relevant data by removing potentially unhelpful or irrelevant entries.)

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

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON is used to specify which records from the `vw_Timesheet` view should be included based on the `TimesheetCode` field. Here's a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter is applied to the `TimesheetCode` column in the `vw_Timesheet` view. This means we are focusing on the codes that represent different types of timesheets.

2. **Included Values**: The filter specifies a list of specific `TimesheetCode` values that should be included in the results. The codes that are included are:
   - `'int'`
   - `'ROEG'`
   - `'ROIG'`
   - `'RSEG'`
   - `'V'`
   - `'Z'`

3. **Exclusion of Other Values**: Any `TimesheetCode` that is **not** one of the above values will be excluded from the results. This means that if a timesheet has a code other than those listed, it will not appear in the filtered data.

In summary, when this filter is applied, only timesheets with the specified codes will be shown, while all other timesheet codes will be ignored. This allows users to focus on specific types of timesheets that are relevant for their analysis or reporting.)
- Filter on `vw_Timesheet`.`RReady_With_V_Z` (Type: Categorical, Explanation: This Power BI filter definition is designed to control which records from the `vw_Timesheet` data view are included when analyzing the `RReady_With_V_Z` field. Here’s a breakdown of what this filter does in simple business terms:

1. **Source of Data**: The filter is applied to a data source identified as `vw_Timesheet`, which likely contains timesheet-related information.

2. **Target Field**: The specific field being filtered is `RReady_With_V_Z`. This field could represent a status or readiness indicator related to timesheets.

3. **Filter Condition**: The filter specifies a condition that focuses on the values of the `RReady_With_V_Z` field. It is looking for records where this field has a specific value.

4. **Included Value**: The filter includes only those records where the `RReady_With_V_Z` field is equal to `0L`. This likely indicates a specific status, such as "not ready" or "pending," depending on the context of the data.

5. **Exclusion of Other Values**: By applying this filter, any records where `RReady_With_V_Z` has values other than `0L` will be excluded from the analysis. This means that only the records that meet this specific condition will be considered.

In summary, this filter is set up to include only those timesheet records that are marked with a readiness status of `0L`, effectively filtering out all other statuses. This allows users to focus their analysis on a specific subset of the data that meets this criterion.)
- Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: This Power BI filter definition is designed to include only specific data from the `vw_Timesheet` view, specifically focusing on the `Approved_With_V_Z` column.

### Breakdown of the Filter:

1. **Source of Data**: The filter is applied to a data source named `vw_Timesheet`, which likely contains timesheet records.

2. **Target Column**: The filter specifically looks at the `Approved_With_V_Z` column within this data source.

3. **Condition**: The filter checks for a condition where the value in the `Approved_With_V_Z` column must equal `1L`. 

### What This Means in Business Terms:

- **Inclusion of Data**: The filter will **include** only those records from the `vw_Timesheet` where the `Approved_With_V_Z` column has a value of `1L`. This typically indicates that these timesheet entries have been approved under a certain condition or status.

- **Exclusion of Data**: Any records where the `Approved_With_V_Z` column does not equal `1L` will be **excluded** from the results. This means that only approved timesheet entries will be visible in the report or analysis.

### Summary:

In summary, this filter is used to focus on and analyze only the approved timesheet entries from the `vw_Timesheet` data source, ensuring that any unapproved entries are not considered in the analysis.)

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
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `lkp_Unit` table are included in your analysis based on the `OWN-Sub-ExtT` column.

### Breakdown of the Filter:

1. **Target Entity**: The filter is applied to the `lkp_Unit` table, specifically focusing on the `OWN-Sub-ExtT` column.

2. **Filter Logic**: The filter uses a "Not" condition, which means it is excluding certain records based on the value in the `OWN-Sub-ExtT` column.

3. **Condition Explained**: 
   - The filter checks if the value in the `OWN-Sub-ExtT` column is **not** equal to `null`.
   - In simpler terms, this means that the filter will **include** only those records where `OWN-Sub-ExtT` has a value (i.e., it is not empty or missing).

### Summary:

When this filter is applied, it will **exclude** any records from the `lkp_Unit` table where the `OWN-Sub-ExtT` column is `null`. Therefore, you will only see records that have a valid, non-null value in that column. This is useful for ensuring that your analysis focuses on complete data entries, avoiding any gaps that could skew your insights.)

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

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is designed to limit the data displayed in a report or visualization based on specific criteria related to the `TimesheetCode` field from the `vw_Timesheet` view.

### Breakdown of the Filter:

1. **Source of Data**: 
   - The filter is applied to a data source identified as `vw_Timesheet`, which is likely a view that aggregates timesheet data.

2. **Target Field**: 
   - The filter specifically targets the `TimesheetCode` column within this view.

3. **Inclusion Criteria**:
   - The filter specifies that only certain values of `TimesheetCode` should be included in the results. The values that are allowed (or included) by this filter are:
     - `'int'`
     - `'ROEG'`
     - `'ROIG'`
     - `'RSEG'`
     - `'V'`
     - `'Z'`

4. **Exclusion of Other Values**:
   - Any `TimesheetCode` that is **not** one of the specified values will be excluded from the results. This means that if a timesheet has a code that is different from the ones listed above, it will not appear in the report or visualization.

### Summary:
In simple terms, this filter is set up to show only the timesheets that have specific codes: `'int'`, `'ROEG'`, `'ROIG'`, `'RSEG'`, `'V'`, or `'Z'`. All other timesheet codes will be ignored, ensuring that the analysis focuses solely on these selected codes.)
- Filter on `vw_Timesheet`.`Rejected` (Type: Categorical, Explanation: This Power BI filter definition JSON is designed to filter data from a view called `vw_Timesheet`, specifically focusing on a column named `Rejected`. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data Source**: The filter is applied to a data source referred to as `vw_Timesheet`, which likely contains records related to timesheets.

2. **Focus on Rejected Timesheets**: The filter specifically looks at the `Rejected` column within this data source. This column likely indicates whether a timesheet has been rejected (e.g., due to errors, missing information, etc.).

3. **Inclusion Criteria**: The filter is set to include only those records where the `Rejected` column has a value of `true`. This means that when this filter is applied, only the timesheets that have been marked as rejected will be included in the analysis or report.

4. **Exclusion of Other Records**: Conversely, any timesheets that are not rejected (i.e., those where `Rejected` is `false` or possibly null) will be excluded from the results.

In summary, this filter ensures that only rejected timesheets are considered in the analysis, allowing users to focus on understanding the reasons for rejections and potentially addressing any issues related to them.)

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
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition JSON is designed to control which data is included when analyzing the `OWN-Sub-ExtT` column from the `lkp_Unit` table. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter is applied to the `OWN-Sub-ExtT` field within the `lkp_Unit` table. This means we are focusing on the values in this specific column.

2. **Filter Condition**: The filter specifies a condition that excludes certain data. Specifically, it is looking for entries in the `OWN-Sub-ExtT` column that are **not** equal to `null`.

3. **Inclusion and Exclusion**:
   - **Included Data**: Any records in the `lkp_Unit` table where the `OWN-Sub-ExtT` column has a value (i.e., it is not empty or null) will be included in the analysis.
   - **Excluded Data**: Any records where the `OWN-Sub-ExtT` column is `null` (meaning there is no value present) will be excluded from the analysis.

In summary, this filter ensures that only the records with valid, non-null values in the `OWN-Sub-ExtT` column are considered in your Power BI reports or visualizations. This helps in focusing on meaningful data and avoiding any entries that do not provide useful information.)

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

- Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `lkp_Unit` table are included based on the values in the `OWN-Sub-ExtT` column. Let's break it down in simple business terms:

### What the Filter Does:

1. **Target Data**: The filter is applied to the `OWN-Sub-ExtT` column in the `lkp_Unit` table.

2. **Inclusion Criteria**:
   - The filter **excludes** any records where the `OWN-Sub-ExtT` value is **null**. This means that if a record does not have a value in this column, it will not be included in the results.
   - The filter also **excludes** any records where the `OWN-Sub-ExtT` value is equal to **'SUB'**. Therefore, any record that has 'SUB' in this column will also be left out.

### Summary of Included Data:
- The filter will include records from the `lkp_Unit` table where the `OWN-Sub-ExtT` column has a value that is **not null** and is **not equal to 'SUB'**. 

### Example:
- If you have records with the following `OWN-Sub-ExtT` values:
  - 'A'
  - 'B'
  - null
  - 'SUB'
  - 'C'

After applying this filter, only the records with 'A', 'B', and 'C' will be included in the results, while the records with null and 'SUB' will be excluded.

In essence, this filter helps to focus on relevant data by filtering out any records that are either missing important information (null) or are categorized as 'SUB', which may not be needed for the analysis at hand.)
- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON specifies a filter that is applied to the `TimesheetCode` column of the `vw_Timesheet` view. Here's a breakdown of what this filter does in simple business terms:

### What Data is Included:
The filter is designed to **include** only those records from the `vw_Timesheet` view where the `TimesheetCode` matches one of the following specific values:

1. **'int'**
2. **'ROEG'**
3. **'ROIG'**
4. **'RSEG'**
5. **'V'**
6. **'Z'**

### What Data is Excluded:
Any records in the `vw_Timesheet` view that have a `TimesheetCode` **not** matching one of the above values will be **excluded** from the results. This means that if a record has a `TimesheetCode` like 'ABC', 'XYZ', or any other value that is not listed, it will not be shown in the filtered results.

### Summary:
In summary, this filter is used to narrow down the data to only those timesheets that have specific codes, allowing users to focus on particular types of timesheets that are relevant for their analysis or reporting needs.)
- Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: This Power BI filter definition JSON is designed to focus on a specific aspect of the data from the `vw_missing_timesheet` view, particularly the `ContractStatusToday` column. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter is applied to the `ContractStatusToday` field within the `vw_missing_timesheet` dataset. This field likely indicates the current status of contracts related to timesheets.

2. **Inclusion Criteria**: The filter specifies that only records where the `ContractStatusToday` is equal to `'Valid'` should be included in the analysis. This means that any records with a different status (such as 'Expired', 'Pending', or 'Invalid') will be excluded from the results.

3. **Purpose**: By applying this filter, the analysis will focus solely on valid contracts. This is useful for reporting or decision-making processes where only active and valid contracts are relevant, ensuring that any insights drawn from the data reflect the current and applicable contracts.

In summary, this filter ensures that only the records with a `ContractStatusToday` of `'Valid'` are considered, filtering out any irrelevant or inactive contract statuses from the analysis.)

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
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `lkp_Unit` table are included in your analysis based on the values in the `OWN-Sub-ExtT` column. Let's break it down in simple business terms:

### What the Filter Does:

1. **Target Data**: The filter is applied to the `OWN-Sub-ExtT` column in the `lkp_Unit` table.

2. **Inclusion Criteria**:
   - The filter **excludes** any records where the `OWN-Sub-ExtT` value is **null**. This means that if there is no value in this column for a record, that record will not be included in the results.
   - The filter also **excludes** any records where the `OWN-Sub-ExtT` value is equal to **'SUB'**. So, if a record has 'SUB' in this column, it will also be left out of the results.

### Summary of Included Data:
- The filter will only include records from the `lkp_Unit` table where the `OWN-Sub-ExtT` column has a value that is **not null** and is **not equal to 'SUB'**. 

### Practical Implication:
- If you are analyzing data related to units, this filter ensures that you are only looking at units that have a specific, meaningful value in the `OWN-Sub-ExtT` column, effectively filtering out any units that are either undefined (null) or categorized as 'SUB'. This helps in focusing on relevant data for your analysis.)

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
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `vw_Timesheet` view are included in your analysis based on the `Approved_With_V_Z` field.

### Breakdown of the Filter:

1. **Target Data Source**: The filter is applied to the `vw_Timesheet` view, specifically focusing on the `Approved_With_V_Z` column.

2. **Filter Condition**: The filter checks the value of the `Approved_With_V_Z` column. 

3. **Comparison**: 
   - The filter is looking for records where the `Approved_With_V_Z` value is **not equal to** `0L` (which typically represents a zero value in a long integer format).
   - In simpler terms, this means the filter is including only those records where `Approved_With_V_Z` has a value other than zero.

### Business Implication:

When this filter is applied, you will only see timesheet records that have been approved (i.e., those with a value greater than zero in the `Approved_With_V_Z` column). Any records where `Approved_With_V_Z` equals zero will be excluded from your analysis. This is useful for focusing on timesheets that have been processed and approved, allowing for more accurate reporting and insights into approved work hours or projects.)
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON specifies a filter that is applied to the `TimesheetCode` field in the `vw_Timesheet` view. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter is focused on the `TimesheetCode` column from the `vw_Timesheet` data view. This means it is looking at the codes that represent different timesheets.

2. **Included Values**: The filter specifies that only certain values of `TimesheetCode` should be included in the analysis. Specifically, it includes:
   - `'int'`
   - `'ROEG'`
   - `'RSEG'`

3. **Exclusion of Other Values**: Any `TimesheetCode` that is **not** one of these three values will be excluded from the data being analyzed. This means that if a timesheet has a code like `'ABC'` or `'XYZ'`, it will not be part of the results.

In summary, when this filter is applied, the analysis will only consider timesheets that have the codes `'int'`, `'ROEG'`, or `'RSEG'`, and all other timesheet codes will be ignored. This helps in focusing the analysis on specific types of timesheets that are relevant for the business context.)
  - Filter on `vw_Timesheet`.`ContractStatusToday` (Type: Categorical, Definition: N/A)

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
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data from the `vw_Timesheet` view, particularly the `Approved_With_V_Z` column. Here’s a breakdown of what this filter does in simple business terms:

1. **Source of Data**: The filter is applied to a data source called `vw_Timesheet`, which likely contains records related to timesheets, such as hours worked, approvals, and other relevant details.

2. **Target Column**: The filter specifically looks at the `Approved_With_V_Z` column within this data source. This column probably indicates whether a timesheet has been approved, with different values representing different approval statuses.

3. **Filter Condition**: The filter condition checks for a specific value in the `Approved_With_V_Z` column. It is looking for records where this column equals `1L`. 

4. **Meaning of the Value**: The value `1L` typically represents a specific status, such as "approved" or "yes" in a binary approval system. Therefore, this filter is including only those records where the timesheet has been approved.

5. **Exclusion of Data**: As a result of this filter, any records in the `vw_Timesheet` view where `Approved_With_V_Z` does not equal `1L` will be excluded from the analysis. This means only approved timesheets will be considered in any reports or visualizations that use this filter.

In summary, this filter ensures that only approved timesheet records are included in the analysis, allowing users to focus on data that reflects approved work hours or activities.)
  - Filter on `vw_Timesheet`.`RReady_With_V_Z` (Type: Advanced, Explanation: This Power BI filter definition JSON is designed to filter data from a specific view called `vw_Timesheet`, focusing on a column named `RReady_With_V_Z`. Here’s a breakdown of what this filter does in simple business terms:

1. **Source of Data**: The filter is applied to a data source identified as `vw_Timesheet`, which is likely a view that aggregates or summarizes timesheet data.

2. **Target Column**: The filter specifically targets the column `RReady_With_V_Z`. This column likely contains values that indicate whether a certain condition related to readiness or status is met.

3. **Filter Condition**: The filter condition checks the values in the `RReady_With_V_Z` column. It compares these values to a specific literal value, which in this case is `0L`. The `0L` typically represents a numeric value of zero, possibly indicating that a certain readiness condition is not met.

4. **Inclusion/Exclusion Logic**: 
   - **Included Data**: The filter will include only those records from `vw_Timesheet` where the value in the `RReady_With_V_Z` column is equal to `0L`. This means that only timesheet entries that do not meet the readiness criteria (as indicated by a value of zero) will be included in the results.
   - **Excluded Data**: Any records where `RReady_With_V_Z` has a value other than `0L` (for example, values like `1`, `2`, or any other non-zero value) will be excluded from the results.

In summary, when this filter is applied, it narrows down the data to show only those timesheet entries that are not ready (indicated by a value of zero in the `RReady_With_V_Z` column).)
  - Filter on `vw_Timesheet`.`[Dax_EmpCount_RReady]` (Type: Advanced, Definition: N/A)
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON specifies a filter that is applied to the `TimesheetCode` field in the `vw_Timesheet` view. Here’s a breakdown of what this filter does in simple business terms:

### What Data is Included:
The filter is designed to **include** only those records from the `vw_Timesheet` view where the `TimesheetCode` matches any of the following values:

1. **'ROEG'**
2. **'ROIG'**
3. **'RSEG'**
4. **'V'**
5. **'Z'**
6. **'int'**

### What Data is Excluded:
Any records in the `vw_Timesheet` view that have a `TimesheetCode` **not** matching one of the above values will be **excluded** from the results. This means that if a `TimesheetCode` is anything other than 'ROEG', 'ROIG', 'RSEG', 'V', 'Z', or 'int', it will not appear in the filtered data.

### Summary:
In summary, this filter narrows down the dataset to only include timesheets that have specific codes, allowing users to focus on those particular entries while ignoring all others.)

**Active Employee**

- Type: `card`
- Name: `756ca04cf0b3a64ce88f`
- Fields Used:
  - `Msr_ActiveContacts` (Query: `PBI_TIMESHEET.Msr_ActiveContacts`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_missing_timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON specifies a filter that is applied to the `TimesheetCode` column of the `vw_missing_timesheet` view. Here's a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Target Data**: The filter is focused on the `TimesheetCode` field within the `vw_missing_timesheet` dataset.
- **Included Values**: The filter includes only those records where the `TimesheetCode` matches one of the specified values. The values that are included are:
  - `'int'`
  - `'ROEG'`
  - `'ROIG'`
  - `'RSEG'`
  - `'Z'`
  - `'V'`

### What the Filter Excludes:
- **Excluded Values**: Any records that have a `TimesheetCode` not listed above will be excluded from the results. This means that if a record has a `TimesheetCode` of, for example, `'ABC'` or `'XYZ'`, it will not be shown in the filtered results.

### Summary:
In summary, this filter is designed to narrow down the data to only show timesheets that have specific codes (like `'int'`, `'ROEG'`, etc.). Any timesheet with a code outside of this list will not be included in the analysis or report. This helps focus on particular types of timesheets that are of interest for further analysis or reporting.)

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
  - Filter on `vw_Timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: This Power BI filter definition JSON is designed to filter data from a specific view called `vw_Timesheet`, focusing on a column named `ContractStatusToday`. Here's a breakdown of what this filter does in simple business terms:

1. **Source of Data**: The filter is applied to a dataset referred to as `vw_Timesheet`, which likely contains information about timesheets, including various statuses related to contracts.

2. **Target Column**: The filter specifically targets the column `ContractStatusToday`. This column likely indicates the current status of contracts associated with the timesheets.

3. **Filter Condition**: The filter includes a condition that specifies which records to include based on the value of `ContractStatusToday`. 

4. **Included Value**: The filter is set to include only those records where the `ContractStatusToday` is equal to `'Valid'`. This means that any timesheet entry with a contract status that is not `'Valid'` will be excluded from the results.

### Summary:
In summary, when this filter is applied, it will only show timesheet records where the contract status is currently valid. Any records with a different status (like expired, pending, or invalid) will not be included in the data being analyzed or reported. This helps users focus on only the relevant and active contracts in their analysis.)

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

**Manager / Employee **

- Type: `pivotTable`
- Name: `763741ea54bdb0030a51`
- Fields Used:
  - `  ` (Query: `vw_missing_timesheet.EmployeeName`) (Role: Rows)
  - `  ` (Query: `vw_missing_timesheet.ManagerName`) (Role: Rows)
  - `  ` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON is designed to specify which records from the `vw_Timesheet` view should be included based on the `TimesheetCode` field. Here’s a breakdown of what this filter does in simple business terms:

### What Data is Included:
The filter is set to include only those records from the `vw_Timesheet` view where the `TimesheetCode` matches one of the following specific values:

1. **'int'**
2. **'ROEG'**
3. **'ROIG'**
4. **'RSEG'**
5. **'V'**
6. **'Z'**

### What Data is Excluded:
Any records in the `vw_Timesheet` view that do not have a `TimesheetCode` equal to one of the values listed above will be excluded from the results. 

### Summary:
In summary, when this filter is applied, you will only see timesheet records that have a `TimesheetCode` of 'int', 'ROEG', 'ROIG', 'RSEG', 'V', or 'Z'. All other timesheet records will be filtered out and not displayed in your report or analysis.)
  - Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data from the `vw_missing_timesheet` view, particularly the `ContractStatusToday` column. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data Source**: The filter is applied to a data source called `vw_missing_timesheet`, which likely contains information about timesheets and their statuses.

2. **Column of Interest**: The filter specifically looks at the `ContractStatusToday` column within this data source. This column probably indicates the current status of contracts related to timesheets.

3. **Inclusion Criteria**: The filter includes only those records where the `ContractStatusToday` is equal to `'Valid'`. This means that any timesheet entries that do not have a status of `'Valid'` will be excluded from the analysis.

4. **Resulting Data**: After applying this filter, the resulting dataset will consist solely of entries from the `vw_missing_timesheet` where the contract status is valid. This helps in focusing on only those timesheets that are currently in good standing, allowing for more accurate reporting and analysis.

In summary, this filter ensures that only valid contract statuses are considered in any reports or visualizations, effectively excluding any entries that may be invalid or not applicable.)

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
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `vw_Timesheet` data view are included in your analysis based on a specific condition related to the `Approved_With_V_Z` field.

### Breakdown of the Filter:

1. **Target Data Source**: The filter is applied to the `vw_Timesheet` view, which likely contains timesheet records.

2. **Field of Interest**: The specific field being filtered is `Approved_With_V_Z`. This field probably indicates whether a timesheet has been approved or not, with different values representing different statuses.

3. **Filter Condition**: The filter checks if the value of `Approved_With_V_Z` is equal to `0L`. 
   - The `0L` likely represents a specific status, such as "not approved" or "pending approval".

### What This Means for Your Data:

- **Included Data**: When this filter is applied, only the records from `vw_Timesheet` where `Approved_With_V_Z` is equal to `0L` will be included in your analysis. This means you will only see timesheets that have not been approved.

- **Excluded Data**: Any records where `Approved_With_V_Z` has a value other than `0L` (such as approved timesheets) will be excluded from your analysis.

### Summary:

In simple terms, this filter is set up to focus exclusively on timesheets that have not yet been approved, allowing you to analyze or report on only those records.)

**funnel**

- Type: `funnel`
- Name: `e119b8a99ea024715e22`
- Fields Used:
  - `Dax_EmpCount_Approved` (Query: `PBI_TIMESHEET.Dax_EmpCount_Approved`) (Role: Y)
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Category)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is designed to limit the data that is displayed based on specific criteria related to the `TimesheetCode` field from the `vw_Timesheet` view. Here’s a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Target Data**: The filter is applied to the `TimesheetCode` column in the `vw_Timesheet` dataset.
- **Included Values**: The filter specifies a list of `TimesheetCode` values that should be included in the results. Only records with these specific codes will be shown.

### Included Timesheet Codes:
The filter includes the following `TimesheetCode` values:
1. **'ROEG'**
2. **'ROIG'**
3. **'RSEG'**
4. **'V'**
5. **'Z'**
6. **'int'**

### Excluded Data:
- Any records in the `vw_Timesheet` that do not have one of the above `TimesheetCode` values will be excluded from the results. This means that if a timesheet has a code that is not listed, it will not appear in the report or visualization.

### Summary:
In summary, this filter is used to focus on specific timesheet entries by including only those that match the defined codes. It helps in narrowing down the data to relevant entries, making it easier for users to analyze or report on specific types of timesheets.)
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `vw_Timesheet` view are included in your analysis based on the `Approved_With_V_Z` field.

### Breakdown of the Filter:

1. **Source of Data**: 
   - The filter is applied to the `vw_Timesheet` view, which likely contains timesheet data for employees.

2. **Field Being Filtered**: 
   - The specific field being evaluated is `Approved_With_V_Z`. This field probably indicates whether a timesheet has been approved or not, with different values representing different statuses.

3. **Filter Condition**:
   - The filter checks if the value of `Approved_With_V_Z` is equal to `0L`. 
   - The `0L` value typically represents a specific state, which in many systems could mean "not approved" or "pending approval".

### What This Means for Your Data:

- **Included Data**: 
  - When this filter is applied, only the records from `vw_Timesheet` where `Approved_With_V_Z` is equal to `0L` will be included in your analysis. This means you will only see timesheets that are not approved.

- **Excluded Data**: 
  - Any records where `Approved_With_V_Z` has a value other than `0L` (such as approved timesheets) will be excluded from your analysis.

### Summary:

In simple terms, this filter is set up to focus exclusively on timesheets that have not yet been approved. This can be useful for identifying pending approvals or for tracking timesheets that require attention.)

**funnel**

- Type: `funnel`
- Name: `4c5e0b8242a93e4d5107`
- Fields Used:
  - `Dax_EmpCount_MissingTS` (Query: `vw_missing_timesheet.Dax_EmpCount_MissingTS`) (Role: Y)
  - `MAIN_UNIT` (Query: `vw_missing_timesheet.MAIN_UNIT`) (Role: Category)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON specifies a filter that is applied to the `TimesheetCode` field in the `vw_Timesheet` view. Here’s a breakdown of what this filter does in simple business terms:

### What Data is Included:
The filter is designed to **include** only those records from the `vw_Timesheet` view where the `TimesheetCode` matches one of the following specific values:

1. **'int'**
2. **'ROEG'**
3. **'ROIG'**
4. **'RSEG'**
5. **'V'**
6. **'Z'**

### What Data is Excluded:
Any records in the `vw_Timesheet` view that have a `TimesheetCode` **not** matching one of the above values will be **excluded** from the results. 

### Summary:
In summary, when this filter is applied, you will only see timesheet entries that have a `TimesheetCode` of 'int', 'ROEG', 'ROIG', 'RSEG', 'V', or 'Z'. All other timesheet entries will be filtered out, ensuring that your analysis focuses solely on these specific codes.)
  - Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Definition: N/A)
  - Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data from the `vw_missing_timesheet` view, particularly the `ContractStatusToday` field. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter is applied to the `ContractStatusToday` column in the `vw_missing_timesheet` view. This column likely indicates the current status of contracts related to timesheets.

2. **Inclusion Criteria**: The filter specifies that only records where the `ContractStatusToday` is equal to `'Valid'` should be included in the analysis. This means that any records with a status other than `'Valid'` (such as `'Expired'`, `'Pending'`, or any other status) will be excluded from the results.

3. **Purpose**: By applying this filter, the analysis will focus solely on contracts that are currently valid. This can help in understanding which timesheets are associated with active contracts, allowing for better decision-making regarding resource allocation, compliance, or financial planning.

In summary, this filter ensures that only the data related to valid contracts is considered, filtering out any irrelevant or inactive contract statuses from the analysis.)

**Manager / Employee**

- Type: `pivotTable`
- Name: `8aaaf26692b347396169`
- Fields Used:
  - `  ` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `  ` (Query: `PBI_TIMESHEET.IPM_ManagerName`) (Role: Rows)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: This Power BI filter definition is designed to refine the data displayed in the `vw_Timesheet` view, specifically focusing on the `Approved_With_V_Z` column. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data Source**: The filter is applied to a data source called `vw_Timesheet`, which likely contains records related to timesheets, such as hours worked, approvals, and possibly other related information.

2. **Column of Interest**: The filter specifically looks at the `Approved_With_V_Z` column within this data source. This column likely indicates whether a timesheet has been approved, with different values representing different approval statuses.

3. **Filter Condition**: The filter condition checks for a specific value in the `Approved_With_V_Z` column. It is looking for records where the value is equal to `1L`. 

4. **Meaning of the Value**: The value `1L` typically represents a specific status, such as "approved." Therefore, this filter is including only those records where the timesheet has been marked as approved.

5. **Exclusion of Other Data**: By applying this filter, any records in the `vw_Timesheet` view that do not have `Approved_With_V_Z` equal to `1L` will be excluded from the results. This means that only approved timesheets will be visible in the report or dashboard.

In summary, this filter ensures that only approved timesheet records are included in the analysis, allowing users to focus solely on the data that meets this approval criterion.)
  - Filter on `vw_Timesheet`.`RReady_With_V_Z` (Type: Advanced, Explanation: This Power BI filter definition is designed to refine the data that is displayed from the `vw_Timesheet` view, specifically focusing on the `RReady_With_V_Z` column. Here’s a breakdown of what this filter does in simple business terms:

1. **Source of Data**: The filter is applied to a data source identified as `vw_Timesheet`, which is likely a view that contains timesheet-related data.

2. **Target Column**: The filter specifically targets the column named `RReady_With_V_Z`. This column presumably indicates whether a certain condition related to readiness is met for the timesheet entries.

3. **Filter Condition**: The filter condition checks the value of the `RReady_With_V_Z` column. It is looking for entries where this column has a value of `0L`. The "0L" typically represents a long integer value of zero.

4. **Inclusion Criteria**: By applying this filter, only those records from the `vw_Timesheet` view where `RReady_With_V_Z` equals `0L` will be included in the results. 

5. **Exclusion Criteria**: Conversely, any records where `RReady_With_V_Z` has a value other than `0L` will be excluded from the results.

### Summary:
In summary, this filter is set up to show only the timesheet entries that are marked with a readiness status of zero (indicating perhaps that they are not ready or completed). All other entries that have a different status will not be displayed in the report or visualization.)
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is designed to include specific data from the `vw_Timesheet` view based on the `TimesheetCode` field. Here's a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter is applied to the `TimesheetCode` column within the `vw_Timesheet` view. This means we are focusing on the codes that identify different types of timesheets.

2. **Included Values**: The filter specifies that only certain `TimesheetCode` values should be included in the analysis. The codes that are included are:
   - `'int'`
   - `'ROEG'`
   - `'ROIG'`
   - `'RSEG'`

3. **Exclusion of Other Values**: Any `TimesheetCode` that is **not** one of these four specified values will be excluded from the data set. This means that if a timesheet has a code like `'XYZ'` or any other code not listed, it will not be part of the results.

In summary, when this filter is applied, the analysis will only consider timesheets that have the codes `'int'`, `'ROEG'`, `'ROIG'`, or `'RSEG'`, effectively filtering out all other timesheet codes. This helps in focusing on specific types of timesheets for reporting or analysis purposes.)

**funnel**

- Type: `funnel`
- Name: `d6ba9f253d604522d7e9`
- Fields Used:
  - `MAIN_UNIT` (Query: `PBI_TIMESHEET.MAIN_UNIT`) (Role: Category)
  - `Dax_EmpCount_RReady` (Query: `PBI_TIMESHEET.Dax_EmpCount_RReady`) (Role: Y)
  - `Count of MAIN_UNIT` (Query: `CountNonNull(vw_Timesheet.MAIN_UNIT)`) (Role: Unknown)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON is designed to specify which records from the `vw_Timesheet` view should be included based on the `TimesheetCode` field. Here's a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Target Field**: The filter is applied to the `TimesheetCode` column in the `vw_Timesheet` view.
- **Inclusion Criteria**: The filter specifies that only records with certain `TimesheetCode` values should be included in the analysis or report.

### Included Values:
The filter includes the following specific `TimesheetCode` values:
1. **'ROEG'**
2. **'ROIG'**
3. **'RSEG'**
4. **'V'**
5. **'Z'**
6. **'int'**

### Summary:
When this filter is applied, only the records from the `vw_Timesheet` view that have a `TimesheetCode` matching one of the values listed above will be included in the results. Any records with a `TimesheetCode` that is not one of these specified values will be excluded from the analysis. 

In essence, this filter helps to focus on specific types of timesheets that are relevant for the analysis, ensuring that only the desired data is considered.)
  - Filter on `vw_Timesheet`.`Approved_With_V_Z` (Type: Advanced, Explanation: This Power BI filter definition is designed to include only specific data from the `vw_Timesheet` view, focusing on the `Approved_With_V_Z` column. Here’s a breakdown of what this filter does in simple business terms:

1. **Source of Data**: The filter is applied to a data source called `vw_Timesheet`, which likely contains records related to timesheets.

2. **Target Column**: The filter specifically looks at the column named `Approved_With_V_Z`. This column probably indicates whether a timesheet has been approved in a certain way, with the value "1L" representing a specific approval status.

3. **Filter Condition**: The filter condition checks if the value in the `Approved_With_V_Z` column is equal to "1L". 

4. **Inclusion Criteria**: When this filter is applied, it will **include only those records** from the `vw_Timesheet` where the `Approved_With_V_Z` column has the value "1L". 

5. **Exclusion Criteria**: Conversely, any records where the `Approved_With_V_Z` column does not equal "1L" will be **excluded** from the results.

In summary, this filter is used to narrow down the data to only those timesheet entries that have a specific approval status, allowing users to focus on a particular subset of approved timesheets.)
  - Filter on `vw_Timesheet`.`RReady_With_V_Z` (Type: Advanced, Explanation: This Power BI filter definition is designed to filter data from a specific view called `vw_Timesheet`, focusing on a column named `RReady_With_V_Z`. Here’s a breakdown of what this filter does in simple business terms:

1. **Source of Data**: The filter is applied to a dataset referred to as `vw_Timesheet`, which likely contains timesheet-related information.

2. **Target Column**: The filter specifically targets the column `RReady_With_V_Z`. This column probably indicates whether a timesheet is ready for some process or approval.

3. **Filter Condition**: The filter condition checks the value in the `RReady_With_V_Z` column. It looks for entries where this value is equal to `0L`. 

4. **Meaning of the Value**: The value `0L` typically represents a specific state or condition. In many contexts, this could mean "not ready" or "not approved." 

5. **Data Included/Excluded**: 
   - **Included**: Only the records from `vw_Timesheet` where `RReady_With_V_Z` equals `0L` will be included in the results. This means you will see only those timesheets that are marked as not ready.
   - **Excluded**: Any records where `RReady_With_V_Z` has a value other than `0L` (such as `1`, `2`, or any other value) will be excluded from the results.

In summary, this filter is used to focus on timesheets that are not ready for processing or approval, allowing users to identify and possibly address those specific entries.)

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

#### <a name="page-timesheet-overviewcategory-per-month"></a>Page: Timesheet Overview/Category per Month

*Internal Name: `4d6dc730d5c5f33b2ea4`, Ordinal: 10*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON is designed to specify which records from the `vw_Timesheet` view should be included based on the `TimesheetCode` field. Here’s a breakdown of what this filter does in simple business terms:

### What Data is Included:
The filter is set to include only those records from the `vw_Timesheet` where the `TimesheetCode` matches any of the following specific values:

1. **'int'**
2. **'ROEG'**
3. **'ROIG'**
4. **'RSEG'**
5. **'V'**
6. **'Z'**

### What Data is Excluded:
Any records in the `vw_Timesheet` that do not have a `TimesheetCode` of 'int', 'ROEG', 'ROIG', 'RSEG', 'V', or 'Z' will be excluded from the results. This means that if a record has a different `TimesheetCode`, it will not be shown in the analysis or report.

### Summary:
In summary, this filter is used to narrow down the data to only those timesheets that have specific codes, allowing users to focus on a defined subset of timesheet entries that are relevant for their analysis or reporting needs.)

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
  - Filter on `vw_Timesheet`.`Project` (Type: Categorical, Definition: N/A)
  - Filter on `vw_Timesheet`.`GroupCat` (Type: Categorical, Definition: N/A)
  - Filter on `vw_Timesheet`.`Sum(Hours)` (Type: Advanced, Definition: N/A)
  - Filter on `vw_Timesheet`.`FinancialYear` (Type: Advanced, Definition: N/A)

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
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `lkp_Unit` table are included in your analysis, specifically focusing on the `OWN-Sub-ExtT` column.

### Breakdown of the Filter:

1. **Source Table**: The filter is applied to the `lkp_Unit` table, which is referred to as `o` in the JSON.

2. **Filter Condition**: The key part of this filter is the condition specified in the `Where` section. It states that we want to **exclude** records where the `OWN-Sub-ExtT` column has a value of `null`.

3. **What This Means**:
   - **Included Data**: Only records from the `lkp_Unit` table where the `OWN-Sub-ExtT` column has a value (i.e., it is not null) will be included in the analysis.
   - **Excluded Data**: Any records where the `OWN-Sub-ExtT` column is null will be excluded from the results.

### Summary:
In simple terms, this filter ensures that you are only looking at records that have valid data in the `OWN-Sub-ExtT` column, effectively filtering out any incomplete or missing information. This helps in maintaining the quality and relevance of the data you are analyzing in Power BI.)

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

#### <a name="page-timesheet-overviewcategory-per-week"></a>Page: Timesheet Overview/Category per Week

*Internal Name: `865887498f4acf763b3d`, Ordinal: 9*

##### Page Level Filters

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is designed to limit the data displayed in a report or visualization based on specific values of the `TimesheetCode` field from the `vw_Timesheet` view. Here's a breakdown of what this filter does in simple business terms:

### What the Filter Does:

1. **Target Data Source**: The filter is applied to a data source called `vw_Timesheet`, which likely contains timesheet records.

2. **Field of Interest**: The specific field being filtered is `TimesheetCode`. This field likely categorizes or identifies different types of timesheets.

3. **Included Values**: The filter specifies a list of `TimesheetCode` values that should be included in the results. Only records with these codes will be shown. The included values are:
   - `'int'`
   - `'ROEG'`
   - `'ROIG'`
   - `'RSEG'`
   - `'V'`
   - `'Z'`

4. **Exclusion of Other Values**: Any records that do not have one of the specified `TimesheetCode` values will be excluded from the results. This means that if a timesheet has a code other than those listed, it will not appear in the report or visualization.

### Summary:

In summary, this filter is set up to only show timesheet records that have one of the following codes: `'int'`, `'ROEG'`, `'ROIG'`, `'RSEG'`, `'V'`, or `'Z'`. All other timesheet records will be filtered out, ensuring that the analysis focuses solely on these specific categories of timesheets.)

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
  - Filter on `vw_Timesheet`.`Project` (Type: Categorical, Definition: N/A)
  - Filter on `vw_Timesheet`.`GroupCat` (Type: Categorical, Definition: N/A)
  - Filter on `vw_Timesheet`.`Sum(Hours)` (Type: Advanced, Definition: N/A)
  - Filter on `vw_Timesheet`.`FinancialYear` (Type: Advanced, Definition: N/A)

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
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `lkp_Unit` table are included in your analysis, specifically focusing on the column `OWN-Sub-ExtT`.

### Breakdown of the Filter:

1. **Source Table**: The filter is applied to the `lkp_Unit` table, which is referenced as `o` in the JSON.

2. **Filter Condition**: The key part of the filter is the condition specified in the `Where` section. It states that we want to **exclude** records where the `OWN-Sub-ExtT` column has a value of `null`.

3. **What This Means**:
   - **Included Data**: Only records from the `lkp_Unit` table where the `OWN-Sub-ExtT` column contains a value (i.e., it is not `null`) will be included in the analysis.
   - **Excluded Data**: Any records where `OWN-Sub-ExtT` is `null` will be filtered out and not considered in your reports or visualizations.

### Summary:
In simple terms, this filter ensures that you are only looking at records that have meaningful data in the `OWN-Sub-ExtT` column, effectively ignoring any records that do not have a specified value in that field. This helps in maintaining data quality and relevance in your analysis.)

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

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is designed to include specific values from the `TimesheetCode` column in the `vw_Timesheet` view. Here's a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Target Data**: The filter is applied to the `TimesheetCode` field within the `vw_Timesheet` dataset.
- **Included Values**: The filter specifies a list of `TimesheetCode` values that should be included in the analysis. Only records with these codes will be considered.

### Included Timesheet Codes:
The following `TimesheetCode` values are included by this filter:
1. **'int'**
2. **'ROEG'**
3. **'ROIG'**
4. **'RSEG'**
5. **'V'**
6. **'Z'**
7. **'RSIG'**

### Excluded Data:
- Any records in the `vw_Timesheet` that do not have one of the above `TimesheetCode` values will be excluded from the analysis. This means that if a timesheet has a code that is not listed, it will not appear in the results.

### Summary:
In summary, this filter is used to focus on specific timesheet entries by including only those with the designated codes. This helps in analyzing or reporting on a targeted subset of timesheet data, ensuring that only relevant entries are considered.)

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
  - `DimDate.CalendarYear` (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `66934532619b0b009073`
- Fields Used:
  - `DimDate.ShortDate` (Role: Values)

**Unit**

- Type: `slicer`
- Name: `9029f0dd89303697364d`
- Fields Used:
  - `PBI_TIMESHEET.MAIN_UNIT` (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `646e8ef1016d8cbb5042`
- Fields Used:
  - `PBI_TIMESHEET.Unit` (Role: Values)

**Employee**

- Type: `slicer`
- Name: `e410cc9b9e813ccb6a09`
- Fields Used:
  - `PBI_TIMESHEET.EmployeeName` (Role: Values)

**Own-Sub-Ext**

- Type: `slicer`
- Name: `4982e2f59b589712a24d`
- Fields Used:
  - `ONEDRIVE_UNIT.OWN-Sub-ExtT` (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `lkp_Unit` table are included in your analysis based on the `OWN-Sub-ExtT` column. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data**: The filter is applied to the `OWN-Sub-ExtT` column in the `lkp_Unit` table. This column likely contains some kind of identifier or classification related to units.

2. **Filter Logic**: The filter specifies a condition that excludes certain records. Specifically, it is looking for records where the `OWN-Sub-ExtT` value is **not** equal to `null`.

3. **Inclusion Criteria**: By applying this filter, you are including only those records from the `lkp_Unit` table where the `OWN-Sub-ExtT` column has a value (i.e., it is not empty or undefined). 

4. **Exclusion Criteria**: Any records where the `OWN-Sub-ExtT` column is `null` (meaning there is no value present) will be excluded from your analysis.

In summary, this filter ensures that you only work with records that have a valid entry in the `OWN-Sub-ExtT` column, effectively filtering out any incomplete or missing data in that specific field.)

**image**

- Type: `image`
- Name: `223afae80d189e808382`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `37aeaba9e2b02eac1abb`
- Fields Used:
  - `PBI_TIMESHEET.Unit` (Role: Rows)
  - `Employee Name` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `PBI_TIMESHEET.Project` (Role: Rows)
  - `Sum(PBI_TIMESHEET.Hours)` (Role: Values)
  - `Year` (Query: `DimDate.CalendarYear`) (Role: Columns)
  - `Month` (Query: `DimDate.MonthNumberOfYear`) (Role: Columns)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which employee names are included in the data being analyzed from the `vw_Timesheet` view. Let's break it down in simple business terms:

1. **Data Source**: The filter is applied to a data source called `vw_Timesheet`, which likely contains timesheet records for employees.

2. **Target Field**: The specific field being filtered is `EmployeeName`. This means the filter is focusing on the names of employees recorded in the timesheet.

3. **Filter Logic**: The filter uses a condition that specifies which employee names should be included or excluded. In this case, the filter is set to **exclude** any records where the `EmployeeName` is `null`. 

   - **What does "null" mean?**: In this context, "null" indicates that there is no employee name recorded for that particular entry in the timesheet. 

4. **Outcome**: When this filter is applied, it will only include records where the `EmployeeName` has a valid, non-empty value. This means that any timesheet entries without an associated employee name will be ignored in the analysis.

In summary, this filter ensures that only timesheet records with actual employee names are considered, effectively excluding any entries that do not have a name associated with them. This helps in generating accurate reports and insights based on valid employee data.)

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

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON specifies a filter that is applied to the `TimesheetCode` column of the `vw_Timesheet` view. Here’s a breakdown of what this filter does in simple business terms:

### What Data is Included:
The filter is designed to **include** only those records from the `vw_Timesheet` view where the `TimesheetCode` matches any of the following specific values:

1. **'int'**
2. **'ROEG'**
3. **'ROIG'**
4. **'RSEG'**
5. **'V'**
6. **'Z'**
7. **'RSIG'**

### What Data is Excluded:
Any records in the `vw_Timesheet` view that have a `TimesheetCode` **not** listed above will be **excluded** from the results. This means that if a timesheet has a code that is different from the ones specified, it will not appear in the filtered data.

### Summary:
In summary, this filter is used to narrow down the dataset to only those timesheets that have specific codes, allowing users to focus on a defined subset of timesheet entries that are relevant for their analysis or reporting needs.)

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
  - `DimDate.CalendarYear` (Role: Values)

**TimeSpan**

- Type: `slicer`
- Name: `e3a7d72294d8d9e61495`
- Fields Used:
  - `DimDate.ShortDate` (Role: Values)

**Unit**

- Type: `slicer`
- Name: `b2b4cc327952867b69e4`
- Fields Used:
  - `PBI_TIMESHEET.MAIN_UNIT` (Role: Values)

**SubUnit**

- Type: `slicer`
- Name: `432e1018d6ab8f57edee`
- Fields Used:
  - `PBI_TIMESHEET.Unit` (Role: Values)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`Unit` (Type: Advanced, Explanation: This Power BI filter definition JSON is designed to control which records are included or excluded when analyzing data from the `vw_Timesheet` view, specifically focusing on the `Unit` column.

### Breakdown of the Filter:

1. **Source of Data**: 
   - The filter is applied to a data source named `vw_Timesheet`, which is likely a view that contains timesheet-related data.

2. **Target Column**: 
   - The filter specifically targets the `Unit` column within the `vw_Timesheet` view.

3. **Filter Condition**:
   - The filter condition is set to exclude any records where the `Unit` column has a value of `null`. 

### Business Terms Explanation:

- **Inclusion/Exclusion**:
  - This filter ensures that only records with a valid (non-null) value in the `Unit` column are included in the analysis. 
  - Any records where the `Unit` is not specified (i.e., it is `null`) will be excluded from the results.

### Summary:
In simple terms, when this filter is applied, you will only see timesheet entries that have a specified unit of work. Any entries that do not have a unit defined will be left out of your analysis, ensuring that the data you are working with is complete and relevant.)

**Employee**

- Type: `slicer`
- Name: `7b4d0077c29ca2050146`
- Fields Used:
  - `PBI_TIMESHEET.EmployeeName` (Role: Values)

**Own-Sub-Ext**

- Type: `slicer`
- Name: `e6c6261fce63077266d0`
- Fields Used:
  - `ONEDRIVE_UNIT.OWN-Sub-ExtT` (Role: Values)
- Visual Level Filters:
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `lkp_Unit` table are included in your analysis based on the `OWN-Sub-ExtT` field.

### Breakdown of the Filter:

1. **Target Entity**: The filter is applied to the `lkp_Unit` table, specifically focusing on the `OWN-Sub-ExtT` column.

2. **Filter Logic**: The filter uses a "Not" condition. This means it will exclude certain records based on the criteria defined.

3. **Condition Explained**:
   - The filter checks if the value in the `OWN-Sub-ExtT` column is **not** equal to `null`.
   - In simpler terms, it is looking for records where the `OWN-Sub-ExtT` field has a value (i.e., it is filled in) and is not empty.

### What Data is Included or Excluded:

- **Included**: Any record in the `lkp_Unit` table where the `OWN-Sub-ExtT` field has a value (not null).
- **Excluded**: Any record where the `OWN-Sub-ExtT` field is empty or has no value (null).

### Summary:

When this filter is applied, you will only see records from the `lkp_Unit` table that have a specified value in the `OWN-Sub-ExtT` column, effectively filtering out any records that do not have this information. This helps ensure that your analysis focuses on relevant data where the `OWN-Sub-ExtT` field is populated.)

**image**

- Type: `image`
- Name: `8887940f9bb84336a480`
- Fields Used: _(None detected)_

**pivotTable**

- Type: `pivotTable`
- Name: `936465ad5c5d30136110`
- Fields Used:
  - `PBI_TIMESHEET.Unit` (Role: Rows)
  - `Employee Name` (Query: `PBI_TIMESHEET.EmployeeName`) (Role: Rows)
  - `PBI_TIMESHEET.Project` (Role: Rows)
  - `Sum(PBI_TIMESHEET.Hours)` (Role: Values)
  - `DimDate.CalendarYear` (Role: Columns)
  - `DimDate.WeekNumberOfYear` (Role: Columns)
- Visual Level Filters:
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which employee names are included in the data being analyzed from the `vw_Timesheet` view. Let's break it down in simple terms:

1. **Source of Data**: The filter is applied to a data source called `vw_Timesheet`, specifically focusing on the `EmployeeName` field.

2. **Filter Condition**: The key part of this filter is the condition specified in the "Where" section. It states that we want to **exclude** any records where the `EmployeeName` is `null`. 

3. **What Does This Mean?**: 
   - If an employee's name is not recorded (i.e., it is `null`), that record will not be included in the results.
   - Conversely, any records where the `EmployeeName` has a valid name (not `null`) will be included in the analysis.

In summary, this filter ensures that only employees with valid names are considered in the report or visualization, effectively filtering out any entries that do not have an associated employee name.)

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

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is designed to include specific data from the `vw_Timesheet` view based on the `TimesheetCode` field. Here's a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Target Data**: The filter is applied to the `TimesheetCode` column in the `vw_Timesheet` view.
- **Included Values**: The filter specifies that only certain `TimesheetCode` values should be included in the analysis. These values are:
  - `'int'`
  - `'ROEG'`
  - `'ROIG'`
  - `'RSEG'`
  - `'V'`
  - `'Z'`

### What This Means:
- **Inclusion**: When this filter is applied, only records from the `vw_Timesheet` that have a `TimesheetCode` matching one of the specified values will be included in the report or analysis.
- **Exclusion**: Any records with a `TimesheetCode` that does not match one of these values will be excluded from the results.

### Summary:
In summary, this filter is used to focus on specific types of timesheets by including only those with the codes `'int'`, `'ROEG'`, `'ROIG'`, `'RSEG'`, `'V'`, and `'Z'`. All other timesheet codes will not be considered in the analysis, allowing for a more targeted view of the data.)

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
  - Filter on `vw_Timesheet`.`EmployeeName` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which employee names are included in a report or visualization based on the `vw_Timesheet` data source. Let's break it down in simple business terms:

1. **Data Source**: The filter is applied to a view called `vw_Timesheet`, which likely contains timesheet data for employees.

2. **Target Field**: The specific field being filtered is `EmployeeName`, which represents the names of employees in the timesheet data.

3. **Filter Logic**: The filter is set up to **exclude** any records where the `EmployeeName` is `null`. In other words, it is looking for employee names that are present and not empty.

4. **What This Means**:
   - **Included Data**: Only those records where `EmployeeName` has a valid name (i.e., it is not `null`) will be included in the report or visualization.
   - **Excluded Data**: Any records where `EmployeeName` is missing (i.e., `null`) will be filtered out and not shown.

In summary, this filter ensures that the report only displays employees who have a name recorded in the timesheet, effectively ignoring any entries that do not have an associated employee name.)

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
  - Filter on `lkp_Unit`.`OWN-Sub-ExtT` (Type: Advanced, Explanation: This Power BI filter definition is designed to control which records from the `lkp_Unit` table are included in your analysis based on the `OWN-Sub-ExtT` column.

### Breakdown of the Filter:

1. **Target Entity**: The filter is applied to the `lkp_Unit` table, specifically focusing on the `OWN-Sub-ExtT` column.

2. **Filter Condition**: The key part of this filter is the condition specified in the "Where" section. It states that we want to **exclude** any records where the `OWN-Sub-ExtT` column has a value of `null`.

### In Simple Terms:

- **Included Data**: This filter will include all records from the `lkp_Unit` table where the `OWN-Sub-ExtT` column has a value (i.e., it is not empty or null).
  
- **Excluded Data**: Any record where the `OWN-Sub-ExtT` column is `null` will be excluded from the results.

### Summary:

When this filter is applied, you will only see records from the `lkp_Unit` table that have a valid, non-null entry in the `OWN-Sub-ExtT` column. This helps ensure that your analysis focuses on meaningful data, avoiding any entries that do not provide useful information.)

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

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition is designed to include specific values from the `TimesheetCode` column in the `vw_Timesheet` view. Here’s a breakdown of what this filter does in simple business terms:

### What the Filter Does:
- **Target Data**: The filter is applied to the `TimesheetCode` field within the `vw_Timesheet` dataset.
- **Included Values**: The filter specifies a list of `TimesheetCode` values that should be included in the analysis. Only records with these specific codes will be considered.

### Included Timesheet Codes:
The filter includes the following `TimesheetCode` values:
1. **'int'**
2. **'ROEG'**
3. **'ROIG'**
4. **'RSEG'**
5. **'V'**
6. **'Z'**
7. **'RSIG'**

### Excluded Data:
- Any records in the `vw_Timesheet` that do not have one of the above `TimesheetCode` values will be excluded from the analysis. This means that if a timesheet has a code that is not listed, it will not appear in the results.

### Summary:
In summary, this filter is used to focus on specific types of timesheets by including only those with the designated codes. This helps in analyzing or reporting on a targeted subset of timesheet data, ensuring that only relevant entries are considered.)
- Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data from the `vw_missing_timesheet` view, particularly the `ContractStatusToday` column. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data Source**: The filter is applied to a data source called `vw_missing_timesheet`, which likely contains information about timesheets and their statuses.

2. **Column of Interest**: The filter specifically looks at the `ContractStatusToday` column within this data source. This column probably indicates the current status of contracts related to timesheets.

3. **Filter Condition**: The filter condition specifies that only records where the `ContractStatusToday` is equal to `'Valid'` should be included in the analysis. 

4. **Inclusion Criteria**: By using the "In" condition, the filter is saying, "Include only those records where the `ContractStatusToday` is 'Valid'." 

5. **Exclusion of Other Data**: As a result, any records where the `ContractStatusToday` is not `'Valid'` (for example, statuses like 'Expired', 'Pending', or 'Invalid') will be excluded from the analysis.

In summary, this filter ensures that only the timesheet records with a current contract status of 'Valid' are considered, allowing users to focus on relevant and actionable data regarding valid contracts.)

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

- Filter on `vw_Timesheet`.`TimesheetCode` (Type: Categorical, Explanation: This Power BI filter definition JSON specifies a filter that is applied to the `TimesheetCode` column of the `vw_Timesheet` view. Here's a breakdown of what this filter does in simple business terms:

### What Data is Included:
The filter is designed to **include** only those records from the `vw_Timesheet` view where the `TimesheetCode` matches any of the following specific values:

1. **'int'**
2. **'ROEG'**
3. **'ROIG'**
4. **'RSEG'**
5. **'V'**
6. **'Z'**
7. **'RSIG'**

### What Data is Excluded:
Any records in the `vw_Timesheet` view that have a `TimesheetCode` **not** listed above will be **excluded** from the results. This means that if a record has a `TimesheetCode` that is different from the ones specified, it will not appear in the filtered data.

### Summary:
In summary, this filter narrows down the data to only show timesheets that have specific codes, allowing users to focus on those particular entries while ignoring all others. This can be useful for reporting or analysis where only certain types of timesheets are relevant.)
- Filter on `vw_missing_timesheet`.`ContractStatusToday` (Type: Categorical, Explanation: This Power BI filter definition is designed to focus on a specific aspect of the data from the `vw_missing_timesheet` view, particularly the `ContractStatusToday` field. Here’s a breakdown of what this filter does in simple business terms:

1. **Target Data Source**: The filter is applied to a data source called `vw_missing_timesheet`, which likely contains information about timesheets and their statuses.

2. **Field of Interest**: The filter specifically looks at the `ContractStatusToday` field within this data source. This field probably indicates the current status of contracts related to timesheets.

3. **Inclusion Criteria**: The filter includes only those records where the `ContractStatusToday` is equal to `'Valid'`. This means that any timesheet records that have a contract status other than `'Valid'` will be excluded from the analysis.

4. **Purpose**: By applying this filter, the analysis will focus solely on valid contracts, allowing users to see only the relevant timesheet data that is associated with contracts that are currently considered valid.

In summary, this filter ensures that only timesheet records with a `ContractStatusToday` of `'Valid'` are included in the report or visualization, effectively excluding any records with different statuses.)

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

