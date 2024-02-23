# LTI Advantage: Using Moodle as an LTI Tool on the Canvas Platform

## Moodle Configuration

### Enable LTI Authentication Plugin

Navigate to `Site administration > Plugins > Authentication > Manage authentication` and enable **LTI**.

### Enable Publish as LTI Tool Plugin

Go to `Site administration > Plugins > Enrolments > Manage enrol plugins` and enable **Publish as LTI tool**.

#### Recommended Setting

It is advised to enable **Allow frame embedding** under `Site Administration > Security > HTTP security`.

> **Note**: Refer to "How to add the Publish as LTI tool enrollment method to a Moodle course" before proceeding with Canvas admin for integration.

### Register a Platform in Moodle

1. Navigate to `Site Administration > Plugins > Enrollments > Tool registration`.
2. Click **Register a platform**.
3. Provide a platform name. The platform name represents the course you are registering.

After providing a platform name, note down the URLs for Manual Registration. You'll need some of these for Canvas integration. For the moment, we are done registering this platform.

## Required Moodle URLs and IDs for Canvas configuration

- Tool URL
- Initiate login URL
- JWKS URL
- Custom Property ID. This can be found at `Course > More > Published as LTI tools`

## Canvas Configuration

### Add Developer Key

1. Go to `Admin > Developer Keys`.
2. Add a key and choose **LTI Key** from the dropdown.
3. Provide a **Key Name** that matches the platform name in Moodle.
4. Under **Redirect URIs**, paste the Tool URL from Moodle.
5. **Method**: Manual Entry (default)
6. **Title and Descriptions**: These can be the same as the Key Name, or they can be more descriptive.
7. **Target Link URI**: Paste the Tool URL from Moodle.
8. **OpenID Connect Initiation Url**: Paste the Initiate login URL from Moodle.
9. **JWK Method**: Select **Public JWK URL** and paste the JWKS URL from Moodle.
10. Toggle **LTI Advantage Services**, and enable the first five options.

They should be:

- Can create and view assignment data in the gradebook associated with the tool.
- Can view assignment data in the gradebook associated with the tool.
- Can view submission data for assignments associated with the tool.
- Can create and update submission results for assignments associated with the tool.
- Can retrieve user data associated with the context the tool is installed in.

11. Toggle **Additional Settings**.
12. Under **Custom Fields**, add the Custom Property ID from Moodle. (id=xxx)
13. **Privacy Level**: Public
14. Under **Placements**, add **Assignment Selection**.
15. Toggle Assignment Selection.
16. Within Assignment Selection, Paste the Tool URL from Moodle within the **Target Link URI** field.
17. Select **LtiDeepLinkingRequest** under **Select Message Type**.
18. Save Developer Key.
19. Toggle new key to **ON**.
20. Note down the **Client ID** from the Details column. It's a string of numbers.

### Add App to Canvas Course

1. Inside the Canvas course, select **Settings**.
2. Click **Apps** in the top navigation.
3. Add app, select **By Client ID** for Configuration Type.
4. Paste Client ID, submit, and install.
5. Click the gear icon on the newly created app.
6. Select **Deployment Id** from the dropdown.
7. Copy down the Deployment Id.

### Additional Moodle Configuration

1. Within **Tool registration** in Moodle, view the settings for the tool that was started earlier.
2. Select the **Deployments** tab and add the Deployment Id that was copied down.
3. For **Deployment name**, use the Key Name of the Developer Key.
4. Select the **Platform details** tab.
5. Click **Edit platform details**.
6. Fill in the remaining fields with the values below.

- Platform ID (issuer): https://canvas.instructure.com (Unless they are self hosting Canvas. In that case, you need the domain for the Canvas app.)
- Client ID: The Client ID from the Developer Key
- Authentication request URL: https://canvas.instructure.com/api/lti/authorize_redirect
- Public keyset URL: https://canvas.instructure.com/api/lti/security/jwks
- Access token URL: https://canvas.instructure.com/login/oauth2/token

6. Save the changes.

The integration is complete. When linking a Canvas assignment to a Moodle course, use submission type **External Tool**. Check **Load This Tool In A New Tab** and use the **Tool URL** from Moodle for the external tool URL.
