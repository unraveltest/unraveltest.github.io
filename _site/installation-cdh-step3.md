  - title  
    Step 3: Enable Impala APM

# Step 3: Enable Impala APM

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

**Unravel 4.4 : Step 3: Enable Impala APM**

</div>

<div id="content" class="container view">

<div class="container page-metadata">

Created by katch, last modified on Sep 10, 2018

</div>

<div id="main-content" class="container wiki-content group">

**Introduction**

This topic explains how to configure Unravel Server to retrieve Impala
query data from either ClouderaManager (CM) or Impala daemons
(`impalad`). It assumes that you already have installed Impala on your
CDH cluster.

Before following these instructions, follow the steps in [Step 1:
Install Unravel Server on
CDH+CM](https://unraveldata.atlassian.net/wiki/spaces/UN43/pages/226197657).

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> 
> 
> <div class="container confluence-information-macro-body">
> 
> By default, the ImpalaSensor task is enabled. If user wishes to
> disable ImpalaSensor, specify the following option
> in `/usr/local/unravel/etc/unravel.properties` on Unravel Server.
> 
> </div>

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
com.unraveldata.sensor.tasks.disabled=iw
```

</div>

</div>

<div class="container panel">

<div class="container panelHeader">

**Workflow Summary**

</div>

<div class="container panelContent">

1.  If you want Unravel Server to retrieve Impala query data from
    ClouderaManager, start at [Using CM as the Data
    Source](#Step3:EnableImpalaAPM-UsingCM).
2.  If you want Unravel Server to retrieve Impala query data from Impala
    daemons, start at [Using Impalad as the Data
    Source](#Step3:EnableImpalaAPM-UsingImpalad).

</div>

</div>

**Using CM as the Data Source**

In order to this you need to add `com.unraveldata.data.source=cm`
in `/usr/local/unravel/etc/unravel.properties` on Unravel Server.
Further, you need to tell Unravel Server some information about your CM:
URL, port number, login credentials, and so on. You do this by adding
the following properties to `/usr/local/unravel/etc/unravel.properties`
on Unravel Server:

<div class="container table-wrap">

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Property</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>com.unraveldata.cloudera.manag</code></pre>
er.url</td>
<td>CM internal URL. Must start with <code>http://</code> or<code> https://</code></td>
</tr>
<tr class="even">
<td><pre><code>com.unraveldata.cloudera.manag</code></pre>
er.port</td>
<td>(Optional) CM port number. You only need to specify this if your Cloudera Manager is not on port 7180.</td>
</tr>
<tr class="odd">
<td><pre><code>com.unraveldata.cloudera.manag</code></pre>
er.username</td>
<td>CM username</td>
</tr>
<tr class="even">
<td><pre><code>com.unraveldata.cloudera.manag</code></pre>
er.password</td>
<td>CM password</td>
</tr>
</tbody>
</table>

</div>

For example:

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
com.unraveldata.data.source=cm
com.unraveldata.cloudera.manager.url=http://mycm.somewhere.secret
com.unraveldata.cloudera.manager.port=9997
com.unraveldata.cloudera.manager.username=mycmname
com.unraveldata.cloudera.manager.password=mycmpassword
com.unraveldata.cloudera.manager.version=5_7_0
com.unraveldata.cloudera.manager.cluster.name.list=cluster1,cluster2,cluster5
com.unraveldata.cloudera.manager.service.impala.name=myimpalaservicename
```

</div>

</div>

<div class="container">

</div>

confluence-information-macro confluence-information-macro-tip

> Hints:
> 
> <div class="container confluence-information-macro-body">
> 
> To find out the cluster name:
> 
> <div class="container code panel pdl">
> 
> <div class="container codeContent panelContent pdl">
> 
> ``` sourceCode syntaxhighlighter-pre
> {cm_url}:{cm_port}/api/v13/clusters/
> ```
> 
> </div>
> 
> </div>
> 
> To find out the service name:
> 
> <div class="container code panel pdl">
> 
> <div class="container codeContent panelContent pdl">
> 
> ``` sourceCode syntaxhighlighter-pre
> {cm_url}:{cm_port}/api/v13/clusters/{cluster_name}/services/
> ```
> 
> </div>
> 
> </div>
> 
> Substitute your particular values for bracketed ClouderaManager (CM)
> properties, i.e., {cm\_port}
> 
> </div>

**Using Impalad as the Data Source**

Use this option if you want to import data from `impalad` daemon. This
option requires no CM credentials. Note that if the `impalad` web
interface is secure as indicated in
<https://www.cloudera.com/documentation/enterprise/5-2-x/topics/impala_security_webui.html>
and
<https://www.cloudera.com/documentation/enterprise/5-3-x/topics/impala_webui.html>,
then you need to use [CM as the data
source](#Step3:EnableImpalaAPM-UsingCM).

In order to use impalad as the data source you need to
add `com.unraveldata.data.source=impalad`
in `/usr/local/unravel/etc/unravel.properties` on Unravel Server.
Further, you need to tell Unravel Server some information about the
`impalad` nodes:

<div class="container table-wrap">

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Property</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>com.unraveldata.data.source</code></pre></td>
<td>Set this to <code>impalad</code>.</td>
</tr>
<tr class="even">
<td><pre><code>com.unraveldata.impalad.nodes</code></pre></td>
<td>A comma-separated list of <code>impalad</code> node IPs and ports (format: <code>IP:port,IP:port,IP:port</code>).</td>
</tr>
</tbody>
</table>

</div>

For example:

<div class="container code panel pdl">

<div class="container codeContent panelContent pdl">

``` sourceCode syntaxhighlighter-pre
com.unraveldata.data.source=impalad
com.unraveldata.impalad.nodes=IP:port,IP:port,IP:port
```

</div>

</div>

**References**

[https://www.cloudera.com/documentation/cdh/5-1-x/Impala/Installing-and-Using-Impala/ciiu\_install.html](http://www.cloudera.com/documentation/cdh/5-1-x/Impala/Installing-and-Using-Impala/ciiu_install.html)

<https://www.cloudera.com/documentation/enterprise/5-2-x/topics/impala_security_webui.html>

<https://www.cloudera.com/documentation/enterprise/5-3-x/topics/impala_webui.html>

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
