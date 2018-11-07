  - title  
    Step 2: Connect Unravel EC2 node to new or existing EMR clusters

# Step 2: Connect Unravel EC2 node to new or existing EMR clusters

<div id="page" class="container">

<div id="main" class="container aui-page-panel">

<div id="main-header" class="container">

<div id="breadcrumb-section" class="container">

1.  [Unravel 4.4](index.html)
2.  [Unravel 4.4](Unravel-4.4_541197025.html)
3.  [Installation Guides](Installation-Guides_541393730.html)
4.  [Amazon Elastic MapReduce (Amazon EMR)](591528087.html)

</div>

**Unravel 4.4 : Step 2: Connect Unravel EC2 node to new or existing EMR
clusters**

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

<div class="container toc-macro rbtoc1541196940639">

  - [Introduction](#Step2:ConnectUnravelEC2nodetoneworexistingEMRclusters-Introduction)
  - [Connect Unravel EC2 node to New EMR
    cluster](#Step2:ConnectUnravelEC2nodetoneworexistingEMRclusters-ConnectUnravelEC2nodetoNewEMRcluster)
  - [Connect Unravel EC2 node to Existing EMR
    cluster](#Step2:ConnectUnravelEC2nodetoneworexistingEMRclusters-ConnectUnravelEC2nodetoExistingEMRcluster)

</div>

</div>

</div>

**Introduction**

This topic explains how to setup and configure your EMR cluster so that
Unravel EC2 node can start monitoring your running jobs on the connected
EMR clusters. In order for Unravel EC2 node able to monitor the EMR
cluster, the cluster nodes are required to run the Unravel EMR bootstrap
script. The bootstrap script performs the following:

On the Master Node:

  - If a hive cluster, updates `hive-site.xml` in `/etc/hive/conf/`.
  - if a spark cluster, updates `spark-defaults.conf`
    in `/etc/spark/conf/`.
  - Updates `mapred-site.xml` in `/etc/hadoop/conf/`.
  - Updates`  ``yarn-site.xml` in`  ``/etc/hadoop/conf/`.
  - If TEZ is installed, updates `tez-site.xml` in `/etc/tez/conf/`.
  - Installs and starts the `unravel_es daemon`
    in `/usr/local/unravel_es`.
  - Installs the Spark & MapReduce sensors
    in `/usr/local/unravel-agent`.
  - Installs the Hive hook sensor in `/usr/lib/hive/lib/`.

On all other nodes:

  - Installs the Spark & MapReduce sensors in `/usr/local/unravel-agent`
    folder.

<div class="container">

</div>

confluence-information-macro confluence-information-macro-note

> Assumption
> 
> <div class="container confluence-information-macro-body">
> 
>   - Unravel node is created and unravel daemon running, tcp ports 3000
>     and 4043 are allowed for EMR cluster nodes.
>   - EMR cluster nodes allow all traffic from Unravel node*. Both EMR
>     cluster and Unravel node are created in same VPC, same subnet and
>     SG is allowing all traffic from same subnet.*
>   - For existing an [EMR cluster connection located on a different
>     VPC](591364250.html), it is required to do vpc peering, route
>     table creation, and security policy update.
> 
> </div>

**Connect Unravel EC2 node to New EMR cluster**

1.  Download Unravel EMR bootstrap python script from
    <https://s3.amazonaws.com/unraveldatarepo/unravel_emr_bootstrap.py>.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # curl  https://s3.amazonaws.com/unraveldatarepo/unravel_emr_bootstrap.py  -o /tmp/unravel_emr_bootstrap.py
    ```
    
    </div>
    
    </div>

2.  Upload the Unravel EMR bootstrap script `unravel_emr_bootstrap.py`
    to a S3 bucket that the EMR cluster can access for bootstrap action.
    You can upload the unravel EMR bootstrap script to the default EMR
    logging S3 bucket. For example your EMR cluster is using a S3 bucket
    for logging and has READ/WRITE access, e.g.,
    s3://aws-logs-for-EMR/elasticmapreduce. 
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # aws s3 cp unravel_emr_bootstrap.py s3://aws-logs-account_number-region/elasticmapreduce
    ```
    
    </div>
    
    </div>

3.  In the AWS console, choose EMR service and click Create cluster
    button.

4.  In the **Create Cluster - Quick Options** screen, click Go to
    advanced options**.**

5.  On the **Advanced Options** screen, select **Step 1: Software and
    Steps**.  Choose EMR release **emr-5.14.0** for the **Release**
    version and select all applications that fit your needs. Click
    **Next.**

6.  In **Hardware Configuration** (**Advanced Options - Step 2**)
    screen, for **Network** and **EC2 Subnet** choose the VPC and subnet
    that is used for EMR. The selected subnet's associated security
    group should have access to Unravel EC2 node. Modify the instance
    type and enter the desired instance count for Core (Slave) node(s).
    Click **Next.**

7.  In **General Options **(**Advanced Options - Step 3**), fill in the
    **Cluster name** and choose the **S3** **folder** (bucket) for your
    Logging (log archive). In the **Add bootstrap action** pull-down
    select **Custom action**. Click **Configure and add**.

8.  In the **Add Bootstrap Action** pop-up, enter the **Script
    location** and **Optional** **arguments**. Click
    Add.
    
    <div class="container">
    
    <div class="container table-wrap">
    
    |                         |                                                                                   |
    | ----------------------- | --------------------------------------------------------------------------------- |
    | Example Script location | s3://aws-logs-217619106665-us-east-1/elasticmapreduc e/unravel\_emr\_bootstrap.py |
    | Optional arguments      | \--unravel-server`  ``UNRAVEL_EC2_IP` --bootstrap                                 |
    

    </div>
    
    </div>
    
    <div class="container">
    
    </div>

9.  In the **Bootstrap Actions** click **Nex**t to continue and return
    to **Advanced Options** screen.\*\* \*\*

10. In **Security Options** screen (**Advanced Options - Step 3)**,
    choose the **EC2 key pair**. Select the **EC2 security groups**.
    Click **Create cluster**. You can pick the security group that is
    used for Unravel EC2 node. AWS EMR service automatically applies
    additional rules that are required for EMR nodes. Below, the
    security group picked for both **Maste**r and **Core & Task** nodes
    have rules allowing all traffic access from Unravel EC2 node.\*\*
    \*\*

11. If everything was entered correctly; your new EMR cluster should
    finish the bootstrap process and be in the **Waiting** state.\*\*
    \*\*

**Connect Unravel EC2 node to Existing EMR cluster**

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> 
> 
> <div class="container confluence-information-macro-body">
> 
> Substitute your local values for text in `{red brackets}`.
> 
> </div>

1.  Locate the public IP address of Master and Core
    nodes.
    
    <div id="expander-2070272269" class="container expand-container">
    
    <div id="expander-control-2070272269" class="container expand-control">
    
    Locate the nodes using the Amazon EMR
    console.
    
    </div>
    
    <div id="expander-content-2070272269" class="container expand-content">
    
    **The Master Node**
    
    **The Core Nodes**  
    \*
    \*
    
    </div>
    
    </div>
    
    <div id="expander-1685115879" class="container expand-container">
    
    <div id="expander-control-1685115879" class="container expand-control">
    
    Locate the nodes using the command line interface
    (CLI).
    
    </div>
    
    <div id="expander-content-1685115879" class="container expand-content">
    
    Get the instance group ID from the EMR cluster ID,
    `{emr_cluster_id}`. The output should be two instance group
    IDs.\*
    
      - 
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # aws emr describe-cluster --cluster-id j-{emr_cluster_id} |grep Id |grep ig
    ```
    
    </div>
    
    </div>
    
    Using the two instance groups ID from above, `{instance_group_id_1}`
    and `{instance_group_id_2}`, obtain the public IP
    addresses.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # aws emr list-instances --cluster-id j-{emr_cluster_id} --instance-group-id ig-{instance_group_id_1} |grep PublicIpAddress
    # aws emr list-instances --cluster-id j-{emr_cluster_id} --instance-group-id ig-{instance_group_id_2} |grep PublicIpAddress
    ```
    
    </div>
    
    </div>
    
    Example output:
    
    </div>
    
    </div>

2.  `ssh` to each EMR node and download Unravel EMR bootstrap python
    script (stored in public READ accessible S3 bucket) into the local
    `/tmp `folder.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # curl https://s3.amazonaws.com/unraveldatarepo/unravel_emr_bootstrap.py -o /tmp/unravel_emr_bootstrap.py
    ```
    
    </div>
    
    </div>
    
    <div class="container">
    
    </div>

3.  Run the Unravel EMR bootstrap script on each EMR cluster node,
    Master and Core & Task nodes`. `Substitute your Unravel Server
    Host's IP  for 
    {`UNRAVEL_SERVER`}.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo puthon /tmp/unravel_emr_bootstrippy --unravel-server {UNRAVEL_SERVER}
    ```
    
    </div>
    
    </div>
    
    <div id="expander-1919255795" class="container expand-container">
    
    <div id="expander-control-1919255795" class="container expand-control">
    
    Example script output from Master
    node.
    
    </div>
    
    <div id="expander-content-1919255795" class="container expand-content">
    
    </div>
    
    </div>
    
    <div id="expander-321432731" class="container expand-container">
    
    <div id="expander-control-321432731" class="container expand-control">
    
    Example script output from EMR Core and Task
    node.
    
    </div>
    
    <div id="expander-content-321432731" class="container expand-content">
    
    </div>
    
    </div>

Once Unravel EMR bootstrap script has been run on all EMR cluster nodes,
the unravel setting should be configured on the cluster. You can run
some jobs on this EMR cluster and monitor the metric data displayed on
the Unravel UI (<http://unravel_ec2_node_public_IP:3000>).

If the EMR cluster is created in a different VPC, see [Step 4 for
configuring VPC peering connection for an Unravel
node.](591364250.html) 

</div>

<div class="container pageSection group">

<div class="container pageSectionHeader">

**Attachments:**

</div>

<div class="container greybox">

![image0](images/icons/bullet_blue.gif)
[image2018-9-14\_17-49-36.png](attachments/591298673/591298676.png)
(image/png) ![image1](images/icons/bullet_blue.gif)
[image2018-9-14\_17-42-53.png](attachments/591298673/591298679.png)
(image/png) ![image2](images/icons/bullet_blue.gif)
[image2018-9-14\_17-40-16.png](attachments/591298673/591298682.png)
(image/png) ![image3](images/icons/bullet_blue.gif)
[image2018-9-14\_17-36-59.png](attachments/591298673/591298685.png)
(image/png) ![image4](images/icons/bullet_blue.gif)
[image2018-9-14\_17-14-37.png](attachments/591298673/591298688.png)
(image/png) ![image5](images/icons/bullet_blue.gif)
[image2018-9-14\_16-59-36.png](attachments/591298673/591298691.png)
(image/png) ![image6](images/icons/bullet_blue.gif)
[image2018-9-14\_16-57-32.png](attachments/591298673/591298694.png)
(image/png) ![image7](images/icons/bullet_blue.gif)
[image2018-8-24\_18-55-52.png](attachments/591298673/591298697.png)
(image/png) ![image8](images/icons/bullet_blue.gif)
[image2018-8-24\_18-38-12.png](attachments/591298673/591298700.png)
(image/png) ![image9](images/icons/bullet_blue.gif)
[image2018-8-24\_18-31-8.png](attachments/591298673/591298703.png)
(image/png) ![image10](images/icons/bullet_blue.gif)
[image2018-8-24\_18-24-38.png](attachments/591298673/591298706.png)
(image/png) ![image11](images/icons/bullet_blue.gif)
[image2018-8-24\_17-46-45.png](attachments/591298673/591298709.png)
(image/png) ![image12](images/icons/bullet_blue.gif)
[image2018-8-24\_17-34-16.png](attachments/591298673/591298712.png)
(image/png) ![image13](images/icons/bullet_blue.gif)
[image2018-8-24\_17-0-48.png](attachments/591298673/591298715.png)
(image/png) ![image14](images/icons/bullet_blue.gif)
[image2018-8-24\_16-56-43.png](attachments/591298673/591298718.png)
(image/png) ![image15](images/icons/bullet_blue.gif)
[image2018-8-24\_16-55-50.png](attachments/591298673/591298721.png)
(image/png) ![image16](images/icons/bullet_blue.gif)
[image2018-8-24\_16-53-24.png](attachments/591298673/591298724.png)
(image/png) ![image17](images/icons/bullet_blue.gif)
[image2018-8-24\_16-49-35.png](attachments/591298673/591298727.png)
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
