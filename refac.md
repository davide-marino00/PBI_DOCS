# Power BI Model & Report Documentation

*Generated on: 2025-04-28 17:10:48*

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

---

## <a name="overview"></a>Overview

The Power BI data model appears to be designed for a business domain focused on project management and timesheet tracking, likely within a professional services or consulting environment. The presence of tables such as 'vw_Timesheet' and 'vw_missing_timesheet' suggests a strong emphasis on monitoring employee hours worked against various projects, units, and codes, which are essential for resource allocation, billing, and performance analysis. The inclusion of lookup tables like 'lkp_Project', 'lkp_Unit', and 'lkp_Code' indicates a structured approach to categorizing and managing data related to projects, organizational units, and timesheet codes, facilitating detailed reporting and insights.

The 'DimDate' table serves as a central date dimension, allowing for time-based analysis of timesheet entries and project timelines, while the 'Hours_nd_Percentage' table likely provides metrics on hours worked and their corresponding percentages, further enhancing the analytical capabilities of the model. Overall, this data model is structured to provide comprehensive visibility into project performance, employee productivity, and potential gaps in timesheet submissions, enabling informed decision-making and operational efficiency within the organization.

---

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

The 'DimDate' table serves as a comprehensive date dimension for analytical reporting, enabling businesses to perform time-based analysis by providing essential date attributes such as day, week, month, and quarter. This table facilitates the aggregation and filtering of data across various time periods, enhancing insights into trends and performance metrics.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `CalendarQuarter` | `string` | The 'CalendarQuarter' column represents the fiscal quarter of the year in a string format, allowing for easy categorization and analysis of data trends and performance metrics across quarterly time periods within the DimDate table. |
| `CalendarYear` | `int64` | The 'CalendarYear' column (int64) in the 'DimDate' table represents the four-digit year value, enabling users to aggregate and analyze data across different calendar years for enhanced time-based reporting and trend analysis. |
| `DateKey` | `int64` | The 'DateKey' column (int64) uniquely identifies each date in the 'DimDate' table, serving as a primary key that enables efficient time-based analysis and aggregation of data across various reporting periods. |
| `DayName` | `string` | The 'DayName' column contains the full name of the day (e.g., Monday, Tuesday) corresponding to each date, enabling users to analyze data trends and performance metrics based on specific days of the week. |
| `DayNumberOfMonth` | `int64` | The 'DayNumberOfMonth' column (int64) represents the numerical day of the month, ranging from 1 to 31, and is used to facilitate detailed time-based analysis and reporting within the 'DimDate' table. |
| `DayNumberOfWeek` | `int64` | The 'DayNumberOfWeek' column (int64) represents the numerical day of the week, ranging from 1 (Sunday) to 7 (Saturday), facilitating time-based analysis and reporting within the 'DimDate' table. |
| `DayNumberOfYear` | `int64` | The 'DayNumberOfYear' column represents the sequential day of the year (ranging from 1 to 365 or 366) for each date entry, enabling precise time-based analysis and reporting within the DimDate table. |
| `MonthName` | `string` | The 'MonthName' column contains the full name of each month, providing a user-friendly reference for time-based analysis and reporting within the 'DimDate' table. |
| `MonthNumberOfYear` | `int64` | The 'MonthNumberOfYear' column (int64) in the 'DimDate' table represents the numerical value of the month (1 for January through 12 for December), enabling efficient time-based analysis and reporting by facilitating the aggregation and filtering of data across monthly periods. |
| `ShortDate` | `dateTime` | The 'ShortDate' column represents a simplified date format that allows for easy identification and comparison of specific dates within the 'DimDate' table, supporting efficient time-based analysis and reporting. |
| `WeekNumberOfYear` | `int64` | The 'WeekNumberOfYear' column (int64) in the 'DimDate' table represents the sequential week number within the calendar year, enabling businesses to analyze and aggregate data on a weekly basis for enhanced time-based reporting and trend analysis. |

##### Calculated Columns

**`CurrentYearFlag`**

- **Description:** The 'CurrentYearFlag' column indicates whether a given date falls within the current calendar year, facilitating time-based analysis and reporting by allowing users to easily filter and aggregate data for the current year.
- **DAX Expression:**
```dax
IF(YEAR(DimDate[ShortDate]) <= YEAR(TODAY()), 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'CurrentYearFlag' in a data model, specifically in a date dimension table (DimDate). Here's a breakdown of what it does in simple business terms:

1. **YEAR(DimDate[ShortDate])**: This part extracts the year from the date in the 'ShortDate' column of the DimDate table. For example, if the date is '2023-05-15', it will return '2023'.

2. **YEAR(TODAY())**: This part gets the current year based on today's date. If today is any date in 2023, this will return '2023'.

3. **IF(..., 1, 0)**: This is a conditional statement. It checks if the year extracted from the 'ShortDate' is less than or equal to the current year. If it is, the expression returns '1'; if not, it returns '0'.

### What it achieves:
- The 'CurrentYearFlag' column will have a value of '1' for all dates that are in the current year or any previous year. 
- It will have a value of '0' for any future dates (dates that are in the next year or beyond).

### Business Use:
This calculated column can be useful for filtering or segmenting data in reports. For instance, you can easily identify and analyze all records that are relevant to the current year and prior years, while excluding any future dates. This helps in making timely business decisions based on historical and current data.

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

The 'Hours_nd_Percentage' table is designed to track and analyze the distribution of hours worked alongside their corresponding percentage contributions to overall productivity or project completion. This table aids in performance evaluation and resource allocation by providing insights into how time is spent across various tasks or projects.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Hours_nd_Percentage` | `string` | The 'Hours_nd_Percentage' column captures the string representation of hours worked and their associated percentage contributions, facilitating the analysis of time allocation and productivity across tasks or projects. |
| `Hours_nd_Percentage Fields` | `string` | The 'Hours_nd_Percentage Fields' column contains string values that represent the specific categories or labels associated with the hours worked and their percentage contributions, facilitating detailed analysis of time distribution across tasks or projects. |
| `Hours_nd_Percentage Order` | `string` | The 'Hours_nd_Percentage Order' column categorizes the sequence of hours worked and their associated percentage contributions, facilitating the analysis of productivity trends and resource allocation across tasks or projects. |

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

The 'lkp_Code' table serves as a reference dataset that categorizes various codes used across the business, with associated qualification levels and sorting criteria to facilitate efficient data retrieval and reporting. This table is essential for ensuring consistency and accuracy in code-related data analysis and decision-making processes.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Code` | `string` | The 'Code' column contains unique string identifiers that categorize various business codes, serving as a key reference for ensuring consistency and accuracy in data analysis and reporting across the organization. |
| `Qualify` | `int64` | The 'Qualify' column (int64) indicates the qualification level associated with each code in the 'lkp_Code' table, enabling effective categorization and prioritization for data retrieval and reporting. |
| `Sort` | `string` | The 'Sort' column contains string values that define the sorting criteria for the codes in the 'lkp_Code' table, enabling organized retrieval and reporting of categorized data. |

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

The 'lkp_fltr_Employee' table serves as a reference for employee data, providing essential identifiers and names to facilitate filtering and reporting across various business analytics. This table enhances data integrity and enables efficient data retrieval related to employee performance, engagement, and resource allocation.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `EmployeeId` | `string` | The 'EmployeeId' column (string) uniquely identifies each employee in the 'lkp_fltr_Employee' table, serving as a critical reference point for accurate filtering and reporting in business analytics related to employee performance and resource management. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees, serving as a key identifier to enhance data retrieval and reporting capabilities within the employee reference framework. |

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

The 'lkp_Project' table serves as a reference for project-related data, detailing key attributes such as project names, qualification metrics, billing amounts, and associated groups, enabling businesses to analyze project performance and financials effectively.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Billing` | `int64` | The 'Billing' column (int64) in the 'lkp_Project' table represents the total billing amount associated with each project, facilitating financial analysis and performance evaluation. |
| `Group` | `string` | The 'Group' column identifies the specific organizational unit or team associated with each project, facilitating the analysis of project performance and financials by grouping related projects together. |
| `Project` | `string` | The 'Project' column contains the names of various projects, serving as a key identifier for referencing and analyzing project-related performance and financial metrics within the 'lkp_Project' table. |
| `Qualify` | `int64` | The 'Qualify' column (int64) indicates the qualification status of a project, serving as a metric to assess its eligibility for specific criteria or performance benchmarks within the project reference framework. |

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

The 'lkp_Unit' table serves as a reference for organizational units within the business, detailing attributes such as billable department status and external affiliations, which aids in resource allocation and financial tracking. This table is essential for ensuring accurate reporting and analysis of departmental performance and billing activities.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `BillableDep` | `int64` | The 'BillableDep' column indicates whether the organizational unit is classified as a billable department, facilitating accurate financial tracking and resource allocation within the business. |
| `Organisatorische eenheid` | `string` | The 'Organisatorische eenheid' column identifies the specific organizational unit within the business, facilitating effective resource allocation and financial tracking by linking departmental performance and billing activities to their respective units. |
| `OWN-Sub-ExtT` | `string` | The 'OWN-Sub-ExtT' column contains string values that represent the external type affiliations of organizational units, facilitating the identification of their relationships with external entities for improved resource allocation and financial tracking. |
| `Unit` | `string` | The 'Unit' column contains the names of organizational units, serving as a key reference for identifying and categorizing departments involved in resource allocation and financial tracking. |

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

