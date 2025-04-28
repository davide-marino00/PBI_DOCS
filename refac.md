# Power BI Model & Report Documentation

*Generated on: 2025-04-28 17:30:32*

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
   - Example entities may include:
     - **Customer**: Represents individuals or organizations that purchase products or services.
     - **Product**: Represents items available for sale.
     - **Order**: Represents transactions made by customers.

2. **Attributes**: 
   - Attributes are the specific data points that describe an entity. Each attribute has a defined data type and may have constraints such as uniqueness or nullability.
   - Example attributes for the Customer entity might include:
     - CustomerID (Primary Key)
     - Name
     - Email
     - PhoneNumber

3. **Relationships**: 
   - Relationships define how entities interact with one another. They can be one-to-one, one-to-many, or many-to-many.
   - Example relationships may include:
     - A Customer can place multiple Orders (one-to-many).
     - An Order can contain multiple Products (many-to-many).

4. **Constraints**: 
   - Constraints are rules that ensure data integrity and consistency within the model. They may include primary keys, foreign keys, unique constraints, and check constraints.
   - Example constraints might include:
     - CustomerID must be unique for each Customer.
     - OrderDate must not be in the future.

5. **Normalization**: 
   - The model adheres to normalization principles to reduce data redundancy and improve data integrity. This involves organizing data into separate tables and establishing relationships between them.

6. **Documentation**: 
   - Comprehensive documentation accompanies the model, detailing each entity, attribute, relationship, and constraint. This documentation serves as a reference for developers, analysts, and stakeholders.

#### Use Cases
- **Data Warehousing**: The model can be used to design a data warehouse schema that supports reporting and analytics.
- **Application Development**: Developers can utilize the model to create

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
The `DimDate` table serves as a dimension table in a star schema data model, primarily used for time-based analysis and reporting. It provides a comprehensive set of attributes related to dates, enabling users to perform time-based queries and analyses efficiently. This table is essential for facilitating time intelligence in data analytics, allowing for the aggregation and comparison of data across various time periods.

#### Key Attributes:
- **DateKey**: A unique identifier for each date, typically formatted as an integer (YYYYMMDD) to facilitate efficient querying and indexing.
- **FullDate**: The complete date in a standard date format (e.g., YYYY-MM-DD) for easy readability and use in reporting.
- **Year**: The year component of the date, allowing for yearly aggregations and comparisons.
- **Quarter**: The quarter of the year (Q1, Q2, Q3, Q4) to enable quarterly reporting and analysis.
- **Month**: The month component of the date, often represented as both a number (1-12) and a name (e.g., January, February).
- **Day**: The day of the month, providing granularity for daily reporting.
- **DayOfWeek**: The day of the week (e.g., Monday, Tuesday) to facilitate weekly analysis.
- **IsWeekend**: A boolean flag indicating whether the date falls on a weekend, useful for analyzing business operations.
- **FiscalYear**: The fiscal year associated with the date, which may differ from the calendar year, allowing for fiscal reporting.
- **HolidayFlag**: A boolean flag indicating whether the date is a recognized holiday, aiding in the analysis of business performance during holiday periods.

#### Usage:
The `DimDate` table is commonly joined with fact tables in a data warehouse to provide context for time-related metrics. Analysts and business intelligence tools leverage this table to generate reports, dashboards, and visualizations that require date-based filtering, grouping, and calculations.

#### Data Type:
The attributes in the `DimDate` table are typically stored in various data types, including integers for keys, strings for names, and date types for date-related fields.

#### Maintenance:
The `DimDate` table is usually populated with a continuous range of dates, often spanning several years into the future and past, to accommodate historical analysis and future planning. Regular updates may be necessary to add new dates as time progresses.

This table is a foundational component of any data warehouse that requires robust

---

#### <a name="table-hours_nd_percentage"></a>Table: `Hours_nd_Percentage`

### Table: Hours_nd_Percentage

