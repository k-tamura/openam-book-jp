# 認証

## 「認証」とは

認証とは、何かによって対象の正当性を確認する行為を指します。最も一般的な認証として思いつくのは、IDとパスワードによってユーザーの正当性を確認することです。対象は人以外であっても構いませんし、正当性の確認はユーザーの入力などが無くても構いません。例えば、OpenAMでは、ネットワーク上のサーバーコンピュータの認証を行うRADIUSサーバー機能や、ログインしようとするユーザーの普段使用するデバイスと大きく異なるデバイスでのログインを

### OpenAMにおける「認証」について

Access management is about controlling access to resources. OpenAM plays a role similar to border control at an international airport. Instead of having each and every airline company deal with access to each destination, all airlines redirects passengers to border control. Border control then determines who each passenger is according to passport credentials. Border control also checks whether the identified passenger is authorized to fly to the destination corresponding to the ticket, perhaps based on visa credentials. Then, at the departure gate, an agent enforces the authorization from border control, allowing the passenger to board the plane as long as the passenger has not gotten lost, or tried to board the wrong plane, or swapped tickets with someone else. Thus, border control handles access management at the airport.

OpenAM is most frequently used to protect web-accessible resources. Users browse to a protected web application page. An agent installed on the server with the web application redirects the user to OpenAM for access management. OpenAM determines who the user is, and whether the user has the right to access the protected page. OpenAM then redirects the user back to the protected page, with authorization credentials that can be verified by the agent. The agent allows OpenAM authorized users access the page.

Notice that OpenAM basically needs to determine two things for access management: the identity of the user, and whether the user has access rights to the protected page. Authentication is how OpenAM identifies the user. This chapter covers how to set up the authentication process. Authorization is how OpenAM determines whether a user has access to a protected resource. Authorization is covered later.

For authentication, OpenAM uses credentials from the user or client application. It then uses defined mechanisms to validate credentials and complete the authentication. The authentication methods can vary. For example, passengers travelling on international flights authenticate with passports and visas. In contrast, passengers travelling on domestic flights might authenticate with an identity card or a driver's license. Customers withdrawing cash from an ATM authenticate with a card and a PIN.

OpenAM allows you to configure authentication processes and then customize how they are applied. OpenAM uses authentication modules to handle different ways of authenticating. Basically, each authentication module handles one way of obtaining and verifying credentials. You can chain different authentication modules together. In OpenAM, this is called authentication chaining. Each authentication module can be configured to specify the continuation and failure semantics with one of the following four flags: required, optional, requisite, or sufficient. [3] The four flags, required, optional, requisite, and sufficient, come from the standards created for the Java Authentication and Authorization Service (JAAS).

- When a required module fails, the rest of the chain is processed, but the authentication fails.  
A required module might be used for login with email and password, but then fall through to another module to handle new users who have not yet signed up.

- When an optional module fails, authentication continues.  
An optional module might be used to permit a higher level of access if the user can present a X.509 certificate for example.

- When a requisite module fails, authentication fails and authentication processing stops.  
A requisite module might be used with exclusive SSO.

- When a sufficient succeeds, authentication is successful and later modules in the chain are skipped.  
You could set Windows Desktop SSO as sufficient, so authenticated Windows users are let through, whereas web users have to traverse another authentication module such as one requiring an email address and a password.

With OpenAM, you can further set authentication levels per module, with higher levels being used typically to allow access to more restricted resources. The OpenAM SPIs also let you develop your own authentication modules, and post-authentication plugins. Client applications can specify the authentication level, module, user, and authentication service to use among those you have configured. As described later in this guide, you can use realms to organize which authentication process applies for different applications or different domains, perhaps managed by different people.

When a user successfully authenticates, OpenAM creates a session, which allows OpenAM to manage that user's access to resources. In some deployments you need to limit how many active sessions a user can have at a given time. For example, you might want to prevent a user from using more than two devices at once. See Configuring Session Quotas for instructions.

OpenAM leaves the authentication process flexible so that you can adapt how it works to your situation. Although at first the number of choices can seem daunting, now that you understand the basic process, you begin to see how choosing authentication modules and arranging them in authentication chains lets you use OpenAM to protect access to a wide range of applications used in your organization.
