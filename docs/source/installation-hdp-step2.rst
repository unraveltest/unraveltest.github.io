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
            #. `Installation
               Guides <Installation-Guides_541393730.html>`__
            #. `Hortonworks Data Platform (HDP) <541197289.html>`__

         .. rubric:: Unravel 4.4 : Step 2: Enable Additional Data
            Collection / Instrumentation for HDP
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified by Grant Liu on Oct 29, 2018

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: Introduction
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-Introduction

            This topic explains how to configure Unravel Sensor for Tez
            (``unravel_us``) to retrieve data from the Tez execution
            engine. Retrievable data includes Hive queries, application
            timelines, sensor agent data, YARN RM data, logs, and so on.

            .. container::
            confluence-information-macro confluence-information-macro-information

               .. container:: confluence-information-macro-body

                  ``HIGHLIGHTED``\ text and text with brackets ( { } )
                  indicate where you must substitute your particular
                  values for the text.

                  ``UNRAVEL_HOST_IP``\ must be a fully qualified DNS or
                  an IP address.

            .. rubric:: 1. Convert Your Unravel Installation to HDP.
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-1.ConvertYourUnravelInstallationtoHDP.

            .. container::

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo /etc/init.d/unravel_all.sh stop
                        # sudo /usr/local/unravel/install_bin/switch_to_hdp.sh

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     Note: This change remains after RPM upgrades.

            .. rubric:: 2. Update Site-Specific HDP Properties in
               unravel.properties
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-2.UpdateSite-SpecificHDPPropertiesinunravel.properties

            -  Add these properties
               ``to/usr/local/unravel/etc/unravel.properties``:

               .. container:: table-wrap

                  +--------------------------------+--------------------------+---------+
                  | Property                       | Description              | Default |
                  |                                |                          | Value   |
                  +================================+==========================+=========+
                  | ``com.unraveldata.yarn.timelin | The http address of the  | http:// |
                  | e-service.webapp.address``     | Timeline service web     | localho |
                  |                                | application              | st      |
                  +--------------------------------+--------------------------+---------+
                  | ``com.unraveldata.yarn.timelin | Timeline service port    | 8188    |
                  | e-service.port``               |                          |         |
                  +--------------------------------+--------------------------+---------+

               For example,

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        com.unraveldata.yarn.timeline-service.webapp.address=http://172.16.1.101
                        com.unraveldata.yarn.timeline-service.port=8188

            -   Open ``/usr/local/unravel/etc/unravel.properties`` to
               see if you need to modify any of these properties which
               were added or modified by the script above
               (``switch_to_hdp.sh``).

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        // Repoint Unravel application logs directory
                        com.unraveldata.job.collector.done.log.base=/mr-history/done
                        com.unraveldata.job.collector.log.aggregation.base=/app-logs/*/logs/
                        com.unraveldata.spark.eventlog.location=hdfs:///spark-history/
                         
                        // Add Hive Metastore database information for Unravel Hive Config 
                        javax.jdo.option.ConnectionURL=jdbc:mysql://{hostname}:3306/{database_name} 
                        javax.jdo.option.ConnectionDriverName=com.mysql.jdbc.Driver
                        javax.jdo.option.ConnectionUserName={HiveMetastoreUserName} 
                        javax.jdo.option.ConnectionPassword={HiveMetastorePassword}

            -  Log into Ambari Web UI (AWU) to verify the above
               properties have been set correctly in
               ``unravel.properties``. Update the value of any property
               which has not been set correctly.

               -  On the left-hand side of AWU's dashboard, click
                  MapReduce2 \| Configs. Select the Advanced tab, and
                  then select\ **Advanced mapred-site**

                  -  Verify
                     ``com.unraveldata.job.collector.done.log.base``
                     equals **mapreduce.jobhistory.done-dir.**

               -  On the left-hand side of AWU's dashboard, click YARN
                  \| Configs. Select the Advanced tab, and then
                  select\ **Node Manager**.

                  -  Verify
                     ``com.unraveldata.job.collector.done.log.``\ ``aggregation.base``
                     equals **yarn.nodemanager.remote-app=log-dir**.

               -  On the left-hand side of AWU's dashboard, click Hive
                  \| Configs. Select the Advanced tab, and then
                  select\ **Hive Metastore**.

                  -  Verify
                     ``javax.jdo.option.ConnectionURL``\ equals jdbc:mysql://\ **Database
                     Host**:3306/**Database URL**.
                  -  Verify
                     ``javax.jdo.option.ConnectionDriverName``\ equals
                     **JDBC Driver Class**\ .
                  -  Verify
                     ``javax.jdo.option.ConnectionUserName``\ equals
                     **Database** **Username**.

            .. rubric:: 3. Start Unravel Server
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-3.StartUnravelServer

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     Note: Unravel must be up for the next step to
                     complete.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo /etc/init.d/unravel_all.sh start

            .. rubric:: 4. Install Unravel Hive Hook and Spark Sensor
               Onto HDP Servers
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-4.InstallUnravelHiveHookandSparkSensorOntoHDPServers

            .. container::

               Install Unravel Hive Hook and Spark Sensor onto HDP
               servers (JAR files) as follows (substitute the correct
               fully qualified host name or IP address
               for \ ``UNRAVEL_HOST_IP``).

            -  Login as root and ensure ``wget`` is installed

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # /usr/local/unravel/install_bin/unraveldata-clients/unravel_hdp_setup.sh 

            -  From Unravel server (eg. edge node) run on each server
               that will use instrumentation.

               .. container::
               confluence-information-macro confluence-information-macro-warning

                  .. container:: confluence-information-macro-body

                     Be sure to substitute valid values.

                     ``SPARK_VERSION_X.Y.Z``\ has the following possible
                     value: 1.3.0, 1.5.0, 1.6.0, 2.0.0, 2.1.0, 2.2.0 and
                     2.3.0 for Spark
                     versions 1.3.x, 1.5.x,  1.6.x, 2.0.x,  2.1.x,
                     2.2.x, and 2.3.x respectively.

                     ``HIVE_VERSION``\  must be a Hive version that
                     Unravel Supports, i.e., 1.2.0 or 0.13.0.  If you
                     are using Hive 1.2.1, please use \ 1.2.0 for the
                     Hive version.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # yum install -y wget
                        # cd /usr/local/unravel/install_bin/unraveldata-clients/
                        # sudo ./unravel_hdp_setup.sh install -y --unravel-server {UNRAVEL_HOST_IP}:3000 --hive-version {HIVE_VERSION} --spark-version {SPARK_VERSION}

            -  Files are installed locally on the edge node under:

               -  ``/usr/local/unravel_client (Hive hook jar)``
               -  ``/usr/local/unravel-agent/jars/ (Resource metrics sensor jars)``

               | 

            -  Manually copy these two directories on all nodes in the
               cluster (worker / edge / master). In all cases,
               instrumented nodes must be able to open port 4043 of
               Unravel Server (``host2`` if multi-host Unravel install)

            .. rubric:: 5. Add Unravel Hive Hook hive-site Settings to
               All of HDP's Servers in the Cluster Using AWU
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-5.AddUnravelHiveHookhive-siteSettingstoAllofHDP'sServersintheClusterUsingAWU

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-warning

                  .. container:: confluence-information-macro-body

                     Completion of this step requires a restart of all
                     affected Hive services in Ambari UI.

            -  In AWU, on the left-hand side, click **Hive** \|
               **Configs**. Select the **Advanced** tab, and then select
               **Custom hive-site** and **Add Property** below and use
               **Bulk property add mode.**

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        hive.exec.driver.run.hooks=com.unraveldata.dataflow.hive.hook.HiveDriverHook 
                        com.unraveldata.hive.hdfs.dir=/user/unravel/HOOK_RESULT_DIR 
                        com.unraveldata.hive.hook.tcp=true 
                        com.unraveldata.host={add unravel gateway internal IP hostname} 
                         
                        // Find below properties as it may already exists, concatenate it with a comma & no spaces
                        hive.exec.pre.hooks=com.unraveldata.dataflow.hive.hook.HivePreHook 
                        hive.exec.post.hooks=com.unraveldata.dataflow.hive.hook.HivePostHook 
                        hive.exec.failure.hooks=com.unraveldata.dataflow.hive.hook.HiveFailHook

               -  If LLAP is enabled copy the above settings in **Custom
                  hive-interactive-site** as well.

                  .. container::
                  confluence-information-macro confluence-information-macro-tip

                     Manual edit hive-site.xml (no AWU)

                     .. container:: confluence-information-macro-body

                        ``hive-site.xml``\ file is located at
                        ``/etc/hive/conf/``.

            -  Add ``AUX_CLASSPATH`` to AWU's **hive-env** template:

               -  In AWU, on the left-hand side, click **Hive**, next
                  click **Configs**, go to the **Advanced** tab, and
                  select/click **Advanced hive-env**.
               -  Next, inside the **Advanced hive-env** template,
                  towards the end of line add below:

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           export AUX_CLASSPATH=${AUX_CLASSPATH}:/usr/local/unravel_client/unravel-hive-1.2.0-hook.jar

               -  If LLAP is enabled copy above line of code into
                  **Advanced hive-interactive-env** as well.

                  .. container::
                  confluence-information-macro confluence-information-macro-tip

                     You can manually edit hive-env.sh without using
                     AWU.

                     .. container:: confluence-information-macro-body

                        The ``hive-env.sh`` file is located at
                        ``/etc/hive/conf/``.

            -  Add ``HADOOP_CLASSPATH`` to AWU's **hadoop-env**
               template:

               -  In AWU, on the left-hand side, click **HDFS**, next
                  click **Configs**, go to the **Advanced** tab, and
                  select/click **Advanced hadoop-env**.
               -  Next, inside the **hadoop-env** template, look
                  for \ **export HADOOP_CLASSPATH** and append unravel's
                  jar path like below:

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           export HADOOP_CLASSPATH=${HADOOP_CLASSPATH}:/usr/local/unravel_client/unravel-hive-1.2.0-hook.jar

            .. rubric:: 6. **Optionally for Tez**, Enable Unravel Tez
               Instrumentation on all of HDP's Services in the Cluster
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-6.OptionallyforTez,EnableUnravelTezInstrumentationonallofHDP'sServicesintheCluster

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-warning

                  .. container:: confluence-information-macro-body

                     Completion of this step requires a restart of all
                     affected Hive services in Ambari UI.

            Confirm that hive-execution.engine is set to tez.

            .. container:: preformatted panel

               .. container:: preformattedContent panelContent

                  ::

                     set hive.execution.engine=tez;

            Using the Ambari Web UI (AWU), configure the Btrace agent
            for Tez:

            -  Append\ ``-javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=libs=mr,config=tez -Dunravel.server.hostport=UNRAVEL_HOST_IP:4043``\ to
               tez.am.launch.cmd-opts and tez.task.launch.cmd-opts.

            -  Restart the affected component(s). The screenshot below
               illustrates this change.

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     In a Kerberos environment you need to
                     modify \ tez.am.view-acls property with the RUN_AS
                     user or \*.

            .. rubric:: 7. **Optionally for Spark on YARN**.
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-7.OptionallyforSparkonYARN.

            Enable Unravel Spark Instrumentation on All of HDP's Servers
            in the Cluster

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-warning

                  .. container:: confluence-information-macro-body

                     Completion of this step requires a restart of all
                     affected Spark services in Ambari UI.

                     Be sure to substitute valid
                     values.for \ ``UNRAVEL_HOST_IPX``
                     and \ ``SPARK_VERSION_X.Y.``

                      ``SPARK_VERSION_X.Y``  has the following possible
                     values: \ **``spark-1.3``**\  for Spark
                     1.3.x, \ **``spark-1.5``\  **\ for Spark
                     1.5.x, \ **``spark-1.6``\  **\ for Spark 1.6.x,
                      \ **``spark-2.0``\  **\ for Spark 2.0.x,
                     spark-\ ``2.2``\  \ for Spark 2.2.x, and
                     spark-\ ``2.3``\  \ for Spark 2.3.x

            -  Add Spark properties into AWU's **Custom
               spark-defaults**:

               -  In AWU, on the left-hand side, click **Spark** \|
                  **Configs** \| **Custom spark-defaults**.

            -  

               .. container::
               confluence-information-macro confluence-information-macro-tip

                  You can manually edit spark-defaults.conf without
                  using AWU.

                  .. container:: confluence-information-macro-body

                     The default location for \ ``spark-defaults.conf``
                     is \ ``/usr/hdp/current/SPARK_VERSION_X.Y`` except
                     when

                     -  The cluster only has one spark 1.X
                        version:  \ ``/usr/hdp/current/spark-client/conf``
                     -  For spark 2.X
                        version: \ ``/usr/hdp/current/spark2-client/conf``

                        Inside **Custom spark-defaults**, click **Add
                        Property** for Spark as follows and use  **Bulk
                        property add mode**:

                        .. container:: code panel pdl

                           .. container:: codeContent panelContent pdl

                              .. code:: syntaxhighlighter-pre

                                 spark.unravel.server.hostport={UNRAVEL_HOST_IP}:4043
                                 spark.driver.extraJavaOptions=-javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=config=driver,libs={SPARK_VERSION_X.Y}
                                 spark.executor.extraJavaOptions=-javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=config=executor,libs={SPARK_VERSION_X.Y} 
                                 spark.eventLog.enabled=true

            .. rubric:: 8. \ **Optionally for MapReduce2** (MR)
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-8.OptionallyforMapReduce2(MR)

            .. rubric:: JVM Sensor Cluster-Wide
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-JVMSensorCluster-Wide

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-warning

                  .. container:: confluence-information-macro-body

                     Completion of this step will require a restart of
                     all affected HDFS, MAPREDUCE2, YARN andHIVE
                     services in Ambari UI.

            -  In AWU, on the left-hand side, click \ **MapReduce2**,
               next click \ **Configs**, go to the \ **Advanced** tab,
               and select/click **Advanced \ mapred-site**

            -  Search for MR AppMaster Java Heap Size property and
               concatenate by copying and pasting the below property for
               ``current.yarn.app.mapreduce.am.command-opts``MR JVM
               sensor. Be sure to leave a white space in-between the
               current and the following new property.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                          -javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=libs=mr -Dunravel.server.hostport={UNRAVEL_HOST_IP}:4043

            -  On the top notification banner, click Save

            -  In AWU, on the left-hand side, click **MapReduce2**, next
               click \ **Configs**, go to the \ **Advanced** tab, and
               select/click \ **Custom mapred-site:**

            -  Inside \ **Custom mapred-site**, click \ **Add
               Property** for MR JVM sensor as follows and use  **Bulk
               property add mode**:
            -  On the top notification banner, click Save

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        mapreduce.task.profile=true
                        mapreduce.task.profile.maps=0-5
                        mapreduce.task.profile.reduces=0-5
                        mapreduce.task.profile.params=-javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=libs=mr -Dunravel.server.hostport=UNRAVEL_HOST_IP:4043

               Notice that in this code block, the blank lines separate
               single full lines of text that are wrapped due to length.
               Also, ensure you replace ``UNRAVEL_HOST_IP``\  with your
               Unravel server's IP address.

               .. container::
               confluence-information-macro confluence-information-macro-tip

                  You can manually edit mapred-site.xml without using
                  AWU.

                  .. container:: confluence-information-macro-body

                     The ``mapred-site.xml`` file is located at
                     ``/etc/hadoop/conf/``.

            -  Propagate Unravel resource metrics sensor jaronto all the
               servers in the clusteras follows\ **:**

               .. container::
               confluence-information-macro confluence-information-macro-note

                  .. container:: confluence-information-macro-body

                     If you have already run the
                     ``unravel_hdp_setup.sh`` script to distribute
                     sensor; you can skip the following steps

            -  ssh to the Unravel gateway host server and use guided
               steps to unzip the Unravel MR jars.

               -  With root or sudo access, change directory to
                  ``/usr/local/unravel-agent`` and run the below
                  ``curl`` get command:

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           # cd /usr/local/unravel-agent
                           # curl http://localhost:3000/hh/unravel-agent-pack-bin.zip -o unravel-agent-pack-bin.zip
                           # unzip -d jars unravel-agent-pack-bin.zip

                  .. container::
                  confluence-information-macro confluence-information-macro-tip

                     .. container:: confluence-information-macro-body

                        Ensure you have already installed ``unzip``,
                        ``curl``, and Unravel port #\ ``3000`` must be
                        open

               -  Tar up the \ ``/usr/local/unravel-agent`` directory,
                  and propagate to all the servers in the HDP cluster

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           # cd /usr/local/
                           # tar -cvf unravel-agent.tar  ./unravel-agent

               -  Copy the ``unravel-agent.tar`` file to all your
                  servers, and ``untar`` to \ ``/usr/local`` directory
                  because when you \ ``untar`` ``unravel-agent``
                  directory will appear

            .. rubric:: 9. Optionally for YARN
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-9.OptionallyforYARN

            -  If \ ``yarn.acl.enable=true`` then you need to grant
               Unravel access and must set one of the following:

               -  ``yarn.acl.enable=false,``\ or
               -  ``yarn.admin.acl=userName``  (set to \* to allow
                  access to everyone).

            .. rubric:: 10. Confirm that Unravel Web UI Shows Tez Data
               :name: Step2:EnableAdditionalDataCollection/InstrumentationforHDP-10.ConfirmthatUnravelWebUIShowsTezData

            -  Run
               ``/usr/local/unravel/install_bin/hive_test_simple.sh`` on
               HDP cluster or any cloud environment where
               ``hive.execution.engine=tez``.
            -  Check Unravel Web UI for Tez data. For instructions,
               see\ `Tez Application
               Manager <https://unraveldata.atlassian.net/wiki/spaces/UN44/pages/541164197/The+Applications+Page#TheApplicationsPage-TezAPMTezApplicationManager>`__\ .

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     Unravel Web UI may take a few seconds to load Tez
                     data.

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image0|
               `bulk.png <attachments/561709534/561709537.png>`__
               (image/png)
               |image1|
               `hdp_part2_image_002.PNG <attachments/561709534/561709540.png>`__
               (image/png)
               |image2|
               `hdp_part2_image_001.PNG <attachments/561709534/561709543.png>`__
               (image/png)
               |image3|
               `image2017-10-25_8-1-4.png <attachments/561709534/561709546.png>`__
               (image/png)

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Nov 02, 2018 15:15

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

.. |image0| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image1| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image2| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image3| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
