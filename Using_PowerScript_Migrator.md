# Using PowerScript Migrator

## Steps to migrate PowerScript to C\#

Step 1: Open your PowerBuilder application in SnapDevelop.

Step 2: Open the PowerScript of the object, locate the function you want to migrate in the code editor, and then right click and select **Translate and Copy to Clipboard** or **Translate and Open in Editor**.

Step 3: Copy the translated C\# code to your C\# project.

## Options for migrating PowerScript to C\#

Select the Tools \| Options menu. In the Options window, select PowerScript Migrator on the left panel, and then configure how you would like to translate the variable-length array and the event.

![](D:\C#_DOC\Tutorials\snapdevelop2019r2beta\images\options_for_migrator.png)

## Rules for migrating PowerScript to C\#

The goal is to translate PowerScript to C\# language which are as native .NET as possible.

Currently, the translation is mainly performed to individual functions, including the common usages of basic data types, DataStores, and embedded SQLs.

C\# language basics: case sensitive, index starts from 0, only one variable can be defined in one line.

### Modifier keywords

Modifier keywords are used to modify declarations of types (class, struct, interface, enum) and type numbers (fields, properties, methods, indexers, ...)

PowerBuilder modifiers will be translated to the C\# modifiers according to the following table.

|  PowerBuilder modifier |  C\# modifier |
|----------------------- |-------------- |
|  Global/ Public        |  public |
|  Protected             |  protected |
|  Private               |  private |
|  Other modifiers       |  internal |

### Structures

PowerBuilder structures are first translated to C\# classes. Users can choose to translate them to C\# structures. The elements of a structure will be translated to C\# properties.

### Objects

All PowerBuilder objects (except Structures) will be translated to C\# classes.

The variables of an object will be translated to C\# fields.

The PowerBuilder variables that are declared in different scopes (Global/Shared/Instance) will be translated to C\# fields according to the following table.

| PowerBuilder variables |  C\# fields |
|-------------------- |---------------------- |
|  Global variables   |  Public static fields |
|  Shared variables   |  Protected fields |
|  Instance variables |  Public fields |

If the modifier is explicitly used, then it will be translated according to table 1.

If the constant modifier is used, it will be translated to C\# const modifier.

另外，若在对象头部进行翻译(包括对象声明/定义、内部对象声明/定义、变量、原型定义)，会将该对象、内部对象、变量全部生成，并且根据原型定义生成一份接口，该功能未完成。

### Events

Events will be translated in two ways:

\#1 (the default): the event that has no parameter and return value will be translated to the C\# event (similar to void xxxHandler(object sender, EventArgs args)).

方案二(可选方案)：全部按照事件进行翻译，若该事件包含参数定义，则生成一个继承 EventArgs的类型，该类型中包含事件的参数定义，该功能未全部完成。

### Functions

Functions will be translated to C\# methods. The global function will be translated to a public static method, as a member of the GlobalFunc class (a public static partial class).

### Statements

Statements will be translated to C\# statements according to the following table.

|  PowerScript statements              |  C\# statements |
|------------------------------------- |------------------- |
|  destroy                             |  Dispose method |
|  if elseif else/oneline if/pound if) |  if elseif else |
|  choose case else                    |  switch case |
|  do loop while/until                 |  do while |
|  do while loop/until                 |  while |
|  for                                 |  for |
|  try catch finally                   |  Try catch finally |
|  label                               |  label |
|  call                                |  (call directly) |
|  goto                                |  goto |
|  exist                               |  break |
|  continue                            |  continue |
|  return                              |  return |
|  create                              |  new |
|  throw                               |  throw |

### Expressions

A local variable will be translated to C\# variable (only one variable can be defined in one line) and the variable will be assigned with an initial value.

Other expressions will be translated according to the following table.

|  Expression Types     |  Examples |
|---------------------- |--------------------------- |
|  Unary operation      |  x++, x\--, +x, -x |
|  Arithmetic operation |  +, -, \*, /, %, \^ |
|  Relational operation |  =, \<\>, \>, \<, \>=, \<= |
|  Logical operation    |  and, or, xor, \>\>, \<\< |
|  Assignment operation |  =, +=, -=, \*=, /=, \^= |
|  Array access         |  xx\[0\] |
|  Member access        |  xx.xx |
|  Array initialization |  {xx, xx} |
|  Type conversion      |  type(expr) |
|  Method/event calls   |  x(), event x() |
|  Literal              |  1, '2', "3" |

