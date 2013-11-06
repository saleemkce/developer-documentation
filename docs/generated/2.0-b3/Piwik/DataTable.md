<small>Piwik</small>

DataTable
=========

The primary data structure used to store analytics data in Piwik.

Description
-----------

<a name="class-desc-the-basics"></a>
### The Basics

DataTables consist of rows and each row consists of columns. A column value can be
be a numeric, string or array.

Every row has an ID. The ID is either the index of the row or [ID_SUMMARY_ROW](#ID_SUMMARY_ROW).

DataTables are hierarchical data structures. Each row can also contain an additional
nested sub-DataTable (commonly referred to as a 'subtable').

Both DataTables and DataTable rows can hold **metadata**. _DataTable metadata_ is information
regarding all the data, such as the site or period that the data is for. _Row metadata_
is information regarding that row, such as a browser logo or website URL.

Finally, DataTables all contain a special _summary_ row. This row, if it exists, is
always at the end of the DataTable.

### Populating DataTables

Data can be added to DataTables in a couple different ways. You can either:

1. create rows one by one and add them through [addRow](#addRow) then truncate if desired,
2. create an array of DataTable\Row instances or an array of arrays and add them using
   [addRowsFromArray](#addRowsFromArray) or [addRowsFromSimpleArray](#addRowsFromSimpleArray)
   then truncate if desired,
3. or set the maximum number of allowed rows (with [setMaximumAllowedRows](#setMaximumAllowedRows))
   and add rows one by one.

If you want to eventually truncate your data (standard practice for all Piwik plugins),
the third method is the most memory efficient. It is, unfortunately, not always possible
to use since it requires that the data be sorted before adding.

### Manipulating DataTables

There are two main ways to manipulate a DataTable. You can either:

1. manually iterate through each row and manipulate the data,
2. or you can use predefined Filters.

A Filter is a class that has a 'filter' method which will manipulate a DataTable in
some way. There are several predefined Filters that allow you to do common things,
such as,

- add a new column to each row,
- add new metadata to each row,
- modify an existing column value for each row,
- sort an entire DataTable,
- and more.

Using these Filters instead of writing your own code will increase code clarity and
reduce code redundancy. Additionally, Filters have the advantage that they can be
applied to DataTable\Map instances. So you can visit every DataTable in a DataTable\Map
without having to write a recursive visiting function.

Note: Anonymous functions can be used as DataTable Filters.

### Applying Filters

Filters can be applied now (via [filter](#filter)), or they can be applied later (via
[queueFilter](#queueFilter)).

Filters that sort rows or manipulate the number of rows should be applied right away.
Non-essential, presentation filters should be queued.

### Learn more

- **ArchiveProcessor** &mdash; to learn how DataTables are persisted.
- **DataTable\Renderer** &mdash; to learn how DataTable data is exported to XML, JSON, etc.
- **DataTable\Filter** &mdash; to see all core Filters.

### Examples

**Populating a DataTable**

    // adding one row at a time
    $dataTable = new DataTable();
    $dataTable->addRow(new Row(array(
        Row::COLUMNS => array('label' => 'thing1', 'nb_visits' => 1, 'nb_actions' => 1),
        Row::METADATA => array('url' => 'http://thing1.com')
    )));
    $dataTable->addRow(new Row(array(
        Row::COLUMNS => array('label' => 'thing2', 'nb_visits' => 2, 'nb_actions' => 2),
        Row::METADATA => array('url' => 'http://thing2.com')
    )));
    
    // using an array of rows
    $dataTable = new DataTable();
    $dataTable->addRowsFromArray(array(
        array(
            Row::COLUMNS => array('label' => 'thing1', 'nb_visits' => 1, 'nb_actions' => 1),
            Row::METADATA => array('url' => 'http://thing1.com')
        ),
        array(
            Row::COLUMNS => array('label' => 'thing2', 'nb_visits' => 2, 'nb_actions' => 2),
            Row::METADATA => array('url' => 'http://thing2.com')
        )
    ));

    // using a "simple" array
    $dataTable->addRowsFromSimpleArray(array(
        array('label' => 'thing1', 'nb_visits' => 1, 'nb_actions' => 1),
        array('label' => 'thing2', 'nb_visits' => 2, 'nb_actions' => 2)
    ));

**Getting & setting metadata**

    $dataTable = \Piwik\Plugins\Referrers\API::getInstance()->getSearchEngines($idSite = 1, $period = 'day', $date = '2007-07-24');
    $oldPeriod = $dataTable->metadata['period'];
    $dataTable->metadata['period'] = Period::factory('week', Date::factory('2013-10-18'));

**Serializing & unserializing**

    $maxRowsInTable = Config::getInstance()->General['datatable_archiving_maximum_rows_standard'];j
    
    $dataTable = // ... build by aggregating visits ...
    $serializedData = $dataTable->getSerialized($maxRowsInTable, $maxRowsInSubtable = $maxRowsInTable,
                                                $columnToSortBy = Metrics::INDEX_NB_VISITS);
    
    $serializedDataTable = $serializedData[0];
    $serailizedSubTable = $serializedData[$idSubtable];

**Filtering for an API method**

    public function getMyReport($idSite, $period, $date, $segment = false, $expanded = false)
    {
        $dataTable = Archive::getDataTableFromArchive('MyPlugin_MyReport', $idSite, $period, $date, $segment, $expanded);
        $dataTable->filter('Sort', array(Metrics::INDEX_NB_VISITS, 'desc', $naturalSort = false, $expanded));
        $dataTable->queueFilter('ReplaceColumnNames');
        $dataTable->queueFilter('ColumnCallbackAddMetadata', array('label', 'url', __NAMESPACE__ . '\getUrlFromLabelForMyReport'));
        return $dataTable;
    }


Constants
---------

This class defines the following constants:

- `MAX_DEPTH_DEFAULT`
- `ARCHIVED_DATE_METADATA_NAME` &mdash; Name for metadata that describes when a report was archived.
- `EMPTY_COLUMNS_METADATA_NAME` &mdash; Name for metadata that describes which columns are empty and should not be shown.
- `TOTAL_ROWS_BEFORE_LIMIT_METADATA_NAME` &mdash; Name for metadata that describes the number of rows that existed before the Limit filter was applied.
- [`COLUMN_AGGREGATION_OPS_METADATA_NAME`](#column_aggregation_ops_metadata_name) &mdash; Name for metadata that describes how individual columns should be aggregated when [addDataTable](#addDataTable) or [DataTable\Row::sumRow](#) is called.
- `ID_SUMMARY_ROW` &mdash; The ID of the Summary Row.
- `LABEL_SUMMARY_ROW` &mdash; The original label of the Summary Row.

<a name="column_aggregation_ops_metadata_name" id="column_aggregation_ops_metadata_name"></a>
### `COLUMN_AGGREGATION_OPS_METADATA_NAME`

This metadata value must be an array that maps column names with valid operations. Valid aggregation operations are:

- `'skip'`: do nothing
- `'max'`: does `max($column1, $column2)`
- `'min'`: does `min($column1, $column2)`
- `'sum'`: does `$column1 + $column2`

See [addDataTable](#addDataTable) and [DataTable\Row::sumRow](#) for more information.

Properties
----------

This class defines the following properties:

- [`$rows`](#$rows) &mdash; Array of Row
- [`$currentId`](#$currentid) &mdash; Id assigned to the DataTable, used to lookup the table using the DataTable_Manager
- [`$depthLevel`](#$depthlevel) &mdash; Current depth level of this data table 0 is the parent data table
- [`$indexNotUpToDate`](#$indexnotuptodate) &mdash; This flag is set to false once we modify the table in a way that outdates the index
- [`$rebuildIndexContinuously`](#$rebuildindexcontinuously) &mdash; This flag sets the index to be rebuild whenever a new row is added, as opposed to re-building the full index when getRowFromLabel is called.
- [`$tableSortedBy`](#$tablesortedby) &mdash; Column name of last time the table was sorted
- [`$queuedFilters`](#$queuedfilters) &mdash; List of Filter queued to this table
- [`$rowsCountBeforeLimitFilter`](#$rowscountbeforelimitfilter) &mdash; We keep track of the number of rows before applying the LIMIT filter that deletes some rows
- [`$enableRecursiveSort`](#$enablerecursivesort) &mdash; Defaults to false for performance reasons (most of the time we don't need recursive sorting so we save a looping over the dataTable)
- [`$enableRecursiveFilters`](#$enablerecursivefilters) &mdash; When the table and all subtables are loaded, this flag will be set to true to ensure filters are applied to all subtables
- [`$rowsIndexByLabel`](#$rowsindexbylabel)
- [`$summaryRow`](#$summaryrow)
- [`$maximumAllowedRows`](#$maximumallowedrows) &mdash; Maximum number of rows allowed in this datatable (including the summary row).

<a name="rows" id="rows"></a>
### `$rows`

Array of Row

#### Signature

- It is a(n) [`Row[]`](../Piwik/DataTable/Row.md) value.

<a name="currentid" id="currentid"></a>
### `$currentId`

Id assigned to the DataTable, used to lookup the table using the DataTable_Manager

#### Signature

- It is a(n) `int` value.

<a name="depthlevel" id="depthlevel"></a>
### `$depthLevel`

Current depth level of this data table 0 is the parent data table

#### Signature

- It is a(n) `int` value.

<a name="indexnotuptodate" id="indexnotuptodate"></a>
### `$indexNotUpToDate`

This flag is set to false once we modify the table in a way that outdates the index

#### Signature

- It is a(n) `bool` value.

<a name="rebuildindexcontinuously" id="rebuildindexcontinuously"></a>
### `$rebuildIndexContinuously`

This flag sets the index to be rebuild whenever a new row is added, as opposed to re-building the full index when getRowFromLabel is called.

#### Description

This is to optimize and not rebuild the full Index in the case where we
add row, getRowFromLabel, addRow, getRowFromLabel thousands of times.

#### Signature

- It is a(n) `bool` value.

<a name="tablesortedby" id="tablesortedby"></a>
### `$tableSortedBy`

Column name of last time the table was sorted

#### Signature

- It is a(n) `string` value.

<a name="queuedfilters" id="queuedfilters"></a>
### `$queuedFilters`

List of Filter queued to this table

#### Signature

- It is a(n) `array` value.

<a name="rowscountbeforelimitfilter" id="rowscountbeforelimitfilter"></a>
### `$rowsCountBeforeLimitFilter`

We keep track of the number of rows before applying the LIMIT filter that deletes some rows

#### Signature

- It is a(n) `int` value.

<a name="enablerecursivesort" id="enablerecursivesort"></a>
### `$enableRecursiveSort`

Defaults to false for performance reasons (most of the time we don't need recursive sorting so we save a looping over the dataTable)

#### Signature

- It is a(n) `bool` value.

<a name="enablerecursivefilters" id="enablerecursivefilters"></a>
### `$enableRecursiveFilters`

When the table and all subtables are loaded, this flag will be set to true to ensure filters are applied to all subtables

#### Signature

- It is a(n) `bool` value.

<a name="rowsindexbylabel" id="rowsindexbylabel"></a>
### `$rowsIndexByLabel`

#### Signature

- It is a(n) `array` value.

<a name="summaryrow" id="summaryrow"></a>
### `$summaryRow`

#### Signature

- It is a(n) [`Row`](../Piwik/DataTable/Row.md) value.

<a name="maximumallowedrows" id="maximumallowedrows"></a>
### `$maximumAllowedRows`

Maximum number of rows allowed in this datatable (including the summary row).

#### Description

If adding more rows is attempted, the extra rows get summed to the summary row.

#### Signature

- It is a(n) `int` value.

Methods
-------

The class defines the following methods:

- [`__construct()`](#__construct) &mdash; Constructor.
- [`__destruct()`](#__destruct) &mdash; Destructor.
- [`sort()`](#sort) &mdash; Sorts the DataTable rows using the supplied callback function.
- [`getSortedByColumnName()`](#getsortedbycolumnname) &mdash; Returns the name of the column this table was sorted by (if any).
- [`enableRecursiveSort()`](#enablerecursivesort) &mdash; Enables recursive sorting.
- [`enableRecursiveFilters()`](#enablerecursivefilters) &mdash; Enables recursive filtering.
- [`filter()`](#filter) &mdash; Applies filter to this datatable.
- [`queueFilter()`](#queuefilter) &mdash; Adds a filter and a list of parameters to the list of queued filters.
- [`applyQueuedFilters()`](#applyqueuedfilters) &mdash; Applies all filters that were previously queued to the table.
- [`addDataTable()`](#adddatatable) &mdash; Sums a DataTable to this one.
- [`getRowFromLabel()`](#getrowfromlabel) &mdash; Returns the Row whose `'label'` column is equal to `$label`.
- [`getRowIdFromLabel()`](#getrowidfromlabel) &mdash; Returns the row id for the row whose `'label'` column is equal to `$label`.
- [`getEmptyClone()`](#getemptyclone) &mdash; Returns an empty DataTable with the same metadata and queued filters as `$this` one.
- [`getRowFromId()`](#getrowfromid) &mdash; Returns a row by ID.
- [`getRowFromIdSubDataTable()`](#getrowfromidsubdatatable) &mdash; Returns the row that has a subtable with ID matching `$idSubtable`.
- [`addRow()`](#addrow) &mdash; Adds a row to this table.
- [`addSummaryRow()`](#addsummaryrow) &mdash; Sets the summary row.
- [`getId()`](#getid) &mdash; Returns the DataTable ID.
- [`addRowFromArray()`](#addrowfromarray) &mdash; Adds a new row from an array.
- [`addRowFromSimpleArray()`](#addrowfromsimplearray) &mdash; Adds a new row a from an array of column values.
- [`getRows()`](#getrows) &mdash; Returns the array of Rows.
- [`getColumn()`](#getcolumn) &mdash; Returns an array containing all column values for the requested column.
- [`getColumnsStartingWith()`](#getcolumnsstartingwith) &mdash; Returns an array containing all column values of columns whose name starts with `$name`.
- [`getColumns()`](#getcolumns) &mdash; Returns the list of columns the rows in this datatable contain.
- [`getRowsMetadata()`](#getrowsmetadata) &mdash; Returns an array containing the requested metadata value of each row.
- [`getRowsCount()`](#getrowscount) &mdash; Returns the number of rows in the table including the summary row.
- [`getFirstRow()`](#getfirstrow) &mdash; Returns the first row of the DataTable.
- [`getLastRow()`](#getlastrow) &mdash; Returns the last row of the DataTable.
- [`getRowsCountRecursive()`](#getrowscountrecursive) &mdash; Returns the number of rows in this DataTable summed with the row count of each subtable in the DataTable hierarchy.
- [`deleteColumn()`](#deletecolumn) &mdash; Delete a column by name in every row.
- [`__sleep()`](#__sleep)
- [`renameColumn()`](#renamecolumn) &mdash; Rename a column in every row.
- [`deleteColumns()`](#deletecolumns) &mdash; Deletes several columns by name in every row.
- [`deleteRow()`](#deleterow) &mdash; Deletes a row by ID.
- [`deleteRowsOffset()`](#deleterowsoffset) &mdash; Deletes rows from `$offset` to `$offset + $limit`.
- [`deleteRows()`](#deleterows) &mdash; Deletes a set of rows by ID.
- [`__toString()`](#__tostring) &mdash; Returns a string representation of this DataTable for convenient viewing.
- [`isEqual()`](#isequal) &mdash; Returns true if both DataTable instances are exactly the same.
- [`getSerialized()`](#getserialized) &mdash; Serializes an entire DataTable hierarchy and returns the array of serialized DataTables.
- [`addRowsFromSerializedArray()`](#addrowsfromserializedarray) &mdash; Adds a set of rows from a serialized DataTable string.
- [`addRowsFromArray()`](#addrowsfromarray) &mdash; Adds many rows from an array.
- [`addRowsFromSimpleArray()`](#addrowsfromsimplearray) &mdash; Adds many rows from an array containing arrays of column values.
- [`makeFromIndexedArray()`](#makefromindexedarray) &mdash; Rewrites the input $array array (     LABEL => array(col1 => X, col2 => Y),     LABEL2 => array(col1 => X, col2 => Y), ) to a DataTable, ie.
- [`setMaximumDepthLevelAllowedAtLeast()`](#setmaximumdepthlevelallowedatleast) &mdash; Sets the maximum depth level to at least a certain value.
- [`getMetadata()`](#getmetadata) &mdash; Returns metadata by name.
- [`setMetadata()`](#setmetadata) &mdash; Sets a metadata value by name.
- [`getAllTableMetadata()`](#getalltablemetadata) &mdash; Returns all table metadata.
- [`setMetadataValues()`](#setmetadatavalues) &mdash; Sets several metadata values by name.
- [`setAllTableMetadata()`](#setalltablemetadata) &mdash; Sets metadata erasing existing values.
- [`setMaximumAllowedRows()`](#setmaximumallowedrows) &mdash; Sets the maximum number of rows allowed in this datatable (including the summary row).
- [`walkPath()`](#walkpath) &mdash; Traverses a DataTable tree using an array of labels and returns the row it finds or false if it cannot find one.
- [`mergeSubtables()`](#mergesubtables) &mdash; Returns a new DataTable in which the rows of this table are replaced with its subtable's rows.
- [`makeFromSimpleArray()`](#makefromsimplearray) &mdash; Returns a new DataTable created with data from a 'simple' array.
- [`fromSerializedArray()`](#fromserializedarray) &mdash; Creates a new DataTable instance from a serialized DataTable string.

<a name="__construct" id="__construct"></a>
### `__construct()`

Constructor.

#### Description

Creates an empty DataTable.

#### Signature

- It does not return anything.

<a name="__destruct" id="__destruct"></a>
### `__destruct()`

Destructor.

#### Description

Makes sure DataTable memory will be cleaned up.

#### Signature

- It does not return anything.

<a name="sort" id="sort"></a>
### `sort()`

Sorts the DataTable rows using the supplied callback function.

#### Signature

- It accepts the following parameter(s):
    - `$functionCallback`
    - `$columnSortedBy`
- It does not return anything.

<a name="getsortedbycolumnname" id="getsortedbycolumnname"></a>
### `getSortedByColumnName()`

Returns the name of the column this table was sorted by (if any).

#### Description

See [sort](#sort).

#### Signature

- _Returns:_ The sorted column name or false if none.
    - `Piwik\false`
    - `string`

<a name="enablerecursivesort" id="enablerecursivesort"></a>
### `enableRecursiveSort()`

Enables recursive sorting.

#### Description

If this method is called [sort](#sort) will also sort all
subtables.

#### Signature

- It does not return anything.

<a name="enablerecursivefilters" id="enablerecursivefilters"></a>
### `enableRecursiveFilters()`

Enables recursive filtering.

#### Description

If this method is called then the [filter](#filter) method
will apply filters to every subtable in addition to this instance.

#### Signature

- It does not return anything.

<a name="filter" id="filter"></a>
### `filter()`

Applies filter to this datatable.

#### Description

If [enableRecursiveFilters](#enableRecursiveFilters) was called, the filter will be applied
to all subtables as well.

#### Signature

- It accepts the following parameter(s):
    - `$className`
    - `$parameters`
- It does not return anything.

<a name="queuefilter" id="queuefilter"></a>
### `queueFilter()`

Adds a filter and a list of parameters to the list of queued filters.

#### Description

These filters will be
executed when [applyQueuedFilters](#applyQueuedFilters) is called.

Filters that prettify the output or don't need the full set of rows should be queued. This
way they will be run after the table is truncated which will result in better performance.

#### Signature

- It accepts the following parameter(s):
    - `$className`
    - `$parameters`
- It does not return anything.

<a name="applyqueuedfilters" id="applyqueuedfilters"></a>
### `applyQueuedFilters()`

Applies all filters that were previously queued to the table.

#### Description

See [queueFilter](#queueFilter)
for more information.

#### Signature

- It does not return anything.

<a name="adddatatable" id="adddatatable"></a>
### `addDataTable()`

Sums a DataTable to this one.

#### Description

This method will sum rows that have the same label. If a row is found in `$tableToSum` whose
label is not found in `$this`, the row will be added to `$this` DataTable.

If the subtables for this table are loaded, they will be summed as well.

Rows are summed together by summing individual columns. By default columns are summed by
adding one column value to another. Some columns cannot be aggregated this way. In these
cases, the [COLUMN_AGGREGATION_OPS_METADATA_NAME](#COLUMN_AGGREGATION_OPS_METADATA_NAME)
metadata can be used to specify a different type of operation.

#### Signature

- It accepts the following parameter(s):
    - `$tableToSum` ([`DataTable`](../Piwik/DataTable.md))
- It does not return anything.

<a name="getrowfromlabel" id="getrowfromlabel"></a>
### `getRowFromLabel()`

Returns the Row whose `'label'` column is equal to `$label`.

#### Description

This method executes in constant time except for the first call which caches row
label => row ID mappings.

#### Signature

- It accepts the following parameter(s):
    - `$label`
- _Returns:_ The row if found, false if otherwise.
    - [`Row`](../Piwik/DataTable/Row.md)
    - `Piwik\false`

<a name="getrowidfromlabel" id="getrowidfromlabel"></a>
### `getRowIdFromLabel()`

Returns the row id for the row whose `'label'` column is equal to `$label`.

#### Description

This method executes in constant time except for the first call which caches row
label => row ID mappings.

#### Signature

- It accepts the following parameter(s):
    - `$label`
- _Returns:_ The row ID.
    - `int`

<a name="getemptyclone" id="getemptyclone"></a>
### `getEmptyClone()`

Returns an empty DataTable with the same metadata and queued filters as `$this` one.

#### Signature

- It accepts the following parameter(s):
    - `$keepFilters`
- It returns a(n) [`DataTable`](../Piwik/DataTable.md) value.

<a name="getrowfromid" id="getrowfromid"></a>
### `getRowFromId()`

Returns a row by ID.

#### Description

The ID is either the index of the row or [ID_SUMMARY_ROW](#ID_SUMMARY_ROW).

#### Signature

- It accepts the following parameter(s):
    - `$id`
- _Returns:_ The Row or false if not found.
    - [`Row`](../Piwik/DataTable/Row.md)
    - `Piwik\false`

<a name="getrowfromidsubdatatable" id="getrowfromidsubdatatable"></a>
### `getRowFromIdSubDataTable()`

Returns the row that has a subtable with ID matching `$idSubtable`.

#### Signature

- It accepts the following parameter(s):
    - `$idSubTable`
- _Returns:_ The row or false if not found
    - [`Row`](../Piwik/DataTable/Row.md)
    - `Piwik\false`

<a name="addrow" id="addrow"></a>
### `addRow()`

Adds a row to this table.

#### Description

If [setMaximumAllowedRows](#setMaximumAllowedRows) was called and the current row count is 
at the maximum, the new row will be summed to the summary row. If there is no summary row,
this row is set as the summary row.

#### Signature

- It accepts the following parameter(s):
    - `$row` ([`Row`](../Piwik/DataTable/Row.md))
- _Returns:_ `$row` or the summary row if we're at the maximum number of rows.
    - [`Row`](../Piwik/DataTable/Row.md)

<a name="addsummaryrow" id="addsummaryrow"></a>
### `addSummaryRow()`

Sets the summary row.

#### Description

Note: A dataTable can have only one summary row.

#### Signature

- It accepts the following parameter(s):
    - `$row` ([`Row`](../Piwik/DataTable/Row.md))
- _Returns:_ Returns `$row`.
    - [`Row`](../Piwik/DataTable/Row.md)

<a name="getid" id="getid"></a>
### `getId()`

Returns the DataTable ID.

#### Signature

- It returns a(n) `int` value.

<a name="addrowfromarray" id="addrowfromarray"></a>
### `addRowFromArray()`

Adds a new row from an array.

#### Description

You can add Row metadata with this method.

#### Signature

- It accepts the following parameter(s):
    - `$row`
- It does not return anything.

<a name="addrowfromsimplearray" id="addrowfromsimplearray"></a>
### `addRowFromSimpleArray()`

Adds a new row a from an array of column values.

#### Description

Row metadata cannot be added with this method.

#### Signature

- It accepts the following parameter(s):
    - `$row`
- It does not return anything.

<a name="getrows" id="getrows"></a>
### `getRows()`

Returns the array of Rows.

#### Signature

- It returns a(n) [`Row[]`](../Piwik/DataTable/Row.md) value.

<a name="getcolumn" id="getcolumn"></a>
### `getColumn()`

Returns an array containing all column values for the requested column.

#### Signature

- It accepts the following parameter(s):
    - `$name`
- _Returns:_ The array of column values.
    - `array`

<a name="getcolumnsstartingwith" id="getcolumnsstartingwith"></a>
### `getColumnsStartingWith()`

Returns an array containing all column values of columns whose name starts with `$name`.

#### Signature

- It accepts the following parameter(s):
    - `$namePrefix`
- _Returns:_ The array of column values.
    - `array`

<a name="getcolumns" id="getcolumns"></a>
### `getColumns()`

Returns the list of columns the rows in this datatable contain.

#### Description

This will return the
columns of the first row with data and assume they occur in every other row as well.

Note: If column names still use their in-database INDEX values (@see Metrics), they
      will be converted to their string name in the array result.

#### Signature

- _Returns:_ Array of string column names.
    - `array`

<a name="getrowsmetadata" id="getrowsmetadata"></a>
### `getRowsMetadata()`

Returns an array containing the requested metadata value of each row.

#### Signature

- It accepts the following parameter(s):
    - `$name`
- It returns a(n) `array` value.

<a name="getrowscount" id="getrowscount"></a>
### `getRowsCount()`

Returns the number of rows in the table including the summary row.

#### Signature

- It returns a(n) `int` value.

<a name="getfirstrow" id="getfirstrow"></a>
### `getFirstRow()`

Returns the first row of the DataTable.

#### Signature

- _Returns:_ The first row or `false` if it cannot be found.
    - [`Row`](../Piwik/DataTable/Row.md)
    - `Piwik\false`

<a name="getlastrow" id="getlastrow"></a>
### `getLastRow()`

Returns the last row of the DataTable.

#### Description

If there is a summary row, it
will always be considered the last row.

#### Signature

- _Returns:_ The last row or `false` if it cannot be found.
    - [`Row`](../Piwik/DataTable/Row.md)
    - `Piwik\false`

<a name="getrowscountrecursive" id="getrowscountrecursive"></a>
### `getRowsCountRecursive()`

Returns the number of rows in this DataTable summed with the row count of each subtable in the DataTable hierarchy.

#### Description

This includes the subtables of subtables and further descendants.

#### Signature

- It returns a(n) `int` value.

<a name="deletecolumn" id="deletecolumn"></a>
### `deleteColumn()`

Delete a column by name in every row.

#### Description

This change is NOT applied recursively to all
subtables.

#### Signature

- It accepts the following parameter(s):
    - `$name`
- It does not return anything.

<a name="__sleep" id="__sleep"></a>
### `__sleep()`

#### Signature

- It does not return anything.

<a name="renamecolumn" id="renamecolumn"></a>
### `renameColumn()`

Rename a column in every row.

#### Description

This change is applied recursively to all subtables.

#### Signature

- It accepts the following parameter(s):
    - `$oldName`
    - `$newName`
- It does not return anything.

<a name="deletecolumns" id="deletecolumns"></a>
### `deleteColumns()`

Deletes several columns by name in every row.

#### Signature

- It accepts the following parameter(s):
    - `$names`
    - `$deleteRecursiveInSubtables`
- It does not return anything.

<a name="deleterow" id="deleterow"></a>
### `deleteRow()`

Deletes a row by ID.

#### Signature

- It accepts the following parameter(s):
    - `$id`
- It does not return anything.
- It throws one of the following exceptions:
    - [`Exception`](http://php.net/class.Exception) &mdash; If the row `$id` cannot be found.

<a name="deleterowsoffset" id="deleterowsoffset"></a>
### `deleteRowsOffset()`

Deletes rows from `$offset` to `$offset + $limit`.

#### Signature

- It accepts the following parameter(s):
    - `$offset`
    - `$limit`
- _Returns:_ The number of rows deleted.
    - `int`

<a name="deleterows" id="deleterows"></a>
### `deleteRows()`

Deletes a set of rows by ID.

#### Signature

- It accepts the following parameter(s):
    - `$rowIds` (`array`)
- It does not return anything.
- It throws one of the following exceptions:
    - [`Exception`](http://php.net/class.Exception) &mdash; If a row ID cannot be found.

<a name="__tostring" id="__tostring"></a>
### `__toString()`

Returns a string representation of this DataTable for convenient viewing.

#### Description

Note: This uses the Html DataTable renderer.

#### Signature

- It returns a(n) `string` value.

<a name="isequal" id="isequal"></a>
### `isEqual()`

Returns true if both DataTable instances are exactly the same.

#### Description

DataTables are equal if they have the same number of rows, if
each row has a label that exists in the other table, and if each row
is equal to the row in the other table with the same label. The order
of rows is not important.

#### Signature

- It accepts the following parameter(s):
    - `$table1` ([`DataTable`](../Piwik/DataTable.md))
    - `$table2` ([`DataTable`](../Piwik/DataTable.md))
- It returns a(n) `bool` value.

<a name="getserialized" id="getserialized"></a>
### `getSerialized()`

Serializes an entire DataTable hierarchy and returns the array of serialized DataTables.

#### Description

The first element in the returned array will be the serialized representation of this DataTable.
Every subsequent element will be a serialized subtable.

This DataTable and subtables can optionally be truncated before being serialized. In most
cases where DataTables can become quite large, they should be truncated before being persisted
in an archive.

The result of this method is intended for use with the [ArchiveProcessor::insertBlobRecord](#) method.

#### Signature

- It accepts the following parameter(s):
    - `$maximumRowsInDataTable`
    - `$maximumRowsInSubDataTable`
    - `$columnToSortByBeforeTruncation`
- _Returns:_ The array of serialized DataTables: array( // this DataTable (the root) 0 => 'eghuighahgaueytae78yaet7yaetae', // a subtable 1 => 'gaegae gh gwrh guiwh uigwhuige', // another subtable 2 => 'gqegJHUIGHEQjkgneqjgnqeugUGEQHGUHQE', // etc. );
    - `array`
- It throws one of the following exceptions:
    - [`Exception`](http://php.net/class.Exception) &mdash; If infinite recursion detected. This will occur if a table&#039;s subtable is one of its parent tables.

<a name="addrowsfromserializedarray" id="addrowsfromserializedarray"></a>
### `addRowsFromSerializedArray()`

Adds a set of rows from a serialized DataTable string.

#### Description

See [serialize](#serialize).

#### Signature

- It accepts the following parameter(s):
    - `$stringSerialized`
- It does not return anything.
- It throws one of the following exceptions:
    - [`Exception`](http://php.net/class.Exception) &mdash; if `$stringSerialized` is invalid.

<a name="addrowsfromarray" id="addrowsfromarray"></a>
### `addRowsFromArray()`

Adds many rows from an array.

#### Description

You can add Row metadata with this method.

#### Signature

- It accepts the following parameter(s):
    - `$array`
- It does not return anything.

<a name="addrowsfromsimplearray" id="addrowsfromsimplearray"></a>
### `addRowsFromSimpleArray()`

Adds many rows from an array containing arrays of column values.

#### Description

Row metadata cannot be added with this method.

#### Signature

- It accepts the following parameter(s):
    - `$array`
- It does not return anything.
- It throws one of the following exceptions:
    - [`Exception`](http://php.net/class.Exception)

<a name="makefromindexedarray" id="makefromindexedarray"></a>
### `makeFromIndexedArray()`

Rewrites the input $array array (     LABEL => array(col1 => X, col2 => Y),     LABEL2 => array(col1 => X, col2 => Y), ) to a DataTable, ie.

#### Description

with the internal structure
array (
    array( Row::COLUMNS => array('label' => LABEL, col1 => X, col2 => Y)),
    array( Row::COLUMNS => array('label' => LABEL2, col1 => X, col2 => Y)),
)

It also works with array having only one value per row, eg.
array (
    LABEL => X,
    LABEL2 => Y,
)
would be converted to:
array (
    array( Row::COLUMNS => array('label' => LABEL, 'value' => X)),
    array( Row::COLUMNS => array('label' => LABEL2, 'value' => Y)),
)

#### Signature

- It accepts the following parameter(s):
    - `$array`
    - `$subtablePerLabel`
- It returns a(n) [`DataTable`](../Piwik/DataTable.md) value.

<a name="setmaximumdepthlevelallowedatleast" id="setmaximumdepthlevelallowedatleast"></a>
### `setMaximumDepthLevelAllowedAtLeast()`

Sets the maximum depth level to at least a certain value.

#### Description

If the current value is
greater than the supplied level, the maximum nesting level is not changed.

The maximum depth level determines the maximum number of subtable levels in the
DataTable tree. For example, if it is set to `2`, this DataTable is allowed to
have subtables, but the subtables are not.

#### Signature

- It accepts the following parameter(s):
    - `$atLeastLevel`
- It does not return anything.

<a name="getmetadata" id="getmetadata"></a>
### `getMetadata()`

Returns metadata by name.

#### Signature

- It accepts the following parameter(s):
    - `$name`
- _Returns:_ The metadata value or false if it cannot be found.
    - `mixed`
    - `Piwik\false`

<a name="setmetadata" id="setmetadata"></a>
### `setMetadata()`

Sets a metadata value by name.

#### Signature

- It accepts the following parameter(s):
    - `$name`
    - `$value`
- It does not return anything.

<a name="getalltablemetadata" id="getalltablemetadata"></a>
### `getAllTableMetadata()`

Returns all table metadata.

#### Signature

- It returns a(n) `array` value.

<a name="setmetadatavalues" id="setmetadatavalues"></a>
### `setMetadataValues()`

Sets several metadata values by name.

#### Signature

- It accepts the following parameter(s):
    - `$values`
- It does not return anything.

<a name="setalltablemetadata" id="setalltablemetadata"></a>
### `setAllTableMetadata()`

Sets metadata erasing existing values.

#### Signature

- It accepts the following parameter(s):
    - `$metadata`
- It does not return anything.

<a name="setmaximumallowedrows" id="setmaximumallowedrows"></a>
### `setMaximumAllowedRows()`

Sets the maximum number of rows allowed in this datatable (including the summary row).

#### Description

If adding more then the allowed number of rows is attempted, the extra
rows are summed to the summary row.

#### Signature

- It accepts the following parameter(s):
    - `$maximumAllowedRows`
- It does not return anything.

<a name="walkpath" id="walkpath"></a>
### `walkPath()`

Traverses a DataTable tree using an array of labels and returns the row it finds or false if it cannot find one.

#### Description

The number of path segments that
were successfully walked is also returned.

If $missingRowColumns is supplied, the specified path is created. When
a subtable is encountered w/o the queried label, a new row is created
with the label, and a subtable is added to the row.

Read [http://en.wikipedia.org/wiki/Tree_(data_structure)#Traversal_methods](http://en.wikipedia.org/wiki/Tree_(data_structure)#Traversal_methods)
for more information about tree walking.

#### Signature

- It accepts the following parameter(s):
    - `$path`
    - `$missingRowColumns`
    - `$maxSubtableRows`
- _Returns:_ First element is the found row or false. Second element is the number of path segments walked. If a row is found, this will be == to count($path). Otherwise, it will be the index of the path segment that we could not find.
    - `array`

<a name="mergesubtables" id="mergesubtables"></a>
### `mergeSubtables()`

Returns a new DataTable in which the rows of this table are replaced with its subtable's rows.

#### Signature

- It accepts the following parameter(s):
    - `$labelColumn`
    - `$useMetadataColumn`
- It returns a(n) [`DataTable`](../Piwik/DataTable.md) value.

<a name="makefromsimplearray" id="makefromsimplearray"></a>
### `makeFromSimpleArray()`

Returns a new DataTable created with data from a 'simple' array.

#### Description

See [addRowsFromSimpleArray](#addRowsFromSimpleArray).

#### Signature

- It accepts the following parameter(s):
    - `$array`
- It returns a(n) [`DataTable`](../Piwik/DataTable.md) value.

<a name="fromserializedarray" id="fromserializedarray"></a>
### `fromSerializedArray()`

Creates a new DataTable instance from a serialized DataTable string.

#### Description

See [getSerialized](#getSerialized) and [addRowsFromSerializedArray](#addRowsFromSerializedArray)
for more information on DataTable serialization.

#### Signature

- It accepts the following parameter(s):
    - `$data`
- It returns a(n) [`DataTable`](../Piwik/DataTable.md) value.
