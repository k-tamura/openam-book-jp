This chapter provides a brief introduction to the web-based OpenAM console. It also lists and describes each command-line interface (CLI) administration tool.
1.1. OpenAM Web-Based Console

After you install OpenAM, log in to the web-based console as OpenAM administrator, amadmin with the password you set during installation. Navigate to a URL, such as http://openam.example.com:8080/openam. In this case, communications proceed over the HTTP protocol to a FQDN (openam.example.com), over a standard Java EE web container port number (8080), to a specific deployment URI (/openam).
Figure 1.1. OpenAM Administration Console
OpenAM Administration Console

When you log in as the OpenAM administrator, amadmin, you have access to the complete OpenAM console. In addition, OpenAM has set a cookie in your browser that lasts until the session expires, you logout, or you close your browser.[1]

When you log in to the OpenAM console as a non-administrative end user, you do not have access to the administrative console. Your access is limited to self-service profile pages and user dashboard.
Figure 1.2. OpenAM Console for Non-Administrative Users
OpenAM Console for Non-Administrative Users

If you configure OpenAM to grant administrative capabilities to another user, then that user is able to access both the administration console in the realms they can administrate, and their self-service profile pages.
Figure 1.3. OpenAM Console for a Delegated Administrator
OpenAM Console for a Delegated Administrator

For more on delegated administration, see Delegating Realm Administration Privileges .
