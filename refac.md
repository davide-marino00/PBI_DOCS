# Power BI Model Documentation

*Generated on: 2025-04-28 15:40:07*

## Table of Contents

- [Overview](#overview)
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

The Power BI data model appears to be designed for a business domain focused on project management and timesheet tracking, likely within a professional services or consulting environment. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a strong emphasis on monitoring employee hours worked against various projects, units, and codes, which are essential for resource allocation, billing, and performance analysis. The inclusion of lookup tables like 'lkp_Project', 'lkp_Unit', and 'lkp_Code' indicates a structured approach to categorizing and managing data related to projects, organizational units, and timesheet codes, facilitating detailed reporting and insights.

The 'DimDate' table serves as a critical dimension for time-based analysis, allowing users to track timesheet entries over different periods, including contract start and end dates. The relationships established between these tables enable comprehensive reporting capabilities, such as identifying missing timesheets, analyzing hours worked by project or unit, and calculating performance metrics. Overall, this data model is likely aimed at enhancing operational efficiency, ensuring accurate project tracking, and supporting strategic decision-making through data-driven insights.

---

## <a name="relationships"></a>Relationships

The following relationships were identified:

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

## <a name="tables"></a>Tables

### <a name="table-dimdate"></a>Table: `DimDate`

The 'DimDate' table serves as a comprehensive date dimension, providing essential temporal attributes that enable businesses to analyze and report on data across various time periods. By offering detailed breakdowns of dates, including day, week, month, and quarter information, this table supports effective time-based analysis and enhances reporting capabilities in Power BI.

#### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the specific quarter of the year (e.g., Q1, Q2, Q3, Q4) associated with each date, facilitating time-based analysis and reporting by allowing businesses to group and compare data across quarterly periods. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the year component of each date, facilitating year-over-year analysis and reporting for enhanced temporal insights in business intelligence applications. |
| `DateKey` | `int64` | The 'DateKey' column (int64) uniquely identifies each date in the 'DimDate' table, facilitating precise time-based analysis and reporting across various periods in business intelligence applications. |
| `DayName` | `string` | The 'DayName' column contains the full name of the day (e.g., Monday, Tuesday) corresponding to each date, facilitating day-of-week analysis and enhancing temporal insights for reporting and decision-making. |
| `DayNumberOfMonth` | `int64` | The 'DayNumberOfMonth' column (int64) represents the specific day of the month for each date entry in the 'DimDate' table, facilitating detailed time-based analysis and reporting by allowing users to easily identify and categorize data according to individual days within each month. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) in the 'DimDate' table represents the numerical day of the week, ranging from 1 (Sunday) to 7 (Saturday), facilitating time-based analysis and reporting by allowing businesses to easily categorize and aggregate data according to weekly cycles. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column represents the sequential day of the year as an integer, facilitating time-based analysis by allowing users to easily identify and compare specific days within the annual calendar. |
| `MonthName` | `string` | The 'MonthName' column contains the full name of each month, facilitating easy identification and analysis of data trends and patterns across different months within the comprehensive date dimension. |
| `MonthNumberOfYear` | `int64` | The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month (1 for January through 12 for December), facilitating time-based analysis and reporting by allowing users to easily categorize and filter data according to specific months. |
| `ShortDate` | `dateTime` | The 'ShortDate' column represents a simplified date format that facilitates quick reference and analysis of specific dates within the comprehensive date dimension, enhancing time-based reporting and insights in business intelligence applications. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number within the calendar year, facilitating time-based analysis and reporting by allowing businesses to aggregate and compare data on a weekly basis. |

#### Calculated Columns

**`CurrentYearFlag`**

- **Description:** The 'CurrentYearFlag' column indicates whether a given date falls within the current calendar year, facilitating time-based analysis and reporting by allowing users to easily filter and identify data relevant to the present year.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CurrentYearFlag' in a data model, specifically in a date dimension table (DimDate). Here's what it does in simple business terms:

1. **Extract Year from Date**: The expression takes a date from the column `DimDate[ShortDate]` and extracts the year from it using the `YEAR` function.

2. **Compare with Current Year**: It then compares this extracted year to the current year, which is obtained using the `YEAR(TODAY())` function. `TODAY()` gives the current date, and `YEAR(TODAY())` gives the year part of that date.

3. **Flagging**: If the year from `DimDate[ShortDate]` is less than or equal to the current year, the expression returns a value of `1`. This indicates that the date is either in the current year or a previous year. If the year is greater than the current year, it returns `0`, indicating that the date is in a future year.

**In summary**: This calculated column flags each date in the `DimDate` table as either being in the current year or a past year (with a value of `1`), or in a future year (with a value of `0`). This can be useful for filtering or analyzing data based on whether dates are current or future.

---

### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table is designed to track and analyze the distribution of hours worked alongside their corresponding percentage contributions to overall performance metrics. This table aids in understanding resource allocation and productivity levels within the organization, facilitating data-driven decision-making for workforce management.

#### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column captures the string representation of hours worked and their corresponding percentage contributions, enabling analysis of resource allocation and productivity metrics for informed workforce management decisions. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string values that represent the specific categories or metrics related to hours worked and their percentage contributions, enabling detailed analysis of resource allocation and productivity within the organization. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column categorizes the sequence of hours worked and their associated percentage contributions, enabling efficient analysis of resource allocation and performance metrics within the organization. |

---

### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, with associated qualification levels and sorting criteria to facilitate efficient data retrieval and reporting. This table is essential for ensuring consistency and accuracy in code-related data analysis and decision-making processes.

#### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers that represent specific categories within the reference dataset, enabling consistent classification and retrieval of code-related information across the business. |
| `Qualify` | `int64` | The 'Qualify' column (int64) indicates the qualification level associated with each code in the 'lkp_Code' table, facilitating the categorization and sorting of codes for accurate data analysis and reporting. |
| `Sort` | `string` | The 'Sort' column contains string values that define the order in which codes are organized within the 'lkp_Code' table, enabling efficient data retrieval and reporting based on predefined sorting criteria. |

---

### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics. This table enhances data integrity and usability by standardizing employee information for analysis and decision-making processes.

#### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column (string) uniquely identifies each employee in the 'lkp_fltr_Employee' table, serving as a critical reference point for filtering and reporting employee-related data in business analytics. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, serving as a key identifier to enhance data integrity and facilitate accurate filtering and reporting in business analytics. |

---

### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, detailing key attributes such as project names, qualification scores, billing amounts, and associated groups. This table is essential for analyzing project performance and financial metrics, enabling informed decision-making and strategic planning within the organization.

#### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the total billing amount associated with each project, providing critical financial data for performance analysis and strategic planning. |
| `Group` | `string` | The 'Group' column identifies the specific organizational unit or team associated with each project, facilitating the analysis of project performance and resource allocation across different groups within the organization. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for referencing project-related data and facilitating performance and financial analysis within the organization. |
| `Qualify` | `int64` | The 'Qualify' column (int64) represents the qualification score assigned to each project, serving as a critical metric for evaluating project performance and guiding strategic decision-making. |

---

### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing attributes such as billable department status and unit classifications. This table is essential for analyzing resource allocation and financial performance across different organizational segments.

#### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column indicates whether an organizational unit is classified as a billable department, represented as an integer value, facilitating financial analysis and resource allocation assessments within the business. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column contains the names of the organizational units, providing a key reference for identifying and categorizing various segments within the business for resource allocation and financial analysis. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column contains string values that represent the external type classification of ownership for each organizational unit, aiding in the analysis of resource allocation and financial performance across various segments. |
| `Unit` | `string` | The 'Unit' column contains the names of organizational units, serving as a key reference for identifying and categorizing various segments within the business for resource allocation and financial analysis. |

---

### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, leadership, and status. This table enables businesses to effectively manage and analyze their projects, facilitating better decision-making and resource allocation across various project groups.

#### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | The 'ActualDateCompleted' column records the precise date and time when a project was officially completed, providing critical data for performance analysis and project timeline assessments within the 'tbl_Project' table. |
| `Administration` | `string` | The 'Administration' column contains string values that represent the administrative oversight or management details associated with each project, aiding in the organization and governance of project activities within the 'tbl_Project' table. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a crucial timestamp for tracking updates and changes within the project management process. |
| `ClientName` | `string` | The 'ClientName' column stores the name of the client associated with each project, providing essential context for project management and stakeholder engagement within the 'tbl_Project' table. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, enabling efficient tracking and management of client-related project activities within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project was initiated, providing a chronological reference for project management and analysis within the 'tbl_Project' table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project, enabling accurate budgeting and financial analysis within the project management framework. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the exact date and time when each project entry was created, providing a chronological reference for project initiation within the tbl_Project repository. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or framework under which the project is categorized, aiding in the alignment of project objectives with organizational goals. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column indicates the scheduled date and time for the project's completion, serving as a critical milestone for project management and resource planning within the 'tbl_Project' table. |
| `Project` | `string` | The 'Project' column contains the name or title of each project, serving as a key identifier that allows for easy reference and organization within the comprehensive project management framework of the 'tbl_Project' table. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column contains a detailed narrative of the project's objectives and scope, providing essential context for stakeholders to understand the project's purpose and goals within the broader project management framework. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the specific group or team responsible for managing and executing the project, enabling effective collaboration and resource allocation within the organization. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a detailed narrative of the specific project group associated with each project, enhancing understanding of the project's context and objectives within the overall project management framework. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for managing and analyzing project-related information. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the project leader, linking each project to its responsible individual for effective management and accountability. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column (decimal) in the 'tbl_Project' table represents the base currency sales price for each project, enabling accurate financial analysis and reporting in relation to project profitability and budget management. |
| `Startdate` | `dateTime` | The 'Startdate' column records the date and time when a project officially commences, providing a crucial reference point for project timelines and scheduling within the 'tbl_Project' table. |
| `Status` | `string` | The 'Status' column indicates the current phase or condition of the project, providing essential insights for project management and resource allocation. |
| `Statuscode` | `string` | The 'Statuscode' column indicates the current status of each project, allowing for effective tracking and management of project progress within the tbl_Project repository. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, enabling businesses to assess financial commitments and manage budgets effectively within the project management framework. |

---

### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table serves as a critical resource for identifying employees who have not submitted their timesheets within a specified timeframe, enabling managers to address compliance issues and ensure accurate payroll processing. By analyzing data such as employee details, contract periods, and the specific weeks in question, organizations can enhance workforce management and optimize resource allocation.

#### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | 'ActualHours' represents the total number of hours an employee has worked during the specified timeframe, providing essential data for assessing timesheet compliance and ensuring accurate payroll calculations. |
| `Approved` | `boolean` | Indicates whether the missing timesheet has been approved by management, facilitating compliance tracking and payroll accuracy for employees identified in the 'vw_missing_timesheet' table. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column indicates the date and time when an employee's contract concludes, providing essential context for assessing timesheet submission compliance and ensuring accurate payroll processing within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column (int64) represents the total number of hours an employee is contracted to work within a specified period, providing essential context for evaluating timesheet compliance and ensuring accurate payroll calculations in the 'vw_missing_timesheet' table. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for evaluating timesheet submission compliance and aligning payroll processing with contractual obligations. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of an employee's contract, providing essential context for evaluating compliance with timesheet submissions and facilitating effective workforce management. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet submissions to ensure compliance and accurate payroll processing. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the names of employees identified in the 'vw_missing_timesheet' table, facilitating the tracking and management of timesheet compliance for accurate payroll processing. |
| `Hours_` | `double` | The 'Hours_' column (double) represents the total number of hours an employee is expected to report for the specified timeframe, aiding in the identification of discrepancies in timesheet submissions for effective payroll management. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when the missing timesheet data was captured, facilitating timely follow-up and compliance management for payroll accuracy. |
| `ManagerID` | `string` | The 'ManagerID' column (string) identifies the unique identifier of the manager responsible for overseeing the employees listed in the 'vw_missing_timesheet' table, facilitating targeted follow-up on timesheet compliance and payroll accuracy. |
| `ManagerName` | `string` | The 'ManagerName' column identifies the name of the employee's manager responsible for overseeing timesheet submissions, facilitating targeted follow-up on compliance issues related to missing timesheets. |
| `MissingHours` | `double` | The 'MissingHours' column (double) quantifies the total number of hours an employee has failed to report in their timesheet, facilitating the identification of compliance gaps and supporting accurate payroll processing within the 'vw_missing_timesheet' table. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with the timesheet entries, facilitating targeted follow-ups with employees regarding missing submissions related to particular assignments. |
| `ProjectCode` | `string` | The 'ProjectCode' column identifies the specific project associated with the timesheet entries, facilitating the tracking of employee contributions and ensuring accurate allocation of labor costs to respective projects. |
| `Rejected` | `boolean` | Indicates whether the timesheet submission for the corresponding employee has been rejected, aiding in the identification of compliance issues for timely payroll processing. |
| `ReportReady` | `boolean` | Indicates whether the report on missing timesheets is ready for review, facilitating timely managerial action to ensure compliance and accurate payroll processing. |
| `SalesAmount` | `double` | The 'SalesAmount' column represents the total revenue generated by each employee during the specified timeframe, providing insights into financial performance and aiding in the assessment of productivity in relation to timesheet compliance. |
| `SalesPrice` | `double` | The 'SalesPrice' column (double) in the 'vw_missing_timesheet' table represents the monetary value associated with sales transactions, providing insights into potential revenue impacts related to employee timesheet compliance and payroll accuracy. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date associated with the missing timesheet entries, facilitating the identification of compliance gaps in employee timesheet submissions. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific sub-department or team within the organization to which the employee belongs, facilitating targeted follow-up on missing timesheets and improving compliance tracking. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet submission, facilitating the tracking and management of employee compliance in timesheet reporting within the 'vw_missing_timesheet' table. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the timesheet submission status for each employee, facilitating managers in understanding the context of missing submissions and addressing compliance issues effectively. |
| `Week_` | `int64` | The 'Week_' column (int64) indicates the specific week number for which the timesheet submission is being tracked, facilitating the identification of missing submissions and supporting timely payroll processing. |
| `Year_` | `int64` | The 'Year_' column (int64) indicates the calendar year associated with the timesheet submission status, facilitating the identification of compliance issues and payroll accuracy for employees within the 'vw_missing_timesheet' table. |

#### Calculated Columns

**`BillableDep`**

- **Description:** The 'BillableDep' column indicates the department associated with billable hours for employees, facilitating the identification of compliance issues related to timesheet submissions within the 'vw_missing_timesheet' table.
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

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of a specific billable dependency, the default assumption is that it is billable.

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that there is a billable dependency present.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that there is no billable dependency.

In summary, this DAX expression effectively categorizes entries based on whether they have a billable dependency or not. It returns **1** if there is a billable dependency (either because it's explicitly defined or because there's no related data), and **0** if the dependency is explicitly zero. This helps in identifying which entries can be considered billable based on their dependencies.

**`CC_ActiveEmployees`**

- **Description:** The 'CC_ActiveEmployees' column indicates the status of employees currently active within the organization, providing essential context for identifying which individuals may be missing timesheet submissions in the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
IF(vw_missing_timesheet[ShortDate] >= vw_missing_timesheet[ContractStartDate] && 
			      (vw_missing_timesheet[ShortDate] <= vw_missing_timesheet[ContractEndDate] || ISBLANK(vw_missing_timesheet[ContractEndDate]))
			        , 1
			        , 0
			    )
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called `CC_ActiveEmployees` in a data model, specifically within a table named `vw_missing_timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether an employee is considered "active" based on their contract dates.

2. **Conditions Checked**:
   - It first checks if the date in the column `ShortDate` (which likely represents a specific date, such as when a timesheet is submitted) is on or after the employee's `ContractStartDate`. This means the employee's contract has started.
   - Then, it checks if the `ShortDate` is on or before the `ContractEndDate` (the date when the employee's contract ends). Alternatively, it also checks if the `ContractEndDate` is blank (meaning the employee's contract does not have a defined end date, which could imply they are still active).

3. **Output**:
   - If both conditions are true (the date is within the contract period or the contract has no end date), the expression returns a value of `1`, indicating that the employee is active.
   - If either condition is false, it returns `0`, indicating that the employee is not active.

In summary, this DAX expression effectively flags employees as active (1) or inactive (0) based on whether the date in question falls within their contract period or if they have an ongoing contract without an end date. This helps in identifying which employees are currently active for reporting or analysis purposes.

**`IncompleteFlag`**

- **Description:** The 'IncompleteFlag' column indicates whether an employee's timesheet submission is incomplete, serving as a key identifier for managers to quickly assess compliance and address potential payroll discrepancies.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IncompleteFlag' in a data table, specifically in the context of tracking timesheets.

Here's what it does in simple terms:

- The expression checks the value in the 'MissingHours' column of the 'vw_missing_timesheet' table.
- If the value of 'MissingHours' is less than 0, it means that there is an issue with the timesheet (for example, it might indicate that hours are missing or incorrectly recorded).
- In this case, the expression returns a value of 1, which can be interpreted as a flag indicating that the timesheet is incomplete or has a problem.
- If the value of 'MissingHours' is 0 or greater, the expression returns 0, indicating that there are no issues with the timesheet.

In summary, this DAX expression effectively flags timesheets as incomplete (1) when there are negative missing hours and marks them as complete (0) when there are no issues. This helps in identifying which timesheets need attention.

**`MAIN_UNIT`**

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit or department to which the employee belongs, facilitating targeted follow-ups on timesheet submissions and improving compliance management within the 'vw_missing_timesheet' table.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression starts by checking if there is a related value in the 'lkp_Unit' table for the current row. Specifically, it looks for the 'Unit' field in that related table.

2. **Handle Missing Data**: If the related 'Unit' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression will return the text "Unknown". This is a way to handle situations where the expected data is missing, ensuring that the output is clear and informative.

3. **Return the Unit Value**: If there is a valid related 'Unit' value (i.e., it is not blank), the expression will return that value. This means that for rows where the related data exists, the calculated column will show the actual unit name.

In summary, this DAX expression effectively populates the 'MAIN_UNIT' column with either the corresponding unit name from the 'lkp_Unit' table or "Unknown" if no unit is found. This helps maintain data integrity and clarity in reporting by clearly indicating when unit information is missing.

**`YearWeek`**

- **Description:** The 'YearWeek' column (string) represents the specific year and week number during which timesheet submissions are being tracked, facilitating the identification of compliance gaps in employee reporting.
- **DAX Expression:**
```dax
YEAR([ContractEndDate]) & "-" & 
			FORMAT(WEEKNUM([ContractEndDate]), "00")