#### Description:
The `Hours_nd_Percentage` table is designed to capture and analyze the relationship between hours worked and the corresponding percentage of total hours allocated to various tasks or projects within an organization. This table serves as a critical resource for performance evaluation, resource allocation, and productivity analysis.

#### Key Attributes:
- **Employee_ID** (Primary Key): A unique identifier for each employee, linking to the employee's profile in the organization.
- **Task_ID**: A unique identifier for each task or project, allowing for detailed tracking of hours spent on specific activities.
- **Hours_Worked**: A numeric value representing the total hours an employee has dedicated to a specific task or project during a defined period.
- **Total_Hours**: A numeric value indicating the total hours available for work during the same period, providing context for the percentage calculation.
- **Percentage_Of_Total_Hours**: A calculated field that represents the proportion of hours worked on a specific task relative to the total hours available, expressed as a percentage.
- **Date**: A timestamp indicating the period during which the hours were recorded, facilitating time-based analysis and reporting.

#### Purpose:
The `Hours_nd_Percentage` table is instrumental in helping organizations monitor employee productivity, assess workload distribution, and identify areas for improvement. By analyzing the data within this table, management can make informed decisions regarding resource allocation, project prioritization, and employee performance evaluations.

#### Use Cases:
- **Performance Analysis**: Evaluate individual and team performance based on hours worked and task completion rates.
- **Resource Management**: Identify over- or under-utilization of resources across various projects.
- **Reporting**: Generate reports for stakeholders to visualize productivity trends and workload distribution.

This table is essential for organizations aiming to optimize their operational efficiency and enhance overall productivity through data-driven insights.

---

#### <a name="table-lkp_code"></a>Table: `lkp_Code`

### Table Name: lkp_Code

#### Table Description:
The `lkp_Code` table serves as a lookup reference for various codes used throughout the database. It is designed to standardize and centralize the management of code values that are frequently referenced in other tables, ensuring consistency and reducing redundancy. This table typically includes a variety of code types, such as status codes, category codes, or type identifiers, which can be utilized across different modules of the application.

#### Key Attributes:
- **Code_ID** (Primary Key): A unique identifier for each code entry in the table. This is typically an integer or string value that ensures each code can be distinctly referenced.
- **Code_Value**: The actual value of the code, which may be a string or numeric representation. This is the value that will be used in other tables to reference the specific code.
- **Description**: A textual description of what the code represents, providing context and clarity for users and developers.
- **Category**: An optional field that categorizes the code into broader groups, facilitating easier filtering and searching of codes.
- **Is_Active**: A boolean flag indicating whether the code is currently active or deprecated, allowing for better management of code lifecycle and usability.

#### Usage:
The `lkp_Code` table is utilized by various other tables within the database to enforce referential integrity and to provide a consistent set of codes for data entry and reporting. By referencing this lookup table, applications can ensure that only valid codes are used, thereby enhancing data quality and integrity.

#### Example Entries:
| Code_ID | Code_Value | Description           | Category   | Is_Active |
|---------|------------|-----------------------|------------|-----------|
| 1       | 'A'       | Active                | Status     | TRUE      |
| 2       | 'I'       | Inactive              | Status     | TRUE      |
| 3       | 'P'       | Pending                | Status     | TRUE      |
| 4       | 'C'       | Completed             | Status     | TRUE      |
| 5       | 'X'       | Cancelled             | Status     | FALSE     |

This structure allows for efficient management and retrieval of code-related information, supporting various business processes and reporting requirements across the organization.

---

#### <a name="table-lkp_fltr_employee"></a>Table: `lkp_fltr_Employee`

### Table Name: lkp_fltr_Employee

#### Table Description:
The `lkp_fltr_Employee` table serves as a lookup table that contains essential information about employees within an organization. This table is designed to facilitate filtering and categorization of employee data across various applications and reports. It provides a standardized reference for employee attributes, ensuring consistency and accuracy in data retrieval and analysis.

