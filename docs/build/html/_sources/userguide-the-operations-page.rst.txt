.. container::
   :name: page

   .. container:: aui-page-panel
      :name: main

      .. container::
         :name: main-header

         .. container::
            :name: breadcrumb-section

            #. `Unravel 4.4 <index.html>`__
            #. `Unravel 4.4 <Unravel-4.4_541197025.html>`__
            #. `User Guide <User-Guide_541295329.html>`__

         .. rubric:: Unravel 4.4 : The Operations Page
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified by Gopala Krishna G on Sep
            16, 2018

         .. container:: wiki-content group
            :name: main-content

            .. container:: panel

               .. container:: panelHeader

                  **Table of Contents**

               .. container:: panelContent

                  .. container:: toc-macro rbtoc1541196990382

                     -  `Dashboard <#TheOperationsPage-DashboardDashboard>`__

                        -  `Finished YARN applications
                           Tile <#TheOperationsPage-FinishedYARNapplicationsTile>`__
                        -  `Running YARN Application
                           Tile <#TheOperationsPage-RunningYARNApplicationTile>`__
                        -  `Resources
                           Tile <#TheOperationsPage-ResourcesTile>`__
                        -  `Inefficient Applications
                           Tile <#TheOperationsPage-InefficientApplicationsTile>`__
                        -  `Recent Events and Alerts
                           Sidebar <#TheOperationsPage-RecentEventsandAlertsSidebar>`__

                     -  `Usage
                        Details <#TheOperationsPage-ChartsUsageDetails>`__

                        -  `Infrastructure <#TheOperationsPage-InfrastructureInfrastructure>`__
                        -  `Jobs <#TheOperationsPage-JobsJobs>`__
                        -  `Nodes <#TheOperationsPage-NodesNodes>`__
                        -  `Impala
                           Usage <#TheOperationsPage-ImpalaImpalaUsage>`__
                        -  `Kafka <#TheOperationsPage-KafkaKafka>`__
                        -  `HBase <#TheOperationsPage-HBaseHBase>`__

            | 

            The Operations Page provides synopsis of your cluster(s) and
            its activities. It has two tabs:

            **Dashboard** and **Usage Detail**. By default it opens
            showing **Operations** \| **Dashboard** tab.

            .. container::
            confluence-information-macro has-no-icon confluence-information-macro-note

               Note

               .. container:: confluence-information-macro-body

                  Click `here <Common-UI-Features_541295593.html>`__ for
                  common features used throughout Unravel's UI.

            .. rubric:: Dashboard
               :name: TheOperationsPage-DashboardDashboard

            To view the Dashboard, click **Operations** \|
            **Dashboard**.

            The Dashboard provides an overview of cluster activities
            with links to drill down into YARN applications, resource
            usage, application inefficiencies, and events/alerts. By
            default it is configured to display *all* clusters *hourly*
            for the past *24 hours*.

            .. rubric:: Finished YARN applications Tile
               :name: TheOperationsPage-FinishedYARNapplicationsTile

            The line graphs display the successful, failed, and killed
            jobs for the *time period*, using the *time incremen*\ t and
            *cluster*\ (s) specified. It textually displays the total
            number over the time period.

            Clicking on\ ** Open Section** brings up the window
            **Applications \| Applications** window listing all
            applications. See\ `Finding
            Applications <The-Applications-Page_541164197.html>`__\ for
            more detail on this view.

            .. rubric:: Running YARN Application Tile
               :name: TheOperationsPage-RunningYARNApplicationTile
               :class: western

            The line graphs display the running and pending jobs for the
            current time. It textually displays the total number at the
            current time period.

            Clicking on\ ** Open Section** opens Operations \| Usage
            Details \| Jobs.
            See\  \ \ \ `here <#TheOperationsPage-ChartsJobs>`__\ \ \ \ for
            more information about this view.

            .. rubric:: Resources Tile
               :name: TheOperationsPage-ResourcesTile
               :class: western

            Displays the available and allocated VCore and Memory for
            the entire cluster.

            Clicking on\  Open Section opensOperations \| Usage Details
            \|
            Infrastructure.See\ `below <#TheOperationsPage-ChartsResources>`__\ \  for
            an example of this view.

            .. rubric:: Inefficient Applications Tile
               :name: TheOperationsPage-InefficientApplicationsTile
               :class: western

            Its three sub-tabs, **HIVE**, **MapReduce**, and **Spark;**
            each lists the inefficiencies that have occurred for that
            job type and the number of apps experiencing it. Click on
            the sub-tab to change the application type. Click on the
            **Event Name** to bring up the list of applications that
            experienced the event.

            The inefficiencies application is equal to
            the \ **Applications \| Applications** excepting that it
            only displays applications that have experienced the event.
            The event type is noted in the upper left hand corner.
            See\ `Finding
            Applications <The-Applications-Page_541164197.html>`__\ for
            more detail on this view.

            .. rubric:: Recent Events and Alerts Sidebar
               :name: TheOperationsPage-RecentEventsandAlertsSidebar
               :class: western

            The sidebar lists all events and alerts that have occurred
            organized by date and time. A separate entry appears for
            each time a particular Auto Action was triggered. In the
            image below, the same auto action triggered at 23:56 and
            2:58. Clicking an event/alert brings up a Cluster Resource
            view (**Operations \| Usage Detail \| Infrastructure**)
            displaying a 10 minute time slice (+-5 minutes of the event)
            and includes a list of applications running at that time.
            See `Auto Actions
            Overview <Auto-Actions-Overview_541131624.html#AutoActionsOverview-ClusterView>`__
            for more information. Click `Add a New AutoAction or
            Alert <Creating-Auto-Actions_541295518.html>`__ to create a
            new auto action.Click Clear to clear the list.

            .. rubric:: Usage Details
               :name: TheOperationsPage-ChartsUsageDetails

            `  <https://unraveldata.atlassian.net/wiki/spaces/UN44/pages/edit/541197498?draftId=541295828&draftShareId=6b31e169-5762-4397-8679-e3efd5351da4&>`__\ Usage
            Detail has four tabs:

            -  `Infrastructure <#TheOperationsPage-Infrastructure>`__
            -  `Jobs <#TheOperationsPage-Jobs>`__
            -  `Nodes <#TheOperationsPage-Nodes>`__ 
            -  `Impala Usage <#TheOperationsPage-ImpalaUsage>`__
            -  `Kafka <#TheOperationsPage-Kafk>`__
            -  `HBase <#TheOperationsPage-HBase>`__

            One of the most difficult challenges in managing
            multi-tenant Hadoop clusters is understanding how resources
            are being used by the applications running in the clusters.
            Through **Usage Details**, Unravel Web UI provides a unique
            forensic view into each cluster's key performance indicators
            (KPIs) over time and how they relate to the applications
            running in the cluster.

            |(lightbulb)| For example, Unravel can pinpoint the
            applications causing a sudden a spike in the total VCores or
            memory MB usage. This allows you to easily you drill down
            into these applications to understand their behavior.
            Whenever possible, Unravel provides `recommendations and
            insights <The-Applications-Page_541164197.html#TheApplicationsPage-EventPanel>`__
            to help improve the application(s) run. 

            All the charts and tables are automatically refreshed;
            however refreshing is disabled when you interact within a
            page to alter its display, e.g., change the date range,
            click on a point within in a graph. When disabled a
            **Refresh** button is displayed in Usage Details title bar.
            Click on **Refresh** to resume automatic refreshes.

            By default the Usage Details tab opens showing the
            Infrastructure tab.

            For all charts, click on the menu bars (), for print and
            download options, e.g. csv, jpeg. Click **Show more** to
            expand it. For a particular point in time, hovering over
            chart raises a popup with details while clicking all
            applications running. Once expanded click on the to return
            to the initial view. To zoom in drag over a section of the
            graph; to return to the complete graph click **Reset
            Graph**. The examples below are showing the Vcores-Total
            graph from the **Infrastructure** Tab expanded and then
            zoomed in a section..

            .. rubric:: Infrastructure
               :name: TheOperationsPage-InfrastructureInfrastructure

            This tab contains four (4) graphs.

            -  The upper two list available and allocated Vcores and
               memory for the entire Cluster, and
            -  The bottom show the Vcores and memory used by specific
               view, i.e., **Application Type**, **User**, **Queue**,
               and **Business** **Tags**.

            Clicking within a chart (1) displays the applications
            running for that point in time.

            You can chose how to display the bottom two graphs by
            clicking on the **View By** type (2). Once you have chosen
            your view, the first three (3) available values for that
            view are displayed in the **Showing** box (3), i.e., job
            types running, mapReduce, Spark. Above the box notes the
            number displayed of the total available values for
            the \ **View** type. Regardless the total available values
            by View By choice, you may only graph up to four (4). Click
            within the **Showing** box to see all available values; to
            remove a value click on the **x** next to its name (3). By
            default **Infrastructure** opens displaying **Application
            Type**. In the example below only one application type if
            available, Mapreduce. In the example
            `above <#TheOperationsPage-Infra-Vcores>`__ Vcores-Total was
            expanded (**show more**) and then zoomed in on a section.

            | 

            To **View by** tags, **use the Business Tags** pull down
            menu, which will display all available tags. The tag you
            chose is displayed in blue to left of the Business Tags. To
            add a tag, click in the **Showing** box to show all
            available tags, click on the tag to use it. You can only
            have up to four (4) tag values. Delete a particular value by
            clicking the x next to remove the value.

            .. rubric:: Jobs
               :name: TheOperationsPage-JobsJobs
               :class: western

            Graphs the running and accepted jobs as applicable. You can
            **Group by** State, App Type, User, and Queue. By default
            the chart uses State. You can change the display of an item
            via the Group By pull-down.

            .. rubric:: 
               Nodes
               :name: TheOperationsPage-NodesNodes
               :class: western

            This chart graphs the **Total** number of Nodes and the
            breakdown by node status, Active, Lost, Unhealthy,
            Decommissioned and Rebooted.

             **Total** = **Active** + **Unhealthy**

            Where:

            -  **Active: ** currently running and healthy nodes, and
            -  **Unhealthy:** currently running and unhealthy nodes.

            You can toggle the display of an item by clicking on its
            name.

            .. rubric:: Impala Usage
               :name: TheOperationsPage-ImpalaImpalaUsage
               :class: western

            Graphs memory MB consumption and Query Number. The **#
            Queries** graph can be displayed by **Tags** and **Group
            By** (User or Queue).

            .. rubric:: Kafka
               :name: TheOperationsPage-KafkaKafka
               :class: western

            Lists all the configured Kafka clusters. See `Kafka
            Application
            Manager <The-Applications-Page_541164197.html#TheApplicationsPage-KafkaAPM>`__
            for more information. See `Kafka Use
            Case <https://unraveldata.atlassian.net/wiki/spaces/UN43/pages/228753804/Kafka+Insights>`__
            for information on drilling down into a Cluster to locate
            lagging and stalled Topics/Partitions.

            Clicking the cluster name brings detailed information about
            the Kafka Cluster

            .. rubric:: HBase
               :name: TheOperationsPage-HBaseHBase
               :class: western

            .. container::
            confluence-information-macro confluence-information-macro-information

               .. container:: confluence-information-macro-body

                  Please see\ \ \ `HBase
                  Configuration <HBASE--Configuration_546537734.html>`__\ \ \ for
                  configuring Unravel UIX for HBase clusters.

            .. rubric:: Clusters View
               :name: TheOperationsPage-ClustersView

            Clusters page lists all the available HBase clusters

            Click on a cluster name to bring up the cluster's
            information the HBase Cluster view.

            .. rubric:: Cluster View
               :name: TheOperationsPage-ClusterView

            This view is divided into four (4) sections. When an
            component's health is noted, hovering over it's health glyph
            brings up details, .

            **Cluster Information**

            A bar at shows what cluster you are displaying with a
            pull-down which allows you to switch between clusters.
            Listed immediately below are the cluster metrics. You can
            choose to tab between clusters by choosing all cluster.
            Shown below is the tabbed view.

            **Region Servers
            **

            Lists the the cluster regional services, with their KPI's
            and health. You can search on the region server by name.
            Click on the server's name to bring up
            its\ `details. <#TheOperationsPage-RegionServ>`__

            Region Servers KPI

            Graphs the regional server metrics, the graphs are linked
            with the table list below them. Click within a graph to see
            up the tables associated servers that point in time. Hover
            over a point to bring up a popup displaying the information
            for that point in time. Click Show More the expand the graph
            with the table list to full window width. Click to print or
            download the graph.

            Tables

            List all the tables associated with the cluster, their KPIs
            and the table's health. Click on the table name to bring up
            its information. You can search for a table by name; any
            table with a name matching or containing the string is
            displayed.

            .. rubric:: Region Server View
               :name: TheOperationsPage-RegionServerRegionServerView

            Server, Operational, and OS Metrics are displayed. Hover
            over the metric for its description. For more information on
            the metrics
            see\ \ `here <HBASE--Alerts-and-Metrics_541197498.html#HBASEAlertsandMetrics-RegionServerMetric>`__\ \ .
            You can change the date range to display using the date
            picker. The first four (4) regional server metrics are
            graphed the daily. Use the pull down menu to the weekly
            view. Click to print or download the graph. The table list
            contains all the tables that have been accessed by this
            server. You can search the table list by name. Click on the
            table name to bring up its details.

            .. rubric:: Table View
               :name: TheOperationsPage-TableView
               :class: western

            Table has two tabs, **Table** and **Region**. It opens in
            the **Table** view, which displays the Table's KPIs. Three
            of the KPIs, regionCount, readRequestCount, and
            writeRequestCount are also graphed and linked with the
            application list below them. Click to print or download the
            graph.Hover over the graph to display the information for
            that point in time. Click within the graph to display the
            applications running at that point in time. Click on the
            application's name to open it in its application manager.

            .. rubric:: Table Regions
               :name: TheOperationsPage-TableRegions

            Lists all the regions with their KPIs and health.

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image1|
               `closeCross.png <attachments/541033301/541098773.png>`__
               (image/png)
               |image2|
               `20180521-OpsUsageHeader.png <attachments/541033301/541197165.png>`__
               (image/png)
               |image3|
               `2018-05-25_172.36.1.124_OpsDash-FinishedYarn.png <attachments/541033301/541295434.png>`__
               (image/png)
               |image4| `201804-18
               TODO.rtf <attachments/541033301/541295438.rtf>`__
               (text/rtf)
               |image5|
               `2018-05-25_172.36.1.124_OpsDash-RunningYarn.png <attachments/541033301/541295442.png>`__
               (image/png)
               |image6|
               `2018-05-25_172.36.1.124_OpsDash-AA-2.png <attachments/541033301/541393657.png>`__
               (image/png)
               |image7| `20180525-172.36.1.124-OpsUD-Infra-Expd
               .png <attachments/541033301/541033315.png>`__ (image/png)
               |image8|
               `2018-05-25_172.36.1.124_OpsResourcesTile.png <attachments/541033301/541393661.png>`__
               (image/png)
               |image9|
               `20180525_172.36.1.124-SelectTags.png <attachments/541033301/541393665.png>`__
               (image/png)
               |image10|
               `2018-05-25_172.36.1.124_4.3.1.3-OpsUD-Jobs.png <attachments/541033301/541328217.png>`__
               (image/png)
               |image11| `20180525-172.36.1.124-OpsUD-Infra-Zoom
               .png <attachments/541033301/541131564.png>`__ (image/png)
               |image12|
               `2018-05-25_172.36.1.124_4.3.1.3-OpsUD-Nodes.png <attachments/541033301/541098777.png>`__
               (image/png)
               |image13|
               `20180525_172.36.1.124-OpsUD-InfaExp.png <attachments/541033301/541295446.png>`__
               (image/png)
               |image14|
               `2018-05-25_172.36.1.124_OpsDash-IneffAppsDrillDown.png <attachments/541033301/541197169.png>`__
               (image/png)
               |image15|
               `20180525_172.36.1.124-OpsUD-Infra-Zoom.png <attachments/541033301/541098781.png>`__
               (image/png)
               |image16|
               `1-Operation-Charts-KafkaWArrow.png <attachments/541033301/541295450.png>`__
               (image/png)
               |image17|
               `2018-05-25_172.36.1.124_4.3.1.3-OpsUD-ImpalaUsage.png <attachments/541033301/541361011.png>`__
               (image/png)
               |image18|
               `20180429_172.36.1.115-Kafka-ClusterView.png <attachments/541033301/541164300.png>`__
               (image/png)
               |image19|
               `20180521-OpsUsageInfraHover.png <attachments/541033301/541131568.png>`__
               (image/png)
               |image20|
               `2018-05-25_172.36.1.124-4.31.3-OpsUD-Infra.png <attachments/541033301/541295454.png>`__
               (image/png)
               |image21|
               `2018-05-25_172.36.1.124_OpsDash-IneffApps.png <attachments/541033301/541361015.png>`__
               (image/png)
               |image22|
               `2018-05-25_172.36.1.124_OpsDash-FY-App.png <attachments/541033301/541295458.png>`__
               (image/png)
               |image23|
               `2018-05-25_172.36.1.124_Operations.png <attachments/541033301/541229761.png>`__
               (image/png)
               |image24|
               `20180806_124_Ops-UsageDetail-Hdr.png <attachments/541033301/546046348.png>`__
               (image/png)
               |image25|
               `20180806_124_Ops-UsageDetail-Hdr.png <attachments/541033301/546046343.png>`__
               (image/png)
               |image26|
               `Ops-HBASECluster-List.png <attachments/541033301/545947981.png>`__
               (image/png)
               |image27|
               `Ops-HBASE-RegionServer.png <attachments/541033301/545947985.png>`__
               (image/png)
               |image28|
               `Ops-HBASE-Table.png <attachments/541033301/545947989.png>`__
               (image/png)
               |image29|
               `Ops-HBASE-Opening.png <attachments/541033301/545915313.png>`__
               (image/png)
               |image30|
               `4.4-OpsUsageDetails-Hdr.png <attachments/541033301/561775663.png>`__
               (image/png)
               |image31|
               `Refresh.png <attachments/541033301/561578763.png>`__
               (image/png)
               |image32|
               `4.4-Op-UD-HbaseClusters.png <attachments/541033301/561873781.png>`__
               (image/png)
               |image33|
               `HBaseView-KPIs.png <attachments/541033301/575734444.png>`__
               (image/png)
               |image34|
               `HBaseViewComplete.png <attachments/541033301/575668699.png>`__
               (image/png)
               |image35|
               `ClusterViewAllClusters.png <attachments/541033301/575504906.png>`__
               (image/png)
               |image36|
               `ClusterView-RegServ.png <attachments/541033301/575603320.png>`__
               (image/png)
               |image37|
               `ClusterView-RegSerGraphs.png <attachments/541033301/575734464.png>`__
               (image/png)
               |image38|
               `ClusterViewTables.png <attachments/541033301/575668713.png>`__
               (image/png)
               |image39|
               `HBase-RegionServer.png <attachments/541033301/575603337.png>`__
               (image/png)
               |image40|
               `MenuBars.png <attachments/541033301/541033311.png>`__
               (image/png)
               |image41|
               `HBASE-Table.png <attachments/541033301/575603355.png>`__
               (image/png)
               |image42|
               `Table-Region.png <attachments/541033301/575472216.png>`__
               (image/png)
               |image43|
               `bad.png <attachments/541033301/575603378.png>`__
               (image/png)
               |image44| `Screen Shot 2018-09-16 at
               22.53.06.png <attachments/541033301/575537734.png>`__
               (image/png)

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Nov 02, 2018 15:16

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

.. |(lightbulb)| image:: images/icons/emoticons/lightbulb_on.png
   :class: emoticon emoticon-light-on
.. |image1| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image2| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image3| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image4| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image5| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image6| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image7| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image8| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image9| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image10| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image11| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image12| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image13| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image14| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image15| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image16| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image17| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image18| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image19| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image20| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image21| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image22| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image23| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image24| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image25| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image26| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image27| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image28| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image29| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image30| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image31| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image32| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image33| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image34| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image35| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image36| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image37| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image38| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image39| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image40| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image41| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image42| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image43| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image44| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
