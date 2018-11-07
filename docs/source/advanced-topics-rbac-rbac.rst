:title: Role Based Access Control (RBAC)

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
            #. `Advanced Topics <Advanced-Topics_541197049.html>`__
            #. `Roles and Role Based Access
               Control <Roles-and-Role-Based-Access-Control_541197074.html>`__

         .. rubric:: Unravel 4.4 : Role Based Access Control (RBAC)
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by katch, last modified on Oct 16, 2018

         .. container:: wiki-content group
            :name: main-content

            Role Based Access Control allows Admin to limit what a
            specific end-user can access based upon tags/defaults. See
            the\ `Role
            Matrix <https://unraveldata.atlassian.net/wiki/spaces/UN43/pages/541393167/Supported+Roles#SupportedRoles-RBAC-Roles>`__\ for
            a description of the various roles.

            When RBAC mode is enabled a end-users' access to the Unravel
            UI is filtered by tags. Two different modes are available:

            -  Extended Mode

               -  `Application \|
                  Applications <The-Applications-Page_541164197.html#TheApplicationsPage-ApplicationsTab>`__,
               -  `Operations \| Usage Details \|
                  Infrastructure <The-Operations-Page_541033301.html#TheOperationsPage-ChartsResources>`__,
               -  `Operations \| Usage Details \| Impala Usage
                   <http://The%20Operations%20Page#Impala>`__
               -  `Reports \| Operational Insights \|
                  Chargeback <http://The%20Reports%20Page#Chargeback>`__

            -  Restricted mode

               -  `Application \|
                  Applications <The-Applications-Page_541164197.html#TheApplicationsPage-ApplicationsTab>`__

            You can always toggle the status of RBAC via the UIX;
            however the unravel
            property\ ``com.unraveldata.login.mode``\ determines if you
            use the UIX to add or edit end-user roles. When the property
            is setto \ ``ldap`` the UIX can't be used to add or edit
            end-user roles, see\ `LDAP Supported
            Roles <Supported-Roles_541360915.html#SupportedRoles-RBAC_LDAP>`__.

            .. rubric:: 1. Go to Manage \| Role Manager for access to
               the Role Manager.
               :name: RoleBasedAccessControl(RBAC)-1.GotoManage|RoleManagerforaccesstotheRoleManager.

            The RBAC default is set via
            ``com.unraveldata.rbac.enabled.`` You can toggle the status
            of RBAC; however, when the unravel daemon is restarted RBAC
            resets.

            .. rubric:: 2. Once RBAC is activated you can add filters
               for specific end-users.
               :name: RoleBasedAccessControl(RBAC)-2.OnceRBACisactivatedyoucanaddfiltersforspecificend-users.

            Any end-user roles you have previously set are displayed. If
            the Unravel daemon was restarted after you added end-user
            roles, those entries are lost. You can add end-users one at
            at time via Add New Role or one or more filters at a time by
            uploading a role file (.``csv``).  If the
            "\ ``login.mode``\ " is set to ``ldap`` the "Select role
            file" box and Add New Role button is not displayed.

            .. rubric:: 3. Adding Roles
               :name: RoleBasedAccessControl(RBAC)-3.AddingRoles

            You limit end-user access through tags. In the example below
            only two tags are available, *project* and *tenant*. If a
            3rd tag, *department*, had been defined would be available.
            The end-user filter between the red brackets were loaded
            using a .\ ``csv`` file.

            .. container::

               .. container::
               confluence-information-macro confluence-information-macro-information

                  .. container:: confluence-information-macro-body

                     The tags used here must also be loaded as
                     `Application <Creating-Application-Tags_541295262.html>`__ `Tagging
                     Workflows <Tagging-Workflows_541327954.html>`__
                     tags. If they are not, end-users with defined roles
                     can not see any applications since their view is
                     based on tags.

            | 

            .. rubric:: Add role directly
               :name: RoleBasedAccessControl(RBAC)-Addroledirectly

            Clicking on Add New Role adds a row to the Roles table
            containing text boxes (1). You must define the User and at
            least one tag restriction. To add multiple tag names under a
            tag type separate the tag names with commas, be sure the
            string contains no spaces or special characters. To save the
            entry click the save glyph (). To delete your new entry
            click the delete glyph ().

            .. rubric:: Add one or more roles via a role file
               :name: RoleBasedAccessControl(RBAC)-Addoneormorerolesviaarolefile

            Click on the **Select role file** to chose the .\ ``csv``
            file.The format of a .\ ``csv`` is as follows:

            -  **first row is a header row defining the columns tags**:
               ``user,``\ ``tagKey[,tagKey]*``

               -  **``tagKey``**: is a valid tag key, i.e., department,
                  tenant.

            -  **one or more rows defining user and tag values**:
               ``user,``\ ``tagValue[,tagValue``\ ``]*``

               -  **``tagValue``**: is empty, a valid tag value for
                  ``tagType``, or ``tagString``,

               -  **``tagString``**: is a series of \ ``tagValue``\ s
                  separated by commas and enclosed in quotes, and
               -  **\***: means zero or more

            .. container::

               .. container::
               confluence-information-macro has-no-icon confluence-information-macro-information

                  Note:

                  .. container:: confluence-information-macro-body

                      The file *must* define at least one tag key, one
                     user and one tag value for the ``user``.

                     After you add your last ``tagValue ``\ you can
                     leave the rest of the row blank. See the userNew
                     filter in the ``.csv`` file below for an example .

                     ``tagValue``\ s must be ordered as defined in the
                     header row.

                     No special characters or spaces are allowed in
                     file.

            The .\ ``csv`` file below was used to load filters within
            the red brackets.

            .. container::

               .. container:: code panel pdl

                  .. container:: codeContent panelContent pdl

                     .. code:: syntaxhighlighter-pre

                        user,project,tenant
                        user72,"group1,group2",mm
                        user25,,"3n,3m"
                        userNew,groupNew
                        user33,"group3,group2","3m,mm"

            .. rubric:: 4. Editing or Deleting Roles
               :name: RoleBasedAccessControl(RBAC)-4.EditingorDeletingRoles

            To edit a role, click the edit glyph (). You may edit the
            tags adding or deleting, but you can not edit the end-user's
            name.

            To delete a role, click the delete glyph.

            .. rubric:: Effect of Role Access Control
               :name: RoleBasedAccessControl(RBAC)-EffectofRoleAccessControl

            .. rubric:: End-user's Access with RBAC turned off
               :name: RoleBasedAccessControl(RBAC)-End-user'sAccesswithRBACturnedoff

            The user has access to all the Unravel UI features and all
            applications.

            .. rubric:: End-user's Access with RBAC turned on.
               :name: RoleBasedAccessControl(RBAC)-End-user'sAccesswithRBACturnedon.

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image0|
               `Save.png <attachments/541131426/541360905.png>`__
               (image/png)
               |image1|
               `closeCross.png <attachments/541131426/541229611.png>`__
               (image/png)
               |image2| `RBAC-Add
               Role.png <attachments/541131426/541131434.png>`__
               (image/png)
               |image3|
               `RBAC-Access.png <attachments/541131426/541393583.png>`__
               (image/png)
               |image4|
               `Edit.png <attachments/541131426/541098665.png>`__
               (image/png)
               |image5|
               `RBAC-ExampleWithLoadByCSV.png <attachments/541131426/541360909.png>`__
               (image/png)
               |image6|
               `RBAC-OffEnduserView.png <attachments/541131426/541229615.png>`__
               (image/png)
               |image7|
               `RBAC-OffApplication.png <attachments/541131426/541197078.png>`__
               (image/png)
               |image8| `4.3 RBAC feature
               guide.png <attachments/541131426/541328094.png>`__
               (image/png)

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Nov 02, 2018 15:18

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