### Basic types

Basic data types will be directly translated into the corresponding C\# data types.

PowerBuilder allows the numeric value to be null, therefore, numeric types will be translated to the C\# nullable types.

|  PowerBuilder Types            |  C\# Types |
|---------------------------------- |----------- |
|  integer/int                      |  short? |
|  unsignedinteger/unsignedint/uint |  ushort? |
|  long                             |  int? |
|  Unsignedlong/ulong               |  uint? |
|  longlong                         |  long? |
|  byte                             |  byte? |
|  char/character                   |  char? |
|  boolean                          |  bool? |
|  dec/decimal/dec{x}/decimal{x}    |  decimal? |
|  real                             |  float? |
|  double                           |  double? |
|  date                             |  DateTime? |
|  time                             |  TimeSpan? |
|  datetime                         |  DateTime? |
|  string                           |  string |
|  any                              |  object |
|  blob                             |  PbBlob |

The PowerScript syntax for date types will be translated to the C\# ParseExact method of the corresponding type. For example,

ld\_41 = 2003-03-03 // lt\_41 = TimeSpan.ParseExact(\"23:23:23.230\", \"g\", null);

lt\_21 = 12:12:12 // ld\_41 = DateTime.ParseExact(\"2003-03-03\", \"yyyy-MM-dd\", null);

Arrays will be translated in two ways:

\#1 (the default): Fixed-length arrays will be translated to C\# array, and variable-length arrays will be translated to PbArray. PbArray is a custom array type defined in PowerScript.Bridge.

\#2: All arrays will be translated to PbArray.

创建默认的定长数组会使用 ArrayEx.CreateInstance方法以处理可空类型默认值问题； .NET 数组的初始化会全部转换为 .NET 的规范(如：二维数组横向赋值顺序、缺失的数据自动补全，丢弃多余的数据)；所有数组之间的相互赋值统一使用 ArrayEx.Assign 方法。

暂不支持维度大于2的数组的翻译。3- or more-dimensional arrays are unsupported currently.

Transaction type will be translated to DataContext of the runtime APIs, the default type is SqlServerDataContext. Users can configure the default type to be others, such as OracleDataContext, PostgreSqlDataContext, OdbcDataContext, etc.

PowerObject type will be translated to C\# object type.

Exception type will be translated to C\# Exception type, and GetMessage method and Text property will be translated to Message property; SetMessage method will be translated to the Exception object which is re-instantiated.

PowerScript:

```C#
Exception lex\_delete
lex\_delete = create Exception
lex\_delete.SetMessage(\"Nothing deleted.\")
```

C\# scripts:

```C#
Exception lex\_delete = null;
lex\_delete = new Exception();
lex\_delete= new Exception(\"Nothing deleted.\");
```

### Enumeration

PowerBuilder dwitemstatus enumerated values will be translated to C\# PropertyState enumerated values, as shown in the following table.

|  PB dwitemstatus enumerated value |  C\# PropertyState enumerated value |
|---------------------------------- |------------------------------------ |
|  new!                             |  NotModified |
|  notmodified!                     |  NotModified |
|  datamodified!                    |  Modified |
|  newmodified!                     |  Modified |

PowerBuilder dwbuffer enumerated values will be translated to C\# DwBuffer enumerated values, as shown in the following table.

|  PB dwbuffer enumerated value |  C\# DwBuffer enumerated value |
|------------------------------ |------------------------------- |
|  primary!                     |  Primary |
|  delete!                      |  Delete |
|  filter!                      |  Filter |

PowerBuilder parmtype enumerated values will be translated to C\# PbDataType enumerated values, as shown in the following table.

|  PB parmtype enumerated value |  C\# PbDataType enumerated value |
|------------------------------ |--------------------------------- |
|  typeboolean!                 |  Boolean |
|  typebyte!                    |  Byte |
|  typedate!                    |  Date |
|  typedatetime!                |  DateTime |
|  typedecimal!                 |  Decimal |
|  typedouble!                  |  Double |
|  typeinteger!                 |  Int |
|  typelong!                    |  Long |
|  typelonglong!                |  LongLong |
|  typereal!                    |  Real |
|  typestring!                  |  String |
|  typetime!                    |  Time |
|  typeuint!                    |  UInt |
|  typeulong!                   |  ULong |
|  typeunknown!                 |  Unknown |