The 'tbl_Project' table serves as a comprehensive repository for project-related information, capturing essential details such as project identifiers, descriptions, leadership, and status. This table enables businesses to effectively manage and analyze their projects, facilitating better decision-making and resource allocation across various project groups.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualDateCompleted` | `dateTime` | 'ActualDateCompleted' records the precise date and time when a project was officially completed, providing critical data for performance analysis and project timeline assessments within the 'tbl_Project' repository. |
| `Administration` | `string` | The 'Administration' column contains string values that represent the administrative oversight or management team responsible for the project's execution and governance within the organization. |
| `Changed_on` | `dateTime` | The 'Changed_on' column records the date and time when the project details were last modified, providing a crucial timestamp for tracking updates and changes within the project management lifecycle. |
| `ClientName` | `string` | The 'ClientName' column stores the name of the client associated with each project, providing essential context for project management and stakeholder engagement within the 'tbl_Project' table. |
| `ClientNumber` | `string` | The 'ClientNumber' column stores a unique identifier for each client associated with a project, enabling efficient tracking and management of client-related project activities within the 'tbl_Project' table. |
| `Created_on` | `dateTime` | The 'Created_on' column records the date and time when each project was initiated, providing a timeline for project management and analysis within the 'tbl_Project' table. |
| `Currency` | `string` | The 'Currency' column stores the currency type used for financial transactions related to each project, enabling accurate budgeting and financial analysis within the project management framework. |
| `DateCreated` | `dateTime` | The 'DateCreated' column records the exact date and time when each project entry was initiated, providing a chronological reference for project management and analysis within the 'tbl_Project' table. |
| `DevoteamPillar` | `string` | The 'DevoteamPillar' column identifies the specific strategic focus area or framework under which the project is categorized, aiding in the alignment of project objectives with organizational goals. |
| `PlannedCompletionDate` | `dateTime` | The 'PlannedCompletionDate' column records the anticipated date and time for the project's completion, serving as a critical milestone for project management and resource planning within the 'tbl_Project' table. |
| `Project` | `string` | The 'Project' column contains the name or title of the project, serving as a key identifier that allows for easy reference and organization within the comprehensive project management framework of the 'tbl_Project' table. |
| `ProjectDescription` | `string` | The 'ProjectDescription' column provides a detailed narrative of the project's objectives, scope, and key features, enhancing understanding and communication among stakeholders involved in project management and execution. |
| `ProjectGroup` | `string` | The 'ProjectGroup' column identifies the specific group or team responsible for overseeing and executing the project, enabling streamlined management and collaboration within the organization. |
| `ProjectGroupDescription` | `string` | The 'ProjectGroupDescription' column provides a detailed narrative of the specific project group associated with each project, enhancing understanding and context for project management and analysis. |
| `ProjectKey` | `int64` | The 'ProjectKey' column (int64) uniquely identifies each project within the 'tbl_Project' table, serving as a primary reference for managing and analyzing project-related information. |
| `ProjectleaderId` | `string` | The 'ProjectleaderId' column stores the unique identifier of the individual responsible for leading the project, enabling effective tracking of leadership roles and accountability within the project's lifecycle. |
| `SalesPriceBaseCurrency` | `decimal` | The 'SalesPriceBaseCurrency' column (decimal) in the 'tbl_Project' table represents the base currency sales price for each project, enabling accurate financial analysis and reporting for effective project management and resource allocation. |
| `Startdate` | `dateTime` | The 'Startdate' column records the date and time when a project officially commences, providing a crucial reference point for project timelines and scheduling within the 'tbl_Project' table. |
| `Status` | `string` | The 'Status' column indicates the current phase or condition of each project, providing essential insights for effective project management and resource allocation. |
| `Statuscode` | `string` | The 'Statuscode' column indicates the current status of each project, allowing for effective tracking and management of project progress within the tbl_Project table. |
| `TotalContractSum` | `decimal` | The 'TotalContractSum' column represents the total monetary value of the contract associated with the project, enabling businesses to assess financial commitments and manage budgets effectively within the project management framework. |

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

The 'vw_missing_timesheet' table serves as a critical resource for identifying employees who have not submitted their timesheets within a specified timeframe, enabling managers to address compliance issues and ensure accurate payroll processing. By tracking missing submissions alongside employee and contract details, this table supports effective workforce management and operational efficiency.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `ActualHours` | `double` | 'ActualHours' (double) represents the total number of hours an employee has worked, providing essential data for assessing compliance with timesheet submissions and ensuring accurate payroll calculations in the context of missing timesheet tracking. |
| `Approved` | `boolean` | Indicates whether the missing timesheet has been approved by management, facilitating oversight and compliance in payroll processing. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column indicates the date and time when an employee's contract concludes, providing essential context for evaluating timesheet submission compliance and ensuring accurate payroll processing within the 'vw_missing_timesheet' table. |
| `ContractHours` | `int64` | The 'ContractHours' column (int64) represents the total number of hours an employee is contracted to work, providing essential context for evaluating timesheet compliance and ensuring accurate payroll calculations in the 'vw_missing_timesheet' table. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column captures the date and time when an employee's contract begins, providing essential context for evaluating timesheet submission compliance and ensuring accurate payroll processing within the 'vw_missing_timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of the employee's contract, providing essential context for understanding the implications of missing timesheet submissions on payroll and compliance. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee in the 'vw_missing_timesheet' table, facilitating the tracking of timesheet submission compliance and supporting effective workforce management. |
| `EmployeeName` | `string` | The 'EmployeeName' column contains the names of employees who have not submitted their timesheets, facilitating the identification of compliance issues and supporting effective workforce management. |
| `Hours_` | `double` | The 'Hours_' column (double) represents the total number of hours an employee is expected to report on their timesheet, facilitating the identification of discrepancies in timesheet submissions for effective payroll management. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the date and time when the missing timesheet data was captured, facilitating timely identification and resolution of compliance issues related to employee timesheet submissions. |
| `ManagerID` | `string` | The 'ManagerID' column (string) identifies the unique identifier of the manager responsible for overseeing the employee associated with the missing timesheet, facilitating targeted follow-up and accountability in timesheet compliance. |
| `ManagerName` | `string` | The 'ManagerName' column contains the name of the employee's manager, facilitating accountability and communication regarding timesheet submissions and compliance within the 'vw_missing_timesheet' table. |
| `MissingHours` | `double` | The 'MissingHours' column (double) quantifies the total number of hours an employee has failed to report on their timesheet, facilitating targeted follow-up actions to ensure compliance and accurate payroll processing. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with the timesheet entries, facilitating targeted follow-up on missing submissions related to particular assignments. |
| `ProjectCode` | `string` | The 'ProjectCode' column contains a string identifier for the specific project associated with the timesheet, facilitating the tracking of employee contributions and ensuring accurate allocation of labor costs within the 'vw_missing_timesheet' table. |
| `Rejected` | `boolean` | Indicates whether the timesheet submission has been rejected, aiding in the identification of compliance issues and facilitating timely follow-up actions by management. |
| `ReportReady` | `boolean` | Indicates whether the report on missing timesheets is ready for review, facilitating timely managerial action on compliance and payroll accuracy. |
| `SalesAmount` | `double` | The 'SalesAmount' column (double) represents the total revenue generated by each employee during the specified timeframe, providing insights into financial performance alongside timesheet compliance for effective workforce management. |
| `SalesPrice` | `double` | The 'SalesPrice' column (double) represents the monetary value assigned to sales transactions, providing essential financial data for analyzing revenue impacts related to employee timesheet submissions and compliance. |
| `ShortDate` | `dateTime` | The 'ShortDate' column captures the specific date and time when an employee's timesheet submission is deemed missing, facilitating timely follow-up and compliance management. |
| `SubUnit` | `string` | The 'SubUnit' column identifies the specific sub-unit or department within the organization to which the employee belongs, facilitating targeted follow-up on missing timesheet submissions for improved compliance and payroll accuracy. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column contains a unique identifier for each timesheet submission, facilitating the tracking and management of missing timesheets for compliance and payroll accuracy. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a textual summary of the specific timesheet entry or period that is missing, aiding managers in understanding the context of the non-submission for effective follow-up and resolution. |
| `Week_` | `int64` | The 'Week_' column (int64) indicates the specific week number during which an employee's timesheet submission is missing, facilitating targeted follow-up and compliance management. |
| `Year_` | `int64` | The 'Year_' column (int64) indicates the calendar year associated with the timesheet submissions, facilitating the identification of missing timesheets for specific years to enhance compliance and payroll accuracy. |

##### Calculated Columns

**`BillableDep`**

- **Description:** The 'BillableDep' column indicates the department associated with billable work for employees, aiding in the identification of missing timesheets and ensuring accurate allocation of labor costs for payroll and project management.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[BillableDep])), 
			    1, 
			    IF(RELATED(lkp_Unit[BillableDep]) <> 0, 1, 0)
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'BillableDep'. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Values**: The expression first checks if the value in the 'BillableDep' column from a related table called 'lkp_Unit' is blank (i.e., has no value). This is done using the `ISBLANK` function.

2. **Return 1 for Blank Values**: If the 'BillableDep' value is blank, the expression returns a value of 1. This indicates that if there is no information available about whether the unit is billable, it defaults to being considered billable.

3. **Check for Non-Zero Values**: If the 'BillableDep' value is not blank, the expression then checks if it is not equal to zero. This is done using the second `IF` statement.

4. **Return 1 for Non-Zero Values**: If the 'BillableDep' value is not zero, it returns 1, indicating that the unit is billable.

5. **Return 0 for Zero Values**: If the 'BillableDep' value is zero, the expression returns 0, indicating that the unit is not billable.

In summary, this DAX expression effectively categorizes units as billable or not based on the 'BillableDep' value from the related table. If there is no information (blank), it assumes the unit is billable (returns 1). If there is information and it is non-zero, it also considers it billable (returns 1). If the value is zero, it marks it as not billable (returns 0).

**`CC_ActiveEmployees`**

- **Description:** The 'CC_ActiveEmployees' column indicates the status of employees currently active within the organization, facilitating the identification of those who may be missing timesheet submissions for timely managerial intervention.
- **DAX Expression:**
```dax
IF(vw_missing_timesheet[ShortDate] >= vw_missing_timesheet[ContractStartDate] && 
			      (vw_missing_timesheet[ShortDate] <= vw_missing_timesheet[ContractEndDate] || ISBLANK(vw_missing_timesheet[ContractEndDate]))
			        , 1
			        , 0
			    )
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named `CC_ActiveEmployees` in a data model, specifically within a table called `vw_missing_timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether an employee is considered "active" based on their contract dates and a specific date (referred to as `ShortDate`).

2. **Conditions Checked**:
   - **Contract Start Date**: It first checks if the `ShortDate` (which likely represents a date when a timesheet is missing) is on or after the employee's `ContractStartDate`. This means the employee should have started their contract before or on this date.
   - **Contract End Date**: Next, it checks if the `ShortDate` is on or before the `ContractEndDate`. If the `ContractEndDate` is blank (meaning the employee's contract is still active or ongoing), the condition is still satisfied.

3. **Output**:
   - If both conditions are true (the date falls within the contract period or the contract is still active), the expression returns a value of `1`, indicating that the employee is "active."
   - If either condition is not met, it returns `0`, indicating that the employee is not considered "active."

In summary, this DAX expression effectively flags employees as active or inactive based on whether the date in question falls within their contract period, helping the business track which employees are currently under contract.

**`IncompleteFlag`**

- **Description:** The 'IncompleteFlag' column indicates whether an employee's timesheet submission is incomplete, serving as a key marker for managers to identify compliance issues and take necessary actions to ensure timely payroll processing.
- **DAX Expression:**
```dax
if(vw_missing_timesheet[MissingHours] < 0, 1, 0)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IncompleteFlag' in a data table named `vw_missing_timesheet`. 

