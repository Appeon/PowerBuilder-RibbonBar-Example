# New Features in SnapDevelop 2019 R2 Beta

SnapDevelop 2019 R2 Beta has the following new features.

## New or Changed APIs

......

## Open PB Workspace

You can right click a solution in the Solution Explorer and select Open PB Workspace. If you open a PowerBuilder workspace in SnapDevelop, the workspace displays in the Solution Explorer with all its targets, libraries and objects.

With a PB workspace opened as read-only in SnapDevelop, you can perform these operations on selected DataWindows/DataStores or userobjects : 

- Convert DataWindow to C# Model. Note that this feature is the C# Model Generator that was previously available in PowerBuilder IDE.  There are two main changes with this feature: (1) DataWindow XML templates can be converted to C# together with the DataWindow; (2) The generated C# models no longer have dependencies on the SRD files. For details, see [Working with DataWindow Converter](Working_with_DataWindow Converter.html).
- Open PowerScript as read-only in the editor and then convert PowerScript to C# using [PowerScript Migrator](Using_PowerScript_Migrator.html). Currently, the translation is mainly performed to individual functions, including the common usages of basic data types, DataStores, and embedded SQLs.

## SQL Query

SnapDevelop's SQL Query allows you to view data from the database, write and execute your SQL statements, select particular ways in which your query results display, see how your SQL statements are executed, and export the query results to an external file. It currently supports connection to the following databases: SQL Server, Oracle, PostgreSQL, and ODBC (ASA Only).

For details, see [Using SQL Query](Using_SQL_Query.html).

## Scaffolding

Scaffolding is a technique to effectively create templates for services and controllers which can be used to easily build an application.

......

## Publish to Docker

SnapDevelop 2019 R2 supports publishing C# projects to Docker. So far, SnapDevelop supports three ways of publication: Web Deploy, File System, and Docker.

For details, see [Publishing a Project with Docker](Publishing_a_Project_with_Docker.html).

## (Preview) Web API Tester

If you create a Web API project, you can use the Web API Tester to test Web APIs locally to check requests and responses.\

**Note: **This Web API Tester is at preview stage in the current beta release, because: (1) Using.NET DataStore as the testing parameter is just completed so the feature may not be stable yet; (2) Using DataPacker as the testing parameter is unsupported yet, and will be supported in the R2 GA version.

For details, see [Testing with Web API Tester](Testing_with_Web_API_Tester.html).

## Export From or Import To Data Models

It is now possible to export XML from .NET DataStore or import XML into .NET DataStore, or use templates to export XML or JSON from .NET DataStore or SnapObjects models.

For .NET APIs added to support the export/import, see [New or Changed APIs](#new-or-changed-apis).
For detailed specification on how to define an XML or JSON template to export data from .NET DataStore or SnapObjects models, see [Using Export Templates for XML or JSON Export](Using_Export_Templates_forXML_or JSON Export.html).

For a sample project on how to execute XML export from .NET DataStore, see <https://github.com/Appeon/.NETDataStore-XML-Export-Example>.