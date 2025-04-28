# Power BI Model & Report Documentation

*Generated on: 2025-04-28 17:18:17*

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

### Model Overview

#### Purpose
The purpose of this data model is to provide a structured representation of the data elements and their relationships within a specific domain. This model serves as a blueprint for database design, data integration, and data analysis, ensuring consistency and clarity in how data is organized and accessed.

#### Scope
This model encompasses various entities relevant to the domain, including their attributes, relationships, and constraints. It is designed to support the operational and analytical needs of the organization, facilitating efficient data management and retrieval.

#### Key Components

1. **Entities**: 
   - Entities represent distinct objects or concepts within the domain. Each entity is characterized by its attributes, which define the properties of the entity.
   - Example entities might include:
     - **Customer**: Attributes may include CustomerID, Name, Email, PhoneNumber, etc.
     - **Product**: Attributes may include ProductID, Name, Description, Price, etc.
     - **Order**: Attributes may include OrderID, OrderDate, CustomerID, TotalAmount, etc.

2. **Relationships**: 
   - Relationships define how entities interact with one another. They can be one-to-one, one-to-many, or many-to-many.
   - Example relationships might include:
     - A **Customer** can place multiple **Orders** (one-to-many).
     - An **Order** can contain multiple **Products** (many-to-many).

3. **Attributes**: 
   - Attributes are the data fields that provide details about each entity. They can be of various data types, such as string, integer, date, etc.
   - Attributes may also include constraints such as primary keys, foreign keys, and unique constraints to ensure data integrity.

4. **Normalization**: 
   - The model may follow normalization principles to reduce data redundancy and improve data integrity. This involves organizing the data into tables in such a way that dependencies are properly enforced.

5. **Data Integrity**: 
   - The model incorporates rules and constraints to maintain data accuracy and consistency. This includes defining primary keys, foreign keys, and validation rules for attributes.

6. **Documentation**: 
   - Comprehensive documentation accompanies the model, detailing each entity, its attributes, relationships, and any relevant business rules. This documentation serves as a reference for developers, analysts, and stakeholders.

#### Use Cases
- **Database Design**: The model serves as a foundation for creating a relational database schema.
- **Data Integration**: It aids in integrating data from various sources by providing

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

### Table Name: DimDate

#### Table Description:
The `DimDate` table serves as a comprehensive dimension table within a data warehouse, designed to facilitate time-based analysis and reporting. It provides a structured representation of dates, enabling users to perform time-related queries and aggregations efficiently. This table is essential for any analytical model that requires temporal insights, such as sales trends, seasonal patterns, and historical comparisons.

#### Key Features:
- **Date Range**: The `DimDate` table typically encompasses a wide range of dates, often spanning several years, to ensure that all relevant time periods are covered for analysis.
- **Granularity**: Each record in the table represents a single day, allowing for detailed daily analysis while also supporting higher-level aggregations (e.g., monthly, quarterly, yearly).
- **Attributes**: The table includes various attributes related to each date, such as:
  - `DateKey`: A unique identifier for each date (often in YYYYMMDD format).
  - `FullDate`: The complete date in a standard format (e.g., '2023-10-01').
  - `Day`: The day of the month (1-31).
  - `Month`: The month of the year (1-12).
  - `Year`: The year (e.g., 2023).
  - `Quarter`: The quarter of the year (Q1, Q2, Q3, Q4).
  - `DayOfWeek`: The day of the week (e.g., Monday, Tuesday).
  - `IsWeekend`: A boolean flag indicating whether the date falls on a weekend.
  - `Fiscal Year/Quarter`: Attributes that align the date with the organization's fiscal calendar, if applicable.
  - `Holidays`: Flags or indicators for public holidays or significant events.

