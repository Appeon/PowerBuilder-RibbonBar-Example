Scaffolding
===========

Scaffolding is a technique for creating database-backed applications. It can be used to easily and quickly generate services and controllers in a project based on existing items and thus enjoys a variety of benefits:

-   Collaboration friendliness;

-   Less repetitive work;

-   Enhanced development efficiency (a developer doesn’t have to build a project from scratch).

Currently SnapDevelop supports you to scaffold controllers or services based on **.NET DataStore** or **SqlModelMapper** models. This tutorial walks you through the process of scaffolding services or controllers in projects in SnapDevelop. You will learn how to access the scaffolding functionality, configure various scaffolding settings, and then generate scaffolded items.

Accessing Scaffolding Functionality
-----------------------------------

If your project is a Web project using **.NET DataStore**, you will see the **Controllers-DataWindows-Services** folders in **Solution Explorer**. You can select the **DataWindows** (instead of **Services** or **Controllers**) folder first, and then **Scaffold** from the right-click context menu.

!![](images/scaffolding/image1.png)

If your project is a Web project using **SqlModelMapper**, you will see the **Controllers-Models-Services** folders in **Solution Explorer**. You can select the **Models** folder first, and then **Scaffold** from the right-click context menu.

There are four scaffolding options under **Scaffolding**: **Controller from Service (DataStore)**, **Service and Controller from Model (DataStore)**, **Controller from Service (SqlModelMapper)**, and **Service and Controller from Model (SqlModelMapper)**. The following table explains what each option represents.

| **Option**                                         | **Description**                                              |
| -------------------------------------------------- | ------------------------------------------------------------ |
| Controller from Service (DataStore)                | Indicates that you can generate controllers from services that operate on DataStore models. |
| Service and Controller from Model (DataStore)      | Indicates that you can generate services and controllers that operate on DataStore models. |
| Controller from Service (SqlModelMapper)           | Indicates that you can generate controllers from services that operate on SqlModelMapper models. |
| Service and Controller from Model (SqlModelMapper) | Indicates that you can generate services and controllers that operate on SqlModelMapper models. |

### Note

It is important to note that currently SnapDevelop would provide all the scaffolding options regardless which item you right-mouse click on. You need to ensure by yourself that you choose the appropriate items to scaffold on. Chances are that you can use the wrong option to generate scaffolded items (service and/or controller) in the target project but such items would fail to work.

Specifying Scaffolding Templates
--------------------------------

### Selecting Template for Generating Service

