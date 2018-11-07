:title: Step 4: VPC Peering for Unravel EC2

Step 4: VPC Peering for Unravel EC2
====================================

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

         .. rubric:: Unravel 4.4 : Step 4: VPC Peering for Unravel EC2
            node (Optional)
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified on Sep 22, 2018

         .. container:: wiki-content group
            :name: main-content

            .. container:: panel

               .. container:: panelHeader

                  **Table of Contents**

               .. container:: panelContent

                  .. container:: toc-macro rbtoc1541196951144

                     -  `Introduction <#Step4:VPCPeeringforUnravelEC2node(Optional)-Introduction>`__
                     -  `Create VPC Peering in VPC
                        Dashboard  <#Step4:VPCPeeringforUnravelEC2node(Optional)-CreateVPCPeeringinVPCDashboard>`__
                     -  `Accept the VPC Peering
                        Request <#Step4:VPCPeeringforUnravelEC2node(Optional)-AccepttheVPCPeeringRequest>`__
                     -  `Create Routes between peered
                        VPC <#Step4:VPCPeeringforUnravelEC2node(Optional)-CreateRoutesbetweenpeeredVPC>`__
                     -  `Update Security
                        Groups <#Step4:VPCPeeringforUnravelEC2node(Optional)-UpdateSecurityGroups>`__
                     -  `Verify Connection between Unravel EC2 node and
                        EMR master
                        node <#Step4:VPCPeeringforUnravelEC2node(Optional)-VerifyConnectionbetweenUnravelEC2nodeandEMRmasternode>`__

            .. container::
            confluence-information-macro confluence-information-macro-information

               .. container:: confluence-information-macro-body

                  Follow these steps **only** if you have an Amazon EMR
                  cluster located in VPC different than the one in the
                  Unravel EC2 node.

            .. rubric:: Introduction
               :name: Step4:VPCPeeringforUnravelEC2node(Optional)-Introduction

            This topic explains how to resolve connectivity issues when
            Unravel EC2 node is created in a VPC different than where
            the EMR cluster located. This documentation also applies for
            Unravel EC2 node connecting to RDS created on different VPC
            of the same region.

            .. container:: panel

               .. container:: panelHeader

                  **Assumptions**

               .. container:: panelContent

                  #. The VPC where Unravel EC2 located is in the same
                     region where the EMR cluster located (e.g.,
                     us-east-1).
                  #. The subnet used by Unravel EC2 does not overlap the
                     IP block range of the subnet used in EMR cluster.

                  #. Network ACL on both VPC for Unravel EC2 and EMR
                     cluster are the default and allow all traffic. The
                     security group is the only security enforcement on
                     network access.

            In the following steps, we have both Unravel EC2 node and
            EMR cluster located in us-east-1 region but configured with
            different VPC and subnet. There is no network access allowed
            between Unravel EC2 and EMR cluster by default.

            .. container:: table-wrap

               +------+-----+-------+----+-----------+---+-------------------------+
               | Reso | Int | Subne | Su | VPC ID    | I | Security Group ID       |
               | urce | ern | t     | bn | (Name)    | P | (Name)                  |
               | s    | al  | ID    | et |           | b |                         |
               |      | IP  |       | IP |           | l |                         |
               |      | Add |       | Bl |           | o |                         |
               |      | res |       | oc |           | c |                         |
               |      | s   |       | k  |           | k |                         |
               |      |     |       |    |           | i |                         |
               |      |     |       |    |           | n |                         |
               |      |     |       |    |           | V |                         |
               |      |     |       |    |           | P |                         |
               |      |     |       |    |           | C |                         |
               +======+=====+=======+====+===========+===+=========================+
               | Unra | 10. | subne | 10 | vpc-0b0e1 | 1 | sg-0e0a03084398287c9 (U |
               | vel  | 10. | t-03b | .1 | 7b01c4a3b | 0 | nravel-EC2_SG)          |
               | EC2  | 0.7 | 82c56 | 0. | 54a       | . |                         |
               | node |     | b2c26 | 0. | (Unravel_ | 1 |                         |
               |      |     | dbd1  | 0/ | VPC)      | 0 |                         |
               |      |     |       | 24 |           | . |                         |
               |      |     |       |    |           | 0 |                         |
               |      |     |       |    |           | . |                         |
               |      |     |       |    |           | 0 |                         |
               |      |     |       |    |           | / |                         |
               |      |     |       |    |           | 1 |                         |
               |      |     |       |    |           | 6 |                         |
               +------+-----+-------+----+-----------+---+-------------------------+
               | EMR  | 10. | subne | 10 | vpc-c3d07 | 1 | sg-0a73c3aea9340ae49 (E |
               | Clus | 11. | t-029 | .1 | 9a4       | 0 | MR_VPC_SG)              |
               | ter  | 0.5 | 4cc17 | 1. | (VPC_for_ | . |                         |
               | Mast | 3   | a42a9 | 0. | VPC       | 1 |                         |
               | er   |     | acfd  | 0/ | Peering)  | 1 |                         |
               | node |     |       | 24 |           | . |                         |
               |      |     |       |    |           | 0 |                         |
               |      |     |       |    |           | . |                         |
               |      |     |       |    |           | 0 |                         |
               |      |     |       |    |           | / |                         |
               |      |     |       |    |           | 1 |                         |
               |      |     |       |    |           | 6 |                         |
               +------+-----+-------+----+-----------+---+-------------------------+
               | EMR  | 10. | subne | 10 | vpc-c3d07 | 1 | sg-0a73c3aea9340ae49 (E |
               | Clus | 11. | t-029 | .1 | 9a4 (VPC_ | 0 | MR_VPC_SG)              |
               | ter  | 0.7 | 4cc17 | 1. | for_VPC   | . |                         |
               | Core | 6   | a42a9 | 0. | Peering)  | 1 |                         |
               | node |     | acfd  | 0/ |           | 1 |                         |
               | s    | 10. |       | 24 |           | . |                         |
               |      | 11. |       |    |           | 0 |                         |
               |      | 0.1 |       |    |           | . |                         |
               |      | 30  |       |    |           | 0 |                         |
               |      |     |       |    |           | / |                         |
               |      |     |       |    |           | 1 |                         |
               |      |     |       |    |           | 6 |                         |
               +------+-----+-------+----+-----------+---+-------------------------+

            .. rubric:: Create VPC Peering in VPC Dashboard 
               :name: Step4:VPCPeeringforUnravelEC2node(Optional)-CreateVPCPeeringinVPCDashboard

            From the **AWS console → VPC services → Peering
            Connections**, click the **Create Peering Connection**, and
            enter the name tag, e.g., "EMR_to_Unravel". **VPC
            (Requester)** field choose VPC of EMR cluster, and **VPC
            (Accepter)** field choose VPC of Unravel node. Click
            **Create Peering Connection.**

            A success message should appear in the screen. Click **OK**.

            .. rubric:: Accept the VPC Peering Request
               :name: Step4:VPCPeeringforUnravelEC2node(Optional)-AccepttheVPCPeeringRequest

            #. In the **VPC Dashboard**, the new VPC peering connection
               will have a Status showing **Pending Acceptance**. Select
               this connection and click **Action** and select **Accept
               Request**.
            #. Click **Yes Accept** in the prompt screen. You will see a
               message regarding "Modify my route tables". Click
               **Close**.

            .. rubric:: Create Routes between peered VPC
               :name: Step4:VPCPeeringforUnravelEC2node(Optional)-CreateRoutesbetweenpeeredVPC

            Go to **VPC Dashboard → Route Tables** to create the routes
            between VPCs where Unravel node located (Unravel_VPC) and
            the EMR cluster is located (Test_EMR_VPC). After locating
            each route table: Click **Edit** and then **Add another
            route**.

            #. Find the Unravel_VPC route table. Enter the IP block of
               EMR VPC, e.g., 10.11.0.0/16 In the **Destination** field.
               In the **Target** field choose the VPC peer connection
               ID, e.g., pcx-0a57a978ef9a525e2. Click **Save.
               **
            #. Find the Test_EMR_VPC route table. Set the
               **Destination** to IP block of Unravel_VPC, e.g.,
               10.10.0.0/16. In the **Target** field choose the VPC peer
               connection ID, e.g., pcx-0a57a978ef9a525e2. Click
               **Save.**\ Choose connection ID (e.g.
               *pcx-0a57a978ef9a525e2*) in the **Target** field. Click
               **Save**.

            .. rubric:: Update Security Groups
               :name: Step4:VPCPeeringforUnravelEC2node(Optional)-UpdateSecurityGroups
               :class: auto-cursor-target

            Go to **VPC Dashboard → Security Group**. After locating
            each security group: Click **Add another rule**. Set
            **Type** to inbound **ALL traffic** and **Protocol** to
            **ALL.**

            #. Locate the security group used on Unravel EC2 node. Enter
               the EMR VPC IP block, e.g., 10.11.0.0/16 in the 
               **Source** field. Click **Save.
               **
            #. Locate the security group used on EMR cluster node. Enter
               the Unravel VPC IP block, e.g., 10.10.0.0/16 in the
               **Source** field. Click **Save**.\ **
               **

            .. rubric:: Verify Connection between Unravel EC2 node and
               EMR master node
               :name: Step4:VPCPeeringforUnravelEC2node(Optional)-VerifyConnectionbetweenUnravelEC2nodeandEMRmasternode
               :class: auto-cursor-target

            #. ``ssh`` to both Unravel EC2 nodeand EMR master node.
               Since the above example allowed **ALL Traffic** from both
               VPC IP blocks, so we should be able to ping the IP
               address of EMR master node from Unravel EC2 node.
            #. Then on Unravel EC2 node, ``telnet`` to EMR master node
               port 8082 (namenode port)\ **
               **
            #. On the EMR master node, ``telnet ``\ to Unravel EC2 node
               port 3000 and port 4043\ **.
               **

            If telnet port tests are positive, the VPC peering
            connection is setup correctly. If not, troubleshoot the
            configuration on network acl, security groups, and route
            tables used on both VPC.

            `See unsupported VPC peering configurations on AWS
            documentation. <https://docs.aws.amazon.com/vpc/latest/peering/invalid-peering-configurations.html>`__

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image0|
               `image2018-9-14_15-51-49.png <attachments/591364250/591364253.png>`__
               (image/png)
               |image1|
               `image2018-9-14_15-41-49.png <attachments/591364250/591364256.png>`__
               (image/png)
               |image2|
               `image2018-9-14_15-40-24.png <attachments/591364250/591364259.png>`__
               (image/png)
               |image3|
               `image2018-9-14_15-38-50.png <attachments/591364250/591364262.png>`__
               (image/png)
               |image4|
               `image2018-9-14_15-34-33.png <attachments/591364250/591364265.png>`__
               (image/png)
               |image5|
               `image2018-9-14_15-29-1.png <attachments/591364250/591364268.png>`__
               (image/png)
               |image6|
               `image2018-9-14_15-21-53.png <attachments/591364250/591364271.png>`__
               (image/png)
               |image7|
               `image2018-9-14_15-11-26.png <attachments/591364250/591364274.png>`__
               (image/png)
               |image8|
               `image2018-9-14_15-5-57.png <attachments/591364250/591364277.png>`__
               (image/png)
               |image9|
               `image2018-9-14_15-1-59.png <attachments/591364250/591364280.png>`__
               (image/png)
               |image10|
               `image2018-9-14_15-0-54.png <attachments/591364250/591364283.png>`__
               (image/png)
               |image11|
               `image2018-9-14_14-59-11.png <attachments/591364250/591364286.png>`__
               (image/png)
               |image12|
               `image2018-9-14_14-58-41.png <attachments/591364250/591364289.png>`__
               (image/png)
               |image13|
               `image2018-9-13_16-39-56.png <attachments/591364250/591364292.png>`__
               (image/png)
               |image14|
               `image2018-9-13_16-38-20.png <attachments/591364250/591364295.png>`__
               (image/png)
               |image15|
               `image2018-9-13_16-37-34.png <attachments/591364250/591364298.png>`__
               (image/png)
               |image16|
               `image2018-9-13_15-51-15.png <attachments/591364250/591364301.png>`__
               (image/png)
               |image17|
               `image2018-9-13_15-33-48.png <attachments/591364250/591364304.png>`__
               (image/png)
               |image18|
               `image2018-9-13_15-29-39.png <attachments/591364250/591364307.png>`__
               (image/png)
               |image19|
               `image2018-9-13_15-28-39.png <attachments/591364250/591364310.png>`__
               (image/png)
               |image20|
               `worddav17bd11fa4b44a52a180fec44ac2a622c.png <attachments/591364250/591364313.png>`__
               (image/png)
               |image21|
               `FileFolder.png <attachments/591364250/591364316.png>`__
               (image/png)
               |image22|
               `WX20180625-104211.png <attachments/591364250/591364319.png>`__
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
.. |image17| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image18| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image19| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image20| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image21| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image22| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