#### Use Cases:
- **Time Series Analysis**: Analysts can leverage the `DimDate` table to conduct time series analysis, identifying trends and patterns over time.
- **Reporting**: The table supports various reporting needs, allowing users to filter and group data by different time dimensions.
- **Data Integration**: It can be joined with fact tables (e.g., sales, transactions) to enrich data with temporal context, enhancing the analytical capabilities of the data model.

#### Maintenance:
The `DimDate` table is typically populated during the ETL (Extract, Transform, Load) process and may require periodic updates to extend the date range or adjust for changes in the fiscal calendar or holiday schedules.

In summary, the

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

### Table Name: Hours_nd_Percentage

#### Table Description:
The `Hours_nd_Percentage` table is designed to capture and analyze the relationship between hours worked and the corresponding percentage of completion or achievement of specific tasks, projects, or goals. This table serves as a critical resource for performance tracking, resource allocation, and productivity analysis within an organization.

#### Key Features:
- **Data Structure**: The table consists of multiple columns that store relevant metrics, including but not limited to hours worked, percentage of completion, task identifiers, and timestamps.
- **Purpose**: It aims to provide insights into how effectively time is being utilized in relation to the outcomes achieved, enabling stakeholders to make informed decisions regarding project management and workforce optimization.
- **Use Cases**: This table can be utilized for generating reports, conducting performance reviews, and identifying trends in productivity over time.

#### Columns:
- **Task_ID**: A unique identifier for each task or project being tracked.
- **Employee_ID**: A reference to the employee or team responsible for the task.
- **Hours_Worked**: The total number of hours spent on the task.
- **Percentage_Complete**: The percentage of the task that has been completed at the time of recording.
- **Date_Logged**: The date when the hours and percentage were recorded, allowing for time-based analysis.

#### Relationships:
The `Hours_nd_Percentage` table may be linked to other tables such as `Employees`, `Tasks`, and `Projects` to provide a comprehensive view of performance metrics across different dimensions.

#### Summary:
Overall, the `Hours_nd_Percentage` table is an essential component of the organization's data model, facilitating the evaluation of efficiency and effectiveness in task execution, and supporting strategic planning and operational improvements.

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

_### Table Name: lkp_Code

#### Table Description:
The `lkp_Code` table serves as a lookup reference for various codes used throughout the database. It is designed to provide a standardized set of codes that can be referenced by other tables, ensuring data consistency and integrity across the application. This table typically includes a variety of code types, such as status codes, category codes, or type identifiers, which are essential for categorizing and filtering data in related entities.

#### Key Attributes:
- **Code_ID** (Primary Key): A unique identifier for each code entry in the table. This is typically an integer or string value that ensures each code can be distinctly referenced.
- **Code_Value**: The actual value of the code, which may be a short string or alphanumeric representation. This value is what users or systems will reference when categorizing or filtering data.
- **Description**: A textual description of what the code represents. This field provides context and clarity, helping users understand the meaning and usage of each code.
- **Category**: An optional field that indicates the category or type of code (e.g., status, type, etc.). This helps in organizing codes into logical groups for easier management and retrieval.
- **Is_Active**: A boolean flag indicating whether the code is currently active or deprecated. This allows for the management of codes over time, ensuring that only relevant codes are used in operations.

#### Usage:
The `lkp_Code` table is utilized by various other tables within the database to enforce referential integrity and to provide a consistent framework for code usage. By referencing this table, applications can ensure that only valid codes are used in data entries, thereby reducing errors and improving data quality.

#### Example Entries:
| Code_ID | Code_Value | Description          | Category | Is_Active |
|---------|------------|----------------------|----------|-----------|
| 1       | ACTIVE     | Active status        | Status   | TRUE      |
| 2       | INACTIVE   | Inactive status      | Status   | TRUE      |
| 3       | PENDING    | Pending status       | Status   | TRUE      |
| 4       | ERROR      | Error code           | Type     | TRUE      |
| 5       | DEPRECATED | Deprecated code      | Type     | FALSE     |