Here's what it does in simple terms:

- It checks the value in the column `MissingHours` for each row in the `vw_missing_timesheet` table.
- If the value of `MissingHours` is less than 0, it assigns a value of 1 to the 'IncompleteFlag' column for that row. This indicates that there is an issue or that the timesheet is incomplete.
- If the value of `MissingHours` is 0 or greater, it assigns a value of 0 to the 'IncompleteFlag' column, indicating that there are no issues with the timesheet.

In summary, this expression helps identify rows where there are missing hours in timesheets, marking them as incomplete with a flag of 1, while marking complete timesheets with a flag of 0.

**`MAIN_UNIT`**

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit or department to which the employee belongs, facilitating targeted follow-up on missing timesheet submissions for improved compliance and payroll accuracy.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'MAIN_UNIT'. Here's a simple breakdown of what it does:

1. **Check for Blank Values**: The expression first checks if there is a related value in the 'Unit' column from the 'lkp_Unit' table. It uses the `ISBLANK` function to determine if this related value is empty or not.

2. **Return "Unknown" if Blank**: If the related 'Unit' value is blank (meaning there is no corresponding unit found), the expression will return the text "Unknown". This helps to clearly indicate that there is no valid unit associated with the current record.

3. **Return the Related Unit if Not Blank**: If there is a valid related 'Unit' value (i.e., it is not blank), the expression will return that value. This means that the calculated column will show the actual unit associated with the record.

In summary, this DAX expression effectively populates the 'MAIN_UNIT' column with either the corresponding unit name from the 'lkp_Unit' table or "Unknown" if no unit is found. This ensures that users can easily identify records that lack a valid unit, improving data clarity and usability.

**`YearWeek`**

- **Description:** The 'YearWeek' column (string) represents the specific year and week number during which timesheet submissions are monitored, facilitating the identification of compliance gaps in employee reporting.
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

In summary, this DAX expression generates a string that represents the year and week number of the 'ContractEndDate', formatted as "YYYY-WW". This can be useful for reporting or analysis purposes, allowing users to easily identify and group data by year and week.

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

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it counts the unique names of employees from the 'EmployeeName' column in the 'vw_missing_timesheet' table.

3. **Filter Condition**: The expression includes a filter condition that specifies only to consider records where the 'MissingHours' column has a value less than 0. This means it focuses on instances where employees have reported negative hours.

In simple terms, this measure calculates how many different employees have reported negative hours on their timesheets. This could be useful for identifying issues with timesheet reporting or understanding employee workload discrepancies.

**`CountNegativeMissingHours`**

- **DAX Expression:**
```dax
CALCULATE(
			    DISTINCTCOUNT('vw_missing_timesheet'[EmployeeID]),
			    'vw_missing_timesheet'[MissingHours] < 0
			)
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the number of unique employees who have recorded negative missing hours in a timesheet report. 

Here's a breakdown of what it does:

1. **CALCULATE Function**: This function modifies the context in which data is evaluated. In this case, it is used to apply a specific filter to the data.

2. **DISTINCTCOUNT Function**: This counts the number of unique values in a specified column. Here, it counts the unique `EmployeeID`s from the `vw_missing_timesheet` table.

3. **Filter Condition**: The expression includes a filter that only considers records where the `MissingHours` is less than 0. This means it focuses on instances where employees have negative missing hours, which could indicate an error or a specific situation that needs attention.

In summary, this measure counts how many different employees have negative missing hours recorded in the timesheet data. This can help the business identify potential issues with timesheet entries or track employees who may need follow-up regarding their reported hours.

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

1. **Data Source**: The expression starts by looking at a data table called `vw_missing_timesheet`, which likely contains records of employees' hours worked, their contracts, and whether they are currently active.

2. **Summarization**: It summarizes the data by grouping it based on several key attributes:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a more specific division within the unit)

3. **Calculating Missing Hours**: For each group, it calculates two important metrics:
   - **MissingHours**: This is calculated by taking the total hours worked (from `vw_missing_timesheet[Hours_]`) and subtracting the maximum contracted hours (from `vw_missing_timesheet[ContractHours]`). If this value is negative, it indicates that the employee has not logged enough hours compared to what they are contracted for.
   - **MinActiveEmp**: This checks if the employee is active by taking the minimum value of a field that indicates whether the employee is currently active (`vw_missing_timesheet[CC_ActiveEmployees]`). If this value is 1, it means the employee is active.

4. **Filtering**: After summarizing the data, it filters the results to only include those employees who are:
   - Active (MinActiveEmp = 1)
   - Have negative missing hours (MissingHours < 0), meaning they have not logged enough hours.

5. **Counting Active Employees**: Finally, the expression counts the number of rows that meet these criteria, which gives the total number of active employees who are missing timesheet hours.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total number of "missing hours" for active employees who have not met their contracted hours in a specific time frame. Here's a breakdown of what it does in simple business terms:

1. **Identify Active Employees**: The code starts by creating a variable called `ActiveEmployees`. It filters a summarized table (`vw_missing_timesheet`) that contains information about employees, their working hours, and their contract hours.

2. **Summarize Data**: Within this summarized table, it groups the data by:
   - Employee Name
   - Year
   - Week
   - Unit (likely a department or team)
   - SubUnit (a smaller division within the unit)

   For each group, it calculates two key pieces of information:
   - **MissingHours**: This is calculated by taking the total hours worked by the employee in that week and subtracting their contracted hours. If the result is negative, it indicates that the employee has worked fewer hours than expected.
   - **MinActiveEmp**: This checks if the employee is active (represented by a value of 1 in the `CC_ActiveEmployees` column).

3. **Filter for Relevant Employees**: The `FILTER` function then narrows down this summarized data to only include:
   - Employees who are active (where `MinActiveEmp` equals 1)
   - Employees who have missing hours (where `MissingHours` is less than 0)

4. **Calculate Total Missing Hours**: Finally, the `RETURN` statement uses `SUMX` to sum up the `MissingHours` for all the filtered active employees. This gives the total number of hours that active employees are missing compared to their contracted hours.

In summary, this DAX measure calculates the total hours that active employees have fallen short of their expected working hours, helping the business identify potential issues with employee attendance or workload management.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total expected contract hours for employees on a weekly basis. Here's a breakdown of what it does in simple terms:

1. **Data Source**: It uses a table called `vw_missing_timesheet`, which likely contains records of timesheets for employees, including their contract hours and the week associated with each entry.

2. **Grouping Data**: The `SUMMARIZE` function is used to group the data by two key pieces of information:
   - The week (`vw_missing_timesheet[Week_]`)
   - The employee ID (`vw_missing_timesheet[EmployeeID]`)

   For each unique combination of week and employee, it calculates the maximum contract hours (`MAX(vw_missing_timesheet[ContractHours])`). This means if an employee has multiple entries for the same week, it will take the highest number of contract hours recorded for that week.

3. **Calculating Total**: After summarizing the data, the `SUMX` function iterates over the summarized table. It sums up the maximum contract hours calculated for each employee for each week.

4. **Final Result**: The final output of this measure is the total expected contract hours for all employees across all weeks, ensuring that if there are multiple entries for an employee in a week, only the highest contract hours are considered.

In summary, this DAX measure helps to determine the total expected contract hours for employees, ensuring that only the highest recorded hours for each employee in each week are counted, which can be useful for reporting and analysis of employee workload and availability.

**`MISSING%`**

- **DAX Expression:**
```dax
DIVIDE([Dax_EmpCount]-[Dax_EmpCount_MissingTS],[Dax_EmpCount]) * 100
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of employees that are not missing a timestamp (TS) in a dataset. Here's a breakdown of what it does:

1. **[Dax_EmpCount]**: This represents the total count of employees in the dataset.

2. **[Dax_EmpCount_MissingTS]**: This represents the count of employees who are missing a timestamp. A missing timestamp could indicate that certain data for those employees is incomplete or not recorded.

3. **[Dax_EmpCount] - [Dax_EmpCount_MissingTS]**: This part of the expression calculates the number of employees who have a timestamp. It does this by subtracting the count of employees missing a timestamp from the total employee count.

4. **DIVIDE(..., [Dax_EmpCount])**: The `DIVIDE` function is used to safely perform division. It takes the number of employees with a timestamp (from the previous step) and divides it by the total employee count. This gives the proportion of employees who have a timestamp.

5. **... * 100**: Finally, the result is multiplied by 100 to convert the proportion into a percentage.

In summary, this DAX measure calculates the percentage of employees who have a timestamp recorded, helping to assess data completeness and identify potential gaps in employee data. A higher percentage indicates better data quality regarding employee timestamps.

**`MissingHoursOut`**

- **DAX Expression:**
```dax
MAX(vw_missing_timesheet[ContractHours]) - MAX(vw_missing_timesheet[Hours_])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the difference between two values related to hours worked and contracted hours for employees or contractors. Here's a breakdown of what it does in simple business terms:

1. **MAX(vw_missing_timesheet[ContractHours])**: This part of the expression finds the maximum number of contracted hours from the `vw_missing_timesheet` table. Contracted hours refer to the total hours that an employee or contractor is expected to work as per their contract.

2. **MAX(vw_missing_timesheet[Hours_])**: This part finds the maximum number of hours actually recorded or worked by the employee or contractor from the same table.

3. **Subtraction**: The expression then subtracts the maximum recorded hours (actual hours worked) from the maximum contracted hours. 

**What it achieves**: The result of this calculation gives you the maximum number of hours that are missing or unaccounted for, which means it shows how many hours an employee or contractor has not worked compared to what they were supposed to work. This can help in identifying gaps in hours worked, which is important for payroll, project management, or resource allocation. 

In summary, this measure helps businesses understand the shortfall in hours worked against what was expected, allowing for better management of workforce resources.

**`PercentageCompleteness`**

- **DAX Expression:**
```dax
DIVIDE(
			    SUM(vw_missing_timesheet[MissingHours])- [ExpectedContractHoursWeekly] ,
			    [ExpectedContractHoursWeekly],
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of completeness regarding missing hours in relation to expected contract hours on a weekly basis. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_missing_timesheet[MissingHours])**: This part adds up all the missing hours recorded in the `vw_missing_timesheet` table. Essentially, it tells you how many hours are missing from the timesheets.

2. **[ExpectedContractHoursWeekly]**: This is a measure that represents the total number of hours that are expected to be worked in a week according to the contract. 

