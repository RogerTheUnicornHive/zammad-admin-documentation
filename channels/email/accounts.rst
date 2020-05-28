Accounts
--------

.. toctree::
   :hidden:

   accounts/2fa-gmail

All system email addresses can be added in this menu item.
If you want to fetch emails via POP3 or IMAP you have to create a mail channel in here.

.. hint:: If you're using Office365 or Exchange mailboxes, please ensure that your Mailbox is not shared, but a normal mailbox account.

.. note:: **Special instructions for two-factor authentication (2FA):**

   If your email provider requires you to enter a one-time passcode (sent via SMS) when logging in, you’ll have to :doc:`generate an app password to use with Zammad <accounts/2fa-gmail>`.

.. _add-a-mail-account:

Add a mail account
^^^^^^^^^^^^^^^^^^

Adding new email accounts to Zammad is super easy. 
Our wizard will help you during the setup. 
Zammad tries to detect the POP3, IMAP and SMTP server settings automatically. This should work most of the time. 
If it doesn’t work, use the “Experts” button to configure it yourself.

.. hint:: 🚯 By default, Zammad will **remove emails from your inbox after importing them**.

   If you want Zammad to leave the contents of your inbox untouched, use the *Keep Messages on Server* option in the *Experts* configuration view.

If you're looking on how to add further email addresses or aliases to existing accounts, you may want to have a look at 
:doc:`/channels/email/aliases`.

So let's get started setting up an email channel! 
For this, simply navigate to "Email" and click on the "new" button.
A wizard will appear and leads you through the following steps.

   .. warning:: Be sure to **disable all auto-reply triggers** whenever adding a new email account. Zammad will begin importing 
      emails shortly after the account has been added, and incoming messages will cause any active triggers to fire.

1. step: **Incoming**

   In order to create an email channel, we need minimal information such as:

   Organization & Department name
      This is the display name Zammad will use when sending out emails via this account. Keep in mind that there's 
      also a setting within the tab "Settings" which allows you to not add the agents name to that display name.

      .. hint:: Organization & Department name may differ between different email addresses of the same channel. 💪

   Email
      The email address of your account. Zammad will use this email address for sending mail as well. 
      You can add further email addresses to this account later if needed.

      .. hint:: If your email address and user name for the account differ, please use *Experts* settings to 
         provide the username.

   Password
      Naturally we need to know how to authenticate you. 😇

   Destination group
      The destination group decides which group we will route the mail into. 

      .. hint:: Especially if using a catchall or several email addresses in general, you can re-route emails if needed. 
         This can be done by either using :doc:`/channels/email/filters` or :doc:`/manage/trigger`. 
         Keep in mind that filters are executed *before* triggers.

   Need more advanced settings?
      If you need further advanced settings or just want to make sure that you've seen all settings, click on "Experts" 
      on the lower left. If you're good to go at this point already, just click on the button "Connect".

   .. figure:: /images/channels/email/adding-email-account-incoming_step1.png
      :alt: Adding a email account to Zammad - Basic inbound configuration
      :align: center

2. step: *optional* **Expert settings**
   
   If you've chosen to see Expert settings, Zammad will no present further advanced settings for the incoming channel.

   Type
      Type decides if you want to fetch emails via IMAP or POP3 protocoll. Usually IMAP is the best pick.

      .. note:: Using POP3 doesn't allow you to keep messages on the server and fetch from specific folders!

   Host
      Please provide your email servers hostname or IP address here.

   Password
      You can skip the password field - you already provided it in step one. 
      Of course you can insert the correct password here, in case you made a mistake in step 1.

   SSL / StartTLS
      Does your email server support fetching emails encrypted? If so, select ``yes`` - if not, select ``no``. 
      Zammad will automatically figure out if it has to use SSL or StartTLS.

   Port
      This is usually ``993`` for IMAP and ``995`` for POP. Sometimes the port can differ from those standard ports. 
      You can adjust them here if needed.

   Folder
      If you want to fetch from the default Inbox, you can keep this field empty. 
      You can define the folder to fetch from with this field - keep in mind that you'll need to provide the whole 
      folder structure if you're using a subfolder.

      .. note:: IMAP uses directory dividers which may differ from system to system. Usually it's either ``/`` or ``.``. 
         If you're not sure, check with your mail server provider. A subfolder of your Inbox could look like this: 
         ``INBOX.MyFolder``

      .. hint:: 📥 **If configuring Zammad to retrieve mail from a folder other than your inbox...**

         During the account setup process, Zammad will verify your credentials by sending you an email from your own account, 
         and then monitoring the folder you selected to confirm that it was delivered successfully.

         Account setup will not succeed until this test message appears in the incoming mail folder you specified. If you don’t 
         have filters set up to move Zammad’s test message there automatically, be sure to move it there yourself.

   Keep messages on server
      This setting is by default set to ``no``. This causes Zammad to *delete* all mails it fetched from mailbox! 
      If you don't want this, switch to ``yes``.

      .. hint:: *How Zammad behaves if you set "Keep messages on server" to* ``yes``

         Every email that Zammad fetched will automatically be marked as seen. This is also an indicator for Zammad if it 
         has to fetch a specific mail. If you mark an email as seen *before* Zammad fetched the mail, it will not be fetched!

         If you mark a seen mail as unseen and it has been fetched by Zammad already, it will mark the mail as seen and 
         automatically ignore the message during the next fetch. 

   .. figure:: /images/channels/email/adding-email-account-incoming_expert-settings_step2.png
      :alt: Expert settings for incoming email channel in Zammad
      :align: center

