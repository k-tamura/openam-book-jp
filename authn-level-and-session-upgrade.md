[TODO 作成中]

## 認証レベルとセッションアップグレード

As shown in Configuring Authentication Modules , authentication modules are configured with an authentication level. This configuration sets the level of security associated with the module, Stronger forms of authentication are assigned higher authentication levels. (Or lower authentication level numbers if the deployment defines stronger authentication with lower authentication level numbers.) Upon successful authentication, a user's session includes information about the authentication level achieved.

Authorization policies can require a particular authentication level for access to sensitive resources (or at most or at least a specified authentication level). When a user who is already authenticated in the realm tries to access a sensitive resource with a valid session that does not have the requisite authentication level, OpenAM denies access to the resource. However, OpenAM also returns advices with the authorization decision. The advices indicate the need for the required authentication level. The policy agent or policy enforcement point can then send the user back to OpenAM for session upgrade.

During session upgrade the user authenticates with a stronger authentication module. The stronger module is typically part of the same authentication chain that handled the original authentication, though not required for access to less sensitive resources. Upon successful stronger authentication, the user session is upgraded to the new authentication level and modified to include any settings related to the stronger authentication.

If unsuccessful, session upgrade leaves the user session as it was before the attempt at stronger authentication. If session upgrade failed because the login page times out, OpenAM redirects the user's browser to the success URL from the last successful authentication.

OpenAM policy agents generally handle session upgrade without additional configuration, as policy agents are built to handle OpenAM's advices. If you build your own policy enforcement point (PEP), however, take advices and session upgrade into consideration. For RESTful PEPs, see Requesting Policy Decisions, and for indications on how to handle advices and session upgrade see Authentication and Logout.

OpenAM's support for session upgrades requires stateful sessions. Be sure that OpenAM is configured for stateful sessions—the default configuration—before attempting to upgrade OpenAM sessions.