3. **SUM(vw_missing_timesheet[MissingHours]) - [ExpectedContractHoursWeekly]**: Here, the expression calculates the difference between the total missing hours and the expected contract hours. If the result is positive, it indicates that the missing hours exceed what was expected; if negative, it means that the hours worked are more than expected.

4. **DIVIDE(..., [ExpectedContractHoursWeekly], 0)**: The `DIVIDE` function then takes the result from the previous step (the difference) and divides it by the expected contract hours. This gives you a ratio that represents how much the missing hours deviate from what was expected. The third argument, `0`, ensures that if the expected contract hours are zero (to avoid division by zero), the result will simply be 0 instead of an error.

In summary, this measure calculates the percentage of completeness by showing how the actual missing hours compare to the expected hours. A positive result indicates a shortfall in hours worked compared to what was expected, while a negative result indicates that more hours were worked than expected. This helps in assessing the performance and compliance with expected work hours.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total number of unique employees who have missing timesheets, grouped by week and main unit. Here’s a breakdown of what it does in simple business terms:

1. **Data Source**: The expression uses a data table called `vw_missing_timesheet`, which presumably contains records of employees and their timesheet submissions.

2. **Grouping**: The `SUMMARIZE` function is used to create a summary table. This table groups the data by three columns:
   - `Week_`: The week of the year.
   - `EmployeeName`: The name of the employee.
   - `MAIN_UNIT`: The main unit or department the employee belongs to.

3. **IncompleteFlag**: For each group created by the `SUMMARIZE` function, it calculates a new column called "IncompleteFlag". This column uses the `MAX` function to determine if there is any indication of incompleteness in the timesheet submissions for that employee in that week. If at least one record indicates that the timesheet is incomplete, the flag will be set to 1 (true); otherwise, it will be 0 (false).

4. **Summation**: The outer `SUMX` function then takes this summarized table and sums up the values in the "IncompleteFlag" column. This effectively counts how many unique employee-week-unit combinations have an incomplete timesheet.

In summary, the measure `UniqueEmployeesPerUnit` calculates the total number of unique employees who have not submitted their timesheets for each week and main unit, helping the business identify areas where timesheet compliance may need improvement.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

The 'vw_Timesheet' table serves as a comprehensive record of employee timesheet submissions, capturing essential details such as financial year, submission dates, and associated project and managerial information. This table enables businesses to analyze labor allocation, project costs, and managerial oversight, facilitating informed decision-making and resource management.

##### Columns

| Name | Data Type | Description (Generated) |
|------|-----------|-------------------------|
| `Approved` | `boolean` | Indicates whether the timesheet submission has been approved by management, facilitating tracking of approval status for accurate labor cost analysis and resource allocation. |
| `Client` | `string` | The 'Client' column identifies the client associated with each timesheet entry, providing essential context for analyzing project costs and labor allocation in relation to specific client engagements. |
| `Code2` | `string` | The 'Code2' column stores a secondary identifier for projects or tasks associated with employee timesheet submissions, aiding in the categorization and analysis of labor allocation and project costs. |
| `ContractEndDate` | `dateTime` | The 'ContractEndDate' column indicates the date and time when an employee's contract is set to expire, providing critical information for managing labor resources and project staffing within the context of timesheet submissions. |
| `ContractStartDate` | `dateTime` | The 'ContractStartDate' column records the date and time when an employee's contract begins, providing essential context for analyzing timesheet submissions in relation to employment duration and project allocation within the 'vw_Timesheet' table. |
| `ContractStatusToday` | `string` | The 'ContractStatusToday' column indicates the current status of employee contracts as of today, providing essential context for evaluating timesheet submissions and associated labor costs within the 'vw_Timesheet' table. |
| `ContractStatusTodayPBI` | `string` | The 'ContractStatusTodayPBI' column indicates the current status of employee contracts as of today, providing essential insights for analyzing labor allocation and project management within the timesheet records. |
| `ContractTransferDate` | `dateTime` | The 'ContractTransferDate' column records the specific date and time when an employee's contract was transferred, providing critical context for timesheet submissions and aiding in the analysis of labor allocation and project costs. |
| `CostAmount` | `double` | The 'CostAmount' column represents the monetary value associated with the hours worked by employees on specific projects, enabling businesses to assess labor costs and budget allocations effectively. |
| `CostPrice` | `double` | The 'CostPrice' column represents the monetary value associated with labor costs for each timesheet entry, enabling businesses to assess project expenditures and optimize resource allocation. |
| `Currency` | `string` | The 'Currency' column specifies the type of currency used for financial transactions related to the timesheet entries, ensuring clarity in cost analysis and budget management. |
| `DebtorName` | `string` | The 'DebtorName' column captures the name of the debtor associated with the timesheet entry, providing essential context for financial accountability and project cost analysis within the employee timesheet records. |
| `EmployeeID` | `string` | The 'EmployeeID' column (string) uniquely identifies each employee within the 'vw_Timesheet' table, linking timesheet submissions to specific individuals for accurate tracking of labor allocation and project costs. |
| `EmployeeIDName` | `string` | The 'EmployeeIDName' column stores the unique identifier and name of the employee associated with each timesheet entry, enabling clear attribution of labor hours to specific personnel for accurate project cost analysis and resource management. |
| `EmployeeKey` | `int64` | The 'EmployeeKey' column (int64) uniquely identifies each employee within the 'vw_Timesheet' table, linking timesheet submissions to specific personnel for accurate labor allocation and project cost analysis. |
| `EmployeeName` | `string` | The 'EmployeeName' column stores the full names of employees who submitted timesheets, providing a clear identification of labor contributions for accurate analysis of project costs and resource allocation. |
| `Employer` | `string` | The 'Employer' column identifies the organization or company that employs the individual submitting the timesheet, providing context for labor allocation and project cost analysis within the 'vw_Timesheet' table. |
| `EmployerCode` | `int64` | The 'EmployerCode' column (int64) uniquely identifies the employer associated with each timesheet entry, enabling accurate tracking of labor costs and resource allocation across different projects within the organization. |
| `ExtraDetails` | `string` | The 'ExtraDetails' column contains additional contextual information or notes related to the timesheet submission, providing further insights into the employee's work activities or project specifics that may not be captured in other fields. |
| `FinancialYear` | `int64` | The 'FinancialYear' column (int64) in the 'vw_Timesheet' table indicates the fiscal year during which the timesheet entries were submitted, enabling businesses to analyze labor costs and project allocations within specific financial periods. |
| `Hours` | `double` | The 'Hours' column (Data Type: double) in the 'vw_Timesheet' table represents the total number of hours worked by an employee on a specific project during a given timesheet submission, enabling accurate analysis of labor allocation and project costs. |
| `HoursperWeek` | `int64` | 'HoursperWeek' (int64) represents the total number of hours an employee has worked in a given week, providing critical insights into labor allocation and productivity for effective project and resource management. |
| `HoursType` | `string` | The 'HoursType' column categorizes the nature of hours recorded in the timesheet, such as regular, overtime, or leave, providing critical insights into employee labor distribution and cost analysis. |
| `HoursTypeCode` | `string` | The 'HoursTypeCode' column categorizes the type of hours recorded in the timesheet, such as regular, overtime, or leave, providing essential context for analyzing labor costs and resource allocation within the organization. |
| `Index` | `int64` | The 'Index' column (int64) uniquely identifies each entry in the 'vw_Timesheet' table, ensuring efficient data retrieval and organization of employee timesheet submissions for analysis and reporting. |
| `IngestDatetime` | `dateTime` | The 'IngestDatetime' column records the precise date and time when each timesheet entry was submitted, providing a timestamp for tracking and auditing purposes within the employee timesheet records. |
| `InternalPMID` | `string` | The 'InternalPMID' column stores a unique identifier for internal project management instances, linking employee timesheet entries to specific projects for enhanced tracking and analysis of labor allocation and project costs. |
| `InternalPMKey` | `int64` | The 'InternalPMKey' column (int64) uniquely identifies the project manager associated with each timesheet entry, enabling efficient tracking and analysis of managerial oversight in labor allocation and project costs. |
| `IPM_ManagerName` | `string` | The 'IPM_ManagerName' column stores the name of the Integrated Project Manager responsible for overseeing the timesheet submissions, providing crucial managerial context for analyzing labor allocation and project oversight. |
| `IPMIDName` | `string` | The 'IPMIDName' column stores the name of the IPM (Integrated Project Management) identifier associated with each timesheet entry, providing a reference for linking employee submissions to specific projects and enhancing project cost analysis and resource allocation. |
| `JobTitle` | `string` | The 'JobTitle' column captures the specific title of the employee's position, providing context for their role in relation to timesheet submissions and aiding in the analysis of labor allocation and project costs within the 'vw_Timesheet' table. |
| `Join_Key` | `string` | The 'Join_Key' column serves as a unique identifier that links employee timesheet records to related datasets, ensuring accurate data integration and analysis across various business dimensions. |
| `ManagerID` | `string` | The 'ManagerID' column (string) identifies the unique identifier of the manager overseeing the employee's timesheet submissions, enabling effective tracking of managerial oversight and accountability in labor allocation and project management. |
| `ManagerIDName` | `string` | The 'ManagerIDName' column contains the names of managers associated with each timesheet entry, providing a clear link between employee submissions and their respective managerial oversight for enhanced accountability and resource management. |
| `ManagerKey` | `int64` | The 'ManagerKey' column (int64) identifies the unique key associated with the manager overseeing the employee's timesheet submissions, enabling effective tracking of managerial oversight and resource allocation within the 'vw_Timesheet' table. |
| `ManagerName` | `string` | The 'ManagerName' column captures the name of the employee's direct supervisor, providing essential managerial context for each timesheet entry to enhance oversight and facilitate effective resource management. |
| `OHWWorkBookings` | `string` | The 'OHWWorkBookings' column contains string values that represent the specific work bookings associated with employee timesheet submissions, providing insights into project allocation and labor distribution for effective resource management and cost analysis. |
| `Project` | `string` | The 'Project' column identifies the specific project associated with each timesheet entry, enabling analysis of labor allocation and project-related costs within the organization's financial framework. |
| `ProjectCode` | `string` | The 'ProjectCode' column stores a unique identifier for each project associated with employee timesheet entries, enabling efficient tracking and analysis of labor allocation and project-related costs. |
| `ProjectProfile` | `string` | The 'ProjectProfile' column contains a string representation of the specific project associated with each timesheet entry, enabling detailed tracking of employee labor allocation and project-related costs within the 'vw_Timesheet' table. |
| `ProjectProfileCode` | `int64` | The 'ProjectProfileCode' column (int64) uniquely identifies the specific project associated with each timesheet entry, enabling detailed analysis of labor allocation and project costs within the 'vw_Timesheet' table. |
| `ProjectType` | `string` | The 'ProjectType' column categorizes the nature of the projects associated with each timesheet entry, providing insights into labor allocation and project-specific resource management. |
| `Rejected` | `boolean` | Indicates whether the timesheet submission has been rejected, providing a clear status for review and approval processes within employee time tracking. |
| `ReportReady` | `boolean` | Indicates whether the timesheet report is ready for review or processing, facilitating timely analysis and decision-making regarding labor allocation and project costs. |
| `SalesAmount` | `double` | The 'SalesAmount' column (Data Type: double) in the 'vw_Timesheet' table represents the total revenue generated from employee labor associated with specific projects, enabling businesses to assess financial performance and project profitability. |
| `SalesPrice` | `double` | The 'SalesPrice' column (double) in the 'vw_Timesheet' table represents the monetary value assigned to the services rendered by employees, enabling analysis of labor costs in relation to project budgets and financial performance. |
| `Status` | `string` | The 'Status' column indicates the current state of each timesheet submission, reflecting whether it is pending, approved, or rejected, thereby aiding in the tracking and management of employee time reporting processes. |
| `Taxed` | `boolean` | Indicates whether the reported hours on the timesheet are subject to taxation, providing clarity on financial implications for payroll processing and compliance. |
| `Tier` | `string` | The 'Tier' column categorizes the level of service or priority associated with the timesheet submission, aiding in the analysis of labor allocation and project prioritization. |
| `TimesheetCode` | `string` | The 'TimesheetCode' column uniquely identifies each timesheet submission, allowing for efficient tracking and management of employee work hours and associated project allocations within the 'vw_Timesheet' table. |
| `TimesheetDate` | `dateTime` | The 'TimesheetDate' column records the specific date and time when an employee's timesheet submission is made, providing a crucial timestamp for tracking labor allocation and submission timelines within the overall timesheet management process. |
| `TimesheetDescription` | `string` | The 'TimesheetDescription' column provides a detailed narrative of the work performed by employees during the timesheet period, enhancing the understanding of labor allocation and project contributions for effective resource management and cost analysis. |
| `Unit` | `string` | The 'Unit' column specifies the measurement unit for the recorded hours in the timesheet, providing clarity on the scale of labor input associated with each submission. |
| `UnitCode` | `string` | The 'UnitCode' column represents a unique identifier for the organizational unit associated with each timesheet entry, enabling precise tracking of labor allocation and project costs within the 'vw_Timesheet' table. |
| `WorkEmail` | `string` | The 'WorkEmail' column stores the professional email addresses of employees, enabling effective communication regarding timesheet submissions and project-related inquiries within the 'vw_Timesheet' table. |
| `YearWeek` | `string` | The 'YearWeek' column (string) represents the specific financial year and week number of the timesheet submission, enabling precise tracking and analysis of employee labor allocation over time. |

