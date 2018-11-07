  - title  
    Step 2: Enable Additional Data Collection / Instrumentation for HDP

# Step 2: Enable Additional Data Collection / Instrumentation for HDP

<div id="page" class="container">

<div id="main" class="container aui-page-panel">

<div id="main-header" class="container">

<div id="breadcrumb-section" class="container">

1.  [Unravel 4.4](index.html)
2.  [Unravel 4.4](Unravel-4.4_541197025.html)
3.  [Installation Guides](Installation-Guides_541393730.html)
4.  [Hortonworks Data Platform (HDP)](541197289.html)

</div>

**Unravel 4.4 : Step 2: Enable Additional Data Collection /
Instrumentation for HDP**

</div>

<div id="content" class="container view">

<div class="container page-metadata">

Created by katch, last modified by Grant Liu on Oct 29, 2018

</div>

<div id="main-content" class="container wiki-content group">

**Introduction**

This topic explains how to configure Unravel Sensor for Tez
(`unravel_us`) to retrieve data from the Tez execution engine.
Retrievable data includes Hive queries, application timelines, sensor
agent data, YARN RM data, logs, and so on.

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> 
> 
> <div class="container confluence-information-macro-body">
> 
> `HIGHLIGHTED`text and text with brackets ( { } ) indicate where you
> must substitute your particular values for the text.
> 
> `UNRAVEL_HOST_IP`must be a fully qualified DNS or an IP address.
> 
> </div>

**1. Convert Your Unravel Installation to HDP.**

<div class="container">

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
> Note: This change remains after RPM upgrades.
> 
> </div>

</div>

