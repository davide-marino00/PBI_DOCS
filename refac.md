# Power BI Model & Report Documentation

*Generated on: 2025-04-28 16:58:21*

## Table of Contents

- [Overview](#overview)
- [Report Structure](#report-structure)
  - [Report Level Filters](#report-level-filters)
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

---

## <a name="overview"></a>Overview

The Power BI data model appears to be designed for a business domain focused on project management and timesheet tracking, likely within a professional services or consulting environment. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a strong emphasis on monitoring employee hours worked against various projects, as well as identifying gaps in timesheet submissions. The inclusion of lookup tables like 'lkp_Project', 'lkp_Unit', and 'lkp_Code' indicates a structured approach to categorizing and analyzing data related to projects, organizational units, and timesheet codes, which are essential for accurate reporting and resource allocation.

The 'DimDate' table serves as a central date dimension, allowing for time-based analysis of project performance and employee productivity over various periods. The relationships established between these tables facilitate comprehensive reporting capabilities, enabling stakeholders to assess project progress, employee contributions, and overall operational efficiency. This model likely supports decision-making processes by providing insights into resource utilization, project timelines, and potential areas for improvement in timesheet compliance.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

- Filter on Unknown Target (Type: N/A, Definition: `N/A`)

_No page details were loaded or parsed._

## <a name="data-model"></a>Data Model

### <a name="relationships"></a>Relationships

The following relationships link the tables:

* `vw_missing_timesheet`.[`Project`] -> `lkp_Project`.[`Project`]
* `vw_missing_timesheet`.[`ShortDate`] -> `DimDate`.[`ShortDate`]
* `vw_missing_timesheet`.[`SubUnit`] -> `lkp_Unit`.[`Organisatorische eenheid`]
* `vw_missing_timesheet`.[`TimesheetCode`] -> `lkp_Code`.[`Code`]
* `vw_Timesheet`.[`ContractEndDate`] -> `DimDate`.[`ShortDate`] (Inactive)
* `vw_Timesheet`.[`ContractStartDate`] -> `DimDate`.[`ShortDate`] (Inactive)
* `vw_Timesheet`.[`Project`] -> `lkp_Project`.[`Project`]
* `vw_Timesheet`.[`ProjectCode`] -> `tbl_Project`.[`Project`] (Filter: bothDirections)
* `vw_Timesheet`.[`TimesheetCode`] -> `lkp_Code`.[`Code`]
* `vw_Timesheet`.[`TimesheetDate`] -> `DimDate`.[`ShortDate`]
* `vw_Timesheet`.[`Unit`] -> `lkp_Unit`.[`Organisatorische eenheid`]

---

### <a name="tables"></a>Tables

#### <a name="table-dimdate"></a>Table: `DimDate`

The 'DimDate' table serves as a comprehensive date dimension that enables businesses to analyze and report on time-based metrics effectively. By providing detailed attributes such as day, week, month, and quarter, this table supports various time intelligence calculations and enhances the ability to filter and aggregate data across different time periods.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of a given date in string format, facilitating time-based analysis and reporting by allowing users to easily categorize and aggregate data according to quarterly performance metrics. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of the date, facilitating year-over-year analysis and time-based reporting for enhanced business insights. |
| `DateKey` | `int64` | The 'DateKey' column (int64) uniquely identifies each date in the 'DimDate' table, facilitating efficient time-based analysis and reporting by serving as a primary reference for various date attributes. |
| `DayName` | `string` | The 'DayName' column contains the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating time-based analysis and reporting within the DimDate table. |
| `DayNumberOfMonth` | `int64` | The 'DayNumberOfMonth' column represents the numerical day of the month (ranging from 1 to 31) for each date entry, facilitating precise time-based analysis and reporting within the DimDate table. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) represents the numerical value of the day within the week, ranging from 1 (Sunday) to 7 (Saturday), facilitating time-based analysis and reporting in the 'DimDate' table. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column represents the sequential day of the year (ranging from 1 to 365 or 366) for each date entry, facilitating time-based analysis and reporting by allowing users to easily identify and aggregate data based on specific days within the annual calendar. |
| `MonthName` | `string` | The 'MonthName' column contains the full name of each month, facilitating easy identification and reporting of time-based metrics within the DimDate table. |
| `MonthNumberOfYear` | `int64` | The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month (1 for January through 12 for December), facilitating time-based analysis and reporting by enabling precise filtering and aggregation of data across different months. |
| `ShortDate` | `dateTime` | The 'ShortDate' column represents a simplified date format that facilitates quick reference and analysis of specific dates within the comprehensive date dimension, enhancing time-based reporting and filtering capabilities. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number within the calendar year, facilitating time-based analysis and reporting by allowing businesses to aggregate and filter data on a weekly basis. |

##### Calculated Columns

**`CurrentYearFlag`**

- **Description:** The 'CurrentYearFlag' column indicates whether a given date falls within the current calendar year, facilitating time-based analysis and reporting for year-over-year comparisons in the 'DimDate' table.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CurrentYearFlag' in a data model, specifically within a date dimension table (DimDate). 

Here's what it does in simple terms:

1. **YEAR(DimDate[ShortDate])**: This part extracts the year from the date in the 'ShortDate' column of the DimDate table. For example, if the date is '2023-05-15', it will return '2023'.

2. **YEAR(TODAY())**: This part gets the current year based on today's date. If today is any date in 2023, this will return '2023'.

3. **IF(..., 1, 0)**: The IF function checks if the year extracted from the 'ShortDate' is less than or equal to the current year. 
   - If it is (meaning the date is from this year or a previous year), it returns '1'.
   - If it is not (meaning the date is from a future year), it returns '0'.

**In summary**, this expression flags each date in the DimDate table as either '1' (if the date is from the current year or earlier) or '0' (if the date is from a future year). This can be useful for filtering or analyzing data based on whether dates are in the current year or not.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table is designed to track and analyze the distribution of hours worked alongside their corresponding percentage contributions to overall performance metrics. This table aids in understanding workforce productivity and resource allocation, enabling informed decision-making for operational efficiency.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column captures the string representation of hours worked alongside their percentage contributions, facilitating the analysis of workforce productivity and resource allocation in relation to overall performance metrics. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string values that represent various metrics related to hours worked and their percentage contributions, facilitating the analysis of workforce productivity and resource allocation. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column categorizes the sequence of hours worked and their associated percentage contributions, facilitating the analysis of workforce productivity and resource allocation within the overall performance metrics. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, with associated qualification levels and sorting criteria to facilitate efficient data retrieval and reporting. This table is essential for maintaining data integrity and consistency in code usage throughout the organization.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers that represent specific categories within the reference dataset, ensuring consistent and accurate code usage across the organization. |
| `Qualify` | `int64` | The 'Qualify' column (int64) indicates the qualification level associated with each code in the 'lkp_Code' table, facilitating the categorization and sorting of codes for improved data retrieval and reporting accuracy. |
| `Sort` | `string` | The 'Sort' column (string) in the 'lkp_Code' table specifies the order in which codes should be displayed or processed, ensuring consistent and efficient retrieval of categorized data across the organization. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics and performance metrics. This table enhances data integrity and usability by standardizing employee information for analysis and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column (string) uniquely identifies each employee in the 'lkp_fltr_Employee' table, serving as a critical reference point for filtering and reporting employee-related data across business analytics and performance metrics. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, serving as a key reference point for filtering and reporting within the 'lkp_fltr_Employee' table to enhance data integrity and support informed decision-making. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, detailing key attributes such as project names, qualification scores, billing amounts, and associated groups, enabling businesses to analyze project performance and financial metrics effectively.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the total billing amount associated with each project, facilitating financial analysis and performance evaluation. |
| `Group` | `string` | The 'Group' column identifies the specific organizational unit or team associated with each project, facilitating targeted analysis of project performance and financial metrics within the 'lkp_Project' table. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for referencing and analyzing project-related performance and financial metrics within the organization. |
| `Qualify` | `int64` | The 'Qualify' column (int64) indicates the qualification score of each project, serving as a metric to assess project viability and performance within the lkp_Project reference table. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing attributes such as billable department status and unit classifications. This table is essential for analyzing resource allocation and financial performance across different organizational segments.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column indicates whether the organizational unit is classified as a billable department, using an integer value to represent its status for financial analysis and resource allocation. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column contains the names of the organizational units, providing a key reference for identifying and categorizing various segments within the business for resource allocation and financial analysis. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column identifies the external type classification of ownership for each organizational unit, facilitating insights into resource allocation and financial performance across various segments. |
| `Unit` | `string` | The 'Unit' column contains the names of organizational units, serving as a key reference for identifying and classifying departments within the business for resource allocation and financial analysis. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, leadership, and status. This table enables businesses to effectively manage and analyze their projects, facilitating better decision-making and resource allocation across various project groups.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | 'ActualDateCompleted' records the precise date and time when a project was officially completed, providing critical data for performance analysis and project timeline assessments within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column contains string values that represent the administrative oversight or management team responsible for the project's execution and governance within the 'tbl_Project' table. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a crucial timestamp for tracking updates and changes within the project management lifecycle. |
| `ClientName` | `string` | The 'ClientName' column stores the name of the client associated with each project, providing essential context for project management and stakeholder engagement within the 'tbl_Project' table. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, enabling efficient tracking and management of client-related project activities within the tbl_Project table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project was initiated, providing a timeline reference for project management and analysis within the 'tbl_Project' table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project, enabling accurate budgeting and financial analysis within the project management framework. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the exact date and time when each project entry was created, providing a chronological reference for project initiation within the tbl_Project repository. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or framework under which the project is categorized, aiding in the alignment of project objectives with organizational priorities. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column captures the scheduled date and time for the completion of a project, enabling effective tracking of project timelines and facilitating timely decision-making and resource allocation. |
| `Project` | `string` | The 'Project' column contains the name or title of each project, serving as a key identifier that allows for easy reference and organization within the comprehensive project management framework of the 'tbl_Project' table. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed narrative of the project's objectives and scope, providing essential context for stakeholders to understand the project's purpose and goals within the 'tbl_Project' repository. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the specific group or team responsible for managing and executing the project, enabling streamlined collaboration and accountability within the organization. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a detailed narrative of the project group, outlining its objectives, scope, and key characteristics to enhance understanding and management of related projects within the 'tbl_Project' table. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for managing and analyzing project-related information. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader responsible for overseeing the project's execution and management, ensuring accountability and effective leadership within the project framework. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column represents the base currency amount for the sales price of the project, allowing for accurate financial analysis and reporting within the project's overall budget and profitability assessments. |
| `Startdate` | `dateTime` | The 'Startdate' column records the date and time when a project officially commences, providing a critical reference point for project timelines and scheduling within the 'tbl_Project' table. |
| `Status` | `string` | The 'Status' column indicates the current phase or condition of the project, providing essential insights for effective project management and resource allocation. |
| `Statuscode` | `string` | The 'Statuscode' column indicates the current status of each project, allowing for effective tracking and management of project progress within the tbl_Project repository. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, enabling businesses to assess financial commitments and manage budgets effectively within the project management framework. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table is designed to identify and track employees who have not submitted their timesheets for specific weeks, providing essential insights for managers to ensure compliance with reporting requirements. It includes key details such as employee and manager information, contract dates, and hours, enabling effective follow-up and resolution of missing submissions.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | 'ActualHours' represents the total number of hours an employee has worked during the specified week, providing critical data for managers to assess compliance and address missing timesheet submissions. |
| `Approved` | `boolean` | Indicates whether the missing timesheet for the employee has been approved by management, facilitating tracking and resolution of compliance issues. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column indicates the date and time when an employee's contract concludes, providing critical context for managers to assess the urgency of timesheet compliance for employees nearing the end of their employment. |
| `ContractHours` | `int64` | The 'ContractHours' column (int64) represents the total number of hours an employee is contracted to work, serving as a critical reference point for managers to assess compliance and address any discrepancies in timesheet submissions. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing crucial context for assessing timesheet submission compliance within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of an employee's contract, providing managers with real-time insights into contract compliance as they track missing timesheet submissions. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet submissions for compliance and reporting purposes. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the names of employees who have not submitted their timesheets, facilitating targeted follow-up and compliance management for reporting requirements. |
| `Hours_` | `double` | The 'Hours_' column (Data Type: double) represents the total number of hours an employee is expected to report for the specified week, serving as a critical metric for identifying discrepancies in timesheet submissions within the 'vw_missing_timesheet' table. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the missing timesheet data was captured, facilitating timely tracking and management of compliance issues. |
| `ManagerID` | `string` | The 'ManagerID' column (string) identifies the unique identifier of the manager responsible for overseeing the employee associated with the missing timesheet, facilitating targeted follow-up and accountability in timesheet compliance. |
| `ManagerName` | `string` | The 'ManagerName' column contains the name of the employee's manager, facilitating accountability and communication regarding timesheet submission compliance within the 'vw_missing_timesheet' table. |
| `MissingHours` | `double` | The 'MissingHours' column (double) represents the total number of hours an employee has failed to report in their timesheet for the specified week, serving as a critical metric for managers to address compliance issues and ensure accurate payroll processing. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with the timesheet submission, allowing managers to track compliance and accountability for time reporting related to various assignments. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a string identifier for the specific project associated with the timesheet, facilitating the tracking of missing submissions by linking them to relevant project activities. |
| `Rejected` | `boolean` | Indicates whether the timesheet submission for the employee has been rejected, aiding managers in identifying compliance issues and facilitating timely follow-up. |
| `ReportReady` | `boolean` | Indicates whether the report on missing timesheets is ready for review, facilitating timely follow-up and resolution of compliance issues. |
| `SalesAmount` | `double` | The 'SalesAmount' column (double) in the 'vw_missing_timesheet' table represents the total sales generated by employees during the specified weeks, providing critical financial context to assess the impact of missing timesheet submissions on overall business performance. |
| `SalesPrice` | `double` | The 'SalesPrice' column (double) represents the monetary value assigned to sales transactions associated with employees, aiding in the analysis of financial impact related to timesheet compliance and performance tracking. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with the missing timesheet submission, facilitating the tracking of compliance and timely follow-up for each employee's reporting obligations. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific sub-unit or department within the organization to which the employee belongs, facilitating targeted follow-up on missing timesheet submissions. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet submission, facilitating the tracking and management of missing timesheets for employees within the 'vw_missing_timesheet' table. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a textual summary of the specific timesheet entries that are missing for each employee, aiding managers in understanding the context and urgency of the submission issue. |
| `Week_` | `int64` | The 'Week_' column (int64) indicates the specific week number for which an employee's timesheet submission is missing, facilitating targeted follow-up and compliance tracking for managers. |
| `Year_` | `int64` | The 'Year_' column (int64) indicates the calendar year associated with the timesheet submission status, facilitating the tracking of compliance and reporting for each employee within the 'vw_missing_timesheet' table. |

##### Calculated Columns

**`BillableDep`**

- **Description:** The 'BillableDep' column indicates the department associated with billable hours for employees, aiding in the identification of revenue-generating activities linked to missing timesheet submissions.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'BillableDep'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression first checks if there is a related value in the 'BillableDep' column from the 'lkp_Unit' table. This is done using the `RELATED` function, which pulls in data from a related table based on the relationships defined in your data model.

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of specific information, the default assumption is that the item is billable.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that the item is billable.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that the item is not billable.

In summary, this DAX expression effectively categorizes items as billable or not based on the related 'BillableDep' value. If there is no related data, or if the related value is non-zero, it assumes the item is billable (returns 1). If the related value is zero, it assumes the item is not billable (returns 0).

**`CC_ActiveEmployees`**

- **Description:** The 'CC_ActiveEmployees' column contains a string representation of the active employees' identifiers, facilitating the identification of those who have not submitted their timesheets within the 'vw_missing_timesheet' table.
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
   - **Start Date Check**: It first checks if the date in the column `ShortDate` (which likely represents a specific date of interest) is on or after the employee's `ContractStartDate`. This means the employee's contract must have started before or on this date for them to be considered active.
   - **End Date Check**: Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. This means the employee's contract must still be valid on this date. However, if the `ContractEndDate` is blank (which could indicate that the employee's contract is ongoing or has no defined end), the employee is still considered active.

3. **Output**: 
   - If both conditions are met (the date is within the contract period or the end date is blank), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is not met, it returns `0`, indicating that the employee is not active.

In summary, this DAX expression effectively flags employees as active or inactive based on whether the date of interest falls within their contract period, allowing for better tracking of employee status in the organization.

**`IncompleteFlag`**

- **Description:** The 'IncompleteFlag' column indicates whether an employee's timesheet submission is incomplete, serving as a critical marker for managers to identify and address compliance issues promptly.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IncompleteFlag' in a data table, specifically in the context of tracking timesheets.

Here's what it does in simple terms:

- The expression checks the value in the 'MissingHours' column of the 'vw_missing_timesheet' table.
- If the value of 'MissingHours' is less than 0, it means that there are negative missing hours, which could indicate an error or an issue with the timesheet data.
- In this case, the expression returns a value of 1, which can be interpreted as a flag indicating that there is an incompleteness or problem with the timesheet.
- If the value of 'MissingHours' is 0 or greater, the expression returns 0, indicating that there are no issues with the timesheet.

In summary, this DAX expression helps identify and flag records where there is a potential problem with the reported hours in timesheets, allowing for easier tracking and resolution of incomplete or erroneous entries.

**`MAIN_UNIT`**

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit or department to which the employee belongs, facilitating targeted follow-up and accountability for timesheet submissions within the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'MAIN_UNIT'. Here's a simple breakdown of what it does:

1. **Check for Blank Values**: The expression starts by checking if the related value from the 'lkp_Unit' table (specifically the 'Unit' column) is blank (i.e., missing or not available).

2. **Return "Unknown" if Blank**: If the 'Unit' value is blank, the expression returns the string "Unknown". This is a way to handle cases where there is no corresponding unit information available.

3. **Return the Unit Value if Not Blank**: If the 'Unit' value is not blank, the expression retrieves and returns that actual unit value from the 'lkp_Unit' table.

In summary, this DAX code ensures that for each row in the main table, if there is no associated unit information, it will label it as "Unknown". Otherwise, it will display the actual unit name. This helps maintain clarity in the data by providing a clear indication when unit information is missing.

**`YearWeek`**

- **Description:** The 'YearWeek' column (string) represents the specific year and week number for which an employee's timesheet submission is missing, facilitating targeted tracking and management of compliance issues.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX expression creates a new calculated column called 'YearWeek' that combines the year and the week number of a date from the 'ContractEndDate' field.

Here's a breakdown of what it does:

1. **YEAR([ContractEndDate])**: This part extracts the year from the 'ContractEndDate'. For example, if the date is December 15, 2023, this would return "2023".

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the 'ContractEndDate'. For instance, if the date is in the 50th week of the year, this would return "50".

3. **FORMAT(..., "00")**: This ensures that the week number is always displayed as two digits. So, if the week number is 5, it will be formatted as "05".

4. **Concatenation (&)**: The expression combines the year and the formatted week number into a single string, separated by a hyphen. For example, if the year is 2023 and the week number is 5, the final result would be "2023-05".

In summary, this DAX expression generates a string that represents the year and the week number of the 'ContractEndDate' in the format "YYYY-WW". This can be useful for reporting or analyzing data on a weekly basis within a specific year.

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

1. **Purpose**: The measure counts the number of unique employees (consultants) who have recorded negative hours in a timesheet.

2. **Components**:
   - **`CALCULATE`**: This function modifies the context in which the data is evaluated. In this case, it is used to apply a filter to the data.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the number of unique employee names from the 'vw_missing_timesheet' table. It ensures that each employee is only counted once, even if they have multiple entries.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This filter condition specifies that only those records where the 'MissingHours' value is less than zero should be considered. In practical terms, this means it looks for instances where employees have reported negative hours, which could indicate an error or a specific situation where hours were deducted.

3. **Outcome**: The final result of this measure is the total number of distinct employees who have negative hours recorded in their timesheets. This information can be useful for management to identify potential issues with timesheet reporting or to address any discrepancies in employee hours.

In summary, this DAX expression helps organizations track and analyze the number of consultants with negative hours, which can be critical for ensuring accurate timekeeping and addressing any underlying issues.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a specific measure called 'CountNegativeMissingHours'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The measure counts the number of unique employees who have recorded negative missing hours in the 'vw_missing_timesheet' table.

2. **Components**:
   - **CALCULATE**: This function modifies the context in which data is evaluated. In this case, it is used to apply a filter to the data.
   - **DISTINCTCOUNT**: This function counts the number of unique values in a specified column. Here, it counts unique Employee IDs.
   - **'vw_missing_timesheet'[EmployeeID]**: This is the column from which unique employee identifiers are being counted.
   - **'vw_missing_timesheet'[MissingHours] < 0**: This condition filters the data to only include records where the 'MissingHours' value is less than zero, meaning it focuses on instances where employees have negative missing hours.

3. **Outcome**: The result of this measure will give you the total number of distinct employees who have a record of negative missing hours. This could be useful for identifying employees who may have discrepancies in their timesheets or for analyzing patterns in missing hours.

In summary, 'CountNegativeMissingHours' helps businesses track how many employees have reported negative hours, which can indicate issues with time reporting or attendance.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of active employees who have missing timesheet hours for a specific period, where the total hours they reported are less than their contracted hours.

Here's a breakdown of what it does:

1. **Data Source**: It uses a table called `vw_missing_timesheet`, which likely contains records of employee hours worked, their contracted hours, and other relevant details.

2. **Summarization**: The `SUMMARIZE` function creates a new table that groups the data by employee name, year, week, unit, and subunit. For each group, it calculates two key values:
   - **MissingHours**: This is calculated by taking the total hours reported by the employee (`SUM(vw_missing_timesheet[Hours_])`) and subtracting their maximum contracted hours (`MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has reported fewer hours than they are contracted for.
   - **MinActiveEmp**: This captures the minimum value of a field called `CC_ActiveEmployees`, which likely indicates whether the employee is currently active (with a value of 1 meaning they are active).

3. **Filtering**: The `FILTER` function then narrows down this summarized data to only include rows where:
   - The employee is active (`[MinActiveEmp] = 1`).
   - The calculated missing hours are less than zero (`[MissingHours] < 0`), meaning they have not logged enough hours compared to their contract.

4. **Counting**: Finally, the `COUNTROWS` function counts the number of rows in the filtered table, which represents the number of active employees who have missing timesheet hours.

In summary, this DAX measure calculates how many active employees have reported fewer hours than they are contracted for, indicating potential issues with timesheet submissions. This information can be crucial for management to address compliance and ensure that employees are accurately reporting their work hours.

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

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. This variable filters a summarized view of a dataset called `vw_missing_timesheet`, which contains information about employee hours worked.

2. **Summarization**: Within this summarized view, the code groups data by key attributes such as employee name, year, week, unit, and subunit. It also calculates two important metrics:
   - **MissingHours**: This is calculated by taking the total hours worked by each employee (using `SUM(vw_missing_timesheet[Hours_])`) and subtracting the maximum contracted hours they are supposed to work (using `MAX(vw_missing_timesheet[ContractHours])`). If this value is negative, it indicates that the employee has not worked enough hours.
   - **MinActiveEmp**: This metric captures the minimum value of a field that indicates whether an employee is active (using `MIN(vw_missing_timesheet[CC_ActiveEmployees])`). If this value equals 1, it means the employee is currently active.

3. **Filtering Criteria**: The `FILTER` function then narrows down the summarized data to only include those employees who are active (`[MinActiveEmp] = 1`) and have negative missing hours (`[MissingHours] < 0`). This means we are only looking at active employees who have not fulfilled their required hours.

4. **Final Calculation**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered list of active employees and sums up their missing hours. This gives the total number of hours that active employees are missing compared to their contracted hours.

In summary, this DAX measure calculates the total hours that active employees have not worked, highlighting those who are falling short of their expected hours in a given week and year. This information can be crucial for management to address attendance issues or workload distribution.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total expected contract hours for employees on a weekly basis. Here's a breakdown of what it does in simple business terms:

1. **Data Source**: It uses a data table called `vw_missing_timesheet`, which likely contains information about employees, their working hours, and possibly other related data.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key fields: `Week_` (which represents the week of the year) and `EmployeeID` (which identifies each employee). This means that for each employee in each week, we will be looking at their contract hours.

3. **Calculating Maximum Contract Hours**: For each group (each employee in each week), the expression calculates the maximum value of `ContractHours`. This is done using the `MAX` function. Essentially, if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

4. **Summing Up Contract Hours**: After summarizing the data and calculating the maximum contract hours for each employee per week, the `SUMX` function then adds up all these maximum contract hours across all employees and weeks. 

5. **Final Result**: The final result of this measure is the total expected contract hours for all employees across all weeks, considering only the highest recorded hours for each employee in each week.

In summary, this DAX measure helps in understanding the total expected working hours for employees on a weekly basis, ensuring that if there are multiple records for an employee in a week, only the highest contract hours are counted.

**`MISSING%`**

- **DAX Expression:**
```dax
DIVIDE([Dax_EmpCount]-[Dax_EmpCount_MissingTS],[Dax_EmpCount]) * 100
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of employees who are not missing time stamps (TS) compared to the total number of employees. Here's a breakdown of what each part does:

1. **[Dax_EmpCount]**: This represents the total number of employees in the dataset.

2. **[Dax_EmpCount_MissingTS]**: This represents the number of employees who are missing time stamps.

3. **[Dax_EmpCount] - [Dax_EmpCount_MissingTS]**: This part calculates the number of employees who have valid time stamps by subtracting the number of employees missing time stamps from the total employee count.

4. **DIVIDE(..., [Dax_EmpCount])**: The DIVIDE function takes the result from the previous step (employees with valid time stamps) and divides it by the total number of employees. This gives the proportion of employees with valid time stamps.

5. **... * 100**: Finally, multiplying by 100 converts the proportion into a percentage.

In summary, this measure calculates the percentage of employees who have valid time stamps, helping the business understand how many employees are properly accounted for in terms of time tracking. A higher percentage indicates better compliance with time tracking requirements.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked and contracted hours for employees or contractors. Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression finds the maximum number of contracted hours from the `vw_missing_timesheet` table. Contracted hours refer to the total hours that an employee or contractor is expected to work as per their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part finds the maximum number of hours actually recorded or worked by the employee or contractor from the same table.

3. **Subtraction**: The expression then subtracts the maximum recorded hours (actual hours worked) from the maximum contracted hours. 

**What it achieves**: The result of this calculation gives you the maximum number of hours that are missing or unaccounted for, meaning it shows how many hours an employee or contractor has not worked compared to what they were supposed to work according to their contract. 

In summary, this measure helps identify any discrepancies between expected and actual hours worked, which can be useful for managing workforce productivity and ensuring compliance with contractual obligations.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of completeness regarding hours worked versus expected hours for a specific time period, such as a week.

Here's a breakdown of what it does:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part sums up all the missing hours recorded in the `vw_missing_timesheet` table. Essentially, it totals how many hours were not logged or completed.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the expected number of hours that an employee should work in a week based on their contract.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: This calculation finds the difference between the total missing hours and the expected hours. If the result is positive, it indicates that the actual hours worked are less than expected.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The `DIVIDE` function takes the result from the previous step (the difference between missing hours and expected hours) and divides it by the expected hours. This gives a ratio or percentage. The third argument, `0`, ensures that if the expected hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

In summary, this measure calculates how much the actual hours worked fall short of the expected hours, expressed as a percentage of the expected hours. A negative result indicates that the employee has not met their expected hours, while a positive result shows how much they are lacking. This measure helps in assessing employee performance and identifying potential issues with time management or attendance.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of unique employees who have missing timesheets, grouped by their respective units and weeks. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The expression uses a table called `vw_missing_timesheet`, which likely contains records of employees and their timesheet submissions, including information about whether their submissions are complete or incomplete.

2. **Grouping Data**: The `SUMMARIZE` function is used to create a new table that groups the data by three key columns:
   - `Week_`: This represents the week of the year.
   - `EmployeeName`: This is the name of the employee.
   - `MAIN_UNIT`: This indicates the main unit or department the employee belongs to.

   For each unique combination of these three columns, the expression also calculates a new column called "IncompleteFlag". This flag indicates whether there is any incomplete timesheet for that specific employee in that week and unit, using the `MAX` function to check the highest value of `IncompleteFlag` (which would be 1 if there is at least one incomplete timesheet).

3. **Calculating Total**: After summarizing the data, the `SUMX` function iterates over the summarized table and sums up the values in the "IncompleteFlag" column. This effectively counts how many unique employee-week-unit combinations have at least one incomplete timesheet.

In summary, this DAX measure calculates the total count of unique employees who have not submitted complete timesheets, organized by week and unit. This information can help management identify areas where timesheet compliance may need improvement.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as the financial year, submission dates, and associated project and managerial information. This table enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet submission has been approved by management, facilitating tracking of employee work validation within the comprehensive timesheet records. |
| `Client` | `string` | The 'Client' column identifies the client associated with each timesheet entry, providing essential context for analyzing labor allocation and project costs within the 'vw_Timesheet' table. |
| `Code2` | `string` | The 'Code2' column stores a secondary identifier related to the timesheet entry, providing additional context for project categorization or employee classification within the comprehensive record of employee timesheet submissions. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column captures the date and time when an employee's contract concludes, providing critical information for analyzing labor allocation and project staffing within the 'vw_Timesheet' table. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column records the date and time when an employee's contract begins, providing essential context for analyzing timesheet submissions in relation to contract duration and project allocation within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of employee contracts as of today, providing insights into contract compliance and workforce availability within the context of timesheet submissions. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of employee contracts as of today, providing essential insights for analyzing labor allocation and project management within the timesheet records. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the specific date and time when an employee's contract was transferred, providing crucial context for timesheet submissions and enabling accurate tracking of labor allocation and project costs. |
| `CostAmount` | `double` | The 'CostAmount' column represents the monetary value associated with the hours worked by employees on specific projects, enabling businesses to assess labor costs and optimize resource allocation within the 'vw_Timesheet' table. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with labor costs for each timesheet entry, enabling businesses to assess project expenses and optimize resource allocation. |
| `Currency` | `string` | The 'Currency' column (string) in the 'vw_Timesheet' table indicates the type of currency used for financial transactions related to employee timesheet submissions, enabling accurate financial analysis and reporting. |
| `DebtorName` | `string` | The 'DebtorName' column captures the name of the client or entity responsible for payment, providing essential context for financial accountability and project cost analysis within employee timesheet submissions. |
| `EmployeeID` | `string` | The 'EmployeeID' column uniquely identifies each employee within the timesheet records, enabling accurate tracking of individual labor contributions and facilitating analysis of project costs and resource allocation. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column stores the unique identifier and name of the employee associated with each timesheet submission, enabling clear identification and tracking of labor contributions within the 'vw_Timesheet' records. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee within the 'vw_Timesheet' table, linking timesheet submissions to specific personnel for accurate labor allocation and project cost analysis. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees who submitted timesheets, providing a clear identification of labor contributions for accurate analysis of project costs and resource allocation. |
| `Employer` | `string` | The 'Employer' column identifies the organization or company that employs the individual submitting the timesheet, providing context for labor allocation and project cost analysis within the 'vw_Timesheet' table. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) uniquely identifies the employer associated with each timesheet submission, enabling accurate tracking of labor costs and resource allocation across different projects within the 'vw_Timesheet' table. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to the timesheet submission, providing further insights into the employee's work activities or project specifics that may not be captured in other fields. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table indicates the fiscal year during which the timesheet entries were submitted, enabling businesses to analyze labor costs and project allocations within specific financial periods. |
| `Hours` | `double` | The 'Hours' column (Data Type: double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee on a specific project during a given timesheet submission, enabling accurate analysis of labor allocation and project costs. |
| `HoursperWeek` | `int64` | The 'HoursperWeek' column (int64) represents the total number of hours an employee is allocated to work each week, providing critical insights into labor distribution and resource utilization within the organization. |
| `HoursType` | `string` | The 'HoursType' column categorizes the nature of hours recorded in the timesheet, distinguishing between regular, overtime, and other types of work hours to enhance analysis of labor allocation and project costs. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, providing essential context for analyzing labor allocation and associated costs. |
| `Index` | `int64` | The 'Index' column (int64) serves as a unique identifier for each entry in the 'vw_Timesheet' table, enabling efficient tracking and retrieval of individual employee timesheet submissions. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when each timesheet entry was submitted, providing a timestamp for tracking and auditing purposes within the employee timesheet records. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, linking employee timesheet entries to specific projects for enhanced tracking and analysis of labor allocation and project costs. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) uniquely identifies the internal project manager associated with each timesheet entry, enabling efficient tracking and management of project oversight and labor allocation. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Manager responsible for overseeing the timesheet submissions, providing critical managerial context for analyzing labor allocation and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the name of the IPM (Integrated Project Management) identifier associated with each timesheet entry, providing a reference for linking employee submissions to specific projects and enhancing project tracking and resource allocation analysis. |
| `JobTitle` | `string` | The 'JobTitle' column captures the specific title or role of the employee submitting the timesheet, providing context for labor allocation and project involvement within the 'vw_Timesheet' table. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier that links employee timesheet records to related datasets, ensuring accurate data integration and analysis across various business dimensions. |
| `ManagerID` | `string` | The 'ManagerID' column stores the unique identifier for the manager overseeing the employee's timesheet submissions, enabling effective tracking of managerial oversight and accountability in labor allocation and project management. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet submission, providing essential context for oversight and accountability in labor allocation and project management. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) identifies the unique key associated with the manager responsible for overseeing the employee's timesheet submissions, enabling effective tracking of managerial oversight and resource allocation within the organization. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct supervisor, providing essential managerial context for each timesheet entry to enhance oversight and facilitate effective resource management. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string values that represent the specific work bookings associated with employee timesheet submissions, providing insights into project allocations and labor distribution for effective resource management and cost analysis. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each timesheet entry, enabling analysis of labor allocation and project-related costs within the organization's financial framework. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with employee timesheet entries, enabling accurate tracking of labor allocation and project-related costs within the 'vw_Timesheet' table. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string representation of the specific project associated with each timesheet entry, providing essential context for analyzing labor allocation and project-related costs within the 'vw_Timesheet' table. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) uniquely identifies the specific project associated with each timesheet entry, enabling detailed analysis of labor allocation and project costs within the 'vw_Timesheet' table. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into the types of work being performed and aiding in the analysis of labor allocation and project costs. |
| `Rejected` | `boolean` | Indicates whether a timesheet submission has been rejected, providing critical insight into the approval status of employee hours logged for accurate labor cost analysis and project management. |
| `ReportReady` | `boolean` | The 'ReportReady' column indicates whether the timesheet data is finalized and ready for reporting, ensuring accurate and timely analysis of employee labor and project costs. |
| `SalesAmount` | `double` | The 'SalesAmount' column (Data Type: double) in the 'vw_Timesheet' table represents the total revenue generated from employee labor associated with specific projects, enabling businesses to assess financial performance and project profitability. |
| `SalesPrice` | `double` | The 'SalesPrice' column (Data Type: double) in the 'vw_Timesheet' table represents the monetary value assigned to the services rendered by employees, enabling analysis of labor costs in relation to project budgets and financial performance. |
| `Status` | `string` | The 'Status' column indicates the current state of each timesheet submission, reflecting whether it is pending, approved, or rejected, thereby aiding in the tracking and management of employee time reporting processes. |
| `Taxed` | `boolean` | Indicates whether the reported hours on the timesheet are subject to taxation, providing clarity on the financial implications of employee labor for accurate cost analysis and compliance. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or project complexity associated with each timesheet entry, aiding in the analysis of labor allocation and cost management across different project tiers. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column uniquely identifies each timesheet submission, allowing for efficient tracking and analysis of employee labor allocation and project costs within the 'vw_Timesheet' table. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column records the specific date and time when an employee's timesheet entry was submitted, providing a crucial timestamp for tracking labor allocation and project-related activities within the 'vw_Timesheet' table. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the work performed by the employee during the timesheet period, enhancing the understanding of labor allocation and project contributions for effective analysis and reporting. |
| `Unit` | `string` | The 'Unit' column represents the specific unit of measurement for time entries in the timesheet, allowing for accurate tracking and analysis of employee labor allocation across various projects. |
| `UnitCode` | `string` | The 'UnitCode' column represents a unique identifier for the organizational unit associated with each timesheet entry, enabling effective tracking and analysis of labor allocation across different departments or projects. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email address of the employee, facilitating communication regarding timesheet submissions and project-related inquiries within the 'vw_Timesheet' table. |
| `YearWeek` | `string` | The 'YearWeek' column (string) in the 'vw_Timesheet' table represents the specific year and week number of the timesheet submission, enabling detailed temporal analysis of employee labor allocation and project costs. |

##### Calculated Columns

**`Approved_With_V_Z`**

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheet submissions, specifically denoting whether the submission has been approved with a specific validation or review process, thereby aiding in tracking compliance and oversight within the timesheet management system.
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
   - It first checks if the value in the column `[Approved]` is `TRUE`. This means it is verifying if an item (like a timesheet or request) has been approved.
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means it is looking for specific codes that might represent certain types of entries or statuses.

2. **Output Values**: 
   - If either of the conditions is met (meaning the item is approved or has a TimesheetCode of "V" or "Z"), the expression returns a value of `1`. This indicates a positive outcome, suggesting that the item is considered approved in some context.
   - If neither condition is met, it returns a value of `0`. This indicates a negative outcome, suggesting that the item is not approved.

3. **Purpose**: The overall purpose of this calculated column is to create a simple binary indicator (1 or 0) that helps identify whether a timesheet or entry is approved based on the approval status or specific codes. This can be useful for reporting, filtering, or further analysis in a business context.

In summary, this DAX expression helps to flag entries as approved based on either their approval status or specific codes, making it easier to manage and analyze timesheet data.

**`BillableDep`**

- **Description:** The 'BillableDep' column indicates the department responsible for billing associated with the timesheet entry, providing insights into departmental cost allocation and project financial management.
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

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of a specific billable dependency, the default assumption is that it is billable.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that there is a billable dependency present.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that there is no billable dependency.

In summary, this DAX expression effectively categorizes each entry based on whether it has a billable dependency or not. It assigns a value of **1** if there is a billable dependency (either because it's blank or has a non-zero value) and a value of **0** if the dependency is explicitly zero. This helps in identifying which entries are considered billable based on their related data.

**`BillablePrj`**

- **Description:** The 'BillablePrj' column identifies the project associated with billable hours recorded in the timesheet, enabling accurate tracking of labor costs and project profitability.
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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'BillablePrj' in a data model, specifically for a table called `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether a particular entry in the timesheet is considered "billable" based on certain conditions.

2. **Conditions Checked**:
   - **Sales Price**: It first checks if the `SalesPrice` for the entry is greater than 0. This means that if there is a positive sales price, the entry is likely billable.
   - **Project Profile Code**: It also checks if the `ProjectProfileCode` is equal to 81. If it is, this specific project is predefined as billable regardless of other factors.
   - **Project Name**: Lastly, it looks for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it indicates that the project is related to customer success services, which are also considered billable.

3. **Output**: 
   - If any of the above conditions are met (i.e., if the sales price is positive, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of **1**. This signifies that the entry is billable.
   - If none of these conditions are met, it returns a value of **0**, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or not based on specific criteria related to sales price, project profile, and project name, helping the business identify which projects can be charged to clients.

**`cc_Employer`**

- **Description:** The 'cc_Employer' column identifies the employer associated with the employee's timesheet submission, providing crucial context for analyzing labor allocation and project costs within the 'vw_Timesheet' table.

**`Employee_WeeklyHours`**

- **Description:** The 'Employee_WeeklyHours' column captures the total hours worked by each employee during the week, represented as a string, providing critical insights into labor allocation and productivity for effective project and resource management.
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

3. **Combining Information**: The expression then combines these two pieces of information into one string. It formats it so that the employee's name is followed by " - " and then the number of hours they work each week.

For example, if an employee named "John Doe" works 40 hours per week, the resulting string in the 'Employee_WeeklyHours' column would be "John Doe - 40".

In summary, this DAX expression helps to create a clear and concise representation of each employee's name along with their weekly working hours, making it easier to read and understand the data at a glance.

**`GroupCat`**

- **Description:** The 'GroupCat' column categorizes timesheet entries into specific groups, aiding in the analysis of labor allocation and project costs within the 'vw_Timesheet' table.
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

1. **Lookup for Group**: The expression first attempts to find a corresponding 'Group' value from a lookup table called `lkp_Project`. It does this by looking for a match between the 'PROJECT' field in the `lkp_Project` table and the 'Project' field in the `vw_Timesheet` table.

2. **Check for Existence**: The `NOT(ISBLANK(...))` part checks if the lookup returned a valid 'Group' value. If it finds a match (meaning the project exists in the lookup table), it will use that 'Group' value.

3. **Assigning Values**: 
   - If a valid 'Group' is found, the calculated column 'GroupCat' will take that 'Group' value.
   - If no valid 'Group' is found (meaning the project does not exist in the lookup table), it checks if the project is billable by looking at the 'BillablePrj' field in the `vw_Timesheet` table.
     - If 'BillablePrj' equals 1 (indicating that the project is billable), it assigns the value "Billable" to 'GroupCat'.
     - If 'BillablePrj' does not equal 1 (indicating that the project is not billable), it assigns the value "Other Unbillable" to 'GroupCat'.

**In summary**, this DAX expression categorizes each project in the timesheet into a group based on its existence in a lookup table. If the project is found, it uses the associated group name; if not, it determines if the project is billable or unbillable and assigns the appropriate label accordingly. This helps in organizing and analyzing projects based on their billing status and group categorization.

**`IsContractActive`**

- **Description:** The 'IsContractActive' column indicates whether the employee's contract is currently active, providing critical context for timesheet submissions and ensuring accurate labor cost analysis and project resource allocation.
- **DAX Expression:**
```dax
IF(
			    ISBLANK('vw_Timesheet'[ContractEndDate]) || 'vw_Timesheet'[ContractEndDate] > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'IsContractActive' in a data table called 'vw_Timesheet'. Here's what it does in simple business terms:

1. **Checks Contract End Date**: The expression first checks if the 'ContractEndDate' for each record (or row) in the 'vw_Timesheet' table is either blank (meaning there is no end date specified) or if the end date is in the future (greater than today's date).

2. **Determines Contract Status**:
   - If either of those conditions is true (the contract has no end date or it ends in the future), the expression labels the contract as "Active".
   - If neither condition is true (meaning there is a specified end date that is in the past), it labels the contract as "Not Active".

In summary, this calculated column helps identify whether a contract is currently active or not based on its end date, providing valuable information for managing contracts effectively.

**`MAIN_UNIT`**

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit associated with each timesheet entry, providing context for labor allocation and project management within the 'vw_Timesheet' table.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression starts by checking if there is a related value in the 'lkp_Unit' table for the 'Unit' column. This is done using the `RELATED` function, which pulls in data from a related table based on existing relationships in the data model.

2. **Handle Missing Data**: The `ISBLANK` function checks if the value retrieved from the 'lkp_Unit[Unit]' column is blank (i.e., there is no corresponding unit found). 

3. **Return Values Based on Check**:
   - If the value is blank (meaning there is no related unit), the expression returns the string "Unknown". This indicates that the unit information is not available for that particular record.
   - If there is a related unit (i.e., the value is not blank), it returns the actual unit name from the 'lkp_Unit[Unit]' column.

In summary, this DAX expression effectively populates the 'MAIN_UNIT' column with either the name of the unit associated with each record or "Unknown" if no unit is linked. This helps in identifying records that lack unit information, ensuring clarity in reporting and analysis.

**`MonthNumber`**

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month in which timesheet entries were submitted, aiding in the organization and analysis of labor data across different periods.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to extract the month number from a date value found in the column named `TimesheetDate`. 

In simple business terms, this means that for each entry in the `TimesheetDate` column, the expression will return a number that represents the month of that date. For example, if the date is January 15, 2023, the expression will return `1`, since January is the first month of the year. If the date is July 10, 2023, it will return `7`, as July is the seventh month.

This calculated column helps in analyzing data by month, allowing businesses to easily group or filter timesheet entries based on the month they occurred in.

**`NR_EMP_COLUMN`**

- **Description:** The 'NR_EMP_COLUMN' stores the unique identification number of the employee associated with each timesheet entry, enabling precise tracking of labor contributions and facilitating analysis of employee-specific project involvement and resource allocation.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

In simple business terms, this means that the expression counts how many different employees have recorded their time in the timesheet, without counting any employee more than once. For example, if three employees submitted timesheets and one of them submitted multiple entries, this calculation would still only count that employee once, resulting in a total of three unique employees.

This is useful for understanding workforce participation, tracking how many distinct employees are working on a project, or analyzing employee engagement over a specific period.

**`Own-Sub-ExtT`**

- **Description:** The 'Own-Sub-ExtT' column captures the type of timesheet submission, indicating whether it is an internal, subcontracted, or external entry, thereby aiding in the analysis of labor allocation and project cost management.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used to retrieve a specific value from a related table in a data model. Here’s a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a calculated column within a table that has a relationship with another table (in this case, `lkp_Unit`).

2. **Purpose**: The expression fetches the value from the `OWN-Sub-ExtT` column in the `lkp_Unit` table. 

3. **How it Works**: 
   - The `RELATED` function looks for a matching row in the `lkp_Unit` table based on the existing relationship between the two tables.
   - It then retrieves the value from the `OWN-Sub-ExtT` column for that matching row.

4. **Outcome**: The result is that for each row in the current table where this calculated column is being created, you will get the corresponding `OWN-Sub-ExtT` value from the `lkp_Unit` table. This allows you to enrich your data with additional information from a related source.

In summary, this DAX expression helps to pull in relevant data from another table, enhancing the insights you can derive from your dataset by linking related information together.

**`QualifyPrj`**

- **Description:** The 'QualifyPrj' column indicates the qualification status of the associated project, providing insights into project eligibility and compliance for financial analysis and resource allocation within employee timesheet submissions.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'QualifyPrj'. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The expression first checks if the 'Qualify' field from a related table called 'lkp_Project' is blank (i.e., has no value). This is done using the `ISBLANK` function.

2. **Return Values Based on the Check**:
   - If the 'Qualify' value is blank, the expression returns a value of **1**. This could indicate a default or fallback status when there is no specific qualification available.
   - If the 'Qualify' value is not blank, it returns the actual value from the 'Qualify' field in the 'lkp_Project' table.

3. **Purpose**: The overall purpose of this calculated column is to ensure that every record has a value for 'QualifyPrj'. If there is no qualification available, it assigns a default value of 1. This helps maintain consistency in the data and ensures that subsequent analyses or calculations can proceed without missing values.

In summary, this DAX expression effectively fills in gaps in the 'Qualify' data by providing a default value when necessary, ensuring that all projects have a qualification status.

**`ReportingEntity`**

- **Description:** The 'ReportingEntity' column identifies the specific organizational unit or department responsible for the timesheet submission, enabling accurate tracking of labor costs and resource allocation within the business.

**`RReady_With_V_Z`**

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of timesheet submissions, providing a string value that reflects whether the submission has been verified and is ready for review or processing within the context of employee timesheet records.
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
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These are specific codes that likely represent certain types of timesheets or statuses.

2. **Output Values**: 
   - If either of the conditions is met (i.e., if the report is ready or if the timesheet code is "V" or "Z"), the expression returns a value of `1`. This indicates a positive outcome, suggesting that the item is ready for further processing or meets the criteria.
   - If neither condition is met, it returns a value of `0`. This indicates a negative outcome, suggesting that the item does not meet the criteria for being considered ready.

In summary, this DAX expression effectively flags records as "ready" (1) or "not ready" (0) based on whether the report is ready or if the timesheet code is one of the specified values ("V" or "Z"). This can help in filtering or analyzing data based on readiness for reporting or processing.

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
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'ContractStatus2' that evaluates the status of a contract and returns different results based on that status.

Here's a breakdown of what it does:

1. **Condition Check**: The expression first checks the value of another measure called `[ContractStatusMeasure]`. It specifically looks to see if this measure equals "Active".

2. **Return Value if Active**: If the contract status is "Active", the expression returns the value of another measure called `[HoursDifference]`. This could represent the number of hours related to the active contract, such as hours worked or hours remaining.

3. **Return Value if Not Active**: If the contract status is anything other than "Active", the expression returns the text "Not Active". This indicates that the contract is not currently active.

In summary, this DAX measure helps to determine whether a contract is active or not and provides relevant information accordingly. If the contract is active, it shows the hours associated with it; if not, it simply states that the contract is not active. This can be useful for reporting and decision-making regarding contract management.

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

1. **Check for End Date**: The expression first looks at the maximum value of the 'ContractEndDate' from the 'vw_Timesheet' table. This means it is checking the latest contract end date available in the data.

2. **Evaluate Conditions**:
   - **ISBLANK**: It checks if this maximum end date is blank (meaning there is no end date recorded).
   - **Comparison with Today**: It also checks if this maximum end date is greater than today's date (meaning the contract is still ongoing).

3. **Determine Status**:
   - If either of the above conditions is true (either there is no end date or the end date is in the future), the measure returns "Active". This indicates that the contract is currently valid and ongoing.
   - If neither condition is true (meaning there is an end date that has already passed), it returns "Not Active". This indicates that the contract has ended.

In summary, this measure effectively categorizes contracts as either "Active" or "Not Active" based on their end dates, helping businesses quickly assess the status of their contracts.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 15, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated starting from the first week of the year.

So, when you combine these two functions, `WEEKNUM(TODAY())` will give you the week number for the current date. 

For instance, if today is in the 42nd week of the year, this expression will return the number 42. This is useful for reporting and analysis purposes, as it allows businesses to track performance or activities on a weekly basis throughout the year.

**`Dax_EmpCount_Approved`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]),
			    vw_Timesheet[Approved] = FALSE()
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'Dax_EmpCount_Approved'. Here's a breakdown of what it does in simple business terms:

1. **CALCULATE Function**: This function changes the context in which data is evaluated. It allows you to apply filters to your calculations.

2. **DISTINCTCOUNT Function**: This part of the expression counts the number of unique entries in a specified column. In this case, it counts the unique names of employees.

3. **vw_Timesheet[EmployeeName]**: This specifies the column from which the unique employee names are being counted. It comes from a table called 'vw_Timesheet'.

4. **Filter Condition**: The expression includes a filter condition that specifies only to count employees whose timesheets are not approved. This is indicated by `vw_Timesheet[Approved] = FALSE()`, meaning it looks for entries where the 'Approved' status is marked as false (or not approved).

### Summary:
In summary, this DAX measure calculates the number of unique employees who have timesheets that are not approved. It helps in understanding how many employees have pending approvals for their submitted timesheets.

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

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it counts the unique names of employees from the `vw_Timesheet` table.

3. **Filter Condition**: The expression includes a filter that specifies only to consider those records where the `ReportReady` column is set to FALSE. This means it will only count employees whose timesheets are not ready for reporting.

In summary, this measure calculates the total number of distinct employees who have timesheets that are still pending or not ready for submission. This can be useful for tracking which employees need to complete their timesheets before they can be reported.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees recorded in the `vw_Timesheet` table.

Here's a breakdown of what it does:

- **DISTINCTCOUNT**: This function counts the number of distinct (unique) values in a specified column. In this case, it is counting unique entries.
  
- **vw_Timesheet[EmployeeName]**: This specifies the column from which the unique values are being counted. It refers to the `EmployeeName` column in the `vw_Timesheet` table.

In simple business terms, this measure tells you how many different employees have submitted timesheets. If an employee appears multiple times in the timesheet records, they will only be counted once. This is useful for understanding workforce participation or tracking how many individual employees are involved in a project or task over a specific period.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_%_Budget_Used`**

- **DAX Expression:**
```dax
DIVIDE(SUM(vw_Timesheet[SalesAmount]), SUM('tbl_Project'[SalesPriceBaseCurrency]), 0)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of the budget that has been used based on sales data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part adds up all the sales amounts recorded in the 'vw_Timesheet' table. Essentially, it totals the revenue generated from sales.

2. **SUM('tbl_Project'[SalesPriceBaseCurrency])**: This part sums up the total budget or sales price from the 'tbl_Project' table, which represents the total amount that was planned or budgeted for the projects.

3. **DIVIDE(..., ..., 0)**: The DIVIDE function takes the total sales amount (from the first part) and divides it by the total budget (from the second part). If the budget is zero (to avoid division by zero), it returns 0 instead of an error.

In simple terms, this measure calculates how much of the budget has been used by comparing actual sales to the planned budget. The result is a ratio that shows the percentage of the budget that has been utilized based on the sales achieved. This helps businesses understand their performance against their financial plans.

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** This DAX expression calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the `vw_Timesheet` table. Essentially, it totals the revenue generated from sales.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the same `vw_Timesheet` table. It gives the total number of hours that have been logged.

3. **Division**: The expression then divides the total sales amount by the total hours worked. 

**What it achieves**: The result of this calculation is the average hourly rate, which tells you how much revenue is generated per hour of work. This metric can be useful for assessing productivity, understanding labor costs, and making informed business decisions regarding pricing and resource allocation.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate a measure called 'Msr_Budget' that dynamically determines the budget value based on the current week in the year. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Current Week**: The code starts by capturing the current week number from a date dimension table (DimDate). This is done using the `SELECTEDVALUE` function, which retrieves the week number that is currently selected in the report or dashboard.

2. **Calculate Previous Budget**: Next, the code calculates the budget value for the most recent week prior to the current week. It does this by:
   - Using the `CALCULATE` function to evaluate the measure `[Msr_SalesPriceBaseCurrency]`, which presumably represents the sales price in the base currency.
   - The `FILTER` function is applied to the date dimension to include only those weeks that are less than the current week. The `ALLSELECTED` function ensures that any filters applied to the date dimension are respected, but it allows for the calculation to consider all weeks up to the current one.

3. **Return the Appropriate Value**: Finally, the code checks if the current measure `[Msr_SalesPriceBaseCurrency]` has a value (i.e., it is not blank). If it does have a value, that value is returned as the budget for the current week. If it does not have a value (meaning there is no budget set for the current week), the measure returns the previously calculated budget value from the most recent week.

In summary, this DAX measure effectively provides a way to display the current week's budget if it exists; if not, it falls back to the last available budget from the previous weeks. This ensures that users always see a relevant budget figure, enhancing decision-making based on the most recent data available.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the cumulative cost up to a selected month in a reporting visual. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It uses the `MAX` function on the `MonthNumberOfYear` from the `DimDate` table to find the highest month number that has been selected. This means if you are looking at data for March, the `SelectedMonth` would be 3.

2. **Calculate Cumulative Cost**: The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to modify the context in which the data is evaluated. It sums up the `CostAmount` from the `vw_Timesheet` table.

4. **Filtering the Data**: Inside the `CALCULATE`, there is a `FILTER` function that adjusts the data being summed. It uses `ALLSELECTED('DimDate')` to consider all the dates that are currently selected in the visual, but then it applies a condition to only include months that are less than or equal to the `SelectedMonth`. This means if you are looking at March, it will include costs from January, February, and March.

5. **Final Result**: The final output of this measure is the total cost from the start of the year up to the month that is currently selected in the visual. This allows users to see how costs accumulate over time, providing valuable insights into spending trends.

In summary, this DAX measure helps users track cumulative costs over time, making it easier to analyze financial performance month by month.

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

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It uses the `MAX` function on the `MonthNumberOfYear` column from the `DimDate` table to find the highest month number that has been selected. This means if you are looking at data for March, `SelectedMonth` would be 3.

2. **Calculate Cumulative Sales**: The main goal of this measure is to calculate the total sales amount from the `vw_Timesheet` table for all months that are less than or equal to the selected month. 

3. **Using CALCULATE and FILTER**: 
   - The `CALCULATE` function is used to change the context in which the data is evaluated. 
   - Inside `CALCULATE`, the `SUM` function adds up the `SalesAmount` from the `vw_Timesheet` table.
   - The `FILTER` function is applied to the `DimDate` table to ensure that only the months up to and including the selected month are considered. The `ALLSELECTED` function allows the measure to respect any filters that might be applied to the date context in the report.

4. **Final Outcome**: The result of this measure is the total sales amount for all months leading up to and including the month that the user has selected in the visual. This helps in understanding how sales have accumulated over time up to a specific point in the year.

In summary, this DAX measure provides insights into cumulative sales performance, allowing users to see how sales have built up over the months leading to the selected month.

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
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of hours worked by a specific group, project, or employee compared to the total hours worked across all selected categories in a report or dashboard.

Here's a breakdown of what it does:

1. **TotalValue Calculation**: 
   - The first part of the code defines a variable called `TotalValue`. This variable calculates the total number of hours worked by summing up the `Hours` column from the `vw_Timesheet` table.
   - The `CALCULATE` function is used here to modify the context of the calculation. It considers all selected filters for `GroupCat`, `Project`, and `EmployeeName`, meaning it looks at the total hours worked across these categories, regardless of any other filters that might be applied in the report.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours worked for the current context (which could be a specific group, project, or employee) by dividing the sum of hours for that context (`SUM(vw_Timesheet[Hours])`) by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safer way to handle division in DAX. If the `TotalValue` is zero (meaning no hours were recorded), it will return 0 instead of causing an error.

In summary, this measure provides insight into how much of the total hours worked is attributed to a specific group, project, or employee, expressed as a percentage. This can help in understanding workload distribution and performance across different categories in the organization.

**`MSR_PROJECT_%_REALIZATION`**

- **DAX Expression:**
```dax
IF(sum(tbl_Project[SalesPriceBaseCurrency]) <> 0, sum(vw_Timesheet[SalesAmount])/sum(tbl_Project[SalesPriceBaseCurrency]))
```
- **DAX Explanation (Generated):** This DAX expression is used to calculate a measure called 'MSR_PROJECT_%_REALIZATION'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The measure calculates the percentage of sales realization for a project. In other words, it shows how much revenue has been generated from the project compared to the total expected sales price.

2. **Components**:
   - **`sum(tbl_Project[SalesPriceBaseCurrency])`**: This part sums up the total expected sales price for the project. It represents the total amount that the project is supposed to earn.
   - **`sum(vw_Timesheet[SalesAmount])`**: This part sums up the actual sales amount that has been recorded from the project. It reflects the revenue that has been realized so far.

3. **Condition**: The `IF` statement checks if the total expected sales price (from the first part) is not equal to zero. This is important because if the expected sales price is zero, dividing by zero would lead to an error.

4. **Calculation**: If the total expected sales price is not zero, the measure calculates the percentage of realization by dividing the actual sales amount (from the timesheet) by the total expected sales price. This gives a ratio that indicates how much of the expected revenue has actually been achieved.

5. **Result**: The final output of this measure is a percentage that helps project managers and stakeholders understand the financial performance of the project. A higher percentage indicates better realization of expected sales, while a lower percentage suggests that the project may not be performing as anticipated.

In summary, this DAX expression helps assess how effectively a project is generating revenue compared to its sales expectations, providing valuable insights for decision-making and performance evaluation.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the profit margin for a project by subtracting the total costs from the total sales. Here's a breakdown of what it does:

1. **sum(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the `vw_Timesheet` table. Essentially, it calculates the total revenue generated from the project.

2. **sum(vw_Timesheet[CostAmount])**: This part sums up all the cost amounts associated with the project from the same table. It represents the total expenses incurred to deliver the project.

3. **Subtraction**: The expression then subtracts the total costs from the total sales. This gives you the project's profit margin, which indicates how much money is left after covering the costs.

In simple terms, this DAX measure helps you understand how profitable a project is by showing the difference between what you earned (sales) and what you spent (costs). A positive result indicates a profit, while a negative result indicates a loss.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called `Msr_SalesPriceBaseCurrency`, which essentially sums up the sales prices in a specific currency for different projects and clients. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The expression starts by looking at a table called `vw_Timesheet`, which likely contains records related to projects and clients.

2. **Grouping**: It uses the `SUMMARIZE` function to group the data by two key dimensions: `ProjectProfile` and `Client`. This means it organizes the data so that each unique combination of project and client is considered separately.

3. **Measure Calculation**: For each of these unique combinations, it creates a new column called `MeasureValue`. This column retrieves the sales price in the base currency from another table called `tbl_Project` using the `SELECTEDVALUE` function. This function gets the current sales price for the project being evaluated.

4. **Summation**: Finally, the `SUMX` function takes all the `MeasureValue` entries generated from the previous step and sums them up. This gives the total sales price in the base currency across all the grouped project and client combinations.

In summary, this DAX expression calculates the total sales price in the base currency for all projects and clients listed in the `vw_Timesheet`, providing a clear view of revenue or sales performance in a standardized currency.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of unique employees who have a "Valid" contract status on a given day. Here's a breakdown of what each part does:

1. **CALCULATE**: This function changes the context in which data is evaluated. It allows us to apply specific filters to our calculation.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it ensures that each employee is only counted once, even if they appear multiple times in the data.

3. **ALL(vw_Timesheet)**: This function removes any existing filters that might be applied to the `vw_Timesheet` table. By doing this, it ensures that the calculation considers all records in the table, rather than just those that might be filtered by other criteria in the report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This is a filter condition that specifies we only want to count employees whose contract status is "Valid" on the current day.

In summary, this DAX expression calculates the total number of unique employees who currently have a valid contract, ignoring any other filters that might be applied to the data. This is useful for understanding the active workforce under valid contracts at any given time.

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

In simple terms, this measure totals all the hours worked by employees as recorded in the timesheet, providing a single number that represents the overall actual hours worked. This can be useful for reporting, budgeting, or analyzing labor costs.

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

2. **MAX(vw_Timesheet[HoursPerWeek])**: For each employee in the unique list, this part finds the maximum number of hours they are contracted to work in a week. If an employee has multiple entries, it will take the highest value of hours recorded.

3. **SUMX(...)**: This function then takes the unique employee list and iterates over it. For each employee, it calculates the maximum hours per week and sums these maximum values together.

In summary, the entire expression calculates the total of the highest contracted hours per week for all employees listed in the timesheet. This gives a clear view of the total contracted hours across the workforce, ensuring that if any employee has multiple records, only their highest contracted hours are counted.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression `[TotalActualHours] - [TotalContractedHours]` calculates the difference between two values: the total actual hours worked and the total contracted hours agreed upon.

In simple business terms:

- **TotalActualHours**: This represents the actual number of hours that have been worked by employees or resources on a project or task.
- **TotalContractedHours**: This is the number of hours that were originally agreed upon in a contract for the project or task.

By subtracting the total contracted hours from the total actual hours, this measure provides insight into how many hours have been worked beyond or below what was initially planned. 

- If the result is positive, it indicates that more hours were worked than contracted, which could suggest overwork or additional effort.
- If the result is negative, it indicates that fewer hours were worked than contracted, which might suggest underperformance or efficiency.

Overall, this measure helps businesses understand their resource utilization and manage their projects more effectively.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'TotalHoursTracked' that calculates the total number of hours tracked in a timesheet. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The expression first checks if the total sum of hours from the `vw_Timesheet` table is blank (i.e., there are no hours recorded). This is done using the `ISBLANK` function.

2. **Return Zero if Blank**: If the sum of hours is blank (meaning no hours have been tracked), the expression returns a value of 0. This ensures that when there are no recorded hours, the measure will still provide a meaningful output instead of showing a blank or an error.

3. **Return the Sum if Not Blank**: If there are hours recorded (i.e., the sum is not blank), the expression simply returns the total sum of hours tracked.

In summary, this measure effectively calculates the total hours tracked in the timesheet, ensuring that if no hours are recorded, it returns 0 instead of a blank value. This makes it easier to interpret the data in reports and dashboards, as it provides a clear numerical output regardless of whether hours have been logged.

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

3. **IF(...)**: This function evaluates the condition provided by ISBLANK. If the total hours are blank (i.e., no hours have been tracked), it returns the text "Empty". If there are hours recorded, it returns the total sum of those hours.

In summary, this DAX expression effectively checks if any hours have been logged in the timesheet. If there are no hours, it returns "Empty" to indicate that no tracking has occurred. If there are hours, it provides the total number of hours tracked. This is useful for reporting and understanding whether time has been logged or if there are gaps in tracking.

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

1. **Total Hours Tracked**: The expression starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked for a specific employee or group of employees.

2. **Maximum Hours Per Week**: The second part of the expression uses the `CALCULATE` function to determine the maximum number of hours that have been recorded in a week for each employee. It does this by:
   - Using `MAXX` to find the highest value of `HoursPerWeek` from a distinct list of hours recorded in the `vw_Timesheet` table.
   - The `ALLEXCEPT` function ensures that this calculation is done while keeping the context of the specific employee (identified by `EmployeeID`). This means it looks at the maximum hours for each employee individually, ignoring any other filters that might be applied to the data.

3. **Calculating the Difference**: Finally, the expression subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. 

### What It Achieves:
In summary, this measure calculates how many hours an employee has tracked beyond their maximum recorded hours in a week. If the result is positive, it indicates that the employee has logged more hours than their maximum weekly average, which could suggest overtime or extra effort. If the result is negative or zero, it means the employee has not exceeded their typical weekly hours. This measure can help managers understand employee workload and productivity.

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
- **DAX Explanation (Generated):** The DAX expression you've provided calculates a measure called `trackedDiff2`, which essentially determines the difference between the total hours tracked for a specific employee and the maximum hours they have recorded in a week, while ignoring any filters applied to the hours per week.

Here's a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts by referencing another measure called `[TotalHoursTracked]`, which represents the total number of hours an employee has logged over a certain period.

2. **Maximum Hours Per Week**: The next part of the expression calculates the maximum number of hours that the employee has recorded in any single week. This is done using the `MAXX` function, which looks at the distinct values of hours per week from the `vw_Timesheet` table.

3. **Removing Filters**: The `REMOVEFILTERS` function is used to ensure that any filters that might be applied to the `HoursPerWeek` column do not affect the calculation of the maximum hours. This means that the calculation will consider all weeks, regardless of any current filters.

4. **Keeping Employee Context**: The `ALLEXCEPT` function is used to maintain the context of the specific employee (identified by `EmployeeID`). This means that while the calculation ignores filters on hours, it still focuses on the data relevant to the particular employee being analyzed.

5. **Final Calculation**: Finally, the measure subtracts the maximum hours per week from the total hours tracked. The result shows how many hours the employee has logged beyond their maximum weekly hours, which can indicate overwork or discrepancies in time tracking.

In summary, `trackedDiff2` calculates how many hours an employee has worked beyond their highest recorded weekly hours, providing insights into their workload and time management.

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

1. **Variable Definition (FilteredHours)**: The expression starts by creating a variable called `FilteredHours`. This variable holds a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**: The filtering is based on two main conditions:
   - The project qualifies as billable if `vw_Timesheet[QualifyPrj]` equals 1 (indicating it's a qualified project) **and** `vw_Timesheet[BillablePrj]` equals 1 (indicating it's a billable project).
   - Alternatively, it includes any projects that contain the phrase "Customer Success Services" in their name. This is checked using the `SEARCH` function, which looks for that specific text within the `vw_Timesheet[Project]` field.

3. **Summing Up Hours**: After filtering the timesheet data based on the above criteria, the expression uses `SUMX` to iterate over the filtered results. It sums up the `vw_Timesheet[Hours]` for all the entries that meet the filtering conditions.

4. **Final Output**: The result of this measure, `UTI_TotalBillableHours`, is the total number of hours that are considered billable based on the specified criteria. This helps the business understand how many hours worked can be charged to clients, which is crucial for revenue tracking and project management.

In summary, this DAX measure calculates the total billable hours from a timesheet, focusing on qualified and billable projects, as well as specific customer success projects, providing valuable insights into the company's billable work.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** This DAX expression is used to calculate the total number of hours worked on specific projects that qualify under certain criteria. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the values in the "Hours" column from the "vw_Timesheet" table. Essentially, it totals the hours worked.

2. **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to include only those entries where the "QualifyPrj" column equals 1. This means it focuses on projects that meet a specific qualification or criteria.

3. **CALCULATE**: This function modifies the context in which the data is evaluated. It takes the total hours calculated in the first part and applies the filter from the second part.

In simple terms, this DAX measure calculates the total hours worked on projects that are marked as qualifying (where "QualifyPrj" is 1). It helps businesses understand how many hours are being spent on projects that meet certain standards or requirements.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the "Billable Hour Ratio" for active employees within a business context. Here's a breakdown of what it does in simple terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable captures the total number of billable hours worked by employees. It pulls this value from another measure called `[UTI_TotalBillableHours]`.
   - `_TotalHours`: This variable captures the total hours worked by employees, sourced from another measure called `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The expression checks if there are any active contacts (employees) using `[Msr_ActiveContacts]`. If there are no active employees, the calculation will not proceed.
   - Next, it checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the calculation will not continue.
   - If both conditions are met, it calculates the ratio of billable hours to total hours. This is done using the `DIVIDE` function, which safely divides `_TotalBillableHours` by `_TotalHours`. If the result of this division is blank (which can happen if `_TotalHours` is zero), it returns 0 instead of an error.

3. **Outcome**:
   - The final output of this measure is the Billable Hour Ratio, which indicates the proportion of hours that were billable compared to the total hours worked by active employees. This ratio is crucial for understanding how effectively employee time is being utilized in generating revenue.

In summary, this DAX measure helps businesses assess the productivity of their active employees by calculating how much of their working time is spent on billable tasks, providing insights into operational efficiency.

---