##### Calculated Columns

**`Approved_With_V_Z`**

- **Description:** The 'Approved_With_V_Z' column indicates the approval status of timesheet submissions, specifically denoting whether the submission has been approved with a specific validation or review process, thereby aiding in tracking compliance and accountability in labor reporting.
- **DAX Expression:**
```dax
IF(
			    [Approved] = TRUE() || 
			    vw_Timesheet[TimesheetCode] IN {"V", "Z"}, 
			    1, 
			    0
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'Approved_With_V_Z'. Here's a simple breakdown of what it does:

1. **Condition Check**: The expression checks two conditions:
   - It first checks if the value in the column `[Approved]` is `TRUE`. This means it is verifying if an item (like a timesheet or request) has been approved.
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". This means it is looking for specific codes that might represent certain types of entries.

2. **Output Values**: 
   - If either of the conditions is met (meaning the item is approved or has a TimesheetCode of "V" or "Z"), the expression returns a value of `1`. This indicates a positive outcome, meaning the item is considered approved in this context.
   - If neither condition is met, it returns a value of `0`, indicating that the item is not approved.

**In summary**, this DAX expression effectively flags items as approved (1) if they are either explicitly marked as approved or have specific codes ("V" or "Z"). If neither condition is true, it flags them as not approved (0). This can help in filtering or analyzing data based on approval status.

**`BillableDep`**

- **Description:** The 'BillableDep' column indicates the department responsible for billable hours recorded in the timesheet, providing insights into departmental contributions to project costs and resource allocation.
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

3. **Check for Non-Zero Values**: If the related 'BillableDep' value is not blank, the expression then checks if this value is not equal to zero. If it is not zero, it again returns **1**, indicating that there is a valid billable dependency.

4. **Return Zero for Zero Values**: If the related 'BillableDep' value is zero, the expression returns **0**, indicating that there is no billable dependency.

In summary, this DAX expression effectively categorizes each entry based on whether it has a billable dependency or not. It assigns a value of **1** if there is a billable dependency (either because it is present and non-zero or because there is no related entry), and a value of **0** if the dependency is explicitly zero. This helps in identifying which entries are considered billable based on their dependencies.

**`BillablePrj`**

- **Description:** The 'BillablePrj' column identifies the projects for which employee hours are chargeable, enabling accurate tracking of billable time and facilitating financial analysis related to project costs.
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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'BillablePrj' in a data model, specifically within a table named `vw_Timesheet`. Here's a breakdown of what it does in simple business terms:

1. **Purpose**: The expression determines whether a particular entry in the timesheet is considered "billable" or not. A billable entry is one that can be charged to a client or project.

2. **Conditions Checked**:
   - **Sales Price**: It first checks if the `SalesPrice` for the entry is greater than 0. If there is a positive sales price, it indicates that the entry can generate revenue.
   - **Project Profile Code**: Next, it checks if the `ProjectProfileCode` is equal to 81. This specific code likely represents a type of project that is always billable.
   - **Project Name**: Finally, it looks for the phrase "Customer Success Services" within the `Project` field. If this phrase is found, it suggests that the project is related to customer success, which is also considered billable.

3. **Output**: 
   - If any of the above conditions are true (meaning the entry is billable), the expression returns a value of `1`.
   - If none of the conditions are met (meaning the entry is not billable), it returns a value of `0`.

In summary, this DAX expression effectively flags timesheet entries as billable (1) or non-billable (0) based on specific criteria related to sales price, project profile, and project name. This helps in identifying which work can be charged to clients, aiding in revenue tracking and project management.

**`cc_Employer`**

- **Description:** The 'cc_Employer' column stores the name of the employer associated with each timesheet submission, providing critical context for analyzing labor costs and project allocations within the 'vw_Timesheet' table.

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

3. **Combining Information**: The expression then combines these two pieces of information into one string. It formats it so that the employee's name is followed by a dash and then the number of hours they work each week. For example, if an employee named "John Doe" works 40 hours a week, the result would be "John Doe - 40".

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
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'GroupCat' in a data model, likely related to project management or timesheet tracking. Here's a breakdown of what it does in simple business terms:

1. **Lookup for Group**: The expression first checks if there is a corresponding 'Group' value for a project in the 'lkp_Project' table. It does this by using the `LOOKUPVALUE` function, which searches for the 'Group' associated with the 'PROJECT' in the 'vw_Timesheet' table.

2. **Check for Blank Values**: The `NOT(ISBLANK(...))` part checks if the result of the lookup is not blank. If there is a valid 'Group' found for the project, it means that the project is categorized under a specific group.

3. **Return Group Value**: If a valid 'Group' is found, the expression returns that 'Group' value directly.

4. **Handle Billable Projects**: If no 'Group' is found (meaning the project is not categorized), the expression then checks if the project is billable by looking at the 'BillablePrj' column. If 'BillablePrj' equals 1, it indicates that the project is billable.

5. **Categorization**: If the project is billable, it assigns the label "Billable" to the 'GroupCat' column. If it is not billable, it assigns the label "Other Unbillable".

In summary, this DAX expression categorizes projects into groups based on their lookup values. If no group is found, it further classifies the project as either "Billable" or "Other Unbillable" based on its billable status. This helps in organizing and analyzing projects effectively in terms of their financial impact and categorization.

**`IsContractActive`**

- **Description:** The 'IsContractActive' column indicates whether the employee's contract is currently active, providing crucial context for timesheet submissions and ensuring accurate labor cost analysis and project resource allocation.
- **DAX Expression:**
```dax
IF(
			    ISBLANK('vw_Timesheet'[ContractEndDate]) || 'vw_Timesheet'[ContractEndDate] > TODAY(),
			    "Active",
			    "Not Active"
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column called 'IsContractActive' in a data model, specifically for a table named 'vw_Timesheet'. Here's what it does in simple business terms:

1. **Checks Contract End Date**: The expression first looks at the 'ContractEndDate' field in the 'vw_Timesheet' table. 

2. **Two Conditions**: It evaluates two conditions:
   - **Is Blank**: It checks if the 'ContractEndDate' is blank (meaning there is no end date specified).
   - **Is Future Date**: It checks if the 'ContractEndDate' is greater than today's date (meaning the contract is still valid and has not yet ended).

3. **Determines Status**: 
   - If either of these conditions is true (the contract has no end date or the end date is in the future), it labels the contract as "Active".
   - If neither condition is true (the contract has an end date that is in the past), it labels the contract as "Not Active".

In summary, this DAX expression helps identify whether a contract is currently active or not based on its end date, providing valuable information for managing contracts effectively.

**`MAIN_UNIT`**

- **Description:** The 'MAIN_UNIT' column identifies the primary organizational unit or department associated with the employee's timesheet submission, aiding in the analysis of labor allocation and project costs within the business.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Unit[Unit])), 
			    "Unknown", 
			    RELATED(lkp_Unit[Unit])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'MAIN_UNIT'. Here's a breakdown of what it does in simple business terms:

1. **Check for Related Data**: The expression starts by checking if there is a related value in the 'Unit' column from a table called 'lkp_Unit'. This is done using the `RELATED` function, which pulls in data from a related table based on existing relationships in the data model.

2. **Handle Missing Values**: The `ISBLANK` function checks if the value retrieved from the 'lkp_Unit[Unit]' column is blank (meaning there is no corresponding unit found). 

3. **Return Values Based on the Check**:
   - If the value is blank (i.e., there is no related unit), the expression returns the string "Unknown". This indicates that the unit information is missing or not available.
   - If there is a related unit (i.e., the value is not blank), it returns the actual unit name from the 'lkp_Unit[Unit]' column.

**In summary**, this DAX expression effectively populates the 'MAIN_UNIT' column with either the name of the unit from the related table or "Unknown" if no unit is associated. This helps ensure that users can easily identify when unit information is missing, improving data clarity and usability.

**`MonthNumber`**

- **Description:** The 'MonthNumber' column (string) in the 'vw_Timesheet' table represents the numerical designation of the month in which timesheet entries were submitted, aiding in the organization and analysis of employee labor data across different time periods.
- **DAX Expression:**
```dax
MONTH([TimesheetDate])
```
- **DAX Explanation (Generated):** The DAX expression `MONTH([TimesheetDate])` is used to extract the month number from a date value found in the column named `TimesheetDate`. 

In simple terms, this means that for each entry in the `TimesheetDate` column, the expression will return a number that represents the month of that date. For example:

- If the date is January 15, 2023, the expression will return `1`.
- If the date is March 10, 2023, it will return `3`.
- If the date is December 5, 2023, it will return `12`.

This calculated column, named 'MonthNumber', helps in organizing or analyzing data by month, making it easier to summarize or filter timesheet entries based on the month they occurred.

**`NR_EMP_COLUMN`**

- **Description:** The 'NR_EMP_COLUMN' stores the unique identification number of the employee associated with each timesheet entry, enabling accurate tracking of labor contributions and facilitating detailed analysis of employee performance and project costs.
- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees listed in the `EmployeeName` column of the `vw_Timesheet` table.

In simple business terms, this means that the expression counts how many different employees have submitted timesheets, without counting any employee more than once. For example, if three timesheets were submitted by the same employee, that employee would only be counted once in the final result. 

This calculation is useful for understanding the total number of distinct employees involved in a project or task, helping businesses track participation and resource allocation effectively.

**`Own-Sub-ExtT`**

- **Description:** The 'Own-Sub-ExtT' column captures the type of timesheet submission, indicating whether it is an internal, subcontracted, or external entry, thereby aiding in the analysis of labor allocation and project cost management.
- **DAX Expression:**
```dax
RELATED(lkp_Unit[OWN-Sub-ExtT])
```
- **DAX Explanation (Generated):** The DAX expression `RELATED(lkp_Unit[OWN-Sub-ExtT])` is used to retrieve a value from a related table in a data model. Here's a breakdown of what it does in simple business terms:

1. **Context**: This expression is typically used in a calculated column within a table that has a relationship with another table called `lkp_Unit`.

2. **Purpose**: The expression fetches the value from the `OWN-Sub-ExtT` column in the `lkp_Unit` table. 

3. **How it Works**: 
   - When you use `RELATED`, it looks for a relationship between the current table (where the calculated column is being created) and the `lkp_Unit` table.
   - It then finds the corresponding row in the `lkp_Unit` table based on that relationship and retrieves the value from the `OWN-Sub-ExtT` column.

4. **Outcome**: The result is that for each row in the current table, you get the specific value from the `OWN-Sub-ExtT` column of the related `lkp_Unit` table. This can be useful for enriching your data with additional information that is linked through a common key.

In summary, this DAX expression helps to pull in relevant data from another table, allowing for more comprehensive analysis and reporting by connecting related pieces of information.

**`QualifyPrj`**

- **Description:** The 'QualifyPrj' column contains string values that indicate the qualification status of projects associated with employee timesheet submissions, aiding in the assessment of project eligibility and compliance for financial analysis and resource allocation.
- **DAX Expression:**
```dax
IF(
			    ISBLANK(RELATED(lkp_Project[Qualify])), 
			    1, 
			    RELATED(lkp_Project[Qualify])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a calculated column named 'QualifyPrj'. Here's a breakdown of what it does in simple business terms:

1. **Check for Blank Value**: The expression first checks if the 'Qualify' field from a related table called 'lkp_Project' is blank (i.e., has no value). This is done using the `ISBLANK` function.

2. **Return Values Based on the Check**:
   - If the 'Qualify' value is blank, the expression returns a value of **1**. This could be interpreted as a default or placeholder value indicating that there is no qualification information available for that project.
   - If the 'Qualify' value is not blank, it returns the actual value from the 'Qualify' field in the 'lkp_Project' table.

**In summary**, this DAX expression effectively ensures that for each row in the current table, if there is no qualification information available from the related 'lkp_Project' table, it assigns a default value of 1. Otherwise, it uses the existing qualification value. This helps maintain consistency in the data by ensuring that every project has a qualification value, either the actual one or a default.

**`ReportingEntity`**

- **Description:** The 'ReportingEntity' column identifies the specific organizational unit or department responsible for the timesheet submission, enabling detailed analysis of labor allocation and project costs within the business.

**`RReady_With_V_Z`**

- **Description:** The 'RReady_With_V_Z' column indicates the readiness status of timesheet submissions, providing a string value that reflects whether the submission meets the necessary criteria for review and processing within the 'vw_Timesheet' table.
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
   - It first checks if the value of the column `[ReportReady]` is `TRUE`. This likely indicates that a report is ready for some action or processing.
   - It also checks if the value in the column `vw_Timesheet[TimesheetCode]` is either "V" or "Z". These codes probably represent specific types of timesheets that are relevant for the analysis.

2. **Output Values**: 
   - If either of the conditions is met (meaning the report is ready or the timesheet code is "V" or "Z"), the expression returns a value of `1`. This could signify that the item meets the criteria for being "ready" or "valid" in some context.
   - If neither condition is met, it returns a value of `0`, indicating that the item does not meet the criteria.

3. **Purpose**: The overall purpose of this calculated column is to flag records as either "ready" (1) or "not ready" (0) based on the specified conditions. This can help in filtering or analyzing data later on, allowing users to easily identify which records are ready for further processing or review based on the report status or specific timesheet codes.

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

1. **Condition Check**: The expression first checks if the value of another measure called `[ContractStatusMeasure]` is equal to "Active". This measure likely indicates whether a contract is currently active or not.

2. **Return Value if Active**: If the contract is indeed "Active", the expression returns the value of another measure called `[HoursDifference]`. This measure probably calculates the difference in hours related to the contract, such as the time remaining until expiration or the total hours worked under the contract.

3. **Return Value if Not Active**: If the contract is not "Active" (meaning it could be "Inactive" or any other status), the expression returns the text "Not Active".

In summary, this DAX expression helps to determine the status of a contract and provides relevant information based on that status. If the contract is active, it shows the hours difference; if not, it simply indicates that the contract is not active. This can be useful for reporting and analysis, allowing users to quickly understand the status of contracts and their associated metrics.

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

1. **Check for Contract End Date**: The expression first looks at the maximum value of the 'ContractEndDate' from the 'vw_Timesheet' table. This means it is checking the latest contract end date available in the data.

2. **Evaluate Conditions**:
   - **ISBLANK**: It checks if this maximum end date is blank (meaning there is no end date recorded).
   - **Comparison with Today**: It also checks if this maximum end date is greater than today’s date (meaning the contract is still ongoing).

3. **Determine Status**:
   - If either of the above conditions is true (the end date is blank or it is in the future), the measure returns "Active". This indicates that the contract is currently active and has not yet ended.
   - If neither condition is true (the end date is not blank and it is in the past), it returns "Not Active". This means the contract has ended.

In summary, this measure effectively categorizes contracts as either "Active" or "Not Active" based on their end dates, helping businesses understand which contracts are currently in effect.

**`CurrentWeekCard`**

- **DAX Expression:**
```dax
WEEKNUM(TODAY())
```
- **DAX Explanation (Generated):** The DAX expression `WEEKNUM(TODAY())` is used to calculate the current week number of the year based on today's date.

Here's a breakdown of what it does:

1. **TODAY()**: This function retrieves the current date. For example, if today is October 10, 2023, this function will return that date.

2. **WEEKNUM()**: This function takes a date as input and returns the week number of that date within the year. The week number is typically calculated starting from the first week of the year.

So, when you combine these two functions, `WEEKNUM(TODAY())` will give you the week number for the current date. For instance, if today is in the 41st week of the year, this expression will return the number 41.

In simple business terms, this measure helps you understand which week of the year it currently is, which can be useful for reporting, planning, or analyzing data on a weekly basis.

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

2. **DISTINCTCOUNT Function**: This part of the expression counts the number of unique entries in the `EmployeeName` column from the `vw_Timesheet` table. Essentially, it ensures that each employee is only counted once, even if they have multiple timesheet entries.

3. **Filter Condition**: The expression includes a filter that specifies `vw_Timesheet[Approved] = FALSE()`. This means that the calculation will only consider timesheet entries where the `Approved` status is marked as false (i.e., the timesheets that have not been approved).

In summary, this DAX measure calculates the total number of unique employees who have submitted timesheets that are still pending approval. This information can be useful for tracking outstanding approvals and managing workforce resources effectively.

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

2. **DISTINCTCOUNT Function**: This function counts the number of unique values in a specified column. Here, it counts the unique names of employees from the `vw_Timesheet` table.

3. **Filter Condition**: The expression includes a filter condition that specifies only to consider those records where the `ReportReady` column is set to FALSE. This means it will only count employees whose timesheets are not ready for reporting.

In summary, this measure calculates the total number of distinct employees who have timesheets that are still pending and not ready for reporting. This can help a business understand how many employees still need to submit or finalize their timesheets before they can be processed or reported.

**`DISTINCT_COUNT_EMP`**

- **DAX Expression:**
```dax
DISTINCTCOUNT(vw_Timesheet[EmployeeName])
```
- **DAX Explanation (Generated):** The DAX expression `DISTINCTCOUNT(vw_Timesheet[EmployeeName])` is used to calculate the number of unique employees who have recorded timesheet entries in a dataset.

Here's a breakdown of what it achieves:

- **DISTINCTCOUNT**: This function counts the number of distinct (unique) values in a specified column.
- **vw_Timesheet[EmployeeName]**: This refers to the column in the `vw_Timesheet` table that contains the names of employees.

In simple terms, this measure will give you the total number of different employees who have submitted timesheets, ignoring any duplicate entries. For example, if three employees submitted timesheets multiple times, the measure would return a count of just three, reflecting the unique individuals rather than the total number of timesheets submitted. This is useful for understanding workforce participation or engagement in timesheet reporting.

**`HoursDifference`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_%_Budget_Used`**

- **DAX Expression:**
```dax
DIVIDE(SUM(vw_Timesheet[SalesAmount]), SUM('tbl_Project'[SalesPriceBaseCurrency]), 0)
```
- **DAX Explanation (Generated):** This DAX expression calculates the percentage of the budget that has been used based on sales data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part adds up all the sales amounts recorded in the 'vw_Timesheet' table. Essentially, it totals the revenue generated from sales.

2. **SUM('tbl_Project'[SalesPriceBaseCurrency])**: This part sums up the total budget or sales price allocated for the projects from the 'tbl_Project' table. It represents the total amount that was planned or budgeted for sales.

3. **DIVIDE(..., ..., 0)**: The DIVIDE function takes the total sales amount (from the first part) and divides it by the total budget (from the second part). If the budget is zero (to avoid division by zero errors), it returns 0 instead of an error.

In simple terms, this measure calculates how much of the budget has been used by comparing actual sales to the planned budget. The result is a ratio that shows the percentage of the budget that has been utilized based on actual sales performance.

**`Msr_ActiveContacts`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`MSR_AVG_HOURLY_RATE`**

- **DAX Expression:**
```dax
SUM(vw_Timesheet[SalesAmount])/SUM(vw_Timesheet[Hours])
```
- **DAX Explanation (Generated):** This DAX expression calculates the average hourly rate for sales based on timesheet data. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[SalesAmount])**: This part of the expression adds up all the sales amounts recorded in the `SalesAmount` column of the `vw_Timesheet` table. Essentially, it gives you the total sales generated over a specific period.

