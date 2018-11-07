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
            #. `Amazon Elastic MapReduce (Amazon
               EMR) <591528087.html>`__

         .. rubric:: Unravel 4.4 : Step 3: Set up AWS RDS for Unravel DB
            (Optional)
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified on Sep 20, 2018

         .. container:: wiki-content group
            :name: main-content

            .. container:: panel

               .. container:: panelHeader

                  **Table of Contents**

               .. container:: panelContent

                  .. container:: toc-macro rbtoc1541196946004

                     -  `Introduction <#Step3:SetupAWSRDSforUnravelDB(Optional)-Introduction>`__

                        -  `Configuration Requirements for
                           RDS <#Step3:SetupAWSRDSforUnravelDB(Optional)-ConfigurationRequirementsforRDS>`__
                        -  `Setup MySQL RDS in
                           AWS <#Step3:SetupAWSRDSforUnravelDB(Optional)-SetupMySQLRDSinAWS>`__

                           -  `Create Unravel RDS
                              instance <#Step3:SetupAWSRDSforUnravelDB(Optional)-CreateUnravelRDSinstance>`__

                        -  `Connecting unravel node to the unravel RDS
                           instance <#Step3:SetupAWSRDSforUnravelDB(Optional)-ConnectingunravelnodetotheunravelRDSinstance>`__
                        -  `Create unravel db schema in RDS unravel
                           database <#Step3:SetupAWSRDSforUnravelDB(Optional)-CreateunraveldbschemainRDSunraveldatabase>`__
                        -  `Start unravel
                           daemon <#Step3:SetupAWSRDSforUnravelDB(Optional)-Startunraveldaemon>`__

            .. rubric:: Introduction
               :name: Step3:SetupAWSRDSforUnravelDB(Optional)-Introduction
               :class: western

            Unravel's default installation uses a bundled database for
            part of it's storage. For performance and ease of
            management, using an RDS (MySQL) instead of Unravel's
            bundled DB is recommended. 

            .. rubric:: Configuration Requirements for RDS
               :name: Step3:SetupAWSRDSforUnravelDB(Optional)-ConfigurationRequirementsforRDS

            -  RDS Security Group: create on VPC of Unravel Server and
               allow access from Unravel Server security group.
            -  Create `new DB subnet
               group <#Step3:SetupAWSRDSforUnravelDB(Optional)-dbSubnetGroup>`__
               for this RDS.
            -  Create  `new DB parameter
               group <#Step3:SetupAWSRDSforUnravelDB(Optional)-dbParameterGroup>`__
            -   that has custom MySQL properties.

            .. rubric:: Setup MySQL RDS in AWS
               :name: Step3:SetupAWSRDSforUnravelDB(Optional)-SetupMySQLRDSinAWS

            .. rubric:: Create Unravel RDS instance
               :name: Step3:SetupAWSRDSforUnravelDB(Optional)-CreateUnravelRDSinstance

            #. In AWS portal → RDS, click **Create database**.
            #. Choose **MySQL** and click **Next**.\ **
               **
            #. Choose **Production - MySQL** for production purposes or
               Dev/Test for testing purposes.
            #. Change the following properties, leave all others as the
               default.

               .. container::

                  -  **License model**: generic-public-license
                  -  **DB engine version**: 5.5.46
                  -  **DB instance class**: db.r3.xlarge — 4 vCPU, 30.5
                     GiB RAM
                  -  **Multi-AZ deployment**: Create replica in
                     different zone
                  -  **Storage type**: Provisioned IOPS (SSD)
                  -  **Allocated storage**: 500GB (or more depending on
                     number of jobs and clusters the unravel node will
                     monitor)
                  -  **Provisioned IOPS**: 1000

               | 

            #. Create a new DB instance and Master user and password.
               Click **Next** to proceed.

               .. container::

                  -  **DB Instance identifier**: unravelmysqlprod
                  -  **Master username**: unravel
                  -  **Master password**:Change_Password

               | 

            #. In the **Advanced Settings** page change the following
               settings. You can leave all other settings as
               defaulted\ **.**

               .. container::

                  **Network & Security Settings**

                  -  **Virtual Private Cloud**: choose the VPC that
                     contains minimally two subsets and on the same
                     region that you plan to deploy unravel and EMR
                     cluster
                  -  **Subnet group**: create a db subnet group
                     "unravel" by using two subnets in the same VPC
                  -  **Public accessibility**: No
                  -  **Availability zone**: No Preference
                  -  **VPC security group**: Create new VPC security
                     group

               .. container::
               confluence-information-macro confluence-information-macro-tip

                  .. container:: confluence-information-macro-body

                     Create a new DB subnet group in advance. It is
                     required for Multi-AZ deployment. The VPC should at
                     least contains two subnets in at least two
                     Availability zones in a given region. For further
                     information please check `AWS
                     documentation. <https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html>`__

                     .. container:: expand-container
                        :name: expander-1891695790

                        .. container:: expand-control
                           :name: expander-control-1891695790

                           Screenshot for DB subnet group.

                        .. container:: expand-content
                           :name: expander-content-1891695790

               | 

               .. container::
               confluence-information-macro confluence-information-macro-tip

                  .. container:: confluence-information-macro-body

                     Create a new DB parameter group in advance, and
                     this group is based on mysql 5.5. Alter the
                     parameters base on the custom db parameters.

                     .. container:: expand-container
                        :name: expander-870479093

                        .. container:: expand-control
                           :name: expander-control-870479093

                           Screenshot for DB parameter group.

                        .. container:: expand-content
                           :name: expander-content-870479093

                     | 

                     .. container:: expand-container
                        :name: expander-2134281034

                        .. container:: expand-control
                           :name: expander-control-2134281034

                           Custom db parameters

                        .. container:: expand-content
                           :name: expander-content-2134281034

                           -  ``key_buffer_size = 268435456``
                           -  ``max_allowed_packet = 33554432``
                           -  ``table_open_cache = 256``
                           -  ``read_buffer_size = 262144``
                           -  ``read_rnd_buffer_size = 4194304``
                           -  ``max_connect_errors=2000000000``
                           -  ``net-read-timeout = 300``
                           -  ``net-write-timeout = 600``
                           -  ``open_files_limit=9000``
                           -  ``innodb_open_files=9000``
                           -  ``character_set_server=utf8``
                           -  ``collation_server = utf8_unicode_ci``
                           -  ``innodb_autoextend_increment=100``
                           -  ``innodb_additional_mem_pool_size = 20971520``
                           -  ``innodb_log_file_size = 134217728``
                           -  ``innodb_log_buffer_size = 33554432``
                           -  ``innodb_flush_log_at_trx_commit = 2``
                           -  ``innodb_lock_wait_timeout = 50``

               .. container::

                  **Database Options Settings**

                  -  **Database name**: unravel_mysql_prod
                  -  **Port**: 3306
                  -  **DB parameter group**: unravel

               For other RDS options such as **Encryption**, **Backup**,
               **Monitoring**, and **Maintenance** you can use default
               settings or choose the ones suitable for your
               requirements. At the bottom page click **Create
               database**. You should see the following message.

               | 

            .. rubric:: Connecting unravel node to the unravel RDS
               instance
               :name: Step3:SetupAWSRDSforUnravelDB(Optional)-ConnectingunravelnodetotheunravelRDSinstance

            By default, the security group created for the unravel RDS
            has no network access granted on port 3306 on the subnet
            connected. You must modify the security group applied on
            Unravel RDS.

            #. Locate the MySQL database endpoint in the RDS dashboard. 
            #. Look for the security group used for unravel RDS instance
               from RDS dashboard.
            #. Edit the inbound rule of the security group. Add new rule
               allow either

               .. container::

                  -  from unravel node's Security Group, or
                  -  subnet IP block which unravel node located.

                  | The SG or IP block works provided the RDS instance
                    is located on the same region as the VPC.
                  | 

            #. Verify the myql connection from unravel node.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # /usr/local/unravel/mysql/bin/mysql -h unravelmysqlprod.csfw1hkmlpgh.us-east-1.rds.amazonaws.com -u unravel -p

               .. container:: expand-container
                  :name: expander-778289300

                  .. container:: expand-control
                     :name: expander-control-778289300

                     Click here to see a sample screenshot.

                  .. container:: expand-content
                     :name: expander-content-778289300

            .. rubric:: Create unravel db schema in RDS unravel database
               :name: Step3:SetupAWSRDSforUnravelDB(Optional)-CreateunraveldbschemainRDSunraveldatabase

            #. Stop the Unravel server.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo /etc/init.d/unravel_all.sh stop

            #. Configure the following properties
               in \ ``unravel.properties`` so that Unravel server knows
               about the database.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # vi /usr/local/unravel/etc/unravel.properties

               Locate and modify the properties below so that they
               reflect your particular values. If the property isn't
               found, add it. Use the actual values you set in the above
               steps,
               `here <#Step3:SetupAWSRDSforUnravelDB(Optional)-dbEndpoint>`__
               and
               `here <#Step3:SetupAWSRDSforUnravelDB(Optional)-UserPass>`__.
               You can use a hostname; but to avoid DNS lookups use an
               IP address. The database password can be
               `encrypted <Encrypting-Passwords-in-Unravel-Properties-and-Settings_541360893.html>`__. 

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        unravel.jdbc.username=unravel
                        unravel.jdbc.password={unraveldata}
                        unravel.jdbc.url=jdbc:mysql://{unravelmysqlprod.csfw1hkmlpgh.us-east-1.rds.amazonaws.com:3306/unravel_mysql_prod}

            #. Ensure the schema is up to date using the schema upgrade
               utility provided by Unravel server. The script step
               connects to the database and applies schema deltas
               in-order until the schema is up to date. The success or
               failure of the update is noted.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo /usr/local/unravel/dbin/db_schema_upgrade.sh

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     If table creation privilege is not granted because
                     an internal DBA support group provides the external
                     database, request that they apply the schemas
                     in \ ``/usr/local/unravel/sql/mysql/`` in numerical
                     order. The schema deltas assume the database name
                     is already picked with a 'use' statement. The
                     schema_migrations table keeps track of what schemas
                     have been applied.

            #. Create the default user ``admin``\ with the SQL statement
               emitted by

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # /usr/local/unravel/install_bin/db_initial_inserts.sh | /usr/local/unravel/install_bin/db_access.sh

            .. rubric:: Start unravel daemon
               :name: Step3:SetupAWSRDSforUnravelDB(Optional)-Startunraveldaemon

            #. Disable the bundled db on the Unravel server. Only one of
               these commands is needed, depending on your exact version
               of 4.3.x Unravel. The unnecessary command produces an
               error that can be ignored.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo chkconfig unravel_db off
                        # sudo chkconfig unravel_pg off

            #.  Start the Unravel server.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo /etc/init.d/unravel_all.sh start

            | 

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image0|
               `image2018-9-11_14-25-13.png <attachments/591233047/591233050.png>`__
               (image/png)
               |image1|
               `image2018-9-11_14-17-18.png <attachments/591233047/591233053.png>`__
               (image/png)
               |image2|
               `image11.JPG <attachments/591233047/591233056.jpg>`__
               (image/jpeg)
               |image3|
               `image2018-9-12_18-49-0.png <attachments/591233047/591233059.png>`__
               (image/png)
               |image4|
               `image2018-9-12_18-48-37.png <attachments/591233047/591233062.png>`__
               (image/png)
               |image5|
               `image2018-9-12_18-47-32.png <attachments/591233047/591233065.png>`__
               (image/png)
               |image6|
               `image2018-9-11_17-57-29.png <attachments/591233047/591233068.png>`__
               (image/png)
               |image7|
               `image2018-9-11_16-47-2.png <attachments/591233047/591233071.png>`__
               (image/png)
               |image8|
               `image2018-9-11_15-32-42.png <attachments/591233047/591233074.png>`__
               (image/png)
               |image9|
               `image2018-9-11_15-7-20.png <attachments/591233047/591233077.png>`__
               (image/png)
               |image10|
               `image2018-9-11_14-57-15.png <attachments/591233047/591233080.png>`__
               (image/png)
               |image11|
               `image2018-9-11_14-49-23.png <attachments/591233047/591233083.png>`__
               (image/png)
               |image12|
               `image2018-9-11_14-44-28.png <attachments/591233047/591233086.png>`__
               (image/png)
               |image13|
               `image2018-9-11_14-3-44.png <attachments/591233047/591233089.png>`__
               (image/png)
               |image14|
               `image2018-9-11_14-1-12.png <attachments/591233047/591233092.png>`__
               (image/png)
               |image15|
               `image2018-9-11_14-0-3.png <attachments/591233047/591233095.png>`__
               (image/png)
               |image16|
               `image2017-2-26_0-20-12.png <attachments/591233047/591233098.png>`__
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