```
- **DAX Explanation (Generated):** This DAX expression creates a new calculated column called 'YearWeek' that combines the year and the week number of a date from the 'ContractEndDate' field. Here's a breakdown of what it does:

1. **YEAR([ContractEndDate])**: This part extracts the year from the 'ContractEndDate'. For example, if the date is December 15, 2023, this will return "2023".

2. **WEEKNUM([ContractEndDate])**: This function calculates the week number of the year for the 'ContractEndDate'. For instance, if the date is in the 50th week of the year, this will return "50".

3. **FORMAT(..., "00")**: This ensures that the week number is always displayed as two digits. So, if the week number is 5, it will be formatted as "05".

4. **Concatenation (&)**: The expression combines the year and the formatted week number into a single string, separated by a hyphen. So, if the year is 2023 and the week number is 5, the final result will be "2023-05".

In summary, this DAX expression generates a string that represents the year and week number of the 'ContractEndDate' in the format "YYYY-WW". This can be useful for reporting or analyzing data on a weekly basis within a specific year.

#### Measures

**`Count_Negative_Consultant`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a specific measure called 'Count_Negative_Consultant'. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The measure counts the number of unique employees (consultants) who have recorded negative hours in their timesheets.

2. **Components**:
   - **`CALCULATE`**: This function modifies the context in which data is evaluated. In this case, it is used to apply a filter to the data.
   - **`DISTINCTCOUNT('vw_missing_timesheet'[EmployeeName])`**: This part counts the number of unique employee names from the 'vw_missing_timesheet' table. It ensures that each employee is only counted once, even if they have multiple entries.
   - **`'vw_missing_timesheet'[MissingHours] < 0`**: This condition filters the data to include only those entries where the 'MissingHours' value is less than zero. This means it focuses on cases where employees have negative hours recorded, which could indicate issues like over-reporting or errors in timesheet submissions.

3. **Outcome**: The final result of this measure is the total number of distinct employees who have negative hours logged in their timesheets. This information can be useful for management to identify potential problems with timesheet accuracy or to address issues with specific consultants.

In summary, this DAX expression helps organizations track and manage timesheet discrepancies by counting how many unique consultants have reported negative hours.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have recorded negative missing hours in a dataset. Here's a breakdown of what it does:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it counts the unique Employee IDs from the 'vw_missing_timesheet' table.

3. **Filter Condition**: The expression includes a filter that only considers rows where the 'MissingHours' value is less than 0. This means it focuses on instances where employees have negative missing hours, which could indicate an error or a specific situation that needs attention.

In summary, this measure calculates how many different employees have negative missing hours recorded in the timesheet data. This information can be useful for identifying potential issues with time reporting or attendance that may need to be addressed.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of active employees who have missing timesheet hours for a specific period. Here's a breakdown of what it does in simple business terms:

1. **Data Source**: The expression starts by looking at a data table called `vw_missing_timesheet`, which likely contains information about employees, their hours worked, and their contractual hours.

2. **Summarization**: It summarizes the data by grouping it based on several criteria:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a more specific division within the unit)

3. **Calculating Missing Hours**: For each group, it calculates:
   - **MissingHours**: This is determined by taking the total hours worked (from `vw_missing_timesheet[Hours_]`) and subtracting the maximum contractual hours (from `vw_missing_timesheet[ContractHours]`). If this value is negative, it indicates that the employee has not logged enough hours compared to what they are contracted to work.

4. **Identifying Active Employees**: It also calculates:
   - **MinActiveEmp**: This finds the minimum value of a field called `CC_ActiveEmployees`, which likely indicates whether an employee is currently active (1 for active, 0 for inactive).

5. **Filtering Criteria**: The expression then filters this summarized data to find:
   - Employees who are active (`MinActiveEmp = 1`)
   - Employees who have missing hours (where `MissingHours < 0`)

6. **Counting Active Employees**: Finally, it counts the number of rows that meet these criteria, which gives the total number of active employees who have not logged enough hours in their timesheets.

In summary, this DAX measure calculates how many active employees have logged fewer hours than they are supposed to, indicating potential issues with timesheet submissions.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of "missing hours" for active employees who have recorded fewer hours than their contracted hours in a specific time frame (defined by year and week).

Here's a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. It filters a summarized view of the data from a table called `vw_missing_timesheet`. This summary includes:
   - Employee names
   - Year and week of the records
   - Unit and subunit information
   - A calculation of "MissingHours," which is the difference between the total hours recorded by the employee and their contracted hours.
   - The minimum number of active employees for that specific record.

2. **Filter Criteria**: The `FILTER` function is used to narrow down this summarized data to only those records where:
   - The minimum number of active employees is 1 (indicating that the employee is active).
   - The calculated "MissingHours" is less than 0 (indicating that the employee has not worked enough hours compared to what they are contracted for).

3. **Calculate Total Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to iterate over the filtered list of active employees and sums up their "MissingHours." This gives the total number of hours that active employees are missing compared to their contracted hours.

In summary, this DAX measure calculates the total hours that active employees are short of their expected contracted hours, helping the business identify potential issues with employee attendance or workload management.

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

1. **Data Source**: The expression uses a data table called `vw_missing_timesheet`, which likely contains information about employees, their working hours, and the weeks they worked.

2. **Grouping Data**: The `SUMMARIZE` function groups the data by two key pieces of information:
   - The week (`vw_missing_timesheet[Week_]`)
   - The employee ID (`vw_missing_timesheet[EmployeeID]`)

   For each unique combination of week and employee, it calculates the maximum contract hours (`MAX(vw_missing_timesheet[ContractHours])`). This means that if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

3. **Calculating Total**: After summarizing the data, the `SUMX` function then takes this summarized table and adds up all the maximum contract hours for each employee across all weeks. 

In summary, this DAX measure provides the total expected contract hours for all employees, ensuring that if there are multiple records for an employee in a week, only the highest contract hours are considered. This helps in understanding the overall expected workload for employees on a weekly basis.

**`MISSING%`**

- **DAX Expression:**
```dax
DIVIDE([Dax_EmpCount]-[Dax_EmpCount_MissingTS],[Dax_EmpCount]) * 100
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of employees that are not missing time stamps (TS) compared to the total number of employees. Here's a breakdown of what each part does:

1. **[Dax_EmpCount]**: This represents the total number of employees in the dataset.

2. **[Dax_EmpCount_MissingTS]**: This represents the number of employees who are missing time stamps.

3. **[Dax_EmpCount] - [Dax_EmpCount_MissingTS]**: This part calculates the number of employees who have valid time stamps by subtracting the number of employees missing time stamps from the total employee count.

4. **DIVIDE(..., [Dax_EmpCount])**: The DIVIDE function is used to safely perform division. It takes the number of employees with valid time stamps (from the previous step) and divides it by the total number of employees. This gives us a fraction that represents the proportion of employees with valid time stamps.

5. **... * 100**: Finally, multiplying by 100 converts the fraction into a percentage.

In summary, this measure calculates the percentage of employees who have valid time stamps, helping the business understand how many employees are accounted for in terms of time tracking. A higher percentage indicates better data completeness regarding employee time stamps.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked and contracted hours for employees or contractors. Here's a breakdown of what it achieves:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression finds the maximum number of hours that are contracted for a specific employee or contractor. Essentially, it retrieves the highest value of hours that they are supposed to work according to their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part finds the maximum number of hours that have actually been recorded or logged by the employee or contractor. It looks at the hours they have worked or reported.

