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

         .. rubric:: Unravel 4.4 : Step 1: Install Unravel Server on
            CDH+CM
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified by Sam Lachterman on Nov 02,
            2018

         .. container:: wiki-content group
            :name: main-content

            .. container:: panel

               .. container:: panelHeader

                  **Table of Contents**

               .. container:: panelContent

                  .. container:: toc-macro rbtoc1541196937144

                     -  `Introduction <#Step1:InstallUnravelServeronCDH+CM-Introduction>`__
                     -  `Pre-Installation
                        Check <#Step1:InstallUnravelServeronCDH+CM-Pre-InstallationCheck>`__

                        -  `Platform
                           Compatibility <#Step1:InstallUnravelServeronCDH+CM-PlatformCompatibility>`__
                        -  `Hardware <#Step1:InstallUnravelServeronCDH+CM-Hardware>`__
                        -  `Software <#Step1:InstallUnravelServeronCDH+CM-Software>`__
                        -  `Access
                           Permissions <#Step1:InstallUnravelServeronCDH+CM-AccessPermissions>`__
                        -  `Network <#Step1:InstallUnravelServeronCDH+CM-Network>`__

                     -  `1. Configure the Host
                        Machine <#Step1:InstallUnravelServeronCDH+CM-1.ConfiguretheHostMachine>`__

                        -  `Allocate a Cluster Gateway/Edge/Client Host
                           with HDFS
                           Access <#Step1:InstallUnravelServeronCDH+CM-AllocateaClusterGateway/Edge/ClientHostwithHDFSAccess>`__

                     -  `2. Install the Unravel Server RPM on the Host
                        Machine <#Step1:InstallUnravelServeronCDH+CM-2.InstalltheUnravelServerRPMontheHostMachine>`__

                        -  `Get the Unravel Server
                           RPM <#Step1:InstallUnravelServeronCDH+CM-GettheUnravelServerRPM>`__
                        -  `Make symlinks if
                           required <#Step1:InstallUnravelServeronCDH+CM-Makesymlinksifrequired>`__
                        -  `Install the Unravel Server
                           RPM <#Step1:InstallUnravelServeronCDH+CM-InstalltheUnravelServerRPM>`__
                        -  `Do Host-Specific Post-Installation
                           Actions <#Step1:InstallUnravelServeronCDH+CM-DoHost-SpecificPost-InstallationActions>`__

                     -  `3. Configure Unravel Server (Basic/Core
                        Optional for
                        CDH) <#Step1:InstallUnravelServeronCDH+CM-3.ConfigureUnravelServer(Basic/CoreOptionalforCDH)>`__

                        -  `Enable Optional
                           Daemons <#Step1:InstallUnravelServeronCDH+CM-EnableOptionalDaemons>`__
                        -  `If Kerberos is
                           Enabled: <#Step1:InstallUnravelServeronCDH+CM-IfKerberosisEnabled:>`__
                        -  `If Sentry is
                           Enabled: <#Step1:InstallUnravelServeronCDH+CM-IfSentryisEnabled:>`__
                        -  `Switch
                           User <#Step1:InstallUnravelServeronCDH+CM-SwitchUser>`__
                        -  `Hive Metastore
                           Access <#Step1:InstallUnravelServeronCDH+CM-HiveMetastoreAccess>`__
                        -  `Restart Unravel
                           Server <#Step1:InstallUnravelServeronCDH+CM-RestartUnravelServer>`__

                     -  `4. Log into Unravel Web
                        UI <#Step1:InstallUnravelServeronCDH+CM-4.LogintoUnravelWebUI>`__

                        -  `Congratulations! <#Step1:InstallUnravelServeronCDH+CM-Congratulations!>`__

                     -  `5. (Optional) Configure Unravel Server
                        (Advanced
                        Options) <#Step1:InstallUnravelServeronCDH+CM-5.(Optional)ConfigureUnravelServer(AdvancedOptions)>`__

                        -  `Enable Additional Data
                           Collection/Instrumentation <#Step1:InstallUnravelServeronCDH+CM-EnableAdditionalDataCollection/Instrumentation>`__
                        -  `Run the Unravel Web UI Configuration
                           Wizard <#Step1:InstallUnravelServeronCDH+CM-RuntheUnravelWebUIConfigurationWizard>`__

            .. rubric:: Introduction
               :name: Step1:InstallUnravelServeronCDH+CM-Introduction

            This topic explains how to deploy Unravel Server 4.4 on a
            CDH gateway/edge node. For instructions that correspond to
            older versions of Unravel Server, contact Unravel Support.

            .. container:: panel

               .. container:: panelHeader

                  **Workflow Summary**

               .. container:: panelContent

                  #. Pre-installation check
                  #. Configure the host machine.

                  #. Install the Unravel Server RPM on the host machine.
                  #. Configure Unravel Server (basic/core options).

                  #. Log into Unravel Web UI.
                  #. (Optional) Configure Unravel Server (advanced
                     options).

            .. rubric:: Pre-Installation Check
               :name: Step1:InstallUnravelServeronCDH+CM-Pre-InstallationCheck

            The following installation requirements must be met for
            successful installation of Unravel.

            .. rubric:: Platform Compatibility
               :name: Step1:InstallUnravelServeronCDH+CM-PlatformCompatibility

            -  CDH 5.4 - 5.14
            -  Hadoop 1.x - 2.x
            -  Kafka 0.9, 0.10, 0.11, and 1.0 (Apache ver. equiv.)
            -  Kerberos
            -  Hive 0.9.x - 1.2.x
            -  Spark 1.3.x - 2.2.x

            .. rubric:: Hardware
               :name: Step1:InstallUnravelServeronCDH+CM-Hardware

            -  Architecture: x86_64
            -  Cores: 8
            -  RAM: 64GB minimum, 128GB for medium volume (>25,000 jobs
               per day), 256GB for high volume (>50,000 jobs per day)
            -  Disk: ``/usr/local/unravel`` with 8GB free minimum (can
               be symlink)
            -  Disk: ``/srv/unravel`` with 500GB free minimum (can be
               symlink)
            -  For 60,000+ MR jobs per day, two or more gateway/edge
               nodes are recommended ("multi-host Unravel")

            .. rubric:: Software
               :name: Step1:InstallUnravelServeronCDH+CM-Software

            -  Operating System: RedHat/Centos 6.4 - 7.4
            -  ``libaio.x86_64`` installed
            -  ``SELINUX=permissive`` (or disabled) should be set in
               ``/etc/selinux/config``
            -  HDFS+Hive+YARN+Spark Unravel host should be a
               client/gateway (Hadoop and Hive commands in ``PATH)``
            -  If Spark2 service is installed, Unravel host should be a
               client/gateway 
            -  LDAP (AD or Open LDAP) compatible for Unravel Web UI user
               authentication (Open signup by default)
            -  On Unravel Edge-node server, please \ **do not** have
               zookeeper installed in same server
            -  NTP should be running and in-sync with the cluster

            .. rubric:: Access Permissions
               :name: Step1:InstallUnravelServeronCDH+CM-AccessPermissions

            -  Local user unravel:unravel is created during
               installation, but can be changed later
            -  If Kerberos is in use, a keytab for an Unravel Kerberos
               principal is required for access to:

               -  Access to YARN’s “done dir” in HDFS
               -  Access to YARN’s log aggregation directory in HDFS
               -  Access to Spark event log directory in HDFS
               -  Access to file sizes under HIve warehouse directory

            -  Access to YARN Resource Manager REST API

               -  principal needs right to find out which RM is active

            -  JDBC access to the Hive Metastore (read-only user is
               sufficient)
            -  For Impala support, a read-only account for Cloudera
               Manager API is needed

            .. rubric:: Network
               :name: Step1:InstallUnravelServeronCDH+CM-Network

            -  Port 3000 from users and entire cluster to Unravel Web UI
            -  HDFS ports open from Hadoop cluster to Unravel Server(s)
            -  For YARN, Hive Metadata DB port open to Unravel Server(s)
               for partition reporting
            -  UDP and TCP port 4043 open from entire cluster to Unravel
               Server(s)
            -  For Oozie, port 11000 open from Unravel Server(s) to the
               Oozie server
            -  Resource Manager (RM) port 8032 from Unravel Server(s) to
               the RM server(s)
            -  Cloudera Manager (CM) port 7180 (or 7183 for HTTPS) from
               Unravel Server(s) to CM
            -  Port 4020, 4176, 4181 through 4189, 3316, 4091 must be
               available for localhost communication between Unravel
               daemons or Unravel servers (if multi-host Unravel
               installation)

            .. rubric:: 1. Configure the Host Machine
               :name: Step1:InstallUnravelServeronCDH+CM-1.ConfiguretheHostMachine

            .. rubric:: Allocate a Cluster Gateway/Edge/Client Host with
               HDFS Access
               :name: Step1:InstallUnravelServeronCDH+CM-AllocateaClusterGateway/Edge/ClientHostwithHDFSAccess

            Use Cloudera Manager to create the gateway configuration for
            the Unravel server(s) that has client roles for HDFS, YARN,
            Spark, Hive, and optionally Spark2.

            .. rubric:: 2. Install the Unravel Server RPM on the Host
               Machine
               :name: Step1:InstallUnravelServeronCDH+CM-2.InstalltheUnravelServerRPMontheHostMachine

            .. rubric:: Get the Unravel Server RPM
               :name: Step1:InstallUnravelServeronCDH+CM-GettheUnravelServerRPM

            See `Download Unravel
            Software <https://unraveldata.atlassian.net/wiki/spaces/UNDOCS/pages/226132074/Download+Unravel+Software+Versions>`__.

            .. rubric:: Make symlinks if required
               :name: Step1:InstallUnravelServeronCDH+CM-Makesymlinksifrequired

            If you want the two disk areas used by Unravel to be on
            different volumes, you can make symlinks to specific areas
            before installing (or do
            a \ ``mv``\  and \ ``symlink``\  symlink after installing).
            Do it before the first install if there is insufficient
            space on the target
            paths \ ``/usr/local/unravel ``\ and \ ``/srv/unravel``\  noted
            above. 

            .. rubric:: Install the Unravel Server RPM
               :name: Step1:InstallUnravelServeronCDH+CM-InstalltheUnravelServerRPM

            .. container::

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo rpm -U unravel-4.*.x86_64.rpm*
                        # /usr/local/unravel/install_bin/await_fixups.sh

            The precise filename can vary, depending on how it was
            fetched or copied. The \ ``rpm`` command does not require
            .\ ``rpm`` suffix. The flag \ ``-U`` works for either
            initial install or upgrade.

            Run the specified \ ``await_fixups.sh``  script to make sure
            background processing is finished before you do other steps.
            In a routine upgrade, it is okay to start all Unravel
            daemons, but do not stop or restart them until
            the \ ``await_fixups.sh``  prints \ ``DONE`` (it takes a few
            minutes).

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     | The installation
                       creates \ ``/usr/local/unravel/`` which contains
                       the executables, scripts, and settings.
                       User \ ``unravel`` is created. The initial
                       internal database and other durable state are put
                       in \ ``/srv/unravel/`` for larger storage.  
                     | The master configuration file is
                       in \ ``/usr/local/unravel/etc/unravel.properties``
                       and the logs are in ``/usr/local/unravel/logs/``.
                       The RPM installation creates user \ ``unravel``
                       if it does not already exist and this can be
                       changed after
                       installation; \ ``/etc/init.d/unravel_*`` scripts
                       for controlling its services as well
                       as \ ``/etc/init.d/unravel_all.sh`` which can be
                       used to manually stop, start, and get status of
                       all daemons in proper order.
                     | During initial install, a bundled database is
                       used. This can be switched to use an `externally
                       managed
                       MySQL <Installing-MySQL-or-Compatible-Database-for-Unravel_541131376.html>`__
                       for production. 

            .. rubric:: Do Host-Specific Post-Installation Actions
               :name: Step1:InstallUnravelServeronCDH+CM-DoHost-SpecificPost-InstallationActions

            For CDH, there are no host-specific post-installation
            actions.

            .. rubric:: 3. Configure Unravel Server (Basic/Core Optional
               for CDH)
               :name: Step1:InstallUnravelServeronCDH+CM-3.ConfigureUnravelServer(Basic/CoreOptionalforCDH)

            .. rubric:: Enable Optional Daemons
               :name: Step1:InstallUnravelServeronCDH+CM-EnableOptionalDaemons

            Depending on your workload volume or kind of activity, you
            can enable optional daemons at this point. See \ `Creating
            Multiple Workers for High Volume
            Data <Creating-Multiple-Workers-for-High-Volume-Data_541131395.html>`__.

            .. rubric:: If Kerberos is Enabled:
               :name: Step1:InstallUnravelServeronCDH+CM-IfKerberosisEnabled:

            .. container::

               .. container:: expand-container
                  :name: expander-1843118745

                  .. container:: expand-control
                     :name: expander-control-1843118745

                     Add authentication for HDFS...

                  .. container:: expand-content
                     :name: expander-content-1843118745

                     `Create <Alternate-Kerberos-Principal-for-Cluster-Access-on-CDH_541164128.html>`__
                     or identify a principal and keytab for Unravel
                     daemons to access HDFS and REST when Kerberos is
                     enabled. 

                     To get going faster, you can use the 'hdfs'
                     principal which often has a pre-existing "headless"
                     keytab.

                     Add properties for Kerberos in 
                     ``/usr/local/unravel/etc/unravel.properties`` (substitute
                     correct filename and principal):

                     .. container:: code panel pdl

                        .. container:: codeContent panelContent pdl

                           .. code:: syntaxhighlighter-pre

                              com.unraveldata.kerberos.principal=unravel/myhost.mydomain@MYREALM
                              com.unraveldata.kerberos.keytab.path=/usr/local/unravel/etc/unravel.keytab

                     You can verify the principal in a keytab by
                     using \ ``klist -kt KETYAB_FILE.``\ The keytab file
                     should have chmod bits 500 and be owned
                     by \ ``unravel`` local user (default) or by the
                     user you want to use, as explained in \ `Run
                     Unravel Daemons with Custom
                     User <Run-Unravel-Daemons-with-Custom-User_541033161.html>`__\ .

            .. rubric:: If Sentry is Enabled:
               :name: Step1:InstallUnravelServeronCDH+CM-IfSentryisEnabled:

            .. container::

               .. container:: expand-container
                  :name: expander-1257977907

                  .. container:: expand-control
                     :name: expander-control-1257977907

                     Add these permissions...

                  .. container:: expand-content
                     :name: expander-content-1257977907

                     For quicker setup, use the hdfs principal. For more
                     narrow privileges, define your own alt principal.
                     The alt user can be \ ``unravel`` (created
                     by \ ``rpm``) or one of your choosing. The
                     corresponding kerberos principal does not need to
                     have the same name as the local user. The
                     user/principal here should correspond to the
                     ``X`` in the next section. 

                     .. container:: table-wrap

                        +-----------------+-----------------+-----------------+-----------------+
                        | Resource        | Principal       | Access          | Purpose         |
                        +=================+=================+=================+=================+
                        | ``hdfs://user/s | hdfs or alt     | read            | Spark event log |
                        | park/applicatio |                 |                 |                 |
                        | nHistory``      |                 |                 |                 |
                        +-----------------+-----------------+-----------------+-----------------+
                        | ``hdfs://user/s | hdfs or alt     | read            | Spark 2 event   |
                        | park/spark2Appl |                 |                 | log             |
                        | icationHistory` |                 |                 |                 |
                        | `               |                 |                 |                 |
                        +-----------------+-----------------+-----------------+-----------------+
                        | ``hdfs://user/h | hdfs or alt     | read            | MapReduce logs  |
                        | istory``        |                 |                 |                 |
                        +-----------------+-----------------+-----------------+-----------------+
                        | ``hdfs://tmp/lo | hdfs or alt     | read            | YARN            |
                        | gs``            |                 |                 | aggregation     |
                        |                 |                 |                 | folder          |
                        +-----------------+-----------------+-----------------+-----------------+
                        | ``hdfs://user/h | hdfs or alt     | read            | Obtain table    |
                        | ive/warehouse`` |                 |                 | partition sizes |
                        |                 |                 |                 | with "stat"     |
                        |                 |                 |                 | only            |
                        +-----------------+-----------------+-----------------+-----------------+

                     Please see \ `Configure Permission for Unravel
                     daemons on CDH Sentry Secured
                     Cluste <Configure-Permission-for-Unravel-daemons-on-CDH-Sentry-Secured-Cluster_541360876.html>`__\ r
                     on how to configure permissions for unravel with a
                     Sentry enforced cluster.

                     You can find the principal by using *'klist -kt
                     KEYTAB_FILE'*

                     If you are using KMS and HDFS encryption and are
                     using the hdfs principal, you might need to
                     adjust \ ``kms-acls.xml``\ *  *\ permissions in CM
                     for DECRYPT_EEK if access is denied. In particular,
                     the "done" directory might not allow decryption of
                     logs by hdfs principal\ *.*

                     If you are using "JNI" based groups for HDFS (a
                     setting in CM), then you will need to add
                     "``export LD_LIBRARY_PATH=/opt/cloudera/parcels/CDH/lib/hadoop/lib/native" to /usr/local/unravel/etc/unravel.ext.sh``

            .. rubric:: Switch User
               :name: Step1:InstallUnravelServeronCDH+CM-SwitchUser

            Depending on your cluster security configuration, you will
            need to run the \ ``switch_to_user`` script. Dependencies
            like kerberos and which target user you used for Sentry
            affect this. 

            .. container::

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo /usr/local/unravel/install_bin/switch_to_user.sh x y 

            where \ ``X ``\ and \ ``Y`` depend on your environment. See
            the
            `switch_to_user <Run-Unravel-Daemons-with-Custom-User_541033161.html>`__ page. 

            .. rubric:: Hive Metastore Access
               :name: Step1:InstallUnravelServeronCDH+CM-HiveMetastoreAccess

            Hive metastore is accessed by Unravel server to analyze
            table usage in conjunction with Hive job instrumentation.
            Information is gathered using a Hive API that works very
            much like beeline connections which leverage the jdbc
            database connection protocol. As a quick-start approach, you
            can set Unravel to use the already-defined 'hive' user that
            is also used by HiveServer2. Alternatively, a read-only
            metastore database user can be define. If you want a custom
            user, then do the following steps for the particular kind of
            database that is used for Hive metastore:

            #. Connect to the Hive metastore using the normal
               conversational interface (mysql or psql, etc.) as an
               admin that can create new users.
            #. Create a user, e.g., \ ``unravel``, allowing access from
               the Unravel server host(s).
            #. Grant select on all table in the hive database.
            #. As the new user, use the conversational interface (mysql
               or psql, etc.) from the Unravel server to verify their
               access.

            .. rubric:: Restart Unravel Server
               :name: Step1:InstallUnravelServeronCDH+CM-RestartUnravelServer

            After the edits to ``com.unraveldata.login.admins`` in
            ``/usr/local/unravel/etc/unravel.properties`` it is
            necessary to run the following script in order to make
            changes take effect. The ``echo`` command shows the page to
            visit with your browser. If you are using an ssh tunnel or
            http proxy, you might need to make adjustments.  

            .. container::

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo /etc/init.d/unravel_all.sh start
                        # echo "http://$(hostname -f):3000/"

            This completes the basic/core configuration.

            .. rubric:: 4. Log into Unravel Web UI
               :name: Step1:InstallUnravelServeronCDH+CM-4.LogintoUnravelWebUI

            Using a web browser, navigate
            to\ ``http://{UNRAVEL_``\ ``HOST``\ \_IP}:3000/ and login as
            user ``admin`` with password ``unraveldata``.  Substitute a
            fully qualified DNS or IP address for
            ``UNRAVEL_HOST``\ \_IP.

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-note

                  .. container:: confluence-information-macro-body

                     For the free trial version, use the Chrome web
                     browser.

            | 

            .. rubric:: Congratulations!
               :name: Step1:InstallUnravelServeronCDH+CM-Congratulations!

            Unravel Server is up and running. Unravel Web UI displays
            collected data. For instructions on using Unravel Web UI,
            see the `User Guide <User-Guide_541295329.html>`__.

            .. rubric:: 5. (Optional) Configure Unravel Server (Advanced
               Options)
               :name: Step1:InstallUnravelServeronCDH+CM-5.(Optional)ConfigureUnravelServer(AdvancedOptions)

            .. rubric:: Enable Additional Data
               Collection/Instrumentation
               :name: Step1:InstallUnravelServeronCDH+CM-EnableAdditionalDataCollection/Instrumentation

            Install the Unravel Sensor Parcel on gateway/edge/client
            nodes that are used to submit Hive queries to push
            additional information to Unravel Server. For details, see
            `Step 2: Install Unravel Sensor Parcel on
            CDH+CM <541229840.html>`__.

            .. rubric:: Run the Unravel Web UI Configuration Wizard
               :name: Step1:InstallUnravelServeronCDH+CM-RuntheUnravelWebUIConfigurationWizard

            Run the Unravel Web UI configuration wizard to choose
            additional configuration options. For instructions on
            configuring advanced options, see the `User
            Guide <User-Guide_541295329.html>`__.

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image0|
               `image2017-2-26_0-20-12.png <attachments/541131652/541295616.png>`__
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
