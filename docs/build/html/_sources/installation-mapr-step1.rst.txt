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

         .. rubric:: Unravel 4.4 : Step 1: Install Unravel Server on
            MapR
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified on Oct 15, 2018

         .. container:: wiki-content group
            :name: main-content

            .. container:: panel

               .. container:: panelHeader

                  **Table of Contents**

               .. container:: panelContent

                  .. container:: toc-macro rbtoc1541196957885

                     -  `Introduction <#Step1:InstallUnravelServeronMapR-Introduction>`__
                     -  `Pre-Installation
                        Check <#Step1:InstallUnravelServeronMapR-Pre-InstallationCheck>`__

                        -  `Platform
                           Compatibility <#Step1:InstallUnravelServeronMapR-PlatformCompatibility>`__
                        -  `Hardware <#Step1:InstallUnravelServeronMapR-Hardware>`__
                        -  `Software <#Step1:InstallUnravelServeronMapR-Software>`__
                        -  `Access
                           Permissions <#Step1:InstallUnravelServeronMapR-AccessPermissions>`__
                        -  `Network <#Step1:InstallUnravelServeronMapR-Network>`__

                     -  `1. Configure the Host
                        Machine <#Step1:InstallUnravelServeronMapR-1.ConfiguretheHostMachine>`__

                        -  `Allocate a Cluster Gateway/Edge/Client Host
                           with HDFS
                           Access <#Step1:InstallUnravelServeronMapR-AllocateaClusterGateway/Edge/ClientHostwithHDFSAccess>`__
                        -  `Configure the Host Before installing the
                           RPM <#Step1:InstallUnravelServeronMapR-ConfiguretheHostBeforeinstallingtheRPM>`__

                     -  `2. Install the Unravel Server RPM on the Host
                        Machine <#Step1:InstallUnravelServeronMapR-2.InstalltheUnravelServerRPMontheHostMachine>`__

                        -  `Get the Unravel Server
                           RPM <#Step1:InstallUnravelServeronMapR-GettheUnravelServerRPM>`__
                        -  `Make symlinks if
                           required <#Step1:InstallUnravelServeronMapR-Makesymlinksifrequired>`__
                        -  `Install the Unravel Server
                           RPM <#Step1:InstallUnravelServeronMapR-InstalltheUnravelServerRPM>`__
                        -  `Do Host-Specific Post-Installation
                           Actions <#Step1:InstallUnravelServeronMapR-DoHost-SpecificPost-InstallationActions>`__

                     -  `3. Configure Unravel Server (Basic/Core
                        Options) <#Step1:InstallUnravelServeronMapR-3.ConfigureUnravelServer(Basic/CoreOptions)>`__

                        -  `Enable Optional
                           Daemons <#Step1:InstallUnravelServeronMapR-EnableOptionalDaemons>`__
                        -  `Modify
                           unravel.properties <#Step1:InstallUnravelServeronMapR-Modifyunravel.properties>`__
                        -  `If Kerberos is
                           Enabled: <#Step1:InstallUnravelServeronMapR-IfKerberosisEnabled:>`__
                        -  `If Sentry is
                           Enabled: <#Step1:InstallUnravelServeronMapR-IfSentryisEnabled:>`__
                        -  `Switch
                           User <#Step1:InstallUnravelServeronMapR-SwitchUser>`__
                        -  `Do Host-Specific Configuration
                           Steps <#Step1:InstallUnravelServeronMapR-DoHost-SpecificConfigurationSteps>`__
                        -  `Restart Unravel
                           Server <#Step1:InstallUnravelServeronMapR-RestartUnravelServer>`__

                     -  `4. Log into Unravel Web
                        UI <#Step1:InstallUnravelServeronMapR-4.LogintoUnravelWebUI>`__

                        -  ` <#Step1:InstallUnravelServeronMapR->`__
                        -  `Congratulations! <#Step1:InstallUnravelServeronMapR-Congratulations!>`__

                     -  `5. (Optional) Configure Unravel Server
                        (Advanced
                        Options) <#Step1:InstallUnravelServeronMapR-5.(Optional)ConfigureUnravelServer(AdvancedOptions)>`__

                        -  `Enable Additional Data
                           Collection/Instrumentation <#Step1:InstallUnravelServeronMapR-EnableAdditionalDataCollection/Instrumentation>`__
                        -  `Run the Unravel Web UI Configuration
                           Wizard <#Step1:InstallUnravelServeronMapR-RuntheUnravelWebUIConfigurationWizard>`__

            .. rubric:: Introduction
               :name: Step1:InstallUnravelServeronMapR-Introduction

            This topic explains how to deploy Unravel Server 4.2 on the
            MapR converged data platform. These instructions apply to
            Unravel Server 4.2. 

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
               :name: Step1:InstallUnravelServeronMapR-Pre-InstallationCheck

            The following installation requirements must be met for
            successful installation of Unravel.

            .. rubric:: Platform Compatibility
               :name: Step1:InstallUnravelServeronMapR-PlatformCompatibility

            -  MapR 5.1-6.0.1
            -  Hadoop 1.x - 2.x
            -  Kafka 0.9, 0.10, 0.11, and 1.0 (Apache ver. equiv.)
            -  Kerberos/MapR Tickets
            -  Hive 0.9.x - 1.2.x
            -  Spark 1.3.x - 2.0.x

            .. rubric:: Hardware
               :name: Step1:InstallUnravelServeronMapR-Hardware

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
               :name: Step1:InstallUnravelServeronMapR-Software

            -  Operating System: RedHat/Centos 6.4 - 7.4
            -  ``libaio.x86_64`` installed
            -  ``SELINUX=permissive`` (or disabled) should be set in
               ``/etc/selinux/config``
            -  HDFS+Hive+YARN+Spark client/gateway, Hadoop and Hive
               commands in ``PATH``
            -  LDAP (AD or Open LDAP) compatible for Unravel Web UI user
               authentication (Open signup by default)
            -  On Unravel Edge-node server, please \ **do not** have
               zookeeper installed in same server

            .. rubric:: Access Permissions
               :name: Step1:InstallUnravelServeronMapR-AccessPermissions

            -  If Kerberos is in use, a keytab for principal hdfs (or
               read-only equivalent) is required for access to:

               -  Access to YARN’s “done dir” in HDFS
               -  Access to YARN’s log aggregation directory in HDFS
               -  Access to Spark event log directory in HDFS
               -  Access to file sizes under HIve warehouse directory

            -  Access to YARN Resource Manager REST API

               -  principal needs right to find out which RM is active

            -  JDBC access to the Hive Metastore (read-only user is
               sufficient)

            .. rubric:: Network
               :name: Step1:InstallUnravelServeronMapR-Network

            -  Port 3000 (or 4020) from users and entire cluster to
               Unravel Web UI
            -  HDFS ports open from Hadoop cluster to Unravel Server(s)
            -  For YARN, Hive Metadata DB port open to Unravel Server(s)
               for partition reporting
            -  UDP and TCP port 4043 open from entire cluster to Unravel
               Server(s)
            -  For Oozie, port 11000 open from Unravel Server(s) to the
               Oozie server
            -  Resource Manager (RM) port 8032 from Unravel Server(s) to
               the RM server(s)
            -  Port 4176, 4181 through 4189, 3316, 4091 must be
               available for localhost communication between Unravel
               daemons or services

            | 

            .. container::
            confluence-information-macro confluence-information-macro-information

               .. container:: confluence-information-macro-body

                  ``HIGHLIGHTED ``\ text and text with brackets ( { } ),
                  except where otherwise noted, indicate where you must
                  substitute your particular values for the text.

                  ``UNRAVEL_``\ ``HOST``\ \_IP must be a fully qualified
                  DNS or IP address

            .. rubric:: 1. Configure the Host Machine
               :name: Step1:InstallUnravelServeronMapR-1.ConfiguretheHostMachine

            .. rubric:: Allocate a Cluster Gateway/Edge/Client Host with
               HDFS Access
               :name: Step1:InstallUnravelServeronMapR-AllocateaClusterGateway/Edge/ClientHostwithHDFSAccess

            If you do not already have a gateway/edge/client host
            provisioned for Unravel server, follow these steps which are
            needed to enable the ``hadoop fs`` command, Hive, and Spark.
            Although Unravel does not launch Hive and Spark jobs, it
            will be convenient for you to have those installed there for
            part 2 of the installation+configuration process. 

            For more information about the MapR client configuration,
            see \ https://maprdocs.mapr.com/52/ReferenceGuide/configure.sh.html

            Run the following commands on Unravel Server as ``root``
            substituting your site-specific values for
            ``NAME, CLDB_LIST, HISTORY_SERVER.``

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

                     # sudo yum install mapr-client.x86_64
                     # sudo /opt/mapr/server/configure.sh -N {NAME} -c -C {CLDB_LIST} -HS {HISTORY_SERVER}
                     # sudo yum install mapr-hive.noarch
                     # sudo yum install mapr-spark.noarch

            .. rubric:: Configure the Host *Before* installing the RPM
               :name: Step1:InstallUnravelServeronMapR-ConfiguretheHostBeforeinstallingtheRPM

            -  Run the following commands on Unravel Server as ``root``:

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo useradd -g mapr unravel
                        # hadoop fs -mkdir /user/unravel
                        # hadoop fs -chown unravel:mapr /user/unravel

            -  If MapR tickets are enabled, check mapr ticket for users
               ``unravel`` and ``mapr`` on target host now. You might
               need to export ticket environment variables in
               ``/etc/unravel_ctl`` first.
            -  Check available RAM to ensure availability:

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # free -g

            -  For instructions on adjusting RAM allocated to MapR-FS
               (mfs), see https://community.mapr.com/docs/DOC-1209. For
               example, edit ``/opt/mapr/conf/warden.conf`` as follows:

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        service.command.mfs.heapsize.maxpercent=10

               (only change this setting on the Unravel gateway/client
               machine). And the restart mfs.

            .. rubric:: 2. Install the Unravel Server RPM on the Host
               Machine
               :name: Step1:InstallUnravelServeronMapR-2.InstalltheUnravelServerRPMontheHostMachine

            .. rubric:: Get the Unravel Server RPM
               :name: Step1:InstallUnravelServeronMapR-GettheUnravelServerRPM

            See `Download Unravel
            Software <https://unraveldata.atlassian.net/wiki/spaces/UNDOCS/pages/226132074/Download+Unravel+Software+Versions>`__.

            .. rubric:: Make symlinks if required
               :name: Step1:InstallUnravelServeronMapR-Makesymlinksifrequired

            If you want the two disk areas used by Unravel to be on
            different volumes, you can make symlinks to specific areas
            before installing (or do a mv and symlink after installing).
            Do it before the first install if there is insufficient
            space on the target paths \ ``/usr/local/unravel`` and
            ``/srv/unravel`` noted above.

            .. rubric:: Install the Unravel Server RPM
               :name: Step1:InstallUnravelServeronMapR-InstalltheUnravelServerRPM

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

                     # sudo rpm -U unravel-4.*.x86_64.rpm*
                     # /usr/local/unravel/install_bin/await_fixups.sh

            The precise filename can vary, depending on how it was
            fetched or copied. The \ ``rpm`` command does not require
            .\ ``rpm`` suffix. The flag \ ``-U ``\ works for either
            initial install or upgrade.

            Run the specified \ ``await_fizup.sh`` script to make sure
            background processing is finished before you do other steps.
            In a routine upgrade, it is okay to start all Unravel
            daemons, but do not stop or restart them until
            the \ ``await_fizup.sh`` prints ``DONE`` (it takes a few
            minutes).

            .. container::
            confluence-information-macro confluence-information-macro-information

               .. container:: confluence-information-macro-body

                  The installation creates \ ``/usr/local/unravel/``
                  which contains the executables, scripts, and settings.
                  User \ ``unravel`` is created. The initial internal
                  database and other durable state are put
                  in \ ``/srv/unravel/`` for larger storage. 

                  | 
                  | The master configuration file is
                    in \ ``/usr/local/unravel/etc/unravel.properties``
                    and the logs are in ``/usr/local/unravel/logs/``.
                    The RPM installation creates user \ ``unravel`` if
                    it does not already exist and this can be changed
                    after installation; \ ``/etc/init.d/unravel_*``
                    scripts for controlling its services as well
                    as \ ``/etc/init.d/unravel_all.sh`` which can be
                    used to manually stop, start, and get status of all
                    daemons in proper order.

                  | 
                  | During initial install, a bundled database is used.
                    This can be switched to use an `externally managed
                    MySQL <Installing-MySQL-or-Compatible-Database-for-Unravel_541131376.html>`__
                    for production. 

            .. rubric:: Do Host-Specific Post-Installation Actions
               :name: Step1:InstallUnravelServeronMapR-DoHost-SpecificPost-InstallationActions

            Run the following commands on Unravel Server:

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

                     # sudo /etc/init.d/unravel_all.sh stop
                     # sudo /usr/local/unravel/install_bin/switch_to_mapr.sh

            .. rubric:: 3. Configure Unravel Server (Basic/Core Options)
               :name: Step1:InstallUnravelServeronMapR-3.ConfigureUnravelServer(Basic/CoreOptions)

            .. rubric:: Enable Optional Daemons
               :name: Step1:InstallUnravelServeronMapR-EnableOptionalDaemons

            Depending on your workload volume or kind of activity, you
            can enable additional daemons at this point, see \ `Creating
            Multiple Workers for High Volume
            Data <Creating-Multiple-Workers-for-High-Volume-Data_541131395.html>`__

            .. rubric:: Modify ``unravel.properties``
               :name: Step1:InstallUnravelServeronMapR-Modifyunravel.properties

            -  Open ``/usr/local/unravel/etc/unravel.properties`` with
               ``vi``.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo vi /usr/local/unravel/etc/unravel.properties

            -  Edit the values in \ ``unravel.properties`` using the
               guidelines and descriptions in the table below. Some of
               these need to be uncommented (remove the leading # char)
               after you edit to enable them.

               .. container::

                  | 

                  .. container:: table-wrap

                     +-----------------------+-----------------------+-----------------------+
                     | Property              | Description           | Example Values        |
                     +=======================+=======================+=======================+
                     | ``com.unraveldata.adv | Defines the Unravel   | ::                    |
                     | ertised.url``         | Server URL for HTTP   |                       |
                     |                       | traffic.              |    http://LAN_DNS:300 |
                     |                       |                       | 0                     |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.cus | Identifies your       | ::                    |
                     | tomer.organization``  | installation for      |                       |
                     |                       | reporting purposes.   |    Company_and_org    |
                     +-----------------------+-----------------------+-----------------------+
                     | ::                    | **Optional**.         | ::                    |
                     |                       | Location where        |                       |
                     |    com.unraveldata.tm | Unravel's temp file   |    /srv/unravel/tmp   |
                     | pdir                  | will reside           |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.his | Sets retention for    | ::                    |
                     | tory.maxSize.weeks``  | search data.          |                       |
                     |                       |                       |    26                 |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.hiv | **Optional**. Defines | ::                    |
                     | e.hook.topic.num.thre | the number of         |                       |
                     | ads``                 | threads. Default is   |    1                  |
                     |                       | 1. Depending on job   |                       |
                     |                       | volume, increase this |                       |
                     |                       | property to *N* where |                       |
                     |                       | *N* is between 1 and  |                       |
                     |                       | 4, or roughly         |                       |
                     |                       | *ThousandJobsPerDay*/ |                       |
                     |                       | 10.                   |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.job | HDFS path to "done"   | ::                    |
                     | .collector.done.log.b | directory of MR logs  |                       |
                     | ase``                 |                       |    /var/mapr/cluster/ |
                     |                       |                       | yarn/rm/staging/histo |
                     |                       |                       | ry/done               |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.job | An HDFS path that     | ::                    |
                     | .collector.log.aggreg | helps locate MR job   |                       |
                     | ation.base``          | logs to process       |    /tmp/logs/*/logs/  |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.log | Defines the usernames | ::                    |
                     | in.admins``           | that can access       |                       |
                     |                       | Unravel Web UI's      |    admin              |
                     |                       | admin pages. Default  |                       |
                     |                       | is ``admin``.         |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.spa | Where to find Spark   | ::                    |
                     | rk.eventlog.location` | event logs            |                       |
                     | `\                    |                       |    maprfs:///apps/spa |
                     |                       |                       | rk                    |
                     +-----------------------+-----------------------+-----------------------+
                     | ``yarn.resourcemanage | Resource Manager web  | ::                    |
                     | r.webapp.address``    | app address           |                       |
                     |                       |                       |    http://example.loc |
                     |                       |                       | aldomain:8088         |
                     +-----------------------+-----------------------+-----------------------+
                     | ``yarn.resourcemanage | Resource Manager      |                       |
                     | r.webapp.username``   | username to login     |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``yarn.resourcemanage | Resource Manager      |                       |
                     | r.webapp``.password   | password to login     |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``https.protocols``   | Enable https access   | TLSv1.2               |
                     |                       | to Resource Manager   |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``javax.jdo.option.Co | A JDBC connection URL | jdbc:mysql://example. |
                     | nnectionURL``         |                       | localdomain:3306/hive |
                     |                       |                       | **OR**                |
                     |                       |                       | jdbc:postgresql://exa |
                     |                       |                       | mple.localdomain:7432 |
                     |                       |                       | /hive_zzzzzz          |
                     +-----------------------+-----------------------+-----------------------+
                     | ``javax.jdo.option.Co | JDBC driver           | com.mysql.jdbc.Driver |
                     | nnectionDriverName``  |                       | **OR** org.postgresql |
                     |                       |                       | .Driver               |
                     +-----------------------+-----------------------+-----------------------+
                     | ``javax.jdo.option.Co | Hive metastore user   | hiveuser              |
                     | nnectionUserName``    | name                  |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``javax.jdo.option.Co | Hive metastore        |                       |
                     | nnectionPassword``    | password              |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata``.m | **Optional**.  Be     | s*|t*|d\*             |
                     | etastore.databasePatt | selective for         |                       |
                     | ern                   | databases to analyze  |                       |
                     |                       | in metastore          |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``oozie.server.url``  | Oozie URL             | ::                    |
                     |                       |                       |                       |
                     |                       |                       |    http://example.loc |
                     |                       |                       | aldomain:11000/oozie  |
                     +-----------------------+-----------------------+-----------------------+

            .. rubric:: If Kerberos is Enabled:
               :name: Step1:InstallUnravelServeronMapR-IfKerberosisEnabled:

            .. container:: expand-container
               :name: expander-1985410250

               .. container:: expand-control
                  :name: expander-control-1985410250

                  Add authentication for HDFS...

               .. container:: expand-content
                  :name: expander-content-1985410250

                  Create or identify a principal and keytab for Unravel
                  daemons to access HDFS and REST when Kerberos is
                  enabled. 

                  To get going faster, you can use the 'hdfs' principal
                  which often has a pre-existing "headless" keytab.

                  Add properties for Kerberos
                  in \ ``/usr/local/unravel/etc/unravel.properties``
                  (substitute correct filename and principal):

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           com.unraveldata.kerberos.principal=unravel/myhost.mydomain@MYREALM
                           com.unraveldata.kerberos.keytab.path=/usr/local/unravel/etc/unravel.keytab

                  You can find and verify the principal the keytab by

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           # klist -kt KEYTAB_FILE

                  .. container::
                  confluence-information-macro confluence-information-macro-information

                     .. container:: confluence-information-macro-body

                        The keytab file should have chmod bits 500 and
                        be owned by ``unravel`` local user (default) or
                        by the user you want to use, as explained
                        in \ `Run Unravel Daemons with Custom
                        User <Run-Unravel-Daemons-with-Custom-User_541033161.html>`__.

            .. rubric:: If Sentry is Enabled:
               :name: Step1:InstallUnravelServeronMapR-IfSentryisEnabled:

            .. container:: expand-container
               :name: expander-1312819624

               .. container:: expand-control
                  :name: expander-control-1312819624

                  Add these permissions...

               .. container:: expand-content
                  :name: expander-content-1312819624

                  For quicker setup, use the mapr principal. For more
                  narrow privileges, define your own alt principal. The
                  alt user can be admin \ ``unravel ``\ (created
                  by \ ``rpm``) or one of your choosing. The
                  corresponding kerberos principal does not need to have
                  the same name as the local user. The user/principal
                  here should correspond to the \ ``X`` in the next
                  section. The paths listed are examples, only. In your
                  environment, they might be customized.

                  .. container:: table-wrap

                     +-----------------+-----------------+-----------------+-----------------+
                     | Resource        | Principal       | Access          | Purpose         |
                     +=================+=================+=================+=================+
                     | ``hdfs://user/s | mapr or alt     | read            | Spark event log |
                     | park/applicatio |                 |                 |                 |
                     | nHistory``      |                 |                 |                 |
                     +-----------------+-----------------+-----------------+-----------------+
                     | ``hdfs://usr/hi | mapr or alt     | read            | MapReduce logs  |
                     | story/done``    |                 |                 |                 |
                     +-----------------+-----------------+-----------------+-----------------+
                     | ``hdfs://tmp/lo | mapr or alt     | read            | YARN            |
                     | gs``            |                 |                 | aggregation     |
                     |                 |                 |                 | folder          |
                     +-----------------+-----------------+-----------------+-----------------+
                     | ``hdfs://user/h | mapr or alt     | read            | Obtain table    |
                     | ive/warehouse`` |                 |                 | partition sizes |
                     +-----------------+-----------------+-----------------+-----------------+
                     | ``Hive Metastor | hive            | read            | Hive table      |
                     | e access``      |                 |                 | information     |
                     +-----------------+-----------------+-----------------+-----------------+

            .. rubric:: Switch User
               :name: Step1:InstallUnravelServeronMapR-SwitchUser

            Depending on your cluster security configuration, you will
            need to run the \ ``switch_to_user``  script. Dependencies
            like kerberos and which target user you used for access
            permissions affect this. 

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

                     # sudo /usr/local/unravel/install_bin/switch_to_user.sh x y 

            where ``X`` and ``Y`` depend on your environment. See the
            `switch_to_user <Run-Unravel-Daemons-with-Custom-User_541033161.html>`__ page. 

            .. rubric:: Do Host-Specific Configuration Steps
               :name: Step1:InstallUnravelServeronMapR-DoHost-SpecificConfigurationSteps

            For MapR, there are no host-specific configuration steps.

            .. rubric:: Restart Unravel Server
               :name: Step1:InstallUnravelServeronMapR-RestartUnravelServer

            After edits to ``com.unraveldata.login.admins`` in
            ``/usr/local/unravel/etc/unravel.properties`` it is
            necessary to run the following script in order to make
            changes take effect. The ``echo`` command shows the page to
            visit with your browser. If you are using an ssh tunnel or
            http proxy, you might need to make adjustments. 

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

                     # sudo /etc/init.d/unravel_all.sh start
                     # sleep 60
                     # echo "http://({UNRAVEL_HOST_IP} -f):3000/"

            This completes the basic/core configuration.

            .. rubric:: 4. Log into Unravel Web UI
               :name: Step1:InstallUnravelServeronMapR-4.LogintoUnravelWebUI

            Using a web browser, navigate
            to\ ``http://({UNRAVEL_``\ ``HOST``\ \_IP} -f):3000/ and
            login as user ``admin`` with password ``unraveldata``.

            .. container::
            confluence-information-macro confluence-information-macro-note

               .. container:: confluence-information-macro-body

                  For the free trial version, use the Chrome web
                  browser.

            .. rubric:: 
               :name: Step1:InstallUnravelServeronMapR-

            .. rubric:: Congratulations!
               :name: Step1:InstallUnravelServeronMapR-Congratulations!

            Unravel Server is up and running. Unravel Web UI displays
            collected data. Check Unravel Web UI for MR jobs loading: on
            the **Applications** page, select the **Map Reduce** tab.

            For instructions on using Unravel Web UI, see the `User
            Guide <User-Guide_541295329.html>`__.

            .. rubric:: 5. (Optional) Configure Unravel Server (Advanced
               Options)
               :name: Step1:InstallUnravelServeronMapR-5.(Optional)ConfigureUnravelServer(AdvancedOptions)

            .. rubric:: Enable Additional Data
               Collection/Instrumentation
               :name: Step1:InstallUnravelServeronMapR-EnableAdditionalDataCollection/Instrumentation

            Install the Unravel Sensor Parcel on ``gateway/edge/client``
            nodes that are used to submit Hive queries to push
            additional information to Unravel Server. For details, see
            `Step 2: Enable Additional Data Collection / Instrumentation
            for MapR <541361101.html>`__.

            .. rubric:: Run the Unravel Web UI Configuration Wizard
               :name: Step1:InstallUnravelServeronMapR-RuntheUnravelWebUIConfigurationWizard

            Run the Unravel Web UI configuration wizard to choose
            additional configuration options. For instructions on
            configuring advanced options, see the `Advanced
            Topics <Advanced-Topics_541197049.html>`__.

            | 

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image0|
               `image2017-2-26_0-20-12.png <attachments/541361105/541229862.png>`__
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