3. **Overall Calculation**: By subtracting the maximum recorded hours (actual hours worked) from the maximum contracted hours (expected hours), the expression calculates the "missing hours." This represents the difference between what the employee was supposed to work and what they actually worked.

In simple terms, this measure helps identify how many hours an employee or contractor has not worked compared to what they were contracted to work. This can be useful for tracking attendance, ensuring compliance with work agreements, or identifying potential issues with time reporting.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of completeness regarding missing hours in relation to expected contract hours for a given period, such as a week. Here’s a breakdown of what it does:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part adds up all the missing hours recorded in the `vw_missing_timesheet` table. Essentially, it tells us how many hours were not logged by employees.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that employees are expected to work in a week according to their contracts.

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the expression calculates the difference between the total missing hours and the expected contract hours. If the result is positive, it indicates that the missing hours exceed what was expected.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The `DIVIDE` function takes the result from the previous step (the difference between missing hours and expected hours) and divides it by the expected contract hours. This gives us a ratio or percentage. The third argument, `0`, ensures that if the expected contract hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

In summary, this measure calculates how much the missing hours deviate from the expected hours, expressed as a percentage of the expected hours. A positive result indicates a shortfall in hours logged compared to what was expected, while a negative result would suggest that more hours were logged than expected. This measure helps in assessing employee compliance with time reporting and can be useful for management to identify potential issues with timesheet submissions.

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

2. **Grouping**: The `SUMMARIZE` function is used to create a summary table. This table groups the data by three columns:
   - `Week_`: The week of the year.
   - `EmployeeName`: The name of the employee.
   - `MAIN_UNIT`: The main unit or department the employee belongs to.

3. **IncompleteFlag**: For each group created by the `SUMMARIZE` function, it calculates a new column called "IncompleteFlag". This column uses the `MAX` function to determine if there is any record in that group indicating that the employee has an incomplete timesheet. If at least one record shows an incomplete timesheet, the flag will be set to 1 (true); otherwise, it will be 0 (false).

4. **Summing Up**: The outer `SUMX` function then takes this summarized table and sums up the values in the "IncompleteFlag" column. This effectively counts how many unique employees have at least one instance of an incomplete timesheet across the specified weeks and units.

In summary, this DAX measure helps the business understand how many unique employees, across different units and weeks, have not submitted their timesheets completely. This information can be crucial for tracking compliance and ensuring that all employees are fulfilling their timesheet responsibilities.

---

### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This table enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

#### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet submission has been approved, facilitating tracking of approval status for accurate labor allocation and project cost analysis. |
| `Client` | `string` | The 'Client' column identifies the client associated with each timesheet entry, providing essential context for analyzing project costs and labor allocation related to specific clients. |
| `Code2` | `string` | The 'Code2' column stores a secondary identifier or code related to the timesheet entry, providing additional context for project categorization or employee classification within the comprehensive record of employee submissions. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column indicates the date and time when an employee's contract is set to expire, providing critical information for managing labor resources and project timelines within the context of timesheet submissions. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column records the date and time when an employee's contract begins, providing essential context for analyzing timesheet submissions in relation to contract duration and project allocation. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of employee contracts as of today, providing essential context for evaluating timesheet submissions and associated labor costs within the 'vw_Timesheet' table. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of employee contracts as of today, providing insights into contract compliance and workforce availability for effective project and resource management. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the specific date and time when a contract was transferred, providing crucial context for timesheet submissions related to contract changes and ensuring accurate tracking of labor allocation and project costs. |
| `CostAmount` | `double` | The 'CostAmount' column represents the monetary value associated with the hours worked by employees on specific projects, enabling businesses to assess labor costs and optimize resource allocation within the 'vw_Timesheet' table. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with the labor costs for each timesheet entry, enabling businesses to assess project expenses and optimize resource allocation. |
| `Currency` | `string` | The 'Currency' column (string) in the 'vw_Timesheet' table indicates the currency type used for financial transactions related to employee timesheet submissions, enabling accurate financial analysis and reporting across different projects and managerial assessments. |
| `DebtorName` | `string` | The 'DebtorName' column stores the name of the debtor associated with the timesheet entry, providing essential context for financial accountability and project cost analysis within the employee timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee within the 'vw_Timesheet' table, linking timesheet submissions to specific personnel for accurate tracking of labor allocation and project costs. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column stores the unique identifier and name of the employee associated with each timesheet entry, enabling clear attribution of labor hours to specific personnel for accurate project cost analysis and resource management. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee within the 'vw_Timesheet' table, linking timesheet submissions to specific individuals for accurate labor allocation and project cost analysis. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, providing a clear identification of individuals associated with each timesheet entry for effective tracking and analysis of labor contributions within the organization. |
| `Employer` | `string` | The 'Employer' column identifies the organization or company that employs the individual submitting the timesheet, providing context for labor allocation and project cost analysis within the 'vw_Timesheet' table. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) uniquely identifies the employer associated with each timesheet submission, enabling accurate tracking of labor costs and resource allocation across different projects within the 'vw_Timesheet' table. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to the timesheet submissions, providing further insights into employee activities or project specifics that may not be captured in other fields. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table represents the fiscal year during which employee timesheet submissions were recorded, enabling analysis of labor costs and resource allocation across different financial periods. |
| `Hours` | `double` | The 'Hours' column (double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee on a specific project during a given timesheet submission, enabling accurate tracking of labor allocation and project costs. |
| `HoursperWeek` | `int64` | 'HoursperWeek' (int64) represents the total number of hours an employee is allocated to work each week, providing critical insights into labor distribution and project resource management within the 'vw_Timesheet' table. |
| `HoursType` | `string` | The 'HoursType' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, providing essential context for analyzing labor allocation and associated costs. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, providing essential context for analyzing labor allocation and associated costs. |
| `Index` | `int64` | The 'Index' column (int64) serves as a unique identifier for each entry in the 'vw_Timesheet' table, enabling efficient data retrieval and organization of employee timesheet submissions. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when each timesheet entry was submitted, providing a timestamp for tracking and auditing purposes within the employee timesheet records. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management purposes, linking each timesheet entry to specific projects for enhanced tracking and analysis of labor allocation and costs. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) uniquely identifies the project manager associated with each timesheet entry, enabling effective tracking of managerial oversight and project accountability within the comprehensive employee timesheet records. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Manager responsible for overseeing the timesheet submissions, providing essential managerial context for analyzing labor allocation and project oversight within the 'vw_Timesheet' table. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the name of the IPM (Integrated Project Management) identifier associated with each timesheet entry, providing a reference for linking employee submissions to specific projects and enhancing project cost analysis and resource allocation. |
| `JobTitle` | `string` | The 'JobTitle' column captures the specific title or role of the employee submitting the timesheet, providing context for labor allocation and project involvement within the comprehensive employee timesheet records. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier that links employee timesheet records to related datasets, ensuring accurate data integration and analysis across various business dimensions. |
| `ManagerID` | `string` | The 'ManagerID' column (string) identifies the unique identifier of the manager overseeing the employee's timesheet submissions, enabling effective tracking of managerial oversight and accountability in labor allocation and project management. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet submission, providing essential context for oversight and accountability in labor allocation and project management. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) identifies the unique key associated with the manager overseeing the employee's timesheet submission, enabling effective tracking of managerial oversight and resource allocation within the organization. |
| `ManagerName` | `string` | The 'ManagerName' column stores the name of the employee's direct supervisor, providing essential managerial context for each timesheet entry to enhance oversight and facilitate effective resource management. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string values that represent the specific work bookings associated with employee timesheet submissions, providing insights into project allocations and labor distribution for effective resource management and cost analysis. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each timesheet entry, enabling analysis of labor allocation and project-related costs within the organization's overall resource management framework. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with employee timesheet entries, enabling effective tracking and analysis of labor allocation and project costs within the organization. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string representation of the specific project associated with each timesheet entry, providing essential context for analyzing labor allocation and project-related costs within the 'vw_Timesheet' table. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) uniquely identifies the specific project associated with each timesheet entry, enabling detailed analysis of labor allocation and project costs within the 'vw_Timesheet' table. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into labor allocation and project-specific resource management. |
| `Rejected` | `boolean` | Indicates whether the timesheet submission has been rejected, providing a clear status for review and approval processes within employee time tracking. |
| `ReportReady` | `boolean` | The 'ReportReady' column indicates whether the timesheet submission is finalized and ready for reporting, facilitating efficient tracking and analysis of employee labor data. |
| `SalesAmount` | `double` | The 'SalesAmount' column (Data Type: double) in the 'vw_Timesheet' table represents the total revenue generated from employee labor for specific projects, enabling businesses to assess financial performance and project profitability. |
| `SalesPrice` | `double` | The 'SalesPrice' column (double) in the 'vw_Timesheet' table represents the monetary value assigned to services rendered by employees, enabling analysis of project profitability and labor cost management. |
| `Status` | `string` | The 'Status' column indicates the current state of each timesheet submission, reflecting whether it is pending, approved, or rejected, thereby aiding in the tracking and management of employee time reporting processes. |
| `Taxed` | `boolean` | Indicates whether the recorded timesheet entry is subject to taxation, providing essential information for financial analysis and compliance within employee labor records. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with each timesheet submission, aiding in the analysis of resource allocation and project management efficiency. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column uniquely identifies each timesheet submission, allowing for efficient tracking and management of employee hours against specific projects and financial periods. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column records the specific date and time when an employee's timesheet entry was submitted, providing a crucial timestamp for tracking labor allocation and submission timelines within the overall timesheet management process. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the work performed by employees during the timesheet period, enhancing the understanding of labor allocation and project contributions for effective resource management and cost analysis. |
| `Unit` | `string` | The 'Unit' column represents the specific organizational unit or department associated with the timesheet entry, providing context for labor allocation and project cost analysis within the 'vw_Timesheet' table. |
| `UnitCode` | `string` | The 'UnitCode' column represents a unique identifier for the organizational unit associated with each timesheet entry, enabling precise tracking of labor allocation and project costs within the 'vw_Timesheet' table. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, facilitating communication and verification related to their timesheet submissions within the 'vw_Timesheet' table. |
| `YearWeek` | `string` | The 'YearWeek' column (string) represents the specific year and week number of the timesheet submission, enabling detailed temporal analysis of employee labor allocation and project costs within the 'vw_Timesheet' table. |

