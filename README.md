
# EmployeeHub API

EmployeeHub API is a comprehensive backend service built with ASP.NET Web API for managing employee and department data. It exposes a set of RESTful endpoints for performing CRUD (Create, Read, Update, Delete) operations. The API is built on the .NET Framework 4.6.2 and is configured to connect to a SQL Server database, making it a robust foundation for an enterprise management application.

This backend is designed to be consumed by a modern web frontend, such as one built with React and Bootstrap.

## Features

*   **RESTful API:** Provides a clean, standard-based API for managing resources.
*   **Department Management:** Full CRUD functionality for departments.
*   **Employee Management:** Full CRUD functionality for employees.
*   **Database Integration:** Connects to a SQL Server database to persist data.
*   **CORS Enabled:** Pre-configured Cross-Origin Resource Sharing (CORS) to allow requests from a frontend application running on `http://localhost:3001`.
*   **API Help Pages:** Includes auto-generated API documentation accessible through the `/Help` route for easy testing and integration.

## Technologies Used

*   **Backend:** ASP.NET Web API 2, C#, .NET Framework 4.6.2
*   **Database:** Microsoft SQL Server
*   **Key NuGet Packages:**
    *   `Microsoft.AspNet.WebApi` (v5.2.7)
    *   `Microsoft.AspNet.Cors` (v5.2.7)
    *   `Newtonsoft.Json` (v12.0.2)
*   **Frontend (as intended):** React, Bootstrap (v3.4.1)

## API Endpoints

The API exposes the following endpoints:

### Department Controller

**Base Route:** `/api/department`

| Method | Route                  | Description                        | Request Body        | Response                                         |
| :----- | :--------------------- | :--------------------------------- | :------------------ | :----------------------------------------------- |
| `GET`  | `/api/department`      | Retrieves a list of all departments. | (None)              | A JSON array of `Department` objects.            |
| `POST` | `/api/department`      | Adds a new department.             | `Department` object | `string` - "Added succesfully" or "Failed to add"    |
| `PUT`  | `/api/department`      | Updates an existing department.    | `Department` object | `string` - "Updated succesfully" or "Failed to update" |
| `DELETE`| `/api/department/{id}` | Deletes a department by its ID.    | (None)              | `string` - "Deleted succesfully" or "Failed to delete" |

### Employee Controller

**Base Route:** `/api/employee`

| Method | Route               | Description                     | Request Body         | Response                                         |
| :----- | :------------------ | :------------------------------ | :------------------- | :----------------------------------------------- |
| `GET`  | `/api/employee`     | Retrieves a list of all employees. | (None)               | A JSON array of `Employee` objects.              |
| `POST` | `/api/employee`     | Adds a new employee.            | `Employees` object   | `string` - "Added succesfully" or "Failed to add"    |
| `PUT`  | `/api/employee`     | Updates an existing employee.   | `Employees` object   | `string` - "Updated succesfully" or "Failed to update" |
| `DELETE`| `/api/employee/{id}`| Deletes an employee by their ID.| (None)               | `string` - "Deleted succesfully" or "Failed to delete" |

## Data Models

### Department

```csharp
public class Department
{
    public int DepartmentID { get; set; }
    public string DepartmentName { get; set; }
}
```

### Employees

```csharp
public class Employees
{
    public int EmployeeID { get; set; }
    public string EmployeeName { get; set; }
    public string Department { get; set; }
    public string MailID { get; set; }
    public DateTime? DOJ { get; set; } // Date of Joining
}
```

## Setup and Installation

### Prerequisites

*   **Visual Studio 2019** (or compatible version)
*   **.NET Framework 4.6.2**
*   **Microsoft SQL Server**

### 1. Database Setup

The API requires a database named `EmployeeDB` with two tables: `Departments` and `Employees`. You can use the following SQL scripts to create them:

```sql
-- Create the database
CREATE DATABASE EmployeeDB;
GO

USE EmployeeDB;
GO

-- Create the Departments table
CREATE TABLE dbo.Departments (
    DepartmentID INT IDENTITY(1,1) PRIMARY KEY,
    DepartmentName VARCHAR(500)
);
GO

-- Create the Employees table
CREATE TABLE dbo.Employees (
    EmployeeID INT IDENTITY(1,1) PRIMARY KEY,
    EmployeeName VARCHAR(500),
    Department VARCHAR(500),
    MailID VARCHAR(500),
    DOJ DATE
);
GO
```

### 2. Configure Connection String

Open the `Web.config` file in the root of the project and locate the `<connectionStrings>` section. Update the `EmployeeAppDB` connection string to point to your SQL Server instance.

```xml
<connectionStrings>
    <!-- Update the Data Source to your SQL Server instance name -->
    <add name="EmployeeAppDB" 
         connectionString="Data Source=YOUR_SERVER_NAME; Initial Catalog=EmployeeDB; Integrated Security=true;" 
         providerName="System.Data.SqlClient" />
</connectionStrings>
```

### 3. Build and Run

1.  Open `WebAPI.sln` in Visual Studio.
2.  Restore the NuGet packages (this should happen automatically, but you can right-click the solution in Solution Explorer and select "Restore NuGet Packages" if needed).
3.  Build the solution (Build > Build Solution).
4.  Run the project (press F5 or click the Start button).

This will launch the application in your default browser, hosted by IIS Express. You can then access the API endpoints (e.g., `http://localhost:<port>/api/department`).

## Configuration Details

### CORS (Cross-Origin Resource Sharing)

CORS is enabled in `App_Start/WebApiConfig.cs`. It is configured to allow requests from `http://localhost:3001` (a common port for React development servers).

```csharp
// In WebApiConfig.cs
config.EnableCors(new EnableCorsAttribute("http://localhost:3001", "*", "*"));
```

If your frontend is running on a different port, you will need to update this line.

### Build Configurations

The project includes `Debug` and `Release` build configurations with corresponding `Web.config` transformations:
*   **Web.Debug.config:** Used for development.
*   **Web.Release.config:** Removes the `debug="true"` attribute from the `<compilation>` tag for production builds.

## Project Structure

```
EmployeeHub-API/
├── App_Start/              # Startup configuration (Bundles, Filters, Routes, Web API)
│   ├── BundleConfig.cs
│   ├── FilterConfig.cs
│   ├── RouteConfig.cs
│   └── WebApiConfig.cs
├── Areas/HelpPage/         # Auto-generated API documentation pages
├── Content/                # CSS and other static content (Bootstrap)
├── Controllers/            # API controllers
│   ├── DepartmentController.cs
│   ├── EmployeeController.cs
│   └── HomeController.cs
├── Models/                 # Data models (POCOs)
│   ├── Department.cs
│   └── Employees.cs
├── Properties/
├── Scripts/                # JavaScript files (jQuery, Bootstrap)
├── Views/                  # MVC Views for the home page and help pages
├── Global.asax             # Application entry point
├── packages.config         # NuGet package dependencies
├── Web.config              # Main application configuration
├── Web.Debug.config        # Debug-specific configuration transforms
├── Web.Release.config      # Release-specific configuration transforms
└── WebAPI.sln              # Visual Studio Solution file
```
