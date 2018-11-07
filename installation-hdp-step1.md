  - title  
    Step 1: Install Unravel Server on HDP

# Step 1: Install Unravel Server on HDP

<div id="page" class="container">

<div id="main" class="container aui-page-panel">

<div id="main-header" class="container">

<div id="breadcrumb-section" class="container">

1.  [Unravel 4.4](index.html)
2.  [Unravel 4.4](Unravel-4.4_541197025.html)
3.  [Installation Guides](Installation-Guides_541393730.html)
4.  [Hortonworks Data Platform (HDP)](541197289.html)

</div>

**Unravel 4.4 : Step 1: Install Unravel Server on HDP**

</div>

<div id="content" class="container view">

<div class="container page-metadata">

Created by katch, last modified on Sep 25, 2018

</div>

<div id="main-content" class="container wiki-content group">

<div class="container panel">

<div class="container panelHeader">

**Table of Contents**

</div>

<div class="container panelContent">

<div class="container toc-macro rbtoc1541196938266">

  - [Introduction](#Step1:InstallUnravelServeronHDP-Introduction)
  - [Pre-Installation
    Check](#Step1:InstallUnravelServeronHDP-Pre-InstallationCheck)
      - [Platform
        Compatibility](#Step1:InstallUnravelServeronHDP-PlatformCompatibility)
      - [Hardware](#Step1:InstallUnravelServeronHDP-Hardware)
      - [Software](#Step1:InstallUnravelServeronHDP-Software)
      - [Access
        Permissions](#Step1:InstallUnravelServeronHDP-AccessPermissions)
      - [Network](#Step1:InstallUnravelServeronHDP-Network)
  - [Installation](#Step1:InstallUnravelServeronHDP-Installation)
      - [1. Configure the Host
        Machine](#Step1:InstallUnravelServeronHDP-1.ConfiguretheHostMachine)
          - [Allocate a Cluster Gateway/Edge/Client Host with HDFS
            Access](#Step1:InstallUnravelServeronHDP-AllocateaClusterGateway/Edge/ClientHostwithHDFSAccess)
      - [2. Install the Unravel Server RPM on the Host
        Machine](#Step1:InstallUnravelServeronHDP-2.InstalltheUnravelServerRPMontheHostMachine)
          - [Get the Unravel Server
            RPM](#Step1:InstallUnravelServeronHDP-GettheUnravelServerRPM)
          - [Make symlinks if
            required](#Step1:InstallUnravelServeronHDP-Makesymlinksifrequired)
          - [Install the Unravel Server
            RPM](#Step1:InstallUnravelServeronHDP-InstalltheUnravelServerRPM)
          - [Do Host-Specific Post-Installation
            Actions](#Step1:InstallUnravelServeronHDP-DoHost-SpecificPost-InstallationActions)
      - [3. Configure Unravel Server (Basic/Core
        Options)](#Step1:InstallUnravelServeronHDP-3.ConfigureUnravelServer\(Basic/CoreOptions\))
          - [Enable Optional
            Daemons](#Step1:InstallUnravelServeronHDP-EnableOptionalDaemons)
          - [Modify
            unravel.properties](#Step1:InstallUnravelServeronHDP-Modifyunravel.properties)
          - [If Kerberos is
            Enabled:](#Step1:InstallUnravelServeronHDP-IfKerberosisEnabled:)
          - [If Ranger is
            Enabled:](#Step1:InstallUnravelServeronHDP-IfRangerisEnabled:)
          - [Disable Impala
            Sensor](#Step1:InstallUnravelServeronHDP-DisableImpalaSensor)
      - [4. Convert Your Unravel Installation to
        HDP](#Step1:InstallUnravelServeronHDP-4.ConvertYourUnravelInstallationtoHDP)
          - [Run the following commands on Unravel
            Server:](#Step1:InstallUnravelServeronHDP-RunthefollowingcommandsonUnravelServer:)
          - [Switch User](#Step1:InstallUnravelServeronHDP-SwitchUser)
          - [Restart Unravel
            Server](#Step1:InstallUnravelServeronHDP-RestartUnravelServer)
      - [5. Log into Unravel Web
        UI](#Step1:InstallUnravelServeronHDP-5.LogintoUnravelWebUI)
      - [6. Configure Unravel Server (Advanced
        Options)](#Step1:InstallUnravelServeronHDP-6.ConfigureUnravelServer\(AdvancedOptions\))
          - [Enable Additional Data
            Collection/Instrumentation](#Step1:InstallUnravelServeronHDP-EnableAdditionalDataCollection/Instrumentation)

</div>

</div>

</div>

**Introduction**

This topic explains how to deploy Unravel Server 4.0 on HDP. These
instructions apply to Unravel Server 4.0. For older versions of Unravel
Server, contact Unravel Support.

<div class="container panel">

<div class="container panelHeader">

**Workflow Summary**

</div>

<div class="container panelContent">

1.  Pre-installation check
2.  Configure the host machine.
3.  Install the Unravel Server RPM on the host machine.
4.  Configure Unravel Server (basic/core options).
5.  Log into Unravel Web UI.
6.  (Optional) Configure Unravel Server (advanced options).

</div>

</div>

**Pre-Installation Check**

The following installation requirements must be met for successful
installation of Unravel.

**Platform Compatibility**

  - HDP 2.2-2.6
  - Hadoop 1.x - 2.x
  - Kafka 0.9-1.0 (Apache ver. equiv.)
  - Kerberos
  - Hive 0.9.x - 1.2.x
  - Spark 1.3.x - 2.2.x

**Hardware**

  - Architecture: x86\_64
  - Cores: 8
  - RAM: 64GB minimum, 128GB for medium volume (\>25,000 jobs per day),
    256GB for high volume (\>50,000 jobs per day)
  - Disk: `/usr/local/unravel` with 8GB free minimum (can be symlink)
  - Disk: `/srv/unravel` with 500GB free minimum (can be symlink)
  - For 60,000+ MR jobs per day, two or more gateway/edge nodes are
    recommended ("multi-host Unravel")

**Software**

  - Operating System: RedHat/Centos 6.4 - 7.4
  - `libaio.x86_64` installed
  - `SELINUX=permissive` (or disabled) should be set in
    `/etc/selinux/config`
  - HDFS+Hive+YARN client/gateway, Hadoop and Hive commands in `PATH`
  - If Spark is in use, Spark client gateway
  - Verify that all clients are installed by checking that RPMs are
    installed; Unravel utilizes clients and associated libraries
  - You need to register edge node to Ambari 
  - LDAP (AD or Open LDAP) compatible for Unravel Web UI user
    authentication (Open signup by default)
  - On Unravel Edge-node server, please **do not** have zookeeper
    installed in same server

**Access Permissions**

  - If Kerberos is in use, a keytab for principal hdfs (or read-only
    equivalent) is required for access to:
      - Access to YARN’s “done dir” in HDFS
      - Access to YARN’s log aggregation directory in HDFS
      - Access to Spark event log directory in HDFS
      - Access to file sizes under HIve warehouse directory
  - Access to YARN Resource Manager REST API
      - principal needs right to find out which RM is active
  - JDBC access to the Hive Metastore (read-only user is sufficient)
  - Application Timeline Server (ATS) read-only

**Network**

  - Port 3000 from users and entire cluster to Unravel Web UI
  - HDFS ports open from Hadoop cluster to Unravel Server(s)
  - For YARN, Hive Metadata DB port open to Unravel Server(s) for
    partition reporting
  - UDP and TCP port 4043 open from entire cluster to Unravel Server(s)
  - For Oozie, port 11000 open from Unravel Server(s) to the Oozie
    server
  - Resource Manager (RM) port 8032 from Unravel Server(s) to the RM
    server(s)
  - ATS port 8188 from Unravel Servers(s) to ATS server(s)
  - Port 4020, 4176, 4181 through 4189, 3316, 4091 must be available for
    localhost communication between Unravel daemons or Unravel servers
    (if multi-host Unravel installation)

**Installation**

**1. Configure the Host Machine**

**Allocate a Cluster Gateway/Edge/Client Host with HDFS Access**

For HDP, use Ambari Web UI to create a Gateway node configuration.

**2. Install the Unravel Server RPM on the Host Machine**

<div class="container">

**Get the Unravel Server RPM**

See [Download Unravel
Software](https://unraveldata.atlassian.net/wiki/spaces/UNDOCS/pages/226132074/Download+Unravel+Software+Versions).

**Make symlinks if required**

If you want the two disk areas used by Unravel to be on different
volumes, you can make symlinks to specific areas before installing (or
do a `mv` and `symlink` symlink after installing). Do it before the
first install if there is insufficient space on the target
paths `/usr/local/unravel` and `/srv/unravel` noted above. 

**Install the Unravel Server RPM**

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
# sudo rpm -U unravel-4.*.x86_64.rpm*
# /usr/local/unravel/install_bin/await_fixups.sh
```

</div>

</div>

The precise filename can vary, depending on how it was fetched or
copied. The `rpm` command does not require .`rpm` suffix. The
flag `-U` works for either initial install or upgrade.

Run the specified `await_fixups.sh` script to make sure background
processing is finished before you do other steps. In a routine upgrade,
it is okay to start all Unravel daemons, but do not stop or restart them
until the `await_fixups.sh` prints `Done` (it takes a few minutes).

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> 
> 
> <div class="container confluence-information-macro-body">
> 
> The installation creates `/usr/local/unravel/` which contains the
> executables, scripts, and settings. User `unravel` is created. The
> initial internal database and other durable state are put
> in `/srv/unravel/` for larger storage. By default, the installation
> supports YARN.  
> The master configuration file is
> in `/usr/local/unravel/etc/unravel.properties` and the logs are in
> `/usr/local/unravel/logs/`. The RPM installation creates
> user `unravel` if it does not already exist; `/etc/init.d/unravel_*`
> scripts for controlling its services as well
> as `/etc/init.d/unravel_all.sh` which can be used to manually stop,
> start, and get status of all daemons in proper order.  
> The RPM installation also creates an HDFS directory for Hive Hook
> information collection.  
> During initial install, a bundled database is used. This can be
> switched to use an externally managed MySQL for production. (The
> bundled database root mysql password will be stored
> in `/root/unravel.install.include` during installation.)
> 
> </div>

**Do Host-Specific Post-Installation Actions**

For HDP, there are no host-specific post-installation actions.

</div>

**3. Configure Unravel Server (Basic/Core Options)**

<div class="container">

**Enable Optional Daemons**

Depending on your workload volume or kind of activity, you can enable
optional daemons at this point. See [Creating Multiple Workers for High
Volume
Data](Creating-Multiple-Workers-for-High-Volume-Data_541131395.html).

**Modify `unravel.properties`**

  - Open `/usr/local/unravel/etc/unravel.properties` with `vi`.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo vi /usr/local/unravel/etc/unravel.properties
    ```
    
    </div>
    
    </div>

  - Edit the values in `unravel.properties` using the guidelines and
    descriptions in the table below.
    
    <div class="container table-wrap">
    
    <table>
    <colgroup>
    <col style="width: 33%" />
    <col style="width: 33%" />
    <col style="width: 33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Property</th>
    <th>Description</th>
    <th>Example Values</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><code>com.unraveldata.adv ertised.url</code></td>
    <td>Defines the Unravel Server URL for HTTP traffic.</td>
    <td><code>http://LAN_DNS:3000</code></td>
    </tr>
    <tr class="even">
    <td><code>com.unraveldata.cus tomer.organization</code></td>
    <td>Identifies your installation for reporting purposes.</td>
    <td><code>Company_and_org</code></td>
    </tr>
    <tr class="odd">
    <td><pre><code>com.unraveldata.tm</code></pre>
    pdir</td>
    <td>Location where Unravel's temp file will reside</td>
    <td><pre><code>/srv/unravel/tmp</code></pre></td>
    </tr>
    <tr class="even">
    <td><code>com.unraveldata.his tory.maxSize.weeks</code></td>
    <td>Sets retention for search data.</td>
    <td><code>26</code></td>
    </tr>
    <tr class="odd">
    <td><code>com.unraveldata.job .collector.done.log.b ase</code></td>
    <td>Only modifiable through Unravel Web UI's configuration wizard.</td>
    <td><pre><code>/mr-history/done</code></pre></td>
    </tr>
    <tr class="even">
    <td><code>com.unraveldata.job .collector.log.aggreg ation.base</code></td>
    <td>Only modifiable through Unravel Web UI's configuration wizard.</td>
    <td><pre><code>/app-logs/*/logs/</code></pre></td>
    </tr>
    <tr class="odd">
    <td><code>com.unraveldata.log in.admins</code></td>
    <td>Defines the usernames that can access Unravel Web UI's admin pages. Default is <code>admin</code>.</td>
    <td><code>admin</code></td>
    </tr>
    <tr class="even">
    <td><code>com.unraveldata.s3. batch.monitoring.inte rval.sec</code></td>
    <td><strong>Optional</strong>. Defines the monitoring frequency. Default is 300 seconds (5 minutes). Set this property to 60 for lower latency.</td>
    <td><code>120</code></td>
    </tr>
    <tr class="odd">
    <td><span class="title-ref">`com.unraveldata.spa rk.eventlog.location</span> `</td>
    <td>Where to find Spark event logs</td>
    <td><pre><code>hdfs:///user/spark</code></pre>
    /applicationHistory/, hdfs:///user/spark/sp arkApplicationHistory /,hdfs:///user/spark/ spark2ApplicationHist ory/</td>
    </tr>
    <tr class="even">
    <td><code>yarn.resourcemanage r.webapp.address</code></td>
    <td>YARN resource manager web address URL</td>
    <td><pre><code>http://example.loc</code></pre>
    aldomain:8088</td>
    </tr>
    <tr class="odd">
    <td><code>oozie.server.url</code></td>
    <td>Oozie URL</td>
    <td><pre><code>http://example.loc</code></pre>
    aldomain:11000/oozie</td>
    </tr>
    </tbody>
    </table>
    
    </div>
    
    **If Kerberos is
    Enabled:**
    
    <div id="expander-1842118070" class="container expand-container">
    
    <div id="expander-control-1842118070" class="container expand-control">
    
    Add authentication for
    HDFS...
    
    </div>
    
    <div id="expander-content-1842118070" class="container expand-content">
    
    Create or identify a principal and keytab for Unravel daemons to
    access HDFS and REST when Kerberos is enabled. 
    
    To get going faster, you can use the 'hdfs' principal which often
    has a pre-existing "headless" keytab.
    
    Add properties for Kerberos
    in `/usr/local/unravel/etc/unravel.properties` (substitute correct
    filename and principal):
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    com.unraveldata.kerberos.principal=unravel/myhost.mydomain@MYREALM
    com.unraveldata.kerberos.keytab.path=/usr/local/unravel/etc/unravel.keytab
    ```
    
    </div>
    
    </div>
    
    You can verify the principal in a keytab by using `properties klist
    -kt KEYTAB_FILE`*.* The keytab file should have chmod bits 500 and
    be owned by 'unravel' local user (default) or by the user you want
    to use, as explained in [Run Unravel Daemons with Custom
    User](541131652.html).
    
    </div>
    
    </div>
    
    **If Ranger is
    Enabled:**
    
    <div id="expander-1683274762" class="container expand-container">
    
    <div id="expander-control-1683274762" class="container expand-control">
    
    Add these
    permissions...
    
    </div>
    
    <div id="expander-content-1683274762" class="container expand-content">
    
    For quicker setup, use the hdfs principal. For more narrow
    privileges, define your own alt principal. The alt user can
    be `unravel` (created by `rpm`) or one of your choosing. The
    corresponding kerberos principal does not need to have the same name
    as the local user. The user/principal here should correspond to
    the `X`  in the switch-user section
    below. 
    
    <div class="container table-wrap">
    
    | Resource                                                                       | Pri nci pal | Acce ss        | Purpose                      |
    | ------------------------------------------------------------------------------ | ----------- | -------------- | ---------------------------- |
    | `hdfs://spark-history`                                                         | hdf s       | read +exe cute | Spark event log              |
    | hdfs://spark2-history                                                          | hdf s       | read +exe cute | Spark2 event log             |
    | `hdfs://mr-history/done`                                                       | hdf s       | read +exe cute | MapReduce logs               |
    | `hdfs://app-logs`                                                              | hdf s       | read +exe cute | YARN aggregation folder      |
    | `hdfs://apps/hive/warehou se (default value of hive. metastore.warehouse.dir)` | hdf s       | read +exe cute | Obtain table partition sizes |
    | Hive Metastore database GRANT                                                  | hiv e       | read +exe cute | Hive table information       |
    

    </div>
    
    </div>
    
    </div>
    
    **Disable Impala Sensor**
    
    <div class="container">
    
    </div>
    
    confluence-information-macro
    confluence-information-macro-information
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > Impala is not officially supported on HDP clusters therefore you
    > should disable the  ImpalaSensor by modifying
    > the `/usr/local/unravel/etc/unravel.properties` file on Unravel
    > Server:
    > 
    > </div>
    
    Open `unravel.properties `file.  Locate or
    add `com.unraveldata.sensor.tasks.disabled`  and set it as shown:
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    com.unraveldata.sensor.tasks.disabled=iw
    ```
    
    </div>
    
    </div>

</div>

**4. Convert Your Unravel Installation to HDP**

<div class="container">

****Run the following commands on Unravel Server:****

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
# sudo /etc/init.d/unravel_all.sh stop
# sudo /usr/local/unravel/install_bin/switch_to_hdp.sh
```

</div>

</div>

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> 
> 
> <div class="container confluence-information-macro-body">
> 
> Note: This change will stick after later RPM upgrades; it does not
> need to be done each time.
> 
> </div>

**Switch User**

Depending on your cluster security configuration, you will need to run
the
[switch\_to\_user](Run-Unravel-Daemons-with-Custom-User_541033161.html) script.
Dependencies like kerberos and which target user you used for Ranger
affect this. 

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
# sudo /usr/local/unravel/install_bin/switch_to_user.sh x y 
```

</div>

</div>

where `X`  and `Y` depend on your environment. See
[switch\_to\_use](Run-Unravel-Daemons-with-Custom-User_541033161.html)
page. 

**Restart Unravel Server**

After edits to `com.unraveldata.login.admins` in
`/usr/local/unravel/etc/unravel.properties` it is necessary to run the
following script in order to make changes take effect. The `echo`
command shows the page to visit with your browser. If you are using an
ssh tunnel or http proxy, you might need to make adjustments.
`UNRAVEL_HOST_IP` is a fully qualified DNS or IP Address.

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
# sudo /etc/init.d/unravel_all.sh start
# sleep 60
# echo "http://$(hostname -f):3000/"
```

</div>

</div>

This completes the basic/core configuration.

</div>

**5. Log into Unravel Web UI**

<div class="container">

Using a web browser, navigate to `http://{UNRAVEL_``HOST`\_IP}:3000 and
login as user "`admin"` with password `"unraveldata`".

<div class="container">

</div>

confluence-information-macro confluence-information-macro-note

> 
> 
> <div class="container confluence-information-macro-body">
> 
> For the free trial version, use the Chrome web browser.
> 
> </div>

**Congratulations\!**

Unravel Server is up and running. Unravel Web UI displays collected
data. For instructions on using Unravel Web UI, see the [User
Guide](User-Guide_541295329.html).

</div>

**6. Configure Unravel Server (Advanced Options)**

<div class="container">

**Enable Additional Data Collection/Instrumentation**

Install the Unravel Sensor Parcel on gateway/edge/client nodes that are
used to submit Hive queries to push additional information to Unravel
Server. For details, see [Step 2: Enable Additional Data Collection /
Instrumentation for HDP](561709534.html).

</div>

</div>

<div class="container pageSection group">

<div class="container pageSectionHeader">

**Attachments:**

</div>

<div class="container greybox">

![image0](images/icons/bullet_blue.gif)
[image2017-2-26\_0-20-12.png](attachments/541098908/541393753.png)
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