#### Calculated Columns

**`Approved_With_V_Z`**

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheet submissions, specifically denoting whether the submission has been approved with a specific validation or review process, thereby aiding in tracking compliance and oversight in labor management.
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
   - It also checks if the 'TimesheetCode' in the 'vw_Timesheet' table is either "V" or "Z".

2. **Output Values**: 
   - If either of the conditions is met (i.e., the item is approved or the TimesheetCode is "V" or "Z"), the expression returns a value of **1**.
   - If neither condition is met, it returns a value of **0**.

3. **Purpose**: The purpose of this calculated column is to create a simple flag (1 or 0) that indicates whether a timesheet is either approved or falls under specific codes ("V" or "Z"). This can be useful for filtering or analyzing data based on approval status or specific timesheet codes in reports or dashboards.

In summary, this DAX expression helps identify and mark timesheets that are either approved or have specific codes, making it easier to manage and analyze timesheet data.

**`BillableDep`**

- **Description:** The 'BillableDep' column indicates the department responsible for billing associated with the timesheet entries, providing clarity on financial accountability and resource allocation within projects.
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

2. **Handle Blank Values**: If the related 'BillableDep' value is blank (meaning there is no corresponding entry in the 'lkp_Unit' table), the expression returns a value of **1**. This indicates that, in the absence of specific information, it defaults to treating this case as billable.

3. **Check for Non-Zero Values**: If there is a related 'BillableDep' value and it is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that the item is considered billable.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that the item is not billable.

In summary, this DAX expression effectively categorizes items as billable or not based on the presence and value of the 'BillableDep' field from the related 'lkp_Unit' table. If there is no related data or if the value is non-zero, it treats the item as billable (1). If the value is zero, it treats it as not billable (0).

**`BillablePrj`**

- **Description:** The 'BillablePrj' column identifies the projects for which employee hours are chargeable, enabling accurate tracking of billable labor costs and enhancing financial accountability in project management.
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

1. **Purpose**: The expression determines whether a particular timesheet entry is considered "billable" based on certain criteria.

2. **Criteria for Billability**:
   - **Sales Price Check**: It first checks if the `SalesPrice` for the entry is greater than 0. This means that if there is a positive sales price associated with the timesheet entry, it is considered billable.
   - **Project Profile Code Check**: Next, it checks if the `ProjectProfileCode` is equal to 81. If it is, this specific project is deemed billable regardless of other factors.
   - **Project Name Check**: Lastly, it looks for the phrase "Customer Success Services" in the `Project` field. If this phrase is found, the entry is also considered billable.

3. **Output**: 
   - If any of the above conditions are met (i.e., if the sales price is positive, the project profile code is 81, or the project name includes "Customer Success Services"), the expression returns a value of **1**, indicating that the entry is billable.
   - If none of these conditions are met, it returns a value of **0**, indicating that the entry is not billable.

In summary, this DAX expression effectively flags timesheet entries as billable or not based on specific financial and project-related criteria, helping the business identify which entries can be charged to clients.

**`cc_Employer`**

- **Description:** The 'cc_Employer' column stores the name of the employer associated with each timesheet entry, providing essential context for analyzing labor allocation and project costs within the 'vw_Timesheet' table.

**`Employee_WeeklyHours`**

- **Description:** The 'Employee_WeeklyHours' column captures the total hours worked by each employee during the week, represented as a string, providing essential data for analyzing labor allocation and project costs within the 'vw_Timesheet' table.
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

3. **Combining Information**: The expression then combines these two pieces of information into one string. It formats this string by placing the employee's name first, followed by a hyphen and a space (" - "), and then the number of hours they work per week.

For example, if an employee's name is "John Doe" and he works 40 hours per week, the resulting string in the 'Employee_WeeklyHours' column would be "John Doe - 40".

In summary, this DAX expression helps to create a clear and concise representation of each employee's name along with their weekly working hours, making it easier to read and understand the data at a glance.

**`GroupCat`**