#### Key Attributes:
- **EmployeeID** (Primary Key): A unique identifier for each employee, ensuring that all records can be distinctly referenced.
- **FirstName**: The first name of the employee, used for identification and personalization in communications.
- **LastName**: The last name of the employee, essential for formal identification and reporting.
- **Department**: The department in which the employee works, allowing for organizational categorization and filtering.
- **Position**: The job title or position held by the employee, providing insight into their role within the organization.
- **HireDate**: The date the employee was hired, useful for tracking tenure and employment history.
- **Status**: The current employment status of the employee (e.g., Active, Inactive, Terminated), which aids in filtering current employees from those who are no longer with the organization.
- **Email**: The official email address of the employee, facilitating communication and contact.

#### Purpose:
The `lkp_fltr_Employee` table is utilized by various systems and applications to streamline employee-related queries, enhance reporting capabilities, and support data integrity across the organization. By maintaining a centralized repository of employee information, this table aids in efficient data management and decision-making processes.

#### Usage:
This table is commonly referenced in employee management systems, HR analytics, and reporting tools, enabling users to quickly access and filter employee data based on various criteria. It is also instrumental in generating insights related to workforce demographics, departmental performance, and employee engagement initiatives.

#### Relationships:
The `lkp_fltr_Employee` table may be linked to other tables within the database, such as payroll, performance reviews, and training records, to provide a comprehensive view of employee-related data across the organization.

---

#### <a name="table-lkp_project"></a>Table: `lkp_Project`

### Table Name: lkp_Project

#### Table Description:
The `lkp_Project` table serves as a lookup reference for project-related information within the database. It is designed to store essential details about various projects, enabling efficient data retrieval and management. This table is crucial for maintaining data integrity and consistency across related entities in the system.

#### Key Attributes:
- **ProjectID** (Primary Key): A unique identifier for each project, ensuring that each entry can be distinctly referenced.
- **ProjectName**: The name of the project, providing a human-readable identifier that describes the project’s purpose or focus.
- **ProjectDescription**: A detailed description of the project, outlining its objectives, scope, and any relevant background information.
- **StartDate**: The date when the project is scheduled to commence, allowing for timeline management and scheduling.
- **EndDate**: The projected completion date of the project, facilitating tracking of project duration and deadlines.
- **Status**: The current status of the project (e.g., Active, Completed, On Hold), which aids in monitoring progress and resource allocation.
- **OwnerID**: A foreign key linking to the user or department responsible for the project, ensuring accountability and clear ownership.

#### Purpose:
The `lkp_Project` table is utilized by various applications and reports within the organization to provide context and details about ongoing and completed projects. It supports project management processes, resource allocation, and performance tracking, making it an integral part of the overall data architecture.

#### Relationships:
- The `lkp_Project` table may have relationships with other tables such as `lkp_User`, `lkp_Department`, or `tbl_Task`, allowing for comprehensive data analysis and reporting across different dimensions of project management.

#### Usage:
This table is primarily accessed by project managers, analysts, and reporting tools to generate insights, track project performance, and facilitate decision-making processes related to project execution and resource management. 

#### Data Integrity:
To maintain data integrity, constraints such as unique keys, foreign key relationships, and validation rules are enforced within the `lkp_Project` table, ensuring that all project data is accurate and reliable.

---

#### <a name="table-lkp_unit"></a>Table: `lkp_Unit`

### Table Name: lkp_Unit

#### Table Description:
The `lkp_Unit` table serves as a lookup reference for various measurement units used throughout the database. This table is essential for standardizing the representation of units across different datasets, ensuring consistency and accuracy in data reporting and analysis. 

#### Key Attributes:
- **UnitID** (Primary Key): A unique identifier for each unit, typically an integer or GUID, which allows for efficient referencing and retrieval.
- **UnitName**: A string that represents the name of the unit (e.g., "Kilogram", "Liter", "Meter"). This field is crucial for user-friendly display and understanding of the unit.
- **UnitSymbol**: A string that contains the symbol associated with the unit (e.g., "kg", "L", "m"). This is used in calculations and reporting to provide a concise representation of the unit.
- **UnitType**: A string that categorizes the unit (e.g., "Weight", "Volume", "Length"). This classification helps in filtering and grouping units based on their application.
- **IsActive**: A boolean flag indicating whether the unit is currently active or deprecated. This allows for historical data integrity while managing changes in unit usage.

