# Conditional Enrollment Via CSV and the AutoEnrol Moodle Plugin

## High-Level Overview

1. The client provides us with a CSV file, which may be delivered through SFTP or another method.

2. A scheduled task that we've created runs, and that CSV file is used to create user accounts or update existing user accounts. 

3. When a user logs in, that user is enrolled by the AutoEnrol plugin into one or more courses based on the values of their custom profile fields.
  
    * If the user is already enrolled in the appropriate courses, then <a href="#enrollment-method-settings" title="Always Enroll: No">no action will be taken</a>. 
  
    * If custom profile field values change, then a user <a href="#site-level-settings" title="Auto unenrol action: Keep user enrolled">will not be unenrolled</a> from their courses, and they will only be enrolled if conditions are met. 

## User Upload Via CLI    

Users can be created or modified using a terminal with admin/tool/uploaduser/cli/uploaduser.php

From [Moodle Docs](https://docs.moodle.org/403/en/Upload_users):

    In Moodle 3.10 onwards, an administrator can upload users via a CLI script.

    To obtain instructions on how to use the script, in the command line from the moodle directory run

    php admin/tool/uploaduser/cli/uploaduser.php --help

## Upload User Script Settings

    Upload type: Add new and update existing users

    New user password: Field required in file

    Existing user details: Override with file

    Existing user password: No changes

    Force password change: None

    Match on email address: Yes

    Allow renames: Yes

    Allow deletes: No

    Allow suspending and activating of accounts: No

    Standardize usernames: No

An example of how to use the command from within the moodle directory:

    sudo -u www-data php admin/tool/uploaduser/cli/uploaduser.php --delimiter_name=comma --file=absolute-path-to-csv-file --uutype=2 --uupasswordnew=0 --uuupdatetype=1 --uupasswordold=0 --uuforcepasswordchange=0 --uumatchemail=1 --uuallowrenames=1 --uuallowdeletes=0 --uuallowsuspends=0 --uustandardusernames=0

## CSV

This is what the CSV looks like using color as an example profile field:

    username,firstname,lastname,email,profile_field_color,auth,password
    "charles@example.com","Charles","Beadle","charles@example.com","red","saml2",""

profile\_field\_color is not the custom field name. The custom field name is color. We must prefix these fields with profile\_field\_

Note: Custom profile fields don't have to be visible to users. Given that these fields affect enrollment, set the visibility of these fields to "Not visible".

The password field is required, however, we can set saml2, or some other protocol as the auth value, and leave the password as an empty string.

## Enrollment Plugin

The enrollment plugin is called AutoEnrol.

Moodle plugin page:
https://moodle.org/plugins/enrol_autoenrol

Repo: https://github.com/bobopinna/moodle-enrol_autoenrol

We must configure the plugin at the site-level, and we must configure an enrollment method in each course that we wish to apply the enrollment method to. 

### Site-Level Settings

    Location: /admin/settings.php?section=enrolsettingsautoenrol

Add instance to new courses: No

Allow new enrolments: Yes

Allow enrolments on login: Yes

Enable self unenrol: No

Default role assignment: Student

Remove groups: No

__IMPORTANT!__ Auto unenrol action: Keep user enrolled

Enrolment expiry action: Keep user enrolled

Hour to send enrollment expiry notifications: 8

Enrolment duration: 0 days

Notify before enrollment expires: No

Notification threshold: 1 days

Unenrol inactive after: Never

Max enrolled users: 0

Send course welcome message: No

## Enrollment Method Settings

    Location: course -> participants -> enrollment methods -> add method

Allow existing enrolments: Yes

Allow new enrolments: Yes

Default assigned role: Student

Enroll When: Logging into Site

Always Enroll: No

Enable self unenrol: No

Enrolment duration: 0 days

Notify before enrollment expires: No

Notification threshold: 1 days

Start date: Disabled

End date: Disabled

Unenroll inactive after: Never

Send course welcome message: No

__Then, set the user filtering conditions to the profile field(s)__