This table is crucial for maintaining a robust and reliable data model, facilitating efficient data management and reporting across the system._

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

### Table Name: lkp_fltr_Employee

#### Table Description:
The `lkp_fltr_Employee` table serves as a lookup reference for employee-related data within the organization. It is designed to provide a standardized set of attributes that can be used to filter and categorize employee records across various applications and reporting tools. This table enhances data integrity and consistency by centralizing key employee information, making it easier to manage and retrieve employee-related data.

#### Key Attributes:
- **EmployeeID**: A unique identifier for each employee, serving as the primary key for the table.
- **FirstName**: The first name of the employee.
- **LastName**: The last name of the employee.
- **Department**: The department in which the employee works, facilitating departmental filtering and reporting.
- **JobTitle**: The official job title of the employee, providing context for their role within the organization.
- **Status**: The employment status of the employee (e.g., Active, Inactive, Terminated), allowing for easy filtering of current versus former employees.
- **HireDate**: The date the employee was hired, useful for tenure calculations and reporting.
- **Location**: The physical location or office where the employee is based, aiding in geographical reporting and analysis.

#### Purpose:
The `lkp_fltr_Employee` table is utilized by various systems and applications to ensure that employee data is consistently referenced and filtered. It supports reporting, analytics, and operational processes by providing a reliable source of employee information that can be easily queried and integrated with other datasets.

#### Usage:
This table is commonly used in HR management systems, payroll processing, and performance evaluation tools. It is also leveraged in business intelligence applications to generate reports and dashboards that provide insights into workforce demographics, departmental performance, and employee trends.

#### Relationships:
The `lkp_fltr_Employee` table may have relationships with other tables such as `EmployeeDetails`, `Payroll`, and `PerformanceReviews`, allowing for comprehensive data analysis and reporting across the organization.

#### Maintenance:
Regular updates to the `lkp_fltr_Employee` table are essential to ensure data accuracy, particularly when employees are hired, promoted, or leave the organization. Data governance practices should be implemented to maintain the integrity of the information contained within this lookup table.

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

### Table Name: lkp_Project

#### Table Description:
The `lkp_Project` table serves as a reference or lookup table that contains essential information about various projects within an organization. This table is designed to provide a standardized set of attributes related to each project, facilitating data integrity and consistency across the database. It is typically used in conjunction with other tables that reference project data, ensuring that all project-related information is centralized and easily accessible.

#### Key Attributes:
- **ProjectID** (Primary Key): A unique identifier for each project, typically an integer or GUID, ensuring that each entry can be distinctly referenced.
- **ProjectName**: A descriptive name for the project, providing a clear indication of its purpose or focus.
- **ProjectDescription**: A detailed description of the project, outlining its objectives, scope, and any relevant details that help stakeholders understand its significance.
- **StartDate**: The date when the project is scheduled to commence, allowing for tracking of project timelines.
- **EndDate**: The projected completion date of the project, which aids in planning and resource allocation.
- **Status**: The current status of the project (e.g., Active, Completed, On Hold, Canceled), providing insight into its progress and viability.
- **ProjectManagerID**: A foreign key linking to the `lkp_Employee` table, identifying the individual responsible for overseeing the project.
- **Budget**: The allocated financial resources for the project, which is crucial for financial tracking and management.
- **CreatedDate**: The date when the project record was created, useful for auditing and historical reference.
- **ModifiedDate**: The date when the project record was last updated, ensuring that users have access to the most current information.

#### Usage:
The `lkp_Project` table is utilized by various applications and reporting tools within the organization to generate insights, track project performance, and facilitate decision-making processes. By maintaining a comprehensive and well-structured lookup table, the organization can ensure that all project-related data is accurate, consistent, and readily available for analysis.

#### Relationships:
This table may have relationships with other tables such as `lkp_Employee`, `tbl_Task`, and `tbl_Resource`, allowing for a more integrated view of project management activities and resource allocation within the organization.

