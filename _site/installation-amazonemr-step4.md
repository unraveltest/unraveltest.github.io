  - title  
    Step 4: VPC Peering for Unravel EC2

# Step 4: VPC Peering for Unravel EC2

<div id="page" class="container">

<div id="main" class="container aui-page-panel">

<div id="main-header" class="container">

<div id="breadcrumb-section" class="container">

1.  [Unravel 4.4](index.html)
2.  [Unravel 4.4](Unravel-4.4_541197025.html)
3.  [Installation Guides](Installation-Guides_541393730.html)
4.  [Amazon Elastic MapReduce (Amazon EMR)](591528087.html)

</div>

**Unravel 4.4 : Step 4: VPC Peering for Unravel EC2 node (Optional)**

</div>

<div id="content" class="container view">

<div class="container page-metadata">

Created by katch, last modified on Sep 22, 2018

</div>

<div id="main-content" class="container wiki-content group">

<div class="container panel">

<div class="container panelHeader">

**Table of
    Contents**

</div>

<div class="container panelContent">

<div class="container toc-macro rbtoc1541196951144">

  - [Introduction](#Step4:VPCPeeringforUnravelEC2node\(Optional\)-Introduction)
  - [Create VPC Peering in VPC
    Dashboard ](#Step4:VPCPeeringforUnravelEC2node\(Optional\)-CreateVPCPeeringinVPCDashboard)
  - [Accept the VPC Peering
    Request](#Step4:VPCPeeringforUnravelEC2node\(Optional\)-AccepttheVPCPeeringRequest)
  - [Create Routes between peered
    VPC](#Step4:VPCPeeringforUnravelEC2node\(Optional\)-CreateRoutesbetweenpeeredVPC)
  - [Update Security
    Groups](#Step4:VPCPeeringforUnravelEC2node\(Optional\)-UpdateSecurityGroups)
  - [Verify Connection between Unravel EC2 node and EMR master
    node](#Step4:VPCPeeringforUnravelEC2node\(Optional\)-VerifyConnectionbetweenUnravelEC2nodeandEMRmasternode)

</div>

</div>

</div>

<div class="container">

</div>

confluence-information-macro confluence-information-macro-information

> 
> 
> <div class="container confluence-information-macro-body">
> 
> Follow these steps **only** if you have an Amazon EMR cluster located
> in VPC different than the one in the Unravel EC2 node.
> 
> </div>

**Introduction**

This topic explains how to resolve connectivity issues when Unravel EC2
node is created in a VPC different than where the EMR cluster located.
This documentation also applies for Unravel EC2 node connecting to RDS
created on different VPC of the same region.

<div class="container panel">

<div class="container panelHeader">

**Assumptions**

</div>

<div class="container panelContent">

1.  The VPC where Unravel EC2 located is in the same region where the
    EMR cluster located (e.g., us-east-1).
2.  The subnet used by Unravel EC2 does not overlap the IP block range
    of the subnet used in EMR cluster.
3.  Network ACL on both VPC for Unravel EC2 and EMR cluster are the
    default and allow all traffic. The security group is the only
    security enforcement on network access.

</div>

</div>

In the following steps, we have both Unravel EC2 node and EMR cluster
located in us-east-1 region but configured with different VPC and
subnet. There is no network access allowed between Unravel EC2 and EMR
cluster by default.

<div class="container table-wrap">

<table style="width:94%;">
<colgroup>
<col style="width: 9%" />
<col style="width: 8%" />
<col style="width: 11%" />
<col style="width: 6%" />
<col style="width: 16%" />
<col style="width: 5%" />
<col style="width: 36%" />
</colgroup>
<thead>
<tr class="header">
<th>Reso urce s</th>
<th>Int ern al IP Add res s</th>
<th>Subne t ID</th>
<th>Su bn et IP Bl oc k</th>
<th>VPC ID (Name)</th>
<th>I P b l o c k i n V P C</th>
<th>Security Group ID  (Name)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Unra vel EC2 node</td>
<td>10. 10. 0.7</td>
<td>subne t-03b 82c56 b2c26 dbd1</td>
<td>10 .1 0. 0. 0/ 24</td>
<td>vpc-0b0e1 7b01c4a3b 54a (<a href="">Unravel</a> VPC)</td>
<td>1 0 . 1 0 . 0 . 0 / 1 6</td>
<td>sg-0e0a03084398287c9 (U nravel-EC2_SG)</td>
</tr>
<tr class="even">
<td>EMR Clus ter Mast er node</td>
<td>10. 11. 0.5 3</td>
<td>subne t-029 4cc17 a42a9 acfd</td>
<td>10 .1 1. 0. 0/ 24</td>
<td>vpc-c3d07 9a4 (<a href="">VPC_for</a> VPC Peering)</td>
<td>1 0 . 1 1 . 0 . 0 / 1 6</td>
<td>sg-0a73c3aea9340ae49 (E MR_VPC_SG)</td>
</tr>
<tr class="odd">
<td>EMR Clus ter Core node s</td>
<td><p>10. 11. 0.7 6</p>
<p>10. 11. 0.1 30</p></td>
<td>subne t-029 4cc17 a42a9 acfd</td>
<td>10 .1 1. 0. 0/ 24</td>
<td>vpc-c3d07 9a4 (<a href="">VPC</a> for_VPC Peering)</td>
<td>1 0 . 1 1 . 0 . 0 / 1 6</td>
<td>sg-0a73c3aea9340ae49 (E MR_VPC_SG)</td>
</tr>
</tbody>
</table>

</div>

**Create VPC Peering in VPC Dashboard **

From the **AWS console → VPC services → Peering Connections**, click the
**Create Peering Connection**, and enter the name tag, e.g.,
"EMR\_to\_Unravel". **VPC (Requester)** field choose VPC of EMR cluster,
and **VPC (Accepter)** field choose VPC of Unravel node. Click **Create
Peering Connection.**

A success message should appear in the screen. Click **OK**.

**Accept the VPC Peering Request**

1.  In the **VPC Dashboard**, the new VPC peering connection will have a
    Status showing **Pending Acceptance**. Select this connection and
    click **Action** and select **Accept Request**.
2.  Click **Yes Accept** in the prompt screen. You will see a message
    regarding "Modify my route tables". Click **Close**.

**Create Routes between peered VPC**

Go to **VPC Dashboard → Route Tables** to create the routes between VPCs
where Unravel node located (Unravel\_VPC) and the EMR cluster is located
(Test\_EMR\_VPC). After locating each route table: Click **Edit** and
then **Add another route**.

1.  Find the Unravel\_VPC route table. Enter the IP block of EMR VPC,
    e.g., 10.11.0.0/16 In the **Destination** field. In the **Target**
    field choose the VPC peer connection ID, e.g.,
    pcx-0a57a978ef9a525e2. Click **Save.**
2.  Find the Test\_EMR\_VPC route table. Set the **Destination** to IP
    block of Unravel\_VPC, e.g., 10.10.0.0/16. In the **Target** field
    choose the VPC peer connection ID, e.g., pcx-0a57a978ef9a525e2.
    Click **Save.**Choose connection ID (e.g. *pcx-0a57a978ef9a525e2*)
    in the **Target** field. Click **Save**.

**Update Security Groups**

Go to **VPC Dashboard → Security Group**. After locating each security
group: Click **Add another rule**. Set **Type** to inbound **ALL
traffic** and **Protocol** to **ALL.**

1.  Locate the security group used on Unravel EC2 node. Enter the EMR
    VPC IP block, e.g., 10.11.0.0/16 in the  **Source** field. Click
    **Save.**
2.  Locate the security group used on EMR cluster node. Enter the
    Unravel VPC IP block, e.g., 10.10.0.0/16 in the **Source** field.
    Click **Save**.\*\* \*\*

**Verify Connection between Unravel EC2 node and EMR master node**

1.  `ssh` to both Unravel EC2 nodeand EMR master node. Since the above
    example allowed **ALL Traffic** from both VPC IP blocks, so we
    should be able to ping the IP address of EMR master node from
    Unravel EC2 node.
2.  Then on Unravel EC2 node, `telnet` to EMR master node port 8082
    (namenode port)\*\* \*\*
3.  On the EMR master node, `telnet `to Unravel EC2 node port 3000 and
    port 4043**.**

If telnet port tests are positive, the VPC peering connection is setup
correctly. If not, troubleshoot the configuration on network acl,
security groups, and route tables used on both VPC.

[See unsupported VPC peering configurations on AWS
documentation.](https://docs.aws.amazon.com/vpc/latest/peering/invalid-peering-configurations.html)

</div>

<div class="container pageSection group">

<div class="container pageSectionHeader">

**Attachments:**

</div>

<div class="container greybox">

![image0](images/icons/bullet_blue.gif)
[image2018-9-14\_15-51-49.png](attachments/591364250/591364253.png)
(image/png) ![image1](images/icons/bullet_blue.gif)
[image2018-9-14\_15-41-49.png](attachments/591364250/591364256.png)
(image/png) ![image2](images/icons/bullet_blue.gif)
[image2018-9-14\_15-40-24.png](attachments/591364250/591364259.png)
(image/png) ![image3](images/icons/bullet_blue.gif)
[image2018-9-14\_15-38-50.png](attachments/591364250/591364262.png)
(image/png) ![image4](images/icons/bullet_blue.gif)
[image2018-9-14\_15-34-33.png](attachments/591364250/591364265.png)
(image/png) ![image5](images/icons/bullet_blue.gif)
[image2018-9-14\_15-29-1.png](attachments/591364250/591364268.png)
(image/png) ![image6](images/icons/bullet_blue.gif)
[image2018-9-14\_15-21-53.png](attachments/591364250/591364271.png)
(image/png) ![image7](images/icons/bullet_blue.gif)
[image2018-9-14\_15-11-26.png](attachments/591364250/591364274.png)
(image/png) ![image8](images/icons/bullet_blue.gif)
[image2018-9-14\_15-5-57.png](attachments/591364250/591364277.png)
(image/png) ![image9](images/icons/bullet_blue.gif)
[image2018-9-14\_15-1-59.png](attachments/591364250/591364280.png)
(image/png) ![image10](images/icons/bullet_blue.gif)
[image2018-9-14\_15-0-54.png](attachments/591364250/591364283.png)
(image/png) ![image11](images/icons/bullet_blue.gif)
[image2018-9-14\_14-59-11.png](attachments/591364250/591364286.png)
(image/png) ![image12](images/icons/bullet_blue.gif)
[image2018-9-14\_14-58-41.png](attachments/591364250/591364289.png)
(image/png) ![image13](images/icons/bullet_blue.gif)
[image2018-9-13\_16-39-56.png](attachments/591364250/591364292.png)
(image/png) ![image14](images/icons/bullet_blue.gif)
[image2018-9-13\_16-38-20.png](attachments/591364250/591364295.png)
(image/png) ![image15](images/icons/bullet_blue.gif)
[image2018-9-13\_16-37-34.png](attachments/591364250/591364298.png)
(image/png) ![image16](images/icons/bullet_blue.gif)
[image2018-9-13\_15-51-15.png](attachments/591364250/591364301.png)
(image/png) ![image17](images/icons/bullet_blue.gif)
[image2018-9-13\_15-33-48.png](attachments/591364250/591364304.png)
(image/png) ![image18](images/icons/bullet_blue.gif)
[image2018-9-13\_15-29-39.png](attachments/591364250/591364307.png)
(image/png) ![image19](images/icons/bullet_blue.gif)
[image2018-9-13\_15-28-39.png](attachments/591364250/591364310.png)
(image/png) ![image20](images/icons/bullet_blue.gif)
[worddav17bd11fa4b44a52a180fec44ac2a622c.png](attachments/591364250/591364313.png)
(image/png) ![image21](images/icons/bullet_blue.gif)
[FileFolder.png](attachments/591364250/591364316.png) (image/png)
![image22](images/icons/bullet_blue.gif)
[WX20180625-104211.png](attachments/591364250/591364319.png) (image/png)

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
