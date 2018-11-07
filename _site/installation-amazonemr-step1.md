  - title  
    Step 1: Install Unravel EC2 node for Amazon EMR

# Step 1: Install Unravel EC2 node for Amazon EMR

<div id="page" class="container">

<div id="main" class="container aui-page-panel">

<div id="main-header" class="container">

<div id="breadcrumb-section" class="container">

1.  [Unravel 4.4](index.html)
2.  [Unravel 4.4](Unravel-4.4_541197025.html)
3.  [Installation Guides](Installation-Guides_541393730.html)
4.  [Amazon Elastic MapReduce (Amazon EMR)](591528087.html)

</div>

**Unravel 4.4 : Step 1: Install Unravel EC2 node for Amazon EMR**

</div>

<div id="content" class="container view">

<div class="container page-metadata">

Created by katch, last modified by Raymond Lui on Oct 19, 2018

</div>

<div id="main-content" class="container wiki-content group">

<div class="container panel">

<div class="container panelHeader">

**Table of
    Contents**

</div>

<div class="container panelContent">

<div class="container toc-macro rbtoc1541196939777">

  - [Introduction](#Step1:InstallUnravelEC2nodeforAmazonEMR-Introduction)
  - [Requirements
    Checklist](#Step1:InstallUnravelEC2nodeforAmazonEMR-RequirementsChecklist)
  - [1. Setup the EC2 instance for unravel
    installation.](#Step1:InstallUnravelEC2nodeforAmazonEMR-1.SetuptheEC2instanceforunravelinstallation.)
      - [Provision an EC2
        Instance](#Step1:InstallUnravelEC2nodeforAmazonEMR-ProvisionanEC2Instance)
      - [Configure the EC2 node at first
        login](#Step1:InstallUnravelEC2nodeforAmazonEMR-ConfiguretheEC2nodeatfirstlogin)
  - [2. Install the Unravel EMR rpm on the created EC2
    instance.](#Step1:InstallUnravelEC2nodeforAmazonEMR-2.InstalltheUnravelEMRrpmonthecreatedEC2instance.)
  - [3. Login to Unravel Web
    UI](#Step1:InstallUnravelEC2nodeforAmazonEMR-3.LogintoUnravelWebUI)

</div>

</div>

</div>

**Introduction**

This topic explains how to deploy Unravel Server 4.4.X on AWS EC2
instance that will be used to monitor existing or newly created EMR
clusters. For instructions that correspond to older versions of Unravel
Server, contact Unravel Support.

<div class="container panel">

<div class="container panelHeader">

**Workflow Summary**

</div>

<div class="container panelContent">

1.  Setup the EC2 instance for unravel installation.
2.  Install the Unravel EMR RPM on the created EC2 instance.
3.  Log into Unravel Web UI.

</div>

</div>

**Requirements Checklist**

<div class="container table-wrap">

<table style="width:96%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 22%" />
<col style="width: 23%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Platform Compatibi lity</th>
<th>Base OS for EC2</th>
<th>EC2 instance type &amp; size</th>
<th>Security Group / IAM role</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><sup>AWS EMR 5.x (5.14 preferred ) </sup></p>
<p><sup>EMR Support Matrix</sup></p>
<p><sup>Hiv e 0.9.x  2.3.x</sup></p>
<p><sup>Spa rk 1.3.x, 1.6.x - 2.3.x</sup></p></td>
<td><p><sup>Base OS Redhat/CentOS 6.4 - 7.4</sup></p>
<p><sup>Recomme nded Centos 7.4 AMI</sup></p>
<p><sup>e.g.  a mi-02e98f78  (in us-east-1)</sup></p></td>
<td><p><sup>Minimum: R4.2xlarge (60 GB Ram)</sup></p>
<p><sup>Recommen ded: R4.4xlarge (122 GB Ram) </sup></p>
<p><sup>Virtuali zation type: HVM</sup></p>
<p><sup>Root device type: EBS – 100 - 500GB</sup></p></td>
<td><div class="line-block"><sup>Allowed ports for inbound access to unravel EC2 node:</sup><br />
<sup>Port 3000 Port 4043</sup></div>
<sup>EC2 instance profile that has permission to</sup> <sup>Read the S3 bucket used in the EMR clusters.</sup></td>
</tr>
</tbody>
</table>

</div>

Below is an example of security group used for Unravel EC2 node.  
Inbound rule

**1. Setup the EC2 instance for unravel installation.**

<div class="container">

**Provision an EC2 Instance**

  - Instance Type: r4.2xlarge; recommended r4.4xlarge; the instance
    type: HVM with EBS.\[

  - OS: centos7.4 (ami-02e98f78)

  - Minimum root disk space is 100GB. Recommendation: 200GB or above
    with "Provisioned IOPS SSD (IO1)" storage.

  - The EC2 instance **must** be in same region with the target EMR
    clusters which Unravel EC2 node will be monitoring.

  - Create a S3 ReadAccess only IAM role and assign it to Unravel EC2
    node to READ the archive logs on the S3 bucket configured for the
    EMR cluster. 
    
    <div class="container">
    
    <sub>For instance, create an IAM role that contains the policy that
    only READ the specific S3 bucket used on EMR cluster. Then create a
    EC2 instance profile and add the IAM role to it.  See</sub>[Creating
    IAM
    Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html)<sub>and</sub>[Create
    Instance Profile
    CLI](https://docs.aws.amazon.com/cli/latest/reference/iam/create-instance-profile.html)
    
    </div>

  - Security Group or policy required for Unravel EC2 node:   
    Unravel EC2 node works with multiple EMR clusters, including
    existing and newly created clusters. A TCP & UDP connection is
    needed from the EMR master node to Unravel EC2 node. You must
    
      - create a security group that allows port 3000 and port 4043 from
        EMR cluster nodes IP address, or
      - put the member of security group used on EMR cluster in this
        rule.  

<div class="container">

Below is an example of security group used for Unravel EC2 node.  
Inbound
rule

<div class="container table-wrap">

| Type            | P r o t o c o l | Po rt Ra ng e | Source                                                                                                                     |
| --------------- | --------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------- |
| All traffic     | A l l           | Al l          | SG ID of this group or Subnet IP block (e.g. 10.10.0.0/16)                                                                 |
| SSH             | T C P           | 22            | 0.0.0.0/0                                                                                                                  |
| Custom TCP Rule | T C P           | 30 00         | SG ID used on EMR cluster or Subnet IP block (if IP block belong to different VPC)  is required for VPC peering connection |
| Custom TCP Rule | T C P           | 40 43         | SG ID used on EMR cluster or Subnet IP block (if IP block belong to different VPC)  is required for VPC peering connection |

</div>

Outbound rule

<div class="container table-wrap">

| Type        | P r o t o c o l | Po rt Ra ng e | Source    |
| ----------- | --------------- | ------------- | --------- |
| All traffic | A l l           | Al l          | 0.0.0.0/0 |

</div>

</div>

**Configure the EC2 node at first login**

  - Disable `selinux`.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo setenforce Permissive
    ```
    
    </div>
    
    </div>

  - Edit the file to make sure the setting persists after reboot and
    make sure "SELINUX=permissive".
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # vi /etc/selinux/config
    ```
    
    </div>
    
    </div>

  - Install `libaio.x86_64 and lzop.x86_64.`
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo yum install -y libaio.x86_64
    # sudo yum install -y lzop.x86_64
    ```
    
    </div>
    
    </div>

  - Start `ntp`  and check time 
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo service ntpd start
    # sudo ntpq -p
    ```
    
    </div>
    
    </div>

  - Create a new user `hadoop`.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo useradd hadoop
    ```
    
    </div>
    
    </div>
    
    <div class="container">
    
    </div>
    
    confluence-information-macro
    confluence-information-macro-information
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > In a PoC or evaluation, the minimal root disk size 100GB should be
    > sufficient. When monitoring more EMR clusters or lots of jobs, we
    > recommend to set minimal root disk from 300 - 500GB  "Provisioned
    > IOPS" EBS volume with 3000 IOPS.
    > 
    > For production unravel use case, 200GB root disk Provisioned IOPS
    > EBS and RDS are recommended 
    > 
    > </div>

</div>

**2. Install the Unravel EMR rpm on the created EC2 instance.**

<div class="container">

</div>

confluence-information-macro has-no-icon
confluence-information-macro-warning

> Reminder
> 
> <div class="container confluence-information-macro-body">
> 
> AWS EMR 5.x support is available via Unravel 4.3.1.6 only 
> 
> </div>

  - Get the Unravel Server RPM; see [Download Unravel
    Software](https://unraveldata.atlassian.net/wiki/spaces/UNDOCS/pages/226132074/Download+Unravel+Software+Versions).
    Look for 4.3.1.6 EMR rpm download instruction.

  - Install the Unravel Server RPM.  The precise filename can vary,
    depending on how it was fetched or copied. 
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo rpm -U unravel-4.3.1.*-EMR-latest.rpm 
    ```
    
    </div>
    
    </div>

  - Run the specified `await_fixups.sh` script to make sure background
    processing is finished before going on to further steps. In a
    routine upgrade, it is okay to start all Unravel daemons, but do not
    stop or restart them until the `await_fixups.sh` prints "`Done"`.
    This may take a few minutes.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # /usr/local/unravel/install_bin/await_fixups.sh
    # /usr/local/unravel/install_bin/switch_to_user.sh hadoop hadoop
    ```
    
    </div>
    
    </div>

  - Edit the `/usr/local/unravel/etc/unravel.properties`file to append
    the following line in it.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    com.unraveldata.onprem=false
    ```
    
    </div>
    
    </div>
    
    For monitoring EMR spark service, add the following properties in
    the `unravel.properties` file.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    com.unraveldata.spark.live.pipeline.enabled=true
    com.unraveldata.spark.hadoopFsMulti.useFilteredFiles=true
    com.unraveldata.spark.events.enableCaching=true
    ```
    
    </div>
    
    </div>
    
    <div class="container">
    
    </div>
    
    confluence-information-macro
    confluence-information-macro-information
    
    > 
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > The installation creates `/usr/local/unravel/` which contains the
    > executables, scripts, and settings. User `unravel` is created. The
    > initial internal database and other durable states are put in
    > `/srv/unravel/` for larger storage.    
    > The master configuration file is in
    > `/usr/local/unravel/etc/unravel.properties` and the logs are in
    > `/usr/local/unravel/logs/`. The RPM installation creates user
    > `unravel (`if not already existing), `/etc/init.d/unravel_*`
    > scripts for controlling services, and `/etc/init.d/unravel_all.sh`
    > which can be used to manually stop, start, and get status of all
    > daemons in proper order.  
    > During initial install, a bundled database is used; you can
    > [switch to use AWS RDS](591233047.html) for production.
    > 
    > </div>
    
    <div class="container">
    
    </div>
    
    confluence-information-macro confluence-information-macro-warning
    
    > Security Remind
    > 
    > <div class="container confluence-information-macro-body">
    > 
    > We do not recommend making Unravel Server UI accessible on the
    > public Internet. 
    > 
    > </div>

**3. Login to Unravel Web UI**

  - Start Unravel daemons.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # sudo /etc/init.d/unravel_all.sh start
    ```
    
    </div>
    
    </div>

  - Create a `ssh` tunnel from your workstation to the Unravel EC2 node.
    
    <div class="container code panel pdl">
    
    <div class="container codeContent panelContent pdl">
    
    ``` sourceCode syntaxhighlighter-pre
    # ssh -i ssh_key.pem centos@{UNRAVEL_HOST_IP} -L 3000:127.0.0.1:3000
    ```
    
    </div>
    
    </div>

  - From your workstation browser, navigate to URL
    <http://127.0.0.1:3000>. Login to the unravel UI with user id*:*
    `admin` and password: `unraveldata.`\* *Congratulations\! Unravel
    EC2 node is up and running. You can proceed to \`connect to your
    existing or new EMR clusters \<591298673.html\>\`\_\_.*
    
      - 

</div>

<div class="container pageSection group">

<div class="container pageSectionHeader">

**Attachments:**

</div>

<div class="container greybox">

![image0](images/icons/bullet_blue.gif)
[image2018-8-23\_15-27-3.png](attachments/591397010/591397013.png)
(image/png) ![image1](images/icons/bullet_blue.gif)
[image2018-8-23\_15-26-1.png](attachments/591397010/591397016.png)
(image/png) ![image2](images/icons/bullet_blue.gif)
[image2017-2-26\_0-20-12.png](attachments/591397010/591397019.png)
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
