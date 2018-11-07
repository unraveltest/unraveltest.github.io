  - title  
    Unravel 4.4 : Step 1: Install Unravel Server on MapR

# Unravel 4.4 : Step 1: Install Unravel Server on MapR

<div id="page" class="container">

<div id="main" class="container aui-page-panel">

<div id="main-header" class="container">

<div id="breadcrumb-section" class="container">

1.  [Unravel 4.4](index.html)
2.  [Unravel 4.4](Unravel-4.4_541197025.html)
3.  [Installation Guides](Installation-Guides_541393730.html)
4.  [MapR](MapR_541197270.html)

</div>

**Unravel 4.4 : Step 1: Install Unravel Server on MapR**

</div>

<div id="content" class="container view">

<div class="container page-metadata">

Created by katch, last modified on Oct 15, 2018

</div>

<div id="main-content" class="container wiki-content group">

<div class="container panel">

<div class="container panelHeader">

**Table of Contents**

</div>

<div class="container panelContent">

<div class="container toc-macro rbtoc1541196957885">

  - [Introduction](#Step1:InstallUnravelServeronMapR-Introduction)
  - [Pre-Installation
    Check](#Step1:InstallUnravelServeronMapR-Pre-InstallationCheck)
      - [Platform
        Compatibility](#Step1:InstallUnravelServeronMapR-PlatformCompatibility)
      - [Hardware](#Step1:InstallUnravelServeronMapR-Hardware)
      - [Software](#Step1:InstallUnravelServeronMapR-Software)
      - [Access
        Permissions](#Step1:InstallUnravelServeronMapR-AccessPermissions)
      - [Network](#Step1:InstallUnravelServeronMapR-Network)
  - [1. Configure the Host
    Machine](#Step1:InstallUnravelServeronMapR-1.ConfiguretheHostMachine)
      - [Allocate a Cluster Gateway/Edge/Client Host with HDFS
        Access](#Step1:InstallUnravelServeronMapR-AllocateaClusterGateway/Edge/ClientHostwithHDFSAccess)
      - [Configure the Host Before installing the
        RPM](#Step1:InstallUnravelServeronMapR-ConfiguretheHostBeforeinstallingtheRPM)
  - [2. Install the Unravel Server RPM on the Host
    Machine](#Step1:InstallUnravelServeronMapR-2.InstalltheUnravelServerRPMontheHostMachine)
      - [Get the Unravel Server
        RPM](#Step1:InstallUnravelServeronMapR-GettheUnravelServerRPM)
      - [Make symlinks if
        required](#Step1:InstallUnravelServeronMapR-Makesymlinksifrequired)
      - [Install the Unravel Server
        RPM](#Step1:InstallUnravelServeronMapR-InstalltheUnravelServerRPM)
      - [Do Host-Specific Post-Installation
        Actions](#Step1:InstallUnravelServeronMapR-DoHost-SpecificPost-InstallationActions)
  - [3. Configure Unravel Server (Basic/Core
    Options)](#Step1:InstallUnravelServeronMapR-3.ConfigureUnravelServer\(Basic/CoreOptions\))
      - [Enable Optional
        Daemons](#Step1:InstallUnravelServeronMapR-EnableOptionalDaemons)
      - [Modify
        unravel.properties](#Step1:InstallUnravelServeronMapR-Modifyunravel.properties)
      - [If Kerberos is
        Enabled:](#Step1:InstallUnravelServeronMapR-IfKerberosisEnabled:)
      - [If Sentry is
        Enabled:](#Step1:InstallUnravelServeronMapR-IfSentryisEnabled:)
      - [Switch User](#Step1:InstallUnravelServeronMapR-SwitchUser)
      - [Do Host-Specific Configuration
        Steps](#Step1:InstallUnravelServeronMapR-DoHost-SpecificConfigurationSteps)
      - [Restart Unravel
        Server](#Step1:InstallUnravelServeronMapR-RestartUnravelServer)
  - [4. Log into Unravel Web
    UI](#Step1:InstallUnravelServeronMapR-4.LogintoUnravelWebUI)
      - [\#Step1:InstallUnravelServeronMapR-](#Step1:InstallUnravelServeronMapR-)
      - [Congratulations\!](#Step1:InstallUnravelServeronMapR-Congratulations!)
  - [5. (Optional) Configure Unravel Server (Advanced
    Options)](#Step1:InstallUnravelServeronMapR-5.\(Optional\)ConfigureUnravelServer\(AdvancedOptions\))
      - [Enable Additional Data
        Collection/Instrumentation](#Step1:InstallUnravelServeronMapR-EnableAdditionalDataCollection/Instrumentation)
      - [Run the Unravel Web UI Configuration
        Wizard](#Step1:InstallUnravelServeronMapR-RuntheUnravelWebUIConfigurationWizard)

</div>

</div>

</div>

**Introduction**

This topic explains how to deploy Unravel Server 4.2 on the MapR
converged data platform. These instructions apply to Unravel Server
4.2. 

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

  - MapR 5.1-6.0.1
  - Hadoop 1.x - 2.x
  - Kafka 0.9, 0.10, 0.11, and 1.0 (Apache ver. equiv.)
  - Kerberos/MapR Tickets
  - Hive 0.9.x - 1.2.x
  - Spark 1.3.x - 2.0.x

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
  - HDFS+Hive+YARN+Spark client/gateway, Hadoop and Hive commands in
    `PATH`
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

**Network**

  - Port 3000 (or 4020) from users and entire cluster to Unravel Web UI
  - HDFS ports open from Hadoop cluster to Unravel Server(s)
  - For YARN, Hive Metadata DB port open to Unravel Server(s) for
    partition reporting
  - UDP and TCP port 4043 open from entire cluster to Unravel Server(s)
  - For Oozie, port 11000 open from Unravel Server(s) to the Oozie
    server
  - Resource Manager (RM) port 8032 from Unravel Server(s) to the RM
    server(s)
  - Port 4176, 4181 through 4189, 3316, 4091 must be available for
    localhost communication between Unravel daemons or services

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> 
> 
> <div class="container confluence-information-macro-body">
> 
> `HIGHLIGHTED `text and text with brackets ( { } ), except where
> otherwise noted, indicate where you must substitute your particular
> values for the text.
> 
> `UNRAVEL_``HOST`\_IP must be a fully qualified DNS or IP address
> 
> </div>

**1. Configure the Host Machine**

**Allocate a Cluster Gateway/Edge/Client Host with HDFS Access**

If you do not already have a gateway/edge/client host provisioned for
Unravel server, follow these steps which are needed to enable the
`hadoop fs` command, Hive, and Spark. Although Unravel does not launch
Hive and Spark jobs, it will be convenient for you to have those
installed there for part 2 of the installation+configuration process. 

For more information about the MapR client configuration,
see <https://maprdocs.mapr.com/52/ReferenceGuide/configure.sh.html>

Run the following commands on Unravel Server as `root` substituting your
site-specific values for `NAME, CLDB_LIST, HISTORY_SERVER.`

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
# sudo yum install mapr-client.x86_64
# sudo /opt/mapr/server/configure.sh -N {NAME} -c -C {CLDB_LIST} -HS {HISTORY_SERVER}
# sudo yum install mapr-hive.noarch
# sudo yum install mapr-spark.noarch
```

</div>

</div>

**Configure the Host *Before* installing the RPM**

  - Run the following commands on Unravel Server as `root`:
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo useradd -g mapr unravel
    # hadoop fs -mkdir /user/unravel
    # hadoop fs -chown unravel:mapr /user/unravel
    ```
    
    </div>
    
    </div>

  - If MapR tickets are enabled, check mapr ticket for users `unravel`
    and `mapr` on target host now. You might need to export ticket
    environment variables in `/etc/unravel_ctl` first.

  - Check available RAM to ensure availability:
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # free -g
    ```
    
    </div>
    
    </div>

  - For instructions on adjusting RAM allocated to MapR-FS (mfs), see
    <https://community.mapr.com/docs/DOC-1209>. For example, edit
    `/opt/mapr/conf/warden.conf` as follows:
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    service.command.mfs.heapsize.maxpercent=10
    ```
    
    </div>
    
    </div>
    
    (only change this setting on the Unravel gateway/client machine).
    And the restart mfs.

**2. Install the Unravel Server RPM on the Host Machine**

**Get the Unravel Server RPM**

See [Download Unravel
Software](https://unraveldata.atlassian.net/wiki/spaces/UNDOCS/pages/226132074/Download+Unravel+Software+Versions).

**Make symlinks if required**

If you want the two disk areas used by Unravel to be on different
volumes, you can make symlinks to specific areas before installing (or
do a mv and symlink after installing). Do it before the first install if
there is insufficient space on the target paths `/usr/local/unravel` and
`/srv/unravel` noted above.

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
copied. The `rpm` command does not require .`rpm` suffix. The
flag `-U `works for either initial install or upgrade.

Run the specified `await_fizup.sh` script to make sure background
processing is finished before you do other steps. In a routine upgrade,
it is okay to start all Unravel daemons, but do not stop or restart them
until the `await_fizup.sh` prints `DONE` (it takes a few minutes).

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
> in `/srv/unravel/` for larger storage. 
> 
>   
> The master configuration file is
> in `/usr/local/unravel/etc/unravel.properties` and the logs are in
> `/usr/local/unravel/logs/`. The RPM installation creates
> user `unravel` if it does not already exist and this can be changed
> after installation; `/etc/init.d/unravel_*` scripts for controlling
> its services as well as `/etc/init.d/unravel_all.sh` which can be used
> to manually stop, start, and get status of all daemons in proper
> order.
> 
>   
> During initial install, a bundled database is used. This can be
> switched to use an [externally managed
> MySQL](Installing-MySQL-or-Compatible-Database-for-Unravel_541131376.html)
> for production. 
> 
> </div>

**Do Host-Specific Post-Installation Actions**

Run the following commands on Unravel Server:

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
# sudo /etc/init.d/unravel_all.sh stop
# sudo /usr/local/unravel/install_bin/switch_to_mapr.sh
```

</div>

</div>

**3. Configure Unravel Server (Basic/Core Options)**

**Enable Optional Daemons**

Depending on your workload volume or kind of activity, you can enable
additional daemons at this point, see [Creating Multiple Workers for
High Volume
Data](Creating-Multiple-Workers-for-High-Volume-Data_541131395.html)

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
    descriptions in the table below. Some of these need to be
    uncommented (remove the leading \# char) after you edit to enable
    them.
    
    <div class="container">
    
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
    <td><pre><code>http://LAN_DNS:300</code></pre>
    0</td>
    </tr>
    <tr class="even">
    <td><code>com.unraveldata.cus tomer.organization</code></td>
    <td>Identifies your installation for reporting purposes.</td>
    <td><pre><code>Company_and_org</code></pre></td>
    </tr>
    <tr class="odd">
    <td><pre><code>com.unraveldata.tm</code></pre>
    pdir</td>
    <td><strong>Optional</strong>. Location where Unravel's temp file will reside</td>
    <td><pre><code>/srv/unravel/tmp</code></pre></td>
    </tr>
    <tr class="even">
    <td><code>com.unraveldata.his tory.maxSize.weeks</code></td>
    <td>Sets retention for search data.</td>
    <td><pre><code>26</code></pre></td>
    </tr>
    <tr class="odd">
    <td><code>com.unraveldata.hiv e.hook.topic.num.thre ads</code></td>
    <td><strong>Optional</strong>. Defines the number of threads. Default is
    1. Depending on job volume, increase this property to <em>N</em> where <em>N</em> is between 1 and 4, or roughly <em>ThousandJobsPerDay</em>/ 10.</td>
    <td><pre><code>1</code></pre></td>
    </tr>
    <tr class="even">
    <td><code>com.unraveldata.job .collector.done.log.b ase</code></td>
    <td>HDFS path to &quot;done&quot; directory of MR logs</td>
    <td><pre><code>/var/mapr/cluster/</code></pre>
    yarn/rm/staging/histo ry/done</td>
    </tr>
    <tr class="odd">
    <td><code>com.unraveldata.job .collector.log.aggreg ation.base</code></td>
    <td>An HDFS path that helps locate MR job logs to process</td>
    <td><pre><code>/tmp/logs/*/logs/</code></pre></td>
    </tr>
    <tr class="even">
    <td><code>com.unraveldata.log in.admins</code></td>
    <td>Defines the usernames that can access Unravel Web UI's admin pages. Default is <code>admin</code>.</td>
    <td><pre><code>admin</code></pre></td>
    </tr>
    <tr class="odd">
    <td><span class="title-ref">`com.unraveldata.spa rk.eventlog.location</span> `</td>
    <td>Where to find Spark event logs</td>
    <td><pre><code>maprfs:///apps/spa</code></pre>
    rk</td>
    </tr>
    <tr class="even">
    <td><code>yarn.resourcemanage r.webapp.address</code></td>
    <td>Resource Manager web app address</td>
    <td><pre><code>http://example.loc</code></pre>
    aldomain:8088</td>
    </tr>
    <tr class="odd">
    <td><code>yarn.resourcemanage r.webapp.username</code></td>
    <td>Resource Manager username to login</td>
    <td></td>
    </tr>
    <tr class="even">
    <td><code>yarn.resourcemanage r.webapp</code>.password</td>
    <td>Resource Manager password to login</td>
    <td></td>
    </tr>
    <tr class="odd">
    <td><code>https.protocols</code></td>
    <td>Enable https access to Resource Manager</td>
    <td>TLSv1.2</td>
    </tr>
    <tr class="even">
    <td><code>javax.jdo.option.Co nnectionURL</code></td>
    <td>A JDBC connection URL</td>
    <td>jdbc:mysql://example. localdomain:3306/hive <strong>OR</strong> jdbc:postgresql://exa mple.localdomain:7432 /hive_zzzzzz</td>
    </tr>
    <tr class="odd">
    <td><code>javax.jdo.option.Co nnectionDriverName</code></td>
    <td>JDBC driver</td>
    <td>com.mysql.jdbc.Driver <strong>OR</strong> org.postgresql .Driver</td>
    </tr>
    <tr class="even">
    <td><code>javax.jdo.option.Co nnectionUserName</code></td>
    <td>Hive metastore user name</td>
    <td>hiveuser</td>
    </tr>
    <tr class="odd">
    <td><code>javax.jdo.option.Co nnectionPassword</code></td>
    <td>Hive metastore password</td>
    <td></td>
    </tr>
    <tr class="even">
    <td><code>com.unraveldata</code>.m etastore.databasePatt ern</td>
    <td><strong>Optional</strong>.  Be selective for databases to analyze in metastore</td>
    <td>s*d*</td>
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
    
    </div>

**If Kerberos is Enabled:**

<div id="expander-1985410250" class="container expand-container">

<div id="expander-control-1985410250" class="container expand-control">

Add authentication for HDFS...

</div>

<div id="expander-content-1985410250" class="container expand-content">

Create or identify a principal and keytab for Unravel daemons to access
HDFS and REST when Kerberos is enabled. 

To get going faster, you can use the 'hdfs' principal which often has a
pre-existing "headless" keytab.

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

You can find and verify the principal the keytab by

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
# klist -kt KEYTAB_FILE
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
> The keytab file should have chmod bits 500 and be owned by
> `unravel` local user (default) or by the user you want to use, as
> explained in [Run Unravel Daemons with Custom
> User](Run-Unravel-Daemons-with-Custom-User_541033161.html).
> 
> </div>

</div>

</div>

**If Sentry is Enabled:**

<div id="expander-1312819624" class="container expand-container">

<div id="expander-control-1312819624" class="container expand-control">

Add these permissions...

</div>

<div id="expander-content-1312819624" class="container expand-content">

For quicker setup, use the mapr principal. For more narrow privileges,
define your own alt principal. The alt user can
be admin `unravel `(created by `rpm`) or one of your choosing. The
corresponding kerberos principal does not need to have the same name as
the local user. The user/principal here should correspond to the `X` in
the next section. The paths listed are examples, only. In your
environment, they might be
customized.

<div class="container table-wrap">

| Resource                                 | Principal   | Access | Purpose                      |
| ---------------------------------------- | ----------- | ------ | ---------------------------- |
| `hdfs://user/s park/applicatio nHistory` | mapr or alt | read   | Spark event log              |
| `hdfs://usr/hi story/done`               | mapr or alt | read   | MapReduce logs               |
| `hdfs://tmp/lo gs`                       | mapr or alt | read   | YARN aggregation folder      |
| `hdfs://user/h ive/warehouse`            | mapr or alt | read   | Obtain table partition sizes |
| `Hive Metastor e access`                 | hive        | read   | Hive table information       |

</div>

</div>

</div>

**Switch User**

Depending on your cluster security configuration, you will need to run
the `switch_to_user`  script. Dependencies like kerberos and which
target user you used for access permissions affect this. 

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
# sudo /usr/local/unravel/install_bin/switch_to_user.sh x y 
```

</div>

</div>

where `X` and `Y` depend on your environment. See the
[switch\_to\_user](Run-Unravel-Daemons-with-Custom-User_541033161.html) page. 

**Do Host-Specific Configuration Steps**

For MapR, there are no host-specific configuration steps.

**Restart Unravel Server**

After edits to `com.unraveldata.login.admins` in
`/usr/local/unravel/etc/unravel.properties` it is necessary to run the
following script in order to make changes take effect. The `echo`
command shows the page to visit with your browser. If you are using an
ssh tunnel or http proxy, you might need to make adjustments. 

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
# sudo /etc/init.d/unravel_all.sh start
# sleep 60
# echo "http://({UNRAVEL_HOST_IP} -f):3000/"
```

</div>

</div>

This completes the basic/core configuration.

**4. Log into Unravel Web UI**

Using a web browser, navigate to`http://({UNRAVEL_``HOST`\_IP} -f):3000/
and login as user `admin` with password `unraveldata`.

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

****

**Congratulations\!**

Unravel Server is up and running. Unravel Web UI displays collected
data. Check Unravel Web UI for MR jobs loading: on the **Applications**
page, select the **Map Reduce** tab.

For instructions on using Unravel Web UI, see the [User
Guide](User-Guide_541295329.html).

**5. (Optional) Configure Unravel Server (Advanced Options)**

**Enable Additional Data Collection/Instrumentation**

Install the Unravel Sensor Parcel on `gateway/edge/client` nodes that
are used to submit Hive queries to push additional information to
Unravel Server. For details, see [Step 2: Enable Additional Data
Collection / Instrumentation for MapR](541361101.html).

**Run the Unravel Web UI Configuration Wizard**

Run the Unravel Web UI configuration wizard to choose additional
configuration options. For instructions on configuring advanced options,
see the [Advanced Topics](Advanced-Topics_541197049.html).

</div>

<div class="container pageSection group">

<div class="container pageSectionHeader">

**Attachments:**

</div>

<div class="container greybox">

![image0](images/icons/bullet_blue.gif)
[image2017-2-26\_0-20-12.png](attachments/541361105/541229862.png)
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
