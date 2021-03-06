.. _console_overview:

Console
-------

This section covers the individual components of the Enstratius console at a high level:

.. figure:: ./images/newconsole.png
   :height: 734 px
   :width: 1199 px
   :scale: 70 %
   :alt: UI Overview
   :align: center

1. Custom Logo
~~~~~~~~~~~~~~
This logo can be customized for your Enstratius installation.

2. User Profile
~~~~~~~~~~~~~~~
.. figure:: ./images/updateduserprofile.png
   :height: 185 px
   :width: 394 px
   :scale: 100 %
   :alt: User Profile
   :align: center

Clicking on the name of the person logged in to the Enstratius console provides
functionality for editing user profile information (:ref:`including setting notification
targets <notification_targets>`), provides functionality for :ref:`editing user profile
information <edit_profile>` (:ref:`including setting notification targets
<notification_targets>`), password changes, :ref:`notification preferences
<user_notifications>`, and API key management.

3. Account & Region
~~~~~~~~~~~~~~~~~~~

.. figure:: ./images/acctdropdown.png
   :height: 240 px
   :width: 471 px
   :scale: 100 %
   :alt: Account
   :align: center



Enstratius can access many accounts in one cloud provider or accounts in separate clouds
simultaneously. Clicking on the account will activate a drop-down menu for selecting an account. 

If the underlying cloud provider has the concept of regions, those regions will be
displayed and be selectable from here as well. When you enter your cloud credentials for the first time, these
regions will auto-populate as Enstratius begins to discover the attributes of the cloud
provider.

4. Cloud
~~~~~~~~

Logo of the current cloud provider. When navigating between different cloud providers, this logo
will change to reflect the selected cloud.

5. Navigation Menu
~~~~~~~~~~~~~~~~~~
The navigation bar displays the first level of interaction with an account's resources, services, and settings.
Available top-level menu items will be limited by the logged-in user's role. 

6. Content Pane
~~~~~~~~~~~~~~~
The main content window for interacting with cloud resources is the content pane. The
content displayed in this main window depends on the selections made in the
navigation menu. For example, if Infrastructure > Servers is selected, all
active servers will be displayed in the content pane.

7. Filter
~~~~~~~~~
The filter text box allows for dynamic filtering of content presented in the content pane.

8. Actions Menu
~~~~~~~~~~~~~~~
.. figure:: ./images/newserveractions.png
   :height: 290 px
   :width: 191 px
   :scale: 95 %
   :alt: Actions Menu
   :align: center

The actions menu is activated by clicking on one of the "actions" links to the right of a resource.
Options in this menu depend on the selected resource.

The action menu shown here is for a cloud server. Note: Some options shown in this
image are only available after the Enstratius agent has been installed on the virtual
machine.

If the cloud administrator for your account has implemented groups and roles, the action
link may or may not be present. Presenting or hiding this link
is one method Enstratius uses to enforce role-based access controls for cloud
infrastructure.

9. Notification/Alert/Support Menu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. figure:: ./images/jobalert.png
   :height: 171 px
   :width: 272 px
   :scale: 95 %
   :alt: Alerts
   :align: center

The status menu is located at the bottom of the content pane. If there are any notifications or alerts in
any of the accounts of which you are a part, they will be displayed here. Alerts are
categorized as High, Medium, or Low. Clicking on an alert color will slide out a truncated
list of alerts. Options for interacting with notifications and alerts include clicking on them to view in more detail or
deleting them.

The support link provided at the bottom of the console provides an integration point for
external help desk functionality, such as Zendesk. In the SaaS offering for Enstratius,
clicking the support link will activate a dialog window for sending a support request to
the Enstratius team.