If you want to generate services via the scaffolding feature, make sure to select the **Generate Service** check box. You should then select a template from the **Template** drop-down list. To understand the template options, refer to [Managing Scaffolding Templates](#managing-scaffolding-templates).

### Selecting Template for Generating Controller

If you want to generate services via the scaffolding feature, make sure to select the **Generate Service** check box. You should then select a template from the **Template** drop-down list. To understand the template options, refer to [Managing Scaffolding Templates](#managing-scaffolding-templates).

### Managing Scaffolding Templates

Scaffolding templates are essential for the scaffolding feature. There are eight system scaffolding templates to choose from in the current version. The source files of the templates are stored in the SnapDevelop installation directory: *SnapDevelop 2019 R2\\modules\\SnapDevelop.Scaffolding\\Templates*.

#### Viewing and Editing Scaffolding Templates

Sometimes you may want to view or edit a particular scaffolding template. You just need to find the file location of the scaffolding templates and then view and edit it.

For example, the folder *SnapDevelop 2019 R2\\modules\\SnapDevelop.Scaffolding\\Templates\\DataStore* contains the source files of the templates on DataStore models.

![](images/scaffolding/image2.png)

Each \*.template file in the folder refers to the configuration of a template. For example, ModelToService.template.

![](images/scaffolding/image3.png)

In the file:

-   **ID**: The template ID. It must be unique for each template.

-   **Name**: The name option to represent the template in the Scaffolding window and Scaffolding option settings.

-   **Template Items**: The actual template to be used. Note that the file and folder are specified with the relative path of the current template directory.

-   **Package**: The NuGet package that must be installed for the item generated based on the template to work properly.

-   **ParsingRules**: The parsing rules used for parsing the source items.

It is recommended that you do not change the configuration and design of system templates. You may add a custom scaffolding template based on a system template, and then edit its configuration or design according to what you want to implement with the template.

#### Adding Scaffolding Template

If you think that the existing templates cannot meet your needs, you can create new templates on your own. If you want to add a new scaffolding template, it is recommended that you to create it in the **Scaffolding Templates** folder in the default directory *C:\\Users\\[appeon]\\.sd\\1.0.0* (you may need to replace *[appeon]* with your user name). You may copy the configuration and source file of an existing template and then build your own based on it.

Specifying Scaffolding Source and Target Items
----------------------------------------------

Before you can specify the scaffolding source, you need to configure the parsing rules. The parsing rules filter out what source items you can select from.

![](images/scaffolding/image4.png)

### Configuring Parsing Rules

Select **Parsing Rules** to go to the **Parsing Rules** configuration page.

![](images/scaffolding/image5.png)

**When Scaffolding Based on Template**

You need to select a scaffolding template first, and then specify classes and class attributes to be parsed. The configured parsing rule will only work for the selected template. You can specify different parsing rules for different items.

Note: You can only view, but not edit, the parsing rules of system templates.

**Specify Classes and Class Attributes to be Parsed**

After you have selected the scaffolding template, you need to specify classes and class attributes to be parsed.

1.  **Get classes by keyword** means that the source items to be parsed shall be **Interface** or **Class**.

2.  **Class(es)/Class attribute(s):** If you want to parse all classes and/or class attributes in your program, you can select the **Class** check box and then the **Class attribute(s)** check box, and leave the input box empty. If you just want to parse some of the classes and/or class attributes in your program, you can select the **Class** check box and then the **Class attribute(s)** check box, and specify the class(es)/class attribute(s) in the input box. If you select neither the **Class(es)** nor the **Class attribute(s)** check box, no class or class attribute will be parsed.

### Selecting Scaffolding Source and Target Items

After you have properly configured the parsing rules, all the source items (be it model or service) will be filtered out and listed. You can then select the ones you want to scaffold on.

![](images/scaffolding/image6.png)

Each selected source item has a corresponding target item, which is the item to generate based on the source item. Each target item (be it service or controller) has a default name. You can click on the default name to modify it if you want to name the target item differently.

Generating Scaffolded Items
---------------------------

You can configure the **Generate Scaffolded Items** settings so as to generate the scaffolded items in the intended way.

### Settings for Target Service

If you want to generate a service in a project, you need to specify the target project, the particular folder in the target project, the DataContext on which your program depends, as well as the namespace in the *Service.cs* file in the generated **Service** folder.

#### Project

Selects from the dropdown list the project in which you want to generate the target service.

![](images/scaffolding/image7.png)

#### Subfolder

Specifies the folder that belongs to the selected project and in which you want to generate the target service. You can select from the dropdown list or choose **New Folder** to create a new folder.

![](images/scaffolding/image8.png)

#### DataContext

Specifies the data source that the target service will use. You can select from the dropdown list or choose **New DataContext** to create a new one. After you create a new DataContext, you should click the Refresh icon to update the dropdown list with the newly created DataContext.

![](images/scaffolding/image9.png)

#### Namespace

Designates the namespace in the target service.

### Settings for Target Controller

If you want to generate a controller in a project, you need to specify the target project, the particular folder in the target project, as well as the
namespace in the *Controller.cs* file in the generated **Controller** folder.

#### Project

Selects from the dropdown list the project in which you want to generate the target controller.

![](images/scaffolding/image10.png)

#### Subfolder

Specifies the folder that belongs to the selected project and in which you want to generate the target controller. You can select from the dropdown list or choose **New Folder** to create a new folder.

![](images/scaffolding/image11.png)

#### Namespace

Designates the namespace in the target service.

#### Setting How to Add Method Attributes

This setting allows you to define a set of method attributes for the methods in the controller. For each listed method attribute, specify the method name to which you want to add the method attribute.

![](images/scaffolding/image12.png)

Note that if a method is not covered in the setting, scaffolding will decide which method attribute to add for it by checking the parameter of the method:

-   If the method has no parameter, the [HttpGet] method attribute will be added to it by default;

-   If the method contains one or more parameters with a non-basic C\# type, the [HttpPost] method attribute will be added to it by default.

#### If Files Already Exist

This setting determines what to do if the files to add for the scaffolded items already exist in the target folder. You can select **Overwrite** to replace the existing files with the newly generated files, or **Skip** to retain the existing files.

### Executing the Scaffolding Operation

When you have properly configured scaffolding settings, then you can click **OK** to export the scaffolded items to the designated folder in the target
project, as specified in [Generating Scaffolded Items](#generating-scaffolded-items).

During the scaffolding process, you may be prompted whether to install the NuGet package required by the scaffolded item(s). You can select to install it right away, or install it manually later.

![](images/scaffolding/image13.png)

After the scaffolding process is completed, you can check the corresponding folder in the target project to see if the scaffolded items are generated successfully.

![](images/scaffolding/image14.png)