2. **SUM(vw_Timesheet[Hours])**: This part sums up all the hours worked recorded in the `Hours` column of the same `vw_Timesheet` table. This represents the total number of hours worked during that same period.

3. **Division**: The expression then divides the total sales amount by the total hours worked. This calculation provides the average hourly rate, which tells you how much revenue is generated for each hour worked.

In simple terms, this measure helps a business understand how effectively it is generating sales relative to the time spent working. It’s a useful metric for assessing productivity and profitability on an hourly basis.

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

2. **Calculate Previous Budget**: The next step is to find the most recent budget value that is available before the current week. This is achieved through the `CALCULATE` function, which modifies the context of the calculation:
   - It uses the measure `[Msr_SalesPriceBaseCurrency]`, which presumably represents the budget or sales price in a base currency.
   - The `FILTER` function is applied to the entire date dimension (using `ALLSELECTED` to respect any filters that might be applied) to only consider weeks that are less than the current week. This means it looks back at previous weeks to find the last available budget.

3. **Return the Appropriate Value**: Finally, the code uses an `IF` statement to decide what value to return:
   - If the current budget measure `[Msr_SalesPriceBaseCurrency]` is not blank (meaning there is a budget value available for the current week), it returns that value.
   - If the current budget is blank (indicating no budget is set for the current week), it falls back to the previously calculated budget value from the last available week.

