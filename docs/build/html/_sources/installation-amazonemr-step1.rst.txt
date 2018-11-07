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

         .. rubric:: Unravel 4.4 : Step 1: Install Unravel EC2 node for
            Amazon EMR
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified by Raymond Lui on Oct 19,
            2018

         .. container:: wiki-content group
            :name: main-content

            .. container:: panel

               .. container:: panelHeader

                  **Table of Contents**

               .. container:: panelContent

                  .. container:: toc-macro rbtoc1541196939777

                     -  `Introduction <#Step1:InstallUnravelEC2nodeforAmazonEMR-Introduction>`__
                     -  `Requirements
                        Checklist <#Step1:InstallUnravelEC2nodeforAmazonEMR-RequirementsChecklist>`__
                     -  `1. Setup the EC2 instance for unravel
                        installation. <#Step1:InstallUnravelEC2nodeforAmazonEMR-1.SetuptheEC2instanceforunravelinstallation.>`__

                        -  `Provision an EC2
                           Instance <#Step1:InstallUnravelEC2nodeforAmazonEMR-ProvisionanEC2Instance>`__
                        -  `Configure the EC2 node at first
                           login <#Step1:InstallUnravelEC2nodeforAmazonEMR-ConfiguretheEC2nodeatfirstlogin>`__

                     -  `2. Install the Unravel EMR rpm on the created
                        EC2
                        instance. <#Step1:InstallUnravelEC2nodeforAmazonEMR-2.InstalltheUnravelEMRrpmonthecreatedEC2instance.>`__
                     -  `3. Login to Unravel Web
                        UI <#Step1:InstallUnravelEC2nodeforAmazonEMR-3.LogintoUnravelWebUI>`__

            .. rubric:: Introduction
               :name: Step1:InstallUnravelEC2nodeforAmazonEMR-Introduction

            This topic explains how to deploy Unravel Server 4.4.X on
            AWS EC2 instance that will be used to monitor existing or
            newly created EMR clusters. For instructions that correspond
            to older versions of Unravel Server, contact Unravel
            Support.

            .. container:: panel

               .. container:: panelHeader

                  **Workflow Summary**

               .. container:: panelContent

                  #. Setup the EC2 instance for unravel installation.
                  #. Install the Unravel EMR RPM on the created EC2
                     instance.
                  #. Log into Unravel Web UI.

            .. rubric:: Requirements Checklist
               :name: Step1:InstallUnravelEC2nodeforAmazonEMR-RequirementsChecklist

            .. container:: table-wrap

               +-----------+---------------+----------------+-----------------------+
               | Platform  | Base OS for   | EC2 instance   | Security Group / IAM  |
               | Compatibi | EC2           | type & size    | role                  |
               | lity      |               |                |                       |
               +===========+===============+================+=======================+
               | :sup:`AWS | :sup:`Base OS | :sup:`Minimum: | | :sup:`Allowed ports |
               | EMR 5.x   | Redhat/CentOS | R4.2xlarge (60 |   for inbound access  |
               | (5.14     | 6.4 - 7.4`    | GB Ram)`       |   to unravel EC2      |
               | preferred |               |                |   node:`              |
               | )         | :sup:`Recomme | :sup:`Recommen | | :sup:`Port 3000     |
               | `         | nded          | ded:           |   Port 4043`          |
               |           | Centos 7.4    | R4.4xlarge     |                       |
               | :sup:`EMR | AMI`          | (122 GB Ram)   | :sup:`EC2 instance    |
               | Support   |               | `              | profile that has      |
               | Matrix`   | :sup:`e.g.  a |                | permission to`        |
               |           | mi-02e98f78   | :sup:`Virtuali | :sup:`Read the S3     |
               | :sup:`Hiv | (in           | zation         | bucket used in the    |
               | e         | us-east-1)`   | type: HVM`     | EMR clusters.`        |
               | 0.9.x     |               |                |                       |
               | 2.3.x`    |               | :sup:`Root     |                       |
               |           |               | device type:   |                       |
               | :sup:`Spa |               | EBS – 100 -    |                       |
               | rk        |               | 500GB`         |                       |
               | 1.3.x,    |               |                |                       |
               | 1.6.x -   |               |                |                       |
               | 2.3.x`    |               |                |                       |
               +-----------+---------------+----------------+-----------------------+

            | Below is an example of security group used for Unravel EC2
              node.
            | Inbound rule

            .. rubric:: 1. Setup the EC2 instance for unravel
               installation.
               :name: Step1:InstallUnravelEC2nodeforAmazonEMR-1.SetuptheEC2instanceforunravelinstallation.

            .. container::

               .. rubric:: Provision an EC2 Instance
                  :name: Step1:InstallUnravelEC2nodeforAmazonEMR-ProvisionanEC2Instance

               -  Instance Type: r4.2xlarge; recommended r4.4xlarge; the
                  instance type: HVM with EBS.[
               -  OS: centos7.4 (ami-02e98f78)
               -  Minimum root disk space is 100GB. Recommendation:
                  200GB or above with "Provisioned IOPS SSD (IO1)"
                  storage.
               -  The EC2 instance \ **must** be in same region with the
                  target EMR clusters which Unravel EC2 node will be
                  monitoring.
               -  Create a S3 ReadAccess only IAM role and assign it to
                  Unravel EC2 node to READ the archive logs on the S3
                  bucket configured for the EMR cluster. 

                  .. container::

                     :sub:`For instance, create an IAM role that
                     contains the policy that only READ the specific S3
                     bucket used on EMR cluster. Then create a EC2
                     instance profile and add the IAM role to it. 
                     See`\ `Creating IAM
                     Role <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html>`__\ :sub:`and`\ `Create
                     Instance Profile
                     CLI <https://docs.aws.amazon.com/cli/latest/reference/iam/create-instance-profile.html>`__

               -  | Security Group or policy required for Unravel EC2
                    node: 
                  | Unravel EC2 node works with multiple EMR clusters,
                    including existing and newly created clusters. A TCP
                    & UDP connection is needed from the EMR master node
                    to Unravel EC2 node. You must

                  -  create a security group that allows port 3000 and
                     port 4043 from EMR cluster nodes IP address, or

                  -  put the member of security group used on EMR
                     cluster in this rule.  

               | 

               .. container::

                  | Below is an example of security group used for
                    Unravel EC2 node.
                  | Inbound rule

                  .. container:: table-wrap

                     +-------------+---+----+---------------------------------------------+
                     | Type        | P | Po | Source                                      |
                     |             | r | rt |                                             |
                     |             | o | Ra |                                             |
                     |             | t | ng |                                             |
                     |             | o | e  |                                             |
                     |             | c |    |                                             |
                     |             | o |    |                                             |
                     |             | l |    |                                             |
                     +=============+===+====+=============================================+
                     | All traffic | A | Al | SG ID of this group or Subnet IP block      |
                     |             | l | l  | (e.g. 10.10.0.0/16)                         |
                     |             | l |    |                                             |
                     +-------------+---+----+---------------------------------------------+
                     | SSH         | T | 22 | 0.0.0.0/0                                   |
                     |             | C |    |                                             |
                     |             | P |    |                                             |
                     +-------------+---+----+---------------------------------------------+
                     | Custom TCP  | T | 30 | SG ID used on EMR cluster or Subnet IP      |
                     | Rule        | C | 00 | block (if IP block belong to different      |
                     |             | P |    | VPC)  is required for VPC peering           |
                     |             |   |    | connection                                  |
                     +-------------+---+----+---------------------------------------------+
                     | Custom TCP  | T | 40 | SG ID used on EMR cluster or Subnet IP      |
                     | Rule        | C | 43 | block (if IP block belong to different      |
                     |             | P |    | VPC)  is required for VPC peering           |
                     |             |   |    | connection                                  |
                     +-------------+---+----+---------------------------------------------+

                  Outbound rule

                  .. container:: table-wrap

                     +-------------+---+----+---------------------------------------------+
                     | Type        | P | Po | Source                                      |
                     |             | r | rt |                                             |
                     |             | o | Ra |                                             |
                     |             | t | ng |                                             |
                     |             | o | e  |                                             |
                     |             | c |    |                                             |
                     |             | o |    |                                             |
                     |             | l |    |                                             |
                     +=============+===+====+=============================================+
                     | All traffic | A | Al | 0.0.0.0/0                                   |
                     |             | l | l  |                                             |
                     |             | l |    |                                             |
                     +-------------+---+----+---------------------------------------------+

               | 

               .. rubric:: Configure the EC2 node at first login
                  :name: Step1:InstallUnravelEC2nodeforAmazonEMR-ConfiguretheEC2nodeatfirstlogin

               -  Disable ``selinux``.

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           # sudo setenforce Permissive

               -  Edit the file to make sure the setting persists after
                  reboot and make sure "SELINUX=permissive".

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           # vi /etc/selinux/config

               -  Install ``libaio.x86_64 and lzop.x86_64.``

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           # sudo yum install -y libaio.x86_64
                           # sudo yum install -y lzop.x86_64

               -  Start ``ntp``  and check time 

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           # sudo service ntpd start
                           # sudo ntpq -p

               -  Create a new user ``hadoop``.

                  .. container:: code panel pdl

                     .. container:: codeContent panelContent pdl

                        .. code:: syntaxhighlighter-pre

                           # sudo useradd hadoop

                  .. container::
                  confluence-information-macro confluence-information-macro-information

                     .. container:: confluence-information-macro-body

                        In a PoC or evaluation, the minimal root disk
                        size 100GB should be sufficient. When monitoring
                        more EMR clusters or lots of jobs, we recommend
                        to set minimal root disk from 300 - 500GB 
                        "Provisioned IOPS" EBS volume with 3000 IOPS.

                        For production unravel use case, 200GB root disk
                        Provisioned IOPS EBS and RDS are recommended 

            .. rubric:: 2. Install the Unravel EMR rpm on the created
               EC2 instance.
               :name: Step1:InstallUnravelEC2nodeforAmazonEMR-2.InstalltheUnravelEMRrpmonthecreatedEC2instance.

            .. container::
            confluence-information-macro has-no-icon confluence-information-macro-warning

               Reminder

               .. container:: confluence-information-macro-body

                  AWS EMR 5.x support is available via Unravel 4.3.1.6
                  only 

            -  Get the Unravel Server RPM; see `Download Unravel
               Software <https://unraveldata.atlassian.net/wiki/spaces/UNDOCS/pages/226132074/Download+Unravel+Software+Versions>`__.
               Look for 4.3.1.6 EMR rpm download instruction.
            -  Install the Unravel Server RPM.  The precise filename can
               vary, depending on how it was fetched or copied. 

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo rpm -U unravel-4.3.1.*-EMR-latest.rpm 

            -  Run the specified \ ``await_fixups.sh`` script to make
               sure background processing is finished before going on to
               further steps. In a routine upgrade, it is okay to start
               all Unravel daemons, but do not stop or restart them
               until the \ ``await_fixups.sh`` prints "``Done"``. This
               may take a few minutes.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # /usr/local/unravel/install_bin/await_fixups.sh
                        # /usr/local/unravel/install_bin/switch_to_user.sh hadoop hadoop

            -  Edit the
               ``/usr/local/unravel/etc/unravel.properties``\ file to
               append the following line in it.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        com.unraveldata.onprem=false

               For monitoring EMR spark service, add the following
               properties in the ``unravel.properties`` file.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        com.unraveldata.spark.live.pipeline.enabled=true
                        com.unraveldata.spark.hadoopFsMulti.useFilteredFiles=true
                        com.unraveldata.spark.events.enableCaching=true

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     | The installation creates ``/usr/local/unravel/``
                       which contains the executables, scripts, and
                       settings. User ``unravel`` is created. The
                       initial internal database and other durable
                       states are put in ``/srv/unravel/`` for larger
                       storage.  
                     | The master configuration file is in
                       ``/usr/local/unravel/etc/unravel.properties`` and
                       the logs are in ``/usr/local/unravel/logs/``. The
                       RPM installation creates user ``unravel (``\ if
                       not already existing), ``/etc/init.d/unravel_*``
                       scripts for controlling services, and
                       ``/etc/init.d/unravel_all.sh`` which can be used
                       to manually stop, start, and get status of all
                       daemons in proper order.
                     | During initial install, a bundled database is
                       used; you can `switch to use AWS
                       RDS <591233047.html>`__ for production.

               .. container::
               confluence-information-macro confluence-information-macro-warning

                  Security Remind

                  .. container:: confluence-information-macro-body

                     We do not recommend making Unravel Server UI
                     accessible on the public Internet. 

            .. rubric:: 3. Login to Unravel Web UI
               :name: Step1:InstallUnravelEC2nodeforAmazonEMR-3.LogintoUnravelWebUI

            -  Start Unravel daemons.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # sudo /etc/init.d/unravel_all.sh start

            -  Create a ``ssh`` tunnel from your workstation to the
               Unravel EC2 node.

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        # ssh -i ssh_key.pem centos@{UNRAVEL_HOST_IP} -L 3000:127.0.0.1:3000

            -  From your workstation browser, navigate to URL
               http://127.0.0.1:3000. Login to the unravel UI with user
               id\ *:* ``admin`` and password: ``unraveldata.``\ *
               *\ Congratulations! Unravel EC2 node is up and running.
               You can proceed to `connect to your existing or new EMR
               clusters <591298673.html>`__.\ *
               *

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image0|
               `image2018-8-23_15-27-3.png <attachments/591397010/591397013.png>`__
               (image/png)
               |image1|
               `image2018-8-23_15-26-1.png <attachments/591397010/591397016.png>`__
               (image/png)
               |image2|
               `image2017-2-26_0-20-12.png <attachments/591397010/591397019.png>`__
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
