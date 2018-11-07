  - title  
    The Operations Page

# The Operations Page

<div id="page" class="container">

<div id="main" class="container aui-page-panel">

<div id="main-header" class="container">

<div id="breadcrumb-section" class="container">

1.  [Unravel 4.4](index.html)
2.  [Unravel 4.4](Unravel-4.4_541197025.html)
3.  [User Guide](User-Guide_541295329.html)

</div>

**Unravel 4.4 : The Operations Page**

</div>

<div id="content" class="container view">

<div class="container page-metadata">

Created by katch, last modified by Gopala Krishna G on Sep 16, 2018

</div>

<div id="main-content" class="container wiki-content group">

<div class="container panel">

<div class="container panelHeader">

**Table of Contents**

</div>

<div class="container panelContent">

<div class="container toc-macro rbtoc1541196990382">

  - [Dashboard](#TheOperationsPage-DashboardDashboard)
      - [Finished YARN applications
        Tile](#TheOperationsPage-FinishedYARNapplicationsTile)
      - [Running YARN Application
        Tile](#TheOperationsPage-RunningYARNApplicationTile)
      - [Resources Tile](#TheOperationsPage-ResourcesTile)
      - [Inefficient Applications
        Tile](#TheOperationsPage-InefficientApplicationsTile)
      - [Recent Events and Alerts
        Sidebar](#TheOperationsPage-RecentEventsandAlertsSidebar)
  - [Usage
        Details](#TheOperationsPage-ChartsUsageDetails)
      - [Infrastructure](#TheOperationsPage-InfrastructureInfrastructure)
      - [Jobs](#TheOperationsPage-JobsJobs)
      - [Nodes](#TheOperationsPage-NodesNodes)
      - [Impala Usage](#TheOperationsPage-ImpalaImpalaUsage)
      - [Kafka](#TheOperationsPage-KafkaKafka)
      - [HBase](#TheOperationsPage-HBaseHBase)

</div>

</div>

</div>

The Operations Page provides synopsis of your cluster(s) and its
activities. It has two tabs:

**Dashboard** and **Usage Detail**. By default it opens showing
**Operations** | **Dashboard** tab.

<div class="container">

</div>

confluence-information-macro has-no-icon
confluence-information-macro-note

> Note
> 
> <div class="container confluence-information-macro-body">
> 
> Click [here](Common-UI-Features_541295593.html) for common features
> used throughout Unravel's UI.
> 
> </div>

**Dashboard**

To view the Dashboard, click **Operations** | **Dashboard**.

The Dashboard provides an overview of cluster activities with links to
drill down into YARN applications, resource usage, application
inefficiencies, and events/alerts. By default it is configured to
display *all* clusters *hourly* for the past *24 hours*.

**Finished YARN applications Tile**

The line graphs display the successful, failed, and killed jobs for the
*time period*, using the *time incremen*t and *cluster*(s) specified. It
textually displays the total number over the time period.

Clicking on\*\* Open Section\*\* brings up the window **Applications |
Applications** window listing all applications. See[Finding
Applications](The-Applications-Page_541164197.html)for more detail on
this view.

**Running YARN Application Tile**

The line graphs display the running and pending jobs for the current
time. It textually displays the total number at the current time period.

Clicking on\*\* Open Section\*\* opens Operations | Usage Details |
Jobs. See [here](#TheOperationsPage-ChartsJobs)for more information
about this view.

**Resources Tile**

Displays the available and allocated VCore and Memory for the entire
cluster.

Clicking on Open Section opensOperations | Usage Details |
Infrastructure.See[below](#TheOperationsPage-ChartsResources) for an
example of this view.

**Inefficient Applications Tile**

Its three sub-tabs, **HIVE**, **MapReduce**, and **Spark;** each lists
the inefficiencies that have occurred for that job type and the number
of apps experiencing it. Click on the sub-tab to change the application
type. Click on the **Event Name** to bring up the list of applications
that experienced the event.

The inefficiencies application is equal to the **Applications |
Applications** excepting that it only displays applications that have
experienced the event. The event type is noted in the upper left hand
corner. See[Finding
Applications](The-Applications-Page_541164197.html)for more detail on
this view.

**Recent Events and Alerts Sidebar**

The sidebar lists all events and alerts that have occurred organized by
date and time. A separate entry appears for each time a particular Auto
Action was triggered. In the image below, the same auto action triggered
at 23:56 and 2:58. Clicking an event/alert brings up a Cluster Resource
view (**Operations | Usage Detail | Infrastructure**) displaying a 10
minute time slice (+-5 minutes of the event) and includes a list of
applications running at that time. See [Auto Actions
Overview](Auto-Actions-Overview_541131624.html#AutoActionsOverview-ClusterView)
for more information. Click [Add a New AutoAction or
Alert](Creating-Auto-Actions_541295518.html) to create a new auto
action.Click Clear to clear the list.

**Usage
Details**

[ ](https://unraveldata.atlassian.net/wiki/spaces/UN44/pages/edit/541197498?draftId=541295828&draftShareId=6b31e169-5762-4397-8679-e3efd5351da4&)Usage
Detail has four tabs:

  - [Infrastructure](#TheOperationsPage-Infrastructure)
  - [Jobs](#TheOperationsPage-Jobs)
  - [Nodes](#TheOperationsPage-Nodes) 
  - [Impala Usage](#TheOperationsPage-ImpalaUsage)
  - [Kafka](#TheOperationsPage-Kafk)
  - [HBase](#TheOperationsPage-HBase)

One of the most difficult challenges in managing multi-tenant Hadoop
clusters is understanding how resources are being used by the
applications running in the clusters. Through **Usage Details**, Unravel
Web UI provides a unique forensic view into each cluster's key
performance indicators (KPIs) over time and how they relate to the
applications running in the cluster.

![(lightbulb)](images/icons/emoticons/lightbulb_on.png) For example,
Unravel can pinpoint the applications causing a sudden a spike in the
total VCores or memory MB usage. This allows you to easily you drill
down into these applications to understand their behavior. Whenever
possible, Unravel provides [recommendations and
insights](The-Applications-Page_541164197.html#TheApplicationsPage-EventPanel)
to help improve the application(s) run. 

All the charts and tables are automatically refreshed; however
refreshing is disabled when you interact within a page to alter its
display, e.g., change the date range, click on a point within in a
graph. When disabled a **Refresh** button is displayed in Usage Details
title bar. Click on **Refresh** to resume automatic refreshes.

By default the Usage Details tab opens showing the Infrastructure tab.

For all charts, click on the menu bars (), for print and download
options, e.g. csv, jpeg. Click **Show more** to expand it. For a
particular point in time, hovering over chart raises a popup with
details while clicking all applications running. Once expanded click on
the to return to the initial view. To zoom in drag over a section of the
graph; to return to the complete graph click **Reset Graph**. The
examples below are showing the Vcores-Total graph from the
**Infrastructure** Tab expanded and then zoomed in a section..

**Infrastructure**

This tab contains four (4) graphs.

  - The upper two list available and allocated Vcores and memory for the
    entire Cluster, and
  - The bottom show the Vcores and memory used by specific view, i.e.,
    **Application Type**, **User**, **Queue**, and **Business**
    **Tags**.

Clicking within a chart (1) displays the applications running for that
point in time.

You can chose how to display the bottom two graphs by clicking on the
**View By** type (2). Once you have chosen your view, the first three
(3) available values for that view are displayed in the **Showing** box
(3), i.e., job types running, mapReduce, Spark. Above the box notes the
number displayed of the total available values for the **View** type.
Regardless the total available values by View By choice, you may only
graph up to four (4). Click within the **Showing** box to see all
available values; to remove a value click on the **x** next to its name
(3). By default **Infrastructure** opens displaying **Application
Type**. In the example below only one application type if available,
Mapreduce. In the example [above](#TheOperationsPage-Infra-Vcores)
Vcores-Total was expanded (**show more**) and then zoomed in on a
section.

To **View by** tags, **use the Business Tags** pull down menu, which
will display all available tags. The tag you chose is displayed in blue
to left of the Business Tags. To add a tag, click in the **Showing** box
to show all available tags, click on the tag to use it. You can only
have up to four (4) tag values. Delete a particular value by clicking
the x next to remove the value.

**Jobs**

Graphs the running and accepted jobs as applicable. You can **Group by**
State, App Type, User, and Queue. By default the chart uses State. You
can change the display of an item via the Group By pull-down.

**Nodes**

This chart graphs the **Total** number of Nodes and the breakdown by
node status, Active, Lost, Unhealthy, Decommissioned and Rebooted.

 \**Total*\* = **Active** + **Unhealthy**

Where:

  - **Active: ** currently running and healthy nodes, and
  - **Unhealthy:** currently running and unhealthy nodes.

You can toggle the display of an item by clicking on its name.

**Impala Usage**

Graphs memory MB consumption and Query Number. The **\# Queries** graph
can be displayed by **Tags** and **Group By** (User or Queue).

**Kafka**

Lists all the configured Kafka clusters. See [Kafka Application
Manager](The-Applications-Page_541164197.html#TheApplicationsPage-KafkaAPM)
for more information. See [Kafka Use
Case](https://unraveldata.atlassian.net/wiki/spaces/UN43/pages/228753804/Kafka+Insights)
for information on drilling down into a Cluster to locate lagging and
stalled Topics/Partitions.

Clicking the cluster name brings detailed information about the Kafka
Cluster

**HBase**

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> 
> 
> <div class="container confluence-information-macro-body">
> 
> Please see[HBase
> Configuration](HBASE--Configuration_546537734.html)for configuring
> Unravel UIX for HBase clusters.
> 
> </div>

**Clusters View**

Clusters page lists all the available HBase clusters

Click on a cluster name to bring up the cluster's information the HBase
Cluster view.

**Cluster View**

This view is divided into four (4) sections. When an component's health
is noted, hovering over it's health glyph brings up details, .

**Cluster Information**

A bar at shows what cluster you are displaying with a pull-down which
allows you to switch between clusters. Listed immediately below are the
cluster metrics. You can choose to tab between clusters by choosing all
cluster. Shown below is the tabbed view.

**Region Servers**

Lists the the cluster regional services, with their KPI's and health.
You can search on the region server by name. Click on the server's name
to bring up its[details.](#TheOperationsPage-RegionServ)

Region Servers KPI

Graphs the regional server metrics, the graphs are linked with the table
list below them. Click within a graph to see up the tables associated
servers that point in time. Hover over a point to bring up a popup
displaying the information for that point in time. Click Show More the
expand the graph with the table list to full window width. Click to
print or download the graph.

Tables

List all the tables associated with the cluster, their KPIs and the
table's health. Click on the table name to bring up its information. You
can search for a table by name; any table with a name matching or
containing the string is displayed.

**Region Server View**

Server, Operational, and OS Metrics are displayed. Hover over the metric
for its description. For more information on the metrics
see[here](HBASE--Alerts-and-Metrics_541197498.html#HBASEAlertsandMetrics-RegionServerMetric).
You can change the date range to display using the date picker. The
first four (4) regional server metrics are graphed the daily. Use the
pull down menu to the weekly view. Click to print or download the graph.
The table list contains all the tables that have been accessed by this
server. You can search the table list by name. Click on the table name
to bring up its details.

**Table View**

Table has two tabs, **Table** and **Region**. It opens in the **Table**
view, which displays the Table's KPIs. Three of the KPIs, regionCount,
readRequestCount, and writeRequestCount are also graphed and linked with
the application list below them. Click to print or download the
graph.Hover over the graph to display the information for that point in
time. Click within the graph to display the applications running at that
point in time. Click on the application's name to open it in its
application manager.

**Table Regions**

Lists all the regions with their KPIs and health.

</div>

<div class="container pageSection group">

<div class="container pageSectionHeader">

**Attachments:**

</div>

<div class="container greybox">

![image1](images/icons/bullet_blue.gif)
[closeCross.png](attachments/541033301/541098773.png) (image/png)
![image2](images/icons/bullet_blue.gif)
[20180521-OpsUsageHeader.png](attachments/541033301/541197165.png)
(image/png) ![image3](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124\_OpsDash-FinishedYarn.png](attachments/541033301/541295434.png)
(image/png) ![image4](images/icons/bullet_blue.gif) [201804-18
TODO.rtf](attachments/541033301/541295438.rtf) (text/rtf)
![image5](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124\_OpsDash-RunningYarn.png](attachments/541033301/541295442.png)
(image/png) ![image6](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124\_OpsDash-AA-2.png](attachments/541033301/541393657.png)
(image/png) ![image7](images/icons/bullet_blue.gif)
[20180525-172.36.1.124-OpsUD-Infra-Expd
.png](attachments/541033301/541033315.png) (image/png)
![image8](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124\_OpsResourcesTile.png](attachments/541033301/541393661.png)
(image/png) ![image9](images/icons/bullet_blue.gif)
[20180525\_172.36.1.124-SelectTags.png](attachments/541033301/541393665.png)
(image/png) ![image10](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124\_4.3.1.3-OpsUD-Jobs.png](attachments/541033301/541328217.png)
(image/png) ![image11](images/icons/bullet_blue.gif)
[20180525-172.36.1.124-OpsUD-Infra-Zoom
.png](attachments/541033301/541131564.png) (image/png)
![image12](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124\_4.3.1.3-OpsUD-Nodes.png](attachments/541033301/541098777.png)
(image/png) ![image13](images/icons/bullet_blue.gif)
[20180525\_172.36.1.124-OpsUD-InfaExp.png](attachments/541033301/541295446.png)
(image/png) ![image14](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124\_OpsDash-IneffAppsDrillDown.png](attachments/541033301/541197169.png)
(image/png) ![image15](images/icons/bullet_blue.gif)
[20180525\_172.36.1.124-OpsUD-Infra-Zoom.png](attachments/541033301/541098781.png)
(image/png) ![image16](images/icons/bullet_blue.gif)
[1-Operation-Charts-KafkaWArrow.png](attachments/541033301/541295450.png)
(image/png) ![image17](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124\_4.3.1.3-OpsUD-ImpalaUsage.png](attachments/541033301/541361011.png)
(image/png) ![image18](images/icons/bullet_blue.gif)
[20180429\_172.36.1.115-Kafka-ClusterView.png](attachments/541033301/541164300.png)
(image/png) ![image19](images/icons/bullet_blue.gif)
[20180521-OpsUsageInfraHover.png](attachments/541033301/541131568.png)
(image/png) ![image20](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124-4.31.3-OpsUD-Infra.png](attachments/541033301/541295454.png)
(image/png) ![image21](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124\_OpsDash-IneffApps.png](attachments/541033301/541361015.png)
(image/png) ![image22](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124\_OpsDash-FY-App.png](attachments/541033301/541295458.png)
(image/png) ![image23](images/icons/bullet_blue.gif)
[2018-05-25\_172.36.1.124\_Operations.png](attachments/541033301/541229761.png)
(image/png) ![image24](images/icons/bullet_blue.gif)
[20180806\_124\_Ops-UsageDetail-Hdr.png](attachments/541033301/546046348.png)
(image/png) ![image25](images/icons/bullet_blue.gif)
[20180806\_124\_Ops-UsageDetail-Hdr.png](attachments/541033301/546046343.png)
(image/png) ![image26](images/icons/bullet_blue.gif)
[Ops-HBASECluster-List.png](attachments/541033301/545947981.png)
(image/png) ![image27](images/icons/bullet_blue.gif)
[Ops-HBASE-RegionServer.png](attachments/541033301/545947985.png)
(image/png) ![image28](images/icons/bullet_blue.gif)
[Ops-HBASE-Table.png](attachments/541033301/545947989.png) (image/png)
![image29](images/icons/bullet_blue.gif)
[Ops-HBASE-Opening.png](attachments/541033301/545915313.png) (image/png)
![image30](images/icons/bullet_blue.gif)
[4.4-OpsUsageDetails-Hdr.png](attachments/541033301/561775663.png)
(image/png) ![image31](images/icons/bullet_blue.gif)
[Refresh.png](attachments/541033301/561578763.png) (image/png)
![image32](images/icons/bullet_blue.gif)
[4.4-Op-UD-HbaseClusters.png](attachments/541033301/561873781.png)
(image/png) ![image33](images/icons/bullet_blue.gif)
[HBaseView-KPIs.png](attachments/541033301/575734444.png) (image/png)
![image34](images/icons/bullet_blue.gif)
[HBaseViewComplete.png](attachments/541033301/575668699.png) (image/png)
![image35](images/icons/bullet_blue.gif)
[ClusterViewAllClusters.png](attachments/541033301/575504906.png)
(image/png) ![image36](images/icons/bullet_blue.gif)
[ClusterView-RegServ.png](attachments/541033301/575603320.png)
(image/png) ![image37](images/icons/bullet_blue.gif)
[ClusterView-RegSerGraphs.png](attachments/541033301/575734464.png)
(image/png) ![image38](images/icons/bullet_blue.gif)
[ClusterViewTables.png](attachments/541033301/575668713.png) (image/png)
![image39](images/icons/bullet_blue.gif)
[HBase-RegionServer.png](attachments/541033301/575603337.png)
(image/png) ![image40](images/icons/bullet_blue.gif)
[MenuBars.png](attachments/541033301/541033311.png) (image/png)
![image41](images/icons/bullet_blue.gif)
[HBASE-Table.png](attachments/541033301/575603355.png) (image/png)
![image42](images/icons/bullet_blue.gif)
[Table-Region.png](attachments/541033301/575472216.png) (image/png)
![image43](images/icons/bullet_blue.gif)
[bad.png](attachments/541033301/575603378.png) (image/png)
![image44](images/icons/bullet_blue.gif) [Screen Shot 2018-09-16 at
22.53.06.png](attachments/541033301/575537734.png) (image/png)

</div>

</div>

</div>

</div>

<div id="footer" class="container">

<div class="container section footer-body">

Document generated by Confluence on Nov 02, 2018 15:16

<div id="footer-logo" class="container">

[Atlassian](http://www.atlassian.com/)

</div>

</div>

</div>

</div>