In summary, this DAX measure effectively ensures that a budget value is always returned for the current week, either by using the current week's budget if it exists or by using the most recent prior week's budget if it does not. This approach helps maintain continuity in budget reporting and analysis, ensuring that users always have a relevant budget figure to work with.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the cumulative cost up to a selected month in a reporting visual. Here's a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The code starts by determining which month is currently selected in the visual. It uses the `MAX` function to find the highest month number from the 'DimDate' table, which represents the month currently being viewed or filtered.

2. **Calculate Cumulative Cost**: The main goal of this measure is to calculate the total cost incurred from the beginning of the year up to and including the selected month. 

3. **Summing Costs**: The `CALCULATE` function is used to modify the context in which the data is evaluated. It sums up the `CostAmount` from the `vw_Timesheet` table.

4. **Filtering the Data**: The `FILTER` function is applied to ensure that only the months that are less than or equal to the selected month are included in the calculation. The `ALLSELECTED` function is used to consider any filters that might be applied to the 'DimDate' table, ensuring that the calculation respects the user's selections in the report.

In summary, this DAX measure calculates the total cost accumulated from the start of the year up to the month that the user has selected in the report. This allows users to see how costs have built up over time, providing valuable insights into spending trends.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the cumulative sales amount up to a selected month in a report or visual. Here’s a breakdown of what it does in simple business terms:

1. **Identify the Selected Month**: The code starts by determining which month is currently selected in the visual. It does this by finding the maximum value of the 'MonthNumberOfYear' from the 'DimDate' table. This means if you are looking at data for March, the `SelectedMonth` variable will hold the value for March (which is 3).

2. **Calculate Cumulative Sales**: The main purpose of the code is to calculate the total sales amount from the 'vw_Timesheet' table, but only for the months that are less than or equal to the selected month. 

3. **Use of CALCULATE and FILTER**: 
   - The `CALCULATE` function changes the context in which the data is evaluated. It allows us to sum the sales amount while applying specific filters.
   - The `FILTER` function is used to create a new context that includes all months up to and including the selected month. The `ALLSELECTED('DimDate')` part ensures that any filters applied to the 'DimDate' table are respected, but it still allows us to look at all months up to the selected one.

4. **Final Result**: The result of this measure is the total sales amount for all months from the beginning of the year up to and including the month that is currently selected in the visual. This helps users understand how sales have accumulated over time, providing insights into trends and performance up to a specific point in the year.

In summary, this DAX measure calculates the cumulative sales up to the month that the user has selected, allowing for better analysis of sales performance over time.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the percentage of hours worked by a specific group, project, or employee relative to the total hours worked across all selected categories in a report or dashboard.

Here's a breakdown of what it does:

1. **TotalValue Calculation**: 
   - The first part of the code defines a variable called `TotalValue`. This variable calculates the total number of hours from the `vw_Timesheet` table. 
   - The `CALCULATE` function is used to sum the `Hours` column, but it does so while ignoring any filters that might be applied to the `GroupCat`, `Project`, or `EmployeeName` columns. This means it considers all the data in these categories, regardless of any selections made in the report.

2. **Percentage Calculation**:
   - The `RETURN` statement then calculates the percentage of hours for the current context (which could be a specific group, project, or employee) by dividing the sum of hours for that context by the `TotalValue` calculated earlier.
   - The `DIVIDE` function is used for this division, which is a safer way to perform division in DAX because it can handle cases where the denominator (in this case, `TotalValue`) might be zero. If `TotalValue` is zero, it will return 0 instead of causing an error.

In summary, this measure calculates what percentage of the total hours worked (across all selected filters) is represented by the hours worked in the current context (like a specific project or employee). This helps in understanding how much of the total effort is contributed by a particular segment of the data.

**`MSR_PROJECT_%_REALIZATION`**

- **DAX Expression:**
```dax
IF(sum(tbl_Project[SalesPriceBaseCurrency]) <> 0, sum(vw_Timesheet[SalesAmount])/sum(tbl_Project[SalesPriceBaseCurrency]))
```
- **DAX Explanation (Generated):** This DAX expression is designed to calculate a measure called 'MSR_PROJECT_%_REALIZATION', which essentially assesses the percentage of sales realization for a project. Here's a breakdown of what it does in simple business terms:

1. **Check for Zero Sales Price**: The expression starts with an `IF` statement that checks if the total sales price (in the base currency) from the `tbl_Project` table is not equal to zero. This is important because if the sales price is zero, it would not make sense to calculate a percentage, as you cannot divide by zero.

2. **Calculate Sales Amount**: If the total sales price is not zero, the expression proceeds to calculate the percentage of realization. It does this by taking the total sales amount from the `vw_Timesheet` table.

3. **Calculate the Percentage**: The sales amount is then divided by the total sales price. This division gives you the percentage of how much of the expected sales (the sales price) has actually been realized (the sales amount).

In summary, this DAX measure calculates the percentage of sales that have been realized for a project, but only if there is a valid sales price. If there is no sales price, it avoids a division error by not performing the calculation. This measure helps businesses understand how effectively they are converting their projected sales into actual sales.

**`MSR_ProjectMargin`**

- **DAX Expression:**
```dax
sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])
```
- **DAX Explanation (Generated):** The DAX expression you provided calculates the profit margin for a project by subtracting total costs from total sales. Here’s a breakdown of what it does:

1. **Total Sales Calculation**: The expression starts with `sum(vw_Timesheet[SalesAmount])`, which adds up all the sales amounts recorded in the `vw_Timesheet` table. This represents the total revenue generated from the project.

2. **Total Costs Calculation**: Next, it calculates the total costs with `sum(vw_Timesheet[CostAmount])`. This sums up all the costs associated with the project, which could include expenses like labor, materials, and overhead.

3. **Profit Margin Calculation**: Finally, the expression subtracts the total costs from the total sales: `sum(vw_Timesheet[SalesAmount]) - sum(vw_Timesheet[CostAmount])`. The result of this calculation gives you the project margin, which indicates how much profit the project is generating after covering its costs.

In simple terms, this DAX measure helps you understand the financial performance of a project by showing how much money is left over after all expenses have been paid, which is crucial for assessing profitability.

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
- **DAX Explanation (Generated):** This DAX expression calculates the total sales price in the base currency for different projects and clients from a timesheet view. Here’s a breakdown of what it does in simple business terms:

1. **SUMMARIZE Function**: The expression starts by creating a summary table from the `vw_Timesheet` data. This summary groups the data by two key dimensions: `ProjectProfile` and `Client`. Essentially, it organizes the data so that we can look at sales prices for each unique combination of project and client.

2. **SELECTEDVALUE Function**: Within this summary, it retrieves the sales price in the base currency from another table called `tbl_Project`. The `SELECTEDVALUE` function is used here to get the specific sales price for the current context (i.e., the project being evaluated). If there’s only one sales price for that project, it returns that value; if there are multiple, it returns blank.

3. **MeasureValue**: The result of the `SELECTEDVALUE` function is named "MeasureValue" in the summary table. This means that for each unique project and client combination, we now have a corresponding sales price in the base currency.

4. **SUMX Function**: Finally, the `SUMX` function takes this summarized table and sums up all the "MeasureValue" entries. This means it adds up all the sales prices in the base currency for each project and client combination.

In summary, this DAX expression calculates the total sales price in the base currency for all projects and clients listed in the timesheet, providing a clear view of sales performance across different projects and clients.

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

4. **vw_Timesheet[ContractStatusToday] = "Valid"**: This is a filter condition that specifies we only want to consider employees whose contract status is "Valid" on the day in question.

Putting it all together, this DAX expression calculates the total number of unique employees who currently have a valid contract, ignoring any other filters that might be applied to the data. This is useful for understanding the active workforce under valid contracts at a specific point in time.

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

