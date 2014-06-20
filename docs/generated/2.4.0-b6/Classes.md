Classes
=======

This is a complete list of available classes:

- [`API\Request`](Piwik/API/Request.md) &mdash; Dispatches API requests to the appropriate API method.
- [`Archive`](Piwik/Archive.md) &mdash; The **Archive** class is used to query cached analytics statistics (termed "archive data").
- [`ArchiveProcessor`](Piwik/ArchiveProcessor.md) &mdash; Used by [Archiver](/api-reference/Piwik/Plugin/Archiver) instances to insert and aggregate archive data.
- [`ArchiveProcessor\Parameters`](Piwik/ArchiveProcessor/Parameters.md) &mdash; Contains the analytics parameters for the reports that are currently being archived.
- [`Common`](Piwik/Common.md) &mdash; Contains helper methods used by both Piwik Core and the Piwik Tracking engine.
- [`Config`](Piwik/Config.md) &mdash; Singleton that provides read & write access to Piwik's INI configuration.
- [`Console`](Piwik/Console.md)
- [`DataAccess\LogAggregator`](Piwik/DataAccess/LogAggregator.md) &mdash; Contains methods that calculate metrics by aggregating log data (visits, actions, conversions, ecommerce items).
- [`DataTable`](Piwik/DataTable.md) &mdash; The primary data structure used to store analytics data in Piwik.
- [`DataTable\BaseFilter`](Piwik/DataTable/BaseFilter.md) &mdash; A filter is set of logic that manipulates a DataTable.
- [`DataTable\Filter\AddColumnsProcessedMetrics`](Piwik/DataTable/Filter/AddColumnsProcessedMetrics.md) &mdash; Adds processed metrics columns to a DataTable using metrics that already exist.
- [`DataTable\Filter\AddColumnsProcessedMetricsGoal`](Piwik/DataTable/Filter/AddColumnsProcessedMetricsGoal.md) &mdash; Adds goal related metrics to a DataTable using metrics that already exist.
- [`DataTable\Filter\AddSummaryRow`](Piwik/DataTable/Filter/AddSummaryRow.md) &mdash; Adds a summary row to DataTables that contains the sum of all other table rows.
- [`DataTable\Filter\BeautifyRangeLabels`](Piwik/DataTable/Filter/BeautifyRangeLabels.md) &mdash; A DataTable filter that replaces range label columns with prettier, human-friendlier versions.
- [`DataTable\Filter\BeautifyTimeRangeLabels`](Piwik/DataTable/Filter/BeautifyTimeRangeLabels.md) &mdash; A DataTable filter that replaces range labels whose values are in seconds with prettier, human-friendlier versions.
- [`DataTable\Filter\CalculateEvolutionFilter`](Piwik/DataTable/Filter/CalculateEvolutionFilter.md) &mdash; A DataTable filter that calculates the evolution of a metric and adds it to each row as a percentage.
- [`DataTable\Filter\ColumnCallbackAddColumn`](Piwik/DataTable/Filter/ColumnCallbackAddColumn.md) &mdash; Adds a new column to every row of a DataTable based on the result of callback.
- [`DataTable\Filter\ColumnCallbackAddColumnPercentage`](Piwik/DataTable/Filter/ColumnCallbackAddColumnPercentage.md) &mdash; Calculates a percentage value for each row of a DataTable and adds the result to each row.
- [`DataTable\Filter\ColumnCallbackAddColumnQuotient`](Piwik/DataTable/Filter/ColumnCallbackAddColumnQuotient.md) &mdash; Calculates the quotient of two columns and adds the result as a new column for each row of a DataTable.
- [`DataTable\Filter\ColumnCallbackAddMetadata`](Piwik/DataTable/Filter/ColumnCallbackAddMetadata.md) &mdash; Executes a callback for each row of a DataTable and adds the result as a new row metadata value.
- [`DataTable\Filter\ColumnCallbackDeleteRow`](Piwik/DataTable/Filter/ColumnCallbackDeleteRow.md) &mdash; Deletes all rows for which a callback returns true.
- [`DataTable\Filter\ColumnDelete`](Piwik/DataTable/Filter/ColumnDelete.md) &mdash; Filter that will remove columns from a DataTable using either a blacklist, whitelist or both.
- [`DataTable\Filter\ExcludeLowPopulation`](Piwik/DataTable/Filter/ExcludeLowPopulation.md) &mdash; Deletes all rows for which a specific column has a value that is lower than specified minimum threshold value.
- [`DataTable\Filter\GroupBy`](Piwik/DataTable/Filter/GroupBy.md) &mdash; DataTable filter that will group DataTable rows together based on the results of a reduce function.
- [`DataTable\Filter\Limit`](Piwik/DataTable/Filter/Limit.md) &mdash; Delete all rows from the table that are not in the given [offset, offset+limit) range.
- [`DataTable\Filter\MetadataCallbackAddMetadata`](Piwik/DataTable/Filter/MetadataCallbackAddMetadata.md) &mdash; Executes a callback for each row of a DataTable and adds the result to the row as a metadata value.
- [`DataTable\Filter\MetadataCallbackReplace`](Piwik/DataTable/Filter/MetadataCallbackReplace.md) &mdash; Execute a callback for each row of a DataTable passing certain column values and metadata as metadata, and replaces row metadata with the callback result.
- [`DataTable\Filter\Pattern`](Piwik/DataTable/Filter/Pattern.md) &mdash; Deletes every row for which a specific column does not match a supplied regex pattern.
- [`DataTable\Filter\PatternRecursive`](Piwik/DataTable/Filter/PatternRecursive.md) &mdash; Deletes rows that do not contain a column that matches a regex pattern and do not contain a subtable that contains a column that matches a regex pattern.
- [`DataTable\Filter\ReplaceColumnNames`](Piwik/DataTable/Filter/ReplaceColumnNames.md) &mdash; Replaces column names in each row of a table using an array that maps old column names new ones.
- [`DataTable\Filter\ReplaceSummaryRowLabel`](Piwik/DataTable/Filter/ReplaceSummaryRowLabel.md) &mdash; Replaces the label of the summary row with a supplied label.
- [`DataTable\Filter\Sort`](Piwik/DataTable/Filter/Sort.md) &mdash; Sorts a DataTable based on the value of a specific column.
- [`DataTable\Filter\Truncate`](Piwik/DataTable/Filter/Truncate.md) &mdash; Truncates a DataTable by merging all rows after a certain index into a new summary row.
- [`DataTable\Map`](Piwik/DataTable/Map.md) &mdash; Stores an array of DataTables indexed by one type of DataTable metadata (such as site ID or period).
- [`DataTable\Row`](Piwik/DataTable/Row.md) &mdash; This is what a [DataTable](/api-reference/Piwik/DataTable) is composed of.
- [`DataTable\Simple`](Piwik/DataTable/Simple.md) &mdash; A [DataTable](/api-reference/Piwik/DataTable) where every row has two columns: **label** and **value**.
- [`Date`](Piwik/Date.md) &mdash; Utility class that wraps date/time related PHP functions.
- [`Db`](Piwik/Db.md) &mdash; Contains SQL related helper functions for Piwik's MySQL database.
- [`DbHelper`](Piwik/DbHelper.md) &mdash; Contains database related helper functions.
- [`Filesystem`](Piwik/Filesystem.md) &mdash; Contains helper functions that deal with the filesystem.
- [`FrontController`](Piwik/FrontController.md) &mdash; This singleton dispatches requests to the appropriate plugin Controller.
- [`Http`](Piwik/Http.md) &mdash; Contains HTTP client related helper methods that can retrieve content from remote servers and optionally save to a local file.
- [`IP`](Piwik/IP.md) &mdash; Contains IP address helper functions (for both IPv4 and IPv6).
- [`Log`](Piwik/Log.md) &mdash; Logging utility class.
- [`Mail`](Piwik/Mail.md) &mdash; Class for sending mails, for more information see: [http://framework.zend.com/manual/en/zend.mail.html](http://framework.zend.com/manual/en/zend.mail.html)
- [`Menu\MenuAbstract`](Piwik/Menu/MenuAbstract.md) &mdash; Base class for classes that manage one of Piwik's menus.
- [`Menu\MenuAdmin`](Piwik/Menu/MenuAdmin.md) &mdash; Contains menu entries for the Admin menu.
- [`Menu\MenuReporting`](Piwik/Menu/MenuReporting.md) &mdash; Contains menu entries for the Reporting menu (the menu displayed under the Piwik logo).
- [`Menu\MenuTop`](Piwik/Menu/MenuTop.md) &mdash; Contains menu entries for the Top menu (the menu at the very top of the page).
- [`Menu\MenuUser`](Piwik/Menu/MenuUser.md) &mdash; Contains menu entries for the User menu (the menu at the very top of the page).
- [`Metrics`](Piwik/Metrics.md) &mdash; This class contains metadata regarding core metrics and contains several related helper functions.
- [`MetricsFormatter`](Piwik/MetricsFormatter.md) &mdash; Contains helper function that format numerical values in different ways.
- [`NoAccessException`](Piwik/NoAccessException.md) &mdash; Exception thrown when a user doesn't have sufficient access to a resource.
- [`Nonce`](Piwik/Nonce.md) &mdash; Nonce class.
- [`Notification`](Piwik/Notification.md) &mdash; Describes a UI notification.
- [`Notification\Manager`](Piwik/Notification/Manager.md) &mdash; Posts and removes UI notifications (see [Notification](/api-reference/Piwik/Notification) to learn more).
- [`Option`](Piwik/Option.md) &mdash; Convenient key-value storage for user specified options and temporary data that needs to be persisted beyond one request.
- [`Period`](Piwik/Period.md) &mdash; Date range representation.
- [`Period\Range`](Piwik/Period/Range.md) &mdash; Arbitrary date range representation.
- [`Piwik`](Piwik/Piwik.md) &mdash; Main piwik helper class.
- [`Plugin`](Piwik/Plugin.md) &mdash; Base class of all Plugin Descriptor classes.
- [`Plugin\API`](Piwik/Plugin/API.md) &mdash; The base class of all API singletons.
- [`Plugin\Archiver`](Piwik/Plugin/Archiver.md) &mdash; The base class that should be extended by plugins that compute their own analytics data.
- [`Plugin\Controller`](Piwik/Plugin/Controller.md) &mdash; Base class of all plugin Controllers.
- [`Plugin\Manager`](Piwik/Plugin/Manager.md) &mdash; The singleton that manages plugin loading/unloading and installation/uninstallation.
- [`Plugin\Menu`](Piwik/Plugin/Menu.md) &mdash; Base class of all plugin menu providers.
- [`Plugin\Settings`](Piwik/Plugin/Settings.md) &mdash; Base class of all plugin settings providers.
- [`Plugin\Tasks`](Piwik/Plugin/Tasks.md) &mdash; Base class for all Tasks declarations.
- [`Plugin\ViewDataTable`](Piwik/Plugin/ViewDataTable.md) &mdash; The base class of all report visualizations.
- [`Plugin\Visualization`](Piwik/Plugin/Visualization.md) &mdash; The base class for report visualizations that output HTML and use JavaScript.
- [`Plugin\Widgets`](Piwik/Plugin/Widgets.md) &mdash; Base class of all plugin widget providers.
- [`Plugins\CoreVisualizations\Visualizations\Cloud`](Piwik/Plugins/CoreVisualizations/Visualizations/Cloud.md) &mdash; Generates a tag cloud from a given data array.
- [`Plugins\CoreVisualizations\Visualizations\Graph`](Piwik/Plugins/CoreVisualizations/Visualizations/Graph.md) &mdash; This is an abstract visualization that should be the base of any 'graph' visualization.
- [`Plugins\CoreVisualizations\Visualizations\HtmlTable`](Piwik/Plugins/CoreVisualizations/Visualizations/HtmlTable.md) &mdash; DataTable visualization that shows DataTable data in an HTML table.
- [`Plugins\CoreVisualizations\Visualizations\HtmlTable\AllColumns`](Piwik/Plugins/CoreVisualizations/Visualizations/HtmlTable/AllColumns.md) &mdash; DataTable Visualization that derives from HtmlTable and sets show_extra_columns to true.
- [`Plugins\CoreVisualizations\Visualizations\JqplotGraph`](Piwik/Plugins/CoreVisualizations/Visualizations/JqplotGraph.md) &mdash; DataTable visualization that displays DataTable data in a JQPlot graph.
- [`Plugins\CoreVisualizations\Visualizations\JqplotGraph\Bar`](Piwik/Plugins/CoreVisualizations/Visualizations/JqplotGraph/Bar.md) &mdash; Visualization that renders HTML for a Bar graph using jqPlot.
- [`Plugins\CoreVisualizations\Visualizations\JqplotGraph\Evolution`](Piwik/Plugins/CoreVisualizations/Visualizations/JqplotGraph/Evolution.md) &mdash; Visualization that renders HTML for a line graph using jqPlot.
- [`Plugins\CoreVisualizations\Visualizations\JqplotGraph\Pie`](Piwik/Plugins/CoreVisualizations/Visualizations/JqplotGraph/Pie.md) &mdash; Visualization that renders HTML for a Pie graph using jqPlot.
- [`Plugins\ExampleVisualization\SimpleTable`](Piwik/Plugins/ExampleVisualization/SimpleTable.md) &mdash; SimpleTable Visualization.
- [`Plugins\Goals\Visualizations\Goals`](Piwik/Plugins/Goals/Visualizations/Goals.md) &mdash; DataTable Visualization that derives from HtmlTable and sets show_goals_columns to true.
- [`Plugins\Insights\Visualizations\Insight`](Piwik/Plugins/Insights/Visualizations/Insight.md) &mdash; InsightsVisualization Visualization.
- [`Plugins\Live\VisitorLog`](Piwik/Plugins/Live/VisitorLog.md) &mdash; A special DataTable visualization for the Live.getLastVisitsDetails API method.
- [`RankingQuery`](Piwik/RankingQuery.md) &mdash; The ranking query class wraps an arbitrary SQL query with more SQL that limits the number of results while aggregating the rest in an a new "Others" row.
- [`ScheduledTask`](Piwik/ScheduledTask.md) &mdash; Contains metadata referencing PHP code that should be executed at regular intervals.
- [`ScheduledTime`](Piwik/ScheduledTime.md) &mdash; Describes the interval on which a scheduled task is executed.
- [`ScheduledTime\Daily`](Piwik/ScheduledTime/Daily.md) &mdash; Daily class is used to schedule tasks every day.
- [`ScheduledTime\Hourly`](Piwik/ScheduledTime/Hourly.md) &mdash; Hourly class is used to schedule tasks every hour.
- [`ScheduledTime\Monthly`](Piwik/ScheduledTime/Monthly.md) &mdash; Monthly class is used to schedule tasks every month.
- [`ScheduledTime\Weekly`](Piwik/ScheduledTime/Weekly.md) &mdash; Weekly class is used to schedule tasks every week.
- [`Segment`](Piwik/Segment.md) &mdash; Limits the set of visits Piwik uses when aggregating analytics data.
- [`SettingsPiwik`](Piwik/SettingsPiwik.md) &mdash; Contains helper methods that can be used to get common Piwik settings.
- [`SettingsServer`](Piwik/SettingsServer.md) &mdash; Contains helper methods that can be used to get information regarding the server, its settings and currently used PHP settings.
- [`Settings\Setting`](Piwik/Settings/Setting.md) &mdash; Base setting type class.
- [`Settings\SystemSetting`](Piwik/Settings/SystemSetting.md) &mdash; Describes a system wide setting.
- [`Settings\UserSetting`](Piwik/Settings/UserSetting.md) &mdash; Describes a per user setting.
- [`Singleton`](Piwik/Singleton.md) &mdash; The singleton base class restricts the instantiation of derived classes to one object only.
- [`Site`](Piwik/Site.md) &mdash; Provides access to individual [site entity](/guides/persistence-and-the-mysql-backend#websites-aka-sites) data (including name, URL, etc.).
- [`TaskScheduler`](Piwik/TaskScheduler.md) &mdash; Manages scheduled task execution.
- [`Url`](Piwik/Url.md) &mdash; Provides URL related helper methods.
- [`UrlHelper`](Piwik/UrlHelper.md) &mdash; Contains less commonly needed URL helper methods.
- [`Version`](Piwik/Version.md) &mdash; Piwik version information.
- [`View`](Piwik/View.md) &mdash; Encapsulates and manages a [Twig](http://twig.sensiolabs.org/) template.
- [`ViewDataTable\Config`](Piwik/ViewDataTable/Config.md) &mdash; Contains base display properties for [ViewDataTable](/api-reference/Piwik/Plugin/ViewDataTable)s.
- [`ViewDataTable\Factory`](Piwik/ViewDataTable/Factory.md) &mdash; Provides a means of creating [ViewDataTable](/api-reference/Piwik/Plugin/ViewDataTable) instances by ID.
- [`ViewDataTable\RequestConfig`](Piwik/ViewDataTable/RequestConfig.md) &mdash; Contains base request properties for [ViewDataTable](/api-reference/Piwik/Plugin/ViewDataTable) instances.
- [`View\UIControl`](Piwik/View/UIControl.md) &mdash; Base type of UI controls.