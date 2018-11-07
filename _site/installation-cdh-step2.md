  - title  
    Step 2: Install Unravel Sensor Parcel on CDH+CM

# Step 2: Install Unravel Sensor Parcel on CDH+CM

<div id="page" class="container">

<div id="main" class="container aui-page-panel">

<div id="main-header" class="container">

<div id="breadcrumb-section" class="container">

1.  [Unravel 4.4](index.html)
2.  [Unravel 4.4](Unravel-4.4_541197025.html)
3.  [Installation Guides](Installation-Guides_541393730.html)
4.  [Cloudera Distribution Including Apache Hadoop (CDH), with Cloudera
    Manager (CM)](541361096.html)

</div>

**Unravel 4.4 : Step 2: Install Unravel Sensor Parcel on CDH+CM**

</div>

<div id="content" class="container view">

<div class="container page-metadata">

Created by katch, last modified on Sep 10, 2018

</div>

<div id="main-content" class="container wiki-content group">

<div class="container panel">

<div class="container panelHeader">

**Table of
    Contents**

</div>

<div class="container panelContent">

<div class="container toc-macro rbtoc1541196937543">

  - [Introduction](#Step2:InstallUnravelSensorParcelonCDH+CM-_17zqoqqgx4oqIntroduction)
  - [1. Obtain and Distribute the Parcel from Unravel
    Server](#Step2:InstallUnravelSensorParcelonCDH+CM-1.ObtainandDistributetheParcelfromUnravelServer)
  - [2. Put the Hive Hook JAR in
    AUX\_CLASSPATH](#Step2:InstallUnravelSensorParcelonCDH+CM-2.PuttheHiveHookJARinAUX_CLASSPATH)
      - [If Sentry is
        Enabled:](#Step2:InstallUnravelSensorParcelonCDH+CM-IfSentryisEnabled:)
  - [3. Configure the Gateway Automatic Deployment of Hive
    Instrumentation](#Step2:InstallUnravelSensorParcelonCDH+CM-ConfigureGateway3.ConfiguretheGatewayAutomaticDeploymentofHiveInstrumentation)
      - [Set the hive-site.xml Snippet in Cloudera Manager, and Deploy
        the Hive Client Configuration to
        Gateways](#Step2:InstallUnravelSensorParcelonCDH+CM-Setthehive-site.xmlSnippetinClouderaManager,andDeploytheHiveClientConfigurationtoGateways)
      - [Check Unravel Web
        UI](#Step2:InstallUnravelSensorParcelonCDH+CM-CheckUnravelWebUI)
  - [4. Configure the Gateway Automatic Deployment of Spark
    Instrumentation](#Step2:InstallUnravelSensorParcelonCDH+CM-4.ConfiguretheGatewayAutomaticDeploymentofSparkInstrumentation)
  - [5. Optional - Configure YARN - MapReduce (MR) JVM Sensor
    Cluster-Wide](#Step2:InstallUnravelSensorParcelonCDH+CM-ConfigureJVMSensorForYARN5.Optional-ConfigureYARN-MapReduce\(MR\)JVMSensorCluster-Wide)
  - [6. (Optional) Advanced
    Configuration](#Step2:InstallUnravelSensorParcelonCDH+CM-6.\(Optional\)AdvancedConfiguration)
  - [Troubleshooting](#Step2:InstallUnravelSensorParcelonCDH+CM-_troubleshootingTroubleshooting)
  - [References](#Step2:InstallUnravelSensorParcelonCDH+CM-References)

</div>

</div>

</div>

**Introduction**

This topic explains how to install the Unravel Sensor parcel on CDH
clusters using Cloudera Manager (CM). The parcel includes Hive Hook and
Spark instrumentation JARs. Hive Hook is used to collect information
about Hive queries in Hadoop. The Spark instrumentation JARs measure
Spark job performance.

These instructions apply to Unravel Sensor 4.3

Before following these instructions, follow the steps in [Step 1:
Install Unravel Server on
CDH+CM](https://unraveldata.atlassian.net/wiki/spaces/UN43/pages/226197657).

<div class="container panel">

<div class="container panelHeader">

**Workflow Summary**

</div>

<div class="container panelContent">

1.  Obtain and distribute the parcel from Unravel Server.
2.  Put the Hive Hook JAR in `AUX_CLASSPATH`.
3.  Configure the gateway automatic deployment of Hive instrumentation.
4.  Configure the gateway automatic deployment of Spark instrumentation.

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
> When Active Directory Kerberos is used,`UNRAVEL_HOST_IP`must be a
> fully qualified path or IP address.
> 
> </div>

</div>

</div>

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> To Upgrade the Unravel Sensor
> 
> <div class="container confluence-information-macro-body">
> 
>   - Check the `UNRAVEL_SENSOR` parcel status in Cloudera Manager.
>   - If an upgrade is available complete [steps 3 through
>     5](#Step2:InstallUnravelSensorParcelonCDH+CM-ConfigureGateway) to
>     perform the upgrade.
> 
> </div>

**1. Obtain and Distribute the Parcel from Unravel Server**

1.  In Cloudera Manager (CM), go to the **Parcels** page by clicking the
    parcels glyph () on the top of the page.
2.  Click **Configuration** to see the **Parcel Settings** pop-up.
3.  In the **Remote Parcel Repository URLs** section of the **Parcel
    Settings** pop-up, click the **+** glyph to add a new entry.
4.  Add `http://{UNRAVEL_HOST_IP`}:3000/parcels/cdh{X.Y}/ (include
    trailing slash) where `X.Y` is the major, minor version of CDH
    version you are running and `UNRAVEL_HOST_IP` is the host name or
    LAN IP address of Unravel Server where `unravel_lr` is running. If
    you are running more than one version of CDH (multiple clusters) in
    Cloudera Manager, you can add more than one parcel directory from
    the `UNRAVEL_HOST_IP`.
5.  Click **Save**.
6.  Click **Check for New Parcels**.
7.  On the **Parcels** page, pick a target cluster in the **Location**
    box.
8.  In the list of **Parcel Names**, find the `UNRAVEL_SENSOR` parcel
    that matches the version of the target cluster and click
    **Download**.
9.  Click **Distribute**.
10. If you have an old parcel from Unravel, you can deactivate it now.
11. Then click **Activate** on the new parcel.

**2. Put the Hive Hook JAR in AUX\_CLASSPATH**

1.  In Cloudera Manager, for the target cluster, click **Hive** |
    **Configuration**, and search for `hive-env`.

2.  In **Gateway Client Environment Advanced Configuration Snippet
    (Safety Valve)** for `hive-env.sh` enter the following exactly as
    shown, with no
    subsitutions:
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    AUX_CLASSPATH=${AUX_CLASSPATH}:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/unravel_hive_hook.jar
    ```
    
    </div>
    
    </div>

3.  In Cloudera Manager, click **YARN** | **Configuration**, and search
    for `hadoop-env`.

4.  In **Gateway Client Environment Advanced Configuration Snippet
    (Safety Valve)** for `hadoop-env.sh`, enter the following exactly as
    shown, with **no
    subsitutions**:
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    HADOOP_CLASSPATH=${HADOOP_CLASSPATH}:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/unravel_hive_hook.jar
    ```
    
    </div>
    
    </div>

****If Sentry is Enabled:****

Sentry commands may also be needed to enable access to the Hive Hook JAR
file. Grant privileges on the JAR files to the roles that run hive
queries. Log into Beeline as user `hive` and use the Hive `SQL GRANT`
statement to do so. For example (substitute `{ROLE}` as
appropriate):

<div class="container">

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
# GRANT ALL ON URI 'file:///opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/unravel_hive_hook.jar' TO ROLE {ROLE}
```

</div>

</div>

</div>

**3. Configure the Gateway Automatic Deployment of Hive
Instrumentation**

Use Cloudera Manager to deploy the `hive-site.xml` snippet, which is the
content of `/usr/local/unravel/hive-hook/hive-site.xml.snip` on Unravel
Server.

<div class="container">

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> 
> 
> <div class="container confluence-information-macro-body">
> 
> On a multi-host Unravel Server deployment, use
> host2's `/usr/local/unravel/hive-hook/hive-site.xml.snip`
> 
> </div>

**Set the `hive-site.xml` Snippet in Cloudera Manager, and Deploy the
Hive Client Configuration to Gateways**

In Cloudera Manager (CM):

1.  Go to Hive service.

2.  Select the **Configuration** tab.

3.  Search for `hive-site.xml` in the middle of the page.

4.  Add the xml snippet to **Hive Client Advanced Configuration Snippet
    for \`\`hive-site.xml\`\`** (Gateway Default Group) (Click **View as
    XML**). 
    
    <div class="container">
    
    </div>
    
    confluence-information-macro confluence-information-macro-warning
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > If cluster has been configured with "Cloudera Navigator"; the
    > `hive.exec.post.hooks` property will have exsiting value(s).
    > Therefore append the unravel's value into the
    > existing `hive.exec.post.hooks` property with a comma and no
    > space. (see example
    > below) 
    > 
    > `com.cloudera.navigator.audit.hive.HiveExecHookContext,org.apache.hadoop.hive.ql.hooks.LineageLogger,
    > com.unraveldata.dataflow.hive.hook.HivePostHook`
    > 
    > **IMPORTANT\!** The "Hive Client Advanced Configuration Snippet
    > for hive-site.xml" should only have the Unravel class.  
    > However, the "HiveServer2 Advanced Configuration Snippet for
    > hive-site.xml" should have all 3 classes: 2 from Navigator and 1
    > from Unravel.
    > 
    > </div>

5.  Add the xml snippet to **HiveServer2 Advanced Configuration Snippet
    for** `hive-site.xml`. (Click **View as XML**). Like above step, if
    properties exists, append unravel's value.

6.  Save the changes with optional comment "Unravel snippet in
    `hive-site.xml`**"**

7.  Perform action **Deploy Hive Client Configuration** by clicking the
    deploy glyph () or by using the **Actions** pull-down menu.

8.  Restart the Hive service. (Cloudera Manager will specify a restart
    which is not necessary for activating these changes. You may act on
    CM's recommendation at a later time. )

Again, monitor the situation to see if all Hive queries are failing with
a class not found or permission problems. **If they are failing**, you
can back-out the `hive-site.xml` advanced snippet changes in Cloudera
Manager, deploy client configuration, and restart the Hive service to
revert, then refer to
[Troubleshooting](https://unraveldata.atlassian.net/wiki/pages/resumedraft.action?draftId=53649678#Part2:InstallUnravelSensorParcelonCDH+CM-_troubleshooting)
below.

**Check Unravel Web UI**

If queries are running fine and appearing in Unravel Web UI, then you
are done.

</div>

**4. Configure the Gateway Automatic Deployment of Spark
Instrumentation**

1.  In Cloudera Manager, select the target cluster, then select the
    Spark service.

2.  Select **Configuration**.

3.  Search for "`spark-defaults`".

4.  In the **Spark Client Advanced Configuration Snippet (Safety Valve)
    for spark-conf/spark-defaults.conf**, enter the following text,
    substituting the highlighted text for your particular values:
    
    <div class="container">
    
    </div>
    
    confluence-information-macro confluence-information-macro-note
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > On a multi-host Unravel Server deployment, use the fully qualified
    > DNS or logical host2 for`UNRAVEL_HOST_IP` which must be .
    > 
    > </div>
    
    <div class="container">
    
    </div>
    
    confluence-information-macro confluence-information-macro-warning
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > Copy the text below and paste it into the Cloudera Manager's
    > **Spark Client Advanced Configuration Snippet (Safety Valve)** box
    > for **\`\`spark-conf/spark-defaults.conf\`\`**. Then, modify the
    > value of `UNRAVEL_HOST_IP`and `SPARK_VERSION-X.Y`.
    > 
    > `SPARK_VERSION``-X.Y`has the following possible values:
    > `spark-1.3` for Spark 1.3.x, `spark-1.5` for Spark 1.5.x,
    > `spark-1.6` for Spark 1.6.x, and `spark-2.0` for Spark 2.0.x.
    > 
    > </div>
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    spark.unravel.server.hostport={UNRAVEL_HOST_IP}:4043 
    spark.driver.extraJavaOptions=-javaagent:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/btrace-agent.jar=config=driver,libs={SPARK_VERSION-X.Y}
    spark.executor.extraJavaOptions=-javaagent:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/btrace-agent.jar=config=executor,libs={SPARK_VERSION-X.Y}
    spark.eventLog.enabled=true
    ```
    
    </div>
    
    </div>

5.  Save changes.

6\. Deploy client configuration by clicking the deploy glyph () or by
using the **Actions** pull-down menu. Your spark-shell will ensure new
JVM containers are created with the necessary extraJavaOptions for the
spark drivers and executors.

<div class="container">

<div class="container">

</div>

confluence-information-macro confluence-information-macro-warning

> 
> 
> <div class="container confluence-information-macro-body">
> 
> Monitor the situation to see if all Spark queries are failing with a
> class not found or permission problems. **If they are failing**, you
> can back-out the `spark-defaults.conf` changes in Cloudera Manager,
> re-deploy client configuration, and then investigate and fix the
> issue.
> 
> </div>

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> 
> 
> <div class="container confluence-information-macro-body">
> 
> In the case of yarn-client mode applications, the Spark default
> configuration won't be sufficient because the driver JVM starts before
> the configuration set through the SparkConf is applied. (See 
> [Apache's Spark
> Configuration](https://spark.apache.org/docs/latest/configuration.html#runtime-environment)
> for more information.)
> See [here](Individual-Applications-Submitted-Through-spark-submit_541164099.html#IndividualApplicationsSubmittedThroughspark-submit-SparkSubmit)
> for how to set up Unravel Sensor for Spark to profile specific Spark
> applications only, i.e.,  *per-application profiling* rather than
> *cluster-wide profiling*.
> 
> </div>

</div>

**5. Optional - Configure YARN - MapReduce (MR) JVM Sensor
Cluster-Wide**

1.  In Cloudera Manager (CM) go to **YARN** service.

2.  Select the **Configuration** tab.

3.  Search for **ApplicationMaster Java Opts Base**and concatenate the
    following xml block properties snippet (ensure to start with a space
    and add below**).**
    
    <div class="container">
    
    </div>
    
    confluence-information-macro confluence-information-macro-note
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > Make sure that "-" is a minus sign. You need to modify the value
    > of `UNRAVEL_HOST_IP` with your Unravel server IP address or a
    > fully qualified DNS. For multi-host Unravel installation, use the
    > IP address of
    Host2.
    > 
    > </div>
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    -javaagent:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/btrace-agent.jar=libs=mr -Dunravel.server.hostport={UNRAVEL_HOST_IP}:4043
    ```
    
    </div>
    
    </div>

4.  Search for **MapReduce Client Advanced Configuration Snippet (Safety
    Valve) for \`\`mapred-site.xml\`\`**in the middle of the page.

5.  Enter following xml four block properties snippet to Gateway Default
    Group  (Click \**View as
    XML*\*).
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    <property><name>mapreduce.task.profile</name><value>true</value></property>
    <property><name>mapreduce.task.profile.maps</name><value>0-5</value></property>
    <property><name>mapreduce.task.profile.reduces</name><value>0-5</value></property>
    <property><name>mapreduce.task.profile.params</name><value>-javaagent:/opt/cloudera/parcels/UNRAVEL_SENSOR/lib/java/btrace-agent.jar=libs=mr -Dunravel.server.hostport={UNRAVEL_HOST_IP}:4043</value></property>
    ```
    
    </div>
    
    </div>

6.  Save changes.

7\. Deploy client configuration by clicking the deploy glyph () or by
using the **Actions** pull-down menu.

8\. Cloudera Manager will specify a restart which is not necessary to
effect these changes. (click \**Restart Stale Services*\* if that is
visible. However, you can also perform this later when you have a
planned maintenance.)

<div class="container">

<div class="container">

</div>

confluence-information-macro confluence-information-macro-tip

> 
> 
> <div class="container confluence-information-macro-body">
> 
> Monitor the situation and you should see in Unravel UI a Resource
> Usage tab showing you mappers and reducers when you view the
> Application page for any completed MRjob. Restart is important for MR
> sensor to be picked up by queries submitted via Hiveserver2.
> 
> </div>

</div>

**6. (Optional) Advanced Configuration**

  - Configuration for high volume data: see [Creating Multiple Workers
    for High Volume
    Data](Creating-Multiple-Workers-for-High-Volume-Data_541131395.html).
  - Add LDAP users: see [Integrating LDAP Authentication for Unravel Web
    UI](Integrating-LDAP-Authentication-for-Unravel-Web-UI_541328067.html).

**Troubleshooting**

<div class="container table-wrap">

<table style="width:99%;">
<colgroup>
<col style="width: 38%" />
<col style="width: 26%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><strong>Symptom</strong></td>
<td><strong>Problem</strong></td>
<td><strong>Remedy</strong></td>
</tr>
<tr class="even">
<td><div class="line-block">``hadoop fs -ls /user/u</div>
nravel/HOOK_RESULT_DIR/`` | shows directory does not exist</td>
<td><ul>
<li>Unravel Server RPM is not yet installed, or</li>
<li>Unravel Server RPM is installed on a different HDFS cluster, or</li>
<li>HDFS home directory for Unravel does not exist, or</li>
</ul>
- kerberos/sent ry actions are needed</td>
<td><div class="line-block">Install Unravel RPM on Unravel service host:<br />
``sudo rpm -U unrav</div>
<dl>
<dt>el*.rpm*<code>| **OR** | Verify that</code>unravel<code>user   exists and has a</code>/user/unravel/``</dt>
<dd><p>directory in HDFS and unravel has write access.</p>
</dd>
</dl></td>
</tr>
<tr class="odd">
<td><code>ClassNotFound</code> error for <code>com.unraveldata.dataflo w.hive.hook.HivePreHook</code> during Hive query execution</td>
<td>Unravel hive hook JAR was not found in in <code>$HIVE_HOME/lib /</code>.</td>
<td><div class="line-block">Check whether UNRAVEL_SENSOR parcel was distributed and activated in CM.<br />
<strong>OR</strong><br />
Put the Unravel hive-hook JAR corresponding to <code>HIVE_VER</code> in <code>JAR_DEST</code> on each gateway by:<br />
``cd /usr/local/unr</div>
avel/hive-hook/;<code>|</code>cp unravel-hive-H IVE_VER*hook.jar <a href="">JAR</a> DEST``</td>
</tr>
</tbody>
</table>

</div>

**References**

[{+}](http://www.cloudera.com/documentation/enterprise/5-3-x/topics/cm_mc_hive_udf.html#concept_nc3_mms_lr_unique_2)<http://www.cloudera.com/documentation/enterprise/5-3-x/topics/cm_mc_hive_udf.html#concept_nc3_mms_lr_unique_2+>
see Creating Permanent Functions.

</div>

<div class="container pageSection group">

<div class="container pageSectionHeader">

**Attachments:**

</div>

<div class="container greybox">

![image0](images/icons/bullet_blue.gif)
[DeployGlyph.png](attachments/541229840/541033431.png) (image/png)
![image1](images/icons/bullet_blue.gif)
[package.png](attachments/541229840/541098884.png) (image/png)

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
