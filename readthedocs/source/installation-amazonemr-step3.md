  - title  
    Step 3: Set up AWS RDS for Unravel DB (Optional)

# Step 3: Set up AWS RDS for Unravel DB (Optional)

<div id="page" class="container">

<div id="main" class="container aui-page-panel">

<div id="main-header" class="container">

<div id="breadcrumb-section" class="container">

1.  [Unravel 4.4](index.html)
2.  [Unravel 4.4](Unravel-4.4_541197025.html)
3.  [Installation Guides](Installation-Guides_541393730.html)
4.  [Amazon Elastic MapReduce (Amazon EMR)](591528087.html)

</div>

**Unravel 4.4 : Step 3: Set up AWS RDS for Unravel DB (Optional)**

</div>

<div id="content" class="container view">

<div class="container page-metadata">

Created by katch, last modified on Sep 20, 2018

</div>

<div id="main-content" class="container wiki-content group">

<div class="container panel">

<div class="container panelHeader">

**Table of
    Contents**

</div>

<div class="container panelContent">

<div class="container toc-macro rbtoc1541196946004">

  - [Introduction](#Step3:SetupAWSRDSforUnravelDB\(Optional\)-Introduction)
      - [Configuration Requirements for
        RDS](#Step3:SetupAWSRDSforUnravelDB\(Optional\)-ConfigurationRequirementsforRDS)
      - [Setup MySQL RDS in
        AWS](#Step3:SetupAWSRDSforUnravelDB\(Optional\)-SetupMySQLRDSinAWS)
          - [Create Unravel RDS
            instance](#Step3:SetupAWSRDSforUnravelDB\(Optional\)-CreateUnravelRDSinstance)
      - [Connecting unravel node to the unravel RDS
        instance](#Step3:SetupAWSRDSforUnravelDB\(Optional\)-ConnectingunravelnodetotheunravelRDSinstance)
      - [Create unravel db schema in RDS unravel
        database](#Step3:SetupAWSRDSforUnravelDB\(Optional\)-CreateunraveldbschemainRDSunraveldatabase)
      - [Start unravel
        daemon](#Step3:SetupAWSRDSforUnravelDB\(Optional\)-Startunraveldaemon)

</div>

</div>

</div>

**Introduction**

Unravel's default installation uses a bundled database for part of it's
storage. For performance and ease of management, using an RDS (MySQL)
instead of Unravel's bundled DB is recommended. 

**Configuration Requirements for RDS**

  - RDS Security Group: create on VPC of Unravel Server and allow access
    from Unravel Server security group.
  - Create [new DB subnet
    group](#Step3:SetupAWSRDSforUnravelDB\(Optional\)-dbSubnetGroup) for
    this RDS.
  - Create  [new DB parameter
    group](#Step3:SetupAWSRDSforUnravelDB\(Optional\)-dbParameterGroup)
  -  that has custom MySQL properties.

**Setup MySQL RDS in AWS**

**Create Unravel RDS instance**

1.  In AWS portal → RDS, click **Create database**.

2.  Choose **MySQL** and click **Next**.\*\* \*\*

3.  Choose **Production - MySQL** for production purposes or Dev/Test
    for testing purposes.

4.  Change the following properties, leave all others as the default.
    
    <div class="container">
    
      - **License model**: generic-public-license
      - **DB engine version**: 5.5.46
      - **DB instance class**: db.r3.xlarge — 4 vCPU, 30.5 GiB RAM
      - **Multi-AZ deployment**: Create replica in different zone
      - **Storage type**: Provisioned IOPS (SSD)
      - **Allocated storage**: 500GB (or more depending on number of
        jobs and clusters the unravel node will monitor)
      - **Provisioned IOPS**: 1000
    
    </div>

5.  Create a new DB instance and Master user and password. Click
    **Next** to proceed.
    
    <div class="container">
    
      - **DB Instance identifier**: unravelmysqlprod
      - **Master username**: unravel
      - **Master password**:Change\_Password
    
    </div>

6.  In the **Advanced Settings** page change the following settings. You
    can leave all other settings as defaulted**.**
    
    <div class="container">
    
    **Network & Security Settings**
    
      - **Virtual Private Cloud**: choose the VPC that contains
        minimally two subsets and on the same region that you plan to
        deploy unravel and EMR cluster
      - **Subnet group**: create a db subnet group "unravel" by using
        two subnets in the same VPC
      - **Public accessibility**: No
      - **Availability zone**: No Preference
      - **VPC security group**: Create new VPC security group
    
    </div>
    
    <div class="container">
    
    </div>
    
    confluence-information-macro confluence-information-macro-tip
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > Create a new DB subnet group in advance. It is required for
    > Multi-AZ deployment. The VPC should at least contains two subnets
    > in at least two Availability zones in a given region. For further
    > information please check [AWS
    > documentation.](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html)
    > 
    > <div id="expander-1891695790" class="container expand-container">
    > 
    > <div id="expander-control-1891695790" class="container expand-control">
    > 
    > Screenshot for DB subnet
    > group.
    > 
    > </div>
    > 
    > <div id="expander-content-1891695790" class="container expand-content">
    > 
    > </div>
    > 
    > </div>
    > 
    > </div>
    
    <div class="container">
    
    </div>
    
    confluence-information-macro confluence-information-macro-tip
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > Create a new DB parameter group in advance, and this group is
    > based on mysql 5.5. Alter the parameters base on the custom db
    > parameters.
    > 
    > <div id="expander-870479093" class="container expand-container">
    > 
    > <div id="expander-control-870479093" class="container expand-control">
    > 
    > Screenshot for DB parameter
    > group.
    > 
    > </div>
    > 
    > <div id="expander-content-870479093" class="container expand-content">
    > 
    > </div>
    > 
    > </div>
    > 
    > <div id="expander-2134281034" class="container expand-container">
    > 
    > <div id="expander-control-2134281034" class="container expand-control">
    > 
    > Custom db
    > parameters
    > 
    > </div>
    > 
    > <div id="expander-content-2134281034" class="container expand-content">
    > 
    >   - `key_buffer_size = 268435456`
    >   - `max_allowed_packet = 33554432`
    >   - `table_open_cache = 256`
    >   - `read_buffer_size = 262144`
    >   - `read_rnd_buffer_size = 4194304`
    >   - `max_connect_errors=2000000000`
    >   - `net-read-timeout = 300`
    >   - `net-write-timeout = 600`
    >   - `open_files_limit=9000`
    >   - `innodb_open_files=9000`
    >   - `character_set_server=utf8`
    >   - `collation_server = utf8_unicode_ci`
    >   - `innodb_autoextend_increment=100`
    >   - `innodb_additional_mem_pool_size = 20971520`
    >   - `innodb_log_file_size = 134217728`
    >   - `innodb_log_buffer_size = 33554432`
    >   - `innodb_flush_log_at_trx_commit = 2`
    >   - `innodb_lock_wait_timeout = 50`
    > 
    > </div>
    > 
    > </div>
    > 
    > </div>
    
    <div class="container">
    
    **Database Options Settings**
    
      - **Database name**: unravel\_mysql\_prod
      - **Port**: 3306
      - **DB parameter group**: unravel
    
    </div>
    
    For other RDS options such as **Encryption**, **Backup**,
    **Monitoring**, and **Maintenance** you can use default settings or
    choose the ones suitable for your requirements. At the bottom page
    click **Create database**. You should see the following message.

**Connecting unravel node to the unravel RDS instance**

By default, the security group created for the unravel RDS has no
network access granted on port 3306 on the subnet connected. You must
modify the security group applied on Unravel RDS.

1.  Locate the MySQL database endpoint in the RDS dashboard. 

2.  Look for the security group used for unravel RDS instance from RDS
    dashboard.

3.  Edit the inbound rule of the security group. Add new rule allow
    either
    
    <div class="container">
    
      - from unravel node's Security Group, or
      - subnet IP block which unravel node located.
    
    The SG or IP block works provided the RDS instance is located on the
    same region as the VPC.  
    
    </div>

4.  Verify the myql connection from unravel
    node.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # /usr/local/unravel/mysql/bin/mysql -h unravelmysqlprod.csfw1hkmlpgh.us-east-1.rds.amazonaws.com -u unravel -p
    ```
    
    </div>
    
    </div>
    
    <div id="expander-778289300" class="container expand-container">
    
    <div id="expander-control-778289300" class="container expand-control">
    
    Click here to see a sample
    screenshot.
    
    </div>
    
    <div id="expander-content-778289300" class="container expand-content">
    
    </div>
    
    </div>

**Create unravel db schema in RDS unravel database**

1.  Stop the Unravel server.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo /etc/init.d/unravel_all.sh stop
    ```
    
    </div>
    
    </div>

2.  Configure the following properties in `unravel.properties` so that
    Unravel server knows about the database.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # vi /usr/local/unravel/etc/unravel.properties
    ```
    
    </div>
    
    </div>
    
    Locate and modify the properties below so that they reflect your
    particular values. If the property isn't found, add it. Use the
    actual values you set in the above steps,
    [here](#Step3:SetupAWSRDSforUnravelDB\(Optional\)-dbEndpoint) and
    [here](#Step3:SetupAWSRDSforUnravelDB\(Optional\)-UserPass). You can
    use a hostname; but to avoid DNS lookups use an IP address. The
    database password can be
    [encrypted](Encrypting-Passwords-in-Unravel-Properties-and-Settings_541360893.html). 
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    unravel.jdbc.username=unravel
    unravel.jdbc.password={unraveldata}
    unravel.jdbc.url=jdbc:mysql://{unravelmysqlprod.csfw1hkmlpgh.us-east-1.rds.amazonaws.com:3306/unravel_mysql_prod}
    ```
    
    </div>
    
    </div>

3.  Ensure the schema is up to date using the schema upgrade utility
    provided by Unravel server. The script step connects to the database
    and applies schema deltas in-order until the schema is up to date.
    The success or failure of the update is noted.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo /usr/local/unravel/dbin/db_schema_upgrade.sh
    ```
    
    </div>
    
    </div>
    
    <div class="container">
    
    </div>
    
    confluence-information-macro
    confluence-information-macro-information
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > If table creation privilege is not granted because an internal DBA
    > support group provides the external database, request that they
    > apply the schemas in `/usr/local/unravel/sql/mysql/` in numerical
    > order. The schema deltas assume the database name is already
    > picked with a 'use' statement. The schema\_migrations table keeps
    > track of what schemas have been applied.
    > 
    > </div>

4.  Create the default user `admin`with the SQL statement emitted
    by
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # /usr/local/unravel/install_bin/db_initial_inserts.sh | /usr/local/unravel/install_bin/db_access.sh
    ```
    
    </div>
    
    </div>

**Start unravel daemon**

1.  Disable the bundled db on the Unravel server. Only one of these
    commands is needed, depending on your exact version of 4.3.x
    Unravel. The unnecessary command produces an error that can be
    ignored.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo chkconfig unravel_db off
    # sudo chkconfig unravel_pg off
    ```
    
    </div>
    
    </div>

2.   Start the Unravel server.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo /etc/init.d/unravel_all.sh start
    ```
    
    </div>
    
    </div>

</div>

<div class="container pageSection group">

<div class="container pageSectionHeader">

**Attachments:**

</div>

<div class="container greybox">

![image0](images/icons/bullet_blue.gif)
[image2018-9-11\_14-25-13.png](attachments/591233047/591233050.png)
(image/png) ![image1](images/icons/bullet_blue.gif)
[image2018-9-11\_14-17-18.png](attachments/591233047/591233053.png)
(image/png) ![image2](images/icons/bullet_blue.gif)
[image11.JPG](attachments/591233047/591233056.jpg) (image/jpeg)
![image3](images/icons/bullet_blue.gif)
[image2018-9-12\_18-49-0.png](attachments/591233047/591233059.png)
(image/png) ![image4](images/icons/bullet_blue.gif)
[image2018-9-12\_18-48-37.png](attachments/591233047/591233062.png)
(image/png) ![image5](images/icons/bullet_blue.gif)
[image2018-9-12\_18-47-32.png](attachments/591233047/591233065.png)
(image/png) ![image6](images/icons/bullet_blue.gif)
[image2018-9-11\_17-57-29.png](attachments/591233047/591233068.png)
(image/png) ![image7](images/icons/bullet_blue.gif)
[image2018-9-11\_16-47-2.png](attachments/591233047/591233071.png)
(image/png) ![image8](images/icons/bullet_blue.gif)
[image2018-9-11\_15-32-42.png](attachments/591233047/591233074.png)
(image/png) ![image9](images/icons/bullet_blue.gif)
[image2018-9-11\_15-7-20.png](attachments/591233047/591233077.png)
(image/png) ![image10](images/icons/bullet_blue.gif)
[image2018-9-11\_14-57-15.png](attachments/591233047/591233080.png)
(image/png) ![image11](images/icons/bullet_blue.gif)
[image2018-9-11\_14-49-23.png](attachments/591233047/591233083.png)
(image/png) ![image12](images/icons/bullet_blue.gif)
[image2018-9-11\_14-44-28.png](attachments/591233047/591233086.png)
(image/png) ![image13](images/icons/bullet_blue.gif)
[image2018-9-11\_14-3-44.png](attachments/591233047/591233089.png)
(image/png) ![image14](images/icons/bullet_blue.gif)
[image2018-9-11\_14-1-12.png](attachments/591233047/591233092.png)
(image/png) ![image15](images/icons/bullet_blue.gif)
[image2018-9-11\_14-0-3.png](attachments/591233047/591233095.png)
(image/png) ![image16](images/icons/bullet_blue.gif)
[image2017-2-26\_0-20-12.png](attachments/591233047/591233098.png)
(image/png)

</div>

</div>

</div>

</div>

<div id="footer" class="container">

<div class="container section footer-body">

Document generated by Confluence on Nov 02, 2018 15:15

<div id="footer-logo" class="container">

[Atlassian](http://www.atlassian.com/)

</div>

</div>

</div>

</div>