#### Purpose:
The `lkp_Unit` table is designed to facilitate the management of measurement units across various applications within the database. By centralizing unit definitions, it supports data integrity, enhances reporting capabilities, and simplifies the process of unit conversion when necessary.

#### Usage:
This table is typically referenced by other tables that require unit information, such as product specifications, inventory management, and sales data. It plays a critical role in ensuring that all measurements are interpreted correctly and consistently across the system.

#### Example Records:
| UnitID | UnitName | UnitSymbol | UnitType | IsActive |
|--------|----------|------------|----------|----------|
| 1      | Kilogram | kg         | Weight   | true     |
| 2      | Liter    | L          | Volume   | true     |
| 3      | Meter    | m          | Length   | true     |
| 4      | Pound    | lb         | Weight   | true     |
| 5      | Gallon   | gal        | Volume   | false    |

This structured approach to unit management ensures that all stakeholders can rely on a consistent framework for measurement across the organization.

---

#### <a name="table-tbl_project"></a>Table: `tbl_Project`

### Table Name: `tbl_Project`

#### Table Description:
The `tbl_Project` table serves as a central repository for storing detailed information about various projects within an organization. This table is designed to facilitate project management by capturing essential attributes related to each project, including its identification, status, timelines, budget, and associated personnel.

#### Key Attributes:
- **ProjectID** (Primary Key): A unique identifier for each project, typically an auto-incrementing integer or a GUID, ensuring that each project can be distinctly referenced.
- **ProjectName**: A descriptive name for the project, providing a clear indication of its purpose or focus.
- **ProjectDescription**: A detailed narrative outlining the objectives, scope, and deliverables of the project.
- **StartDate**: The date when the project is officially initiated, marking the beginning of project activities.
- **EndDate**: The projected or actual completion date of the project, indicating when the project is expected to conclude or has concluded.
- **Status**: A categorical field representing the current state of the project (e.g., Planned, In Progress, Completed, On Hold, Canceled).
- **Budget**: The allocated financial resources for the project, which may include estimates for labor, materials, and other expenses.
- **ProjectManagerID**: A foreign key linking to the `tbl_Employee` table, identifying the individual responsible for overseeing the project.
- **CreatedDate**: The timestamp indicating when the project record was created in the database.
- **LastUpdatedDate**: The timestamp of the most recent update made to the project record, allowing for tracking of changes over time.

#### Purpose:
The `tbl_Project` table is integral to project tracking and management processes, enabling stakeholders to monitor progress, allocate resources effectively, and ensure that projects align with organizational goals. By maintaining comprehensive records of each project, the table supports reporting, analysis, and decision-making activities.

#### Relationships:
- The `ProjectManagerID` field establishes a relationship with the `tbl_Employee` table, linking projects to the employees responsible for their execution.
- Additional relationships may exist with other tables, such as `tbl_Task` for task management or `tbl_Resource` for resource allocation, depending on the overall database schema.

#### Usage:
This table is utilized by project managers, team members, and organizational leaders to gain insights into project performance, manage timelines and budgets, and facilitate communication among stakeholders. It is a critical component of the organization's project management framework.

---

#### <a name="table-vw_missing_timesheet"></a>Table: `vw_missing_timesheet`

### Table Name: `vw_missing_timesheet`

#### Table Description:
The `vw_missing_timesheet` is a database view designed to provide a consolidated overview of employees who have not submitted their timesheets within the designated reporting period. This view serves as a critical tool for payroll and human resources departments to identify and address compliance issues related to timesheet submissions.