PowerBuilder saveastype enumerated values will be translated to C\# DataFormat enumerated values, as shown in the following table.

|  PB saveastype enumerated value |  C\# DataFormat enumerated value |
|-------------------------------- |--------------------------------- |
|  xml!                           |  Xml |
|  text!                          |  Text |
|  excel!                         |  Text |
|  csv!                           |  Text |
|  sylk!                          |  Text |
|  wks!                           |  Text |
|  wk1!                           |  Text |
|  dif!                           |  Text |
|  dbase2!                        |  Text |
|  dbase3!                        |  Text |
|  sqlinsert!                     |  Text |
|  clipboard!                     |  Text |
|  psreport!                      |  Text |
|  wmf!                           |  Text |
|  htmltable!                     |  Text |
|  excel5!                        |  Text |
|  xslfo!                         |  Text |
|  pdf!                           |  Text |
|  excel8!                        |  Text |
|  emf!                           |  Text |
|  xlsx!                          |  Text |
|  xlsb!                          |  Text |

PowerBuilder encoding enumerated values will be translated to C\# Encoding enumerated values, as shown in the following table.

|  PB encoding enumerated value |  C\# Encoding enumerated value |
|------------------------------ |------------------------------- |
|  encodingansi!                |  Default |
|  encodingutf8!                |  UTF8 |
|  encodingutf16le!             |  Unicode |
|  encodingutf16be!             |  BigEndianUnicode |

### DataStores

DataStore events will be translated to the C\# DataStore events according to the following table.

| PB DataStore events |  C\# DataStore events |  Remarks |
|-------------------- |---------------------- |----------------------------- |
|  retrievestart      |  RetrieveStart        |   |
|  retrieveend        |  RetrieveEnd          |   |
|  updatestart        |  UpdateStart          |   |
|  updateend          |  UpdateEnd            |   |
|  constructor        |  Constructor          |  Unsupported by runtime APIs |
|  dberror            |  Dberror              |  Unsupported by runtime APIs |
|  destructor         |  Destructor           |  Unsupported by runtime APIs |
|  itemchanged        |  ItemChanged          |  Unsupported by runtime APIs |
|  itemerror          |  ItemError            |  Unsupported by runtime APIs |
|  printend           |  PrintEnd             |  Unsupported by runtime APIs |
|  printpage          |  PrintPage            |  Unsupported by runtime APIs |
|  printstart         |  PrintStart           |  Unsupported by runtime APIs |
| retrieverow         | RetrieveRow           | Unsupported by runtime APIs |
|  sqlpreview         |  SqlPreview           |  Unsupported by runtime APIs |

DataStore properties will be translated to the C\# DataStore properties according to the following table.

|  PB DataStore properties |  C\# DataStore properties |  Remarks |
|------------------------- |-------------------------- |----------------------------- |
|  dataobject              |  DataObject               |
|  classdefinition         |  ClassDefinition          |  Unsupported by runtime APIs |
|  object                  |  Object                   |  Unsupported by runtime APIs |

DataStore functions will be translated to the C\# DataStore properties/methods according to the following table.

