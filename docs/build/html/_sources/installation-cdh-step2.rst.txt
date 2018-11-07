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
            #. `Cloudera Distribution Including Apache Hadoop (CDH),
               with Cloudera Manager (CM) <541361096.html>`__

         .. rubric:: Unravel 4.4 : Step 2: Install Unravel Sensor Parcel
            on CDH+CM
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified on Sep 10, 2018

         .. container:: wiki-content group
            :name: main-content

            | 

            .. container:: panel

               .. container:: panelHeader

                  **Table of Contents**

               .. container:: panelContent

                  .. container:: toc-macro rbtoc1541196937543

                     -  `Introduction <#Step2:InstallUnravelSensorParcelonCDH+CM-_17zqoqqgx4oqIntroduction>`__
                     -  `1. Obtain and Distribute the Parcel from
                        Unravel
                        Server <#Step2:InstallUnravelSensorParcelonCDH+CM-1.ObtainandDistributetheParcelfromUnravelServer>`__
                     -  `2. Put the Hive Hook JAR in
                        AUX_CLASSPATH <#Step2:InstallUnravelSensorParcelonCDH+CM-2.PuttheHiveHookJARinAUX_CLASSPATH>`__

                        -  `If Sentry is
                           Enabled: <#Step2:InstallUnravelSensorParcelonCDH+CM-IfSentryisEnabled:>`__

                     -  `3. Configure the Gateway Automatic Deployment
                        of Hive
                        Instrumentation <#Step2:InstallUnravelSensorParcelonCDH+CM-ConfigureGateway3.ConfiguretheGatewayAutomaticDeploymentofHiveInstrumentation>`__

                        -  `Set the hive-site.xml Snippet in Cloudera
                           Manager, and Deploy the Hive Client
                           Configuration to
                           Gateways <#Step2:InstallUnravelSensorParcelonCDH+CM-Setthehive-site.xmlSnippetinClouderaManager,andDeploytheHiveClientConfigurationtoGateways>`__
                        -  `Check Unravel Web
                           UI <#Step2:InstallUnravelSensorParcelonCDH+CM-CheckUnravelWebUI>`__

                     -  `4. Configure the Gateway Automatic Deployment
                        of Spark
                        Instrumentation <#Step2:InstallUnravelSensorParcelonCDH+CM-4.ConfiguretheGatewayAutomaticDeploymentofSparkInstrumentation>`__
                     -  `5. Optional - Configure YARN - MapReduce (MR)
                        JVM Sensor
                        Cluster-Wide <#Step2:InstallUnravelSensorParcelonCDH+CM-ConfigureJVMSensorForYARN5.Optional-ConfigureYARN-MapReduce(MR)JVMSensorCluster-Wide>`__
                     -  `6. (Optional) Advanced
                        Configuration <#Step2:InstallUnravelSensorParcelonCDH+CM-6.(Optional)AdvancedConfiguration>`__
                     -  `Troubleshooting <#Step2:InstallUnravelSensorParcelonCDH+CM-_troubleshootingTroubleshooting>`__
                     -  `References <#Step2:InstallUnravelSensorParcelonCDH+CM-References>`__

            | 

            .. rubric:: Introduction
               :name: Step2:InstallUnravelSensorParcelonCDH+CM-_17zqoqqgx4oqIntroduction

            This topic explains how to install the Unravel Sensor parcel
            on CDH clusters using Cloudera Manager (CM). The parcel
            includes Hive Hook and Spark instrumentation JARs. Hive Hook
            is used to collect information about Hive queries in Hadoop.
            The Spark instrumentation JARs measure Spark job
            performance.

            These instructions apply to Unravel Sensor 4.3

            Before following these instructions, follow the steps in
            `Step 1: Install Unravel Server on
            CDH+CM <https://unraveldata.atlassian.net/wiki/spaces/UN43/pages/226197657>`__.

            .. container:: panel

               .. container:: panelHeader

                  **Workflow Summary**

               .. container:: panelContent

                  #. Obtain and distribute the parcel from Unravel
                     Server.
                  #. Put the Hive Hook JAR in ``AUX_CLASSPATH``.

                  #. Configure the gateway automatic deployment of Hive
                     instrumentation.

                  #. Configure the gateway automatic deployment of Spark
                     instrumentation.

                  .. container::
                  confluence-information-macro confluence-information-macro-information

                     .. container:: confluence-information-macro-body

                        ``HIGHLIGHTED ``\ text and text with brackets (
                        { } ), except where otherwise noted, indicate
                        where you must substitute your particular values
                        for the text.

                        When Active Directory Kerberos is
                        used,\ ``UNRAVEL_HOST_IP``\ must be a fully
                        qualified path or IP address.

            | 

            .. container::
            confluence-information-macro confluence-information-macro-information

               To Upgrade the Unravel Sensor

               .. container:: confluence-information-macro-body

                  -  Check the \ ``UNRAVEL_SENSOR`` parcel status in
                     Cloudera Manager.

                  -  If an upgrade is available complete `steps 3
                     through
                     5 <#Step2:InstallUnravelSensorParcelonCDH+CM-ConfigureGateway>`__
                     to perform the upgrade.

            .. rubric:: 1. Obtain and Distribute the Parcel from Unravel
               Server
               :name: Step2:InstallUnravelSensorParcelonCDH+CM-1.ObtainandDistributetheParcelfromUnravelServer

            #. In Cloudera Manager (CM), go to the **Parcels** page by
               clicking the parcels glyph () on the top of the page.
            #. Click **Configuration** to see the **Parcel Settings**
               pop-up.
            #. In the **Remote Parcel Repository URLs** section of the
               **Parcel Settings** pop-up, click the **+** glyph to add
               a new entry.
            #. Add ``http://{UNRAVEL_HOST_IP``}:3000/parcels/cdh{X.Y}/
               (include trailing slash) where ``X.Y`` is the major,
               minor version of CDH version you are running and
               ``UNRAVEL_HOST_IP`` is the host name or LAN IP address of
               Unravel Server where ``unravel_lr`` is running. If you
               are running more than one version of CDH (multiple
               clusters) in Cloudera Manager, you can add more than one
               parcel directory from the ``UNRAVEL_HOST_IP``.

            #. Click **Save**.
            #. Click **Check for New Parcels**.
            #. On the **Parcels** page, pick a target cluster in the
               **Location** box.
            #. In the list of **Parcel Names**, find the
               ``UNRAVEL_SENSOR`` parcel that matches the version of the
               target cluster and click **Download**.
            #. Click **Distribute**.
            #. If you have an old parcel from Unravel, you can
               deactivate it now.
            #. Then click **Activate** on the new parcel.

            .. rubric:: 2. Put the Hive Hook JAR in AUX_CLASSPATH
               :name: Step2:InstallUnravelSensorParcelonCDH+CM-2.PuttheHiveHookJARinAUX_CLASSPATH

            #. In Cloudera Manager, for the target cluster, click
               **Hive** \| **Configuration**, and search for
               ``hive-env``.
            #. In **Gateway Client Environment Advanced Configuration
               Snippet (Safety Valve)** for ``hive-env.sh`` enter the
               following exactly as shown, with no subsitutions:

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        AUX_CLASSPATH=${AUX_CLASSPATH}:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/unravel_hive_hook.jar

            #. In Cloudera Manager, click **YARN** \| **Configuration**,
               and search for ``hadoop-env``.
            #. In **Gateway Client Environment Advanced Configuration
               Snippet (Safety Valve)** for ``hadoop-env.sh``, enter the
               following exactly as shown, with **no subsitutions**:

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        HADOOP_CLASSPATH=${HADOOP_CLASSPATH}:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/unravel_hive_hook.jar

            .. rubric:: **If Sentry is Enabled:**
               :name: Step2:InstallUnravelSensorParcelonCDH+CM-IfSentryisEnabled:

            Sentry commands may also be needed to enable access to the
            Hive Hook JAR file. Grant privileges on the JAR files to the
            roles that run hive queries. Log into Beeline as user
            ``hive`` and use the Hive ``SQL GRANT`` statement to do so.
            For example (substitute ``{ROLE}`` as appropriate):

            .. container::

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # GRANT ALL ON URI 'file:///opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/unravel_hive_hook.jar' TO ROLE {ROLE}

            .. rubric:: 3. Configure the Gateway Automatic Deployment of
               Hive Instrumentation
               :name: Step2:InstallUnravelSensorParcelonCDH+CM-ConfigureGateway3.ConfiguretheGatewayAutomaticDeploymentofHiveInstrumentation

            Use Cloudera Manager to deploy the ``hive-site.xml``
            snippet, which is the content of
            ``/usr/local/unravel/hive-hook/hive-site.xml.snip`` on
            Unravel Server.

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     On a multi-host Unravel Server deployment, use
                     host2's \ ``/usr/local/unravel/hive-hook/hive-site.xml.snip``

               .. rubric:: Set the ``hive-site.xml`` Snippet in Cloudera
                  Manager, and Deploy the Hive Client Configuration to
                  Gateways
                  :name: Step2:InstallUnravelSensorParcelonCDH+CM-Setthehive-site.xmlSnippetinClouderaManager,andDeploytheHiveClientConfigurationtoGateways

               In Cloudera Manager (CM):

               #. Go to Hive service.
               #. Select the **Configuration** tab.
               #. Search for ``hive-site.xml`` in the middle of the
                  page.
               #. Add the xml snippet to **Hive Client Advanced
                  Configuration Snippet for ``hive-site.xml``** (Gateway
                  Default Group) (Click **View as XML**). 

                  .. container::
                  confluence-information-macro confluence-information-macro-warning

                     .. container:: confluence-information-macro-body

                        If cluster has been configured with "Cloudera
                        Navigator"; the ``hive.exec.post.hooks``
                        property will have exsiting value(s). Therefore
                        append the unravel's value into the
                        existing \ ``hive.exec.post.hooks`` property
                        with a comma and no space. (see example below) 

                        ``com.cloudera.navigator.audit.hive.HiveExecHookContext,org.apache.hadoop.hive.ql.hooks.LineageLogger, com.unraveldata.dataflow.hive.hook.HivePostHook``

                        | **IMPORTANT!** The "Hive Client Advanced
                          Configuration Snippet for hive-site.xml"
                          should only have the Unravel class.
                        | However, the "HiveServer2 Advanced
                          Configuration Snippet for hive-site.xml"
                          should have all 3 classes: 2 from Navigator
                          and 1 from Unravel.

               #. Add the xml snippet to **HiveServer2 Advanced
                  Configuration Snippet for** ``hive-site.xml``. (Click
                  **View as XML**). Like above step, if properties
                  exists, append unravel's value.
               #. Save the changes with optional comment "Unravel
                  snippet in ``hive-site.xml``\ **"
                  **
               #. Perform action **Deploy Hive Client Configuration** by
                  clicking the deploy glyph () or by using the
                  **Actions** pull-down menu.
               #. Restart the Hive service. (Cloudera Manager will
                  specify a restart which is not necessary for
                  activating these changes. You may act on CM's
                  recommendation at a later time. )

               Again, monitor the situation to see if all Hive queries
               are failing with a class not found or permission
               problems. **If they are failing**, you can back-out the
               ``hive-site.xml`` advanced snippet changes in Cloudera
               Manager, deploy client configuration, and restart the
               Hive service to revert, then refer to
               `Troubleshooting <https://unraveldata.atlassian.net/wiki/pages/resumedraft.action?draftId=53649678#Part2:InstallUnravelSensorParcelonCDH+CM-_troubleshooting>`__
               below.

               .. rubric:: Check Unravel Web UI
                  :name: Step2:InstallUnravelSensorParcelonCDH+CM-CheckUnravelWebUI

               If queries are running fine and appearing in Unravel Web
               UI, then you are done.

            .. rubric:: 4. Configure the Gateway Automatic Deployment of
               Spark Instrumentation
               :name: Step2:InstallUnravelSensorParcelonCDH+CM-4.ConfiguretheGatewayAutomaticDeploymentofSparkInstrumentation

            #. In Cloudera Manager, select the target cluster, then
               select the Spark service.
            #. Select **Configuration**.
            #. Search for "``spark-defaults``".
            #. In the \ **Spark Client Advanced Configuration Snippet
               (Safety Valve) for spark-conf/spark-defaults.conf**,
               enter the following text, substituting the highlighted
               text for your particular values:

               .. container::
               confluence-information-macro confluence-information-macro-note

                  .. container:: confluence-information-macro-body

                     On a multi-host Unravel Server deployment, use the
                     fully qualified DNS or logical host2
                     for\ ``UNRAVEL_HOST_IP`` which must be .

               .. container::
               confluence-information-macro confluence-information-macro-warning

                  .. container:: confluence-information-macro-body

                     Copy the text below and paste it into the Cloudera
                     Manager's **Spark Client Advanced Configuration
                     Snippet (Safety Valve)** box for
                     **``spark-conf/spark-defaults.conf``**. Then,
                     modify the value of ``UNRAVEL_HOST_IP``\ and
                     ``SPARK_VERSION-X.Y``\ .

                     ``SPARK_VERSION``\ ``-X.Y``\ has the following
                     possible values: ``spark-1.3`` for Spark 1.3.x,
                     ``spark-1.5`` for Spark 1.5.x, ``spark-1.6`` for
                     Spark 1.6.x, and ``spark-2.0`` for Spark 2.0.x.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        spark.unravel.server.hostport={UNRAVEL_HOST_IP}:4043 
                        spark.driver.extraJavaOptions=-javaagent:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/btrace-agent.jar=config=driver,libs={SPARK_VERSION-X.Y}
                        spark.executor.extraJavaOptions=-javaagent:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/btrace-agent.jar=config=executor,libs={SPARK_VERSION-X.Y}
                        spark.eventLog.enabled=true

            5. Save changes.

            6. Deploy client configuration by clicking the deploy glyph
            () or by using the **Actions** pull-down menu. Your
            spark-shell will ensure new JVM containers are created with
            the necessary extraJavaOptions for the spark drivers and
            executors.

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-warning

                  .. container:: confluence-information-macro-body

                     Monitor the situation to see if all Spark queries
                     are failing with a class not found or permission
                     problems. \ **If they are failing**, you can
                     back-out the \ ``spark-defaults.conf`` changes in
                     Cloudera Manager, re-deploy client configuration,
                     and then investigate and fix the issue.

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     In the case of yarn-client mode applications, the
                     Spark default configuration won't be sufficient
                     because the driver JVM starts before the
                     configuration set through the SparkConf is applied.
                     (See  `Apache's Spark
                     Configuration <https://spark.apache.org/docs/latest/configuration.html#runtime-environment>`__
                     for more information.)
                     See \ `here <Individual-Applications-Submitted-Through-spark-submit_541164099.html#IndividualApplicationsSubmittedThroughspark-submit-SparkSubmit>`__
                     for how to set up Unravel Sensor for Spark to
                     profile specific Spark applications only, i.e., 
                     *per-application profiling* rather than
                     *cluster-wide profiling*.

            .. rubric:: 5. Optional - Configure YARN - MapReduce (MR)
               JVM Sensor Cluster-Wide
               :name: Step2:InstallUnravelSensorParcelonCDH+CM-ConfigureJVMSensorForYARN5.Optional-ConfigureYARN-MapReduce(MR)JVMSensorCluster-Wide

            #. In Cloudera Manager (CM) go to **YARN** service.
            #. Select the \ **Configuration** tab.
            #. Search for \ **ApplicationMaster Java Opts Base**\ and
               concatenate the following xml block properties snippet
               (ensure to start with a space and add below\ **).**

               .. container::
               confluence-information-macro confluence-information-macro-note

                  .. container:: confluence-information-macro-body

                     Make sure that "-" is a minus sign. You need
                     to modify the value of ``UNRAVEL_HOST_IP`` with
                     your Unravel server IP address or a fully qualified
                     DNS. For multi-host Unravel installation, use the
                     IP address of Host2.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                         -javaagent:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/btrace-agent.jar=libs=mr -Dunravel.server.hostport={UNRAVEL_HOST_IP}:4043

            #. Search for \ **MapReduce Client Advanced Configuration
               Snippet (Safety Valve) for ``mapred-site.xml``**\ in the
               middle of the page.
            #. Enter following xml four block properties snippet to
               Gateway Default Group  (Click **View as XML**).

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        <property><name>mapreduce.task.profile</name><value>true</value></property>
                        <property><name>mapreduce.task.profile.maps</name><value>0-5</value></property>
                        <property><name>mapreduce.task.profile.reduces</name><value>0-5</value></property>
                        <property><name>mapreduce.task.profile.params</name><value>-javaagent:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/btrace-agent.jar=libs=mr -Dunravel.server.hostport={UNRAVEL_HOST_IP}:4043</value></property>

            6. Save changes.

            7. Deploy client configuration by clicking the deploy glyph
            () or by using the \ **Actions** pull-down menu.

            8. Cloudera Manager will specify a restart which is not
            necessary to effect these changes. (click **Restart Stale
            Services** if that is visible. However, you can also perform
            this later when you have a planned maintenance.)

            | 

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-tip

                  .. container:: confluence-information-macro-body

                     Monitor the situation and you should see in Unravel
                     UI a Resource Usage tab showing you mappers and
                     reducers when you view the Application page for any
                     completed MRjob. Restart is important for MR sensor
                     to be picked up by queries submitted via
                     Hiveserver2.

            .. rubric:: 6. (Optional) Advanced Configuration
               :name: Step2:InstallUnravelSensorParcelonCDH+CM-6.(Optional)AdvancedConfiguration

            -  Configuration for high volume data: see `Creating
               Multiple Workers for High Volume
               Data <Creating-Multiple-Workers-for-High-Volume-Data_541131395.html>`__.
            -  Add LDAP users: see `Integrating LDAP Authentication for
               Unravel Web
               UI <Integrating-LDAP-Authentication-for-Unravel-Web-UI_541328067.html>`__.

            .. rubric:: Troubleshooting
               :name: Step2:InstallUnravelSensorParcelonCDH+CM-_troubleshootingTroubleshooting

            .. container:: table-wrap

               +---------------------------+------------------+-----------------------+
               | **Symptom**               | **Problem**      | **Remedy**            |
               +---------------------------+------------------+-----------------------+
               | | ``hadoop fs -ls /user/u | -  Unravel       | | Install Unravel RPM |
               | nravel/HOOK_RESULT_DIR/`` |    Server RPM is |   on Unravel service  |
               | | shows directory does    |    not yet       |   host:               |
               |   not exist               |    installed, or | | ``sudo rpm -U unrav |
               |                           | -  Unravel       | el*.rpm*``            |
               |                           |    Server RPM is | | **OR**              |
               |                           |    installed on  | | Verify that         |
               |                           |    a different   |   ``unravel`` user    |
               |                           |    HDFS cluster, |   exists and has a    |
               |                           |    or            |   ``/user/unravel/``  |
               |                           | -  HDFS home     |   directory in HDFS   |
               |                           |    directory for |   and unravel has     |
               |                           |    Unravel does  |   write access.       |
               |                           |    not exist, or |                       |
               |                           | -  kerberos/sent |                       |
               |                           | ry               |                       |
               |                           |    actions are   |                       |
               |                           |    needed        |                       |
               +---------------------------+------------------+-----------------------+
               | ``ClassNotFound`` error   | Unravel hive     | | Check whether       |
               | for                       | hook JAR was not |   UNRAVEL_SENSOR      |
               | ``com.unraveldata.dataflo | found in in      |   parcel was          |
               | w.hive.hook.HivePreHook`` | ``$HIVE_HOME/lib |   distributed and     |
               | during Hive query         | /``.             |   activated in CM.    |
               | execution                 |                  | | **OR**              |
               |                           |                  | | Put the Unravel     |
               |                           |                  |   hive-hook JAR       |
               |                           |                  |   corresponding       |
               |                           |                  |   to \ ``HIVE_VER``   |
               |                           |                  |   in ``JAR_DEST`` on  |
               |                           |                  |   each gateway by:    |
               |                           |                  | | ``cd /usr/local/unr |
               |                           |                  | avel/hive-hook/;``    |
               |                           |                  | | ``cp unravel-hive-H |
               |                           |                  | IVE_VER*hook.jar JAR_ |
               |                           |                  | DEST``                |
               +---------------------------+------------------+-----------------------+

            .. rubric:: References
               :name: Step2:InstallUnravelSensorParcelonCDH+CM-References

            `{+} <http://www.cloudera.com/documentation/enterprise/5-3-x/topics/cm_mc_hive_udf.html#concept_nc3_mms_lr_unique_2>`__\ http://www.cloudera.com/documentation/enterprise/5-3-x/topics/cm_mc_hive_udf.html#concept_nc3_mms_lr_unique_2+
            see Creating Permanent Functions.

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image0|
               `DeployGlyph.png <attachments/541229840/541033431.png>`__
               (image/png)
               |image1|
               `package.png <attachments/541229840/541098884.png>`__
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