3. step: **Outbound**

   Send mails via
      This option lets you either use a ``SMTP`` server or use another ``local MTA`` like e.g. sendmail.

      .. note:: ``local MTA`` is not available as a option within Hosted Setup.

   Host
      If you chose to use SMTP, you'll need to define your outgoing mail server here. It's totally fine if your 
      email server setup uses the same host for incoming and outgoing.

   User *(optional)*
      If your SMTP server requires authentication, you can provide a username here. It's usually the same username 
      as for incoming - however, it may also differ.

   Password *(optional)*
      If your SMTP server requires authentication, you can provide the users password here. Zammad will by default use 
      the same password as for the incoming server.

   Port
      Provide your SMTP servers port here. Default ports usually are ``587`` or ``465``. 
      Zammad will automatically detect if it can use SSL or StartTLS and do so!

   If you're ready to go, click on the "Continue" button to check your configuration.

   .. figure:: /images/channels/email/adding-email-account-outbound_step3.png
      :alt: Configuring an outbound email server in Zammad
      :align: center

4. step: **Verification**

   Zammad will now ensure that the provided configuration is valid. 
   At this step it will send a test email to the mail account you're configuring at this moment. 
   By this it can also check if fetching the mail does work without issues.

   This may take a short moment. 
   If the modal does not disappear or bring back the wizard, Zammad most proberbly can't find the mail in your inbox.

   If everything is good to go, the modal will disappear and channel will appear within the interface. 
   Zammad now already fetches your mailbox! 🎉

     .. hint:: Can't send mails from a ticket yet? You may want to check your :doc:`Group Settings </manage/groups>`.

   .. figure:: /images/channels/email/adding-email-account_verification-send-and-receive.gif
      :alt: Zammad verifying email settings.
      :align: center

Servicing existing accounts
^^^^^^^^^^^^^^^^^^^^^^^^^^^

After adding an account, it will automatically appear within the email channel overview. 
At this point Zammad will allow you to adjust settings whenever needed depending on what you'll want to do.

Below we'll handle all email account based settings - if you want to learn more about handling email addresses, 
please have a look at the :doc:`/channels/email/aliases` section.

.. figure:: /images/channels/email/email-channel-overview.png
   :alt: Zammad list all configured email accounts at one place
   :align: center

Changing the destination group for new emails
+++++++++++++++++++++++++++++++++++++++++++++

Sometimes plans change and you may want to change the default destination group of your email channel. 
You can change your destination group at any time - to do so, simply click on the group name if the channel 
you want to change. 

Zammad then will provide a new modal dialogue in which you can change the group to another one. 

.. note:: Zammad will only display active groups for selection.

.. hint:: Changing destination groups does not affect existing tickets, they'll remain in their original group.

.. figure:: /images/channels/email/change-destination-group-for-email-account.png
   :alt: Changing the destination group for an email account
   :align: center

Updating email account settings (Hostname, Password, ...)
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Some situations require to update email account settings. Those include e.g. changing the password or using 
another mail host if required. 

To do so, Zammad provides you with *edit* links for inbound and outbound. 
The exact same wizard will be used for updating like upon adding the account. 
If something is unclear, you can safely scroll up and have a look. 💪

.. note:: If you edit "Inbound" , Zammad will also provide the "Outbound" configuration.
      If you just have to edit your Outbound configuration, you can safely just edit outbound.

.. hint:: In some situations your browser may "change" the password living in the password field. 
      To ensure Zammad won't fail authentication, it's the best to also insert the password of the email account.

.. figure:: /images/channels/email/updating-email-account.png
   :alt: Changing account information of an email channel
   :align: center

Disabling an email account
++++++++++++++++++++++++++

If you temporarily don't need an account to be fetched, sometimes disabling the account is the better approach 
instead of deleting it. This is also a good idea during maintenance work or even migration Zammad to a new host. 

.. warning:: If you deactivate an email channel, all associated email addresses can no longer **send emails**.
   Zammad does not clear the email address from groups, but throw errors in the ticket if you still try to send a email.

Deleting an email account
+++++++++++++++++++++++++

If you no longer wish to use a specific email account with Zammad or maybe want to merge several email addresses to 
one single account, you can simply delete an existing channel. 

This will remove the channel configuration, but not email addresses. You will need to remove them in a second step. 
Zammad will display all not associated email addresses above your email accounts. 

.. hint:: For you overview we'll tell you how to re-assign email addresses on :doc:`/channels/email/aliases`.

.. figure:: /images/channels/email/email-addresses-without-channel.png
   :alt: email addresses without channels attached
   :align: center