#### Data Integrity:
To maintain data integrity, constraints such as unique keys, foreign key relationships, and validation rules should be enforced on the `lkp_Project` table. This ensures that all project entries are valid and that relationships

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

### Table Name: lkp_Unit

#### Table Description:
The `lkp_Unit` table serves as a lookup reference for various measurement units used throughout the database. This table is essential for standardizing unit representations across different datasets, ensuring consistency and accuracy in data reporting and analysis. Each entry in the `lkp_Unit` table corresponds to a specific unit of measurement, which can be utilized in various contexts such as inventory management, sales reporting, and product specifications.

#### Key Attributes:
- **UnitID** (Primary Key): A unique identifier for each unit of measurement. This is typically an integer value that serves as the primary key for the table.
- **UnitName**: A descriptive name of the unit (e.g., "Kilogram", "Liter", "Meter"). This field provides a human-readable representation of the unit.
- **UnitSymbol**: The symbol associated with the unit (e.g., "kg", "L", "m"). This field is used for shorthand representation in reports and user interfaces.
- **ConversionFactor**: A numeric value that indicates how the unit relates to a base unit. This is useful for converting between different units of measurement.
- **IsActive**: A boolean flag indicating whether the unit is currently active and available for use. This allows for the deactivation of obsolete units without deleting them from the database.

#### Usage:
The `lkp_Unit` table is utilized by various other tables within the database that require unit information, such as product specifications, inventory records, and sales transactions. By referencing this lookup table, applications can ensure that all measurements are consistent and adhere to the defined units, facilitating accurate calculations and reporting.

#### Example Entries:
| UnitID | UnitName   | UnitSymbol | ConversionFactor | IsActive |
|--------|------------|------------|------------------|----------|
| 1      | Kilogram   | kg         | 1.0              | true     |
| 2      | Liter      | L          | 1.0              | true     |
| 3      | Meter      | m          | 1.0              | true     |
| 4      | Centimeter | cm         | 0.01             | true     |
| 5      | Inch       | in         | 0.0254           | true     |

This table is crucial for maintaining data integrity and ensuring that all measurements are accurately represented across the system.

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

### Table Name: `tbl_Project`

#### Table Description:
The `tbl_Project` table serves as a central repository for storing detailed information about various projects within an organization. This table is designed to facilitate project management by capturing essential attributes related to each project, enabling effective tracking, reporting, and analysis.

#### Key Attributes:
- **ProjectID** (Primary Key): A unique identifier for each project, typically an auto-incrementing integer or a GUID, ensuring that each project can be distinctly referenced.
- **ProjectName**: A descriptive name for the project, providing a clear indication of its purpose or objective.
- **ProjectDescription**: A detailed narrative outlining the scope, goals, and deliverables of the project, allowing stakeholders to understand its significance.
- **StartDate**: The date when the project is officially initiated, marking the beginning of project activities.
- **EndDate**: The anticipated or actual completion date of the project, helping to assess project timelines and deadlines.
- **Status**: The current state of the project (e.g., Planned, In Progress, Completed, On Hold), which aids in monitoring project progress and performance.
- **Budget**: The allocated financial resources for the project, essential for financial planning and management.
- **ProjectManagerID**: A foreign key linking to the `tbl_Employee` table, identifying the individual responsible for overseeing the project and ensuring its successful execution.
- **CreatedDate**: The timestamp indicating when the project record was created, useful for auditing and historical reference.
- **ModifiedDate**: The timestamp of the last update made to the project record, providing insight into the project's evolution over time.

#### Purpose:
The `tbl_Project` table is integral to project management systems, enabling organizations to maintain a comprehensive overview of their projects. By capturing critical project data, it supports decision-making processes, resource allocation, and performance evaluation, ultimately contributing to the successful delivery of projects.

