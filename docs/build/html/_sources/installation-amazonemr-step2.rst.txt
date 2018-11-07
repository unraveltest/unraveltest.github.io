:title: Step 2: Connect Unravel EC2 node to new or existing EMR clusters

Step 2: Connect Unravel EC2 node to new or existing EMR clusters
=================================================================

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

         .. rubric:: Unravel 4.4 : Step 2: Connect Unravel EC2 node to
            new or existing EMR clusters
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

                  .. container:: toc-macro rbtoc1541196940639

                     -  `Introduction <#Step2:ConnectUnravelEC2nodetoneworexistingEMRclusters-Introduction>`__
                     -  `Connect Unravel EC2 node to New EMR
                        cluster <#Step2:ConnectUnravelEC2nodetoneworexistingEMRclusters-ConnectUnravelEC2nodetoNewEMRcluster>`__
                     -  `Connect Unravel EC2 node to Existing EMR
                        cluster <#Step2:ConnectUnravelEC2nodetoneworexistingEMRclusters-ConnectUnravelEC2nodetoExistingEMRcluster>`__

            .. rubric:: Introduction
               :name: Step2:ConnectUnravelEC2nodetoneworexistingEMRclusters-Introduction

            This topic explains how to setup and configure your EMR
            cluster so that Unravel EC2 node can start monitoring your
            running jobs on the connected EMR clusters. In order for
            Unravel EC2 node able to monitor the EMR cluster, the
            cluster nodes are required to run the Unravel EMR bootstrap
            script. The bootstrap script performs the following:

            On the Master Node:

            -  If a hive cluster, updates \ ``hive-site.xml`` in
               ``/etc/hive/conf/``.
            -  if a spark cluster, updates \ ``spark-defaults.conf``
               in \ ``/etc/spark/conf/``.
            -  Updates \ ``mapred-site.xml`` in \ ``/etc/hadoop/conf/``.
            -  Updates\ ````\ ``yarn-site.xml``
               in\ ````\ ``/etc/hadoop/conf/``.
            -  If TEZ is installed, updates \ ``tez-site.xml``
               in \ ``/etc/tez/conf/``.
            -  Installs and starts the \ ``unravel_es daemon``
               in \ ``/usr/local/unravel_es``.
            -  Installs the Spark & MapReduce sensors
               in \ ``/usr/local/unravel-agent``.
            -  Installs the Hive hook sensor
               in \ ``/usr/lib/hive/lib/``.

            On all other nodes:

            -  Installs the Spark & MapReduce sensors in
               ``/usr/local/unravel-agent`` folder.

            .. container::
            confluence-information-macro confluence-information-macro-note

               Assumption

               .. container:: confluence-information-macro-body

                  -  Unravel node is created and unravel daemon running,
                     tcp ports 3000 and 4043 are allowed for EMR cluster
                     nodes.
                  -  EMR cluster nodes allow all traffic from Unravel
                     node\ *. Both EMR cluster and Unravel node are
                     created in same VPC, same subnet and SG is allowing
                     all traffic from same subnet.*
                  -  For existing an `EMR cluster connection located on
                     a different VPC <591364250.html>`__, it is required
                     to do vpc peering, route table creation, and
                     security policy update.

            .. rubric:: Connect Unravel EC2 node to New EMR cluster
               :name: Step2:ConnectUnravelEC2nodetoneworexistingEMRclusters-ConnectUnravelEC2nodetoNewEMRcluster

            #. Download Unravel EMR bootstrap python script from
               https://s3.amazonaws.com/unraveldatarepo/unravel_emr_bootstrap.py.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # curl  https://s3.amazonaws.com/unraveldatarepo/unravel_emr_bootstrap.py  -o /tmp/unravel_emr_bootstrap.py

            #. Upload the Unravel EMR bootstrap script
               ``unravel_emr_bootstrap.py`` to a S3 bucket that the EMR
               cluster can access for bootstrap action. You can upload
               the unravel EMR bootstrap script to the default EMR
               logging S3 bucket. For example your EMR cluster is using
               a S3 bucket for logging and has READ/WRITE access, e.g.,
               s3://aws-logs-for-EMR/elasticmapreduce. 

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # aws s3 cp unravel_emr_bootstrap.py s3://aws-logs-account_number-region/elasticmapreduce

            #. In the AWS console, choose EMR service and click Create
               cluste\ r button.
            #. In the **Create Cluster - Quick Options** screen, click
               Go to advanced options\ **.
               **
            #. On the **Advanced Options** screen, select **Step 1:
               Software and Steps**.  Choose EMR release **emr-5.14.0**
               for the **Release** version and select all applications
               that fit your needs. Click **Next.
               **
            #. In **Hardware Configuration** (**Advanced Options - Step
               2**) screen, for **Network** and **EC2 Subnet** choose
               the VPC and subnet that is used for EMR. The selected
               subnet's associated security group should have access to
               Unravel EC2 node. Modify the instance type and enter the
               desired instance count for Core (Slave) node(s). Click
               **Next.
               **
            #. In **General Options **\ (**Advanced Options - Step 3**),
               fill in the **Cluster name** and choose the **S3**
               **folder** (bucket) for your Logging (log archive). In
               the **Add bootstrap action** pull-down select **Custom
               action**. Click **Configure and add**.
            #. In the **Add Bootstrap Action** pop-up, enter the
               **Script location** and **Optional** **arguments**. Click
               Add.

               .. container::

                  .. container:: table-wrap

                     +---------------+------------------------------------------------------+
                     | Example       | s3://aws-logs-217619106665-us-east-1/elasticmapreduc |
                     | Script        | e/unravel_emr_bootstrap.py                           |
                     | location      |                                                      |
                     +---------------+------------------------------------------------------+
                     | Optional      | --unravel-server````\ ``UNRAVEL_EC2_IP`` --bootstrap |
                     | arguments     |                                                      |
                     +---------------+------------------------------------------------------+

               .. container::

            #. In the **Bootstrap Actions** click **Nex**\ t to continue
               and return to **Advanced Options** screen.\ **
               **
            #. In **Security Options** screen (**Advanced Options - Step
               3)**, choose the **EC2 key pair**. Select the **EC2
               security groups**. Click **Create cluster**. You can pick
               the security group that is used for Unravel EC2 node. AWS
               EMR service automatically applies additional rules that
               are required for EMR nodes. Below, the security group
               picked for both **Maste**\ r and **Core & Task** nodes
               have rules allowing all traffic access from Unravel EC2
               node.\ **
               **
            #. If everything was entered correctly; your new EMR cluster
               should finish the bootstrap process and be in the
               **Waiting** state.\ **
               **

            .. rubric:: Connect Unravel EC2 node to Existing EMR cluster
               :name: Step2:ConnectUnravelEC2nodetoneworexistingEMRclusters-ConnectUnravelEC2nodetoExistingEMRcluster

            .. container::
            confluence-information-macro confluence-information-macro-information

               .. container:: confluence-information-macro-body

                  Substitute your local values for text
                  in \ ``{red brackets}``.

            #. Locate the public IP address of Master and Core nodes.

               .. container:: expand-container
                  :name: expander-2070272269

                  .. container:: expand-control
                     :name: expander-control-2070272269

                     Locate the nodes using the Amazon EMR console.

                  .. container:: expand-content
                     :name: expander-content-2070272269

                     **The Master Node**

                     | **The Core Nodes**
                     | *
                       *

               .. container:: expand-container
                  :name: expander-1685115879

                  .. container:: expand-control
                     :name: expander-control-1685115879

                     Locate the nodes using the command line interface
                     (CLI).

                  .. container:: expand-content
                     :name: expander-content-1685115879

                     Get the instance group ID from the EMR cluster ID,
                     ``{emr_cluster_id}``. The output should be two
                     instance group IDs.\ *
                     *

                     .. container:: code panel pdl

                        .. container:: codeContent panelContent pdl

                           .. code:: syntaxhighlighter-pre

                              # aws emr describe-cluster --cluster-id j-{emr_cluster_id} |grep Id |grep ig

                     Using the two instance groups ID from above,
                     ``{instance_group_id_1}`` and
                     ``{instance_group_id_2}``, obtain the public IP
                     addresses.

                     .. container:: code panel pdl

                        .. container:: codeContent panelContent pdl

                           .. code:: syntaxhighlighter-pre

                              # aws emr list-instances --cluster-id j-{emr_cluster_id} --instance-group-id ig-{instance_group_id_1} |grep PublicIpAddress
                              # aws emr list-instances --cluster-id j-{emr_cluster_id} --instance-group-id ig-{instance_group_id_2} |grep PublicIpAddress

                     Example output:

            #. ``ssh`` to each EMR node and download Unravel EMR
               bootstrap python script (stored in public READ accessible
               S3 bucket) into the local ``/tmp ``\ folder.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # curl https://s3.amazonaws.com/unraveldatarepo/unravel_emr_bootstrap.py -o /tmp/unravel_emr_bootstrap.py

               .. container::

            #. Run the Unravel EMR bootstrap script on each EMR cluster
               node, Master and Core & Task nodes\ ``. ``\ Substitute
               your Unravel Server Host's IP  for  {``UNRAVEL_SERVER``}.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo puthon /tmp/unravel_emr_bootstrippy --unravel-server {UNRAVEL_SERVER}

               .. container:: expand-container
                  :name: expander-1919255795

                  .. container:: expand-control
                     :name: expander-control-1919255795

                     Example script output from Master node.

                  .. container:: expand-content
                     :name: expander-content-1919255795

               .. container:: expand-container
                  :name: expander-321432731

                  .. container:: expand-control
                     :name: expander-control-321432731

                     Example script output from EMR Core and Task node.

                  .. container:: expand-content
                     :name: expander-content-321432731

            Once Unravel EMR bootstrap script has been run on all EMR
            cluster nodes, the unravel setting should be configured on
            the cluster. You can run some jobs on this EMR cluster and
            monitor the metric data displayed on the Unravel UI
            (http://unravel_ec2_node_public_IP:3000).

            If the EMR cluster is created in a different VPC, see `Step
            4 for configuring VPC peering connection for an Unravel
            node. <591364250.html>`__ 

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image0|
               `image2018-9-14_17-49-36.png <attachments/591298673/591298676.png>`__
               (image/png)
               |image1|
               `image2018-9-14_17-42-53.png <attachments/591298673/591298679.png>`__
               (image/png)
               |image2|
               `image2018-9-14_17-40-16.png <attachments/591298673/591298682.png>`__
               (image/png)
               |image3|
               `image2018-9-14_17-36-59.png <attachments/591298673/591298685.png>`__
               (image/png)
               |image4|
               `image2018-9-14_17-14-37.png <attachments/591298673/591298688.png>`__
               (image/png)
               |image5|
               `image2018-9-14_16-59-36.png <attachments/591298673/591298691.png>`__
               (image/png)
               |image6|
               `image2018-9-14_16-57-32.png <attachments/591298673/591298694.png>`__
               (image/png)
               |image7|
               `image2018-8-24_18-55-52.png <attachments/591298673/591298697.png>`__
               (image/png)
               |image8|
               `image2018-8-24_18-38-12.png <attachments/591298673/591298700.png>`__
               (image/png)
               |image9|
               `image2018-8-24_18-31-8.png <attachments/591298673/591298703.png>`__
               (image/png)
               |image10|
               `image2018-8-24_18-24-38.png <attachments/591298673/591298706.png>`__
               (image/png)
               |image11|
               `image2018-8-24_17-46-45.png <attachments/591298673/591298709.png>`__
               (image/png)
               |image12|
               `image2018-8-24_17-34-16.png <attachments/591298673/591298712.png>`__
               (image/png)
               |image13|
               `image2018-8-24_17-0-48.png <attachments/591298673/591298715.png>`__
               (image/png)
               |image14|
               `image2018-8-24_16-56-43.png <attachments/591298673/591298718.png>`__
               (image/png)
               |image15|
               `image2018-8-24_16-55-50.png <attachments/591298673/591298721.png>`__
               (image/png)
               |image16|
               `image2018-8-24_16-53-24.png <attachments/591298673/591298724.png>`__
               (image/png)
               |image17|
               `image2018-8-24_16-49-35.png <attachments/591298673/591298727.png>`__
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