- **Description:** The 'GroupCat' column categorizes timesheet entries into specific groups, aiding in the analysis of labor distribution and project allocation within the 'vw_Timesheet' table.
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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'GroupCat' in a data model, specifically within a table called `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Lookup for Group**: The expression first attempts to find a corresponding 'Group' value from a related table called `lkp_Project`. It does this by looking up the 'Group' based on the 'Project' field in the `vw_Timesheet` table. 

2. **Check for Existence**: The `NOT(ISBLANK(...))` part checks if the lookup returned a valid 'Group' value. If it did (meaning the project exists in the `lkp_Project` table), it will use that 'Group' value as the result for the 'GroupCat' column.

3. **Fallback Logic**: If the lookup does not return a valid 'Group' (i.e., the project does not exist in the `lkp_Project` table), the expression then checks if the project is billable. This is determined by checking if the `BillablePrj` field in the `vw_Timesheet` table equals 1.

4. **Categorization**: 
   - If `BillablePrj` is 1, it categorizes the project as "Billable".
   - If `BillablePrj` is not 1, it categorizes the project as "Other Unbillable".

In summary, this DAX expression categorizes each project in the `vw_Timesheet` table into one of three categories:
- The corresponding 'Group' from the `lkp_Project` table if it exists.
- "Billable" if the project is marked as billable.
- "Other Unbillable" if the project is not billable and does not have a corresponding group. 

This helps in organizing and analyzing projects based on their billing status and group affiliation.

**`IsContractActive`**

- **Description:** Indicates whether the employee's contract is currently active, providing critical context for timesheet submissions and labor allocation analysis.
- **DAX Expression:**
```dax
IF(
			    ISBLANK('vw_Timesheet'[ContractEndDate]) || 'vw_Timesheet'[ContractEndDate] > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IsContractActive' in a data model, specifically for a table named 'vw_Timesheet'. Here's what it does in simple business terms:

1. **Checks Contract End Date**: The expression first checks if the 'ContractEndDate' for a particular record (or row) in the 'vw_Timesheet' table is blank (meaning there is no end date specified) or if the end date is greater than today's date.

2. **Determines Contract Status**:
   - If either of those conditions is true (the end date is blank or it is in the future), the expression labels the contract as "Active".
   - If neither condition is true (meaning there is a specific end date that is in the past), it labels the contract as "Not Active".

In summary, this calculated column helps identify whether a contract is currently active or not based on its end date, providing valuable information for tracking contract statuses in the business.

**`MAIN_UNIT`**

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit or department associated with the employee's timesheet submission, aiding in the analysis of labor distribution and project cost allocation within the business.
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

2. **Handle Missing Data**: If there is no related value found (meaning the 'Unit' field is blank), the expression will return the text "Unknown". This is a way to handle situations where the data might be incomplete or missing.

3. **Return the Unit Name**: If there is a related value (meaning the 'Unit' field is not blank), the expression will return that value. This means it successfully retrieves the name of the unit from the related table.

In summary, this DAX expression effectively ensures that for each row in the current table, you either get the name of the unit from the related 'lkp_Unit' table or "Unknown" if no unit is associated. This helps maintain clarity in your data by clearly indicating when unit information is missing.

**`MonthNumber`**

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month in which timesheet entries were submitted, aiding in the organization and analysis of labor data by month for improved financial and resource management.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to extract the month number from a date value found in the column named `TimesheetDate`. 

In simple terms, this means that for each entry in the `TimesheetDate` column, the expression will return a number that represents the month of that date. For example:

- If the date is January 15, 2023, the expression will return `1`.
- If the date is February 10, 2023, it will return `2`.
- If the date is December 5, 2023, it will return `12`.

This calculated column, named 'MonthNumber', helps in organizing or analyzing data by month, making it easier to perform further calculations or visualizations based on monthly trends or patterns.

**`NR_EMP_COLUMN`**

- **Description:** The 'NR_EMP_COLUMN' stores the unique identification number for each employee, enabling precise tracking of timesheet submissions and facilitating analysis of labor allocation and project costs within the 'vw_Timesheet' table.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to count the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

In simple business terms, this calculation achieves the following:

- It identifies how many different employees have recorded timesheets, ensuring that each employee is only counted once, regardless of how many times they appear in the data.
- This is useful for understanding workforce participation, tracking employee engagement, or analyzing the number of distinct contributors to a project or task over a specific period.

Overall, this expression helps businesses get a clear picture of their employee involvement by providing a count of unique employees.

**`Own-Sub-ExtT`**

- **Description:** The 'Own-Sub-ExtT' column captures the type of timesheet submission, indicating whether it is an internal, subcontracted, or external entry, thereby aiding in the analysis of labor allocation and project cost management.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used to retrieve a value from a related table in a data model. Here's a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a calculated column within a table that has a relationship with another table. In this case, the current table is related to the `lkp_Unit` table.

2. **Purpose**: The expression fetches the value from the `OWN-Sub-ExtT` column in the `lkp_Unit` table. This means that for each row in the current table, it looks up the corresponding value from the `lkp_Unit` table based on the established relationship.

3. **Outcome**: The result is that each row in the current table will have a new column (the calculated column) that contains the value from the `OWN-Sub-ExtT` column of the related `lkp_Unit` table. This allows users to bring in additional information that is relevant to the current data, enhancing analysis and reporting.

In summary, this DAX expression helps to enrich the current dataset by pulling in specific related information from another table, making it easier to analyze and understand the data in context.

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
   - If the 'Qualify' field is blank, the expression returns a value of **1**. This could indicate a default or fallback status when there is no specific qualification available for the project.
   - If the 'Qualify' field is not blank (meaning it has a value), the expression returns the actual value from the 'Qualify' field in the 'lkp_Project' table.

**In summary**, this DAX expression effectively assigns a value to the 'QualifyPrj' column based on whether the related project's qualification information is available. If it's missing, it defaults to 1; if it's present, it uses that specific qualification value. This helps ensure that every project has a qualification status, either a default or the actual value.

**`ReportingEntity`**

- **Description:** The 'ReportingEntity' column identifies the specific organizational unit or department responsible for the timesheet submission, enabling accurate tracking of labor costs and resource allocation within the business.

**`RReady_With_V_Z`**

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of timesheet submissions, providing a string value that reflects whether the submission is complete and compliant with the necessary verification processes.
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
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These are specific codes that likely represent certain types of timesheets.

2. **Output Values**: 
   - If either of the conditions is met (i.e., if the report is ready or if the timesheet code is "V" or "Z"), the expression returns a value of `1`. This indicates a positive condition where the item is considered "ready" in some context.
   - If neither condition is met, it returns a value of `0`, indicating that the item is not ready.

3. **Purpose**: The overall purpose of this calculated column is to flag records as "ready" (1) or "not ready" (0) based on the readiness of the report or specific timesheet codes. This can be useful for filtering, reporting, or further analysis in a business context, helping teams quickly identify which records are actionable. 

In summary, this DAX expression helps to categorize records based on their readiness status, making it easier for users to manage and analyze their data.

#### Measures

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

1. **Condition Check**: The expression first checks the value of another measure called `[ContractStatusMeasure]`. It looks to see if this measure equals "Active".

2. **Return Value if Active**: If the condition is true (meaning the contract is indeed active), the expression returns the value of another measure called `[HoursDifference]`. This likely represents the difference in hours related to the contract, such as hours worked or hours remaining.

3. **Return Value if Not Active**: If the condition is false (meaning the contract is not active), the expression returns the text "Not Active".

In summary, this measure helps to determine the status of a contract. If the contract is active, it provides the relevant hours difference; if not, it simply indicates that the contract is not active. This can be useful for reporting or analysis purposes, allowing users to quickly assess the status of contracts and their associated hours.

**`ContractStatusMeasure`**

- **DAX Expression:**
```dax
IF(
			    ISBLANK(MAX('vw_Timesheet'[ContractEndDate])) || MAX('vw_Timesheet'[ContractEndDate]) > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'ContractStatusMeasure' that determines the status of a contract based on its end date. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank End Date**: The expression first checks if the maximum value of the 'ContractEndDate' from the 'vw_Timesheet' table is blank (i.e., there is no end date specified). This could mean that the contract is ongoing or has not been finalized.

2. **Compare with Today's Date**: If the end date is not blank, it then compares this maximum end date to today's date. If the end date is greater than today, it indicates that the contract is still active and has not yet expired.

3. **Determine Status**: Based on these checks:
   - If the end date is blank or if it is in the future (greater than today), the measure returns "Active".
   - If the end date is in the past (less than today), it returns "Not Active".

In summary, this measure effectively categorizes contracts as either "Active" or "Not Active" based on whether they have an end date that is still valid or not. This helps businesses quickly assess the status of their contracts.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 10, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated starting from the first week of the year.

So, when you combine these two functions, `WEEKNUM(TODAY())` will give you the week number for the current date. 

For instance, if today is October 10, 2023, and it falls in the 41st week of the year, this expression will return the number 41. 

In business terms, this measure can be useful for reporting and analysis purposes, allowing you to understand which week of the year you are currently in, which can help in tracking performance, planning, and making timely decisions.

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

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it ensures that each employee is only counted once, even if they have multiple entries in the timesheet.

3. **vw_Timesheet[Approved] = FALSE()**: This is a filter condition that specifies we only want to consider timesheet entries where the `Approved` status is set to FALSE. In other words, it filters the data to include only those timesheets that have not been approved.

Putting it all together, this DAX measure calculates the total number of distinct employees who have submitted timesheets that are still pending approval. This can be useful for tracking how many employees are waiting for their timesheet approvals, helping management understand the workload or bottlenecks in the approval process.

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

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data being analyzed.

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it counts the unique names of employees from the `vw_Timesheet` table.

3. **Filter Condition**: The expression includes a filter that specifies `vw_Timesheet[ReportReady] = FALSE()`. This means that only the timesheets that are not marked as "Report Ready" will be considered in the count.

In summary, this measure calculates the total number of distinct employees who have timesheets that are still pending or not ready for reporting. This can be useful for tracking which employees still need to submit their timesheets or for identifying potential delays in the reporting process.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheets in the `vw_Timesheet` table.

Here's a breakdown of what it achieves:

- **DISTINCTCOUNT**: This function counts the number of different (or distinct) values in a specified column.
- **vw_Timesheet[EmployeeName]**: This specifies the column from which the distinct values are being counted. In this case, it refers to the names of employees in the timesheet data.

In simple terms, this measure tells you how many individual employees have submitted timesheets, without counting any employee more than once, even if they have multiple entries. This is useful for understanding workforce participation or engagement in a given period.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_%_Budget_Used`**

- **DAX Expression:**
```dax
DIVIDE(SUM(vw_Timesheet[SalesAmount]), SUM('tbl_Project'[SalesPriceBaseCurrency]), 0)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of the budget that has been used based on sales data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the 'vw_Timesheet' table. Essentially, it totals the revenue generated from sales.

2. **SUM('tbl_Project'[SalesPriceBaseCurrency])**: This part sums up the total budget or sales price available for the projects from the 'tbl_Project' table. It represents the total amount that was planned or budgeted for sales.

3. **DIVIDE(..., ..., 0)**: The DIVIDE function takes the total sales amount (from the first part) and divides it by the total budget (from the second part). The third argument, 0, is a safeguard that specifies what to return if the denominator (total budget) is zero. In this case, it will return 0 instead of an error.

In summary, this measure calculates the ratio of actual sales to the budgeted sales, effectively showing how much of the budget has been utilized. If the result is, for example, 0.75, it means that 75% of the budget has been used based on the sales achieved.

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the timesheet. Essentially, it totals the revenue generated from sales over a specific period.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked as recorded in the timesheet. It gives the total number of hours that employees or team members have worked during that same period.

3. **Division**: The expression then divides the total sales amount by the total hours worked. This calculation gives you the average hourly rate, which represents how much revenue is generated for each hour worked.

In summary, this DAX measure calculates the average revenue earned per hour worked, helping businesses understand their efficiency and productivity in generating sales relative to the time invested.

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

1. **Identify the Current Week**: The code starts by capturing the current week number from a date dimension table (DimDate). This is done using the `SELECTEDVALUE` function, which retrieves the week number that is currently being analyzed.

2. **Calculate Previous Budget**: Next, the code calculates the budget value for the most recent week prior to the current week. It does this by:
   - Using the `CALCULATE` function to evaluate the measure `[Msr_SalesPriceBaseCurrency]`, which presumably represents the sales price in the base currency.
   - The `FILTER` function is applied to the date dimension to include only those weeks that are less than the current week. The `ALLSELECTED` function ensures that any filters applied to the date dimension are respected, but it allows for the calculation of previous weeks.

3. **Return the Appropriate Value**: Finally, the code checks if the current measure `[Msr_SalesPriceBaseCurrency]` has a value (i.e., it is not blank). 
   - If it does have a value, that value is returned as the result of the measure.
   - If it does not have a value (meaning there is no budget for the current week), the measure instead returns the previously calculated budget value from the most recent week.

In summary, this DAX expression effectively ensures that for any given week, if there is a budget available, it uses that; if not, it falls back to the most recent available budget from the previous weeks. This approach helps maintain continuity in budget reporting, ensuring that users always have a relevant budget figure to work with.

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

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It does this by finding the maximum value of the `MonthNumberOfYear` from the `DimDate` table. This means if you are looking at data for March, the `SelectedMonth` would be 3.

2. **Calculate Cumulative Cost**: The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to and including the selected month. 

3. **Use of CALCULATE**: The `CALCULATE` function is used to change the context in which the data is evaluated. It allows us to sum up the `CostAmount` from the `vw_Timesheet` table, but with specific conditions applied.

4. **Filtering the Data**: The `FILTER` function is applied to the `DimDate` table. It uses `ALLSELECTED` to consider all the dates that are currently selected in the visual, but it only includes those months where the `MonthNumberOfYear` is less than or equal to the `SelectedMonth`. This means if you are looking at March, it will include costs from January, February, and March.

5. **Final Result**: The final result of this measure is the total cost from the `vw_Timesheet` for all months leading up to and including the month that is currently selected in the visual. This cumulative total helps businesses understand their costs over time and make informed decisions based on historical data.

In summary, this DAX measure effectively tracks and sums costs over a specified period, allowing users to see how costs accumulate month by month.

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

1. **Identify the Selected Month**: The expression starts by determining which month is currently selected in the visual. It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month that the user is focusing on.

2. **Calculate Cumulative Sales**: The main goal of this measure is to sum up all sales amounts from the 'vw_Timesheet' table for all months that are less than or equal to the selected month. This means if the user selects March, the measure will calculate the total sales for January, February, and March.

3. **Use of FILTER and ALLSELECTED**: The `FILTER` function is used to create a condition that includes all months up to the selected month. The `ALLSELECTED` function ensures that the calculation respects any filters that might be applied to the date dimension, allowing for a more dynamic and context-aware calculation.

In summary, this DAX measure calculates the total sales accumulated from the beginning of the year up to the month that the user has selected in the report. This is useful for understanding sales performance over time and helps in analyzing trends and making informed business decisions.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the percentage of hours worked by a specific group, project, or employee compared to the total hours worked across all selected categories. Here’s a breakdown of what it does in simple business terms:

1. **TotalValue Calculation**: 
   - The first part of the code creates a variable called `TotalValue`. This variable calculates the total number of hours from the `vw_Timesheet` table.
   - The `CALCULATE` function is used to sum up the hours, but it does so while ignoring any filters applied to the `GroupCat`, `Project`, and `EmployeeName` columns. This means it looks at all the data in these categories, regardless of what specific selections the user has made in a report or dashboard.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours for the current context (which could be a specific group, project, or employee) by dividing the sum of hours for that context by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safe way to handle division in DAX. If the `TotalValue` is zero (meaning there are no hours to compare against), it will return 0 instead of causing an error.

In summary, this measure calculates what portion of the total hours worked is represented by the hours in the current context (like a specific project or employee), allowing users to see how much of the total workload is attributed to different segments of their data.

**`MSR_PROJECT_%_REALIZATION`**

- **DAX Expression:**
```dax
IF(sum(tbl_Project[SalesPriceBaseCurrency]) <> 0, sum(vw_Timesheet[SalesAmount])/sum(tbl_Project[SalesPriceBaseCurrency]))
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called 'MSR_PROJECT_%_REALIZATION', which essentially assesses the percentage of sales realized from a project compared to its total sales price.

Here's a breakdown of what it does:

1. **Condition Check**: The expression starts with an `IF` statement that checks if the total sales price (from the `tbl_Project` table) is not equal to zero. This is important because if the total sales price is zero, dividing by it would lead to an error.

2. **Calculating Sales Amount**: If the total sales price is not zero, the expression proceeds to calculate the percentage of sales realized. It does this by taking the total sales amount (from the `vw_Timesheet` table) and dividing it by the total sales price (from the `tbl_Project` table).

3. **Result**: The result of this calculation is a percentage that indicates how much of the project's sales price has been realized through actual sales. If the total sales price is zero, the expression will return a blank (or no value), avoiding any division errors.

In summary, this DAX measure helps project managers or business analysts understand the effectiveness of a project in generating sales relative to its expected sales price. It provides insight into how much of the projected revenue has been achieved, which is crucial for assessing project performance.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the profit margin for a project by subtracting the total costs from the total sales. Here's a breakdown of what it does:

1. **Total Sales Calculation**: The expression starts with `sum(vw_Timesheet[SalesAmount])`, which adds up all the sales amounts recorded in the `vw_Timesheet` table. This represents the total revenue generated from the project.

2. **Total Costs Calculation**: Next, it calculates the total costs with `sum(vw_Timesheet[CostAmount])`. This sums up all the costs associated with the project, which could include expenses like labor, materials, and overhead.

3. **Profit Margin Calculation**: Finally, the expression subtracts the total costs from the total sales: `sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`. The result of this calculation is the project's profit margin, which indicates how much profit the project has generated after covering its costs.

In simple terms, this DAX measure helps a business understand how profitable a project is by showing the difference between what they earned (sales) and what they spent (costs). A positive result indicates a profit, while a negative result indicates a loss.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total sales price in a base currency for different projects and clients from a timesheet view. Here's a breakdown of what it does in simple business terms:

1. **SUMMARIZE Function**: The expression starts by creating a summary table from the `vw_Timesheet` data. This summary groups the data by two key dimensions: `ProjectProfile` and `Client`. Essentially, it organizes the timesheet entries based on which project and client they belong to.

2. **SELECTEDVALUE Function**: For each unique combination of `ProjectProfile` and `Client`, it retrieves the corresponding sales price in the base currency from the `tbl_Project` table. The `SELECTEDVALUE` function is used here to get the specific sales price for the current context of the project being evaluated.

3. **MeasureValue**: The result of the `SELECTEDVALUE` function is stored in a new column called "MeasureValue" within the summarized table. This column holds the sales price for each project-client combination.

4. **SUMX Function**: Finally, the `SUMX` function iterates over the summarized table and adds up all the values in the "MeasureValue" column. This gives the total sales price in the base currency across all the projects and clients included in the timesheet.

In summary, this DAX measure calculates the total sales price in a base currency for all projects and clients listed in the timesheet, providing a clear view of revenue based on the selected projects and clients.

**`Static Total Employees`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT(vw_Timesheet[EmployeeName]), 
			    ALL(vw_Timesheet),
			    vw_Timesheet[ContractStatusToday] = "Valid"
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of unique employees who have a "Valid" contract status on a specific day. Here’s a breakdown of what each part does:

1. **CALCULATE**: This function changes the context in which data is evaluated. It allows us to apply filters to the data before performing calculations.

2. **DISTINCTCOUNT(vw_Timesheet[EmployeeName])**: This part counts the number of unique employee names in the `vw_Timesheet` table. Essentially, it ensures that each employee is only counted once, even if they appear multiple times in the data.

3. **ALL(vw_Timesheet)**: This function removes any existing filters on the `vw_Timesheet` table. By doing this, it ensures that the count of employees is not affected by any other filters that might be applied in the report or dashboard.

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This is a filter condition that specifies we only want to consider employees whose contract status is "Valid" for the calculation.

Putting it all together, this DAX expression calculates the total number of unique employees who currently have a valid contract, ignoring any other filters that might be applied to the data. This is useful for understanding the active workforce under valid contracts at a given point in time.

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

In simple terms, this measure totals all the hours that have been logged in the timesheet, providing a single number that represents the overall hours worked. This can be useful for reporting, payroll calculations, or analyzing employee productivity.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the total contracted hours for all employees listed in the `vw_Timesheet` table. Here’s a breakdown of what it does in simple business terms:

1. **VALUES(vw_Timesheet[EmployeeName])**: This part of the expression creates a unique list of employee names from the `vw_Timesheet` table. Essentially, it identifies each employee without duplicates.

2. **MAX(vw_Timesheet[HoursPerWeek])**: For each employee in the unique list, this part finds the maximum number of hours that are recorded for that employee in the `HoursPerWeek` column. This means if an employee has multiple entries for hours worked, it will take the highest value.

3. **SUMX(...)**: The `SUMX` function then takes the unique employee list and the maximum hours per week for each employee and sums them up. This means it adds together the highest contracted hours for each employee to get a total.

In summary, this DAX measure calculates the total of the maximum contracted hours per week for all employees, ensuring that if an employee has multiple records, only their highest contracted hours are counted. This gives a clear picture of the total contracted hours across the workforce, based on the highest values recorded.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression `[TotalActualHours] - [TotalContractedHours]` calculates the difference between two values: the total actual hours worked and the total contracted hours agreed upon.

In simple business terms, this measure helps to determine how many hours have been worked beyond or below what was originally contracted. 

- **TotalActualHours**: This represents the actual hours that employees or resources have worked on a project or task.
- **TotalContractedHours**: This is the number of hours that were initially agreed upon in the contract for the project or task.

By subtracting the total contracted hours from the total actual hours, the measure provides insight into whether the project is over budget in terms of hours worked (if the result is positive) or under budget (if the result is negative). This information is crucial for project management, as it helps in assessing performance, managing resources, and making informed decisions about future work or contracts.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'TotalHoursTracked' that calculates the total hours recorded in a timesheet. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the 'Hours' column of the 'vw_Timesheet' table. Essentially, it calculates the total hours worked.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (i.e., if there are no hours recorded). 

3. **IF(...)**: The IF function evaluates the condition provided by ISBLANK. If the total hours calculated is blank (meaning there are no hours to sum), it returns 0. If there are hours recorded, it returns the total hours calculated.

In summary, this measure ensures that if there are no hours tracked, it will return 0 instead of a blank value. This is useful for reporting and analysis, as it provides a clear numerical value (0) rather than leaving it empty, which can help in understanding the data better.

**`TotalHoursTrackedMissing`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    "Empty",
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total hours tracked from a timesheet, but it also includes a check to see if there are any hours recorded. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the `Hours` column of the `vw_Timesheet` table.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (meaning there are no hours recorded).

3. **IF(...)**: This function evaluates the condition provided by ISBLANK. If the total hours are blank (i.e., there are no hours tracked), it returns the text "Empty". If there are hours tracked, it returns the total sum of those hours.

In simple terms, this measure checks if any hours have been logged in the timesheet. If no hours are logged, it indicates that with the word "Empty". If hours are logged, it provides the total number of hours tracked. This helps users quickly understand whether there is any data available or not.

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

1. **[TotalHoursTracked]**: This part represents the total number of hours that have been tracked for a specific employee or context. It could be the sum of hours worked, logged, or recorded.

2. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to find a specific value related to hours worked.

3. **MAXX Function**: This function is used to find the maximum value from a set of data. Here, it is applied to a distinct list of hours worked per week (from the `vw_Timesheet` table).

4. **DISTINCT(vw_Timesheet[HoursPerWeek])**: This part creates a unique list of hours worked per week, ensuring that each value is only counted once.

5. **ALLEXCEPT Function**: This function removes all filters from the `vw_Timesheet` table except for the filter on `EmployeeID`. This means that the calculation will consider all records for a specific employee, regardless of other filters that might be applied.

### What It Achieves:
The entire expression calculates the difference between the total hours tracked for an employee and the maximum hours they have worked in a single week. 

- If the total hours tracked is greater than the maximum hours worked in any week, the result will be positive, indicating that the employee has logged more hours overall than they did in their busiest week.
- Conversely, if the total hours tracked is less than or equal to the maximum weekly hours, the result will be zero or negative, suggesting that the employee has not exceeded their peak weekly hours.

In summary, this measure helps to assess whether an employee's total tracked hours exceed their maximum weekly performance, providing insights into their workload and productivity over time.

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
- **DAX Explanation (Generated):** The DAX expression you've provided calculates a measure called `trackedDiff2`, which essentially determines the difference between the total hours tracked and the maximum hours recorded per week for each employee, while ignoring any filters on the hours per week.

Here's a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked for a specific context (like a project, employee, or time period).

2. **Maximum Hours Per Week**: The expression then calculates the maximum number of hours that have been recorded in a week for each employee. This is done using the `MAXX` function, which looks at the distinct values of `HoursPerWeek` from the `vw_Timesheet` table.

3. **Removing Filters**: The `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])` part ensures that any filters applied to the `HoursPerWeek` column are ignored. This means that the calculation will consider all possible hours per week, not just those that might be filtered in the current context.

4. **Keeping Employee Context**: The `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` function keeps the context of the employee intact while removing other filters. This ensures that the maximum hours per week are calculated specifically for each employee, regardless of any other filters that might be applied to the data.

5. **Calculating the Difference**: Finally, the measure subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. This gives you the difference between what has been tracked and the highest amount of hours that could have been logged in a week for that employee.

In summary, `trackedDiff2` provides insight into how much the total hours tracked for an employee deviate from their maximum recorded hours per week. A positive result indicates that the employee has tracked fewer hours than their maximum, while a negative result would suggest they have exceeded their maximum weekly hours. This can help in understanding workload and efficiency for each employee.

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
   - The second condition uses the `SEARCH` function to look for the phrase "Customer Success Services" within the `Project` column. If this phrase is found (indicated by a result greater than 0), those records are also included in the filtered results.

3. **Summation of Hours**: After filtering the timesheet data based on the above criteria, the `SUMX` function is used to iterate over the filtered records and sum up the `Hours` column. This means it adds together all the hours that meet the filtering conditions.

4. **Return Value**: Finally, the expression returns the total sum of hours that are considered billable based on the defined criteria.

In simple terms, this DAX measure calculates the total number of hours worked on projects that are either specifically marked as billable or fall under the "Customer Success Services" category. This helps businesses track and report on billable hours effectively, ensuring they capture all relevant work for invoicing or performance analysis.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** This DAX expression is used to calculate the total number of hours worked on specific projects that meet a certain qualification criteria. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the values in the "Hours" column from the "vw_Timesheet" table. Essentially, it totals the hours recorded.

2. **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to include only those entries where the "QualifyPrj" column equals 1. This means it focuses on projects that are marked as qualified.

3. **CALCULATE**: This function modifies the context in which the data is evaluated. It takes the total hours calculated in the first part and applies the filter from the second part.

In simple terms, this DAX measure calculates the total hours worked on projects that are considered qualified (where "QualifyPrj" is 1). It helps businesses understand how many hours are being spent on projects that meet specific criteria, which can be useful for reporting, budgeting, or performance analysis.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the "Billable Hour Ratio" for active employees in a business context. Here's a breakdown of what it does in simple terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable captures the total number of billable hours worked by employees. It pulls this value from another measure called `[UTI_TotalBillableHours]`.
   - `_TotalHours`: This variable captures the total hours worked by employees, sourced from another measure called `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The expression checks if there are any active contacts (employees) using `[Msr_ActiveContacts]`. If there are no active contacts, the calculation will not proceed.
   - If there are active contacts, it then checks if the total hours worked (`_TotalHours`) is not blank. If it is blank, the calculation will not proceed further.
   - If both conditions are met (active contacts exist and total hours are available), it calculates the Billable Hour Ratio by dividing the total billable hours by the total hours worked.

3. **Handling Division**:
   - The `DIVIDE` function is used to perform the division. This function is preferred because it safely handles cases where the denominator (total hours) might be zero or blank. If the result of the division is blank (which can happen if total hours are zero), it returns 0 instead.

**In Summary**: This DAX measure calculates the ratio of billable hours to total hours worked for active employees. It ensures that the calculation only occurs when there are active employees and valid total hours, providing a clear metric of how effectively employees are utilizing their time for billable work.

---

