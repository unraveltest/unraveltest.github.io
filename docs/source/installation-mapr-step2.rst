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
            #. `MapR <MapR_541197270.html>`__

         .. rubric:: Unravel 4.4 : Part 2: Enable Additional Data
            Collection / Instrumentation for MapR
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified on Oct 12, 2018

         .. container:: wiki-content group
            :name: main-content

            | 

            .. container:: panel

               .. container:: panelHeader

                  **Table of Contents**

               .. container:: panelContent

                  .. container:: toc-macro rbtoc1541196958366

                     -  `Introduction <#Part2:EnableAdditionalDataCollection/InstrumentationforMapR-Introduction>`__
                     -  `1. Enable Additional Instrumentation on Unravel
                        Server's
                        Host <#Part2:EnableAdditionalDataCollection/InstrumentationforMapR-1.EnableAdditionalInstrumentationonUnravelServer'sHost>`__
                     -  `2. Confirm that Unravel Web UI Shows Additional
                        Data <#Part2:EnableAdditionalDataCollection/InstrumentationforMapR-2.ConfirmthatUnravelWebUIShowsAdditionalData>`__
                     -  `3. Confirm and Adjust the Settings in
                        yarn-site.xml <#Part2:EnableAdditionalDataCollection/InstrumentationforMapR-3.ConfirmandAdjusttheSettingsinyarn-site.xml>`__
                     -  `5. Enable Instrumentation manually by updating
                        the following
                        files: <#Part2:EnableAdditionalDataCollection/InstrumentationforMapR-5.EnableInstrumentationmanuallybyupdatingthefollowingfiles:>`__

                        -  `Update hive-site.xml <#Part2:EnableAdditionalDataCollection/InstrumentationforMapR-hive-site.xmlUpdatehive-site.xml>`__
                        -  `Update hive-env.sh <#Part2:EnableAdditionalDataCollection/InstrumentationforMapR-hive-env.shUpdatehive-env.sh>`__
                        -  `Update spark-defaults.conf <#Part2:EnableAdditionalDataCollection/InstrumentationforMapR-spark-defaults.confUpdatespark-defaults.conf>`__
                        -  `Update
                           hadoop-env.sh  <#Part2:EnableAdditionalDataCollection/InstrumentationforMapR-hadoop-env.shUpdatehadoop-env.sh>`__
                        -  `Update
                           mapred-site.xml  <#Part2:EnableAdditionalDataCollection/InstrumentationforMapR-mapred-site.xmlUpdatemapred-site.xml>`__

            .. rubric:: Introduction
               :name: Part2:EnableAdditionalDataCollection/InstrumentationforMapR-Introduction

            This topic explains how to enable additional data collection
            or instrumentation on the MapR converged data platform.
            These instructions apply to Unravel Server v4.0. For older
            versions of Unravel Server, contact Unravel Support.

            .. container:: panel

               .. container:: panelHeader

                  **Workflow Summary**

               .. container:: panelContent

                  #. Enable additional instrumentation on Unravel
                     Server's host.
                  #. Enter correct value for Hive Metastore, Resource
                     Manager and Oozie properties.
                  #. Confirm that Unravel Web UI shows additional data.
                  #. Confirm and adjust the settings in
                     ``yarn-site.xml``.
                  #. Enable additional instrumentation on other hosts in
                     the cluster.

            .. rubric:: 1. Enable Additional Instrumentation on Unravel
               Server's Host
               :name: Part2:EnableAdditionalDataCollection/InstrumentationforMapR-1.EnableAdditionalInstrumentationonUnravelServer'sHost

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-note

                  .. container:: confluence-information-macro-body

                     Substitute valid values for:

                     -  ``UNRAVEL_HOST_IP`` -\ fully qualified domain
                        name or IP address
                     -  ``SPARK_VERSION_X.Y.Z``\ - target Spark version,
                        e.g.,1.3.0, 1.6.0, 2.0.1.
                     -  ``HIVE_VERSION``\ ``_X.Y.Z`` - target Hive
                        version, e.g., 1.2.0.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo python /usr/local/unravel/install_bin/cluster-setup-scripts/unravel_mapr_setup.py --unravel-server {UNRAVEL_HOST_IP} --spark-version {SPARK_VERSION_X.Y.Z} --hive-version {HIVE_VERSION_X.Y.Z}


                        # sudo python /usr/local/unravel/install_bin/cluster-setup-scripts/unravel_mapr_setup.py --unravel-server {UNRAVEL_HOST_IP} --spark-version {SPARK_VERSION_X.Y.Z} --hive-version {HIVE_VERSION_X.Y.Z} --sensor-only

               **For Sensor Upgrade only**

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo python /usr/local/unravel/install_bin/cluster-setup-scripts/unravel_mapr_setup.py --unravel-server {UNRAVEL_HOST_IP} --spark-version {SPARK_VERSION_X.Y.Z} --hive-version {HIVE_VERSION_X.Y.Z} --sensor-only

               **Dry-Run (test/check instrumentation, this does not
               change any configuration file):**

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo python /usr/local/unravel/install_bin/cluster-setup-scripts/unravel_mapr_setup.py --unravel-server {UNRAVEL_HOST_IP} --spark-version {SPARK_VERSION_X.Y.Z} --hive-version {HIVE_VERSION_X.Y.Z} --dry-run

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     Hive hook jar is installed under:

                     ``/usr/local/unravel_client/`` 

                     Resource metrics sensor jars are installed under:

                     ``/usr/local/unravel-agent/ ``

                     Configuration changes (for MapR 5.2/MapR 6.0) are
                     made to the following files,
                     (<``SPARK_VERSION X.Y.Z>``, etc., is your
                     particular version)

                     ``/opt/mapr/spark/spark-<SPARK VERSION X.Y.Z>/conf/spark-defaults.conf ``

                     ``/opt/mapr/hive/hive-<HIVE VERSION X.Y>/conf/hive-site.xml ``

                     ``/opt/mapr/hive/hive-<HIVE VERSION X.Y>/conf/hive-env.sh`` 

                     ``/opt/mapr/hadoop/hadoop-<HADOOP VERSION X.Y.Z>/etc/hadoop/yarn-site.xml``

                     ``/opt/mapr/hadoop/hadoop-<HADOOP VERSION X.Y.Z>/etc/hadoop/mapred-site.xml``

                     ``/usr/local/unravel/etc/unravel.properties``

                     Copy of original configuration is saved in same
                     directory named \ ``*.preunravel`` , 
                     e.g., \ ``/opt/mapr/hive/hive-1.2/conf/hive-site.xml.preunravel``

                     Once the files are present on edge host where
                     Unravel rpm is installed, you can
                     tar \ ``tar`` these changes/additions up and put on
                     other hosts, if that is more convenient than
                     running the script. In all cases, instrumented
                     nodes must be able to open port 4043 of Unravel
                     Server (host2 if multi-host Unravel install).

            .. rubric:: 2. Confirm that Unravel Web UI Shows Additional
               Data
               :name: Part2:EnableAdditionalDataCollection/InstrumentationforMapR-2.ConfirmthatUnravelWebUIShowsAdditionalData

            .. container::

               Run a Hive job using a test script provided by Unravel
               Server:

               |(lightbulb)| This is where you can see the effects of
               the instrumentation setup. Best practice is to run this
               test script on Unravel Server rather than on a
               gateway/edge/client node. That way you can verify that
               instrumentation is working first, and then enable
               instrumentation on other gateway/edge/client nodes.

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     ``someUser`` **must** be user that can create
                     tables in the default database. If you need to use
                     a different database, copy the script and edit it
                     to change the target database.

                     This script creates a uniquely named table in the
                     default database, adds some data, runs a Hive query
                     on it, and then deletes the table.

                     It runs the query twice using different workflow
                     tags so you can clearly see the two different runs
                     of the same workflow in Unravel Web UI.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo -u {someUser} /usr/local/unravel/install_bin/hive_test_simple.sh

            .. rubric:: 3. Confirm and Adjust the Settings in
               ``yarn-site.xml``
               :name: Part2:EnableAdditionalDataCollection/InstrumentationforMapR-3.ConfirmandAdjusttheSettingsinyarn-site.xml

            .. container::

               Check specific properties in
               ``/opt/mapr/hadoop/hadoop-2.7.0/etc/hadoop/yarn-site.xml``
               to be sure that these settings are present (with your
               particular values for your Resource Manager web app
               address):

               -  ``yarn.resourcemanager.webapp.address``:

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           <property>
                           <name>yarn.resourcemanager.webapp.address</name>
                           <value>10.0.0.110:8088</value>
                           <source>yarn-site.xml</source>
                           </property>

               -  ``yarn.log-aggregation-enable``:

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           <property>
                           <name>yarn.log-aggregation-enable</name>
                           <value>true</value>
                           <description>For log aggregations</description>
                           </property>

            4. Enable Additional Instrumentation on Other Hosts in the
            Cluster

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     To instrument more servers, you can use the setup
                     script we provide or see the effect it has and
                     replicate it using your own provisioning automation
                     system. If you already have a way to customize and
                     deploy ``hive-site.xml``, \ ``yarn-site.xml`` and
                     user defined function jars, you can add the changes
                     and jar from Unravel to your existing mechanism.

               -  Run the shell script \ ``unravel_mapr_setup.sh`` on
                  each node of the cluster, just like it was run on the
                  Unravel server above.
               -  Copy the newly edited \ ``yarn-site.xml``, from step
                  3, to all nodes.
               -  Do a rolling-restart of HiveServer2

            | 

            .. rubric:: 5. Enable Instrumentation manually by updating
               the following files:
               :name: Part2:EnableAdditionalDataCollection/InstrumentationforMapR-5.EnableInstrumentationmanuallybyupdatingthefollowingfiles:
               :class: _mce_tagged_br

            .. container::

               -  ``hive-site.xml``
               -  ``hive-env.sh``
               -  ``spark-defaults.conf``
               -  ``hadoop-env.sh``
               -  ``mapred-site.xml``

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     Once the files are updated on edge host where
                     Unravel rpm is installed, you can \ ``scp`` these
                     changes/additions and put on other hosts (backup
                     original files in case need to rollback), In all
                     cases, instrumented nodes must be able to open port
                     4043 of Unravel Server (host2 if multi-host Unravel
                     install).

                     .. container::
                     confluence-information-macro confluence-information-macro-note

                        .. container:: confluence-information-macro-body

                           Be sure to substitute valid values for:

                           -  ``UNRAVEL_HOST_IP`` -\ fully qualified
                              domain name or IP address
                           -  ``SPARK_VERSION_X.Y.Z``\ - target Spark
                              version, e.g.,1.3.0, 1.6.0, 2.0.1.
                           -  ``HIVE_VERSION``\ ``_X.Y.Z`` - target Hive
                              version, e.g., 1.2.0.

               .. rubric:: Update \ ``hive-site.xml``
                  :name: Part2:EnableAdditionalDataCollection/InstrumentationforMapR-hive-site.xmlUpdatehive-site.xml

               .. container::

                  Copy the content
                  in \ ``/usr/local/unravel/hive-hook/hive-site.xml.snip`` and
                  append it in
                  ``/opt/mapr/hive/hive-HIVE_VERSION``\ ``_X.Y.Z``/conf/hive-site.xml 
                  right before </configuration> (replace
                  ``{UNRAVEL_HOST_IP``}).

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           <property>
                             <name>com.unraveldata.host</name>
                             <value>{UNRAVEL_HOST_IP}</value>
                             <description>Unravel hive-hook processing host</description>
                           </property>

                           <property>
                             <name>com.unraveldata.hive.hook.tcp</name>
                             <value>true</value>
                           </property>

                           <property>
                             <name>com.unraveldata.hive.hdfs.dir</name>
                             <value>/user/unravel/HOOK_RESULT_DIR</value>
                             <description>destination for hive-hook, Unravel log processing</description>
                           </property>

                           <property>
                             <name>hive.exec.driver.run.hooks</name>
                             <value>com.unraveldata.dataflow.hive.hook.HiveDriverHook</value>
                             <description>for Unravel, from unraveldata.com</description>
                           </property>

                           <property>
                             <name>hive.exec.pre.hooks</name>
                             <value>com.unraveldata.dataflow.hive.hook.HivePreHook</value>
                             <description>for Unravel, from unraveldata.com</description>
                           </property>

                           <property>
                             <name>hive.exec.post.hooks</name>
                             <value>com.unraveldata.dataflow.hive.hook.HivePostHook</value>
                             <description>for Unravel, from unraveldata.com</description>
                           </property>

                           <property>
                             <name>hive.exec.failure.hooks</name>
                             <value>com.unraveldata.dataflow.hive.hook.HiveFailHook</value>
                             <description>for Unravel, from unraveldata.com</description>
                           </property>

                           </configuration>

               .. rubric:: Update \ ``hive-env.sh``
                  :name: Part2:EnableAdditionalDataCollection/InstrumentationforMapR-hive-env.shUpdatehive-env.sh

               .. container::

                  In \ ``/opt/mapr/hive/hive-HIVE_VERSION``\ ``_X.Y.Z/conf/hive-env.sh``
                  append the following; substituting your local values
                  ``({HIVE_VERSION_X.Y.Z}``).

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           export AUX_CLASSPATH=${AUX_CLASSPATH}:/usr/local/unravel_client/unravel-hive-{HIVE_VERSION X.Y.Z}-hook.jar
                           export HIVE_AUX_JARS_PATH=${HIVE_AUX_JARS_PATH}:/usr/local/unravel_client

               .. rubric:: Update \ ``spark-defaults.conf``
                  :name: Part2:EnableAdditionalDataCollection/InstrumentationforMapR-spark-defaults.confUpdatespark-defaults.conf

               .. container::

                  ``In /opt/mapr/spark/spark-SPARK_VERSION``\ ``_X.Y.Z/conf/``\ ``spark-defaults.conf``
                  append the following; substituting your local values
                  where noted (``{UNRAVEL_HOST_IP}`` and
                  ``{SPARK_VERSION_X.Y.Z}``).

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           spark.unravel.server.hostport {UNRAVEL_HOST_IP}:4043
                           spark.eventLog.dir maprfs:///apps/spark
                           spark.history.fs.logDirectory maprfs:///apps/spark
                           spark.driver.extraJavaOptions  -javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=libs=spark-{SPARK_VERSION_X.Y.Z},config=driver
                           spark.executor.extraJavaOptions -javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=libs=spark-{SPARK_VERSION_X.Y.Z},config=executor

               .. rubric:: Update hadoop-env.sh 
                  :name: Part2:EnableAdditionalDataCollection/InstrumentationforMapR-hadoop-env.shUpdatehadoop-env.sh

               .. container::

                  In
                  ``/opt/mapr/hadoop/hadoop-HADOOP_VERSION``\ ``_X.Y.Z/etc/hadoop/hadoop-env.sh``
                  append the following; substituting your local values
                  where noted\ ``({HIVE_VERSION_X.Y.Z}``).

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           export HADOOP_CLASSPATH=${HADOOP_CLASSPATH}:/usr/local/unravel_client/unravel-hive-{HIVE_VERSION_X.Y.Z}.0-hook.jar

               .. rubric:: Update mapred-site.xml 
                  :name: Part2:EnableAdditionalDataCollection/InstrumentationforMapR-mapred-site.xmlUpdatemapred-site.xml

               .. container::

                  In \ ``/opt/mapr/hadoop/hadoop-HADOOP_VERSION``\ ``_X.Y.Z/etc/hadoop/mapred-site.xml``
                  append the following; substituting your local values
                  where noted (``{UNRAVEL_HOST_IP}``).

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           <property>
                             <name>mapreduce.task.profile</name>
                             <value>true</value>
                           </property>

                           <property>
                             <name>mapreduce.task.profile.maps</name>
                             <value>0-5</value>
                           </property>

                           <property>
                             <name>mapreduce.task.profile.reduces</name>
                             <value>0-5</value>
                           </property>

                           <property>
                             <name>mapreduce.task.profile.params</name>
                             <value>-javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=libs=mr -Dunravel.server.hostport={UNRAVEL_HOST_IP}:4043</value>
                           </property>

                           <property>
                             <name>yarn.app.mapreduce.am.command-opts</name>
                             <value>-javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=libs=mr -Dunravel.server.hostport=172.36.1.126:4043</value>
                           </property>

                  .. container::
                  confluence-information-macro confluence-information-macro-note

                     .. container:: confluence-information-macro-body

                        Make sure the original value
                        of \ ``yarn.app.mapreduce.am.command-opts``\ ** **\ is
                        preserved, by appending the java agent setup
                        rather than replacing the original value.

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Nov 02, 2018 15:15

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

.. |(lightbulb)| image:: images/icons/emoticons/lightbulb_on.png
   :class: emoticon emoticon-light-on