In simple terms, this measure totals all the hours worked by employees as recorded in the timesheet. It provides a single number that represents the cumulative hours, which can be useful for reporting, payroll, or analyzing workforce productivity.

**`TotalContractedHours`**

- **DAX Expression:**
```dax
SUMX(
			    VALUES(vw_Timesheet[EmployeeName]),
			    MAX(vw_Timesheet[HoursPerWeek])
			)
```
- **DAX Explanation (Generated):** This DAX expression calculates the total contracted hours for all employees based on their maximum hours per week. Here’s a breakdown of what it does in simple business terms:

1. **VALUES(vw_Timesheet[EmployeeName])**: This part of the expression creates a unique list of employee names from the 'vw_Timesheet' table. Essentially, it identifies each employee who has recorded hours.

2. **MAX(vw_Timesheet[HoursPerWeek])**: For each employee in the unique list, this part finds the maximum number of hours that employee is contracted to work in a week. If an employee has different entries for hours in the timesheet, it will take the highest value.

3. **SUMX(...)**: This function then takes the unique list of employees and iterates over it. For each employee, it calculates the maximum hours per week and sums these maximum values together.

In summary, the entire expression calculates the total of the highest contracted hours per week for all employees. This gives a clear picture of the total maximum hours that the organization has contracted for its workforce on a weekly basis.

**`TotalHoursDeltaComplete`**

- **DAX Explanation (Generated):** (No DAX expression found)

**`TotalHoursDeltaCompleteNr`**

- **DAX Expression:**
```dax
[TotalActualHours]-[TotalContractedHours]
```
- **DAX Explanation (Generated):** The DAX expression `[TotalActualHours] - [TotalContractedHours]` calculates the difference between two measures: **Total Actual Hours** and **Total Contracted Hours**.

In simple business terms, here's what it achieves:

- **Total Actual Hours** refers to the total number of hours that have actually been worked or logged by employees or resources on a project or task.
- **Total Contracted Hours** represents the total number of hours that were agreed upon in a contract for the same project or task.

By subtracting the **Total Contracted Hours** from the **Total Actual Hours**, this measure provides insight into how many hours have been worked beyond what was originally contracted. 

If the result is positive, it indicates that more hours were worked than planned, which could suggest overwork or potential budget issues. If the result is negative, it means that the actual hours worked were less than what was contracted, which could indicate efficiency or underutilization of resources. 

Overall, this measure helps businesses understand their labor performance relative to contractual agreements.

**`TotalHoursTracked`**

- **DAX Expression:**
```dax
IF (
			    ISBLANK(SUM(vw_Timesheet[Hours])),
			    0,
			    SUM(vw_Timesheet[Hours])
			)
```
- **DAX Explanation (Generated):** This DAX expression is used to create a measure called 'TotalHoursTracked' that calculates the total hours tracked from a timesheet. Here’s a breakdown of what it does in simple business terms:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the 'Hours' column of the 'vw_Timesheet' table. Essentially, it calculates the total hours worked.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (i.e., if there are no hours recorded). 

3. **IF(...)**: The IF function evaluates the condition provided by ISBLANK. If the total hours calculated is blank (meaning there are no hours to sum), it returns 0. If there are hours recorded, it returns the total hours calculated.

In summary, this measure ensures that if there are no hours tracked, it will return 0 instead of a blank value. This is useful for reporting and analysis, as it provides a clear numerical value (0) rather than leaving it empty, making it easier to understand and work with the data.

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

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the hours recorded in the `Hours` column of the `vw_Timesheet` table. Essentially, it calculates the total hours worked.

2. **ISBLANK(...)**: This function checks if the result of the SUM calculation is blank (meaning there are no hours recorded). 

3. **IF(...)**: The IF function evaluates the condition provided by ISBLANK. If the total hours are blank (i.e., there are no hours tracked), it returns the text "Empty". If there are hours recorded, it returns the total number of hours calculated.

In summary, this DAX expression helps to determine whether any hours have been tracked in the timesheet. If no hours are recorded, it indicates that with the word "Empty". If there are hours, it simply shows the total number of hours tracked. This is useful for reporting and understanding whether time tracking data is available or not.

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

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been logged or tracked for a specific employee or group of employees.

2. **Maximum Hours Per Week**: The expression then calculates the maximum number of hours that have been recorded per week for each employee. This is done using the `CALCULATE` function combined with `MAXX` and `DISTINCT`. Essentially, it looks at the unique weekly hours logged in the `vw_Timesheet` table and finds the highest value for each employee.

3. **Context Filtering**: The `ALLEXCEPT` function is used to ensure that the calculation of maximum hours per week is done while keeping the context of the specific employee (identified by `EmployeeID`). This means that the maximum hours are calculated only for the relevant employee, ignoring any other filters that might be applied to the data.

4. **Final Calculation**: Finally, the expression subtracts the maximum hours per week (calculated in the previous step) from the total hours tracked. This gives you the difference between the total hours an employee has logged and the maximum hours they have logged in any single week.

### Summary:
In summary, this DAX measure calculates how many hours an employee has tracked beyond their maximum weekly hours. If the result is positive, it indicates that the employee has logged more hours than their highest weekly total, which could be useful for understanding workload or overtime. If it's negative or zero, it suggests that the employee has not exceeded their maximum weekly hours.

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
- **DAX Explanation (Generated):** The DAX expression you've provided is used to calculate a measure called 'trackedDiff2'. Here's a breakdown of what it does in simple business terms:

1. **Total Hours Tracked**: The measure starts with `[TotalHoursTracked]`, which represents the total number of hours that have been tracked for a specific employee or project.

2. **Calculating Maximum Hours Per Week**: The expression then calculates the maximum number of hours tracked per week from a table called `vw_Timesheet`. It does this by:
   - Using `DISTINCT(vw_Timesheet[HoursPerWeek])` to get a unique list of hours per week.
   - Applying `MAXX` to find the highest value from this list.

3. **Removing Filters**: The `CALCULATE` function modifies the context in which the data is evaluated. Here, it uses `REMOVEFILTERS(vw_Timesheet[HoursPerWeek])` to ignore any filters that might be applied to the 'HoursPerWeek' column. This means it looks at all the data without any restrictions based on the hours tracked per week.

4. **Keeping Employee Context**: The `ALLEXCEPT(vw_Timesheet, vw_Timesheet[EmployeeID])` part ensures that while it removes filters on 'HoursPerWeek', it still keeps the context of the specific employee (identified by `EmployeeID`). This means the calculation is focused on the hours for that particular employee only.

5. **Final Calculation**: Finally, the measure subtracts the maximum hours tracked per week (calculated in the previous steps) from the total hours tracked. 

### Summary:
In summary, 'trackedDiff2' calculates the difference between the total hours tracked for an employee and the maximum hours they have tracked in any single week. This can help identify if an employee is consistently working more hours than their maximum weekly average, which could be useful for workload management or identifying potential burnout.

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
- **DAX Explanation (Generated):** This DAX expression is designed to calculate the total billable hours from a timesheet data source, specifically focusing on certain criteria. Here’s a breakdown of what it does in simple business terms:

1. **Variable Definition**: The expression starts by defining a variable called `FilteredHours`. This variable will hold a filtered version of the `vw_Timesheet` data.

2. **Filtering Criteria**: The `Filter` function is used to create this filtered dataset. It applies two main conditions:
   - **Condition 1**: It checks if the project qualifies as billable (`vw_Timesheet[QualifyPrj] = 1` and `vw_Timesheet[BillablePrj] = 1`). This means that only projects that are both qualified and marked as billable will be included.
   - **Condition 2**: It also includes any projects that contain the phrase "Customer Success Services" in their name (`SEARCH("Customer Success Services", vw_Timesheet[Project], 1, 0) > 0`). This allows for additional projects that may not be marked as billable but are still relevant to customer success.

3. **Calculating Total Hours**: After filtering the data based on the above criteria, the `RETURN` statement uses the `SUMX` function. This function iterates over the `FilteredHours` variable and sums up the `Hours` column from the `vw_Timesheet`. Essentially, it adds up all the hours that meet the filtering conditions.

In summary, this DAX expression calculates the total number of billable hours from a timesheet, focusing on projects that are either explicitly marked as billable or are related to customer success services. This measure helps businesses understand how many hours are being billed for work that qualifies under these specific criteria.

**`UTI_TOTALHOURS`**

- **DAX Expression:**
```dax
CALCULATE(SUM(vw_Timesheet[Hours]), vw_Timesheet[QualifyPrj] = 1)
```
- **DAX Explanation (Generated):** This DAX expression is used to calculate the total number of hours worked on specific projects that qualify under certain criteria. Here's a breakdown of what it does:

1. **SUM(vw_Timesheet[Hours])**: This part of the expression adds up all the values in the "Hours" column from the "vw_Timesheet" table. Essentially, it totals the hours recorded.

2. **vw_Timesheet[QualifyPrj] = 1**: This condition filters the data to include only those entries where the "QualifyPrj" column equals 1. This means it focuses on projects that meet a specific qualification or criteria defined in the dataset.

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
- **DAX Explanation (Generated):** This DAX code snippet is designed to calculate the "Billable Hour Ratio" for active employees within a business context. Here's a breakdown of what it does in simple terms:

1. **Variables Defined**:
   - `_TotalBillableHours`: This variable captures the total number of hours that have been billed to clients. It is derived from another measure called `[UTI_TotalBillableHours]`.
   - `_TotalHours`: This variable captures the total number of hours worked by employees, obtained from the measure `[UTI_TOTALHOURS]`.

2. **Return Logic**:
   - The code checks if there are any active contacts (employees) using the measure `[Msr_ActiveContacts]`. If there are no active contacts, the calculation does not proceed.
   - If there are active contacts, it then checks if the total hours worked (`_TotalHours`) is not blank (meaning there are hours recorded).
   - If `_TotalHours` is available, it calculates the ratio of billable hours to total hours. This is done using the `DIVIDE` function, which safely divides `_TotalBillableHours` by `_TotalHours`. If the result of this division is blank (which can happen if `_TotalHours` is zero), it defaults to 0 instead of returning an error.

3. **Outcome**:
   - The final result of this measure is the Billable Hour Ratio, which indicates the proportion of hours that were billable compared to the total hours worked by active employees. This ratio is crucial for understanding how effectively employee time is being utilized for billable work, which can impact profitability and resource management in the organization.

In summary, this DAX expression helps businesses assess the efficiency of their active employees by calculating how much of their working time is spent on billable tasks.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

- Filter on Unknown Target (Type: N/A, Definition: `N/A`)

_No page details were loaded or parsed._