|  PB DataStore functions |  C\# DataStore properties/methods |  Remarks |
|------------------------ |---------------------------------- |----------------------------- |
|  retrieve               |  Retrieve                         |    |
|  update                 |  Update                           |    |
|  reselectrow            |  ReselectRow                      |    |
|  find                   |  FindIndex                        |    |
|  getchild               |  GetChild                         |    |
|  getitemdate            |  GetItemDate                      |    |
|  getitemdatetime        |  GetItemDateTime                  |    |
|  getitemdecimal         |  GetItemDecimal                   |    |
|  getitemstatus          |  GetItemStatus                    |    |
|  getitemstring          |  GetItemString                    |    |
|  getitemtime            |  GetItemTime                      |   |
|  getrowfromrowid        |  GetRowFromRowId                  |   |
|  getrowidfromrow        |  GetRowIdFromRow                  |   |
|  insertrow              |  InsertRow                        |   |
|  getsqlselect           |  GetSqlSelect                     |   |
|  getvalidate            |  GetValidate                      |   |
|  getitemnumber          |  GetItemNumber                    |   |
|  setitem                |  SetItem                          |   |
|  setsort                |  SetSort                          |   |
|  setfilter              |  SetFilter                        |   |
|  sort                   |  Sort                             |   |
|  rowsmove               |  RowsMove                         |   |
|  rowsdiscard            |  RowsDiscard                      |   |
|  rowscopy               |  RowsCopy                         |   |
|  resetupdate            |  ResetUpdate                      |   |
|  setsqlselect           |  SetSqlSelect                     |   |
|  settrans               |  SetDataContext                   |   |
|  settransobject         |  SetDataContext                   |   |
|  filter                 |  Filter                           |   |
|  deleterow              |  DeleteRow                        |   |
|  setitemstatus          |  SetItemStatus                    |   |
|  setvalidate            |  SetValidate                      |   |
|  setvalue               |  SetItem                          |   |
|  getvalue               |  GetItem                          |   |
|  setsqlpreview          |  SetSqlSelect                     |   |
|  reset                  |  Clear                            |   |
|  classname              |  ClassName                        |   |
|  exportjson             |  ExportJson                       |   |
|  exportrowasjson        |  ExportRowAsJson                  |   |
|  importjson             |  ImportJson                       |   |
|  importjsonbykey        |  ImportJsonByKey                  |   |
|  importrowfromjson      |  ImportRowFromJson                |   |
|  importstring           |  ImportString                     |   |
|  importfile             |  ImportFile                       |   |
|  getnextmodified        |  GetNextModified                  |   |
|  reset                  |  Clear                            |   |
|  classname              |  ClassName                        |   |
|  exportjson             |  ExportJson                       |   |
|  exportrowasjson        |  ExportRowAsJson                  |   |
|  importjson             |  ImportJson                       |   |
|  importjsonbykey        |  ImportJsonByKey                  |   |
|  importrowfromjson      |  ImportRowFromJson                |   |
|  importstring           |  ImportString                     |   |
|  importfile             |  ImportFile                       |   |
|  getnextmodified        |  GetNextModified                  |   |
|  rowcount               |  RowCount                         |  Translated as a property |
|  deletedcount           |  DeletedCount                     |  Translated as a property |
|  modifiedcount          |  ModifiedCount                    |  Translated as a property |
|  filteredcount          |  FilteredCount                    |  Translated as a property |
|  saveas                 |  SaveAs                           |  Unsupported by runtime APIs |
|  getrow                 |  GetRow                           |  Unsupported by runtime APIs |
|  getselectedrow         |  GetSelectedRow                   |  Unsupported by runtime APIs |
|  gettext                |  GetText                          |  Unsupported by runtime APIs |
|  gettrans               |  GetTrans                         |  Unsupported by runtime APIs |
|  selectrow              |  SelectRow                        |  Unsupported by runtime APIs |
|  settext                |  SetText                          |  Unsupported by runtime APIs |
|  datacount              |  DataCount                        |  Unsupported by runtime APIs |
|  getcolumn              |  GetColumn                        |  Unsupported by runtime APIs |
|  getcolumnname          |  GetColumnName                    |  Unsupported by runtime APIs |
|  getclickedcolumn       |  GetClickedColumn                 |  Unsupported by runtime APIs |
|  getclickedrow          |  GetClickedRow                    |  Unsupported by runtime APIs |
|  accepttext             |  AcceptText                       |  Unsupported by runtime APIs |
|  categorycount          |  CategoryCount                    |  Unsupported by runtime APIs |
|  categoryname           |  CategoryName                     |  Unsupported by runtime APIs |
|  clearvalues            |  ClearValues                      |  Unsupported by runtime APIs |
|  clipboard              |  Clipboard                        |  Unsupported by runtime APIs |
|  copyrtf                |  CopyRTF                          |  Unsupported by runtime APIs |
|  create                 |  Create                           |  Unsupported by runtime APIs |
|  createfrom             |  CreateFrom                       |  Unsupported by runtime APIs |
|  dbcancel               |  DBCancel                         |  Unsupported by runtime APIs |
|  describe               |  Describe                         |  Unsupported by runtime APIs |
|  findcategory           |  FindCategory                     |  Unsupported by runtime APIs |
|  findgroupchange        |  FindGroupChange                  |  Unsupported by runtime APIs |
|  findrequired           |  FindRequired                     |  Unsupported by runtime APIs |
|  findseries             |  FindSeries                       |  Unsupported by runtime APIs |
|  generatehtmlform       |  GenerateHTMLForm                 |  Unsupported by runtime APIs |
|  generateresultset      |  GenerateResultSet                |  Unsupported by runtime APIs |
|  obsoletefunction       |  Obsoletefunction                 |  Unsupported by runtime APIs |
|  getborderstyle         |  GetBorderStyle                   |  Unsupported by runtime APIs |
|  getchanges             |  GetChanges                       |  Unsupported by runtime APIs |
|  getcontextservice      |  GetContextService                |  Unsupported by runtime APIs |
|  getdata                |  GetData                          |  Unsupported by runtime APIs |
|  getdatapieexplode      |  GetDataPieExplode                |  Unsupported by runtime APIs |
|  getdatastyle           |  GetDataStyle                     |  Unsupported by runtime APIs |
|  getdatavalue           |  GetDataValue                     |  Unsupported by runtime APIs |
|  getformat              |  GetFormat                        |  Unsupported by runtime APIs |
|  getfullstate           |  GetFullState                     |  Unsupported by runtime APIs |
|  getparent              |  GetParent                        |  Unsupported by runtime APIs |
|  getseriesstyle         |  GetSeriesStyle                   |  Unsupported by runtime APIs |
|  getstatestatus         |  GetStateStatus                   |  Unsupported by runtime APIs |
|  groupcalc              |  GroupCalc                        |  Unsupported by runtime APIs |
|  importclipboard        |  ImportClipboard                  |  Unsupported by runtime APIs |
|  insertdocument         |  InsertDocument                   |  Unsupported by runtime APIs |
|  modify                 |  Modify                           |  Unsupported by runtime APIs |
|  pastertf               |  PasteRTF                         |  Unsupported by runtime APIs |
|  postevent              |  PostEvent                        |  Unsupported by runtime APIs |
|  print                  |  Print                            |  Unsupported by runtime APIs |
|  printcancel            |  PrintCancel                      |  Unsupported by runtime APIs |
|  resetdatacolors        |  ResetDataColors                  |  Unsupported by runtime APIs |
|  resettransobject       |  ResetTransObject                 |  Unsupported by runtime APIs |
|  saveasascii            |  SaveAsAscii                      |  Unsupported by runtime APIs |
|  savenativepdftoblob    |  SaveNativePDFToBlob              |  Unsupported by runtime APIs |
|  seriescount            |  SeriesCount                      |  Unsupported by runtime APIs |
|  seriesname             |  SeriesName                       |  Unsupported by runtime APIs |
|  setborderstyle         |  SetBorderStyle                   |  Unsupported by runtime APIs |
|  setchanges             |  SetChanges                       |  Unsupported by runtime APIs |
|  setcolumn              |  SetColumn                        |  Unsupported by runtime APIs |
|  setdatapieexplode      |  SetDataPieExplode                |  Unsupported by runtime APIs |
|  setdatastyle           |  SetDataStyle                     |  Unsupported by runtime APIs |
|  setdetailheight        |  SetDetailHeight                  |  Unsupported by runtime APIs |
|  setformat              |  SetFormat                        |  Unsupported by runtime APIs |
|  setfullstate           |  SetFullState                     |  Unsupported by runtime APIs |
|  sethtmlaction          |  SetHTMLAction                    |  Unsupported by runtime APIs |
|  setposition            |  SetPosition                      |  Unsupported by runtime APIs |
|  setrow                 |  SetRow                           |  Unsupported by runtime APIs |
|  setseriesstyle         |  SetSeriesStyle                   |  Unsupported by runtime APIs |
|  setwsobject            |  SetWSObject                      |  Unsupported by runtime APIs |
|  sharedata              |  ShareData                        |  Unsupported by runtime APIs |
|  sharedataoff           |  ShareDataOff                     |  Unsupported by runtime APIs |
|  triggerevent           |  TriggerEvent                     |  Unsupported by runtime APIs |
|  typeof                 |  TypeOf                           |  Unsupported by runtime APIs |

When translating the scripts, the migrator will detect the objects that are derived from DataStore, and prompt the user whether to translate these objects as DataStores.

翻译代码时，会推测出疑似用户自定义的DataStore的派生对象，并提示给用户，用户可选择按照DataStore的方式翻译该对象。

### Embedded SQLs

暂无