#### Relationships:
- The `ProjectManagerID` field establishes a relationship with the `tbl_Employee` table, linking projects to the employees responsible for their management.
- Additional relationships may exist with other tables, such as `tbl_Task` for task management or `tbl_Resource` for resource allocation, depending on the overall database schema.

#### Usage:
This table is utilized by project managers, team members, and stakeholders to input, update, and retrieve project-related information, ensuring that all parties have access to the latest project data for informed decision-making and collaboration.

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

### Table Description: `vw_missing_timesheet`

The `vw_missing_timesheet` is a database view designed to provide a consolidated overview of employees who have not submitted their timesheets for a specified period. This view aggregates relevant data from various tables, including employee records, timesheet submissions, and payroll information, to identify individuals with missing timesheet entries.

#### Key Features:
- **Employee Identification**: The view includes essential employee details such as Employee ID, Name, Department, and Position, allowing for easy identification of individuals.
- **Timesheet Status**: It highlights the status of timesheet submissions, specifically focusing on those that are missing or overdue.
- **Date Range Filtering**: The view can be filtered by specific date ranges to assess compliance with timesheet submission deadlines.
- **Reporting Utility**: This view serves as a valuable tool for HR and management to monitor timesheet compliance, facilitating timely follow-ups and ensuring accurate payroll processing.

#### Columns:
- **Employee_ID**: Unique identifier for each employee.
- **Employee_Name**: Full name of the employee.
- **Department**: The department in which the employee works.
- **Position**: The job title of the employee.
- **Missing_Timesheet_Period**: The specific period for which the timesheet is missing.
- **Submission_Deadline**: The deadline by which the timesheet should have been submitted.
- **Last_Submission_Date**: The date of the last submitted timesheet, if applicable.

#### Use Cases:
- **HR Management**: To track and manage timesheet submissions and ensure compliance with company policies.
- **Payroll Processing**: To identify employees who may not be compensated accurately due to missing timesheets.
- **Performance Monitoring**: To assess employee adherence to timesheet submission protocols and identify potential areas for improvement.

This view is essential for maintaining accurate records and ensuring that all employees are held accountable for their timesheet submissions, ultimately contributing to the efficiency of payroll operations and workforce management.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

### Table Description: `vw_Timesheet`

The `vw_Timesheet` is a database view designed to provide a consolidated and user-friendly representation of timesheet data for employees within an organization. This view aggregates relevant information from various underlying tables, such as employee records, project assignments, and time entries, to facilitate reporting and analysis of work hours, project allocations, and overall productivity.

#### Key Features:
- **Employee Information**: Includes essential details about employees, such as employee ID, name, department, and role, allowing for easy identification and categorization of timesheet entries.
- **Time Entries**: Captures daily or weekly time logged by employees, detailing hours worked on specific projects or tasks, including start and end times.
- **Project Details**: Links time entries to specific projects, providing insights into project workloads and resource allocation.
- **Date Range**: Supports filtering by date ranges, enabling users to analyze timesheet data over specific periods for better project management and payroll processing.
- **Calculated Fields**: May include calculated fields such as total hours worked, overtime hours, and billable hours, enhancing the analytical capabilities of the view.

#### Use Cases:
- **Payroll Processing**: Streamlines the payroll process by providing accurate and up-to-date timesheet data for compensation calculations.
- **Project Management**: Assists project managers in tracking time spent on various projects, helping to assess project progress and resource utilization.
- **Reporting and Analytics**: Serves as a foundation for generating reports and dashboards that visualize employee productivity and project performance metrics.

#### Access and Security:
Access to the `vw_Timesheet` view is typically restricted to authorized personnel, such as HR staff, project managers, and finance teams, to ensure the confidentiality and integrity of employee time data.

This view is essential for organizations aiming to optimize their workforce management and enhance operational efficiency through effective time tracking and reporting.

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

- Filter on Unknown Target (Type: N/A, Definition: `N/A`)

_No page details were loaded or parsed._