#### Key Features:
- **Employee Identification**: The view includes essential employee details such as Employee ID, Name, and Department, allowing for easy identification of individuals who have missing timesheets.
- **Reporting Period**: It captures the specific reporting period for which timesheets are missing, enabling targeted follow-up actions.
- **Submission Status**: The view indicates the status of timesheet submissions, highlighting those that are overdue or not submitted at all.
- **Date Filters**: The view can be filtered by date ranges to focus on specific payroll cycles or reporting periods, enhancing its usability for various administrative needs.
- **Aggregated Data**: It may aggregate data to show trends in timesheet submissions over time, helping management to identify patterns or recurring issues.

#### Use Cases:
- **Compliance Monitoring**: HR personnel can use this view to ensure that all employees are adhering to timesheet submission policies.
- **Payroll Processing**: Payroll departments can quickly identify employees whose timesheets are missing, preventing delays in payroll processing.
- **Performance Management**: Managers can utilize this information to address potential performance issues with employees who frequently miss submissions.

#### Data Sources:
The `vw_missing_timesheet` view is derived from the underlying timesheet and employee tables, ensuring that it reflects the most current data available regarding timesheet submissions.

#### Maintenance:
Regular updates to the underlying data sources are necessary to keep the view accurate and relevant. It is recommended to review the view's logic periodically to ensure it aligns with any changes in timesheet policies or reporting requirements.

This view is an essential component of the organization's data management strategy, facilitating efficient tracking and management of timesheet submissions across the workforce.

---

#### <a name="table-vw_timesheet"></a>Table: `vw_Timesheet`

### Table Description: `vw_Timesheet`

The `vw_Timesheet` table is a database view designed to provide a consolidated and user-friendly representation of timesheet data for employees within an organization. This view aggregates relevant information from various underlying tables related to employee work hours, project assignments, and payroll details. 

#### Purpose:
The primary purpose of the `vw_Timesheet` view is to facilitate reporting and analysis of employee time tracking, ensuring that managers and HR personnel can easily access and interpret timesheet data for payroll processing, project management, and compliance purposes.

#### Key Features:
- **Employee Information**: The view includes essential employee details such as employee ID, name, department, and position, allowing for easy identification and categorization of timesheet entries.
- **Time Tracking**: It captures daily work hours, overtime, and leave taken, providing a comprehensive overview of employee attendance and productivity.
- **Project Association**: Each timesheet entry is linked to specific projects or tasks, enabling analysis of time allocation across different initiatives and facilitating project costing and budgeting.
- **Date Range Filtering**: The view supports filtering by date ranges, allowing users to generate reports for specific pay periods or project timelines.
- **Status Indicators**: It may include status indicators (e.g., submitted, approved, pending) to streamline the review and approval process for timesheet submissions.

#### Columns:
The `vw_Timesheet` view typically includes the following columns (this may vary based on implementation):
- `EmployeeID`: Unique identifier for the employee.
- `EmployeeName`: Full name of the employee.
- `Department`: The department in which the employee works.
- `Position`: The job title of the employee.
- `ProjectID`: Identifier for the project associated with the timesheet entry.
- `ProjectName`: Name of the project.
- `Date`: The date of the timesheet entry.
- `HoursWorked`: Total hours worked on the specified date.
- `OvertimeHours`: Total overtime hours recorded.
- `LeaveHours`: Total hours taken as leave.
- `Status`: Current status of the timesheet entry (e.g., submitted, approved).

#### Use Cases:
- **Payroll Processing**: Streamlining the payroll process by providing accurate and timely timesheet data.
- **Project Management**: Assisting project managers in tracking time spent on various tasks and projects.
- **Compliance and Auditing**: Ensuring compliance with labor regulations and facilitating audits by providing a clear record of employee hours.

#### Conclusion:
The

---

## <a name="report-structure"></a>Report Structure

### <a name="report-level-filters"></a>Report Level Filters

- Filter on Unknown Target (Type: N/A, Definition: `N/A`)

_No page details were loaded or parsed._

