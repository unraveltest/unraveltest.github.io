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

         .. rubric:: Unravel 4.4 : Step 3: Enable Impala APM
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified on Sep 10, 2018

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: Introduction
               :name: Step3:EnableImpalaAPM-IntroductionIntroduction

            This topic explains how to configure Unravel Server to
            retrieve Impala query data from either ClouderaManager (CM)
            or Impala daemons (``impalad``). It assumes that you already
            have installed Impala on your CDH cluster.

            Before following these instructions, follow the steps in
            `Step 1: Install Unravel Server on
            CDH+CM <https://unraveldata.atlassian.net/wiki/spaces/UN43/pages/226197657>`__.

            .. container::
            confluence-information-macro confluence-information-macro-information

               .. container:: confluence-information-macro-body

                  By default, the ImpalaSensor task is enabled. If user
                  wishes to disable ImpalaSensor, specify the following
                  option
                  in \ ``/usr/local/unravel/etc/unravel.properties`` on
                  Unravel Server.

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

                     com.unraveldata.sensor.tasks.disabled=iw

            .. container:: panel

               .. container:: panelHeader

                  **Workflow Summary**

               .. container:: panelContent

                  #. If you want Unravel Server to retrieve Impala query
                     data from ClouderaManager, start at `Using CM as
                     the Data
                     Source <#Step3:EnableImpalaAPM-UsingCM>`__.
                  #. If you want Unravel Server to retrieve Impala query
                     data from Impala daemons, start at `Using Impalad
                     as the Data
                     Source <#Step3:EnableImpalaAPM-UsingImpalad>`__.

            .. rubric:: Using CM as the Data Source
               :name: Step3:EnableImpalaAPM-UsingCMUsingCMastheDataSource

            In order to this you need to
            add \ ``com.unraveldata.data.source=cm``
            in \ ``/usr/local/unravel/etc/unravel.properties`` on
            Unravel Server. Further, you need to tell Unravel Server
            some information about your CM: URL, port number, login
            credentials, and so on. You do this by adding the following
            properties
            to \ ``/usr/local/unravel/etc/unravel.properties`` on
            Unravel Server:

            .. container:: table-wrap

               +-----------------------------------+-----------------------------------+
               | Property                          | Description                       |
               +===================================+===================================+
               | ::                                | CM internal URL. Must start with  |
               |                                   | ``http://`` or`` https://``       |
               |    com.unraveldata.cloudera.manag |                                   |
               | er.url                            |                                   |
               +-----------------------------------+-----------------------------------+
               | ::                                | (Optional) CM port number. You    |
               |                                   | only need to specify this if your |
               |    com.unraveldata.cloudera.manag | Cloudera Manager is not on port   |
               | er.port                           | 7180.                             |
               +-----------------------------------+-----------------------------------+
               | ::                                | CM username                       |
               |                                   |                                   |
               |    com.unraveldata.cloudera.manag |                                   |
               | er.username                       |                                   |
               +-----------------------------------+-----------------------------------+
               | ::                                | CM password                       |
               |                                   |                                   |
               |    com.unraveldata.cloudera.manag |                                   |
               | er.password                       |                                   |
               +-----------------------------------+-----------------------------------+

            For example:

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

                     com.unraveldata.data.source=cm
                     com.unraveldata.cloudera.manager.url=http://mycm.somewhere.secret
                     com.unraveldata.cloudera.manager.port=9997
                     com.unraveldata.cloudera.manager.username=mycmname
                     com.unraveldata.cloudera.manager.password=mycmpassword
                     com.unraveldata.cloudera.manager.version=5_7_0
                     com.unraveldata.cloudera.manager.cluster.name.list=cluster1,cluster2,cluster5
                     com.unraveldata.cloudera.manager.service.impala.name=myimpalaservicename

            | 

            .. container::
            confluence-information-macro confluence-information-macro-tip

               Hints:

               .. container:: confluence-information-macro-body

                  To find out the cluster name:

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           {cm_url}:{cm_port}/api/v13/clusters/

                  To find out the service name:

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           {cm_url}:{cm_port}/api/v13/clusters/{cluster_name}/services/

                  Substitute your particular values for
                  bracketed ClouderaManager (CM) properties, i.e.,
                  {cm_port}

            .. rubric:: Using Impalad as the Data Source
               :name: Step3:EnableImpalaAPM-UsingImpaladUsingImpaladastheDataSource

            Use this option if you want to import data from ``impalad``
            daemon. This option requires no CM credentials. Note that if
            the ``impalad`` web interface is secure as indicated in
            https://www.cloudera.com/documentation/enterprise/5-2-x/topics/impala_security_webui.html
            and
            https://www.cloudera.com/documentation/enterprise/5-3-x/topics/impala_webui.html,
            then you need to use `CM as the data
            source <#Step3:EnableImpalaAPM-UsingCM>`__.

            In order to use impalad as the data source you need to
            add \ ``com.unraveldata.data.source=impalad``
            in \ ``/usr/local/unravel/etc/unravel.properties`` on
            Unravel Server. Further, you need to tell Unravel Server
            some information about the ``impalad`` nodes:

            .. container:: table-wrap

               +-----------------------------------+-----------------------------------+
               | Property                          | Description                       |
               +===================================+===================================+
               | ::                                | Set this to ``impalad``.          |
               |                                   |                                   |
               |    com.unraveldata.data.source    |                                   |
               +-----------------------------------+-----------------------------------+
               | ::                                | A comma-separated list of         |
               |                                   | ``impalad`` node IPs and ports    |
               |    com.unraveldata.impalad.nodes  | (format:                          |
               |                                   | ``IP:port,IP:port,IP:port``).     |
               +-----------------------------------+-----------------------------------+

            For example:

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

                     com.unraveldata.data.source=impalad
                     com.unraveldata.impalad.nodes=IP:port,IP:port,IP:port

            .. rubric:: References
               :name: Step3:EnableImpalaAPM-_troubleshootingReferences

            `https://www.cloudera.com/documentation/cdh/5-1-x/Impala/Installing-and-Using-Impala/ciiu_install.html <http://www.cloudera.com/documentation/cdh/5-1-x/Impala/Installing-and-Using-Impala/ciiu_install.html>`__

            https://www.cloudera.com/documentation/enterprise/5-2-x/topics/impala_security_webui.html

            https://www.cloudera.com/documentation/enterprise/5-3-x/topics/impala_webui.html

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Nov 02, 2018 15:15

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__