**2. Update Site-Specific HDP Properties in unravel.properties**

  - Add these properties
    `to/usr/local/unravel/etc/unravel.properties`:
    
    <div class="container table-wrap">
    
    | Property                                                | Description                                              | Default Value        |
    | ------------------------------------------------------- | -------------------------------------------------------- | -------------------- |
    | `com.unraveldata.yarn.timelin e-service.webapp.address` | The http address of the Timeline service web application | <http://> localho st |
    | `com.unraveldata.yarn.timelin e-service.port`           | Timeline service port                                    | 8188                 |
    

    </div>
    
    For
    example,
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    com.unraveldata.yarn.timeline-service.webapp.address=http://172.16.1.101
    com.unraveldata.yarn.timeline-service.port=8188
    ```
    
    </div>
    
    </div>

  -  Open `/usr/local/unravel/etc/unravel.properties` to see if you need
    to modify any of these properties which were added or modified by
    the script above (`switch_to_hdp.sh`).
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    // Repoint Unravel application logs directory
    com.unraveldata.job.collector.done.log.base=/mr-history/done
    com.unraveldata.job.collector.log.aggregation.base=/app-logs/*/logs/
    com.unraveldata.spark.eventlog.location=hdfs:///spark-history/
    
    // Add Hive Metastore database information for Unravel Hive Config 
    javax.jdo.option.ConnectionURL=jdbc:mysql://{hostname}:3306/{database_name} 
    javax.jdo.option.ConnectionDriverName=com.mysql.jdbc.Driver
    javax.jdo.option.ConnectionUserName={HiveMetastoreUserName} 
    javax.jdo.option.ConnectionPassword={HiveMetastorePassword}
    ```
    
    </div>
    
    </div>

  - Log into Ambari Web UI (AWU) to verify the above properties have
    been set correctly in `unravel.properties`. Update the value of any
    property which has not been set correctly.
    
      - On the left-hand side of AWU's dashboard, click MapReduce2 |
        Configs. Select the Advanced tab, and then select**Advanced
        mapred-site**
          - Verify `com.unraveldata.job.collector.done.log.base` equals
            **mapreduce.jobhistory.done-dir.**
      - On the left-hand side of AWU's dashboard, click YARN | Configs.
        Select the Advanced tab, and then select**Node Manager**.
          - Verify
            `com.unraveldata.job.collector.done.log.``aggregation.base`
            equals **yarn.nodemanager.remote-app=log-dir**.
      - On the left-hand side of AWU's dashboard, click Hive | Configs.
        Select the Advanced tab, and then select**Hive Metastore**.
          - Verify
            `javax.jdo.option.ConnectionURL`equals jdbc:mysql://**Database
            Host**:3306/**Database URL**.
          - Verify `javax.jdo.option.ConnectionDriverName`equals **JDBC
            Driver Class**.
          - Verify `javax.jdo.option.ConnectionUserName`equals
            **Database** **Username**.

**3. Start Unravel Server**

<div class="container">

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> 
> 
> <div class="container confluence-information-macro-body">
> 
> Note: Unravel must be up for the next step to complete.
> 
> </div>

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
# sudo /etc/init.d/unravel_all.sh start
```

</div>

</div>

</div>

**4. Install Unravel Hive Hook and Spark Sensor Onto HDP Servers**

<div class="container">

Install Unravel Hive Hook and Spark Sensor onto HDP servers (JAR files)
as follows (substitute the correct fully qualified host name or IP
address for `UNRAVEL_HOST_IP`).

</div>

  - Login as root and ensure `wget` is
    installed
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # /usr/local/unravel/install_bin/unraveldata-clients/unravel_hdp_setup.sh 
    ```
    
    </div>
    
    </div>

  - From Unravel server (eg. edge node) run on each server that will use
    instrumentation.
    
    <div class="container">
    
    </div>
    
    confluence-information-macro confluence-information-macro-warning
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > Be sure to substitute valid values.
    > 
    > `SPARK_VERSION_X.Y.Z`has the following possible value: 1.3.0,
    > 1.5.0, 1.6.0, 2.0.0, 2.1.0, 2.2.0 and 2.3.0 for Spark
    > versions 1.3.x, 1.5.x,  1.6.x, 2.0.x,  2.1.x, 2.2.x, and 2.3.x
    > respectively.
    > 
    > `HIVE_VERSION` must be a Hive version that Unravel Supports, i.e.,
    > 1.2.0 or 0.13.0.  If you are using Hive 1.2.1, please use 1.2.0
    > for the Hive version.
    > 
    > </div>
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # yum install -y wget
    # cd /usr/local/unravel/install_bin/unraveldata-clients/
    # sudo ./unravel_hdp_setup.sh install -y --unravel-server {UNRAVEL_HOST_IP}:3000 --hive-version {HIVE_VERSION} --spark-version {SPARK_VERSION}
    ```
    
    </div>
    
    </div>

  - Files are installed locally on the edge node under:
    
      - `/usr/local/unravel_client (Hive hook jar)`
      - `/usr/local/unravel-agent/jars/ (Resource metrics sensor jars)`

  - Manually copy these two directories on all nodes in the cluster
    (worker / edge / master). In all cases, instrumented nodes must be
    able to open port 4043 of Unravel Server (`host2` if multi-host
    Unravel install)

**5. Add Unravel Hive Hook hive-site Settings to All of HDP's Servers in
the Cluster Using AWU**

<div class="container">

<div class="container">

</div>

confluence-information-macro confluence-information-macro-warning

> 
> 
> <div class="container confluence-information-macro-body">
> 
> Completion of this step requires a restart of all affected Hive
> services in Ambari UI.
> 
> </div>

</div>

  - In AWU, on the left-hand side, click **Hive** | **Configs**. Select
    the **Advanced** tab, and then select **Custom hive-site** and **Add
    Property** below and use **Bulk property add
    mode.**
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    hive.exec.driver.run.hooks=com.unraveldata.dataflow.hive.hook.HiveDriverHook 
    com.unraveldata.hive.hdfs.dir=/user/unravel/HOOK_RESULT_DIR 
    com.unraveldata.hive.hook.tcp=true 
    com.unraveldata.host={add unravel gateway internal IP hostname} 
    
    // Find below properties as it may already exists, concatenate it with a comma & no spaces
    hive.exec.pre.hooks=com.unraveldata.dataflow.hive.hook.HivePreHook 
    hive.exec.post.hooks=com.unraveldata.dataflow.hive.hook.HivePostHook 
    hive.exec.failure.hooks=com.unraveldata.dataflow.hive.hook.HiveFailHook
    ```
    
    </div>
    
    </div>
    
      - If LLAP is enabled copy the above settings in **Custom
        hive-interactive-site** as well.
        
        <div class="container">
        
        </div>
        
        confluence-information-macro confluence-information-macro-tip
        
        > Manual edit hive-site.xml (no AWU)
        > 
        > <div class="container confluence-information-macro-body">
        > 
        > `hive-site.xml`file is located at `/etc/hive/conf/`.
        > 
        > </div>

  - Add `AUX_CLASSPATH` to AWU's **hive-env** template:
    
      - In AWU, on the left-hand side, click **Hive**, next click
        **Configs**, go to the **Advanced** tab, and select/click
        **Advanced hive-env**.
    
      - Next, inside the **Advanced hive-env** template, towards the end
        of line add
        below:
        
        <div class="container code panel pdl">
        
        <div class="container codeContent panelContent pdl">
        
        ``` sourceCode syntaxhighlighter-pre
        export AUX_CLASSPATH=${AUX_CLASSPATH}:/usr/local/unravel_client/unravel-hive-1.2.0-hook.jar
        ```
        
        </div>
        
        </div>
    
      - If LLAP is enabled copy above line of code into **Advanced
        hive-interactive-env** as well.
        
        <div class="container">
        
        </div>
        
        confluence-information-macro confluence-information-macro-tip
        
        > You can manually edit hive-env.sh without using AWU.
        > 
        > <div class="container confluence-information-macro-body">
        > 
        > The `hive-env.sh` file is located at `/etc/hive/conf/`.
        > 
        > </div>

  - Add `HADOOP_CLASSPATH` to AWU's **hadoop-env** template:
    
      - In AWU, on the left-hand side, click **HDFS**, next click
        **Configs**, go to the **Advanced** tab, and select/click
        **Advanced hadoop-env**.
    
      - Next, inside the **hadoop-env** template, look for **export
        HADOOP\_CLASSPATH** and append unravel's jar path like
        below:
        
        <div class="container code panel pdl">
        
        <div class="container codeContent panelContent pdl">
        
        ``` sourceCode syntaxhighlighter-pre
        export HADOOP_CLASSPATH=${HADOOP_CLASSPATH}:/usr/local/unravel_client/unravel-hive-1.2.0-hook.jar
        ```
        
        </div>
        
        </div>

**6. **Optionally for Tez**, Enable Unravel Tez Instrumentation on all
of HDP's Services in the Cluster**

<div class="container">

<div class="container">

</div>

confluence-information-macro confluence-information-macro-warning

> 
> 
> <div class="container confluence-information-macro-body">
> 
> Completion of this step requires a restart of all affected Hive
> services in Ambari UI.
> 
> </div>

</div>

Confirm that hive-execution.engine is set to tez.

<div class="container preformatted panel">

<div class="container preformattedContent panelContent">

    set hive.execution.engine=tez;

</div>

</div>

Using the Ambari Web UI (AWU), configure the Btrace agent for
    Tez:

  - Append`-javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=libs=mr,config=tez
    -Dunravel.server.hostport=UNRAVEL_HOST_IP:4043`to
    tez.am.launch.cmd-opts and tez.task.launch.cmd-opts.

  - Restart the affected component(s). The screenshot below illustrates
    this change.
    
    <div class="container">
    
    </div>
    
    confluence-information-macro
    confluence-information-macro-information
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > In a Kerberos environment you need to modify tez.am.view-acls
    > property with the RUN\_AS user or \*.
    > 
    > </div>

**7. **Optionally for Spark on YARN**.**

Enable Unravel Spark Instrumentation on All of HDP's Servers in the
Cluster

<div class="container">

<div class="container">

</div>

confluence-information-macro confluence-information-macro-warning

> 
> 
> <div class="container confluence-information-macro-body">
> 
> Completion of this step requires a restart of all affected Spark
> services in Ambari UI.
> 
> Be sure to substitute valid values.for `UNRAVEL_HOST_IPX`
> and `SPARK_VERSION_X.Y.`
> 
>  `SPARK_VERSION_X.Y`  has the following possible
> values: **\`\`spark-1.3\`\`** for Spark
> 1.3.x, **\`\`spark-1.5\`\` **for Spark
> 1.5.x, **\`\`spark-1.6\`\` **for Spark 1.6.x,
>  **\`\`spark-2.0\`\` **for Spark 2.0.x, spark-`2.2` for Spark 2.2.x,
> and spark-`2.3` for Spark 2.3.x
> 
> </div>

</div>

  - Add Spark properties into AWU's **Custom spark-defaults**:
    
      - In AWU, on the left-hand side, click **Spark** | **Configs** |
        **Custom spark-defaults**.

  - 
    
    <div class="container">
    
    </div>
    
    confluence-information-macro confluence-information-macro-tip
    
    > You can manually edit spark-defaults.conf without using AWU.
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > The default location for `spark-defaults.conf`
    > is `/usr/hdp/current/SPARK_VERSION_X.Y` except when
    > 
    >   - The cluster only has one spark 1.X
    >     version:  `/usr/hdp/current/spark-client/conf`
    > 
    >   - For spark 2.X version: `/usr/hdp/current/spark2-client/conf`
    >     
    >     Inside **Custom spark-defaults**, click **Add Property** for
    >     Spark as follows and use  \**Bulk property add mode*\*:
    >     
    >     <div class="container code panel pdl">
    >     
    >     <div class="container codeContent panelContent pdl">
    >     
    >     ``` sourceCode syntaxhighlighter-pre
    >     spark.unravel.server.hostport={UNRAVEL_HOST_IP}:4043
    >     spark.driver.extraJavaOptions=-javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=config=driver,libs={SPARK_VERSION_X.Y}
    >     spark.executor.extraJavaOptions=-javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=config=executor,libs={SPARK_VERSION_X.Y} 
    >     spark.eventLog.enabled=true
    >     ```
    >     
    >     </div>
    >     
    >     </div>
    > 
    > </div>

**8. **Optionally for MapReduce2** (MR)**

**JVM Sensor Cluster-Wide**

<div class="container">

<div class="container">

</div>

confluence-information-macro confluence-information-macro-warning

> 
> 
> <div class="container confluence-information-macro-body">
> 
> Completion of this step will require a restart of all affected HDFS,
> MAPREDUCE2, YARN andHIVE services in Ambari UI.
> 
> </div>

</div>

  - In AWU, on the left-hand side, click **MapReduce2**, next
    click **Configs**, go to the **Advanced** tab, and select/click
    **Advanced mapred-site**

  - Search for MR AppMaster Java Heap Size property and concatenate by
    copying and pasting the below property for
    `current.yarn.app.mapreduce.am.command-opts`MR JVM sensor. Be sure
    to leave a white space in-between the current and the following new
    property.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    -javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=libs=mr -Dunravel.server.hostport={UNRAVEL_HOST_IP}:4043
    ```
    
    </div>
    
    </div>

  - On the top notification banner, click Save

  - In AWU, on the left-hand side, click **MapReduce2**, next
    click **Configs**, go to the **Advanced** tab, and
    select/click **Custom mapred-site:**

  - Inside **Custom mapred-site**, click **Add Property** for MR JVM
    sensor as follows and use  \**Bulk property add mode*\*:

  - On the top notification banner, click Save
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    mapreduce.task.profile=true
    mapreduce.task.profile.maps=0-5
    mapreduce.task.profile.reduces=0-5
    mapreduce.task.profile.params=-javaagent:/usr/local/unravel-agent/jars/btrace-agent.jar=libs=mr -Dunravel.server.hostport=UNRAVEL_HOST_IP:4043
    ```
    
    </div>
    
    </div>
    
    Notice that in this code block, the blank lines separate single full
    lines of text that are wrapped due to length. Also, ensure you
    replace `UNRAVEL_HOST_IP` with your Unravel server's IP address.
    
    <div class="container">
    
    </div>
    
    confluence-information-macro confluence-information-macro-tip
    
    > You can manually edit mapred-site.xml without using AWU.
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > The `mapred-site.xml` file is located at `/etc/hadoop/conf/`.
    > 
    > </div>

  - Propagate Unravel resource metrics sensor jaronto all the servers in
    the clusteras follows**:**
    
    <div class="container">
    
    </div>
    
    confluence-information-macro confluence-information-macro-note
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > If you have already run the `unravel_hdp_setup.sh` script to
    > distribute sensor; you can skip the following steps
    > 
    > </div>

  - ssh to the Unravel gateway host server and use guided steps to unzip
    the Unravel MR jars.
    
      - With root or sudo access, change directory to
        `/usr/local/unravel-agent` and run the below `curl` get command:
        
        <div class="container code panel pdl">
        
        <div class="container codeContent panelContent pdl">
        
        ``` sourceCode syntaxhighlighter-pre
        # cd /usr/local/unravel-agent
        # curl http://localhost:3000/hh/unravel-agent-pack-bin.zip -o unravel-agent-pack-bin.zip
        # unzip -d jars unravel-agent-pack-bin.zip
        ```
        
        </div>
        
        </div>
        
        <div class="container">
        
        </div>
        
        confluence-information-macro confluence-information-macro-tip
        
        > 
        > 
        > <div class="container confluence-information-macro-body">
        > 
        > Ensure you have already installed `unzip`, `curl`, and Unravel
        > port \#`3000` must be open
        > 
        > </div>
    
      - Tar up the `/usr/local/unravel-agent` directory, and propagate
        to all the servers in the HDP cluster
        
        <div class="container code panel pdl">
        
        <div class="container codeContent panelContent pdl">
        
        ``` sourceCode syntaxhighlighter-pre
        # cd /usr/local/
        # tar -cvf unravel-agent.tar  ./unravel-agent
        ```
        
        </div>
        
        </div>
    
      - Copy the `unravel-agent.tar` file to all your servers, and
        `untar` to `/usr/local` directory because when
        you `untar` `unravel-agent` directory will appear

**9. Optionally for YARN**

  - If `yarn.acl.enable=true` then you need to grant Unravel access and
    must set one of the following:
      - `yarn.acl.enable=false,`or
      - `yarn.admin.acl=userName`  (set to \* to allow access to
        everyone).

**10. Confirm that Unravel Web UI Shows Tez Data**

  - Run `/usr/local/unravel/install_bin/hive_test_simple.sh` on HDP
    cluster or any cloud environment where `hive.execution.engine=tez`.

  - Check Unravel Web UI for Tez data. For instructions, see[Tez
    Application
    Manager](https://unraveldata.atlassian.net/wiki/spaces/UN44/pages/541164197/The+Applications+Page#TheApplicationsPage-TezAPMTezApplicationManager).
    
    <div class="container">
    
    </div>
    
    confluence-information-macro
    confluence-information-macro-information
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > Unravel Web UI may take a few seconds to load Tez data.
    > 
    > </div>

</div>

<div class="container pageSection group">

<div class="container pageSectionHeader">

**Attachments:**

</div>

<div class="container greybox">

![image0](images/icons/bullet_blue.gif)
[bulk.png](attachments/561709534/561709537.png) (image/png)
![image1](images/icons/bullet_blue.gif)
[hdp\_part2\_image\_002.PNG](attachments/561709534/561709540.png)
(image/png) ![image2](images/icons/bullet_blue.gif)
[hdp\_part2\_image\_001.PNG](attachments/561709534/561709543.png)
(image/png) ![image3](images/icons/bullet_blue.gif)
[image2017-10-25\_8-1-4.png](attachments/561709534/561709546.png)
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
