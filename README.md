# SQL Server Performance Health Check

## Usage

1. Download the [zip] file and copy the `HealthCheck` sub-directory to the
   desired location.
2. Open `.\HealthCheck\DBList.txt` and replace server and database names as
   required.

**For example:**

    Server1\InstanceA; DB1
    Server2\InstanceB; DB2

For servers that don't have the SQL Browser Service enabled, you can specify the
port number delimited by a back-tick and a comma, as follows:

    Server1`,1433; DB1

3. Execute `.\HealthCheck\HealthCheck.bat`.

The report can be found in the `.\HealthCheck\Reports` directory.

## How Does It Work?

The batch file reads a list of SQL Server instance and database names and passes
them as parameter values to a PowerShell script, which it executes in a loop,
once for each database. The PowerShell script then executes a set of diagnostic
T-SQL and PowerShell scripts, exporting metadata to CSV files. Finally, a VBA
macro is executed from PowerShell to merge the CSV files into an Excel
spreadsheet and then formats the report.

## Software Requirements

The following software is required to run the health check:

- [PowerShell] v2.0 or higher (comes with Windows 7 and Windows Server 2008 R2)
- Microsoft Excel 2007 or higher.
- [SQLPS PowerShell Module], which is installed with [SQL Server Management Tools].

## Permission Requirements

A Windows account must be used with the following access to the target server
and database:

**Server Scope**

- View Server State

**Database Scope**

- View definition in target database.
- `ddl_admin` to run `DBCC SHOW_STATISTICS` if the target server is pre-SQL 2012
  SP1, otherwise `db_datareader` is enough.

Remote WMI access is required to run the `ServerInformation.ps1` script. See
[this article] on how to grant this access to non-admin users. If the user does
not have the necessary access, the report will still run but will not include
the server information worksheet.

[PowerShell]: https://msdn.microsoft.com/en-us/powershell/scripting/setup/installing-windows-powershell
[SQLPS PowerShell Module]: https://msdn.microsoft.com/en-us/library/hh245198.aspx
[SQL Server Management Tools]: https://msdn.microsoft.com/en-us/library/hh213248.aspx
[zip]: https://github.com/jonlabelle/HealthCheck/archive/master.zip
[this article]: http://www.adilhindistan.com/2013/10/permissions-to-access-wmi-remotely.html
