# Using Export Templates for XML or JSON Export

##  Overview

It is now possible to export XML from .NET DataStore or import XML into .NET DataStore, or use templates to export XML or JSON from .NET DataStore or SnapObjects models. This document explains how to define an export template for exporting data from .NET DataStore or SnapObjects models to XML or JSON. The export template features simplified syntax, and is database-transparent and data format-transparent.

## Running the Sample Project

First, you can run a sample project to experience the process of exporting .NET DataStore data to XML with or without template:

1. Download the sample project from: <https://github.com/Appeon/.NETDataStore-XML-Export-Example>.

2. Load the sample project XmlDataDemo.sln in SnapDevelop.

3. Open the SampleController.cs file to view the scripts for export in the application.

4. Right mouse click in the code block of a method (for example, ExportXML), and select Run Test(s). 

5. Wait a short while. The Web API Tester will be launched.  

   The Web API Tester opens with the post URL: [http://localhost:5000/api/Sample/ExportXML](http://localhost:5000/api/Sample/ExportXML), which means that it is ready to test the ExportXML web API.

6. Because ExportXML directly exports data to XML in standard format, no template used and no parameter is required, you can directly click the Send button to send the request to the provided URL. The response will be displayed in the ResponseBody section with the response status.

7. If you change the "ExportXml" method name in the URL to a different one defined in the SampleController.cs, for example, ExportByTemplate, you will view the effect of exporting using a different template.

   ExportByTemplate requires you to specify the template as the parameter. For testing purpose, you can paste the following code to the Body section in the Web API Tester , and then Send.

   ```
   "<?xml version=\"1.0\" encoding=\"UTF-16LE\" standalone=\"no\"?>
   <untitled>
     @foreach
     {
     <untitled_row>
       <departmentid>@(departmentid)</departmentid>
       <name>@(name)</name>
       <groupname>@(groupname)</groupname>
       <modifieddate>@(modifieddate)</modifieddate>
     </untitled_row>
     }
   </untitled>"
   ```

## Template Syntax and Examples

### Using Expressions in the template

**Syntax**

@(expression ?? default value)

-   Using this syntax to output the result of the specified expression. ?? default value is optional and refers to the default value.

-   If the syntax of the expression is incorrect, output the expression string;

-   If the syntax of the expression is correct but the expression fails to execute, output the default value if it is defined; otherwise output the expression string.

**Example**

Example 1:

```
@(id)
```

-   Use this syntax to export the "id" column value.

Example 2:

```
@("hello" + name + count(id) ?? 'failed')
```

Use this syntax to output the concatenation of "hello", the value of name, and the value from count(). If the expression fails to execute, output the default value "failed".

### Using Foreach in the template

**Syntax**

```
@foreach(source orderby("") groupby("" orderby("")) into item) { }	
```

The @foreach() syntax iterates over a set of data and outputs the specified content.


| Argument                           | Description                                                                                                                     |
| ---------------------------------- | ------------------------ |
| source                             | The data source to iterate through. If not defined, the current data model to be exported will be regarded as the data source.  |
| orderby("") (optional)                        | Optional. It specifies how to order the data source before the iteration starts.                                                |
| groupby("" orderby("")) (optional) | Optional. The first argument specifies how to group the data source and whether to order the data in each group after grouping. |
| into (optional)                               | Optional. It specifies the iteration parameter and can be used to store each iteration instance.                                |
| { }                                | The output of each iteration.                                                                                                   |

**Example**

Example 1:

```
@foreach
{
  @(id)
}
```

Use this syntax to iterate the current model and use the data from each iteration to execute the expression id and then output.

Example 2:

```
@foreach(groupby(”stateprovinceid”) into group1)
{
@foreach(group1 orderby(“city d”) groupby(”city” orderby(“#1 D”) ) into group2) 
{
	@foreach(group2)
	{
		@(id)
	}
}
}
```

This example contains three iteration loops:

1.  The first loop has no data source defined. It uses the current data model to be exported as the data source, and groups it by "stateprovinceid". When iterating through each subgroup, the subgroup is stored into the parameter group1.

2.  The second loop defines the group1 from the first loop as the data source, and groups it by "city" in descending order, and then groups each subgroup by the value of the first column in descending order. When iterating through each subgroup, the subgroup is stored into the parameter group2.

3.  The third loop defines the group2 from the second loop as the data source, and uses the data from each iteration to execute the expression id and then output.

### Exporting composite report

**Syntax**

```
@partial(name)
```

-   Use this syntax to export the specified data using the current export template.

**Example**

```
@partial(dw_1)
```

This example will export the data in dw\_1 node using the current export template.

### Other syntax

For the syntax that is not covered in the above sections, the data will be exported in their original state.

## Converting PB template to export template

When you use the DataWindow converter to export DWs to .NET DataStore, you will be asked whether to export templates and to which folder to save the exported templates.

**Convert expressions (XML template)**

For each expression node in the PB template, the content, as well as the attribute of the node, will be enclosed using @(). If the expression has a default value, the default value is added in @().

**Convert the iteration structures**

The \_\_pbband=\"detail\" will be converted to @foreach { }.

The \_\_pbband=\"group\" will be converted to @foreach (groupby("" order="")) { }. The brother node above \_\_pbband=\"group\" will be taken as the group by condition; and an iteration parameter will be defined and then used as the data source of the inner loop \_\_pbband=\"group\" or \_\_pbband=\"detail\".

## Example of Exporting .NET DataStore

**Export using specified template**

```
// Create a DataStore object
var ds = new DataStore("d_address2", _context);
// Retrieve data in the DataStore 
ds.Retrieve();

// Read the content of the template
var template = File.ReadAllText("grid.xml");

// Create the template object
var dataTemplate = new DataTemplate();

// Load the template content
dataTemplate.LoadContent(template);

// Export
var xml = DwTemplateExporter.Export(ds.dataTemplate);
```

This example executes the Export() method using the template defined in grid.xml file.

**Export using the template defined in model file**

For example:

```
[DataWindow("d_address_"2", DwStyle.Grid )]
[DwTemplate(DataFormat.Xml, "tx2", "adventureworkstables.pbw\\adventureworkstables \\d_address_g2.tpl.tx2.xml", IsDefault = true)]
```

The export script is as below:

```
var ds = new DataStore("d_address_g2", _context);
ds.Retrieve();

var dateTemplate = dataStore.GetTemplate();
var xml = DwTemplateExporter.Export(ds, dataTemplate);
```

In this example, the export method does not specify a template for export. In such cases, the template accompanying the model will be used as the export template.

## Example of exporting other types of data models

**Exporting ModelEntry data**

```
// Read the packed data
var data = File.ReadAllText("data.xml");
// Create an unpacker
var dataUnpacker = new DataUnpacker(data, DataFormat.Xml);

// Unpack to get ModelEntry data
IEnumerable<IModelEntry> entries = dataUnpacker
    .GetModelEntries<SalesOrderDetail>("data");
// Read the template file
var tempalte = File.ReadAllText("guid.xml");

// Create an exporter
var exporter = new TemplateExporter(entries);
// Execute export
var xml = exporter.Export(tempalte);
```

**Exporting common model**

```
// Load data from a Model
List<SalesOrderDetail> models = _context.SqlModelMapper
    .LoadAll<SalesOrderDetail>()
    .ToList();

// Create an exporter
var exporter = new TemplateExporter(models);
// Create the model template exporter provider
exporter.Provider = new ModelTemplateExportProvider(models);

// Read the template file
var template = File.ReadAllText("grid.xml");
// Execute export
var xml = exporter.Export(template);
```