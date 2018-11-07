:title: Step 1: Install Unravel Server on HDP

Step 1: Install Unravel Server on HDP
==========================================

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

         .. rubric:: Unravel 4.4 : Step 1: Install Unravel Server on HDP
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified on Sep 25, 2018

         .. container:: wiki-content group
            :name: main-content

            .. container:: panel

               .. container:: panelHeader

                  **Table of Contents**

               .. container:: panelContent

                  .. container:: toc-macro rbtoc1541196938266

                     -  `Introduction <#Step1:InstallUnravelServeronHDP-Introduction>`__
                     -  `Pre-Installation
                        Check <#Step1:InstallUnravelServeronHDP-Pre-InstallationCheck>`__

                        -  `Platform
                           Compatibility <#Step1:InstallUnravelServeronHDP-PlatformCompatibility>`__
                        -  `Hardware <#Step1:InstallUnravelServeronHDP-Hardware>`__
                        -  `Software <#Step1:InstallUnravelServeronHDP-Software>`__
                        -  `Access
                           Permissions <#Step1:InstallUnravelServeronHDP-AccessPermissions>`__
                        -  `Network <#Step1:InstallUnravelServeronHDP-Network>`__

                     -  `Installation <#Step1:InstallUnravelServeronHDP-Installation>`__

                        -  `1. Configure the Host
                           Machine <#Step1:InstallUnravelServeronHDP-1.ConfiguretheHostMachine>`__

                           -  `Allocate a Cluster Gateway/Edge/Client
                              Host with HDFS
                              Access <#Step1:InstallUnravelServeronHDP-AllocateaClusterGateway/Edge/ClientHostwithHDFSAccess>`__

                        -  `2. Install the Unravel Server RPM on the
                           Host
                           Machine <#Step1:InstallUnravelServeronHDP-2.InstalltheUnravelServerRPMontheHostMachine>`__

                           -  `Get the Unravel Server
                              RPM <#Step1:InstallUnravelServeronHDP-GettheUnravelServerRPM>`__
                           -  `Make symlinks if
                              required <#Step1:InstallUnravelServeronHDP-Makesymlinksifrequired>`__
                           -  `Install the Unravel Server
                              RPM <#Step1:InstallUnravelServeronHDP-InstalltheUnravelServerRPM>`__
                           -  `Do Host-Specific Post-Installation
                              Actions <#Step1:InstallUnravelServeronHDP-DoHost-SpecificPost-InstallationActions>`__

                        -  `3. Configure Unravel Server (Basic/Core
                           Options) <#Step1:InstallUnravelServeronHDP-3.ConfigureUnravelServer(Basic/CoreOptions)>`__

                           -  `Enable Optional
                              Daemons <#Step1:InstallUnravelServeronHDP-EnableOptionalDaemons>`__
                           -  `Modify
                              unravel.properties <#Step1:InstallUnravelServeronHDP-Modifyunravel.properties>`__
                           -  `If Kerberos is
                              Enabled: <#Step1:InstallUnravelServeronHDP-IfKerberosisEnabled:>`__
                           -  `If Ranger is
                              Enabled: <#Step1:InstallUnravelServeronHDP-IfRangerisEnabled:>`__
                           -  `Disable Impala
                              Sensor <#Step1:InstallUnravelServeronHDP-DisableImpalaSensor>`__

                        -  `4. Convert Your Unravel Installation to
                           HDP <#Step1:InstallUnravelServeronHDP-4.ConvertYourUnravelInstallationtoHDP>`__

                           -  `Run the following commands on Unravel
                              Server: <#Step1:InstallUnravelServeronHDP-RunthefollowingcommandsonUnravelServer:>`__
                           -  `Switch
                              User <#Step1:InstallUnravelServeronHDP-SwitchUser>`__
                           -  `Restart Unravel
                              Server <#Step1:InstallUnravelServeronHDP-RestartUnravelServer>`__

                        -  `5. Log into Unravel Web
                           UI <#Step1:InstallUnravelServeronHDP-5.LogintoUnravelWebUI>`__
                        -  `6. Configure Unravel Server (Advanced
                           Options) <#Step1:InstallUnravelServeronHDP-6.ConfigureUnravelServer(AdvancedOptions)>`__

                           -  `Enable Additional Data
                              Collection/Instrumentation <#Step1:InstallUnravelServeronHDP-EnableAdditionalDataCollection/Instrumentation>`__

            .. rubric:: Introduction
               :name: Step1:InstallUnravelServeronHDP-Introduction

            This topic explains how to deploy Unravel Server 4.0 on HDP.
            These instructions apply to Unravel Server 4.0. For older
            versions of Unravel Server, contact Unravel Support.

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
               :name: Step1:InstallUnravelServeronHDP-Pre-InstallationCheck

            The following installation requirements must be met for
            successful installation of Unravel.

            .. rubric:: Platform Compatibility
               :name: Step1:InstallUnravelServeronHDP-PlatformCompatibility

            -  HDP 2.2-2.6
            -  Hadoop 1.x - 2.x
            -  Kafka 0.9-1.0 (Apache ver. equiv.)
            -  Kerberos
            -  Hive 0.9.x - 1.2.x
            -  Spark 1.3.x - 2.2.x

            .. rubric:: Hardware
               :name: Step1:InstallUnravelServeronHDP-Hardware

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
               :name: Step1:InstallUnravelServeronHDP-Software

            -  Operating System: RedHat/Centos 6.4 - 7.4
            -  ``libaio.x86_64`` installed
            -  ``SELINUX=permissive`` (or disabled) should be set in
               ``/etc/selinux/config``
            -  HDFS+Hive+YARN client/gateway, Hadoop and Hive commands
               in ``PATH``
            -  If Spark is in use, Spark client gateway
            -  Verify that all clients are installed by checking that
               RPMs are installed; Unravel utilizes clients and
               associated libraries
            -  You need to register edge node to Ambari 
            -  LDAP (AD or Open LDAP) compatible for Unravel Web UI user
               authentication (Open signup by default)
            -  On Unravel Edge-node server, please \ **do not** have
               zookeeper installed in same server

            .. rubric:: Access Permissions
               :name: Step1:InstallUnravelServeronHDP-AccessPermissions

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
            -  Application Timeline Server (ATS) read-only

            .. rubric:: Network
               :name: Step1:InstallUnravelServeronHDP-Network

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
            -  ATS port 8188 from Unravel Servers(s) to ATS server(s)
            -  Port 4020, 4176, 4181 through 4189, 3316, 4091 must be
               available for localhost communication between Unravel
               daemons or Unravel servers (if multi-host Unravel
               installation)

            .. rubric:: Installation
               :name: Step1:InstallUnravelServeronHDP-Installation

            .. rubric:: 1. Configure the Host Machine
               :name: Step1:InstallUnravelServeronHDP-1.ConfiguretheHostMachine

            .. rubric:: Allocate a Cluster Gateway/Edge/Client Host with
               HDFS Access
               :name: Step1:InstallUnravelServeronHDP-AllocateaClusterGateway/Edge/ClientHostwithHDFSAccess

            For HDP, use Ambari Web UI to create a Gateway node
            configuration.

            .. rubric:: 2. Install the Unravel Server RPM on the Host
               Machine
               :name: Step1:InstallUnravelServeronHDP-2.InstalltheUnravelServerRPMontheHostMachine

            .. container::

               .. rubric:: Get the Unravel Server RPM
                  :name: Step1:InstallUnravelServeronHDP-GettheUnravelServerRPM

               See `Download Unravel
               Software <https://unraveldata.atlassian.net/wiki/spaces/UNDOCS/pages/226132074/Download+Unravel+Software+Versions>`__.

               .. rubric:: Make symlinks if required
                  :name: Step1:InstallUnravelServeronHDP-Makesymlinksifrequired

               If you want the two disk areas used by Unravel to be on
               different volumes, you can make symlinks to specific
               areas before installing (or do
               a \ ``mv`` and ``symlink`` symlink after installing). Do
               it before the first install if there is insufficient
               space on the target paths \ ``/usr/local/unravel``
               and \ ``/srv/unravel`` noted above. 

               .. rubric:: Install the Unravel Server RPM
                  :name: Step1:InstallUnravelServeronHDP-InstalltheUnravelServerRPM

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo rpm -U unravel-4.*.x86_64.rpm*
                        # /usr/local/unravel/install_bin/await_fixups.sh

               The precise filename can vary, depending on how it was
               fetched or copied. The ``rpm`` command does not require
               .\ ``rpm`` suffix. The flag \ ``-U`` works for either
               initial install or upgrade.

               Run the specified \ ``await_fixups.sh`` script to make
               sure background processing is finished before you do
               other steps. In a routine upgrade, it is okay to start
               all Unravel daemons, but do not stop or restart them
               until the \ ``await_fixups.sh`` prints ``Done`` (it takes
               a few minutes).

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     | The installation
                       creates \ ``/usr/local/unravel/`` which contains
                       the executables, scripts, and settings.
                       User \ ``unravel`` is created. The initial
                       internal database and other durable state are put
                       in \ ``/srv/unravel/`` for larger storage. By
                       default, the installation supports YARN.
                     | The master configuration file is
                       in \ ``/usr/local/unravel/etc/unravel.properties``
                       and the logs are in ``/usr/local/unravel/logs/``.
                       The RPM installation creates user \ ``unravel``
                       if it does not already
                       exist; \ ``/etc/init.d/unravel_*`` scripts for
                       controlling its services as well
                       as \ ``/etc/init.d/unravel_all.sh`` which can be
                       used to manually stop, start, and get status of
                       all daemons in proper order.
                     | The RPM installation also creates an HDFS
                       directory for Hive Hook information collection.
                     | During initial install, a bundled database is
                       used. This can be switched to use an externally
                       managed MySQL for production. (The bundled
                       database root mysql password will be stored
                       in \ ``/root/unravel.install.include`` during
                       installation.)

               .. rubric:: Do Host-Specific Post-Installation Actions
                  :name: Step1:InstallUnravelServeronHDP-DoHost-SpecificPost-InstallationActions

               For HDP, there are no host-specific post-installation
               actions.

            .. rubric:: 3. Configure Unravel Server (Basic/Core Options)
               :name: Step1:InstallUnravelServeronHDP-3.ConfigureUnravelServer(Basic/CoreOptions)

            .. container::

               .. rubric:: Enable Optional Daemons
                  :name: Step1:InstallUnravelServeronHDP-EnableOptionalDaemons

               Depending on your workload volume or kind of activity,
               you can enable optional daemons at this point.
               See \ `Creating Multiple Workers for High Volume
               Data <Creating-Multiple-Workers-for-High-Volume-Data_541131395.html>`__.

               .. rubric:: Modify ``unravel.properties``
                  :name: Step1:InstallUnravelServeronHDP-Modifyunravel.properties

               -  Open ``/usr/local/unravel/etc/unravel.properties``
                  with ``vi``.

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           # sudo vi /usr/local/unravel/etc/unravel.properties

               -  Edit the values in \ ``unravel.properties`` using the
                  guidelines and descriptions in the table below.

                  .. container:: table-wrap

                     +-----------------------+-----------------------+-----------------------+
                     | Property              | Description           | Example Values        |
                     +=======================+=======================+=======================+
                     | ``com.unraveldata.adv | Defines the Unravel   | ``http://LAN_DNS:3000 |
                     | ertised.url``         | Server URL for HTTP   | ``                    |
                     |                       | traffic.              |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.cus | Identifies your       | ``Company_and_org``   |
                     | tomer.organization``  | installation for      |                       |
                     |                       | reporting purposes.   |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ::                    | Location where        | ::                    |
                     |                       | Unravel's temp file   |                       |
                     |    com.unraveldata.tm | will reside           |    /srv/unravel/tmp   |
                     | pdir                  |                       |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.his | Sets retention for    | ``26``                |
                     | tory.maxSize.weeks``  | search data.          |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.job | Only modifiable       | ::                    |
                     | .collector.done.log.b | through Unravel Web   |                       |
                     | ase``                 | UI's configuration    |    /mr-history/done   |
                     |                       | wizard.               |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.job | Only modifiable       | ::                    |
                     | .collector.log.aggreg | through Unravel Web   |                       |
                     | ation.base``          | UI's configuration    |    /app-logs/*/logs/  |
                     |                       | wizard.               |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.log | Defines the usernames | ``admin``             |
                     | in.admins``           | that can access       |                       |
                     |                       | Unravel Web UI's      |                       |
                     |                       | admin pages. Default  |                       |
                     |                       | is ``admin``.         |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.s3. | **Optional**. Defines | ``120``               |
                     | batch.monitoring.inte | the monitoring        |                       |
                     | rval.sec``            | frequency. Default is |                       |
                     |                       | 300 seconds (5        |                       |
                     |                       | minutes). Set this    |                       |
                     |                       | property to 60 for    |                       |
                     |                       | lower latency.        |                       |
                     +-----------------------+-----------------------+-----------------------+
                     | ``com.unraveldata.spa | Where to find Spark   | ::                    |
                     | rk.eventlog.location` | event logs            |                       |
                     | `\                    |                       |    hdfs:///user/spark |
                     |                       |                       | /applicationHistory/, |
                     |                       |                       | hdfs:///user/spark/sp |
                     |                       |                       | arkApplicationHistory |
                     |                       |                       | /,hdfs:///user/spark/ |
                     |                       |                       | spark2ApplicationHist |
                     |                       |                       | ory/                  |
                     +-----------------------+-----------------------+-----------------------+
                     | ``yarn.resourcemanage | YARN resource manager | ::                    |
                     | r.webapp.address``    | web address URL       |                       |
                     |                       |                       |    http://example.loc |
                     |                       |                       | aldomain:8088         |
                     +-----------------------+-----------------------+-----------------------+
                     | ``oozie.server.url``  | Oozie URL             | ::                    |
                     |                       |                       |                       |
                     |                       |                       |    http://example.loc |
                     |                       |                       | aldomain:11000/oozie  |
                     +-----------------------+-----------------------+-----------------------+

                  .. rubric:: If Kerberos is Enabled:
                     :name: Step1:InstallUnravelServeronHDP-IfKerberosisEnabled:

                  .. container:: expand-container
                     :name: expander-1842118070

                     .. container:: expand-control
                        :name: expander-control-1842118070

                        Add authentication for HDFS...

                     .. container:: expand-content
                        :name: expander-content-1842118070

                        Create or identify a principal and keytab for
                        Unravel daemons to access HDFS and REST when
                        Kerberos is enabled. 

                        To get going faster, you can use the 'hdfs'
                        principal which often has a pre-existing
                        "headless" keytab.

                        Add properties for Kerberos
                        in \ ``/usr/local/unravel/etc/unravel.properties``
                        (substitute correct filename and principal):

                        .. container:: code panel pdl

                           .. container:: codeContent panelContent pdl

                              .. code:: syntaxhighlighter-pre

                                 com.unraveldata.kerberos.principal=unravel/myhost.mydomain@MYREALM
                                 com.unraveldata.kerberos.keytab.path=/usr/local/unravel/etc/unravel.keytab

                        You can verify the principal in a keytab by
                        using \ ``properties klist -kt KEYTAB_FILE``\ *.*
                        The keytab file should have chmod bits 500 and
                        be owned by 'unravel' local user (default) or by
                        the user you want to use, as explained in \ `Run
                        Unravel Daemons with Custom
                        User <541131652.html>`__.

                  .. rubric:: If Ranger is Enabled:
                     :name: Step1:InstallUnravelServeronHDP-IfRangerisEnabled:

                  .. container:: expand-container
                     :name: expander-1683274762

                     .. container:: expand-control
                        :name: expander-control-1683274762

                        Add these permissions...

                     .. container:: expand-content
                        :name: expander-content-1683274762

                        For quicker setup, use the hdfs principal. For
                        more narrow privileges, define your own alt
                        principal. The alt user can
                        be \ ``unravel`` (created by \ ``rpm``) or one
                        of your choosing. The corresponding kerberos
                        principal does not need to have the same name as
                        the local user. The user/principal here should
                        correspond to the \ ``X``  in the switch-user
                        section below. 

                        .. container:: table-wrap

                           +----------------------------+-----+------+---------------------------+
                           | Resource                   | Pri | Acce | Purpose                   |
                           |                            | nci | ss   |                           |
                           |                            | pal |      |                           |
                           +============================+=====+======+===========================+
                           | ``hdfs://spark-history``   | hdf | read | Spark event log           |
                           |                            | s   | +exe |                           |
                           |                            |     | cute |                           |
                           +----------------------------+-----+------+---------------------------+
                           | hdfs://spark2-history      | hdf | read | Spark2 event log          |
                           |                            | s   | +exe |                           |
                           |                            |     | cute |                           |
                           +----------------------------+-----+------+---------------------------+
                           | ``hdfs://mr-history/done`` | hdf | read | MapReduce logs            |
                           |                            | s   | +exe |                           |
                           |                            |     | cute |                           |
                           +----------------------------+-----+------+---------------------------+
                           | ``hdfs://app-logs``        | hdf | read | YARN aggregation folder   |
                           |                            | s   | +exe |                           |
                           |                            |     | cute |                           |
                           +----------------------------+-----+------+---------------------------+
                           | ``hdfs://apps/hive/warehou | hdf | read | Obtain table partition    |
                           | se (default value of hive. | s   | +exe | sizes                     |
                           | metastore.warehouse.dir)`` |     | cute |                           |
                           +----------------------------+-----+------+---------------------------+
                           | Hive Metastore database    | hiv | read | Hive table information    |
                           | GRANT                      | e   | +exe |                           |
                           |                            |     | cute |                           |
                           +----------------------------+-----+------+---------------------------+

                  .. rubric:: Disable Impala Sensor
                     :name: Step1:InstallUnravelServeronHDP-DisableImpalaSensor

                  .. container::
                  confluence-information-macro confluence-information-macro-information

                     .. container:: confluence-information-macro-body

                        Impala is not officially supported on HDP
                        clusters therefore you should disable the 
                        ImpalaSensor by modifying
                        the \ ``/usr/local/unravel/etc/unravel.properties`` file
                        on Unravel Server:

                  Open \ ``unravel.properties ``\ file.  Locate or
                  add \ ``com.unraveldata.sensor.tasks.disabled``  and
                  set it as shown:

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           com.unraveldata.sensor.tasks.disabled=iw

            .. rubric:: 4. Convert Your Unravel Installation to HDP
               :name: Step1:InstallUnravelServeronHDP-4.ConvertYourUnravelInstallationtoHDP

            .. container::

               .. rubric:: **Run the following commands on Unravel
                  Server:**
                  :name: Step1:InstallUnravelServeronHDP-RunthefollowingcommandsonUnravelServer:

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo /etc/init.d/unravel_all.sh stop
                        # sudo /usr/local/unravel/install_bin/switch_to_hdp.sh

               | 

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     Note: This change will stick after later RPM
                     upgrades; it does not need to be done each time.

               .. rubric:: Switch User
                  :name: Step1:InstallUnravelServeronHDP-SwitchUser

               Depending on your cluster security configuration, you
               will need to run the
               `switch_to_user <Run-Unravel-Daemons-with-Custom-User_541033161.html>`__ script.
               Dependencies like kerberos and which target user you used
               for Ranger affect this. 

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo /usr/local/unravel/install_bin/switch_to_user.sh x y 

               where \ ``X``  and ``Y`` depend on your environment. See
               `switch_to_use <Run-Unravel-Daemons-with-Custom-User_541033161.html>`__
               page. 

               .. rubric:: Restart Unravel Server
                  :name: Step1:InstallUnravelServeronHDP-RestartUnravelServer

               After edits to ``com.unraveldata.login.admins`` in
               ``/usr/local/unravel/etc/unravel.properties`` it is
               necessary to run the following script in order to make
               changes take effect. The ``echo`` command shows the page
               to visit with your browser. If you are using an ssh
               tunnel or http proxy, you might need to make adjustments.
               ``UNRAVEL_HOST_IP`` is a fully qualified DNS or IP
               Address.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo /etc/init.d/unravel_all.sh start
                        # sleep 60
                        # echo "http://$(hostname -f):3000/"

               This completes the basic/core configuration.

            .. rubric:: 5. Log into Unravel Web UI
               :name: Step1:InstallUnravelServeronHDP-5.LogintoUnravelWebUI

            .. container::

               Using a web browser, navigate to
               ``http://{UNRAVEL_``\ ``HOST``\ \_IP}:3000 and login as
               user "``admin"`` with password ``"unraveldata``".

               .. container::
               confluence-information-macro confluence-information-macro-note

                  .. container:: confluence-information-macro-body

                     For the free trial version, use the Chrome web
                     browser.

               **Congratulations!**

               Unravel Server is up and running. Unravel Web UI displays
               collected data. For instructions on using Unravel Web UI,
               see the `User Guide <User-Guide_541295329.html>`__.

            .. rubric:: 6. Configure Unravel Server (Advanced Options)
               :name: Step1:InstallUnravelServeronHDP-6.ConfigureUnravelServer(AdvancedOptions)

            .. container::

               .. rubric:: Enable Additional Data
                  Collection/Instrumentation
                  :name: Step1:InstallUnravelServeronHDP-EnableAdditionalDataCollection/Instrumentation

               Install the Unravel Sensor Parcel on gateway/edge/client
               nodes that are used to submit Hive queries to push
               additional information to Unravel Server. For details,
               see `Step 2: Enable Additional Data Collection /
               Instrumentation for HDP <561709534.html>`__.

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image0|
               `image2017-2-26_0-20-12.png <attachments/541098908/541393753.png>`__
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
